# Week 4
Daniel Fuller  
2017-09-07  



## HKR 6130 Week 4  

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

1. Esliger, D., Rowlands, A V., Hurst, T L., Michael, C., MURRAY, P.,  ROGER R G. Validation of the GENEA Accelerometer. Medicine & Science in Sports & Exercise. 2011, 43(6): 1085-1093. [www.dx.doi.org/10.1249/MSS.0b013e31820513be](www.dx.doi.org/10.1249/MSS.0b013e31820513be) **Select LWW Journals.**

2. Schaeger, C A.,  Nigg, C R., Hill, J O., Brink, L., Browning, R. Establishing and Evaluating Wrist Cutpoints for the GENEActiv Accelerometer in Youth. Medicine & Science in Sports & Exercise. 2014, 46(4): 826–833. [www.dx.doi.org/10.1249/MSS.0000000000000150](www.dx.doi.org/10.1249/MSS.0000000000000150) **Select LWW Journals.**

## Task - Geneactiv Data - Assignment 5  

1. Down the files from GitHub.  
2. Import the 4 files into R.  
    i) Hint: Use `read.csv`
3. Append (stack) the four files together.
    i) Hint: Use `dplyr::bind_rows`
    ii) Hint: Make sure you have a variable that indicates where the data are from
4. Aggregate the data by second and calculate the gravity subtracted vector magnitude, and the standard deviation for each axis (x, y, z,). 
    i) Hint: The data are at 100Hz (100 per second)
    ii) Hint: The formula for the gravity subtracted vector magnitude is `sum(x^2, y^2, z^2)-9.81`
    iii) Hint: Use the `sd` function to calcualte the standard deviation
    iiii) Hint: Use `dplyr`, `group_by`, and `mutate`  
5. Use the cut points from Reading 1, recode the vector magnitude variable, and create a new variable called `activity`. 
6. Create a figure of the physical activity intensity by time using `ggplot2`

You can find the documentation about the Geneactiv data on there website [https://www.activinsights.com/resources-support/geneactiv/downloads-software/](https://www.activinsights.com/resources-support/geneactiv/downloads-software/).
    
## Output from my analysis

Below is the output from my analysis without the code. I've posted the top `head` of the dataframe or other relevant information so you can check your work and make sure you are on the right track. 

#### 2. Import the files 

There are 4 files. The import process will be the same for each. The csv file has 100 rows of information that we will want to remove. We can use `skip` in `read.csv`. There are also no headers and no variable names in the file. You will need to use `header` and `col.names`. Type `?read.csv` to get the documentation for `read.csv`. 

The first rows for *p1_right hip_038757_2017-08-29 10-55-45.csv* should look like this:   


    

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



Great. Our updated files for P1 look like this now.

Hip:  


```r
head(p1_hip)
```

```
##                      time x_axis  y_axis  z_axis lux button temp id
## 1 2017-08-29 09:30:00:500 0.0100 -0.0217 -1.0131 381      0 31.6 P1
## 2 2017-08-29 09:30:00:510 0.0139 -0.0177 -1.0131 398      0 31.6 P1
## 3 2017-08-29 09:30:00:520 0.0218 -0.0098 -1.0091 414      0 31.6 P1
## 4 2017-08-29 09:30:00:530 0.0377 -0.0376 -1.0131 414      0 31.6 P1
## 5 2017-08-29 09:30:00:540 0.0179 -0.0336 -1.0051 414      0 31.6 P1
## 6 2017-08-29 09:30:00:550 0.0060 -0.0297 -1.0051 398      0 31.6 P1
##   wear_loc
## 1      hip
## 2      hip
## 3      hip
## 4      hip
## 5      hip
## 6      hip
```

Wrist:  

```r
head(p1_wrist)
```

```
##                      time  x_axis  y_axis  z_axis lux button temp id
## 1 2017-08-29 09:30:00:500 -0.9659 -0.0579 -0.4145 138      0 32.7 P1
## 2 2017-08-29 09:30:00:510 -1.0550 -0.0740 -0.4829 138      0 32.7 P1
## 3 2017-08-29 09:30:00:520 -1.1157 -0.0821 -0.4749 156      0 32.7 P1
## 4 2017-08-29 09:30:00:530 -1.1116 -0.1303 -0.3623 156      0 32.7 P1
## 5 2017-08-29 09:30:00:540 -1.0752 -0.1625 -0.4588 138      0 32.7 P1
## 6 2017-08-29 09:30:00:550 -1.0550 -0.0981 -0.5633 156      0 32.7 P1
##   wear_loc
## 1    wrist
## 2    wrist
## 3    wrist
## 4    wrist
## 5    wrist
## 6    wrist
```

Now we can append (stack) the data together. 

```
## Warning in bind_rows_(x, .id): Unequal factor levels: coercing to character
```

```
## Warning in bind_rows_(x, .id): binding character and factor vector,
## coercing into character vector

## Warning in bind_rows_(x, .id): binding character and factor vector,
## coercing into character vector

## Warning in bind_rows_(x, .id): binding character and factor vector,
## coercing into character vector

## Warning in bind_rows_(x, .id): binding character and factor vector,
## coercing into character vector
```

The warning is telling us we are trying to combine character and factors. That's ok. The data was coerced and appended together.

Quick gut check. Our previous dataframes had p1_hip = 511500, p1_wrist = 467700, p2_hip = 525900, p2_wrist = 500400. If we add those together we should get 2005500 = 2005500. 

I did that using inline code in RMarkdown. Looks like this:  

```r
#Quick gut check. Our previous dataframes had p1_hip = `r nrow(p1_hip)`, p1_wrist = `r nrow(p1_wrist)`, p2_hip = #r nrow(p2_hip)`, p2_wrist = `r nrow(p2_wrist)`. If we add those together we should get `r #nrow(p1_hip)+nrow(p1_wrist)+nrow(p2_hip)+nrow(p2_wrist)` = `r nrow(accel_data)`.`
```


    i) Hint: Use `dplyr::bind_rows`
    ii) Hint: Make sure you have a variable that indicates where the data are from
4. Aggregate the data by second and calculate the gravity subtracted vector magnitude, and the standard deviation for each axis (x, y, z,). 
    i) Hint: The data are at 100Hz (100 per second)
    ii) Hint: The formula for the gravity subtracted vector magnitude is `sum(x^2, y^2, z^2)-9.81`
    iii) Hint: Use the `sd` function to calcualte the standard deviation
    iiii) Hint: Use `dplyr`, `group_by`, and `mutate`  
5. Use the cut points from Reading 1, recode the vector magnitude variable, and create a new variable called `activity`. 
6. Create a figure of the physical activity intensity by time using `ggplot2`
