# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

We download, extract and import the data.

```r
download.file("https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip",
              "repdata-data-activity.zip")
unzip("repdata-data-activity.zip")
activity <- read.csv("activity.csv")
```


## What is mean total number of steps taken per day?

The first assignment is divided in 2 parts. The first is
 
 > Make a histogram of the total number of steps taken each day

This is done with a `barplot`

```r
totalstepsperday <- tapply(activity$steps, activity$date, sum, na.rm = TRUE)
barplot(totalstepsperday, xlab = "Days", ylab = "Total steps taken")
```

![](PA1_template_files/figure-html/unnamed-chunk-2-1.png)

The second part is 

 > Calculate and report the mean and median total number of steps taken
per day

This is done for the mean by

```r
tapply(activity$steps, activity$date, mean, na.rm = TRUE)
```

```
## 2012-10-01 2012-10-02 2012-10-03 2012-10-04 2012-10-05 2012-10-06 
##        NaN  0.4375000 39.4166667 42.0694444 46.1597222 53.5416667 
## 2012-10-07 2012-10-08 2012-10-09 2012-10-10 2012-10-11 2012-10-12 
## 38.2465278        NaN 44.4826389 34.3750000 35.7777778 60.3541667 
## 2012-10-13 2012-10-14 2012-10-15 2012-10-16 2012-10-17 2012-10-18 
## 43.1458333 52.4236111 35.2048611 52.3750000 46.7083333 34.9166667 
## 2012-10-19 2012-10-20 2012-10-21 2012-10-22 2012-10-23 2012-10-24 
## 41.0729167 36.0937500 30.6284722 46.7361111 30.9652778 29.0104167 
## 2012-10-25 2012-10-26 2012-10-27 2012-10-28 2012-10-29 2012-10-30 
##  8.6527778 23.5347222 35.1354167 39.7847222 17.4236111 34.0937500 
## 2012-10-31 2012-11-01 2012-11-02 2012-11-03 2012-11-04 2012-11-05 
## 53.5208333        NaN 36.8055556 36.7048611        NaN 36.2465278 
## 2012-11-06 2012-11-07 2012-11-08 2012-11-09 2012-11-10 2012-11-11 
## 28.9375000 44.7326389 11.1770833        NaN        NaN 43.7777778 
## 2012-11-12 2012-11-13 2012-11-14 2012-11-15 2012-11-16 2012-11-17 
## 37.3784722 25.4722222        NaN  0.1423611 18.8923611 49.7881944 
## 2012-11-18 2012-11-19 2012-11-20 2012-11-21 2012-11-22 2012-11-23 
## 52.4652778 30.6979167 15.5277778 44.3993056 70.9270833 73.5902778 
## 2012-11-24 2012-11-25 2012-11-26 2012-11-27 2012-11-28 2012-11-29 
## 50.2708333 41.0902778 38.7569444 47.3819444 35.3576389 24.4687500 
## 2012-11-30 
##        NaN
```
and for the median by

```r
tapply(activity$steps, activity$date, median, na.rm = TRUE)
```

```
## 2012-10-01 2012-10-02 2012-10-03 2012-10-04 2012-10-05 2012-10-06 
##         NA          0          0          0          0          0 
## 2012-10-07 2012-10-08 2012-10-09 2012-10-10 2012-10-11 2012-10-12 
##          0         NA          0          0          0          0 
## 2012-10-13 2012-10-14 2012-10-15 2012-10-16 2012-10-17 2012-10-18 
##          0          0          0          0          0          0 
## 2012-10-19 2012-10-20 2012-10-21 2012-10-22 2012-10-23 2012-10-24 
##          0          0          0          0          0          0 
## 2012-10-25 2012-10-26 2012-10-27 2012-10-28 2012-10-29 2012-10-30 
##          0          0          0          0          0          0 
## 2012-10-31 2012-11-01 2012-11-02 2012-11-03 2012-11-04 2012-11-05 
##          0         NA          0          0         NA          0 
## 2012-11-06 2012-11-07 2012-11-08 2012-11-09 2012-11-10 2012-11-11 
##          0          0          0         NA         NA          0 
## 2012-11-12 2012-11-13 2012-11-14 2012-11-15 2012-11-16 2012-11-17 
##          0          0         NA          0          0          0 
## 2012-11-18 2012-11-19 2012-11-20 2012-11-21 2012-11-22 2012-11-23 
##          0          0          0          0          0          0 
## 2012-11-24 2012-11-25 2012-11-26 2012-11-27 2012-11-28 2012-11-29 
##          0          0          0          0          0          0 
## 2012-11-30 
##         NA
```
The data look really bad. Not so many `NA`s, but a lot of `0`s.
This makes some computations useless, like the median that is always `0`.

