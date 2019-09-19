Untitled
================
Weijia Xiong
9/19/2019

## Import datasets

``` r
options(tibble.print_min = 3)

litters_data = read_csv("./data/FAS_litters.csv",
  col_types = "ccddiiii")
litters_data = janitor::clean_names(litters_data)

pups_data = read_csv("./data/FAS_pups.csv",
  col_types = "ciiiii")
pups_data = janitor::clean_names(pups_data)
```

## Selecting

``` r
select(litters_data, group, litter_number, gd0_weight, pups_born_alive)
```

    ## # A tibble: 49 x 4
    ##   group litter_number gd0_weight pups_born_alive
    ##   <chr> <chr>              <dbl>           <int>
    ## 1 Con7  #85                 19.7               3
    ## 2 Con7  #1/2/95/2           27                 8
    ## 3 Con7  #5/5/3/83/3-3       26                 6
    ## # … with 46 more rows

``` r
select(litters_data, group:gd_of_birth)
```

    ## # A tibble: 49 x 5
    ##   group litter_number gd0_weight gd18_weight gd_of_birth
    ##   <chr> <chr>              <dbl>       <dbl>       <int>
    ## 1 Con7  #85                 19.7        34.7          20
    ## 2 Con7  #1/2/95/2           27          42            19
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19
    ## # … with 46 more rows

``` r
select(litters_data, gd0_weight,starts_with("pups"))
```

    ## # A tibble: 49 x 4
    ##   gd0_weight pups_born_alive pups_dead_birth pups_survive
    ##        <dbl>           <int>           <int>        <int>
    ## 1       19.7               3               4            3
    ## 2       27                 8               0            7
    ## 3       26                 6               0            5
    ## # … with 46 more rows

``` r
select(litters_data,litter_number, group, gd0_weight)
```

    ## # A tibble: 49 x 3
    ##   litter_number group gd0_weight
    ##   <chr>         <chr>      <dbl>
    ## 1 #85           Con7        19.7
    ## 2 #1/2/95/2     Con7        27  
    ## 3 #5/5/3/83/3-3 Con7        26  
    ## # … with 46 more rows

``` r
select(litters_data,litter_number,group,everything())  ## everything and put litter_number first.
```

    ## # A tibble: 49 x 8
    ##   litter_number group gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr>         <chr>      <dbl>       <dbl>       <int>           <int>
    ## 1 #85           Con7        19.7        34.7          20               3
    ## 2 #1/2/95/2     Con7        27          42            19               8
    ## 3 #5/5/3/83/3-3 Con7        26          41.4          19               6
    ## # … with 46 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

``` r
select(litters_data,-group) ## remove
```

    ## # A tibble: 49 x 7
    ##   litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 #85                 19.7        34.7          20               3
    ## 2 #1/2/95/2           27          42            19               8
    ## 3 #5/5/3/83/3-3       26          41.4          19               6
    ## # … with 46 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

``` r
litters_selected  = select(litters_data,litter_number, gd0_weight:pups_born_alive)  # didn't change litters_data


select(litters_data, GROUP = group, litter_number)  ## rename & select variables
```

    ## # A tibble: 49 x 2
    ##   GROUP litter_number
    ##   <chr> <chr>        
    ## 1 Con7  #85          
    ## 2 Con7  #1/2/95/2    
    ## 3 Con7  #5/5/3/83/3-3
    ## # … with 46 more rows

``` r
rename(litters_data,GROUP = group) ##just rename 
```

    ## # A tibble: 49 x 8
    ##   GROUP litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # … with 46 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

``` r
##starts_with(), ends_with(), and contains() 
```

## Learning Assessment

``` r
select(pups_data,litter_number,sex,pd_ears)
```

    ## # A tibble: 313 x 3
    ##   litter_number   sex pd_ears
    ##   <chr>         <int>   <int>
    ## 1 #85               1       4
    ## 2 #85               1       4
    ## 3 #1/2/95/2         1       5
    ## # … with 310 more rows

## Filter

