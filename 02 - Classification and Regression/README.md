# Classification and regression

In this session, you will learn how to use tools in Azure Machine Learning along with some code in Python, clean and transform data. Collectively, data scientists refer to these processes as ‘data munging’. You will work with a version of the automotive price dataset which has been modified to illustrate some of the points.

Solved version: https://gallery.cortanaintelligence.com/Experiment/Autos-price-prediction-Analytics-in-a-day

## Part 1: Ingesting and Joining Data

Most data science experiments require you to ingest data from one or more sources, and potentially join data from diverse sources based on a common keyfield. Here, you will explore data from two source files, and then create an Azure Machine Learning experiment in which you read data from the sources and implement a join to merge the two datasets into a single table.

## Part 2: Manipulating Data and Metadata

Here you will start to prepare the joined automotive dataset so that it is ready for meaningful exploration and analysis. You will remove missing values, remove outliers, create new features and basic manipulation.
<ol>
    <li>Remove Unnecessary Columns</li>
    <li>Create a Grouped Categorical Feature</li>
    <li>Remove Rows with Missing or Repeated Values</li>
    <li>Create a Calculated Column</li>
    <li>Rename Columns</li>
</ol>

## Part 3: Finding and Filtering Outliers

Here,you will apply methods for detecting and filtering outliers in the automotive dataset. Detectingand filtering of outliers is often anecessary step in preparing a dataset for analysis. In this step you will give you hands-on experience with this process. Detecting and classifying outliers can be challenging. Typically, several iterations are needed to identify and confirm the presence of outliers. Further, when identifying outliers, some caution is required. Be sure you are filter outliers, rather than interesting or unusual values that may beimportant in to your analysis.


## Part 4: Scaling Numeric Columns

Here you will scale the numeric columns in the dataset. With the outliers in the numeric features removed, the dataset is now ready to scale. Scaling ensures that features with large numeric values will not dominate the training of a machine learning model.

