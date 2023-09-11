---
title: "Fundamentos de ciencia de datos con **R**"
author: "Gema Fernández-Avilés y José-María Montero"
date: "2023-09-11"
output:
  html_document:
    df_print: paged
documentclass: book
bibliography: ["cdr-bilio.bib" ]
cover-image: 'img/cover.png'
biblio-style: apalike
description: |
  Ciencia de datos con R.
link-citations: yes
urlcolor: blue
linkcolor: red
nocite: '@*'
site: bookdown::bookdown_site
---




# Prefacio {-}



## ¡Hola, mundo! {-}  


<a><img src="img/cover.png" width="300" height="400" class="cover" /></a>

<!-- <img src="img/cover.png" class="cover" width="250" height="328"/> -->

El siglo XXI está siendo testigo de grandes cambios vertiginosos en el contexto social y tecnológico, entre otros. Los tiempos han cambiado, la sociedad se ha globalizado y "exige" respuestas inmediatas a problemas muy complejos. Vivimos en el mundo de la **información**, de los **datos**, o mejor, de las **bases de datos masivas**, y los ciudadanos y, sobre todo, las empresas y los gobiernos, dirigen su mirada hacia el mundo científico para que les ayude a "**oír las historias**" que cuentan esos datos acerca de la realidad de la que han sido extraídos. Y dado su enorme volumen y sofisticación (en el nuevo mundo las imágenes y los textos, por ejemplo, también son datos), exigen algoritmos de nueva generación en el campo del *machine learning*, o incluso del *deep learning*, para "oír las historias" que cuentan. No parecen mirar al "antiguo" investigador científico, sino al "nuevo" **científico de datos**.

Ello, inevitablemente, se traduce en la necesidad de profesionales con una gran capacidad de adaptación a este nuevo paradigma: los científicos de datos, también llamados por algunos los "nuevos hombres del Renacimiento", para lo cual las universidades y demás instituciones educativas especializadas se apresuran a incluir el grado de Ciencia de Datos en su oferta educativa y a ofrecer seminarios de software estadístico de acceso abierto para sus estudiantes de primeros cursos.

Con la emergencia de la nueva sociedad, en la que el manejo de la ingente cantidad de información que genera se hace absolutamente necesario para circular por ella, la **ciencia de datos** ha venido para quedarse. Sin embargo, el mundo de la ciencia de datos es cualquier cosa menos sencillo. En él, cualquier ayuda, cualquier guía es bienvenida. Por ello, es muy recomendable que la persona que se quiera introducir en él, sea con fines de investigación o con fines profesionales, se agarre de la mano de un guía especializado que le lleve, de una manera amena, comprensible y eficiente, desde el planteamiento de su problema y la captura de la información necesaria para poderle dar una solución, hasta la redacción de las conclusiones finales que ha obtenido con los modernos informes reproducibles colaborativos. Y como en la parte central de ese camino tendrá que luchar con grandes gigantes (en la actualidad denominados técnicas estadísticas y algoritmos), el guía tendrá que explicarle, de manera sencilla y amena, en qué consiste la lucha (las técnicas y los algoritmos) y cómo llegar a la victoria lo más rápido posible, enseñándole a moverse por el mundo del software estadístico, en nuestro caso **R**, que le permitirá realizar los cálculos necesarios para vencer al problema planteado a una velocidad vertiginosa.

En resumen, la información masiva y el moderno tratamiento estadístico de la misma son la "mano invisible" que gobierna la sociedad del siglo XXI, y este manual pretende ser ese guía que le llevará de la mano cuando quiera caminar por ella.


## ¿Por qué este libro? {-}

Lo dicho anteriormente ya justifica por sí solo la aparición de este manual. Afortunadamente, no es el primero en la materia, pues son ya bastantes los materiales de calidad publicados sobre ciencia de datos. Sin embargo, quizás, este pueda ser considerado el más completo. Y ello por varias razones.

