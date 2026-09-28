---
layout: page
title: Aggregation and summarizing
description: This page is reserved for my 9th BLOG.
---

### Aggregation and summarizing

### 1. Grouped summaries with summarise()
Collapse many values down to a single summary **(summarise())**.

```
summarise(flights, delay = mean(dep_delay, na.rm = TRUE))
```

With **summarise()** one can use aggregate functions.
When you group by multiple variables, each summary peels off one level of the grouping. That makes it easy to progressively roll up a dataset:

```
daily <- group_by(flights, year, month, day)
(per_day   <- summarise(daily, flights = n()))
(per_month <- summarise(per_day, flights = sum(flights)))
(per_year  <- summarise(per_month, flights = sum(flights)))
```


If you need to remove grouping, and return to operations on ungrouped data, use ungroup():
```
daily %>%
  ungroup() %>%             # no longer grouped
  summarise(flights = n())
```

### 2. Example from the project
Task: filtering participants whose average error on the validation stimuli was more than 0.2
```
# check the validation stimuli and delete those participants whose average error exceeds 0.2
validation_stimuli <- results %>% filter(isValidation == 1) %>% group_by(Participant) %>% summarise(mean = mean(Error))
validation_stimuli_passed <- filter(validation_stimuli, mean < 0.2)
filtered_res <- results %>% filter(isValidation == 0) %>% inner_join(validation_stimuli_passed, by="Participant")
```
