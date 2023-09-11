
# (PART) Ciencia, datos, software... y científicos {.unnumbered}

# ¿Es la ciencia de datos una ciencia? {#ciencia-datos}

*Gema Fernández-Avilés[^bilal]* 

Universidad de Castilla-La Mancha

## ¿Qué se entiende por ciencia? {#ciencia}



No es posible determinar si la ciencia de datos es una ciencia sin
previamente consensuar la definición de ciencia. La palabra *ciencia*
deriva del latín (*scientia*), que deriva a su vez del verbo latino
*scire* (saber). En este sentido, @Bunge_2004 distingue dos categorías
fundamentales de conocimiento: el vulgar y el científico.
El conocimiento vulgar se adquiere de manera cotidiana a partir de percepciones y
sensaciones individuales, apoyándose en la evidencia (pruebas) y el sentido común.
Este tipo de conocimiento constituye la base sobre la que se sustenta el
conocimiento científico, que se enriquece al verificar, con la ayuda
de un método, la validez de las observaciones realizadas. Se adquiere,
por tanto, de forma consciente, deliberada y metódica. Puede ser
sometido a prueba y, llegado el caso, ser superado [@Montero_1997].

[^bilal]: Quisiera agradecer a Bilal Laouah y a José-María Montero los comentarios realizados durante el desarrollo de este capítulo.

\index{ciencia} \index{saber} \index{método} \index{método!científico}

Una definición generalmente aceptada de ciencia es la propuesta por
@Blaug_1980: "ciencia es el cuerpo de proposiciones sintéticas acerca
del mundo real que es susceptible, al menos en principio, de falsaciones
por medio de la observación empírica, ya que excluye la posibilidad de
que ciertos acontecimientos se produzcan. Así pues, la ciencia se
caracteriza por su método de formulación de proposiciones contrastables,
y no por su contenido, ni por su pretensión de certeza en el
conocimiento; si alguna certeza proporciona la ciencia, esta será más
bien la certeza de la ignorancia."

Si bien esta definición está encuadrada dentro del enfoque del falsacionismo de 
Karl Popper (para el cual las teorías son conjeturas, hipótesis generales que permiten explicar fenómenos), introduce la palabra clave de la discusión planteada, el **método**, y
más concretamente, el **método científico**.

La palabra *método* procede del latín *methodus*, y esta del griego
 $\mu \varepsilon \theta o \delta 0 \varsigma$, que quiere decir
"**el camino a seguir para ir más allá**". Es, pues, un procedimiento
para conseguir algo; y como el fin que busca la ciencia es la verdad, el
método científico es el camino mediante el cual las ciencias pueden
llegar a encontrar sus respectivas verdades. El método científico es,
por tanto, el conjunto de procedimientos por medio de los cuales se
adquieren conocimientos rigurosos, ciertos y seguros acerca de un
objeto. El método científico ha de recorrerse siguiendo cuatro fases
[@cancelo_1991]: ($i$) inventario previo de los fenómenos o de los hechos
significativos no rutinarios; ($ii$) planteamiento de un tema problemático
que hace necesaria una explicación; ($iii$) ideación de conjeturas
tendentes a darla, y ($iv$) tratamiento de las diversas hipótesis hasta
que sólo una se mantenga.

Por tanto, aunque no hay acuerdo a la hora de dar una definición exacta
de ciencia, sí lo hay a la hora de aceptar el método
científico como el elemento que define la ciencia: "el método científico y
la finalidad a la cual se aplica (el conocimiento objetivo del mundo)
constituyen la entera diferencia que existe entre la ciencia y la
no-ciencia [...]. El método científico es un rasgo característico de la
ciencia, tanto de la pura como de la aplicada: donde no hay método
científico no hay ciencia", @Bunge_2004. "Una ciencia es una disciplina
que utiliza el método científico con la finalidad de hallar estructuras
generales (leyes)." @Montero_1997.

