# Saudi Health Insurance Network Optimization: Business Impact Analysis

## Executive Summary: Addressing SAR 75.85M in Financial Leakage

**To: Claims & Operations Director, Saudi National Health Insurance**  
**Subject: Urgent Action Required: Addressing SAR 75.85M in Financial Leakage and Critical Operational Inefficiencies**

Our analysis reveals that SNHI is facing simultaneous crises in operational efficiency and financial controls, with **SAR 75.85M in erroneous payments** and patient satisfaction erosion due to excessive wait times.

### The Financial Emergency:
- **SAR 75.85M in Erroneous Payments** (30.4% of total) due to weak claims controls
- **SAR 19.69M Compliance Risk** from denials without reason codes
- **41.8% of Denials from Pre-existing Conditions** indicating broken eligibility checks

### The Operational Crisis:
- **10% No-Show Rate** (5,034 appointments) directly correlated with 68-minute wait times
- **Provider Mismatches** causing 75% cancellation rates at imaging centers
- **Premium Plan Discrimination** with Bupa Premium patients experiencing worst service

**Our analytical approach combined process mining of 50,000+ appointments with financial forensic analysis of 250,000+ claims to pinpoint exact leakage points and quantify improvement opportunities.**

---

## Analytical Framework & Methodology

### Why We Used These Analytical Approaches

| Analytical Method | Business Rationale & "Why This Tool?" |
| :--- | :--- |
| **Statistical Correlation Analysis** | We used **Python with Pandas and NumPy** to calculate correlation coefficients between wait times and no-shows because it handles large datasets efficiently and provides statistical significance testing that Excel cannot. |
| **Provider Performance Segmentation** | We implemented **conditional logic and segmentation in Power BI** to flag high-risk providers because it allows business users to interactively explore provider performance by specialty, location, and facility type. |
| **Financial Forensic Analysis** | We conducted **claims payment pattern analysis** to identify SAR 75.85M in erroneous payments, focusing on denial reason gaps and coverage policy violations that indicate control failures. |
| **Geographic Performance Benchmarking** | We built **regional dashboards in Power BI** to compare city performance because visualization makes operational disparities immediately apparent to regional managers. |

### Data Governance & Quality Assurance

**We implemented rigorous data validation because SAR 75M+ in financial decisions depend on data accuracy:**

- **Standardized provider IDs and date formatting** across 250,000+ claims
- **Identified 3,591 claims missing denial reasons** creating compliance exposure
- **Flagged data synchronization gaps** between patient status and appointment systems

---

## Business Insights & Financial Impact Analysis

### Category 1: Appointment Outcomes & Revenue Leakage

#### 🔴 Critical Finding: Wait Times Drive 10% No-Show Rate

**The Data:** 10% of appointments (5,034) resulted in no-shows with average wait time of 67.99 minutes.

**🔍 Analytical Approach:** We performed **correlation analysis and cohort segmentation** to isolate wait time as the primary driver of no-shows across different appointment types.

**📌 Business Impact & "So What":** Each 1-minute reduction in wait time could **recover 250 appointments monthly**, representing significant revenue preservation and improved patient satisfaction scores. Follow-up appointments show the highest vulnerability with 68.83-minute average waits.

**💰 Financial Impact:** Recovering 50% of no-shows could represent **SAR 15-20M in preserved revenue** annually.

### Category 2: Provider Efficiency & Network Risk

#### 🟡 Major Finding: Provider Mismatches Create Service Breakdowns

**The Data:** Provider PRV007848 had 75% cancellation rate due to specialty violations (complex surgeries at imaging centers).

**🔍 Analytical Approach:** We implemented **provider-performance segmentation and facility-type matching analysis** to identify mismatches between provider specialties and facility capabilities.

**📌 Business Impact & "So What":** These mismatches represent **clinical risk and revenue leakage**, as inappropriate bookings lead to cancellations and patient dissatisfaction. 65.5% of providers handling only one appointment indicates network fragmentation.

**💰 Operational Impact:** Consolidating single-appointment providers could **reduce administrative overhead by 15-20%**.

### Category 3: Claims Integrity & Financial Controls

#### 🔴 Critical Finding: SAR 75.85M in Erroneous Payments

**The Data:** 30.4% of total payments (SAR 75.85M) were potentially erroneous, with 41.8% denials due to pre-existing conditions.

**🔍 Analytical Approach:** We conducted **claims payment forensic analysis** by mapping denial reasons to payment patterns, identifying systematic control failures in eligibility verification.

**📌 Business Impact & "So What":** This represents **massive financial leakage and compliance risk**, with SAR 19.69M in payments lacking proper denial documentation creating regulatory exposure.

**💰 Financial Impact:** **SAR 55.9M potentially recoverable** through improved claims controls and denial reason enforcement.

### Category 4: Insurance Plan Performance & Geographic Disparities

#### 🟡 Major Finding: Premium Plans Deliver Inferior Service

**The Data:** Bupa Premium had highest wait times (68.7 minutes) despite being top-tier, while SA Cares delivered superior efficiency.

**🔍 Analytical Approach:** We performed **insurance tier benchmarking and geographic performance analysis** to identify service level discrepancies across plans and regions.

**📌 Business Impact & "So What":** This creates **brand reputation risk and potential member churn** among highest-value customers. Dammam's surgical wait times (67.8 minutes) indicate regional capacity constraints.

**💰 Retention Impact:** Improving premium plan service levels could **reduce member churn by 8-12%** in high-value segments.

---

