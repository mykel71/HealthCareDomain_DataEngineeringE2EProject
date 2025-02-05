# Data Engineering E2E Project

## Domain: Health Care Revenue Cycle Management (RCM)
Revenue Cycle Management (RCM) is the process that hospitals use to manage financial operations, from patient appointment scheduling to provider payment.

## Simplified Breakdown:
1. **Patient Visit**
   - Collection of patient details, insurance information, etc.
   - Determines who will cover the cost (hospital, clinic, doctor, insurance, medical aid, or the patient).

2. **Service Delivery**
   - The healthcare provider offers services to the patient.

3. **Billing Process**
   - The hospital generates a bill for the provided services.

4. **Claims Processing**
   - Insurance company reviews the bill.
   - Outcomes: Full payment, partial payment, or claim denial.

5. **Payments & Follow-ups**
   - If only a partial payment is made, the remaining balance is billed to the patient.
   - If the insurance denies the claim, the patient is responsible for the full amount.

6. **Tracking & Continuous Improvement**
   - Monitoring outstanding balances and optimizing the collection process.

## Key Aspects of RCM:
RCM ensures financial sustainability while maintaining quality care. Two critical components:
- **Accounts Receivable (AR)** - Focus of this project.
- **Accounts Payable (AP)**

### Challenges in Payment Collection:
- High patient financial responsibility due to:
  - Low insurance coverage.
  - Out-of-pocket payments for private clinics, dental treatments, and deductibles.

### Objectives for AR:
1. Maximize cash inflow.
2. Minimize the collection period (reduce the impact of inflation).

**Collection Probability Over Time:**
- 93% of money due within 30 days is collectible.
- 85% of money due within 60 days is collectible.
- 73% of money due within 90 days is collectible.

## Key Performance Indicators (KPIs) for AR:
### Benchmarks & Resources:
- [Healthcare AR Management](https://mdmanagementgroup.com/healthcare-accounts-receivable-management/)
- [Revenue Cycle KPIs](https://gentem.com/blog/revenue-cycle-kpis-definitions-and-benchmarks/)

**1. AR Aging (> 90 days):**
   - Example:
     - Total amount to collect: R1,000,000
     - Amount older than 90 days: R100,000
     - AR > 90 days = (R100,000 / R1,000,000) * 100 = **10%**

**2. Days in AR:**
   - Example:
     - R1,000,000 collected over 100 days
     - Per day collection rate: **R10,000**

---

## Data Engineering Tasks:
1. **Data Integration from Multiple Sources**
2. **ETL Pipeline Development** to create **fact and dimension tables** for reporting.
3. **Generate KPIs for Analysis**

### Datasets:
- **Electronic Medical Records (EMR) - Azure SQL Database**
  - Patients, Providers, Departments, Transactions, Encounters
  - **Sources:**
    - Hospital A: `techfiedsolutions-hospital-a`
    - Hospital B: `techfiedsolutions-hospital-b`

- **Claims Data - Insurance Company (Flat Files in Data Lake)**
  - Data uploaded monthly to the **Landing Zone**.

- **National Provider Identifier (NPI) - Public API**
  - Unique identifier for each doctor.

- **ICD Data - API Source**
  - Standardized system for mapping diagnosis codes and descriptions.

### Summary of Data Sources:
| Source | Format |
|--------|--------|
| EMR | Database |
| Claims | Flat Files |
| NPI & ICD | APIs |

---

## Solution Architecture:
### Medallion Architecture
| Stage | Storage Format | Purpose |
|--------|--------------|---------|
| **Landing** | Flat Files | Raw data from external sources |
| **Bronze** | Parquet Files | Source of Truth |
| **Silver** | Delta Tables | Cleaned, Enriched Data (CDM, SCD2) |
| **Gold** | Delta Tables | Aggregated Data for Business Users |

### User Access by Layer:
| Layer | Users |
|--------|--------|
| **Gold** | Business Users |
| **Silver** | Data Scientists, Machine Learning Engineers, Analysts |
| **Bronze** | Data Engineers |

### TechfiedSolutions Healthcare Azure Data Warehousing Project - Architecture
![Project Architecture](https://github.com/user-attachments/assets/73b1e11e-c679-440b-8640-cf1160a8e0ad)

---

## Data Pipeline: Step 1 - EMR Data to Bronze Layer
### Technologies Used:
- **Azure Data Factory (ADF)** - Ingestion
- **Azure Databricks** - Data Processing
- **Azure SQL DB** - EMR Source
- **Azure Storage Account (ADLS Gen2)** - Data Storage
- **Azure Key Vault** - Credential Management

### Azure Storage Account:
**techfiedsolutionadlsdev (ADLS Gen2) - Storage Structure**
```
/landing
/bronze
/silver
/gold
/configs
  /metadata-driven pipeline (e.g., load_config.csv)
```

**Data Flow:**
- EMR (Azure SQL DB) → ADLS Gen2 (Bronze - Parquet format)
- Audit Table (Delta Table) Created
- **ADF Pipeline Components:**
  - **Linked Services:** Azure SQL DB, ADLS Gen2, Delta Lake, Key Vault
  - **Datasets:**
    - Azure SQL (Database Name, Table Name, Schema Name)
    - Delimited Text (ADLS Gen2)
    - Parquet (ADLS Gen2)
    - Databricks Delta Lake (Delta Lake)
  - **Pipeline Activities:**
    - **Lookup Activity:** Reads Config File (`configs/emr/load_config.csv`)

---

## High-Level ERD:
![ERD](https://github.com/user-attachments/assets/286c5652-b3a1-4a92-b27b-c7ddd29bdad4)

### Fact & Dimension Tables:
- **Fact:** Numeric values (e.g., Transactions, Payments, Claims)
- **Dimension:** Supporting entities (e.g., Patients, Providers, Insurance Companies, ICD Codes)

This README provides a structured overview of the project, ensuring clarity and professional presentation.

