---
layout: default
title: Team Contributions
---

<nav style="background: #f5f5f5; padding: 1rem; border-radius: 8px; margin-bottom: 2rem;">
  <strong>Navigation:</strong> 
  <a href="index.html">Home</a> | 
  <a href="etl_pipeline.html">ETL Pipeline</a> | 
  <a href="insights.html">Insights</a> | 
  <a href="team_contrib.html">Team Contributions</a> | 
  <a href="about.html">About / Presentation</a>
</nav>

# Team Contributions

---

## Team Roles & Responsibilities

Our team collaborated on the design, implementation, validation, and documentation of an ETL pipeline integrating **OpenEMR FHIR**, **Hermes SNOMED CT**, a **Primary Care EHR FHIR server**, and **HL7 v2 message generation**.

Each member took primary ownership of specific coding tasks (1–5), while also reviewing and testing one another's work to ensure consistency and quality.

---

<div class="team-grid">

<div class="team-card" markdown="1">
<div class="member-header">
  <img src="assets/images/sakshi.jpeg" alt="Sakshi Mehta" class="profile-pic">
  <div>
    <h2>👤 Sakshi Mehta</h2>
    <p class="role">ETL Setup & Task 1 Lead</p>
  </div>
</div>

### **Main Responsibilities**
* Set up the project structure, virtual environment, and Git repository
* Implemented **Coding Task 1** (patient & parent condition ETL) and common helper functions for FHIR & API calls
* Helped write the **Home & Project Overview** section of the website and described the overall ETL pipeline

### **Reflection**
> "I learned how to set up a real ETL project from scratch and connect to a FHIR server. Working on the overview page helped me see the big picture of how all the tasks fit together."
</div>

<div class="team-card" markdown="1">
<div class="member-header">
  <img src="assets/images/lakshitha.jpeg" alt="FNU Lakshitha" class="profile-pic">
  <div>
    <h2>👤 FNU Lakshitha</h2>
    <p class="role">Terminology & Validation Lead</p>
  </div>
</div>

### **Main Responsibilities**
* Implemented **Coding Task 2** (child condition ETL) using SNOMED parent/child concepts and Hermes
* Wrote and refined validation scripts for Task 1 and Task 2 using custom FHIR profiles and `$validate`
* Helped write the **ETL Pipeline Documentation** page, especially the Extraction & Transformation sections and Task 1-2 descriptions

### **Reflection**
> "This project taught me how clinical codes like SNOMED and ICD-10 are used in practice. I also saw how important validation and clear documentation are for making the pipeline understandable."
</div>

<div class="team-card" markdown="1">
<div class="member-header">
  <img src="assets/images/bhavitha.jpeg" alt="Bhavitha Asam" class="profile-pic">
  <div>
    <h2>👤 Bhavitha Asam</h2>
    <p class="role">Observation & Procedure ETL Developer</p>
  </div>
</div>

### **Main Responsibilities**
* Implemented **Coding Task 3** (blood pressure Observation ETL) and **Task 4** (Procedure ETL)
* Added extract-transform-load logic, file saving, and basic verification (reading back created resources)
* Contributed to the **ETL Pipeline Documentation** page for Tasks 3-4 and helped create content for **Insights page** and visualizations

### **Reflection**
> "Working on Observations and Procedures helped me get comfortable with FHIR resource structures and ETL patterns. I liked turning raw JSON responses into something we could explain and visualize on the website."
</div>

<div class="team-card" markdown="1">
<div class="member-header">
  <img src="assets/images/aravind.jpeg" alt="Aravind Kuruvikkattil Venugopalan" class="profile-pic">
  <div>
    <h2>👤 Aravind Kuruvikkattil Venugopalan</h2>
    <p class="role">HL7 & Website / Presentation Lead</p>
  </div>
</div>

### **Main Responsibilities**
* Implemented **Coding Task 5** (HL7 ADT message generation from FHIR data) using `hl7apy` and Hermes mappings
* Organized screenshots, outputs, and wrote most of the **About & Presentation** page content
* Created the **Team Contributions** page and helped polish the **Home page** for readability and flow

