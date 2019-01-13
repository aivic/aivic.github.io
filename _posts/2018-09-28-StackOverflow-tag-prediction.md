---
title: "Kaggle: Stack Overflow Tag prediction"
header:
  teaser: /images/blogs/so_logo.jpg
date: 2018-09-28
categories:
  - Project
tags: 
  - Python
  - Machine Learning
  - EDA
excerpt: "Suggest the tags based on the question content"
---

# Problem statement
Suggest the tags based on the question content on Stack Overflow (SO).

### Source
[Kaggle: Facebook recruiting III keyword extraction](https://www.kaggle.com/c/facebook-recruiting-iii-keyword-extraction)

### Focus area
To achieve high precision and recall without higher latency keeping in mind that incorrect tags can impact customer experience on SO.

### Data source
The data is available on [Kaggle](https://www.kaggle.com/c/facebook-recruiting-iii-keyword-extraction/data) and briefly summarised here - 

* **Train.csv** size 6.75 GB, contains 4 columns: Id, Title, Body, Tags.
* **Test.csv** size 2 GB, contains the same columns but without the Tags, which you are to predict.

# Formulating a ML problem
Since for each question (title + body), we need to predict at max 5 tags. Hence, this is a **multi-label classification** problem. Refer [scikit-learn](http://scikit-learn.org/stable/modules/multiclass.html) for more information on this topic.  

The performance matric on Kaggle for this competition is **mean F score** which is the weighted average of precision and recall where 1 represents the best value and 0 represents the worst value.  

<a href="https://www.codecogs.com/eqnedit.php?latex=Mean&space;F1&space;score&space;=&space;2\frac{precision&space;*&space;recall}{precision&space;&plus;&space;recall}" target="_blank"><img src="https://latex.codecogs.com/gif.latex?Mean&space;F1&space;score&space;=&space;2\frac{precision&space;*&space;recall}{precision&space;&plus;&space;recall}" title="Mean F score = 2\frac{precision * recall}{precision + recall}" /></a>

We can also measure **Hamming Loss** which accounts for the accuracy in a multi-label classification.

# Exploratory Data Analysis

### Step 1: Loading the data
We start by creating a database using [SQLite](https://www.sqlite.org/index.html) engine out of given CSV files (just to explore SQLite) with a chunksize of nearly 0.2 million.

### Step 2: Checking duplicates
To fetch number of duplicates from .db file, we run following command -  
```python
pd.read_sql_query("""SELECT Title, Body, Tags, COUNT(\*) as cnt_dup FROM data GROUP BY Title, Body, Tags""")
```

giving us 1827881 questions as duplicate, nearly 30.29% 

 
| Occurence of duplicate questions | How many such questions |
| ------------- | ------------- |
|1 |    2656284|
|2 |   1272336|
|3 |    277575|
|4 |        90|
|5 |        25|
|6 |         5|

Next, we find the number of times tag appear for each question and find their distribution.

| Occurence of tags | Number of questions |
| ------------- | ------------- |
|3  |  1206157|
|2  |  1111706|
|4  |   814996|
|1  |   568298|
|5 |     505158|

Finally, duplicates have been removed from the database.

### Step 3: Analysis of Tags
Left with nearly 4.2 million question, the number of unique tags came out to be 42048. 

**I.** Plotting total number of times a tag appears in descending order results into a highly right-skewed distribution as shown.  

![](/images/projects/Kaggle_StackOverflow_tag_prediction/1.png)

Observations -
* There are total 153 tags which are used more than 10000 times.
* 14 tags are used more than 100000 times.
* Most frequent tag (i.e. c#) is used 331505 times.
* Since some tags occur much more frequenctly than others, Micro-averaged F1-score (Mean F-score) is the appropriate metric.

**II.** Looking for tags per question  

![](/images/projects/Kaggle_StackOverflow_tag_prediction/2.png)

Observations - 
* Maximum number of tags per question: 5
* Minimum number of tags per question: 1
* Avg. number of tags per question: 2.899
* Most of the questions are having 2 or 3 tags

**III.** Frequent tags  

To arrive on frequent tags, simple wordcloud is a good choice.  

![](/images/projects/Kaggle_StackOverflow_tag_prediction/3.png)

Also, we can look for top-most 20 tags using a bar-chart.  

![](/images/projects/Kaggle_StackOverflow_tag_prediction/4.png)

### Data preprocessing




# References
* [Applied AI course](https://www.appliedaicourse.com)
