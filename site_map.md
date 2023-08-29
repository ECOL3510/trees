---
title: "Map of sites"
output: 
  html_document:
    keep_md: true
---
## Load packages

```r
# install.packages('pacman', repos = "http://cran.us.r-project.org")
pacman::p_load(tidyverse, maps)
```

## Read in metadata for all NEON sites

```r
sites_all <- read_csv('./NEON_sites/NEON_Field_Site_Metadata_20210226_0.csv') %>% 
  select(domainID:colocated_site, latitude:longitude, state:country)
```

## Define the list of your project's sites

```r
# Update this list to include your team's study sites
# Note that each site should be in all-caps, and contained inside quotation marks

mySites <- c('HARV', 'ABBY', 'ONAQ', 'UNDE', 'DSNY', 'JERC', 'MLBS', 'SCBI')
```

## Subset "sites" object for your project sites

```r
sites <- sites_all %>% 
  filter(siteID %in% mySites) %>% 
  arrange(siteID)

print(sites)
```

```
## # A tibble: 8 x 10
##   domainID siteID site_name      site_type  site_subtype colocated_site latitude
##   <chr>    <chr>  <chr>          <chr>      <chr>        <chr>             <dbl>
## 1 D16      ABBY   Abby Road NEON Relocatab~ <NA>         <NA>               45.8
## 2 D03      DSNY   Disney Wilder~ Relocatab~ <NA>         <NA>               28.1
## 3 D01      HARV   Harvard Fores~ Core Terr~ <NA>         HOPB               42.5
## 4 D03      JERC   The Jones Cen~ Relocatab~ <NA>         FLNT               31.2
## 5 D07      MLBS   Mountain Lake~ Relocatab~ <NA>         <NA>               37.4
## 6 D15      ONAQ   Onaqui NEON    Core Terr~ <NA>         <NA>               40.2
## 7 D02      SCBI   Smithsonian C~ Core Terr~ <NA>         POSE               38.9
## 8 D05      UNDE   University of~ Core Terr~ <NA>         <NA>               46.2
## # ... with 3 more variables: longitude <dbl>, state <chr>, country <chr>
```

## List state names of your sites

```r
myStates <- as.character(sites$state)
print(myStates)
```

```
## [1] "Washington"    "Florida"       "Massachusetts" "Georgia"      
## [5] "Virginia"      "Utah"          "Virginia"      "Michigan"
```


## Map your sites!
If all your sites are in the contiguous US ("lower 48"), you can use the script below:

```r
# Run this chunk all at once to avoid plotting errors
par(oma=c(2,2,1,1), mar = c(0,0,0,0),mgp=c(0,0,0))

maps::map('state', col='black', lwd=2) 
maps::map('state', region = 'Washington', add = T, fill=T, col='#F8766D')  # Customize the fill color if you want!
maps::map('state', region = 'Florida', add = T, fill=T, col='#C59900')
maps::map('state', region = 'Massachusetts', add = T, fill=T, col='#85AD00')
maps::map('state', region = 'Georgia', add = T, fill=T, col='#00BC59')
maps::map('state', region = 'Virginia', add = T, fill=T, col='#9590FF')
maps::map('state', region = 'Utah', add = T, fill=T, col='#00B0F6')
maps::map('state', region = 'Michigan', add = T, fill=T, col='#F763E0')

points(sites$longitude, sites$latitude, 
       col='black', pch=16, cex=1.5,lwd=1)

axis(1, at=seq(-124,-68, 8), padj=1.5, cex.axis=1)
mtext("Longitude", 1, line=2.5, cex=1.25)

axis(2, at=c(25,31, 37, 43, 49), las=2, hadj=1.75, cex.axis=1)
mtext("Latitude", 2, line=2.5, cex=1.25)

text(sites$longitude, sites$latitude, sites$siteID,
     cex=1, font=2, 
     pos = c(1, 4, 3, 3, 1, 1, 3, 1)) # 1 = below; 2 = left; 3 = above; 4 = right
```

![](site_map_files/figure-html/myMap-1.png)<!-- -->
In addition to printing the in-line map, R will automatically save a copy of your map into a folder called "site_map_files"! 
