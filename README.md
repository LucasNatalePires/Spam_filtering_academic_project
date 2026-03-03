# Spam_Filtering_Academic_Project

## CA 2: Data Analysis Project: Spam Filtering

### Description
This repository contains the analysis of a real-world dataset describing and classifying emails as Spam (True) or Not Spam (False). The goal of this project is to prepare the dataset for the creation of a machine learning (ML) model capable of filtering spam emails. The analysis includes data exploration, data preparation for classification, and dimensionality reduction using PCA (Principal Component Analysis).

### Files in the Repository
- **Dataset, notebook.ipynb**: Jupyter Notebook with the complete analysis, including:
  - Characterization of the dataset
  - Application of data cleaning and exploratory data analysis (EDA) techniques
  - Application of PCA for dimensionality reduction
  - Explanations and justifications for the choices made in each step
- **requirements.txt**: Lists all dependencies needed to run the Jupyter Notebook. The code requires a CSV file as input.

---

## Justifications for the Choices

### 1. Dataset Characterization
The dataset characterization included analyzing the number of attributes, observations, presence of missing values, and other important details. This step was crucial to understanding the data structure before starting the preparation and analysis process.

### 2. Data Preparation and Exploratory Data Analysis (EDA)
Data preparation included cleaning (removing inconsistent data and renaming columns) and applying EDA techniques, such as graphical visualizations (e.g., bar charts, histograms, and boxplots), to explore the distribution of data and the relationships between attributes.  

The visualizations helped understand key attribute distributions, such as the presence of words in emails and their relationship with spam classification.

### 3. PCA - Dimensionality Reduction
Dimensionality reduction was performed using PCA (Principal Component Analysis), aiming to retain 99.5% of the variance from the original dataset. This process was necessary due to the high dimensionality of the data and helped identify the minimum number of features needed to build an efficient model.  

After applying PCA, the number of features was reduced to a more manageable value, retaining most of the data variance.

### 4. Curse of Dimensionality
The detailed explanation of the "Curse of Dimensionality" and how it may affect the analysis is included in the notebook. In summary, it refers to issues that arise when the number of features (or dimensions) in a dataset becomes too high, leading to increased complexity in modeling and analysis.

---

## Visualizations in the Project
- **Chart 1**: Bar chart showing the distribution of classes (Spam vs. Not Spam)  
- **Chart 2**: Histogram of an important variable, such as the presence of key words  
- **Chart 3**: Boxplot to identify outliers in the data  
- **Chart 4**: Visualization of principal components after dimensionality reduction using PCA  

*These visualizations were generated during the Exploratory Data Analysis (EDA) and are used to justify the decisions made in data preparation and dimensionality reduction.*
