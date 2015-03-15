# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data
Loading the data:

```r
activity <- read.csv("activity.csv");
```

Preprocessing the data:

```r
totalSteps<-aggregate(steps~date,data=activity,sum,na.rm=TRUE)
```

## What is mean total number of steps taken per day?
The mean of steps per day is 10765 and is computed as follows:

```r
mean(totalSteps$steps);
```

```
## [1] 10766.19
```



## What is the average daily activity pattern?



## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?

