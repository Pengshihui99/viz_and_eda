Visualization part 1
================
Shihui Peng
2023-10-12

# Get the data for plotting

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USW00022534", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2021-01-01",
    date_max = "2022-12-31") |>
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USW00022534 = "Molokai_HI",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) |>
  select(name, id, everything())
```

    ## using cached file: /Users/peng_/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2023-10-12 05:40:09.606797 (8.534)

    ## file min/max dates: 1869-01-01 / 2023-10-31

    ## using cached file: /Users/peng_/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00022534.dly

    ## date created (size, mb): 2023-10-12 05:40:14.620904 (3.839)

    ## file min/max dates: 1949-10-01 / 2023-10-31

    ## using cached file: /Users/peng_/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2023-10-12 05:40:16.392605 (0.997)

    ## file min/max dates: 1999-09-01 / 2023-10-31

- we are using rnoaa package and function ’meteo_pull_monitors. The
  function of this agency is to monitors weather around the country and
  often around the world. There are 3 monitors to pull now:
  `"USW00094728", "USW00022534", "USS0023B17S"`, they locate in
  `"CentralPark_NY", "Molokai_HI", "Waterhole_WA"`.
- `var = c("PRCP", "TMIN", "TMAX"), date_min = "2021-01-01", date_max = "2022-12-31"`
  : give me the precipitation, the min daily temperature, and the max
  daily temperature for everyday bt 2021.1.1 and 2022.12.31.
- `mutate` part: (1) recode or create a new var that ,aps the weather
  station id into a more informative name. (2) the min and max temp come
  in 10s of degrees Celsius, divided by 10 to get better interpretation.
- `select` part: do reorganization, put name and id as the 1st and 2nd
  col, and keep everything else.
- take some time to run bc these data comes from the internet - from
  online database.

# Let’s make a plot!

``` r
ggplot(weather_df, aes(x = tmin, y = tmax)) + geom_point()
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

![](visualization_part_1_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

- how we say to ggplot?
  - 1st: tell which dataset to be used.
  - 2nd: definie the aesthetic mappings.
    - **`aes(x = tmin, y = tmax)`**: assign variable tmin to x-axis and
      variable tmax to y-axis.
      - nothing on the plot bc haven’t said what kind of plot is to be
        made.
  - 3rd: define which graph to be used for the dataset.
    - **`geom_point()`**: **scatterplot**
- there is a warning message “Removed 17 rows containing missing values
  (`geom_point()`)”
  - across these 2 years, there are 17 days w/o tmin and tmax
    observations. these records are removed when doing the scatterplot

## Pipes and stuff

``` r
weather_df |>  
  filter(name == "CentralPark_NY") |> 
  ggplot(aes(x = tmin, y = tmax)) + geom_point()
```

![](visualization_part_1_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->
\* using pipes makes things easy when we want to use stuff like
`filter`. \* here we left only w the NY dataset, but we can also filter
based on year or other stuff. With pipes, we don’t need to create
additional datasets, eg. ny_df, 2021_df –\> creating too much dataset
would not be a good idea.

## Save ggplot object

``` r
ggp_nyc_weather = 
  weather_df |>  
  filter(name == "CentralPark_NY") |> 
  ggplot(aes(x = tmin, y = tmax)) + geom_point()

ggp_nyc_weather
```

![](visualization_part_1_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->
\* do the 1st part and then do `ggp_nyc_weather` to print and save the
plot. if we want more gems on top of this, we can also use
`ggp_nyc_weather + blablabla` to be more condense. So, save it = draw
the plot + name it

## Fancy plot
