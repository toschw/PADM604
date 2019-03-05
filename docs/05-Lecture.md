---
output: html_document
editor_options: 
  chunk_output_type: console
---
# Lecture 5 -  Regression

##Acknowledgment
This chapter was modfied based on work that is licensed under a Creative Commons Attribution 4.0 International License.

Citation: Prabhakaran, Selva (https://r-statistics.co/Linear-Regression.html)

## Learning Objectives

In this lesson, you will learn:

- Use R's internal datasets to practice regression
- Interpret model results and conduct model/data checking

First, let's start a brand new RMarkdown document and save that somewhere, calling it regression or similar. 

## Regression example
For this analysis, we will use the cars dataset that comes with R by default. cars is a standard built-in dataset, that makes it convenient to demonstrate linear regression in a simple and easy to understand fashion. You can access this dataset simply by typing in cars in your R console. You will find that it consists of 50 observations(rows) and 2 variables (columns) – dist and speed. It contains data that was recorded in the 1920s and shows speed and the measured stopping distance associated with that speed.  Lets print out the first six observations here.


```r
head(cars)
```

Before we begin building the regression model, it is a good practice to analyze and understand the variables. The graphical analysis and correlation study below will help with this.

###Research question
What are we interested in looking at the cars data? In other words, for the cars data set, showing us car speed and car breaking distance, what is the independent and what's the dependent variable?

##Graphical Analysis
The aim of this exercise is to build a simple regression model that we can use to predict Distance (dist) by establishing a statistically significant linear relationship with Speed (speed). But before jumping in to the syntax, lets try to understand these variables graphically. Typically, for each of the independent variables (predictors), the following plots are drawn to visualize the following behavior:

  1. Scatter plot: Visualize the linear relationship between the predictor and response
  2. Box plot: To spot any outlier observations in the variable. Having outliers in your predictor can drastically affect the predictions as they can easily affect the direction/slope of the line of best fit.
  3. Density plot: To see the distribution of the predictor variable. Ideally, a close to normal distribution (a bell shaped curve), without being skewed to the left or right is preferred. Let us see how to make each one of them.
  

###Scatter Plot
Scatter plots can help visualize any linear relationships between the dependent (response) variable and independent (predictor) variables. Ideally, if you are having multiple predictor variables, a scatter plot is drawn for each one of them against the response, along with the line of best as seen below.

```r
scatter.smooth(x=cars$speed, y=cars$dist, main="Dist ~ Speed")
```

The scatter plot along with the smoothing line above suggests a linearly increasing relationship between the ‘dist’ and ‘speed’ variables. This is a good thing, because, one of the underlying assumptions in linear regression is that the relationship between the response and predictor variables is linear and additive.

###BoxPlot – Check for outliers
Generally, any datapoint that lies outside the `1.5 * interquartile-range (1.5*IQR)` is considered an outlier, where, IQR is calculated as the distance between the 25th percentile and 75th percentile values for that variable.

Box plot for speed

```r
boxplot(cars$speed, main="Speed", sub=paste("Outlier rows: ", boxplot.stats(cars$speed)$out))
```

Box plot for distance

```r
boxplot(cars$dist, main="Distance", sub=paste("Outlier rows: ", boxplot.stats(cars$dist)$out))
```

###Density plot
Now let's check if the response variable is close to normality and calculate the skewness. The skewness is a measure of symmetry. A negative skewness indicates that the mean is less than the median, and the distribution is left skewed. A positive skewness indicates that the mean is larger than the median and the data distribution is right skewed. Consequently, if skewness is close to 0, the distirbution is symmetrical. 


```r
plot(density(cars$dist), main="Density Plot for Distance", ylab="Frequency")
```

Calculating skewness

```r
library(e1071)
skewness(cars$dist)
```

Plotting density for speed

```r
plot(density(cars$speed), main="Density Plot for Speed", ylab="Frequency")
```

Calculating skewness for speed

```r
library(e1071)
skewness(cars$speed)
```

##Correlation
Correlation is a statistical measure that suggests the level of linear dependence between two variables, that occur in pair – just like what we have here in speed and dist. Correlation can take values between -1 to +1. If we observe for every instance where speed increases, the distance also increases along with it, then there is a high positive correlation between them and therefore the correlation between them will be closer to 1. The opposite is true for an inverse relationship, in which case, the correlation between the variables will be close to -1.

A value closer to 0 suggests a weak relationship between the variables. A low correlation (-0.2 < x < 0.2) probably suggests that much of variation of the response variable (Y) is unexplained by the predictor (X), in which case, we should probably look for better explanatory variables.

Calculating correlation between speed and distance


```r
cor(cars$speed, cars$dist)
```

##Build Linear Model
Now that we have seen the linear relationship pictorially in the scatter plot and by computing the correlation, lets see the syntax for building the linear model. The function used for building linear models is `lm()`. The `lm()` function takes in two main arguments, namely: 1. Formula 2. Data. The data is typically a data.frame and the formula is a object of class formula. But the most common convention is to write out the formula directly in place of the argument as written below.

Let's build a linear regression model.  


```r
linearMod <- lm(dist ~ speed, data=cars)  # 
print(linearMod)
```

Now that we have built the linear model, we also have established the relationship between the predictor and response in the form of a mathematical formula for Distance (dist) as a function for speed. For the above output, you can notice the ‘Coefficients’ part having two components: Intercept: -17.579, speed: 3.932 These are also called the beta coefficients. In other words, 
`dist=Intercept+(β∗speed)`
=> dist = −17.579 + 3.932∗speed

###Linear Regression Diagnostics
Now the linear model is built and we have a formula that we can use to predict the dist value if a corresponding speed is known. Is this enough to actually use this model? NO! Before using a regression model, you have to ensure that it is statistically significant. How do you ensure this? Lets begin by printing the summary statistics for linearMod.


```r
summary(linearMod)
```

##The p Value: Checking for statistical significance
The summary statistics above tells us a number of things. One of them is the model p-Value (bottom last line) and the p-Value of individual predictor variables (extreme right column under ‘Coefficients’). The p-Values are very important because, We can consider a linear model to be statistically significant only when both these p-Values are less that the pre-determined statistical significance level, which is ideally 0.05. This is visually interpreted by the significance stars at the end of the row. The more the stars beside the variable’s p-Value, the more significant the variable.

Null and alternate hypothesis
When there is a p-value, there is a hull and alternative hypothesis associated with it. In Linear Regression, the Null Hypothesis is that the coefficients associated with the variables are equal to zero. The alternate hypothesis is that the coefficients are not equal to zero (i.e. there exists a relationship between the independent variable in question and the dependent variable), so at least one coefficient (if you have multiple) is not zero.

t-value
We can interpret the t-value something like this. A larger t-value indicates that it is less likely that the coefficient is not equal to zero purely by chance. So, higher the t-value, the better.

Pr(>|t|) or p-value is the probability that you get a t-value as high or higher than the observed value when the Null Hypothesis (the β coefficient is equal to zero or that there is no relationship) is true. So if the Pr(>|t|) is low, the coefficients are significant (significantly different from zero). If the Pr(>|t|) is high, the coefficients are not significant.

What this means to us? when p Value is less than significance level (< 0.05), we can safely reject the null hypothesis that the co-efficient β of the predictor is zero. In our case, linearMod, both these p-Values are well below the 0.05 threshold, so we can conclude our model is indeed statistically significant.

It is absolutely important for the model to be statistically significant before we can go ahead and use it to predict (or estimate) the dependent variable, otherwise, the confidence in predicted values from that model reduces and may be construed as an event of chance.

