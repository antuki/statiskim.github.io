---
layout: post
comments : true
title: "Faire des cartes lissées sur R"
tags: [S'amuser sur R, Méthodo]
--- 

Quelques chercheurs de Géographie-Cités et de l'UMS-RIATE développent depuis quelques années le package *cartography* permettant de réaliser très facilement des cartes sur R. Ils développent actuellement sur [leur compte GitHub](https://github.com/Groupe-ElementR/cartography), une nouvelle fonction *smoothLayer* s'appuyant sur leur package *SpatialPosition* permettant de faire des cartes lissées comme celles que je viens de réaliser.   

![](https://antuki.github.io/figure/cartes_lissees_fig1.png){: style="display: block; max-width: 100%;  height: auto; margin: auto; float:none!important "}

Amusez-vous bien et n'hésitez pas à faire un tour sur le [blog de Thimothée Giraud](https://rgeomatic.hypotheses.org/author/rgeomatic) qui publie régulièrement des articles intéressants sur les méthodes de carto sur R ! 

<!--break-->

Explication de la méthode des cartes lissées.

```r
rm(list=ls())
setwd("C:/Users/Desktop/Dossier/") #à modifier par l'utilisateur 

############ CHARGEMENT DES DONNEES DE POPULATION ET DE SUPERFICIE ######################
#Téléchargement du fichier des populations légales du site de l'Insee dans un dossier temporaire
lien_telechargement<-"https://www.insee.fr/fr/statistiques/fichier/2128166/base-cc-serie-historique.zip"
tmp <- tempdir() # création d'un dossier temporaire
download.file(lien_telechargement,destfile = paste0(tmp,"\\","pop.zip")) #on télécharge
unzip(zipfile=paste0(tmp,"\\","pop.zip"), exdir=paste0(tmp,"\\","pop")) #on dézippe

#Import de fichiers Excel sous R
#install.packages("XLConnectJars")
#install.packages("XLConnect")
options(java.parameters = "-Xmx200g")
library(XLConnectJars)
library(XLConnect)
wb <- loadWorkbook(paste0(tmp,"\\pop","\\","base-cc-serie-historique.xls"), create = FALSE) #lecture du fichier
bdd <- readWorksheet(wb, sheet = "COM_2013", startRow =6, startCol = 1)[,c("CODGEO","P13_POP","SUPERF")] #transformation d'une feuille de classeur en data.frame
rm(wb) #on libère un peu de mémoire
bdd <- bdd[which(substr(bdd$CODGEO,1,2)%in%c("75","77","78","91","92","93","94","95")),] #On ne garde que les communes d'Île-de-France
bdd$densite <- bdd$P13_POP/bdd$SUPERF

############ CHARGEMENT DE LA COUCHE COMMUNALE ######################

#Téléchargement du shape des départements d'OSM
lien_telechargement<-"http://osm13.openstreetmap.fr/~cquest/openfla/export/communes-20150101-100m-shp.zip"
tmp <- tempdir() # création d'un dossier temporaire
download.file(lien_telechargement,destfile = paste0(tmp,"\\","carto_osm.zip")) #on télécharge
unzip(zipfile=paste0(tmp,"\\","carto_osm.zip"), exdir=paste0(tmp,"\\","carto_osm")) #on dézippe

#Chargement de la couche d'OSM
library(devtools)
#devtools::install_github("Groupe-ElementR/cartography")
library(cartography)
library(SpatialPosition)
library(rgdal)
library(classInt)

setwd(paste0(tmp,"\\","carto_osm"))
couche_com <- readOGR(dsn = ".", layer = "communes-20150101-100m") #on importe la couche carto
#Ne garder que les départements d'Île-de-France
couche_com <- subset(couche_com,substr(couche_com@data$insee,1,2)%in%c("75","77","78","91","92","93","94","95")) 
couche_com@data$insee <- as.character(couche_com@data$insee)
couche_com@data[which(substr(couche_com@data$insee,1,2)=="75"),"insee"]<-"75056" #Les données de Paris d'OSM sont codés par arrondissement

breaks <- classIntervals(bdd$densite, 8, style = "quantile")$brks #On définit les bornes : 8 classes par méthode des quantiles

par(mar=c(1,1,1,1), mfrow=c(2,2))

#Carte de densité par commune
choroLayer(spdf=couche_com, df=bdd, spdfid = "insee", dfid = "CODGEO", var="densite", border=NA,breaks=breaks,legend.title.txt = "densité (par commune)",legend.pos = "topleft")

#Carte de densité lissée avec la méthode exponentielle au carré et un paramètre "span" = 1000
smoothLayer(spdf=couche_com, df=bdd, spdfid = "insee", dfid = "CODGEO", var="P13_POP", var2 = "SUPERF",
            mask=couche_com,typefct = "exponential", span=1000, beta=2,breaks=breaks,border=NA,legend.title.txt = "densité lissée (exponentielle^2 et span=1000)",legend.pos = "topleft")

#Carte de densité lissée avec la méthode exponentielle au carré et un paramètre "span" = 2000
smoothLayer(spdf=couche_com, df=bdd, spdfid = "insee", dfid = "CODGEO", var="P13_POP", var2 = "SUPERF",
            mask=couche_com,typefct = "exponential", span=2000, beta=2,breaks=breaks,border=NA,legend.title.txt = "densité lissée (exponentielle^2 et span=2000)",legend.pos = "topleft")

#Carte de densité lissée avec la méthode exponentielle au carré et un paramètre "span" = 5000
smoothLayer(spdf=couche_com, df=bdd, spdfid = "insee", dfid = "CODGEO", var="P13_POP", var2 = "SUPERF",
            mask=couche_com,typefct = "exponential", span=5000, beta=2,breaks=breaks,border=NA,legend.title.txt = "densité lissée (exponentielle^2 et span=5000)",legend.pos = "topleft")
```

