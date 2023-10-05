p8105_hw2_NL2836
================
Ngan Le
2023-10-04

Load the necessary libraries.

``` r
library(tidyverse)
library(readr)
library(readxl)
```

# Problem 1

Read the dataset pols-month into R Studio, then clean up accordingly.

``` r
pols_data = 
  read_csv("./fivethirtyeight_datasets/pols-month.csv") %>% 
  separate(mon, into = c("year","month","day"), sep = "-") %>%
  janitor::clean_names() %>% 
  mutate(
    month = month.abb[as.integer(month)],
    president = case_when(
      prez_gop == 1 ~ "gop",
      prez_dem == 1 ~ "dem",
      TRUE ~ NA_character_)
  ) %>% 
  select(-prez_dem, -prez_gop, -day)
```

Read the dataset snp into R Studio, then clean up accordingly.

``` r
snp_data = 
  read_csv("./fivethirtyeight_datasets/snp.csv") %>% 
  janitor::clean_names() %>% 
  mutate(
    date = as.Date(date, format = "%m/%d/%y"),
    year = format(date, "%Y"),
    month = format(date, "%m"),
    month = month.abb[as.integer(month)]
  ) %>%
  select(-date) %>%
  arrange(year, month) %>% 
  select(year, month, everything())
```

Read the dataset unemployment into R Studio, then clean up accordingly.

``` r
unemp_data = 
  read_csv("./fivethirtyeight_datasets/unemployment.csv") %>% 
  janitor::clean_names() %>% 
  pivot_longer(
    jan:dec,
    names_to = "month",
    values_to = "unemployment_rate") %>% 
 mutate(year = as.character(year))
```

Merge the 3 datasets.

``` r
merged_data = pols_data %>% 
  left_join(snp_data, by = c("year", "month")) %>% 
  left_join(unemp_data, by = c("year", "month"))
```

View the merged dataset and short description.

``` r
view(merged_data)
```

The merged dataset `merged_data` from the 3 datasets `pols_data`,
`snp_data`, `unemp_data` contains:

- The information on the number of Republican and Democratic senators
  (`gop_sen`, `dem_sen`) and presidents (`president`), the closing
  values of the S&P stock index (`close`), and unemployment rate
  (`unemployment_rate`); all reported monthly from 1947 to 2015.
- 11 columns (variables), and 822 rows (observations).
- Each of the three dataset was modified to have the variables `year`
  and `month`.
- Additionally, `snp_data` contains the the closing values of the S&P
  stock index (`close`); `unemp_data` contains the unemployment rate
  (`unemployment_rate`); the rest of the mentioned variables are
  `pols_data`.

# Problem 2

Rename the `202309 Trashwheel Collection Data` file locally to
`trashwheel`.

Load the Mr. Trashwheel dataset.

``` r
mrtrashwheel_data = 
  read_excel("./trashwheel.xlsx", sheet = "Mr. Trash Wheel") %>% 
  janitor::clean_names() %>% 
  select(dumpster:homes_powered) %>% 
  drop_na(dumpster) %>% 
  mutate(
    homes_powered = (weight_tons*500)/30)
```

    ## New names:
    ## • `` -> `...15`
    ## • `` -> `...16`

Load the Prof. Trashwheel dataset.

``` r
proftrashwheel_data = 
  read_excel("./trashwheel.xlsx", sheet = "Professor Trash Wheel") %>% 
  janitor::clean_names() %>% 
  select(dumpster:homes_powered) %>% 
  drop_na(dumpster) %>% 
  mutate(
    homes_powered = (weight_tons*500)/30)
```

Load the Gwynnda Trashwheel dataset.

``` r
gwynntrashwheel_data = 
  read_excel("./trashwheel.xlsx", sheet = "Gwynnda Trash Wheel") %>% 
  janitor::clean_names() %>% 
  select(dumpster:homes_powered) %>% 
  drop_na(dumpster) %>% 
  mutate(
    homes_powered = (weight_tons*500)/30)
```

# Problem 2
