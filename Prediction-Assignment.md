---
title: "Prediction Assignment"
output: 
  html_document: 
    keep_md: yes
---



## Data Proccessing

First the data are imported into R and formatted:


```r
dat = read.csv("C:/Users/U410DA/Desktop/coursera/Practical Machine learning/pml-training.csv")
dat$classe<-as.factor(dat$classe)
dat[,7:159] <- sapply( dat[,7:159], as.numeric )
```

```
## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion

## Warning in lapply(X = X, FUN = FUN, ...): NAs introduced by coercion
```

```r
library(caret)
```

```
## Loading required package: lattice
```

```
## Loading required package: ggplot2
```

```r
library(gbm)
```

```
## Loaded gbm 2.1.5
```

## Splitting Dataset into Test/Train datasets

Since the dataset is rather large, only 1/4 of the data and only 6 predictors are used as the training data set. THe results later showed that this was sufficient to build a good model.


```r
set.seed(200)
inTrain = createDataPartition(dat$classe, p = 1/4)[[1]]


training = dat[ inTrain,c(6:11,160)]
testing = dat[-inTrain,]
```

## Fitting a model

A GBM model was constructed to predict the value of "classe"



```r
set.seed(200)

modelFit2<-train(classe~.,data=training,method="gbm",verbose=FALSE, na.action = na.pass)
```

## Results

Confusion Matrices show high accuracy on both the training and testing datasets:


```r
confusionMatrix(table(training$classe,predict(modelFit2,training) ))
```

```
## Confusion Matrix and Statistics
## 
##    
##        A    B    C    D    E
##   A 1393    2    0    0    0
##   B    3  944    3    0    0
##   C    0    1  855    0    0
##   D    0    2    0  802    0
##   E    7    1    0    6  888
## 
## Overall Statistics
##                                           
##                Accuracy : 0.9949          
##                  95% CI : (0.9925, 0.9967)
##     No Information Rate : 0.2859          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.9936          
##                                           
##  Mcnemar's Test P-Value : NA              
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9929   0.9937   0.9965   0.9926   1.0000
## Specificity            0.9994   0.9985   0.9998   0.9995   0.9965
## Pos Pred Value         0.9986   0.9937   0.9988   0.9975   0.9845
## Neg Pred Value         0.9972   0.9985   0.9993   0.9985   1.0000
## Prevalence             0.2859   0.1936   0.1749   0.1647   0.1810
## Detection Rate         0.2839   0.1924   0.1742   0.1634   0.1810
## Detection Prevalence   0.2843   0.1936   0.1744   0.1638   0.1838
## Balanced Accuracy      0.9962   0.9961   0.9981   0.9960   0.9983
```

```r
confusionMatrix(table(testing$classe,predict(modelFit2,testing) ))
```

```
## Confusion Matrix and Statistics
## 
##    
##        A    B    C    D    E
##   A 4149   34    0    0    2
##   B   10 2827    9    1    0
##   C    0    6 2549   11    0
##   D    0    3    0 2402    7
##   E   43    4    0   24 2634
## 
## Overall Statistics
##                                           
##                Accuracy : 0.9895          
##                  95% CI : (0.9878, 0.9911)
##     No Information Rate : 0.2856          
##     P-Value [Acc > NIR] : < 2.2e-16       
##                                           
##                   Kappa : 0.9868          
##                                           
##  Mcnemar's Test P-Value : NA              
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity            0.9874   0.9836   0.9965   0.9852   0.9966
## Specificity            0.9966   0.9983   0.9986   0.9992   0.9941
## Pos Pred Value         0.9914   0.9930   0.9934   0.9959   0.9738
## Neg Pred Value         0.9950   0.9960   0.9993   0.9971   0.9993
## Prevalence             0.2856   0.1953   0.1738   0.1657   0.1796
## Detection Rate         0.2820   0.1921   0.1732   0.1632   0.1790
## Detection Prevalence   0.2844   0.1935   0.1744   0.1639   0.1838
## Balanced Accuracy      0.9920   0.9910   0.9975   0.9922   0.9954
```





