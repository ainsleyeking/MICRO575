# Homework 03

# Base R and R Basics

HINT: Remember that you can get help on any function by typing
`?`(function name). For instance, `?rnorm` gives help on the `rnorm()`
function.

## Creating and naming variables

1.  Create a variable called `x` and use it to store the result of the
    calculation `(3*(4+2)`.

    x \<- (3\*(4+2))

2.  Calculate the product of `x` (from the above question) times π.

    x\*pi

3.  Use the `getwd()` function to show your current working directory.
    Is that a good working directory, and what program do you think set
    it that way?

    getwd()

## Vectors

1.  Use the `c()` function to create a vector of numbers.

``` r
vector.of.numbers <- c(2.1, 3.7, 9.9, 5,0, 4.8)
```

2.  Use the `c()` function to create a vector of characters.

``` r
vector.of.characters <- c("a", "b", "c", "d", "e")
```

3.  Use the `:` implicit function to create a vector of integers from 1
    to 10.

``` r
    vector.of.integers <- 1:10
```

4.  Explain *why* the following code returns what it does. Also address
    whether you think this was a good decision on the part of the
    designers of R?

``` r
v1 <- 1:3
v2 <- c(1:4)
v1 + v2
```

    [1] 2 4 6 5

This code is adding the two vectors v1 and v2 together. However, v1 only
has 3 numbers in it, while v2 has four. R deals with this by looping
back around to the beginning of v1 and adding the last number of v2 to
first number of v1. I do not think it was a good decision to design R
this way, as it may prevent you from catching a mistake where your
vectors contain different numbers of elements when they aren’t supposed
to. I think it would be better if R showed an error message when you
attempt this.

5.  Explain what the following code does. It may be helpful to reference
    the answer to the previous question:

``` r
c(1, 5, 9) + 3
```

    [1]  4  8 12

    This code takes each of the numbers in the vector and adds 3 to it. It returns a new vector with the resulting numbers.

6.  Remove (delete) every variable in your workspace.

``` r
rm(list=ls())
```

## Graphics

1.  Load the tidyverse package. **NOTE:** Be sure to use the chunk
    option `message=FALSE` to suppress the messages that tidyverse
    prints when loaded. These messages are useful in the

``` r
library(tidyverse)
library(palmerpenguins)
```

2.  Recreate the visualization of `body_mass_g` to `flipper_length_mm`,
    from the penguins data set, that is shown in question 8 of section
    2.2.5 of [R4DS](https://r4ds.hadley.nz/data-visualize).

``` r
    ggplot(data=penguins, mapping = aes(x=flipper_length_mm, y=body_mass_g ) ) + geom_point(mapping = aes(color=bill_depth_mm)) + geom_smooth()
```

    `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    Warning: Removed 2 rows containing non-finite values (`stat_smooth()`).

    Warning: Removed 2 rows containing missing values (`geom_point()`).

![](hmk_03_files/figure-commonmark/unnamed-chunk-8-1.png)

3.  Explain why each aesthetic is mapped at the level that it is (i.e.,
    at the global level, in the `ggplot()` function call, or at the geom
    level, in the `geom_XXX()` function call). Note: A lot of different
    options will work, but some options are clearly better than others.

    When you want settings to apply to everything in the plot, including
    the subsequent geom() functions (i.e. the x and y axes), you should
    map those things in the ggplot() function. If you only want it to
    apply to one of the geom() funtions, you have to map it in that
    function. For example, we want the color in the previous question to
    apply to the points, but not the line, so we chose the color in the
    geom_points() function.
