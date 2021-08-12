# R básico aplicado a Sistemas de Información Geográfico en Google Colab

Jessica Daniela Ocaña Falcón

Este texto forma parte de la presentación desarrollada el 17 de agosto de 2021 para Rladies Guadalajara. También puedes encontrar las diapositivas aquí y los Jupyter Notebook completos en este mismo repositorio. El objetivo principal es dar una breve introducción a R y a los paquetes especializados en Sistemas de Información Geográfica en Google Colab. Un agradecimiento especial al Dr. Ivvan Valdez y a Centro Geo por la capacitación y asesoría, así como la información que compartieron y usaron en la Escuela de Verano Centro Geo 2021. Compartiendo el conocimiento, crecemos como seres humanos.

Para empezar, vamos a cargar la extensión que nos permite ejecutar código de R en Colab:

    %load_ext rpy2.ipython

Luego, tenemos que indicar que esta es una "cell-magic" que usa R, y en cada celda mágica se tiene que poner el simbolo de R:
           
    %%R

### Librerías para mapas vectoriales y raster

Utilizaremos las librerías para manejo de archivos de mapas vectoriales y raster
Vamos a necesitar los siguientes paquetes: 

    %%R
    install.packages("rgdal")
    install.packages("raster")
    install.packages("rgeos")
    library(rgdal)
    library(raster)
     
rgdal: nos permite leer archivos shape. 
raster: nos permite leer archivos raster.

### Descarga de los datos a tu drive

¿Qué hace gdown? La función gdown nos ayuda a descargar los datos públicos de drive del Dr. Ivvan hacia mi drive o hacia el tuyo.

    %%R
    #Archivo INEGI_Entidad_.shx
    system("gdown --id 1QwuDgUbqEbm5hbJhzyLdREZxWQ3DIf62")#https://drive.google.com/file/d/1QwuDgUbqEbm5hbJhzyLdREZxWQ3DIf62/view?usp=sharing
    #Archivo INEGI_Entidad_.shp
    system("gdown --id 1CgPFApI2gKhZ91sUAtlJ37qXvVT5oA47")#https://drive.google.com/file/d/1CgPFApI2gKhZ91sUAtlJ37qXvVT5oA47/view?usp=sharing
    #Archivo INEGI_Entidad_.dbf
    system("gdown --id 12Dv6PpmUKkqmsNm6Q4Q93ekJM4wzWasT")#https://drive.google.com/file/d/12Dv6PpmUKkqmsNm6Q4Q93ekJM4wzWasT/view?usp=sharing
    
### Usando GDAL para leer un shape

La función readOGR leé el shape y carga la base de datos en un objeto.
La librería rgdal sobrecarga la funcion "plot", para que pueda graficar los shapes.

    %%R
    #Cargamos el shape
    shapeMex <- readOGR("./INEGI_Entidad_.shp")
    #Graficamos el shape
    plot(shapeMex)


### Ver que campos contiene la base de datos y extraer un submapa

Vemos los nombre de campos
Vemos los valores "únicos" en un campo
Extraemos una parte del mapa filtrando la base de datos.

    %%R
    #Vemos como se llaman los campos
    print(names(shapeMex))
    #Con lo de arriba ya vimos que uno de los campos es "NOMBRE"
    print(unique(shapeMex$NOMBRE))
    #Con lo de arriba vimos que un elemento es "Colima"
    #Extraemos el subshape
    shapeColima=shapeMex[shapeMex$NOMBRE=="Colima",]
    #Graficamos el mapa de colima extraido
    plot(shapeColima)
    print(shapeColima)

### Recortar una parte del shape

El mapa de colima tiene algunas islas que no son de interés para la tarea. Las vamos a recortar haciendo un rectangulo alrededor de la parte continental.

    %%R
    # print(shapeColima) #Sacamos los datos de extensión de shape
    #Hacemos un rectangulo mas o menos que cubra la parte continental de Colima
    pol=extent(cbind(c(-110,-103.4863),c(18.33886, 19.51252)))
    #Recortamos la intersección entre el rectangulo y 
    shapeTerraColima=crop(shapeColima,pol)
    plot(shapeTerraColima)

### Guardamos el shape

Por último, vamos a almacenar el shape que acabamos de recortar

    %%R
    writeOGR(obj=shapeTerraColima,layer="./colima",dsn=".",driver="ESRI Shapefile", overwrite_layer=TRUE)

---
