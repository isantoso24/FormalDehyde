Formaldehyde analysis with TRI and NEI
================
M Ilham AR Santoso
09/05/2024

``` r
#install.packages("readxl")
library(readxl)
#install.packages("dplyr")
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
#install.packages("readr")
library(readr)
#install.packages("stringr")
library(stringr)
#install.packages("tidyr")
library(tidyr)
#install.packages("ggplot2")
library(ggplot2)
```

``` r
NEI <- read_excel("~/FormalDehyde/Formaldehyde NEI.xlsx")
TRI <- read_csv("~/FormalDehyde/2020_us-TRI basic data.csv")
```

    ## Rows: 78381 Columns: 122
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (100): 2. TRIFD, 4. FACILITY NAME, 5. STREET ADDRESS, 6. CITY, 7. COUNTY...
    ## dbl  (15): 1. YEAR, 3. FRS ID, 10. BIA, 12. LATITUDE, 13. LONGITUDE, 22. IND...
    ## lgl   (7): 20. STANDARD FOREIGN PARENT CO NAME, 24. PRIMARY SIC, 25. SIC 2, ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

Filtering the TRI data to match the Chemical column into only
Formaldehyde

``` r
TRI <- TRI %>%
  filter(str_detect(str_replace_all(`37. CHEMICAL`, " ", ""), regex("formaldehyde", ignore_case = TRUE)))
```

``` r
TRI$`51. 5.1 - FUGITIVE AIR` <- as.double(TRI$`51. 5.1 - FUGITIVE AIR`)

total_sum_fugitive_air <- TRI %>%
  summarise(Total = sum(`51. 5.1 - FUGITIVE AIR`, na.rm = TRUE))

# View the result
print(total_sum_fugitive_air)
```

    ## # A tibble: 1 × 1
    ##     Total
    ##     <dbl>
    ## 1 361706.

The total fugitive air in TRI is 3.6170633^{5} pounds or 180.8531669
Tons

``` r
TRI$`52. 5.2 - STACK AIR` <- as.double(TRI$`52. 5.2 - STACK AIR`)

total_sum_stack_air <- TRI %>%
  summarise(Total = sum(`52. 5.2 - STACK AIR`, na.rm = TRUE))

# View the result
print(total_sum_stack_air)
```

    ## # A tibble: 1 × 1
    ##      Total
    ##      <dbl>
    ## 1 4328212.

The total stack air in TRI is 4.3282119^{6} pounds or 2164.1059707 Tons

``` r
total_sum_NEI_air <- NEI %>%
  summarise(Total = sum(`Emissions (Tons)`, na.rm = TRUE))

# View the result
print(total_sum_NEI_air)
```

    ## # A tibble: 1 × 1
    ##    Total
    ##    <dbl>
    ## 1 24956.

The total emission in NEI is 2.4955924^{4} tons
