---
layout: post
comments : true
title: "L'Insee met en ligne le fichier des prénoms"
tags: [DataViz, Open data et open source]
--- 

En ce début d'année 2017, l'Insee a mis en ligne le fichier des prénoms. Il contient les sexes et prénoms attribués aux enfants nés en France entre 1900 et 2015. Les données sont disponibles au niveau national mais également par département ! Rendez-vous [ici](https://www.insee.fr/fr/statistiques/2540004) et amusez-vous bien !  

<div style="position:relative; max-width: 100%; width:800px; height:0px; padding-bottom:65%;">
    <iframe style="position:absolute; left:0; top:0; width:100%; height:100%;max-width: 100%"
        src="https://antuki.github.io/figure/fichier_prenoms_html1.html">
    </iframe>
</div>

<!--break-->

```r
library(foreign)
library(dygraphs)

#Téléchargement du fichier sur le site de l'Insee et suppression une fois chargé dans R
lien_telechargement<-"https://www.insee.fr/fr/statistiques/fichier/2540004/dpt2015.zip"
tmp <- tempdir() # création d'un dossier temporaire
download.file(lien_telechargement,destfile = paste0(tmp,"\\","dpt2015.zip")) #on télécharge 
unzip(zipfile=paste0(tmp,"\\","dpt2015.zip"), exdir=paste0(tmp,"\\","dpt2015")) #on dézippe 
dpt2015 <- read.dbf(paste0(tmp,"\\","dpt2015/dpt2015.dbf"), as.is = FALSE) 
#unlink(paste0(tmp,"\\","dpt2015.zip"), recursive = FALSE, force = FALSE) #non utile car dossier temporaire
#unlink(paste0(tmp,"\\","dpt2015"), recursive = T, force=T)  #attention ne pas mettre de / à la fin #non utile car dossier temporaire


#install.packages("dygraphs")


data_KIM <- dpt2015[which(dpt2015$preusuel=="KIM" & dpt2015$annais!="XXXX" & dpt2015$sexe==2),]
test <- aggregate(nombre ~ annais, data = data_KIM, sum)
test$annais <- as.numeric(as.character(test$annais))

dygraph(test, main = "Nombre de petites Kim nées en France") %>%
  dyAxis("x", drawGrid = FALSE) %>%
  dySeries(c("nombre"), label = "Kim") %>%
  dyOptions(colors = "#2AA198", stackedGraph = TRUE) 
```
