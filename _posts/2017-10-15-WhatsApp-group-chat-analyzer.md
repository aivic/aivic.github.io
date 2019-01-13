---
title: "WhatsApp group chat analyzer"
header:
  teaser: /images/projects/whatsapp_logo.png
date: 2017-10-15
categories:
  - Blogs
tags: 
  - Machine learning
  - Exploratory data analysis
  - Data visualization
  - Pandas
  - Python
  - Numpy
  - Matplotlib
excerpt: "Whatsapp time and top-words frequency analyzer"
---

**Just another weekend fun**  
This small project covers topics like:
* finding top frequent words used by your friends in a group 
* finding peak chatting time


I have done mainly following things regarding data cleaning:
* Concatenating same people chats which are taken in next line during encoding. 
* Splitting Timestamp and Chats in different columns
* Mapping phone numbers to their person holder names
* Removing messages when someone changes their numbers
* removing rows with either of the cases:
    - image omitted
    - video omitted
    - GIF omitted
* Visualization  
A demoable output:  
![Final result of a day](/images/projects/whatsapp_chat_freq.png)  
![Final result of a day](/images/projects/whatsapp_top_words.png)
The project is available here: [Whatsapp group chat analyzer](https://github.com/vivekec/datascience/tree/gh-pages/projects/misc/whatsapp_msg_analyser)
