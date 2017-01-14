---
layout: post
comments : true
title: "Réaliser des graphiques dynamiques sur R"
tags: [S'amuser sur R]
---

Depuis une petite année, j'ai eu l'occasion de découvrir quelques htmlwidgets pour R qui permettent de générer en code html des graphiques bien sympathiques. 

En voici quelques exemples et empressez-vous de découvrir les autres [ici](http://gallery.htmlwidgets.org/) !


```r
#install.packages("leaflet")
library(leaflet)
m <- leaflet()
m <- addTiles(map = m)
m <- fitBounds(map=m, lng1=-1.74586, lat1=48.04683, lng2=-1.73245, lat2=48.05210)
m <- addMarkers(map=m,lng=-1.742, lat=48.051, popup="Là où j'ai fait mes études !")
m
```
<div style="position:relative; max-width: 100%; width:100%; height:0px; padding-bottom:65%;">
    <iframe style="position:absolute; left:0; top:0; width:100%; height:100%;max-width: 100%"
        src="https://antuki.github.io/figure/graphiquesdyn_html2.html">
    </iframe>
</div>

<!--break-->

```r
#install.packages("sparkline")
library(sparkline)
x <- rnorm(20)
sparkline(x)
```
test1
<iframe width="100" height="100" src="https://antuki.github.io/figure/graph_html_test.html" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"></iframe>

test2
<div style="position:relative;width:75px;height:36px;overflow:hidden;"><iframe scrolling="no" style="position:absolute;width:1000px;height:1000px;left:-41px;top:-41px;" src="https://antuki.github.io/figure/graph_html_test.html"></iframe></div>


