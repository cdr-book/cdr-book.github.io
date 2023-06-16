
# Detección de fraude de tarjetas de crédito {#cap-fraude}


*Pedro Albarracín García*

## Introducción

En un informe publicado en diciembre de 2021 por Nilson Report
(<https://nilsonreport.com/upload/content_promo/NilsonReport_Issue1209.pdf>),
se informó de que los emisores de tarjetas de crédito, comerciantes y
consumidores sufrieron un total de 28.580 millones de dólares de
pérdidas por fraude en 2020, es decir, 6,8 centavos por cada 100 dólares
en volumen de compras. El fraude sólo en USA representa el 35,83% del
total mundial.

En Europa la situación no es más alentadora. Según un informe del Banco
Central Europeo publicado en 2020
(<https://www.ecb.europa.eu/pub/cardfraud/html/ecb.cardfraudreport202008~521edb602b.en.html#toc2>),
el valor total de las transacciones con tarjeta en la zona SEPA
ascendieron a 4.84 billones de euros en 2018, de los cuales 1.800
millones correspondieron a operaciones fraudulentas.

Las entidades financieras trabajan a diario en el desarrollo de modelos
de machine learning y deep learning que les permitan detectar, con la
mayor precisión posible, aquellas operaciones de compra con tarjetas de
crédito, débito o prepago que puedan ser sospechosas de fraude, o que al
menos puedan ser identificadas como anómalas. En este sentido, es
importante destacar que no existe una única solución posible, ya que el
problema presenta, en la mayoría de los casos, múltiples variantes que
hacen de éste, un problema complejo y que puede y debe ser abordado
desde múltiples perspectivas y con diferentes enfoques.

En primer lugar, es posible identificar dos tipos de fraude. Por un
lado, el que se comete físicamente, como, por ejemplo, la compra o la
retirada de efectivo con tarjetas robadas o falsas. Por otro lado, están
aquellas transacciones fraudulentas que se cometen online, en las que no
es necesaria la tarjeta física, y en las que se utilizan los datos de
las tarjetas obtenidas por los delincuentes mediante técnicas como el
phishing y que son utilizados posteriormente para realizar pagos online.

Otro hándicap asociado a este tipo de escenarios es el derivado de la
gran diversidad de fuentes de datos que forman parte de una transacción
y que pueden dar lugar a divergencias metodológicas, tanto en la
recogida y transmisión de los datos, como en su posterior almacenaje, lo
que ocasiona que, en muchos casos, la calidad de los datos disponibles
no sea la esperada, o simplemente nos encontremos ante datasets
inconsistentes. Los datos requeridos para este caso de uso pueden
categorizarse en variables relativas a:

• Cliente 

• Transacción 

• Geolocalización 

• Comercio 

• Tarjetas 

• Hábitos de compra

Cada una de estas categorías, y otras que puedan aparecer aportan
información que permite abordar el problema desde diferentes ángulos.
Por un lado es posible enfocar el problema desde el punto de vista del
cliente y sus hábitos de compra para ver si existe alguna característica
anómala en una transacción, tal vez la hora de la compra, o quizás
analizar los datos de geolocalización junto con los del comercio para
analizar si es una compra en un comercio habitual y desde una
localización conocida, etc.



## Modelización del fraude en la compra con tarjetas de crédito

El objetivo de este caso de uso es la construcción de un modelo que
permita detectar si una operación de compra realizada con tarjeta de
crédito es fraudulenta o no. Para ello, se utilizará un dataset
anonimizado de operaciones con tarjeta de crédito ya etiquetadas
disponible desde la web de Kaggle en el siguiente enlace:
<https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud>. 
No obstante, los datos se han incorporado la paquete `CDR` del libro
con el nombre `creditcard`. El conjunto de datos consta de 
284.807 transacciones, de las cuales 492 están
etiquetadas como fraudulentas, es decir, sólo un 0,172% del total de las
transacciones. Es un dataset por lo tanto muy desequilibrado, lo que
añade cierto grado de dificultad. El dataset `creditcard` tiene un conjunto de 31
variables, de las cuales 28 están identificadas como `V1`, ..., `V28`, una
variable `Time` que registra los segundos transcurridos entre esa
transacción y la primera, una variable `Amount` que registra el importe
de la transacción, y la variable dependiente `class` que indica, con
valor 0, que la operación es "no fraudulenta", y con valor 1 las
operaciones fraudulentas.

Para concluir esta breve descripción del dataset, es necesario recordar
que todos los valores de entrada son numéricos y que ya han sufrido
algunas transformaciones. Por motivos de confidencialidad, las variables
V1 a V28 no incluyen sus nombres originales ni se añade más información
de contexto.

**Carga de los datos y obtención de algunos descriptivos**


```r
creditcard <- CDR::creditcard
head(creditcard)
#>   Time         V1          V2        V3         V4          V5          V6
#> 1    0 -1.3598071 -0.07278117 2.5363467  1.3781552 -0.33832077  0.46238778
#> 2    0  1.1918571  0.26615071 0.1664801  0.4481541  0.06001765 -0.08236081
#> 3    1 -1.3583541 -1.34016307 1.7732093  0.3797796 -0.50319813  1.80049938
#> 4    1 -0.9662717 -0.18522601 1.7929933 -0.8632913 -0.01030888  1.24720317
#> 5    2 -1.1582331  0.87773675 1.5487178  0.4030339 -0.40719338  0.09592146
#> 6    2 -0.4259659  0.96052304 1.1411093 -0.1682521  0.42098688 -0.02972755
#>            V7          V8         V9         V10        V11         V12
#> 1  0.23959855  0.09869790  0.3637870  0.09079417 -0.5515995 -0.61780086
#> 2 -0.07880298  0.08510165 -0.2554251 -0.16697441  1.6127267  1.06523531
#> 3  0.79146096  0.24767579 -1.5146543  0.20764287  0.6245015  0.06608369
#> 4  0.23760894  0.37743587 -1.3870241 -0.05495192 -0.2264873  0.17822823
#> 5  0.59294075 -0.27053268  0.8177393  0.75307443 -0.8228429  0.53819555
#> 6  0.47620095  0.26031433 -0.5686714 -0.37140720  1.3412620  0.35989384
#>          V13        V14        V15        V16         V17         V18
#> 1 -0.9913898 -0.3111694  1.4681770 -0.4704005  0.20797124  0.02579058
#> 2  0.4890950 -0.1437723  0.6355581  0.4639170 -0.11480466 -0.18336127
#> 3  0.7172927 -0.1659459  2.3458649 -2.8900832  1.10996938 -0.12135931
#> 4  0.5077569 -0.2879237 -0.6314181 -1.0596472 -0.68409279  1.96577500
#> 5  1.3458516 -1.1196698  0.1751211 -0.4514492 -0.23703324 -0.03819479
#> 6 -0.3580907 -0.1371337  0.5176168  0.4017259 -0.05813282  0.06865315
#>           V19         V20          V21          V22         V23         V24
#> 1  0.40399296  0.25141210 -0.018306778  0.277837576 -0.11047391  0.06692807
#> 2 -0.14578304 -0.06908314 -0.225775248 -0.638671953  0.10128802 -0.33984648
#> 3 -2.26185710  0.52497973  0.247998153  0.771679402  0.90941226 -0.68928096
#> 4 -1.23262197 -0.20803778 -0.108300452  0.005273597 -0.19032052 -1.17557533
#> 5  0.80348692  0.40854236 -0.009430697  0.798278495 -0.13745808  0.14126698
#> 6 -0.03319379  0.08496767 -0.208253515 -0.559824796 -0.02639767 -0.37142658
#>          V25        V26          V27         V28 Amount Class
#> 1  0.1285394 -0.1891148  0.133558377 -0.02105305 149.62     0
#> 2  0.1671704  0.1258945 -0.008983099  0.01472417   2.69     0
#> 3 -0.3276418 -0.1390966 -0.055352794 -0.05975184 378.66     0
#> 4  0.6473760 -0.2219288  0.062722849  0.06145763 123.50     0
#> 5 -0.2060096  0.5022922  0.219422230  0.21515315  69.99     0
#> 6 -0.2327938  0.1059148  0.253844225  0.08108026   3.67     0
# psych::describe(creditcard)  # descomentar para ver los descriptivos
```

**División de los datos**

A continuación es necesario dividir los datos en dos dataframes que
denominados `creditcard_X` y `creditcard_y`, de esta forma se separan
las variables independientes de la variable dependiente o `Class`.


```r
# Se dividen los datos
creditcard_X <- creditcard[,-31]
creditcard_y <- creditcard$Class
```

**Tratamiento de datos desequilibrados**

Uno de los principales problemas a la hora de abordar este tipo de
escenarios, es lo que se conoce como "datos desequilibrados". \index{datos!desequilibrados} 
Se dice que un dataset está desequilibrado cuando la variable dependiente
presenta más observaciones de una clase que de otra. En el caso de
transacciones fraudulentas con tarjeta de crédito, es evidente que la
mayoría de las operaciones son legítimas o benignas, y que sólo un
pequeño porcentaje resultan ser maliciosas. ¿Cuál es el problema?

Por lo general, los modelos entrenados con datasets desequilibrados no
se comportan bien cuando tienen que generalizar, es decir, cuando tienen
que realizar predicciones sobre conjuntos de datos que no han sido
vistos anteriormente por el modelo. El desequilibrio de los datos es un
sesgo hacia la clase mayoritaria, por lo que, en última instancia,
muestra una tendencia al sobreajuste u overfitting hacia esa clase.
Existen diversas técnicas que permiten corregir esta situación:

+ ***Undersampling*** o Submuestreo. \index{submuestreo} Esta técnica consiste en reducir el
número de observaciones de la clase mayoritaria, estableciendo quizás
una ratio de 60/40. Esta técnica resulta efectiva si se respetan los
grupos naturales que existen en los datos, así como el resto de las
características presentes en la clase mayoritaria.

+ ***Oversampling*** o Sobremuestreo. \index{sobremuestreo} Esta técnica consiste en aumentar
el número de observaciones de la clase minoritaria mediante la creación
de datos sintéticos que, al igual que la técnica anterior, respeten las
características de esa clase.

Para la creación de datos sintéticos \index{datos!sintéticos} en escenarios de oversampling
existen varios algoritmos que proporcionan buenos resultados. Quizás el
más conocido y utilizado sea *Synthetic Minority Oversampling
TEchnique* (SMOTE). SMOTE no realiza una copia de las observaciones del
dataset, sino que en su lugar genera nuevos datos de forma sintética
utilizando los vecinos más cercanos de esos casos, respetando las
características estadísticas de la clase. Además, los ejemplos de la
clase mayoritaria también son submuestreados, lo que da lugar a un
conjunto de datos más equilibrado. En R, el algoritmo SMOTE pertenece al
paquete `DMwR`.

Para este caso particular, se utilizará una técnica simple de
submuestreo basada en el paquete `unbalanced`, que actualmente no se
encuentra disponible en el repositorio de CRAN por lo que, para su
instalación, se debe ejecutar el siguiente código:


```r
#install.packages("devtools") # descomentar para instalar 
library("devtools")
devtools::install_github("dalpozz/unbalanced")
library("unbalanced")
```

Una vez instalado, al igual que todas sus dependencias, se realiza el
submuestreo del dataset siguiendo los siguientes pasos:

1.- Convertir la variable dependiente "Class" en factor:


```r
creditcard$Class <-as.factor(creditcard$Class)
levels(creditcard$Class) <- c('0', '1') 
```

2.- A continuación, ejecutar la función de submuestreo:


```r
undersampled_creditcard <- ubBalance(creditcard_X, creditcard$Class, type='ubUnder', verbose = TRUE)
#> Proportion of positives after ubUnder : 50 % of 984 observations
undersampled_combined <- cbind(undersampled_creditcard$X,   
                               undersampled_creditcard$Y)

names(undersampled_combined)[names(undersampled_combined) == "undersampled_creditcard$Y"] <- "Class"
levels(undersampled_combined$Class) <- c('Legítima', 'Fraude')
```

3.- Comprobar el número de casos en el dataset sobre el que se ha
ejecutado la función de submuestreo


```r
creditcard$Class <-as.factor(creditcard$Class)
levels(creditcard$Class) <- c('0', '1') 
```

4.- Realizar la gráfica para visualizar el número de elementos de cada
clase después de realizar el submuestreo


```r
library("ggplot2")
ggplot(data = undersampled_combined, aes(fill = Class))+
  geom_bar(aes(x = Class))+
  ggtitle("Número de casos de cada clase después de submuestreo", 
          subtitle="Total muestrass: 984")+
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

<img src="212060_cd_finanzas_files/figure-html/unnamed-chunk-7-1.png" width="60%" style="display: block; margin: auto;" />


**Modelo de clasificación mediante regresión lógística**

A continuación se procederá a la construcción de un modelo de regresión
logística (véase Cap. \@ref(cap-glm)) para una clasificación binaria
en relación al fraude en transacciones con tarjeta de crédito a partir
de los datos equilibrados \index{equilibrados} obtenidos anteriormente. El dataframe que se
utilizará, por lo tanto, será "undersampled_combined" que contiene 984
observaciones, un 50% de las cuales son transacciones identificadas como
fraude.

Lo primero, será realizar un par de pequeños cambios en el dataset, es
decir, eliminar las variables `Time` y `Amount`, ya que no van a ser
relevantes para el modelo, y cambiar por 0 y 1 las etiquetas "Legitima"
y "Fraude", respectivamente.


```r
undersampled_combined <- subset( undersampled_combined, select = -c(Time, Amount ) )
undersampled_combined$Class <- ifelse(undersampled_combined$Class == "Fraude",1,0)
```

Lo siguiente será dividir el conjunto de datos en los datasets de
entrenamiento y test, para lo cual se aplicará la función "split()" con
un SplitRatio de 0.80, es decir, un 80% de los datos irán de forma
aleatoria al dataset de entrenamiento, 788 observaciones, frente a las
196 observaciones que formarán el dataset de testing.


```r
#install.packages("caTools") # descomentar para instalar
library("caTools")
set.seed(123)
split = sample.split(undersampled_combined$Class, SplitRatio = 0.80)
training = subset(undersampled_combined, split == TRUE)
test = subset(undersampled_combined, split == FALSE)
```

Con los datasets necesarios ya disponibles, el siguiente paso es
entrenar el modelo de regresión logística que clasificará las
transacciones en legítimas o fraudulentas. Para ello se utilizará el
algoritmo GLM, creando un clasificador que se identificará como
`undersampledModel`y al que se le pasarán los parámetros siguientes:

• `formula`: con este parámetro se indica la variable dependiente, class,
seguida del simbolo \~ y un punto (con el punto se hace referencia al
resto de variables del dataset, es decir, `V1` a `V28`).

• `data`: el dataset con los datos de entrenamiento.

• `family`: al ser un clasificador con dos valores posibles (0, 1), se
indica que será de tipo "binomial".


```r
undersampledModel = glm(Class ~ ., data = training, family = binomial())
```

Para ver,


```r
summary(undersampledModel)
#> 
#> Call:
#> glm(formula = Class ~ ., family = binomial(), data = training)
#> 
#> Deviance Residuals: 
#>     Min       1Q   Median       3Q      Max  
#> -1.5541  -0.1782   0.0000   0.0000   3.3888  
#> 
#> Coefficients:
#>              Estimate Std. Error z value Pr(>|z|)   
#> (Intercept)   0.13277    1.62081   0.082  0.93471   
#> V1          -10.16466    4.14244  -2.454  0.01414 * 
#> V2            9.92651    4.08833   2.428  0.01518 * 
#> V3          -23.95129    9.50395  -2.520  0.01173 * 
#> V4           15.65199    5.75859   2.718  0.00657 **
#> V5          -16.35394    6.53594  -2.502  0.01234 * 
#> V6           -6.06917    2.25913  -2.687  0.00722 **
#> V7          -31.07144   12.40824  -2.504  0.01228 * 
#> V8            6.23448    2.83950   2.196  0.02812 * 
#> V9          -16.15734    6.24416  -2.588  0.00966 **
#> V10         -37.44063   14.46013  -2.589  0.00962 **
#> V11          26.22151   10.11499   2.592  0.00953 **
#> V12         -46.92783   18.16360  -2.584  0.00978 **
#> V13           0.19265    0.28429   0.678  0.49798   
#> V14         -49.12923   18.80355  -2.613  0.00898 **
#> V15          -1.03184    0.43742  -2.359  0.01833 * 
#> V16         -43.64101   17.03238  -2.562  0.01040 * 
#> V17         -78.84821   30.87240  -2.554  0.01065 * 
#> V18         -29.25816   11.52421  -2.539  0.01112 * 
#> V19           9.68638    3.79255   2.554  0.01065 * 
#> V20           5.23863    2.20505   2.376  0.01751 * 
#> V21           7.06688    2.49069   2.837  0.00455 **
#> V22           0.71537    0.40820   1.753  0.07968 . 
#> V23          -0.82654    0.29222  -2.828  0.00468 **
#> V24          -0.65786    0.54123  -1.216  0.22417   
#> V25           1.76627    0.80186   2.203  0.02761 * 
#> V26           0.02523    0.56788   0.044  0.96456   
#> V27           9.41649    3.33780   2.821  0.00478 **
#> V28           6.15618    2.34266   2.628  0.00859 **
#> ---
#> Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
#> 
#> (Dispersion parameter for binomial family taken to be 1)
#> 
#>     Null deviance: 1092.40  on 787  degrees of freedom
#> Residual deviance:  179.97  on 759  degrees of freedom
#> AIC: 237.97
#> 
#> Number of Fisher Scoring iterations: 19
```

Con el modelo ya entrenado, se realizan las predicciones para los datos
del conjunto de testing, utilizando para ello la función "predict()".
Los parámetros son simples: el primero es el modelo o clasificador que
se va a utilizar y que será "undersampledModel"; a continuación el tipo
de dato que devolverá, en este caso "response", el cual indica que el
algoritmo devolverá las probabilidades de fraude listadas en un único
vector, el cual estará disponible a partir de la variable "fraud_prob";
y por último, el parámetro "newdata" que hace referencia al dataset en
el que se descarta la última columna por ser la que representa la
variable dependiente.


```r
fraud_prob = predict(undersampledModel, type = "response", newdata = test[,-29])
head(fraud_prob)
#>        1428        3286        3488        4922        6109        6720 
#> 0.061932387 0.001207266 0.010887549 0.387159249 1.000000000 1.000000000
```

La visualización del vector con las predicciones puede parecer algo
confusa, por lo que, a menudo, es preciso realizar una conversión de
esas predicciones en valores 0 y 1, dependiendo del rango de valores a
partir del cual se estime que una transacción es fraudulenta: por
ejemplo, a partir del 60% de probabilidad, la transacción será
etiquetada como "1"; en caso contrario será etiquetada como "0". Para
ello se utiliza el siguiente código "ifelse":


```r
y_pred = ifelse(fraud_prob > 0.6, 1, 0)
```

**La matriz de confusión**

Como último paso del ejercicio se crea la matriz de confusión con el fin
de visualizar qué tal se ha comportado el algoritmo, es decir, cuántos
positivos y negativos ha logrado predecir correctamente. Para ello se
creará la variable `confusionMatrix`, en la cual se almacenará el
resultado de la comparación entre el vector del dataset de testing, es
decir, los datos etiquetados originalmente, y el vector de sus
traducciones a 0 y 1, resultado del algoritmo. El resultado, como se
puede comprobar es que ha evaluado correctamente 186 de las 196
observaciones.


```r
confusionMatrix = table(test[, 29], y_pred)
confusionMatrix
#>    y_pred
#>      0  1
#>   0 96  2
#>   1  8 90
```
