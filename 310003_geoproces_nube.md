
# Geoprocesamiento en nube {#geoproces}

*Dominic Royé*

Fundación de la Investigación del Clima

## Introducción

El planteamiento de un problema basado en datos de diversos proveedores habitualmente implica la descarga de grandes volúmenes de datos. La actual proliferación de servicios de Open Data, despliegues de sensores y diversas fuentes, incluyendo los satélites, dificulta su procesamiento en equipos personales. El gran crecimiento en grandes volúmenes de datos espaciotemporales de tipo vectorial o *raster* lleva a la necesidad en trabajar con servicios en nube para ahorrar tiempo computacional y espacio de almacenamiento. En la actualidad existen diferentes servicios de \index{geoprocesamiento} geoprocesamiento en \index{nube} nube que ayudan a hacer análisis online sin necesidad de descargar los datos ni preocuparse por el rendimiento computacional. Uno de estos servicios es \index{Google Earth Engine, GEE} Google Earth Engine (GEE), donde se combina un catálogo de varios petabytes de imágenes satelitales y conjuntos de datos geoespaciales multidimensionales (vectorial y *raster*) de alta resolución con capacidades de análisis a escala planetaria. Este servicio gratuito para uso no comercial incluye incluso la posibilidad de crear aplicaciones. 

GEE consiste en una API con bibliotecas de cliente para JavaScript y Python que traducen (a la lengua nativa que es JavaScript) los \index{análisis! geoespacial} análisis geoespaciales y hacen posible acceder a los datos. No es necesario descargar grandes volúmenes de datos ni configurar la computación. Para el lenguaje de **R** se puede hacer uso del paquete `rgee`, que hace puente entre **R** y la API GEE. 

En este capítulo se utiliza el conjunto de datos "NOAA CDR OISST v02r01", una interpolación óptima de la temperatura diaria de la superficie del mar (OISST, por sus siglas en inglés) con una resolución de 1/4 grados (27 km). Los datos los proporciona la National Oceanic and Atmospheric Administration (NOAA); son campos completos de temperatura oceánica construidos mediante la combinación de observaciones ajustadas por sesgo de diferentes plataformas (satélites, barcos, boyas) en una cuadrícula global regular, con lagunas estimadas por interpolación. Los datos satelitales del radiómetro avanzado de muy alta resolución (AVHRR, por sus siglas en inglés) proporcionan la entrada principal que permite la alta cobertura temporal-espacial desde finales de 1981 hasta el presente[^geoproc-web] [@reynolds2008]. 

Dicho lo anterior, el objetivo de este capítulo es mostrar el potencial del uso de las API directamente en **R**. El resultado se empleará en el Cap. \@ref(cambioclimatico). 

[^geoproc-web]: Véase https://developers.google.com/earth-engine/datasets/catalog/NOAA_CDR_OISST_V2_1.




## Sintaxis de Google Earth Engine

