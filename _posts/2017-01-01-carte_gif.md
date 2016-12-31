# 

```r
#Téléchargement des données de mortalité de la BDM
library(rsdmx)
```

```
## Warning: package 'rsdmx' was built under R version 3.3.2
```

```r
library(RJSDMX)
```

```
## Warning: package 'RJSDMX' was built under R version 3.3.2
```

```
## Loading required package: rJava
```

```
## Warning: package 'rJava' was built under R version 3.3.1
```

```
## Loading required package: zoo
```

```
## Warning: package 'zoo' was built under R version 3.3.2
```

```
## 
## Attaching package: 'zoo'
```

```
## The following objects are masked from 'package:base':
## 
##     as.Date, as.Date.numeric
```

```r
library(plyr)
```

```
## Warning: package 'plyr' was built under R version 3.3.2
```

```r
base_bdm <- "http://www.bdm.insee.fr/series/sdmx/data/FR1,TAUX-MORTALITE-AGE-DEP,1.0/65-."
base_bdm <- readSDMX(base_bdm) #on récupère un data.frame des données de la BDM
base_bdm <- as.data.frame(base_bdm)
base_bdm<- base_bdm[which(base_bdm$AGE=="65-"),c("DEPARTEMENT","OBS_VALUE","TIME_PERIOD")]
base_bdm$OBS_VALUE <- as.numeric(base_bdm$OBS_VALUE)
base_bdm$TIME_PERIOD <- as.numeric(base_bdm$TIME_PERIOD)


#Téléchargement du shape des départements d'OSM
lien_telechargement<-"http://osm13.openstreetmap.fr/~cquest/openfla/export/departements-20140306-100m-shp.zip"
download.file(lien_telechargement,destfile = "C:/Users/kantunez/Desktop/carto_osm.zip") #on télécharge
unzip(zipfile="C:/Users/kantunez/Desktop/carto_osm.zip", exdir="C:/Users/kantunez/Desktop/carto_osm") #on dézippe

#Chargement de la couche d'OSM
library(rgdal)
```

```
## Warning: package 'rgdal' was built under R version 3.3.1
```

```
## Loading required package: sp
```

```
## rgdal: version: 1.1-10, (SVN revision 622)
##  Geospatial Data Abstraction Library extensions to R successfully loaded
##  Loaded GDAL runtime: GDAL 2.0.1, released 2015/09/15
##  Path to GDAL shared files: C:/Users/kantunez/Documents/R/R-3.3.0/library/rgdal/gdal
##  Loaded PROJ.4 runtime: Rel. 4.9.2, 08 September 2015, [PJ_VERSION: 492]
##  Path to PROJ.4 shared files: C:/Users/kantunez/Documents/R/R-3.3.0/library/rgdal/proj
##  Linking to sp version: 1.2-3
```

```r
library(cartography)
```

```
## Warning: package 'cartography' was built under R version 3.3.2
```

```r
setwd("C:/Users/kantunez/Desktop/carto_osm/")
couche_dep <- readOGR(dsn = ".", layer = "departements-20140306-100m") #on importe la couche carto
```

```
## OGR data source with driver: ESRI Shapefile 
## Source: ".", layer: "departements-20140306-100m"
## with 101 features
## It has 4 fields
```

```r
setwd("C:/Users/kantunez/Desktop/")

#Suppression des fichiers téléchargés une fois chargés dans R
unlink("C:/Users/kantunez/Desktop/carto_osm.zip", recursive = FALSE, force = FALSE)
unlink("C:/Users/kantunez/Desktop/carto_osm", recursive = T, force=T)  #attention ne pas mettre de / à la fin

#Enlever les DOM de la carte qui sinon a une projection étrange
couche_dep <- subset(couche_dep,!substr(couche_dep@data$code_insee,1,2)=="97") 

#Cartographie finale
library(classInt)
breaks <- classIntervals(base_bdm[,"OBS_VALUE"], 5,style="jenks")$brks

#install.packages("animation")
library("animation")
```

```
## Warning: package 'animation' was built under R version 3.3.2
```

```r
Sys.setenv(path = "C:/Users/kantunez/Documents/ImageMagick-6.8.5-5/")
saveGIF({
  for (annee in 1998:2014){
    par(mar=c(0.5,0.5,0.5,0.5))
    choroLayer(couche_dep, base_bdm[which(base_bdm$TIME_PERIOD==annee),], spdfid = , dfid = "DEPARTEMENT", var="OBS_VALUE", breaks = breaks, col = carto.pal(pal1 = "turquoise.pal", n1 = 5) , 
               legend.title.txt = paste0("Taux de mortalité des\nplus de 65 ans (o/oo)\npar département en ",annee), 
               legend.pos = "topleft", add=F)
    layoutLayer(author = "antuki.github.io", sources = paste0("Insee, Etat civil ",annee), scale=NULL,
                frame=F, title=annee, col="white",coltitle="black") 
  } 
},convert="convert", movie.name="git_tauxmortalite.gif")
```

```
## Executing: 
## ""convert" -loop 0 -delay 100 Rplot1.png Rplot2.png Rplot3.png
##     Rplot4.png Rplot5.png Rplot6.png Rplot7.png Rplot8.png
##     Rplot9.png Rplot10.png Rplot11.png Rplot12.png Rplot13.png
##     Rplot14.png Rplot15.png Rplot16.png Rplot17.png
##     "git_tauxmortalite.gif""
```

```
## Output at: git_tauxmortalite.gif
```

```
## [1] TRUE
```
