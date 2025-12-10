---
layout: default
title: ETL Pipeline
---

<div class="immersive-header-container">
  <div class="header-content-wrapper">
    
    <h1 class="glow-text">ETL Pipeline</h1>

    <nav class="dark-nav">
      <strong>Navigation:</strong> 
      <a href="index.html">Home</a> | 
      <a href="etl_pipeline.html" class="active">ETL Pipeline</a> | 
      <a href="insights.html">Insights</a> | 
      <a href="team_contrib.html">Team Contributions</a> | 
      <a href="about.html">About / Presentation</a>
    </nav>

    <div class="hero-image-wrapper">
      <img src="assets/images/etlpipeline.png" 
           alt="ETL Pipeline Architecture" 
           class="blended-hero-image">
    </div>
      
  </div>
</div>

---

# Overview

This project implements a complete Extract–Transform–Load (ETL) pipeline for clinical data interoperability. We used:

- **OpenEMR FHIR API** (source server)
- **Hermes SNOMED CT terminology server** (terminology and mappings)
- **Primary Care EHR FHIR API** (target server)
- **Python** for extraction, validation, and HL7 v2 message generation

The pipeline executes five coding tasks, each building on the previous one and demonstrating end-to-end data transfer between healthcare systems.

---

## 1. Extraction

### API Endpoints Used

- **OpenEMR FHIR Server** :- `https://in-info-web20.luddy.indianapolis.iu.edu/apis/default/fhir`

#### Key Read Operations
- **Patient Lookup:** `/Patient?family={name}&gender={gender}`
- **Patient by ID:** `/Patient/{id}`
- **Conditions:** `/Condition?patient={id}`
- **Observations:** `/Observation?patient={id}&code=85354-9`
- **Procedures:** `/Procedure?patient={id}`


### Authentication & Authorization

OpenEMR calls require an OAuth2 access token stored in a local JSON file:
```python
def get_headers():
    access_token = get_access_token_from_file()
    return {
        "Authorization": f"Bearer {access_token}",
        "Accept": "application/fhir+json",
        "Content-Type": "application/fhir+json"
    }
```

- Every request uses a Bearer token in headers.
- Token is read from `src/data/access_token.json`.
- If the token is missing or invalid, the ETL process terminates safely.

### Error Handling Strategy

Across all ETL stages, every API call and file operation was wrapped with basic safety checks to prevent runtime failures and support easier debugging:

- **Authentication Check** – The script stops safely if the access token is missing or expired.
- **HTTP Response Validation** – For each GET/POST request, we check `status_code` = 200. Non 200 responses were corrected.
- **Network Exception Handling** – All requests are wrapped in try/except to catch timeouts or connectivity issues without crashing the program.
- **Defensive JSON Access** – We always use `.get()` with default values instead of assuming keys exist, preventing KeyError/IndexError crashes when bundles are empty.
- **Safe Pipeline Stops** – If a mandatory prerequisite is missing (e.g., source patient, parent condition, or validation failure), the script exits cleanly rather than continuing with invalid data.

This approach keeps the ETL process predictable, prevents silent failures, and makes debugging easier when interacting with live FHIR, Hermes, and target EHR servers.

---

## 2. Transformation

 **Hermes Terminology Server**
-  'http://159.203.121.13:8080/v1/snomed/concepts/80967001/extended'

### Data cleaning & standardization

We applied lightweight transformations as required by the destination system:

#### Address formatting (Task 1)
- OpenEMR address objects did not include `district`
- The Primary Care EHR required it for validation
- Our script added "Middlesex" into each address before cloning the patient
```python
def transform_address(address_list):
    for addr in address_list:
        addr["district"] = "Middlesex"
    return address_list
```

### Terminology Resolution

Using the Hermes SNOMED CT API, we:
- Retrieved parent and child concepts
- Resolved readable display labels
- Mapped SNOMED - ICD-10 for HL7 v2

Transformation priority order:
1. `preferredDescription.term`
2. `pt.term`
3. `fsn.term`

This guarantees a clean display term even when fields are missing.

### FHIR Date-Time Standardization

For Observation and Procedure resources:
- We dynamically set runtime timestamps using `datetime.now()`
```python
observation["effectiveDateTime"] = datetime.datetime.now().isoformat()
```

