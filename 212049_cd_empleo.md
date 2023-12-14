
# El impacto de las crisis financiera y de la COVID-19 en el paro de CLM {#paro-clm}

*Isidro Hidalgo Arellano*$^{a,b}$ y *Ángel Jiménez Rojas*$^{a}$  

$^{a}$Observatorio del Mercado de Trabajo de Castilla-La Mancha  
$^{b}$Universidad de Castilla-La Mancha

## Planteamiento

En los últimos 15 años el mundo ha sufrido dos grandes períodos de **crisis económica**: en **2008**, de tipo financiero, y en **2020**, a causa de la pandemia de **COVID-19**\index{COVID-19}. Una de las variables socioeconómicas que se ven más afectadas por este tipo de procesos es el paro registrado. El paro registrado se define como el conjunto de los demandantes inscritos en las oficinas de empleo, una vez excluidos los inscritos sin disponibilidad para trabajar y los demandantes no parados, tales como estudiantes, desempleados en formación, etc. [@TOHARIA].
**Castilla-La Mancha** (CLM), comunidad autónoma interior de España, no ha sido ajena a las crisis económicas mencionadas, por lo que en este caso de uso se analiza el impacto de las mismas en la estructura del **paro registrado**\index{paro!registrado} de la región. Para ello se utilizan las siguientes variables explicativas: **sexo**\index{sexo} y **edad**\index{edad} de la persona desempleada, **sector de actividad económica de procedencia**\index{sector} y **tiempo de búsqueda de empleo**\index{tiempo de búsqueda de empleo}. El conjunto de datos utilizado comprende la **media anual del paro registrado en la comunidad autónoma de Castilla-La Mancha** desagregado según estas variables, a lo largo de los años que van desde 2007 a 2022.  

Para el análisis se usan las librerías y objetos (paletas de colores para los gráficos) siguientes:

```r
library("CDR")
library("tidyverse")
library("ggpubr")
paleta_heatmaps <- c(rgb(.7,1,0,.5),  rgb(.13,.22,.58,1))
paleta_lineas <- c("blue4", "orange","darkgreen")
```
Para cargar el conjunto de datos, `parados_clm`, incluido en el paquete `CDR`, y mostrar la estructura de la `tibble`\index{tibble@\textit{tibble}} se usa:

```r
data("parados_clm")
parados_clm
# A tibble: 92,215 × 8
# anyo   sexo    edad   sector  t_bus_e    tramo_edad  t_bus_e_agr  parados
# <ord>  <fct>   <dbl>  <fct>   <ord>      <ord>       <ord>       <dbl>
# 2007   hombre  16     agricu  t<=7 días  <30 años    t<=6 meses   0.66666667
# 2018   mujer   36     sinact  t<=7 días  30-44 años  t<=6 meses   1.66666667
# 2012   mujer   30     agricu  t<=7 días  30-44 años  t<=6 meses   5.33333333
# 2022   mujer   49     constr  t<=7 días  >44 años    t<=6 meses   0.75000000
# 2007   mujer   54     indust  t<=7 días  >44 años    t<=6 meses   1.50000000
# … with 92,210 more rows
```

## Evolución del paro medio anual en Castilla-La Mancha

Para ver el paro medio anual en función del tiempo, se construye un gráfico de evolución. Para ello se representa el paro medio por año, marcando los años que suponen un máximo o mínimo en la serie:

