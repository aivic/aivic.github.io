---
title: "Kaggle: Quora question pair similarity"
header:
  teaser: /images/projects/quora_logo.jpg
date: 2018-02-15
categories:
  - Project
tags: 
  - Python
  - Machine Learning
  - EDA
excerpt: "Identify duplicate questions on Quora"
---

# Problem statement
To predict which of the provided pairs of questions contain two questions with the same meaning. 

### Source
[Kaggle: Quora question pair](https://www.kaggle.com/c/quora-question-pairs)

### Focus area
To achieve a probability of a pair of questions to be duplicates so that you can choose any threshold of choice with minimal misclassification.

### Data source
The data is available on [Kaggle](https://www.kaggle.com/c/quora-question-pairs/data), features of which are briefly summarised here - 

* id - the id of a training set question pair
* qid1, qid2 - unique ids of each question (only available in train.csv)
* question1, question2 - the full text of each question
* is_duplicate - the target variable, set to 1 if question1 and question2 have essentially the same meaning, and 0 otherwise.

# Formulating a ML problem
Since the target column is binary (0 - no similarity, 1 - similar), hence it's a **binary classification** problem.  

The metric as suggested by Kaggle for this competition is **Log Loss** which is absolutely necessary to predict the certainity of two question similarity in terms of probability. A perfect value of log-loss is 0 and worst is 1.


# Exploratory Data Analysis

### Loading the data
The data is loaded into a pandas dataframe with nearly 400K rows in `train.csv`.


# References
* [Applied AI course](https://www.appliedaicourse.com)
* [Kaggle](https://www.kaggle.com/c/quora-question-pairs)
* [Quora](https://www.quora.com)
