---
layout: post
comments : true
title: "L'Insee met en ligne le fichier des prénoms"
tags: [Open data et open source]
--- 

commentaire

<div style="position:relative; max-width: 100%; width:800px; height:0px; padding-bottom:65%;">
    <iframe style="position:absolute; left:0; top:0; width:100%; height:100%;max-width: 100%"
        src="https://antuki.github.io/figure/fichier_prenoms_html1.html">
    </iframe>
</div>

com1

<div style="position:relative; max-width: 100%; width:800px; height:400px; padding-bottom:65%;">
    <iframe style="position:absolute; left:0; top:0; width:100%; height:100%;max-width: 100%"
        src="https://antuki.github.io/figure/fichier_prenoms_html1.html">
    </iframe>
</div>

comA

<div style="position:relative; max-width: 100%; width:800px; height:400px;">
    <iframe style="position:absolute; left:0; top:0; width:100%; height:100%;max-width: 100%"
        src="https://antuki.github.io/figure/fichier_prenoms_html1.html">
    </iframe>
</div>


com2

<div style="position:relative; max-width: 100%; width:800px; height:400px; padding-bottom:400px;">
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
