---
layout: page
title: Data transformation and wrangling 2
description: This page is reserved for my 8th BLOG.
---

### Data transformation and wrangling 2
The second part of data wrangling topic to cover more functions to organise the data.

### 0. tidyverse unctions

- operations on the combining and splitting the columns: **separate()** / **unite()**
- joining and combining tables: **bind_cols()** / **bind_rows()** / **_join()**
- modifying the shape of tables: **spread()** / **gather()**


### 1. Modify columns
#### separate() function

...pulls apart one column into multiple columns, by splitting wherever a separator character appears:
```
data %>%
    separate(user_device, c("user", "device"))
```
#### unite() function
is the inverse of separate(): it combines multiple columns into a single column:
```
data %>%
    unite(user_device, user, device, sep="-")
```

### 2. Combine tables

```
bind_cols(data_left, data_right)
bind_rows(data_01, data_02)
```
**JOIN** finctions:

An **inner join** matches pairs of observations whenever their keys are equal:
```
inner_join(data_time, data_error, by=c("user", "device"))
```
An **outer join** keeps observations that appear in at least one of the tables; all empty cells from the other table will be substituted by "NA". There are three types of outer joins:
- A **left_join()** keeps all observations in x.
- A **right_join()** keeps all observations in y.
- A **full_join()** keeps all observations in x and y.


### 3. Change the shape of the tables

**spread()**
```
table2
#> # A tibble: 12 x 4
#>   country      year type           count
#>   <chr>       <int> <chr>          <int>
#> 1 Afghanistan  1999 cases            745
#> 2 Afghanistan  1999 population  19987071
#> 3 Afghanistan  2000 cases           2666
#> 4 Afghanistan  2000 population  20595360
#> 5 Brazil       1999 cases          37737
#> 6 Brazil       1999 population 172006362
#> # … with 6 more rows

table2 %>%
    spread(key = type, value = count)
#> # A tibble: 6 x 4
#>   country      year  cases population
#>   <chr>       <int>  <int>      <int>
#> 1 Afghanistan  1999    745   19987071
#> 2 Afghanistan  2000   2666   20595360
#> 3 Brazil       1999  37737  172006362
#> 4 Brazil       2000  80488  174504898
#> 5 China        1999 212258 1272915272
#> 6 China        2000 213766 1280428583
```
**gather()**

```
table4a %>%
  gather(`1999`, `2000`, key = "year", value = "cases")
#> # A tibble: 6 x 3
#>   country     year   cases
#>   <chr>       <chr>  <int>
#> 1 Afghanistan 1999     745
#> 2 Brazil      1999   37737
#> 3 China       1999  212258
#> 4 Afghanistan 2000    2666
#> 5 Brazil      2000   80488
#> 6 China       2000  213766
```
