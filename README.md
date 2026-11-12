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
    
# Task V 
## 📊 Executive Power BI Dashboard & Visual Analytics

To operationalize the process mining insights, an interactive, multi-page **Power BI Executive Dashboard** was built for **CareFlow**, translating complex EHR event streams and transition metrics into an intuitive command center for hospital administrators.

### 🏛️ Dashboard Architecture & Multi-Page Design

The dashboard is structured into dedicated analytical views to evaluate emergency room throughput, resource bottlenecks, and administrative friction points at a glance.

### Operational Pulse & Bottleneck Command Center
* **Top-Level KPI Cards:**
  * **Total Patient Volume:** Tracks active throughput (**200 total cases**).
  * **Average Length of Stay (LOS):** Monitors overall visit duration (**20.03 minutes** baseline transition average).
  * **Paperwork Error Rate:** Quantifies administrative friction (**37.30%** error rate).
  * **Critical Loop-Back Count:** Highlights structural workflow failures (**71 instances** of the `X-Ray` $\rightarrow$ `Triage` rework loop).
    
* **Core Visuals:**
  * **The Key Bottleneck Visual:** A transition wait-time bar chart highlighting primary choke points—specifically isolating the heavy queueing delay between `Triage` and `Doctor Consultation` and retrograde movements.
  * **Location & Acuity Distribution:** Slices patient volume across emergency room locations (`Consultation Bay B`, `Triage Room 1`, `Radiology Lab 2`) and ESI Acuity Levels (Levels 2 through 5).
  * **Process Pathway Funnel Chart:** Maps patient progression across milestones (`Check-in` $\rightarrow$ `Triage` $\rightarrow$ `Doctor Consultation` $\rightarrow$ `X-Ray` $\rightarrow$ `Discharge`), clearly visualizing volume counts and stage drop-offs.
  * **Paperwork Impact Analysis:** Directly contrasts complete vs. missing paperwork scenarios to prove how missing documentation inflates wait times.
  * **Doctor Performance Matrix:** Evaluates individual physician workloads (`Dr. Adams`, `Dr. Chen`, `Dr. House`, `Dr. Robert`, `Dr. Smith`), tracking patient case counts and average wait durations per attending doctor.
  * **Demographic Breakdown:** Visualizes patient age distribution segmented by gender.

### 🎛️ Interactive Filtering & Slicing Capabilities
The dashboard incorporates dynamic global filters enabling hospital managers to drill down into specific operational subsets instantly:
1. **Acuity Level Slicer:** Isolate performance metrics by patient urgency (ESI Levels 2–5).
2. **Attending Doctor Slicer:** Review specific physician consultation queues and workload distribution.
3. **Paperwork Missing Flag (`True` / `False`):** Filter the entire canvas to inspect how incomplete intake records impact overall hospital efficiency.

### 💡 Executive Impact
By leveraging this multi-page Power BI dashboard, clinical supervisors can move beyond static reports—instantly identifying that missing intake paperwork is the root cause of 37%+ of administrative delays and using data-driven insights to optimize triage staffing and physician shift allocations.

# TASK VI
# Summary & Recommendations

## 📋 Project Summary

The **CareFlow Clinical Pathway Process Mining** project successfully bridged the gap between raw Electronic Health Record (EHR) data and actionable hospital operations management. By replacing misleading aggregate BI metrics (such as blended average wait times) with a rigorous event-log architecture powered by **Python (`pandas`, `pm4py`)** and visualized in **Power BI**, the project exposed the hidden non-linear realities of Emergency Room operations.

### Key Findings Recap:
* **Throughput Volume:** Analyzed **200 patient cases** spanning **1,142 chronological event logs** across diverse ESI Acuity Levels (Levels 2–5).
* **The Administrative Rework Loop:** Discovered that **37.3%** of patient journeys suffered from missing intake paperwork, forcing **71 patient visits** to loop backward from `X-Ray` back to `Triage` and adding an average of **21.38 minutes** of redundant wait time per incident.
* **Primary Resource Bottleneck:** Quantified the longest primary queueing delay occurring between `Triage` and `Doctor Consultation`, averaging **33.47 minutes** across 271 occurrences, pointing directly to consultation-bay and physician availability constraints.
* **Executive Command Center:** Deployed a multi-page Power BI dashboard equipped with dynamic ESI acuity slicers, doctor performance matrices, and paperwork impact filters to provide clinical leadership with real-time operational visibility.
  
## 💡 Strategic Recommendations for Hospital Administrators

To eliminate administrative friction, reclaim operational capacity, and reduce overall patient Length of Stay (LOS), hospital management should implement the following data-driven interventions:

### 1. Digitize and Enforce Intake Verification at Triage
* **The Problem:** 37.3% paperwork error rates and 71 retrograde loops (`X-Ray` $\rightarrow$ `Triage`) are entirely preventable administrative failures.
* **The Action:** Implement mandatory digital validation checks on tablet-based check-in kiosks or triage workstations. The system should block progression to radiology or consultation until all mandatory intake paperwork fields are locked and verified, eliminating the root cause of the rework loop.

### 2. Reallocate Staffing and Optimize Consultation Bay Capacity
* **The Problem:** The 33.47-minute average delay between `Triage` and `Doctor Consultation` is the single largest operational choke point in the emergency department.
* **The Action:** Dynamic staffing models should be introduced during peak admission hours. Adjust physician shift overlaps and open fast-track consultation bays specifically for ESI Level 4 and Level 5 patients to accelerate throughput and relieve physician backlog.

### 3. Operationalize Process Mining for Continuous Monitoring
* **The Problem:** Static reports obscure operational drift and emerging workflow bottlenecks.
* **The Action:** Integrate the CareFlow Power BI dashboard into daily shift handoffs. Clinical supervisors should actively monitor live rework rates and transition wait times to proactively adjust resource allocations before emergency room congestion peaks.