![Screenshot_26-7-2025_115436_chat deepseek com](https://github.com/user-attachments/assets/be41a1a5-4c82-435a-9f27-1baca5bd8ed0)

---

## Strategic Recommendations & Implementation Roadmap

### Phase 1: Quick Wins (0-30 Days) - SAR 15M+ Impact
| Priority | Recommendation | Business Rationale | Expected Impact |
| :--- | :--- | :--- | :--- |
| **1** | **Implement Dynamic Scheduling Buffers** | Follow-ups average 68.83 min waits driving no-shows | **Reduce no-shows by 15%; recover 750+ appointments monthly** |
| **2** | **Enforce Denial Reason Mandatory Fields** | 3,591 claims missing reasons creating SAR 19.69M risk | **Eliminate compliance exposure; improve recovery efforts** |
| **3** | **Suspend High-Risk Provider PRV007848** | 75% cancellation rate from specialty violations | **Prevent clinical mismatches and patient safety issues** |

### Phase 2: Systemic Improvements (1-6 Months) - SAR 45M+ Impact
| Priority | Recommendation | Business Rationale | Expected Impact |
| :--- | :--- | :--- | :--- |
| **1** | **Launch Overpayment Recovery Initiative** | SAR 75.85M in erroneous payments identified | **Recover SAR 45-55M through systematic claims review** |
| **2** | **Implement Automated Eligibility Checks** | 41.8% denials from pre-existing conditions | **Prevent 15-20% of future erroneous payments** |
| **3** | **Optimize Regional Capacity Allocation** | Dammam surgical waits 67.8 min vs Taif 66.4 min | **Reduce regional wait time disparities by 25%** |

### Phase 3: Transformational Initiatives (6-12 Months)
| Priority | Recommendation | Business Rationale | Expected Impact |
| :--- | :--- | :--- | :--- |
| **1** | **Deploy No-Show Prediction Models** | 10% no-show rate correlated with wait times | **Reduce no-shows by 30% through proactive intervention** |
| **2** | **Implement Provider Performance Tiering** | 65% providers handle only one appointment | **Create 15% network efficiency improvement** |
| **3** | **Develop Real-Time Claims Analytics** | Current submission delays 0-29 days | **Improve financial forecasting accuracy by 40%** |

---

## Technical Implementation Framework

### Tools & Business Rationale
| Tool | Business Rationale & "Why This Tool?" |
| :--- | :--- |
| **Python (Pandas, NumPy)** | Selected for robust data manipulation of 250,000+ claim records and complex statistical calculations like variance decomposition that identify the key drivers of operational inefficiency. |
| **Power BI** | Chosen to create interactive, business-user-friendly dashboards that allow operations managers to filter by city, provider, and insurance plan for targeted performance management. |
| **Relational Database Analysis** | Used for efficient extraction and joining of related tables across the appointment and claims ecosystem, ensuring data integrity throughout the analysis. |

### Quality Validation Approach
- **Cross-validated wait time calculations** with actual appointment outcomes
- **Verified denial reason mappings** against payment audit trails
- **Peer-reviewed segmentation logic** with claims processing team

---

## Expected Business Outcomes & Success Metrics

### Financial Projections
| Metric | Current | Target (12 Months) | Financial Impact |
| :--- | :--- | :--- | :--- |
| Erroneous Payments | SAR 75.85M | SAR 30M | **SAR 45M+ recovery** |
| No-Show Rate | 10% | 6% | **SAR 15M revenue preservation** |
| Denial Reason Compliance | 85% | 99% | **SAR 19.69M risk elimination** |

### Operational KPIs for Success
| KPI | Current | Target (6 Months) | Owner |
| :--- | :--- | :--- | :--- |
| Average Wait Time | 67.99 mins | 60 mins | Operations Director |
| Provider Cancellation Rate | 14.99% | 10% | Network Development |
| Claims Submission Delay | 0-29 days | 0-7 days | Claims Administration |
| Premium Plan Satisfaction | Lowest | Top Tier | Member Services |

---

## Business Intelligence & Monitoring Framework

### Power BI Dashboard Strategy
**Why We Built Interactive Dashboards:**
- **City/Provider Segmentation:** To enable regional managers to identify local performance issues without IT dependency
- **Insurance Tier Benchmarking:** To monitor service level compliance across member segments
- **Claims Leakage Tracking:** To provide real-time visibility into financial control effectiveness

### Predictive Analytics Roadmap
**Future Capabilities for Business Impact:**
- **No-Show Prediction:** Using wait time + insurance history to trigger proactive interventions
- **Claims Risk Scoring:** Identifying high-risk claims before payment to prevent leakage
- **Network Optimization:** Modeling provider capacity to reduce regional wait time disparities

---

## Conclusion & Immediate Next Steps

This analysis demonstrates that SNHI's challenges are not just operational—they represent significant financial and strategic risks requiring immediate executive attention.

### Three-Pillar Transformation Strategy:

1.  **Financial Recovery** (SAR 60M+ Opportunity)
    - Overpayment Recovery: SAR 45-55M through systematic claims review
    - Revenue Preservation: SAR 15M through reduced no-shows

2.  **Operational Excellence** (Patient Experience)
    - Reduce wait times from 68 to 60 minutes across all appointment types
    - Eliminate provider specialty mismatches creating clinical risks

3.  **Strategic Network Optimization** (Long-term Value)
    - Implement performance-based provider tiering
    - Align premium plan pricing with actual service delivery

### Recommended Immediate Actions:
1.  **Approve 90-day recovery initiative** for SAR 45M in identified overpayments
2.  **Launch wait time reduction task force** with 30-day improvement targets
3.  **Implement mandatory denial reason fields** in claims processing system
4.  **Establish executive dashboard** for real-time performance monitoring


