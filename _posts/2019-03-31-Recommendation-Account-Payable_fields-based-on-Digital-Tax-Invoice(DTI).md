---
layout: post
title:  "Recommendation Account Payable Fields base on Digital Tax Invoice(DTI)"
date:   2019-03-31
excerpt: "Machine Learning in ERP System"
categories:
- Project
tag:
- ERP
- AP
- DTI
- General Account
- Vendor
- Tax Code
- Cost Center 
comments: false
---

## Project Description

In Korea, companies use DTI(Digital Tax Invoice) system to tax about their purchases and sales. The charge of payment in the company need to check the DTI and create AP(Account Payable)s for payment. The employees have to fill several fields of AP like title of general account, tax code, vendor information, and etc.

Our team including me analyzed the DTI and AP dataset and try to recommend appropriate value of fields in AP documents. This recommendation make employees to support their repetitive task. For this recommendation, we analyze DTI and AP dataset and categorize several input fields. Also, we make derivation values from original fields.

There are two distinct algorithms. The one is the Hierarchical Algorithm that has two step. First step groups  output field's values into the higher level values and predict this group value. And second step predict final values in each group. 

Another is the Hybrid Algorithm that is ensemble model. The Hybrid Algorithm has 3 different models; Simple Association Rule based model, Naive Bayesian, Decision Tree. This algorithm try to all kind of models and vote each model's result by decision tree.

In real environment, we choose the better algorithm by each fields of DTI.

For this project, the following steps: 

1. Preprocessing raw data and create derivation values(grouping)
2. Choose the algorithm by target value
3. Training
   1. Hierarchical Algorithm
      1. Group values into the higher level
      2. Training SVM (X: inputs, y:higher level value)
      3. Split dataset by group value
      4. Training SVM by each group
   2. Hybrid Algorithm
      1. Make Association Rules
      2. Train Naive Bayesian and Decision Tree
      3. Train Voting model using Decision Tree
4. Predict each values by related trained model

For this project, using the following skills:

![python](https://img.shields.io/badge/python-green.svg?logo=python&style=for-the-badge&colorB=AAAAAA){:class="img-badge"} ![scala](https://img.shields.io/badge/scala-green.svg?logo=scala&style=for-the-badge&colorB=AAAAAA){:class="img-badge"} ![brightics](https://img.shields.io/badge/Brightics-green.svg?logo=samsung&style=for-the-badge&colorB=AAAAAA){:class="img-badge"} ![Spark](https://img.shields.io/badge/Spark-green.svg?style=for-the-badge&colorB=AAAAAA){:class="img-badge"} 

----

## Raw Data

## Preprocess Data and Create derivation values

## Make Train Batch Dataset

##  Train by Dual Algorithm

## Predict