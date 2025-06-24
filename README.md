---

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

* Quantify and reduce no-shows, cancellations, and long wait times that degrade appointment performance.
* Identify high-risk providers with mismatched specialties or inefficient scheduling practices.
* Detect claims leakage caused by denial errors, improper overpayments, and pre-authorization gaps.
* Benchmark performance by city, provider, and insurance tier to implement targeted efficiency strategies.

---

## **Insights and Recommendations**

### **Category 1: Appointment Outcomes & Wait Times**

#### Key Insights:

* Completed appointments: 32,481 (65%)
* No-shows: 5,034 (10%), **strongly correlated with wait times** (avg 67.99 mins)
* Follow-ups & consultations had the **highest no-show and cancellation wait times**
* Emergency visits had **lowest wait times and highest completion efficiency**

#### Recommendations:

* Implement SMS reminders + proactive rescheduling for appointments exceeding 67 minutes.
* Introduce 15-min buffer slots for follow-ups and consultations.
* Replicate emergency department triage speed in other appointment types.

---

### **Category 2: Provider Efficiency & Operational Risk**

#### Key Insights:

* 23,938 unique providers, **65% handled only one appointment**
* Over 1,100 providers averaged the **maximum 120-min wait**
* Provider PRV007848 had a 75% cancellation rate, **handling complex surgeries inappropriately at imaging centers**

#### Recommendations:

* Reassign providers with 15-min avg wait to consultation-heavy departments.
* Suspend providers with >50% cancellation rate pending retraining.
* Consolidate appointments from 15,000+ single-appointment providers to improve patient continuity.

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

* Launch 90-day overpayment recovery & voluntary repayment initiative.
* Enforce mandatory denial reason codes before approval.
* Penalize claim submissions >7 days post-visit with 5% reimbursement cut.

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

Sure! Based on your formatting example, here's the rewritten **Data Structure & Initial Checks** section, styled uniformly across all tables like your reference.

---

---

### 📅 Table: `appointments`

| **Column Name**                                                            |                            **Business Meaning**                               |                          **Business Benefit for Data Analyst**                              |
| -------------------------------------------------------------------------- | ----------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| `appointment_id`                                                           | Unique ID for each patient appointment.                                       | Tracks appointment lifecycle and supports filtering and completion status analysis.         |
| `patient_id`                                                               | Links the appointment to the patient profile.                                 | Enables segmentation by demographics, insurance, and health history.                        |
| `appointment_type`                                                         | Type of visit: Consultation, Emergency, Follow-up, Surgery, etc.              | Allows comparison of no-show/cancellation rates and wait times by service category.         |
| `provider_id`                                                              | Identifier for the doctor or care provider assigned to the appointment.       | Supports provider efficiency evaluation and mismatch detection.                             |
| `wait_time`                                                                | Time (in minutes) between appointment scheduling and actual service delivery. | Key metric for operational efficiency, strongly correlated with cancellations and no-shows. |
| `outcome`                                                                  | Status of the appointment: Completed, Cancelled, No-show, Rescheduled.        | Enables performance tracking, churn risk detection, and root cause analysis.                |
| `city`                                                                     | Location where the appointment took place.                                    | Supports regional performance benchmarking.                                                 |
| `scheduled_time`                                                           | Original scheduled time of the appointment.                                   | Used to analyze scheduling delays and slot optimization.                                    |
| `actual_time`                                                              | Actual start time of the service.                                             | Helps calculate actual wait and punctuality metrics.                                        |

---

### 🧑‍⚕️ Table: `providers`

| **Column Name**                                                            |                            **Business Meaning**                                        |                       **Business Benefit for Data Analyst**                             |
| -------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| `provider_id`                                                              | Unique identifier for each medical provider.                                           | Used for analyzing provider performance and patient load.                               |
| `specialty`                                                                | Medical field of the provider (e.g., Cardiology, Radiology, Surgery).                  | Helps identify mismatches, staffing needs, and provider assignment optimization.        |
| `facility_type`                                                            | Type of facility where the provider operates (e.g., Clinic, Imaging Center, Hospital). | Enables facility-level performance insights and specialty-appropriate service matching. |
| `avg_wait_time`                                                            | Average wait time for services performed by the provider.                              | Used to detect bottlenecks, underutilization, and overbooking trends.                   |
| `city`                                                                     | Geographic location of the provider.                                                   | Facilitates comparison of provider performance across regions.                          |

---

### 💸 Table: `claims`