## What is the average daily activity pattern?

Again, two main steps for this assignment

 > Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis)
and the average number of steps taken, averaged across all days (y-axis)

First the plot, again with `base` plotting system


```r
meanstepsperinterval <- tapply(activity$steps, activity$interval, mean, na.rm = TRUE)
plot(unique(activity$interval), meanstepsperinterval, type = "l", xlab = "Interval number",
     ylab = "Average of steps")
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png)

 > Which 5-minute interval, on average across all the days in the dataset,
contains the maximum number of steps?

Second the most used, in average, time frame to walk


```r
names(meanstepsperinterval)[which(meanstepsperinterval %in% max(meanstepsperinterval))]
```

```
## [1] "835"
```

## Imputing missing values
This assignment has `4` steps

 > Calculate and report the total number of missing values in the dataset
 (i.e. the total number of rows with NAs)

There are many `NA`s; more than 10% of the dataset is filled with `NA`s.

```r
sum(is.na(activity$steps))
```

```
## [1] 2304
```

```r
sum(is.na(activity$steps))/length(activity$steps)*100
```

```
## [1] 13.11475
```


 > Devise a strategy for filling in all of the missing values in the dataset.
 The strategy does not need to be sophisticated. 
 For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

I decide to fill the `NA`s with the mean of the steps per interval

 > Create a new dataset that is equal to the original dataset but with the missing data filled in.

The `newactivity` dataset is created here


```r
newactivity <- activity