Dado que la utilización del método científico es la piedra angular alrededor  de la cual se articula el concepto de ciencia, a continuación se aborda la cuestión de si, en base a lo anterior, la ciencia de datos es o no una ciencia. No obstante, previamente, se abordará la cuestión de qué se entiende por ciencia de datos.



## ¿Qué es la ciencia de datos?

\index{Tukey} \index{ciencia de datos} \index{análisis!de datos}

La ciencia de datos es una disciplina emergente donde, a diferencia de
otros saberes como, por ejemplo, las ciencias matemáticas, el corpus o
la acumulación de conocimiento se ha generado en un lapso de tiempo
relativamente corto (y de una forma muy intensa), y no a lo largo de
siglos de historia. Su inicio data de la década de 1970, 
aunque ya el término **análisis de datos**, acuñado por J. Tukey en 1962 en su artículo
*The Future of Data Analysis* [@tukey_1962]
se puede considerar como un precedente del término **ciencia de datos**. 
En dicho artículo, Tukey definió, por primera vez, el análisis de datos como:
"procedimientos para analizar datos, técnicas
para interpretar los resultados de dichos procedimientos, formas de
planificar la recopilación de datos para hacer su análisis más fácil,
más preciso o acertado, y toda la maquinaria y los resultados de las
estadísticas matemáticas que se aplican al análisis de datos"
[@tukey_1962]. A partir de este momento, toda una serie de
acontecimientos fueron consolidando el término **ciencia de datos**
como una nueva disciplina. Una breve descripción de los acontecimientos
se muestran en la Fig. \@ref(fig:time-line-ds).

