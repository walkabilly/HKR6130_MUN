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

## Apple Watch Data

```r
aw_data <- read_csv("https://github.com/walkabilly/HKR6130_MUN/raw/master/data/apple_watch_data.csv")
```

The data have 8 columns. Here is a list and description: 

- **X1**: A sequence counting by 1 from 1 to 6137. 
- **p_id**: The unique identifier for the person in the data. We only have 1 person so this is just `participant1` 
- **device_name**: The name of the device. We only have 1 so it's just Apple Watch. 
- **date_time**: The data and time. These data are at the minute level so you have on measurement every minute. 
- **heart**: Average heart rate for the minute. Apple Watch series 2 only collects 1 minute of data every 5 minutes. If the value is not collected it is returned as a zero. Zero heart rate is impossible. 
- **calories**: Active calories burned that minute. This does not consider resting or basal metalic rate. 
- **steps**: Count of the number of steps taked during that minute. 
- **distance**: Estimate of the total distance travelled during that minute. 

# Tasks for Path_A

#### At each step of Path_A you will need to produce the result and describe in your own words what you think the function you are using is doing. It is a good idea to refer back to your Intro to R work. You have already done the majority of the things you will need to for this assignment in Intro to R. 

__Example__

* What does the function `read_csv` do? 
* You can learn more about a function using the `?function_name` command in your console. 
* Try `?read_csv` in your console. You should see a new window open in the bottom right of your RStudio window. 

### Tasks

#### 1. Read in the data

__Hint:__   

* Use `read_csv`  

__Subtask:__  

* What does the function `read_csv` do?  
* How many days of data do we have?  
* What would you expect to be reasonable values for heart rate and steps if we average heart rate and summed steps over the 4 day per day?  
  
#### 2. Replace the Zero's in the heart with _NA_ (missing)

A zero for heart rate is no plausible but zeros for active calories, steps, and distance per minute are plausible so we will keep those in. 

__Hint:__

* There is function called `na_if` in `tidyverse`. You can combine it with the pipe operator to turn all zeros from one column into NA.  

#### 3. Calculate the mean and standard deviation for heart rate & calculate the sum for calories, steps, and distance.

__Subtask:__  

* Do these values make sense? What should we expect if we have someone's average heart rate for around 4days?  

__Hint:__  

* You can use the `mean` function. Be careful and check the function because we not have _NA_ values and if you try and calculate the mean of nothing you get nothing. Check `?mean` if you are not getting what you expect. 

#### 4. Create a histogram of the calories column

__Subtask:__ 

* In your description describe in general how you understand ggplot works. 
* Describe with the function `ggplot` does and what the `geom` to create the histogram does. 

__Hint:__  

* Use ggplot. 

#### 5. Create a scatter plot of heart rate on the y axis and time on the x axis

__Subtask:__  

* What is the difference between the `geom` for creating scatter plots and the `geom` for creating historgrams. 
* Intepret the figure. Describe what is hapenning in 3 sentences. 

__Hint:__  

* Use ggplot. 

#### 6. Create a scatter plot of steps and heart rate on the y axis and time on the x axis on the same figure

__Subtask:__  

* What is the difference between the `geom` for creating scatter plots and the `geom` for creating historgrams. 

__Hint:__  

* Start with your previous plot and see if you can build on by adding another layer.
* You will need to find a way to change the colour of the points in the scatter plot because the default will have a bunch of black points that won't make sense. 

#### 7. Recode steps into zero, 1-99 per minute and 100+ per minute

__Subtask:__  

* Describe what the functions are doing

__Hint:__  

* Use the function `case_when` combined with `mutate` 
* Refer to your Intro to R material if you are lost

#### 8. Create a table with the categories of your new categorical steps data

__Subtask:__  

* As a sanity check, see the sum of the values from your table match with the total number of observations in your data

__Hint:__  

* There is function called `table` 

#### 9. Calculate the average heart rate and calories for each of the values of your steps_cat variable

__Subtask:__  

* Describe what the functions are doing in your own words
* Describe if the results make sense to you

__Hint:__  

* Use `group_by`, the pipe operator, and `summarize` 
* Don't forget the `na.rm` function

# Tasks for Path_B

# Coming soon

Complete the tasks for Path_A plus the following 


# Solutions for Path_A and Path_B can be found [here](https://github.com/walkabilly/HKR6130_MUN/blob/master/apple_watch_data_solutions.md)