---

## 3. Loading

### Target Server - Primary Care EHR FHIR Server

All new data was posted to:
`http://159.203.105.138:8080/fhir`

Each resource was:

1. Cleaned to remove any existing `id` or `meta`
2. Posted to the destination using automatically created JSON
```python
response = requests.post(
    f"{TARGET_URL}/{resource_type}",
    headers=HEADERS_FHIR,
    json=resource_json
)
```

After creation:
- The assigned resource ID (observation & procedure) was saved locally for verification.

We selected a single patient (**Samuel Kuvalis**) across:

- **Task 1:** Search and extract a patient and their condition from OpenEMR, identify the parent SNOMED concept, automatically create a Patient and a Parent Condition on the Primary Care EHR server, and validate both resources against their required profiles. 
- **Task 2:** Identify a child SNOMED term from the patient's existing conditions using Hermes, create a new condition using that child term on the Primary Care EHR FHIR server, and validate it.  
- **Task 3:** Create a new Blood Pressure Observation for the patient created in Task 1 and post it to the Primary Care EHR FHIR server (or reuse if it already exists in OpenEMR)**.
- **Task 4:** Create a new Procedure resource for the patient created in Task 1 and post it to the Primary Care EHR FHIR server (or reuse if it already exists in OpenEMR)  
- **Task 5:** Generate an HL7 ADT message using FHIR Patient and Condition data, automatically map SNOMED to ICD-10, and save the message as a .txt file.**.

Each task reuses a shared authentication layer and follows the same pattern:

---

## Baseline setup & Headers

All tasks use the same base URLs, headers, token file, and helper to get the OAuth access token into FHIR requests.
```python
OPENEMR_URL = "https://in-info-web20.luddy.indianapolis.iu.edu/apis/default/fhir"
TARGET_URL  = "http://159.203.105.138:8080/fhir"
HERMES_URL  = "http://159.203.121.13:8080/v1/snomed"

HEADERS_FHIR = {"Accept": "application/fhir+json", "Content-Type": "application/fhir+json"}
TOKEN_FILE   = Path(__file__).parent / "data" / "access_token.json"
DATA_DIR     = Path(__file__).parent / "data"

def get_headers():
    """Read the access token and build standard FHIR headers."""
    with open(TOKEN_FILE, "r") as f:
        access_token = json.load(f).get("access_token")
    return {
        "Authorization": f"Bearer {access_token}",
        "Accept": "application/fhir+json",
        "Content-Type": "application/fhir+json",
    }
```


---

## Task 1  

### Goal

- Use FHIR search parameters (e.g., family name and gender) to query OpenEMR and retrieve the patient record for Samuel Kuvalis.
- Extract the patient's active conditions and identify the target SNOMED code for Gingivitis (66383009).
- Look up the parent SNOMED concept and its preferred name using the Hermes terminology server.
- Transform and load the Patient resource onto the target Primary Care FHIR server.
- Create a Condition resource on the target server using the parent concept, linked to the new patient.
- Validate the patient and condition resources against the required FHIR profiles (my-patient-profile and my-condition-profile).

### Code snippet

**Step 1. Authentication & Header Setup**

```python
def get_access_token_from_file():
    with open("data/access_token.json") as file:
        return json.load(file).get("access_token")

def get_headers():
    return {
        "Authorization": f"Bearer {get_access_token_from_file()}",
        "Accept": "application/fhir+json",
        "Content-Type": "application/fhir+json"
    }
```
Loads token & builds FHIR-auth headers for API requests.


**Step 2. Search Patient by name + gender**

```python
def search_patient_by_name_gender(name, gender):
    url = f"{OPENEMR_URL}/Patient?family={name}&gender={gender}"
    return requests.get(url, headers=get_headers())
```
Used to locate Samuel Kuvalis in OpenEMR.

**Step 3.Retrieve all Conditions for a Patient**

```python
def search_condition(patient_id):
    return requests.get(f"{OPENEMR_URL}/Condition?patient={patient_id}",
                         headers=get_headers())
```
We checked if patient has Gingivitis (66383009).

**Step 4. Get Parent SNOMED using Hermes**

