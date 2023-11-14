
# Gobierno, gestión y calidad del dato {#DGDQM}

*Ismael Caballero*$^{a}$, *Ricardo Pérez del Castillo*$^{a}$, y *Fernando Gualo*$^{a,b}$

$^{a}$Universidad de Castilla-La Mancha  
$^{b}$DQTeam SL 

## Introducción
\index{gobierno del dato} \index{gestión de calidad del dato} \index{gestión del dato}

Los datos se han convertido en un elemento vital para el desarrollo económico de las organizaciones, ya que permiten una mayor eficiencia en el uso de los recursos y un aumento de su productividad.
Tanto es así, que la Unión Europea establece, a través de la [Estrategia Europea de Datos](https://ec.europa.eu/info/strategy/priorities-2019-2024/europe-fit-digital-age/european-data-strategy_es),[^DG1] que en 2030 se establecerá un Espacio Único Europeo de Datos para fomentar un ecosistema con nuevos productos y servicios basados en los datos.
Para ello, en esta Estrategia Europea de Datos --que prevé un incremento del 530% del volumen global de datos-- se reclama la necesidad de implantar **mecanismos de gobierno del dato** a través de políticas y directrices consensuadas a alto nivel para alcanzar los objetivos de la **estrategia organizacional** y satisfacer tanto aspectos regulatorios genéricos (por ejemplo, las leyes europeas [_General Data Protection Regulacy_ --GPDR--](https://eur-lex.europa.eu/legal-content/ES/TXT/PDF/?uri=CELEX:32016R0679&from=EN)[^DG2] o [_Data Governance Act_](https://eur-lex.europa.eu/legal-content/ES/TXT/PDF/?uri=CELEX:52020PC0767&from=EN),[^DG3] o las españolas [Esquema Nacional de Seguridad --ENS--](https://www.boe.es/buscar/act.php?id=BOE-A-2022-7191),[^DG4] o [Esquema Nacional de Interoperabilidad --ENI--[^DG5])](https://www.boe.es/buscar/doc.php?id=BOE-A-2010-1331) como aspectos sectoriales específicos (como [Solvencia II](https://www.eiopa.europa.eu/browse/solvency-2_en)[^DG6] para el sector seguros o Basilea III[^DG7] para el sector financiero).

[^DG1]: <https://ec.europa.eu/info/strategy/priorities-2019-2024/europe-fit-digital-age/european-data-strategy_es>
[^DG2]: GPDR: <https://eur-lex.europa.eu/legal-content/ES/TXT/PDF/?uri=CELEX:32016R0679&from=EN>
[^DG3]: Data Governance Act:<https://eur-lex.europa.eu/legal-content/ES/TXT/PDF/?uri=CELEX:52020PC0767&from=EN>
[^DG4]: ENS: <https://www.boe.es/buscar/act.php?id=BOE-A-2022-7191>
[^DG5]: ENI: <https://www.boe.es/buscar/doc.php?id=BOE-A-2010-1331>
[^DG6]: Solvencia II: <https://www.eiopa.europa.eu/browse/solvency-2_en>
[^DG7]: Basilea III: <https://www.bis.org/bcbs/basel3.htm>

Estos mecanismos de gobierno del dato deben abordar aspectos verticales relacionados con la adquisición, tenencia, compartición, uso y explotación de los datos en los procesos de negocio, abordando a la vez aspectos transversales relacionados con su gestión: calidad de los datos, aspectos éticos y privacidad, interoperabilidad, gestión del conocimiento y el control sobre los activos de datos a través de las políticas correspondientes, y despliegue de estructuras organizativas con una conveniente separación de los roles de gobierno del dato de los de gestión del dato [@ISO2017].
Por tanto, puede decirse que el **gobierno del dato** marca la dirección de cómo la organización debe realizar la **gestión del dato** para alcanzar los objetivos establecidos en su(s) estrategia(s) del dato. Esto se consigue mediante la definición e implementación de una serie de políticas del dato.


## Concepto de gobierno del dato

El gobierno de los datos se ha convertido en un habilitador de la economía de los datos [@Engels2019; @Weber2009], así como también en un pilar básico para la mejora de la transparencia y eficiencia de las administraciones públicas [@OECD2019; @Osimo2020; @OsorioSanabria2020]. Aunque existen algunas aproximaciones académicas y profesionales al gobierno del dato, no hay una definición consensuada de este concepto que permita aunar las distintas visiones. No obstante, la definición más aceptada de gobierno del dato es la propuesta por DAMA en DMBoK2 [@DAMA2017]: "colección de prácticas y procesos que ayudan a asegurar la gestión formal de los activos de datos dentro de una organización mediante el ejercicio de autoridad, control y toma de decisiones compartidas, planificadas, monitorizadas y forzadas".
Teniendo en cuenta los matices que introduce, es también interesante la lectura de la propuesta por @soares2015chief, que define **gobierno del dato** como "la formulación de políticas para optimizar, conseguir los niveles adecuados de seguridad y protección, y potenciar los datos como activos organizacionales mediante la alineación de los objetivos de diferentes funciones organizacionales; por su naturaleza, el gobierno del dato requiere cooperación interdepartamental para entregar oportuna y fielmente datos con el máximo valor para la toma de decisiones en la organización".

De alguna manera, se podría entender que gobernar los datos implica **el diseño, implementación y mantenimiento de un sistema de gobierno del dato**. El gobierno del dato tiene tres características destacables [@Caballeroetal2022]:

-   **Está dirigido por el valor de los datos**: pues el principal objetivo del gobierno del dato es asegurar que los datos son tratados como activos de datos y que la gestión y uso que se hace de ellos permita alcanzar el máximo valor organizacional que se espera de ellos. Por tanto, todas las acciones están encaminadas a la obtención de este valor organizacional.
-   **Está centrado en la arquitectura empresarial**: para poder gobernar los datos adecuadamente, es preciso revisar o incorporar ciertos componentes a la arquitectura empresarial tales que, o bien den el soporte adecuado, o bien formen parte del resultado del gobierno del dato.
-   **Es iterativo e incremental**, pues para alcanzar un estado en el que se considere que los activos de datos están perfectamente gobernados es preciso desarrollar un conjunto de programas de gobierno del dato. Así, a través de la ejecución de diferentes proyectos relacionados entre sí, se conseguirá el desarrollo y la puesta en valor de los artefactos típicos de un sistema de gobierno del dato (véase Sec. \@ref(artefactosDG)). Esto solo se puede conseguir en incrementos relevantes (por ejemplo, la creación de más componentes del sistema de gobierno del dato o la inclusión de nuevos datos a ser gobernados en el alcance de gobierno del dato).

Un aspecto interesante es que, a medida que se avanza en la ejecución de estos programas de gobierno del dato, más sensible se vuelve la organización hacia la importancia de los datos, más aprende a gestionarlos y gobernarlos y más amplio es el alcance del gobierno del dato; en definitiva, se puede decir que más madura se vuelve la organización en lo que se refiere al gobierno y a la gestión del dato.

### Beneficios del gobierno del dato {#beneficiosDG}

Cuando se desarrolla un sistema de gobierno del dato, con cada incremento del sistema se espera conseguir uno de los siguientes beneficios o una combinación de los mismos [@ISACA2019]:

1.  **Alineamiento estratégico**: optimización del valor organizacional de los datos mediante el alineamiento con la estrategia organizacional.

2. **Realización de beneficios**: aseguramiento de que los datos son entregados en condiciones aceptables a los diferentes consumidores del dato.

3.  **Optimización de riesgos**: paliar o minimizar, dentro de la propensión al riesgo de la organización, los riesgos relacionados con la adquisición, uso y explotación de los datos, asegurando el cumplimiento de la normativa interna y regulatoria.

4.  **Optimización de recursos**: optimización de las capacidades de los recursos humanos y tecnológicos necesarios y que se utilizan para dar un soporte más eficiente a las distintas operaciones involucradas en la gestión del dato, minimizando el desperdicio de recursos al gestionar, usar y explotar los datos.

Estos beneficios deben especificarse como parte de la estrategia del dato de la organización.
Así, por ejemplo, una organización que considere realizar un alineamiento estratégico y una optimización de riesgos estará desarrollando una **estrategia defensiva** que debería implementarse a través de un **gobierno técnico**; por otro lado, si una organización quiere, por ejemplo, maximizar la realización de beneficios, podría considerarse que estaría trazando una **estrategia ofensiva** que se podría materializar mediante un **gobierno para el valor**.

### Componentes de un sistema de gobierno del dato {#artefactosDG}

\index{sistema de gobierno del dato}
Para poder obtener los beneficios descritos anteriormente, las organizaciones deben implantar los mecanismos de gobierno del dato propuestos en la Estrategia Europea de Datos, particularizándolos a su realidad y en función de su madurez.
Estos mecanismos implican el desarrollo de un **sistema de gobierno del dato**, que involucra la creación o mantenimiento de forma interrelacionada y sujeto a las restricciones correspondientes de una serie de componentes.
Dependiendo de si se tiene un gobierno técnico o un gobierno para el valor, la creación y uso de los distintos tipos de componentes será más o menos intensiva.
Estos componentes son los siguientes[@Caballeroetal2022]:

1.  **Procesos de gestión del dato, gestión de calidad del dato y gobierno del dato**, que se refieren al diseño y posterior particularización e implantación de las buenas prácticas relacionadas con las tareas típicas de los datos a nivel de las correspondientes disciplinas. Es posible obtener descripciones genéricas de estos procesos en diferentes modelos de referencia de procesos, tales como **Data Maturity Model (DMM)**[@mecca_data_2014], **The Data Management Capability Assessment Model (DCAM)**[@Council2020] o el **Modelo Alarcos de Madurez de Datos (MAMD)**[@MAMD2023].

2.  **Estructuras organizacionales**, que deben recoger las cadenas de responsabilidades y rendición de cuentas, haciendo una adecuada separación entre las responsabilidades propias del gobierno del dato y aquellas propias de la gestión del dato y de su calidad. Los roles que deben asumir estas responsabilidades son el de *chief data officer* [@soares2015chief; @Treder2020], desde un punto de vista más ejecutivo/estratégico, y los de *data stewards* [@plotkin2020data], desde una perspectiva más táctica/operativa.
\index{política del dato}

3.  **Principios, políticas y marcos de referencia**, que deberían incluir todos los principios rectores en los que se basará el uso de los datos [tales como los *generaly accepted information principles* listados en @ladley2019], las directrices o políticas y los controles correspondientes asociados necesarios para modelar y gestionar el valor de los datos, el riesgo a asumir y las restricciones a considerar según se describe en ISO/IEC 38505-2 [@ISO2018].  \index{metadato}

4. **Datos e información**, que se deben gobernar como las descripciones necesarias a través de los metadatos correspondientes. Para la parte del dato es fundamental poder establecer una adecuada \index{arquitectura del dato} **arquitectura del dato** con los correspondientes modelos que recojan la semántica del entorno de la organización y reflejen cómo esta usa los datos para desarrollar su actividad organizacional y/o económica. Para dar soporte al uso correspondiente deben generarse y mantenerse los metadatos correspondientes, que pueden ser de varios tipos [@DAMA2017]:

    + **metadatos de negocio**, normalmente recogidos en el **glosario de negocio** y que describen la relación del dato con el negocio;
    + **metadatos técnicos**, recogidos habitualmente en los **catálogos de datos**, que describen detalles técnicos de los datos; y
    + **metadatos operacionales**, que suelen incorporarse en los **diccionarios de datos**  y que recogen aspectos relacionados con el procesamiento y acceso a los datos. Es importante que todos estos metadatos estén reconciliados convenientemente entre ellos, ya que su visión conjunta permitirá una descripción adecuada de los datos bajo el gobierno del dato; y si esta descripción es suficiente será posible usarlos con garantía de éxito.

\index{datos!catálogos}
\index{datos!diccionarios}
\index{glosario de negocio}

5.  **Cultura, ética y comportamiento**, \index{cultura del dato} cuyo objetivo es identificar aquellos aspectos culturales y éticos que deben regular la forma en la que la organización abordará las tareas relacionadas con los datos para que estos tengan el valor organizacional deseado [@harrison_data_2019].

6.  **Personas, habilidades y competencias**, componente que trata de organizar los roles que deben asumir las diferentes responsabilidades relacionadas con los diferentes procesos; también debe enfocarse en asegurar que esos roles tienen los conocimientos, habilidades y competencias necesarias para abordar las tareas asociadas mediante los programas formativos correspondientes; finalmente, este artefacto incluye asegurar que la organización tiene planes de contingencia ante la eventual rotación funcional de los recursos humanos dedicados a las responsabilidades relacionadas con los datos [@plotkin2020data].

7.  **Servicios, infraestructuras y aplicaciones**: este componente aborda todo lo relacionado con las tecnologías y sistemas de información para dar soporte a las diferentes actividades de los procesos de gestión del dato, así como de la gestión de su calidad y de su gobierno.




## Marcos y metodologías de gobierno del dato

En la literatura, tanto académica como profesional, existen algunas propuestas de creación de sistemas de gobierno del dato.
Es interesante resaltar que en el ámbito académico se han desarrollado algunas revisiones sistemáticas de literatura científica para identificar los componentes del gobierno del dato, aunque de forma general, y salvo algunas referencias, se mantienen desconectados de las propuestas profesionales.
También es importante mencionar que salvo COBIT 2019 (_Control Objectives for Information and Related Technology_, objetivos de control para la información y tecnologías relacionadas),[^DG8] la inmensa mayoría de estos marcos no identifican explícitamente el concepto de **sistema de gobierno del dato**, sino que se establece bajo un paraguas más genérico de **gobierno del dato**. En cualquier caso, la idea es la misma.

[^DG8]: Véase <https://www.isaca.org/resources/cobit>

En los siguientes párrafos se resumen los aspectos más importantes de los marcos más relevantes, que pueden servir para que el lector encuentre el que mejor se adapta a su circunstancia profesional:

-   @Abraham2019 analizan la literatura para identificar los elementos de un marco de trabajo teórico, que se clasifican en torno a cuatro áreas del gobierno del dato: (1) alcance organizacional (aspectos intra e interorganizacionales), (2) alcance de los datos (datos tradicionales *vs. big data*), (3) alcance del dominio (calidad del dato, seguridad del dato, arquitectura del dato, ciclo de vida, metadatos, almacenamiento e infraestructura del dato) y (4) mecanismos de gobierno (estructurales, procedimentales y relacionales).

-   @AlRuithe2019 identifican, también a través de una revisión sistemática de literatura, las áreas o retos del gobierno del dato donde merece la pena investigar. Estas son tecnología (seguridad, privacidad, disponibilidad, rendimiento, clasificación de los datos y migración del dato), legalidad, y aspectos organizacionales o del negocio.

-   @Brous2016 derivan, de nuevo basándose en una revisión sistemática de la literatura, los principios para desarrollar de forma efectiva estrategias y aproximaciones para el gobierno del dato, que agrupan en torno a cuatro conceptos fundamentales: (1) organización, (2) alineamiento, (3) cumplimiento y (4) entendimiento común de los datos.

-   @carruthers2020chief identifican los posibles elementos que deben contemplarse en la transformación digital, la cual debe apoyarse en el gobierno del dato. Estos elementos (personas, datos, procesos, tecnologías) son representados mediante un triángulo en cuyo centro están los datos. En la continuación de este trabajo [@Jackson2019], se propone un modelo de transformación convenientemente soportado en el gobierno del dato.

-   \index{DCAM} DCAM (_data management capability assessment model_) [@Council2020] es un modelo de referencia para la evaluación de la capacidad de gestión del dato desarrollado por el EDM Council. El modelo tiene ocho componentes agrupados en cuatro niveles: (1) fundamentos (estrategia del dato y casos de negocio; programas de gestión del dato y financiación), (2) ejecución (arquitectura del dato y de negocio; arquitectura del dato y de tecnología; gestión de calidad del dato; gobierno del dato), (3) colaboración (entorno de control del dato) y (4) formalización del diseño e implementación de las actividades analíticas. \index{DMBoK}\index{DAMA}

-   DMBoKv2 (_data management body of knowledge_) [@DAMA2017] es un marco de referencia de procesos desarrollado por DAMA que posiciona el gobierno del dato como la función que guía el resto de las acciones relacionadas con la gestión del dato. Identifica una serie de elementos que deben generarse a partir del gobierno del dato: estrategia de gobierno del dato; estrategia del dato; hoja de ruta del gobierno del dato; principios de gobierno del dato, políticas de gobierno del dato; procesos; marco operativo de gobierno del dato; hoja de ruta y guía de implementación; plan de operaciones; glosario de términos; plan de operaciones; cuadro de mando de gobierno del dato; etc.

-   @Eryurek2021 identifica los "ingredientes" propios de un sistema de gobierno del dato (herramientas; personas y procesos; cultura del dato), así como las áreas en las que debería enfocarse el gobierno de datos a lo largo del ciclo de vida de los datos (descubrimiento y limpieza del dato; gestión del dato; políticas de privacidad, seguridad y acceso). \index{limpieza del dato}

-   COBIT 2019 [@ISACA2019] identifica  los siguientes componentes en el sistema de gobierno de tecnologías y de información: procesos; estructuras organizacionales; principios, políticas y marcos de referencia; información; cultura, ética y comportamiento; personas, habilidades y competencias; y servicios, infraestructuras y aplicaciones.

-   ISO 38505-1 [@ISO2017] e ISO 38505-2 [@ISO2018] muestran los aspectos clave del gobierno del dato (valor de los datos, riesgo y restricciones) e introducen seis principios (responsabilidad, estrategia, adquisición, rendimiento, cumplimiento y comportamiento humano). Identifican una serie de procesos (evaluar, dirigir, monitorizar) como áreas propias de actuación del gobierno del dato, distinguiéndolos de las operaciones propias de la gestión del dato y estableciendo las correspondientes relaciones con ellas. Sin embargo, no describen actividades específicas para la creación de sistemas de gobierno del dato.

-   @Khatri2010 aducen que el gobierno del dato implica tomar decisiones sobre activos claves de datos en varios dominios de decisión (principios, gestión de calidad del dato, metadatos, acceso a datos y ciclo de vida de los mismos).

-   @janssen2020data exploran las capacidades de gobierno del dato necesarias para que las organizaciones dirigidas por datos puedan extraer el máximo beneficio de los sistemas algorítmicos basados en *big data* (*big data algorithmic systems*) y proponen un marco para la creación del gobierno del dato que permita optimizar estos sistemas.

-   @ladley2019 presenta un marco de gobierno del dato basado en cinco pilares: compromiso, estrategia, arquitectura y diseño, implementación y operación, y, por último, gestión del cambio.

-   @Lillie2019 estudian la literatura existente para identificar los aspectos más interesantes sobre (1) el alcance y los constructos más importantes del gobierno y gestión del dato, y (2) las capacidades ágiles requeridas en el gobierno y la gestión de datos.

-   En el llamado **proceso Unificado de Gobierno del dato** de IBM [@soares2010ibm] se identifican cinco **ingredientes clave** que deberían ser cubiertos por cualquier marco de gobierno del dato: (1) fuerte respaldo por parte de la organización con soporte de las TI, (2) centrarse en los elementos de datos críticos, (3) énfasis en los artefactos de datos, (4) alineación en torno a métricas y aplicación de políticas, y (5) celebración de las victorias rápidas conseguidas como hitos en una hoja de ruta a largo plazo.

-   La Organización para  la Cooperación y el Desarrollo Económicos  (_Organisation for Economic Cooperation and Development_, OECD), en su informe sobre gobierno del dato para administraciones públicas [@OECD2019], recoge las mejores prácticas llevadas a cabo por diferentes administraciones de los países que la componen en lo que se refiere a transparencia del gobierno del dato e incremento del valor de la información disponible sobre la ciudadanía de cara a una mejor prestación de servicios públicos.
-   @Treder2020 identifica algunos componentes específicos que debería tener un sistema de gobierno del dato (cadenas de valor; estrategia de dato; procesos de datos; descripción de los roles y sus responsabilidades; gestión del equipo de la oficina del dato), así como las áreas en las que debe enfocarse el gobierno del dato (casos de negocio; aspectos éticos y cumplimiento; gestión y análisis del dato).


\index{MAMD, Modelo Alarcos de Madurez de Datos}
A modo de ejemplo, se dan más detalles sobre un marco de referencia basado en estándares internacionales ISO: el **Modelo Alarcos de Madurez de Datos (MAMD) v4.0** [@MAMD2023].
MAMD v4.0 es un marco de trabajo que se usa para la evaluación y mejora de la capacidad de los procesos de la organización relacionados con la gestión, la gestión de la calidad, y el gobierno del dato.
Tiene dos componentes principales:

-   Un **modelo de referencia de procesos (MRP)**, que contiene una descripción de los procesos de gestión del dato, de gestión de calidad del dato y de gobierno del dato. Está alineado con los principales estándares en el área: ISO 8000-61 [@ISO8000-61], e ISO/IEC 38505-2 [@ISO2018], así como con las buenas prácticas de otros modelos como DAMA, DMM o COBIT 2019 (véase Fig. \@ref(fig:mamd-mrp)).

\begin{figure}

{\centering \includegraphics[width=1\linewidth]{img/fig-1-gobierno} 

}

\caption{Modelo de referencia de procesos de MAMD. DM: gestión del dato; DQM: gestión de calidad del dato; DG: gobierno del dato.}(\#fig:mamd-mrp)
\end{figure}

-   El **modelo de evaluación de procesos (MEP)**, que sigue las directrices de evaluación y los niveles de capacidad y madurez descritos por ISO/IEC 33000 y adaptados a la evaluación de procesos de datos conforme al modelo de madurez propuesto en ISO 8000-62 [@ISO8000-62] (véase Fig. \@ref(fig:mamd-modelomadurez)).

\begin{figure}

{\centering \includegraphics[width=1\linewidth]{img/fig-2-gobierno} 

}

\caption{Modelo de madurez organizacional de MAMD; N: nivel;  DM: gestión del dato; DQM: gestión de calidad del dato; DG: gobierno del dato.}(\#fig:mamd-modelomadurez)
\end{figure}




## Gestión de calidad del dato

Los datos con niveles inadecuados de calidad acaban teniendo un impacto negativo para las organizaciones, bien en términos económicos, bien en términos de reputación [@redman2016getting]. Por eso es importante que las organizaciones cuiden del nivel de calidad de sus datos y se aseguren de que dicho nivel permanece dentro de los permitidos para que la ejecución de los procesos de negocio se haga dentro del margen de riesgo de la organización.

\index{característica de calidad del dato} \index{dimensión de calidad del dato}

Se dice que un conjunto de datos tiene calidad cuando sirve para el propósito para el que fue recogido (*fitness for use*) [@Strongetal1997]. Para determinar si un conjunto de datos tiene calidad suficiente para dicho propósito es preciso identificar y seleccionar un conjunto de criterios --llamados en la literatura **dimensiones** [@Wang1998] o **características de calidad del dato** [@ISO25012]-- que permitan determinar si dicho conjunto cumple los requisitos de calidad que exige el usuario de tales datos.
\index{modelo de calidad del dato}
Al conjunto de dimensiones o características de calidad del dato seleccionadas se le denomina **modelo de calidad del dato**.
\index{ISO/IEC 25012} 
La Tabla \@ref(tab:TablaCaracteristicasISO25012) muestra una descripción de las características de calidad del dato incluidas en el estándar ISO/IEC 25012.


<!-- Para compilar en HTML -->
<!-- tabla-html -->




\begin{table}[]
\caption{\label{tab:TablaCaracteristicasISO25012}Características de calidad del dato}

\begin{small}
\begin{tabular}{p{3cm}p{8cm}cp{1cm}cp{4cm}}
\hline
Característica & Definición & Inherente & Dependiente del sistema \\
\hline
Exactitud     & Grado en que los datos representan correctamente el valor de un atributo de una entidad del mundo real & $\checkmark$ & \\
Completitud   & Grado en el que existen suficientes valores para todos los atributos necesarios para una aplicación que involucre una representación de una entidad del mundo real & $\checkmark$  & \\
Consistencia  & Grado en que los datos están libres de contradicción y son coherentes con el resto de los datos de su contexto de uso & $\checkmark$ & \\
Credibilidad  & Grado en que los  datos se consideran ciertos y creíbles por los usuarios & $\checkmark$ & \\
Actualidad    & Grado en que los datos tienen fechas y tiempos adecuados para el uso previsto & $\checkmark$ &  \\
Accesibilidad & Grado en que los datos pueden ser accedidos por cualquier usuario autorizado & $\checkmark$ & $\checkmark$ \\
Cumplimiento  & Grado en el que los datos están construidos conforme estándares, convenciones o regulaciones & $\checkmark$ & $\checkmark$ \\
Confidencialidad & Grado en el que los datos tienen atributos específicos que solo pueden ser accedidos por   usuarios autorizados & $\checkmark$   & $\checkmark$ \\
Eficiencia    & Grado en el que los datos tienen atributos que pueden ser accedidos y procesados dentro de los niveles de rendimiento esperados     & $\checkmark$ & $\checkmark$ \\
Precisión     & Grado en el que los datos son precisos. & $\checkmark$ & $\checkmark$ \\
Trazabilidad  & Grado en el que los datos tienen atributos que proveen información detallada sobre los cambios realizados en los datos & $\checkmark$ & $\checkmark$ \\
Comprensibilidad & Grado en el que los datos se expresan de manera que los usuarios puedan leerlos e interpretarlos correctamente & $\checkmark$ & $\checkmark$ \\
Disponibilidad & Grado en el que los datos están disponibles para que los usuarios y/o aplicaciones autorizadas puedan acceder a ellos & & $\checkmark$ \\
Portabilidad   & Grado en el que los datos pueden ser alojados, reemplazados o movidos desde un sistema a otro & & $\checkmark$ \\
Recuperabilidad & Grado en el que los datos disponen de formas de mantener un nivel especificado de operabilidad incluso cuando se producen fallos & & $\checkmark$ \\
\hline
\end{tabular}
\end{small}
\end{table}



Como puede observarse, estas características se clasifican en dos grandes bloques: **inherentes** y **dependientes del sistema**.
Las **inherentes** se refieren al grado con el que las características de calidad de los datos tienen un potencial intrínseco para satisfacer las necesidades establecidas y necesarias cuando los datos son utilizados bajo condiciones específicas; las **dependientes del sistema** permiten determinar el grado con el que la calidad del dato es alcanzada y preservada a través de un sistema informático cuando los datos son utilizados bajo condiciones específicas.

Para ilustrar el significado de algunas de estas características (por ejemplo, **exactitud**, **completitud** o **consistencia**). A continuación se exponen algunos ejemplos:

-   Como ejemplo de nivel inadecuado de **exactitud sintáctica** podría ponerse el hecho de que el atributo `Nombre` de la entidad `Persona` tomase un valor o dato "Marja" (no existente en los datos de referencia de nombre) en lugar de "María" (que sí que está incluido).
-   El hecho de que el atributo `Nombre` de la entidad `Persona` tome el valor de "George" en vez de "Jorge" para almacenar datos de la persona llamada realmente "Jorge" es un ejemplo de nivel inadecuado de **exactitud semántica**. Ambos valores son sintácticamente correctos, pero `George` es otra persona distinta a `Jorge`, y quien capturó y guardó los datos simplemente se equivocó de persona.
-   Supóngase que, para una determinada aplicación, se necesita recoger valores (o datos) para los siguientes atributos de una entidad Persona: `DNI`, `Nombre`, `Apellido1`, y `Apellido2` para ser empleados adecuadamente en un contexto de uso. En caso de faltar alguno de ellos (nivel inadecuado de completitud), podría ocurrir que los datos de la persona no se pudieran utilizar; incluso podrían faltar algunos atributos más, como por ejemplo `email`, pero si no es relevante para la aplicación, no habría ese problema de completitud.
-   Un ejemplo de falta de consistencia puede darse, por ejemplo, cuando el valor (o dato) del atributo `FechaNacimiento` de la entidad Persona es posterior a la fecha de hoy.


Diferentes autores han proporcionado diferentes mecanismos para medir y evaluar la calidad de los datos usando las dimensiones o características de calidad seleccionadas.
Aunque para dar soporte a este proceso se han propuesto numerosas metodologías de evaluación [@batini2016data], el principal problema de estas contribuciones es que, normalmente, se han realizado *ad hoc* y no permiten ni generalizar los resultados obtenidos ni compararlos con los obtenidos por otras organizaciones [@loshin2011master].

Para paliar estos problemas, se han desarrollado estándares que recogen los conocimientos y principios básicos comunes para medir y evaluar la calidad de los datos.
Ejemplos de estos estándares pueden ser la mencionada ISO/IEC 25012 [@ISO25012] y la ISO 8000-8 [@ISO8000-8], que recogen características de calidad; la ISO/IEC 25024 [@ISO25024], que recoge aspectos específicos de cómo llevar a cabo las mediciones de las características; o la ISO/IEC 25040 [@ISO25040], que proporciona una metodología de evaluación de calidad del software que puede ser adaptada a la evaluación rigurosa y sistemática de la calidad de los datos.
\index{medición de calidad del dato} \index{evaluación de calidad del dato}
En este punto es necesario introducir la principal diferencia entre **medir** y **evaluar** la calidad: medir consiste en determinar la cantidad de calidad del dato que tiene un conjunto de datos; evaluar implica determinar si, de acuerdo al nivel de riesgo que asume la organización, la cantidad de calidad del dato medida es suficiente y adecuada para usar los datos en el contexto de uso establecido para esos datos.
La evaluación de calidad del dato requiere primero medir la calidad; y para medir la calidad, primero deben definirse procedimientos de medición.

\index{ISO/IEC 25024}En ese sentido, ISO 25024 [@ISO25024] proporciona una serie de propiedades medibles para cada una de las características presentadas en la Tabla \@ref(tab:TablaCaracteristicasISO25012); además, para cada una de estas propiedades medibles, el estándar proporciona un método de medición genérico que permitirá, convenientemente particularizado, medir dichas propiedades y luego agruparlas para determinar el valor de la característica de calidad del dato.
La Fig. \@ref(fig:propiedadesISO25024) muestra las propiedades medibles para las características de calidad identificadas como inherentes (véase Tabla \@ref(tab:TablaCaracteristicasISO25012)).

\begin{figure}

{\centering \includegraphics[width=1\linewidth]{img/fig-3-gobierno} 

}

\caption{Algunas propiedades de las características inherentes de calidad del dato.}(\#fig:propiedadesISO25024)
\end{figure}


Una de las ventajas de usar estas propiedades medibles es que, en caso de niveles inadecuados de calidad de datos, es posible identificar mejor qué está causando que esto ocurra y, por tanto, es más fácil actuar directamente sobre dichas causas.

A modo de ejemplo, supóngase que se quiere medir el grado de **exactitud** de un conjunto de datos. Para ello se considera necesario medir las propiedades **exactitud sintáctica**, **exactitud semántica** y **rango de exactitud**. Con lo resultados de la medición habrá que hacer algún tipo de agrupación que tenga en cuenta la importancia o peso relativo de cada una de estas propiedades a la hora de evaluar la **exactitud**. Supóngase que una organización determina que la mejor forma de hacerlo es mediante una media aritmética ponderada de los resultados de la medición de las tres propiedades medibles. En base a su nivel de riesgo para un determinado proceso de negocio, supóngase que la organización considera que, para una determinada aplicación, puede asignar, para la media aritmética ponderada, los siguientes pesos: 0,4; 0,4 y 0,2 para la exactitud semántica, para la exactitud sintáctica y para el rango de exactitud respectivamente.

A la hora de medir las propiedades medibles correspondientes a las características de calidad del dato, es interesante tener en cuenta que la ISO/IEC 25024 proporciona procedimientos de medición cuya implementación depende fuertemente de la naturaleza de la propiedad y del objeto cuya calidad quiere medirse. La medición de algunas de estas propiedades implica contar el porcentaje de registros que violan las reglas de negocio que regulan la adecuación al uso de los datos en un contexto determinado [@loshin2002rule]. Sin embargo, a la hora de la medición, uno de los ejercicios más difíciles es recolectar y validar las reglas de negocio específicas que rigen la validez de los datos [@Caballeroetal2022br4dq].
Para el ejemplo propuesto, imagínese que si se pretende medir el nivel de exactitud sintáctica del dato recogido en el atributo `DNI` se pudiera usar la siguiente regla de negocio "*el DNI tiene que seguir la especificación para DNI, o para el NIE, correspondiente con la expresión regular* `(d{8})([A-Z])`". Habría que comprobar bien manualmente, bien mediante algún tipo de script, cuántos registros verifican la anterior regla de negocio para el atributo `DNI`. 

Supóngase que, para el ejemplo, y tras haber realizado todas las mediciones de las propiedades y haberlas agrupado realizando la media aritmética ponderada, se obtiene una exactitud de 70.

Una vez realizada la medición, el siguiente paso es la evaluación propiamente dicha. La evaluación consiste en comparar el resultado obtenido (70 en el ejemplo) con el umbral mínimo de aceptación, que depende del nivel de riesgo que decida asumir la organización al utilizar estos datos [@redman2016getting]. Si, por ejemplo, dicho umbral se hubiese establecido en 75 para el uso concreto que se le va a dar a estos datos, se concluiría que no deberían ser utilizados. Esto no significa que los datos no puedan usarse en otro contexto en el que, por ejemplo, el valor umbral se estableciese en 65.

como se verá posteriormente en el Cap. \@ref(130009), en algunos contextos de uso  antes de usar los datos se realiza un proceso de preparación de los mismos que tiene como objetivo determinar y adecuar los niveles de calidad al uso que se pretende dar mediante un proceso de evaluación y mejora que se centra en la **limpieza de los datos**. Normalmente, en este proceso suele recurrirse a métodos estadísticos, frente a la aproximación basada en la medición de las características de calidad del dato presentada anteriormente. 
Se pierde entonces, de alguna manera, la capacidad de establecer una dirección más efectiva y, sobre todo, alineada a las necesidades reales de la organización, de las operaciones de evaluación y limpieza del dato. \index{limpieza del dato}

Finalmente, es interesante mencionar que, basándose en los estándares ISO/IEC 25012 [@ISO25012] e ISO/IEC 25024 [@ISO25024], es posible certificar el nivel de calidad del dato de un repositorio de datos.
En @Gualoetal2021 se recogen experiencias de medición, evaluación y certificación de calidad del dato a este respecto.



### Medición de calidad de datos *vs.* perfilado del dato

\index{perfilado del dato}
En esta subsección se plantea el **perfilado del dato** como una técnica base para realizar la medición de las propiedades medibles de las características de calidad del dato o para descubrir nuevas reglas de negocio. El perfilado de datos es el conjunto de actividades y procesos que sirve para obtener algunos metadatos (incluyendo reglas de negocio) de un conjunto de datos.  Estos metadatos pueden usarse como base para realizar la medición de las propiedades medibles de las características de calidad del dato. @abedjan2015profiling clasifican los tipos de **perfilado del dato** en las siguientes categorías:

-   **Perfilado de columna simple**, que implicaría tareas de identificación de cardinalidades, identificación de patrones y tipos de datos, distribución de valores de datos, clasificación de dominios.

-   **Perfilado de columnas múltiples**, que implicaría tareas de correlación y reglas de asociación, identificación de clusters y *outliers*, elaboración de resúmenes de datos y bocetos.

-   **Perfilado de dependencias**, que a su vez implica:

    -   **Detección de reglas de unicidad**, tales como la identificación de claves, identificación de condiciones e identificación de sinónimos.
    -   **Detección de dependencias de inclusión**, que puede abarcar el descubrimiento de claves ajenas o la identificación de dependencias condicionales de inclusión.
    -   **Dependencias funcionales**, como pueden ser las dependencias condicionales.

En **R** software el paquete `dlookr` [@dlookr2022] contiene algunas funciones interesantes para llevar a cabo determinadas tareas de perfilado.
Por ejemplo, la función `overview()` da información general sobre un conjunto de datos; resulta muy interesante la función `diagnose()`, que proporciona información realizando un perfilado de los valores únicos y los valores únicos de un conjunto de valores.

El siguiente fragmento de código (*chunck*) mostraría, si se ejecutase el tipo de información proporcionada por `diagnose (Madrid_POIS$City_Center)`: `variables` muestra el nombre de los atributos del conjunto de datos (en este caso `Lon`de longitud y `Lat` de latitud); `types` muestra el tipo de dato de cada `variable`; `missing_count`, `missing_percent`, `unique_count` y `unique_rate` describen respectivamente el conteo de valores nulos, el porcentaje de dichos valores, el número de valores únicos o no repetidos y su correspondiente porcentaje;  `<chr>`, `<int>`, `<dbl>` se hacen referencia al tipo de dato de cada uno de los parámetros anteriores (carácter, *integer*, *double*).


```r
library("dlookr")
library("idealista18")
diagnose(Madrid_POIS$City_Center)
```

Si estas funciones de perfilado proporcionan información suficiente y adecuada, es posible usar los resultados para computar las mediciones de las propiedades medibles.
Por ejemplo, se puede utilizar el resultado de la columna `missing count` para calcular el grado de completitud de las variables `longitud`  y `latitud`, que se pueden establecer en 100% al ser `missing count = 0` para las dos variables.
Incluso se pueden utilizar funciones como `plot_na_pareto()` para visualizar un gráfico de Pareto que muestre las variables que no tienen valores nulos.
\index{dlookr}
Finalmente, es interesante mencionar que el paquete `dlookr` incluye funciones como `diagnose_paged_report()`, que permiten elaborar informes que contienen información sobre las estructuras de datos del conjunto de datos, avisos, descripción de las variables, valores perdidos, valores únicos de las variables categóricas y numéricas, distribuciones de valores nulos y negativos, posibles valores atípicos (*outliers*), etc. El siguiente fragmento de código explica cómo crear un informe de 15 páginas en formato PDF con toda esa información sobre la variable `idealista18::Madrid_POIS$Metro` :


```r
#diagnose_paged_report(idealista18::Madrid_POIS$Metro)
```

En ocasiones, y retomando la idea de las reglas de negocio, cabe decir que la información proporcionada por el perfilado del dato puede usarse para $(i)$ derivar reglas de negocio a partir del estado actual de los datos y $(ii)$ recoger información que se puede emplear durante el proceso de medición de determinadas características de calidad del dato.

En el Cap. \@ref(130009) se profundizará en el proceso de estudio de dos características de calidad del dato: completitud y consistencia.

\index{mejora del dato}

### Mejora del dato

Si los datos no tienen el nivel de calidad necesario, es preciso mejorar su calidad para que no arruinen los procesos de negocio.
Para ello, a partir de los resultados de las mediciones, los analistas de calidad del dato deben determinar las causas raíz de esos niveles inadecuados de calidad del dato.
@Strongetal1997_Potholes identifican diez posibles obstáculos que pueden hacer que los datos no tengan esos niveles adecuados de calidad:

1.  Múltiples fuentes de datos producen diferentes valores para el mismo atributo de la misma entidad.
2.  La realización de juicios subjetivos en la producción de los datos puede llevar a valores diferentes.
3.  Errores sistemáticos en la producción de información llevan a la pérdida de información.
4.  Grandes volúmenes de información almacenada dificultan su acceso en tiempo razonable.
5.  Sistemas heterogéneos distribuidos llevan a definiciones, formatos y valores inconsistentes.
6.  La información no numérica es difícil de indexar.
7.  El análisis automatizado de los contenidos en colecciones de información puede no producir resultados adecuados. 
8.  A medida que las necesidades de los usuarios cambian, la  información que es relevante y útil para la realización de una determinada tarea también cambia.
9.  Un acceso fácil a la información puede entrar en conflicto con los requisitos de seguridad, confidencialidad y privacidad.
10.  La falta de recursos de computación limita el acceso a los datos en circunstancias favorables.

En función de la naturaleza del problema detectado, las acciones correctivas pueden ser de distinta naturaleza:

-   **Corrección de causas sistemáticas**.
    Si se observa que los problemas se suceden de forma sistemática y repetida, entonces las acciones de mejora del dato deben estar orientadas a eliminar esas causas sistemáticas (véase [@Strongetal1997_Potholes]).
    Por ejemplo: si los errores de calidad del dato se deben a que un proceso de negocio está mal diseñado, entonces hay que rediseñarlo; si las causas se deben a que hay personas desempeñando ciertos roles para los que no tienen los conocimientos o habilidades adecuadas, entonces hay que darles la formación adecuada; o si se deben a que hay software (por ejemplo, procesos ETL) que falla, entonces hay que realizar el mantenimiento correctivo correspondiente.

-   **Corrección de errores debidos a causas aleatorias**.
    Si no es posible identificar cuáles son las causas raíz, porque son completamente desconocidas o aleatorias, no queda más remedio que actuar sobre los valores de los datos, cambiándolos para asegurar que cumplen las reglas de negocio establecidas.
    A este proceso se le suele llamar **depuración o limpieza de datos (_data cleansing_)**.
    @ilyas2019data identifican diversas técnicas de limpieza de datos (que pueden incluir operaciones de \index{imputación del dato} **imputación de datos** como las incluidas en la Sec. \@ref(imputacion) del Cap. \@ref(130009): limpieza basada en reglas de negocio, de duplicación de datos, transformación de datos, o guiada por *machine learning* (véanse Caps. \@ref(130009) y \@ref(chap-feature)). En este caso, sería posible utilizar algunas funciones del paquete `dlookr` relacionadas con la transformación de los datos,  como `imputate_na()` o `imputate_outlier()`, que generan valores para los datos faltantes o valores que garantizan niveles adecuados de exactitud o de consistencia.

::: {.infobox_resume data-latex=""}
### Resumen {-}

En este capítulo se presentan los fundamentos del gobierno del dato.
Es importante tener en cuenta los siguientes aspectos:

-   El gobierno del dato tiene como objetivo asegurar que los datos que se usan y gestionan en las organizaciones están alineados con las estrategias del dato de la organización, maximizando así su valor organizacional.

-   Gobernar los datos implica el diseño, implementación y mantenimiento de un sistema de gobierno del dato. Un sistema de gobierno del dato tiene siete tipos de componentes: procesos de gestión del dato, gestión de calidad del dato y gobierno del dato; estructuras organizacionales; principios, políticas y marcos de referencia; datos y descripción de los datos; cultura, ética y comportamiento; personas, habilidades y competencias; servicios, infraestructuras y aplicaciones.

-   Existen modelos de referencia que pueden ser usados como base para la creación de sistemas de gobierno del dato.

-   El gobierno del dato persigue cuatro beneficios básicos para la organización: alineamiento estratégico, realización de beneficios, optimización de riesgos, optimización de recursos.

-   La gestión de la calidad del dato es el proceso mediante el cual se garantiza que los datos tengan el nivel de calidad adecuado para las tareas para las que fueron recogidos.

-   Para evaluar y medir la calidad se necesitan criterios; estos criterios se llaman características o dimensiones de calidad del dato.

-   La evaluación y medición de calidad del dato requiere la identificación y clasificación de las reglas de negocio que rigen la validez de los datos.  Las técnicas y herramientas de perfilado del dato se pueden utilizar como base para la identificación de reglas de negocio a partir de los datos.

-   Cuando las mediciones y evaluaciones realizadas indican que los datos no tienen calidad, deben investigarse cuáles son las posibles causas. 
Si las causas son sistemáticas hay que enfocar el problema desde un punto de vista organizacional; si son aleatorias se pueden usar las técnicas de limpieza de datos.
:::
