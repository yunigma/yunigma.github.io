---
layout: page
title: Data transformation and wrangling 1
description: This page is reserved for my 6th BLOG.
---

### Data transformation and wrangling 1

In this blog I will post about the basic operations to transfrom and observe the data. The data often have the form that does not fit exactly the research purposes and some modifications are needed.
Data wrangling video: <https://www.youtube.com/watch?v=85tud7I8MAE&list=PLXugMGL39JWO_n-WNdppkyMuYfQBU_h_U&index=6>


### 0. dplyr (tidyverse) functions

- Pick observations by their values **(filter())**.
- Reorder the rows **(arrange())**.
- Pick variables by their names **(select())**.
- Create new variables with functions of existing variables **(mutate())**.
- Modular arithmetic


### 1. Filter rows with filter()
To choose the rows with certain values.
```
filter(flights, month == 1, day == 1)
```

Computers use finite precision arithmetic (they obviously can’t store an infinite number of digits!) so remember that every number you see is an approximation. Instead of relying on ==, use near():
```
near(sqrt(2) ^ 2,  2)
near(1 / 49 * 49, 1)
```

#### Logical operators
```
filter(flights, month == 11 | month == 12)
nov_dec <- filter(flights, month %in% c(11, 12))
filter(flights, !(arr_delay > 120 | dep_delay > 120))
filter(flights, arr_delay <= 120, dep_delay <= 120)
```
**filter()** only includes rows where the condition is TRUE; it excludes both FALSE and NA values.


### 2. Arrange rows with arrange()
**arrange()** works similarly to filter() except that instead of selecting rows, it changes their order:
```
arrange(df, desc(x))

df %>%
  arrange(desc(x))
```
Aggregation functions obey the usual rule of missing values: if there’s any missing value in the input, the output will be a missing value. Fortunately, all aggregation functions have an `na.rm argument which removes the missing values prior to computation:


### 3. Select columns with select()

Use **everything()** to reorder columns.
To rename columns, use new_name=old_name.
```
select(flights, year:day)
select(flights, -(year:day))
select(flights, time_hour, air_time, everything())
```

There are a number of helper functions you can use within select():

- **starts_with("abc")**: matches names that begin with “abc”.

- **ends_with("xyz")**: matches names that end with “xyz”.

- **contains("ijk")**: matches names that contain “ijk”.

- **matches("(.)\\1")**: selects variables that match a regular expression. This one matches any variables that contain repeated characters. You’ll learn more about regular expressions in strings.

- **num_range("x", 1:3)**: matches x1, x2 and x3.


### 4. Add new variables with mutate()
**mutate()** always adds new columns at the end of your dataset.
```
flights_sml <- select(flights,
  year:day,
  ends_with("delay"),
  distance,
  air_time
)
mutate(flights_sml,
  gain = dep_delay - arr_delay,
  speed = distance / air_time * 60
)
```
Note that you can refer to columns that you’ve just created:
```
mutate(flights_sml,
  gain = dep_delay - arr_delay,
  hours = air_time / 60,
  gain_per_hour = gain / hours
)
```
If you only want to keep the new variables, use transmute():
```
transmute(flights,
  gain = dep_delay - arr_delay,
  hours = air_time / 60,
  gain_per_hour = gain / hours
)
```

### 5. Useful creation functions
**Modular arithmetic**: %/% (integer division) and %% (remainder), where x == y * (x %/% y) + (x %% y). Modular arithmetic is a handy tool because it allows you to break integers up into pieces. For example, in the flights dataset, you can compute hour and minute from dep_time with:
```
transmute(flights,
  dep_time,
  hour = dep_time %/% 100,
  minute = dep_time %% 100
)
#> # A tibble: 336,776 x 3
#>   dep_time  hour minute
#>      <int> <dbl>  <dbl>
#> 1      517     5     17
#> 2      533     5     33
#> 3      542     5     42
#> 4      544     5     44
#> 5      554     5     54
#> 6      554     5     54
#> # … with 3.368e+05 more rows
```
Ranking:
```
y <- c(1, 2, 2, NA, 3, 4)
min_rank(y)
#> [1]  1  2  2 NA  4  5
min_rank(desc(y))
#> [1]  5  3  3 NA  2  1
row_number(y)
#> [1]  1  2  3 NA  4  5
dense_rank(y)
#> [1]  1  2  2 NA  3  4
percent_rank(y)
#> [1] 0.00 0.25 0.25   NA 0.75 1.00
cume_dist(y)
#> [1] 0.2 0.6 0.6  NA 0.8 1.0
```
Conditions:
```
data %>%
  mutate(error_category = if_else(error < 3, "low", "high"))

data %>%
  mutate(time_category = case_when(
    time < 13             ~ "fast",
    between(time, 13, 17) ~ "medium",
    TRUE                  ~ "low"))
```
