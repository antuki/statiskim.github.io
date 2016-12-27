---
layout: post
comments : true
title: "Réaliser des graphiques dynamiques sur R"
tags: [S'amuser sur R]
---

Depuis une petite année, j'ai eu l'occasion de découvrir quelques [htmlwidgets pour R](http://gallery.htmlwidgets.org/) qui permettent de générer en code html des graphiques bien sympathiques. 

<iframe width="100" height="100" src="https://antuki.github.io/figure/graph_html_test.html" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"></iframe><iframe width="100" height="100" src="https://antuki.github.io/figure/graph_html_test.html" frameborder="0" scrolling="no" marginheight="0" marginwidth="0"></iframe>

En voici quelques exemples...

<!--break-->


```r
library(sparkline)
x <- rnorm(20)
sparkline(x)

```

<div style="width:100px;height:100px;overflow:hidden;">
  <iframe style="width:100px;height:100px;left:-20px;top:-20px;" src="https://antuki.github.io/figure/graph_html_test.html" />
</div>
