Lab 1-2 Replication
================
Christopher Prener, Ph.D.
(February 07, 2022)

## Introduction

This notebook provides a replication of Lab 1-2.

## Dependencies

This notebook requires the following packages:

``` r
# tidyverse packages
library(ggplot2)       # static mapping

# mapping packages
library(mapview)      # preview spatial data
library(sf)           # spatial tools
```

    ## Linking to GEOS 3.9.1, GDAL 3.2.1, PROJ 7.2.1; sf_use_s2() is TRUE

``` r
# other packages
library(here)         # file path management
```

    ## here() starts at C:/Users/prenercg/Documents/GitHub/PrenerAssignments22/Labs/lab-02

``` r
library(viridis)      # viridis color palettes
```

    ## Loading required package: viridisLite

## Load Data

The data for today’s lecture are county-level population estimates from
the 2010 Decennial Census for Missouri. These can be found in
`data/MO_DEMOS_Black_Population`. They are `.geojson` data:

``` r
mo <- st_read(here("data", "MO_DEMOS_Black_Population", "MO_DEMOS_Black_Population.geojson"))
```

    ## Reading layer `MO_DEMOS_Black_Population' from data source 
    ##   `C:\Users\prenercg\Documents\GitHub\PrenerAssignments22\Labs\lab-02\data\MO_DEMOS_Black_Population\MO_DEMOS_Black_Population.geojson' 
    ##   using driver `GeoJSON'
    ## Simple feature collection with 115 features and 9 fields
    ## Geometry type: MULTIPOLYGON
    ## Dimension:     XY
    ## Bounding box:  xmin: 265131.4 ymin: 3986650 xmax: 847376.1 ymax: 4496645
    ## Projected CRS: NAD83 / UTM zone 15N

We’re now ready to map these data.

## Part 1 - Exploration

We’ll use the `mapview()` function to explore these data:

``` r
mapview(mo)
```

![](lab-1-2-replication_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

We can see that our data line-up with the basemap (always a good sign!)
and that they have a number of possible columns for mapping.

## Part 2 - Mapping

In order to map the Black population in Missouri correctly, we need to
decide on how we want to normalize our map. We have three column choices
in this lab:

1.  `black_den` is the number of African American residents per square
    kilometer
2.  `black_pct` is the percentage of African American residents in each
    county
3.  `blackr_` is the per capita rate per 1,000, so, the number of Black
    residents per 1,000 total residents

Any of these three are appropriate for this map!

``` r
p1 <- ggplot() +
  geom_sf(data = mo, mapping = aes(fill = black_pct)) +
  scale_fill_viridis(name = "% African American") +
  labs(
    title = "Black Population by Missouri County",
    subtitle = "2010 U.S. Census",
    caption = "Data via the U.S. Census Bureau \n Map by Christopher Prener, PhD"
  )

p1
```

![](lab-1-2-replication_files/figure-gfm/map-1-1.png)<!-- -->

If we map using percent, we see that St. Louis City has the highest
percent of African American residents. Other counties with relatively
high percentages are St. Louis County, Jackson County (the main part of
Kansas City), and the Bootheel counties in southeastern MO.

If we map using density (not shown), the only counties that stand out
are St. Louis City, which has the highest density of African American
residents, as well as St. Louis County and Jackson County (the main part
of Kansas City).

If we map using per rate per 1,000, we see that St. Louis City has the
highest rate of African American residents. Other counties with
relatively high rates are St. Louis County, Jackson County (the main
part of Kansas City), and the Bootheel counties in southeastern MO.

Finally, we’ll save our map to the `results/` folder. Remember that
you’ll have had to create this ahead of time!

``` r
ggsave(plot = p1, filename = here("results", "mo_african_americans.png"))
```

    ## Saving 7 x 5 in image

Now our map has been saved!
