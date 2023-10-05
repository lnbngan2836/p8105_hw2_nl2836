p8105_hw2_NL2836
================
Ngan Le
2023-10-04

Load the necessary libraries.

``` r
library(tidyverse)
library(readr)
```

# Problem 1

Read the dataset into R Studio, then clean up accordingly.

``` r
pols_data = 
  read_csv("./fivethirtyeight_datasets/pols-month.csv") %>% 
  separate(mon, into = c("year","month","day"), sep = "-") %>%
  mutate(
    month = month.abb[as.integer(month)],
    president = ifelse(prez_gop == 1, "gop", ifelse(prez_dem == 1, "dem", NA))
  ) %>% 
  select(-prez_dem, -prez_gop, -day)
```