```python
def find_parent_concept_hermes(code):
    return requests.get(f"{HERMES_URL}/concepts/{code}/extended")

def get_concept_name_hermes(code):
    data = requests.get(f"{HERMES_URL}/concepts/{code}/extended").json()
    return data.get('pt',{}).get('term') or "Unknown Term"
```
Hermes returned parent = Gingival disease.

**Step 5.Create Patient on Target FHIR**

```python
def create_patient_on_target(patient):
    patient["address"][0]["district"] = "Middlesex"
    return requests.post(f"{TARGET_URL}/Patient",
                         headers=HEADERS_FHIR, json=patient)
```
Cloned patient into the target EHR.

**Step 6.Create Condition in Target using Parent Term**

```python
def create_condition_on_target(pid, code, term):
    condition = {
        "resourceType":"Condition",
        "code":{"coding":[{"system":"http://snomed.info/sct",
                           "code":code,"display":term}]},
        "subject":{"reference":f"Patient/{pid}"}
    }
    return requests.post(f"{TARGET_URL}/Condition",
                         headers=HEADERS_FHIR, json=condition)
```
Created Condition/Gingival Disease for new patient.

**Step 7. Final Execution flow**

```python
search_patient_by_name_gender("Kuvalis","male")
search_condition(patient_id)
find_parent_concept_hermes("66383009")
create_patient_on_target(patient)
create_condition_on_target(new_patient_id,parent_id,parent_term)
```
Task-1 Completed: Patient + Condition cloned & saved locally.

**Step 8. Patient & Condition Validation**

```python
data_dir = Path(__file__).parent / "data"
FHIR_SERVER_URL = "http://159.203.105.138:8080/fhir"

MY_PATIENT_PROFILE = (
    "http://159.203.105.138:8080/fhir/StructureDefinition/my-patient-profile"
)
MY_CONDITION_PROFILE = (
    "http://159.203.105.138:8080/fhir/StructureDefinition/my-condition-profile"
)
```
Points to the target FHIR server and defines the custom profiles that our resources must conform to.

**Step 9. Generic validation helper function**

```python
```python
def validate_resource(file_name, resource_type, profile_url):
    """
    Load a Patient/Condition JSON file, attach its profile,
    and send it to the FHIR server's $validate operation.
    """
    file_path = data_dir / f"{file_name}.json"

    if not file_path.exists():
        print(f"Error: File {file_name}.json not found at {file_path}")
        return

    print(f"\n--- Validating {file_name}.json as {resource_type} ---")

    with open(file_path, "r") as f:
        resource = json.load(f)

    # Attach meta.profile required by the server-side validator
    resource["meta"] = {"profile": [profile_url]}

    response = requests.post(
        f"{FHIR_SERVER_URL}/{resource_type}/$validate",
        headers={
            "Content-Type": "application/fhir+json",
            "Accept": "application/fhir+json",
        },
        params={"profile": profile_url},
        json=resource,
    )

    print(f"Status: {response.status_code}")
    outcome = response.json()
    issues = outcome.get("issue", [])

    if issues:
        print("Validation Result:", json.dumps(issues[0], indent=2))
    else:
        print("Validation Successful.")
```

This function is reusable for both Patient and Condition, it loads the saved JSON, sets `meta.profile`, and calls `$validate` on the server.

**Step 10. Running validation for Patient & Condition**

```python
if __name__ == "__main__":
    # Validate cloned Patient from Task 1
    validate_resource("patient", "Patient", MY_PATIENT_PROFILE)

    # Validate cloned Condition from Task 1
    validate_resource("condition", "Condition", MY_CONDITION_PROFILE)
```
This is the entry point: it validates `patient.json` and `condition.json` from Task 1 against your custom profiles.

## Task 2 

### Goal

- Extract gingivitis concept ID and use the Hermes terminology API to look up a child SNOMED term for gingivitis (e.g., 31642005 – Acute gingivitis).
- Use the patient that was already created in Task 1 on the Primary Care EHR server and create a new condition resource for that patient using the child SNOMED concept.
- Perform validation separately using the $validate endpoint and the my-condition-profile.

## Code snippet

**Step 1. Verify Parent Gingivitis Exists in OpenEMR**
```python
def get_conditions(patient_id):
    return requests.get(f"{OPENEMR_URL}/Condition?patient={patient_id}",
                         headers=get_headers())
