# R_Spatial

## R Setup

install.packages("devtools")
devtools::install_github("r-spatial/lwgeom", force = T)

install.packages(c(
### TOOLS
"sf", # reading/writing/analysis of spatial data
"dplyr", # wrangling and summarizing data
"viridis", # a nice color palette
"measurements", # for converting measurements (DMS to DD or UTM"
"mapview", # interactive maps
"tmap", # good mapping package mostly for static chloropleth style maps
"ggspatial", # for adding scale bar/north arrow and basemaps
"ggmap", # for google basemaps NOW REQUIRES API registration
"ggrepel", # labeling ggplot figures
"ggforce", # amazing cool package for labeling and fussing 
"cowplot", ## MOOOOOOO!! for plotting multiple plots
### DATA
"tigris", # ROAD/CENSU DATA, see also the "tidycensus" package
"USAboundaries", # great package for getting state/county boundaries
"sharpshootR"))

#copied from https://ryanpeek.org/mapping-in-R-workshop/vig_workflow_in_R_snowdata.html

---
title: "Some background Geospatial Concepts"
date: "last updated: `r Sys.Date()`"
institution: ""
output:
  xaringan::moon_reader:
    lib_dir: libs
    css: xaringan-themer.css
    chakra: libs/remark.js
    nature:
      highlightStyle: github
      highlightLines: true
      countIncrementalSlides: false
---



```{r setup, include=FALSE}
options(htmltools.dir.version = FALSE)
knitr::opts_chunk$set(fig.align="center", fig.width=5, fig.height=5, warning = FALSE, message = FALSE)
```

```{r xaringan-themer, include = FALSE}
library(xaringanthemer)
duo_accent(
  primary_color = "ivory",
  secondary_color = "#310A31",
  header_font_google = google_font("Roboto", "400"),
  text_font_google   = google_font("Lato", "300"),
  code_font_family = "Fira Code",
  code_font_url = "https://cdn.rawgit.com/tonsky/FiraCode/1.204/distr/fira_code.css",
  header_color = "#f54278",
  title_slide_text_color = "#354a66"
)
```


## Two types of Geospatial Data Formats


1. There are many file formats for geospatial data that fall into 2 main categories: 

  * raster: divides the area into points/cells and specifies values for each point/cell
  
  
  * vector: use points, lines and polygons to describe entities in geographical space
  
  
2. Popularity/appropriateness of each will vary from case to case or from field to field


3. Working with raster and vector formats in R will typically require using different R packages.


---
## Coordinate Reference Systems (CRSs)

How do coordinates get mapped to actual points on the surface of the earth?

  * When you are given geographic coordinates, often latitude and longitude, there are many implicit assumptions/simplifications being made:

    * a coordinate origin 
  
    * reference meridian (0 longitude)
  
    * datum surface: model shape of the earth
  
    * earth's gravitational constant, $GM$ 
  
    * gravitational model: to model sea level 
  
    * magnetic model


The entire system built on these assumptions/simplifications is referred to as a Coordinate Reference System


---
## There are multiple CRSs


  * <tt>WGS84</tt> (World Geodetic System 1984) is a commonly-used standard globally
    * Used in the Global Positional System (GPS)
    
  * Mulitiple other CRSs are popular in different contexts e.g. <tt>NAD83</tt> is widely-used by agencies of the US government 
    
  * Geographic region being depicted may influence the choice of CRS




---
## Projections

  * Impossible to project the surface of the earth onto a flat surface without some form of distortion 
  
  * The various mathematical systems that determine how the translation to a flat surface is done are called projections
  
  * Choice of projection, influenced by factors including:
    
    * location on earth
    
    * need to preserve shape
    
    * need to preserve size


---
## Proj strings

Geographic data often comes with a proj string

PROJ is an open source software application that translates coordinates from one CRS to another. 

  * proj=: the projection of the data
  * zone=: the zone of the data (this is specific to the UTM projection)
  * datum=: the datum use
  * units=: the units for the coordinates of the data
  * ellps=: the ellipsoid (how the earth’s roundness is calculated) for the data
  
---
## Key point: Check your CRS and projection

  * If data comes from multiple sources, you may have to convert between CRS
  
  * The accuracy of some calculations, e.g. distance between two points,is predicated on using the correct CRS
  
  * An inappropriate projection may be harmful
  
  
  
---
## Other software underlying r-spatial packages

R packages for geospatial analysis rely on other open-source software applications, including:

  * <tt>GEOS</tt> (Geometry Engine - Open Source) core geometric functions
  
  * <tt>GDAL</tt> (Geospatial Data Abstraction Library): converts geospatial data between different file formats.

  * <tt>PROJ</tt> translates coordinates from one CRS to another.