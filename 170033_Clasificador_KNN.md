# Clasificador k-vecinos más próximos {#cap-knn}

*Ramón A. Carrasco*$^{a}$ e *Itzcóatl Bueno*$^{b,a}$

$^{a}$Universidad Complutense de Madrid 
$^{b}$Instituto Nacional de Estadística

## Introducción

El k-vecinos más próximos (KNN, por sus siglas en inglés *k-Nearest Neighbors*) es un algoritmo de aprendizaje no paramétrico. Un algoritmo no paramétrico no presupone la forma concreta del modelo a entrenar, siendo más flexible. Sin embargo, esto se consigue a costa de necesitar más datos de entrenamiento y siendo más lentos que los algoritmos paramétricos. Al contrario que otros algoritmos de aprendizaje que permiten deshacerse de los datos de entrenamiento una vez se entrena el modelo, el modelo KNN guarda las observaciones de entrenamiento en memoria. Esto es, al incorporar una nueva observación *$x$*, el algoritmo KNN encuentra las $k$ observaciones del conjunto de datos de entrenamiento más similares a la nueva y proporciona la clase mayoritaria (en el caso de clasificación) o el valor medio (en el caso de regresión).

El número de casos ($k$)\index{k-vecinos} a utilizar para clasificar las nuevas observaciones es un parámetro crucial para este algoritmo [@james2013introduction]. Si, por ejemplo, $k=3$, el modelo KNN utilizará las tres observaciones más similares (vecinos) al nuevo caso para clasificarlo. Es recomendable probar distintos valores de $k$ para conseguir el mejor ajuste del modelo, y por tanto, es conveniente evitar valores extremos de $k$. Si se establece un valor muy bajo de $k$ aumentará el sesgo y llevará a clasificaciones erróneas. Mientras que valores muy elevados de $k$ harán que el algoritmo sea computacionalmente costoso y además tampoco será un buen clasificador. Además, también se recomienda establecer valores impares de $k$ para evitar puntos muertos estadísticos (empate entre categorías) y un resultado no válido. 

