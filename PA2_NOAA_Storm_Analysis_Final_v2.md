---
output: html_document
---

# Population Health and Economic Effects From Storms Across the United States (1950 to 2011)

##Synopsis
This report summarizes which types of storm events were most harmful to population health and had the greatest economic impact across the United States for the years 1950 to 2011. The population health effects are measured by number fatalities and injuries. The economic impact is measured by property damage and crop damage. The storm event that had both the highest fatality and injury incidents was tornado. The storm event that had both the highest property and crop damage was flood. An estimate of the fatalities, injuries, property damage, and crop damage of the top 10 storm events is provided in the results section. More information about the NOAA storm data can be found at [Storm data Documentation](https://d396qusza40orc.cloudfront.net/repdata%2Fpeer2_doc%2Fpd01016005curr.pdf).
#### Author: jdhuffaker

##Data Proccessing

###DownLoad the Storm Data and Read it into R:

```r
# Author: jdhuffaker
# Set the working directory
setwd("C:/Users/jdhuffaker/Documents/Coursera JHU Data Science Courses/05 Reproducible Research/Week 3")

# URL variable for NOAA Storm Data
f_url <- "https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2FStormData.csv.bz2"

# Download storm data and read it into R
library(downloader)
download(url=f_url,destfile="StormData.csv.bz2")
# download.file(url=f_url, destfile='StormData.csv.bz2')
sdata <- read.csv(bzfile('StormData.csv.bz2'), stringsAsFactors = FALSE)
```

###Information about the Data

```r
str(sdata) # Info about data classes, etc.
```

```
## 'data.frame':	902297 obs. of  37 variables:
##  $ STATE__   : num  1 1 1 1 1 1 1 1 1 1 ...
##  $ BGN_DATE  : chr  "4/18/1950 0:00:00" "4/18/1950 0:00:00" "2/20/1951 0:00:00" "6/8/1951 0:00:00" ...
##  $ BGN_TIME  : chr  "0130" "0145" "1600" "0900" ...
##  $ TIME_ZONE : chr  "CST" "CST" "CST" "CST" ...
##  $ COUNTY    : num  97 3 57 89 43 77 9 123 125 57 ...
##  $ COUNTYNAME: chr  "MOBILE" "BALDWIN" "FAYETTE" "MADISON" ...
##  $ STATE     : chr  "AL" "AL" "AL" "AL" ...
##  $ EVTYPE    : chr  "TORNADO" "TORNADO" "TORNADO" "TORNADO" ...
##  $ BGN_RANGE : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ BGN_AZI   : chr  "" "" "" "" ...
##  $ BGN_LOCATI: chr  "" "" "" "" ...
##  $ END_DATE  : chr  "" "" "" "" ...
##  $ END_TIME  : chr  "" "" "" "" ...
##  $ COUNTY_END: num  0 0 0 0 0 0 0 0 0 0 ...
##  $ COUNTYENDN: logi  NA NA NA NA NA NA ...
##  $ END_RANGE : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ END_AZI   : chr  "" "" "" "" ...
##  $ END_LOCATI: chr  "" "" "" "" ...
##  $ LENGTH    : num  14 2 0.1 0 0 1.5 1.5 0 3.3 2.3 ...
##  $ WIDTH     : num  100 150 123 100 150 177 33 33 100 100 ...
##  $ F         : int  3 2 2 2 2 2 2 1 3 3 ...
##  $ MAG       : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ FATALITIES: num  0 0 0 0 0 0 0 0 1 0 ...
##  $ INJURIES  : num  15 0 2 2 2 6 1 0 14 0 ...
##  $ PROPDMG   : num  25 2.5 25 2.5 2.5 2.5 2.5 2.5 25 25 ...
##  $ PROPDMGEXP: chr  "K" "K" "K" "K" ...
##  $ CROPDMG   : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ CROPDMGEXP: chr  "" "" "" "" ...
##  $ WFO       : chr  "" "" "" "" ...
##  $ STATEOFFIC: chr  "" "" "" "" ...
##  $ ZONENAMES : chr  "" "" "" "" ...
##  $ LATITUDE  : num  3040 3042 3340 3458 3412 ...
##  $ LONGITUDE : num  8812 8755 8742 8626 8642 ...
##  $ LATITUDE_E: num  3051 0 0 0 0 ...
##  $ LONGITUDE_: num  8806 0 0 0 0 ...
##  $ REMARKS   : chr  "" "" "" "" ...
##  $ REFNUM    : num  1 2 3 4 5 6 7 8 9 10 ...
```

```r
head(sdata) # First 5 rows
```

```
##   STATE__           BGN_DATE BGN_TIME TIME_ZONE COUNTY COUNTYNAME STATE
## 1       1  4/18/1950 0:00:00     0130       CST     97     MOBILE    AL
## 2       1  4/18/1950 0:00:00     0145       CST      3    BALDWIN    AL
## 3       1  2/20/1951 0:00:00     1600       CST     57    FAYETTE    AL
## 4       1   6/8/1951 0:00:00     0900       CST     89    MADISON    AL
## 5       1 11/15/1951 0:00:00     1500       CST     43    CULLMAN    AL
## 6       1 11/15/1951 0:00:00     2000       CST     77 LAUDERDALE    AL
##    EVTYPE BGN_RANGE BGN_AZI BGN_LOCATI END_DATE END_TIME COUNTY_END
## 1 TORNADO         0                                               0
## 2 TORNADO         0                                               0
## 3 TORNADO         0                                               0
## 4 TORNADO         0                                               0
## 5 TORNADO         0                                               0
## 6 TORNADO         0                                               0
##   COUNTYENDN END_RANGE END_AZI END_LOCATI LENGTH WIDTH F MAG FATALITIES
## 1         NA         0                      14.0   100 3   0          0
## 2         NA         0                       2.0   150 2   0          0
## 3         NA         0                       0.1   123 2   0          0
## 4         NA         0                       0.0   100 2   0          0
## 5         NA         0                       0.0   150 2   0          0
## 6         NA         0                       1.5   177 2   0          0
##   INJURIES PROPDMG PROPDMGEXP CROPDMG CROPDMGEXP WFO STATEOFFIC ZONENAMES
## 1       15    25.0          K       0                                    
## 2        0     2.5          K       0                                    
## 3        2    25.0          K       0                                    
## 4        2     2.5          K       0                                    
## 5        2     2.5          K       0                                    
## 6        6     2.5          K       0                                    
##   LATITUDE LONGITUDE LATITUDE_E LONGITUDE_ REMARKS REFNUM
## 1     3040      8812       3051       8806              1
## 2     3042      8755          0          0              2
## 3     3340      8742          0          0              3
## 4     3458      8626          0          0              4
## 5     3412      8642          0          0              5
## 6     3450      8748          0          0              6
```

```r
#tail(sdata) # Last 5 rows

dimsd <- dim(sdata) # Data frame dimensions

# summary statistics for each variable
summary(sdata$FATALITIES) 
```

```
##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
##   0.0000   0.0000   0.0000   0.0168   0.0000 583.0000
```

```r
summary(sdata$INJURIES)
```

```
##      Min.   1st Qu.    Median      Mean   3rd Qu.      Max. 
##    0.0000    0.0000    0.0000    0.1557    0.0000 1700.0000
```

```r
#summary(sdata$PROPDMG)
#summary(sdata$CROPDMG)

# Unique property and crop damage exponents for determing the cost estimates. Note that there are some data errors and/or missing data for estimating overall costs.
upd <- unique(sdata$PROPDMGEXP)
ucd <- unique(sdata$CROPDMGEXP)
```

Storm data frame dimensions: 902297, 37.

Unique exponents for property damage: K, M, , B, m, +, 0, 5, 6, ?, 4, 2, 3, h, 7, H, -, 1, 8.

Unique exponents for crop damage: , M, K, m, B, ?, 0, k, 2.


###Data Preparation for Final Results

```r
# Calculate actual property damage cost using designators K, M, and B in PROPDMGEXP.
sdata$PropDmgCost[sdata$PROPDMGEXP == "K"] <- 1e3*sdata$PROPDMG[sdata$PROPDMGEXP == "K"]
sdata$PropDmgCost[sdata$PROPDMGEXP == "k"] <- 1e3*sdata$PROPDMG[sdata$PROPDMGEXP == "k"]
sdata$PropDmgCost[sdata$PROPDMGEXP == "M"] <- 1e6*sdata$PROPDMG[sdata$PROPDMGEXP == "M"]
sdata$PropDmgCost[sdata$PROPDMGEXP == "m"] <- 1e3*sdata$PROPDMG[sdata$PROPDMGEXP == "m"]
sdata$PropDmgCost[sdata$PROPDMGEXP == "B"] <- 1e6*sdata$PROPDMG[sdata$PROPDMGEXP == "B"]
sdata$PropDmgCost[sdata$PROPDMGEXP == "b"] <- 1e3*sdata$PROPDMG[sdata$PROPDMGEXP == "b"]

# Calculate actual crop damage cost using designators K, M, and B in CROPDMGEXP.
sdata$CropDmgCost[sdata$CROPDMGEXP == "K"] <- 1e3*sdata$CROPDMG[sdata$CROPDMGEXP == "K"]
sdata$CropDmgCost[sdata$CROPDMGEXP == "k"] <- 1e3*sdata$CROPDMG[sdata$CROPDMGEXP == "k"]
sdata$CropDmgCost[sdata$CROPDMGEXP == "M"] <- 1e6*sdata$CROPDMG[sdata$CROPDMGEXP == "M"]
sdata$CropDmgCost[sdata$CROPDMGEXP == "m"] <- 1e6*sdata$CROPDMG[sdata$CROPDMGEXP == "m"]
sdata$CropDmgCost[sdata$CROPDMGEXP == "B"] <- 1e6*sdata$CROPDMG[sdata$CROPDMGEXP == "B"]
sdata$CropDmgCost[sdata$CROPDMGEXP == "b"] <- 1e6*sdata$CROPDMG[sdata$CROPDMGEXP == "b"]

# Generate summaries of property and crop damage costs (note the number of NAs).
summary(sdata$PropDmgCost)
```

```
##      Min.   1st Qu.    Median      Mean   3rd Qu.      Max.      NA's 
##         0         0      1000    347900     10000 929000000    466255
```

```r
summary(sdata$CropDmgCost)
```

```
##      Min.   1st Qu.    Median      Mean   3rd Qu.      Max.      NA's 
##         0         0         0    125100         0 596000000    618440
```

```r
# Sum fatalities and injuries by types of event and by types of event and time zone.
sdsum1 <- aggregate(cbind(FATALITIES, INJURIES) ~ EVTYPE, data = sdata, sum)

# Sum property and crop damage costs by types of event and by types of event and time zone.
sdsum3 <- aggregate(cbind(PropDmgCost, CropDmgCost) ~ EVTYPE, data = sdata, sum)

# Sum of top 10 event types for fatalities with the sum of all other event types put into "Other" category.
sdsum1a <- sdsum1[order(-sdsum1$FATALITIES),]
sdsum1a$INJURIES <- NULL

a <- nrow(sdsum1a)
sdsum1a$EventType <- sdsum1a$EVTYPE
sdsum1a$EventType[11:a] <- "Other"
sdsum1a1 <- aggregate(FATALITIES ~ EventType, data = sdsum1a, sum)
sdsum1a1 <- sdsum1a1[order(-sdsum1a1$FATALITIES),]
Other_row <- which(grepl("Other",sdsum1a1$EventType))
b <- nrow(sdsum1a1)
sdsum1a1[b+1,] <- sdsum1a1[Other_row,] 
sdsum1a1 <- sdsum1a1[-c(Other_row), ]  

# Sum of top 10 event types for injuries with the sum of all other event types put into "Other" category.
sdsum1b <- sdsum1[order(-sdsum1$INJURIES),]
sdsum1b$FATALITIES <- NULL

a <- nrow(sdsum1b)
sdsum1b$EventType <- sdsum1b$EVTYPE
sdsum1b$EventType[11:a] <- "Other"
sdsum1b1 <- aggregate(INJURIES ~ EventType, data = sdsum1b, sum)
sdsum1b1 <- sdsum1b1[order(-sdsum1b1$INJURIES),]
Other_row <- which(grepl("Other",sdsum1b1$EventType))
b <- nrow(sdsum1b1)
sdsum1b1[b+1,] <- sdsum1b1[Other_row,] 
sdsum1b1 <- sdsum1b1[-c(Other_row), ]  

# Sum of top 10 event types for property damage with the sum of all other event types put into "Other" category.
sdsum3a <- sdsum3[order(-sdsum3$PropDmgCost),]
sdsum3a$CropDmgCost <- NULL

c <- nrow(sdsum3a)
sdsum3a$EventType <- sdsum3a$EVTYPE
sdsum3a$EventType[11:c] <- "Other"
sdsum3a1 <- aggregate(PropDmgCost ~ EventType, data = sdsum3a, sum)
sdsum3a1 <- sdsum3a1[order(-sdsum3a1$PropDmgCost),]
Other_row <- which(grepl("Other",sdsum3a1$EventType))
d <- nrow(sdsum3a1)
sdsum3a1[d+1,] <- sdsum3a1[Other_row,] 
sdsum3a1 <- sdsum3a1[-c(Other_row), ]  

# Sum of top 10 event types for crop damage wwith the sum of all other event types put into "Other" category.
sdsum3b <- sdsum3[order(-sdsum3$CropDmgCost),]
sdsum3b$PropDmgCost <- NULL

c <- nrow(sdsum3b)
sdsum3b$EventType <- sdsum3b$EVTYPE
sdsum3b$EventType[11:c] <- "Other"
sdsum3b1 <- aggregate(CropDmgCost ~ EventType, data = sdsum3b, sum)
sdsum3b1 <- sdsum3b1[order(-sdsum3b1$CropDmgCost),]
Other_row <- which(grepl("Other",sdsum3b1$EventType))
d <- nrow(sdsum3b1)
sdsum3b1[d+1,] <- sdsum3b1[Other_row,] 
sdsum3b1 <- sdsum3b1[-c(Other_row), ]  
```



##Results:

###Question 1: Across the United States, which types of events (as indicated in the EVTYPE variable) are most harmful with respect to population health?

Figure 1: Top 10 Storm Events for Fatalities and Injuries

```r
library(ggplot2)

# Change EventType to a factor and set levels to keep order of EventType.
sdsum1a1$EventType <- factor(sdsum1a1$EventType, levels=sdsum1a1$EventType)

# Generate a bar chart (pareto chart) of the summarized fatalities data (top 10 and Other)
gfat <- ggplot(sdsum1a1, aes(x = EventType, y = FATALITIES)) + geom_bar(stat = "identity", fill = "red") + theme(axis.text.x = element_text(angle = 90, hjust = 1)) + xlab("Event Type") +  ylab("Frequency") + ggtitle("Number of Fatalities in the top 10 Event Types (w/ other event types grouped in Other)")

#plot(gfat)

# Change EventType to a factor and set levels to keep order of EventType.
sdsum1b1$EventType <- factor(sdsum1b1$EventType, levels=sdsum1b1$EventType)

# Generate a bar chart (pareto chart) of the summarized fatalities data (top 10 and Other)
ginj <- ggplot(sdsum1b1, aes(x = EventType, y = INJURIES)) + geom_bar(stat = "identity", fill = "blue") + theme(axis.text.x = element_text(angle = 90, hjust = 1)) + xlab("Event Type") +  ylab("Frequency") + ggtitle("Number of Injuries in the top 10 Event Types (w/ other event types grouped in Other)")

#plot(ginj)


# plot the two graphs in a grid
library(gridBase)
library(gridExtra)

grid.arrange(gfat, ginj, ncol = 1)
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4-1.png) 


Table 1a: Top 10 Storm Events for Number of Fatalities

```r
t1a <- grid.table(sdsum1a1)
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5-1.png) 

