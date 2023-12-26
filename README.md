# Big-Tech-Stock-Market-Analysis

# Overview
This project demonstrates an end-to-end MS Azure pipeline for big tech stock comparative analysis. It is an emulation of real-world business pipelines and solely for the sake of self-learning. The pipeline starts from a local SQL database and ends with a visualization in tableau. It utilizes the lakehouse architecture for cloud storage of both raw and processed data for analytical needs. The project uses the following modules:
- On-prem SQL Database
- Azure Data Factory
- Azure Data Lake Storage Gen2
- Azure Databricks 
- Azure Synapse Analytics
- Tableau

# Pipeline
![image](https://github.com/seandixit/Big-Tech-Stock-Market-Analysis/assets/153400712/9e9db63a-e69f-4cb6-987a-735abbe43180)

# Data Ingestion
The dataset used contains historical daily prices for tickers trading on NASDAQ. It contains prices for up to 01 of April 2020 and can be found on Kaggle: https://www.kaggle.com/datasets/jacksoncrow/stock-market-dataset/data.

For our purposes, data for 6 stocks was used: Apple, Facebook, Tesla, Twitter, Nvidia, Google. Raw datasets can be found in the raw folder.





