# CareFlow-Clinical-Pathway-Process-Mining

## 📋 Executive Summary
Hospital administrators often struggle with high Emergency Room (ER) wait times because standard BI dashboards only show aggregate averages ( for example "average wait time: 42 minutes"). They fail to capture the chaotic, non-linear sequences of actual patient journeys. 

**CareFlow** solves this by treating healthcare operations as an **event-log architecture**. By ingesting raw chronological event data from Electronic Health Record (EHR) systems and processing them, CareFlow exposes hidden organizational friction.

# TASK I
## 🎯 Aim of the Project

The **CareFlow** project leverages process mining techniques to bridge the gap between raw Electronic Health Record (EHR) data and operational healthcare decision-making.

- **Eliminate Operational Blind Spots:** Go beyond standard static Business Intelligence (BI) dashboards that only display high-level averages (e.g., overall wait times) to uncover the real-world, highly variable paths patient journeys actually follow.
  
- **Reconstruct Actual Patient Pathways:** Ingest raw, timestamped event logs from EHR systems to automatically reconstruct and visualize end-to-end patient flows using Python's `pm4py` library.
  
- **Identify Hidden Operational Bottlenecks:** Pinpoint specific root causes of delay and process loops (such as patients being re-routed to Triage after X-Ray due to incomplete paperwork).
  
- **Measure Step-by-Step Transition & Wait Times:** Calculate precise, node-to-node duration metrics between specific clinical stages (e.g., exact time elapsed from Triage to Doctor Consultation).

## 📌 Problem Definition

Healthcare organizations lack visibility into real-world patient workflows because traditional Business Intelligence metrics rely on aggregate averages that obscure operational delays. Standard reporting tools fail to track complex, non-linear patient paths, leaving systemic friction—such as redundant departmental loops and prolonged transition wait times—unidentified, unaddressed, and unmanaged by hospital administrators.

## About the dataset
* Domain: Healthcare Operations & Process Analytics
* Dataset Source Code: df = pd.read_csv("careflow_synthetic_ehr_logs.csv")
* Data Type: Comma Separated Values 
* Rows:1142
* Columns:10 
  
## Key features evaluated include:
* Temporal & Spatial Data: timestamp, location, resource
* Patient Demographics: patient_age, patient_gender, acuity_level
* Clinical Operations: attending_doctor, paperwork_missing_flag, activity

## Steps Required:
1.Data Loading and Initial Overview

2.Data Pre-processing

3.Exploratory Data Analysis (EDA)

4.Visualizations

5.Insight Generation and Report

# TASK II
## Dataset Loading and  Overview:
* Total no. of rows:1141
* Total no. of columns:10 columns

## Data Quality Assessment:
* Count of non-null values: 1141
* No duplicate columns are found.
* The dataset is consistent throughout.
* Range index: 0 to 1141
* Datatype issues are found: The datatype of timestamp is wrongly given as 'object' instead of 'datetime'.
* Memory usage: 81.5+ KB

## Patient Age Summary Statistics:
* Average of patient age: 54.05 years
* Median value: 56.00 years
* Standard deviation: 19.22 years
* Minimum value of patient age: 18.00 years
* Maximum value of patient age: 85.00 years

## Data Pre-processing part Overview:
* The dataset is devoid of missing values.
* No duplicate rows are detected.
* Converted the timestamp column from an object format to a proper datetime datatype.
* Extracted date and time values into separate attributes from the main timestamp.
* Filtered out a summary table, missing significant paperwork, to ensure data integrity..
* Data values are aggregated based on:
  
    1.Patient Case-Level Summary (Duration & Activity Count).
  
    2.Resource & Staff Workload.
  
    3.Acuity Level & Demographics Breakdown.
  
    4.Activity & Location Bottleneck.
  
# TASK III
## 🗺️ Event Normalization & Process Discovery

To transition from flat tabular logs to a true process-mining architecture, the normalized dataset was ingested into **PM4Py** (the industry-standard process mining library in Python). 

### ⚙️ Implementation & Pipeline
1. **Data Formatting:** Configured the event log schema by mapping PM4Py parameters (`case_id`, `activity_key="activity"`, and `timestamp_key="timestamp"`).
2. **Process Discovery:** Applied the **Directly-Follows Graph (DFG)** discovery algorithm to map out the actual chronological movement of patients through the emergency room.
3. **Artifact Generation:** Successfully compiled and rendered the "As-Is" Spaghetti Diagram output as `careflow_as_is_process_map.png`.

## 🔍 Architectural Insight
By transforming static EHR event rows into a Directed Graph, the pipeline bypasses traditional BI limitations. Instead of viewing isolated timestamps, the resulting visualization exposes the true, non-linear routing of emergency room patients—providing the foundational architecture needed to spot structural loops and workflow friction in subsequent tasks.

