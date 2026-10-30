# CareFlow-Clinical-Pathway-Process-Mining

## 📋 Executive Summary
Hospital administrators often struggle with high Emergency Room (ER) wait times because standard BI dashboards only show aggregate averages ( for example "average wait time: 42 minutes"). They fail to capture the chaotic, non-linear sequences of actual patient journeys. 

**CareFlow** solves this by treating healthcare operations as an **event-log architecture**. By ingesting raw chronological event data from Electronic Health Record (EHR) systems and processing them, CareFlow exposes hidden organizational friction.

# PHASE I
## 🎯 Aim of the Project

The **CareFlow** project leverages process mining techniques to bridge the gap between raw Electronic Health Record (EHR) data and operational healthcare decision-making.

- **Eliminate Operational Blind Spots:** Go beyond standard static Business Intelligence (BI) dashboards that only display high-level averages (e.g., overall wait times) to uncover the real-world, highly variable paths patient journeys actually follow.
  
- **Reconstruct Actual Patient Pathways:** Ingest raw, timestamped event logs from EHR systems to automatically reconstruct and visualize end-to-end patient flows using Python's `pm4py` library.
  
- **Identify Hidden Operational Bottlenecks:** Pinpoint specific root causes of delay and process loops (such as patients being re-routed to Triage after X-Ray due to incomplete paperwork).
  
- **Measure Step-by-Step Transition & Wait Times:** Calculate precise, node-to-node duration metrics between specific clinical stages (e.g., exact time elapsed from *Triage* to *Doctor Consultation*).

## 📌 Problem Definition

Healthcare organizations lack visibility into real-world patient workflows because traditional Business Intelligence metrics rely on aggregate averages that obscure operational delays. Standard reporting tools fail to track complex, non-linear patient paths, leaving systemic friction—such as redundant departmental loops and prolonged transition wait times—unidentified, unaddressed, and unmanaged by hospital administrators.

## About the dataset
* Domain: Healthcare Operations & Process Analytics
* Dataset Source Code: df = pd.read_csv("careflow_synthetic_ehr_logs.csv")
* Data Type: Comma Separated Values 
* Rows:1142
* Columns:10 
  
##Key features evaluated include:
* Temporal & Spatial Data: timestamp, location, resource
* Patient Demographics: patient_age, patient_gender, acuity_level
* Clinical Operations: attending_doctor, paperwork_missing_flag, activity