```
Checks source patient for Gingivitis (66383009).

**Step 2. Resolve Child SNOMED Using Hermes**
```python
def get_child_term(code):
    data = requests.get(f"{HERMES_URL}/concepts/{code}/extended").json()
    return data.get("pt",{}).get("term") or "Unknown Term"
```
Hermes returns readable child term (e.g., Acute gingivitis).

**Step 3. Create Child Condition on Target**
```python
def create_child_condition(pid, code, term):
    cond = {
        "resourceType":"Condition",
        "code":{"coding":[{"system":"http://snomed.info/sct",
                           "code":code,"display":term}]},
        "subject":{"reference":f"Patient/{pid}"}
    }
    return requests.post(f"{TARGET_URL}/Condition",
                         headers=HEADERS_FHIR, json=cond)
```
Posts new child Condition for cloned patient.



**Step 4. Validate Created Condition**

```python
r_val = validate_resource("Condition",
                          r_cond.json(),
                          MY_CONDITION_PROFILE)
print("Validation:", r_val.status_code)
```
Ensures Condition follows my-condition-profile.

## Task 3

### Goal

- Check OpenEMR for an existing Blood Pressure panel Observation (LOINC 85354-9) for Samuel Kuvalis.
- If none exists, create a JSON file (`observation_task3.json`).
- Created observation for patient and set current `effectiveDateTime`.
- Post the Observation to the target FHIR server.

### Code snippet

**Step 1. Check OpenEMR for Existing BP Observation (LOINC 85354-9)**
```python
def check_openemr_for_observation(source_patient_id):
    url = f"{OPENEMR_URL}/Observation?patient={source_patient_id}&code=85354-9"
    r = requests.get(url, headers=get_headers())
    bundle = r.json()
    return bundle.get("entry", [])[0]["resource"] if bundle.get("total", 0) > 0 else None
```
Checks if OpenEMR already has a Blood Pressure panel for the source patient.

**Step 2. Prepare Observation Payload from Local Template**

```python
def prepare_observation_payload(target_patient_id):
    obs = json.load(open(DATA_DIR / "observation_task3.json"))
    obs["subject"]["reference"]   = f"Patient/{target_patient_id}"
    obs["effectiveDateTime"]      = datetime.datetime.now().isoformat()
    return obs
```
Loads the template JSON, links it to the cloned patient, and updates the timestamp.

**Step 3. Post Observation to Target FHIR Server**

```python
def post_observation_to_target(observation_json):
    observation_json.pop("id",   None)
    observation_json.pop("meta", None)
    return requests.post(f"{TARGET_URL}/Observation",
                         headers=HEADERS_FHIR,
                         json=observation_json)
```

Removes source-specific fields and creates a new Observation on the target server.

**Step 4. Execution Flow**

```python
obs_data = check_openemr_for_observation(SOURCE_PATIENT_ID)

if not obs_data:
    obs_data = prepare_observation_payload(PATIENT_ID_ON_TARGET)
else:
    obs_data["subject"]["reference"] = f"Patient/{PATIENT_ID_ON_TARGET}"

response = post_observation_to_target(obs_data)
```

If OpenEMR has a BP panel, reuse it; otherwise build from template, then post to the Primary Care FHIR server.

## Task 4 

### Goal

- Check OpenEMR for an existing Procedure for Samuel Kuvalis.
- If one exists, reuse it as a starting resource; otherwise create a `procedure_task4.json`.
- Update `body site`, 'text' and `interpretation`.
- Post the Procedure to the target FHIR server.

### Code snippet

**Step 1. Load cloned patient ID from Task 1**

```python
DATA_DIR = Path(__file__).parent / "data"

def get_target_patient_id():
    """Load the cloned patient ID created in Task 1."""
    with open(DATA_DIR / "patient.json", "r") as f:
        patient_resource = json.load(f)
    return patient_resource.get("id")
