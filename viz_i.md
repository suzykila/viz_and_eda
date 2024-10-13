visualization
================
ww2745
2024-10-13

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(ggridges)
```

``` r
weather_df = 
  read_csv("./data/weather_df.csv")
```

    ## Rows: 2190 Columns: 6
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (2): name, id
    ## dbl  (3): prcp, tmax, tmin
    ## date (1): date
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
weather_df
```

    ## # A tibble: 2,190 × 6
    ##    name           id          date        prcp  tmax  tmin
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl>
    ##  1 CentralPark_NY USW00094728 2021-01-01   157   4.4   0.6
    ##  2 CentralPark_NY USW00094728 2021-01-02    13  10.6   2.2
    ##  3 CentralPark_NY USW00094728 2021-01-03    56   3.3   1.1
    ##  4 CentralPark_NY USW00094728 2021-01-04     5   6.1   1.7
    ##  5 CentralPark_NY USW00094728 2021-01-05     0   5.6   2.2
    ##  6 CentralPark_NY USW00094728 2021-01-06     0   5     1.1
    ##  7 CentralPark_NY USW00094728 2021-01-07     0   5    -1  
    ##  8 CentralPark_NY USW00094728 2021-01-08     0   2.8  -2.7
    ##  9 CentralPark_NY USW00094728 2021-01-09     0   2.8  -4.3
    ## 10 CentralPark_NY USW00094728 2021-01-10     0   5    -1.6
    ## # ℹ 2,180 more rows

\##ScatterPlots create my first plot ever!

``` r
ggplot(weather_df,aes(x=tmin,y=tmax))+
  geom_point()
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_i_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

new approach, same plot 就可以在画图之前mutate

``` r
weather_df |> 
  ggplot(aes(x=tmin,y=tmax))+
  geom_point()
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_i_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

save and edit a plot, least frequent to use

``` r
weather_plot=
  weather_df |> 
  ggplot(aes(x=tmin,y=tmax))

weather_plot+geom_point()
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_i_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

\##Advanced scatter plot Start with the same one and make it fancy

``` r
weather_df |> 
  ggplot(aes(x=tmin,y=tmax,color=name))+
  geom_point()+
  geom_smooth(se=FALSE)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_i_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

what about the ‘aes’ placement?

``` r
weather_df |> 
  ggplot(aes(x=tmin,y=tmax))+
  geom_point(aes(color=name))
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_i_files/figure-gfm/unnamed-chunk-6-1.png)<!-- --> same graph

``` r
weather_df |> 
  ggplot(aes(x=tmin,y=tmax))+
  geom_point(aes(color=name))+
  geom_smooth()
```

    ## `geom_smooth()` using method = 'gam' and formula = 'y ~ s(x, bs = "cs")'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_i_files/figure-gfm/unnamed-chunk-7-1.png)<!-- --> only a blue
line ,因为color只对geom point work

Lets facet some thing！

``` r
weather_df |> 
  ggplot(aes(x=tmin,y=tmax,color=name))+
  geom_point()+
  geom_smooth(se=FALSE)+ #don't forget the + 
  facet_grid(. ~name) #nothing facet the rows, but facet col by names
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_i_files/figure-gfm/unnamed-chunk-8-1.png)<!-- --> rows version

``` r
weather_df |> 
  ggplot(aes(x=tmin,y=tmax,color=name))+
  geom_point()+
  geom_smooth(se=FALSE)+ #don't forget the + 
  facet_grid(name~.) #nothing facet the rows, but facet col by names
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_i_files/figure-gfm/unnamed-chunk-9-1.png)<!-- --> 15%
(a=0.5)transparency for individual point

``` r
weather_df |> 
  ggplot(aes(x=tmin,y=tmax,color=name))+
  geom_point(alpha=0.2)+
  geom_smooth(se=FALSE)+ #don't forget the + 
  facet_grid(. ~name) #nothing facet the rows, but facet col by names
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_i_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

size of smooth

``` r
weather_df |> 
  ggplot(aes(x=tmin,y=tmax,color=name))+
  geom_point(alpha=0.2)+
  geom_smooth(se=FALSE,size=3)+ #don't forget the + 
  facet_grid(. ~name) #nothing facet the rows, but facet col by names
```

    ## Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
    ## ℹ Please use `linewidth` instead.
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_i_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

alpha can be set to vary according to t min in upper level

``` r
weather_df |> 
  ggplot(aes(x=tmin,y=tmax,color=name,alpha=tmin))+
  geom_point()+
  geom_smooth(se=FALSE)+ #don't forget the + 
  facet_grid(. ~name) #nothing facet the rows, but facet col by names
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: The following aesthetics were dropped during statistical transformation: alpha.
    ## ℹ This can happen when ggplot fails to infer the correct grouping structure in
    ##   the data.
    ## ℹ Did you forget to specify a `group` aesthetic or to convert a numerical
    ##   variable into a factor?
    ## The following aesthetics were dropped during statistical transformation: alpha.
    ## ℹ This can happen when ggplot fails to infer the correct grouping structure in
    ##   the data.
    ## ℹ Did you forget to specify a `group` aesthetic or to convert a numerical
    ##   variable into a factor?
    ## The following aesthetics were dropped during statistical transformation: alpha.
    ## ℹ This can happen when ggplot fails to infer the correct grouping structure in
    ##   the data.
    ## ℹ Did you forget to specify a `group` aesthetic or to convert a numerical
    ##   variable into a factor?

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_i_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

let’s combine some elements and try a new plot.

``` r
weather_df |> 
  ggplot(aes(x=date,y=tmax,color=name))+
  geom_point(aes(size=prcp),alpha=.5 )+
  geom_smooth(se=FALSE )+
  facet_grid(.~name)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

    ## Warning: Removed 19 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_i_files/figure-gfm/unnamed-chunk-13-1.png)<!-- --> map size of
