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
p1_data = 
  read_csv("./fivethirtyeight_datasets/pols-month.csv") %>% 
  separate(mon, into = c("year","month","day"), sep = "-") %>%
  mutate(
    month = month.abb[as.integer(month)],
    president = ifelse(prez_gop == 1, "gop", ifelse(prez_dem == 1, "dem", NA))
  ) %>% 
  select(-prez_dem, -prez_gop, -day)
```

    ## Rows: 822 Columns: 9
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## dbl  (8): prez_gop, gov_gop, sen_gop, rep_gop, prez_dem, gov_dem, sen_dem, r...
    ## date (1): mon
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
