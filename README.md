# Healthcare-Operations-Revenue-BI-Dashboard
End-to-end Healthcare Operations &amp; Revenue BI Dashboard built with Power BI and DAX. Features an AI-driven data extraction and automated cleaning pipeline that prepares normalized CSV schemas to analyze clinical throughput, doctor performance, and billing realization.
# 🏥 Healthcare Operations & Revenue Analytics Dashboard

An end-to-end Business Intelligence solution developed in **Power BI** to monitor operational throughput, doctor performance, patient demographics, and revenue realization across a multi-department clinic network.

---

## 📌 Project Overview
This project processes, models, and visualizes transactional healthcare data across 5 relational entities: Appointments, Billing, Doctors, Patients, and Treatments. 

Raw, unstructured clinic records were processed through an **AI-driven data pipeline** to automate data cleansing, schema standardization, and CSV ingestion prior to semantic modeling in Power BI.

---

## 🤖 AI Data Cleaning & Transformation Pipeline
Before data modeling, an automated AI pipeline was implemented to handle raw transaction anomalies and prepare production-ready datasets:

* **Automated Data Sanitization:** Leveraged AI workflows to clean corrupted records, standardize conflicting date/time timestamps, and reconcile patient identification tags.
* **Schema Restructuring:** Parsed unstructured inputs into 5 clean, normalized CSV schemas (`appointments.csv`, `bills.csv`, `doctors.csv`, `patients.csv`, and `treatments.csv`).
* **Data Integrity Checks:** Enforced primary key and foreign key relational constraints to prevent circular dependencies before loading into Power Query.

---

## 🏗️ Data Architecture & Star Schema
The semantic data model is designed around a single-direction relational schema to eliminate circular filters and optimize DAX engine performance:

- **Fact Tables / Core Transactions:** `appointments`, `treatments`, `bills`
- **Dimension Tables:** `patients`, `doctors`
- **Model Hierarchy:** 
  `[patients]` & `[doctors]` $\rightarrow$ `[appointments]` $\rightarrow$ `[treatments]` $\rightarrow$ `[bills]`

---

## 📊 Dashboard Structure

### 1. Executive KPI Overview
- **High-Level Metrics:** Total Appointments, Completed Visits, Invoiced Revenue, and Collection Efficiency.
- **Monthly Trajectory:** Time-series trends of patient visit volumes.
- **Financial Status:** Proportional breakdown of Paid vs. Pending vs. Failed billings.

### 2. Patient Demographics & Cohorts
- **Cohort Profiling:** Distribution across age brackets, gender splits, and regional locations.
- **Payor Mix:** Patient volume mapped across primary insurance providers.

### 3. Clinical & Revenue Operations
- **Physician Scorecard:** Matrix evaluating doctor appointment load, completion rates, and billable revenue.
- **Treatment Analysis:** High-cost vs. routine procedural volume breakdown.
- **Operational Loss:** Slot utilization across scheduled, cancelled, and no-show statuses.

---

## 📐 Key DAX Measures

```dax
// Completion Rate %
Completion Rate % = 
DIVIDE(
    CALCULATE(COUNTROWS(appointments), appointments[status] = "Completed"),
    COUNTROWS(appointments),
    0
)

// Collection Efficiency %
Collection Efficiency % = 
DIVIDE(
    CALCULATE(SUM(bills[amount]), bills[payment_status] = "Paid"),
    SUM(bills[amount]),
    0
)