La primera es su **completitud**: este manual lleva de la mano al lector desde el planteamiento del problema hasta el informe que contiene la solución al mismo; o desde no saber qué hacer con la información de la que dispone, hasta ser capaz de transformar tales bases de datos masivas, y casi imposibles de manejar, en respuestas a problemas fundamentales de una empresa, institución o cualquier agente social.

La segunda es su **amplitud temática**:

(i)	Parte de las dos primeras preguntas que un neófito se puede hacer sobre esta temática: ¿qué es eso de la ciencia de datos que está en boca de todos? Y, ¿qué diablos es **R** y cómo funciona?

(ii)	Enseña cómo moverse en la jungla del *big data* y de los "nuevos" tipos de datos, siempre bajo el paraguas de la ética de los datos y del buen gobierno de dichos datos.

(iii)	Muestra al lector cómo obtener conocimiento de la oscuridad del enorme banco de información a su disposición, que no sabe cómo abordar ni manejar.

(iv)	No deja a nadie atrás, y de forma previa al contenido central del manual (las técnicas de ciencia de datos), incluye unas breves, pero magníficas, secciones sobre los rudimentos de la probabilidad, la inferencia estadística y el muestreo, para aquellos no familiarizados con estas cuestiones.

(v)	Aborda una treintena de técnicas de ciencia de datos en el ámbito de la modelización, análisis de datos cualitativos, discriminación, *machine learning* supervisado y no supervisado, con especial incidencia en las tareas de clasificación y clusterización --así como, en el caso no supervisado, de reducción de la dimensionalidad, escalamiento multidimensional y análisis de correspondencias--, *deep learning*, análisis de datos textuales y de redes, y, finalmente, ciencia de datos espaciales (desde las perspectivas de la geoestadística, la econometría espacial y los procesos de punto).

(vi)	Hace especial hincapié en la reproducibilidad en tiempo real (o no) entre los distintos miembros de un equipo (sea universitario, empresarial o de otro tipo) y en la difusión de los resultados obtenidos, enseñando al lector cómo generar informes reproducibles mediante RMarkdown y documentos Quarto o en otros modernos formatos.

(vii)	Dedica un capítulo a la creación de aplicaciones web interactivas (con Shiny).

(viii)	Para aquellos con pasión por la codificación, y que quieran compartir código y colaborar con otros desarrolladores, este manual aborda la gestión rápida y eficaz de proyectos (del tamaño que sean) mediante Git, un sistema de control de versiones distribuido, gratuito y de código abierto, y GitHub, un servicio de alojamiento de repositorios Git del cual, aquellos no familiarizados con la cuestión de la codificación, o con aversión a ella, podrán tomar el código que necesitan.

(ix)	Muestra al lector los primeros pasos para iniciarse en el geoprocesamiento en la nube.

(x)	Y, finalmente, aborda más de una docena de casos de uso (en medicina, periodismo, economía, criminología, marketing, moda, demanda de electricidad, cambio climático, reconocimiento de patrones en la forma de tuitear...) que ilustran la puesta en práctica de todos los conocimientos anteriormente adquiridos.

La tercera razón es que todo lo que el lector aprende en este manual lo puede reproducir y poner en práctica inmediatamente con **R**, puesto que el manual está lleno de *chunks* (o trozos de código **R**) que no tiene más que cortar y pegar para reproducir los ejemplos que se muestran en el libro, cuyos datos están en el paquete `CDR`; o utilizar dichas *chunks* para abordar el problema que le ocupa con los datos que tenga a su disposición. Una buena razón, sin duda. Por consiguiente, el manual es una buena combinación "teoría-práctica-software" que permite abordar cualquier problema que el científico de datos se plantee en cualquier disciplina o situación empresarial, médica, periodística...

