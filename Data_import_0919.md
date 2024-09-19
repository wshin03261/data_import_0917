Data_manipulation_0919
================
2024-09-18

tidyverse will also load dplyr

Impoer the two data sets that we’re going to manipulate.

\##select

You can specify the columns you want to keep by naming all of them:
select()

``` r
select(litters_df, group, litter_number, gd0_weight)
```

    ## # A tibble: 49 × 3
    ##   group litter_number gd0_weight
    ##   <chr> <chr>              <dbl>
    ## 1 Con7  #85                 19.7
    ## 2 Con7  #1/2/95/2           27  
    ## 3 Con7  #5/5/3/83/3-3       26  
    ## # ℹ 46 more rows

You can specify the specify a range of columns to keep: so column 1 ~
until gd18_weight

``` r
select(litters_df, group:gd18_weight)
```

    ## # A tibble: 49 × 4
    ##   group litter_number gd0_weight gd18_weight
    ##   <chr> <chr>              <dbl>       <dbl>
    ## 1 Con7  #85                 19.7        34.7
    ## 2 Con7  #1/2/95/2           27          42  
    ## 3 Con7  #5/5/3/83/3-3       26          41.4
    ## # ℹ 46 more rows

You can also specify columns you’d like to remove: remove from the end
until gd18_weight.

``` r
select(litters_df, -(group:gd18_weight))
```

    ## # A tibble: 49 × 4
    ##   gd_of_birth pups_born_alive pups_dead_birth pups_survive
    ##         <dbl>           <dbl>           <dbl>        <dbl>
    ## 1          20               3               4            3
    ## 2          19               8               0            7
    ## 3          19               6               0            5
    ## # ℹ 46 more rows

you can also have a keyword selected

``` r
select(litters_df, starts_with("gd"))
```

    ## # A tibble: 49 × 3
    ##   gd0_weight gd18_weight gd_of_birth
    ##        <dbl>       <dbl>       <dbl>
    ## 1       19.7        34.7          20
    ## 2       27          42            19
    ## 3       26          41.4          19
    ## # ℹ 46 more rows

``` r
select(litters_df, contains("pups"))
```

    ## # A tibble: 49 × 3
    ##   pups_born_alive pups_dead_birth pups_survive
    ##             <dbl>           <dbl>        <dbl>
    ## 1               3               4            3
    ## 2               8               0            7
    ## 3               6               0            5
    ## # ℹ 46 more rows

You can rename variables as part of this process:

group to GROUP

``` r
select(litters_df, GROUP = group, LiTtEr_NuMbEr = litter_number)
```

    ## # A tibble: 49 × 2
    ##   GROUP LiTtEr_NuMbEr
    ##   <chr> <chr>        
    ## 1 Con7  #85          
    ## 2 Con7  #1/2/95/2    
    ## 3 Con7  #5/5/3/83/3-3
    ## # ℹ 46 more rows

``` r
rename(litters_df, GROUP = group)
```

    ## # A tibble: 49 × 8
    ##   GROUP litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ℹ 46 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

If all you want to do is rename something, you can use `rename` instead
of `select`. This will rename the variables you care about, and keep
everything else:

``` r
rename(litters_df, GROUP = group, LiTtEr_NuMbEr = litter_number)
```

    ## # A tibble: 49 × 8
    ##   GROUP LiTtEr_NuMbEr gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ℹ 46 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

There are some handy helper functions for select; read about all of them
using ?select_helpers. I use starts_with(), ends_with(), and contains()
often, especially when there variables are named with suffixes or other
standard patterns:

``` r
select(litters_df, starts_with("gd"))
```

    ## # A tibble: 49 × 3
    ##   gd0_weight gd18_weight gd_of_birth
    ##        <dbl>       <dbl>       <dbl>
    ## 1       19.7        34.7          20
    ## 2       27          42            19
    ## 3       26          41.4          19
    ## # ℹ 46 more rows