<div class="figure" style="text-align: center">
<img src="img/time-line-ds6.png" alt="Línea del tiempo de la ciencia de datos. Elaboración propia." width="100%" />
<p class="caption">(\#fig:time-line-ds)Línea del tiempo de la ciencia de datos. Elaboración propia.</p>
</div>

La ciencia de datos implica la limpieza, la agregación y la manipulación
de datos recabados de la web, de teléfonos inteligentes, de clientes,
de pacientes, de sensores o de encuestas, entre otras fuentes, para
llevar a cabo un análisis de datos avanzado de los mismos, 
así como su modelización, para ayudar a detectar patrones, 
tendencias, comportamientos y, por tanto, facilitar
la toma de decisiones. El crecimiento acelerado del volumen de fuentes
de datos y, posteriormente, de datos, ha hecho que la ciencia de datos
sea uno de los campos de más rápido desarrollo en todas las industrias.
Como resultado, no sorprende que surgiera la nueva profesión del
**científico de datos**, para ayudar a comprender y a analizar los volúmenes
masivos de datos que se acumulaban en ese momento, trabajo que fue
calificado como el "trabajo más sexy del siglo XXI" por 
@hbr_2012.

\index{estadística} \index{matemáticas} \index{informática}
\index{programación} 

La ciencia de datos es, por tanto, una disciplina
relativamente nueva que combina la estadística, las matemáticas, la
informática y la programación, para obtener valor de los datos. 

Se utiliza en una amplia variedad de campos, como la astronomía, la
medicina, la economía, el marketing, las finanzas, la biología, la
industria, etc. Esta naturaleza transdisciplinaria de la ciencia de
datos añade cierta complejidad a su caracterización pues, como se ha
avanzado, siendo una única disciplina, subsume en su ejercicio otras
disciplinas como las ciencias matemáticas y la estadística y la ciencia
de la computación, que a su vez son aplicadas a un amplio rango de
dominios de manera integral. La ciencia de datos se sirve de los métodos
formales de las matemáticas y de las aplicaciones prácticas e
ingenieriles de las ciencias de la computación para la generación de
conocimiento y para la resolución de problemas prácticos en múltiples
campos. Esta ubicuidad la sitúa, transversalmente, 
entre los saberes de primer orden. En otras palabras,
la ciencia de datos va adoptando los paradigmas, modelos, teorías o
constructos propios del campo sustantivo en el que se ejerce, de forma
que, para resolver alguna problemática sobre personas, puede recurrir
al corpus relativo de la psicología o de la sociología y, para
profundizar sobre alguna condición de salud, puede hacer lo propio con
la medicina o la biología, por mencionar algunos ejemplos.



## Lo científico de la ciencia de datos


En la Sec. \@ref(ciencia) se manifestó que un aspecto fundamental de
la ciencia es que utiliza el método científico con la finalidad de
hallar estructuras generales (principios y leyes) con capacidad
predictiva y comprobable (en el sentido amplio del término). Es por ello que el marco general de la
**metodología científica** ha sido bien fundamentado a lo largo de las
últimas décadas gracias a las contribuciones de diferentes teóricos de
la ciencia [@chalmers_2000; @Bunge_2004; @Diez_Moulines_1999]. Por otra
parte, las ciencias se clasifican, según el objeto de estudio [@Bunge_2018], 
en: empíricas y formales[^nota-bunge1]. Dado que la ciencia de datos
subsume diferentes disciplinas y se aplica a diferentes campos, puede
tener características tanto de las ciencias empíricas como de las
formales.

[^nota-bunge1]: Las ciencias empíricas emplean una aproximación
    hipotético-inductivo-deductiva y amplían el conocimiento. Las
    ciencias formales se fundamentan en la deducción y explicitan el
    saber, pero no lo amplían. Ejemplo de ciencia empírica es la
    biología, mientras que la lógica lo es de la ciencia formal.
    \index{ciencia!empírica} \index{ciencia!formal}

Si se analiza el conjunto de saberes científicos se aprecia que tienen
en común una serie de características [@Bunge_2018]. Por tanto, la
pregunta fundamental en este punto es: ¿comparte la ciencia de datos
estas características? De ser satisfechas, conferirían a la ciencia de
datos el estatuto de ciencia que comparten otros saberes científicos:

(i) La actividad científica es **metódica**. Es decir, utiliza un método, se caracteriza
    por proceder de manera ordenada y planificada. Esta estructuración
    le otorga solidez y consistencia. En ciencia de datos también se
    actúa de manera metódica, a través de diferentes metodologías, como
    *Knowledge Discovery in Databases* (KDD), *Sample, Explore, Modify,
    Model, Assess* (SEMMA) y *CRoss-Industry Standard Process for Data
    Mining* (CRISP-DM), tal y como se expone en el Cap.
    \@ref(metodologia).

(ii) El conocimiento científico **se fundamenta en hechos**. En general,
     los científicos disponen de diferentes instrumentos para observar y
     registrar la realidad sobre la que conjeturan. Esta labor también
     la realizan los científicos de datos, quienes cuentan con un
     elevado número de instrumentos y metodologías para la recolección
     de datos. Tal es el caso de los cuestionarios, escalas
     psicométricas o datos transaccionales producidos por diferentes
     tecnologías.

(iii) El saber científico implica que las
      afirmaciones científicas puedan ser **contrastadas** a través de los
      hechos. En ciencia de datos, esto también sucede, ya que, estadísticamente, los
      resultados a los que se llega no están ligados a la subjetividad
      del analista, sino a la objetividad de los datos y a las técnicas 
      estadísticas de contrastación.

(iv) La ciencia es una actividad que **trasciende los hechos**. Es
     decir, la ciencia parte de evidencias empíricas que tienden a ser
     superadas, puesto que la explotación de las mismas suele generar
     nuevas evidencias que, a su vez, pueden contribuir a crear nuevos
     marcos teóricos explicativos o a ampliar los existentes. La ciencia
     de datos puede ejercerse en el mismo sentido. Por ejemplo, la
     construcción de un recomendador, como Netflix, parte de ciertos
     datos, pero su uso genera nuevos *inputs* comportamentales que pueden
     ser empleados para optimizar su sustrato algorítmico.

(v) La investigación científica se caracteriza también por ser una
    **actividad analítica**. Es decir, tiende a descomponer los
    problemas en sus partes constitutivas. Cabe observar que la
    consecuencia de ello es que no se pueda hablar de una ciencia
    general, sino de especializaciones. Naturalmente, la especialización
    también existe en esta disciplina; por eso, cuando la ciencia de
    datos se aplica intensivamente en recursos humanos, por ejemplo, es
    posible hablar de *Human Resource Analytics*. Lo mismo ocurre en
    Economía, con el *Business Analytics*, y así en un sinfín de
    disciplinas.

(vi) La ciencia es **comunicable** y, para ello, se sirve de sistemas
     representacionales lógico-formales. Este atributo también se
     aprecia en la ciencia de datos, puesto que los resultados tienden a
     ser compartidos a través de diferentes estrategias, entre ellas, la
     visualización de datos.

La ciencia, sin embargo, no solo puede describirse mediante sus
características constitutivas, sino también **funcionalmente** [@Hempel_2005]. 
De hecho, las características anteriormente citadas son
las que posibilitan las funciones **descriptiva**, **explicativa** y
**predictiva**.

(i)   La primera, la **descriptiva**, permite recabar información sobre el
    suceso que se analiza para tratar de conocerlo en mayor profundidad
    y detalle. En ciencia de datos, usualmente, una de las primeras
    tareas consiste en describir el conjunto de datos para conocer en
    detalle sus características, es decir, el número de variables, el
    número de observaciones, los valores nulos, etc. Esta tarea se
    conoce como "comprensión de los datos" en la metodología
    CRIPS-DM (véase Sec. \@ref(met-crisp-dm)).

(ii)   La segunda, la **explicativa**, determina cómo se relacionan los
    fenómenos que se observan. En general, cuando un científico de
    datos emplea un modelo de regresión lineal, lo que hace es
    establecer una relación explicativa entre la variable dependiente y
    las independientes. Esta parte se
    conoce como "modelado" en la metodología CRIPS-DM
    (véase Sec. \@ref(met-crisp-dm)).

(iii)   La tercera, la **predictiva**, permite anticipar ciertos eventos en
    el tiempo o en el espacio. Tal es el caso de los científicos de
    datos que ejercen su labor en el ámbito comercial y emplean, por
    ejemplo, el análisis de series temporales para pronosticar las
    ventas futuras y poder realizar una planificación del
    aprovisionamiento de existencias con mayor eficiencia. Esta parte
    está incluida en la fase de "validación" en la metodología CRIPS-DM
    (véase Sec. \@ref(met-crisp-dm)).

A la luz de lo expuesto hasta aquí, se puede sostener, sin lugar a dudas,
que la ciencia de datos emplea el método científico y comparte las
principales funciones de la ciencia. Ahora bien, la ciencia de datos no
puede entenderse plenamente sin presuponer las disciplinas en las que se
aplica. Por tanto, uno de los interrogantes que deberán resolver los
futuros profesionales es si la ciencia de datos es un saber de primer
orden, que lidia directamente con la realidad, como la física o la
química, o si, por el contrario, es un saber de segundo orden, es decir,
una suerte de disciplina que se sirve de otros saberes para desplegarlos
y actualizarlos.




::: {.infobox_resume data-latex=""}
### Resumen {-}

+ Para determinar si la ciencia de datos es, realmente, una ciencia, en
primer lugar se debe consensuar la definición de ciencia, que va
íntimamente ligada a la definición de método científico.

+ Las ciencias tienen en común una serie de características, que deben ser satisfechas
por la ciencia de datos para adquirir el estatus de ciencia.

+ Dado que la ciencia de datos emplea el método científico y comparte las
principales funciones de la ciencia, se concluye que la ciencia de datos
es una ciencia.
:::