La cuarta es su **variedad de perspectivas**. Son **más de 40 los participantes** en este manual. Algunos de ellos, prestigiosos profesores universitarios; otros, destacados miembros de instituciones públicas; otros, CEO de empresas en la órbita de la ciencia de datos; otros, *big names* del mundo de **R** software... El manual es, sin duda, un magnífico ejemplo de colaboración Universidad-Empresa para buscar soluciones a los problemas de las sociedades modernas.


## ¿A quién va dirigido? {-}

*Fundamentos de ciencia de datos con* ***R*** está dirigido a todos aquellos que desean desarrollar las habilidades necesarias para abordar proyectos complejos de ciencia de datos y "pensar con datos" (como lo acuñó Diane Lambert, de Google). El deseo de resolver problemas utilizando datos es su piedra angular. Por tanto, como se avanzó anteriormente, este manual no deja a nadie atrás, y lo único que requiere es "el deseo de resolver problemas utilizando datos". No excluye ninguna disciplina, no excluye a las personas que no tengan un elevado nivel de análisis estadístico de datos, no excluye a nadie. Se ha procurado una combinación de rigor y sencillez, y de teoría y práctica, todo ello con sus correspondientes códigos en **R**, que satisfaga tanto a los más exigentes como a los principiantes.

También está destinado a todos aquellos que quieran sustituir la navegación por la web (la búsqueda del vídeo, publicación de blog o tutorial *online* que solucione su problema --frustración tras frustración por la falta de consistencia, rigor e integridad de dichos materiales, así como por su sesgo hacia paquetes singulares para la implementación de las cuestiones que tratan--), por una "**biblia de la ciencia de datos**" rigurosa pero sencilla, práctica y de aplicación inmediata sin ser ni un experto estadístico ni un experto informático.

Pero si a alguien está destinado especialmente, es a la comunidad hispanohablante. Este manual es un guiño a dicha comunidad, para que tenga a su disposición, en su lengua nativa, uno de los mejores manuales de diencia de datos de la actualidad.


## El paquete `CDR` {-}

<img src="img/LogoCDR_transparente.png" width="25%" style="display: block; margin: auto;" />

El paquete `CDR` contiene la mayoría de conjuntos de datos utilizados en este libro que no están disponibles en otros paquetes. Para instalarlo use la función `install_github()` del paquete `remotes`.


```r
# este comando solo necesita ser ejecutado una vez
# si el paquete remotes no está instalado, descomentar para instalarlo

# install.packages("remotes")
remotes::install_github("cdr-book/CDR")
```

La lista de todos los conjuntos de datos puede obtenerse haciendo `data()`.

```r
library('CDR')
data(package = "CDR")
```

Este paquete ayudará al lector a reproducir todos los ejemplos del libro. De acuerdo con las mejores prácticas en **R**, el paquete `CDR` solo contiene los datos utilizados en el libro.



## ¿Por qué **R**? {-}

**R** es un lenguaje de código abierto para computación estadística que se ha consolidado entre la comunidad científica internacional, en las últimas dos décadas, como una herramienta de primer nivel, estableciéndose como líder permanente en el ámbito de la implementación de metodologías estadísticas para el análisis de datos. La utilidad de **R** para la ciencia de datos deriva de un fantástico ecosistema de paquetes (activo y en crecimiento), así como de un buen elenco de otros excelentes recursos: libros, manuales, blogs, foros y *chats* interactivos en las redes sociales, y una gran comunidad dispuesta a colaborar, a orientar y a resolver diferentes cuestiones relacionadas con **R**.

Por otra parte, **R** es el lenguaje estadístico y de análisis de datos más utilizado en la mayoría de los entornos académicos y, cómo no, por una larga lista de importantes empresas, entre las que se cuentan Facebook (análisis de patrones de comportamientos relacionado con actualizaciones de estado e imágenes de perfil), Google (para la efectividad de la publicidad y la previsión económica), Twitter (visualización de datos y agrupación semántica), Microsoft (adquirió la empresa Revolution R), Uber (análisis estadístico), Airbnb (ciencia de datos), IBM (se unió al grupo del consorcio R), New York Times (visualización)...