# TASK IV
## 📊 Bottleneck & Transition Time Analytics

By analyzing the chronological event sequences in our simulated EHR dataset, **CareFlow** successfully unmasked the hidden operational friction inside the Emergency Room. Instead of relying on misleading aggregate averages, our transition-time matrix exposes exactly where patient flow breaks down.

### 🔍 Key Operational Findings

1. **The Hidden Rework Loop (`X-Ray` $\rightarrow$ `Triage`):** 
   * **71 patient visits** were forced to loop backward from **X-Ray** back to **Triage** due to missing intake documentation.
   * This retrograde movement adds an average of **21.38 minutes** of redundant waiting time per affected patient, creating unnecessary congestion for incoming cases.
2. **Primary Capacity Bottlenecks:** 
   * The longest transition delay in the hospital occurs between **Triage** and **Doctor Consultation**, averaging **33.47 minutes** across 271 occurrences. This points to a severe physician availability or consultation-bay bottleneck.
3. **Discharge and Diagnostic Delays:** 
   * Post-consultation and post-X-ray handoffs to `Discharge` both hover around **27 to 28 minutes**, indicating administrative or transport delays before final exit clearances.

### 📈 Transition Wait-Time Summary (Computed Output)

Below is the exact analytical output generated by our transition script, detailing wait times and frequencies across every hospital pathway:

| From Activity | To Activity | Total Occurrences | Avg Wait (Minutes) | Median Wait (Minutes) | Bottleneck Status |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Triage** | **Doctor Consultation** | **271** | **33.47** | **31.0** | 🔴 **Critical Resource Bottleneck** |
| Doctor Consultation | Discharge | 71 | 28.39 | 29.0 | 🟡 Moderate Delay |
| X-Ray | Discharge | 129 | 27.33 | 26.0 | 🟡 Moderate Delay |
| **X-Ray** | **Triage** | **71** | **21.38** | **21.0** | 🔴 **Critical Bottleneck (Rework Loop)** |
| Doctor Consultation | X-Ray | 200 | 20.86 | 21.0 | 🟡 Moderate Delay |
| Check-in | Triage | 200 | 12.84 | 13.0 | 🟢 Normal Flow |

### 💡 Executive Takeaway for Hospital Administrators

Standard BI dashboards typically display an unhelpful blended average for the entire visit. **CareFlow's event-log architecture proves that delays are structurally fragmented:**
* **The Structural Flaw:** The 71 instances of patients cycling backward from `X-Ray` to `Triage` prove that a broken intake form is actively wasting over 21 minutes per incident.
* **The Resource Constraint:** The 33+ minute wait between `Triage` and `Doctor Consultation` reveals that staffing allocations need to be adjusted right at the consultation bays.

# TASK V
### 📈 Power BI Executive Dashboard Integration

To bridge the gap between back-end data pipelines and executive decision-making, the normalized EHR event logs and transition metrics were integrated directly into **Power BI**, establishing a dynamic, executive-ready operational command center for hospital leadership.

### 🏛️ Dashboard Architecture & Design

The Power BI workspace is structured around key operational dimensions, utilizing the cleaned dataset (`careflow_cleaned_ehr_logs.xlsx`) to answer critical administrative questions regarding throughput, wait times, and bottleneck locations:

1. **Top-Level KPI Cards (Operational Pulse):**
   * **Total Patient Volume:** Tracks overall throughput across active cases (**200 unique patient cases**).
   * **Average Length of Stay (LOS):** Monitors the complete patient journey from initial check-in to final discharge.
   * **Rework / Paperwork Error Rate:** Quantifies administrative friction (**37.3%** of events flagged with missing documentation).
   * **Critical Loop-Back Count:** Highlights structural workflow failures (specifically tracking the **71 instances** of the `X-Ray` $\rightarrow$ `Triage` loop).

2. **Core Visual Components:**
   * **Process Pathway Funnel / Flow:** Traces patient volume step-by-step from `Check-in` $\rightarrow$ `Triage` $\rightarrow$ `Doctor Consultation` $\rightarrow$ `X-Ray` $\rightarrow$ `Discharge`.
   * **Transition Bottleneck Matrix:** Combines `activity` and `next_activity` with average transition wait times, using conditional formatting to flag severe delays (such as the **33.47-minute** wait between `Triage` and `Doctor Consultation`).
   * **Patient Acuity Distribution (ESI Tiers 2–5):** Evaluates incoming patient severity evenly spread across acuity levels to monitor resource strain.

---

### 📐 DAX Core Measures Implemented

To power dynamic filtering, cross-highlighting, and KPI evaluations within the model, custom DAX measures were established:

```dax
// 1. Total Patient Volume Measure
Total_Patients = DISTINCTCOUNT(careflow_cleaned_ehr_logs[case_id])
 


