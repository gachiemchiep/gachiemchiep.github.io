---
title: "Book : Deep Learning Engineering"
excerpt: "Personal note when reading "Deep Learning Engineering"."
categories: 
  - Machine Learning
tags: 
  - book
comments: true
toc: true
support: true
published: false
order: 9
author: vugia.truong
---

# Summary

Found a book called "Deep Learning Engineering" by [Andriy Burkov](https://www.linkedin.com/in/andriyburkov/). The book is available for free reading at [http://www.mlebook.com/wiki/doku.php](http://www.mlebook.com/wiki/doku.php)



## Chapter 1: Introduction

Explaining some Definitions and notation used in the Machine Learning field. This chapter can be skipped

Some good points are as follows

* When to use Machine Learning

```bash
When the Problem Is Too Complex for Coding
When the Problem Is Constantly Changing
When It Is a Perceptive Problem
When It Is an Unstudied Phenomenon
When the Problem Has a Simple Objective
When It Is Cost-Effective
```

* When not to use Machine Learning

```bash
• every change in the system’s behavior compared to its past behavior in a similar
situation has to be explainable,
• the cost of an error made by the system is too high,
• you want to get to the market as fast as possible,
• getting right data is too complex or impossible,
• you know how to solve the problem using traditional software development at a lower
cost,
• the phenomenon has too many outcomes while you cannot get a sufficient amount of
examples to represent those outcomes (like in video games or word processing software),
• you build a system that will not have to be improved frequently over time,
• you can fill an exhaustive lookup table manually by providing the expected output
for any input (that is the number of possible input values is not too large or getting
outputs is fast and cheap).
```

* The scope of Machine Learning Engineering 

Note: some of the points above conflict with each other. In my opinion, 

![Machine Learning engineering](/assets/images/2020/MLE_001.jpg)

## Chapter 2: before the project start

Good points :

* very difficult to estimate complexity (cost-time) for machine learning project
* Machine Learning Team can be divided in 2 ways
  * Separating Machine Learning Team into data scientists, software engineering
  * All engineer need to know both machine learning and software engineering skills
* DevOps engineers should be included in the Machine Learning Team to automate deployment, loading, monitoring

## Chapter 3: Data Collection and Preparation

Good dataset has the following properties:

• it contains enough information that can be used for modeling,
• it has good coverage of what you want to do with the model,
• it reflects real inputs that the model will see in production,
• is as unbiased as possible,
• is not a result of the model itself,
• it has consistent labels,
• it is big enough to allow generalization

Working with biased dataset

![Machine Learning engineering](/assets/images/2020/MLE_002.jpg)

Can use the **Tomek links** for undersampling

![Machine Learning engineering](/assets/images/2020/MLE_003.png)

Working with missing attributes : use the data imputation technique to add missing attribute, or we can remove the missing attribute from data

Working wtih big-data : we can use the following sampling methods to take only small part of data for modelling

To be continued

# Personal comments

This book expalined shallowly a lot of term and definition used in Machine Learning field.
But it lacks a big real-world example to glue everything together. 
Like in the [Design Patterns books](https://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612), in the first part all of design pattern were explained. Then in the second part, the authors demonstrate a GUI application which use all of design patterns in the first part. Through this demonstrations, the readers can understand the role of each pattern in real-world software development. The "Machine Learning Engineering" book lack this important part. As a result, the book itself become a collection medium tutorial pages. I hope in the future, the author can add the missing link and make this book valuable enough to be bought and kept in book-shelves.