| **Column Name**                                                            |                            **Business Meaning**           |                                             **Business Benefit for Data Analyst**                              |
| -------------------------------------------------------------------------- | --------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| `claim_id`                                                                 | Unique identifier for each claim submitted to SNHI.       | Core key for tracking and aggregating financial data across related tables.                                    |
| `patient_id`                                                               | Identifies the patient associated with the claim.         | Supports analysis by patient demographics and health plan.                                                     |
| `provider_id`                                                              | Identifies the provider who submitted the claim.          | Enables linking to appointment/provider records for holistic performance review.                               |
| `amount_claimed`                                                           | Total amount billed by the provider.                      | Important for trend analysis, overbilling detection, and reimbursement policy design.                          |
| `amount_paid`                                                              | Final amount reimbursed by SNHI.                          | Enables payout accuracy checks, overpayment audits, and policy cost assessments.                               |
| `denial_reason`                                                            | Code or explanation for claim rejection (if applicable).  | Critical for analyzing denial trends and root causes, essential for compliance and claims improvement efforts. |
| `insurance_plan`                                                           | Plan tier or product under which the claim was submitted. | Useful for evaluating claim volume and financial exposure by plan.                                             |

---

### 🧍 Table: `patients`


| **Column Name**                                                            |                            **Business Meaning**                          |                          **Business Benefit for Data Analyst**                               |
| -------------------------------------------------------------------------- | ------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------- |
| `patient_id`                                                               | Unique identifier for the patient.                                       | Core join key for linking with claims, appointments, and provider history.                   |
| `dob`                                                                      | Date of birth of the patient.                                            | Enables age segmentation, eligibility validation, and demographic risk analysis.             |
| `insurance_plan`                                                           | Health plan tier assigned to the patient (e.g., Bupa Premium, SA Cares). | Supports cost-to-serve analysis, plan utilization trends, and patient equity assessments.    |
| `status`                                                                   | Indicates whether the patient is Active or Inactive.                     | Helps track churn, identify reactivation opportunities, and flag claims from lapsed members. |
| `registration_date`                                                        | Date the patient joined SNHI.                                            | Enables member tenure analysis, new user trend identification, and engagement scoring.       |

---

### 🧾 Table: `claim_details_data`


| **Column Name**                                                            |                            **Business Meaning**                               |                          **Business Benefit for Data Analyst**                              |
| -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| `detail_id`                                                                | Unique identifier for each claim line item. A single `claim_id` may have multiple `detail_id` records for different services or items billed. | Enables granular, line-level analysis of services, allowing analysts to break down claims into specific cost components.   |
| `claim_id`                                                                 | Identifier linking each line item to its parent claim.                                                                                        | Allows grouping and aggregation of multiple details under a single claim for total cost, denial impact, or fraud review.   |
| `diagnosis_code`                                                           | Standardized medical code (e.g., ICD-10) indicating the reason for the medical service.                                                       | Supports diagnosis-based analysis such as disease prevalence, cost of care by condition, and risk adjustment modeling.     |
| `procedure_code`                                                           | Standardized code (e.g., CPT/HCPCS) representing the medical procedure, test, or treatment performed.                                         | Enables service mix analysis, cost benchmarking by procedure, and procedure frequency tracking across departments.         |
| `quantity`                                                                 | Number of units for the given service (e.g., 3 doses of medication or 5 physiotherapy sessions).                                              | Helps calculate utilization rates, spot unusual billing patterns (e.g., abnormally high units), and forecast supply needs. |
| `unit_cost`                                                                | Cost per unit of the procedure or service billed by the hospital.                                                                             | Allows cost-per-service analysis, identification of high-cost services, and cost standardization opportunities.            |

---

### 🩺 Table: `diagnosis`

| **Column Name** **Business Meaning** **Business Benefit for Data Analyst** |                                                                                                     |                                                                                                                                       |
| -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| `diagnosis_code`                                                           | A standardized medical code (e.g., ICD-10) representing a patient’s diagnosis or medical condition. | Enables analysis of disease prevalence, case-mix complexity, and supports clinical audits, resource allocation, and DRG grouping.     |
| `description`                                                              | The textual explanation or name of the diagnosis associated with the code.                          | Enhances readability in reports and dashboards, supports NLP-based analysis, and facilitates non-technical stakeholder understanding. |
| `category`                                                                 | Broad classification of the diagnosis (e.g., Cardiovascular, Respiratory, Infectious Disease).      | Enables segmentation of cases by disease group, supports trend analysis by specialty, and guides service line planning.               |

---

### 🛠️ Table: `procedures`