```
Reuses the cloned Patient from Task 1 so the Procedure is linked to the same target patient.

**Step 2. Check OpenEMR for existing Procedures**

```python
def check_openemr_procedure(patient_id):
    """Check OpenEMR for any Procedure resources for this patient."""
    url = f"{OPENEMR_URL}/Procedure?patient={patient_id}"
    print(f"\nSearch Query: GET {url}")

    response = requests.get(url, headers=get_headers())
    if response.status_code == 200:
        bundle = response.json()
        total = bundle.get("total", 0)
        print(f"Found {total} existing procedures on OpenEMR.")
        if total > 0:
            return bundle["entry"][0]["resource"]  # reuse first Procedure
    return None
```
Implements the Extract step: “If a procedure exists in OpenEMR, reuse it; otherwise, we’ll fall back to a local template.”

**Step 3.Prepare Procedure payload (from local JSON template)**

```python
def prepare_procedure_payload(target_patient_id):
    """Load local Procedure template and link it to the cloned patient."""
    file_path = DATA_DIR / "procedure_task4.json"

    with open(file_path, "r") as f:
        procedure = json.load(f)

    # Point the Procedure to the cloned patient on the target server
    procedure["subject"]["reference"] = f"Patient/{target_patient_id}"

    # Stamp performedDateTime with current timestamp
    procedure["performedDateTime"] = datetime.datetime.now().isoformat()

    return procedure
```
Implements the Transform step using a reusable JSON template (procedure_task4.json).

**Step 4. Post Procedure to target FHIR server**

```python
def post_procedure_to_target(procedure_json):
    """Create a Procedure on the target FHIR server."""
    url = f"{TARGET_URL}/Procedure"

    # Let the target server assign id and meta
    procedure_json.pop("id", None)
    procedure_json.pop("meta", None)

    response = requests.post(url, headers=HEADERS_FHIR, json=procedure_json)
    response.raise_for_status()

    new_proc = response.json()
    proc_id = new_proc.get("id")
    print("Procedure posted successfully. ID:", proc_id)

    # Save ID for verification
    if proc_id:
        with open(DATA_DIR / "procedure_id.txt", "w") as f:
            f.write(proc_id)

    return proc_id
```
Implements the Load step: creates the Procedure on the Primary Care FHIR server and remembers its ID.

**Step 5.Verify created Procedure on target server**

```python
def get_EHR_procedure():
    """Verify the Procedure exists on the target server by reading it back."""
    with open(DATA_DIR / "procedure_id.txt", "r") as f:
        proc_id = f.read().strip()

    url = f"{TARGET_URL}/Procedure/{proc_id}"
    response = requests.get(url, headers=HEADERS_FHIR)
    print(f"\nVerify GET {url}")

    if response.status_code == 200:
        print("Verification Successful")
    else:
        print("Verification Failed:", response.status_code)
```

Confirms that the newly created Procedure can be successfully retrieved.

**Step 6.Final Execution Flow**

```python
target_patient_id = get_target_patient_id()
procedure_data = check_openemr_procedure(SOURCE_PATIENT_ID)

if not procedure_data:
    procedure_data = prepare_procedure_payload(target_patient_id)

new_proc_id = post_procedure_to_target(procedure_data)
get_EHR_procedure()
```

Task-4 Completed: Procedure created for the cloned patient on the target FHIR server and successfully verified with a read-back GET request.



## Task 5 

### Goal

- Use Condition, and Encounter data from OpenEMR for the same patient.
- Look through the Conditions to find SNOMED 840539006 – COVID-19 (priority diagnosis).
- Use Hermes to map SNOMED 840539006 - ICD-10 using reference set 447562003.
- Build an HL7 v2.5 ADT^A01 message using `hl7apy` including MSH, EVN, PID, PV1, DG1.
- Include both the cross-mapped ICD-10 code and the original SNOMED code in DG1-3.
- Export the HL7 message to `task5_adt_message.txt`.

### Code snippet

**Step 1. Get Access Token & Build Headers**

```python
def get_access_token_from_file():
    with open(TOKEN_FILE) as f:
        return json.load(f).get("access_token")

def get_headers():
    return {
        "Authorization": f"Bearer {get_access_token_from_file()}",
        "Accept": "application/fhir+json",
        "Content-Type": "application/fhir+json"
    }
