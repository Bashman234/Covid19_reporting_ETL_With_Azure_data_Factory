# Covid19 Reporting ETL Pipeline with Azure Data Factory
This Project is about ingesting Covid-19 Data from the ECDC website, transforming it using various ADF components, then loading it into SQL Datawarehouse for the Analytics to derive useful and actionable insights from the various data.
We want to know the impact of Covid in the European Region as a whole during the year 2020.

## Azure Data Factory
Azure Data Factory (ADF) is a fully managed, serverless data integration solution for ingesting, preparing, and transforming all your data at scale. It enables every organization in every industry to use it for a rich variety of use cases: data Engineering, migrating their on-premises SSIS packages to Azure, operational data integration, analytics, ingesting data into data warehouses, and more.

### Situation
We have various Covid-19 data from multiple sources. 
Due to the fact that these datasets are scattered everywhere, it became relatively difficult to extract useful information from them.

### Task
The task of this project is to ingest the dataset from the various data sources, clean and transform the data to make it more robust and fit for purspose. 
The cleaned data should then be loaded into a central repo, like a Datawarehosue and Datalake so that the analytics team can work with it via BI tools such as Power BI. Also, we can also run ML Models on Datalake.

#### Environment Setup
* Azure Subscription
* Data Factory 
* Azure Blob Storage Account
* Data Lake Storage Gen 2
* Azure SQL Database
* Azure Databricks Cluster
* HD Insight Cluster
* Azure Devops
#### Solution Architecture Diagram
<img src="https://github.com/Bashman234/Covid19_reporting_ETL_With_Azure_data_Factory/blob/main/images2/solution_architecture.png">

#### Extraction/Ingestion
Four different datasets was ingested from both the ECDC website and azure blob storage into Datalake Gen2.
They are;
* Cases and Deaths Data
* Hospital Admissions Data
* Population Data
* Test Conducted Data

We used various components of **ADF Pipeline** activities to ingest the data from both HTTP Data Source and Azure Storage Account to Azure DataLake. 
Some of those activities are;
* Validation Activity
* Get Metadata Activity
* Copy Activity

#### Transformation
The Cases and Deaths data together with the Hospital admissions data was trasnformed using ADF Data flows.
The Data Flows transformation used on both dataset include;
* Select transformation
* LookUp transformation
* Filter transformation
* Join transformation
* Sort transformation
* Conditional split transformation
* Derived columns transformation
* Sink transformation etc

These set of transformations was done to make the datasets more robust and fit for purpose.
<img src="https://github.com/Bashman234/Covid19_reporting_ETL_With_Azure_data_Factory/blob/main/images2/transformation_cases_and_deaths.png">
<img src="https://github.com/Bashman234/Covid19_reporting_ETL_With_Azure_data_Factory/blob/main/images2/transformation_cases_and_deaths2.png">

For the Population dataset, Azure databricks was solely used to transform it so that it can be robust and fit for purpose.
The following steps was taken to transform the data on Azure Databricks.
* Azure Datalake storage was mounted on databricks so as to allow databricks read the data into the pyspark notebook easily.
<img src="https://github.com/Bashman234/Covid19_reporting_ETL_With_Azure_data_Factory/blob/main/images2/databricks_mount.png">

* Next the Population data was read and a temp view was created which was followed by pivoting the dataset by age group as shown below
<img src="https://github.com/Bashman234/Covid19_reporting_ETL_With_Azure_data_Factory/blob/main/images2/population_databricks.png">

* The LookUp data was read, followed by a Join transformation before the transformed data was written back to the mount point in ADLS
<img src="https://github.com/Bashman234/Covid19_reporting_ETL_With_Azure_data_Factory/blob/main/images2/join_databricks.png">

#### Load
Now that the datasets are now fully prepared, we loaded it into the SQL Datawarehouse for the Analytics team to consume with their BI tools with ease.
* The first thing was to create the tables in the SQL database that will house each of the datasets
<img src="https://github.com/Bashman234/Covid19_reporting_ETL_With_Azure_data_Factory/blob/main/images2/create_covid_tables_sql.png">
* After creating the tables, the copy activity was used to load the dataset to its respective tables in the SQL Datawarehouse.

### Result and Analysis
#### The SQL Datawarehouse was connected to Power BI for the following Insights and Visualizations
* Covid-19 Trend in the EU/EEA & UK 2020 by Cases, Deaths, Hospital Occupancy and ICU Occupancy
<img src="https://github.com/Bashman234/Covid19_reporting_ETL_With_Azure_data_Factory/blob/main/images2/powerbi_report1.png">

* Covid-19 Cases and Death breakdown by population in the UK, France and Germany
<img src="https://github.com/Bashman234/Covid19_reporting_ETL_With_Azure_data_Factory/blob/main/images2/powerbi_report2.png">


* Total Number of Covid test carried out Vs Confirmed Cases
<img src="https://github.com/Bashman234/Covid19_reporting_ETL_With_Azure_data_Factory/blob/main/images2/powerbi_report2.png">



