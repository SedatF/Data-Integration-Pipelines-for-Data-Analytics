# Data-Integration-Pipelines-for-NYC-Payroll-Data-Analytics
# Create high-quality data pipelines that are dynamic, can be automated, and monitored for efficient operation.


### Project Introduction
The City of New York would like to develop a Data Analytics platform on Azure Synapse Analytics to accomplish two primary objectives:
  - Analyze how the City's financial resources are allocated and how much of the City's budget is being devoted to overtime.
  - Make the data available to the interested public to show how the City’s budget is being spent on salary and overtime pay for all municipal employees.
  
As a Data Engineer, I have to create high-quality data pipelines that are dynamic, can be automated, and monitored for efficient operation. The project team also includes the city’s quality assurance experts who will test the pipelines to find any errors and improve overall data quality.

The source data resides in Azure Data Lake and needs to be processed in a NYC data warehouse in Azure Synapse Analytics. The source datasets consist of CSV files with Employee master data and monthly payroll data entered by various City agencies.

<img src="screenshot/db-schema.jpeg" alt="data model" width="1000">

### For this project, we will do our work in the Azure Portal, using several Azure resources including:
  - Azure Data Lake Gen2
  - Azure SQL DB
  - Azure Data Factory
  - Azure Synapse Analytics

# TASKS

### 1.Create the data lake and upload data

Create an Azure Data Lake Storage Gen2 (storage account) with three directories in this storage container named
  - dirpayrollfiles
  - dirhistoryfiles
  - dirstaging

<img src="screenshot/" alt="data model" width="1000">

Upload data from the project data to the dirpayrollfiles folder
![image](https://user-images.githubusercontent.com/61830624/198900724-0b64e006-dac5-401a-8db2-f511d115cc6f.png)


Upload `nycpayroll_2020.csv` file (historical data) from the project data to the dirhistoryfiles folder
<img src="screenshot/" alt="data model" width="1000">

### 2. Create an Azure Data Factory Resource
<img src="screenshot/" alt="data model" width="1000">





