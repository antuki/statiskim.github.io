---
layout: post
comments : true
title: "Réaliser des graphiques dynamiques sur R"
tags: [S'amuser sur R]
---

Depuis une petite année, j'ai eu l'occasion de découvrir quelques [htmlwidgets pour R](http://gallery.htmlwidgets.org/) qui permettent de générer en code html des graphiques bien sympathiques. 


En voici quelques exemples...


<!--break-->


```r
#install.packages("sparkline")
library(sparkline)
x <- rnorm(20)
sparkline(x)
```

```r
#install.packages("leaflet")
library(leaflet)
m <- leaflet()
m <- addTiles(map = m)
m <- fitBounds(map=m, lng1=-1.74586, lat1=48.04683, lng2=-1.73245, lat2=48.05210)
m <- addMarkers(map=m,lng=-1.742, lat=48.051, popup="Là où j'ai fait mes études !")
m
```

```r
#install.packages("plotly")
library(plotly)
plot_ly(z = ~volcano, type = "surface")
```
