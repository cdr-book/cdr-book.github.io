
# Detección de fraude de tarjetas de crédito {#cap-fraude}


*Pedro Albarracín García*

DXC Technology

## Introducción

En un informe publicado en diciembre de 2021 por Nilson Report,^[https://nilsonreport.com/newsletters/1209/]
se anunció que los emisores de tarjetas de crédito, comerciantes y
consumidores sufrieron un total de 28.580 millones de dólares de
pérdidas por fraude en 2020, es decir, 6,8 centavos por cada 100 dólares
en volumen de compras. Solo el fraude representa el 35,83% del
total mundial.

\index{fraude}

En Europa la situación no es más alentadora. Según un informe del Banco Central Europeo publicado en 2020,^[https://www.ecb.europa.eu/pub/cardfraud/html/ecb.cardfraudreport202008~521edb602b.en.html#toc2]
el valor total de las transacciones con tarjeta en la zona SEPA
ascendió a 4,84 billones de euros en 2018, de los cuales 1.800
millones correspondieron a operaciones fraudulentas.

Las entidades financieras trabajan a diario en el desarrollo de modelos
de *machine learning* y *deep learning* que les permitan detectar, con la
mayor precisión posible, aquellas operaciones de compra con tarjetas de
crédito, débito o prepago que puedan ser sospechosas de fraude, o que al
menos puedan ser identificadas como anómalas. En este sentido, es
importante destacar que no existe una única solución posible, ya que el
problema presenta, en la mayoría de los casos, múltiples variantes que
hacen de este un problema complejo y que puede y debe ser abordado
desde múltiples perspectivas y con diferentes enfoques.

\index{machine learning@\textit{machine learning}}
\index{deep learning@\textit{deep learning}}

En primer lugar, es posible identificar dos tipos de fraude. Por un
lado, el que se comete físicamente, como, por ejemplo, la compra o la
retirada de efectivo con tarjetas robadas o falsas. Por otro lado, están
aquellas transacciones fraudulentas que se cometen online, en las que no
es necesaria la tarjeta física, y en las que se utilizan los datos de
las tarjetas obtenidas por los delincuentes mediante técnicas como el
*phishing* y que son utilizados posteriormente para realizar pagos online.


Otro hándicap asociado a este tipo de escenarios es el derivado de la
gran diversidad de fuentes de datos que forman parte de una transacción
y que pueden dar lugar a divergencias metodológicas, tanto en la
recogida y transmisión de los datos como en su posterior almacenaje, lo
que ocasiona que, en muchos casos, la calidad de los datos disponibles
no sea la esperada, o simplemente que los datasets
sean inconsistentes. Los datos requeridos para este caso de uso pueden
categorizarse en variables relativas a:

- Cliente.

- Transacción.

- Geolocalización.

- Comercio.

- Tarjetas.

- Hábitos de compra.

Cada una de estas categorías, y otras que puedan aparecer, aportan
información que permite abordar el problema desde diferentes ángulos.
Se puede enfocar el problema desde el punto de vista del
cliente y sus hábitos de compra para ver si existe alguna característica
anómala en una transacción como, por ejemplo, la hora de la compra. También se pueden analizar los datos de geolocalización junto con los del comercio para
analizar si es una compra en un comercio habitual y desde una
localización conocida, etc.



## Modelización del fraude en la compra con tarjetas de crédito

El objetivo de este caso de uso es la construcción de un modelo que
permita detectar si una operación de compra realizada con tarjeta de
crédito es fraudulenta o no. Para ello, se utilizará un conjunto de datos
anonimizado de operaciones con tarjeta de crédito ya etiquetadas
disponible desde la web de Kaggle en el siguiente enlace:
<https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud>. 
No obstante, los datos se han incorporado al paquete `CDR` del libro
con el nombre `creditcard`. 

El conjunto de datos consta de 
284.807 transacciones, de las cuales 492 están
etiquetadas como fraudulentas, es decir, solo un 0,172% del total de las
transacciones. Como puede apreciarse, se trata de un dataset muy desequilibrado, lo que
añade cierto grado de dificultad al análisis. El dataset `creditcard` incluye 31
variables, de las cuales 28 están identificadas como `V1`, ..., `V28`, una
identificada como `Time` que registra los segundos transcurridos entre una
transacción y la primera, otra con nombre `Amount` que registra el importe
de la transacción, y la variable dependiente, `class`, con
valor 0 para las operaciones no fraudulentas y 1 para las fraudulentas.

Para concluir esta breve descripción del conjunto de datos, es necesario recordar
que todos los valores de entrada son numéricos y que ya han sufrido
algunas transformaciones. Por motivos de confidencialidad, las variables
V1 a V28 no incluyen sus nombres originales ni se añade más información
de contexto.

**Carga de los datos y obtención de algunos descriptivos**


```r
creditcard <- CDR::creditcard
names(creditcard)
#>  [1] "Time"   "V1"     "V2"     "V3"     "V4"     "V5"     "V6"     "V7"    
#>  [9] "V8"     "V9"     "V10"    "V11"    "V12"    "V13"    "V14"    "V15"   
#> [17] "V16"    "V17"    "V18"    "V19"    "V20"    "V21"    "V22"    "V23"   
#> [25] "V24"    "V25"    "V26"    "V27"    "V28"    "Amount" "Class"
# psych::describe(creditcard)  # descomentar para ver los descriptivos
```

**División de los datos**

A continuación es necesario dividir los datos en dos *dataframes*
denominados `creditcard_X` y `creditcard_y`. De esta forma, se separan
las variables independientes de la variable dependiente o `Class`.


```r
# Se dividen los datos
creditcard_X <- creditcard[,-31]
creditcard_y <- creditcard$Class
```

**Tratamiento de datos desequilibrados**

Uno de los principales problemas a la hora de abordar este tipo de
escenarios es lo que se conoce como "datos desequilibrados". \index{datos!desequilibrados} 
Se dice que un conjunto de datos está desequilibrado cuando la variable dependiente
presenta más observaciones de una clase que de otra. En el caso de
transacciones con tarjeta de crédito, es evidente que la
mayoría de las operaciones son legítimas y que solo un
pequeño porcentaje resulta ser fraudulento. ¿Cuál es el problema?

El problema es que, por lo general, los modelos entrenados con conjuntos de datos desequilibrados no
se comportan bien cuando tienen que generalizar, es decir, cuando tienen
que realizar predicciones sobre conjuntos de datos que no han sido
vistos anteriormente por el modelo. El desequilibrio de los datos es un
sesgo hacia la clase mayoritaria, por lo que, en última instancia,
hay una tendencia al sobreajuste (*overfitting*) hacia esa clase.
Existen diversas técnicas que permiten corregir esta situación:

+ ***Undersampling*** o submuestreo. \index{submuestreo} Esta técnica consiste en reducir el
número de observaciones de la clase mayoritaria, estableciendo, por ejemplo,
una ratio de 60/40. Esta técnica resulta efectiva si se respetan los
grupos naturales que existen en los datos, así como el resto de las
características presentes en la clase mayoritaria.

+ ***Oversampling*** o sobremuestreo. \index{sobremuestreo} Consiste en aumentar
el número de observaciones de la clase minoritaria mediante la creación
de datos sintéticos que, al igual que la técnica anterior, respeten las
características de esa clase.

Para la creación de datos sintéticos \index{datos!sintéticos} con *oversampling*
existen varios algoritmos que proporcionan buenos resultados. Quizás el
más conocido y utilizado sea *Synthetic Minority Oversampling
TEchnique* (SMOTE). SMOTE no realiza una copia de las observaciones del
dataset, sino que, en su lugar, genera nuevos datos de forma sintética
utilizando los vecinos más cercanos de esos casos, respetando las
características estadísticas de la clase. Además, los ejemplos de la
clase mayoritaria también son submuestreados, lo que da lugar a un
conjunto de datos más equilibrado. En **R**, el algoritmo SMOTE está implementado en el 
paquete `DMwR`.

Para este caso de uso particular, se utiliza una técnica simple de
submuestreo implementada en una función del paquete `unbalanced`. Este paquete no se
encuentra disponible en el repositorio de CRAN por lo que, para su
instalación, se debe ejecutar el siguiente código:


```r
#install.packages("devtools") # descomentar para instalar 
library("devtools")
# devtools::install_github("dalpozz/unbalanced") # descomentar para instalar 
library("unbalanced")
```

Una vez instalado `unbalanced` y todas sus dependencias, se realiza el
submuestreo del dataset siguiendo los siguientes pasos:

1. Convertir la variable dependiente `Class` en factor:


```r
creditcard$Class <-as.factor(creditcard$Class)
levels(creditcard$Class) <- c('0', '1') 
```

2. Ejecutar la función de submuestreo:


```r
undersampled_creditcard <- ubBalance(creditcard_X, creditcard$Class, type='ubUnder', verbose = TRUE)
#> Proportion of positives after ubUnder : 50 % of 984 observations
undersampled_combined <- cbind(undersampled_creditcard$X,   
                               undersampled_creditcard$Y)

names(undersampled_combined)[names(undersampled_combined) == "undersampled_creditcard$Y"] <- "Class"
levels(undersampled_combined$Class) <- c('Legítima', 'Fraude')
```

3. Comprobar el número de casos del conjunto de datos sobre el que se ha
ejecutado la función de submuestreo:


```r
creditcard$Class <-as.factor(creditcard$Class)
levels(creditcard$Class) <- c('0', '1') 
```

4. Construir el gráfico incluido en la Fig. \@ref(fig:muestras) para visualizar el número de elementos de cada
clase después de realizar el submuestreo:


```r
library("ggplot2")
ggplot(data = undersampled_combined, aes(fill = Class))+
  geom_bar(aes(x = Class))+
  ggtitle("Número de casos de cada clase después de submuestreo", 
          subtitle="Total muestras: 984")+
  xlab("")+
  ylab("Muestras")+
  scale_y_continuous(expand = c(0,0))+
  scale_x_discrete(expand = c(0,0))+
  theme(legend.position = "ninguna", 
        legend.title = element_blank(),
        panel.grid.major = element_blank(),
        panel.grid.minor = element_blank(),
        panel.background = element_blank())
```

<div class="figure" style="text-align: center">
<img src="212060_cd_finanzas_files/figure-html/muestras-1.png" alt="Número de casos de cada clase después de submuestreo." width="60%" />
<p class="caption">(\#fig:muestras)Número de casos de cada clase después de submuestreo.</p>
</div>


### Modelo de clasificación mediante regresión lógística

A continuación se procede a la construcción de un modelo de regresión
logística (véase Cap. \@ref(cap-glm)) para una clasificación binaria
en relación al fraude en transacciones con tarjeta de crédito a partir
de los datos equilibrados \index{datos!equilibrados} obtenidos anteriormente. Por tanto, el *dataframe* que se utiliza es `undersampled_combined`, que contiene 984
observaciones, un 50% de las cuales son transacciones identificadas como
fraude.

Lo primero que hay que hacer es realizar un par de pequeños cambios en el dataset; concretamente: $(i)$ eliminar las variables `Time` y `Amount`, ya que no son relevantes en el modelo, y $(ii)$ cambiar por 0 y 1 las etiquetas `Legitima`
y `Fraude`, respectivamente.


```r
undersampled_combined <- subset( undersampled_combined, select = -c(Time, Amount ) )
undersampled_combined$Class <- ifelse(undersampled_combined$Class == "Fraude",1,0)
```

Lo siguiente será dividir el conjunto de datos en los subconjuntos de
entrenamiento y test, para lo cual se aplica la función `split()` con
un `SplitRatio` de  0,80, es decir, un 80% de los datos irá de forma
aleatoria al subconjunto de entrenamiento (788 observaciones) y
el 20% restante (196 observaciones) al subconjunto de test.


```r
#install.packages("caTools") # descomentar para instalar
library("caTools")
set.seed(123)
split = sample.split(undersampled_combined$Class, SplitRatio = 0.80)
training = subset(undersampled_combined, split == TRUE)
test = subset(undersampled_combined, split == FALSE)
```

Una vez en disposición de los subconjuntos de entrenamiento y test, el siguiente paso es entrenar el modelo de regresión logística que clasificará las
transacciones en legítimas o fraudulentas. Para ello se utiliza el
algoritmo GLM, creando un clasificador, que se identificará como
`undersampledModel`, al que se le pasarán los parámetros siguientes:

• `formula`: con este parámetro se indica la variable dependiente
seguida del símbolo \~ y un punto (con el punto se hace referencia al
resto de variables del conjunto de datos: `V1`...`V28`).

• `data`: el dataset con los datos de entrenamiento.

• `family`: al ser un clasificador con dos valores posibles (0, 1), se
indica que será de tipo `binomial`.


```r
undersampledModel = glm(Class ~ ., data = training, family = binomial())
```

Para ver los resultados del modelo, se ejecutará:


```r
summary(undersampledModel)
#> 
#> Call:
#> glm(formula = Class ~ ., family = binomial(), data = training)
#> 
#> Coefficients:
#>              Estimate Std. Error z value Pr(>|z|)    
#> (Intercept)    2.5009     1.7509   1.428 0.153183    
#> V1           -17.9509     4.9954  -3.594 0.000326 ***
#> V2            17.5583     4.9540   3.544 0.000394 ***
#> V3           -43.3067    11.7479  -3.686 0.000228 ***
#> V4            27.9272     7.2714   3.841 0.000123 ***
#> V5           -29.4626     8.0343  -3.667 0.000245 ***
#> V6           -10.1965     2.7901  -3.654 0.000258 ***
#> V7           -56.5920    15.4443  -3.664 0.000248 ***
#> V8            11.1555     3.2997   3.381 0.000723 ***
#> V9           -28.9426     7.7225  -3.748 0.000178 ***
#> V10          -68.2216    18.1381  -3.761 0.000169 ***
#> V11           48.0556    12.8217   3.748 0.000178 ***
#> V12          -85.8596    22.9005  -3.749 0.000177 ***
#> V13            0.3407     0.2936   1.160 0.245887    
#> V14          -89.9649    23.8261  -3.776 0.000159 ***
#> V15           -1.1620     0.4824  -2.409 0.016008 *  
#> V16          -79.8930    21.4102  -3.732 0.000190 ***
#> V17         -145.4601    38.9605  -3.734 0.000189 ***
#> V18          -54.4924    14.5879  -3.735 0.000187 ***
#> V19           18.0532     4.8254   3.741 0.000183 ***
#> V20            8.8181     2.6847   3.285 0.001022 ** 
#> V21           11.8179     3.1719   3.726 0.000195 ***
#> V22            1.2859     0.4156   3.094 0.001977 ** 
#> V23           -1.6211     0.3723  -4.354 1.34e-05 ***
#> V24           -0.6138     0.5190  -1.183 0.236943    
#> V25            3.2724     1.1378   2.876 0.004027 ** 
#> V26            0.7369     0.5534   1.332 0.182950    
#> V27           16.3942     4.2152   3.889 0.000101 ***
#> V28           14.4211     3.7671   3.828 0.000129 ***
#> ---
#> Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
#> 
#> (Dispersion parameter for binomial family taken to be 1)
#> 
#>     Null deviance: 1092.40  on 787  degrees of freedom
#> Residual deviance:  186.13  on 759  degrees of freedom
#> AIC: 244.13
#> 
#> Number of Fisher Scoring iterations: 19
```

Con el modelo ya entrenado, se realizan las predicciones para los datos
del subconjunto de test, utilizando para ello la función `predict()`.
Los parámetros son simples: el primero es el modelo o clasificador que
se va a utilizar y que será `undersampledModel`; a continuación el tipo
de dato que devolverá, en este caso `response`: las probabilidades de fraude listadas en un único
vector que estará disponible a partir de la variable `fraud_prob`;
y, por último, el parámetro `newdata`, que hace referencia al dataset en
el que se descarta la última columna por ser la que representa la
variable dependiente.


```r
fraud_prob = predict(undersampledModel, type = "response", 
                     newdata = test[,-29])
head(fraud_prob)
#>          646         1923         2963         4025         5332         6109 
#> 4.544401e-04 1.247090e-13 3.944901e-02 2.558175e-02 4.559502e-04 1.000000e+00
```

La visualización del vector con las predicciones puede parecer algo
confusa, por lo que, a menudo, es preciso realizar una conversión de
esas predicciones en valores 0 y 1, dependiendo del rango de valores a
partir del cual se estime que una transacción es fraudulenta: por
ejemplo, a partir del 60% de probabilidad, la transacción será
etiquetada como "1"; en caso contrario será etiquetada como "0". Para
ello se utiliza la función `ifelse`:


```r
y_pred = ifelse(fraud_prob > 0.6, 1, 0)
```

**La matriz de confusión**

Como último paso del ejercicio se crea la matriz de confusión con el fin
de visualizar qué tal se ha comportado el algoritmo, es decir, cuántos
positivos y negativos ha logrado predecir correctamente. Para ello se
crea la variable `confusionMatrix`, en la cual se almacenará el
resultado de la comparación entre el vector del suconjunto de test, es
decir, los datos etiquetados originalmente, y el vector de sus
traducciones a 0 y 1, resultado del algoritmo. El resultado, como se
puede comprobar, es que ha evaluado correctamente 186 de las 196
observaciones.


```r
confusionMatrix = table(test[, 29], y_pred)
confusionMatrix
#>    y_pred
#>      0  1
#>   0 95  3
#>   1  8 90
```

