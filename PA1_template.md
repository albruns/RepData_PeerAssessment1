---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---


## Loading and preprocessing the data

### Package requiered: *base*

```r
DT<-read.csv("activity.csv", header=T)
```

```
## Warning in file(file, "rt"): cannot open file 'activity.csv': No such file
## or directory
```

```
## Error in file(file, "rt"): cannot open the connection
```


## What is mean total number of steps taken per day?

### Histogram total number of steps:


```r
steps<-by(DT$steps, DT$date, function(x) sum(x))
hist(steps)
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2-1.png) 

### Mean number of steps per day : 1.0766189 &times; 10<sup>4</sup>
### Median number of steps per day 10765

## What is the average daily activity pattern?


```r
dailyavg<-by(DT$steps, DT$interval, function(x) mean(x, na.rm="true"))
plot(dailyavg, type="l", xlab="interval", ylab="avg steps")
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3-1.png) 


### Maximum interval: 520 th minute

## Imputing missing values

### NA values are imputed by the mean across all days


```r
nas<-is.na(DT$steps)
DTimp<-DT
DTimp[nas,"steps"]<-mean(steps, na.rm="true")
```
### Calculate histogram, mean and median with imputed dataset


```r
steps2<-by(DTimp$steps, DTimp$date, function(x) sum(x))
hist(steps2)
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5-1.png) 

### Mean number of steps per day : 4.1599847 &times; 10<sup>5</sup> Difference: 4.0523228 &times; 10<sup>5</sup>
### Median number of steps per day: 1.1458 &times; 10<sup>4</sup> Difference: 691.8113208



## Are there differences in activity patterns between weekdays and weekends?


```r
DTweekdays<-weekdays(as.Date(DT$date))
DTweekdays[DTweekdays=="lunes"]<-1
DTweekdays[DTweekdays=="martes"]<-1
DTweekdays[DTweekdays=="miercoles"]<-1
DTweekdays[DTweekdays=="jueves"]<-1
DTweekdays[DTweekdays=="viernes"]<-1
DTweekdays[DTweekdays=="sabado"]<-0
DTweekdays[DTweekdays=="domingo"]<-0
DT1<-cbind(DT,DTweekdays)
weekday<-DT1[DT1$DTweekdays==1,]
weekdayavg<-by(weekday$steps, weekday$interval, function(x) mean(x, na.rm="true"))
weekend<-DT1[DT1$DTweekdays==0,]
weekendavg<-by(weekend$steps, weekend$interval, function(x) mean(x, na.rm="true"))
par(mfrow=c(2,1)) 
plot(weekdayavg, type="l", xlab="interval weekday", ylab="avg steps", main="weekdays")
```

```
## Warning in min(x): no non-missing arguments to min; returning Inf
```

```
## Warning in max(x): no non-missing arguments to max; returning -Inf
```

```
## Warning in min(x): no non-missing arguments to min; returning Inf
```

```
## Warning in max(x): no non-missing arguments to max; returning -Inf
```

```
## Error in plot.window(...): endliche 'xlim' Werte nötig
```

```r
plot(weekendavg, type="l", xlab="interval weekend", ylab="avg steps", main="weekend")
```

```
## Warning in min(x): no non-missing arguments to min; returning Inf
```

```
## Warning in max(x): no non-missing arguments to max; returning -Inf
```

```
## Warning in min(x): no non-missing arguments to min; returning Inf
```

```
## Warning in max(x): no non-missing arguments to max; returning -Inf
```

```
## Error in plot.window(...): endliche 'xlim' Werte nötig
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6-1.png) 

###Yes there are differences
