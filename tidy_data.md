data tidying
================
Weijia Xiong
9/24/2019

## Wide to long

``` r
pulse_data = haven::read_sas("./data/public_pulse_data.sas7bdat") %>%
  janitor::clean_names()

pulse_tidy_data = 
  pivot_longer(
    pulse_data, 
    bdi_score_bl:bdi_score_12m,
    names_to = "visit", 
    names_prefix = "bdi_score_",
    values_to = "bdi") %>% 
  mutate(
    visit = recode(visit, "00m" = "bl")  #recode/replace ... bl 00m - bl
  )
```

``` r
## converting visit to a factor variable
pulse_data = 
  haven::read_sas("./data/public_pulse_data.sas7bdat") %>%
  janitor::clean_names() %>%
  pivot_longer(
    bdi_score_bl:bdi_score_12m,
    names_to = "visit", 
    names_prefix = "bdi_score_",# remove matching text from the start of each variable name.
    values_to = "bdi") %>%
  select(id, visit, everything()) %>% ## move id and visit to the front
  mutate(
    visit = replace(visit, visit == "bl", "00m"),
    visit = factor(visit, levels = str_c(c("00", "01", "06", "12"), "m"))) %>%
  arrange(id, visit)

print(pulse_data, n = 12)
```

    ## # A tibble: 4,348 x 5
    ##       id visit   age sex     bdi
    ##    <dbl> <fct> <dbl> <chr> <dbl>
    ##  1 10003 00m    48.0 male      7
    ##  2 10003 01m    48.0 male      1
    ##  3 10003 06m    48.0 male      2
    ##  4 10003 12m    48.0 male      0
    ##  5 10015 00m    72.5 male      6
    ##  6 10015 01m    72.5 male     NA
    ##  7 10015 06m    72.5 male     NA
    ##  8 10015 12m    72.5 male     NA
    ##  9 10022 00m    58.5 male     14
    ## 10 10022 01m    58.5 male      3
    ## 11 10022 06m    58.5 male      8
    ## 12 10022 12m    58.5 male     NA
    ## # … with 4,336 more rows

## Seperate for litters group

``` r
litters_data = 
  read_csv("./data/FAS_litters.csv", col_types = "ccddiiii") %>% 
  janitor::clean_names() %>%
  separate(group, into = c("dose", "day_of_tx"), sep = 3) %>% #sep: where to do split. go 3 characters...
  mutate(
    dose = str_to_lower(dose),
    wt_gain = gd18_weight - gd0_weight) %>%
  arrange(litter_number)

litters_data
```

    ## # A tibble: 49 x 10
    ##   dose  day_of_tx litter_number gd0_weight gd18_weight gd_of_birth
    ##   <chr> <chr>     <chr>              <dbl>       <dbl>       <int>
    ## 1 con   7         #1/2/95/2             27        42            19
    ## 2 con   7         #1/5/3/83/3-…         NA        NA            20
    ## 3 con   8         #1/6/2/2/95-2         NA        NA            20
    ## 4 mod   7         #1/82/3-2             NA        NA            19
    ## 5 low   8         #100                  20        39.2          20
    ## # … with 44 more rows, and 4 more variables: pups_born_alive <int>,
    ## #   pups_dead_birth <int>, pups_survive <int>, wt_gain <dbl>

## Learning Assessment 1

produces new variables gd and weight; and makes gd a numeric variable
taking values 0 and 18 (for the last part, you might want to use recode
…). Is this version “tidy”?

``` r
litters_data %>% 
  select(litter_number, ends_with("weight")) %>% 
  pivot_longer(
    gd0_weight:gd18_weight,
    names_to = "gd",
    values_to = "weight"
  ) %>% 
  mutate(
    gd = recode(gd, "gd0_weight" = 0, "gd18_weight" = 18)
  )
```

    ## # A tibble: 98 x 3
    ##   litter_number      gd weight
    ##   <chr>           <dbl>  <dbl>
    ## 1 #1/2/95/2           0     27
    ## 2 #1/2/95/2          18     42
    ## 3 #1/5/3/83/3-3/2     0     NA
    ## 4 #1/5/3/83/3-3/2    18     NA
    ## 5 #1/6/2/2/95-2       0     NA
    ## # … with 93 more rows

## pivot\_wider

``` r
analysis_result = tibble(
  group = c("treatment", "treatment", "placebo", "placebo"),
  time = c("pre", "post", "pre", "post"),
  mean = c(4, 8, 3.5, 4)
)

analysis_result
```

    ## # A tibble: 4 x 3
    ##   group     time   mean
    ##   <chr>     <chr> <dbl>
    ## 1 treatment pre     4  
    ## 2 treatment post    8  
    ## 3 placebo   pre     3.5
    ## 4 placebo   post    4

``` r
pivot_wider(
  analysis_result,
  names_from = time,
  values_from = mean
)
```

    ## # A tibble: 2 x 3
    ##   group       pre  post
    ##   <chr>     <dbl> <dbl>
    ## 1 treatment   4       8
    ## 2 placebo     3.5     4

## Binding rows