La comunidad **R** también es particularmente generosa e inclusiva, y hay grupos increíbles, como *R-Ladies* y *Minority R Users*, diseñados para ayudar a garantizar que todos aprendan y usen las capacidades de **R**.


## Agradecimientos {-}

No queremos dar por finalizado este prefacio sin agradecer a los 44 autores participantes en esta obra su esfuerzo por condensar, en no más de 20 páginas, la teoría, práctica y tratamiento informático de la parte de la ciencia de datos que les fue encargada. Y no solo eso; el "más difícil todavía" fue que debían dirigirse a un abanico de potenciales lectores tan grande como personas haya con "el deseo de resolver problemas utilizando datos". Era misión imposible. Sin embargo, a la vista del resultado, ha sido misión cumplida. El esfuerzo mereció la pena.

Además, nos gustaría agradecer el apoyo incondicional recibido por (en orden alfabético): Itzcoátl Bueno, Ismael Caballero,  Emilio L. Cano, Diego Hernangómez, Michal M. Kinel, Ricardo Pérez, Manuel Vargas y Jorge Velasco.

También queremos poner de manifiesto que la edición de este texto ha sido financiada por diversos entes de la Universidad de Castilla-La Mancha. En su mayor parte, por el [**Máster en *Data Science* y *Business Analytics* (con R software)**](https://blog.uclm.es/tp-mbsba/) (a través de la orgánica: 02040M0280), pero también por la Facultad de Ciencias Jurídicas y Sociales de Toledo (a través de su contrato programa: orgánica 00440710), el Departamento de Economía Aplicada I (mediante sus fondos departamentales, DEAI 00421I126) y el Grupo de Investigación Economía Aplicada y Métodos Cuantitativos (que ha dedicado parte de sus fondos a la edición de esta obra, orgánica 01110G3044-2023-GRIN-34336).

A todos, eternamente agradecidos por ayudarnos en este reto de transformar la oscuridad en conocimiento, de convertir en una ciencia y en un arte la difícil tarea de sacar valor de los datos, el petróleo del futuro. Quizás en este momento no seamos conscientes de que hemos puesto nuestro granito de arena a la ciencia que, a buen seguro, juegue uno de los papeles más importantes de este siglo, caracterizado por el predominio de la información. Una ciencia, la ciencia de datos, que combina el análisis estadístico de datos, la algoritmia y el conocimiento del negocio para sacar valor del bien más abundante de la sociedad en la que vivimos: la información. Una disciplina cuyo dominio caracteriza a los científicos de datos (también denominados los nuevos personajes del Renacimiento), profesión que ya fue calificada hace más de veinte años en la *Harvard Business Review* y en *The New York Times*, entre otros, como la "más *sexy* del siglo XXI".


## Información del software {-}

Se ha dejado a disposición del lector una versión *online* de este manual en el siguiente enlace [https://cdr-book.github.io/](https://cdr-book.github.io/) para facilitar la reproducibilidad del del libro, concretamente los cuadros de código **R**. Además, se ha preparado un repositorio en GitHub [https://github.com/cdr-book/cdr-book.github.io](https://cdr-book.github.io/) con información sobre las versiones exactas del software **R** y los paquetes utilizados para garantizar la completa reproducibilidad del los ejercios del manual.


::: {.infobox_resume data-latex=""}
### Este manual está publicado por [McGraw Hill](https://www.mheducation.com/). Las copias físicas están disponibles en [McGraw Hill](https://www.mheducation.com/). La versión *online* se puede leer de forma gratuita en [https://cdr-book.github.io/](https://cdr-book.github.io/) y tiene la [licencia de Creative Commons Reconocimiento-NoComercial-SinObraDerivada 4.0 Internacional](https://creativecommons.org/licenses/by-nc-nd/4.0/). Si tiene algún comentario o sugerencia, no dude en contactar con los editores y los autores. ¡Gracias! {-} 
:::

