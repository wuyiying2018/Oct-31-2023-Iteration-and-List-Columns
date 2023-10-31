Iteration and List Columns
================
Yiying Wu
2023-10-31

## lists

``` r
vec_numeric = 5:8
vec_char = c("My", "name", "is", "Jeff")
vec_logical = c(TRUE, TRUE, TRUE, FALSE)
```

different length

``` r
l = list(
  vec_numeric = 5:8,
  mat         = matrix(1:8, 2, 4),
  vec_logical = c(TRUE, FALSE),
  summary     = summary(rnorm(1000)))
l
```

    ## $vec_numeric
    ## [1] 5 6 7 8
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8
    ## 
    ## $vec_logical
    ## [1]  TRUE FALSE
    ## 
    ## $summary
    ##      Min.   1st Qu.    Median      Mean   3rd Qu.      Max. 
    ## -2.970239 -0.649691  0.013026  0.002396  0.671716  2.823985

accessing lists

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]
```

    ## [1] 5 6 7 8

``` r
l[[1]][1:3]
```

    ## [1] 5 6 7

## `for` loops

``` r
list_norms = 
  list(
    a = rnorm(20, 3, 1),
    b = rnorm(20, 0, 5),
    c = rnorm(20, 10, .2),
    d = rnorm(20, -3, 1)
  )

is.list(list_norms)
```

    ## [1] TRUE

write function

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Argument x should be numeric")
  } else if (length(x) == 1) {
    stop("Cannot be computed for length 1 vectors")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)

  tibble(
    mean = mean_x, 
    sd = sd_x
  )
}
```

apply the mean_and_sd function to each element of list_norms using the
lines below.

``` r
mean_and_sd(list_norms[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.74  1.12

``` r
mean_and_sd(list_norms[[2]])
```

    ## # A tibble: 1 × 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0780  4.56

``` r
mean_and_sd(list_norms[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.1 0.195

``` r
mean_and_sd(list_norms[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.86 0.798

a for loop

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  output[[i]] = mean_and_sd(list_norms[[i]])
}
```

## use `map`

``` r
output = map(list_norms, mean_and_sd)
```

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  output[[i]] = median(list_norms[[i]])
}

output = map(list_norms, median)
```

## create dataframe

``` r
listcol_df = 
  tibble(
    name = c("a", "b", "c", "d"),
    samp = list_norms
  )
```

pull name col, pull samp col

``` r
listcol_df |> pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df |> pull(samp)
```

    ## $a
    ##  [1] 0.8161857 3.5368248 4.2731213 0.9074579 3.3901861 1.5532885 3.0883152
    ##  [8] 4.4103126 4.0068994 2.5742637 2.5665076 2.2095686 2.2828811 2.3632318
    ## [15] 3.7638788 3.2597847 1.3354703 1.8831821 4.2680895 2.2511981
    ## 
    ## $b
    ##  [1]  2.8494021  5.2987099 -4.7769445  0.5180710  7.9752042 -5.2848057
    ##  [7] -0.9879004 -2.9636070  1.4051456  4.8251367 -3.7693300 -7.0295014
    ## [13]  2.4209455  4.9733109 -2.7995237  3.3318268 -8.4915350  2.6460554
    ## [19] -2.1040061  3.5240038
    ## 
    ## $c
    ##  [1] 10.324316 10.266169 10.058938  9.660162 10.078031 10.010004 10.133266
    ##  [8] 10.011320 10.130280 10.007533  9.965896 10.043360 10.345411 10.035235
    ## [15] 10.009058  9.813120 10.460346  9.799476  9.857985 10.136525
    ## 
    ## $d
    ##  [1] -2.631023 -3.199504 -4.700160 -2.082669 -3.655905 -3.208587 -3.407313
    ##  [8] -3.569867 -1.225540 -1.878173 -2.878194 -2.278294 -2.164327 -2.398859
    ## [15] -2.085479 -2.885594 -2.830078 -3.780577 -3.370674 -2.882198

apply mean_and_sd to the first element of our list column.

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.74  1.12

apply mean_and_sd function to each element using map.

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.74  1.12
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 0.0780  4.56
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.1 0.195
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.86 0.798

using mutate to define a new variable in a data frame

``` r
listcol_df = 
  listcol_df |> 
  mutate(summary = map(samp, mean_and_sd))

listcol_df
```

    ## # A tibble: 4 × 3
    ##   name  samp         summary         
    ##   <chr> <named list> <named list>    
    ## 1 a     <dbl [20]>   <tibble [1 × 2]>
    ## 2 b     <dbl [20]>   <tibble [1 × 2]>
    ## 3 c     <dbl [20]>   <tibble [1 × 2]>
    ## 4 d     <dbl [20]>   <tibble [1 × 2]>

## Revisiting NSDUH

``` r
nsduh_table <- function(html, table_num) {
  
  table = 
    html |> 
    html_table() |> 
    nth(table_num) |>
    slice(-1) |> 
    select(-contains("P Value")) |>
    pivot_longer(
      -State,
      names_to = "age_year", 
      values_to = "percent") |>
    separate(age_year, into = c("age", "year"), sep = "\\(") |>
    mutate(
      year = str_replace(year, "\\)", ""),
      percent = str_replace(percent, "[a-c]$", ""),
      percent = as.numeric(percent)) |>
    filter(!(State %in% c("Total U.S.", "Northeast", "Midwest", "South", "West")))
  
  table
}
```

use this function to import three tables using the next code chunk,
which downloads and extracts the page HTML and then iterates over table
numbers. The results are combined using bind_rows(). Note that, because
this version of our function doesn’t include table_name, that
information is lost for now.

``` r
nsduh_url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

nsduh_html = read_html(nsduh_url)

output = vector("list", 3)

for (i in c(1, 4, 5)) {
  output[[i]] = nsduh_table(nsduh_html, i)
}

nsduh_results = bind_rows(output)
```

We can also import these data using map(). Here I’m supplying the html
argument after the name of the function that I’m iterating over.

``` r
nsduh_results = 
  map(c(1, 4, 5), nsduh_table, html = nsduh_html) |> 
  bind_rows()
```

As with previous examples, using a for loop is pretty okay but the map
call is clearer.

We can also do this using data frames and list columns.

``` r
nsduh_results= 
  tibble(
    name = c("marj", "cocaine", "heroine"),
    number = c(1, 4, 5)) |> 
  mutate(table = map(number, \(num) nsduh_table(html = nsduh_html, num))) |> 
  unnest(cols = "table")
```
