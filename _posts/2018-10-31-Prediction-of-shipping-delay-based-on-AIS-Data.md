---
layout: post
title:  "Prediction of shipping delay based on AIS Data"
date:   2018-10-31
excerpt: "Intelligent Logistics Risk Analytics"
project: true
tag:
- AIS
- prediction
- clustering
- vessel
- logistics
comments: false
---

![KRPUS-USLGB_baseline](https://user-images.githubusercontent.com/8471958/49633346-5f616400-fa3c-11e8-89e8-c09471441166.png)
​    

<center><b>The final baseline</b> from Port Busan(KRPUS) to Port Long-beach(USLGB).</center>

## Project Description

The company who use transportation by ship want to know that their carriages will be arrived at the destination port. However, the transportation by ship is not stable and short. For this reasons, many companies try to predict arrival of ship with ship's GPS Data(AIS), their own logistic data and various external data, but their accuracy is too low to use.

Out team including me try  to predict the arrival of ship using only AIS data. Before the prediction, we separate each path in the whole AIS Data. After separating, paths having same start, end clustered into reasonable number of clusters with a single represented path(baseline).

The prediction worked in a point of 70% progress of distance between start, end points. Using the points where the target ship is in, choose the most proper baseline and check the remained consuming time of selected baseline.

For this project, following steps: 

1. Preprocessing raw data to analyze
2. Extract staying point nearby port in raw AIS data and make route path to move between staying point
3. Making baseline of each route(Depart port - Arrival port)
  - Clustering extracted historical paths that have similar distance and time
  - Making major points(latitude, longitude) base on the total distance of the route
  - Fill available point instead of missing points on the baseline
4. Calculate average staying time and area of each port.
5. Calculate the estimated time of arrival using baseline
  - Evaluate appropriate baseline about moving the vessel
  - Estimate time of arrival and whether or not that vessel will be delayed

For this project, using following skills:

![python](https://img.shields.io/badge/python-green.svg?logo=python&style=for-the-badge&colorB=AAAAAA){:class="img-badge"} ![scala](https://img.shields.io/badge/scala-green.svg?logo=scala&style=for-the-badge&colorB=AAAAAA){:class="img-badge"} ![brightics](https://img.shields.io/badge/Brightics-green.svg?logo=samsung&style=for-the-badge&colorB=AAAAAA){:class="img-badge"} ![MSSQL](https://img.shields.io/badge/MSSQL-green.svg?logo=microsoft&style=for-the-badge&colorB=AAAAAA){:class="img-badge"} ![Spark](https://img.shields.io/badge/Spark-green.svg?style=for-the-badge&colorB=AAAAAA){:class="img-badge"} 

----

## Raw Data

## Pre-process

## Extract Baseline

##  Post-process





