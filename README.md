<a href='https://github.com/titogbruni/Tidyverse'><img src='https://raw.githubusercontent.com/titogbruni/Tidyverse/main/dplyr.png?token=GHSAT0AAAAAABXANQWRBG7IBZTVFFYNO6ROYZH2XFQ' align="right" height="139" /></a>

<!-- README.md is generated from README.Rmd. Please edit that file -->

# dplyr 

<!-- badges: start -->
[![CRAN status](https://www.r-pkg.org/badges/version/dplyr)](https://cran.r-project.org/package=dplyr)
[![R-CMD-check](https://github.com/tidyverse/dplyr/actions/workflows/R-CMD-check.yaml/badge.svg)](https://github.com/tidyverse/dplyr/actions/workflows/R-CMD-check.yaml)
[![Codecov test coverage](https://codecov.io/gh/tidyverse/dplyr/branch/main/graph/badge.svg)](https://app.codecov.io/gh/tidyverse/dplyr?branch=main)
<!-- badges: end -->

Tidyverse has many interesting tools but here the focus will be the *dplyr* package which is the most famous package for data manipulation. My aim is to introduce the most important functions from *dplyr* and then enumerate some less popular functions which I think have proved to be helpful for me. 

## Disclaimer

I use the *pipe* (%>%) command throughout this tutorial. It comes from *tidyverse*. It's quite simple and useful:

```{r}
# Instead of writing:

funtion_name(dataframe_name, ...)

# We write:

dataframe_name %>%
  function_name(...)

```
## Display all functions from *dplyr*

```{r}
lsf.str("package:dplyr")  
```

<a href='https://github.com/titogbruni/Tidyverse'><img src='https://raw.githubusercontent.com/titogbruni/Tidyverse/main/holy_grail.jpg?token=GHSAT0AAAAAABXANQWQ5I7SYIWK3ZV3FNNYYZH2WWQ' align="right" height="139" /></a>

## Dplyr's "Holy Grail": 

- `mutate()`: edit columns from dataframe

- `select()`: selects columns from dataframe 

- `filter()`: select lines that obey some condition applied to a column

- `rename()`: renames columns

- `group_by()`: identifies groups within one column

- `summarise()`: usually used with group_by to generate statistics for groups from a column

In this tutorial, we'll use the classic *starwars* dataset already avaiable in R.

```{r}
data <- starwars

data
```

# Part One: Basics

```{r}
library(tidyverse)
```

## 1 mutate

This bad boy can change the values from a column, change the name of a column, add new columns,...

```{r}
# (1) Add column (that I'll name "ratio") of ratio between height and mass

# (2) Change height from centimeters to meters

data %>%
  mutate(ratio = height/mass,
         height = height/100)
```

## 2 select

```{r}
data %>%
  select(height,mass)
```

## 3 filter

```{r}
# extracting the lines where the sex is female AND the height is >100

data %>%
  filter(sex == "female",
         height > 100)

```

## 4 rename

```{r}
# Changing the names from the "height" and "mass" columns 

data %>%
  rename(altura = height,
         massa = mass)
```

## 5 summarise

```{r}
# Creates a 2 column data frame (whose columns I named "media" and "observacoes") 

# The data frame has the mean (discarding NA) and the number of observations from the the column height

data %>%
  summarise(media = mean(height, na.rm = TRUE),
            observacoes = n())
```
## 6 group_by + summarise

```{r}
# For each sex, I'll extract the average height, average mass and number of observations

data %>%
  group_by(sex) %>% 
  summarise(mean(height, na.rm = TRUE),
            mean(mass, na.rm = TRUE),
            n())
```

# Part Two: Some cool functions

## 1 case_when

It's an awesome way to work with *if* sentences. It's useful to combine it with *mutate* so that you edit the lines that obey a condition.

```{r}
# Translating the column "hair_color" to portuguese

# (1) Bad way:

for (i in 1:length(hair_color){
     if(data$hair_color[i] == "blond") {
        data$hair_color[i] <- "loiro}}

# (2) Good way: 

data %>%
  mutate(hair_color = case_when(hair_color == "blond" ~ "loiro",
                                TRUE ~ hair_color))
# Translating multiple lines:

data %>%
  mutate(hair_color = case_when(hair_color == "blond" ~ "loiro",
                                hair_color == "black" ~ "preto",
                                hair_color == "brown" ~ "castanho",
                                TRUE ~ hair_color))
```


## 2 contains 

Very useful when combined with *select*. A similar function is *matches*.

```{r}
# Select columns with "color" in its name

data %>%
  select(contains("color"))

```

## 3 mutate_at

Mutates (applies a chosen function) only the columns which are specified. 

```{r}
# Columns "height" and "mass" will be in log from now on.

data %>%
  mutate_at(c("height", "mass"), log)
```

Another way of doing this is by using *mutate(across(col_names, ~function(.x)))*

```{r}
data %>%
  mutate(across(c("height","mass"), ~log(.x)))
```


