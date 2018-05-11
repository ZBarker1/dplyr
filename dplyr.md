---
title: "Dplyr tut"
author: "Ziqi Liu"
date: "May 10, 2018"
output: 
  html_document:
    keep_md: true
    toc: true
    toc_float: true
    toc_depth: 2
editor_options:
  chunk_output_type: console
---

# 1.0 Loading dplyr

- 5 basic verbs:
1. filter()
2. select()
3. arrange()
4. mutate()
5. summarise()

```r
suppressMessages(library(dplyr))
library(hflights)
sessionInfo()
```

```
## R version 3.5.0 (2018-04-23)
## Platform: i386-w64-mingw32/i386 (32-bit)
## Running under: Windows 10 x64 (build 16299)
## 
## Matrix products: default
## 
## locale:
## [1] LC_COLLATE=English_Canada.1252  LC_CTYPE=English_Canada.1252   
## [3] LC_MONETARY=English_Canada.1252 LC_NUMERIC=C                   
## [5] LC_TIME=English_Canada.1252    
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
## [1] hflights_0.1 dplyr_0.7.4 
## 
## loaded via a namespace (and not attached):
##  [1] Rcpp_0.12.16     digest_0.6.15    rprojroot_1.3-2  assertthat_0.2.0
##  [5] R6_2.2.2         backports_1.1.2  magrittr_1.5     evaluate_0.10.1 
##  [9] pillar_1.2.2     rlang_0.2.0      stringi_1.1.7    bindrcpp_0.2.2  
## [13] rmarkdown_1.9    tools_3.5.0      stringr_1.3.0    glue_1.2.0      
## [17] yaml_2.1.19      compiler_3.5.0   pkgconfig_2.0.1  htmltools_0.3.6 
## [21] bindr_0.1.1      knitr_1.20       tibble_1.4.2
```

```r
#explore data
data(hflights)
head(hflights)
```

```
##      Year Month DayofMonth DayOfWeek DepTime ArrTime UniqueCarrier
## 5424 2011     1          1         6    1400    1500            AA
## 5425 2011     1          2         7    1401    1501            AA
## 5426 2011     1          3         1    1352    1502            AA
## 5427 2011     1          4         2    1403    1513            AA
## 5428 2011     1          5         3    1405    1507            AA
## 5429 2011     1          6         4    1359    1503            AA
##      FlightNum TailNum ActualElapsedTime AirTime ArrDelay DepDelay Origin
## 5424       428  N576AA                60      40      -10        0    IAH
## 5425       428  N557AA                60      45       -9        1    IAH
## 5426       428  N541AA                70      48       -8       -8    IAH
## 5427       428  N403AA                70      39        3        3    IAH
## 5428       428  N492AA                62      44       -3        5    IAH
## 5429       428  N262AA                64      45       -7       -1    IAH
##      Dest Distance TaxiIn TaxiOut Cancelled CancellationCode Diverted
## 5424  DFW      224      7      13         0                         0
## 5425  DFW      224      6       9         0                         0
## 5426  DFW      224      5      17         0                         0
## 5427  DFW      224      9      22         0                         0
## 5428  DFW      224      9       9         0                         0
## 5429  DFW      224      6      13         0                         0
```

- tbl_df creates a local data frame, wraps text so prints nicely

```r
flights <- tbl_df(hflights)
flights
```

```
## # A tibble: 227,496 x 21
##     Year Month DayofMonth DayOfWeek DepTime ArrTime UniqueCarrier
##  * <int> <int>      <int>     <int>   <int>   <int> <chr>        
##  1  2011     1          1         6    1400    1500 AA           
##  2  2011     1          2         7    1401    1501 AA           
##  3  2011     1          3         1    1352    1502 AA           
##  4  2011     1          4         2    1403    1513 AA           
##  5  2011     1          5         3    1405    1507 AA           
##  6  2011     1          6         4    1359    1503 AA           
##  7  2011     1          7         5    1359    1509 AA           
##  8  2011     1          8         6    1355    1454 AA           
##  9  2011     1          9         7    1443    1554 AA           
## 10  2011     1         10         1    1443    1553 AA           
## # ... with 227,486 more rows, and 14 more variables: FlightNum <int>,
## #   TailNum <chr>, ActualElapsedTime <int>, AirTime <int>, ArrDelay <int>,
## #   DepDelay <int>, Origin <chr>, Dest <chr>, Distance <int>,
## #   TaxiIn <int>, TaxiOut <int>, Cancelled <int>, CancellationCode <chr>,
## #   Diverted <int>
```

# 2.0 filter()

- allows you to filter by rows matching certain criteria

### Command Structure
- first argument is a data frame, following arguments are what to filter by
- return value is a data frame

```r
filter(flights, Month==1, DayofMonth==1)
```