``` r
fellowship_ring = 
  readxl::read_excel("./data/LotR_Words.xlsx", range = "B3:D6") %>%
  mutate(movie = "fellowship_ring")

two_towers = 
  readxl::read_excel("./data/LotR_Words.xlsx", range = "F3:H6") %>%
  mutate(movie = "two_towers")

return_king = 
  readxl::read_excel("./data/LotR_Words.xlsx", range = "J3:L6") %>%
  mutate(movie = "return_king")
```

``` r
lotr_data =
  bind_rows(fellowship_ring,two_towers,return_king) %>% 
  janitor::clean_names() %>% 
  pivot_longer(
    female:male,
    names_to = "sex",
    values_to = "words"
  ) %>% 
  mutate(race = str_to_lower(race)) %>% 
  select(movie, everything())
```

## Pup data & Litter data (litter\_number)

``` r
pup_data = 
  read_csv("./data/FAS_pups.csv", col_types = "ciiiii") %>%
  janitor::clean_names() %>%
  mutate(sex = recode(sex, `1` = "male", `2` = "female")) 
pup_data
```

    ## # A tibble: 313 x 6
    ##   litter_number sex   pd_ears pd_eyes pd_pivot pd_walk
    ##   <chr>         <chr>   <int>   <int>    <int>   <int>
    ## 1 #85           male        4      13        7      11
    ## 2 #85           male        4      13        7      12
    ## 3 #1/2/95/2     male        5      13        7       9
    ## 4 #1/2/95/2     male        5      13        8      10
    ## 5 #5/5/3/83/3-3 male        5      13        8      10
    ## # … with 308 more rows

``` r
litter_data = 
  read_csv("./data/FAS_litters.csv", col_types = "ccddiiii") %>%
  janitor::clean_names() %>%
  select(-pups_survive) %>%
  mutate(
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group))
litter_data
```

    ## # A tibble: 49 x 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <int>           <int>
    ## 1 con7  #85                 19.7        34.7          20               3
    ## 2 con7  #1/2/95/2           27          42            19               8
    ## 3 con7  #5/5/3/83/3-3       26          41.4          19               6
    ## 4 con7  #5/4/2/95/2         28.5        44.1          19               5
    ## 5 con7  #4/2/95/3-3         NA          NA            20               6
    ## # … with 44 more rows, and 2 more variables: pups_dead_birth <int>,
    ## #   wt_gain <dbl>

``` r
fas_data =
  left_join(pup_data, litter_data, by = "litter_number") # can join by multiple variables
fas_data
```

    ## # A tibble: 313 x 13
    ##   litter_number sex   pd_ears pd_eyes pd_pivot pd_walk group gd0_weight
    ##   <chr>         <chr>   <int>   <int>    <int>   <int> <chr>      <dbl>
    ## 1 #85           male        4      13        7      11 con7        19.7
    ## 2 #85           male        4      13        7      12 con7        19.7
    ## 3 #1/2/95/2     male        5      13        7       9 con7        27  
    ## 4 #1/2/95/2     male        5      13        8      10 con7        27  
    ## 5 #5/5/3/83/3-3 male        5      13        8      10 con7        26  
    ## # … with 308 more rows, and 5 more variables: gd18_weight <dbl>,
    ## #   gd_of_birth <int>, pups_born_alive <int>, pups_dead_birth <int>,
    ## #   wt_gain <dbl>

``` r
names(fas_data)
```

    ##  [1] "litter_number"   "sex"             "pd_ears"        
    ##  [4] "pd_eyes"         "pd_pivot"        "pd_walk"        
    ##  [7] "group"           "gd0_weight"      "gd18_weight"    
    ## [10] "gd_of_birth"     "pups_born_alive" "pups_dead_birth"
    ## [13] "wt_gain"

``` r
names(pup_data)
```

    ## [1] "litter_number" "sex"           "pd_ears"       "pd_eyes"      
    ## [5] "pd_pivot"      "pd_walk"

``` r
  full_join(pup_data, litter_data, by = "litter_number") %>% 
  filter(is.na(sex))
```

    ## # A tibble: 2 x 13
    ##   litter_number sex   pd_ears pd_eyes pd_pivot pd_walk group gd0_weight
    ##   <chr>         <chr>   <int>   <int>    <int>   <int> <chr>      <dbl>
    ## 1 #112          <NA>       NA      NA       NA      NA low7        23.9
    ## 2 #7/82-3-2     <NA>       NA      NA       NA      NA mod8        26.9
    ## # … with 5 more variables: gd18_weight <dbl>, gd_of_birth <int>,
    ## #   pups_born_alive <int>, pups_dead_birth <int>, wt_gain <dbl>

``` r
## these litter_number don't have data in pups
```

``` r
surv_os = read_csv("data/surv_os.csv") %>% 
  janitor::clean_names() %>% 
  rename(id = what_is_your_uni, os = what_operating_system_do_you_use)
```

    ## Parsed with column specification:
    ## cols(
    ##   `What is your UNI?` = col_character(),
    ##   `What operating system do you use?` = col_character()
    ## )

