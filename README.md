# Project 3: Song Play Analysis With S3 and Redshift

## Summary
* [Introduction](#Introduction)
* [Schema design](#Schema-design)
* [ETL process](#ETL-process)
* [Usage](#Usage)
* [Files in repository](#Files-in-repository)
--------------------------------------------

#### Introduction

This database was designed to **Sparkify** for analytics purposes.
In this project it was used to different Amazon Web Services:
* [S3](https://aws.amazon.com/en/s3/) (Data storage)
* [Redshift](https://aws.amazon.com/en/redshift/) (Data warehouse)

Data sources are provided by two ``S3 buckets``:
* Songs and artists data (JSON metadata)
* Users songplays activity (JSON files)

The data warehouse is hosted in the ``Redshift service``. An ETL pipeline will be developed with the following structure:
1. Load data from S3 to staging tables on Redshift 
2. Execute SQL statements that create the analytics tables from these staging tables

--------------------------------------------

#### Schema design

A star schema was defined with the following structure:

**Fact tables:**  
* `songplays`: records in log data associated with song plays.

**Dimensional tables:** 
* `users`:  users in the app.
* `songs`: songs in music database.
* `artists`: artists in music database.
* `time`: timestamps of records in songplays broken down into specific units.

The fact table **songplays** aggregate data from the different dimensinal tables.
Although the dimensional table have all the data required for the analysis, the fact table will provide the metric of the business process in a fastest way, in this case, **songs played by users analysis**.

There are also two staging tables:
* `staging_events`: staging data from song dataset 
* `staging_songs`: staging data from users activity log 


--------------------------------------------

#### ETL process

Most of ETL was developed in SQL (Python used as bridge), done by queries in ``sql_queries.py`` file:
The ETL pipeline was build with the following data cleaning steps:
1. DROP existing tables
2. CREATE new tables
3. COPY data from S3 into stagging tables
4. INSERT data from stagging tables into dimensional tables and the fact table **songplays**.


--------------------------------------------

#### Usage

1. Update `dwh.cfg` file with you Amazon Redshift cluster credentials and IAM role
2. Open a terminal session
3. Set the filesystem on project root folder
4. Run `python create_tables.py` to create the database and respective table
5. Run `python etl.py` to start ETL pipeline to populate the Redshift tables.

--------------------------------------------

#### Files in repository

* <b> create_tables.py </b> - Script to drop old tables in Redshift ad create new tables
* <b> dhw.cfg </b> - Configuration file for Redshift, IAM and S3
* <b> etl.py </b> - Script to execute the ETL queries to extract JSON data from S3 and populates Redshift tables
* <b> sql_queries.py </b> - Strings with SQL queries partitioned by CREATE, DROP, COPY and INSERT statements


--------------------------------------------