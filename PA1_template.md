# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data


```r
library(knitr)
library(ggplot2)
```

1. Load the data


```r
setwd("C:\\DataScience\\05-ReproducibleResearch\\RepData_PeerAssessment1")
unzip("activity.zip")
activity <- read.csv("activity.csv", header = TRUE, sep = ",",
colClasses=c("numeric", "character", "numeric"))
```

2. Process/transform the data


```r
activity$date <- as.Date(activity$date, format = "%Y-%m-%d")
activity$interval <- as.factor(activity$interval)
```

## What is mean total number of steps taken per day?

1. Calculate the total number of steps taken per day


```r
stepsPerDay <- aggregate(steps ~ date, activity, sum)
colnames(stepsPerDay) <- c("date","steps")
head(stepsPerDay)
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

2. Make a histogram of the total number of steps taken each day


```r
ggplot(stepsPerDay, aes(x = steps)) + 
geom_histogram(col = "red", fill = "lightblue", binwidth = 500) + 
labs(title="Histogram - Total Number Of Steps Taken Each Day", 
x = "Steps per Day", y = "Daily Frequency") + theme_bw() 
```

![](PA1_template_files/figure-html/histogram-1.png) 

3. Calculate and report the mean and median total number of steps taken per day

```r
meanStepsPerDay <- mean(stepsPerDay, na.rm=TRUE)
medianStepsPerDay <- median(stepsPerDay, na.rm=TRUE)
```

The mean is **9354.23**.
The median is **10395**.

## What is the average daily activity pattern?

1.Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)


```r
stepsPerInterval <- aggregate(steps ~ interval, data=activity, FUN=mean)
colnames(stepsPerInterval) <- c("interval", "steps")
head(stepsPerInterval)
```

```
##   interval     steps
## 1        0 1.7169811
## 2        5 0.3396226
## 3       10 0.1320755
## 4       15 0.1509434
## 5       20 0.0754717
## 6       25 2.0943396
```

```r
ggplot(stepsPerInterval, aes(x=interval, y=steps)) +   
       geom_line(color="orange", size=1) +  
       labs(title="Average Daily Activity Pattern", x="Interval", y="Number of steps") +  
       theme_bw()
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png) 


2.Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?


```r
maxStepsPerInterval <- stepsPerInterval$interval[which.max(stepsPerInterval$steps)]
```

## Imputing missing values

1.Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)


2.Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.


3.Create a new dataset that is equal to the original dataset but with the missing data filled in.


4.Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?



## Are there differences in activity patterns between weekdays and weekends?

1.Create a new factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day.


2.Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.

