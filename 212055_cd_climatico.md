
# Una nota sobre el cambio climático {#cambioclimatico}


*Dominic Royé*

Fundación de la Investigación del Clima

\index{cambio climático}

## Introducción

La temperatura media global en la superficie terrestre ha aumentado en 1,1 ºC desde la era preindustrial (1880-1900). A pesar de parecer un leve incremento en la temperatura, implica un aumento significativo en el calor acumulado del sistema Tierra. Cuando se combina el aumento de la temperatura en la superficie terrestre y en la superficie oceánica, la tasa de incremento promedio es de 0,08 ºC por década desde 1880. Sin embargo, la tasa promedio de aumento desde 1981 ha sido más del doble: 0,18 ºC por año. Los \index{océanos} océanos se caracterizan por una menor tasa de calentamiento debido a su capacidad calorífica. No obstante, son los océanos los que absorben la mayoría del calor adicional del planeta debido al cambio climático.^[https://www.ncei.noaa.gov/news/global-climate-202112.]

Entre todas las regiones, la \index{region@región!mediterránea} región mediterránea se está calentando un 20 % más rápido que el promedio mundial. Este lugar representa actualmente el punto crítico más importante del cambio climático, donde se percibe un significativo aumento de las vulnerabilidades. La temperatura de las aguas superficiales en el Mediterráneo ha estado subiendo 0,34 ºC cada década desde principios de los 80 [@cramer2020climate].

En este caso práctico con datos sobre el cambio climático se tratan las anomalías de la temperatura superficial del mar Mediterráneo en los meses estivales desde 1982 a 2022.[^Note-cambioclimatico-1] Igual que en el Cap. \@ref(geoproces) se utiliza el conjunto de datos "NOAA CDR OISST v02r01", una interpolación óptima de la temperatura diaria de la superficie del mar (OISST, por sus siglas en inglés) con una resolución de 1/4 grados (27 km). Los datos los proporciona la National Oceanic and Atmospheric Administration (NOAA), son campos completos de temperatura oceánica construidos mediante la combinación de observaciones ajustadas por sesgo de diferentes plataformas (satélites, barcos, boyas) en una cuadrícula global regular, con lagunas estimadas por interpolación. Los datos satelitales del radiómetro avanzado de muy alta resolución (AVHRR, por sus siglas en inglés) proporcionan la entrada principal que permite la alta cobertura espaciotemporal desde finales de 1981 hasta el presente^[https://developers.google.com/earth-engine/datasets/catalog/NOAA_CDR_OISST_V2_1] [@reynolds2008]. 


[^Note-cambioclimatico-1]: Se entiende por anomalía la diferencia entre el promedio de la temperatura estival un determinado año y el promedio estival en el período 1982-2010 (período de referencia para las anomalías). Véase Cap. \@ref(geoproces).

## Consideraciones iniciales

La \index{información!espaciotemporal} información espaciotemporal es clave en muchas disciplinas, especialmente en la climatología o la meteorología, y ello hace necesario disponer de un formato que permita una estructura multidimensional. Además, es importante que ese formato tenga un alto grado de compatibilidad de intercambio y pueda almacenar un elevado número de datos. Estas características llevaron al desarrollo del estándar abierto netCDF (*Network Common Data Form*). El formato netCDF es un estándar abierto de intercambio de datos científicos multidimensionales que se utiliza con datos de observaciones o modelos, principalmente en disciplinas como la climatología, la meteorología y la oceanografía. Se trata de un formato espaciotemporal con una cuadrícula regular o irregular. La \index{estructura!multidimensional} estructura multidimensional en forma de matriz (*array*) permite usar no solo datos espaciotemporales, sino también multidimensionales. Los datos multidimensionales en formato *geotiff* son menos comunes, pero también se pueden llegar a usar. Además, es posible crear objetos multidimensionales importando múltiples archivos *raster*. 

## Paquetes

El manejo de datos en formato netCDF o múltiples archivos *raster* es posible a través de varios paquetes de forma directa o indirecta. Destaca el paquete `ncdf4`, específicamente diseñado para esto, del que hacen uso también otros paquetes de forma oculta. El manejo con `ncdf4` es algo complejo, particularmente por la necesidad de gestionar la memoria RAM cuando se trata con grandes conjuntos de datos o también por la forma de manejar la clase *array*. Otro paquete muy potente es `terra`, clave en el trabajo con datos *raster* y que permite usar sus funciones también para el manejo del formato netCDF.   


```r
# paquetes
library("tidyverse")
library("sf")
library("terra")
library("lubridate")
library("fs")
library("patchwork")
library("giscoR")
library("scales")
library("rmapshaper")
library("RColorBrewer")
library("CDR")
```

## Visualización de mapas de facetas

\index{visualización} \index{facetas} \index{paneles} 

Una forma muy efectiva para mostrar cambios espaciotemporales son los mapas de facetas o paneles, en los que se representa, en una rejilla para cada año, la distribución espacial de las \index{anomalías} anomalías observadas, lo que permite una comparación temporal sencilla. 

### Importación de datos

Se importa el polígono del \index{mar Mediterráneo} mar Mediterráneo para limitar los datos al área de interés.  


```r
data("med_limit") 
```

A continuación, se importan todos los años empleando la función `dir_ls()` del paquete `fs` y la función `rast()` de `terra`. La primera función crea un vector de todos los archivos ubicados en la carpeta "data". Finalmente se renombran todas las capas con los correspondientes años. Es importante que se garantice el correcto orden de los archivos. Siempre que se haya realizado el \index{geoprocesamiento} geoprocesamiento en nube de las anomalías (Cap. \@ref(geoproces)) se puede usar la alternativa: `anom <- dir_ls("data-cc", regexp = "tif") |> rast()`). 


```r
# importar
anom <- dir_ls(system.file("external/data-cc/", package = "CDR"), regexp = "tif") |> 
  rast()

# renombrar las capas
names(anom) <- 1982:2022 
```

### Preparación de los datos

Después de importar el polígono del mar Mediterráneo, es necesario recortar y enmascarar el área de interés. Para ello se usa primero la función `crop()` con los límites del mar Mediterráneo y se pasa el resultado a la función `mask()`. El paquete `terra` necesita los datos vectoriales en su propia clase `SpatVector`; por eso se pasa con la función `vect()`, que lo convierte de la clase *sf* a *SpatVector*. Finalmente, se reproyectan los *rasters* a *ETRS89-extended / LAEA Europe* con el código EPSG:3035. 


```r
anom <- crop(anom, med_limit) |> mask(vect(med_limit))
anom <- project(anom, "EPSG:3035")
```

En la Fig. \@ref(fig:raster-crop-mask-plot) se puede ver el resultado de los primeros años. 


```r
plot(anom)
```

<div class="figure" style="text-align: center">
<img src="img/anom-med.PNG" alt="Distribución espacial de las anomalías en la  temperatura superficial del mar Mediterráneo: 1982-1997." width="100%" />
<p class="caption">(\#fig:raster-crop-mask-plot)Distribución espacial de las anomalías en la  temperatura superficial del mar Mediterráneo: 1982-1997.</p>
</div>


Un *raster* consiste en latitud, longitud y uno o múltiples valores, también llamados capas (véase Cap. \@ref(datos-espaciales)). Para poder visualizarlo en `ggplot2`, es necesario convertirlo en un `data.frame`. En este caso, se obtienen 41 columnas para las anomalías, además de las primeras dos columnas, que corresponden a la longitud y a la latitud. 

No obstante, es necesario realizar cambios en la distribución de las variables. Ahora se tiene la misma variable, la anomalía, distribuida en muchas columnas, pero la estructura adecuada debe ser un conjunto total de dos columnas: una con las anomalías y otra con los años. Para conseguirlo se hace uso de `pivot_longer()`, indicando el total de columnas que deben ser fusionadas y los nombres de las dos columnas resultantes.


```r
df <- as.data.frame(anom, xy = TRUE)
df <- pivot_longer(df, 3:length(df), 
                   names_to = "yr",
                   values_to = "anom")
```

Se añaden los años de la década de los 80 que faltan (1980, 1981) y se limitan las anomalías a un rango entre $-2$ ºC y $+2$ ºC. 


```r
df <- bind_rows(df, filter(df, yr == "1982") |> 
                    mutate(yr = "1981", anom = NA),
                   filter(df, yr == "1982") |> 
                    mutate(yr = "1980", anom = NA)
                ) |>
      mutate(anom2 = case_when(anom > 2 ~ 2,
                               anom < -2 ~ -2,
                               TRUE ~ anom))
```

Previo a la construcción del gráfico, se estima, para cada año, la media de las anomalías en toda la cuenca mediterránea. Estos datos se añaden como texto a cada mapa. Con el objetivo de obtener las coordenadas de la posición en la proyección EPSG:3035, se fija un punto vectorial que se reproyecta. 


```r
# media global
med_anom <- global(anom, fun = "mean", na.rm = TRUE)
med_anom <- rownames_to_column(med_anom, "yr")

# posición 
pos_global <- st_point(c(34.24, 41.5)) |> 
                 st_sfc(crs = 4326) |> 
                   st_transform(3035) |> 
                     st_coordinates()
```

### Construcción del gráfico de facetas

En el primer paso se definen los estilos partiendo de `theme_void()`, configurando los títulos, la leyenda y el color de fondo en `theme()`. 


```r
theme_SST_facet <- function(base_family = "Bahnschrift",
                            base_size = 11, 
                            base_line_size = base_size/22, 
                            base_rect_size = base_size/22) {
  
theme_void(base_family = base_family, base_size = base_size,
           base_line_size = base_line_size, base_rect_size = base_rect_size) +
  theme(strip.text = element_text(colour = "white", 
                                  face = "bold",
                                  size = 12, 
                                  margin = margin(b = 15)),
        legend.text = element_text(colour = "white"),
        legend.position = "top",
        legend.justification = .48,
        plot.margin = margin(20, 20, 20, 20),
        plot.title = element_text(colour = "white", size = 30, hjust = .5),
        plot.subtitle = element_text(colour = "white", size = 15, hjust = .5,
                                     margin = margin(t = 5, b = 5)),
        plot.caption = element_text(colour = "white", size = 10, hjust = 0),
        plot.background = element_rect(fill = "grey10", colour = NA),
        panel.spacing = unit(2, "lines"),
        panel.background = element_rect(fill = "grey10", colour = NA))
}
```

Para representar datos *raster* en forma de `xyz` se utiliza `geom_tile()` o `geom_raster()` en `ggplot2`. La última geometría requiere una rejilla regular. Para este primer ensayo se filtra solo el año 2003 y, además, se añade, con `geom_sf()`, el límite del mar Mediterráneo. La función `coord_sf()` permite fijar una proyección para objetos `sf` y, por último, se cambia el estilo definido anteriormente.  


```r
filter(df, yr == "2003") |> 
 ggplot() +
  geom_tile(aes(x, y, fill = anom2)) +
  geom_sf(data = med_limit, 
          fill = NA, colour = "white", size = .1) +
   coord_sf(crs = 3035) +
    theme_SST_facet()
```

Siguiendo el ejemplo, se modifica la gama de colores con `scale_fill_gradientn()`, en la que se pasa la paleta de colores, los valores mínimo y máximo tras el re-escalamiento (en este caso, $-2$ y $+2$), se reajustan los valores a una escala divergente  y se definen las etiquetas y sus posiciones. Dentro de la función `guides()`, se cambia el ancho y la altura de la barra colores empleando la función `guide_colorbar()`. 

Las geometrías `geom_point()` y `geom_text()` añaden la información de la anomalía global. La posición se pasa de forma directa en `aes()`; además, se definen el color y el tamaño de texto. El objetivo es situar el texto a la derecha del punto. Por esa razón, es necesario un ajuste en longitud indicando un valor correspondiente en el argumento `nudge_x` en la unidad del sistema de coordenadas (SC). Recuérdese que el SC está en metros.

La función `number()` del paquete `scales` facilita formatear las cifras con un decimal y los símbolos negativo y positivo.   


```r
# gama de colores
rdbu_pal <- rev(brewer.pal(11, "RdBu"))

# mapa 2003
filter(df, yr == "2003") |> 
ggplot() +
  geom_tile(aes(x, y, fill = anom2)) +
   geom_sf(data = med_limit, 
          fill = NA,
          colour = "white",
          size = .1) +
  geom_point(data = filter(med_anom, yr == "2003"),
             aes(x = pos_global[1,1], y = pos_global[1,2], fill = mean),
             size = 3.5, shape = 21, colour = "white") +
  geom_text(data = filter(med_anom, yr == "2003"),
            aes(x = pos_global[1,1], y = pos_global[1,2], 
                label = number(mean, .1, style_positive = "plus")),
            size = 3.5, nudge_x = 700000, colour = "white") +
  scale_fill_gradientn(colours = rdbu_pal, na.value = NA,
                       values = rescale(c(-2, 0, 2)),
                       limits = c(-2, 2),
                       breaks = c(-2, -1.5, -1, -0.5, 0, 
                                  .5, 1, 1.5, 2),
                       labels = c("< -2.0", "-1.5", "-1.0", "-0.5", "0.0", 
                                  "0.5", "1.0", "1.5", "> 2.0")) +
  guides(fill = guide_colorbar(barwidth = 20, 
                               barheight = .5)) +
  coord_sf(crs = 3035) +
  theme_SST_facet()
```

En la base de datos se han añadido dos años con valores perdidos (1980 y 1981) con el objetivo de que en cada fila del mapa de facetas (paneles o pequeños gráficos) haya 10 facetas (una para cada año de la década correspondiente, aunque las facetas de 1980 y 1981 aparezcan vacías). Para que no aparezcan las facetas de 1980 y 1981, es decir, para que los límites de la cuenca mediterránea no aparezcan en las facetas  de los años 1980/81, se debe repetir la geometría para todos los años (excepto para 1980 y 1981, lógicamente).  


```r
med <- slice(med_limit, rep(1, 41)) |> 
             select(geometry) |> 
             mutate(yr = as.character(1982:2022))
```

A continuación, se construye todo el gráfico con todas las facetas de mapas. Lo único nuevo es la función `facet_wrap()`, en la que se indica la variable por la que se crean las facetas. A diferencia de `facet_grid()`, esta variable permite fijar el número de filas y/o columnas. Además, se pasa una función menor[^Note-cambioclimatico-5] en la función `labeller()` en el mismo argumento. Esta función permite modificar las etiquetas de las facetas (aquí únicamente el texto de los años 1980/81).  

[^Note-cambioclimatico-5]: Una función incluida en otra función.


```r
# paso 1
g <- ggplot(df) +
      geom_tile(aes(x, y, fill = anom2)) +
      geom_sf(data = med, fill = NA, colour = "white", size = .1) +
      geom_point(data = med_anom,
             aes(x = pos_global[1,1], y = pos_global[1,2], fill = mean),
             size = 3.5, shape = 21, colour = "white") +
      geom_text(data = med_anom,
                aes(x = pos_global[1,1], y = pos_global[1,2], 
                    label = number(mean, .1, style_positive = "plus")),
                size = 3.5, nudge_x = 700000, colour = "white") +
 scale_fill_gradientn(colours = rdbu_pal,
                       na.value = NA, values = rescale(c(-2, 0, 2)),
                       limits = c(-2, 2),
                       breaks = c(-2, -1.5, -1, -0.5, 0, 
                                  .5, 1, 1.5, 2),
                       labels = c("< -2.0", "-1.5", "-1.0", "-0.5", "0.0", 
                                  "0.5", "1.0", "1.5", "> 2.0")) +
  guides(fill = guide_colorbar(barwidth = 20, 
                               barheight = .5)) +
  facet_wrap(yr ~ ., 
             ncol = 10,
             labeller = labeller(yr = function(lab){ 
               ifelse(lab %in% c("1980", "1981"), "", lab)})) 
```

Finalmente, se combina el objeto con definiciones finales, como los títulos, el sistema de coordenadas y el estilo. Es importante indicar `clip = off`, dado que en caso contrario se cortan visualmente los valores de las anomalías globales al encontrarse fuera de los límites de cada mapa. 


```r
# paso 2
g <- g + 
  labs(title = "ANOMALÍA ESTIVAL DE LA TEMPERATURA DE SUPERFICIE DEL\nMar Mediterráneo", 
       subtitle = "período de referencia 1982-2010.", fill = "") +
  coord_sf(crs = 3035, clip = "off") +
   theme_SST_facet()
```

*A priori*, no sería necesaria una ampliación del resultado. No obstante, en ocasiones se requiere un mapa de orientación.

### Mapa de orientación

Los límites administrativos se obtienen (seleccionan) con el paquete `giscoR`. También se limita la extensión a aproximadamente la de la cuenca mediterránea. La función `ms_innerlines()` del paquete `rmapshaper` facilita la obtención de los límites compartidos o interiores de los países seleccionados. Los nombres de los países, en forma de código ISO-3, se incluyen con ayuda de `geom_sf_text()`. 


```r
# límites de países
countries_med <- gisco_get_countries() |> 
                    filter(ISO3_CODE %in% c("ESP", "MAR", "FRA", "ITA",
                                            "GRC", "TUR", "DZA", "TUN",
                                            "LBY", "EGY", "ALB")) |> 
                     st_crop(xmin = -6, xmax = 36, ymin = 28, ymax = 45)

# límites inernos
innerlimit <- ms_innerlines(countries_med)

# mapa 
insetp <- ggplot() +
            geom_sf(data = med, size = .4, colour = NA, fill = "grey90") +
            geom_sf(data = innerlimit, size = .2, colour = "white") +
            geom_sf_text(data = countries_med, 
                          aes(label = ISO3_CODE), 
                         size = 2, colour = "white", fontface = "bold", nudge_y = .1) +
      coord_sf(crs = 3035, expand = FALSE) +
      theme_void() +
      theme(plot.background = element_blank(),
            panel.background = element_blank()) 
```

### Exportación del mapa final

El mapa de orientación se inserta en el gráfico, como elemento adicional, en la esquina superior derecha. El paquete `patchwork` ayuda a crear composiciones de distinos gráficos. La función empleada, `inset_element()`, indica la posición relativa en *xmin*, *ymin*, *xmax* e *ymax*. Es importante recordar que cualquier modificación del tamaño de impresión (veáse *height* y *width* en  `ggsave()`) puede llevar a ajustes en la posición. El argumento `align_to = "full"` permite posicionar sobre todo el lienzo. 


```r
# paso 3
p_final <- g + inset_element(insetp, 0, .75, .25, .95, align_to = "full")

ggsave("sst_anom_med2.png", 
       p_final, 
       bg = "grey10",
       height = 10, 
       width = 20, 
       unit = "in",
       type = "cairo-png",
       dpi = 400)
```

El resultado final, un gráfico de paneles (o facetas), puede verse en la Fig. \@ref(fig:sst-anom-med2). 
Los mapas muestran claramente el efecto del calentamiento global, siendo los años 2003 y 2022 los que muestran una mayor anomalía positiva. Destaca el hecho de que desde 1997 en todos los años se ha superado la media anual del período de referencia (1982-2010). 


<div class="figure" style="text-align: center">
<img src="img/sst_anom_med2.png" alt="Distribución espacial de las anomalías estivales de la temperatura de superficie del mar Mediterráneo." width="100%" />
<p class="caption">(\#fig:sst-anom-med2)Distribución espacial de las anomalías estivales de la temperatura de superficie del mar Mediterráneo.</p>
</div>

## Conclusión

Una forma muy efectiva para mostrar cambios espaciotemporales son los mapas de pequeños múltiples, donde se representan los datos para cada unidad temporal, lo que permite una comparación sencilla. En este caso práctico con datos sobre el cambio climático se han tratado las anomalías de la temperatura superficial del mar Mediterráneo en los meses estivales desde 1982 a 2022. El gráfico final muestra claramente el efecto del calentamiento global, siendo los años 2003 y 2022 los de mayor anomalía positiva. Destaca el hecho de que no ha habido un año con temperaturas más bajas de lo normal desde el año 1997.