``` r
surv_os
```

    ## # A tibble: 173 x 2
    ##   id          os        
    ##   <chr>       <chr>     
    ## 1 student_87  <NA>      
    ## 2 student_106 Windows 10
    ## 3 student_66  Mac OS X  
    ## 4 student_93  Windows 10
    ## 5 student_99  Mac OS X  
    ## # … with 168 more rows

``` r
surv_pr_git = read_csv("data/surv_program_git.csv") %>% 
  janitor::clean_names() %>% 
  rename(
    id = what_is_your_uni, 
    prog = what_is_your_degree_program,
    git_exp = which_most_accurately_describes_your_experience_with_git)
```

    ## Parsed with column specification:
    ## cols(
    ##   `What is your UNI?` = col_character(),
    ##   `What is your degree program?` = col_character(),
    ##   `Which most accurately describes your experience with Git?` = col_character()
    ## )

``` r
surv_pr_git 
```

    ## # A tibble: 135 x 3
    ##   id         prog  git_exp                                                 
    ##   <chr>      <chr> <chr>                                                   
    ## 1 student_1… MS    Pretty smooth: needed some work to connect Git, GitHub,…
    ## 2 student_32 MS    Not smooth: I don't like git, I don't like GitHub, and …
    ## 3 <NA>       MPH   Pretty smooth: needed some work to connect Git, GitHub,…
    ## 4 student_1… MPH   Not smooth: I don't like git, I don't like GitHub, and …
    ## 5 student_17 PhD   Pretty smooth: needed some work to connect Git, GitHub,…
    ## # … with 130 more rows

``` r
left_join(surv_os, surv_pr_git)
```

    ## Joining, by = "id"

    ## # A tibble: 175 x 4
    ##   id        os       prog  git_exp                                         
    ##   <chr>     <chr>    <chr> <chr>                                           
    ## 1 student_… <NA>     MS    Pretty smooth: needed some work to connect Git,…
    ## 2 student_… Windows… Other Pretty smooth: needed some work to connect Git,…
    ## 3 student_… Mac OS X MPH   Smooth: installation and connection with GitHub…
    ## 4 student_… Windows… MS    Smooth: installation and connection with GitHub…
    ## 5 student_… Mac OS X MS    Smooth: installation and connection with GitHub…
    ## # … with 170 more rows

``` r
inner_join(surv_os, surv_pr_git)
```

    ## Joining, by = "id"

    ## # A tibble: 129 x 4
    ##   id        os       prog  git_exp                                         
    ##   <chr>     <chr>    <chr> <chr>                                           
    ## 1 student_… <NA>     MS    Pretty smooth: needed some work to connect Git,…
    ## 2 student_… Windows… Other Pretty smooth: needed some work to connect Git,…
    ## 3 student_… Mac OS X MPH   Smooth: installation and connection with GitHub…
    ## 4 student_… Windows… MS    Smooth: installation and connection with GitHub…
    ## 5 student_… Mac OS X MS    Smooth: installation and connection with GitHub…
    ## # … with 124 more rows

``` r
anti_join(surv_os, surv_pr_git)
```

    ## Joining, by = "id"

    ## # A tibble: 46 x 2
    ##   id          os        
    ##   <chr>       <chr>     
    ## 1 student_86  Mac OS X  
    ## 2 student_91  Windows 10
    ## 3 student_24  Mac OS X  
    ## 4 student_103 Mac OS X  
    ## 5 student_163 Mac OS X  
    ## # … with 41 more rows

``` r
anti_join(surv_pr_git, surv_os)
```

    ## Joining, by = "id"

    ## # A tibble: 15 x 3
    ##    id        prog  git_exp                                                 
    ##    <chr>     <chr> <chr>                                                   
    ##  1 <NA>      MPH   Pretty smooth: needed some work to connect Git, GitHub,…
    ##  2 student_… PhD   Pretty smooth: needed some work to connect Git, GitHub,…
    ##  3 <NA>      MPH   Pretty smooth: needed some work to connect Git, GitHub,…
    ##  4 <NA>      MPH   Pretty smooth: needed some work to connect Git, GitHub,…
    ##  5 <NA>      MS    Pretty smooth: needed some work to connect Git, GitHub,…
    ##  6 student_… MS    Pretty smooth: needed some work to connect Git, GitHub,…
    ##  7 <NA>      MS    Smooth: installation and connection with GitHub was easy
    ##  8 student_… PhD   Pretty smooth: needed some work to connect Git, GitHub,…
    ##  9 student_… MPH   Smooth: installation and connection with GitHub was easy
    ## 10 student_… MS    Smooth: installation and connection with GitHub was easy
    ## 11 <NA>      MS    Pretty smooth: needed some work to connect Git, GitHub,…
    ## 12 <NA>      MS    "What's \"Git\" ...?"                                   
    ## 13 <NA>      MS    Smooth: installation and connection with GitHub was easy
    ## 14 <NA>      MPH   Pretty smooth: needed some work to connect Git, GitHub,…
    ## 15 <NA>      MS    Pretty smooth: needed some work to connect Git, GitHub,…
