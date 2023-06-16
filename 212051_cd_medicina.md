
# Análisis de datos en medicina {#cap-medicina}

*Alberto M. Borobia*$^{a}$ y *María Jiménez-González*$^{a}$

$^{a}$Hospital Universitario La Paz - IdiPAZ


## Justificación 

La aplicación de la estadística en la investigación clínica ha sido una de las herramientas clave en los últimos dos años. La pandemia mundial causada por la enfermedad por coronavirus (COVID‑19) es una enfermedad infecciosa provocada por el virus SARS-CoV-2. Drante el año 2020, más de 13 millones de casos diagnosticados en España arrojaban un diagnóstico claro: se necesita más investigación. 

El primer apartado de este capítulo señala la importancia de la identificación de los sesgos (en concreto, del sesgo de selección) que aparecen en los estudios de investigación no aleatorizados. Tras ello, se abordará un ejemplo práctico de una de las aplicaciones más significativas de la bioestadística: el análisis de supervivencia, con el que se resuelven preguntas tan importantes cómo: ¿qué factores de riesgos están asociados a la mortalidad provocada por coronavirus?.
\index{coronavirus}
\index{bioestadística}
\index{COVID‑19}
\index{virus SARS-CoV-2}


## Introducción al uso de datos en investigación clínica y ensayos clínicos

En este capítulo, y a modo ilustrativo del ámbito de la investigación clínica, se abordarán tres análisis a partir de los datos:

- Un análisis relativo a la eliminación de sesgos, o más concretamente, a la eliminación del sesgo de selección.
- Un análisis relativo a la estimación e interpretación de las curvas de supervivencia.
- Un análisis relativo a la estimación e interpretación de la Regresión de COX.




### ¿Qué es un ensayo clínico?
\index{ensayo clínico}
\index{estudio observacional}

En la investigación clínica existen dos tipos de estudios: *estudios observacionales* y *ensayos clínicos*.

Los ensayos clínicos aleatorios se definen como el diseño experimental óptimo para proporcionar evidencia, eficacia y seguridad de una intervención [@Lium3164]. Los tratamientos estudiados o investigados son asignados aleatoriamente en grupos que garantizan que las diferencias en los resultados después del tratamiento reflejen los efectos del mismo [@rosenbaum2005observational]. Cuando estas condiciones ideales no son posibles (falta de recursos, financiación, tiempo, etc), se definen como estudios observacionales.

Previo a la puesta en marcha de un ensayo clínico, es imprescindible la redacción de un **Protocolo** y un **Plan de Análisis Estadístico (PAE)**.

- El protocolo, elaborado por los investigadores del estudio, precisa y justifica los métodos y planes del proceso que se llevará a cabo en el ensayo clínico [@rivera2020guidelines].

- El PAE detalla las características principales del eventual análisis estadístico de los datos, que deben describirse en la sección estadística del protocolo [@lewis1999statistical].

Los documentos anteriormente mencionados, y el resto de directrices necesarias para un ensayo clínico, están regulados por la "Conferencia Internacional sobre armonización de requisitos técnicos para el registro de productos farmacéuticos para uso humano" (sus siglas ICH en inglés).


### Limitaciones de los estudios observacionales

En el apartado anterior, se puso de manifiesto la importancia de lso ensayos clínicos aleatorizados. Sin embargo, la posible falta de recursos, financiación, tiempo o materiales, dificultan la puesta en marcha y realización de los mismos.

En consecuencia, la puesta en práctica de la investigación puede no ser la ideal. Los estudios observacionales, sin embargo, son una herramienta elemental en circunstancias no tan óptimas, ya que permiten analizar e investigar (contra viento y marea).

La limitación principal de los estudios observacionales es ue introducen sesgos en el análisis. Los ensayos clínicos tienen como principal objetivo eliminar el **sesgo de selección**: cuando los sujetos no son asignados aleatoriamente, por ejemplo, los resultados diferentes pueden reflejar estas diferencias iniciales en lugar de los efectos de los tratamientos [@rosenbaum2005observational]. 

\index{sesgo de selección}
\index{índice de propensión}

### Índice de propensión (*propensity score*)