I also frequently use is everything(), which is handy for reorganizing
columns without discarding anything:

``` r
select(litters_df, litter_number, pups_survive, everything())
```

    ## # A tibble: 49 × 8
    ##   litter_number pups_survive group gd0_weight gd18_weight gd_of_birth
    ##   <chr>                <dbl> <chr>      <dbl>       <dbl>       <dbl>
    ## 1 #85                      3 Con7        19.7        34.7          20
    ## 2 #1/2/95/2                7 Con7        27          42            19
    ## 3 #5/5/3/83/3-3            5 Con7        26          41.4          19
    ## # ℹ 46 more rows
    ## # ℹ 2 more variables: pups_born_alive <dbl>, pups_dead_birth <dbl>

relocate does a similar thing (and is sort of like rename in that it’s
handy but not critical):

``` r
relocate(litters_df, litter_number, pups_survive)
```

    ## # A tibble: 49 × 8
    ##   litter_number pups_survive group gd0_weight gd18_weight gd_of_birth
    ##   <chr>                <dbl> <chr>      <dbl>       <dbl>       <dbl>
    ## 1 #85                      3 Con7        19.7        34.7          20
    ## 2 #1/2/95/2                7 Con7        27          42            19
    ## 3 #5/5/3/83/3-3            5 Con7        26          41.4          19
    ## # ℹ 46 more rows
    ## # ℹ 2 more variables: pups_born_alive <dbl>, pups_dead_birth <dbl>

In larger datasets,

Lastly, like other functions in dplyr, select will export a dataframe
even if you only select one column. Mostly this is fine, but sometimes
you want the vector stored in the column. To pull a single variable, use
pull.

\##Learning assessment

In the pups data, select the columns containing litter number, sex, and
PD ears.

``` r
select(pups_df, litter_number, sex, pd_ears)
```

    ## # A tibble: 313 × 3
    ##   litter_number   sex pd_ears
    ##   <chr>         <dbl>   <dbl>
    ## 1 #85               1       4
    ## 2 #85               1       4
    ## 3 #1/2/95/2         1       5
    ## # ℹ 310 more rows

\##filter

You will often filter using comparison operators (\>, \>=, \<, \<=, ==,
and !=). You may also use %in% to detect if values appear in a set, and
is.na() to find missing values. The results of comparisons are logical –
the statement is TRUE or FALSE depending on the values you compare – and
can be combined with other comparisons using the logical operators & and
\|, or negated using !.

Some ways you might filter the litters data are:

gd_of_birth == 20 pups_born_alive \>= 2 pups_survive != 4 !(pups_survive
== 4) group %in% c(“Con7”, “Con8”) group == “Con7” & gd_of_birth == 20

``` r
filter(litters_df, gd_of_birth == 20)
```

    ## # A tibble: 32 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #4/2/95/3-3         NA          NA            20               6
    ## 3 Con7  #2/2/95/3-2         NA          NA            20               6
    ## # ℹ 29 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, gd_of_birth == 19)
