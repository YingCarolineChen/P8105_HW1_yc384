Ying Chen (UNI: yc384)
9/14/2019

# **P8105 DSI Homework 1**

## Problem 0.1: Use R Markdown to write reproducible reports, GitHub for version control, and R Projects to organize your work.

  - Created a public GitHub repo and a local R Project both named
    P8105\_HW1\_yc384
  - Created a single .Rmd file: P8105\_HW1\_yc384.Rmd
  - My link to homework1 repo is:
    <https://github.com/YingCarolineChen/P8105_HW1_yc384.git>

## Problem 0.2: Code styling: pay attention the the points listed below

  - Meaningful variable / object names
  - Readable code (one command per line; adequate whitespace and
    indentation; etc)
  - Clearly-written text to explain code and results
  - A lack of superfluous code (no unused variables are defined; no
    extra library calls;
ect)

## Problem 1: Understand variable types in R: numeric, character, and factor variables:

### 1\_1. First code chunk: This chunk Create a data frame comprised of:

  - a random sample of size 8 from a standard Normal distribution
  - a logical vector indicating whether elements of the sample are
    greater than 0
  - a character vector of length 8
  - a factor vector of length 8, with 3 different factor “levels”

<!-- end list -->

``` r
knitr::opts_chunk$set(echo = TRUE)

library(tidyverse)
```

    ## ── Attaching packages ───────────────────────────────────────── tidyverse 1.2.1 ──

    ## ✔ ggplot2 3.2.1     ✔ purrr   0.3.2
    ## ✔ tibble  2.1.3     ✔ dplyr   0.8.3
    ## ✔ tidyr   0.8.3     ✔ stringr 1.4.0
    ## ✔ readr   1.3.1     ✔ forcats 0.4.0

    ## ── Conflicts ──────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
## Create a dataframe 
hw1_df = tibble(
 ## A random sample of size 8 from a standard Normal distribution
 norm_samp = rnorm (8, mean = 0, sd = 1),
 ## Logic vector: if elements of the random sample are positive
 norm_samp_pos = norm_samp > 0,
 ## Character vector of length 8
 vec_char = c("a", "b", "c", "abc", "ad", "ac", "bc", " "),
 ## Factor vector of length 8 with 3 differesnt levels
 vec_factor = factor(c("low", "high", "medium", "high", "low", "medium", "high",  
                       "low")),
)

## Take the mean of each variable
## Examing the mean of varible norm_samp: 
mean(hw1_df$norm_samp)
```

    ## [1] 0.2463512

``` r
## Examing the mean of varible norm_samp_pos: 
mean(hw1_df$norm_samp_pos)
```

    ## [1] 0.75

``` r
## Examing the mean of varible var_char: 
mean(hw1_df$vec_char)
```

    ## Warning in mean.default(hw1_df$vec_char): argument is not numeric or
    ## logical: returning NA

    ## [1] NA

``` r
## Examing the mean of varible var_factor: 
mean(hw1_df$vec_factor)
```

    ## Warning in mean.default(hw1_df$vec_factor): argument is not numeric or
    ## logical: returning NA

    ## [1] NA

  - **From results above, we can see that the ransom normal sample and
    the logic vector had their mean calculated. But the character
    variable and the factor variable did not have the mean
calculated.**

### 1\_2. Second code chunk applies as.numeric function to the other types of varaibles(only show code, not output)

``` r
as.numeric(pull(hw1_df, norm_samp_pos))
as.numeric(pull(hw1_df, vec_char))
as.numeric(pull(hw1_df, vec_factor))
```

  - When apply as.numeric function to the logical variables, it returns
    0 and 1;

  - When apply as.numeric function to the character variables, it
    returns NA;

  - When apply as.numeric function to the factor variables, it returns
    three levels in  
    numbers according to their alphabet order;

  - Yes. This is helpful to explain why we could not take the mean of a
    character or a factor variables. They are not numeric variables.

### 1\_3. Third code chunk:

  - convert the logical vector to numeric, and multiply the random
    sample by the result
  - convert the logical vector to a factor, and multiply the random
    sample by the result
  - convert logical vector to factor & then convert result to numeric &
    multiply the random sample by the result

<!-- end list -->

``` r
## Create a data frame that converts different types of variable
hw1_dt = tibble(
  norm_samp = rnorm (8, mean = 0, sd = 1),
  
  ## Convert logical vector to numeric
  logic_num = as.numeric(pull(hw1_df, norm_samp_pos)),
  ## multiply converted variable by the random sample
  logic_num_normsamp = logic_num * norm_samp,
  
  ## Covert logical vector to factor
  logic_factor = as.factor(pull(hw1_df, norm_samp_pos)),
  ## multiply converted variable by the random sample
  logic_factor_normsamp = logic_factor * norm_samp,
  
  ## convert logical vector to factor & then convert result to numeric
  logic_factor_num = as.numeric(as.factor(pull(hw1_df, norm_samp_pos))),
  ## multiply converted variable by the random sample
  logic_factor_num_normsamp = logic_factor_num * norm_samp
)
```

    ## Warning in Ops.factor(logic_factor, norm_samp): '*' not meaningful for
    ## factors

## Problem 2: Using ggplot to plot different types of variables

### 2\_1 Create data frame: HW1\_2\_df

``` r
knitr::opts_chunk$set(echo = TRUE)

## set seed to ensure reproducibility
set.seed(1)

## Create a dataframe 
HW1_2_df = tibble(
## A random sample of size 500
 x = rnorm (500),
 y = rnorm (500),

 # Logic vector: if x + y > 1
 Logical_var1 = x + y > 1,
 # coercing the above logical vector to numeric
 numeric_var1 = as.numeric(Logical_var1),
 # coercing the above logical vector to factor
 factor_var1 = as.factor(Logical_var1),
)
```

### 2\_2 Description of Dataset HW1\_2\_df:

  - The dataset: hw2\_df contains two random samples of size 500 and 5
    variables.
  - The mean of x is 0.0226441, the median of x is -0.0367783 and
    standard deviation of x is 1.0119283
  - the proportion of cases for which x + y \> 1 is: 25.2%.

### 2\_3 Make a scatterplot of y vs x

``` r
## Scatter plot clolored for logical variable
first_plot = ggplot(HW1_2_df, aes(x = x, y = y, color=Logical_var1)) + geom_point() 
## Export the first scatterplot to project directory using ggsave
ggsave("first_plot.pdf", first_plot, width = 8, height = 5)

## Scatter plot clolored for numerical variable
ggplot(HW1_2_df, aes(x = x, y = y, color=numeric_var1)) + geom_point()
```

![](P8105_HW1_yc384-Knit-file_files/figure-gfm/HW1_2_plots-1.png)<!-- -->

``` r
## Scatter plot clolored for factor variable
ggplot(HW1_2_df, aes(x = x, y = y, color=factor_var1)) + geom_point()
```

![](P8105_HW1_yc384-Knit-file_files/figure-gfm/HW1_2_plots-2.png)<!-- -->