| **Column Name**                                                            |                            **Business Meaning**                               |                          **Business Benefit for Data Analyst**                              |
| -------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `procedure_code`                                                           | Standardized code (e.g., CPT, HCPCS) representing the medical procedure or service performed.     | Enables grouping and analysis of procedures, frequency trends, cost benchmarking, and detecting code-related denials. |
| `description`                                                              | Human-readable description of the procedure code (e.g., "MRI scan of brain").                     | Improves interpretability in reports and dashboards; supports categorization for clinical and financial analysis.     |
| `allowed_amount`                                                           | Maximum amount the insurer has agreed to pay for the procedure based on contract or policy terms. | Useful for analyzing cost containment, comparing billed vs. allowed vs. paid, and detecting pricing outliers.         |

---

**Documenting Issues**

| Table        | Column         | Issues                                      | Magnitude | Solvable | Resolutions                                   |
| ------------ | -------------- | ------------------------------------------- | --------- | -------- | --------------------------------------------- |
| claims       | denial\_reason | 3,591 claims missing denial reason          | High      | Yes      | Enforce non-null constraint                   |
| patients     | status         | Inactive patients still had appointments    | Low       | Yes      | Sync patient status table with booking system |

---

## **Executive Summary**

**(Stakeholder: Claims & Operations Director)**
Appointment bottlenecks and claim overpayments are costing SNHI millions. The biggest risks come from surgeries with long waits, providers handling mismatched cases (e.g. emergencies in imaging centers), and poorly governed claim denial systems.

> Three takeaways:
>
> * Reducing wait time by just **1 minute** may eliminate **250 no-shows**.
> * Over **SAR 75M in claim payments were made erroneously** or without justification.
> * Top-tier insurance patients receive **worse service** than economy-tier—indicating broken prioritization.

---

## **Insights Deep Dive**

### **Category 1: Appointment Outcomes & Wait Times**

* **10% of appointments end in no-shows**, with avg wait time of 67.99 mins
* **Follow-up appointments**: highest no-show wait time (68.83 mins)
* **Reschedules occur earliest** (66.92 mins), showing patients reschedule proactively if wait is long
* Emergency care had shortest wait (66.97 mins), validating triage effectiveness

---

### **Category 2: Provider Efficiency & Operational Risk**

* 65.5% of providers had only **1 completed appointment**
* **PRV007848** had a 75% cancellation rate, violating specialty scope
* Over 1,100 providers had **120-min avg waits**, driving up system-wide attrition
* Best-performing providers (15-min avg wait) were underutilized

---

### **Category 3: Claims Denials & Financial Exposure**

* SAR 75.3M paid erroneously; 98.5% coverage rate shows **excessive leniency**
* 41.8% of denials due to **pre-existing conditions** = weak eligibility checks
* No-denial-reason claims still got **SAR 55.9M paid**
* Submission delays range up to 29 days = poor cash flow management

---

### **Category 4: Insurance Plan & Regional Performance**

* **Dammam** had worst surgical wait (67.8 mins); **Taif** had best (66.4 mins)
* **Bupa Premium patients** received worse service than SA Cares
* **Internal Medicine** had highest wait times (68.0 mins) despite being core specialty
* **Khobar** had worst overall performance vs. mid-level patient volume

---

## **Recommendations**

For the **Claims, Operations, and Network Management Teams**:

* Redirect surgeries to compliant facilities with <66-min avg wait
* Freeze payouts to providers with unexplained overpayments > SAR 100,000
* Reassign underutilized efficient providers to high-wait appointment types
* Enforce contractual penalties for >90-min provider wait times
* Offer fast-track lanes for Bupa Premium members to match expectations

---

## **Future Work**

* Incorporate patient feedback surveys to understand non-wait-related cancellations
* Develop predictive models for no-show likelihood using wait, type, insurance, and provider history
* Launch monthly claim denial audit dashboards with NLP on clinical notes

---

## **Technical Details**

**Tools Used:**

* **Python (Pandas, Seaborn, NumPy)**: For statistical analysis and advanced filtering
* **Tableau**: For building dynamic dashboards and wait time visualizations

**Links:**

* SQL queries for data cleaning: \[link]
* SQL queries for business questions: \[link]
* Tableau dashboard: \[link]

---

## **Assumptions and Caveats**

* **Assumption 1**: No-show appointments without a completion timestamp were considered canceled if rescheduled within 3 days
* **Assumption 2**: Missing wait times were imputed using the average wait by appointment type
* **Assumption 3**: Claims where `amount_paid > amount_claimed` were treated as overpayments unless tagged "bonus" or "penalty adjustment"

---