```

    ## # A tibble: 17 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #1/2/95/2           27          42            19               8
    ##  2 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ##  3 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ##  4 Con8  #5/4/3/83/3         28          NA            19               9
    ##  5 Con8  #2/2/95/2           NA          NA            19               5
    ##  6 Mod7  #59                 17          33.4          19               8
    ##  7 Mod7  #103                21.4        42.1          19               9
    ##  8 Mod7  #1/82/3-2           NA          NA            19               6
    ##  9 Mod7  #3/83/3-2           NA          NA            19               8
    ## 10 Mod7  #4/2/95/2           23.5        NA            19               9
    ## 11 Mod7  #5/3/83/5-2         22.6        37            19               5
    ## 12 Mod7  #94/2               24.4        42.9          19               7
    ## 13 Mod7  #62                 19.5        35.9          19               7
    ## 14 Low7  #112                23.9        40.5          19               6
    ## 15 Mod8  #5/93/2             NA          NA            19               8
    ## 16 Mod8  #7/110/3-2          27.5        46            19               8
    ## 17 Low8  #79                 25.4        43.8          19               8
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, pups_born_alive > 8)
```

    ## # A tibble: 12 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  2 Con8  #3/83/3-3             NA          NA            20               9
    ##  3 Con8  #5/4/3/83/3           28          NA            19               9
    ##  4 Mod7  #103                  21.4        42.1          19               9
    ##  5 Mod7  #4/2/95/2             23.5        NA            19               9
    ##  6 Mod7  #8/110/3-2            NA          NA            20               9
    ##  7 Low7  #107                  22.6        42.4          20               9
    ##  8 Low7  #98                   23.8        43.8          20               9
    ##  9 Low7  #102                  22.6        43.3          20              11
    ## 10 Low7  #101                  23.8        42.7          20               9
    ## 11 Mod8  #5/93                 NA          41.1          20              11
    ## 12 Mod8  #2/95/2               28.5        44.5          20               9
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, pups_born_alive >=8)
```

    ## # A tibble: 28 × 8
    ##   group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #1/2/95/2               27          42          19               8
    ## 2 Con7  #1/5/3/83/3-3/2         NA          NA          20               9
    ## 3 Con8  #3/83/3-3               NA          NA          20               9
    ## # ℹ 25 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df,pups_born_alive !=9) #not equal
```

    ## # A tibble: 39 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ℹ 36 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, group == "Low8")
```

    ## # A tibble: 7 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Low8  #53                 21.8        37.2          20               8
    ## 2 Low8  #79                 25.4        43.8          19               8
    ## 3 Low8  #100                20          39.2          20               8
    ## 4 Low8  #4/84               21.8        35.2          20               4
    ## 5 Low8  #108                25.6        47.5          20               8
    ## 6 Low8  #99                 23.5        39            20               6
    ## 7 Low8  #110                25.5        42.7          20               7
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, group %in% c("Low7","Low8"))
```

    ## # A tibble: 15 × 8
    ##    group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Low7  #84/2               24.3        40.8          20               8
    ##  2 Low7  #107                22.6        42.4          20               9
    ##  3 Low7  #85/2               22.2        38.5          20               8
    ##  4 Low7  #98                 23.8        43.8          20               9
    ##  5 Low7  #102                22.6        43.3          20              11
    ##  6 Low7  #101                23.8        42.7          20               9
    ##  7 Low7  #111                25.5        44.6          20               3
    ##  8 Low7  #112                23.9        40.5          19               6
    ##  9 Low8  #53                 21.8        37.2          20               8
    ## 10 Low8  #79                 25.4        43.8          19               8
    ## 11 Low8  #100                20          39.2          20               8
    ## 12 Low8  #4/84               21.8        35.2          20               4
    ## 13 Low8  #108                25.6        47.5          20               8
    ## 14 Low8  #99                 23.5        39            20               6
    ## 15 Low8  #110                25.5        42.7          20               7
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
filter(litters_df, group %in% c("Low7","Low8"), pups_born_alive == 8)
```

    ## # A tibble: 6 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Low7  #84/2               24.3        40.8          20               8
    ## 2 Low7  #85/2               22.2        38.5          20               8
    ## 3 Low8  #53                 21.8        37.2          20               8
    ## 4 Low8  #79                 25.4        43.8          19               8
    ## 5 Low8  #100                20          39.2          20               8
    ## 6 Low8  #108                25.6        47.5          20               8
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

filter missing variables

``` r
drop_na(litters_df)
```

    ## # A tibble: 31 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ℹ 28 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
drop_na(litters_df, gd0_weight) #drop missing values in gd0_weight
```

    ## # A tibble: 34 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ℹ 31 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

