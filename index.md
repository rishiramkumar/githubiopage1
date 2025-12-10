---
layout: default
title: Home
---

<div class="immersive-header-container">
  <div class="header-content-wrapper">
    
    <h1 class="glow-text">INFO-B581 Final Project: ETL Pipeline</h1>

    <nav class="dark-nav">
      <strong>Navigation:</strong> 
      <a href="index.html" class="active">Home</a> | 
      <a href="etl_pipeline.html">ETL Pipeline</a> | 
      <a href="insights.html">Insights</a> | 
      <a href="team_contrib.html">Team Contributions</a> | 
      <a href="about.html">About / Presentation</a>
    </nav>

    <div class="hero-image-wrapper">
      <img src="assets/images/Gemini_Generated_Image_oipml1oipml1oipm.png" 
           alt="ETL Data Pipeline Visualization" 
           class="blended-hero-image">
    </div>
      
  </div>
</div>
---

## Project Overview

Our INFO-B581 final project implements an **ETL (Extract–Transform–Load) pipeline** that moves clinical data from an **OpenEMR FHIR server** (source) to a **Primary Care FHIR server** (target), using the **Hermes SNOMED CT terminology server** to standardize condition concepts.

<div class="overview-grid">

  <div class="card">
    <h3> Goal</h3>
    <p>
      Build a reproducible ETL pipeline that extracts FHIR resources, transforms them with SNOMED CT 
      parent/child relationships, and loads clean, validated data into a Primary Care FHIR server.
    </p>
  </div>

  <div class="card">
    <h3> Data Sources</h3>
    <ul>
      <li>Patients, Conditions, Observations and Procedures from the OpenEMR FHIR server</li>
      <li>SNOMED CT concepts and relationships from the Hermes terminology server</li>
      <li>Target resources stored in a Primary Care FHIR server</li>
    </ul>
  </div>

<div class="card">
  <h3>Technologies</h3>
  <ul>
    <li><strong>Python 3</strong> (Scripting & ETL)</li>
    <li><strong>PyCharm</strong> (Development IDE)</li>
    <li><strong>Postman</strong> (API Testing & Validation)</li>
    <li><strong>FHIR R4 REST APIs</strong> (JSON)</li>
    <li><strong>Hermes</strong> (SNOMED CT Terminology Server)</li>
    <li><strong>Git & GitHub</strong> (Version Control)</li>
    <li><strong>GitHub Pages + Jekyll</strong> (Documentation Website)</li>
  </ul>
  </div>

</div>

---

## ETL Pipeline at a Glance

The pipeline follows a systematic approach:

1. **Extract** – Retrieve patient, condition, observation and procedure data from the OpenEMR FHIR server
2. **Transform** – Transform condition codes using SNOMED CT parent and child concepts from Hermes
3. **Load** – Store standardized FHIR resources into the Primary Care FHIR server
4. **Validate** – Verify resources against FHIR profile (Patient and Condition)

<p align="center">
  <img src="assets/images/BPMN.png" 
       alt="ETL Project Architecture" 
       style="max-width: 100%; height: auto; border-radius: 10px; box-shadow: 0 4px 15px rgba(0,0,0,0.15); margin: 2rem 0;" />
</p>

---

## Summary of Deliverables

- Python ETL scripts for **Tasks 1–5** in the `src/` folder  
- Validated **Patient** and **Condition** resources using custom profiles  
- Reproducible approach for creating Observations  
- Comprehensive project documentation including:
   - System architecture and ETL pipeline details
   - Detailed coding tasks and implementation
   - Individual team contributions and reflections

---

## Quick Links

- [View ETL Pipeline Documentation](etl_pipeline.html)
- [Read Project Insights](insights.html)
- [Meet the Team](team_contrib.html)
- [About & Presentation](about.html)

---

<style>
/* --- OPTION 1: IMMERSIVE HEADER STYLES --- */

/* A dark container for the whole top section */
.immersive-header-container {
  /* Dark blue/black gradient background matching the image aesthetic */
  background: linear-gradient(135deg, #021129 0%, #072c5e 100%);
  color: #ffffff;
  
  /* Negative margins help it stretch wider than the standard text column */
  margin-left: -1rem; 
  margin-right: -1rem;
  padding: 3rem 1.5rem 1rem 1.5rem;
  
  border-radius: 0 0 20px 20px; /* Rounded bottom corners only */
  box-shadow: 0 10px 30px rgba(0,0,0,0.4); /* Strong shadow for depth */
  margin-bottom: 3rem;
}

@media (min-width: 800px) {
  .immersive-header-container {
     margin-left: -3rem; 
     margin-right: -3rem;
  }
}

.header-content-wrapper {
  max-width: 900px; /* Keep content centered */
  margin: 0 auto;
  text-align: center;
}

/* Typography for the Header */
.glow-text {
  color: #fff;
  text-shadow: 0 0 20px rgba(0, 122, 204, 0.9); /* Blue glow */
  margin-bottom: 1.5rem;
  font-weight: 700;
}

/* Custom Dark Navigation Bar */
.dark-nav {
  background: rgba(255, 255, 255, 0.05); /* Very subtle transparency */
  border: 1px solid rgba(255, 255, 255, 0.15);
  padding: 0.8rem 1.2rem;
  border-radius: 50px; /* Pill shape */
  color: #ccc;
  display: inline-block;
  backdrop-filter: blur(5px); /* Frosted glass effect */
}

.dark-nav strong {
    color: #fff;
    margin-right: 5px;
}

.dark-nav a {
  color: #4db8ff !important; /* Brighter cyan/blue for dark background */
  text-decoration: none;
  padding: 0 5px;
  transition: color 0.3s ease;
}

.dark-nav a:hover {
  color: #ffffff !important;
  text-shadow: 0 0 10px #4db8ff;
  text-decoration: none;
}

/* Hero Image Styling */
.hero-image-wrapper {
  position: relative;
  margin-top: 2.5rem;
  border-radius: 12px;
  /* Glowing halo behind the image */
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

/* --- EXISTING CARD STYLES --- */
.overview-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
  gap: 1.5rem;
  margin: 2rem 0;
}

.card {
  background: #ffffff;
  border-radius: 12px;
  padding: 1.5rem;
  box-shadow: 0 2px 12px rgba(0,0,0,0.1);
  border-left: 4px solid #007acc;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.card:hover {
  transform: translateY(-5px);
  box-shadow: 0 4px 20px rgba(0,0,0,0.15);
}

.card h3 {
  margin-top: 0;
  margin-bottom: 1rem;
  color: #007acc;
}

.card ul {
  margin: 0.5rem 0;
  padding-left: 1.5rem;
}
</style>