```
## # A tibble: 552 x 21
##     Year Month DayofMonth DayOfWeek DepTime ArrTime UniqueCarrier
##    <int> <int>      <int>     <int>   <int>   <int> <chr>        
##  1  2011     1          1         6    1400    1500 AA           
##  2  2011     1          1         6     728     840 AA           
##  3  2011     1          1         6    1631    1736 AA           
##  4  2011     1          1         6    1756    2112 AA           
##  5  2011     1          1         6    1012    1347 AA           
##  6  2011     1          1         6    1211    1325 AA           
##  7  2011     1          1         6     557     906 AA           
##  8  2011     1          1         6    1824    2106 AS           
##  9  2011     1          1         6     654    1124 B6           
## 10  2011     1          1         6    1639    2110 B6           
## # ... with 542 more rows, and 14 more variables: FlightNum <int>,
## #   TailNum <chr>, ActualElapsedTime <int>, AirTime <int>, ArrDelay <int>,
## #   DepDelay <int>, Origin <chr>, Dest <chr>, Distance <int>,
## #   TaxiIn <int>, TaxiOut <int>, Cancelled <int>, CancellationCode <chr>,
## #   Diverted <int>
```

# 3.0 select()

- allows you to pick columns by name

### Command Structure
- first argument is a data frame, following arguments are column names

```r
select(flights, DepTime, ArrTime, FlightNum)
```

```
## # A tibble: 227,496 x 3
##    DepTime ArrTime FlightNum
##  *   <int>   <int>     <int>
##  1    1400    1500       428
##  2    1401    1501       428
##  3    1352    1502       428
##  4    1403    1513       428
##  5    1405    1507       428
##  6    1359    1503       428
##  7    1359    1509       428
##  8    1355    1454       428
##  9    1443    1554       428
## 10    1443    1553       428
## # ... with 227,486 more rows
```

## 3.1 Chaining or Pipelining

- allows you to perform multiple operations in one line by nesting commands
- use the %>% operator (pronounced "and then")

```r
flights %>%
  select(UniqueCarrier, DepDelay) %>%
  filter(DepDelay > 60)
```

```
## # A tibble: 10,242 x 2
##    UniqueCarrier DepDelay
##    <chr>            <int>
##  1 AA                  90
##  2 AA                  67
##  3 AA                  74
##  4 AA                 125
##  5 AA                  82
##  6 AA                  99
##  7 AA                  70
##  8 AA                  61
##  9 AA                  74
## 10 AS                  73
## # ... with 10,232 more rows
```

```r
# use dataframe flights AND THEN select column names UniqueCarrier and DepDelay AND THEN filter by rows where DepDelay value > 60
```

# 4.0 arrange()

- allows you to reorder rows by sorting in ascending order (default)

### Command Structure

- call dataframe, then select columns, then arrange by the column of choice
- for descending order use arrange(desc(column_name))

```r
flights %>%
  select(UniqueCarrier, DepDelay) %>%
  arrange(DepDelay)
```

```
## # A tibble: 227,496 x 2
##    UniqueCarrier DepDelay
##    <chr>            <int>
##  1 OO                 -33
##  2 MQ                 -23
##  3 XE                 -19
##  4 XE                 -19
##  5 CO                 -18
##  6 EV                 -18
##  7 XE                 -17
##  8 CO                 -17
##  9 XE                 -17
## 10 MQ                 -17
## # ... with 227,486 more rows
```

```r
# selects UniqueCarrier and DepDelay columns and then rearranges by DepDelay ascending
```

# 5.0 mutate()

- allows you to add new variables that are functions of existing variables

```r
flights %>%
  select(Distance, AirTime) %>%
  mutate(Speed = Distance/AirTime*60)
```

```
## # A tibble: 227,496 x 3
##    Distance AirTime Speed
##       <int>   <int> <dbl>
##  1      224      40  336 
##  2      224      45  299.
##  3      224      48  280 
##  4      224      39  345.
##  5      224      44  305.
##  6      224      45  299.
##  7      224      43  313.
##  8      224      40  336 
##  9      224      41  328.
## 10      224      45  299.
## # ... with 227,486 more rows
```

```r
# creates new variable Speed that is a function of Distance and Airtime

# to add to existing dataframe, don't select the columns
flights <- flights %>%
  mutate(Speed = Distance/AirTime*60)
```

# 6.0 summarise()

- reduces variables to values, useful for data that has been grouped by one or more variables
- group_by() creates the groups that will be operated on, summarise() uses the provided aggregation function to summarise each group

```r
flights %>%
  group_by(Dest) %>%
  summarise(avg_delay = mean(ArrDelay, na.rm=TRUE))
```

```
## # A tibble: 116 x 2
##    Dest  avg_delay
##    <chr>     <dbl>
##  1 ABQ        7.23
##  2 AEX        5.84
##  3 AGS        4   
##  4 AMA        6.84
##  5 ANC       26.1 
##  6 ASE        6.79
##  7 ATL        8.23
##  8 AUS        7.45
##  9 AVL        9.97
## 10 BFL      -13.2 
## # ... with 106 more rows
```

```r
# create a table grouped by Dest, then summarise each group into avg_delay by taking the mean of ArrDelay
```

# Notes
- glimpse() lets you see the dataframe data better


```r
library(rgl)
yes <- flights %>% select(AirTime, Speed, Distance)
print(plot3d(yes))
```