A very common filtering step requires you to omit missing observations.
You can do this with filter, but I recommend using drop_na from the
tidyr package:

drop_na(litters_df) will remove any row with a missing value
drop_na(litters_df, wt_increase) will remove rows for which wt_increase
is missing.

\##Learning Assessment2 In the pups data:

1.Filter to include only pups with sex 1 2.Filter to include only pups
with PD walk less than 11 and sex 2

``` r
filter(pups_df, sex == 1)
```

    ## # A tibble: 155 × 6
    ##   litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##   <chr>         <dbl>   <dbl>   <dbl>    <dbl>   <dbl>
    ## 1 #85               1       4      13        7      11
    ## 2 #85               1       4      13        7      12
    ## 3 #1/2/95/2         1       5      13        7       9
    ## # ℹ 152 more rows

``` r
filter(pups_df, sex == 2, pd_walk < 11)
```

    ## # A tibble: 127 × 6
    ##   litter_number   sex pd_ears pd_eyes pd_pivot pd_walk
    ##   <chr>         <dbl>   <dbl>   <dbl>    <dbl>   <dbl>
    ## 1 #1/2/95/2         2       4      13        7       9
    ## 2 #1/2/95/2         2       4      13        7      10
    ## 3 #1/2/95/2         2       5      13        8      10
    ## # ℹ 124 more rows

\##Mutate

Sometimes you need to select columns; sometimes you need to change them
or create new ones. You can do this using mutate.

The example below creates a new variable measuring the difference
between gd18_weight and gd0_weight and modifies the existing group
variable.

``` r
mutate(litters_df,
  wt_gain = gd18_weight - gd0_weight,
  group = str_to_lower(group)
)
```

    ## # A tibble: 49 × 9
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 con7  #85                 19.7        34.7          20               3
    ## 2 con7  #1/2/95/2           27          42            19               8
    ## 3 con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ℹ 46 more rows
    ## # ℹ 3 more variables: pups_dead_birth <dbl>, pups_survive <dbl>, wt_gain <dbl>

``` r
# can do multiple things

mutate(litters_df, sq_pups = pups_born_alive^2)
```

    ## # A tibble: 49 × 9
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Con7  #1/2/95/2           27          42            19               8
    ## 3 Con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ℹ 46 more rows
    ## # ℹ 3 more variables: pups_dead_birth <dbl>, pups_survive <dbl>, sq_pups <dbl>

A few things in this example are worth noting:

Your new variables can be functions of old variables New variables
appear at the end of the dataset in the order that they are created You
can overwrite old variables You can create a new variable and
immediately refer to (or change) it Creating a new variable that does
exactly what you need can be a challenge; the more functions you know
about, the easier this gets.

\##Learning Assessment3 In the pups data:

1.Create a variable that subtracts 7 from PD pivot 2.Create a variable
that is the sum of all the PD variables

``` r
mutate(pups_df, pivot_minus7 = pd_pivot - 7)
```

    ## # A tibble: 313 × 7
    ##   litter_number   sex pd_ears pd_eyes pd_pivot pd_walk pivot_minus7
    ##   <chr>         <dbl>   <dbl>   <dbl>    <dbl>   <dbl>        <dbl>
    ## 1 #85               1       4      13        7      11            0
    ## 2 #85               1       4      13        7      12            0
    ## 3 #1/2/95/2         1       5      13        7       9            0
    ## # ℹ 310 more rows

``` r
mutate(pups_df, pd_sum = pd_ears + pd_eyes + pd_pivot + pd_walk)
```

    ## # A tibble: 313 × 7
    ##   litter_number   sex pd_ears pd_eyes pd_pivot pd_walk pd_sum
    ##   <chr>         <dbl>   <dbl>   <dbl>    <dbl>   <dbl>  <dbl>
    ## 1 #85               1       4      13        7      11     35
    ## 2 #85               1       4      13        7      12     36
    ## 3 #1/2/95/2         1       5      13        7       9     34
    ## # ℹ 310 more rows

