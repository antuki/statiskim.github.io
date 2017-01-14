---
layout: post
comments : true
title: "L'Insee met en ligne le fichier des prénoms"
tags: [Open data et open source]
--- 

commentaires 

<iframe frameborder="0" height="600" src="https://antuki.github.io/figure/fichier_prenoms_html1.html" width="100%" align="center"></iframe>

commentaires 

<iframe frameborder="0" height="100%" src="https://antuki.github.io/figure/fichier_prenoms_html1.html" width="100%" align="center"></iframe>

com

<iframe frameborder="0" height="auto" src="https://antuki.github.io/figure/fichier_prenoms_html1.html" width="100%" align="center"></iframe>

com3 

<div style="position:relative; width:100%; height:0px; padding-bottom:56.25%;">
    <iframe style="position:absolute; left:0; top:0; width:100%; height:100%"
        src="https://antuki.github.io/figure/fichier_prenoms_html1.html">
    </iframe>
</div>

com3bis 

<div style="position:relative; width:100%; height:0px; padding-bottom:80%;">
    <iframe style="position:absolute; left:0; top:0; width:100%; height:100%"
        src="https://antuki.github.io/figure/fichier_prenoms_html1.html">
    </iframe>
</div>



com4

<iframe src="https://antuki.github.io/figure/fichier_prenoms_html1.html" style="resize: both;"               
        onload="this.style.height=this.contentDocument.body.scrollHeight +'px'; this.style.width=this.contentDocument.body.scrollWidth +'px';"
        onresize="this.style.height=this.contentDocument.body.scrollHeight +'px'; this.style.width=this.contentDocument.body.scrollWidth +'px';">
</iframe>

<!--break-->

```r
library(foreign)
library(dygraphs)

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
