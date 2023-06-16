# Boosting y el algoritmo XGBoost {#cap-boosting-xgboost}

*Ramón A. Carrasco*$^{a}$ e *Itzcóatl Bueno*$^{b,a}$

$^{a}$Universidad Complutense de Madrid 
$^{b}$Instituto Nacional de Estadística

## Métodos ensamblados: bagging vs boosting

En el Cap. \@ref(cap-bagg-rf) se presentó la idea de métodos ensamblados. Una serie de modelos útiles cuando ningún modelo de aprendizaje supervisado es capaz de explicar bien la variable dependiente de interés. En el aprendizaje ensamblado se entrenan un gran número de modelos con menor precisión, y se combinan sus predicciones para obtener un metamodelo de una precisión más alta. Los dos modelos de aprendizaje ensamblado más populares son el *bagging* (Cap. \@ref(cap-bagg-rf)) y el *boosting*. 

La principal diferencia entre ellos radica en cómo se combinan los modelos individuales para obtener una predicción final. Por ejemplo, si se quisiera organizar una fiesta y se tuviese que tomar una decisión sobre el tema para la decoración, se podría hacer tanto con *bagging* como con *boosting*. Si se utilizase el *bagging*, se pediría opinión a distintos grupos de amigos. Después, se combinarán todas sus ideas para tomar una decisión final sobre la decoración de la fiesta. Por otro lado, si se utiliza *boosting*, en lugar de preguntarle a diferentes grupos de amigos al mismo tiempo, se pediría a un amigo en particular su opinión. Si su respuesta no es convincente, entonces se busca la ayuda de otro amigo, quien se enfocará en mejorar la respuesta anterior. Este proceso continúa secuencialmente, solicitando la ayuda de diferentes amigos y construyendo sobre las respuestas anteriores para obtener una decisión final más precisa y refinada sobre la decoración de la fiesta.

## ¿Qué es el boosting?

El *boosting*\index{boosting} [@schapire2012] es el otro de los paradigmas de aprendizaje ensamblado,\index{aprendizaje ensamblado} presentado en el Cap. \@ref(cap-bagg-rf). Como el *bagging*, el *boosting* agrega múltiples modelos con menor precisión (débiles) combinando sus predicciones para obtener un metamodelo con una precisión más alta. Los árboles de decisión\index{árbol!de decisión} son los modelos base o débiles que se usan más frecuentemente. En este caso, para llegar al metamodelo a partir de los modelos base, es necesario introducir ponderaciones a los árboles basándose en las clasificaciones erróneas del árbol entrenado previamente a dicho árbol.

El *boosting*\index{boosting} reduce el problema del sobreajuste\index{sobreajuste} utilizando menos árboles que un modelo *random forest*\index{random forest}. Mientras que agregar más árboles al *random forest* ayuda a compensar el sobreajuste, también puede llevar a un aumento del mismo y, por ello, hay que ser cauteloso a la hora de agregar nuevos árboles. Sin embargo, el *boosting*\index{boosting} aprende iterativamente de los errores en árboles anteriores, pudiendo llevar a sobreajustar\index{sobreajuste} el modelo. Aunque este enfoque produce predicciones más precisas, muchas veces mejores a la mayoría de algoritmos, puede llevar a ajustar las observaciones atípicas. Es por esto que el *random forest*\index{random forest} es una técnica más recomendada cuando se trabaja con conjuntos de datos muy complejos con un gran número de observaciones atípicas.

Otra de las grandes desventajas del *boosting* es que su tiempo de procesamiento es muy elevado, puesto que su entrenamiento sigue una lógica secuencial. En el proceso de entrenamiento, un árbol debe esperar a que el inmediatamente anterior sea entrenado, para iniciar su entrenamiento, y esto limita la escalabilidad del modelo. Mientras tanto, un *random forest* entrena los árboles en paralelo, lo que hace que su tiempo de procesamiento sea más rápido.

Tanto los algoritmos de *boosting*\index{boosting} como a los de *bagging*\index{bagging} presentan el inconveniente de la dificultad de interpretación que tienen respecto a los árboles de decisión\index{árbol!de decisión}. En este aspecto estos algoritmos de ensamblado se pueden considerar como de caja negra.

## Gradient Boosting (GB)