## arrange

In comparison to the preceding, arranging is pretty straightforward. You
can arrange the rows in your data according to the values in one or more
columns:

``` r
arrange(litters_df, gd0_weight)
```

    ## # A tibble: 49 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Mod7  #59                 17          33.4          19               8
    ## 2 Mod7  #62                 19.5        35.9          19               7
    ## 3 Con7  #85                 19.7        34.7          20               3
    ## # ℹ 46 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
arrange(litters_df, desc(gd0_weight)) #descending order
```

    ## # A tibble: 49 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Mod8  #82/4               33.4        52.7          20               8
    ## 2 Con7  #5/4/2/95/2         28.5        44.1          19               5
    ## 3 Con8  #3/5/2/2/95         28.5        NA            20               8
    ## # ℹ 46 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
arrange(litters_df, pups_born_alive, gd0_weight) 
```

    ## # A tibble: 49 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 Con7  #85                 19.7        34.7          20               3
    ## 2 Low7  #111                25.5        44.6          20               3
    ## 3 Low8  #4/84               21.8        35.2          20               4
    ## # ℹ 46 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

``` r
#order pups born alive first, then gd0_weight 
```

``` r
head(arrange(litters_df, group, pups_born_alive), 10)
```

    ## # A tibble: 10 × 8
    ##    group litter_number   gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##    <chr> <chr>                <dbl>       <dbl>       <dbl>           <dbl>
    ##  1 Con7  #85                   19.7        34.7          20               3
    ##  2 Con7  #5/4/2/95/2           28.5        44.1          19               5
    ##  3 Con7  #5/5/3/83/3-3         26          41.4          19               6
    ##  4 Con7  #4/2/95/3-3           NA          NA            20               6
    ##  5 Con7  #2/2/95/3-2           NA          NA            20               6
    ##  6 Con7  #1/2/95/2             27          42            19               8
    ##  7 Con7  #1/5/3/83/3-3/2       NA          NA            20               9
    ##  8 Con8  #2/2/95/2             NA          NA            19               5
    ##  9 Con8  #1/6/2/2/95-2         NA          NA            20               7
    ## 10 Con8  #3/6/2/2/95-3         NA          NA            20               7
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, pups_survive <dbl>

\##Piping

We’ve seen several commands you will use regularly for data manipulation
and cleaning. You will rarely use them in isolation. For example,
suppose you want to load the data, clean the column names, remove
pups_survive, and create wt_gain. There are a couple of options for this
kind of multi-step data manipulation:

1.define intermediate datasets (or overwrite data at each stage) 2.nest
function calls The following is an example of the first option:

``` r
litters_df_raw = 
    read_csv("FAS_litters.csv", na = c("NA", ".", ""))
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_df_clean_names = janitor::clean_names(litters_df_raw)

litters_df_selected_cols = select(litters_df_clean_names, -pups_survive)

litters_df_with_vars = 
  mutate(
    litters_df_selected_cols, 
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group))

litters_df_with_vars_without_missing = 
  drop_na(litters_df_with_vars, wt_gain)

litters_df_with_vars_without_missing
```

    ## # A tibble: 31 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 con7  #85                 19.7        34.7          20               3
    ## 2 con7  #1/2/95/2           27          42            19               8
    ## 3 con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ℹ 28 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, wt_gain <dbl>

Below, we try the second option:

``` r
litters_df_clean = 
  drop_na(
    mutate(
      select(
        janitor::clean_names(
          read_csv("FAS_litters.csv", na = c("NA", ".", ""))
          ), 
      -pups_survive
      ),
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group)
    ),
  wt_gain
  )
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_df_clean
```

    ## # A tibble: 31 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 con7  #85                 19.7        34.7          20               3
    ## 2 con7  #1/2/95/2           27          42            19               8
    ## 3 con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ℹ 28 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, wt_gain <dbl>

