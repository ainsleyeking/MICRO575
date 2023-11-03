# HMK 08

# Joins

1.  Imagine you’ve found the top 10 most popular destinations using this
    code:

``` r
library(tidyverse)
library(nycflights13)

top_dest <- flights |>
  count(dest, sort = TRUE) |>
  head(10)
```

How can you find all flights to those destinations?

``` r
library(tidyverse)
library(nycflights13)

# Find flight numbers
flight_numbers <- flights |>
  select(flight, dest) |>
  filter(!is.na(flight))

# Join
all_flights <- top_dest |>
  left_join(flight_numbers, join_by(dest))
```

# Functions

2.  Write a function to ‘rescale’ a numeric vector by subtracting the
    mean of the vector from each element and then dividing each element
    by the standard deviation.

``` r
#Make vector
num_vector <- c(25, 12, 48, 19, 37, 42, 29)

# Rescale function
rescale <- function(x) {
  ((x - mean(x)) / sd(x))
}

# Rescale vector
rescale(num_vector)
```

    [1] -0.4120277 -1.4253933  1.3808497 -0.8797349  0.5233866  0.9131426 -0.1002230
