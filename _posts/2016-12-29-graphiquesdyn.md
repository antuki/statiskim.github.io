---
layout: post
comments : true
title: "Réaliser des graphiques dynamiques sur R"
tags: [S'amuser sur R]
---

Depuis une petite année, j'ai eu l'occasion de découvrir un peu (sans trop creuser) quelques htmlwidget de R qui permettent de générer en code html des graphiques bien sympathiques. En voici quelques exemples.

<iframe width="100" height="100" src="https://antuki.github.io/figure/graph_html_test.html" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"></iframe><iframe width="100" height="100" src="https://antuki.github.io/figure/graph_html_test.html" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"></iframe>



```r
library(sparkline)
x <- rnorm(20)
sparkline(x)
```
