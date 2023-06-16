
# Geoprocesamiento en nube {#geoproces}

*Dominic Royé*

Fundación de la Investigación del Clima

## Introducción

Cuando se plantea un problema basado en datos desde diversos proveedores, habitualmente implica la descarga de grandes volúmenes. La actual proliferación de servicios de Open Data, despliegues de sensores y diversas fuentes incluyendo los satélites dificulta su procesamiento en equipos personales. El gran crecimiento en grandes volúmenes de datos espacio-temporales de tipo vectorial o ráster lleva a la necesidad en trabajar con servicios en nube para ahorrar tiempo computacional y espacio de almacenamiento. En la actualidad existen diferentes servicios de \index{geoprocesamiento} geoprocesamiento en \index{nube} nube que ayudan a hacer análisis online sin necesidad de descargar los datos ni preocuparse por el rendimiento computacional. Uno de estos servicios es \index{google earth engine, gee} *Google Earth Engine* (GEE), donde se combina un catálogo de varios petabytes de imágenes satelitales y conjuntos de datos geoespaciales multidimensionales (vectorial y ráster) de alta resolución con capacidades de análisis a escala planetaria. Este servicio gratuito para uso no comercial incluye incluso la posibilidad en crear aplicaciones. 

GEE consiste en una API con bibliotecas de cliente para JavaScript y Python, que traducen los \index{análisis! geoespacial} análisis geoespaciales  y hacen posible acceder a los datos. No es necesario descargar grandes volúmenes de datos ni configurar la computación. Para el lenguaje de R se puede hacer uso del paquete `rgee` que hace puente entre **R** y la API GEE. 

