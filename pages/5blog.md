---
layout: page
title: Dates and times
description: This page is reserved for my 5th BLOG.
---

### Dates and times
Time is often used in the data and its formate can be various. In the present blog I will post about possible operations to work with dates and times using the library "lubridate".

```
library(lubridate)
```

Three types of date/time data that refer to an instant in time:

- A **date**. Tibbles print this as <date>.
- A **time** within a day. Tibbles print this as <time>.
- A **date-time** is a date plus a time: it uniquely identifies an instant in time (typically to the nearest second). Tibbles print this as <dttm>.

```
today()
now()

ymd("2017-01-31")
mdy("January 31st, 2017")
dmy("31-Jan-2017")

ymd_hms("2017-01-31 20:11:59")  # to create a date-time
mdy_hm("01/31/2017 08:01")

ymd(20170131, tz = "UTC")  # supplying timezone

as_datetime(today())
as_date(now())

parse_datetime("2010-10-01T2010")
parse_datetime("20101010")
```


#### 1. From individual components

```
flights %>%
    select(year, month, day, hour, minute) %>%
    mutate(departure = make_datetime(year, month, day, hour, minute))
```


#### 2. Getting & setting components

```
datetime <- ymd_hms("2016-07-08 12:34:56")

year(datetime)
month(datetime)
mday(datetime)
yday(datetime)
wday(datetime)

(datetime <- ymd_hms("2016-07-08 12:34:56"))

year(datetime) <- 2020
month(datetime) <- 01
hour(datetime) <- hour(datetime) + 1

ymd("2015-02-01") %>%
  update(mday = 30)
ymd("2015-02-01") %>%
  update(hour = 400)
```


#### 3. Time spans
- durations
- periods
- intervals

**Durations**:
```
as.duration(h_age)
dseconds(15)
dminutes(10)
dhours(c(12, 24))
ddays(0:5)
dweeks(3)
dyears(1)
```

**Periods**:
```
seconds(15)
minutes(10)
hours(c(12, 24))
days(7)
months(1:6)
weeks(3)
years(1)

10 * (months(6) + days(1))
days(50) + hours(25) + minutes(2)
```

**Intervals**. An interval is a duration with a starting point: that makes it precise so you can determine exactly how long it is:
```
next_year <- today() + years(1)
(today() %--% next_year) / ddays(1)
```


#### 4. Time zones

Unless otherwise specified, lubridate always uses UTC. UTC (Coordinated Universal Time) is the standard time zone used by the scientific community and roughly equivalent to its predecessor GMT (Greenwich Mean Time).

```
Sys.timezone()

x4a <- with_tz(x4, tzone = "Australia/Lord_Howe")
x4b <- force_tz(x4, tzone = "Australia/Lord_Howe")
```