Table 1b: Top 10 Storm Events for Number of Injuries 

```r
t1a <- grid.table(sdsum1a1)
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6-1.png) 


###Question 2: Across the United States, which types of events have the greatest economic consequences?

Figure 2: Top 10 Storm Events for Property and Crop Damage

```r
library(ggplot2)

# Change EventType to a factor and set levels to keep order of EventType.
sdsum3a1$EventType <- factor(sdsum3a1$EventType, levels=sdsum3a1$EventType)

# Generate a bar chart (pareto chart) of the summarized property damage cost data (top 10 and Other)
gprop <- ggplot(sdsum3a1, aes(x = EventType, y = PropDmgCost)) + geom_bar(stat = "identity", fill = "red", las=3) + theme(axis.text.x = element_text(angle = 90, hjust = 1)) + xlab("Event Type") +  ylab("Cost") + ggtitle("Property Damage cost of the top 10 Event Types (w/ other event types grouped in Other)")

# Change EventType to a factor and set levels to keep order of EventType.
sdsum3b1$EventType <- factor(sdsum3b1$EventType, levels=sdsum3b1$EventType)

# Generate a bar chart (pareto chart) of the summarized crop damage cost data (top 10 and Other)
gcrop <- ggplot(sdsum3b1, aes(x = EventType, y = CropDmgCost)) + geom_bar(stat = "identity", fill = "blue", las=3) + theme(axis.text.x = element_text(angle = 90, hjust = 1)) + xlab("Event Type") +  ylab("Cost") + ggtitle("Crop damage cost of the top 10 Event Types (w/ other event types grouped in Other)")

# plot the two graphs in a grid
library(gridBase)
library(gridExtra)

grid.arrange(gprop, gcrop, ncol = 1)
```

![plot of chunk unnamed-chunk-7](figure/unnamed-chunk-7-1.png) 

Table 2a: Top 10 Storm Events for Property Damage Costs

```r
grid.table(sdsum3a1)
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-8-1.png) 

Table 2b: Top 10 Storm Events for Crop Damage Costs

```r
grid.table(sdsum3b1)
```

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9-1.png) 


##END OF REPORT




```r
#library(knitr)
#setwd("C:/Users/jdhuffaker/Documents/Coursera JHU Data Science Courses/05 Reproducible Research/Week 3/")
#knit2pdf("PA2_NOAA_Storm_Analysis_Final.Rmd")

#library(rmarkdown)
#render("PA2_NOAA_Storm_Analysis_Final.Rmd") # you could also use "Untitled.md"
```