Se hará uso del dataset con el nombre "NOAA CDR OISST v02r01", una interpolación óptima diaria de temperatura de la superficie del mar (OISST, por sus siglas en inglés) con una resolución de 1/4 grados (27 km). Los datos los proporciona la National Oceanic and Atmospheric Administration (NOAA) con campos completos de temperatura del océano construidos mediante la combinación de observaciones ajustadas por sesgo de diferentes plataformas (satélites, barcos, boyas) en una cuadrícula global regular, con lagunas estimadas por interpolación (https://developers.google.com/earth-engine/datasets/catalog/NOAA_CDR_OISST_V2_1) (@reynolds2008). 

El objetivo de este capítulo es mostrar el potential del uso de APIs directamente en **R**. El resultado se empleará en el Cap. \ref{cambioclimatico}. 



## Sintaxis de Google Earth Engine

Con ayuda de GEE se preprocesan los datos de tal manera que el resultado son las anomalías estivales en forma de ráster para cada año entre 1981 y 2022. El primer paso consiste en crear el usuario en earthengine.google.com. Además, es necesario instalar *CLI* de *gcloud* (https://cloud.google.com/sdk/docs/install?hl=es-419). 

Antes se deben conocer algunos conceptos fundamentales sobre la sintaxis en GEE. En general, el lenguaje nativo es Javascript el que se caracteriza por la forma combinando funciones y variables usando el punto, el que se sustuye por el **$** en R. Todas las funciones GEE en **R** empiezan por el prefijo ee_* (`ee_print()`, `ee_image_to_drive()`). Los términos más relevantes son los siguientes:

* *ImageCollection*: serie temporal de imágenes.
* *Geometry*: dato vectorial.
* *Functions*: `map()` aplica funciones sobre *ImageCollections*, `ee.Date()` define una fecha, `filterDate()` permite filtrar por fecha una *ImageCollection*, `select()` selecciona una banda, etc. 

Muchas funciones son similares a las de `tidyverse`.

Se puede obtener más ayuda en https://r-spatial.github.io/rgee/reference/rgee-package.html y en la propia página de GEE. 

## Primeros pasos

Después de darse de alta en GEE y haber instalado *CLI gcloud* en el sistema operativo, se crea un entorno virtual de Python con todas las dependencias de GEE usando la función `ee_install()`.


```r
library(rgee)

ee_install() # crear entorno virtual de Python; ¡sólo una vez!
ee_check() # comprobar si todo está correcto
```

Antes de pasar a programar con la sintaxis propia de GEE, se debe autenticar e inicializar GEE empleando la función `ee_Initialize()`.


```r
ee_Initialize(drive = TRUE) # autenticar e inicializar GEE
ee_user_info() # inf sobre usuario
```

Hay que tener en cuenta que, únicamente cuando se envían tareas, GEE ejecuta el cálculo en los servidores enviando todos los objetos creados. En la mayoría de los pasos se crean objetos *EarthEngine*, que se usan una vez que se construyó un mapa interactivo, la exportación o la impresión en consola de un objeto. 

Por ejemplo, se puede selecionar la banda NDVI del producto MODIS MOD13A2 e imprimir los metadatos del primer día disponible con `ee_print()`. Existe un límite 5000 elementos que se podrían ver usando esta función. 


```r
# imageCollection NDVI
img <- ee$ImageCollection('MODIS/006/MOD13A2')$select('NDVI')

# metadatos
ee_print(img$first())
  
```

## Cálculo de anomalias 

### Definiciones previas 

Los datos NOAA CDR OISST contienen la \index{temperatura superficial} temperatura superficial de los océanos a nivel global, por eso, se fija un rectángulo que cubre la extensión del Mar Mediterráneo como objeto de estudio.


```r
# extensión del Mar Mediterráneo
geom <- ee$Geometry$Polygon(coords = list(
    c(-6.046418548121442, 46.733937391710846), 
    c(-6.046418548121442, 29.680544334046786),
    c(42.469206451878556, 29.680544334046786),
    c(42.469206451878556, 46.733937391710846)
    ),
    proj = "EPSG:4326",
    geodesic = FALSE)

geom #vemos que es un objeto EarthEngine de tipo geometría
str(geom) # construcción javascript
```

En el siguiente paso se define el período de interés, desde el año 1982 hasta el 2022. 


```r
startDate <- ee$Date('1982-01-01') # fecha inicio
endDate <- ee$Date('2023-01-16') # fecha final
```

Se puede acceder a todas las \index{colecciones} colecciones (*ImageCollection*) indicando su identifcación. Además, se filtran y se recortan los datos con respecto al periodo y a la extensión fijada. Finalmente se selecciona la banda o variable de de interés "sst" (*surface sea temperature*). 


```r
collection_era5 <- ee$ImageCollection("NOAA/CDR/OISST/V2_1")$
                    filterDate(startDate, endDate)$
                      filterBounds(geom)$
                       select('sst')
```

Finalmente, se procede a calcular el número de años en el período fijado. 


```r
numberOfyears <- endDate$difference(startDate, 'years')$round()
```

### Promedio estival 

Después de las anteriores definiciones se crea una nueva colección con el promedio estival de cada año durante el periodo objeto de estudio. Para ello se crea una lista de los años sobre la que se mapea otra función. Esta función se debe pasar con  `ee_utilspyfunc()`, que traduce una función **R** a una de Python. 

En la función personalizada se filtran los meses de verano, se calcula el promedio y se multiplica por 0,01, un factor de escala. Cuando se crean nuevas colecciones es importante fijar la nueva fecha con `set()`. 


```r
yearly <- ee$ImageCollection(
  ee$List$sequence(0, numberOfyears$subtract(1L))$ 
  map(ee_utils_pyfunc(function(dayOffset) {
    yr = startDate$advance(dayOffset, 'years')$get('year')
    start = ee$Date$fromYMD(yr, 12L, 1L)
    end = ee$Date$fromYMD(yr$add(1L), 2L, 28L)
    return(collection_era5$
    filterDate(start, end)$
    mean()$
    multiply(0.01)$
    set('system:time_start', start$millis()))
  }))
)
```

En el siguiente paso se calcula la temperatura media estival entre 1982 y 2010, como período de referencia para las anomalías. 


```r
msst <- collection_era5$filterDate('1982-01-01','2010-12-31')$
         filter(ee$Filter$calendarRange(12L,2L,'month'))$
          mean()$
            multiply(0.01)

```

Se aplica otra función personalizada sobre las medias estivales de todos los años, en la que se resta la temperatura del periodo de referencia, obteniéndose así las diferencias entre la temperatura media de cada año en el periodo estival y la temperatura media global del periodo estival en el periodo 1982-2010. 


```r
anom <- yearly$map(ee_utils_pyfunc(function (im) {
  return(im$subtract(msst)$set('system:time_start',           
                         im$get('system:time_start')))
}))
```

Es puede crear un mapa interactivo de un año concreto aplicando la función `Map.addLayer()` (Fig.  \@ref(fig:interactive-map-plot)). En este paso es la primera vez que GEE calcula lo que se ha creado anteriormente. 


```r
# metadatos 
ee_print(anom$first()) # año 1982

# mapa interactiva del año 1982
Map$setCenter(9, 40, 5) # centrar mapa en el mediterráneo con nivel de zoom 5

# crear mapa con leyenda
Map$addLayer(
  eeObject = anom$first(),
  visParams = list(
    palette = rev(RColorBrewer::brewer.pal(11, "RdBu")),
    min = -3,
    max = 3
  ),
  name = "MED_SST"
) + 
Map$addLegend(list(min = -3, max = 3, 
                    palette = rev(RColorBrewer::brewer.pal(11, "RdBu"))), 
              name = "SST Anomaly", 
              position = "bottomright", 
              bins = 4)
```


<div class="figure" style="text-align: center">
<img src="img/interactivo_mapa_geopro.png" alt="Mapa interactivo del mar Mediterraneo (1982)" width="60%" />
<p class="caption">(\#fig:interactive-map-plot)Mapa interactivo del mar Mediterraneo (1982)</p>
</div>



Hasta este momento, no se ha enviado una tarea para que GEE la realice. Para ello hay que exportar las anomalías de cada año en formato `geotiff`. La función `ee_imagecollection_to_local()` facilita la exportación de todas las capas de una *ImageCollection*. En cambio, la función `ee_image_to_drive()` exporta datos individuales de una única imagen a *Google Drive*. El argumento *scale* indica con qué resolución se exporta. Aunque la resolución de los datos originales es de 27 km, se fija una resolución de 2 km, lo que implica una interpolación en la exportación por razones de estética en la visualización. 


```r
tmp <- tempdir() # carpeta temporal o cualquier otra ruta

ic_drive_files_2 <- ee_imagecollection_to_local(
  ic = anom,
  region = geom,
  fileFormat = "GEO_TIFF",
  scale = 2000,
  lazy = FALSE,
  dsn = file.path(tmp, "rast_"), # prefijo del archivo
  add_metadata = TRUE
)
```

Este ejemplo ha mostrado una pequeña parte de la capacidad del geoprocesamiento en manejar grandes volúmenes de datos sin que implique su descarga ni el cálculo localmente. Pero también es posible manejar datos vectoriales u otros datos de alta resolución. Incluso se puede llegar a crear apliaciones basadas en GEE. 

::: {.infobox_resume data-latex=""}
### Resumen {-}

El crecimiento de volúmenes de datos en la actualidad lleva a la necesidad de emplear servicios de geoprocesamiento en nube que ayuden a hacer análisis online sin necesidad de descargar los datos ni preocuparse por el rendimiento computacional. Uno de estos servicios es *Google Earth Engine*, donde se combina un catálogo de imágenes satelitales y conjuntos de datos geoespaciales multidimensionales de alta resolución con capacidades de análisis a escala planetaria. En este ejemplo se ha mostrado cómo procesar la temperatura superficial del mar calculando las anomalías estivales de la cuenca mediterránea desde 1982 a 2022. 
:::
