# Databook Data Management System Design Challenge
### Author: Zhongyuan(Thomas) Li
### Date: 2022/08/28

## Overview
This repo is the design for the Data Management System


## How you would approach such a project?
1. Have all the requirements
2. List all possible solution(s)/technologies for each item in the requirement list
3. Get those which can cover all the requirements
4. Pick the most suitable solution among the solutions from the previous step

### Requirements and Solutions (candidate)
1. The system will encapsulate existing SQL, NoSQL databases, a large corpus of PDFs, along with any necessary future database technologies.
   - **Solution**: DataLake which can handle structured, unstructured and semi-structured data
2. Downstream applications (e.g. mobile app, web site, ML training) should be agnostic to where data comes from.
   - **Solution**: Database, Data warehouse, or Datalake
3. Non-technical users should be able to self-serve to add and manage new datasets.
   - **Solution**: A user-friendly UI or API 
4. The platform will automate ingesting of these datasets.
   - **Solution**: Job orchestration tool (Airflow, Databricks, Snowflake pipeline, etc..)
5. The system will expose a unified data addressing approach.
   - **Solution**: Catalog/Schema or Any other metadata system
6. Data points can be associated with particular entities (e.g. Companies, Executives, Industries) and can be accessed to be embedded in templated documents.
   - **Solution for data availability**: Data governance (via AWS IMA or Databricks Unity Catalog)
   - **Solution for accessing**: Data can be accessed via files (CSV, PDF, Google Sheet), or newsletter
7. Non-technical users can create expressions to aggregate data points.
   - **Solution**: Interactive dashboard, or SQL endpoint (users have to know how to write query)
8. Dependent data points will be updated automatically.
   - **Solution**: Cross table transaction or real-time update dependent table(s)
9. Users should be able to retrieve the state of data at arbitrary points in time.
   - **Solution**: SLA or pipeline monitoring, and data quality framework
10. The system should be fast enough to support interactive use cases.
    - **Solution**: Big data, and query performance optimization
11. It should be easy for other parts of the stack to make use of this system.
    - **Solution**: system scalability and interface with other systems

## Questions you would ask?
1. **Data volume(size)**: how big the data (whole data, each table, number of columns, etc.) is? what is the frequency of data loading/updating?
2. **Data source(s)**: where does the source data exist? (PDF in S3, FTP server, etc..)
3. **Data format**: Non-structured data, PDF file format, etc..
4. **Downstream application**:How does downstream application consume the data? What are their frequencies?
5. Do the non-technical users know query?
6. What does the templated document mean? Do you mean files like (CSV, JSON, PDF, or Google Sheet)? How to deliver them? (Via email, api, or file delivery) Can we use dashboard to show the data?
7. Does the creating expressions mean writing SQL like query? 
8. What is the updating frequency for dependent data? Daily, Hourly, or realtime? Is the size of dependent data big?
9. What states of data are needed? Completion only? Or include Data Quality? 
10. What are the interactive use cases? And how fast do you require? 
11. How does other part of the stack use this system? Only use the data or use the computation, storage, ML, BI, etc..?

## The resource that you would need?
1. **Communication**: Points of Contact from Source data and downstream application teams
2. **Sample Data**: Sample data of all data sources
3. **Downstream Application**: Template or schema of data deliveries
4. **Data Modeling**: Dependencies and relation among different data
5. **Testing**: All details of testing destinations
6. **Sign-Off**: Acceptance criteria, SLA and validation process (who, when and how?)
7. **Plan/MileStones**: The due and milestone at each stage (plan and tasks)
8. **Engineers**: How many and who can be involved in this project

## The technologies you would consider?
### Infra
   - Because of different data sources and types (SQL, NoSQL, PDF, etc..), data warehouse and database may not good solutions for this data management system.
   - Data lake can be used to store and process different types of data (structured, unstructured, or semi-structured)
   - For future usages, integration with other systems, ML related applications, a data LakeHouse could be a better choice for this project. 

![Screenshot](infra.png)

### Tools/Technologies
![Screenshot](datalakehouse.png)


## How you would execute against the implementation?
1. **Data Ingestion** *: Read data from source and load raw data (no any change) into tables at Bronze layer in delta lake
2. **Data transformation**: Clean/transform/join raw data and save results to tables at Silver layer
3. **Data aggregation**: Save aggregated data into tables at gold layer for AI, BI, or Reporting
4. **Consumption Layer** *: Create consumer layer views directly from Gold tables
5. **Data governance**: Utilize UC (Unity Catalog) to grant users permissions on each table
6. **Data visualization**: Create dashboard (with permission) for specific entity based on the template

![Screenshot](tablelayers.png)


````commandline
Data ingestion: self-onboarding can be done via live table or customized ingestion engine
Consumption Layer: To handle schema changing or pipeline updating avoid impacting downstream applications 
````
### Development and Testing Plan
1. Build self-serve data Ingestion engine to load data
2. Validate data at each step
3. Apply data governance to manage data accessibility
4. Deliver testing data to users
5. Create SLA and data quality to monitor jobs and send alerts for any failure
6. Deploy the above steps into Prod
7. Sign-off
