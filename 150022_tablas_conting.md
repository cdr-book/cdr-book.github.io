---
editor_options: 
  markdown: 
    wrap: 72
---

# Análisis de tablas de contingencia {#tablas-contingencia}

*José-María Montero*

Universidad de Castilla-La Mancha

## Introducción {#motiv}

\index{tabla!de contingencia}

Las **tablas de contingencia** analizan la relación existente entre
variables categóricas, o susceptibles de categorizar, con un número de
categorías finito. Dada su naturaleza, no permiten el uso de las
tradicionales operaciones aritméticas, con lo cual, en el ámbito de la
Estadística Descriptiva su análisis suele basarse en diagramas de barras
y porcentajes (véase Cap. \@ref(cap-120006-aed)), y en la esfera de la
Inferencia Estadística (Cap. \@ref(Fundainfer)) se centra en los
contrastes de hipótesis no paramétricos y, básicamente, en el contraste
de independencia entre dos o más de estas variables. Una pregunta que
suele hacerse toda aquella persona que se acerca por primera vez al
análisis de tablas de contingencia es el significado del término
"contingencia". Pues bien, este término fue acuñado por Pearson
[@Pearson1904] al apuntar: "Este resultado nos permite partir de la
teoría matemática de la independencia probabilística, tal como se
desarrolla en los libros de texto elementales, y construir a partir de
ella una teoría generalizada de la **asociación** o, como yo la llamo,
**contingencia**." \index{contingencia}

El análisis de tablas de contingencia (o de asociación) permite dar
respuesta, entre otras, a preguntas como: los factores involucrados en
una tabla de contingencia, ¿son independientes o están asociados? Si
están asociados, ¿qué niveles de dichos factores son los que están
asociados?, ¿cuál es la intensidad de dicha asociación?

### Notación {#notac}

\index{atributo} \index{factor} \index{nivel} \index{categoría}
\index{modalidad}

Sea una población (o una muestra) de *N* elementos sobre la que se
pretende analizar, simultáneamente, dos (por simplicidad) **atributos**
o **factores** (*A* y *B*) con *R* y *C* **niveles**, **modalidades** o
**categorías**, respectivamente. Sean {*A*~1~, *A*~2~, ..., *A~R~*} y
{*B*~1~, *B*~2~, ..., *B~C~*} los niveles anteriormente aludidos. Sea
*n~ij~* el número de elementos que presentan a la vez las modalidades
*i* y *j* de los factores *A* y *B*, respectivamente. La tabla
estadística que describe, conjuntamente, estos *N* elementos (en otros
términos, que muestra las frecuencias conjuntas de los niveles de ambos
factores) se denomina **tabla de contingencia** (Tabla
\@ref(tab:tabla-contingencia)).\
\index{tabla!de contingencia}

|            |              |              |   Factor B   |       |              |           |
|-----------|-----------|:---------:|:---------:|-----------|:---------:|:---------:|
|            |              | Nivel *B*~1~ | Nivel *B*~2~ | . . . | Nivel *B*~C~ | **Total** |
|            | Nivel *A*~1~ |   *n*~11~    |   *n*~12~    | . . . |  *n*~1*C*~   |  *n*~1.~  |
|            | Nivel *A*~2~ |   *n*~21~    |   *n*~22~    | . . . |  *n*~2*C*~   |  *n*~2.~  |
|            | .            |      .       |      .       | .     |      .       |     .     |
| Factor *A* | .            |      .       |      .       | .     |      .       |     .     |
|            | .            |      .       |      .       | .     |      .       |     .     |
|            | Nivel *A~R~* |   *n~R~*~1~  |  *n~R~*~2~   | . . . |   *n~RC~*    | *n~R~*~.~ |
|            | **Total**    |   *n.*~1~    |   *n*~.2~    | . . . |  *n*~.*C*~   |    *n*    |

