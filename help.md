---
title: "HELP"
author: "Daniel Fuller"
date: "August 30, 2017"
output:
      html_document:
        keep_md: true
---



# Help Page for HKR 6130

> There is no geek gene

You will get errors when you code. Everyone does, all the time. It's just part of the work. The first week will probably be the toughest for errors because you are setting your environment and there are lots of different strange things that can go wrong. This is normal. You are doing great. Below I've compiled all of the questions I have received so far are provided some ressources for helping answer questions. 

__When you ask a question it helps if you provide a the follow:__

1. The code you are trying to run
2. The error message you get

Later in the course it will help if you can produce a minimal reproducible example. You can see guidelines for how to do that here [https://stackoverflow.com/help/minimal-reproducible-example](https://stackoverflow.com/help/minimal-reproducible-example). 

## Google it

I strongly encourage you to copy and paste your error directly into Google. This is a good learning opporunity for you. It is nearly 100% certain that someone else has had the same error. If you end up on StackOverflow you are probably in the right place. More many errors a quick google will help you quickly solve the problem. 

# Questions to Date

__Question__  
```
I get an error when I try reading something with your file path. For example, cchs <- read.csv("/Users/dfuller/Dropbox/Teaching/MUN/HKR 6130/2017/HKR6130_MUN/data/cchs.csv"). There have a been a few questions about this.
```

__Answer__  
You need to assign the file path on your computer. The file path on my computer "/User/dfuller/Desktop/" won’t work on your computer. 

Here is quick tutorial on file paths: [https://www.youtube.com/watch?v=hUW5MEKDtMM](https://www.youtube.com/watch?v=hUW5MEKDtMM)

You need to assign a place where you want to save your work for this course. It could be in desktop, documents or somewhere else. 

Here is a link for how to get the file path for a folder [http://tips4pc.com/windows-xp/how_to_find_a_path_to_a_file_or.htm](http://tips4pc.com/windows-xp/how_to_find_a_path_to_a_file_or.htm)

You can replace the location on your computer where you want to save your stuff for this course. 

-----

__Question__  
```
I am just starting to work through some of the R stuff for week 2. I am doing the "lets get started" section. I am having trouble with the packages we will need to for this tutorial:
1. tidyverse
2. ggplot2
3. car
4. haven
5. read_excel

I have tried the install.packages() but am getting an error message. Is this what I am sposed to be doing?
```

__Answer__   
Try the following: 

```
install.packages(“tidyverse”)
install.packages(“ggplot2”)
install.packages(“car”)
install.packages(“haven”)
install.packages(“read_excel”)

library(tidyverse)
```
and the same thing for the rest of the packages. 

You need to input each one of the packages individually (for now). 

The first line of the code is a function that does something. In this case install.packages() installs packages. You need to put something in the bracket that the will be applied to the function. In this case the name of the package. 

If it is working you should see some text in your console saying the packages are being download. 

-----

__Question__   
```
I'm trying to knit my .Rmd file to PDF but I'm getting an error that Pandoc is not working or LATEX (pronounced LATEK) is not installed. 
```

__Answer__

1. Try kniting to .html fist. Does that work?

2. Install MacTex for Mac [https://www.tug.org/mactex/mactex-download.html](https://www.tug.org/mactex/mactex-download.html) or [https://miktex.org/howto/install-miktex](https://miktex.org/howto/install-miktex) for PC. 
3. Restart RStudio and see if it works. 

-----

__Question__   
```
I got my RMarkdown file set up. And the simple math code worked no problem when I knit it. But when I got down to the install packages I get errors. 
```

__Answer__

You cannot have `install.packages()` in a code chunk.  If you have something htat looks like this...

```{}
install.packages()
```

you will get the following error.

```
Error in contrib.url(repos, "source") : trying to use CRAN without setting a mirror calls: ... withVisible -> eval -> eval -> install.packages -> contrib.url Execution halted
```

You only need to `install.packages` once. I recommend doing it in the console. Once you have done that you can use `library()` to load your packges at the top of the RMardown file. 

----

## Online Ressources

If you are completely lost or stuck with your data wrangling or analysis there are a number of great ressources you can access. I would suggest you read these first rather than do a general google search. 

### Data Wrangling

1. Dr. Jenny Bryan's Stat 545 course is available on GitHub [http://stat545.com/topics.html](http://stat545.com/topics.html). Fantastic ressource.

2. RStudio has a number of cheat sheets if you can't quite remember the exact bit of code you need. I have these up all the time.  
    i) [Data Import](https://github.com/rstudio/cheatsheets/raw/master/source/pdfs/data-import-cheatsheet.pdf)
    ii) [Data Transformation](https://github.com/rstudio/cheatsheets/raw/master/source/pdfs/data-transformation-cheatsheet.pdf)
    iii) [RMarkdown](https://www.rstudio.com/wp-content/uploads/2016/03/rmarkdown-cheatsheet-2.0.pdf)
    iiii) [ggplot2](https://www.rstudio.com/wp-content/uploads/2016/11/ggplot2-cheatsheet-2.1.pdf)
    iiiii) Many more [here](https://www.rstudio.com/resources/cheatsheets/)
    
### Data Analysis

1. I use the [ULCA Institute for Digital Research and Education](https://stats.idre.ucla.edu/) a lot when I'm running analyses, often I need to do a non trivial analysis in a new software (I'm looking at you SAS) but don't know the code very well. This helps get oriented quickly. [Data Analysis Examples](https://stats.idre.ucla.edu/other/dae/).

2. Garrett Grolemund and Hadley Wickham's [R for Data Science](http://r4ds.had.co.nz/) provides a nice overview of modelling in [section 4: Model](http://r4ds.had.co.nz/model-basics.html).

### Stackoverflow

1. If you are really stuck, have posted your question to Slack, I can't figure it out, you read a bunch of tutorials, and still can't figure it out, it's time to turn to your friends on [Stackoverflow](https://stackoverflow.com/). Stackoverflow is an online community for asking questions and getting help about code. Follow these rules:  
    i) Search to see your question has been asked
    ii) [Ask a good question](https://stackoverflow.com/help/how-to-ask)  
    iii) Create a [reproducible example](https://stackoverflow.com/help/mcve)   
