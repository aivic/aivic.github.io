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

# References
* [Applied AI course](https://www.appliedaicourse.com)
