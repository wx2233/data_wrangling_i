Data\_Import
================
Weijia Xiong
9/17/2019

## Load in the litters dataset

``` r
## read in a dataset

## absolute path (use getwd())
#"/Users/osukuma/Downloads/DS/Data_Import/data_wrangling_i/data/FAS_litters.csv"
## relative path
#"./data//FAS_litters.csv""

litters_data = read_csv(file = "./data/FAS_litters.csv") ## don't use read.csv
```

    ## Parsed with column specification:
    ## cols(
    ##   Group = col_character(),
    ##   `Litter Number` = col_character(),
    ##   `GD0 weight` = col_double(),
    ##   `GD18 weight` = col_double(),
    ##   `GD of Birth` = col_double(),
    ##   `Pups born alive` = col_double(),
    ##   `Pups dead @ birth` = col_double(),
    ##   `Pups survive` = col_double()
    ## )

``` r
## view(data)
# view(litters_data)  only use it in the console
names(litters_data)
```

    ## [1] "Group"             "Litter Number"     "GD0 weight"       
    ## [4] "GD18 weight"       "GD of Birth"       "Pups born alive"  
    ## [7] "Pups dead @ birth" "Pups survive"

``` r
## clean up the names
litters_data = janitor::clean_names(litters_data)  #janitor is the package. We only use one function of this package. Sometimes there are name conflicts .
names(litters_data)
```

    ## [1] "group"           "litter_number"   "gd0_weight"      "gd18_weight"    
    ## [5] "gd_of_birth"     "pups_born_alive" "pups_dead_birth" "pups_survive"

``` r
## Check the data in console
#view/View/str/head/tail
#tail(litters_data, 5)
#skimr::skim(litters_data) Skim summary statistics
```

## Load in the pups data

``` r
## absolute path
pups_data = read_csv(file = "~/Downloads/DS/Data_Import/data_wrangling_i/data/FAS_pups.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   `Litter Number` = col_character(),
    ##   Sex = col_double(),
    ##   `PD ears` = col_double(),
    ##   `PD eyes` = col_double(),
    ##   `PD pivot` = col_double(),
    ##   `PD walk` = col_double()
    ## )

``` r
## relative path
pups_data = read_csv(file = "./data/FAS_pups.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   `Litter Number` = col_character(),
    ##   Sex = col_double(),
    ##   `PD ears` = col_double(),
    ##   `PD eyes` = col_double(),
    ##   `PD pivot` = col_double(),
    ##   `PD walk` = col_double()
    ## )

``` r
pups_data = janitor::clean_names(pups_data)
# view(pups_data)
```

\#\#Skip the first 50 lines of data and not assume the first row are
variable
names

``` r
#litters_data = read_csv(file = "./data/FAS_litters.csv", skip = 10, col_names = FALSE) 
#head(litters_data)
```
