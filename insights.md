---
layout: default
title: Insights
---

<div class="immersive-header-container">
  <div class="header-content-wrapper">
    
    <h1 class="glow-text">Project Insights & Key Learnings</h1>

    <nav class="dark-nav">
      <strong>Navigation:</strong> 
      <a href="index.html">Home</a> | 
      <a href="etl_pipeline.html">ETL Pipeline</a> | 
      <a href="insights.html" class="active">Insights</a> | 
      <a href="team_contrib.html">Team Contributions</a> | 
      <a href="about.html">About / Presentation</a>
    </nav>

    <div class="hero-image-wrapper">
      <img src="assets/images/insights_hero.png" 
           alt="Healthcare Data Insights Visualization" 
           class="blended-hero-image">
    </div>
      
  </div>
</div>
---

## What We Learned

Our project journey with OpenEMR and FHIR was as much about collaboration and problem-solving as it was about technology. Along the way, we deepened our understanding of healthcare data standards, refined our engineering practices, and learned how to work effectively as a team on a complex integration pipeline.

---

## Technical Insights

Throughout the project, we explored real-world interoperability challenges and built practical solutions around them.

- **OAuth token reuse**: We learned how to authenticate once and safely reuse OAuth access tokens across multiple FHIR endpoints, simplifying our workflow and reducing redundant handshakes.

- **Efficient FHIR searches**: By using query parameters such as `family`, `gender`, `birthdate`, and `patient`, we were able to retrieve precise clinical data without over-fetching.

- **SNOMED CT relationships**: Working with the SNOMED CT hierarchy through the Hermes `/extended` endpoint helped us understand how parent and child concepts are linked within the terminology service.

- **Idempotent ETL design**: We designed our ETL scripts to be safe for re-runs—ensuring repeatable and predictable results whenever we needed to test or debug our code.

- **Resource validation**: Using the `$validate` operation on our FHIR server allowed us to confirm that our custom profiles adhered to expected standards, improving the overall quality and consistency of our data.

---

## Project & Team Insights

Beyond the technical work, the success of our project depended on how we organized and collaborated as a team.

- **Task-based division**: Dividing the workflow into Tasks 1-5 made it easier to assign ownership, maintain accountability, and work efficiently in parallel.

- **Consistent repository structure**: Using a uniform folder layout (`src/`, `data/`, `assets/`) kept our codebase clean and intuitive for everyone.

- **Document-first mindset**: Writing comprehensive documentation for each step forced us to articulate our process clearly, which helped us identify knowledge gaps early.

- **Incremental commits**: Making small, frequent commits proved far more effective than large, end-of-day pushes when debugging complex ETL logic.

- **Lightweight communication**: We used GitHub Issues and Discussions as an informal but effective way to track progress, troubleshoot problems, and stay aligned across tasks.

---

## Challenges and How We Resolved Them

Every significant technical journey comes with hurdles. Here are some of the key challenges we faced and how we overcame them:

- **OAuth access token authentication**: Initially, we encountered difficulties obtaining and maintaining valid access tokens for API authentication. We resolved this by seeking guidance from our professor and organizing troubleshooting sessions where all four team members worked together, testing the authentication flow on individual machines to identify environment-specific issues and share solutions.

- **SNOMED CT hierarchy mapping**: Mapping child terms to their correct parent concepts required careful analysis of the Hermes API responses. We experimented with the `/extended` endpoint and studied fields like `parentRelationships`, `preferredDescription`, and `fsn` until we could consistently retrieve accurate hierarchical mappings.

- **ICD-10 mapping limitations**: We discovered that Gingivitis (SNOMED CT code 66383009) did not have a direct or reliable mapping to ICD-10 codes in our terminology server. To demonstrate the complete ETL workflow including ICD-10 conversion for HL7 v2 messaging, we switched our clinical scenario to use COVID-19 as the primary condition, which has well-established ICD-10 mappings and allowed us to showcase the full interoperability pipeline.

- **GitHub Pages deployment issues**: Publishing our documentation website to GitHub Pages presented several technical challenges. We resolved these by standardizing our folder structure, using relative links consistently, and thoroughly testing the site locally before deployment.

---

## ETL Pipeline Architecture

The diagram below illustrates our complete end-to-end ETL workflow, showing how data flows from the OpenEMR source system through our Python transformation pipeline, with terminology enrichment from Hermes, and ultimately into both modern FHIR-based and legacy HL7 v2 target systems.

<p align="center">
  <img src="assets/etl_diagram.png" 
       alt="ETL Pipeline Architecture" 
       style="max-width: 100%; height: auto; border-radius: 8px; box-shadow: 0 4px 12px rgba(0,0,0,0.1);" />
</p>

**Key workflow components:**

1. **Extract**: Patient, Condition, Observation, and Procedure resources are retrieved from the OpenEMR FHIR server using authenticated API calls
2. **Transform**: Clinical data is enriched using SNOMED CT parent/child concept lookups from the Hermes terminology server, and restructured to match target system requirements
3. **Load to Target**: Transformed resources are validated and posted to the Primary Care FHIR server with proper patient references and profile conformance
4. **Generate HL7 v2**: FHIR resources are converted into ADT^A01 messages for legacy system interoperability, including ICD-10 code mapping

This architecture demonstrates how modern healthcare systems can bridge the gap between contemporary FHIR-based platforms and traditional HL7 v2 messaging infrastructure.

---

## Potential Improvements

Looking ahead, there's plenty of room for refinement and expansion. Some of our priority ideas include:

- Extending the ETL pipeline to more FHIR resources such as Encounters, Allergies, and Medications, following the same architectural pattern.

- Implementing more robust error handling and structured logging to capture timeouts, retries, and other runtime events.

- Moving all configuration—URLs, IDs, codes, profile links—into a single, external YAML or JSON file for easier updates and standardization.

- Packaging the entire ETL logic into a Python module for smoother reuse across projects or teams.

---

<style>
/* --- IMMERSIVE HEADER STYLES (MATCHING HOME PAGE) --- */

/* A dark container for the whole top section */
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

/* Custom Dark Navigation Bar */
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

/* Highlight the active link manually for this page */
.dark-nav a.active {
    color: #ffffff !important;
    text-shadow: 0 0 10px #4db8ff;
    font-weight: bold;
    text-decoration: underline;
}

/* Hero Image Styling */
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
