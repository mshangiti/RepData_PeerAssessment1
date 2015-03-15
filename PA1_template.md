---
title: "Reproducible Research: Peer Assessment 1"    
---    


## Loading and preprocessing the data
Loading the data:

```r
activity <- read.csv("activity.csv");
```

## What is mean total number of steps taken per day?
1. Calculate the total number of steps taken per day

```r
totalSteps<-aggregate(steps~date,data=activity,sum,na.rm=TRUE)
```

2. Make a histogram of the total number of steps taken each day

```r
hist(totalSteps$steps);
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png) 

3. Calculate and report the mean and median of the total number of steps taken per day:

```r
mean(totalSteps$steps);
```

```
## [1] 10766.19
```

```r
median(totalSteps$steps);
```

```
## [1] 10765
```
* The mean is 1.0766189\times 10^{4} steps per day.
* The median is 10765 steps per day.



## What is the average daily activity pattern?
1. Make a time series plot:

```r
stepsInterval<-aggregate(steps~interval,data=activity,mean,na.rm=TRUE)
plot(steps~interval,data=stepsInterval,type="l")
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png) 

2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

```r
stepsInterval[which.max(stepsInterval$steps),]$interval
```

```
## [1] 835
```
* it's the 835 interval.

## Imputing missing values
1. Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)

```r
sum(is.na(activity$steps))
```

```
## [1] 2304
```
* It's 2304 missing value.

2. Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.
* The following is a function that gives us the mean for a given interval

```r
intervalMean<-function(interval){
    stepsInterval[stepsInterval$interval==interval,]$steps
}
```

3. Create a new dataset that is equal to the original dataset but with the missing data filled in.

```r
activityNew<-activity   # Make a new dataset with the original data
count=0           # Count the number of data filled in
for(i in 1:nrow(activityNew)){
    if(is.na(activityNew[i,]$steps)){
        activityNew[i,]$steps<-intervalMean(activityNew[i,]$interval)
        count=count+1
    }
}
cat("Total ",count, "NA values were filled.")  
```

```
## Total  2304 NA values were filled.
```

4. Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?

```r
totalStepsWithoutNA<-aggregate(steps~date,data=activityNew,sum)
hist(totalStepsWithoutNA$steps)
```

![](PA1_template_files/figure-html/unnamed-chunk-10-1.png) 

```r
mean(totalStepsWithoutNA$steps)
```

```
## [1] 10766.19
```

```r
median(totalStepsWithoutNA$steps)
```

```
## [1] 10766.19
```



## Are there differences in activity patterns between weekdays and weekends?

1. Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.

```r
activityNew$day=ifelse(as.POSIXlt(as.Date(activityNew$date))$wday%%6==0,
                          "weekend","weekday")
# For Sunday and Saturday : weekend, Other days : weekday 
activityNew$day=factor(activityNew$day,levels=c("weekday","weekend"))
```

2. Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.

```r
stepsIntervalWithoutNA=aggregate(steps~interval+day,activityNew,mean)
library(lattice)
xyplot(steps~interval|factor(day),data=stepsIntervalWithoutNA,aspect=1/2,type="l")
```

![](PA1_template_files/figure-html/unnamed-chunk-12-1.png) 