Una solución aconsejable y recomendable ante los sesgos "escondidos" en los estudios observacionales es la técnica *propensity score* o índice de propensión. Esta técnica de emparejamiento equilibra las covariables observadas sesgadas ajustando por su índice de propensión, eliminando presumiblemente el sesgo. Habitualmente, el índice de propensión se obtiene a partir de un modelo de regresión cuya variable dependiente corresponde a la intervención o el resultado principal (por ejemplo, la muerte) y las variables independientes o covariables corresponden a las variables que puedan tener un efecto confusor en la variable dependiente [molina2015indices].





### Ejemplo práctico en **R** de un estudio observacional

El *dataset* sintético `datos_observacional` reproduce los datos de un hipotético estudio observacional sobre una enfermedad **X**. El objetivo del estudio es estudiar los factores de riesgo asociados a la mortalidad causada por esa enfermedad.


```
#> # A tibble: 5 × 8
#>      ID fecha_hospitalizacion sexo    edad comorbilidades      fecha_alta exitus
#>   <dbl> <dttm>                <chr>  <dbl> <chr>               <chr>       <dbl>
#> 1     1 2015-04-17 00:00:00   Mujer     76 1 o más comorbilid… 17/04/2020      1
#> 2     2 2015-03-21 00:00:00   Mujer     64 1 o más comorbilid… 31/03/2020      0
#> 3     3 2015-04-09 00:00:00   Hombre    65 1 o más comorbilid… 16/04/2020      0
#> 4     4 2015-04-04 00:00:00   Hombre    77 1 o más comorbilid… 13/04/2020      0
#> 5     5 2015-03-24 00:00:00   Mujer     66 1 o más comorbilid… 27/03/2020      0
#> # ℹ 1 more variable: fecha_exitus <dttm>
```


En la literatura, se ha evidenciado que las mujeres de mayor edad y con una o más comorbilidades tienen más riesgo de fallecer (exitus) por la enfermedad **XX**. En investigación, la estructura de
los resultados en un paper o en un informe estadístico, independientemente de la revista o PAE,
comienza en el mismo punto: una tabla resumen de las características basales de la población
objeto de estudio El paquete `tableone` (sencillo juego de palabras) integra funciones específicas para la creación de dichas tablas. La función principal de este paquete es `CreateTableOne()`.


```r
library("tableone")
my_vars <- c("sexo", "edad", "comorbilidades")
nonnormal <- c("edad")
factor_vars <- c("sexo", "comorbilidades")

# crea la tabla
tab1 <- CreateTableOne(
  vars = my_vars, factorVars = factor_vars,
  strata = "exitus", data = datos_observacional
)

# imprime la tabla
tab1 <- print(tab1,
  showAllLevels = TRUE, formatOptions = list(big.mark = ","),
  exact = "stage", nonnormal = nonnormal
)
```

Para presentar la tabla de resultados formateada basta con usar la función `kable()`:

```r
knitr::kable(tab1,
  caption = "Características basales de la población",
  col.names = c("level", "Vivo", "Exitus", "p-valor", "test")
)
```