Con ayuda de GEE se preprocesan los datos de tal manera que el resultado son las anomalías estivales en forma de *raster* para cada año entre 1981 y 2022. El primer paso consiste en crear el usuario en [earthengine.google.com](earthengine.google.com). Además, es necesario instalar CLI de Gcloud (https://cloud.google.com/sdk/docs/install?hl=es-419). 

No obstante, antes se deben conocer algunos conceptos fundamentales sobre la sintaxis en GEE. En general, el lenguaje nativo es Javascript, que se caracteriza por una gramática en la que se combinan funciones y variables usando el punto, que se sustituye por el "$" en **R**. Todas las funciones de GEE en **R** empiezan por el prefijo ee_* (`ee_print()`, `ee_image_to_drive()`). Los términos más relevantes son los siguientes:

* *ImageCollection*: serie temporal de imágenes.
* *Geometry*: dato vectorial.
* *Functions*: `map()` aplica funciones sobre *ImageCollections*, `ee.Date()` define una fecha, `filterDate()` permite filtrar por fecha una *ImageCollection*, `select()` selecciona una banda, etc. 

Muchas funciones son similares a las de `tidyverse`.

Se puede obtener más información en https://r-spatial.github.io/rgee/reference/rgee-package.html y en la propia página de GEE. 

## Primeros pasos

Después de darse de alta en GEE y haber instalado CLI Gcloud en el sistema operativo, se crea un entorno virtual de Python con todas las dependencias de GEE usando la función `ee_install()`.


```r
library(rgee)

ee_install() # crear entorno virtual de Python; ¡solo una vez!
ee_check()   # comprobar si todo está correcto
```

Antes de pasar a programar con la sintaxis propia de GEE, se debe autenticar e inicializar GEE empleando la función `ee_Initialize()`.


```r
ee_Initialize(drive = TRUE) # autenticar e inicializar GEE
ee_user_info() # inf sobre usuario
```

Hay que tener en cuenta que, únicamente cuando se envían tareas, GEE ejecuta el cálculo en los servidores enviando todos los objetos creados. En la mayoría de los pasos se crean objetos *EarthEngine* sin ejecutarse, que se ejecutan en las últimas fases del análisis.


Por ejemplo, se puede seleccionar la banda NDVI del producto MODIS MOD13A2 e imprimir los metadatos del primer día disponible con `ee_print()`. El número de elementos que se puede ver con esta función está limitado a 5.000. 


```r
# imageCollection NDVI
img <- ee$ImageCollection('MODIS/006/MOD13A2')$select('NDVI')

# metadatos
ee_print(img$first())
  
```

## Cálculo de anomalías en la temperatura del mar Mediterráneo 

### Definiciones previas 

Los datos NOAA CDR OISST contienen la \index{temperatura superficial} temperatura oceánica superficial para todo el globo terráqueo; por eso, se fija un rectángulo que cubre la extensión del mar Mediterráneo como objeto de estudio.


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

En el siguiente paso se define el período de interés: 1982-2022. 


```r
startDate <- ee$Date('1982-01-01') # fecha inicio
endDate <- ee$Date('2023-01-16') # fecha final
```

Se puede acceder a todas las \index{colecciones} colecciones (*ImageCollection*) indicando su identifcación. Además, se filtran y se recortan los datos con respecto al período y a la extensión fijada. Finalmente se selecciona la banda o variable de interés `sst` (*surface sea temperature*). 


```r
collection_era5 <- ee$ImageCollection("NOAA/CDR/OISST/V2_1")$
                    filterDate(startDate, endDate)$
                      filterBounds(geom)$
                       select('sst')
```

Por último, se procede a calcular el número de años en el período fijado. 


```r
numberOfyears <- endDate$difference(startDate, 'years')$round()
```

### Promedio estival 

Después de las anteriores definiciones se crea una nueva colección con el promedio estival de cada año durante el período objeto de estudio. Para ello se crea una lista de los años sobre la que se mapea otra función. Esta función se debe pasar con  `ee_utilspyfunc()`, que traduce una función **R** a una de Python. 

En la función personalizada se filtran los meses de verano, se calcula el promedio de sus valores diarios y se multiplica por 0,01, un factor de escala. Cuando se crean nuevas colecciones es importante fijar la nueva fecha con `set()`. 


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

Se aplica otra función personalizada sobre las medias estivales de todos los años en la que se resta la temperatura del período de referencia, obteniéndose así las diferencias entre la temperatura media de cada año en el período estival y la temperatura media global del período estival en 1982-2010. 


```r
anom <- yearly$map(ee_utils_pyfunc(function (im) {
  return(im$subtract(msst)$set('system:time_start',           
                         im$get('system:time_start')))
}))
```

Se puede crear un mapa interactivo de un año concreto aplicando la función `Map.addLayer()` (Fig.  \@ref(fig:interactive-map-plot)). En este paso es la primera vez que GEE calcula lo que se ha creado anteriormente. 


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
<img src="img/interactivo_mapa_geopro.png" alt="Mapa interactivo del mar Mediterráneo (1982)." width="60%" />
<p class="caption">(\#fig:interactive-map-plot)Mapa interactivo del mar Mediterráneo (1982).</p>
</div>



Hasta este momento no se ha enviado una tarea para que GEE la realice. Para ello hay que exportar las anomalías de cada año en formato `geotiff`. La función `ee_imagecollection_to_local()` facilita la exportación de todas las capas de una *ImageCollection*. En cambio, la función `ee_image_to_drive()` exporta datos individuales de una única imagen a Google Drive. El argumento `scale` indica con qué resolución se exporta. Aunque la resolución de los datos originales es de 27 km, se fija una resolución de 2 km, lo que implica una interpolación en la exportación por razones de estética en la visualización. 


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

En resumen, este ejemplo ha mostrado una pequeña parte de la capacidad del geoprocesamiento en manejar grandes volúmenes de datos sin que implique su descarga ni el cálculo localmente. Pero también es posible manejar datos vectoriales u otros datos de alta resolución. Incluso se puede llegar a crear apliaciones basadas en GEE. 

::: {.infobox_resume data-latex=""}
### Resumen {-}

+ El aumento del volumen de datos que ha tenido lugar en los últimos años ha llevado a la necesidad de utilizar servicios de geoprocesamiento en nube que ayuden a llevar a cabo su análisis online, sin necesidad de descargar los datos ni de preocuparse por el rendimiento computacional. 

+ Uno de estos servicios es Google Earth Engine, donde se combina un catálogo de imágenes satelitales y conjuntos de datos geoespaciales multidimensionales de alta resolución con capacidades de análisis a escala planetaria. 

+ En este ejemplo que se utiliza en el capítulo, se muestra cómo procesar la temperatura oceánica superficial para calcular las anomalías de temperatura de la cuenca mediterránea en la estación estival, desde 1982 a 2022. 
:::
