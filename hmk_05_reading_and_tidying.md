# HMK 5: Reading and tidying data

# Reading

- [R4DS Chapters 6-9](https://r4ds.hadley.nz/data-import)

# Data import

## Q1:

- Create a directory, within your main class directory, called `data`.
  - Note: in general, you should store raw data in a directory called
    `data`.
- Download the example file for Ch 9
  [here](https://pos.it/r4ds-students-csv). Save it inside the `ddata`
  directory.
- Use `read_csv()` to read the file to an R data frame. Follow the
  instructions in Ch 9 to format it properly. Follow the directions in
  Ch 9 to make sure the following are true:
  - Column names should be *syntactic*, meaning they don’t contain
    spaces.
  - N/A values should be represented with the R value `NA`, not the
    character “N/A”.
  - Data types (character vs factor vs numeric) should be appropriate.

``` r
library(tidyverse)

sample <- read_csv("data/sample_ch9.csv")
```

## Q2

Find (or make) a data file of your own, in text format. Read it into a
well-formatted data frame.

``` r
# Original format
growth_curve <- read_csv("data/growth_curve.csv")
glimpse(growth_curve)
```

    Rows: 24
    Columns: 6
    $ hours                                <dbl> 0, 2, 4, 6, 8, 10, 12, 24, 0, 2, …
    $ replicate                            <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 2, 2, 2, …
    $ wild_type                            <dbl> 0.100, 0.187, 0.634, 2.423, 6.442…
    $ hyperactive_ste11                    <dbl> 0.100, 0.199, 0.584, 1.763, 4.697…
    $ xog1_overexpressor                   <dbl> 0.100, 0.220, 0.661, 2.387, 6.516…
    $ hyperactive_ste11_xog1_overexpressor <dbl> 0.100, 0.198, 0.547, 1.730, 4.839…

``` r
growth_curve_tidy <- growth_curve |>
  pivot_longer(cols = wild_type:hyperactive_ste11_xog1_overexpressor,
    names_to = "strain",
    values_to = "OD600"
  )

glimpse(growth_curve_tidy)
```

    Rows: 96
    Columns: 4
    $ hours     <dbl> 0, 0, 0, 0, 2, 2, 2, 2, 4, 4, 4, 4, 6, 6, 6, 6, 8, 8, 8, 8, …
    $ replicate <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, …
    $ strain    <chr> "wild_type", "hyperactive_ste11", "xog1_overexpressor", "hyp…
    $ OD600     <dbl> 0.100, 0.100, 0.100, 0.100, 0.187, 0.199, 0.220, 0.198, 0.63…

# Tidying

Download the data set available at
<https://tiny.utk.edu/MICR_575_hmk_05>, and save it to your `data`
folder. **This is a real data set:** it is the output from the
evaluation forms for student colloquium speakers in the Microbiology
department. I have eliminated a few columns, changed some of the scores,
and edited comments, to protect student privacy, but the output is real.

First, open this .csv file with Microsoft Excel or a text reading app,
to get a sense of the structure of the document. It is weird.

Why is the file formatted so inconveniently? I have no idea, but I do
know that this is about an average level of inconvenient formatting for
real data sets you will find in the wild.

*Note: In theory, you can pass a URL to `read_csv()` and read the file
directly from the internet. In practice, that doesn’t seem to work for
this file. So you’ll want to download this file to your hard drive.*

## Q3a

Next, use `read_csv()` to read the data into a data frame. Note that
you’ll need to make use of some of the optional arguments. Use
`?read_csv` to see what they are.

*If you are struggling with this task, email me for hints.*

As we discussed in class, the correct shape depends on what you want to
do with the data. Use `pivot_longer()` to make the data frame longer, in
a way that makes sense.

``` r
colloquium_assessment <- read_csv("data/colloquium_assessment.csv")

# Remove the first four rows
ca_correct_start <- colloquium_assessment |>
  filter(!row_number() %in% c(1:4))

# Select out the odd rows
row_odd <- seq_len(nrow(ca_correct_start)) %% 2
ca_no_na_rows <- ca_correct_start[row_odd == 0, ]
  
# Remove empty columns
ca_no_na_columns <- subset(ca_no_na_rows, select = -c(RecipientLastName:ExternalReference))

# Pivot
ca_tidy <- ca_no_na_columns |>
  pivot_longer(cols = starts_with("Q"),
               names_to = "question",
               values_to = "rating")

glimpse(ca_tidy)
```

    Rows: 144
    Columns: 14
    $ StartDate               <chr> "11/11/22 9:01", "11/11/22 9:01", "11/11/22 9:…
    $ EndDate                 <chr> "11/11/22 9:02", "11/11/22 9:02", "11/11/22 9:…
    $ Status                  <chr> "0", "0", "0", "0", "0", "0", "0", "0", "0", "…
    $ Progress                <chr> "100", "100", "100", "100", "100", "100", "100…
    $ `Duration (in seconds)` <chr> "18", "18", "18", "18", "18", "18", "18", "18"…
    $ Finished                <chr> "1", "1", "1", "1", "1", "1", "1", "1", "1", "…
    $ RecordedDate            <chr> "11/11/22 9:02", "11/11/22 9:02", "11/11/22 9:…
    $ ResponseId              <chr> "R_2b14qToYhcmCVFD", "R_2b14qToYhcmCVFD", "R_2…
    $ LocationLatitude        <chr> "35.9539", "35.9539", "35.9539", "35.9539", "3…
    $ LocationLongitude       <chr> "-83.9357", "-83.9357", "-83.9357", "-83.9357"…
    $ DistributionChannel     <chr> "anonymous", "anonymous", "anonymous", "anonym…
    $ UserLanguage            <chr> "EN", "EN", "EN", "EN", "EN", "EN", "EN", "EN"…
    $ question                <chr> "Q4", "Q5", "Q6", "Q7", "Q8", "Q9", "Q10", "Q1…
    $ rating                  <chr> "1", "2", "4", "5", "5", "4", "4", "4", NA, "1…

## Q3b

Finally, calculate this student’s average score for each of questions
7-10.

``` r
#Filter for only the questions we want
q710 <- ca_tidy |>
  filter(question == "Q7" | question == "Q8" | question == "Q9" | question == "Q10") |>
  arrange(question)

#Convert to numbers instead of characters
q710_int <- transform(q710, rating = as.numeric(q710$rating))

# Find the average
average <- summarise( q710_int, 
    average.score = mean(rating),
    .by = question)

glimpse(average)
```

    Rows: 4
    Columns: 2
    $ question      <chr> "Q10", "Q7", "Q8", "Q9"
    $ average.score <dbl> 4.4375, 4.5000, 4.6250, 4.3125

## Important note about file paths in Quarto documents

When you render a Quarto document, RStudio spins up a new instance of R,
which is separate from the instance of R that you cna interact with. The
working directory for this instance of R is whatever directory your
Quarto document is saved in.

If your quarto document is saved in the same directory as your RStudio
project (e.g., `MICR_475`), then there is no difference between your
interactive working directory and the working directory for your Quarto
document.

However, if your homeworks are saved in a `HMK` directory, then the
Quarto working directory will be `HMK`. To access the saved `.csv` file,
`read_csv()` will need to look *up* one directory and then go back
*down* into `HMK`. `..` means “up one directory”, so you would need to
use `read_csv("../colloquium_assessment.csv")` instead of
`read_csv("colloquium_assessment.csv")`.
