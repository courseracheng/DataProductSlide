---
title       : Shiny Project
subtitle    : Random Forest for Longley data
author      : Cheng Wang
job         : 
framework   : io2012   # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax]            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---


## Introduction

The Shiny app use random forest as machine learning algorithm and the highly collinear data set Longley as training set to show how to select the parameters for random forest. In this shiny app:

1. Longley data set is discussed. 
2. Random forest parameter mtry is discussed. 
3. Both parameters mtry and ntree are discussed.

Note:

- The random forest is discussed in the Practical Machine Learning course in Coursera. The detail algorithm is not discussed here.

---

## Data 

Longley data
- 7 variables
- Target variable: Employed
- 6 Features
- Highly collinear as the figure in the shiny app


```r
library(MASS)
names(longley)
```

```
## [1] "GNP.deflator" "GNP"          "Unemployed"   "Armed.Forces"
## [5] "Population"   "Year"         "Employed"
```

---


## mtry

`caret` package is used for selecting mtry as the code below. Plots also shown in the shiny app.


```r
suppressMessages(library(MASS));suppressMessages(library(caret));
rffitscan = train(Employed ~ ., data = longley, tuneLength=5)
rffitscan$results
```

```
##   mtry     RMSE  Rsquared    RMSESD RsquaredSD
## 1    2 1.214886 0.8629211 0.4842170  0.2023636
## 2    3 1.242251 0.8581076 0.5085266  0.2075792
## 3    4 1.224036 0.8618742 0.4991155  0.2042514
## 4    5 1.222141 0.8598252 0.5050756  0.2097231
## 5    6 1.225497 0.8609199 0.4922192  0.2033362
```

Two metrics can be used for selecting the best mtry.

1. RMSE (root mean square error) or MSE (mean square erro);
2. Rsquared


--- 

## ntree and mtry

Besides `mtry`, another parameter for random forest is `ntree`. plots of MSE (or Rsquare) versus number of trees are shown in the shiny app. Both mtry and ntree can be tuned in shiny app.



```r
mbest=2
suppressMessages(library(MASS));suppressMessages(library(randomForest));set.seed(1)
rffit = randomForest(Employed ~ ., data = longley, mtry=mbest, ntree=5000)
plot(rffit$mse, xlab="trees", ylab="MSE", type="l",pch=16)
```

<img src="assets/fig/unnamed-chunk-3-1.png" title="plot of chunk unnamed-chunk-3" alt="plot of chunk unnamed-chunk-3" style="display: block; margin: auto;" />
