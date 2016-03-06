---
title       : "Predicting child's height from parents' heights"
subtitle    : Quick and dirty Shiny app for Coursera DDP course
author      : 
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : [mathjax]     # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## What does it do?

1. User provides his/her, as well as partner's or hypothetical partner's height and sex
2. The app uses *father.son* dataset from **UsingR** package to build a simple linear regression model
3. Child's height prediction is obtained from parents' adjusted average height and is output on the screen together with the prediction error and the accompanying plot

--- .class #id 

## Inner workings of the app


- The linear prediction model is built using the following command:


```r
fit <- lm(sheight~fheight,data=father.son)
```

- This results in a model with the following coefficients:


```
##              Estimate Std. Error  t value     Pr(>|t|)
## (Intercept) 33.886604 1.83235382 18.49348 1.604044e-66
## fheight      0.514093 0.02704874 19.00618 1.121268e-69
```

--- .class #id

## Inner workings of the app (contd.)
- User inputs their sex and height as well as their partner's sex and height
- Heights are input in centimeters and the prediction result is in centimeters too, while the dataset is in inches
- To provide conversion between units, two functions are implemented:


```r
in2cm <- function(inches) {inches/0.39370}
cm2in <- function(cm) {cm * 0.39370}
```
- Female parent's height is adjusted with multiplication by 1.08 and midheight of the two parents is calculated inside a function and then subsequently used to predict child's height with:
 

```r
getChildHeight <- function(lmfit, newData) {
    predict(lmfit, data.frame(fheight = c(newData)), interval = "prediction")
}
```

--- .class #id

## Example
- Suppose user is female and is 160 cm high, while their partner is male and is 180 cm high


- Parents' adjusted average height is then 176.4 cm.



- Child's predicted height is then 176.8 +/- 12.2 cm.

- If the user inputs heights such that the adjusted average height is outside of the data cloud, then the script outputs the following message:
    
    "PREDICTION UNRELIABLE, IT IS OUTSIDE OF THE DATA CLOUD!"
    
- Similarly is the user specifies same sex couple, then the script states:
    
    "Unfortunately script doesn't work for same sex couples."



