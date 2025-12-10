# Clinical Data ETL Pipeline

## Overview
This project implements an end-to-end **Extract-Transform-Load (ETL) pipeline** designed to facilitate clinical data interoperability. It extracts patient data (demographics, conditions, observations, procedures) from an **OpenEMR FHIR server**, transforms the data using the **Hermes SNOMED CT Terminology Service** for code resolution and mapping, and loads the standardized resources into a destination **Primary Care EHR FHIR server**.

Additionally, the pipeline demonstrates legacy system interoperability by generating valid **HL7 v2.5 ADT^A01** messages mapped with ICD-10 billing codes.

**Project Website:** [Link to your GitHub Pages or Project Website Here]

## Features
* **Extraction:** Securely pulls clinical resources (Patient, Condition, Observation, Procedure) via FHIR API.
* **Transformation:**
    * Standardizes address formats for validation compliance.
    * Resolves SNOMED CT codes to human-readable terms using Hermes.
    * Maps SNOMED CT concepts to ICD-10 for HL7 messaging.
* **Loading:** Validates and posts resources to a target FHIR server.
* **HL7 Generation:** creating ADT messages using `hl7apy`.

## Prerequisites

* **Python 3.8+**
* **Network Access:** You must be connected to the IU VPN (or specific network) to access the OpenEMR and Hermes servers.

## Installation

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/your-username/your-repo-name.git](https://github.com/your-username/your-repo-name.git)
    cd your-repo-name
    ```

2.  **Install dependencies:**
    ```bash
    pip install -r requirements.txt
    ```

    *If `requirements.txt` is not yet created, install these packages manually:*
    ```bash
    pip install requests hl7apy
    ```

3.  **Configure Authentication:**
    * Ensure your OAuth2 access token is saved in `src/data/access_token.json`.
    * The file structure should look like this:
        ```json
        {
          "access_token": "YOUR_ACCESS_TOKEN_HERE"
        }
        ```

## Usage

To run the full ETL pipeline, execute the main script from the terminal:

```bash
python src/main.py
