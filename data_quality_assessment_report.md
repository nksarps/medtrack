# Data Quality Assessment Report  

Author: *Nana Kwaku Sarpong*  
Specialization: *Quality Assurance*


## 1. Executive Summary  

MedTrack Ghana is a health technology startup that relies on accurate, complete, and timely patient appointment data to manage operations, clinical care, and billing processes. Recent operational challenges have highlighted the consequences of poor data quality, including:  

- Failure to send SMS reminders to patients, resulting in missed appointments.  
- Duplicate patient counts, leading to inaccurate analytics.  
- Billing discrepancies and failed payment processing, causing revenue loss.  

This report provides a comprehensive assessment of a sample dataset containing 50 patient appointment records. It identifies data quality issues based on established dimensions—**accuracy, completeness, consistency, timeliness, validity, and uniqueness**—evaluates the operational and business impact, and presents actionable recommendations to resolve critical problems.


## 2. Task 1: Data Quality Issues  

The following section presents violations identified for each data quality dimension.

### 2.1 Accuracy  

**Definition:** Accuracy refers to how well data reflects the real-world entity or fact it represents. Accurate data is essential for operational processes such as SMS reminders, reporting, and billing.  

**Observation:**  
`P002, Ama Serwa, 244789012, 15/10/2025, dr. osei, paid`  

- The phone number `244789012` is likely inaccurate, missing a leading zero.  
- Inaccurate phone numbers prevent SMS messages from reaching patients, directly impacting operations.  

**Impact:**  
- Operations: SMS reminders fail, increasing missed appointments.  
- Clinical: Patients may miss scheduled consultations, disrupting care delivery.  
- Financial: Failed reminders can indirectly affect billing if appointments are missed.  


### 2.2 Completeness  

**Definition:** Completeness measures the extent to which required data fields are populated. Missing data can hinder operational, clinical, and financial processes.  

**Observation:**  
`P004,, 0555234567, 2025-10-17, Dr. Mensah, Paid`  

- The `PatientName` field is missing.  
- Missing critical identifiers creates ambiguity and may prevent staff from confirming appointments or billing correctly.  

**Impact:**  
- Clinical teams cannot properly identify patients, increasing the risk of mismanagement.  
- Operational processes such as generating appointment lists or sending reminders are hindered.  
- In billing, missing identifiers may result in unprocessed payments or errors in patient accounts.  


### 2.3 Consistency  

**Definition:** Consistency refers to uniform representation of data across records and systems. Inconsistent data can confuse automated processes and human decision-making.  

**Observations:**  

- Doctor names are inconsistently formatted: `Dr. Osei` vs `dr. osei`.  
- Payment status appears in inconsistent cases: `Paid` vs `paid`.  
- Appointment dates use mixed formats: `2025-10-15`, `15/10/2025`, `10/16/2025`.  

**Impact:**  
- Clinical and operational reports may be inaccurate due to misgrouped records.  
- Automated processes may fail or misinterpret data, leading to missed reminders or incorrect scheduling.  
- Financial systems may miscategorize payments due to inconsistent status entries.  


### 2.4 Timeliness  

**Definition:** Timeliness ensures that data is current and recorded within the correct timeframe. Outdated or incorrectly dated data affects operational reliability.  

**Observation:**  

- Mixed or ambiguous date formats can lead to incorrect interpretation. For example, `10/16/2025` may be interpreted as October 16th or, in some systems, as invalid.  

**Impact:**  
- Operations: Appointment reminders may be sent on incorrect dates, reducing efficiency.  
- Clinical: Patients may arrive on the wrong date, causing scheduling conflicts.  
- Financial: Billing processes tied to appointment dates may fail if dates are misinterpreted.  


### 2.5 Validity  

**Definition:** Validity ensures that data adheres to prescribed formats, business rules, and regulatory standards. Invalid data compromises automation and reporting.  

**Observations:**  

- Phone numbers must conform to the Ghanaian format `0XXXXXXXXX`, yet some records fail this rule.  
- Payment status values do not consistently follow the expected set (`Paid`, `Pending`, `Failed`).  
- Date formats are inconsistent and do not conform to a standard ISO or system-recognized format.  

**Impact:**  
- Automation failures occur during billing and reminder notifications.  
- Reports may generate misleading insights due to invalid or non-standard entries.  
- Poor compliance with formatting rules may complicate integration with external systems.  


### 2.6 Uniqueness  

**Definition:** Uniqueness ensures that each record is distinct without unnecessary duplication. Duplicate entries distort analytics and operational tracking.  

**Observations:**  

- `P001, Kwame Mensah, 0244123456` appears more than once.  
- `P002, Ama Serwa` is also duplicated.  

