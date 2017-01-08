# 

commentaires 


```r
library(foreign)
library(dygraphs)
```

```
## Warning: package 'dygraphs' was built under R version 3.3.2
```

```r
#Téléchargement du fichier sur le site de l'Insee et suppression une fois chargé dans R
lien_telechargement<-"https://www.insee.fr/fr/statistiques/fichier/2540004/dpt2015.zip"
travail <- "C:/Users/kantunez/Desktop/" #pensez à changer votre espace de travail
download.file(lien_telechargement,destfile = paste0(travail,"dpt2015.zip")) #on télécharge
unzip(zipfile=paste0(travail,"dpt2015.zip"), exdir=paste0(travail,"dpt2015")) #on dézippe
dpt2015 <- read.dbf(paste0(travail,"dpt2015/dpt2015.dbf"), as.is = FALSE) 
unlink(paste0(travail,"dpt2015.zip"), recursive = FALSE, force = FALSE)
unlink(paste0(travail,"dpt2015"), recursive = T, force=T)  #attention ne pas mettre de / à la fin


#install.packages("dygraphs")


data_KIM <- dpt2015[which(dpt2015$preusuel=="KIM" & dpt2015$annais!="XXXX" & dpt2015$sexe==2),]
test <- aggregate(nombre ~ annais, data = data_KIM, sum)
test$annais <- as.numeric(as.character(test$annais))

dygraph(test, main = "Nombre de petites Kim nées en France") %>%
  dyAxis("x", drawGrid = FALSE) %>%
  dySeries(c("nombre"), label = "Kim") %>%
  dyOptions(colors = "#2AA198", stackedGraph = TRUE) 
```

<!--html_preserve--><div id="htmlwidget-3ed3a0636c4233ced71a" style="width:672px;height:480px;" class="dygraphs html-widget"></div>
<script type="application/json" data-for="htmlwidget-3ed3a0636c4233ced71a">{"x":{"attrs":{"axes":{"x":{"pixelsPerLabel":60,"drawGrid":false,"drawAxis":true},"y":{"drawAxis":true}},"title":"Nombre de petites Kim nées en France","labels":["annais","Kim"],"legend":"auto","retainDateWindow":false,"series":{"Kim":{"axis":"y"}},"stackedGraph":true,"fillGraph":false,"fillAlpha":0.15,"stepPlot":false,"drawPoints":false,"pointSize":1,"drawGapEdgePoints":false,"connectSeparatedPoints":false,"strokeWidth":1,"strokeBorderColor":"white","colors":["#2AA198"],"colorValue":0.5,"colorSaturation":1,"includeZero":false,"drawAxesAtZero":false,"logscale":false,"axisTickSize":3,"axisLineColor":"black","axisLineWidth":0.3,"axisLabelColor":"black","axisLabelFontSize":14,"axisLabelWidth":60,"drawGrid":true,"gridLineWidth":0.3,"rightGap":5,"digitsAfterDecimal":2,"labelsKMB":false,"labelsKMG2":false,"labelsUTC":false,"maxNumberWidth":6,"animatedZooms":false,"mobileDisableYTouch":true},"annotations":[],"shadings":[],"events":[],"format":"numeric","data":[[1956,1957,1958,1959,1963,1965,1967,1971,1972,1978,1979,1980,1981,1982,1983,1984,1985,1986,1987,1988,1989,1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015],[3,3,4,4,3,6,5,3,3,3,8,8,3,20,14,13,34,17,50,71,80,92,117,123,161,113,88,78,76,56,80,95,50,26,61,68,97,99,90,104,97,120,180,153,147,152,146]],"fixedtz":false,"tzone":""},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
