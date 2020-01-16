---
layout: post
title: "Twitter Sentiment Analysis"
date: 2020-01-16
---

> “But if thought corrupts language, language can also corrupt thought.”
>   *- George Orwell, 1984*

I have been interested in learning more about text analytics and NLP. Machine learning can seem magical, given the patterns it can draw from data using intuitive mathematical and statistical techniques. But getting a machine 
to understand text would unlock the path for machines to understand human thought.

I decided to start with the basics. I registered for **Sentiment Analysis competition on Datahack**.
The dataset contains tweets from customers about various tech firms who manufacture and sell mobiles, computers, 
laptops, etc. the task is to identify if the tweets have a negative sentiment towards such companies or products.

#### Challenges
The tweets dataset poses unique challenges different from other text corpora like Wikipedia and books. 
* Tweets are mainly a means of expressing an emotion by users, so lot of words would be mis-spelled similar to SMS 
language, like *aaaargggghhhh*, or acronyms like *ROFL*
* Tweets contain a lot of emojis too
* Lots of hashtags contain words combined without any clear word boundary delimiters.
* Since the dataset contains tweets related to technology products and companies, there are a lot of non-dictionary 
words.
* Training dataset is not too large with only ~7.9k records.
* The dataset is also imbalanced with 3:1 ratio of the two classes.

#### Conclusion
These are some of the initial challenges I observed in the initial stages.
