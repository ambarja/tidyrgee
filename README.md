
<!-- README.md is generated from README.Rmd. Please edit that file -->

# tidyrgee

<!-- badges: start -->

[![R-CMD-check](https://github.com/r-tidy-remote-sensing/tidyrgee/workflows/R-CMD-check/badge.svg)](https://github.com/r-tidy-remote-sensing/tidyrgee/actions)
[![CRAN
status](https://www.r-pkg.org/badges/version/tidyrgee)](https://CRAN.R-project.org/package=tidyrgee)
[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://lifecycle.r-lib.org/articles/stages.html#experimental)
<!-- badges: end -->

The goal of tidyrgee is to create tidyverse style methods (filter,
group_by, summarise, etc) for Google Earth Engine (GEE) Images and
ImageCollections.

## Installation

You can install the development version of tidyrgee from
[GitHub](https://github.com/) with:

``` r
# install.packages("devtools")
devtools::install_github("r-tidy-remote-sensing/tidyrgee")
```

## Example

This is a basic example which shows you how to solve a common problem:
filter an ImageCollection by date and then group by year and month and
then take the mean.

### load and initialize

``` r
library(tidyrgee)
#> 
#> Attaching package: 'tidyrgee'
#> The following object is masked from 'package:stats':
#> 
#>     filter
library(rgee)
ee_Initialize()
#> -- rgee 1.1.2.9000 ---------------------------------- earthengine-api 0.1.295 -- 
#>  v user: not_defined
#>  v Initializing Google Earth Engine: v Initializing Google Earth Engine:  DONE!
#>  v Earth Engine account: users/zackarno 
#> --------------------------------------------------------------------------------
```

### dplyresque syntax

In this example, by piping in `filter`, `group_by` , `summarise` just as
you would for a data.frame you are sequentially:

1.  filtering the ImageCollection by date
2.  grouping it by year
3.  summarizing at the pixel-level by taking the mean of all pixels in
    your date range for each year

The result will be an `ImageCollection` with the one `Image` per year
that holds the mean pixel values

``` r
modis_ic <- ee$ImageCollection("MODIS/006/MOD13Q1")

modis_mean_ndvi_yearly <- modis_ic$select("NDVI") %>% # still rgee/GEE syntax
   filter(date>="2016-01-01",date<="2019-12-31") %>%
   group_by(year) %>%
   summarise(stat="mean")
#> returning imageCol grouped by yearreturning  ImageCollection of x
```