: (#tab:tabla-contingencia) Tabla de contingencia de dos factores

A modo de ejemplo, considérese una muestra de 80 ayuntamientos de una
C.C. A.A., anotándose en la base `ayuntam`, incluida en el paquete `CRD`
del libro, el signo político del equipo gubernamental (`signo_gob`) y si
prestan o no públicamente el servicio *X* (`serv`). Los resultados
obtenidos fueron los siguientes:


```r
library("CDR")
data("ayuntam")

summarytools::ctable(ayuntam$signo_gob, ayuntam$serv, headings = TRUE) |>
  print()
#> Cross-Tabulation, Row Proportions  
#> signo_gob * serv  
#> Data Frame: ayuntam  
#> 
#> ------------ ------ ------------ ------------ -------------
#>                serv           No           Sí         Total
#>    signo_gob                                               
#>    Avanzados          14 (33.3%)   28 (66.7%)   42 (100.0%)
#>   Ilustrados           6 (15.8%)   32 (84.2%)   38 (100.0%)
#>        Total          20 (25.0%)   60 (75.0%)   80 (100.0%)
#> ------------ ------ ------------ ------------ -------------
```

Del estudio de las distribuciones marginales de ambos factores se deduce
que el 52,5% de los ayuntamientos de la CC.AA. están regidos por los
Avanzados y el 47,5% por los Ilustrados. Y, más interesante, que en el
75% de los ayuntamientos prestan el servicio *X*.

El análisis de la tabla de contingencia daría respuesta a las siguientes
preguntas: ¿La prestación pública del servicio *X* es independiente del
signo político del ayuntamiento o depende de dicho signo? En este último
caso: ¿Qué signo político está asociado con la prestación pública y cuál
no?, ¿la asociación entre los factores "Signo político del equipo
gubernamental" y "Prestación pública del servicio *X*" es muy intensa?
Pero dicho análisis se abordará posteriormente.

<!-- |          |            |            |         |           | -->

<!-- |----------|------------|:----------:|:-------:|:---------:| -->

<!-- |          |            | Prestación | pública |           | -->

<!-- |          |            |     NO     |   SÍ    | **Total** | -->

<!-- | Signo    | Avanzados  |     14     |   28    |  **42**   | -->

<!-- | político | Ilustrados |     6      |   32    |  **38**   | -->

<!-- |          | **Total**  |   **20**   | **60**  |  **80**   | -->

<!-- Del estudio de las distribuciones marginales de ambos factores se deduce que el 52,5% de los ayuntamientos de la CC.AA. están regidos por los Avanzados y el 47,5% por los Ilustrados. Y, más interesante, que en el 75% de los ayuntamientos prestan el servicio *X*. -->

<!-- El análisis de la tabla de contingencia daría respuesta a las siguientes preguntas: ¿La prestación pública del servicio *X* es independiente del signo político del ayuntamiento o depende de dicho signo? En este último caso: ¿Qué signo político está asociado con la prestación pública y cuál no?, ¿la asociación entre los factores "Signo político del equipo gubernamental" y "Prestación pública del servicio *X*" es muy intensa? Dicho análisis se abordará posteriormente. Por el momento, la atención se centra en su construcción a partir de la base de datos de este ejemplo y en el cálculo de porcentajes (en este caso, por fila). Se procede como sigue: -->

En función del número de factores involucrados en la tabla y del número
de niveles de cada uno de ellos se tiene la siguiente tipología de
tablas de contingencia:

-   Tablas $R\times C$: 2 factores, el primero con *R* niveles y el
    segundo con *C* niveles.

-   Tablas $R\times C \times M$: 3 factores, con *R*, *C* y *M* niveles,
    respectivamente.

-   Y así sucesivamente.

\index{tabla!de contingencia!$2\times2$} 
\index{tabla!de contingencia!R $\times$ C}
\index{tabla!de contingencia!multidimensional}

Dentro de las tablas $R\times C$ se distinguen las tablas $2\times 2$ de las demás, por su especial interés en la realidad y por criterios
pedagógicos, al ser las más sencillas.

### Diseños experimentales o procedimientos de muestreo que dan lugar a una tabla de contingencia {#prodecim}

\index{diseño experimental} \index{procedimiento de muestreo}

Una cuestión a la que no se le da la suficiente importancia es la forma
en la que se toma la información contenida en la tabla (el diseño del
experimento o procedimiento de muestreo). Dada una determinada tabla de
contingencia, esta puede haber sido obtenida mediante uno u otro diseño
de experimento o procedimiento de muestreo, y esta circunstancia no es
baladí, puesto que condiciona su análisis, sobre todo cuando el tamaño
muestral es pequeño.

Sin ánimo de exhaustividad, los diseños experimentales o procedimientos
de muestreo más habituales que dan lugar a una tabla de contingencia son
los siguientes:[^150022_tablas_conting-1]

[^150022_tablas_conting-1]: Otros procedimientos de muestreo o diseños
    experimentales no habituales pueden verse en @Ruiz_Maya_et_al1995.

-   **Tipo 1: se fijan los totales marginales de ambos factores**

\index{diseño experimental!totales marginales de ambos factores fijos}

*Ejemplo*:

Se desea investigar si la preferencia de la larva de gorgojo por el tipo
de judía es independiente de la cubierta de la semilla o depende de
esta. Para ello se toman 22 judías de tipo *A* y 18 de tipo *B*, que se
introducen en un recipiente con 33 larvas. Dadas las condiciones de
densidad, no entrará más de una larva por judía. Pasado un tiempo
prudencial para que las larvas entren en las judías, se procede a contar
las que han sido atacadas de cada tipo y las que
no.[^150022_tablas_conting-2]

[^150022_tablas_conting-2]: Como en el resto del manual, las variables
    aleatorias se escriben en mayúsculas y los valores que toman, en
    minúscula. En este caso, los totales marginales han sido fijados y
    son valores, pero no ocurre lo mismo con las frecuencias absolutas
    en las cuatro celdas de las tablas, que son variables aleatorias
    (Tabla \@ref(tab:tabla-contin-totmarg)).

|         |           | Presencia de | larva atacante |           |
|---------|-----------|:------------:|:--------------:|:---------:|
|         |           |      NO      |       SÍ       | **Total** |
| Tipo de | A         |   *N*~11~    |    *N*~12~     |  **22**   |
| judía   | B         |   *N*~21~    |    *N*~22~     |  **18**   |
|         | **Total** |    **7**     |     **33**     |  **40**   |

: (#tab:tabla-contin-totmarg) Tabla de contingencia con totales
marginales en dos factores


Como puede apreciarse en la Tabla \@ref(tab:tabla-contin-totmarg), los
totales marginales de ambos factores han sido fijados en el diseño del
experimento.

-   **Tipo 2: solo se fijan los totales marginales de uno de los
    factores**

\index{diseño experimental!totales marginales de uno de los factores fijos}

*Ejemplo*:

En un municipio se desea investigar si el desempleo es o no
independiente del sexo del desempleado. Se seleccionan aleatoriamente
100 varones y 100 mujeres y se les pregunta por su situación laboral,
trabajando o en paro (Tabla
\@ref(tab:tabla-contin-totmarg-fijo1)).[^150022_tablas_conting-3]

[^150022_tablas_conting-3]: En este caso los totales marginales por
    columnas son variables.

|      |           | Situación  | laboral |           |
|------|-----------|:----------:|:-------:|:---------:|
|      |           | Trabajando | En paro | **Total** |
| Sexo | Varón     |  *N*~11~   | *N*~12~ |  **100**  |
|      | Mujer     |  *N*~21~   | *N*~22~ |  **100**  |
|      | **Total** |  *N*~.1~   | *N*~.2~ |  **200**  |

: (#tab:tabla-contin-totmarg-fijo1) Tabla de contingencia con totales
marginales fijos en un factor

-   **Tipo 3: únicamente se fija el tamaño muestral**

\index{diseño experimental!total muestral fijo}

*Ejemplo*:

Un estudio transversal sobre la prevalencia de osteoporosis y su
relación con dietas pobres en calcio incluyó a 400 mujeres entre 50 y 54
años. Cada una de ellas realizó una densiometría de columna y rellenó un
cuestionario sobre sus antecedentes dietéticos para determinar si su
dieta era o no pobre en calcio (Tabla
\@ref(tab:tabla-contin-totmarg-filas)).[^150022_tablas_conting-4]

[^150022_tablas_conting-4]: Ahora tanto los totales marginales por
    columnas como por filas son variables.

|              |           | Dieta pobre | en calcio |           |
|--------------|-----------|:-----------:|:---------:|:---------:|
|              |           |     NO      |    SÍ     | **Total** |
| Osteoporosis | SÍ        |   *N*~11~   |  *N*~12~  |  *N*~1.~  |
|              | NO        |   *N*~21~   |  *N*~22~  |  *N*~2.~  |
|              | **Total** |   *N*~.1~   |  *N*~.2~  |  **400**  |

: (#tab:tabla-contin-totmarg-filas) Tabla de contingencia con totales
marginales variables

## Contraste de independencia en tablas $2 \times 2$

\index{contraste de independencia}
\index{contraste de independencia!en tablas $2\times 2$}

Como se avanzó en la Sec. \@ref(motiv), la primera pregunta a la que
debe dar respuesta el análisis de tablas de contingencia es si los
factores involucrados en la tabla son independientes o, por el
contrario, están asociados. La respuesta a esta pregunta exige llevar a
cabo un contraste de independencia y, para ilustrarlo, se aborda
inicialmente el caso de las tablas $2\times 2$. Dicho contraste se lleva
a cabo de tres formas: ($i$) exacta, ($ii$) aproximada, y ($iii$)
aproximada con corrección de continuidad.

\index{contraste de independencia!en tablas $2\times 2$!contraste exacto}
\index{contraste de independencia!en tablas $2\times 2$!contraste aproximado}
\index{contraste de independencia!en tablas $2\times 2$!contraste aproximado con corrección de continuidad}

### Planteamiento general del contraste exacto de independencia {#plantgen}

-   Hipótesis:
    -   $H_0$: los factores son independientes.
    -   $H_1$: están asociados.[^150022_tablas_conting-5]
-   Filosofía del contraste: se trata de un contraste de significación.
    Por tanto, la tabla observada será "rara" (bajo $H_0$) si su
    probabilidad, más la probabilidad de obtener tablas más alejadas de
    H~0~ que ella, es inferior al nivel de significación, $\alpha$,
    prefijado para el contraste. En ese caso, se rechaza la hipótesis de
    independencia entre los factores involucrados en la tabla.

[^150022_tablas_conting-5]: También puede establecerse como hipótesis
    alternativa la asociación en un determinado sentido: "el nivel 1 del
    factor *A* está asociado con el 1 del *B* y el 2 del *A* con el 2
    del *B*" (asociación positiva); o "el nivel 1 del factor *A* está
    asociado con el 2 del *B* y el 2 del *A* con el 1 del *B*"
    (asociación negativa). En estos dos casos el contraste no sería
    bilateral sino unilateral.

\index{contraste de independencia!unilateral}
\index{contraste de independencia!bilateral}

### Algoritmo para la realización del contraste exacto de independencia {#algoritmo}
\index{asociación!positiva}
De acuerdo con la filosofía de los contrastes de significación (Sec.
\@ref(contrhip)), el algoritmo para la realización del contraste de
independencia en tablas de contingencia es como sigue:

1.  Selección de la tablas del espacio muestral que se alejen de la
    hipótesis de independencia, en la dirección marcada por la hipótesis
    alternativa, tanto o más que la tabla observada, incluida esta
    última.

2.  Cálculo, bajo la hipótesis de independencia, de la probabilidad de
    ocurrencia de cada una de las tablas seleccionadas en el punto 1.

3.  Suma de dichas probabilidades y comparación con el $\alpha$
    prefijado.

4.  Toma de la decisión relativa al rechazo o no de la hipótesis de
    independencia.

Nótese que $(i)$ los pasos 1 y 2 dependen del diseño del experimento o
procedimiento de muestreo llevado a cabo; $(ii)$ en ausencia del
software adecuado, la realización de un test exacto es un procedimiento
laborioso (a veces un reto), con lo cual, si ese fuera el caso, los test
aproximados de independencia son bienvenidos.

A continuación, se expone el contraste de independencia, en sus
versiones exacta, aproximada y aproximada con corrección de continuidad,
cuando el procedimiento de muestreo o diseño experimental es el de
tipo 1. En la Sec. \@ref(dise) se comentan algunas cuestiones de interés
cuando el diseño de muestreo es de tipo 2 o tipo 3.

### Contraste de independencia: diseño tipo 1

\index{contraste de independencia!en tablas $2\times 2$!totales marginales de ambos factores fijos}

#### Contraste exacto (test exacto de Fisher)

\index{contraste de independencia!en tablas $2\times 2$!contraste exacto}
\index{contraste de independencia!en tablas $2\times 2$!test exacto de Fisher}

Considérese el ejemplo del diseño tipo 1 expuesto en la Sec.
\@ref(prodecim).[^150022_tablas_conting-6] Supóngase que el resultado
obtenido fue el mostrado en la Tabla \@ref(tab:tabla-fisher1):

[^150022_tablas_conting-6]: Se trata de un ejemplo clásico de
    @Sokal2012.


```r
datos_jud <- c(1, 6, 21, 12)
tabla <- cbind(
  expand.grid(list(
    Tipo_de_judía = c("A", "B"),
    Presencia_larva = c("No", "Sí")
  )),
  count = datos_jud
)
tabla_jud <- ftable(xtabs(count ~ Tipo_de_judía + Presencia_larva, tabla))
```

|         |           |                    |          |           |
|---------|-----------|:------------------:|:--------:|:---------:|
|         |           | Presencia de larva | atacante |           |
|         |           |         NO         |    SÍ    | **Total** |
| Tipo de | A         |         1          |    21    |  **22**   |
| judía   | B         |         6          |    12    |  **18**   |
|         | **Total** |       **7**        |  **33**  |  **40**   |

: (#tab:tabla-fisher1) Ejemplo de tabla de contingencia con totales
marginales variables

Según el algoritmo expuesto en la Sec. \@ref(algoritmo), el contraste es
como sigue:

**1**. **Selección de las tablas que se alejan de** $H_0$ tanto o más
que la observada.[^150022_tablas_conting-7] Como se señalaba en
@Pearson1904, la teoría de la independencia probabilística indica que,
bajo la hipótesis de independencia, el porcentaje de judías de tipo *A*
y de tipo *B* no atacadas (o atacadas) por una larva de gorgojo tiene
que ser el mismo. En otros términos, bajo la hipótesis de independencia,
en cada una de las cuatro celdas se tiene que verificar que:
$N_{ij} = \frac{N_{i.} N_{.j}}{N}, \forall i,j$, donde
$\frac{N_{i.} N_{.j}}{N}= {E}_{ij}$ se denomina frecuencia esperada bajo
la hipótesis de independencia (en este caso, al estar los totales
marginales fijos, ${E}_{ij}=\frac{n_{i.} n_{.j}}{n}$). Denominando
${D}_{ij}=N_{ij}-{E}_{ij}, \hspace{0,2cm}\forall i,j, \hspace{0,2cm} i=1,2,\hspace{0,2cm} j=1,2$,
se puede que comprobar que en una tabla $2\times 2$,
${D}_{11}={D} _{22}= -{D}_{12}= - {D}_{21}$, con lo cual, tomando de
referencia, por ejemplo, la celda {1,1}, las tablas que se alejan tanto
o más que la observada de la hipótesis de independencia son aquellas que
verifican, en valor absoluto, que
${D}_{11}=N_{11}-{E}_{11}\geq n_{11}-\frac {n_{1.} n_{\cdot1}}{n}$.

[^150022_tablas_conting-7]: El contraste se ha llevado a cabo de forma
    bilateral. En caso de una alternativa unilateral (asociación
    positiva o en el sentido de la diagonal principal; o negativa, en el
    sentido de la diagonal no principal), el procedimiento sería el
    mismo y las tablas seleccionadas serían, en el primer caso, todas
    menos la $T_7$, y en el segundo, $T_0$ y $T_1$.

\index{frecuencia esperada}

En el ejemplo que se considera, las ${D}_{11}$ son las siguientes (en
negrita las de la tabla observada y aquellas otras que se alejan tanto o
más que ella de $H_0$):

***T***~0~: -3,85; ***T***~1~: -2,85; *T*~2~: -1,85; *T*~3~: -0,85;
*T*~4~: 0,15; *T*~5~: 1,15; *T*~6~: 2,15; ***T~7~***: 3,15,

donde el subíndice de *T* indica el valor de $N_{11}$ en dicha tabla.

\index{asociación!positiva} \index{asociación!negativa} Nótese que el
criterio anterior no es otro que el criterio general de seleccionar las
tablas en las que la diferencia de porcentajes, por ejemplo, por fila,
en valor absoluto, sea superior a la de la tabla observada, puesto que
$\left|\frac {N_{11}}{n_{1.}} -\frac {N_{21}}{n_{2.}} \right|=|{D}_{11} | \frac {n}{n_{1.} n_{2.} }$.

**2**. **Cálculo, bajo la hipótesis de independencia, de la probabilidad
de ocurrencia de cada una de las tablas seleccionadas en 1**. La
probabilidad de ocurrencia de una tabla de contingencia con los totales
marginales fijos se puede obtener como el cociente entre el número de
disposiciones de las frecuencias observadas favorables a dicha tabla y
el número de disposiciones posibles. El número de disposiciones
favorables coincide con el coeficiente multinomial (maneras de que de
$n$ frecuencias observadas, $n_{11}$ caigan en la celda {1,1}, $n_{12}$
lo hagan en la celda {1,2}, $n_{21}$ lo hagan en la celda {2,1} y
$n_{22}$ lo hagan en la celda
{2,2}:$\frac {n!}{n_{11}! n_{12}! n_{21}! n_{22}!}$).

El número de disposiciones posibles, supuesta $H_0$, es:
$\binom{n}{n_{1.}} \binom{n}{n_{\cdot1}} = \frac{n!}{(n_{1.}! n_{2.}!)} \frac {n!}{(n_{\cdot1} ! n_{\cdot2})}$.

Por tanto, el cociente entre ambas es:
$P=\frac {n_{1.} !n_{2.} !n_{\cdot1} !n_{\cdot2} !} {n!n_{11} !n_{12} !n_{21} !n_{22} !}$.

En consecuencia, las probabilidades de las tablas seleccionadas en el
punto 1 son: $T_0$: 0,0017; $T_1$: 0,0219; $T_7$: 0,0091.

**3**. **Suma de dichas probabilidades**: 0,0327.

**4**. **Comparación con** $\alpha$ y decisión sobre el rechazo o no de
la hipótesis de independencia: La decisión depende del valor de
$\alpha$. Si fuera, por ejemplo, 0,05, se rechazaría la independencia
entre el tipo de judía y si es o no atacada por la larva de gorgojo.

El código **R** necesario para tomar llevar a cabo el test exacto de
Fisher anterior es:
\index{contraste de independencia!en tablas $2\times 2$!test exacto de Fisher}


```r
# Ho: Los factores son independientes.
# H1: Los factores están asociados
fisher <- fisher.test(tabla_jud, alternative = "two.sided")
fisher$p.value
#> [1] 0.0327607
```


```r
# Ho: Los factores son independientes.
# H1: Existe asociación negativa.
fisher_less <- fisher.test(tabla_jud, alternative = "less")
fisher_less$p.value
#> [1] 0.02361309
```


```r
# Ho: Los factores son independientes.
# H1: Existe asociación positiva.
fisher_greater <- fisher.test(tabla_jud, alternative = "greater")
fisher_greater$p.value
#> [1] 0.998293
```

Como puede apreciarse, se rechaza la hipótesis de independencia frente a
la de asociación (test bilateral). Esto no significa que las larvas
ataquen a un tipo de judía y no al otro. Atacan a ambos tipos, ¡y
bastante! Este es el primer hecho que se constata. Sin embargo, atacan
más a las judías de tipo *A* (un 95% son atacadas) que a las de tipo B
(dos terceras partes son atacadas). Esa diferencia porcentual de judías
atacadas se considera significativa bajo el supuesto de independencia y,
en ese sentido, se dice que existe asociación entre el tipo de judía y
la presencia o no de larva atacante. La asociación sería *A*-SÍ y
*B*-NO. Sin embargo, ¡cuidado!, las larvas atacan siempre. La asociación
anterior debe entenderse como "el porcentaje de ataque es muy grande en
ambos casos, pero en *A* (mucho) más que en *B*. Este es el segundo
hecho importante que se constata: las larvas muestran una preferencia
significativa por las judías tipo *A*. Aunque ya se ha visto la
dirección de la asociación (en el sentido de la diagonal ascendente), en
la Sec. \@ref(medidas), dedicada a las medidas de asociación en tablas
$2 \times 2$, se cuantificará su intensidad.

#### Contraste aproximado

\index{contraste de independencia!en tablas $2\times 2$!contraste aproximado}

En este caso (tipo 1), bajo $H_0$: independencia, la frecuencia conjunta
de una celda, $N_{ij}$, cualquiera que sea, se distribuye según una ley
hipergeométrica con $E(N_{ij})=\frac{N_{ij}} {n_{i.}n_{\cdot j}}$ y
$V(N_{ij})=\frac{n_{i.}n_{\cdot j}(n-n_{i.}) (n-n_{\cdot j})}{n^2 (n-1)}$.
Por consiguiente,
$$P\left(\left( N_{11}-\frac{n_{1.} n_{\cdot1}} {n} \right) ^2 \geq \left(n_{11} - \frac{n_{1.} n_{\cdot1}}{n}\right)^2 \right)= P\left(\frac{(N_{11}-\frac{n_{1.} n_{\cdot1}}
{n})^2}{\frac{n_{1.}n_{\cdot1}n_{2.} n_{\cdot2}}{n^2 (n-1)}}
\geq \frac{(n_{11}-\frac{n_{1.} n_{\cdot1}}{n})^2}{\frac{n_{1.}n_{\cdot1}n_{2.} n_{\cdot2})}{n^2 (n-1)}}\right).$$

Y si ninguna $\hat{E}_{ij}=\frac{n_{ij}} {n_{i.}n_{\cdot j}}$ es
inferior a 5, la probabilidad anterior puede aproximarse (teorema
central del límite) por:

\index{estadístico!Chi-cuadrado}
\index{estadístico!Chi-cuadrado!ajustado}

$$P\left(\chi^2_1 \geq {\frac{\left( n_{11}-\frac{n_{1.} n_{\cdot1}}
{n}\right)^2} {\frac{n_{1.}n_{\cdot1}n_{2.} n_{\cdot2}}{n^2 (n-1)}}}\right)=P\left( \chi^2_1 \geq {\frac{ (n-1)(n_{11}n_{22}-n_{21}n_{12}
)^2} {n_{1.}n_{\cdot1}n_{2.} n_{\cdot2}}}\right),$$ donde el estadístico
$\frac{ (n-1)(n_{11}n_{22}-n_{21}n_{12} )^2} {n_{1.}n_{\cdot1}n_{2.} n_{.2}}$
se denomina Chi-cuadrado ajustado $(\chi^2_{ajd})$ y es tal que
$\chi^2_{ajd}= \frac {n-1} {n}\frac{ n (n_{11}n_{22}-n_{21}n_{12} )^2} {n_{1.}n_{\cdot1}n_{2.} n_{\cdot2}}$,
donde
$\frac{ n (n_{11}n_{22}-n_{21}n_{12} )^2} {n_{1.}n_{\cdot1}n_{2.} n_{\cdot2}}$
es el estadístico Chi-cuadrado ($\chi^2$) que proporcionan todos los
softwares de contraste de independencia en tablas de contingencia.

En el ejemplo propuesto:


```r
chisq.test(tabla_jud)$expected
#>      [,1]  [,2]
#> [1,] 3.85 18.15
#> [2,] 3.15 14.85
chisq.test(tabla_jud, correct = FALSE)
#> 
#> 	Pearson's Chi-squared test
#> 
#> data:  tabla_jud
#> X-squared = 5.6828, df = 1, p-value = 0.01713
```

Como $\chi^2= 5,6828$, entonces $\chi^2_{ajd}=5,54073$ y
$P\left(\chi^2_{ajd} \geq 5,54073\right)=0,0186$.[^150022_tablas_conting-8]

[^150022_tablas_conting-8]: Este "cálculo extra" es un pequeño peaje que
    hay que pagar en aras de la exactitud. Y es importante, porque
    pudiera hacer cambiar la decisión resultante del contraste.

Nótese que la probabilidad exacta de obtener una tabla tan alejada o más
de la hipótesis de independencia que la observada (incluida esta) es
0,0327, mientras que la probabilidad aproximada es 0,0186. La
aproximación no es muy buena, y ello se debe a la existencia de
frecuencias esperadas menores que 5.

#### Contraste aproximado con corrección de continuidad

\index{contraste de independencia!en tablas $2\times 2$!contraste aproximado con corrección de continuidad}

\index{corrección de continuidad!de Yates} \index{error!de continuidad}
\index{estadístico!Chi-cuadrado!corregido de continuidad de Yates}
\index{estadístico!Chi-cuadrado!ajustado!corregido de continuidad de Yates}

Como se vio en la subsección anterior, al aproximar la probabilidad de
obtención de tablas tanto o más alejadas de $H_0$ que la observada (que
se calcula con una distribución hipergeométrica, que es discreta)
mediante una distribución $\chi^2_1$ (que es continua), se comete un
"error de continuidad". Dicho error se intenta corregir incluyendo en el
contraste una **corrección de continuidad**. Hay varias correcciones que
han tenido cierto éxito en la literatura. La más popular es la
corrección de Yates, si bien solo se recomienda cuando las ${E}_{ij}$
sean múltiplos de 0,5.

En el contraste aproximado con corrección de Yates, se rechaza $H_0$ si:
$$P\left( \chi^2_1 \geq {\frac{ (n-1)(|n_{11}n_{22}-n_{21}n_{12}
|-0,5n)^2} {n_{1.}n_{\cdot1}n_{2.} n_{\cdot2}}}  \right)\leq \alpha ,$$
donde el estadístico
${\frac{ (n-1)(|n_{11}n_{22}-n_{21}n_{12}|-0,5n)^2} {n_{1.}n_{\cdot1}n_{2.} n_{\cdot2}}}$
se denomina estadístico Chi-cuadrado ajustado corregido de continuidad
de Yates ($\chi^2_{ajd,CCY}$).

En el ejemplo propuesto, el test Chi-cuadrado con corrección de
continuidad de Yates se obtiene directamente con la función
`chisq.test()` que, por defecto, incluye el argumento `correct = TRUE`.


```r
chisq.test(tabla_jud)
#> 
#> 	Pearson's Chi-squared test with Yates' continuity correction
#> 
#> data:  tabla_jud
#> X-squared = 3.8637, df = 1, p-value = 0.04934
```

Como el estadístico Chi-cuadrado corregido de continuidad $\chi^2_{CCY}$
vale 3,8337, entonces
$\chi^2_{ajd,CCY}=\frac{39}{40} \cdot 3,8337=3,7636$; y como
$P\left( \chi^2_1 \geq 3,7636\right)=0,0524$, $H_0$ se rechazaría cuando
$\alpha > 0,0524$. Nótese que si, por ejemplo, $\alpha = 0,05$, la
decisión sobre el rechazo o no de $H_0$ es distinta con $\chi^2_{CCY}$ y
$\chi^2_{ajd,CCY}$; de ahí la importancia de utilizar el estadístico
ajustado.

Por tanto, la corrección de Yates ha transformado la infraestimación de
la probabilidad exacta en una sobreestimación de más o menos el mismo
tamaño. Ello se debe a que en la tabla observada, hay frecuencias
esperadas (${E}_{ij}$) que distan mucho de ser múltiplos de 0,5. La
corrección de Yates es la que incluye la librería utilizada (`stats`).
Otras correcciones pueden verse en @Ruiz_Maya_et_al1995 y @Montero2002.

### Contraste de independencia: diseños tipo 2 y tipo 3 {#dise}

\index{contraste de independencia!en tablas $2\times 2$!totales marginales de un factor fijos}

\index{contraste de independencia!en tablas $2\times 2$!contraste exacto}
\index{contraste de independencia!en tablas $2\times 2$!contraste aproximado}
\index{contraste de independencia!en tablas $2\times 2$!contraste aproximado con corrección de continuidad}

En el caso tipo 2, para la realización del test exacto, las tablas que
se alejan tanto o más que la observada de la hipótesis de independencia
son las que verifican:
$$\left| \frac {N_{11}}{n_{1.}}-\frac{N_{21}}{n_{2.}}\right|
\geq \left|\frac{n_{11}}{n_{1.}}-\frac{n_{21}}{n_{2.}}\right|,$$ y la
probabilidad de ocurrencia de una tabla de contingencia viene dada por:

```{=tex}
\begin{equation}
P\left({N_{11}}={n_{11}} ;{N_{12}}={n_{12}};{N_{21}}={n_{21}};{N_{22}}={n_{22}} |N=n\right)
= \\
\binom{n_{1.}}{N_{11}} \binom {n_{2.}}{N_{21}}
 \left( \frac{N_{\cdot1}}{n} \right)^{N_{\cdot1}} \left( \frac {N_{\cdot2}}{n}\right)^{N_{\cdot2}}.
 \end{equation}
```
El estadístico de contraste en el test aproximado viene dado por
$\chi^2=\frac{ n (n_{11}n_{22}-n_{21}n_{12})^2} {n_{1.}n_{\cdot1}n_{2.} n_{\cdot2}}$,
y por
$\chi^2_{CC}=\frac { n \left( \left|n_{11}n_{22}-n_{21}n_{12}\right|-\frac {f} {2}\right)^{2}} {n_{1.}n_{\cdot1}n_{2.} n_{\cdot2}}$
en el caso de estar corregido de continuidad, siendo *f* el mayor factor
común de los tamaños muestrales fijados.

En el caso tipo 3, las tablas que se alejan tanto o más que la observada
de la hipótesis de independencia son las que verifican la condición
expuesta en el tipo 2 (y tipo 1), siendo su probabilidad de ocurrencia: \index{contraste de independencia!en tablas $2\times 2$!total muestral fijo}

```{=tex}
\begin{equation}
\begin{split}
 P\left({N_{11}}  ={n_{11}} ;{N_{12}}={n_{12}};{N_{21}}={n_{21}};{N_{22}}={n_{22}} |N=n \right) = \\
 \frac {n!}{{n_{1.}! n_{2.}! n_{\cdot 1}! n_{\cdot2}!}}
\left(\frac {n_{1.}}{n} \frac{n_{\cdot1}}{n} \right)^{n_{11}}
\left(\frac {n_{1.}}{n} \frac{n_{\cdot2}}{n} \right)^{n_{12}}
\left(\frac {n_{2.}}{n} \frac{n_{\cdot1}}{n} \right)^{n_{21}}
\left(\frac {n_{1.}}{n} \frac{n_{\cdot2}}{n} \right)^{n_{22}}.
\end{split}
\end{equation}
```
El test aproximado, en este caso, es un test razón de verosimilitudes
donde el estadístico de contraste,
$G=-2ln\frac {n_{1.}^{n_{1.}} n_{2.}^{n_{2.}} n_{\cdot1}^{n_{\cdot1}} n_{\cdot2}^{n_{\cdot2}}}{n_{11}^{n_{11}} n_{21}^{n_{21}} n_{12}^{n_{12}} n_{22}^{n_{22}} n^n}$,
también se distribuye como una $\chi^2_1$ en caso de independencia.
Apenas hay literatura sobre correcciones de continuidad en este modelo y
la poca que hay sugiere la aplicación de la corrección de Yates.

::: {.infobox data-latex=""}
**Nota**

En el diseño de muestreo tipo 3 se recomienda no usar ninguna corrección
de continuidad, salvo que el tamaño muestral sea muy pequeño y sea
imprescindible la realización del test.
:::

El código **R** para llevar a cabo estos dos contrastes aproximados
puede verse en la Sec. \@ref(contaprox).

\index{contraste de independencia!en tablas $2\times 2$!contraste de razón de verosimilitudes}

## Contraste de independencia en tablas $R \times C$ {#contrasteRC}

\index{contraste de independencia!en tablas R $ \times $ C}

El análisis de tablas $R\times C$ puede considerarse, en principio, una
generalización del caso de tablas $2\times 2$. Ahora bien, en el caso
$R\times C$, los test exactos, recomendados en el caso de que
$E_{ij}\leq5$ en más del 20% de las celdas[^150022_tablas_conting-9]
[@Reynolds1984], son un auténtico reto y aún no están disponibles en el
software convencional.

[^150022_tablas_conting-9]: La razón es que, sea cual sea el diseño de
    muestreo, si la probabilidad de que un elemento de la tabla caiga en
    una determinada celda {*i*, *j*} es muy pequeña (se suele utilizar
    el límite del 5%), entonces la distribución de probabilidad de
    $N_{ij}$ es muy asimétrica, y para que se simetrice lo suficiente y
    se pueda aproximar por una normal (que después, al cuadrado,
    participará en una Chi-cuadrado), el tamaño muestral, en el tipo 3,
    y los totales marginales fijados, en los tipos 1 y 2, tienen que ser
    grandes (se suele fijar el valor de 100), de ahí el límite de
    $E_{ij}\leq5$; por eso es recomendable que el tamaño muestral sea
    grande y los totales marginales no estén desequilibrados. En todo
    caso, lo ideal es que el requisito $E_{ij}\leq5$ se cumpla en todas
    las celdas de la tabla.

Si no se cumple el requisito anterior, una solución es agrupar
categorías, con sentido común y coherencia.[^150022_tablas_conting-10]
Si la agrupación de categorías no se pudiese hacer, por carecer de
sentido o cualquier otro motivo, lo más honesto sería no realizar el
contraste hasta disponer de una base de datos mejor.

[^150022_tablas_conting-10]: En tablas $2\times 2$ no se pueden agrupar
    filas, y la única solución son los tests exactos.

A la luz de lo anteriormente expuesto, en el caso de tablas $R\times C$
la atención se centra en los tests aproximados.

### Contrastes aproximados {#contaprox}

Cuando el procedimiento de muestreo o el diseño experimental es de tipo
1 o 2, el contraste aproximado de independencia se denomina contraste
Chi-cuadrado. La filosofía de dicho contraste es la siguiente: parece
lógico que el contraste se base en las diferencias (cuadráticas, para
que no se compensen las negativas con las positivas) entre las
frecuencias observadas y las esperadas bajo la hipótesis de
independencia. Si los factores son independientes, dichas diferencias
serán pequeñas y atribuibles a fluctuaciones aleatorias. Si están
asociados, serán grandes y atribuibles a la asociación existente entre
sus niveles. Pearson propuso el siguiente estadístico de contraste:
$$\chi^2 =\sum_{i=1}^{R} \sum_{j=1}^{C}\frac {\left(N_{ij}-{E}_{ij}\right)^2} {{E}_{ij}},$$

\index{estadístico!Chi-cuadrado}
\index{contraste de independencia!en tablas R $ \times $ C!contraste aproximado}
\index{contraste de independencia!contraste Chi-cuadrado}

que, si no se incumple el requisito expuesto en las primeras lineas de
la Sec. \@ref(contrasteRC), y bajo la hipótesis de independencia, se
distribuye como una $\chi_{(R-1)(C-1)}^2$. En caso de que
$P\left( \chi_{(R-1)(C-1)}^2 \geq \sum_{i=1}^{R} \sum_{j=1}^{C}\frac {\left(n_{ij}-\hat{E}_{ij}\right)^2} {\hat{E}_{ij}}\right )$
sea inferior al nivel de significación prefijado, se rechaza la
hipótesis de independencia.[^150022_tablas_conting-11] Téngase en cuenta
que en el caso $R\times C$ la hipótesis alternativa es "al menos un
nivel de un factor está asociado con un nivel del otro factor".

[^150022_tablas_conting-11]: $\hat{E}_{ij}=\frac{n_{i.} n_{.j}}{n}$ es
    el valor de ${E}_{ij}$ calculado con los datos de la tabla
    observada. Igualmente, $n_{ij}$ son las frecuencias de la tabla
    observada.

La razón de que las diferencias $\left(N_{ij}-\hat{E}_{ij}\right)^2$ se
dividan por $\hat{E}_{ij}$ en el estadístico Chi-cuadrado de Pearson es
la siguiente: la misma diferencia $N_{ij}-\hat{E}_{ij}$ puede significar
cosas bien diferentes. Una diferencia de 5 no es nada si $\hat{E}_{ij}=$
1.000; pero es muchísimo si $\hat{E}_{ij}=2$. Por eso la diferencia
(cuadrática) se pone en relación con la frecuencia esperada.

\index{frecuencia esperada}

```{=html}
<!-- -
¿¿LO METO AQUÍ O EN MEDIDAS DE ASOCIACIÓN??
El estadístico Chi-cuadrado tiene un límite superior fijo: $N(K-1)$, donde $N$ es el tamaño de la muestra y $K$ es el número más pequeño de filas o columnas. **Por eso no se puede utilizar como medida de asociación. ¿Un valor Chi-cuadrado de 35 es grande o pequeño, es decir, indica mucha o poca asociación? Pues depende; hay que compararlo con su valor máximo.**  -->
```
<!-- -   Si el tamaño muestral es grande y/o el número de niveles de los factores es elevado, el test Chi-cuadrado indicará siempre "rechazo de la hipótesis de independencia". En este caso el resultado no es muy interesante porque lo que el test podría estar detectando es un alejamiento insignificante de la hipótesis de independencia en al menos una celda y, dado que el test se alimenta con mucha información (tamaño muestral grande), lo considera significativo. Sin embargo, cuando valoremos el tamaño de la asociación detectada la medida de asociación nos dirá: los factores no son independientes, pero la intensidad de su asociación es despreciable. -->

<!-- -   Más sobre lo mismo: si en una Tabla RxC se multiplican por 10 las frecuencias observadas, las diferencias de porcentajes serán las mismas, pero el estadístico de contraste se multiplica por 10 y si con el primero no se rechazaba la hipótesis de independencia, con el segundo puede ocurrir que sí, sindo las proporciones las mismas. -->

En el caso de que el procedimiento de muestreo sea del tipo 3, aunque
puede aplicarse el contraste Chi-cuadrado, es recomendable proceder con
el contraste de independencia de razón de verosimilitudes, que compara
por cociente las frecuencias esperadas bajo la hipótesis de
independencia y las observadas. Se basa en la razón de la verosimilitud
de la hipótesis de independencia a la luz de la muestra obtenida y del
máximo de la función de verosimilitud, $\Lambda$. Bajo el supuesto de
independencia la razón será cercana a la unidad, atribuyéndose la
diferencia a fluctuaciones aleatorias; el logaritmo neperiano de dicha
razón (negativo) estará cercano a cero. En caso contrario, el cociente
de verosimilitudes (negativo) disminuye, tanto más cuanto más diferencia
hay entre la verosimilitud de la hipótesis de independencia y el máximo
de la función de verosimilitud. En @Wilks1935 se demostró que, cuando la
hipótesis de independencia es cierta, $G=-2ln \Lambda$, con
$\Lambda= \prod_{i=1}^R \prod_{j=1}^C \left (\frac {{E}_{ij}} {N_{ij}}\right)^{N_{ij}}$
se distribuye como una $\chi_{(R-1)(C-1)}^2$. Ambos estadísticos de
contraste, $\chi^2$ y $G$, son asintóticamente equivalentes.

\index{contraste de independencia!contraste razón de verosimilitudes}
\index{estadístico!G @\textit{G}}

A modo de ejemplo, se quiere contrastar si en la Comunidad de Madrid la
opinión sobre la presidente Dña. Isabel Díaz Ayuso depende de la zona
geográfica o si, por el contrario, es independiente de ella. Para ello
se encuestan, por algún procedimiento aleatorio, 2.795 personas con
derecho a voto en la comunidad y se eliminan las respuestas "NS/NC/me es
indiferente". Los resultados obtenidos fueron los siguientes (dataset
`ayuso` del paquete `CDR`):


```r
data("ayuso")
tabla_ayuso <- table(ayuso)
tabla_ayuso
#>                opinion
#> zona            n1_nefasta n2_mala n3_buena n4_excelente
#>   n1_mad_muni           25     500       50         1000
#>   n2_metropol           10     280       50          460
#>   n3_extraradio          5     130       25          260
chisq.test(tabla_ayuso)$expected
#>                opinion
#> zona            n1_nefasta  n2_mala n3_buena n4_excelente
#>   n1_mad_muni    22.540250 512.7907 70.43828     969.2308
#>   n2_metropol    11.449016 260.4651 35.77818     492.3077
#>   n3_extraradio   6.010733 136.7442 18.78354     258.4615
chisq.test(tabla_ayuso, correct = FALSE)
#> 
#> 	Pearson's Chi-squared test
#> 
#> data:  tabla_ayuso
#> X-squared = 19.486, df = 6, p-value = 0.003418
```


```r
library("DescTools")
GTest(tabla_ayuso, correct = "none")
#> 
#> 	Log likelihood ratio (G-test) test of independence without correction
#> 
#> data:  tabla_ayuso
#> G = 19.357, X-squared df = 6, p-value = 0.003602
```

Como puede verse, sea cual sea el estadístico de contraste, la hipótesis
de independencia se rechaza para cualquiera de los valores de $\alpha$
utilizados en la práctica (1%, 2,5%, 5%, 10%).

### Contraste aproximado con corrección de continuidad

\index{contraste de independencia!en tablas R $ \times $ C!contraste aproximado con corrección de continuidad}

Afortunadamente, en la mayoría de las ocasiones el tamaño muestral es
grande y los totales marginales no están muy desequilibrados, con lo que
los estadísticos Chi-cuadrado y Chi-cuadrado corregido de continuidad
son prácticamente iguales, sobre todo si el número de niveles de ambos
factores es elevado. En caso de utilizar una corrección de continuidad,
hay unanimidad en utilizar la de Yates, sea cual sea el procedimiento de
muestreo y el test (Chi-cuadrado o $G$), si bien dicha unanimidad tiene
mucho que ver con que es la única que está programada en el software
convencional sobre tablas de contingencia.

## Medidas de asociación en tablas $2 \times 2$ {#medidas}

\index{medidas de asociación!en tablas $2\times 2$}
\index{fuentes de asociación}

Si no se rechaza la hipótesis de independencia, el análisis de la tabla
se puede dar por finalizado. En caso contrario, el nuevo objetivo es
determinar la dirección de la asociación detectada (o las fuentes de
asociación en el caso $R\times C$) y su intensidad, y para ello se
utilizan las denominadas medidas de asociación. Igual que en el
contraste de independencia, se distinguirán los casos $2 \times 2$ y
$R\times C$, en esta ocasión no tanto por motivos pedagógicos sino
porque las situaciones son bien diferentes.

En el caso $2\times 2$, los tipos de asociación en función de su
dirección (positiva y negativa) ya se definieron en la Sec.
\@ref(plantgen). Por lo que se refiere a los límites de su intensidad,
se dice que la asociación es perfecta cuando al menos uno de los niveles
de uno de los factores queda determinado por un nivel de ese otro
factor. La asociación perfecta puede ser estricta o implícita de tipo
2:[^150022_tablas_conting-12]

[^150022_tablas_conting-12]: La asociación perfecta implícita de tipo 1,
    que no puede darse en tablas $2\times 2$, consiste en que, dado un
    nivel de un factor, el nivel del otro queda inmediatamente
    determinado; pero dado el nivel de este último no se puede
    determinar el nivel del primero.

-   Estricta: dado el nivel de un factor, el nivel del otro queda
    inmediatamente determinado.
-   Implícita de tipo 2: dado un nivel de un factor, el nivel del otro
    queda inmediatamente determinado; dado el otro nivel, no queda
    determinado el nivel del otro factor.

\index{asociación!positiva} \index{asociación!negativa}
\index{asociación!perfecta y estricta}
\index{asociación!perfecta e implícita de tipo 1}
\index{asociación!perfecta e implícita de tipo 2}

### La *Q* de Yule

\index{medidas de asociación!en tablas $2\times 2$!Q@\textit{Q} de Yule}

En caso de independencia, las frecuencias observadas coinciden con las
esperadas. A medida que las primeras se separan de las segundas, se
produce un alejamiento de dicha hipótesis y los niveles de los factores
aumentan la intensidad de su asociación. Por consiguiente, las
diferencias ${D}_{ij}$ entre las frecuencias observadas y las esperadas
bajo el supuesto de independencia pueden ser la base de una magnífica
medida de asociación. A mayores diferencias, mayor asociación. Más
sencillo todavía: una única diferencia, por ejemplo la ${D}_{11}$,
podría servir como medida de asociación porque, como bajo la hipótesis
de independencia, ${D}_{ij}=0$ y
${D}_{11}={D}_{22}= -{D}_{12}=-{D}_{21}$, entonces se tiene que:

-   En caso de independencia: ${D}_{11}={D}_{22}= 0$ y
    ${D}_{12}={D}_{21}=0$, o simplemente, ${D}_{11}=0$.

-   En caso de asociación positiva: ${D}_{11}={D}_{22} \geq 0$ y
    ${D}_{12}={D}_{21}\leq 0$, o simplemente, ${D}_{11}\geq 0$.

-   En caso de asociación negativa: ${D}_{11}={D}_{22} \leq 0$ y
    ${D}_{12}={D}_{21}\geq 0$, o simplemente, ${D}_{11}\leq 0$.

Por tanto, ${D}_{11}$ determina muy fácilmente la dirección de la
asociación. Sin embargo, en cuanto a la intensidad de la misma, el campo
de variación de ${D}_{11}$,
$\left[-\frac {N_{12} N_{21}} {n};\frac {N_{12} N_{21}} {n}\right]$,
depende de los valores de las frecuencias observadas (esto es un
problema a la hora de la interpretación) y la máxima intensidad
asociativa se da cuando la diagonal descendente o la diagonal ascendente
solo contienen ceros, es decir en caso de asociación perfecta estricta
(negativa o positiva).

Para solucionar el problema anterior, se define la ${Q}$ de Yule como:

$${Q}=\frac {n {D}_{11}} {N_{11}N_{22} - N_{12}N_{21}}=\frac{N_{11}N_{22} - N_{12}N_{21}} {N_{11}N_{22}+ N_{12}N_{21}}.$$

El campo de variación de ${Q}$ es $[-1;1]$ y:

-   En caso de independencia, ${Q}=0$.
-   En caso de asociación positiva, ${Q}< 0$.
-   En caso de asociación negativa, ${Q}> 0$.

Por tanto, cuando se sustituyen los datos de la tabla observada en
${Q}$, obteniéndose su valor muestral u observado:

$$\hat{Q}=\frac{n_{11}n_{22} - n_{12}n_{21}} {n_{11}n_{22}+ n_{12}n_{21}},$$
se actúa como sigue:

-   Cuando $\hat{Q}=0$ se dice que hay independencia.
-   Cuando $\hat{Q}< 0$ se dice que hay asociación negativa.
-   Cuando $\hat{Q}> 0$ se dice que hay asociación positiva.

Lógicamente, a mayor valor absoluto de $\hat{Q}$ mayor intensidad de la
asociación.

En el ejemplo utilizado para ilustrar el diseño experimental de tipo 1,
el valor observado de $Q$ es $\hat{Q}=0,83$:


```r
YuleQ(tabla_jud)
#> [1] -0.826087
```

A la luz del valor del valor de $\hat{Q}$ se concluye la existencia de
una fuerte asociación negativa.

### Otras medidas de asociación para tablas $2 \times 2$

#### Cuadrado medio de la contingencia de Pearson {#cmc}

\index{medidas de asociación!en tablas $2\times 2$!cuadrado medio de la contingencia}
\index{medidas de asociación!en tablas R $ \times $ C!coeficiente de contingencia}

La primera medida de asociación que suele venirnos a todos a la cabeza
es el propio estadístico de contraste $\chi^2$. Sin embargo, no puede
utilizarse como medida de asociación porque es siempre positivo y, sobre
todo, porque su valor máximo, $n(k-1)$, depende del tamaño muestral $N$
y de $k$, el número más pequeño de filas o columnas. En el caso
$2\times 2$ depende únicamente de $n$ porque $k-1=1$. Para eliminar el
efecto de tamaño muestral, se define el cuadrado medio de la
contingencia de Pearson como:

$${\phi^2}= \frac{\chi^2}{n}=\frac{\left(N_{11}N_{22} - N_{12}N_{21}\right)^2} {N_{1.} N_{2.} N_{\cdot1}N_{\cdot2}},$$
y se estima como
$$\hat{\phi^2}=\frac{\left(n_{11}n_{22} - n_{12}n_{21}\right)^2} {n_{1.} n_{2.} n_{\cdot1}n_{\cdot2}}$$
a partir de la tabla observada.

Su campo de variación es $[0; 1]$, tomando el valor 0 en caso de
independencia y 1 cuando hay asociación perfecta y estricta. Cuanto
mayor sea el valor del coeficiente, mayor es la intensidad de la
asociación.

No proporciona la dirección de la asociación, si bien se puede saber por
el signo de $n_{11}n_{22}-n_{12}n_{21}$. Otra consideración importante
es que, si se codifican los niveles de los factores como (0; 1),
${\phi}^2$ coincide con el coeficiente de determinación lineal entre los
factores. Por tanto, la asociación que mide es "lineal" (de ahí que su
valor suela ser más bajo que el de ${Q}$). Su raíz cuadrada es conocida
como la *V* de Cramer. En el ejemplo utilizado se tiene que:

\index{medidas de asociación!en tablas $2\times 2$!V@\textit{V} de Cramer}


```r
(Phi(tabla_jud))^2
#> [1] 0.1420701
```

#### *Odds ratio* o cociente de posibilidades[^150022_tablas_conting-13]

[^150022_tablas_conting-13]: Para no confundirlo con el riesgo relativo.

\index{medidas de asociación!en tablas $2\times 2$!odds@\textit{odds} ratio}
\index{medidas de asociación!en tablas $2\times 2$!riesgo relativo}

Se define como $\alpha=\frac {P_{11}/P_{12}} {P_{21}/P_{22}}$, donde
$P_{ij}$ es la probabilidad de que un elemento de la tabla pertenezca al
*i*-ésimo nivel de *A* y al *j*-ésimo de *B*, y se estima como
$\hat{\alpha}=\frac {n_{11}/n_{12}} {n_{21}/n_{22}}=\frac {n_{11}n_{22}} {n_{12}n_{21}}$.

Su campo de variación es $[0;\infty)$, asimétrico, y por consiguiente,
difícil de interpretar. En todo caso:

-   Si $\alpha < 1$, la probabilidad (aquí denominada posibilidad) de
    pertenecer al nivel 1 del factor B es menor en el nivel 1 del factor
    $A$ que en el 2.
-   Si $\alpha = 1$, la probabilidad de pertenecer al nivel 1 del factor
    $B$ es la misma en ambos niveles del factor $A$.
-   Si $\alpha > 1$, la probabilidad de pertenecer al nivel 1 del factor
    $B$ es mayor en el nivel 1 del factor $A$ que en el 2.

Una posible solución a la dificultad de interpretación es definir
$ln{\alpha}$, que es una medida simétrica en $(-\infty;+\infty)$. Sin
embargo, su interpretación, dada la gran amplitud del campo de
variación, continúa siendo muy difusa.

<!-- tal que: -->

<!-- -   Si $ln\hat{\alpha} \leq 0$,la posibilidad de trabajar en el colectivo de los varones es igual que en el colectivo de las mujeres. -->

<!-- -   Si $ln\hat{\alpha}= 0$, la posibilidad de trabajar en el colectivo de los varones es mayor que en el colectivo de las mujeres. -->

<!-- -   Si $ln\hat{\alpha} \geq 0$, la posibilidad de trabajar en el colectivo de los varones es menor que en el colectivo de las mujeres. -->

<!-- Sin embargo, la interpretación, dada la gran amplitud del campo de variación, aunque simétrico, continúa siendo muy difusa. -->

Una ventaja que tiene respecto a ${\alpha}$ es que no cambia si las
filas se convierten en columnas y las columnas en
filas.[^150022_tablas_conting-14] Por ello, la razón de posibilidades se
puede utilizar no solo en estudios retrospectivos, sino también en
aquellos prospectivos y transversales. Finalmente, nótese que $(i)$ la
razón de posibilidades y el riesgo relativo ($P_1/P_2$) se relacionan
como sigue: $\alpha= \frac {P_1 (1-P_2)} {P_2 (1-P_1)}$; y que $(ii)$
ambos son similares cuando la probabilidad de éxito $P_i$ está cerca de
cero en ambos grupos.

[^150022_tablas_conting-14]: Es decir, no es necesario identificar la
    variable respuesta para utilizar esta medida.

El código siguiente proporciona el valor muestral de $\alpha$
($\hat\alpha$) en el ejemplo que nos ocupa, así como su intervalo de
confianza del 95%:


```r
library("epiR")
epi.2by2(tabla_jud, method = "cohort.count")$massoc.summary[2, ]
#>              var       est      lower     upper
#> 2 Inc odds ratio 0.0952381 0.01021365 0.8880565
```

## Medidas de asociación en tablas $R$ $\times$ $C$ {#medidas-rxc}

\index{medidas de asociación!en tablas R $ \times $ C}

En caso de rechazo de la hipótesis de independencia en una tabla
$R\times C$, se concluye que al menos un nivel de uno de los factores
está asociado con uno del otro factor. En ese caso, se utilizarán las
medidas de asociación para determinar la intensidad de la misma. Las
asociaciones de determinados niveles del factor $A$ con determinados
niveles del factor $B$ que llevan al rechazo de la independencia de
ambos se denominan **fuentes de asociación**, y se determinan mediante
los residuos estandarizados ajustados.

\index{fuentes de asociación} \index{residuos!estandarizados!ajustados}

### Medidas derivadas del estadístico Chi-cuadrado

\index{medidas de asociación!en tablas R $ \times $ C!derivadas del estadístico Chi-cuadrado}

Como se avanzó en la Sec. \@ref(cmc), el estadístico $\chi^2$ no puede
utilizarse como medida de asociación porque su valor máximo, $n(k-1)$,
siendo $n$ el tamaño muestral y $k$ el número más pequeño de filas o
columnas, depende tanto de $N$ como del número de niveles de los
factores.

El cuadrado medio de la contingencia, $\phi^2$, elimina el efecto
"tamaño muestral", pero no el efecto "número de niveles de los
factores". Igual le ocurre al coeficiente de contingencia y a la *T* de
Tschuprow, salvo en las tablas cuadradas. La única medida derivada del
estadístico $\chi^2$ que corrige ambos efectos es la *V* de Cramer:
$$V=\sqrt\frac{\chi^2}{kn},$$ con $k=min\left(R-1; C-1\right)$. Su campo
de variación es $[0,1]$ y alcanza su máximo en caso de asociación
perfecta. En tablas cuadradas $V=T$.

\index{medidas de asociación!en tablas R $ \times $ C!cuadrado medio de la contingencia}
\index{medidas de asociación!en tablas R $ \times $ C!coeficiente de contingencia}
\index{medidas de asociación!en tablas R $ \times $ C!T@\textit{T} de Tschuprow}
\index{medidas de asociación!en tablas R $ \times $ C!V@\textit{V} de Cramer}

En el ejemplo utilizado en la Sec. \@ref(contaprox):


```r
CramerV(tabla_ayuso)
#> [1] 0.0590406
```

Aunque se rechaza la hipótesis de independencia, la asociación existente
entre la opinión sobre la presidente y la zona geográfica es muy
pequeña. Y ello porque, sea cual sea la zona geográfica, aunque hay
ligerísimas variaciones, la opinión es muy favorable: para alrededor del
60% es excelente, para la tercera parte muy buena y tan solo para el 5%
es mala (alrededor del 4%) o muy mala (apenas el 1%).

### Medidas basadas en la reducción proporcional del error: $\lambda$ de Goodman y Kruskal

\index{medidas de asociación!en tablas R $ \times $ C!basadas en la reducción proporcional del error}
\index{medidas de asociación!en tablas R $ \times $ C!lambda de Goodman y Kruskal}

Al contrario que las medidas basadas en el estadístico Chi-cuadrado,
exigen determinar cuál es el factor explicativo y cuál el factor a
explicar. Sea *A* el factor explicativo y *B* el factor a explicar:
supóngase que se selecciona aleatoriamente uno de los elementos de la
tabla. Este elemento pertenecerá a un nivel *i* de *A* y a un nivel *j*
de *B*. Supóngase que se quiere predecir el nivel de *B* al que
pertenece, $(i)$ sin utilizar el hecho de saber a qué nivel de *A*
pertenece, y $(ii)$ utilizando dicho hecho. Lógicamente, tanto en el
caso $(i)$ como en el $(ii)$ se comete un error $(P(i)$ y $P(ii)$,
respectivamente). La probabilidad de error será la misma si los factores
son independientes. Sin embargo, si están asociados, el conocimiento del
nivel de *A* al que pertenece el elemento seleccionado ayudará en la
predicción del nivel de *B* al que pertenece (tanto más cuanto más
asociados estén los factores) y la probabilidad de error disminuirá
respecto al caso $(i)$. La reducción proporcional que se opera en el
error es:

$$\lambda=\frac{P(i)-P(ii)} {P(i)},$$

donde $P(i)$ y $P(ii)$ se estiman como sigue:
$\hat{P}(i)= n- \max_{j} n_{\cdot j}$ y
$\hat{P}(ii)= n-\sum_{j=1}^C \max_{j} n_{\cdot j}.$

En el caso en que *A* sea el factor a explicar,
$\hat{P}(i)= n- \max_{i} n_{i.}$ y
$\hat{P}(ii)= n-\sum_{i=1}^R \max_{i} n_{i.}$.

En caso de no tener claro cuál es el factor a explicar, se utiliza la
media agregativa de las dos medidas anteriores:

$$\hat\lambda=\frac{\sum_{j=1}^C \max_i n_{i.}-max_i n_{i.}+\sum_{i=1}^R \max_j n_{\cdot j}-\max_j n_{\cdot j} }{2n-\max_i n_{i.}\max_j n_{\cdot j}}.$$

Su campo de variación es $[0,1]$. En caso de independencia, $\lambda=0$.
Ahora bien, que $\hat\lambda=0$ no implica necesariamente que *A* y *B*
tengan que ser independientes, puesto que $\lambda$ también toma el
valor 0 cuando en uno de los niveles del factor a explicar las
frecuencias son superiores a las de los demás niveles, y ello para todos
los niveles del factor explicativo, aunque los factores no sean
independientes. En caso de asociación, $0< \lambda \leq 1$, alcanzándose
la unidad en caso de asociación perfecta.

Una limitación de $\lambda$ (además de la anterior y de que exige
determinar el factor explicativo y el factor a explicar) es su
sensibilidad a totales marginales desequilibrados; en este caso, toma
valores anormalmente bajos. Tal es el caso del ejemplo que nos ocupa,
donde $\hat\lambda=0$, con factor a explicar la opinión, y, sin embargo,
los factores no son independientes. Y es que, sea cual sea la zona
geográfica, las frecuencias de la categoría de opinión "excelente" son
siempre las más elevadas.


```r
Lambda(tabla_ayuso, direction = "row")
#> [1] 0
```

### Determinación de las fuentes de asociación

En el caso de tablas $R \times C$, el rechazo a la hipótesis de
independencia no indica que cada nivel de uno de los factores esté
asociado con uno de los niveles del otro factor, como en las tablas
$2\times 2$. Lo que indica es que al menos uno de los niveles de uno de
los factores está asociado con un nivel del otro. Por tanto, puede ser,
y así es normalmente, que dicho rechazo se deba a que algunos niveles de
uno de los factores (incluso solo uno) están asociados con alguno de los
del otro factor. Ya no hay dirección de la asociación, sino fuentes de
asociación.

\index{fuentes de asociación}

Para identificar las fuentes de asociación lo lógico es fijarse en cada
celda en las diferencias entre la frecuencia observada y la esperada
bajo el supuesto de independencia (tales diferencias juegan el papel de
un término de error). Pero su interpretación depende del tamaño de la
frecuencia esperada, y por ello se estandarizan, es decir, se ponen en
relación con la raíz cuadrada de las correspondientes frecuencias
esperadas.

\index{residuos!estandarizados}
\index{residuos!estandarizados!ajustados} \index{residuos!de Haberman}

Para decidir si tales diferencias estandarizadas son significativamente
grandes (asociación) o no (independencia), se necesita conocer su
distribución de probabilidad bajo la hipótesis de independencia. Para
ello se dividen por su desviación típica porque de esta manera tienen
aproximadamente una distribución $N(0;1)$. Cuando estas diferencias
estandarizadas divididas por sus desviaciones típicas se calculan (se
estiman) a partir de los resultados observados y dispuestos en la tabla,
se denominan residuos estandarizados ajustados (o de Haberman), y son
los que se utilizan para identificar las fuentes de asociación.

Por tanto, la estimación de las diferencias (los residuos) son:
$$\hat{R}_{ij}=n_{ij}-\hat{E}_{ij},$$ y los de las diferencias
estandarizadas (los residuos estandarizados) vienen dados por:

$$\hat{R}_{ij}(est)=\frac {n_{ij}-\hat{E}_{ij}}{\sqrt{\hat{E}_{ij}}},$$
mientras que la siguiente expresión corresponde a las estimaciones de
las diferencias estandarizadas ajustadas, que no son otras que los
denominados residuos estandarizados ajustados:

$$\hat{R}_{ij}(est;ajd)=\frac{\hat{R}_{ij}(est)}{\sqrt{\left(1-\frac {n_{i.}} {N}\right)\left(1-\frac {n_{\cdot j}} {N}\right)}}.$$

Habrá una fuente de asociación en cada celda $\{i; j\}$ que verifique:
$|\hat{R}_{ij}(est;ajd)|\geq{k}$, con $k$ = 2,33; 1,96; 1,64 para
$\alpha$ = 0,01; 0,05; 0,10, respectivamente.

En el ejemplo que nos ocupa, los residuos estandarizados ajustados son:


```r
library("questionr")
chisq.residuals(tabla_ayuso, digits = 2, std = TRUE)
#>                opinion
#> zona            n1_nefasta n2_mala n3_buena n4_excelente
#>   n1_mad_muni         0.79   -1.04    -3.77         2.41
#>   n2_metropol        -0.51    1.74     2.88        -2.78
#>   n3_extraradio      -0.45   -0.76     1.59         0.17
```

Asumiendo $\alpha$ = 0,05, las fuentes de asociación son "Madrid
municipio-excelente" a costa de "buena" y "Madrid metropolitano-buena" a
costa de "excelente".

## Contrastes de independencia en tablas multidimensionales

\index{contraste de independencia!en tablas multidimensionales}

En tablas con más de dos factores (el objetivo aquí es el caso
$R \times C \times M$, por simplicidad), no solo se puede contrastar la
hipótesis de independencia global, sino que, en caso de ser rechazada,
también se pueden contrastar las hipótesis de $(i)$ independencia
parcial: dos factores están asociados y el tercero es independiente de
ellos, y $(ii)$ independencia condicional: dos de los factores son
independientes para cada nivel del tercero, pero están asociados con él.

En los tres casos, el estadístico de contraste (contraste aproximado) es
$$\chi^2=\sum_{i=1}^R \sum_{j=1}^C \sum_{m=1}^M \frac{\left (N_{ijm}-{E}_{ijm}\right)^2}{{E}_{ijm})},$$
con los siguientes grados de libertad (g.l.) y ${E}_{ijm}$ bajo la
correspondiente $H_0$:

\index{independencia!global} \index{independencia!parcial}
\index{independencia!condicional}

**Independencia global**

$dl=(R\times C\times M)-(R-1)-(C-1)-(M-1)-1$ y
${E}_{ijm}=\frac{N_{i..}N_{.j.}N_{..m}}{n^2}$

**Independencia parcial**

*A* y *B* asociados entre sí pero independientes de *C*:

-   $g.l.=(R\times C\times M)-(R\times C-1)-(M-1)-1$ y
    ${E}_{ijm}=\frac{N_{ij.}N_{..m}}{n}$

*A* y *C* asociados entre sí pero independientes de *B*:

-   $g.l.=(R\times C\times M)-(R\times M-1)-(C-1)-1$ y
    ${E}_{ijm}=\frac{N_{i.m}N_{.j.}}{n}$

*B* y *C* asociados entre sí pero independientes de *A*:

-   $g.l.=(R\times C\times M)-(C\times M-1)-(R-1)-1$ y
    ${E}_{ijm}=\frac{N_{.jm}N_{i..}}{n}$

**Independencia condicional**

*A* y *B* son independientes entre sí, pero están asociados con *C*:

-   $g.l.=(R\times C\times M)-(R\times M-1)-(C\times M-1)-1$ y
    ${E}_{ijm}=\frac{N_{i.m}N_{.jm}} {n}$

*A* y *C* son independientes entre sí, pero están asociados con *B*:

-   $g.l.=(R\times C\times M)-(R\times C-1)-(M\times C-1)-1$ y
    ${E}_{ijm}=\frac{N_{ij.}N_{.jm}}{n}$

*B* y *C* son independientes entre sí, pero están asociados con *A*:

-   $g.l.=(R\times C\times M)-(C\times R-1)-(M\times R-1)-1$ y
    ${E}_{ij.}=\frac{N_{i.m}N_{.jm}}{n}$

Para calcular el valor muestral del estadístico de contraste para las
diferentes hipótesis, basta con sustituir las $N_{ij}$ por las
frecuencias observadas (las $n_{ij}$) y los totales marginales
($N_{i.m}$, $N_{.j.}$, $N_{i..}$, etc.) por los totales marginales
observados y que figuran en la tabla que surge de los datos en estudio
($n_{i.m}$, $n_{.j.}$, $n_{i..}$, etc.).

También son interesantes las relaciones de segundo orden o superior (por
ejemplo, si la asociación entre dos de los factores difiere en dirección
y/o intensidad para distintos niveles del tercero), pero se estudian
mediante **modelos logarítmico lineales**.

\index{modelo!logarítmico lineal}

::: {.infobox_resume data-latex=""}
### Resumen {.unnumbered}

-   Las tablas de contingencia analizan la relación entre variables
    categóricas.

-   Su análisis responde preguntas como: los factores involucrados en la
    tabla, ¿son independientes o están asociados? Si están asociados,
    ¿qué niveles de dichos factores son los que están asociados?, ¿cuál
    es la intensidad de dicha asociación?

-   Se aborda ampliamente el caso de tablas bifactoriales y se proponen
    tests exactos y aproximados para el contraste de la hipótesis de
    independencia (para tres procedimientos de muestreo diferentes) y
    una selección de medidas de asociación.

-   Finalmente, se hace una breve incursión en el ámbito de las tablas
    multidimensionales.
:::
