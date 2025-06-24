# **Table of Contents**
* [Project Name](#project-name)
* [Project Background](#project-background)
* [Project Goals](#project-goals)
* [Insights and Recommendations](#insights-and-recommendations)
  * [Category 1: Appointment Outcomes & Wait Times](#category-1-appointment-outcomes--wait-times)
  * [Category 2: Provider Efficiency & Operational Risk](#category-2-provider-efficiency--operational-risk)
  * [Category 3: Claims Denials & Financial Exposure](#category-3-claims-denials--financial-exposure)
  * [Category 4: Insurance Plan & Regional Performance](#category-4-insurance-plan--regional-performance)
* [Data Collection and Sources](#data-collection-and-sources)
* [Formal Data Governance](#formal-data-governance)
* [Regulatory Reporting](#regulatory-reporting)
* [Methodology](#methodology)
* [Data Structure & Initial Checks](#data-structure--initial-checks)
* [Documenting Issues](#documenting-issues)
* [Executive Summary](#executive-summary)
* [Insights Deep Dive](#insights-deep-dive)
* [Recommendations](#recommendations)
* [Future Work](#future-work)
* [Technical Details](#technical-details)
* [Assumptions and Caveats](#assumptions-and-caveats)
---

## **Project Name**
**Saudi National Health Insurance – Healthcare Appointment & Claims Efficiency Analysis**

---

## **Project Background**
Saudi National Health Insurance (SNHI) is a key government-backed insurer serving over 5 million citizens and residents since 2018. SNHI partners with thousands of clinics, hospitals, and specialized providers across major Saudi cities such as Riyadh, Jeddah, Khobar, and Dammam.

SNHI manages an integrated digital health claims and appointment booking ecosystem. Over 50,000 appointment records and 250,000+ healthcare claim records are processed annually. The analysis below was conducted to identify operational bottlenecks, minimize financial inefficiencies, and optimize patient flow across its network.

---

## **Project Goals**
As the lead data analyst, my goals were to:
* Quantify and reduce no-shows, cancellations, and long wait times that degrade appointment performance
* Identify high-risk providers with mismatched specialties or inefficient scheduling practices
* Detect claims leakage caused by denial errors, improper overpayments, and pre-authorization gaps
* Benchmark performance by city, provider, and insurance tier to implement targeted efficiency strategies

---

## **Insights and Recommendations**

### **Category 1: Appointment Outcomes & Wait Times**
#### Key Insights:
* Completed appointments: 32,481 (65%)
* No-shows: 5,034 (10%), **strongly correlated with wait times** (avg 67.99 mins)
* Follow-ups & consultations had the **highest no-show and cancellation wait times**
* Emergency visits had **lowest wait times and highest completion efficiency**

#### Recommendations:
* Implement SMS reminders + proactive rescheduling for appointments exceeding 67 minutes
* Introduce 15-min buffer slots for follow-ups and consultations
* Replicate emergency department triage speed in other appointment types

---

### **Category 2: Provider Efficiency & Operational Risk**
#### Key Insights:
* 23,938 unique providers, **65% handled only one appointment**
* Over 1,100 providers averaged the **maximum 120-min wait**
* Provider PRV007848 had a 75% cancellation rate, **handling complex surgeries inappropriately at imaging centers**

#### Recommendations:
* Reassign providers with 15-min avg wait to consultation-heavy departments
* Suspend providers with >50% cancellation rate pending retraining
* Consolidate appointments from 15,000+ single-appointment providers to improve patient continuity

---

### **Category 3: Claims Denials & Financial Exposure**
#### Key Insights:
* **SAR 75.3M paid in error**; **SAR 55.9M paid on claims without denial codes**
* Top denial reasons:
  * Pre-existing conditions: 41.8%
  * Non-covered services: 29.6%
  * Insufficient documentation: 18.9%
* Submission delays range 0–29 days, disrupting financial forecasting

#### Recommendations:
* Launch 90-day overpayment recovery & voluntary repayment initiative
* Enforce mandatory denial reason codes before approval
* Penalize claim submissions >7 days post-visit with 5% reimbursement cut

---

### **Category 4: Insurance Plan & Regional Performance**
#### Key Insights:
* **Bupa Premium** had the **highest wait times (68.7 mins)** despite being top-tier
* **Dammam** showed the **worst surgical wait times (67.8 mins)**
* **Khobar** had high overall waits despite mid-volume patient load
* **SA Cares** consistently delivered highest coverage efficiency

#### Recommendations:
* Guarantee ≤60-min wait for Bupa Premium via contract renegotiation
* Expand Taif’s low-wait surgeon scheduling model to Dammam
* Launch \$25 credit reactivation program targeting 7,507 inactive patients

---

## **Data Collection and Sources**
Data was extracted from the SNHI relational claims and appointment databases. No APIs were used. Manual CSV ingestion was performed for supplementary patient info (insurance tier, city, specialty) and claim denial mappings.

---

## **Formal Data Governance**
* Column standardization for provider IDs, date formatting (ISO 8601), and appointment outcomes
* Null value handling: rescheduling dates and wait times cleaned and verified against actual timestamps
* Proposed governance: implement denial reason validation logic + timestamp auto-auditing

---

## **Regulatory Reporting**
Compliance issues were flagged, especially in overpaid claims with no denial code (SAR 55.9M). System redesign is recommended to enforce documentation compliance and reduce legal exposure. A regulator-aligned denial taxonomy should be implemented.

---

## **Methodology**
* SQL used for data cleaning, joining, transformation
* Statistical analysis (mean, correlation r-values, variance decomposition) for wait time and claims
* Tableau dashboards built for city/provider/insurance segmentation
* Conditional logic for risk flagging (e.g., cancellation % > 50, wait time > 90 mins)

---

## **Data Structure & Initial Checks**

### 📅 Table: `appointments`
| **Column Name**       | **Business Meaning**                                       | **Business Benefit for Data Analyst**                          |
|-----------------------|------------------------------------------------------------|----------------------------------------------------------------|
| `appointment_id`      | Unique ID for each patient appointment                     | Tracks appointment lifecycle and status analysis               |
| `patient_id`          | Links appointment to patient profile                       | Enables segmentation by demographics/insurance                 |
| `appointment_type`    | Visit type: Consultation, Emergency, etc.                  | Compare no-show rates by service category                      |
| `provider_id`         | Doctor/care provider identifier                            | Supports provider efficiency evaluation                        |
| `wait_time`           | Minutes between scheduling and service delivery            | Key metric for operational efficiency                          |
| `outcome`             | Status: Completed, Cancelled, No-show                      | Enables performance tracking and root cause analysis           |
| `city`                | Appointment location                                       | Supports regional benchmarking                                 |
| `scheduled_time`      | Original scheduled time                                    | Analyze scheduling delays and slot optimization                |
| `actual_time`         | Actual service start time                                  | Calculate actual wait and punctuality metrics                  |

### 🧑‍⚕️ Table: `providers`
| **Column Name**       | **Business Meaning**                                 | **Business Benefit for Data Analyst**                     |
|-----------------------|------------------------------------------------------|-----------------------------------------------------------|
| `provider_id`         | Unique medical provider identifier                   | Analyze provider performance and patient load             |
| `specialty`           | Medical field (Cardiology, Surgery, etc.)            | Identify mismatches and staffing needs                    |
| `facility_type`       | Facility type (Clinic, Imaging Center, etc.)         | Facility-level performance insights                       |
| `avg_wait_time`       | Provider's average service wait time                 | Detect bottlenecks and overbooking trends                 |
| `city`                | Provider's geographic location                       | Compare provider performance across regions               |

### 💸 Table: `claims`
| **Column Name**       | **Business Meaning**                           | **Business Benefit for Data Analyst**                        |
|-----------------------|------------------------------------------------|-------------------------------------------------------------|
| `claim_id`            | Unique claim identifier                        | Core key for financial data tracking                        |
| `patient_id`          | Patient associated with claim                  | Analysis by patient demographics/plan                       |
| `provider_id`         | Claim-submitting provider                      | Holistic performance review                                 |
| `amount_claimed`      | Total amount billed                            | Trend analysis and overbilling detection                    |
| `amount_paid`         | Final reimbursed amount                        | Payout accuracy checks and policy assessments               |
| `denial_reason`       | Claim rejection code/explanation               | Critical for denial trend analysis                          |
| `insurance_plan`      | Plan tier for claim submission                 | Evaluate claim volume by plan                               |

### 🧍 Table: `patients`
| **Column Name**       | **Business Meaning**                     | **Business Benefit for Data Analyst**                  |
|-----------------------|------------------------------------------|--------------------------------------------------------|
| `patient_id`          | Unique patient identifier                | Core join key for cross-table analysis                 |
| `dob`                 | Patient date of birth                    | Age segmentation and eligibility validation            |
| `insurance_plan`      | Health plan tier (Bupa Premium, etc.)    | Cost-to-serve analysis and utilization trends          |
| `status`              | Active/Inactive status                   | Track churn and reactivation opportunities            |
| `registration_date`   | Patient join date                        | Member tenure analysis and engagement scoring          |

### 🧾 Table: `claim_details_data`
| **Column Name**       | **Business Meaning**                                       | **Business Benefit for Data Analyst**                      |
|-----------------------|------------------------------------------------------------|------------------------------------------------------------|
| `detail_id`           | Unique claim line item ID                                  | Granular service-level cost analysis                       |
| `claim_id`            | Parent claim identifier                                    | Group line items under single claim                        |
| `diagnosis_code`      | Medical diagnosis code (ICD-10)                            | Disease prevalence and cost-of-care analysis               |
| `procedure_code`      | Medical procedure code (CPT/HCPCS)                         | Service mix analysis and cost benchmarking                 |
| `quantity`            | Units of service provided                                  | Utilization rate analysis                                  |
| `unit_cost`           | Cost per service unit                                      | Cost-per-service analysis                                  |

### 🩺 Table: `diagnosis`
| **Column Name**       | **Business Meaning**                                 | **Business Benefit for Data Analyst**                  |
|-----------------------|------------------------------------------------------|--------------------------------------------------------|
| `diagnosis_code`      | Standardized medical condition code                  | Disease prevalence and resource allocation             |
| `description`         | Textual diagnosis explanation                        | Enhances report readability                            |
| `category`            | Diagnosis classification group                       | Trend analysis by specialty                            |

### 🛠️ Table: `procedures`
| **Column Name**       | **Business Meaning**                                 | **Business Benefit for Data Analyst**                  |
|-----------------------|------------------------------------------------------|--------------------------------------------------------|
| `procedure_code`      | Standardized medical procedure code                  | Procedure frequency and cost analysis                  |
| `description`         | Procedure explanation                                | Improves report interpretability                       |
| `allowed_amount`      | Maximum insurer-approved payment                     | Detect pricing outliers                                |

---

## **Documenting Issues**
| Table        | Column          | Issues                                  | Magnitude | Solvable | Resolutions                                   |
|--------------|-----------------|-----------------------------------------|-----------|----------|-----------------------------------------------|
| claims       | denial_reason   | 3,591 claims missing denial reason      | High      | Yes      | Enforce non-null constraint                   |
| patients     | status          | Inactive patients had appointments      | Low       | Yes      | Sync patient status with booking system       |

---

## **Executive Summary**
**(Stakeholder: Claims & Operations Director)**  
Appointment bottlenecks and claim overpayments are costing SNHI millions. The biggest risks come from:
- Surgeries with excessive wait times  
- Providers handling mismatched cases  
- Poorly governed claim denial systems  

> **Three Critical Takeaways:**  
> 1. Reducing wait time by **1 minute** may eliminate **250 no-shows**  
> 2. Over **SAR 75M in claim payments were erroneous/unjustified**  
> 3. Top-tier insurance patients receive **worse service** than economy-tier  

---

## **Insights Deep Dive**

### **Category 1: Appointment Outcomes & Wait Times**
* **10% of appointments (5,034) resulted in no-shows** with avg wait of 67.99 mins  
* **Follow-up appointments**: Highest no-show wait time (68.83 mins)  
* Emergency care had shortest wait (66.97 mins), validating triage effectiveness  

### **Category 2: Provider Efficiency & Operational Risk**
* 65.5% of providers had only **1 completed appointment**  
* **PRV007848**: 75% cancellation rate due to specialty violations  
* Over 1,100 providers had **120-min avg waits**, driving system-wide attrition  

### **Category 3: Claims Denials & Financial Exposure**
* SAR 75.3M paid erroneously; 98.5% coverage rate shows **excessive leniency**  
* 41.8% denials due to **pre-existing conditions** = weak eligibility checks  
* Claims without denial reasons received **SAR 55.9M in payments**  

### **Category 4: Insurance Plan & Regional Performance**
* **Dammam**: Worst surgical wait (67.8 mins) vs **Taif** best (66.4 mins)  
* **Bupa Premium**: Higher wait times than lower-tier plans  
* **Khobar**: Worst regional performance despite mid-level patient volume  

---

## **Recommendations**
**For Claims, Operations, and Network Management Teams:**  
1. Redirect surgeries to facilities with <66-min avg wait  
2. Freeze payouts to providers with unexplained overpayments > SAR 100,000  
3. Reassign efficient providers to high-wait appointment types  
4. Enforce contractual penalties for >90-min provider wait times  
5. Create fast-track lanes for Bupa Premium members  

---

## **Future Work**
* Incorporate patient feedback surveys for non-wait-related cancellations  
* Develop no-show prediction models using wait time + insurance + provider history  
* Launch monthly claim denial audit dashboards with NLP on clinical notes  

---

## **Technical Details**
**Tools Used:**  
* **Python (Pandas, Seaborn, NumPy)**: Statistical analysis  
* **Tableau**: Dynamic dashboards and visualizations  

**Links:**  
* SQL queries for data cleaning: [link]  
* SQL queries for business questions: [link]  
* Tableau dashboard: [link]  

---

## **Assumptions and Caveats**
* **Assumption 1**: No-shows without completion timestamp considered canceled if rescheduled within 3 days  
* **Assumption 2**: Missing wait times imputed using type-specific averages  
* **Assumption 3**: `amount_paid > amount_claimed` treated as overpayments unless tagged "bonus/adjustment"  
