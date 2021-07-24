# R básico aplicado a Sistemas de Información Geográfico en Google Colab
## Jessica Daniela Ocaña Falcón
## Universidad Juárez Autónoma de Tabasco
## 24/07/2021

Este texto forma parte de la presentación desarrollada el xx de agosto de 2021 para Rladies Puebla. También puedes encontrar las diapositivas aquí y los Scripts completos en esta carpeta. El objetivo principal es dar una breve introducción a R y a los paquetes especializados en Sistemas de Información Geográfica en Google Colab.

Para empezar, vamos a cargar la extensión que nos permite ejecutar código de R en Colab:

    %load_ext rpy2.ipython

Luego, tenemos que indicar que esta es una "cell-magic" que usa R, y en cada celda mágica se tiene que poner el simbolo de R:
           
    %%R

Utilizaremos las librerías para manejo de archivos de mapas vectoriales y raster
Vamos a necesitar los siguientes paquetes: 

    %%R
    install.packages("rgdal")
    install.packages("raster")
    install.packages("rgeos")
    library(rgdal)
     library(raster)
     
rgdal: nos permite leer archivos shape. raster: nos permite leer archivos raster.
---