### **Reflection**
> "I enjoyed translating FHIR data into HL7 v2 and then explaining it in simple language on the website. Helping with the final presentation content and layout made the project feel complete and professional."
</div>

</div>

---

## 📊 Contributions Summary Table

| Team Member | Role | Primary Tasks | Key Deliverables |
|-------------|------|---------------|------------------|
| Sakshi Mehta | ETL Setup & Task 1 Lead | Task 1, Project Setup | Patient creation, Parent Condition, Project structure, Home page |
| FNU Lakshitha | Terminology & Validation Lead | Task 2, Validation | Child Condition, Validation scripts, ETL Documentation |
| Bhavitha Asam | Observation & Procedure ETL Developer | Task 3, Task 4 | Blood Pressure Observation, Procedure, Insights page |
| Aravind Kuruvikkattil Venugopalan | HL7 & Website / Presentation Lead | Task 5, Website | HL7 ADT message, Presentation content, Team Contributions page |

---

## 🤝 Summary of Teamwork

<div class="summary-box">

✅ <strong>Shared Ownership of Tasks 1–5</strong> – Each member led one or more coding tasks but all scripts were reviewed by the group.<br>
✅ <strong>Consistent ETL Design</strong> – The same patient and Condition data were reused across tasks to build a unified interoperability story.<br>
✅ <strong>Collaborative Debugging</strong> – We worked together on token issues, Hermes edge cases, validation errors, and JSON structure fixes.<br>
✅ <strong>Aligned with Grading Criteria</strong> – Our code, website, diagrams, and documentation were planned together to meet the project rubric.

</div>

---

## 🎓 Collective Lessons Learned

**Technical Skills:**
* FHIR resource structure and API interactions
* SNOMED CT terminology hierarchies and parent/child relationships
* OAuth2 authentication and secure token management
* Python ETL pipeline development with error handling
* HL7 v2 message construction and segment mapping
* Resource validation against FHIR profiles

**Collaboration Skills:**
* Clear communication is essential for distributed work
* Regular check-ins prevent integration issues
* Documentation should be written alongside code
* Code reviews improve overall quality and catch errors early

---

<style>
/* CHANGED: Team Grid is now a Flex Column (Row by Row) 
*/
.team-grid {
  display: flex;
  flex-direction: column; /* This forces them into a vertical stack */
  gap: 2.5rem;            /* Space between each card */
  margin: 2rem 0;
}

/* The 3D Animation Card Style */
.team-card {
  background: #ffffff;
  border-radius: 15px;
  padding: 2rem;
  box-shadow: 0 4px 15px rgba(0,0,0,0.1);
  border-top: 5px solid #007acc; /* Blue accent top */
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

/* The Hover Animation Effect */
.team-card:hover {
  transform: translateY(-5px); /* Moves up slightly */
  box-shadow: 0 10px 20px rgba(0,0,0,0.15); /* Casts deeper shadow */
}

/* Member Header Layout */
.member-header {
  display: flex;
  align-items: center;
  gap: 1.5rem;
  margin-bottom: 1.5rem;
  padding-bottom: 1rem;
  border-bottom: 2px solid #f0f0f0;
}

/* Profile Picture Style */
.profile-pic {
  width: 100px;
  height: 100px;
  border-radius: 50%;
  object-fit: cover;
  border: 4px solid #007acc;
  box-shadow: 0 4px 10px rgba(0,0,0,0.15);
}

.member-header h2 {
  margin: 0;
  color: #007acc;
  font-size: 1.5rem;
}

.role {
  color: #555;
  font-weight: bold;
  font-size: 1rem;
  margin: 0.5rem 0 0 0;
}

/* Reflection Quote Style */
.team-card blockquote {
  background: #f9f9f9;
  border-left: 4px solid #007acc;
  padding: 1rem;
  margin: 1rem 0 0 0;
  font-style: italic;
  color: #555;
  border-radius: 0 8px 8px 0;
}

/* Summary Box Gradient */
.summary-box {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  padding: 2rem;
  border-radius: 12px;
  font-size: 1.1rem;
  line-height: 1.8;
  box-shadow: 0 4px 15px rgba(0,0,0,0.2);
}
</style>
