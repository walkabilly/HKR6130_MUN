---
title: "Accel Data Solutions"
author: "Daniel Fuller"
date: '2019-08-30'
output:
      html_document:
        keep_md: true
---



## HKR 6130 Accel Data

## Introduction

This week we will focus on using R to work with data from Geneactiv accelerometers. [Active Insights](https://www.activinsights.com/products/geneactiv/) is the company that makes Geneativ devices, which are research grade accelerometers. Henry and I went for a run this summer to collect some data for this portion of the course. The estimated times and intensities for the run times are.  

| Time Stamp | Activity |
|------------|----------|
| 10:10 | Walk |
| 10:12 | Jog |
| 10:18 | Fast Run |
| 10:20 | Walk |
| 10:22 | Jog |
| 10:24 | Fast Run |
| 10:26 | Walk |
| 10:28 | Stand |
| 10:30 | Walk |
| 10:32 | Sit |
| 10:34 | End |

There are 2 short readings then we will jump into analyzing data. For this assignment, I will provide the output PDF that will have all of the analysis, but I will not provide the code used to create the analysis. There will be nothing new with the code, we have seen a version of everything you will need to do in Week 2, and I will provide lots of hints.  

## Objectives

1. Analyze Geneactiv data using R. 
2. Create physical activity cut points based on the literature. 
3. Use real data. 

## Readings 

1. Esliger, D., Rowlands, A V., Hurst, T L., Michael, C., MURRAY, P.,  ROGER R G. Validation of the GENEA Accelerometer. Medicine & Science in Sports & Exercise. 2011, 43(6): 1085-1093. [http://www.dx.doi.org/10.1249/MSS.0b013e31820513be](http://www.dx.doi.org/10.1249/MSS.0b013e31820513be) **Select LWW Journals.**

2. Schaeger, C A.,  Nigg, C R., Hill, J O., Brink, L., Browning, R. Establishing and Evaluating Wrist Cutpoints for the GENEActiv Accelerometer in Youth. Medicine & Science in Sports & Exercise. 2014, 46(4): 826–833. [http://www.dx.doi.org/10.1249/MSS.0000000000000150](http://www.dx.doi.org/10.1249/MSS.0000000000000150) **Select LWW Journals.**

## Task - Geneactiv Data - Assignment 5  

1. Down the files from GitHub.  
2. Import the 4 files into R.  
    i) Hint: Use `read.csv`
3. Append (stack) the four files together.
    i) Hint: Use `dplyr::bind_rows`
    ii) Hint: Make sure you have a variable that indicates where the data are from
4. Aggregate the data by second and calculate the gravity subtracted vector magnitude, and the standard deviation for each axis (x, y, z,).   
    i) Hint: The data are at 100Hz (100 per second)  
    ii) Hint: The formula for the gravity subtracted vector magnitude is `sqrt(x^2, y^2, z^2)-1`  
    iii) Hint: Use the `sd` function to calcualte the standard deviation  
    iiii) Hint: Use `dplyr`, `group_by`, and `summarise`    
5. Use the cut points from Reading 1, recode the vector magnitude variable, and create a new variable called `activity`. 
6. Create a figure of the physical activity intensity by time using `ggplot2`

