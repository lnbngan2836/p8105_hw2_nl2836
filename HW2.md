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

Read the dataset pols-month into R Studio, then clean up accordingly.

``` r
pols_data = 
  read_csv("./fivethirtyeight_datasets/pols-month.csv") %>% 
  separate(mon, into = c("year","month","day"), sep = "-") %>%
  mutate(
    month = month.abb[as.integer(month)],
    president = case_when(
      prez_gop == 1 ~ "gop",
      prez_dem == 1 ~ "dem",
      TRUE ~ NA_character_)
  ) %>% 
  select(-prez_dem, -prez_gop, -day)

view(pols_data)
```

Read the dataset snp into R Studio, then clean up accordingly.

``` r
snp_data = 
  read_csv("./fivethirtyeight_datasets/snp.csv") %>% 
  mutate(
    date = as.Date(date, format = "%m/%d/%y"),
    year = format(date, "%Y"),
    month = format(date, "%m"),
    month = month.abb[as.integer(month)]
  ) %>%
  select(-date) %>%
  arrange(year, month) %>% 
  select(year, month, everything())

view(snp_data)
```

Read the dataset unemployment into R Studio, then clean up accordingly.

``` r
unemp_data = 
  read_csv("./fivethirtyeight_datasets/unemployment.csv") %>% 
  pivot_longer(
    Jan:Dec,
    names_to = "month",
    values_to = "unemployment_rate") %>% 
  rename(year = Year) %>% 
  mutate(year = as.character(year))

view(unemp_data)
```

Merge the 3 datasets.

``` r
merged_data = pols_data %>% 
  left_join(snp_data, by = c("year", "month")) %>% 
  left_join(unemp_data, by = c("year", "month"))
  
view(merged_data)
```
