---
title: "Kaggle: Netflix movie rating recommendation system"
header:
  teaser: /images/projects/Netflix-movie-recommendation/netflix_logo.jpg
date: 2018-06-06
categories:
  - Project
tags: 
  - Python
  - Machine Learning
  - EDA
excerpt: "Recommend movies rating based on user interests"
---

# Problem statement
Predict the rating that a user would give to a movie that he has not yet rated.

### Data sources
Get the data from [Kaggle](https://www.kaggle.com/netflix-inc/netflix-prize-data) and convert all 4 files into a CSV file having features:  
* Movie ID
* User ID
* Date (on which user gave rating)
* Rating (on a scale of 5)

### Exploratory Data Analysis

With analysis, we see that there is **no missing** and **no duplicate** data.  

Checking on unique entities:  

* Total no of unique ratings : 100480507
* Total No of unique Users   : 480189
* Total No of unique movies  : 17770

After Train(80%):Test(20%) data split, we perform below operations.  

**Distribution of rating**  

![](/images/projects/Netflix-movie-recommendation/1.png)

**Number of ratings per month**  

![](/images/projects/Netflix-movie-recommendation/2.png)

**Distribution of ratings given by users**  

![](/images/projects/Netflix-movie-recommendation/3.png)  

Majority of users are giving very less number of ratings as cleared from the right skewed PDF.  

**Distribution of ratings grouped by movies**  

![](/images/projects/Netflix-movie-recommendation/4.png)  

Many (Popular) movies are getting large number of ratings as compared to other movies.  

**Number of ratings on each day**  

![](/images/projects/Netflix-movie-recommendation/5.png)  

![](/images/projects/Netflix-movie-recommendation/6.png)  

### Constructing a compressed sparse matrix  

A compressed sparse row matrix with user ID (~480K) as index and movie ID (~17K) as features.




# References
* [Applied AI course](https://www.appliedaicourse.com)
* [Kaggle](https://kaggle.com)
