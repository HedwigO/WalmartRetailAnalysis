# WalmartRetailAnalysis

# Overview
This project focuses on predicting Walmart's retail profits and identifying key features that significantly influence profitability. By leveraging historical retail data and employing robust machine learning techniques, the aim is to derive actionable insights to optimize business strategies and improve overall performance.

To achieve this goal, big data tools such as MapReduce and Scala are used to preprocess the data. By training a XGBoost regression model, we can predict the profit using customer, sales and product, shipping, and geographical data. In addition, there is a feature importance analysis using SHAP (SHapley Additive exPlanations) to help people identify the most important features affecting profits.

This topic is important to investigate because the results can provide important insights to understand the dynamics that drive sales and customer satisfaction, therefore helping the decision-making process on sales resource allocation and management.

# How to Run the Model
All code needed for this project are included in this github repository. For the data, you would need to download them from Data World follow the instructions in the "Data Source" section below. After downloading the code and data, you can put them in the your local or drive.

For the code files ended with .java, you would need to run them on Dataproc following these command:

```bash
javac -classpath `yarn classpath` -d . MapperFile.java
javac -classpath `yarn classpath` -d . ReducerFile.java
javac -classpath `yarn classpath`:. -d . DriverFile.java

jar -cvf DriverFile.jar *.class

hadoop fs -put NameOfWalmartDataset.csv
hadoop jar DriverFile.jar DriverFile NameOfWalmartDataset.csv DictionaryName

hdfs dfs -ls DictionaryName

hdfs dfs -cat DictionaryName/part-r-00000

hdfs dfs -get DictionaryName/part-r-00000
```
For the code file ended with .py, you would need to create a PySpark section on Dataproc following this command:

```bash
pyspark --deploy-mode client
```

Please run the code in this order: Data Profiling --> Data Cleaning --> Training_Evaluation. Notice that the model training, model evaluation, and feature importance analysis are in the same train.py file, so if you run that file, then you will get all results you want.

# Data Source  
There are two datasets in this project. The Walmart retail data from 2012 to 2015 is created by the user Gary Hoover on Dec 10, 2021, accessed on Mar 21, 2024. The Walmart retail data from 2019 to 2023 is created by the user Mnif Ahmed on Aug 27, 2023, accessed on Mar 21, 2024. Let us denote the two datasets ”Walmart1215” and ”Walmart1923” in this project.
Walmart 1215 has the size of 1.22 MB, with 25 columns (1 target profit and 24 features) and 8,399 rows. Walmart 1923 has the size of 269.25MB, with 23 columns (1 target profit and 22 features) and 1,037,247 rows.

Due to the size limit, I cannot upload those datasets here. Please download the datasets from the original sources on Data World. Many thanks to the data source from: 

- [Walmart1215](https://data.world/garyhoove470/walmart-retail-dataset) provided by `@Gary Hoover` on Data World
- [Walmart1923](https://data.world/ahmedmnif150/walmart-retail-dataset) provided by `@Mnif Ahmed` on Data World