the dot to percipation

\##Some small notes

How many geoms have to exist?

You can have whatever geoms you want

``` r
weather_df |> 
  ggplot(aes(x=tmin,y=tmax,color=name))+
  geom_smooth(se=FALSE)
```

    ## `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_smooth()`).

![](viz_i_files/figure-gfm/unnamed-chunk-14-1.png)<!-- --> not
recommended by just showing the smooth line

You can use a neat geom!

``` r
weather_df |> 
  ggplot(aes(x=tmin,y=tmax))+
  geom_hex()
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_binhex()`).

![](viz_i_files/figure-gfm/unnamed-chunk-15-1.png)<!-- --> 2d
version,instead of showing every point,good for huge nums of data to
show data distribution

``` r
weather_df |> 
  ggplot(aes(x=tmin,y=tmax))+
  geom_bin2d()
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_bin2d()`).

![](viz_i_files/figure-gfm/unnamed-chunk-16-1.png)<!-- --> another
version of hex, with square dots

``` r
weather_df |> 
  ggplot(aes(x=tmin,y=tmax))+
  geom_density2d()+
  geom_point(alpha=.3 ) 
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_density2d()`).

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_i_files/figure-gfm/unnamed-chunk-17-1.png)<!-- -->

\#Univariate plots Histograms are really great—look for distribution and
if anything is missed

``` r
weather_df |> 
  ggplot(aes(x=tmin))+
  geom_histogram()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](viz_i_files/figure-gfm/unnamed-chunk-18-1.png)<!-- -->

Can we add colors?

``` r
weather_df |> 
  ggplot(aes(x=tmin,color=name))+
  geom_histogram()
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](viz_i_files/figure-gfm/unnamed-chunk-19-1.png)<!-- --> bad:1.all
stack together and 2. color on the frame is not what we wanted

``` r
weather_df |> 
  ggplot(aes(x=tmin, fill=name))+
  geom_histogram(position = "dodge")
```

    ## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_bin()`).

![](viz_i_files/figure-gfm/unnamed-chunk-20-1.png)<!-- --> compare
dodge(difficult to distinguish the individual) vs facet_grid(need to
compare the distribution by ourselves)

so when using the histogram to compare distribution , a better choice:
Let’s try a new geometry

``` r
weather_df |> 
  ggplot(aes(x=tmin, fill=name))+
  geom_density(alpha=.3, adjust=.5) #show more bumps and individual
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_density()`).

![](viz_i_files/figure-gfm/unnamed-chunk-21-1.png)<!-- --> lose some
info, but easier to compare the distribution

What about box plots?

``` r
weather_df |> 
  ggplot(aes(x=name,y=tmin))+ 
  geom_boxplot()
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_boxplot()`).

![](viz_i_files/figure-gfm/unnamed-chunk-22-1.png)<!-- --> let x equals
categerical var–show each box for each name

Trendy plots :-)

``` r
weather_df |> 
  ggplot(aes(x=name,y=tmin,fill=name))+
  geom_violin(alpha=.5)+
  stat_summary(fun="median")
```

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_ydensity()`).

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_summary()`).

    ## Warning: Removed 3 rows containing missing values or values outside the scale range
    ## (`geom_segment()`).

![](viz_i_files/figure-gfm/unnamed-chunk-23-1.png)<!-- -->

Ridge plots–most popular plots in 2017

``` r
weather_df |> 
  ggplot(aes(x=tmin,y=name))+
  geom_density_ridges()
```

    ## Picking joint bandwidth of 1.41

    ## Warning: Removed 17 rows containing non-finite outside the scale range
    ## (`stat_density_ridges()`).

![](viz_i_files/figure-gfm/unnamed-chunk-24-1.png)<!-- --> useful eg.
age distribution across 50 states

\##Save and embed Let’s save a scatterplot.

``` r
weather_plot=
  weather_df |> 
  ggplot(aes(x=tmin,y=tmax,color=name))+
  geom_point(alpha=.3)

ggsave("./result/weather_plot.pdf",weather_plot, width=8, height=5)
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

What about embedding…

``` r
weather_plot
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_i_files/figure-gfm/unnamed-chunk-26-1.png)<!-- -->

Embed at different size

``` r
weather_plot
```

    ## Warning: Removed 17 rows containing missing values or values outside the scale range
    ## (`geom_point()`).

![](viz_i_files/figure-gfm/unnamed-chunk-27-1.png)<!-- -->
