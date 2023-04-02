---
layout: page
title: R basics and data visualisation
description: This page is reserved for my 3d BLOG.
---

In fact, this is my first steps in learning R and I have created a separate blog to cover some basic terms like factors, data types, pipes, and a basic template for visualisation.

### 1. Type of the data
- **int** stands for integers: **as.numeric()**

- **dbl** stands for doubles, or real numbers.

- **chr** stands for character vectors, or strings.

- **dttm** stands for date-times (a date + a time).

- **lgl** stands for logical, vectors that contain only TRUE or FALSE.

- **fctr** stands for factors, which R uses to represent categorical variables with fixed possible values.

- **date** stands for dates.


### 2. Factors
```
y1 <- factor(x1, levels = month_levels)
y1
#> [1] Dec Apr Jan Mar
#> Levels: Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec
```

**fct_reorder()** takes three arguments:

- f, the factor whose levels you want to modify.
- x, a numeric vector that you want to use to reorder the levels.
- Optionally, fun, a function that’s used if there are multiple values of x for each value of f. The default value is median.

```
by_age <- gss_cat %>%
  filter(!is.na(age)) %>%
  count(age, marital) %>%
  group_by(age) %>%
  mutate(prop = n / sum(n))

fct_infreq() -- to order levels in increasing frequency.

fct_lump(relig, n = 10) --  to progressively lump together n of the smallest groups, ensuring that the aggregate is still the smallest group.
```

### 3. Pipes
Behind the scenes, x %>% f(y) turns into f(x, y), and x %>% f(y) %>% g(z) turns into g(f(x, y), z) and so on.

`
library(magrittr)
`
Instead of
```
bop(
  scoop(
    hop(foo_foo, through = forest),
    up = field_mice
  ),
  on = head
)
```
use:
```
foo_foo %>%
  hop(through = forest) %>%
  scoop(up = field_mice) %>%
  bop(on = head)
```
The pipe won’t work for two classes of functions:

- functions that use the current environment (e.g. assign(), get() and load()).
- functions that use lazy evaluation (tryCatch(stop("!"), error = function(e) "An error")).

 %T>% works like %>% except that it returns the left-hand side instead of the right-hand side.

Another example:
```
by_dest <- group_by(flights, dest)
delay <- summarise(by_dest,
  count = n(),
  dist = mean(distance, na.rm = TRUE),
  delay = mean(arr_delay, na.rm = TRUE)
)
delay <- filter(delay, count > 20, dest != "HNL")

delays <- flights %>%
  group_by(dest) %>%
  summarise(
    count = n(),
    dist = mean(distance, na.rm = TRUE),
    delay = mean(arr_delay, na.rm = TRUE)
  ) %>%
  filter(count > 20, dest != "HNL")
  ```


### 4. Data visualisation
```
ggplot(data = <DATA>) +
  <GEOM_FUNCTION>(
     mapping = aes(<MAPPINGS>),
     stat = <STAT>,
     position = <POSITION>
  ) +
  <COORDINATE_FUNCTION> +
  <FACET_FUNCTION>
```
The use of **mutate()** function for better visualisation:
```
# define the desired order of the levels
data %>%
  mutate(device = factor(device, levels=c("Trackpad", "Mouse"))) %>%
  ggplot(aes(x = device, y = time)) +
  geom_point() +
  coord_flip()

# alternatively fct_rev() for the reverse order can be used
data %>%
  mutate(device = fct_rev(factor(device))) %>%
  ggplot(aes(x = device, y = time)) +
  geom_point() +
  coord_flip()
```

Useful links:
<https://r4ds.had.co.nz/data-visualisation.html>
The Chartmaker Directory:
<http://chartmaker.visualisingdata.com>


### 5. Cheat sheet "BASICS"
![Picture](images/r_cheat_sheet_basics1.jpg)

![Picture](images/r_cheat_sheet_basics2.jpg)
