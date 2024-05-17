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

# Data Flow Diagram
![Data Flow of Project](https://github.com/HedwigO/WalmartRetailAnalysis/assets/97476561/7fa12736-e356-46b8-8add-6834d86a1835)
A more detailed data ingestion, data preprocessing, model training, and model evaluation explanation is inlcuded below:

# Data Preprocess
The data preprocessing stage is crucial for ensuring that the datasets are clean, consistent, and ready for analysis. Here are the detailed steps involved:

1. Data Ingestion and Initial Cleaning
- Loading the Datasets: Fist, loading the Walmart1215.csv and Walmart1923.csv datasets from the data directory.
- Standardizing Column Names: Ensuring that column names are consistent across both datasets to facilitate merging and analysis.
- Handling Missing Values: Removing rows containing placeholder values such as '\N'. This is done to prevent the inclusion of incomplete or corrupt data, which could adversely affect subsequent analyses.

2. Data Profiling
Identifying Unique Records: Using HDFS MapReduce to identify unique records within the dataset. This helps in understanding the distribution and variance of the data, which is crucial for selecting appropriate modeling techniques and for feature engineering during the machine learning pipeline construction.

3. Combining Datasets
Merging Datasets: Combining Walmart1215 and Walmart1923 datasets for a comprehensive analysis. Ensuring that the structure of the combined dataset is consistent.

4. Encoding Categorical Variables
Label Encoding: Converting categorical variables into numerical values using label encoding.

5. Feature Scaling
Normalizing Data: Applying feature scaling to ensure that the numerical features are on a similar scale.

# Model Training and Evaluation
The model training stage involves building and evaluating the XGBoost model to predict profits from Walmart's retail data.

1. Splitting the Data
Train-Test Split: Dividing the dataset into training and testing sets to evaluate model performance.

2. Training the Model
XGBoost Model: Training the XGBoost model using the training dataset. XGBoost is chosen for its robustness and ability to handle complex interactions within the data.

3. Evaluating the Model
Performance Metrics: Evaluating the model's performance using RMSE (Root Mean Square Error) to measure prediction accuracy.

4. Feature Importance Analysis
SHAP Values: Applying SHAP (SHapley Additive exPlanations) to interpret the model’s predictions and visualize the impact of each feature on the prediction.

![Feature Importance of Model](https://github.com/HedwigO/WalmartRetailAnalysis/assets/97476561/2c3b9de7-dfc4-4fa8-a1a9-89a8e78311bb)

Above is the SHAP diagram. Observe that:

- **Sales & Unit Price** have the highest positive impact on profits, meaning that higher sales/unit prices will lead to higher profits.
- **Product Base Margin & Product Container** have notable correlations with profits, meaning that type of product container might affect shipping costs, thus affects overall profits
- **Geographical Features (City, Zip Code, and Region)**: have smaller SHAP value, reflecting local market conditions, demographic differences, and economic factors that can influence consumer behavior and, consequently, Walmart’s profitability in different regions.
- **Discount & Customer Age** can have both positive and negative impacts on profits. For **Discount**, higher discounts will boost sales volume, but it will reduce the profit margin per unit at the same time. For **Customer Age**, customers at different ages may have different spending power and preferences.
