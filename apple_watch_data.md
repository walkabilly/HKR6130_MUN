---
title: "Apple Watch Data"
author: "Daniel Fuller"
date: "18/09/2019"
output:
      html_document:
        keep_md: true
---



# Assignment Description

There are 2 paths for this assignment. If you felt less comfortable with the `Intro to R` assignment I recomment `Path_A` if you are more comfortable I recommend `Path_B`. First let's load the packages we need and look at the data. 

### Loading requires packages


```r
library(tidyverse)
```

```
## Registered S3 methods overwritten by 'ggplot2':
##   method         from 
##   [.quosures     rlang
##   c.quosures     rlang
##   print.quosures rlang
```

```
## ── Attaching packages ──────────────────────────────────────────────────────────────────────────────────────────────── tidyverse 1.2.1 ──
```

```
## ✔ ggplot2 3.1.1     ✔ purrr   0.3.2
## ✔ tibble  2.1.1     ✔ dplyr   0.8.3
## ✔ tidyr   0.8.3     ✔ stringr 1.4.0
## ✔ readr   1.3.1     ✔ forcats 0.4.0
```

```
## ── Conflicts ─────────────────────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

## Apple Watch Data

```r
aw_data <- read_csv("https://github.com/walkabilly/HKR6130_MUN/raw/master/data/apple_watch_data.csv")
```

```
## Warning: Missing column names filled in: 'X1' [1]
```

```
## Parsed with column specification:
## cols(
##   X1 = col_double(),
##   p_id = col_character(),
##   device_name = col_character(),
##   date_time = col_datetime(format = ""),
##   heart = col_double(),
##   calories = col_double(),
##   steps = col_double(),
##   distance = col_double()
## )
```

```r
summary(aw_data)
```

```
##        X1           p_id           device_name       
##  Min.   :   1   Length:6137        Length:6137       
##  1st Qu.:1535   Class :character   Class :character  
##  Median :3069   Mode  :character   Mode  :character  
##  Mean   :3069                                        
##  3rd Qu.:4603                                        
##  Max.   :6137                                        
##    date_time                       heart           calories      
##  Min.   :2018-07-28 02:30:00   Min.   :  0.00   Min.   :0.00000  
##  1st Qu.:2018-07-31 00:57:00   1st Qu.:  0.00   1st Qu.:0.00003  
##  Median :2018-08-01 02:31:00   Median :  0.00   Median :0.14612  
##  Mean   :2018-07-31 21:55:25   Mean   : 13.57   Mean   :0.45021  
##  3rd Qu.:2018-08-02 04:05:00   3rd Qu.:  0.00   3rd Qu.:0.45252  
##  Max.   :2018-08-03 05:39:00   Max.   :124.00   Max.   :6.24054  
##      steps            distance     
##  Min.   : 0.0000   Min.   : 0.000  
##  1st Qu.: 0.0000   1st Qu.: 0.000  
##  Median : 0.4258   Median : 0.309  
##  Mean   :10.1795   Mean   : 8.195  
##  3rd Qu.:13.6088   3rd Qu.:10.422  
##  Max.   :99.5206   Max.   :86.714
```

The data have 8 columns. Here is a list and description: 

- **X1**: A sequence counting by 1 from 1 to 6137. 
- **p_id**: The unique identifier for the person in the data. We only have 1 person so this is just `participant1` 
- **device_name**: The name of the device. We only have 1 so it's just Apple Watch. 
- **date_time**: The data and time. These data are at the minute level so you have on measurement every minute. 
- **heart**: Average heart rate for the minute 
- **calories**: Active calories burned that minute. This does not consider resting or basal metalic rate. 
- **steps**: Count of the number of steps taked during that minute. 
- **distance**: Estimate of the total distance travelled during that minute. 

