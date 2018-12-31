---
title: "Scraping Overflow"
date: 2018-12-31
header:
  teaser: /images/blogs/so_logo.jpg
categories:
  - Blogs
tags: 
  - Pandas
  - SQL
  - R
  - Scraping
  - StackOverflow
  - API  
excerpt: "Making a dataset on Pandas question answered by 40 Gold users"
---

This works focuses upon creating a data set on **Pandas** Q/A over [StackOverflow](https://stackoverflow.com/tags/pandas/info).
Presently, there are more than [90k+ questions](https://stackoverflow.com/questions/tagged/pandas?sort=newest) available on StackOverflow which are asked under Pandas section. Many questions on SO have bad quality or are duplicate of already answered questions. A new SO user can ask a question which can fall in any of these sections. Similarly, a new SO user might not flag a question if it doesn't abide with SO guidelines. Therefore, users who have spent a long of efforts on SO are the ones who can classify a question as duplicated, can close them, downvote, etc.

We focus upon 40 such users who have earned [Pandas gold tag](https://stackoverflow.com/help/badges/3296/pandas) on their profile which in simple term means that they have answered enough questions to at least evaluate an upcoming question quality.

# Implementation

There is no need to perform any web scraping as such to extract SO data. SO provides an online API where one can simply run **SQL** query to get a downloadable CSV file. But before we start with the query, we need to fetch the SO Id of these 40 users. Their Id is present on their profile URL.
E.g.: https://stackoverflow.com/users/3877338/johne
Here, user *johne* has Id *3877338*

## Step 1
To do this I visited [this](https://stackoverflow.com/help/badges/3296/pandas) page and scrapped their feature which is *div.user-details a*.

## Step 2
Once I have the feature which gives me the profile link of all 40 users, I extracted these links using simple **R** script using *rvest*. Linked [here](https://github.com/aivic/blogs/blob/master/SO-pandas-dataset/scrapping.R).

## Step 3
Now I have a CSV file in which I stored the username and their IDs. I choose 10 Ids at a time, so making 4 sets of total 40 Ids and passed them to given SQL query on [Compose Query](https://data.stackexchange.com/stackoverflow/queries). Query linked [here](https://github.com/aivic/blogs/blob/master/SO-pandas-dataset/extractor.sql).

This resulted into 4 CSV files which is concatenated using given code in Python (code linked [here](https://github.com/aivic/blogs/blob/master/SO-pandas-dataset/concat_sets.py)) resulting into a final CSV file uploaded as [Kaggle dataset](https://www.kaggle.com/vivek42/pandas-qa-on-stack-overflow/home).


# Your takeaways 
Visit [Kaggle](https://www.kaggle.com/vivek42/pandas-qa-on-stack-overflow/home), read the Overview and try to arrive on a result which answers *What it takes for an answer to be accepted when one of the associated tag is "pandas"*

Happy learning!
