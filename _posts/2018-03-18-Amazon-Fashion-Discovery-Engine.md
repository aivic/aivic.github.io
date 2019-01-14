---
title: "Amazon Fashion Discovery Engine"
header:
  teaser: /images/projects/amazon_logo.png
date: 2018-03-18
categories:
  - Project
tags: 
  - Python
  - Machine Learning
  - EDA
excerpt: "Recommend similar apparel products using description and images"
---

# Problem statement
To recommend similar apparel products in e-commerce using description and images

### Data Source
[Amazon Product Advertising API](https://affiliate-program.amazon.in/assoc_credentials/home)  

The extracted json file has following focused features -
* Brand
* Color
* Product type name
* Medium image URL
* Title
* Formatted price

# Exploratory Data Analysis

Number of data points ~ 183K

### Missing data 
We targeted the products whose price are missing and drop the products, in favor of reducing the dataset to 28K data points. 

### Checking on duplicates, 
We have 2325 products which have same title but different color. So we can drop these and keep only the uniques here.  

There are also 2 possibilities of dupes like -  

**First possibility (title differ in end) -**  
woman's place is in the house and the senate shirts for Womens XXL White  
woman's place is in the house and the senate shirts for Womens M Grey  

**Second possibility (only few words differ in title) -**  
UltraClub Women's Classic Wrinkle-Free Long Sleeve Oxford Shirt, Pink, XX-Large  
UltraClub Ladies Classic Wrinkle-Free Long-Sleeve Oxford Light Blue XXL  

We remove such possibilities as well as those titles whose length is less than 4 words, leaving us to 16K data points.

### Stopwords and Stemming
We removed possible stopwords and implemeted Porter Stemmer on title feature.

# Product similarity model using text features
We implemented various similarity models using only `Title` feature, as listed -
* Bag of Words
* TF-IDF
* IDF
* Word2Vec
* TF-IDF weighted Word2Vec
* IDF weighted Word2Vec

While getting the results from each of these methods, the last one presents the best similar products.  
We further combined weighted `Color` and `Brand` features along with `Title` feature to increase the similarity.

**Input text (using only title) -**  
burnt umber tiger tshirt zebra stripes xl xxl  

**Recommended products -**  
pink tiger tshirt zebra stripes xl xxl  
brown white tiger tshirt tiger stripes xl xxl  
grey white tiger tank top tiger stripes xl xxl  

# Product similarity using Images
We used VGG16 (a type of CNN architecture) to find similar apparels implemted in Keras backed by Tensorflow.

```python
# dimensions of our images.
img_width, img_height = 224, 224
nb_train_samples = 16042
epochs = 50
batch_size = 1

# the weights are taken from imagenet
```
Output -
Target product -  

![](/images/projects/Amazon-fashion-discovery-engine/1.jpg)  

Recommended products -

![](/images/projects/Amazon-fashion-discovery-engine/2.jpg)
![](/images/projects/Amazon-fashion-discovery-engine/3.jpg)
![](/images/projects/Amazon-fashion-discovery-engine/4.jpg)
![](/images/projects/Amazon-fashion-discovery-engine/5.jpg)
![](/images/projects/Amazon-fashion-discovery-engine/6.jpg)
![](/images/projects/Amazon-fashion-discovery-engine/7.jpg)
![](/images/projects/Amazon-fashion-discovery-engine/8.jpg)

# References
* [Applied AI course](https://www.appliedaicourse.com)
* [Amazon](https://www.amazon.com)
