---
output: html_document
editor_options: 
  chunk_output_type: console
---
# Lecture 2

## RMarkdown

Now that we know some basic R syntax, let's learn a little about RMarkdown. You will drive yourself crazy (and fail to generate a reproducible workflow!) running code directly in the console. RMarkdown is really key for collaborative research, so we're going to get started with it early and then use it for the rest of the course. 

An RMarkdown file will allow us to weave markdown text with chunks of R code to be evaluated and output content like tables and plots.

File -> New File -> RMarkdown... -> Document of output format HTML, OK.

<div style="width:300px">
![](images/rstudio_new-rmd-doc-html.png)
</div>

You can give it a Title like "My Project". Then click OK. 

OK, first off: by opening a file, we are seeing the 4th pane of the RStudio console, which is essentially a text editor. This lets us organize our files within RStudio instead of having a bunch of different windows open.

Let's have a look at this file — it's not blank; there is some initial text is already provided for you. Notice a few things about it: 

- There are white and grey sections. R code is in grey sections, and other text is in white. 

![](images/rmarkdown.png)

<br>

Let's go ahead and "Knit HTML" by clicking the blue yarn at the top of the RMarkdown file. When you first click this button, RStudio will prompt you to save this file. Create a new folder for it somewhere that you will be able to find again (such as your Desktop or Documents), and name that folder something you'll remember (like `training_files`).

<br>

![](images/rmarkdown_side_by_side.png)

What do you notice between the two? 

Notice how the grey **R code chunks** are surrounded by 3 backticks and `{r LABEL}`. These are evaluated and return the output in form of an html file, that you could host on a website for example or print into a pdf or word document if you like. 

Here, R shows you two examples related to two of its example datasets that come with the R language, `cars` and `pressure`. 
The output for `summary(cars)` shows the descriptive statistics for the two variables in the car dataset, `speed` and `distance`. The figure below generates a scatterplot from the `pressure` dataset, showing the vapor pressure of mercury (in millimeters) as a function of temperature in degrees Celsius.The output plot shows the relationship between the two variables. 

Notice how the code `plot(pressure)` is not shown in the HTML output because of the R code chunk option `echo=FALSE` written at the beginning of the code chunk like this `{r, echo=FALSE}`.

More details...

This RMarkdown file has 2 different languages within it: **R** and **Markdown**. 

We don't know that much R yet, but you can see that we are taking a summary of some data called 'cars', and then plotting. There's a lot more to learn about R, and we'll get into it for the next few days. 

The second language is Markdown. This is a formatting language for plain text, and there are only about 15 rules to know. 

Notice the syntax for:

- **headers** get rendered at multiple levels: `#`, `##`
- **bold**: `**word**`