<table>
<caption>(\#tab:tabla1)Características basales de la población</caption>
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:left;"> level </th>
   <th style="text-align:left;"> Vivo </th>
   <th style="text-align:left;"> Exitus </th>
   <th style="text-align:left;"> p-valor </th>
   <th style="text-align:left;"> test </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> n </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> 79 </td>
   <td style="text-align:left;"> 21 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sexo (%) </td>
   <td style="text-align:left;"> Hombre </td>
   <td style="text-align:left;"> 28 (35.4) </td>
   <td style="text-align:left;"> 2 ( 9.5) </td>
   <td style="text-align:left;"> 0.042 </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Mujer </td>
   <td style="text-align:left;"> 51 (64.6) </td>
   <td style="text-align:left;"> 19 (90.5) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> edad (median [IQR]) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> 64.00 [53.00, 73.00] </td>
   <td style="text-align:left;"> 82.00 [72.00, 85.00] </td>
   <td style="text-align:left;"> &lt;0.001 </td>
   <td style="text-align:left;"> nonnorm </td>
  </tr>
  <tr>
   <td style="text-align:left;"> comorbilidades (%) </td>
   <td style="text-align:left;"> 1 o más comorbilidades </td>
   <td style="text-align:left;"> 43 (54.4) </td>
   <td style="text-align:left;"> 18 (85.7) </td>
   <td style="text-align:left;"> 0.018 </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> No </td>
   <td style="text-align:left;"> 36 (45.6) </td>
   <td style="text-align:left;"> 3 (14.3) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
</tbody>
</table>




En la Tabla \@ref(tab:tabla1) y acorde a la bibliografía existente, se confirma el sesgo de selección a través del desequilibrio de la variable principal (`exitus`) en las variables `sexo`, `edad` y `comorbilidades`, evidenciado a través de la significación de éstas. Un argumento que motiva la aplicación, en este caso, de la técnica *propensity score* se fundamenta en la viabilidad de, por ejemplo, un modelo multivariante (como puede ser un modelo de predicción). La recogida de datos de un estudio observacional, como el de este ejemplo, normalmente viene dada por la disponibilidad de la población: sujetos ingresados en el Hospital por la enfermedad (en nuestro caso, coronavirus). Por tanto, esta muestra seleccionada recogerá pacientes con pronóstico más grave que la población general (mujer de mayor edad con una o más comorbilidades). 


El paquete `MatchIt` integra las funciones principales para el ajuste de la técnica *propensity score*, concretamente la función `matchit()` integra la teoría de [@ho2007matching] para el emparejamiento óptimo de los grupos estudiados. Los argumentos más importantes de esta función son:
    - `formula`: modelo de regresión que estudia la relación entre la variable principal de estudio (exitus) con las variables sesgadas (sexo, edad y comorbilidades).
    - `method`: especifica el método de *matching*. 
    - `distance`: especifica el método para la estimación del índice de propensión.

La función `get_matches()` empareja, poteriormente,  las coincidencias que resultan del `MatchIt`.

<span style="background-color:rgba(255, 203, 230, 0.8)"> 
*Nota:* es imprescindible que los casos del *dataset* estén completos. 
</span>


```r
library("MatchIt")
match <- matchit(exitus ~ edad + as.factor(sexo) + as.factor(comorbilidades),
  method = "nearest", distance = "mahalanobis",
  data = datos_observacional
)
datos_observacional_match <- get_matches(match, datos_observacional)
```

Para comprobar que el sesgo evidenciado en estudios anteriores ha desaparecido, se reproduce la tabla anterior.


```r
tab1_corregida <- CreateTableOne(
  vars = my_vars, factorVars = factor_vars,
  strata = "exitus", data = datos_observacional_match
)

# se imprime en el objeto tab1_corregida
tab1_corregida <- print(tab1_corregida,
  showAllLevels = TRUE, formatOptions = list(big.mark = ","),
  exact = "stage", nonnormal = nonnormal
)
```

Se formatea la salida de la tabla:

```r
knitr::kable(tab1_corregida,
  caption = "Características basales de la población aplicando la técnica de propensity score",
  col.names = c("level", "Vivo", "Exitus", "p-valor", "test")
)
```

<table>
<caption>(\#tab:tab1-corregida)Características basales de la población aplicando la técnica de propensity score</caption>
 <thead>
  <tr>
   <th style="text-align:left;">   </th>
   <th style="text-align:left;"> level </th>
   <th style="text-align:left;"> Vivo </th>
   <th style="text-align:left;"> Exitus </th>
   <th style="text-align:left;"> p-valor </th>
   <th style="text-align:left;"> test </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> n </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> 21 </td>
   <td style="text-align:left;"> 21 </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> sexo (%) </td>
   <td style="text-align:left;"> Hombre </td>
   <td style="text-align:left;"> 2 ( 9.5) </td>
   <td style="text-align:left;"> 2 ( 9.5) </td>
   <td style="text-align:left;"> 1.000 </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> Mujer </td>
   <td style="text-align:left;"> 19 (90.5) </td>
   <td style="text-align:left;"> 19 (90.5) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;"> edad (median [IQR]) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> 72.00 [69.00, 84.00] </td>
   <td style="text-align:left;"> 82.00 [72.00, 85.00] </td>
   <td style="text-align:left;"> 0.182 </td>
   <td style="text-align:left;"> nonnorm </td>
  </tr>
  <tr>
   <td style="text-align:left;"> comorbilidades (%) </td>
   <td style="text-align:left;"> 1 o más comorbilidades </td>
   <td style="text-align:left;"> 18 (85.7) </td>
   <td style="text-align:left;"> 18 (85.7) </td>
   <td style="text-align:left;"> 1.000 </td>
   <td style="text-align:left;">  </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;"> No </td>
   <td style="text-align:left;"> 3 (14.3) </td>
   <td style="text-align:left;"> 3 (14.3) </td>
   <td style="text-align:left;">  </td>
   <td style="text-align:left;">  </td>
  </tr>
</tbody>
</table>







En la Tabla \@ref(tab:tab1_corregida) se observa que el sesgo de selección existente en la muestra se ha resuelto equilibrando las variables (aunque reduciendo la muestra). Tras este paso previo, podría realizarse un análisis estándar de esta muestra intentando aproximarse lo máximo posible a un estudio aleatorizado.



## Análisis de supervivencia

Durante la pandemia ocasionada por el SARS-CoV-2, la pregunta principal de los investigadores clínicos se centró en un mismo objetivo: factores de riesgo asociados a la mortalidad causada por COVID-19. El análisis de supervivencia ha permitido a los investigadores intentar explicar las causas más factibles que producen esa mayor probabilidad de fallecer. El análisis de supervivencia permite estudiar los factores de riesgo asociados a la mortalidad. La ventaja principal de este análisis frente a un análisis estándar (como puede ser una regresión logística) se centra en la integración en la variable respuesta del evento y del tiempo hasta el evento, que tiene como consecuencia la interpretación de "riesgo" y no de "probabilidad" en los resultados.
\index{riesgo}
\index{análisis!de supervivencia}


El *dataset* utilizado, `datos_supervivencia` está incluido en el paquete `CDR` y está formado por 301 pacientes, 101 diagnosticados con infección por SARS-CoV-2 y 100 exitus.


```r
head(datos_supervivencia, 5)
#> # A tibble: 5 × 7
#>      id EXITUS_TIME DIAG_COVID EXITUS N_COMORBIDITIES SEX     EDAD
#>   <dbl>       <dbl>      <dbl>  <dbl>           <dbl> <chr>  <dbl>
#> 1   262           0          1      1               5 Hombre    83
#> 2   236           1          1      1               5 Hombre    72
#> 3   170          11          0      0               2 Mujer     65
#> 4   204          11          1      1               4 Hombre    80
#> 5    46          14          1      1               5 Hombre    90
```

### Estimación y comparación de curvas de supervivencia

La **función (o curva) de supervivencia** estudia la probabilidad de que el paciente o sujeto, sobreviva a un tiempo **X**. El estimador más común utilizado para el ajuste de la función de supervivencia es el estimador no paramétrico **Kaplan-Meier** y su función escalonada. Una vez generadas estas curvas de supervivencia, existen diferentes métodos (paramétricos y no paramétricos) para su comparación. En este apartado, se utiliza la prueba de **Mantel-Cox** (o test **Log-Rank**) para el contraste de funciones.

Los paquetes `survival` y `survminer` integran las funciones principales de la técnica:

- La función `Surv()`, de la librería `survival`, crea un objeto de supervivencia formado por el evento (exitus) y el tiempo hasta la ocurrencia del evento.

- La función `survfit()`, de la librería `survival`,  estima la función de supervivencia mediante el método *Kaplan-Meier* del objeto `Surv` y los factores de riesgo asociados.

- La función `ggsurvplot()`, genera el gráfico de la gurva de supervivencia (basada en la librería `ggplot2`). El argumento principal de la función es la función de supervivencia estimada, `survfit()`. Los argumentos más importantes (y recomendables) a la hora de graficar la función de supervivencia son:
   
    - `pval`: muestra el p-valor correspondiente a la comparación a través del test *Log-Rank*.
   
    - `conf.int`: muestra los intervalos de confianza de la(s) curva(s) de supervivencia.
   
    - `risk.table`: añade el número de sujetos (absoluto o relativo) en riesgo en cada momento del periodo objeto de estudio.

Se cragan los paquetes

```r
library("survival")
library("survminer")
```

Se ajusta el modelo y, posteriormente, se representa:


```r
fit <- survfit(Surv(EXITUS_TIME, EXITUS) ~ DIAG_COVID,
  data = datos_supervivencia
)

ggsurvplot(fit,
  data = datos_supervivencia,
  pval = TRUE,
  conf.int = TRUE,
  ggtheme = theme_bw(),
  palette = c("#E7B800", "#2E9FDF"),
  xlab = "Tiempo (días)",
  ylab = "Probabilidad de supervivencia",
  legend.labs = c("Sano", "COVID"),
  # añade tabla de supervivencia
  risk.table = TRUE,
  tables.height = 0.2,
  tables.theme = theme_cleantable()
)
```

<img src="212051_cd_medicina_files/figure-html/survplot-1.png" width="60%" style="display: block; margin: auto;" />

En la Fig. \@ref(fig:survplot), dónde el eje X corresponde al tiempo en días y el eje Y a la probabilidad de supervivencia, se observa que la probabilidad de supervivencia de las personas expuestas a COVID es significativamente menor (p-valor < 0.001) a las personas sanas. La mediana de supervivencia (línea trazada desde el 0.5 del eje Y, correspondiente al 50% de la probabilidad de supervivencia) corresponde a los 120 días, es decir, el 50% de los sujetos diagnosticados por COVID y objeto de estudio sobrevivieron, al menos, 120 días.

Por tanto, se puede concluir que se ha encontrado evidencia sobre el aumento de mortalidad asociada a la enfermedad COVID-19.


## Regresión de COX

La regresión de Cox[^cox1] o modelo de riesgos proporcionales es una técnica utilizada para el estudio del efecto de covariables sobre el tiempo hasta la ocurrencia de un evento (exitus, recaída, progresión, etc). La regresión de Cox es realmente una Regresión en la que  la variable dependiente es siempre una función de riesgo o supervivencia (están íntimamente relacionadas) y los predictores son una función del tiempo y una función de las variables consideradas como explicativas. En general, se suele expresar así: 
$$h(t, x_1, x_2,..., x_ p)=h_0(t)+g(x_1,x_2,...,x_p),$$
y más concretamente, 
$$h(t, x_1, x_2,..., x_ p)=h_0(t)+e^g(x_1,x_2,...,x_p),$$ donde $g$, normalmente, indica una combinación lineal de las covariables o variables explicativas. Es, por tanto, una técnica semi-paramétrica.

[^cox1]: Es importante distinguir la regresión de Cox de la regresión logística (véase el Cap. \@ref(cap-glm)). La Regresión logística relaciona la variable dependiente dicotómica con un conjunto de variables independientes sin contemplar el tiempo o contemplándolo sólo de forma estática, viendo en un punto fijo del tiempo si el suceso estudiado ha acontecido o no, pero no teniendo en consideración en qué momento ha sucedido.  La Regresión de Cox proporciona un análisis más fino. No analiza, en un instante de tiempo dado, si un acontecimiento de interés ha sucedido o no, sino cuándo ha sucedido, si es que ha sucedido, y lo hace teniendo en cuenta el comportamiento de una o varias variables independientes. Es por ello que la regresión logística trabaja con la *odds ratio* y la regresión de Cox con la *hazard ratio*.

La función principal para el ajuste de un modelo de regresión de Cox es `coxph()`. Esta función, al igual que la función `survfit()`, está formada por un objeto `Surv` y las covariables del modelo.


```r
fit_cox <- coxph(Surv(EXITUS_TIME, EXITUS) ~ DIAG_COVID + EDAD + SEX + N_COMORBIDITIES,
                 data = datos_supervivencia
)
```

\index{regresión!de Cox}
\index{razón de riesgo}

El *output* principal de una regresión de Cox, 
$$h(t, x_1, x_2,..., x_ p),$$ son las **razones de riesgos** o ***hazard ratios (HR)**. Es decir, la relación entre las dos funciones de riesgo  en función de los cambios operados en las variables explicativas. En concreto, la exponencial del coeficiente estimado para la va 




El *output* principal de una regresión de Cox ,h(t, x_1, x_2,..., x_ p), son las razones de riesgos o hazard ratios (HR). Es decir, la relación entre las dos funciones de riesgo  en función de los cambios operados en las variables explicativas. En concreto, la exponencial del coeficiente estimado para la variable explicativa X_i indica el incremento en el riesgo de fallecer cuando la variable explicativa aumenta en una unidad y las demás permanecen constantes. Esta razón de riesgos oscila entre 0 a 	$\infty$	, siendo el intervalo [0,1] una relación de riesgo bajo y [1,	$\infty$	] una relación de riesgo alto. 

::: {.infobox data-latex=""}
**Nota** 

- Los HR localizados entre 1 y 2 se interpretan en porcentaje. Es decir, HR = 1.5 indica a un aumento del riesgo del 50%.

- Los HR localizados entre 2 e	$\infty$	se interpretan en "veces". Es decir, HR = 3 indica a un aumento del riesgo de 3 veces.

- Los HR localizados entre 0 y 1 se interpretan como una reducción del riesgo del $(1-HR)\times 100 \%$. Es decir, HR = 0.8 indica a una disminución del riesgo del 20%.
:::




```r
summary(fit_cox)
#> Call:
#> coxph(formula = Surv(EXITUS_TIME, EXITUS) ~ DIAG_COVID + EDAD + 
#>     SEX + N_COMORBIDITIES, data = datos_supervivencia)
#> 
#>   n= 271, number of events= 100 
#>    (30 observations deleted due to missingness)
#> 
#>                       coef  exp(coef)   se(coef)      z Pr(>|z|)    
#> DIAG_COVID       1.3023581  3.6779594  0.5184547  2.512   0.0120 *  
#> EDAD             0.0006006  1.0006008  0.0116113  0.052   0.9587    
#> SEXMujer        -1.1256901  0.3244285  0.2360183 -4.770 1.85e-06 ***
#> N_COMORBIDITIES  0.1643743  1.1786554  0.0774043  2.124   0.0337 *  
#> ---
#> Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
#> 
#>                 exp(coef) exp(-coef) lower .95 upper .95
#> DIAG_COVID         3.6780     0.2719    1.3314   10.1605
#> EDAD               1.0006     0.9994    0.9781    1.0236
#> SEXMujer           0.3244     3.0823    0.2043    0.5153
#> N_COMORBIDITIES    1.1787     0.8484    1.0127    1.3717
#> 
#> Concordance= 0.815  (se = 0.025 )
#> Likelihood ratio test= 130.1  on 4 df,   p=<2e-16
#> Wald test            = 117.9  on 4 df,   p=<2e-16
#> Score (logrank) test = 165.9  on 4 df,   p=<2e-16
```

De la compleja salida del modelo, deben resaltarse y explicarse los siguientes puntos:

- Apartado 1: Fórmula del modelo, tamaño muestral y número de eventos.
- Apartado 2: Tabla con los coeficientes del modelo y su p-valor.
- Apartado 3: Tabla con los Hazard Ratio (exponencial de los coeficientes de la tabla anterior) y sus intervalos de confianza (lower .95 y upper .95).
- Apartado 4: parámetros de bondad de ajuste del modelo.

Por tanto, de este modelo se pueden concluir las siguientes interpretaciones:

- Un paciente diagnosticado de COVID-19 tiene 3.6 veces más riesgo de fallecer que un paciente sano.

- Una mujer tiene un 67.6% menos riesgo de fallecer que un hombre.

- Por cada comorbilidad, el riesgo de fallecer aumenta un 17.9%.



##  Conclusión

Ha sido necesaria una pandemia mundial para que la sociedad empiece a dar visibilidad y reconocimiento no sólo a la bioestadística, sino a la investigación clínica y a la necesidad de gestionar el uso masivo de datos en salud. A pesar de los múltiples estudios y experiencias pasadas que llamaban a la prudencia y a la acción concreta si se daba una situación similar, el mundo ha sido incapaz de actuar convenientemente. Esto último se ve reflejado en el mínimo aumento de inversión, reconocimiento y notoriedad no sólo en investigación o desarrollo, sino en el apoyo a la ciencia.

Es, quizá, la paradoja más extraña pero que representa el dicho popular:
<center>
La sociedad no avanzará si la ciencia no lo hace primero.
</center>
