You can find the documentation about the Geneactiv data on there website [https://www.activinsights.com/resources-support/geneactiv/downloads-software/](https://www.activinsights.com/resources-support/geneactiv/downloads-software/).
    
## Output from my analysis

Below is the output from my analysis without the code. I've posted the top `head` of the dataframe or other relevant information so you can check your work and make sure you are on the right track. 

#### 2. Import the files 

There are 4 files. The import process will be the same for each. The csv file has 100 rows of information that we will want to remove. We can use `skip` in `read.csv`. There are also no headers and no variable names in the file. You will need to use `header` and `col.names`. Type `?read.csv` to get the documentation for `read.csv`. 

The first rows for *p1_right hip_038757_2017-08-29 10-55-45.csv* should look like this:   


```r
p1_hip <- read.csv("https://github.com/walkabilly/HKR6130_MUN/blob/master/data/p1_right%20hip.csv?raw=true", skip = 99, header = FALSE, col.names = c("time", "x_axis", "y_axis", "z_axis", "lux", "button", "temp"))
```

```
## Warning in readLines(file, skip): line 27 appears to contain an embedded nul
```

```
## Warning in readLines(file, skip): line 31 appears to contain an embedded nul
```

```
## Warning in readLines(file, skip): line 32 appears to contain an embedded nul
```

```
## Warning in readLines(file, skip): line 33 appears to contain an embedded nul
```

```
## Warning in readLines(file, skip): line 34 appears to contain an embedded nul
```

```
## Warning in readLines(file, skip): line 35 appears to contain an embedded nul
```
    

```r
head(p1_hip)
```

```
##                      time x_axis  y_axis  z_axis lux button temp
## 1 2017-08-29 09:30:00:500 0.0100 -0.0217 -1.0131 381      0 31.6
## 2 2017-08-29 09:30:00:510 0.0139 -0.0177 -1.0131 398      0 31.6
## 3 2017-08-29 09:30:00:520 0.0218 -0.0098 -1.0091 414      0 31.6
## 4 2017-08-29 09:30:00:530 0.0377 -0.0376 -1.0131 414      0 31.6
## 5 2017-08-29 09:30:00:540 0.0179 -0.0336 -1.0051 414      0 31.6
## 6 2017-08-29 09:30:00:550 0.0060 -0.0297 -1.0051 398      0 31.6
```

Now we repeat for each file. I'm not doing this with a list but we should. More on this later. 

The first rows for *p1_right wrist_038764_2017-08-29 10-50-05.csv* should look like this:   


```r
p1_wrist <- read.csv("https://github.com/walkabilly/HKR6130_MUN/blob/master/data/p1_right%20wrist.csv?raw=true", skip = 99, header = FALSE, col.names = c("time", "x_axis", "y_axis", "z_axis", "lux", "button", "temp"))
```

```
## Warning in readLines(file, skip): line 27 appears to contain an embedded nul
```

```
## Warning in readLines(file, skip): line 31 appears to contain an embedded nul
```

```
## Warning in readLines(file, skip): line 32 appears to contain an embedded nul
```

```
## Warning in readLines(file, skip): line 33 appears to contain an embedded nul
```

```
## Warning in readLines(file, skip): line 34 appears to contain an embedded nul
```

```
## Warning in readLines(file, skip): line 35 appears to contain an embedded nul
```
    

```r
head(p1_wrist)
```

```
##                      time  x_axis  y_axis  z_axis lux button temp
## 1 2017-08-29 09:30:00:500 -0.9659 -0.0579 -0.4145 138      0 32.7
## 2 2017-08-29 09:30:00:510 -1.0550 -0.0740 -0.4829 138      0 32.7
## 3 2017-08-29 09:30:00:520 -1.1157 -0.0821 -0.4749 156      0 32.7
## 4 2017-08-29 09:30:00:530 -1.1116 -0.1303 -0.3623 156      0 32.7
## 5 2017-08-29 09:30:00:540 -1.0752 -0.1625 -0.4588 138      0 32.7
## 6 2017-08-29 09:30:00:550 -1.0550 -0.0981 -0.5633 156      0 32.7
```

The first rows for *p2_right hip_038758_2017-08-29 10-57-53.csv* should look like this:   


```r
p2_hip <- read.csv("https://github.com/walkabilly/HKR6130_MUN/blob/master/data/p2_right%20hip.csv?raw=true", skip = 99, header = FALSE, col.names = c("time", "x_axis", "y_axis", "z_axis", "lux", "button", "temp"))
```

```
## Warning in readLines(file, skip): line 27 appears to contain an embedded nul
```

```
## Warning in readLines(file, skip): line 31 appears to contain an embedded nul
```

```
## Warning in readLines(file, skip): line 32 appears to contain an embedded nul
```

```
## Warning in readLines(file, skip): line 33 appears to contain an embedded nul
```

```
## Warning in readLines(file, skip): line 34 appears to contain an embedded nul
```

```
## Warning in readLines(file, skip): line 35 appears to contain an embedded nul
```
    

```r
head(p2_hip)
```

```
##                      time x_axis  y_axis  z_axis lux button temp
## 1 2017-08-29 09:30:00:500 0.0159  0.0069 -0.9974 202      0 30.3
## 2 2017-08-29 09:30:00:510 0.0079  0.0030 -1.0015 185      0 30.3
## 3 2017-08-29 09:30:00:520 0.0239  0.0030 -1.0015 202      0 30.3
## 4 2017-08-29 09:30:00:530 0.0079  0.0148 -0.9853 202      0 30.3
## 5 2017-08-29 09:30:00:540 0.0119 -0.0010 -0.9893 202      0 30.3
## 6 2017-08-29 09:30:00:550 0.0239 -0.0128 -0.9934 202      0 30.3
```

The first rows for *p2_right wrist_038756_2017-08-29 10-53-45.csv* should look like this:   


```r
p2_wrist <- read.csv("https://github.com/walkabilly/HKR6130_MUN/blob/master/data/p2_right%20wrist.csv?raw=true", skip = 99, header = FALSE, col.names = c("time", "x_axis", "y_axis", "z_axis", "lux", "button", "temp"))
```

```
## Warning in readLines(file, skip): line 27 appears to contain an embedded nul
```

```
## Warning in readLines(file, skip): line 31 appears to contain an embedded nul
```

```
## Warning in readLines(file, skip): line 32 appears to contain an embedded nul
```

```
## Warning in readLines(file, skip): line 33 appears to contain an embedded nul
```

```
## Warning in readLines(file, skip): line 34 appears to contain an embedded nul
```

```
## Warning in readLines(file, skip): line 35 appears to contain an embedded nul
```
    

```r
head(p2_wrist)
```

```
##                      time  x_axis  y_axis  z_axis lux button temp
## 1 2017-08-29 09:30:00:500 -0.6057 -0.6495 -0.4931  49      0 32.7
## 2 2017-08-29 09:30:00:510 -0.6097 -0.6375 -0.4931  49      0 32.7
## 3 2017-08-29 09:30:00:520 -0.6057 -0.6334 -0.4972  49      0 32.7
## 4 2017-08-29 09:30:00:530 -0.6176 -0.6294 -0.5094  49      0 32.7
## 5 2017-08-29 09:30:00:540 -0.6097 -0.6495 -0.5094  49      0 32.7
## 6 2017-08-29 09:30:00:550 -0.6057 -0.6415 -0.5094  49      0 32.7
```

#### 3. Append (stack) the four files together

Before we stack the data we want to make sure we know that there is a variable that indicates the participant and the wear location. That information is not currently in the dataframe. To do this we can use `dplyr::mutate`. We will create 2 new variables. One called `id` to indicate the participant and one called `wear_loc` to indicate the wear location. 


```r
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
```

```
## ✓ ggplot2 3.3.3     ✓ purrr   0.3.4
## ✓ tibble  3.1.2     ✓ dplyr   1.0.6
## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
## ✓ readr   1.4.0     ✓ forcats 0.5.1
```

```
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
p1_hip <- p1_hip %>% 
            mutate(id = "P1",
                   wear_loc = "hip")
p1_wrist <- p1_wrist %>% 
            mutate(id = "P1",
                   wear_loc = "wrist")
p2_hip <- p2_hip %>% 
            mutate(id = "P2",
                   wear_loc = "hip")
p2_wrist <- p2_wrist %>% 
            mutate(id = "P2",
                   wear_loc = "wrist")
```

Great. Our updated files for P1 look like this now.

Hip:  


```r
head(p1_hip)
```

```
##                      time x_axis  y_axis  z_axis lux button temp id wear_loc
## 1 2017-08-29 09:30:00:500 0.0100 -0.0217 -1.0131 381      0 31.6 P1      hip
## 2 2017-08-29 09:30:00:510 0.0139 -0.0177 -1.0131 398      0 31.6 P1      hip
## 3 2017-08-29 09:30:00:520 0.0218 -0.0098 -1.0091 414      0 31.6 P1      hip
## 4 2017-08-29 09:30:00:530 0.0377 -0.0376 -1.0131 414      0 31.6 P1      hip
## 5 2017-08-29 09:30:00:540 0.0179 -0.0336 -1.0051 414      0 31.6 P1      hip
## 6 2017-08-29 09:30:00:550 0.0060 -0.0297 -1.0051 398      0 31.6 P1      hip
```

Wrist:  

```r
head(p1_wrist)
```

```
##                      time  x_axis  y_axis  z_axis lux button temp id wear_loc
## 1 2017-08-29 09:30:00:500 -0.9659 -0.0579 -0.4145 138      0 32.7 P1    wrist
## 2 2017-08-29 09:30:00:510 -1.0550 -0.0740 -0.4829 138      0 32.7 P1    wrist
## 3 2017-08-29 09:30:00:520 -1.1157 -0.0821 -0.4749 156      0 32.7 P1    wrist
## 4 2017-08-29 09:30:00:530 -1.1116 -0.1303 -0.3623 156      0 32.7 P1    wrist
## 5 2017-08-29 09:30:00:540 -1.0752 -0.1625 -0.4588 138      0 32.7 P1    wrist
## 6 2017-08-29 09:30:00:550 -1.0550 -0.0981 -0.5633 156      0 32.7 P1    wrist
```

Now we can append (stack) the data together. 

```r
accel_data <- bind_rows(p1_hip, p1_wrist, p2_hip, p2_wrist)
```

The warning is telling us we are trying to combine character and factors. That's ok. The data was coerced and appended together.


```r
head(accel_data)
```

```
##                      time x_axis  y_axis  z_axis lux button temp id wear_loc
## 1 2017-08-29 09:30:00:500 0.0100 -0.0217 -1.0131 381      0 31.6 P1      hip
## 2 2017-08-29 09:30:00:510 0.0139 -0.0177 -1.0131 398      0 31.6 P1      hip
## 3 2017-08-29 09:30:00:520 0.0218 -0.0098 -1.0091 414      0 31.6 P1      hip
## 4 2017-08-29 09:30:00:530 0.0377 -0.0376 -1.0131 414      0 31.6 P1      hip
## 5 2017-08-29 09:30:00:540 0.0179 -0.0336 -1.0051 414      0 31.6 P1      hip
## 6 2017-08-29 09:30:00:550 0.0060 -0.0297 -1.0051 398      0 31.6 P1      hip
```

Quick gut check. Our previous dataframes had p1_hip = 511500, p1_wrist = 467700, p2_hip = 525900, p2_wrist = 500400. If we add those together we should get 2005500 = 2005500. Excellent.

I did that using inline code in RMarkdown. Looks like this:  

*Quick gut check. Our previous dataframes had p1_hip = 511500, p1_wrist = 467700, p2_hip # = r nrow(p2_hip)`, p2_wrist = 500400. If we add those together we should get  = 2005500.*

#### 4. Aggregate the data by second and calculate the gravity subtracted vector magnitude, and the standard deviation for each axis (x, y, z,). 

Now we need to work a bit with time data. Time data can be scary. Use the package `luridate` to help. The `time` variable is currently a factor. We don't want that. Ok, this part is hard. I'm including the code below. Take a look at the lubridate documentation and make sure you understand what is happening. 

Geneactiv makes a list that is 100milliseconds long then uses that for aggregatation. The problem with that is the aggregation happens at the 0.5 second mark rather than the one second mark. This makes the data challenging to combine with GPS data, which we will be going later. I'm going to show you how to write this two ways. First, a way to replicated the Geneactive data. Second, a way to aggregate by the second.

**Geneactiv Method**  

```r
leng <- nrow(accel_data)/100
accel_data$milli <- rep(seq(1:leng), each=100)
```

**Proper Method**  

```r
library(lubridate)
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following objects are masked from 'package:base':
## 
##     date, intersect, setdiff, union
```

```r
library(stringr)

accel_data$time1 <- str_sub(accel_data$time, 1, str_length(accel_data$time)-4)  ## Remove the milliseconds
accel_data$time2 <- ymd_hms(accel_data$time1)  ## Create a POSIXct formated variable
accel_data$hour <- hour(accel_data$time2)  ## Create an hour variable
accel_data$minute <- minute(accel_data$time2)  ## Create a minute variable
accel_data$second <- second(accel_data$time2)  ## Create a second variable
```

Here we need to create the vector magnitude variables. You are going to create 2 vector magnitude variables. The `x_axis`, `y_axis`, `z_axis` variables are measured in **g**. So we will create two new variables:    
* Vector Magnitude: `sqrt(accel_data$x_axis^2+accel_data$y_axis^2+accel_data$z_axis^2)`
* Gravity subtracted vector magnitude: `sqrt(accel_data$x_axis^2+accel_data$y_axis^2+accel_data$z_axis^2)-1`  


```r
accel_data$vec_mag = sqrt(accel_data$x_axis^2+accel_data$y_axis^2+accel_data$z_axis^2)
accel_data$vec_mag_g = sqrt(accel_data$x_axis^2+accel_data$y_axis^2+accel_data$z_axis^2)-1
```

No we will aggregate and create the new variables at the same time. We will use `dplyr`, `group_by`, and `summarise`, just like we did in week 2, except in week 2 used `mutate` rather than `summarise`. We will need to create the summarized variables. Make sure that you include the `abs()` function in your summary in the gravity substracted vector magnitude variable. 

Note that I'm using `glimpse` instead of `head` because there are too many variables and I want a quick view of them all. 

**Geneactiv Method**  

```r
accel_sec_genea <- accel_data %>%
  group_by(milli) %>%
  summarise(
         time = first(time2), 
         id = first(id),
         wear_loc = first(wear_loc),
         m_x_axis = mean(x_axis),
         m_y_axis = mean(y_axis),
         m_z_axis = mean(z_axis),
         vec_mag = mean(vec_mag),
         vec_mag_g = mean(abs(vec_mag_g)),
         sd_x_axis = sd(x_axis),
         sd_y_axis = sd(y_axis),
         sd_z_axis = sd(z_axis) 
        )
```


```r
glimpse(accel_sec_genea)
```

```
## Rows: 20,055
## Columns: 12
## $ milli     <int> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 1…
## $ time      <dttm> 2017-08-29 09:30:00, 2017-08-29 09:30:01, 2017-08-29 09:30:…
## $ id        <chr> "P1", "P1", "P1", "P1", "P1", "P1", "P1", "P1", "P1", "P1", …
## $ wear_loc  <chr> "hip", "hip", "hip", "hip", "hip", "hip", "hip", "hip", "hip…
## $ m_x_axis  <dbl> 0.022196, 0.020494, 0.021442, 0.020218, 0.021446, 0.020854, …
## $ m_y_axis  <dbl> -0.034350, -0.034621, -0.034263, -0.034900, -0.033832, -0.03…
## $ m_z_axis  <dbl> -1.00474, -1.00406, -1.00434, -1.00410, -1.00418, -1.00538, …
## $ vec_mag   <dbl> 1.005635, 1.004927, 1.005210, 1.004977, 1.005044, 1.006297, …
## $ vec_mag_g <dbl> 0.007686551, 0.007443477, 0.007521750, 0.007466425, 0.008552…
## $ sd_x_axis <dbl> 0.008121028, 0.007577729, 0.007769432, 0.008493258, 0.008106…
## $ sd_y_axis <dbl> 0.007909239, 0.008273339, 0.007395653, 0.007960661, 0.008165…
## $ sd_z_axis <dbl> 0.007327160, 0.007598405, 0.007427930, 0.007507572, 0.008896…
```

**Proper Method**  

```r
accel_sec <- accel_data %>%
  group_by(id, wear_loc, hour, minute, second) %>%
  summarise(
         time = first(time2), 
         m_x_axis = mean(x_axis),
         m_y_axis = mean(y_axis),
         m_z_axis = mean(z_axis),
         vec_mag = mean(vec_mag),
         vec_mag_g = mean(abs(vec_mag_g)),
         sd_x_axis = sd(x_axis),
         sd_y_axis = sd(y_axis),
         sd_z_axis = sd(z_axis) 
        )
```

```
## `summarise()` has grouped output by 'id', 'wear_loc', 'hour', 'minute'. You can override using the `.groups` argument.
```


```r
glimpse(accel_sec)
```

```
## Rows: 20,059
## Columns: 14
## Groups: id, wear_loc, hour, minute [336]
## $ id        <chr> "P1", "P1", "P1", "P1", "P1", "P1", "P1", "P1", "P1", "P1", …
## $ wear_loc  <chr> "hip", "hip", "hip", "hip", "hip", "hip", "hip", "hip", "hip…
## $ hour      <int> 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, 9, …
## $ minute    <int> 30, 30, 30, 30, 30, 30, 30, 30, 30, 30, 30, 30, 30, 30, 30, …
## $ second    <dbl> 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17…
## $ time      <dttm> 2017-08-29 09:30:00, 2017-08-29 09:30:01, 2017-08-29 09:30:…
## $ m_x_axis  <dbl> 0.022946, 0.020813, 0.021126, 0.021446, 0.020292, 0.020579, …
## $ m_y_axis  <dbl> -0.033392, -0.035144, -0.033906, -0.035097, -0.034545, -0.03…
## $ m_z_axis  <dbl> -1.00534, -1.00374, -1.00470, -1.00434, -1.00434, -1.00402, …
## $ vec_mag   <dbl> 1.006224, 1.004632, 1.005551, 1.005244, 1.005205, 1.004869, …
## $ vec_mag_g <dbl> 0.008264282, 0.007040167, 0.007869534, 0.007576557, 0.007454…
## $ sd_x_axis <dbl> 0.007838581, 0.008230734, 0.007622039, 0.007844749, 0.008415…
## $ sd_y_axis <dbl> 0.008912885, 0.007604163, 0.007654124, 0.008075120, 0.007876…
## $ sd_z_axis <dbl> 0.007768487, 0.007128716, 0.007531751, 0.007621236, 0.007340…
```


```r
summary(accel_sec)
```

```
##       id              wear_loc              hour            minute    
##  Length:20059       Length:20059       Min.   : 9.000   Min.   : 0.0  
##  Class :character   Class :character   1st Qu.: 9.000   1st Qu.:20.0  
##  Mode  :character   Mode  :character   Median :10.000   Median :35.0  
##                                        Mean   : 9.641   Mean   :32.9  
##                                        3rd Qu.:10.000   3rd Qu.:46.0  
##                                        Max.   :10.000   Max.   :59.0  
##      second           time                        m_x_axis       
##  Min.   : 0.00   Min.   :2017-08-29 09:30:00   Min.   :-1.42253  
##  1st Qu.:14.00   1st Qu.:2017-08-29 09:50:53   1st Qu.:-0.79999  
##  Median :29.00   Median :2017-08-29 10:11:47   Median :-0.29377  
##  Mean   :29.44   Mean   :2017-08-29 10:11:51   Mean   :-0.26634  
##  3rd Qu.:44.00   3rd Qu.:2017-08-29 10:32:40   3rd Qu.: 0.01962  
##  Max.   :59.00   Max.   :2017-08-29 10:57:39   Max.   : 1.13246  
##     m_y_axis            m_z_axis           vec_mag         vec_mag_g       
##  Min.   :-1.015853   Min.   :-1.06611   Min.   :0.7671   Min.   :0.005100  
##  1st Qu.:-0.046400   1st Qu.:-0.99265   1st Qu.:1.0055   1st Qu.:0.009222  
##  Median : 0.008874   Median :-0.33960   Median :1.0171   Median :0.073481  
##  Mean   : 0.180734   Mean   :-0.39050   Mean   :1.1090   Mean   :0.199987  
##  3rd Qu.: 0.392452   3rd Qu.:-0.04864   3rd Qu.:1.1017   3rd Qu.:0.278038  
##  Max.   : 1.507083   Max.   : 1.49243   Max.   :2.8017   Max.   :1.845207  
##    sd_x_axis          sd_y_axis          sd_z_axis       
##  Min.   :0.003315   Min.   :0.003479   Min.   :0.005634  
##  1st Qu.:0.007880   1st Qu.:0.007260   1st Qu.:0.008966  
##  Median :0.101758   Median :0.084678   Median :0.100044  
##  Mean   :0.243840   Mean   :0.185981   Mean   :0.163476  
##  3rd Qu.:0.303751   3rd Qu.:0.249812   3rd Qu.:0.212789  
##  Max.   :2.157539   Max.   :1.855834   Max.   :1.968616
```

#### 5. Use the cut points from Reading 2, recode the vector magnitude variable, and create a new variable called `activity`. 

The cut points in Table 3 from the paper are as follows: 

| Activity | Cut Point (vec_mag_g) | 
|----------|-----------|
| Sedentary | <= 0.19000 |
| Light | >0.19001 to <0.31400 |
| Moderate | >=0.314001 to <0.9989 | 
| Vigorous | >0.999 to max |

We will use the `car::recode` function here. If you want you can try with `dplyr.` We will be using the "accel_sec" data. 


```r
accel_sec <- accel_sec %>%
	mutate(activity = case_when(
		vec_mag_g <= 0.1900 ~ "1.Sedentary",
		vec_mag_g > 0.19001 & vec_mag_g <= 0.314 ~ "2.Light",
		vec_mag_g >  0.314001 & vec_mag_g <= 0.99989 ~ "3.Moderate",
		vec_mag_g > 0.999 ~ "4.Vigorous",
		TRUE ~ "other"
	))
```


```r
table(accel_sec$activity)
```

```
## 
## 1.Sedentary     2.Light  3.Moderate  4.Vigorous 
##       11944        4145        3520         450
```

#### 6. Create a figure of the physical activity (`vec_mag_g`) intensity by time using `ggplot2`


```r
fig1 <- ggplot(accel_sec, aes(x = time, y = vec_mag_g)) + 
          geom_point(alpha = 1/10) + 
            xlab("Time") +
            ylab("Vector Magnitude")
plot(fig1)
```

![](accel_data_solutions_files/figure-html/unnamed-chunk-24-1.png)<!-- -->

Now we will create a figure that colours the Vector Magnitude by our activity cut point classification.


```r
fig2 <- ggplot(accel_sec, aes(x = time, y = vec_mag_g)) + 
          geom_point(alpha = 1/5, aes(colour = factor(activity))) + 
            xlab("Time") +
            ylab("Vector Magnitude Minus G") +
          theme_classic()
plot(fig2)
```

![](accel_data_solutions_files/figure-html/unnamed-chunk-25-1.png)<!-- -->

## You just processed 2 Million accelerometer records! Well done. 
