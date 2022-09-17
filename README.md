
# Tidyverse

Tidyverse has many interesting tools but here the focus will be the *dplyr* package which is the most famous package for data manipulation. My aim is to introduce the most important functions from *dplyr* and then enumerate some frequent situations when this package proves to be helpful. 

## Display all functions from *dplyr*

```{r,results=FALSE}
lsf.str("package:dplyr")  
```

## Dplyr's "Holly Grail":

```{r,echo=FALSE}
functions <- data.frame(Functions = c("mutate: edit columns from dataframe","select: selects columns from dataframe", "filter: select lines that obey some condition applied to a column","rename: renames columns","group_by: identifies groups within one column ","summarise: usually used with group_by to generate statistics for groups from a column"))
```

```{r,echo=FALSE}
functions %>%
  kbl() %>%
  kable_styling()
```

In this tutorial, we'll use the classic *starwars* dataset already avaiable in R.

```{r}
data <- starwars
```

# Part One: Basics

## 1 mutate

This bad boy can change the values from a column, change the name of a column, add new columns,...

```{r}
# (1) Add column (that I'll name "ratio") of ratio between height and mass

# (2) Change height from centimeters to meters

df <- data %>%
  mutate(ratio = height/mass,
         height = height/100)
```

## 2 
