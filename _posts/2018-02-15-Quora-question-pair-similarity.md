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

### Missing data
Only `Question 2` has 2 missing values which are filled with blank.

### Distribution of target data
There are more number of unique question pairs (63%) as compared to duplicate question pairs.  

![](/images/projects/Quora_Question_Pair/1.png)

### Number of unique questions combining both columns question1, question2

Total num of  Unique Questions are: 537933  
Number of unique questions that appear more than one time: 111780 (20.8%). Hence, nearly 80% of questions are unique.  
Max number of times a single question is repeated: 157

![](/images/projects/Quora_Question_Pair/2.png)  

![](/images/projects/Quora_Question_Pair/3.png)

### Duplicate data
There are 0 duplicate rows.

## Feature engineering
Constructed few features like:

* freq_qid1 = Frequency of qid1's
* freq_qid2 = Frequency of qid2's
* q1len = Length of q1
* q2len = Length of q2
* q1_n_words = Number of words in Question 1
* q2_n_words = Number of words in Question 2
* word_Common = (Number of common unique words in Question 1 and Question 2)
* word_Total =(Total num of words in Question 1 + Total num of words in Question 2)
* word_share = (word_common)/(word_Total)
* freq_q1+freq_q2 = sum total of frequency of qid1 and qid2
* freq_q1-freq_q2 = absolute difference of frequency of qid1 and qid2

Checking how the features `word_share` and `word_Common` is going to be helpful -

![](/images/projects/Quora_Question_Pair/4.png)  

As observed from above plots, the PDFs of `word_share` for 0 and 1 can be useful as they are neither overlapping completely nor separated apart ideally.

![](/images/projects/Quora_Question_Pair/5.png)  

The `word_Common` doesn't help much in distinguishing 0 from 1.

## Text preprocessing

* Removing html tags
* Removing Punctuations
* Performing stemming
* Removing Stopwords
* Expanding contractions (expanding % symbol to percentage, etc)


# References
* [Applied AI course](https://www.appliedaicourse.com)
* [Kaggle](https://www.kaggle.com/c/quora-question-pairs)
* [Quora](https://www.quora.com)
