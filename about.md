<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>About / Presentation</title>
  <style>
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
      line-height: 1.6;
      color: #333;
      max-width: 1200px;
      margin: 0 auto;
      padding: 20px;
      background-color: #f9f9f9;
    }

    /* Immersive Header Styles */
    .immersive-header-container {
      background: linear-gradient(135deg, #021129 0%, #072c5e 100%);
      color: #ffffff;
      margin-left: -20px;
      margin-right: -20px;
      padding: 3rem 1.5rem 1rem 1.5rem;
      border-radius: 0 0 20px 20px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.4);
      margin-bottom: 3rem;
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

    h1 {
      color: #2c3e50;
      border-bottom: 3px solid #4CAF50;
      padding-bottom: 10px;
    }

    h2 {
      color: #34495e;
      margin-top: 2rem;
      border-left: 4px solid #4CAF50;
      padding-left: 15px;
    }

    h3 {
      color: #2c3e50;
    }

    .pipeline-diagram {
      background: #fff;
      border: 2px solid #e0e0e0;
      border-radius: 8px;
      padding: 20px;
      margin: 20px 0;
      font-family: 'Courier New', monospace;
      white-space: pre;
      overflow-x: auto;
    }

    .team-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 20px;
      margin-top: 20px;
    }

    .team-card {
      background-color: #ffffff;
      border: 1px solid #e0e0e0;
      border-radius: 12px;
      padding: 20px;
      box-shadow: 0 4px 6px rgba(0,0,0,0.1);
      transition: transform 0.3s ease, box-shadow 0.3s ease;
      text-align: center;
    }

    .team-card:hover {
      transform: translateY(-5px);
      box-shadow: 0 10px 20px rgba(0,0,0,0.15);
      border-color: #4CAF50;
    }

    .team-card img {
      width: 150px;
      height: 150px;
      border-radius: 50%;
      object-fit: cover;
      margin-bottom: 15px;
      border: 4px solid #4CAF50;
    }

    .team-card h3 {
      margin-top: 0;
      color: #2c3e50;
      font-size: 1.2rem;
      border-bottom: 2px solid #4CAF50;
      padding-bottom: 10px;
      margin-bottom: 10px;
    }

    .team-location {
      font-weight: bold;
      color: #666;
      font-size: 0.9rem;
      margin-bottom: 10px;
      display: block;
    }

    .task-section {
      background: #fff;
      border-left: 4px solid #4CAF50;
      padding: 20px;
      margin: 20px 0;
      border-radius: 4px;
      box-shadow: 0 2px 4px rgba(0,0,0,0.05);
    }

    .task-section h3 {
      color: #4CAF50;
      margin-top: 0;
    }

    .highlight-box {
      background: #e8f5e9;
      border: 1px solid #4CAF50;
      border-radius: 6px;
      padding: 15px;
      margin: 15px 0;
    }

    ul {
      line-height: 1.8;
    }

    @media (max-width: 768px) {
      .team-grid {
        grid-template-columns: 1fr;
      }
      
      .immersive-header-container {
        margin-left: -20px;
        margin-right: -20px;
      }
    }

    .footer-nav {
      margin-top: 3rem;
      padding-top: 2rem;
      border-top: 2px solid #e0e0e0;
      text-align: center;
    }

    .footer-nav a {
      color: #4CAF50;
      text-decoration: none;
      margin: 0 15px;
      font-weight: 500;
    }
  </style>
</head>
<body>

<div class="immersive-header-container">
  <div class="header-content-wrapper">
    
    <h1 class="glow-text">About / Presentation</h1>

    <nav class="dark-nav">
      <strong>Navigation:</strong> 
      <a href="index.html">Home</a> | 
      <a href="etl_pipeline.html">ETL Pipeline</a> | 
      <a href="insights.html">Insights</a> | 
      <a href="team_contrib.html">Team Contributions</a> | 
      <a href="about.html" class="active">About / Presentation</a>
    </nav>

    <div class="hero-image-wrapper">
      <img src="assets/images/etlpipeline.png" 
           alt="Healthcare Interoperability Project Overview" 
           class="blended-hero-image">
    </div>
      
  </div>
</div>

<h2>Problem Statement & Objective</h2>
<p>Healthcare systems often struggle to exchange data due to incompatible formats, standards, and protocols.</p>
<p>Our goal was to extract patient data from OpenEMR (FHIR), transform it, and load it into a Primary Care FHIR server, then generate an HL7 v2 message for interoperability with legacy systems.</p>

<h3>Why FHIR?</h3>
<ul>
  <li>It is a modern standard used for health data exchange</li>
  <li>Stores data in JSON/XML, which is easy for software to process</li>
  <li>Works through REST APIs, so programs can access data automatically</li>
  <li>Makes it easier for systems to share medical information</li>
</ul>