```

Reads the OAuth token and builds headers for OpenEMR FHIR calls.

**Step 2. Fetch Patient & Condition Data from OpenEMR**

```python
def get_patient_by_id(pid):
    return requests.get(f"{OPENEMR_URL}/Patient/{pid}",
                        headers=get_headers())

def get_patient_conditions(pid):
    return requests.get(f"{OPENEMR_URL}/Condition?patient={pid}",
                        headers=get_headers())
```

Used to pull Patient and all Condition resources for the same ID.

**Step 3. SNOMED → ICD-10 Mapping via Hermes**

```python
def get_icd10_mapping(snomed_code):
    r = requests.get(f"{HERMES_URL}/concepts/{snomed_code}/map/447562003",
                     timeout=10)
    data = r.json()
    row  = data[0] if isinstance(data, list) else data
    return {
        "code": row.get("mapTarget"),
        "display": row.get("mapTargetName", row.get("mapTarget"))
    }
```

Automatically maps a SNOMED CT code to an ICD-10 code using Hermes.

**Step 4. HL7 Date/Time Helpers**

```python
def format_hl7_date(d):
    return datetime.fromisoformat(d.replace("Z","+00:00")).strftime("%Y%m%d")

def format_hl7_timestamp():
    return datetime.now().strftime("%Y%m%d%H%M%S")
```

Converts FHIR-style dates into HL7 v2 date / timestamp format.

**Step 5. Create ADT^A01 HL7 Message (Core Segments)**

```python
def create_adt_message(patient, conditions):
    m = Message("ADT_A01", version="2.5")

    # MSH
    m.msh.msh_3 = "OpenEMR"
    m.msh.msh_5 = "HL7_Processor"
    m.msh.msh_7 = format_hl7_timestamp()
    m.msh.msh_9 = "ADT^A01^ADT_A01"

    # PID
    name = patient.get("name",[{}])[0]
    m.pid.pid_3 = f"{patient.get('id','UNK')}^^^OpenEMR^MR"
    m.pid.pid_5 = f"{name.get('family','')}^{name.get('given',[''])[0]}"

    # DG1 (example: use first condition)
    entry = conditions.get("entry",[{}])[0].get("resource",{})
    coding = entry.get("code",{}).get("coding",[{}])[0]
    mapping = get_icd10_mapping(coding.get("code"))

    dg1 = m.add_segment("DG1")
    dg1.dg1_3 = f"{mapping['code']}^{mapping['display']}^I10"

    return m
```

Builds MSH, PID, and DG1 using FHIR patient/condition data plus Hermes mapping.

**Step 6. Final Execution Flow & Save HL7 to File**

```python
pt_data   = get_patient_by_id(SOURCE_PATIENT_ID).json()
cond_data = get_patient_conditions(SOURCE_PATIENT_ID).json()