**Impact:**  
- Inflated patient counts affect operational reporting and capacity planning.  
- Double billing or repeated reminders may occur, causing financial loss and patient dissatisfaction.  
- Data cleaning and reconciliation become more resource-intensive.  


## 3. Task 2: Business Impact Assessment  

The table below explains how each identified data quality issue directly causes operational, clinical, or financial problems within MedTrack Ghana.

| Data Quality Issue | Why It Causes Operational Problems | Most Affected Business Function |
|-------------------|------------------------------------|---------------------------------|
| Inaccurate phone numbers | When phone numbers are incorrect or incomplete, SMS reminders cannot be delivered successfully. As a result, patients may miss appointments, leading to wasted time slots, reduced clinic efficiency, and lost revenue opportunities. | Operations |
| Missing patient name | Without a patient name, clinical staff cannot reliably identify individuals during check-in or treatment. This increases the risk of patient misidentification, delays in service, and potential clinical errors. | Clinical |
| Inconsistent doctor names | When doctor names are formatted differently (e.g., capitalization differences), reporting systems may treat them as separate individuals. This results in inaccurate performance reports and misallocation of clinical workload. | Clinical |
| Mixed date formats | Ambiguous or inconsistent date formats may cause the system to misinterpret appointment dates. This can lead to reminders being sent on the wrong day or appointments being scheduled incorrectly, disrupting daily operations. | Operations |
| Invalid payment status | If payment status values do not match predefined system rules, automated billing workflows may fail. This leads to delayed reconciliation, incorrect financial reports, and increased manual intervention by finance staff. | Finance |
| Duplicate PatientIDs | Duplicate patient records can inflate patient counts and cause multiple invoices to be generated for the same individual. This creates financial discrepancies, damages patient trust, and distorts reporting metrics. | Finance |


## 4. Task 3: Recommended Solutions (Top 3 Critical Issues)  

### 4.1 Duplicate Patient Records (Uniqueness)  

**Problem:** Duplicate records create reporting inaccuracies and billing errors.  

**Recommended Solution:**  
- Enforce `PatientID` as a primary key in the database.  
- Introduce duplicate detection scripts during data import.  
- Apply database-level unique constraints to prevent future duplicates.  

**Responsible Roles:**  
- Database Administrator  
- Data Steward  

**Verification:**  
- Run queries to identify duplicates and confirm elimination.  
- Validate that reports match expected patient counts.  
- Monitor incoming data for compliance with uniqueness constraints.  


### 4.2 Invalid Phone Numbers (Accuracy & Validity)  

**Problem:** Incorrect phone numbers prevent automated SMS reminders from reaching patients.  

**Recommended Solution:**  
- Implement regex validation:  
- Reject invalid phone numbers at data entry.  
- Execute a cleansing script to correct existing incorrect numbers.  

**Responsible Roles:**  
- Data Steward  

**Verification:**  
- Test SMS delivery to confirm success.  
- Ensure all phone numbers in the database conform to the defined format.  


### 4.3 Inconsistent Date Formats (Consistency & Timeliness)  

**Problem:** Mixed and ambiguous date formats cause scheduling errors and operational confusion.  

**Recommended Solution:**  
- Standardize all appointment dates to ISO 8601 format (`YYYY-MM-DD`).  
- Transform imported dates using automated scripts.  
- Use DATE data types in the database to enforce consistency.  

**Responsible Roles:**    
- Data Engineer  

**Verification:**  
- Confirm all dates follow the standardized format.  
- Test automated reminders and scheduling processes for correctness.  


## 5. Task 4: Risk Assessment  

From a Quality Assurance (QA) perspective, the **most critical risk of poor data consistency** is the failure to reliably validate, verify, and maintain high-quality data throughout the system. Inconsistent data formats and representations can result in:  

- Failed or broken automated test scripts and validation workflows.  
- Inaccurate test results due to mismatched or misinterpreted data.  
- False positives/negatives in QA reports, masking real defects.  
- Increased regression defects when inconsistent data propagates across environments.  
- Financial and operational errors, including double billing or missed payments, that may not be caught during testing.  
- Additional QA effort to manually investigate and correct data inconsistencies, increasing testing time and cost.  

Ensuring data consistency is therefore critical not only for operational reliability but also for maintaining test accuracy, validating system behavior, and supporting effective risk management in software delivery.  


## 6. Conclusion  

The dataset demonstrates violations across all six key data quality dimensions. These issues adversely affect operational efficiency, clinical service delivery, and financial accuracy.  

Addressing the most critical problems through database constraints, validation rules, standardization scripts, and clear assignment of data stewardship responsibilities will significantly improve data integrity.  

Robust data governance and stewardship practices are essential to ensure that MedTrack Ghana can operate efficiently, maintain accurate reporting, and deliver reliable patient services.
