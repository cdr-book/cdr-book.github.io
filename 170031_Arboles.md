# (PART) Machine learning supervisado {.unnumbered}

# Árboles de clasificación y regresión {#cap-arboles}

*Ramón A. Carrasco*$^{a}$ e *Itzcóatl Bueno*$^{b,a}$

$^{a}$Universidad Complutense de Madrid
$^{b}$Instituto Nacional de Estadística

\nocite{theobald2017machine,burkov2019hundred,boehmke2019hands}

## Introducción {#intro_dectree}

Los árboles de decisión son modelos que se utilizan principalmente para
la resolución de problemas de clasificación, en los que hay que predecir
las distintas categorías de la variable objetivo o dependiente, aunque
también son aplicables a la predicción de valores numéricos de dicha
variable objetivo, esto es, como modelos de regresión. De ahí que sean
conocidos como árboles de clasificación y regresión (CART,
Classification and Regression Trees). Algunos ejemplos de árboles de
decisión son:\index{árbol!de decisión}

-   **Clasificación**:\index{árbol!de clasificación} en la medida que
    la variable objetivo debe ser categórica se podrían usar por ejemplo
    para tomar la decisión de qué empleados deberían de promocionar
    (variable con dos categorías: sí promocionar o no promocionar) en
    base a sus méritos, capacidades, edad, etc. Otro ejemplo podría ser
    su uso para decidir sí se juega o no un partido de tenis en base a
    la climatología prevista. Este ejemplo se muestra gráficamente en la
    Fig. \@ref(fig:dectree-plot). En este último caso, el algoritmo que
    se utilice indicará la decisión a tomar en base a los registros
    climatológicos de los partidos que ya se hayan jugado. Así, si un
    determinado día se quiere jugar al tenis, se deberán tomar como
    variables de entrada las previsiones de Tipo de día (soleado,
    nublado o lluvioso), la fuerza del Viento y la Humedad. En caso de
    ser un día nublado, el algoritmo sugerirá que se juegue. En caso de
    ser soleado, comprobará el nivel de Humedad y, si no es muy elevada,
    recomendará que se juegue el partido. Lo mismo pasará si la
    previsión es de lluvia pero la fuerza del Viento prevista no es lo
    suficientemente intensa como para impedir el normal desarrollo del
    partido.

