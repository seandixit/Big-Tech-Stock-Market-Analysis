# Big-Tech-Stock-Market-Analysis-Using-MS-Azure

# Overview
This project demonstrates an end-to-end MS Azure pipeline for big tech stock comparative analysis. It is an emulation of real-world business pipelines and solely for the sake of self-learning. The pipeline starts from a local SQL database and ends with a visualization in tableau. It utilizes the lakehouse architecture for cloud storage of both raw and processed data for analytical needs. The project uses the following modules:
- On-prem SQL Database
- Azure Data Factory
- Azure Data Lake Storage Gen2
- Azure Databricks 
- Azure Synapse Analytics
- Tableau

The dataset used contains historical daily prices for tickers trading on NASDAQ. It contains prices for up to 01 of April 2020 and can be found on Kaggle: https://www.kaggle.com/datasets/jacksoncrow/stock-market-dataset/data.

For our purposes, data for 6 tech companies was used: Apple, Facebook, Tesla, Twitter, Nvidia, Google. Raw datasets can be found in the raw folder.
An SQL database was created on MS SQL Server Management Studio with the 6 csv files as tables.

# Pipeline
![image](https://github.com/seandixit/Big-Tech-Stock-Market-Analysis/assets/153400712/9e9db63a-e69f-4cb6-987a-735abbe43180)

## Data Ingestion
Data ingestion from the on-premises SQL server to Azure SQL is accomplished via Azure Data Factory. The process involves:

- Installation of Self-Hosted Integration Runtime.
- Establishing a connection between Azure Data Factory and the local SQL Server.
- Setting up a copy pipeline to transfer all tables from the local SQL server to the Azure Data Lake's "bronze" folder.

## Data Transformation
After ingesting data into the "bronze" folder, it is transformed following the medallion data lake architecture (bronze, silver, gold). 

Azure Databricks, using PySpark, is used for these transformations. Data initially stored in csv format in the "bronze" folder is converted to the delta format as it progresses to "silver" and "gold". The delta format allows for access of previous versions of the data when needed. The transformation is carried out through 3 Databricks notebooks:
- mountstorage- mounts the storage
- bronze-to-silver- to clean data, takes the csv format files from the bronze container, converts into pandas dataframes, unifies header/column names and converts dates from datetime format to date format. The dataframes are then loaded into the silver container in delta format.
- silver-to-gold- takes latest delta file from silver container, converts into pandas dataframe and creates new columns. Columns are as follows: 50 day moving average (ma50), 200 day moving average (ma200), previousDayClose, closing price change from previous day (priceChange), closing price percent change (pricePercentChange), volumePreviousDay, volumeChange, and VolumePercentChange. Any NA value instances are then dropped, and the transformed dataframes are then loaded into the gold container in delta format.

![Ingestion and Transformation in Data Factory](https://github.com/seandixit/Big-Tech-Stock-Market-Analysis/assets/153400712/203e3cad-0438-4b65-9a28-8ccc2fcb37fb)

## Data Loading into Semantic Layer
Now that we have delta files in the gold layer representing our data, we load it into a serverless SQL database using Azure Synapse Analytics. The steps involved are:
- Creating a link from Data Lake Storage Gen 2 to Azure Synapse Analytics
- Writing stored procedures to extract table information as a SQL view.
- Storing views within a server-less SQL Database in Synapse.

![Synapse Pipeline to create views for each company](https://github.com/seandixit/Big-Tech-Stock-Market-Analysis/assets/153400712/feac0dc6-5c6c-482f-b0ce-185b73985683)

## Data Visualization
Tableau is then connected to the Azure SQL database to access the stock tables and create visualizations. 

![Stock_Tableau_GIF](https://github.com/seandixit/Big-Tech-Stock-Market-Analysis/assets/153400712/b9f60e8e-336f-4110-ab91-729fc901e8a7)

