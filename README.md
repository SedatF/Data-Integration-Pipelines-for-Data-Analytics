# Data-Integration-Pipelines-for-NYC-Payroll-Data-Analytics
# Create high-quality data pipelines that are dynamic, can be automated, and monitored for efficient operation.


### Project Introduction
The City of New York would like to develop a Data Analytics platform on Azure Synapse Analytics to accomplish two primary objectives:
  - Analyze how the City's financial resources are allocated and how much of the City's budget is being devoted to overtime.
  - Make the data available to the interested public to show how the City’s budget is being spent on salary and overtime pay for all municipal employees.
  
As a Data Engineer, I have to create high-quality data pipelines that are dynamic, can be automated, and monitored for efficient operation. The project team also includes the city’s quality assurance experts who will test the pipelines to find any errors and improve overall data quality.

The source data resides in Azure Data Lake and needs to be processed in a NYC data warehouse in Azure Synapse Analytics. The source datasets consist of CSV files with Employee master data and monthly payroll data entered by various City agencies.

<img src="screenshot/db-schema.jpeg" alt="data model" width="500" class="center">

### For this project, we will do our work in the Azure Portal, using several Azure resources including:
  - Azure Data Lake Gen2
  - Azure SQL DB
  - Azure Data Factory
  - Azure Synapse Analytics

# STEPS :

# Step 1: Prepare the Data Infrastructure
Setup Data and Resources in Azure

### 1.Create the data lake and upload data

Create an Azure Data Lake Storage Gen2 (storage account) with three directories in this storage container named
  - dirpayrollfiles
  - dirhistoryfiles
  - dirstaging

<img src="screenshot/DataLake_1.jpg" alt="data model" width="1000">

Upload data from the project data to the dirpayrollfiles folder
<img src="screenshot/DataLake_2.jpg" alt="data model" width="1000">

Upload `nycpayroll_2020.csv` file (historical data) from the project data to the dirhistoryfiles folder
<img src="screenshot/DataLake_3.jpg" alt="data model" width="1000">

### 2. Create an Azure Data Factory Resource
<img src="screenshot/Data_Lake_4.jpg" alt="data model" width="1000">

### 3. Create a SQL Database to store the current year of the payroll data

Create a table called `NYC_Payroll_Data` in db_nycpayroll in the Azure Query Editor with SQL Script:
<img src="screenshot/NYC_Payroll_Data.jpg" alt="data model" width="1000">

### 4. Create A Synapse Analytics workspace
<img src="screenshot/Analytics_workspace.jpg" alt="data model" width="1000">

Create a SQL dedicated pool in the Synapse Analytics workspace
  - Select DW100c as performance level. 
<img src="screenshot/Pool.png" alt="data model" width="1000">

In the SQL dedicated pool, Create master data tables and payroll transaction tables:
<img src="screenshot/payroll_transaction_tables.jpg" alt="data model" width="1000">

The table are successfully created in the SQL Database

<img src="screenshot/SQLDB_Tables.jpg" alt="data model" width="300">


# Step 2: Create Linked Services

### 1.Create a Linked Service for Azure Data Lake

In Azure Data Factory, create a linked service to the data lake that contains the data files
  - From the data stores, select Azure Data Lake Gen 2
  - Test the connection
<img src="screenshot/Linked_service_Gen2.jpg" alt="data model" width="1000">

### 2.Create a Linked Service to SQL Database that has the current (2021) data
If you get a connection error, remember to add the IP address to the firewall settings in SQL DB in the Azure Portal
<img src="screenshot/Linked_service_2.jpg" alt="data model" width="1000">


### 3. Create a Linked Service for Synapse Analytics
  - Create the linked service to the SQL pool.

All the 3 linked services successfully created:
<img src="screenshot/Linked_synapse.jpg" alt="data model" width="1000">

# Step 3: Create Datasets in Azure Data Factory

### 1.Create the datasets for the 2021 Payroll file on Azure Data Lake Gen2
  - Select DelimitedText
  - Set the path to the nycpayroll_2021.csv in the Data Lake
  - Preview the data to make sure it is correctly parsed
  <img src="screenshot/Data_Factory_Preview.jpg" alt="data model" width="1000">

  <img src="screenshot/Data_Factory_01.jpg" alt="data model" width="1000">

