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

## Learning Assessment 2

``` r
filter(pups_data,sex == 1)
```

    ## # A tibble: 155 x 6
    ##   litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##   <chr>         <int>   <int>   <int>    <int>   <int>
    ## 1 #85               1       4      13        7      11
    ## 2 #85               1       4      13        7      12
    ## 3 #1/2/95/2         1       5      13        7       9
    ## # … with 152 more rows

``` r
filter(pups_data, pd_walk < 11, sex == 2)
```

    ## # A tibble: 127 x 6
    ##   litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##   <chr>         <int>   <int>   <int>    <int>   <int>
    ## 1 #1/2/95/2         2       4      13        7       9
    ## 2 #1/2/95/2         2       4      13        7      10
    ## 3 #1/2/95/2         2       5      13        8      10
    ## # … with 124 more rows

## Mutate

``` r
mutate(
  litters_data,
  wt_gain = gd18_weight -  gd0_weight,
  group = str_to_lower(group))
```

    ## # A tibble: 49 x 9
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 con7  #85                 19.7        34.7          20               3
    ## 2 con7  #1/2/95/2           27          42            19               8
    ## 3 con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # … with 46 more rows, and 3 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>, wt_gain <dbl>

## Learning Assessment 3

``` r
mutate(pups_data, pivot_minus7 = pd_pivot - 7)
```

    ## # A tibble: 313 x 7
    ##   litter_number   sex pd_ears pd_eyes pd_pivot pd_walk pivot_minus7
    ##   <chr>         <int>   <int>   <int>    <int>   <int>        <dbl>
    ## 1 #85               1       4      13        7      11            0
    ## 2 #85               1       4      13        7      12            0
    ## 3 #1/2/95/2         1       5      13        7       9            0
    ## # … with 310 more rows

``` r
mutate(pups_data, pd_sum = pd_ears + pd_eyes + pd_pivot + pd_walk)
```

    ## # A tibble: 313 x 7
    ##   litter_number   sex pd_ears pd_eyes pd_pivot pd_walk pd_sum
    ##   <chr>         <int>   <int>   <int>    <int>   <int>  <int>
    ## 1 #85               1       4      13        7      11     35
    ## 2 #85               1       4      13        7      12     36
    ## 3 #1/2/95/2         1       5      13        7       9     34
    ## # … with 310 more rows

## Arrange

``` r
arrange(litters_data,pups_born_alive)
```

    ## # A tibble: 49 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Low7  #111                25.5        44.6          20               3
    ## 3 Low8  #4/84               21.8        35.2          20               4
    ## # … with 46 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

``` r
arrange(litters_data, desc(pups_born_alive))   ## descending
```

    ## # A tibble: 49 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Low7  #102                22.6        43.3          20              11
    ## 2 Mod8  #5/93               NA          41.1          20              11
    ## 3 Con7  #1/5/3/83/3-…       NA          NA            20               9
    ## # … with 46 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

``` r
arrange(litters_data,pups_born_alive, gd0_weight)
```

    ## # A tibble: 49 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Low7  #111                25.5        44.6          20               3
    ## 3 Low8  #4/84               21.8        35.2          20               4
    ## # … with 46 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   pups_survive <int>

## use %\>%

``` r
litters_data = 
  read_csv("./data/FAS_litters.csv",col_types = "ccddiiii") %>%
  janitor::clean_names() %>% 
  select(-pups_survive) %>% 
  mutate(
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group)) %>% 
  drop_na(gd0_weight) ## know the data 


litters_data = 
  read_csv("./data/FAS_litters.csv",col_types = "ccddiiii") %>%
  janitor::clean_names() %>% 
  select(-pups_survive) %>% 
  mutate(
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group)) %>% 
  drop_na(gd0_weight) ## know the data 

litters_data %>%
  lm(wt_gain ~ pups_born_alive, data = .) %>%
  broom::tidy()
```

    ## # A tibble: 2 x 5
    ##   term            estimate std.error statistic  p.value
    ##   <chr>              <dbl>     <dbl>     <dbl>    <dbl>
    ## 1 (Intercept)       13.1       1.27      10.3  3.39e-11
    ## 2 pups_born_alive    0.605     0.173      3.49 1.55e- 3

``` r
litters_data %>% view()
litters_data %>% pull(gd0_weight) %>% mean
```

    ## [1] 24.37941

Learning Assessment: Write a chain of commands that:

loads the pups data cleans the variable names filters the data to
include only pups with sex 1 removes the PD ears variable creates a
variable that indicates whether PD pivot is 7 or more days

## Learning Assessment 4

``` r
pups_data =
  read_csv("./data/FAS_pups.csv",  col_types = "ciiiii") %>% 
  janitor::clean_names() %>% 
  filter(sex == 1) %>% 
  select( -pd_ears) %>% 
  mutate(var_pdv_7 = pd_pivot > 7)
```