Uno de los algoritmos de *boosting* más conocidos es el **gradient boosting** \index{gradient boosting}. Mientras que el *random forest*\index{random forest} seleccionaba combinaciones aleatorias de variables en cada proceso de construcción de un árbol, el *gradient boosting* selecciona variables que mejoren la precisión con cada nuevo árbol. Por lo tanto, la construcción del modelo es secuencial, puesto que cada nuevo árbol se construye utilizando información derivada del árbol anterior y, en consecuencia, la construcción de estos árboles no son independientes. En cada iteración se registran los errores cometidos en los datos de entrenamiento y se tienen en cuenta para la siguiente ronda de entrenamiento. Además, se incorporan ponderaciones, como se observa en la Fig. \@ref(fig:boosting) a los datos basándose en los resultados de la iteración anterior. Las ponderaciones más altas se aplicarán a las observaciones que fueron erróneamente clasificadas, y no se dará tanta atención a las bien clasificadas. Este proceso se repite hasta que se llega a un nivel bajo de error. El resultado final se obtiene a través de la media ponderada de las predicciones de los árboles de decisión.\index{árbol!de decisión}


<div class="figure" style="text-align: center">
<img src="img/boosting.png" alt="Ejemplo de Boosting." width="70%" />
<p class="caption">(\#fig:boosting)Ejemplo de Boosting.</p>
</div>

Matemáticamente, un algoritmo *gradient boosting*\index{gradient boosting} para clasificación sigue los pasos que a continuación se detallan. Sea un problema de clasificación binaria y, asumiendo que se tienen $K$ árboles de decisión de clasificación, la predicción del modelo ensamblado se obtiene utilizando la función sigmoidal, como en la regresión logística (Cap. \@ref(cap-glm)), tal que:

```{=tex}
\begin{equation}
P(y=1|x,f)=\frac{1}{1+e^{-f(x)}}
\end{equation}
```
Donde $f(x)=\sum_{\kappa=1}^{K}{f_{\kappa}(x)}$ y $f_m$ es un árbol de decisión. De nuevo, como en la regresión logística, se aplica el principio de máxima verosimilitud tratando de hallar una $f$ que maximice $\mathcal{L}_f = \sum_{i=1}^{N}{\ln(P(y_i=1|x_i,f))}$

El algoritmo, en origen, es un modelo constante de la forma $f=f_0=\frac{p}{1-p}$ donde $p=\frac{1}{N}\sum^{N}_{i=1}$. Tras cada iteración se añade un nuevo árbol $f_\kappa$ al modelo. Para encontrar el mejor árbol $f_\kappa$, la primera derivada parcial $g_i$ del modelo actual se obtiene para $i=1,\dots,N$:

```{=tex}
\begin{equation}
g_i = \frac{\delta\mathcal{L}_f}{\delta f}
\end{equation}
```
Donde $f$ es el modelo de clasificación ensamblado construido en la iteración previa. Se necesita obtener las derivadas de $\ln(P(y_i=1|x_i,f))$ con respecto a $f$ para todo $i$ para poder calcular $g_i$. Nótese que:

```{=tex}
\begin{equation}
\ln(P(y_i=1|x_i,f))=\ln(\frac{1}{1+e^{-f(x_i)}})
\end{equation}
```
Y, por tanto, la derivada respecto a $f$ es igual a:

```{=tex}
\begin{equation}
\frac{\delta \ln(P(y_i=1|x_i,f))}{\delta f} = \frac{1}{e^{f(x_i)}+1}
\end{equation}
```
Después, se reemplaza en el conjunto de entrenamiento la categoría original $y_i$ por su correspondiente derivada parcial $g_i$ y se construye un nuevo modelo $f_\kappa$ utilizando el conjunto de entrenamiento transformado. Tras esto, se obtiene la actualización óptima ($\rho_\kappa$) como:

```{=tex}
\begin{equation}
\rho_\kappa = \arg \max\limits_{\rho}{\mathcal{L}_{f+\rho f_\kappa}}
\end{equation}
```
Al terminar la iteración $\kappa$, se actualiza el modelo ensamblado $f$ añadiendo el nuevo árbol $f_\kappa$:

```{=tex}
\begin{equation}
f\leftarrow f+\alpha\rho_\kappa f_\kappa
\end{equation}
```
Se itera hasta que $\kappa=K$, entonces el proceso se detiene y se obtiene el modelo ensamblado final $f$.

### Hiperparámetros del modelo *gradient boosting*

Un modelo de *gradient boosting* tiene dos tipos de hiperparámetros\index{hiperparámetro}:

-   Hiperparámetros de *boosting*.
-   Hiperparámetros del árbol.

#### Hiperparámetros de *boosting*

Los hiperparámetros de *boosting* son principalmente dos: el número de árboles\index{número!de árboles} y la tasa de aprendizaje\index{tasa!de aprendizaje}.

El primero indica el número de árboles a construir y que, como se ha comentado, es importante optimizar para evitar el sobreajuste del modelo. A diferencia de los modelos *random forest*\index{random forest} o *bagging*\index{bagging}, en el *boosting*\index{boosting} los árboles crecen en secuencia para que cada árbol corrija los errores del anterior. El número de árboles necesarios para que el modelo sea buen predictor puede verse incrementado en función de los valores que tomen los otros hiperparámetros.

La tasa de aprendizaje es el hiperparámetro con el que se determina la contribución de cada árbol en el resultado final y controla la rapidez con la que el algoritmo avanza por el descenso del gradiente, es decir, la velocidad a la que aprende. Este hiperparámetro toma valores entre 0 y 1, aunque los valores habituales oscilan entre 0,001 y 0,3. El modelo es más robusto a las características específicas de cada árbol, permitiendo una buena generalización, cuando la tasa de aprendizaje toma valores bajos. Estos valores también facilitan la parada temprana antes del sobreajuste\index{sobreajuste} del modelo. Sin embargo, utilizar estos valores vuelve al modelo más exigente computacionalmente y dificulta alcanzar el modelo óptimo con un número fijo de árboles. En resumen, cuanto menor sea este valor, más preciso puede ser el modelo, pero también requerirá más árboles en la secuencia.

#### Hiperparámetros de árbol

Los principales hiperparámetros\index{hiperparámetro} de árbol son: la profundidad del árbol\index{profundidad!del árbol} y el número mínimo de observaciones en nodos terminales, como se vio en el Cap. \@ref(cap-arboles).

El primer hiperparámetro controla la profundidad de los árboles individuales\index{árbol!de decisión}. Los valores habituales de profundidad oscilan entre 3 y 8. Los árboles de menor profundidad son eficientes computacionalmente, pero menos precisos. Sin embargo, los árboles de mayor profundidad permiten que el algoritmo capture interacciones únicas, aunque aumentan el riesgo de sobreajuste.

El segundo hiperparámetro, además de controlar el número mínimo de observaciones en los nodos terminales, controla la complejidad de cada árbol. Los valores típicos de este hiperparámetro suelen estar entre 5 y 15. Los valores más altos ayudan a evitar que un modelo aprenda relaciones que pueden ser muy específicas de la muestra particular seleccionada para entrenar el árbol, evitando así el sobreajuste. Sin embargo, los valores más pequeños pueden ayudar con clases desbalanceadas en problemas de clasificación.

### Estrategia de ajuste de hiperparámetros

A diferencia del *random forest*\index{random forest}, los modelos *gradient boosting*\index{gradient boosting} pueden variar mucho en su precisión de acuerdo a su configuración de hiperparámetros. Por ello, el ajuste puede requerir seguir una estrategia. Un buen enfoque para esto es:

-   Elegir una tasa de aprendizaje relativamente alta. El valor predeterminado es 0,1 y generalmente funciona. Sin embargo, para la mayoría de problemas funcionan valores entre 0,05 y 0,2.
-   Determinar el número óptimo de árboles para la tasa de aprendizaje elegida.
-   Ajustar los hiperparámetros del árbol y la tasa de aprendizaje y evaluar la velocidad frente al rendimiento.
-   Ajustar los hiperparámetros específicos del árbol para determinar la tasa de aprendizaje.
-   Una vez que se ajustan los parámetros específicos del árbol, se reduce la tasa de aprendizaje para evaluar cualquier mejora en la precisión.
-   Utilizar la configuración final de hiperparámetros y aumentar los procedimientos de validación cruzada para obtener estimaciones más robustas. Si se utiliza validación cruzada en los pasos anteriores, entonces este paso no es necesario.

### Procedimiento con R: la función `gbm()`

En el paquete `gbm` de **R** se encuentra la función con el mismo nombre `gbm()` que se utiliza para entrenar un modelo *gradient boosting*:


```r
gbm(formula,data=..., ...)
```

-   `formula`: Refleja la relación entre la variable dependiente $Y$ y los predictores tal que $Y \sim X_1 + ... + X_p$.
-   `data`: Conjunto de datos con el que entrenar el árbol de acuerdo a la fórmula indicada.

### Aplicación del modelo *gradient boosting* en R

A través de los datos de compras `dp_entr` incluidos en el paquete `CDR` se va a aplicar el modelo *gradient boosting* para clasificar qué clientes van a comprar un nuevo producto (*tensiómetro digital*) y quienes no. Se entrena el modelo utilizando el conjunto de datos de entrenamiento sin transformar (en su escala original). Así, en lugar de tener las variables categóricas transformadas mediante one-hot-encoding se usan en su escala original, como ocurre con el caso de la variable que mide el nivel educativo.


```r
library("CDR")
library("caret")
library("gbm")
library("reshape")
library("ggplot2")
data(dp_entr)
```


```r
# se determina la semilla aleatoria
set.seed(101)

# se entrena el modelo
model <- train(CLS_PRO_pro13 ~ ., 
            data=dp_entr, 
            method="gbm", 
            metric="Accuracy",
            trControl = trainControl(classProbs = TRUE, 
                                     method = "cv", number = 10)
            )

```




```r
model

Stochastic Gradient Boosting 

558 samples
 17 predictor
  2 classes: 'S', 'N' 

No pre-processing
Resampling: Cross-Validated (10 fold) 
Summary of sample sizes: 502, 502, 502, 503, 503, 502, ... 
Resampling results across tuning parameters:

  interaction.depth  n.trees  Accuracy   Kappa    
  1                   50      0.8564610  0.7130031
  1                  100      0.8690909  0.7383556
  1                  150      0.8762338  0.7526413
  2                   50      0.8690909  0.7382344
  2                  100      0.8762338  0.7526413
  2                  150      0.8799026  0.7599227
  3                   50      0.8763636  0.7528004
  3                  100      0.8781494  0.7563575
  3                  150      0.8835390  0.7671499

Tuning parameter 'shrinkage' was held constant at a value of 0.1
Tuning
 parameter 'n.minobsinnode' was held constant at a value of 10
Accuracy was used to select the optimal model using the largest value.
The final values used for the model were n.trees = 150, interaction.depth =
 3, shrinkage = 0.1 and n.minobsinnode = 10.
```

El modelo resultante del proceso de entrenamiento es un *gradient boosting* que ha ajustado los hiperparámetros a 150 árboles y una profundidad igual a 3. Además, los valores tanto del número mínimo de observaciones en nodos como de la tasa de aprendizaje, toman los valores por defecto de 10 y 0,1, respectivamente. Los resultados en el proceso de validación cruzada se muestran en la Fig. \@ref(fig:GBMBOXPLOT), en el que se observa como la precisión oscila entre el 84% y el 93% en las iteraciones.


```r
ggplot(melt(model$resample[,-4]), aes(x = variable, y = value, fill=variable)) + 
  geom_boxplot(show.legend=FALSE) + 
  xlab(NULL) + ylab(NULL)
```

<div class="figure" style="text-align: center">
<img src="img/boxplotgbm.png" alt="Resultados del modelo GB obtenidos durante el proceso de validación cruzada." width="60%" />
<p class="caption">(\#fig:GBMBOXPLOT)Resultados del modelo GB obtenidos durante el proceso de validación cruzada.</p>
</div>

### *Gradient Boosting* con ajuste automático

Se repite el procedimiento para el ejemplo anterior. Sin embargo, en este ejemplo se ajustan de forma automática\index{ajuste automático} los hiperparámetros más relevantes de dicho algoritmo para mejorar los resultados respecto al modelo anterior. Se observa como los hiperparámetros a ajustar para el método `gbm` son: el número de árboles, la profundidad, la tasa de aprendizaje y el número de observaciones en un nodo.


```r
modelLookup("gbm")
model         parameter                   label forReg forClass probModel
1   gbm           n.trees   # Boosting Iterations   TRUE     TRUE      TRUE
2   gbm interaction.depth          Max Tree Depth   TRUE     TRUE      TRUE
3   gbm         shrinkage               Shrinkage   TRUE     TRUE      TRUE
4   gbm    n.minobsinnode Min. Terminal Node Size   TRUE     TRUE      TRUE
```

Siguiendo la estrategia descrita se definen rangos de posibles valores para los principales hiperparámetros a optimizar.


```r
# Se especifica un rango de valores posibles para los hiperparámetros
tuneGrid <- expand.grid(interaction.depth = c(4,6,8),
                        n.trees = c(10*ncol(dp_entr),300,500), 
                        shrinkage = c(0.05,0.1,0.2), 
                        n.minobsinnode = c(5,10,15))
```

Esta red de posibles valores para los hiperparámetros del modelo se incorporan a la función de entrenamiento. Cuanto más exhaustivo sea el ajuste de estos valores, mayor será el tiempo de ajuste del modelo. La red presentada está formada por 81 combinaciones de los posibles cuatro hiperparámetros.


```r
# se fija la semilla aleatoria
set.seed(101)


# se entrena el modelo
model <- train(CLS_PRO_pro13~., 
            data=dp_entr, 
            method="gbm", 
            metric="Accuracy",
            trControl=trainControl(classProbs = TRUE,
                                   method="cv", number=10),
            tuneGrid=tuneGrid)
```



El modelo que mejores resultados proporciona es aquel que ajusta los hiperparámetros a los siguientes valores: 180 árboles, una profundidad igual a 6, una tasa de aprendizaje de 0.05 y un tamaño mínimo de los nodos de 10 observaciones.


```r
model$bestTune
   n.trees interaction.depth shrinkage n.minobsinnode
13     180                 6      0.05             10
```

En la Fig. \@ref(fig:modelgbmboxplot) se muestran los resultados obtenidos durante el proceso de validación cruzada. Se puede ver que los resultados son similares a los del modelo anterior, aunque hay diferencias importantes. En primer lugar, se alcanza un valor máximo de precisión mayor al anterior, pues en este caso la precisión oscila entre el 84% y el 95%. En segundo lugar, vemos que el valor mediano de la precisión ha subido del 87.5% del modelo anterior hasta el 90% de este modelo. Por último, que el rendimiento haya variado tan poco desde el modelo por defecto a un modelo en el que se ha intentado ajustar los hiperparámetros, confirma lo ya expuesto sobre el buen rendimiento de un modelo de *gradient boosting* con los parámetros por defecto.


```r
ggplot(melt(model$resample[,-4]), aes(x = variable, y = value, fill=variable)) + 
  geom_boxplot(show.legend=FALSE) + 
  xlab(NULL) + ylab(NULL)
```

<div class="figure" style="text-align: center">
<img src="img/boxplottunedgbm.png" alt="Resultados del modelo GB con ajuste autmático obtenidos durante el proceso de validación cruzada." width="60%" />
<p class="caption">(\#fig:modelgbmboxplot)Resultados del modelo GB con ajuste autmático obtenidos durante el proceso de validación cruzada.</p>
</div>

## eXtreme Gradient Boosting (XGB)

El *eXtreme Gradient Boosting*\index{extreme gradient boosting} es una implementación eficiente y escalable del modelo *gradient boosting*\index{gradient boosting}. Este modelo, abreviado como XGBoost, es un paquete de código abierto en C++, Java, Python [@wade2020hands], R, Julia, Perl y Scala. En R el modelo se incluye dentro del paquete `xgboost` [@chen2015xgboost]. El paquete incluye un procedimiento para la solución eficiente de modelos lineales y un algoritmo de aprendizaje de árboles.

El paquete es compatible con funciones objetivo de regresión, clasificación y ranking. Además, tiene varias características importantes:

1.  Velocidad: `xgboost` puede realizar automáticamente cálculos paralelos. Por lo general, es 10 veces más rápido que el modelo *gradient boosting*.

2.  Tipo de entrada: `xgboost` toma varios tipos de datos de entrada en `R`:

-   Matriz densa (`matrix`)
-   Matriz dispersa (`Matrix::dgCMatrix`)
-   Archivo de datos locales
-   Un tipo de datos propio del paquete: `xgb.DMatrix`

3.  Dispersión: `xgboost` acepta datos de entrada dispersos para los modelos incluidos.

4.  Personalización: `xgboost` admite tanto funciones de objetivo y funciones de evaluación personalizadas.

5.  Rendimiento: `xgboost` alcanza generalmente una mayor precisión.

### Hiperparámetros del modelo XGBoost

El modelo XGBoost proporciona los hiperparámetros\index{hiperparámetro} que ya incluía el modelo *gradient boosting*\index{gradient boosting} referentes tanto al *boosting*\index{boosting} como a los árboles\index{árbol!de decisión}. Sin embargo, `xgboost` también proporciona hiperparámetros adicionales que pueden ayudar a reducir las posibilidades de sobreajuste, lo que lleva a una menor variabilidad de predicción y, por lo tanto, a una mayor precisión. Estos hiperparámetros son: la regularización y el dropout.

Los parámetros de regularización\index{regularización} se incluyen para ayudar a evitar el sobreajuste\index{sobreajuste} y reducir la complejidad\index{complejidad} del modelo. Existen tres hiperparámetros que tienen esta funcionalidad: gamma ($\gamma$), alpha ($\alpha$) y lambda ($\lambda$). Gamma es un hiperparámetro de pseudo-regularización conocido como multiplicador Lagrangiano y controla la complejidad de un árbol dado. Este hiperparámetro establece que para hacer una partición adicional en un nodo es necesaria una reducción de pérdida mínima especificada por `gamma`. Al especificarlo, el modelo XGBoost hace crecer los árboles hasta una profundidad máxima establecida, pero en un paso de poda eliminará las divisiones que no cumplan con la regularización $\gamma$. Este hiperparámetro toma valores entre 0 e infinito ($\infty$), siguiendo la regla de que a mayor valor, mayor será la regularización. Los otros hiperparámetros de regularización, $\alpha$ y $\lambda$, son más clásicos. Mientras que $\alpha$ proporciona una regularización $L_1$, $\lambda$ proporciona una regularización $L_2$. Estos parámetros de regularización establecen un límite a cómo de extremos pueden llegar a ser los pesos de los nodos en un árbol. Sus valores se encuentran, al igual que los de $\gamma$, entre 0 y $\infty$.

El dropout es un enfoque alternativo para reducir el sobreajuste\index{sobreajuste}. Cuando se entrena un modelo de *gradient boosting*\index{gradient boosting}, los primeros árboles tienden a dominar el rendimiento del modelo, mientras que los que se agregan después suelen mejorar la predicción solo para un pequeño grupo de variables. Esto puede llevar a que se incremente el riesgo de sobreajuste. Con el dropout, se descartan árboles aleatoriamente en el proceso de entrenamiento.

En su implementación en R, el modelo XGBoost incluye principalmente los siguientes parámetros para ser optimizados: número de iteraciones, profundidad máxima de los árboles, tasa de aprendizaje y la regularización $\gamma$.


```r
head(modelLookup("xgbTree"),4)
    model parameter                  label forReg forClass probModel
1 xgbTree   nrounds  # Boosting Iterations   TRUE     TRUE      TRUE
2 xgbTree max_depth         Max Tree Depth   TRUE     TRUE      TRUE
3 xgbTree       eta              Shrinkage   TRUE     TRUE      TRUE
4 xgbTree     gamma Minimum Loss Reduction   TRUE     TRUE      TRUE
```

### Procedimiento con R: la función `xgboost()`

En el paquete `xgboost` de **R** se encuentra la función `xgboost()` que se utiliza para entrenar un modelo *extreme gradient boosting*:


```r
xgboost(data = ..., label = ..., ...)
```

-   `data`: Conjunto de datos con el que entrenar el modelo.
-   `label`: Vector con la variable respuesta.

### Aplicación del modelo XGBoost en R

Se entrena este modelo utilizando el conjunto de entrenamiento sin transformar (en su escala original). Se continúa así el ejemplo expuesto durante la aplicación del modelo *gradient boosting* sin y con ajuste automático de sus hiperparámetros. Se repite el procedimiento de entrenar el modelo para los hiperparámetros por defecto que proporciona R.


```r
# se determina la semilla aleatoria
set.seed(101)

# se entrena el modelo
model <- train(CLS_PRO_pro13~.,
               data=dp_entr,
               method="xgbTree",
               metric="Accuracy",
               trControl=trainControl(classProbs = TRUE,
                                      method = "cv",
                                      number=10))
```



Por defecto, el entrenamiento establece valores constantes para la regularización $\gamma$ (0) y para el tamaño mínimo del nodo (1). En cambio, ajusta los hiperparámetros del modelo dentro de los valores por defecto de la función. Así, el modelo XGBoost resultante tiene 50 iteraciones, una profundidad máxima igual a 2 y una tasa de aprendizaje de 0,3. Los resultados de la validación cruzada muestran que la precisión obtenida oscila entre el 85% y el 95%, resultado similar al del *gradient boosting* con hiperparámetros ajustados. Sin embargo, el valor mediano de la precisión es del 88%, ligeramente inferior a la observada en el modelo *gradient boosting* con ajuste automático.


```r
ggplot(melt(model$resample[,-4]), aes(x = variable, y = value, fill=variable)) + 
  geom_boxplot(show.legend=FALSE) + 
  xlab(NULL) + ylab(NULL)
```

<div class="figure" style="text-align: center">
<img src="img/boxplotxgbm.png" alt="Resultados del modelo durante la validación cruzada." width="60%" />
<p class="caption">(\#fig:XGBRESULTS)Resultados del modelo durante la validación cruzada.</p>
</div>

### XGBoost con ajuste automático

Se continúa el ejemplo aplicado a los datos sobre compra de un nuevo producto por parte de los clientes utilizando un modelo XGBoost en R. Sin embargo, se quieren mejorar los resultados obtenidos, y por ello ajustan automáticamente\index{ajuste automático} los hiperparámetros más relevantes de dicho algoritmo generando una red de posibles valores para dichos hiperparámetros. Por motivos computacionales, ésta no se hace excesivamente exhaustiva para evitar largos tiempos de entrenamiento. Si se dispone de tiempo suficiente para el entrenamiento, es aconsejable tratar de estudiar más valores para los hiperparámetros a optimizar.


```r
# Se especifica un rango de valores típicos para los hiperparámetros
tuneGrid <- expand.grid (nrounds=c(50,100,500), 
                         max_depth = c(3,4,8),
                         eta =c(0.05,0.1,0.2,0.3),
                         gamma=c(0,0.5,5),
                         colsample_bytree=c(0.8),
                         min_child_weight=c(5),
                         subsample=c(0.5)) 
```


```r
# se determina la semilla aleatoria
set.seed(101)


# se entrena el modelo
model <- train(CLS_PRO_pro13~., 
               data=dp_entr, 
               method="xgbTree", 
               metric="Accuracy",
               trControl=trainControl(classProbs = TRUE,
                                      method = "cv",
                                      number = 10),
               tuneGrid=tuneGrid)

```




```r
model$bestTune[,1:4]
   nrounds max_depth eta gamma
71     100         4 0.2     5
```

El modelo resultante establece que se utilicen 100 iteraciones, que los árboles tengan una profundidad máxima de 4, una tasa de aprendizaje del 0,2 y que la regularización $\gamma$ tome el valor 5.



<div class="figure" style="text-align: center">
<img src="img/boxplottunedxgbm.png" alt="Resultados del modelo durante la validación cruzada." width="60%" />
<p class="caption">(\#fig:xgb-tuned-RESULTS)Resultados del modelo durante la validación cruzada.</p>
</div>

Los resultados obtenidos durante la validación cruzada muestran que la precisión es muy similar a la del modelo por defecto, al encontrarse entre el 85% y el 95%. Sin embargo, se observa en el valor mediano de la precisión una ligera mejoría, al aumentar hasta el 90%.

::: {.infobox_resume data-latex=""}
### Resumen {.unnumbered}

En este capítulo se introduce al lector en el algoritmo de aprendizaje supervisado conocido como *gradient boosting*, en concreto:

-   Se presenta el otro paradigma principal de aprendizaje ensamblado: el *boosting*.
-   Se explica el modelo basado en este paradigma, el *gradient boosting*, así como sus diferencias con el *random forest* (basado en *bagging*).
-   Se exponen los hiperparámetros más relevantes a la hora de optimizar un modelo de *gradient boosting*.
-   Se presenta el *eXtreme gradient boosting*, una implementación eficiente y escalable del modelo *gradient boosting*. Así como los hiperparámetros de regularización y otros parámetros importantes en esta implementación.
-   Se aplican ambos algoritmos en `R` en un caso práctico para la clasificación binaria de datos.
:::