for (i in 1:dim(newactivity)[1]) {
    if(is.na(newactivity[i,1])) {
        newactivity[i, 1] <- meanstepsperinterval[as.character(newactivity[i,3])]
    }
}
```

Last part of this assignment is

 > Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. 
 Do these values differ from the estimates from the first part of the assignment?
 What is the impact of imputing missing data on the estimates of the total
daily number of steps?

Again with `base` plotting, the histrogram is a `barplot`


```r
newtotalstepsperday <- tapply(newactivity$steps, newactivity$date, sum, na.rm = TRUE)
barplot(newtotalstepsperday, xlab = "Days", ylab = "Total steps taken - filled database")
```

![](PA1_template_files/figure-html/unnamed-chunk-9-1.png)

And these are mean and median of this new dataset


```r
tapply(newactivity$steps, activity$date, mean, na.rm = TRUE)
```

```
## 2012-10-01 2012-10-02 2012-10-03 2012-10-04 2012-10-05 2012-10-06 
## 37.3825996  0.4375000 39.4166667 42.0694444 46.1597222 53.5416667 
## 2012-10-07 2012-10-08 2012-10-09 2012-10-10 2012-10-11 2012-10-12 
## 38.2465278 37.3825996 44.4826389 34.3750000 35.7777778 60.3541667 
## 2012-10-13 2012-10-14 2012-10-15 2012-10-16 2012-10-17 2012-10-18 
## 43.1458333 52.4236111 35.2048611 52.3750000 46.7083333 34.9166667 
## 2012-10-19 2012-10-20 2012-10-21 2012-10-22 2012-10-23 2012-10-24 
## 41.0729167 36.0937500 30.6284722 46.7361111 30.9652778 29.0104167 
## 2012-10-25 2012-10-26 2012-10-27 2012-10-28 2012-10-29 2012-10-30 
##  8.6527778 23.5347222 35.1354167 39.7847222 17.4236111 34.0937500 
## 2012-10-31 2012-11-01 2012-11-02 2012-11-03 2012-11-04 2012-11-05 
## 53.5208333 37.3825996 36.8055556 36.7048611 37.3825996 36.2465278 
## 2012-11-06 2012-11-07 2012-11-08 2012-11-09 2012-11-10 2012-11-11 
## 28.9375000 44.7326389 11.1770833 37.3825996 37.3825996 43.7777778 
## 2012-11-12 2012-11-13 2012-11-14 2012-11-15 2012-11-16 2012-11-17 
## 37.3784722 25.4722222 37.3825996  0.1423611 18.8923611 49.7881944 
## 2012-11-18 2012-11-19 2012-11-20 2012-11-21 2012-11-22 2012-11-23 
## 52.4652778 30.6979167 15.5277778 44.3993056 70.9270833 73.5902778 
## 2012-11-24 2012-11-25 2012-11-26 2012-11-27 2012-11-28 2012-11-29 
## 50.2708333 41.0902778 38.7569444 47.3819444 35.3576389 24.4687500 
## 2012-11-30 
## 37.3825996
```

```r
tapply(newactivity$steps, activity$date, median, na.rm = TRUE)
```

```
## 2012-10-01 2012-10-02 2012-10-03 2012-10-04 2012-10-05 2012-10-06 
##   34.11321    0.00000    0.00000    0.00000    0.00000    0.00000 
## 2012-10-07 2012-10-08 2012-10-09 2012-10-10 2012-10-11 2012-10-12 
##    0.00000   34.11321    0.00000    0.00000    0.00000    0.00000 
## 2012-10-13 2012-10-14 2012-10-15 2012-10-16 2012-10-17 2012-10-18 
##    0.00000    0.00000    0.00000    0.00000    0.00000    0.00000 
## 2012-10-19 2012-10-20 2012-10-21 2012-10-22 2012-10-23 2012-10-24 
##    0.00000    0.00000    0.00000    0.00000    0.00000    0.00000 
## 2012-10-25 2012-10-26 2012-10-27 2012-10-28 2012-10-29 2012-10-30 
##    0.00000    0.00000    0.00000    0.00000    0.00000    0.00000 
## 2012-10-31 2012-11-01 2012-11-02 2012-11-03 2012-11-04 2012-11-05 
##    0.00000   34.11321    0.00000    0.00000   34.11321    0.00000 
## 2012-11-06 2012-11-07 2012-11-08 2012-11-09 2012-11-10 2012-11-11 
##    0.00000    0.00000    0.00000   34.11321   34.11321    0.00000 
## 2012-11-12 2012-11-13 2012-11-14 2012-11-15 2012-11-16 2012-11-17 
##    0.00000    0.00000   34.11321    0.00000    0.00000    0.00000 
## 2012-11-18 2012-11-19 2012-11-20 2012-11-21 2012-11-22 2012-11-23 
##    0.00000    0.00000    0.00000    0.00000    0.00000    0.00000 
## 2012-11-24 2012-11-25 2012-11-26 2012-11-27 2012-11-28 2012-11-29 
##    0.00000    0.00000    0.00000    0.00000    0.00000    0.00000 
## 2012-11-30 
##   34.11321
```

We can see that by adding these missing datas we did not touch the datas that we 
had before already. In particular the median is still not significative, because
it is due to too many `0`s in the dataset.  
And for the mean, we just mutated the `0`s in a positive value.

## Are there differences in activity patterns between weekdays and weekends?
Last two assignments of the exercise

> Create a new factor variable in the dataset with two levels – “weekday”
and “weekend” indicating whether a given date is a weekday or weekend
day.


```r
weekends <- weekdays(as.Date(newactivity$date), abbreviate = T) == "Sun" |
    weekdays(as.Date(newactivity$date), abbreviate = T) == "Sat"
newactivity$weekdays <- factor(weekends, labels = c("weekday", "weekend"))
```


 > Make a panel plot containing a time series plot (i.e. type = "l") of the
5-minute interval (x-axis) and the average number of steps taken, averaged
across all weekday days or weekend days (y-axis). The plot should look
something like the following, which was creating using simulated data:

I do this last assignment with the `lattice` plotting system.

```r
library(lattice) 
pl <- xyplot(steps ~ interval | weekdays, type = "l", data = newactivity,
             layout=c(1,2), xlab = "Interval number", ylab = "Number of steps")
print(pl)
```

![](PA1_template_files/figure-html/unnamed-chunk-12-1.png)