### 2. Repeat the same process to create datasets for the rest of the data files in the Data Lake
  - EmpMaster.csv
  - TitleMaster.csv
  - AgencyMaster.csv
  - Remember to publish all the datasets
  <img src="screenshot/Data_Factory_02.jpg" alt="data model" width="1000">

### 3. Create the dataset for transaction data table that should contain current (2021) data in SQL DB
<img src="screenshot/Data_Factory_SQLDB.jpg" alt="data model" width="1000">

### 4. Create the datasets for destination (target) tables in Synapse Analytics
  - dataset for NYC_Payroll_EMP_MD
  <img src="screenshot/Data_Factory_EMP.jpg" alt="data model" width="1000">

  - for NYC_Payroll_TITLE_MD
    <img src="screenshot/Data_Factory_Title.jpg" alt="data model" width="1000">

  - for NYC_Payroll_AGENCY_MD
    <img src="screenshot/Data_Factory_Agent.jpg" alt="data model" width="1000">

  - for NYC_Payroll_Data
    <img src="screenshot/Data_Factory_Payroll_Data.jpg" alt="data model" width="1000">

# Step 4: Create Data Flows

### 1 .In Azure Data Factory, create the data flow to load 2021 Payroll Data to SQL DB transaction table (in the future NYC will load all the transaction data into this table).
  - Create a new data flow
  - Select the dataset for the 2021 payroll file as the source  
  - Select the sink dataset as the payroll table on SQL DB
  - Make sure to reassign any missing source to target mappings
  The screenshot is as below : 
  
   <img src="screenshot/Create_Data_Flows_1.png" alt="data model" width="1000">

### 2.Create Pipeline to load 2021 Payroll data into transaction table in the SQL DB
  - Create a new pipeline
  - Select the data flow to load the 2021 file into SQLDB
  - Trigger the pipeline , Monitor the pipeline
  - Take a screenshot of the Azure Data Factory screen pipeline run after it has finished.
    <img src="screenshot/Create_DataFlow_2.png" alt="data model" width="1000">

  - Make sure the data is successfully loaded into the SQL DB table
    <img src="screenshot/Create_DataFlow_3.png" alt="data model" width="1000">

### 3. Create data flows to load the data from the data lake files into the Synapse Analytics data tables
  - Create the data flows for loading Employee, Title, and Agency files into corresponding SQL pool tables on Synapse Analytics
  - For each Employee, Title, and Agency file data flow, sink the data into each target Synaspe table
  
    <img src="screenshot/Create_DataFlow_4.png" alt="data model" width="1000">
    <img src="screenshot/Create_DataFlow_5.png" alt="data model" width="1000">

### 4. Create a data flow to load 2021 data from SQL DB to Synapse Analytics
   <img src="screenshot/Create_DataFlow_6.png" alt="data model" width="1000">
  

### 5. Create pipelines for Employee, Title, Agency, and year 2021 Payroll transaction data to Synapse Analytics containing the data flows.
  - Select the dirstaging folder in the data lake storage for staging
  - Optionally you can also create one master pipeline to invoke all the Data Flows
  - Validate and publish the pipelines
    
    #### Employee
    <img src="screenshot/Create_DataFlow_7.png" alt="data model" width="1000">
    <img src="screenshot/Create_DataFlow_8.png" alt="data model" width="1000">
     
    #### Load all to synapse
    <img src="screenshot/Create_DataFlow_9.png" alt="data model" width="1000">


# Step 5: Data Aggregation and Parameterization

In this step, we'll extract the 2021 year data and historical data, merge, aggregate and store it in Synapse Analytics. The aggregation will be on Agency Name, Fiscal Year and TotalPaid.

### 1.Create a Summary table in Synapse and create a dataset named table_synapse_nycpayroll_summary
<img src="screenshot/" alt="data model" width="1000">

### 2.Create a new dataset for the Azure Data Lake Gen2 folder that contains the historical files.
<img src="screenshot/" alt="data model" width="1000">


### 3.Create new data flow and name it Dataflow Aggregate Data
  - Create a data flow level parameter for Fiscal Year
  - Add first Source for table_sqldb_nyc_payroll_data table
  - Add second Source for the Azure Data Lake history folder
    
### 4.Create a new Union activity in the data flow and Union with history files
  <img src="screenshot/" alt="data model" width="1000">



