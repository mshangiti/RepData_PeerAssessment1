# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data
Loading the data:

```r
activity=read.csv("activity.csv");
```

Preprocessing the data:

```r
totalSteps<-aggregate(steps~date,data=activity,sum,na.rm=TRUE)
head(totalSteps)
```

```
##         date steps
## 1 2012-10-02   126
## 2 2012-10-03 11352
## 3 2012-10-04 12116
## 4 2012-10-05 13294
## 5 2012-10-06 15420
## 6 2012-10-07 11015
```

## What is mean total number of steps taken per day?



## What is the average daily activity pattern?



## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
