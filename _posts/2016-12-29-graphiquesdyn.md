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
<div style="position:relative; max-width: 100%; width:150px; height:100px; padding-bottom:200px;">
    <iframe style="position:absolute; left:-41px; top:-41px; width:100%; height:100%;max-width: 100%"
        src="https://antuki.github.io/figure/graphiquesdyn_html1.html" scrolling="no">
    </iframe>
</div>


test2
<div style="position:relative; max-width: 100%; width:100px; height:50px; padding-bottom:200px;">
    <iframe style="position:absolute; left:-41px; top:-41px; width:100%; height:100%;max-width: 100%"
        src="https://antuki.github.io/figure/graphiquesdyn_html1.html" scrolling="no">
    </iframe>
</div>