<h3>Why HL7 v2?</h3>
<ul>
  <li>Many hospitals still use older systems that expect HL7 messages</li>
  <li>HL7 allows those systems to understand patient information</li>
  <li>By creating HL7 output, we show that our data can be used by both new and old systems</li>
  <li>It proves real interoperability in healthcare</li>
</ul>

<hr>

<h2>ETL Pipeline Overview</h2>
<p>We built a reusable ETL workflow that:</p>

<ol>
  <li><strong>Extracts</strong> a real Patient and Condition from OpenEMR</li>
  <li><strong>Transforms</strong> SNOMED codes using Hermes to identify parent and child terms</li>
  <li><strong>Creates</strong> structured resources (Patient, Condition, Observation, Procedure) on the target FHIR server</li>
  <li><strong>Performs validation</strong> to confirm profile compliance using <code>$validate</code></li>
  <li><strong>Generates</strong> an HL7 ADT message using selected Condition and ICD-10 mapping</li>
</ol>

<p>Below is a conceptual visualization:</p>

<div class="pipeline-diagram">
OpenEMR (FHIR Source)
    │
    │ Extract (FHIR API + Token Auth)
    ▼
Python ETL Script
    │
    │ Transform (Hermes SNOMED CT + Mapping)
    ▼
Primary Care FHIR Target
    │
    │ Load + Validate
    ▼
HL7 v2
</div>

<hr>

<h2>ETL Pipeline Demonstration</h2>
<p>We will now walk through each task separately, demonstrating the required five coding tasks.</p>

<div class="task-section">
  <h3>Task 1 – Parent ETL: Patient + Gingivitis Condition</h3>
  
  <p><strong>Extract:</strong></p>
  <ul>
    <li>We searched patients using FHIR search parameters: family=Kuvalis&gender=male</li>
    <li>Retrieved conditions → identified SNOMED 66383009 Gingivitis</li>
  </ul>
  
  <p><strong>Transform:</strong></p>
  <ul>
    <li>Looked up parent SNOMED concept using Hermes → 18718003 Gingival disease</li>
  </ul>
  
  <p><strong>Load:</strong></p>
  <ul>
    <li>Created a new cloned Patient with transformed address + identifier</li>
    <li>Posted Condition to Target FHIR server</li>
    <li>Saved patient.json & condition.json locally</li>
  </ul>
  
  <div class="highlight-box">
    <strong>Output:</strong> Patient + Condition successfully cloned & stored
  </div>
</div>

<div class="task-section">
  <h3>Task 2 – Child ETL: Create Child Condition</h3>
  
  <p><strong>Extract:</strong></p>
  <ul>
    <li>Used gingivitis again for the same patient (validation of task flow)</li>
  </ul>
  
  <p><strong>Transform:</strong></p>
  <ul>
    <li>Used child concept → 31642005 Acute Gingivitis</li>
  </ul>
  
  <p><strong>Load:</strong></p>
  <ul>
    <li>Posted Condition to Target server</li>
    <li>Saved → task2_child.json</li>
    <li>Validated using $validate with meta.profile</li>
  </ul>
  
  <div class="highlight-box">
    <strong>Result:</strong> Both parent & child conditions now exist for cloned patient
  </div>
</div>

<div class="task-section">
  <h3>Task 3 – Observation ETL (Blood Pressure)</h3>
  
  <p><strong>Extract:</strong></p>
  <ul>
    <li>Check if Observation 85354-9 exists; if server returns 0 or fails, fall back to template</li>
    <li>Check OpenEMR for BP Observation → none found</li>
  </ul>
  
  <p><strong>Transform:</strong></p>
  <ul>
    <li>Load local observation_task3.json template</li>
    <li>Insert Patient ID</li>
    <li>Updated timestamp automatically</li>
  </ul>
  
  <p><strong>Load:</strong></p>
  <ul>
    <li>Posted to target → Observation created successfully</li>
    <li>Save observation/results locally</li>
    <li>Verification GET performed</li>
  </ul>
  
  <div class="highlight-box">
    <strong>Output:</strong> Generated new BP Observation successfully (ID example: 124)
  </div>
</div>

<div class="task-section">
  <h3>Task 4 – Procedure ETL</h3>
  
  <p><strong>Extract:</strong></p>
  <ul>
    <li>Check Procedures for patient</li>
    <li>Search OpenEMR → No procedure found</li>
  </ul>
  
  <p><strong>Transform:</strong></p>
  <ul>
    <li>If none → use template (procedure_task4.json)</li>
    <li>Insert patient link + performedDateTime</li>
  </ul>
  
  <p><strong>Load:</strong></p>
  <ul>
    <li>Posted to target → Saved new Procedure ID</li>
    <li>Verified using GET request</li>
  </ul>
  
  <div class="highlight-box">
    <strong>Output:</strong> Procedure successfully created and confirmed via read-back check
  </div>
</div>