<div class="figure" style="text-align: center">
<img src="img/dectree_tenis.png" alt="Ejemplo de árbol de decisión." width="60%" />
<p class="caption">(\#fig:dectree-plot)Ejemplo de árbol de decisión.</p>
</div>

-   **Regresión**:\index{árbol!de regresión} siguiendo con el ejemplo
    del partido de tenis, también se puede aplicar un árbol de decisión
    para determinar cuántas horas jugar de acuerdo a las condiciones
    climatológicas. En la Fig. \@ref(fig:dectree-plot) se sustituirían la predicciones
    dicotómicas SI/NO por valores numéricos, como se muestra en la Fig. \@ref(fig:regtree-plot). Por ejemplo, el algoritmo puede sugerir jugar 5 horas si el día está soleado pero la Humedad
    es del 30% de vapor de agua por $m^3$, y 3,5 horas si está soleado
    pero la Humedad es del 80%. También puede decidir que si el día está
    nublado se jueguen 4 horas. O en caso de lluvia, podría decidir que
    el partido dure 0,75 horas si la fuerza del Viento es de 62km/h y
    1,15 horas si es de 27km/h.

<div class="figure" style="text-align: center">
<img src="img/tenis-tree-reg.png" alt="Ejemplo de árbol de regresión" width="60%" />
<p class="caption">(\#fig:regtree-plot)Ejemplo de árbol de regresión</p>
</div>

\index{algoritmo!de árbol} Como se ha comentado, CART es un término
genérico para describir este tipo de algoritmos de árbol y también un
nombre específico para el algoritmo original de
[@breiman1984classification] de construcción de árboles de clasificación
y regresión. Sin embargo, existen otros como el ID3 (Induction Decission
Trees), o el C4.5 que está basado en el ID3. En la Tabla
\@ref(tab:alg-dectree) se muestra una pequeña comparativa de estos tres
algoritmos:

| Algoritmo | Criterio de división    | Tipo de variables input |           Estrategia de poda           |
|-----------------:|:-----------------|------------------|:----------------:|
|       ID3 | Ganancia de información | Solo categóricas        |                No poda                 |
|      CART | Índice de Gini          | Categóricas y numéricas | Poda basada en el coste de complejidad |
|      C4.5 | Ratio de ganancia       | Categóricas y numéricas |        Poda basada en el error         |

: (#tab:alg-dectree) Características de los principales algoritmos de
árboles de decisión.

Los árboles de decisión tienen múltiples ventajas. Entre ellas destacan:

-   Son fáciles de entender e interpretar. Su visualización clara
    permite interpretar la salida del modelo y entender su proceso como
    un conjunto de condicionantes.
-   El mismo algoritmo incorporado en `R` (CART) es válido tanto para
    problemas de clasificación como de regresión y, por tanto, la
    variable objetivo puede ser continua o categórica. Respecto al resto
    de variables de entrada, las independientes, comentar que puede ser
    tanto categóricas como numéricas. Al contrario que ocurre con otros
    algoritmos, este último tipo de variables no requieren la
    estandarización, puesto que se basa en reglas y no en el cálculo de
    distancias entre observaciones.
-   Tratan mejor que otros algoritmos el problema de la no linealidad.
-   Respecto a los datos, hacen un tratamiento automático de valores
    ausentes (en la mayoría de los árboles de clasificación) y no se ven
    afectados con las observaciones atípicas.

Sin embargo, también tienen ciertas desventajas:

-   Son inestables ya que la inclusión de una nueva observación en la
    fase de entrenamiento obliga a reconstruirlo, pudiendo modificar la
    estructura del árbol final.
-   No son recomendables en caso de grandes conjuntos de datos, puesto
    que el modelo entrenado puede estar sobreajustado. Este sobreajuste
    es el principal problema de los árboles de decisión, ya que modelos
    demasiados complejos pueden ajustar muy bien los datos observados,
    pero también pueden cometer muchos errores en la fase de predicción.
    Cuando esta circunstancia se da, el modelo ha aprendido los datos de
    entrenamiento pero no la generalidad del problema que es lo que
    normalmente se pretende. El sobreajuste da lugar también a una
    varianza elevada.

## Procedimiento con R: la función `rpart()`

En el paquete `rpart` de **R** se encuentra la función `rpart()` que se
utiliza para entrenar un árbol de decisión:


```r
rpart(formula, data, ...)
```

-   `formula`: Refleja la relación entre la variable dependiente $Y$ y
    los predictores tal que $Y \sim X_1 + ... + X_p$.
-   `data`: Conjunto de datos con el que entrenar el árbol de acuerdo a
    la fórmula indicada.

## Árboles de clasificación\index{árbol!de clasificación}

Formalmente, un árbol de decisión es un grafo acíclico (un grafo sin
ciclos, siendo un ciclo un circuito completo) que se inicia en un **nodo
raíz**\index{nodo raíz}, el cual se divide en **ramas**\index{rama},
también conocidas como **aristas**\index{arista}. De las ramas salen las
**hojas**\index{hoja}, también denominadas **nodos**\index{nodo} Estos
nodos pueden ser nodos finales o **puntos de
decisión**\index{punto!de decisión} (si de ellos no salen nuevas ramas
con nuevos nodos) o no (de ellos salen nuevas ramas con nuevas hojas o
nodos) y así hasta que todos los nodos sean puntos de decisión. En el
ejemplo de la Fig. \@ref(fig:dectree-plot) el nodo raíz es la caja *Tipo
de día*. Las ramas o aristas, son sus tres niveles o categorías:
*Soleado*, *Nublado* o *Lluvia*. Cada una de estas ramas conecta con una
nueva hoja o nodo: *Humedad* o *Viento* en los casos de soleado o
lluvia, respectivamente. Sin embargo, en ese ejemplo, *Nublado*
representa un nodo terminal, puesto que, llegado a ese punto, la salida
que proporcionaría el árbol es *"Jugar al tenis"*. Este proceso se
repite utilizando el conjunto de datos disponible en cada hoja,
generándose una clasificación final cuando una hoja no tenga ramas
nuevas, en cuyo caso recibe la denominación de nodo final. El objetivo
es que el árbol sea lo más general y pequeño posible. Esto se consigue
seleccionando, en cada paso, la variable que optimice la división de los
datos en conjuntos homogéneos, de tal forma que se prediga mejor la
clase objetivo.

### ¿Cómo se va formando el árbol de clasificación? \index{partición}

Como ya se ha mencionado, en la construcción de un árbol de decisión se
va dividiendo en nuevas ramas de forma recursiva, es decir, cada división
está condicionada por las anteriores. El objetivo en cada hoja, es
encontrar la variable más adecuada para dividir los datos de ese nodo en
dos nuevas hojas, de tal forma que el error global entre la clase
observada y la predicha por el árbol se minimice. Para la construcción
de árboles de clasificación, el algoritmo CART utiliza la medida de
impureza de Gini para generar las particiones, mientras que los
algoritmos ID3 y C4.5 están basados en las de entropía.

#### Impureza de Gini\index{impureza!de Gini}

La **Impureza de Gini**, utilizada por el algoritmo CART, es una medida
de la frecuencia con la que una observación elegida aleatoriamente del
conjunto se asignaría a la clase errónea si se etiqueta al azar en una
de las clases que se consideran. Formalmente, sea $X$ un conjunto de
datos con $\kappa$ clases, y sea $p_i$ la probabilidad de que una
observación pertenezca a la clase $i$. La Impureza de Gini para $X$ se
define como:

```{=tex}
\begin{equation}
Gini(X) = 1 - \sum^{\kappa}_{i=1}{p^{2}_{i}}
\end{equation}
```
Como se ha comentado, a la hora de construir el árbol se selecciona el
atributo con la menor impureza de Gini para dividir el conjunto de datos
en el nodo en dos. Si un conjunto de datos $X$ se divide en un atributo
$\varphi$ en dos subconjuntos $X_1$ y $X_2$ con tamaños $n_1$ y $n_2$,
respectivamente, la impureza ponderada de Gini se define como:

```{=tex}
\begin{equation}
Gini_{\varphi}(X) = \frac{n_1}{n}{Gini(X_{1})} + \frac{n_2}{n}{Gini(X_{2})}
\end{equation}
```
En el ejemplo de la Fig. \@ref(fig:dectree-plot) considérese la
siguiente situación:

| Día | Tipo de día | Humedad | Viento | Decisión |
|-----|:-----------:|:-------:|:------:|:--------:|
| 1   |   Soleado   | Fuerte  | Débil  |    NO    |
| 2   |   Soleado   | Fuerte  | Fuerte |    NO    |
| 3   |   Lluvia    | Fuerte  | Débil  |    SI    |
| 4   |   Nublado   | Fuerte  | Débil  |    SI    |
| 5   |   Lluvia    |  Débil  | Débil  |    SI    |
| 6   |   Lluvia    |  Débil  | Fuerte |    NO    |
| 7   |   Soleado   | Fuerte  | Débil  |    NO    |
| 8   |   Nublado   |  Débil  | Fuerte |    SI    |
| 9   |   Soleado   |  Débil  | Débil  |    SI    |
| 10  |   Lluvia    |  Débil  | Débil  |    SI    |
| 11  |   Soleado   |  Débil  | Fuerte |    SI    |
| 12  |   Nublado   | Fuerte  | Fuerte |    SI    |
| 13  |   Nublado   |  Débil  | Débil  |    SI    |
| 14  |   Lluvia    | Fuerte  | Fuerte |    SI    |
| 15  |   Soleado   | Fuerte  | Fuerte |    NO    |

: (#tab:data-imp-gini) Datos para decidir si se juega el partido

Para el *Tipo de día*, los datos se agruparían como muestra la Tabla
\@ref(tab:data-td-imp-gini), permitiendo el cálculo de la impureza de
Gini para cada una de sus categorías.

| Tipo de día | SI  | NO  | \# observaciones |
|-------------|:---:|:---:|:----------------:|
| Soleado     |  2  |  4  |        6         |
| Nublado     |  4  |  0  |        4         |
| Lluvia      |  4  |  1  |        5         |

: (#tab:data-td-imp-gini) Días que se juega o no de acuerdo al *Tipo de
día*

\begin{equation*}
Gini(Soleado) = 1 - \Bigl(\frac{2}{6}\Bigr)^{2} - \Bigl(\frac{4}{6}\Bigr)^{2} = 0,45
\end{equation*} 
\begin{equation*}
Gini(Nublado) = 1 - \Bigl(\frac{4}{4}\Bigr)^{2} = 0
\end{equation*} 
\begin{equation*}
Gini(Lluvia) = 1 - \Bigl(\frac{4}{5}\Bigr)^{2} - \Bigl(\frac{1}{5}\Bigr)^{2} = 0,32
\end{equation*}

Ahora, se calcula la suma ponderada de la impureza de Gini para la
variable *Tipo de día*:

```{=tex}
\begin{equation*}
Gini(\text{Tipo de día}) = 0,45\cdot\Bigl(\frac{6}{15}\Bigr) + 0\cdot\Bigl(\frac{4}{15}\Bigr) + 0,32\cdot\Bigl(\frac{5}{15}\Bigr) = 0,29
\end{equation*}
```
Del mismo modo, se puede calcular la impureza de Gini para el resto de
variables. La Tabla \@ref(tab:hum-imp-gini) y la Tabla
\@ref(tab:wind-imp-gini) presentan los resultados para *Humedad* y
*Viento*, respectivamente.

| Humedad | SI  | NO  | \# observaciones | $p_{SI}$ | $p_{NO}$ | Impureza de Gini |
|---------|:---:|:---:|:----------------:|:--------:|:--------:|:----------------:|
| Fuerte  |  4  |  4  |        8         |   0,50   |   0,50   |       0,50       |
| Débil   |  6  |  1  |        7         |   0,86   |   0,14   |       0,76       |

: (#tab:hum-imp-gini) Impureza de Gini para las categorías de Humedad

```{=tex}
\begin{equation*}
Gini(Humedad) = 0,5\cdot\Bigl(\frac{8}{15}\Bigr) + 0,76\cdot\Bigl(\frac{7}{15}\Bigr) = 0,62
\end{equation*}
```

| Viento | SI  | NO  | \# observaciones | $p_{SI}$ | $p_{NO}$ | Impureza de Gini |
|--------|:---:|:---:|:----------------:|:--------:|:--------:|:----------------:|
| Fuerte |  4  |  3  |        7         |   0,57   |   0,43   |       0,49       |
| Débil  |  6  |  2  |        8         |   0,75   |   0,25   |       0,38       |

: (#tab:wind-imp-gini) Impureza de Gini para las categorías de Viento

```{=tex}
\begin{equation*}
Gini(Viento) = 0,49\cdot\Bigl(\frac{7}{15}\Bigr) + 0,38\cdot\Bigl(\frac{8}{15}\Bigr) = 0,43
\end{equation*}
```
En la Tabla \@ref(tab:features-imp-gini) se puede ver que la impureza de
Gini para las tres variables incluidas en el ejemplo. La variable con la
menor impureza de Gini, el *Tipo de día*, es la elegida para ser el nodo
raíz del árbol de clasificación.

| Variable    | Impureza de Gini |
|-------------|:----------------:|
| Tipo de día |       0,29       |
| Humedad     |       0,62       |
| Viento      |       0,43       |

: (#tab:features-imp-gini) Impureza de Gini para las variables de
entrada

Al entrenar un árbol de decisión, se repite este proceso, y a la hora de
dividir cada nodo, se elige el atributo que proporcione el menor
$Gini_{\varphi}(X)$.

Para obtener la ganancia de información para una variable, las impurezas
ponderadas de los nodos hijos se restan de la impureza del nodo padre.
La ganancia de Gini para la variable X, $\Delta Gini()$, se calcula
así:

```{=tex}
\begin{equation}
\Delta Gini(\varphi) =  Gini(X) - Gini_{\varphi}(X)
\end{equation}
```
Siguiendo el ejemplo del árbol de clasificación, para saber si se puede
jugar al tenis o no, se tendría que obtener la impureza de Gini para el
nodo *Humedad* o el nodo *Viento*. Repitiendo el proceso anteriormente
mostrado, dado que el *Tipo de día* sea soleado, se obtienen los
resultados de la Tabla \@ref(tab:sunfeat-imp-gini).

| Variable | Impureza de Gini |
|----------|:----------------:|
| Humedad  |       0,00       |
| Viento   |       0,44       |

: (#tab:sunfeat-imp-gini) Impureza de Gini para las variables en días
soleados

Entonces, la ganancia de Gini para cada variable será:

```{=tex}
\begin{center}
$\Delta Gini(Humedad) = 0,45-0=0,45$\\
$\Delta Gini(Viento) =0,45-0,45=0$
\end{center}
```
Puede observarse que la ganancia de información al dividir por *Humedad*
es mayor que al hacerlo por *Viento*, por lo que el árbol se dividirá
respecto a la *Humedad*, como se observó en la Fig.
\@ref(fig:dectree-plot).

#### Entropía \index{entropía}

La entropía es un concepto matemático que mide la incertidumbre de una
fuente de información, es decir, la varianza en los datos entre
diferentes clases. Para cada nodo y su partición, la entropía se calcula
como:

```{=tex}
\begin{equation}
E = -p_1\log_2 (p_1) - p_2\log_2 (p_2)
(\#eq:entropy)
\end{equation}
```
donde $p_1$ y $p_2$ representan la probabilidad de pertenecer a cada una
de las clases en ese nodo. En teoría de la información, la base
logarítmica varía dependiendo de la aplicación, y con ella varía la
unidad de medida. En este caso, la ganancia de información se obtiene
como:

```{=tex}
\begin{equation}
IG = E_{\varkappa} - E_{\varkappa + 1},
\end{equation}
```
donde $E_\varkappa$ representa la entropía en el nodo padre, mientras
que $E_{\varkappa+1}$ es la entropía en el nodo que resulta de dividir
el nodo padre. Entonces, siguiendo el ejemplo basado en los datos de la
Tabla \@ref(tab:data-td-imp-gini) se tendría que la entropía en origen
es:

```{=tex}
\begin{equation*}
E = -\frac{10}{15}\log_2 \Bigl(\frac{10}{15}\Bigr) - \frac{5}{15}\log_2 \Bigl(\frac{5}{15}\Bigr) = 0,9183
\end{equation*}
```
Si se obtiene la entropía para cada variable, se determinará el nodo
raíz para aquel que aporte una mayor ganancia de información. En el caso
de la variable *Tipo de día* se calcula:

\begin{equation*}
E_{Soleado} = -\frac{2}{6}\log_2 \Bigl(\frac{2}{6}\Bigr) - \frac{4}{6}\log_2 \Bigl(\frac{4}{6}\Bigr) = 0,9183
\end{equation*} 
\begin{equation*}
E_{Nublado} = -\frac{4}{4}\log_2 \Bigl(\frac{4}{4}\Bigr) - \frac{0}{4}\log_2 \Bigl(\frac{0}{4}\Bigr) = 0
\end{equation*} 
\begin{equation*}
E_{Lluvia} = -\frac{4}{5}\log_2 \Bigl(\frac{4}{5}\Bigr) - \frac{1}{5}\log_2 \Bigl(\frac{1}{5}\Bigr) = 0,7219
\end{equation*}

Y por tanto:

```{=tex}
\begin{equation*}
E_{\text{Tipo de día}} = \frac{6}{15}\cdot 0,9183 + \frac{4}{15}\cdot 0 + \frac{5}{15}\cdot 0,7219 = 0,608
\end{equation*}
```
Repitiendo el mismo procedimiento con las variables *Viento* y *Humedad*
se puede comprobar que $E(Viento) = 0,893$ y $E(Humedad) = 0,809$. A
partir de esto se puede obtener la ganancia de información como:

\begin{equation*}
IG_{\text{Tipo de día}} = E - E_{\text{Tipo de día}} = 0,918 - 0,608 = 0,31
\end{equation*} 
\begin{equation*}
IG_{Viento} = E - E_{Viento} = 0,918 - 0,893 = 0,025
\end{equation*} 
\begin{equation*}
IG_{Humedad} = E - E_{Humedad} = 0,918 - 0,809 = 0,109
\end{equation*}

Se puede comprobar que la disminución de la aleatoriedad, o la ganancia
de información, es mayor para la variable *Tipo de día* y por tanto se
elige para ser el nodo raíz. Repitiendo este proceso se va construyendo
el árbol hasta alcanzar los nodos terminales.






### Sobreajuste \index{sobreajuste}

Ya se ha comentado en la Sec. \@ref(intro_dectree) que una de las
principales desventajas de los árboles de decisión es su propensión a
sobreajustar el modelo al conjunto de datos de entrenamiento y, por
tanto, hay que prestar especial atención a la complejidad del modelo.
Basándose en las observaciones utilizadas en la fase de entrenamiento,
un árbol de decisión puede extraer los patrones presentes en el conjunto
de observaciones de entrenamiento y ser muy preciso en el ajuste de
dichas observaciones. Sin embargo, puede ocurrir que el árbol resultante
no sea capaz de clasificar correctamente ni el conjunto de validación ni
nuevas observaciones. Esta circunstancia puede ocurrir porque haya
patrones no observados en los datos de entrenamiento que el modelo no es
capaz de detectar, o porque la división de los datos entre entrenamiento
y validación no se realizó correctamente siendo los datos de
entrenamiento no representativos del conjunto de datos completo.
Intentando que el árbol entrenado tenga la capacidad de aprender
patrones muy complejos, se puede producir este sobreajuste materializado
con árboles muy profundos. La forma de evitar el sobreajuste es
controlar el crecimiento del árbol para evitar que se vuelva
excesivamente complejo.

### ¿Cuánto debe crecer un árbol de clasificación?
\index{profundidad!del árbol}

En cada paso de construcción del árbol se determina la variable óptima
para realizar la división de las observaciones de un nodo padre en sus
nodos hijos. La pregunta es: ¿cuándo se detiene?, ¿cuál es el criterio
de parada? Por ejemplo, se puede utilizar como criterio de parada que el
árbol alcance un tamaño o profundidad determinado, para que no sea
excesivamente complejo y así no tengan lugar las consecuencias derivadas
del sobreajuste\index{sobreajuste}.

En consecuencia, se debe llegar a un equilibrio entre la profundidad y
complejidad\index{complejidad} del árbol para optimizar la predicción de
futuras observaciones. Este equilibrio se puede lograr siguiendo alguno
de los siguientes enfoques: la parada temprana o la poda.

#### La parada temprana 
\index{parada temprana}

La parada temprana restringe el crecimiento del árbol, tanto de
clasificación como de regresión, de forma explícita. Existen distintas
maneras de establecer esta restricción al árbol, pero dos de las
técnicas más populares son las de restringir la profundidad a un cierto
nivel o la de establecer un número mínimo de observaciones permitidas en
un nodo terminal. En el primer caso, el árbol deja de dividirse al
llegar a cierta profundidad. Así, cuanto menos profundo sea el árbol,
menos variación habrá en las predicciones que proporcione. Sin embargo,
existe el riesgo de introducir mucho sesgo al modelo al no ser capaz de
captar interacciones y patrones complejos en los datos. El segundo
enfoque lo que provoca es que no se dividan nodos intermedios con pocas
observaciones. En el caso extremo, si se permite que un nodo terminal
sólo contuviese una observación esta actuaría como predicción. De este
modo, los resultados probablemente no serían generalizables y tendrían
mucha variabilidad. En el otro extremo, si se exigen un gran número de
observaciones en el nodo terminal se reduce el número de divisiones y,
por lo tanto, se reduce la varianza.

#### La poda 
\index{poda}

El otro enfoque es el de la poda que consiste en construir un árbol muy
profundo y complejo y después podarlo para encontrar el subárbol óptimo.
Este subárbol se obtiene utilizando un hiperparámetro de complejidad
$(\zeta)$ que penaliza la función objetivo de la partición por el número
de nodos terminales del árbol $(\tau)$, es decir, se busca minimizar:

```{=tex}
\begin{equation}
R_{\zeta}(\tau) = R(\tau) + \zeta|\tau|
(\#eq:poda)
\end{equation}
```
Donde $R(\tau)$ es el error total de entrenamiento de los nodos,
$|\tau|$ es el número total de nodos y $\zeta$ es el hiperparámetro de
complejidad. A medida que $\zeta$ aumenta, más ramas del árbol son
podadas. Mientras que a valores más bajos, los modelos producidos son
más complejos y en consecuencia más grandes. En conclusión, a medida que
un árbol crece, el error de entrenamiento debe tener una reducción mayor
que la penalización por la complejidad.

### Ejemplo: Árbol de clasificación para determinar la intención de compra

A continuación se describe el caso que se va a resolver mediante modelos
de clasificación tanto en este como en los siguientes capítulos. Existen
diversas aserciones para definir Comercio Electrónico (CE). Entre ellas,
la Organización para la Cooperación y el Desarrollo Económico (OCDE) lo
define como el proceso de compra, venta o intercambio de bienes,
servicios e información a través de redes de comunicación, comúnmente
Internet. La clasificación más básica del CE se hace en base al tipo de
entes que se relacionan: empresas (businesses, B), consumidores
(consumers, C) y entes públicos (governments, G). De esta forma, una
empresa de CE convencional suele ser B2B si vende a otras empresas, B2G
si su relación comercial es con administraciones o B2C si vende a consumidores finales.

En este caso, se puede considerar que la empresa "Beauty eSheep" lleva a
cabo un CE de tipo B2C. Su producto estrella es una crema hidratante
unisex, denominada internamente como "Crema Luxury", con mucho éxito
entre su clientela. A partir de este producto inicial, la empresa ha ido
ofreciendo un catálogo de productos tanto de tanto de belleza como de
bienestar y salud.

Hace tiempo la empresa instauró una estrategia relacional, centrada en
el cliente, de tal manera que ha ido recabando diversos datos sobre los
mismos, incluidas las distintas compras que han realizado.

Basándose en los datos recopilados para cada cliente, la empresa quiere
realizar una campaña para impulsar la venta de tensiómetros digitales.
La empresa tiene acceso a un stock muy flexible en fechas de envío de
estos productos y el precio de los tensiómetros es muy bueno, por lo que
se espera una buena rentabilidad en su venta.

Por tanto, en este proyecto hay que identificar el público objetivo
susceptible de comprar dicho producto para ofrecérselo a través de la
plataforma de CE de la compañía, SMS y/o webmail durante el periodo que
dura la campaña.

La tabla con los datos integrados a nivel de cliente, incluyendo el
consumo de los distintos productos de la empresa, es **dp_ENTR**,
incluida en el paquete `CDR`, y que se resume en la Tabla
\@ref(tab:dpentr). Este ejemplo se va a replicar en el resto de
capítulos de machine learning supervisado para clasificación.

| *COLUMNA*     | *TIPO* | *DESCRIPCIÓN*                                                                                                         |
|-------------------|-------------------|----------------------------------|
| CLS_PRO_pro13 | Factor | Clase objetivo, es un indicador de si el cliente es consumidor de ese producto "Tensiómetro Digital" ('S') o no ('N') |
| ind_pro11     | Factor | Indicador de si el cliente es consumidor del producto "Fragancia Luxury" ('S') o no ('N')                             |
| ind_pro12     | Factor | Indicador de si el cliente es consumidor del producto "Depiladora Eléctrica" ('S') o no ('N')                         |
| ind_pro14     | Factor | Indicador de si el cliente es consumidor del producto "Crema Luxury" ('S') o no ('N')                                 |
| ind_pro15     | Factor | Indicador de si el cliente es consumidor del producto "Smartwatch Fitness" ('S') o no ('N')                           |
| ind_pro16     | Factor | Indicador de si el cliente es consumidor del producto "Kit Pesas Inteligentes" ('S') o no ('N')                       |
| ind_pro17     | Factor | Indicador de si el cliente es consumidor del producto "Estimulador Muscular" ('S') o no ('N')                         |
| importe_pro11 | Doble  | Importe neto global gastado por el cliente en ese producto en euros                                                   |
| importe_pro12 | Doble  | Importe neto global gastado por el cliente en ese producto en euros                                                   |
| importe_pro14 | Doble  | Importe neto global gastado por el cliente en ese producto en euros                                                   |
| importe_pro15 | Doble  | Importe neto global gastado por el cliente en ese producto en euros                                                   |
| importe_pro16 | Doble  | Importe neto global gastado por el cliente en ese producto en euros                                                   |
| importe_pro17 | Doble  | Importe neto global gastado por el cliente en ese producto en euros                                                   |
| edad          | Entero | Edad del cliente                                                                                                      |
| tamano_fam    | Entero | Número de miembros de la unidad familiar a la que pertenece el cliente incluyéndolo a él mismo                        |
| anos_exp      | Entero | Años de trabajo del cliente                                                                                           |
| ingresos_ano  | Doble  | Ingresos anuales del cliente en euros                                                                                 |
| des_nivel_edu | Factor | Descripción del nivel de educación del cliente                                                                        |

: (#tab:dpentr) Descripción de las variables del conjunto de datos
**dp_entr**.

Se construye un árbol de clasificación \index{árbol!de clasificación}
utilizando el conjunto de entrenamiento, como se ha comentado, sin
transformar (en su escala original) mediante el algoritmo CART
implementado en el paquete `rpart` con Árboles de Regresión y Partición
Recursiva (Recursive Partitioning and Regression Trees, RPART) que se
puede usar tanto para regresión como para clasificación.


```r
library("CDR")
library("reshape")
library("caret")
library("rpart")
library("rpart.plot")
library("ggplot2")

data("dp_entr")
head(dp_entr)

    ind_pro11 ind_pro12 ind_pro14 ind_pro15 ind_pro16 ind_pro17 importe_pro11
1           S         N         S         S         S         N           157
497         N         N         S         N         S         N             0
265         N         N         S         S         S         S             0
534         N         S         S         N         N         N             0
415         N         S         S         N         S         N             0
298         S         N         S         N         N         N           115
    importe_pro12 importe_pro14 importe_pro15 importe_pro16 importe_pro17 edad
1               0            40           200           180             0   49
497             0           240             0           180             0   38
265             0           425           200           180           300   61
534           120            60             0             0             0   47
415           120           133             0           180             0   34
298             0           220             0             0             0   43
    tamano_fam anos_exp ingresos_ano des_nivel_edu CLS_PRO_pro13
1            4       24        30000         MEDIO             S
497          2       12        53000         MEDIO             N
265          4       37       172000        BASICO             S
534          3       21        38000         MEDIO             N
415          1       10        38000        BASICO             N
298          2       18        60000          ALTO             N

trControl <- trainControl(
  method = "cv",
  number = 10,
  classProbs = TRUE,
  summaryFunction = twoClassSummary
)
```

En primer lugar, se carga la librería necesaria para entrenar el modelo,
así como los datos de compras de los clientes. En este caso se usa el
método de remuestreo de validación cruzada con 10 folds, visto en el
Cap. \@ref(chap-feature). A continuación, se determina la semilla
aleatoria para que los resultados sean replicables, y se entrena el
modelo.


```r

# se fija una semilla aleatoria
set.seed(101)

# se entrena el modelo
model <- train(CLS_PRO_pro13 ~ .,  # . equivale a incluir todas las variables
             data=dp_entr,
             method="rpart",
             metric="ROC",
             trControl=trControl)
```




```r
model

CART

558 samples
 17 predictor
  2 classes: 'S', 'N'

No pre-processing
Resampling: Cross-Validated (10 fold)
Summary of sample sizes: 502, 502, 502, 503, 503, 502, ...
Resampling results across tuning parameters:

  cp          ROC        Sens       Spec
  0.05017921  0.8172123  0.9214286  0.7026455
  0.10394265  0.7559406  0.8386243  0.6914021
  0.51971326  0.6347222  0.8564815  0.4129630

ROC was used to select the optimal model using the largest value.
The final value used for the model was cp = 0.05017921.
```


```r
ggplot(melt(model$resample[,-4]), aes(x = variable, y = value, fill=variable)) +
  geom_boxplot(show.legend=FALSE) +
  xlab(NULL) + ylab(NULL)
```

<div class="figure" style="text-align: center">
<img src="img/notune_rpart_boxplot.png" alt="Resultados del modelo durante la validación cruzada." width="60%" />
<p class="caption">(\#fig:006-002-001RPARTRESULTS)Resultados del modelo durante la validación cruzada.</p>
</div>

Los resultados de validación cruzada quedan recogidos en los boxplot,
por lo que se puede ver los valores entre los que oscilan las
principales medidas en los 10 folds del proceso de validación. Estas
medidas (ROC, sensibilidad y especificidad) se definieron en el Cap.
\@ref(chap-feature), y en el caso de árboles de clasificación se
utilizan para medir la precisión del modelo. A continuación se muestra
el árbol generado. Se puede observar que este árbol es muy sencillo, y
por tanto es fácil obtener su interpretación. En primer lugar decide si
un cliente que compra el *smartchwatch fitness* comprará el nuevo
producto. En caso de no comprar el *smartchwatch fitness* (No a
ind_pro15S=1), pero sí compra la *depiladora eléctrica* (Yes a
ind_pro12S=1) sí comprará el *tensiómetro digital*. Si no compra ninguno
de esos dos productos no comprará el nuevo producto.


```r
# Gráfico del árbol obtenido
rpart.plot(model$finalModel)
```

<div class="figure" style="text-align: center">
<img src="img/notune_rpart_plot.png" alt="Árbol de clasificación sin ajuste automático de hiperparámetros." width="80%" />
<p class="caption">(\#fig:006-002-001RPARTRESULTS2)Árbol de clasificación sin ajuste automático de hiperparámetros.</p>
</div>

Este modelo se puede mejorar ajustando automáticamente
\index{ajuste automático} el hiperparámetro\index{hiperparámetro}
incluido en `rpart` para el entrenamiento de árboles de decisión. Los
hiperparámetros son los valores utilizadas durante el proceso de
entrenamiento en la configuración del modelo. Por consiguiente, primero
es necesario conocer el hiperparámetro a optimizar en el algoritmo
implementado en **R** que estemos usando. Esto se consigue mediante la
siguiente instrucción incluida en el paquete `caret`:


```r
modelLookup("rpart")

  model parameter                label forReg forClass probModel
1 rpart        cp Complexity Parameter   TRUE     TRUE      TRUE
```

El hiperparámetro\index{hiperparámetro} a optimizar es la complejidad
del árbol, `cp`, que es un hiperparámetro que se aplica en la fase de
parada durante la construcción del árbol. Según se ha comentado, esta
fase tiene como función principal evitar desarrollar divisiones que no
valgan la pena. Se puede entender `cp` como la mejora mínima necesaria
en cada nodo del modelo. Es necesario definir los valores de `cp` que se
quieren evaluar con el objetivo de obtener su valor óptimo.


```r
# Se especifica un rango de valores típicos para el hiperparámetro
tuneGrid <- expand.grid(cp = seq(0.01,0.05,0.01))
```


```r
# se entrena el modelo
set.seed(101)

model <- train(CLS_PRO_pro13 ~ .,
             data=dp_entr,
             method="rpart",
             metric="ROC",
             trControl=trControl,
             tuneGrid=tuneGrid)
```




```r
model

CART

558 samples
 17 predictor
  2 classes: 'S', 'N'

No pre-processing
Resampling: Cross-Validated (10 fold)
Summary of sample sizes: 502, 502, 502, 503, 503, 502, ...
Resampling results across tuning parameters:

  cp    ROC        Sens       Spec
  0.01  0.8962254  0.8678571  0.8167989
  0.02  0.8663454  0.9000000  0.7667989
  0.03  0.8458097  0.9392857  0.7310847
  0.04  0.8449381  0.9214286  0.7383598
  0.05  0.8172123  0.9214286  0.7026455

ROC was used to select the optimal model using the largest value.
The final value used for the model was cp = 0.01.
```

De forma automática se construyen diversos árboles para cada uno de los
valores explicitados del parámetro `cp`. Para cada uno de esos árboles
se obtienen las correspondientes métricas de precisión: el área bajo la
curva (denotada como ROC, por las siglas en inglés de *Receiver Operating
Characteristic*), sensibilidad (Sens) y especificidad (Spec), todas ellas
definidas en el Cap. \@ref(chap-feature). El valor ROC es el utilizado
para la elección del valor óptimo de `cp`, por lo que se determina que
finalmente el óptimo es $cp=0,01$ al maximizar el valor ROC alcanzando
un 89,6%. Por tanto, ajustando el hiperparámetro se ha aumentado la
precisión del modelo en casi un 8% respecto al 81,7% que tenía el modelo
sin ajustar automáticamente el valor de `cp`.

En la Fig. \@ref(fig:006-002-003RPARTRESULTS1) se puede ver el
rendimiento de cada una de las métricas del árbol entrenado utilizando
validación cruzada. Dicha figura se obtiene con la siguiente instrucción:


```r
ggplot(melt(model$resample[,-4]), aes(x = variable, y = value, fill=variable)) +
   geom_violin(show.legend=FALSE) +
   xlab(NULL) + 
   ylab(NULL)
```

<div class="figure" style="text-align: center">
<img src="img/tune_rpart_boxplot.png" alt="Resultados del modelo con ajuste automático durante la validación cruzada" width="60%" />
<p class="caption">(\#fig:006-002-003RPARTRESULTS1)Resultados del modelo con ajuste automático durante la validación cruzada</p>
</div>

En la Fig. \@ref(fig:006002003RPARTPLOT2) se muestra el árbol generado.
Dicha visualización se ha obtenido con el siguiente código:


```r
# Gráfico del árbol obtenido
rpart.plot(model$finalModel)
```

<div class="figure" style="text-align: center">
<img src="img/classbigtree.png" alt="Árbol de clasificación con ajuste automático." width="80%" />
<p class="caption">(\#fig:006002003RPARTPLOT2)Árbol de clasificación con ajuste automático.</p>
</div>

Con el objetivo de aumentar la generalidad del árbol y facilitar su
interpretación, se procede a reducir su tamaño podándolo. Para ello se
establece que un nodo terminal tenga como mínimo 50 observaciones, dando
lugar al árbol que se muestra en la Fig. \@ref(fig:PLOTCLASSPRUNEDTREE).


```r
set.seed(101)
prunedtree <- rpart(CLS_PRO_pro13 ~ ., data=dp_entr,
                    cp= 0.01, control = rpart.control(minbucket = 50))

rpart.plot(prunedtree)
```

<div class="figure" style="text-align: center">
<img src="img/pruned_class_tree.png" alt="Árbol de clasificación con ajuste automático y podado." width="100%" />
<p class="caption">(\#fig:PLOTCLASSPRUNEDTREE)Árbol de clasificación con ajuste automático y podado.</p>
</div>

El árbol ha reducido el número de nodos terminales, en tres de ellos el
árbol predice que un cliente comprará el nuevo producto si:

1.  Compra el *smartwatch fitness* (ind_pro15 = S - Yes) y la
    *depiladora eléctrica* (ind_pro12 = S - Yes).

2.  Compra el *smartwatch fitness* (ind_pro15 = S - Yes) y el
    *estimulador muscular* (ind_pro17 = S - Yes), pero no la *depiladora
    eléctrica* (ind_pro12 = S - No).

3.  No compra el *smartwatch fitness* (ind_pro15 = S - No), pero si la
    *depiladora eléctrica* (ind_pro12 = S - Yes).

Sin embargo, dos nodos terminales predicen que el cliente no compra el
nuevo producto si:

1.  Compra el *smartwatch fitness* (ind_pro15 = S - Yes), pero no la
    *depiladora eléctrica* (ind_pro12 = S - No) ni el *estimulador
    muscular* (ind_pro17 = S - No).

2.  No compra el *smartwatch fitness* (ind_pro15 = S - No) ni la
    *depiladora eléctrica* (ind_pro12 = S - No).



## Árboles de regresión \index{árbol!de regresión}

Como se ha comentado, los árboles de decisión también pueden ser usados
para resolver problemas de regresión. En este caso, la idea es que la
predicción dada en cada hoja sea un valor numérico en lugar de un valor
de una categoría. En la Tabla \@ref(tab:dataregtree) se muestran los
datos para un problema de regresión equivalente al presentado en
secciones anteriores para clasificación. Como ya se ha mencionado, la
variable objetivo (*Horas jugadas*) ahora es continua en lugar de
categórica, como ocurría en el ejemplo anterior con la variable
*Decisión*.

| Día | Tipo de día | Humedad | Viento | Horas jugadas |
|-----|:-----------:|:-------:|:------:|:-------------:|
| 1   |   Soleado   | Fuerte  | Débil  |      2,3      |
| 2   |   Soleado   | Fuerte  | Fuerte |      1,5      |
| 3   |   Lluvia    | Fuerte  | Débil  |      1,3      |
| 4   |   Nublado   | Fuerte  | Débil  |      2,4      |
| 5   |   Lluvia    |  Débil  | Débil  |      1,9      |
| 6   |   Lluvia    |  Débil  | Fuerte |      2,4      |
| 7   |   Soleado   | Fuerte  | Débil  |      2,3      |
| 8   |   Nublado   |  Débil  | Fuerte |      2,2      |
| 9   |   Soleado   |  Débil  | Débil  |      1,3      |
| 10  |   Lluvia    |  Débil  | Débil  |      1,8      |
| 11  |   Soleado   |  Débil  | Fuerte |      1,2      |
| 12  |   Nublado   | Fuerte  | Fuerte |      2,9      |
| 13  |   Nublado   |  Débil  | Débil  |      2,2      |
| 14  |   Lluvia    | Fuerte  | Fuerte |      1,5      |
| 15  |   Soleado   | Fuerte  | Fuerte |      1,5      |

: (#tab:dataregtree) Datos de Horas jugadas dada la climatología del día

Se pueden calcular medidas descriptivas de la variable respuesta, *Horas
jugadas*, como la media, varianza, desviación típica y coeficiente de
variación siendo estas:

```{=tex}
\begin{equation}
\bar{x}_{\text{Horas jugadas}} = \frac{1}{n}\sum{x} = 1,91
(\#eq:mean-horas)
\end{equation}
```
```{=tex}
\begin{equation}
\sigma^{2}_{\text{Horas jugadas}} = \frac{\sum{(x-\bar{x}\Bigr)^{2}}}{n} = 0,25
(\#eq:varhoras)
\end{equation}
```
```{=tex}
\begin{equation}
\sigma_{\text{Horas jugadas}} = \sqrt{\sigma^{2}} = 0,50
(\#eq:sdhoras)
\end{equation}
```
```{=tex}
\begin{equation}
CV_{\text{Horas jugadas}} = \frac{\sigma}{\bar{x}} = 0,26
(\#eq:cvhoras)
\end{equation}
```
### ¿Cómo se va formando el árbol de regresión? \index{partición}

Mientras que en los árboles de clasificación se utilizaba la entropía o
la impureza de Gini para medir la homogeneidad de un nodo, en los
árboles de regresión se utiliza como métrica la desviación típica
$(\sigma)$. Por tanto, cuando se selecciona una variable para hacer la
división, se calcula la desviación típica para cada una de las ramas, y
se obtiene una media ponderada en función del número de elementos de
cada una de ellas. Esta media ponderada se calcula del siguiente
modo:

```{=tex}
\begin{equation}
\sigma_{X} = \sum_{r\in X}{P(r)\cdot\sigma_{r}}
(\#eq:sigmavar)
\end{equation}
```
Donde X es la variable de la cual se quiere obtener la desviación típica
y $r$ son las ramas que crecen desde este nodo. Para llevar a cabo la
división, en primer lugar se debe calcular para cada nodo su desviación
típica. A continuación, se seleccionan posibles variables para hacer la
división y se obtiene su desviación típica. Para cada una de estas
variables se calcula el decremento de la desviación, y se selecciona
aquel que introduzca la mayor reducción.

Por ejemplo, para los datos mostrados en la Tabla
\@ref(tab:dataregtree), la desviación típica es 0,50 Horas jugadas, como
se calculó en la ecuación \@ref(eq:sdhoras), y el árbol se construiría
como se muestra a continuación. En primer lugar, se selecciona *Tipo de
día*, *Humedad* y *Viento* como candidatos a nodo raíz y se obtiene su
desviación típica tal que:

| Tipo de día | \# observaciones | $\sigma_{\text{Horas jugadas}}$ |
|-------------|:----------------:|:-------------------------------:|
| Soleado     |        6         |              0,45               |
| Nublado     |        4         |              0,29               |
| Lluvia      |        5         |              0,38               |

: (#tab:sd-tipodia) Desviación típica en las ramas de la variable *Tipo
de día*

| Humedad | \# observaciones | $\sigma_{\text{Horas jugadas}}$ |
|---------|:----------------:|:-------------------------------:|
| Fuerte  |        8         |              0,55               |
| Débil   |        7         |              0,43               |

: (#tab:sd-Humedad) Desviación típica en las ramas de la variable
*Humedad*

| Viento | \# observaciones | $\sigma_{\text{Horas jugadas}}$ |
|--------|:----------------:|:-------------------------------:|
| Fuerte |        7         |              0,57               |
| Débil  |        8         |              0,42               |

: (#tab:sd-Viento) Desviación típica en las ramas de la variable
*Viento*

A partir de las desviaciones típicas en las ramas de cada variable, se
puede obtener la desviación típica de cada variable de acuerdo a la
ecuación \@ref(eq:sigmavar). Además, se calcula la reducción de
desviación típica como la diferencia entre la desviación de la variable
respuesta y la desviación si se divide el conjunto de datos en base a
alguna de las variables. Tanto la desviación típica de cada variable
como el decremento en la desviación que producen se muestran en la Tabla
\@ref(tab:sdvars).

| Variable    | $\sigma_{\text{Horas jugadas}}$ | Decremento |
|-------------|:-------------------------------:|:----------:|
| Tipo de día |              0,38               |    0,12    |
| Humedad     |              0,49               |    0,00    |
| Viento      |              0,49               |    0,00    |

: (#tab:sdvars) Desviación típica y decremento de desviación de cada
variable

Dado que la variable *Tipo de día* es la que produce una mayor reducción
en la desviación típica, resulta elegida como nodo raíz. El árbol
seguiría creciendo repitiendo este proceso. Por ejemplo, se muestra cuál
sería la siguiente división desde la rama soleado que crece desde nodo
*Tipo de día*

| Humedad | \# observaciones | $\sigma_{\text{Horas jugadas}}$ |
|---------|:----------------:|:-------------------------------:|
| Fuerte  |        4         |               0,4               |
| Débil   |        2         |              0,05               |

: (#tab:sd-sol-Humedad) Desviación típica en las ramas de la variable
*Humedad* en días soleados

| Viento | \# observaciones | $\sigma_{\text{Horas jugadas}}$ |
|--------|:----------------:|:-------------------------------:|
| Fuerte |        3         |              0,14               |
| Débil  |        3         |              0,47               |

: (#tab:sd-sol-Viento) Desviación típica en las ramas de la variable
*Viento*

En la Tabla \@ref(tab:sdvarssol) se muestra la desviación típica para
cada variable así como la reducción de desviación que produce. Por
tanto, la siguiente división se realizaría con la variable *Humedad*.

| Variable | $\sigma_{\text{Horas jugadas}}$ | Decremento |
|----------|:-------------------------------:|:----------:|
| Humedad  |              0,28               |    0,17    |
| Viento   |              0,31               |    0,14    |

: (#tab:sdvarssol) Desviación típica y decremento de desviación de cada
variable en la rama soleado

### ¿Cuánto debe crecer el árbol de regresión? \index{profundidad!del árbol}

Como ocurría en los árboles de clasificación, es necesario establecer
reglas que pongan fin al proceso de crecimiento del árbol. Además de los
criterios de parada que se utilizan en árboles de clasificación (número
de elementos mínimos en un nodo y nivel máximo del árbol), en árboles de
regresión se detiene su crecimiento estableciendo un *threshold* (umbral
de decisión) sobre el coeficiente de variación del nodo. En el ejemplo
expuesto sobre Horas jugadas, se puede ver qué nodos podrían seguir
creciendo si se establece que el árbol continúe creciendo en nodos con
un coeficiente de variación de un 15% o más, y que tenga al menos 5
observaciones en el nodo.

| Nodo padre  |  Rama   | CV en nodo hijo | \# observaciones |
|-------------|:-------:|:---------------:|:----------------:|
| Tipo de día | Nublado |     11,80%      |        4         |
| Tipo de día | Lluvia  |     21,14%      |        5         |
| Humedad     | Fuerte  |     21,04%      |        4         |
| Humedad     |  Débil  |      4,04%      |        2         |

: (#tab:cv-nodos) Medidas para decidir si el árbol sigue creciendo

En este ejemplo, el árbol seguiría creciendo por la rama *Lluvia* donde
habría que seleccionar la siguiente variable de división. En el resto de
ramas, no se supera el número mínimo establecido de observaciones en el
nodo, y en ocasiones tampoco se alcanza el coeficiente de variación
mínimo. Por otra parte, en los árboles de regresión la poda se lleva a
cabo del mismo modo que para árboles de clasificación. En la ecuación
\@ref(eq:poda) se mediría el error de entrenamiento a través de la suma
de los cuadrados de los errores (en inglés *Sum of Squared Estimate of
Errors*, SSE), es decir:

```{=tex}
\begin{equation}
SSE_{\zeta}(\tau) = SSE(\tau) + \zeta|\tau|
(\#eq:regpoda)
\end{equation}
```
### Árbol de regresión para estimar el número de días de hospitalización

En este ejemplo se utilizan los datos `cleveland`, contenidos en el
paquete `CDR`, y que han sido utilizados en el Cap. \@ref(cap-glm) para
estimar la variable *dhosp*. El conjunto de datos contiene información
sobre pacientes que llegan a un hospital con dolor de pecho y de los
cuales se han recogido distintas características. Se pretende predecir
el número de días de hospitalización que necesitará un paciente en base
al resto de características observadas:\index{árbol!de regresión} si
el paciente está diagnosticado de accidente coronario, su edad, su sexo,
el tipo de dolor que padece y la depresión en el segmento ST inducida
por ejercicio en relación al reposo.


```r
# se cargan los datos
data("cleveland")

# se entrena el modelo
set.seed(101)
model <- rpart(dhosp ~ diag + edad + sexo + tdolor + dep,
               data=cleveland, method="anova")
```




```r
model$cptable

          CP nsplit rel error    xerror       xstd
1 0.37275022      0 1.0000000 1.0128283 0.09213359
2 0.01674747      1 0.6272498 0.6427926 0.06048143
3 0.01132433      4 0.5770074 0.6788431 0.06681871
4 0.01007684      6 0.5543587 0.6825792 0.06505426
5 0.01000000      7 0.5442819 0.6843192 0.06514439
```

Se observa que para valores muy altos del hiperparámetro de complejidad,
el SSE es muy elevado. Esto es, produce modelos muy sencillos pero con
nula potencia predictiva. En el otro extremo, para $\zeta=0,01$ el SSE
se minimiza hasta llegar a $SSE=0,54$, por lo que el árbol se poda de
acuerdo a la ecuación \@ref(eq:regpoda) con dicho valor de $\zeta$. El
resultado del modelo se muestra en el árbol de la Fig.
\@ref(fig:dhosp-plot). La interpretación de este árbol sería:

1.  Si el paciente no tiene diagnóstico de accidente coronario, solo
    necesitará un día de hospitalización.

2.  En el caso de tener este diagnóstico, y una depresión mayor o igual
    a dos en el segmento ST inducida por ejercicio en relación al
    reposo, necesitará 2,8 días de hospitalización.

3.  En un último ejemplo, si la depresión en el segmento ST inducida por
    ejercicio en relación al reposo está entre 0,35 y 2 entonces el
    paciente necesitará 3,8 días de hospitalización. Si por el
    contrario, la depresión en el segmento ST inducida por ejercicio en
    relación al reposo es menor a 0,35, el número de días de
    hospitalización depende del sexo del paciente: los hombres
    necesitarán 3,2 días y las mujeres tan solo 1,9 días.


```r
# se pinta el árbol obtenido
rpart.plot(model)
```

<div class="figure" style="text-align: center">
<img src="img/regtree_dhosp.png" alt="Árbol de regresión para predecir el número de días de hospitalización." width="80%" />
<p class="caption">(\#fig:dhosp-plot)Árbol de regresión para predecir el número de días de hospitalización.</p>
</div>

### Árbol de regresión para la predicción del precio unitario de la vivienda en Madrid

En este ejemplo se va a entrenar un árbol de regresión para predecir el
precio unitario de la vivienda en Madrid. Para ello, se van a utilizar
los datos de viviendas a la venta en Madrid publicadas en Idealista
durante el año 2018. Estos datos están incluidos en el paquete
`idealista18`. Para facilitar la interpretación del modelo, sólo se van
a utilizar 8 de las variables incluidas en el conjunto de datos:
superficie construida, número de dormitorios, número de baños, si tiene
terraza, si tiene ascensor, si el precio incluye el parking, distancia
al centro de Madrid y distancia a una parada de metro.


```r
library("idealista18")
data("Madrid_Sale")

Madrid_Sale <- Madrid_Sale |>
  dplyr::select(UNITPRICE, CONSTRUCTEDAREA, ROOMNUMBER, BATHNUMBER,
         HASTERRACE,HASLIFT, ISPARKINGSPACEINCLUDEDINPRICE,
         DISTANCE_TO_CITY_CENTER, DISTANCE_TO_METRO)

head(Madrid_Sale)

  UNITPRICE CONSTRUCTEDAREA ROOMNUMBER BATHNUMBER HASTERRACE HASLIFT
1  2680.851              47          1          1          0       1
2  4351.852              54          1          1          0       0
3  4973.333              75          2          1          0       0
4  5916.667              48          1          1          0       1
5  4560.000              50          0          1          0       0
6  3921.260             127          3          2          0       1
  ISPARKINGSPACEINCLUDEDINPRICE DISTANCE_TO_CITY_CENTER DISTANCE_TO_METRO
1                             0               8.0584293         0.8720746
2                             0               0.8763693         0.1163821
3                             0               0.9074793         0.1391088
4                             0               0.8454622         0.1442990
5                             0               1.2502313         0.3370982
6                             0               0.5417727         0.1614363
```


```r
# Se entrena el modelo
library("rpart")
set.seed(101)
model <- rpart(UNITPRICE ~ ., Madrid_Sale, method = "anova")
```





Como en el ejemplo anterior, para $\zeta=0,01$ el SSE se minimiza hasta
llegar a $SSE=0,56$, por lo que el árbol se poda de acuerdo a la
ecuación \@ref(eq:regpoda) con dicho valor de $\zeta$. El resultado del
modelo se muestra en el árbol de la Fig. \@ref(fig:idealista-treeplot).
La interpretación de este árbol sería:

1.  Si una vivienda con ascensor se encuentra a menos de 3,2km del
    centro de Madrid y a menos de 0,46km de una estación de Metro, el
    precio unitario predicho para esa vivienda será de 5.248€/$m^{2}$.

2.  Si una vivienda se encuentra a más de 3,2km del centro de Madrid y
    no tiene ascensor, el precio unitario predicho será de
    2.160€/$m^{2}$.

3.  Si una vivienda se encuentra a menos de 3,2km del centro de Madrid y
    a más de 0,46km de una estación de Metro, el precio unitario
    predicho para esa vivienda será de 3.873€/$m^{2}$.


```r
# se pinta el árbol obtenido
rpart.plot(model)
```

<div class="figure" style="text-align: center">
<img src="img/regtree_idealista.png" alt="Árbol de regresión para predecir el precio unitario de las viviendas en Madrid." width="80%" />
<p class="caption">(\#fig:idealista-treeplot)Árbol de regresión para predecir el precio unitario de las viviendas en Madrid.</p>
</div>

::: {.infobox_resume data-latex=""}
### Resumen {.unnumbered}

En este capítulo se introduce al lector en los árboles de decisión para
clasificación y regresión, en particular:

-   Se presenta la lógica para la construcción de árboles de decisión,
    ya sean de regresión o clasificación.
-   Se contemplan diferentes medidas con las que el árbol decide avanzar
    hacia un nuevo punto de decisión.
-   Se presentan los conceptos de sobreajuste y complejidad del árbol,
    así como la forma de controlarlos.
-   Se muestra el uso de **R** para la clasificación de clases binarias y
    para la predicción de variables respuesta numéricas en casos
    aplicados.
:::
