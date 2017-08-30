# Week 2
Daniel Fuller  
August 30, 2017  



## Getting Started in R

The [R project for statistical computing](https://www.r-project.org/) is a free open source statistical programming language and project. Follow these steps to get started:

1. Download and install R from the Dalhousie CRAN (Comprehensive R Archive Network) Mirror <http://mirror.its.dal.ca/cran/>
2. Download and install the Open Source Rstudio Desktop License <https://www.rstudio.com/products/rstudio/download/>. RStudio is a free IDE (Integrated Development Environment) built for R. It makes your life easier! 

#### Watch a cheezy R Studio video below
<iframe src="https://player.vimeo.com/video/97166163?color=428bca&title=0&byline=0&portrait=0" width="640" height="360" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen></iframe>
<p><a href="https://vimeo.com/97166163">RStudio Overview - 1:30</a> from <a href="https://vimeo.com/rstudioinc">RStudio, Inc.</a> on <a href="https://vimeo.com">Vimeo</a>.</p>

## Open RStudio

You should see something that looks like in the video. 

* If yes, **great**
* If no, **Post a note to Slack**.

# 1. What is R? 

R was first written as a research project by Ross Ihaka and Robert Gentleman, and is now under active development by a group of statisticians called 'the R core team'. R is available free of charge and is distributed under the terms of the Free Software Foundation's GNU General Public License. You can download the program from the Comprehensive R Archive Network (CRAN). Ready-to-run 'binaries' are available for Windows, Mac OS X, and Linux. The source code is also available for download and can be compiled for other platforms. [German Rodriguez](http://data.princeton.edu/R/)

# 2. Why use R?

The main advantages of R are the fact that R is freeware and that there is a lot of help available online.

* Strengths  
    + free and open source, supported by a strong user community  
    + highly extensible and flexible  
    + implementation of modern statistical methods  
    + assists with replication and extension of your work
* Weaknesses  
    + slow with very large data sets  
    + non-standard programming paradigms  

# 3. Important Notes  

1. I am using the [Teach the tidyverse to beginners](http://varianceexplained.org/r/teach-tidyverse/) philosophy developed by David Robinson. That said, because I've been using R for a while I sometimes fall back on base R or other packages. 

2. There is almost always more than one way to do the same thing in R. The tidyverse will be our main approach because it's consisent and powerful.  

3. Coding style is important. Follow the [Hadley](http://adv-r.had.co.nz/Style.html) or [Goolge](https://google.github.io/styleguide/Rguide.xml) style guide. Leave comments so your team can pass the "hit by a bus test."

# 4. Let's get started

## First R Commands

### Calculator

You can use R as a basic calculator. If you pass the command 2+2 it will return 4. 

```r
2+2
```

```
## [1] 4
```

You can use arithmetic and mathematical operators in your commands  

Code  | Action
------------- | -------------
+  | Addition
-  | Subtraction
_*_  | Multiplication
/  | Division
^  | Raise to the power of

#### Assignment and Variables

You can use the "<-" symbol to assign a variable. This is not a variable in the social science/statistics sense. Think if it as storing something to be used again later. So if we want to store the number *4* and call it *a* we can. Then we ask R to return the value of *a* we get *4*.


```r
a <- 4
a
```

```
## [1] 4
```

With our newly stored *a* we can do calculations.

```r
a*20
```

```
## [1] 80
```




#### Functions

We can apply functions to the vector we just created. For example, if we wanted to compute the mean of the vector we could write

```r
(1+2+3+4)/4
```

```
## [1] 2.5
```

That is long and error prone. So we can call a function from Base R named `mean` to do the same thing.

```r
vector <- c(1,2,3,4)
mean(vector)
```

```
## [1] 2.5
```

Within the brackets you specify the `arguments`. Arguments give extra information to the function.

There a lots of pre-existing arguments in R. If you are thinking about it, someone probably already wrote a function for it. Here is a non-exaustive list of mathematical and statistical functions borrowed from [Wagner - BIO360](http://nicolascampione.weebly.com/uploads/1/9/4/1/19411255/r_cheat_sheet.pdf). 

Code  | Action
------------- | -------------
mean(x)  | Average
median(x) | Median
var(x)  | Variance
sd(x)  | Standard Deviation
log(x)   | Logarithm of x
exp(x)   | Exponential function ex
sin(x)   | Sin
cos(x)   | Cosine
tan(x)   | Tangent
min(x)   | Minimum value
max(x)   | Maximum value
sum(x)   | Sum of values
cumsum(x)   | Cummulative sum of values
length(x)   | Length of vector

### Packages/Libraries

R can do many statistical and data analyses. They are organized in so-called packages or libraries. With the standard installation, most common packages are installed. There are lots of packages. It's probable that if you have thought about the analysis, there is a package that is able to do it already. There are two basic steps to using a package: 

  * Installing the package  `install.packages("tidyverse")`
  * Loading the package     `library(tidyverse)`

You can ask for help about a package by typing `?tidyverse` or search the R website by typing `??tidyverse`.

### Data Frames

Data frames are probably what you are used to working with. This is the typical data format other statistical software, like SPSS and Stata use. In R, are a data frame can be comprised of many different data types (i.e., numeric, string, factor) so long as all variables are the same length. Data frames also typically have labelled headers giving you a short description of the variable. 

# 6. Working with real data

As our example we will be using the Canadian Community Health Survey (CCHS) 2011-2012 data extracted from CHASS at the University of Toronto. The Canadian Community Health Survey (CCHS) is a cross-sectional survey that collects information related to health status, health care utilization and health determinants for the Canadian population. It surveys a large sample of respondents and is designed to provide reliable estimates at the health region level.

## Set a working directory

**Mac**    *setwd("/Users/dfuller/Desktop/")*   
**PC**     *setwd("C:\\Users\\Andrie\\R\\win-library\\")*  
   
You can import pretty much any type of data into R with the `foreign` package. It allows you to import and work with data without owning the software. Once you do that you can also write the data to .csv or another reasonable data format. Some examples: 

Package | Code  | Action
------------- | ------------- | -------------
haven | read.sav  | SPSS data
foreign | read.dta | Stata data
read_excel | readxl | Excel data

You can read many file formats using the `haven` package. 

### Example of `read.csv`

```r
cchs <- read.csv("cchs.csv")
```

### Example of `read_sav`

```r
library(haven)

cchs_spss <- read_sav("cchs.sav")
```

### Example of `readxl`

```r
library(readxl)

cchs_xl <- read_excel("cchs.xlsx", col_types = c("numeric", "text", "text", "numeric", "numeric"))
```

Can also write data to many different formats SPSS, Stata, SAS, csv...

### Exploring the Data  

Now we have some data. The variables are as follows: 

* CASEID
    + Unique identifier for each participant
* verdate
    + Date the survey was complete
* geogprv
    + Province 
* hwtghtm
    + Height in Meters
* hwtgwtk
    + Weight in Kilograms

We can also use `head` to get a quick view of the data frame

```r
head(cchs)
```

```
##   CASEID  verdate geogprv hwtghtm hwtgwtk
## 1      1 20130913      35   1.575   65.25
## 2      2 20130913      59   1.905   99.00
## 3      3 20130913      35   1.803   77.40
## 4      4 20130913      46   1.727   85.50
## 5      5 20130913      24   1.803   81.00
## 6      6 20130913      48   1.727   78.75
```

We can also use `tibble` to get a quick view of the data frame

```r
library(tidyverse)
tbl_df(cchs)
```

```
## # A tibble: 124,929 x 5
##    CASEID  verdate geogprv hwtghtm hwtgwtk
##     <int>    <int>   <int>   <dbl>   <dbl>
##  1      1 20130913      35   1.575   65.25
##  2      2 20130913      59   1.905   99.00
##  3      3 20130913      35   1.803   77.40
##  4      4 20130913      46   1.727   85.50
##  5      5 20130913      24   1.803   81.00
##  6      6 20130913      48   1.727   78.75
##  7      7 20130913      12   1.626   62.10
##  8      8 20130913      48   9.999  999.99
##  9      9 20130913      35   1.905  105.75
## 10     10 20130913      59   1.854   85.50
## # ... with 124,919 more rows
```

We will mostly be working the **Hadley/Rstudio** way to do this. That said, one of the nice/horrific things about R is that there are many many ways to do the same thing.  

Some basic concepts: 

Unlike other stats programs R makes you call the data and variable together. So if I want the `weight` variable from the `cchs` data you can do that a few different ways:
  
  * `cchs$hwtghtm` - Call the data and variable by name using the $ symbol
  * `cchs %>% hwtghtm` - Call the data, a function using the pipe operator
  * Sometimes packages have a `data = ` to call the data and then you can call the varible name at the correct places. This is common in regression type packages.


#### Descriptive Statistics

Summarize the entire data frame using `summary`

```r
summary(cchs)
```

```
##      CASEID          verdate            geogprv         hwtghtm     
##  Min.   :     1   Min.   :20130913   Min.   :10.00   Min.   :1.270  
##  1st Qu.: 31233   1st Qu.:20130913   1st Qu.:24.00   1st Qu.:1.600  
##  Median : 62465   Median :20130913   Median :35.00   Median :1.676  
##  Mean   : 62465   Mean   :20130913   Mean   :36.25   Mean   :2.014  
##  3rd Qu.: 93697   3rd Qu.:20130913   3rd Qu.:47.00   3rd Qu.:1.778  
##  Max.   :124929   Max.   :20130913   Max.   :60.00   Max.   :9.999  
##     hwtgwtk       
##  Min.   :  30.60  
##  1st Qu.:  63.00  
##  Median :  74.25  
##  Mean   : 128.04  
##  3rd Qu.:  87.75  
##  Max.   : 999.99
```

Summarize a specific variable using `summary`

```r
summary(cchs$hwtghtm)
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   1.270   1.600   1.676   2.014   1.778   9.999
```

#### Graph the Data


```r
library(ggplot2)

scatter <- ggplot(cchs, aes(x = hwtghtm, y = hwtgwtk)) + 
  geom_point() + 
  xlab("Height in Meters") +
  ylab("Weight in Kilograms")
plot(scatter)
```

![](week2_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

Wooo what the heck is happening here? Looks like the data are coded in a specific way (SPSS style) for missing data. With 999.96 and 999.99 for weight and 9.996 and 9.999 for height. 

##### Missing Values
"The value NA is used to represent missing values for input in R. An NA can be assigned to a variable directly to create a missing value, but to test for missing values, the function is.na must be used.
R propogates missing values throughout computations, so often computations performed on data con- taining missing values will result in more missing values. Some of the basic statistical functions (like mean, min, max, etc.) have an argument called na.rm, which, if set to TRUE, will remove the NAs from your data before calculations are performed. In addition, the statistical modeling functions (like aov, glm, loess, etc.) provide an argument called na.action. This argument can be set to a function which will be called to operate on the data before it is processed. One very useful choice for the na.action argument is na.omit. This function (which can be called idependently of the statistical modelling functions) will remove all the rows of a data frame or matrix which contain any missing values." ([Spector - An introduction to R](http://www.stat.berkeley.edu/~spector/R.pdf))

##### Recoding data 

Recoding a values from a continous variable. We use dplyr and the pipe operator. The code does the follow:
    i) Tells R to write the new result to the CCHS data
    ii) Tells R to use the CCHS data for the function
    iii) Uses the `mutate` function to create a new variable called `height_m` and `ifelse` function to replace with 9.996 and 9.999 from `hwtghtm` with `NA`, the missing value specification for R. It also tells R to replace everything else with the existing values of `hwtghtm`.


```r
# Recoding the height data
library(dplyr)
cchs <- cchs %>%
  mutate(height = ifelse(hwtghtm == 9.996 | hwtghtm == 9.999, NA, hwtghtm))
```

Now we know we are going to need to clean the weight variable and we are going to want to calculate BMI, BMI category, and give some meaning names to the provinces. We can do that entire process in one chunk of code using the same logic we had before. 


```r
library(dplyr)
cchs <- cchs %>%
  mutate(weight = ifelse(hwtgwtk == 999.96 | hwtgwtk == 999.99, NA, hwtgwtk),
         bmi = weight/height^2)
```

The mutate method works particularly well when you are using numeric variables. I'm going to fall back on the `car` package recode function. I typically use this instead of `dplyr::recode`.


```r
library(car)

cchs$c_bmi <- car::recode(cchs$bmi, 
                      "lo:18.499='1. Underweight';
                      18.5:25='2. Normal';
                      25.00001:30='3. Overweight';
                      30.00001:hi='4. Obese';", as.factor.result=TRUE)

## Let's also recode geogprv so it makes more sense

cchs$province <- car::recode(cchs$geogprv, 
                      "10='NL';
                      11='PEI';
                      12='NS';
                      13='NB';
                      24='QC';
                      35='ON';
                      46='MN';
                      47='SK';
                      48='AB';
                      59='BC';
                      60='TR';
                      96:99=NA;", as.factor.result=TRUE)
```

Let's graph that data again

```r
library(ggplot2)

scatter <- ggplot(cchs, aes(x = height, y = weight)) + 
  geom_point(aes(colour = factor(c_bmi)), alpha = 1/5) + 
  theme(axis.line = element_line(colour = "black"),
        panel.background = element_blank(),
        axis.title=element_text(size=16),
        text=element_text(size=14)) + 
  xlab("Height in Meters") +
  ylab("Weight in Kilograms")
plot(scatter)
```

![](week2_files/figure-html/unnamed-chunk-17-1.png)<!-- -->

Here I added `aes(colour = factor(c_bmi))` to the `geom_point` command to get colours by BMI category. There are many ways to change the colour scheme but I'm not going into that at the moment. 

### Correlation Table

What if we wanted to look at the correlation between height and weight now that we have cleaned the data.

```r
library(PerformanceAnalytics) 
library(dplyr)

### Bit of jump here but we are going to subset columns
bmi_corr <- select(cchs, height, weight, bmi)

### Run the correlation chart on the new data frame
chart.Correlation(bmi_corr, histogram=TRUE, pch=19)
```

![](week2_files/figure-html/unnamed-chunk-18-1.png)<!-- -->

##### Subsetting

Image want to run a t-test with only 2 provinces, NL and BC. To simplify that task I can create a new data frame with only NL and BC. Then I can run the t-test relatively easily.


```r
library(dplyr)

bmi_NL_BC <- filter(cchs, province == c("NL", "BC"))

table(bmi_NL_BC$province)
```

```
## 
##   AB   BC   MN   NB   NL   NS   ON  PEI   QC   SK   TR 
##    0 7694    0    0 1747    0    0    0    0    0    0
```

##### T-Test

Now that we have created the new data frame we can run a t-test comparing the 2 provinces. Also nice to graph the data. 

```r
# Sort by group then ID
bmi_NL_BC <- arrange(bmi_NL_BC, province, CASEID)

# t-test
t.test(bmi ~ province, bmi_NL_BC)
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  bmi by province
## t = -12.234, df = 2234.5, p-value < 2.2e-16
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -2.088945 -1.511795
## sample estimates:
## mean in group BC mean in group NL 
##         25.40356         27.20393
```

Let's make a graph of the t-test data

```r
library(ggplot2)

histo_ttest <- ggplot(bmi_NL_BC, aes(bmi, colour = factor(province))) + 
  geom_density(alpha=.5) + 
  theme(axis.line = element_line(colour = "black"),
        panel.background = element_blank(),
        axis.title=element_text(size=16),
        text=element_text(size=14)) + 
  xlab("BMI") +
  ylab("Density")
plot(histo_ttest)
```

![](week2_files/figure-html/unnamed-chunk-21-1.png)<!-- -->

##### Collapsing by  

Let's say I want to get the mean and SD of BMI for each province. I can do that with `dplyr`.


```r
library(dplyr)

bmi_prov <- cchs %>%
  group_by(province) %>%
    summarise(mean_BMI = mean(bmi, na.rm=TRUE),
              sd_BMI = sd(bmi, na.rm=TRUE),
              total = n())
bmi_prov
```

```
## # A tibble: 11 x 4
##    province mean_BMI   sd_BMI total
##      <fctr>    <dbl>    <dbl> <int>
##  1       AB 26.30560 5.492599 11321
##  2       BC 25.45033 4.969990 15413
##  3       MN 26.86344 5.679759  6962
##  4       NB 27.00482 5.661688  4786
##  5       NL 27.10976 5.426855  3625
##  6       NS 26.78828 5.590665  4629
##  7       ON 26.07027 5.336154 42915
##  8      PEI 26.63559 5.319220  1774
##  9       QC 25.47341 5.030777 23260
## 10       SK 26.83456 5.477853  7161
## 11       TR 26.35760 5.588537  3083
```


##### Running a regression

```r
regressionModel <- lm(height ~ weight, data=cchs)
summary(regressionModel)
```

```
## 
## Call:
## lm(formula = height ~ weight, data = cchs)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -0.35906 -0.05776  0.00136  0.06048  0.33749 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 1.463e+00  1.106e-03  1322.9   <2e-16 ***
## weight      3.008e-03  1.444e-05   208.3   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.08615 on 116980 degrees of freedom
##   (7947 observations deleted due to missingness)
## Multiple R-squared:  0.2705,	Adjusted R-squared:  0.2705 
## F-statistic: 4.338e+04 on 1 and 116980 DF,  p-value: < 2.2e-16
```
Here we see an example of the `data =` call we talked about earlier. 

We use summary to get the summary information from the model. We can also use `anova` or `plot` to look at model fit. 