<div class="task-section">
  <h3>Task 5 – HL7 Interoperability Conversion</h3>
  
  <p><strong>Data used:</strong> FHIR JSON → HL7 v2.5 ADT A01 format</p>
  
  <p><strong>Steps:</strong></p>
  <ul>
    <li>Retrieve Patient + Conditions + Encounters again</li>
    <li>Fetch Patient, Condition, Encounter</li>
    <li>Prioritize COVID-19 SNOMED 840539006</li>
    <li>Map SNOMED 840539006 (COVID-19) → ICD10 using Hermes</li>
    <li>Automatically map SNOMED → ICD-10 using Hermes mapping set</li>
    <li>Generate segments:
      <ul>
        <li>MSH (header)</li>
        <li>PID (patient demographics)</li>
        <li>PV1 (visit/encounter info)</li>
        <li>DG1 (diagnosis with SNOMED → ICD10 mapping)</li>
      </ul>
    </li>
    <li>Generate HL7 ADT_A01 message with MSH, EVN, PID, PV1, DG1</li>
    <li>Saved to task5_adt_message.txt</li>
    <li>Export .txt file</li>
  </ul>
  
  <div class="highlight-box">
    <strong>Output:</strong> HL7 message produced successfully and ready for integration workflows
  </div>
</div>

<hr>

<h2>Why This Project Matters</h2>
<ul>
  <li><strong>FHIR</strong> enables consistent clinical data exchange</li>
  <li><strong>SNOMED CT</strong> enriches meaning and hierarchical context</li>
  <li><strong>ICD-10 mappings</strong> support downstream billing workflows</li>
  <li><strong>HL7 v2</strong> remains widely used in hospitals, especially for admissions, transitions of care, and claims</li>
  <li><strong>Modern interoperability</strong> requires all these pieces working together</li>
</ul>

<p>Our implementation shows how health information moves from EHR to terminology service, and finally to another EHR and HL7 v2 system, maintaining:</p>
<ul>
  <li>Data structure</li>
  <li>Clinical meaning</li>
  <li>Coding integrity</li>
  <li>Interoperability</li>
</ul>

<hr>

<h2>Real-World Application</h2>
<p><strong>If we were working in a real healthcare organization:</strong></p>
<ul>
  <li>This ETL pipeline enables clean migration of patient data</li>
  <li>Supports system-to-system interoperability</li>
  <li>Allows FHIR clinical data to be consumed by legacy HL7 systems</li>
  <li>Automates patient cloning, condition mapping, observation + procedure recording</li>
</ul>
<p>This ensures that hospitals, clinics, and EHRs exchange information accurately and securely.</p>

<hr>

<h2>About the Team</h2>
<p>We are currently pursuing a <strong>Master of Science in Health Informatics</strong> at Indiana University Indianapolis, combining our clinical backgrounds with data systems, interoperability standards, and applied informatics.</p>

<div class="team-grid">

  <div class="team-card">
    <img src="assets/images/lakshitha.jpeg" alt="FNU Lakshitha" onerror="this.src='https://via.placeholder.com/150/4CAF50/ffffff?text=FL'">
    <h3>FNU Lakshitha</h3>
    <span class="team-location">Delhi, India</span>
    <p>Bachelor's in Pharmacy — now studying Health Informatics and developing skills in standardized terminologies, API data integration, and digital workflows.</p>
  </div>

  <div class="team-card">
    <img src="assets/images/sakshi.jpeg" alt="Sakshi Mehta" onerror="this.src='https://via.placeholder.com/150/4CAF50/ffffff?text=SM'">
    <h3>Sakshi Mehta</h3>
    <span class="team-location">Gujarat, India</span>
    <p>Bachelor's in Dental Surgery — now studying Health Informatics and applying knowledge of clinical documentation and interoperability.</p>
  </div>

  <div class="team-card">
    <img src="assets/images/bhavitha.jpeg" alt="Bhavitha Asam" onerror="this.src='https://via.placeholder.com/150/4CAF50/ffffff?text=BA'">
    <h3>Bhavitha Asam</h3>
    <span class="team-location">Telangana , India</span>
    <p>Bachelor's in Dental Surgery — now studying Health Informatics with interest in EHR usability, digital care documentation, and practice workflows.</p>
  </div>

  <div class="team-card">
    <img src="assets/images/aravind.jpeg" alt="Aravind Kuruvikkattil Venugopalan" onerror="this.src='https://via.placeholder.com/150/4CAF50/ffffff?text=AK'">
    <h3>Aravind Kuruvikkattil Venugopalan</h3>
    <span class="team-location">Kerala, India</span>
    <p>Bachelor's in Ayurvedic Medicine — now studying Health Informatics and focusing on structured terminology, data mapping, and ETL automation.</p>
  </div>

</div>

<hr>

<div class="footer-nav">
  <a href="index.html">← Back to Home</a>
  <a href="etl_pipeline.html">View ETL Pipeline →</a>
</div>

</body>
</html>