```r
resumen <- parados_clm |>
     group_by(anyo) |>
     summarise(parados = sum(parados)) |>
     mutate(anyo = as.numeric(as.character(anyo)))
anyos <- c(2007, 2013, 2019, 2020, 2022)
paro_anyos <- resumen |>
     filter(anyo %in% anyos) |>
     select(parados) |>
     mutate(parados = round(parados, 0))
puntos <- data.frame(anyos, paro_anyos)

graf <- ggplot(resumen, aes(anyo, parados)) +
     geom_line(linewidth = 2, col = paleta_lineas[1], alpha = 0.5) +
     xlab("")+ ylab("número de parados") +
     geom_point(puntos, mapping = aes(x = anyos, y = parados,
          shape = "circle filled", size = 1, fill = paleta_lineas[1],
          alpha = 0.5)) +
     theme(legend.position = "none",
          axis.title = element_text(face="bold", size = 10),
          axis.text = element_text(face="bold", size = 10),
          strip.text = element_text(size = 9, face = "bold")) +
     scale_y_continuous(labels = function(x) format(x, big.mark = ".",
          scientific = FALSE))
graf
```
<div class="figure" style="text-align: center">
<img src="img/empleo-total.jpeg" alt="Evolución del paro medio anual en CLM." width="50%" />
<p class="caption">(\#fig:empleo-total)Evolución del paro medio anual en CLM.</p>
</div>


En la Fig. \@ref(fig:empleo-total) se puede observar el devastador impacto de la crisis de 2008 en la economía castellano-manchega (el paro registrado fue más del triple), mucho mayor que el de la  **COVID-19** que, además, como se  verá posteriormente, se localizó básicamente en el sector servicios. Sin embargo, a partir de 2013 el paro registrado inició una tendencia a la baja muy pronunciada que aún hoy continúa, después de haber repuntado ligeramente por la pandemia. En consonancia con los anteriores comentarios, en lo que sigue se toman como puntos de referencia los años previos a las crisis financiera y de la COVID-19 (2007 y 2019, respectivamente) y el último año, 2022. 


## Evolución del paro medio anual en función de la edad y el sexo
Para ver cómo ha cambiado la estructura del paro registrado en función de la `edad` y el `sexo` de los parados se pueden utilizar diferentes gráficos. En este análisis, se usan mapas de calor y gráficos de distribución de densidad. Para hacer un mapa de calor\index{mapa!de calor} que permita comparar dos variables simultáneamente, se construye la función `heatmap_anyos()` con el código siguiente:


```r
heatmap_anyos <- function(var1, var2, inicio = 2007, intermedio = 2019,
                          fin = 2022){
     tabla <- select(parados_clm, anyo, var1, var2, parados) |>
          filter(anyo %in% c(inicio, intermedio, fin))
     names(tabla) <- c("anyo", "var1", "var2", "parados")
     tabla <- tabla |>
          group_by(anyo, var1, var2) |>
          summarise(parados = sum(parados))
     graf <- ggplot(tabla, aes(x = var1, y= var2, fill = parados)) +
          geom_raster() +
          scale_fill_gradientn(colours = paleta_heatmaps) +
          facet_wrap(~ anyo) + 
          labs(x = "", y = "") +
          theme(axis.text = element_text(size = 10, face = "bold"),
                axis.title = element_text(size = 10, face = "bold"),
                strip.text = element_text(size = 10, face = "bold"))
     return(graf)}
```
Si se lanza la función `heatmap_anyos()` para las variables `edad` y `sexo`, tomando como años comparativos 2007, 2019 y 2022, se obtiene la Fig. \@ref(fig:empleo-heatmap-edad-sexo).

```r
heatmap_anyos("sexo", "edad")
```
<div class="figure" style="text-align: center">
<img src="img/empleo-heatmap-edad-sexo.jpeg" alt="Paro medio anual según edad y sexo en 2007, 2019 y 2022." width="60%" />
<p class="caption">(\#fig:empleo-heatmap-edad-sexo)Paro medio anual según edad y sexo en 2007, 2019 y 2022.</p>
</div>
En la Fig. \@ref(fig:empleo-heatmap-edad-sexo) se puede apreciar que en los dos procesos críticos se ha producido un **desplazamiento del paro hacia los intervalos de mayor edad**, siendo este cambio más pronunciado en las **mujeres**.  
El mapa de calor es muy útil para una primera impresión de estos cambios, pero si se desea observar detalladamente cómo ha cambiado la distribución\index{distribución} del paro según el `sexo` y la `edad`, es mejor programar la función `densidad_compara()`, que proporciona un mayor nivel de detalle: produce un cuadro de gráficos que permite comparar la distribución del paro según la edad para cada estrato de la variable elegida (en este caso sexo) para tres años diferentes (2007, 2019 y 2022 por defecto). Los parámetros `alpha` y `size` permiten ajustar tamaño y opacidad de las líneas, respectivamente, mejorando la apariencia general del gráfico.

```r
densidad_compara <- function(variab, inicio = 2007, medio = 2019,
                             fin = 2022){
     tabla <- select(parados_clm, anyo, variab, edad, parados) |>
          filter(anyo %in% c(inicio, medio, fin))
     names(tabla) <- c("anyo", "variable", "edad", "parados")
     tabla <- tabla |>
          group_by(anyo, edad, variable) |> 
          summarise(parados = sum(parados))# |>
     
     graf <- ggplot(tabla, aes(x = edad, y = parados, color = anyo,
                    fill = anyo)) + geom_line(alpha=0.6, size = 1) +
          facet_wrap(~ variable, ncol = dim(table(tabla$variable))[1]) +
          ylab("número de parados") + labs(color="año") +
          scale_color_manual(values = paleta_lineas) +
          scale_y_continuous(labels = function(x) format(x,
               big.mark = ".",
               scientific = FALSE)) +
          theme(strip.text = element_text(size = 10, face = "bold"),
                axis.title = element_text(size = 10, face = "bold"),
                axis.text = element_text(size = 10, face = "bold"))
     return(graf)}
```
Ejecutando la función `densidad_compara()` para la variable `sexo` se obtiene la Fig. \@ref(fig:empleo-densidad-sexo):

```r
densidad_compara("sexo")
```
<div class="figure" style="text-align: center">
<img src="img/empleo-densidad-sexo.jpeg" alt="Distribución del paro medio anual por edad y sexo (2007, 2019 y 2022)." width="60%" />
<p class="caption">(\#fig:empleo-densidad-sexo)Distribución del paro medio anual por edad y sexo (2007, 2019 y 2022).</p>
</div>
En la Fig. \@ref(fig:empleo-densidad-sexo) se observa que en 2007, antes de ambas crisis, la evolución del paro registrado en los varones tiene **dos máximos**, en torno a 25 y 60 años, mientras que el número de mujeres desempleadas tiene una distribución bastante centrada entre 30 y 40 años. En cambio, en 2022 se aprecia el desplazamiento de la distribución de los parados de ambos sexos hacia los estratos de edad **mayores de 50 años**. Este desplazamiento es algo más intenso en las mujeres. Se observa igualmente que, comparando las distribuciones de las mujeres en 2019 y 2022, la crisis de la COVID-19 incrementó entre 5 y 10 años la distribución de la edad de las mujeres paradas. Este desplazamiento es inferior en los hombres, donde supone menos de 5 años.


## Evolución del paro medio anual según el tiempo de búsqueda de empleo

Se define el **tiempo de búsqueda de empleo** como el tiempo transcurrido ininterrumpidamente desde la última inscripción de la persona en el paro registrado [@JIPINFANTE]. Si, para simplificar, se agregan los doce intervalos que considera la estadística de paro registrado para el tiempo de búsqueda de empleo en tan solo cuatro, ejecutando la función `densidad_compara()` para la variable `t_bus_e_agr` se obtiene la Fig. \@ref(fig:empleo-densidad-bus-agr).

```r
densidad_compara("t_bus_e_agr")
```
<div class="figure" style="text-align: center">
<img src="img/empleo-densidad-bus-agr.jpeg" alt="Distribución del paro medio anual por edad y tiempo de búsqueda de empleo." width="60%" />
<p class="caption">(\#fig:empleo-densidad-bus-agr)Distribución del paro medio anual por edad y tiempo de búsqueda de empleo.</p>
</div>
En la Fig. \@ref(fig:empleo-densidad-bus-agr) se pone de manifiesto que el tramo con mayor incremento de número de parados es el correspondiente a más de 24 meses de búsqueda de empleo (**paro de muy larga duración**\index{paro!de muy larga duración}), ya que la crisis financiera de 2008 les redujo su probabilidad de encontrar empleo. Se puede afirmar también que los dos períodos de crisis han provocado la creación de un **paro estructural de larga duración**.

Se deja al lector ejecutar la función `heatmap_anyos()` para las variables `sexo` y `t_bus_e`, tomando como años comparativos 2007, 2019 y 2022. Observará en el gráfico resultante que el incremento en el paro de muy larga duración es más intenso en el colectivo de las mujeres. El código a utilizar es:

```r
heatmap_anyos("sexo", "t_bus_e")
```
## Evolución del paro medio anual según sexo, edad y sector de procedencia
La variable **sector de procedencia** es un tanto particular, ya que, cuando un parado lleva mucho tiempo buscando empleo ininterrumpidamente, "pierde" el sector de procedencia y se clasifica automáticamente en la rúbrica "sin actividad". A la hora de analizar esta variable, por tanto, es importante tener en cuenta que una parte de los parados ubicados en la rúbrica "sin actividad" realmente tuvo un trabajo hace mucho tiempo.  
La visualización de los cambios producidos en estas variables con un mapa de calor se puede llevar a cabo ejecutando de nuevo la función `heatmap_anyos()`, obteniendo la Fig. \@ref(fig:empleo-heatmap-sexo-sector):

```r
heatmap_anyos("sexo", "sector")
```
<div class="figure" style="text-align: center">
<img src="img/empleo-heatmap-sexo-sector.jpeg" alt="Paro medio anual según sexo y sector de procedencia." width="60%" />
<p class="caption">(\#fig:empleo-heatmap-sexo-sector)Paro medio anual según sexo y sector de procedencia.</p>
</div>
En la Fig. \@ref(fig:empleo-heatmap-sexo-sector) se aprecia el incremento del paro registrado en el sector **servicios**, especialmente en el colectivo femenino.  
Ejecutando la función `densidad_compara()` para la variable `sector` se obtiene la Fig. \@ref(fig:empleo-densidad-sector).

```r
densidad_compara("sector")
```
<div class="figure" style="text-align: center">
<img src="img/empleo-densidad-sector.jpeg" alt="Distribución del paro medio anual por edad y tiempo de búsqueda de empleo." width="60%" />
<p class="caption">(\#fig:empleo-densidad-sector)Distribución del paro medio anual por edad y tiempo de búsqueda de empleo.</p>
</div>
Como se observa en la Fig. \@ref(fig:empleo-densidad-sector), las diferencias a lo largo del tiempo del número de parados por sector de actividad económica revelan algunas particularidades interesantes. **Industria**\index{industria} y **construcción**\index{construcción} se comportan de modo similar: hay un fuerte desplazamiento en edad desde 2007, pero se mantiene el volumen de paro en ambos sectores a lo largo de los 15 años de estudio. El paro en el sector **agropecuario** y en el sector **servicios** también presenta desplazamiento en edad, pero además se ha incrementado notablemente en estos 15 años; ambos efectos son mucho más evidentes en el sector **servicios**. Finalmente, en el colectivo **sin actividad** se aprecian dos características: en primer lugar, los parados menores de 30 años suponen el mayor volumen en este colectivo, como era de esperar, ya que la población joven que accede al mercado laboral por primera vez no cuenta con experiencia previa; en segundo lugar, desde 2007 a 2019, y algo menos desde 2019 a 2022, hay un incremento de volumen de paro en los **mayores de 45 años** que, con toda probabilidad, corresponde a los parados de larga duración de mayor edad. En todos los sectores se aprecia el descenso del volumen total de paro registrado desde 2019 a 2022, a pesar de la crisis sanitaria de la COVID-19.  

## Conclusiones

La crisis de 2008 tuvo un gran impacto en el paro registrado de Castilla-La Mancha, multiplicándolo por un factor mayor que 3 desde 2007. Sin embargo, a partir del año 2013 el paro registrado inicia una tendencia a la baja muy pronunciada que aún hoy continúa, después de haber sufrido un rebote debido a la crisis de la COVID-19.  

La estructura interna de la población parada en la región ha cambiado. En efecto, la población mayor de 45 años, las mujeres, los parados de larga duración y el sector servicios son los grandes perjudicados por ambos procesos de crisis.


<!-- ```{r img-paq-cdr, echo=FALSE, out.width='15%',} -->
<!-- knitr::include_graphics("img/LogoCDR_transparente.png") -->
<!-- ``` -->