There are some good [cheatsheets](https://github.com/adam-p/markdown-here/wiki/Markdown-Here-Cheatsheet) to get you started, and here is one built into RStudio: Go to Help > Markdown Quick Reference
<br />
<br />

**Important**: note that the hashtag `#` is used differently in Markdown and in R: 

- in R, a hashtag indicates a comment that will not be evaluated. You can use as many as you want: `#` is equivalent to `######`. It's just a matter of style.
- in Markdown, a hashtag indicates a level of a header. And the number you use matters: `#` is a "level one header", meaning the biggest font and the top of the hierarchy. `###` is a level three header, and will show up nested below the `#` and `##` headers.

Learn more: http://rmarkdown.rstudio.com/

### Your Turn

1. In Markdown, Write some italic text, and make a numbered list. And add a few subheaders.
Use the Markdown Quick Reference (in the menu bar: Help > Markdown Quick Reference). 
1. Reknit your html file. 


### Code chunks

OK. Now let's practice with some of those commands.

Create a new chunk in your RMarkdown first in one of these ways: 

- click "Insert > R" at the top of the editor pane
- type by hand 
   \```{r}
   \```
- if you haven't deleted a chunk that came with the new file, edit that one

Now, let's write some R code. 

```
x <- 4*3
x
```

Now, hitting return does not execute this command; remember, it's just a text file. To execute it, we need to get what we typed in the the R chunk (the grey R code) down into the console. How do we do it? There are several ways (let's do each of them):

1. copy-paste this line into the console.
1. select the line (or simply put the cursor there), and click 'Run'. This is available from 
    a. the bar above the file (green arrow)
    b. the menu bar: Code > Run Selected Line(s)
    c. keyboard shortcut: command-return
1. click the green arrow at the right of the code chunk

### Your turn

Add a few more commands to your file. Execute them by trying the three ways above. Then, save your R Markdown file. 


## R functions, help pages

So far we've learned some of the basic syntax and concepts of R programming, how to navigate RStudio, and RMarkdown, but we haven't done any complicated or interesting programming processes yet. This is where functions come in!

R has a mind-blowing collection of built-in functions that are used with the same syntax: function name with parentheses around what the function needs in order to do what it was built to do. When you type a function like this, we say we are "calling the function". `function_name(argument1 = value1, argument2 = value2, ...)`. 

As our first function, we will introduce `read.csv`, which will be in the first lines of many of your future scripts. It does exactly what it says, it reads in a csv file to R, but we don't quite know how to use it yet. The help pages in RStudio will help us with this!

To access the help page for `read.csv`, enter the following into your console:


```r
?read.csv
```

The help pane will show up in the lower right hand corner of your RStudio.

You can see from this pane that the help page for this function is actually for a family of related functions that read in data. The help page tells the name of the package in the top left, and is broken down into sections:

 - Description: An extended description of what the function does.
 - Usage: The arguments of the function(s) and their default values.
 - Arguments: An explanation of the data each argument is expecting.
 - Details: Any important details to be aware of.
 - Value: The data the function returns.
 - See Also: Any related functions you might find useful.
 - Examples: Some examples for how to use the function.

The help for `read.csv` has a lot of information in it, as this function has a lot of arguments, but the first one seems pretty critical. We have to tell it what file to look for. Let's get a file!

##Sampling in R
Let's conduct a survey with pilots who reside in Alaska. For that, we first need a sample frame and then create a sample. 

### Download a sample frame for pilots

The Federal Aviation Administration (FAA) publishes the contact information for pilots on its website here: [FAA Airmen Certification Database](https://www.faa.gov/licenses_certificates/airmen_certification/releasable_airmen_download/), and download [this zip file](http://registry.faa.gov/database/CS022019.zip) to your downloads folder. 

Unzip the folder and move the `PILOT_BASIC.csv` file into a place you can easily find it. I recommend creating a folder called `data` in your previously-created directory `training_files`, and putting the file there.

### Use a function to read a file into R

Now we have to tell `read.csv` how to find the file. We do this using the `file` argument which you can see in usage section in the help page. In RMarkdown, you can either use absolute paths (which will start with your home directory `~/`) or paths **relative to the location of the RMarkdown.** RStudio and RMarkdown have some great autocomplete capabilities when using relative paths, so we will go that route. Assuming you have moved your file to a folder within `training_files` called `data`, your `read.csv` call will look like this:


```r
pilots <- read.csv("data/PILOT_BASIC.csv")
```

You should now have an object of the class `data.frame` in your environment called `pilots`. Check your environment pane to ensure this is true. How many pilots are in this dataframe?

Note that in the help page there are a whole bunch of arguments that we didn't use in the call above. Some of the arguments in function calls are optional, and some are required. Optional arguments will be shown in the usage section with a `name = value` pair, with the default value shown. If you do not specify a `name = value` pair for that argument in your function call, the function will assume the default value (example: `header = TRUE` for `read.csv`). Required arguments will only show the name of the argument, without a value. Note that the only required argument for `read.csv` is `file`.

Many R users (including myself) will override the default `stringsAsFactors` argument. The following code will turn all character variables into character strings. read.csv("data/PILOT_BASIC.csv", stringsAsFactors = FALSE)


And there's also help for when you only sort of remember the function name: double-questionmark:

```r
??install 
```

Not all functions have (or require) arguments:

```r
date()
```

```
## [1] "Mon Mar 04 12:13:52 2019"
```


### Using `data.frames`

A `data.frame` is a two dimensional data structure in R that mimics spreadsheet behavior. It is a collection of rows and columns of data, where each column has a name and represents a variable, and each row represents a measurement of that variable. When we ran `read.csv`, the object `pilots` that we created is a `data.frame`. There are a bunch of ways R and RStudio help you explore data frames. Here are a few, give them each a try:

- click on the word `pilots` in the environment pane
- click on the arrow next to `pilots` in the environment pane
- execute `head(pilots)` in the console
- execute `View(pilots)` in the console

### Subsetting data to get the appropriate sample frame
Let's say we are interested in conducting a survey with pilots who reside in Alaska. Then we need to subset this national database into a dataset that includes just pilots who reside in Alaska.To do this we need to know how Alaska residents are coded. It looks like the STATE variable will help. But there Alaska could be coded in any way. Check the metadata that comes with the dataset on the website, or use the following code to see how Alaska may be coded. This code will return all the unique values in the STATE variable.


```r
unique(pilots$STATE)
```



```r
pilotsAK <- subset(pilots,STATE=="AK")
```

How large is our population of pilots?

###Your Turn!
Use the function nrow() to find out. 


###Calculating sample size
First we need to calculate the desired sample size for which we need the following:
`Np` - population size
`prop` - proportion of population expected to choose one of two response categories: 
If we don't know about these expectations, we assume prop = 0.5 for a 50/50 split. Then we are more conservative (get a larger required sample) than assuming a 80/20 split.
`serror` - acceptable amount of sampling error of the true population value, let's set it at 3% 
`conf` - confidence level, also called p-value, let's say 95%. For calculations we use the decimal 0.95

Let's first declare the values for each of the variables above. 

```r
Np <- 7663
prop <- 0.5
serror <- 0.03
conf <- 0.95
```

The confidence level will give us the `z-score` or `z-statistic` which we will need for calculating the sample size.
For a 95% confidence interval, you want to know how many standard deviations away from the mean your point estimate is (the "z-score"). To get that, you take off the 5% "tails". Working in percentile form you have 1-.95 which yields a value 0.05. Divide that in half to get 0.025. So we basically do this: `(1-conf)/2`.

In R, use the `qnorm` function to get the "critical value". Since you only care about one "side" of the curve (the values on either side are mirror images of each other) and you want a positive number, pass the argument lower.tail=FALSE.

```r
zscore <- qnorm((1-conf)/2,lower.tail=FALSE)
```
For a 95% confidence interval this should be about 1.96

Now we are ready to calculate the sample size, Ns

```r
Ns <- Np * prop *(1-prop)/((Np-1)*(serror/zscore)^2+prop*(1-prop))
Ns
```

```
## [1] 936.7516
```


###Drawing a sample of size Ns from the sample frame, pilotsAK
First we are going to install the sampling package that let's us draw a random sample without replacement.
Install the `sampling` package or by typing install.packages(sampling) in the console. 

```r
install.packages(sampling)
library(sampling)
library(dplyr)
```

Now we need to create the simple random sample without replacement, for our first letter mail out

```r
sam.srswor2 <- srswor(n=Ns,N=Np)
sample.srswor2 <- pilotsAK[which(x=(sam.srswor2 ==1)),]
```

Finally, we select just the columns needed for names and addresses and save the file as a csv so we can share it and open in Excel for mailmerge, etc. 

```r
mailout <- select(sample.srswor2, FIRST.NAME, LAST.NAME, STREET.1, CITY, ZIP.CODE)

write.csv(mailout,"data/mailout.csv")
```
