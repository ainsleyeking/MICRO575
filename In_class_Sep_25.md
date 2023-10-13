# In_class_Sep_25

1.  Use read_csv() to read the data into a data frame.

``` r
eagle_nest_counts <- read.csv("fake_eagle_nest_counts.csv")

library(tidyverse)
```

    ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ✔ dplyr     1.1.3     ✔ readr     2.1.4
    ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ✔ ggplot2   3.4.3     ✔ tibble    3.2.1
    ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
    ✔ purrr     1.0.2     
    ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ✖ dplyr::filter() masks stats::filter()
    ✖ dplyr::lag()    masks stats::lag()
    ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

2.  Would you describe this as a “tidy” data frame? Why or why not?

This is not a tidy data frame, because they have each different region
in its own column. Tidy data should have one column be one variable. In
this instance, “region” should be one variable, and therefore, one
column.

3.  If not, “pivot” it to be a tidy data frame. Remember to use the
    names_to and values_to arguments to generate a new data frame that
    has logical column names.

``` r
eagles_longer <- eagle_nest_counts |>
  pivot_longer(cols = !starts_with("year"),
               names_to = "region",
               values_to = "nests")
```

4.Make a scatter plot of the resulting data, showing the number of
eagles’ nests in each region by year. Add a linear trend line using
geom_smooth(method=“lm”).

``` r
ggplot(data = eagles_longer, mapping = aes(x = year, y = nests)) +
  geom_point(mapping = aes(color = region)) +
  geom_smooth(method = "lm", color="black") +
  scale_color_brewer(palette = "Dark2")
```

![](In_class_Sep_25_files/figure-commonmark/unnamed-chunk-3-1.png)

``` r
library(tidyr)
```
