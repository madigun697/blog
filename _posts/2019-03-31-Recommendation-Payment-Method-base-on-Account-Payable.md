---
layout: post
title:  "Recommendation Payment Method base on Account Payable"
date:   2019-03-31
excerpt: "Machine Learning in ERP System"
categories:
- Project
tags:
- ERP
- AP
- Payment Method
comments: false
---

![Accuracy](https://user-images.githubusercontent.com/8471958/56104493-e38ba100-5f72-11e9-8e7e-56a6f16ef574.png)
    

<center>The Accuracy by batch</center>



## Project Description

There are a lot of AP(Account Payable)s in the company. In general, the companies manage these data using the ERP system. However, some kind of properties is entered by human efforts. This task consume lots of time and human efforts. For this reason, companies want to save their time and human resources making the machine recommend the appropriate value in each properties.

Our team including me try to analyze and recommend the payment method using previous ERP data. At first, we use boosting algorithm like Light GBM. However, the boosting algorithm is too slow to use in real-time environment and the customer want to explainable results. So we use Naive Bayesian and Decision Tree algorithms.

For real-time environment, I split dataset by proposal time and re-bind by batch size(5000). Each batch data is trained using two different algorithms; The data including text data is trained by Multinomial Naive Bayesian after vectorizing by TF-IDF. Another data excluding text data is trained by Very Fast Decision Tree(VFDT) algorithm that is available batch training.

For this project, the following steps: 

1. Preprocessing raw data to analyze
2. Split train dataset by proposal time and re-bind by batch size
3. Batch Train
   1. Dataset with text
      1. Tokenize and remove stop words in text field
      2. TF-IDF Vectorizing
      3. Multinomial Naive Bayesian
   2. Dataset without text
      1. One-hot encoding
      2. Train by Very Fast Decision Tree(VFDT)
4. Predict payment method in Test Dataset

For this project, using the following skills:

![python](https://img.shields.io/badge/python-green.svg?logo=python&style=for-the-badge&colorB=AAAAAA){:class="img-badge"} ![scala](https://img.shields.io/badge/scala-green.svg?logo=scala&style=for-the-badge&colorB=AAAAAA){:class="img-badge"} ![brightics](https://img.shields.io/badge/Brightics-green.svg?logo=samsung&style=for-the-badge&colorB=AAAAAA){:class="img-badge"} ![Spark](https://img.shields.io/badge/Spark-green.svg?style=for-the-badge&colorB=AAAAAA){:class="img-badge"} 

----

## Raw Data

## Preprocess Data and Create derivation values

## Make Train Batch Dataset

##  Train by Dual Algorithm

## Predict