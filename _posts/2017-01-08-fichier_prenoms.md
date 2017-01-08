---
layout: post
comments : true
title: "L'Insee met en ligne le fichier des prénoms"
tags: [Open data et open source]
--- 

commentaires 

<div style="position:relative;width:1000px;height:1000px;overflow:hidden;"><iframe scrolling="no" style="position:absolute;width:1000px;height:1000px;left:-41px;top:-41px;" src="https://antuki.github.io/figure/fichier_prenoms_html1.html"></iframe></div>


<iframe scrolling="no" src="https://antuki.github.io/figure/fichier_prenoms_html1.html"></iframe>

test2

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
