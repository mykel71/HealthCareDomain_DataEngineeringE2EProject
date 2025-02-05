# Data Engineering E2E Project
### Domain:
  #### Health Care Revenue Cycle Management (RCM)

RCM is the process that hospitals use to manage the financial aspects, from the time the patient schedule an appointment till the time the provider gets paid..

### Simplified Breakdown:
  1. It starts with a patient visit:
     - patient details are collected, Insuarance details etc..
     - This ensure the provide knows who will pay for the service (Provider i.e. hospital, clinic, doctor)
     - the payer => insuarance, the patient, mediacal aid etc..

2. Services are provided
3. Billing take place:
   - The Hospital will create the bill for the service provided
  
4. Claims are reviewed:
   - Insuarance company review the bill
   - They might accept it i.e. pay in full, or partial or decline the claim
  
5. Payment and follow ups"
   - If partial payment is done by insuarance then the remaining balance is directed to the patient.
   - The same goes if the insuarance declines the claim
  
6. Tracking and improvement:


### Notes:
RCM ensures the hospital can provide quality care while also staying financially healthy.
As part of RCM we have 2 main aspects:
- Account Receivables (AR) -> Important and key focus of this project
- Account Payable (AP)

Patient paying is often a risk.
- Scenarios when patient has to pay
- Low Insuarance - these insuarance providers put most of the burden on the patients

Private Clinics
Dental Treatment
Deductables

### 2 Objectives for AR
- Bring Cash
- Minimise the collection period (Inflation)

The probability of collecting your full amount decreases with time..
- 93% of money due 30 days old..
- 85% of money deu 60 days old..
- 73% of money due 90 days old..

### KPI to measure AR & set benchmarks:
#### Resources that can help:
https://mdmanagementgroup.com/healthcare-accounts-receivable-management/
https://gentem.com/blog/revenue-cycle-kpis-definitions-and-benchmarks/

1. AR > 90 days..
   e.g. - you have to collect a total of 1 million Rand
       - R100K older than 90 days
       - 100K / 1 million = 10%

2. Days in AR
   1 million Rand in 100 days
   per day 10000 Rand


### Data Engineering Task:
1. We will have data in different sources
2. We need to create a pipeline, the result of this pipeline will be fact tbls and dims tbls.
   - We will have the reporting team to create/ generate some KPIs.
  
     #### Datasets:
     - EMR Data - Electronic Medical Records (Azure SQL DB)
         - Patients
         - Providers
         - Department
         - Transactions
         - Encounter
     Hospital a - techfiedsolutions-hospital-a
     Hospital b - techfiedsolutions-hospital-b

     - Claims Data - Insuarance Company Data (Flat-Files) the Ins Co. will upload data 
         - folder in Datalake - Landing (monthly once)
           
     - NPI data - National Provider Identifier (Public API)
         - Unique indentifier that identifies each doctor
           
     - ICD data - ICD codes are a standardized system used by health care providers map diagnosis code and description. (API)
    
       #### SUmmary of Datasets:
       - EMR (Database)
       - Claims (Flat files)
       - API (NPI / ICD)
      
  ### Solution Architecture

#### Medallion Architecture

landing 	  ->  bronze 		  	-> 		silver 			  ->   gold

flat file   -> parquet files	->		Delta Tables	-> 	 Delta tables

EMR (SQL DB) - we use adf to silver layer in form of parquet file
Claims (FLat files) - only flat files will go to the landing container
Codes (Parquet Files) - bronze layer

bronze - parquet format (source of truth)
silver - data cleaning, enrich, CDM (common data model), SCD2
gold - aggregation, Fact table and Dimension table

Gold - Business Users
Silver - Data Scientist, Machine Learning, Data Analysts
Bronze - Data Engineer

### TechfiedSolutions Health Care Azure Data Warehousing Project - Architecture

![image](https://github.com/user-attachments/assets/73b1e11e-c679-440b-8640-cf1160a8e0ad)