<div class="figure" style="text-align: center">
<img src="img/knn.png" alt="Ejemplo de k-vecinos más próximos." width="60%" />
<p class="caption">(\#fig:knn-ejemplo)Ejemplo de k-vecinos más próximos.</p>
</div>

La escala\index{escala} de las variables puede impactar en el resultado del modelo KNN. Por ello, el conjunto de datos debe escalarse para que aquellas variables con unidades de medida grandes no tengan más importancia en el cálculo que otras con magnitudes menores. Así se reduce la importancia de las variables debido a sus unidades de medida y se estandariza la varianza.

Pese a que el modelo KNN es fácil de entender y generalmente preciso, almacenar el conjunto de datos de entrenamiento, así como calcular la distancia entre cada nueva observación a clasificar y las observaciones del conjunto de datos, supone la necesidad de recursos computacionales altos. Esto implica que cuanto mayor es la cantidad de observaciones en el conjunto de datos, mayor es el tiempo para la ejecución de una sola predicción, y, por tanto, esto puede dar lugar a tiempos de procesamiento lentos. Por este motivo, no se recomienda el uso del algoritmo KNN cuando se dispone de conjuntos de datos muy grandes. Otra desventaja a tener en cuenta es la dificultad de aplicar KNN a conjuntos de datos con un gran número de variables, puesto que calcular las distancias entre observaciones con múltiples dimensiones también incrementará la necesidad de recursos computacionales y podría dificultar que se clasifique de forma precisa.

## Decisiones a tener en cuenta

La elección de la función de distancia, así como el número de vecinos $k$ son decisiones que debe tomar el investigador antes de ejecutar el algoritmo. Siendo este último el hiperparámetro\index{hiperparámetro} del modelo que la función `modelLookup()` de `caret` nos devuelve considerando que es primordial ajustarlo: 


```r
modelLookup("knn")
  model parameter      label forReg forClass probModel
1   knn         k #Neighbors   TRUE     TRUE      TRUE
```

### Función de distancia a utilizar

El modelo KNN determina la cercanía entre dos observaciones a través de una función de distancia\index{distancia}. Generalmente, se utilizan la distancia\index{distancia} euclídea\index{distancia!euclídea}, mostrada en la ecuación \@ref(eq:dist-eu), y la distancia de Manhattan\index{distancia!manhattan}, mostrada en la ecuación \@ref(eq:dist-man). Otras funciones de distancia, como las presentadas en el Cap. \@ref(cap-cluster), también pueden ser utilizadas para el entrenamiento de este algoritmo.

\begin{equation}
d(x_i,x_k)=\sqrt{\sum_{j=1}^{p}{(x_i^{(j)}-x_k^{(j)})^2}}
(\#eq:dist-eu)
\end{equation}

\begin{equation}
d(x_i,x_k)=\sum_{j=1}^{p}{|x_i^{(j)}-x_k^{(j)}|}
(\#eq:dist-man)
\end{equation}

En el caso de querer incluir tanto variables cuantitativas como variables cualitativas en el cálculo de la distancia, el coeficiente de disimilitud de Gower\index{distancia!de Gower} es la función de distancia más popular para esta situación. El coeficiente de disimilitud de Gower se define como:

\begin{equation}
d(i,j) = \frac{\sum^{p}_{k=1}{\omega_k \delta^{(k)}_{ij}d^(k)_{ij}}}{\omega_k \delta^{(k)}_{ij}}
\end{equation}

El coeficiente de Gower es una media ponderada de las distancias $d^{(k)}_{ij}$ con ponderaciones $\omega_{k}\delta_{ij}^{(k)}$.

### Número de vecinos (k) seleccionados

Como se ha reiterado, la elección de cuántos vecinos ($k$)\index{k-vecinos} intervienen en el ajuste del algoritmo es determinante para su rendimiento. Si se escogen demasiado pocos vecinos, se producirá sobreajuste en el modelo. En el extremo en el que sólo se utilizará un vecino ($k=1$), la predicción se basará en la observación con la menor distancia al elemento a clasificar. Por otro lado, un número alto de vecinos ($k$) hace que el modelo no ajuste bien al tener en cuenta un vecindario más grande. En este sentido, en el caso extremo de elegir todas las observaciones como vecinos más próximos ($k=n$), se obtendrá el valor medio (en el caso de la regresión) o la clase mayoritaria (en el caso de la clasificación) como valor predicho para todas las observaciones del conjunto de entrenamiento.

No existe una regla general para la elección óptima de $k$, puesto que en gran medida dependerá del conjunto de datos utilizado. Cuando el conjunto de datos tiene pocas variables que no aporten información, valores pequeños de $k$ tienden a funcionar mejor. Cuantas más variables sin importancia se incluyen en el conjunto de datos, mayor deberá ser el valor de $k$ para suavizar su efecto.

## Procedimiento con **R**: la función `knn()`

En el paquete `class` de R se encuentra la función `knn()` que se utiliza para entrenar el modelo k-vecinos más próximos:


```r
knn(train, test, cl, k = 1, ...)
```

+ `train`: conjunto de datos con las observaciones de entrenamiento.
+ `test`: conjunto de datos con las observaciones de validación. Un vector se interpreta como una única observación a validar.
+ `cl`: clases de las observaciones de entrenamiento.
+ `k`: número de vecinos a considerar

## Aplicación del modelo KNN en R

En este ejemplo se entrena un modelo KNN para clasificar qué clientes comprarán el *tensiómetro digital* teniendo en cuenta sus características y el resto de sus compras. Este conjunto de datos está incluido en el paquete `CDR` con el nombre `dp_entr_NUM`. En este conjunto de datos todas las variables son cuantitativas (excepto la clase objetivo) pero dichas variables tienen distintas escalas de medida (euros, años, unidades, etc.) por lo que es necesario indicar en la función `trainControl()` que se haga un preprocesamiento para estandarizar las variables. Además, se define como método de remuestreo la validación cruzada, como la presentada en el Cap. \@ref(chap-feature), con 10 folds.



```r
library("CDR")
library("class")
library("caret")
library("reshape")
library("ggplot2")

data(dp_entr_NUM)

head(dp_entr_NUM)

  ind_pro11 ind_pro12 ind_pro14 ind_pro15 ind_pro16 ind_pro17 des_nivel_edu.ALTO
1         1         0         1         1         1         0                  0
2         0         0         1         0         1         0                  0
3         0         0         1         1         1         1                  0
4         0         1         1         0         0         0                  0
5         0         1         1         0         1         0                  0
6         1         0         1         0         0         0                  1
  des_nivel_edu.BASICO des_nivel_edu.MEDIO importe_pro11 importe_pro12 importe_pro14
1                    0                   1           157             0            40
2                    0                   1             0             0           240
3                    1                   0             0             0           425
4                    0                   1             0           120            60
5                    1                   0             0           120           133
6                    0                   0           115             0           220
  importe_pro15 importe_pro16 importe_pro17 edad tamano_fam anos_exp ingresos_ano CLS_PRO_pro13
1           200           180             0   49          4       24        30000             S
2             0           180             0   38          2       12        53000             N
3           200           180           300   61          4       37       172000             S
4             0             0             0   47          3       21        38000             N
5             0           180             0   34          1       10        38000             N
6             0             0             0   43          2       18        60000             N


# Definimos un método de remuestreo
cv <- trainControl(
  method = "repeatedcv",
  number = 10,
  repeats = 5,
  classProbs = TRUE,
  preProcOptions = list("center"),
  summaryFunction = twoClassSummary
)
```

Antes de obtener el modelo definitivo, es necesario la selección del número óptimo de vecinos $k$ entrenando distintos modelos variando dicho hiperparámetro. Para facilitar este trabajo arduo, en el paquete `caret` de **R** se puede definir una red de posibles valores sobre los que evaluar el modelo KNN y que de forma automática se determine el valor que mejor rendimiento proporcione. A continuación se definen los posibles valores de $k$ que se quieren evaluar.


```r
# Definimos la red de posibles valores del hiperparámetro
hyper_grid <- expand.grid(k = c(1:10,15,20,30,50,75,100))
```

Una vez se que se ha definido tanto el método de remuestreo como la red de posibles valores del hiperparámetro se puede entrenar el modelo:


```r

set.seed(101)
# Se entrena el modelo ajustando el hiperparámetro óptimo
model <- train(
  CLS_PRO_pro13 ~ .,
  data = dp_entr_NUM,
  method = "knn",
  trControl = cv,
  tuneGrid = hyper_grid,
  metric = "ROC"
)

model

k-Nearest Neighbors

558 samples
 17 predictor
  2 classes: 'S', 'N'

No pre-processing
Resampling: Cross-Validated (10 fold, repeated 5 times)
Summary of sample sizes: 502, 502, 502, 503, 503, 502, ...
Resampling results across tuning parameters:

  k    ROC        Sens       Spec
    1  0.6584524  0.6466402  0.6702646
    2  0.6769109  0.6179101  0.6131217
    3  0.6828893  0.6216402  0.6496561
    4  0.6851087  0.6404233  0.6394709
    5  0.6951129  0.6540212  0.6666138
    6  0.6914664  0.6216402  0.6543386
    7  0.6982592  0.6252381  0.6953439
    8  0.6974556  0.6281481  0.6960053
    9  0.6992229  0.6159524  0.7117725
   10  0.6994133  0.6037037  0.7052910
   15  0.6875879  0.5749206  0.7232011
   20  0.6731477  0.5722751  0.7010582
   30  0.6752986  0.5529630  0.7024603
   50  0.6890259  0.5163757  0.7605556
   75  0.6852886  0.5092593  0.7670106
  100  0.6719378  0.5049471  0.7820106

ROC was used to select the optimal model using the largest value.
The final value used for the model was k = 10.
```

En la Fig. \@ref(fig:006-002-102KNNKCHOOSING3) se observa que el número óptimo de vecinos es $k=10$, donde se alcanza el rendimiento óptimo del modelo.


```r

ggplot(model) + geom_vline(xintercept = unlist(model$bestTune),col="red",linetype="dashed") + theme_light()

```

<div class="figure" style="text-align: center">
<img src="img/knn-tune-k.png" alt="Número óptimo de vecinos (k)." width="60%" />
<p class="caption">(\#fig:006-002-102KNNKCHOOSING3)Número óptimo de vecinos (k).</p>
</div>

El boxplot de los resultados obtenidos durante el proceso de validación cruzada muestra que el AUC del modelo oscila entre un 60% y un 85% aproximadamente.


```r
ggplot(melt(model$resample[,-4]), aes(x = variable, y = value, fill=variable)) +
   geom_boxplot(show.legend=FALSE) +
  xlab(NULL) + ylab(NULL)
```

<div class="figure" style="text-align: center">
<img src="img/knn-boxplot.png" alt="Resultados obtenidos durante el proceso de validación cruzada." width="60%" />
<p class="caption">(\#fig:006-002-102KNNRESULTS3)Resultados obtenidos durante el proceso de validación cruzada.</p>
</div>

El modelo se resiente en su rendimiento al tener dificultades en predecir correctamente la clase positiva, más concretamente se puede observar en la Fig. \@ref(fig:006-002-102KNNRESULTS3) que la sensibilidad oscila entre el 40% y el 75%; resultados ligeramente peores que los que obtiene al predecir observaciones de la clase negativa, los cuales oscilan entre el 50% y el 85%.

::: {.infobox_resume data-latex=""}
### Resumen {-}
En este capítulo se introduce al lector en el algoritmo de aprendizaje supervisado conocido como k-vecinos más próximos, destacando:

- Las decisiones a tener en cuenta antes de entrenar el modelo de k-vecinos más próximos.
- Se exponen algunas de las distancias más utilizadas para el entrenamiento de este modelo.
- Se especifican las ventajas y desventajas del número de vecinos a tener en cuenta.
:::
