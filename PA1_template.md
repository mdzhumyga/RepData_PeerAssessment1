# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data


```r
setwd("C:/Disk A/Different/Economentrics/Coursera/Reprodusible research")
activitydata <- read.csv("./activity.csv", header = TRUE)
head(activitydata)
```

```
##   steps       date interval
## 1    NA 2012-10-01        0
## 2    NA 2012-10-01        5
## 3    NA 2012-10-01       10
## 4    NA 2012-10-01       15
## 5    NA 2012-10-01       20
## 6    NA 2012-10-01       25
```



## What is mean total number of steps taken per day?
Calculate mean and median

```r
agg_by_date <- aggregate(activitydata$steps, by = list(date = activitydata$date), 
    FUN = mean, na.rm = TRUE)
mean(agg_by_date$x, na.rm = TRUE)
```

```
## [1] 37.38
```

```r
median(agg_by_date$x, na.rm = TRUE)
```

```
## [1] 37.38
```

Make a histogram of the total number of steps taken each day


```r
library(datasets)
hist(agg_by_date$x, xlab = "steps be date", ylab = "frequency", main = "Histogram of total number of steps taken each day")
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3.png) 


## What is the average daily activity pattern?
Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)

```r
agg_by_interval <- aggregate(activitydata$steps, by = list(interv = activitydata$interval), 
    FUN = mean, na.rm = TRUE)
agg_by_interval$index <- agg_by_interval$interv/5

plot(agg_by_interval$index, agg_by_interval$x, main = "Time series plot", type = "l", 
    pch = 16, xlab = "daily interval", ylab = "Average number of steps")
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4.png) 

#Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

```r
subset(agg_by_interval, agg_by_interval$x == max(agg_by_interval$x, na.rm = TRUE))$index
```

```
## [1] 167
```


## Imputing missing values
Total number of N/A's

```r
for (Var in names(activitydata)) {
    missing <- sum(is.na(activitydata[, Var]))
    if (missing > 0) {
        print(c(Var, missing))
    }
}
```

```
## [1] "steps" "2304"
```

Devise a strategy for filling in all of the missing values in the dataset- missing values could be replaced with mean/median for that 5-minute interval, 