These are both confusing and bad: the first gets confusing and clutters
our workspace, and the second has to be read inside out.

Piping solves this problem. It allows you to turn the nested approach
into a sequential chain by passing the result of one function call as an
argument to the next function call:

``` r
litters_df = 
  read_csv("FAS_litters.csv", na = c("NA", ".", "")) |> 
  janitor::clean_names() |> 
  select(-pups_survive) |> 
  mutate(
    wt_gain = gd18_weight - gd0_weight,
    group = str_to_lower(group)) |> 
  drop_na(wt_gain)
```

    ## Rows: 49 Columns: 8
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Group, Litter Number
    ## dbl (6): GD0 weight, GD18 weight, GD of Birth, Pups born alive, Pups dead @ ...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
litters_df
```

    ## # A tibble: 31 × 8
    ##   group litter_number gd0_weight gd18_weight gd_of_birth pups_born_alive
    ##   <chr> <chr>              <dbl>       <dbl>       <dbl>           <dbl>
    ## 1 con7  #85                 19.7        34.7          20               3
    ## 2 con7  #1/2/95/2           27          42            19               8
    ## 3 con7  #5/5/3/83/3-3       26          41.4          19               6
    ## # ℹ 28 more rows
    ## # ℹ 2 more variables: pups_dead_birth <dbl>, wt_gain <dbl>

The functions in dplyr (and much of the tidyverse) are designed to work
smoothly with the pipe operator. By default, the pipe will take the
result of one function call and use that as the first argument of the
next function call; by design, functions in dplyr will take a tibble as
an input and return a tibble as a result. As a consequence, functions in
dplyr are easy to connect in a data cleaning chain.

In the majority of cases (and everywhere in the tidyverse) you can trust
that the first argument is the right one and be happy with life, but
there are some cases where what you’re piping isn’t going into the first
argument. Here, using the placeholder \_ is necessary to indicate where
the object being piped should go. For example, to regress wt_gain on
pups_born_alive, you might use:

``` r
litters_df |>
  lm(wt_gain ~ pups_born_alive, data = _) |>
  broom::tidy()
```

    ## # A tibble: 2 × 5
    ##   term            estimate std.error statistic  p.value
    ##   <chr>              <dbl>     <dbl>     <dbl>    <dbl>
    ## 1 (Intercept)       13.1       1.27      10.3  3.39e-11
    ## 2 pups_born_alive    0.605     0.173      3.49 1.55e- 3

here are limitations to the pipe. You shouldn’t have sequences that are
too long; there isn’t a great way to deal with multiple inputs and
outputs; and not everyone will know what \|\> means or does. That said,
compared to days when R users only had the first two options, life is
much better!

\##learning assessment4

Write a chain of commands that:

1.loads the pups data 2.cleans the variable names 3.filters the data to
include only pups with sex 1 4.removes the PD ears variable 5.creates a
variable that indicates whether PD pivot is 7 or more days

``` r
pups_df = 
    read_csv("FAS_pups.csv", na = c("NA", ".")) |>
  janitor::clean_names() |> 
  filter(sex == 1) |> 
  select(-pd_ears) |> 
  mutate(pd_pivot_gt7 = pd_pivot > 7)
```

    ## Rows: 313 Columns: 6
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (1): Litter Number
    ## dbl (5): Sex, PD ears, PD eyes, PD pivot, PD walk
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
pups_df
```

    ## # A tibble: 155 × 6
    ##   litter_number   sex pd_eyes pd_pivot pd_walk pd_pivot_gt7
    ##   <chr>         <dbl>   <dbl>    <dbl>   <dbl> <lgl>       
    ## 1 #85               1      13        7      11 FALSE       
    ## 2 #85               1      13        7      12 FALSE       
    ## 3 #1/2/95/2         1      13        7       9 FALSE       
    ## # ℹ 152 more rows