msg     = create_adt_message(pt_data, cond_data)
hl7_str = msg.to_er7()
open(HL7_OUTPUT_FILE, "w").write(hl7_str)
```
Fetches FHIR data, creates the ADT^A01 message, converts to ER7, and saves it as `task5_adt_message.txt`.

Inside `create_adt_message(...)` we:

1. Scan the patient's Conditions for SNOMED 840539006 (COVID-19).
2. Resolve a human-readable SNOMED term using a helper (`lookup_snomed_term`).
3. Call `get_icd10_mapping("840539006")` to select the correct ICD-10 code and label.
4. Populate DG1 with both the ICD-10 billing code and the original SNOMED clinical code.
5. Convert the HL7 message to ER7 format and save it as `task5_adt_message.txt`.

**Justfication for choosing condition as COVID-19 ("840539006")**

We used a different SNOMED condition for Task 5 because gingivitis could not be reliably cross-mapped to an ICD-10 code using the Hermes mapping service. Our mapping attempts returned acute gingivitis instead of the intended gingivitis parent code. To generate a proper HL7 message with a valid SNOMED - ICD-10 mapping, we selected a broader and clinically well-mapped condition - COVID-19 (SNOMED 840539006) - which correctly translated to ICD-10 U07.1.

---

## 8. ETL Flow Summary

Across all five tasks, we consistently:

### Extract

- FHIR Patient, Condition, Observation, Procedure, Encounter from OpenEMR
- SNOMED CT terms and ICD-10 mappings from Hermes

### Transform

- Adjust addresses, timestamps, subject references
- Resolve parent and child SNOMED terms
- Map SNOMED to ICD-10 for HL7 DG1
- Add `meta.profile` when validating against custom FHIR profiles

### Load / Export

- POST new Patient, Condition, Observation, Procedure to the target FHIR server
- Call `$validate` for parent/child Conditions as required
- Generate and save a standards-compliant HL7 v2.5 ADT^A01 message

This pipeline demonstrates how one patient's data can move safely and consistently between FHIR servers, terminology services, and legacy HL7 v2 systems, while staying aligned with SNOMED CT and ICD-10 coding standards.

### Challenges & Resolutions

- **Challenge: Missing or incomplete terminology data from Hermes (e.g., unknown preferredDescription).**  
  **Resolution:** We implemented a cascading lookup strategy that tries `preferredDescription - pt - fsn` to reliably retrieve readable SNOMED terms.

- **Challenge: Target FHIR server validation errors caused by missing fields (e.g., clinicalStatus, verificationStatus, or address.district).**  
  **Resolution:** We enhanced the transformation logic to inject required fields into Patient and Condition templates before POST/validate, ensuring schema compliance.

- **Challenge: Difficulty handling 401 Unauthorized errors due to expired access tokens.**  
  **Resolution:** Token retrieval and header construction were centralized, and the system checks token presence before running ETL, preventing invalid API calls.

- **Challenge: Empty search bundles or missing resources in OpenEMR (patient conditions, observations, procedures).**  
  **Resolution:** We implemented conditional logic - if a resource is not found, fallback to local JSON templates so the ETL pipeline does not break.

- **Challenge: HL7 ADT message formatting issues and mapping SNOMED - ICD-10.**  
  **Resolution:** We added helper functions to auto-format HL7 dates and used Hermes mapping to fetch ICD-10 codes, allowing DG1 segments to be populated accurately.

---

<style>
/* --- IMMERSIVE HEADER STYLES (MATCHING INSIGHTS PAGE) --- */

.immersive-header-container {
  background: linear-gradient(135deg, #021129 0%, #072c5e 100%);
  color: #ffffff;
  margin-left: -1rem; 
  margin-right: -1rem;
  padding: 3rem 1.5rem 1rem 1.5rem;
  border-radius: 0 0 20px 20px; 
  box-shadow: 0 10px 30px rgba(0,0,0,0.4); 
  margin-bottom: 3rem;
}

@media (min-width: 800px) {
  .immersive-header-container {
     margin-left: -3rem; 
     margin-right: -3rem;
  }
}

.header-content-wrapper {
  max-width: 900px;
  margin: 0 auto;
  text-align: center;
}

.glow-text {
  color: #fff;
  text-shadow: 0 0 20px rgba(0, 122, 204, 0.9);
  margin-bottom: 1.5rem;
  font-weight: 700;
}

.dark-nav {
  background: rgba(255, 255, 255, 0.05); 
  border: 1px solid rgba(255, 255, 255, 0.15);
  padding: 0.8rem 1.2rem;
  border-radius: 50px; 
  color: #ccc;
  display: inline-block;
  backdrop-filter: blur(5px); 
}

.dark-nav strong {
    color: #fff;
    margin-right: 5px;
}

.dark-nav a {
  color: #4db8ff !important; 
  text-decoration: none;
  padding: 0 5px;
  transition: color 0.3s ease;
}

.dark-nav a:hover {
  color: #ffffff !important;
  text-shadow: 0 0 10px #4db8ff;
  text-decoration: none;
}

.dark-nav a.active {
    color: #ffffff !important;
    text-shadow: 0 0 10px #4db8ff;
    font-weight: bold;
    text-decoration: underline;
}

.hero-image-wrapper {
  position: relative;
  margin-top: 2.5rem;
  border-radius: 12px;
  box-shadow: 0 0 50px rgba(0, 122, 204, 0.25); 
  transition: transform 0.3s ease;
}

.hero-image-wrapper:hover {
    transform: scale(1.01);
}

.blended-hero-image {
  max-width: 100%;
  height: auto;
  display: block;
  border-radius: 12px;
  border: 1px solid rgba(255, 255, 255, 0.1);
}
</style>