``` r
filter(litters_data,group == "Con7") ##check xxx is equal to xxx? when this is true, keeps ...
```

    ## # A tibble: 7 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## 4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ## 5 Con7  #4/2/95/3-3         NA          NA            20               6
    ## 6 Con7  #2/2/95/3-2         NA          NA            20               6
    ## 7 Con7  #1/5/3/83/3-…       NA          NA            20               9
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
filter(litters_data,gd_of_birth == 20)
```

    ## # A tibble: 32 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #4/2/95/3-3         NA          NA            20               6
    ## 3 Con7  #2/2/95/3-2         NA          NA            20               6
    ## # … with 29 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

``` r
filter(litters_data, pups_born_alive >= 2)
```

    ## # A tibble: 49 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # … with 46 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

``` r
filter(litters_data, pups_born_alive == 6, group == "Con7")
```

    ## # A tibble: 3 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #5/5/3/83/3-3         26        41.4          19               6
    ## 2 Con7  #4/2/95/3-3           NA        NA            20               6
    ## 3 Con7  #2/2/95/3-2           NA        NA            20               6
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
filter(litters_data, pups_born_alive == 6 & group == "Con7")
```

    ## # A tibble: 3 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #5/5/3/83/3-3         26        41.4          19               6
    ## 2 Con7  #4/2/95/3-3           NA        NA            20               6
    ## 3 Con7  #2/2/95/3-2           NA        NA            20               6
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
filter(litters_data, group %in% c("Con7", "Con8"))
```

    ## # A tibble: 15 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2           27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3         NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2         NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-…       NA          NA            20               9
    ##  8 Con8  #3/83/3-3           NA          NA            20               9
    ##  9 Con8  #2/95/3             NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95         28.5        NA            20               8
    ## 11 Con8  #5/4/3/83/3         28          NA            19               9
    ## 12 Con8  #1/6/2/2/95-2       NA          NA            20               7
    ## 13 Con8  #3/5/3/83/3-…       NA          NA            20               8
    ## 14 Con8  #2/2/95/2           NA          NA            19               5
    ## 15 Con8  #3/6/2/2/95-3       NA          NA            20               7
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
filter(litters_data, group == "Con7" | group == "Con8")
```

    ## # A tibble: 15 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #85                 19.7        34.7          20               3
    ##  2 Con7  #1/2/95/2           27          42            19               8
    ##  3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  4 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  5 Con7  #4/2/95/3-3         NA          NA            20               6
    ##  6 Con7  #2/2/95/3-2         NA          NA            20               6
    ##  7 Con7  #1/5/3/83/3-…       NA          NA            20               9
    ##  8 Con8  #3/83/3-3           NA          NA            20               9
    ##  9 Con8  #2/95/3             NA          NA            20               8
    ## 10 Con8  #3/5/2/2/95         28.5        NA            20               8
    ## 11 Con8  #5/4/3/83/3         28          NA            19               9
    ## 12 Con8  #1/6/2/2/95-2       NA          NA            20               7
    ## 13 Con8  #3/5/3/83/3-…       NA          NA            20               8
    ## 14 Con8  #2/2/95/2           NA          NA            19               5
    ## 15 Con8  #3/6/2/2/95-3       NA          NA            20               7
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
filter(litters_data,pups_born_alive >= 4, pups_born_alive <= 6)
```

    ## # A tibble: 12 x 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ##  1 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  2 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  3 Con7  #4/2/95/3-3         NA          NA            20               6
    ##  4 Con7  #2/2/95/3-2         NA          NA            20               6
    ##  5 Con8  #2/2/95/2           NA          NA            19               5
    ##  6 Mod7  #1/82/3-2           NA          NA            19               6
    ##  7 Mod7  #3/82/3-2           28          45.9          20               5
    ##  8 Mod7  #5/3/83/5-2         22.6        37            19               5
    ##  9 Mod7  #106                21.7        37.8          20               5
    ## 10 Low7  #112                23.9        40.5          19               6
    ## 11 Low8  #4/84               21.8        35.2          20               4
    ## 12 Low8  #99                 23.5        39            20               6
    ## # … with 2 more variables: pups_dead_birth <int>, pups_survive <int>

``` r
filter(litters_data,gd0_weight + gd18_weight > 50)
```

    ## # A tibble: 31 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # … with 28 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

``` r
## missing data
# don't do this 
# filter(litters_data,!is.na(gd0_weight))

drop_na(litters_data, gd0_weight)  ##remove rows
```

    ## # A tibble: 34 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # … with 31 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>
