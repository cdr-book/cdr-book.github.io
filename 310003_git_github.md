


# Git y GitHub R {#github}

*Michal Kinel*


## ¿Qué es Git y GitHub?


Git \index{git} es un sistema de control de versiones distribuido, diseñado para
registrar y rastrear los cambios realizados en un archivo o conjunto de
archivos a lo largo del tiempo [@chacon2009]. Al utilizar Git, se pueden ver y
restaurar versiones anteriores de un archivo, así como fusionar cambios
realizados por diferentes personas en una única versión actualizada.

Por otro lado, GitHub \index{github} es una plataforma de alojamiento de código online
que utiliza Git como sistema de control de versiones subyacentes. Esta
plataforma permite a los desarrolladores compartir y colaborar en
proyectos de software, alojando el código fuente en la nube [@astigarraga2022]. Además de
alojar repositorios de Git, GitHub ofrece herramientas adicionales como
seguimiento de problemas, solicitudes de extracción, y wiki de
proyectos, lo que la hace una herramienta popular para el desarrollo de
software colaborativo y de código abierto.

El uso de Git y GitHub se ha extendido a otros campos más allá del
desarrollo de software, como la ciencia de datos, la documentación
técnica y la colaboración en general. Su popularidad se debe en gran
parte a la facilidad de uso, la flexibilidad y la capacidad de trabajar
en proyectos de software colaborativos de manera eficiente y segura tanto.


## ¿Por qué usar Git y GitHub?


Git y GitHub son herramientas para el desarrollo de software moderno, y
su uso se ha extendido a otros campos como la ciencia de datos, la
documentación técnica y la colaboración entre los desarrolladores. A continuación, se
presentan tres ventajas clave de uso de Git y GitHub:

1.  **Control de versiones y colaboración eficiente** Git es un sistema
    de control de versiones \index{control de versiones} distribuido que permite registrar y rastrear
    los cambios realizados en un archivo o conjunto de archivos a lo
    largo del tiempo. Esto es especialmente útil cuando se trabaja en
    proyectos de software colaborativos, donde múltiples personas pueden
    estar editando el mismo archivo al mismo tiempo. Con Git, se pueden
    ver y restaurar versiones anteriores de un archivo, y también
    fusionar cambios realizados por diferentes personas en una única
    versión actualizada. Además, GitHub ofrece herramientas adicionales
    como seguimiento de problemas, solicitudes de extracción, y wiki de
    proyectos.

2.  **Mejora la eficiencia y la seguridad en el desarrollo de software**
    El uso de Git y GitHub permite a los desarrolladores trabajar de
    manera más eficiente y segura en proyectos de software. Al utilizar
    un sistema de control de versiones como Git, los desarrolladores
    pueden colaborar de manera más efectiva y reducir el riesgo de
    conflictos o errores en el código. Además, GitHub ofrece
    características como la integración continua y la entrega continua
    (CI/CD), que automatizan y agilizan el proceso de desarrollo de
    software.

3.  **Fomenta la transparencia y la comunidad** GitHub es una plataforma
    de alojamiento de código abierto \index{código abierto}, lo que significa que los proyectos
    alojados en ella son de acceso público y pueden ser revisados y
    mejorados por otros desarrolladores. Esto fomenta la transparencia
    de código abierto como privado dentro de una empresa. Además, GitHub
    cuenta con una gran comunidad de desarrolladores que pueden ofrecer
    soporte y retroalimentación a otros miembros de la comunidad.



## Instalación y/o actualización de **R** y RStudio {#sec-instala-git}
\index{Rstudio}

**R** es un lenguaje de programación utilizado en la estadística y la
ciencia de datos para realizar análisis, modelado y visualización de
datos. **RStudio**, por otro lado, es un entorno de desarrollo integrado
(IDE) que proporciona una interfaz gráfica de usuario para trabajar con
R. Instalar o actualizar **R** y RStudio es un proceso relativamente
sencillo, y se pueden seguir los siguientes pasos:

1.  Descargar e instalar **R** Lo primero que se debe hacer es descargar
    **R** desde la [página oficial de
    **R**](https://www.r-project.org/). Dependiendo del sistema
    operativo, se debe elegir la versión correcta de **R** para
    descargar. Una vez que se haya descargado el archivo, se debe seguir
    el asistente de instalación para instalar **R** en el equipo.

2.  Descargar e instalar RStudio: una vez instalado **R**, se puede
    proceder a instalar RStudio desde su [página
    oficial](https://posit.co/download/rstudio-desktop/). Al igual que
    con **R**, se debe elegir la versión adecuada para el sistema
    operativo y seguir el asistente de instalación para instalar
    RStudio en el equipo.

3.  Actualización de **R** y RStudio Para actualizar **R**, se debe
    abrir **R** y ejecutar el siguiente comando en la consola:

  

```r
install.packages("installr") 
library(installr)
updateR()
```

Esto instalará el paquete `installr` y actualizará automáticamente **R**
a la última versión disponible. Para actualizar RStudio, se debe
abrir RStudio y verificar si hay una actualización disponible en el
menú "Help" -> "Check for Updates". Si hay una actualización
disponible, se debe seguir el asistente de actualización para instalar
la última versión de RStudio.

En resumen, la instalación o actualización de **R** y RStudio es un
proceso sencillo que se puede realizar siguiendo los pasos mencionados
anteriormente. Mantener estas herramientas actualizadas es importante
para asegurarse de tener acceso a las últimas características y
correcciones de errores.

## Configuración de Git y GitHub {#configurar-git-github}

Configurar Git y GitHub es un paso importante antes de comenzar a
trabajar en proyectos de software colaborativos. Se pueden seguir los
siguientes pasos para configurar Git y GitHub:

1. Instalar Git \index{instalar git}. En primer lugar, es necesario instalar Git en el
equipo. Git puede descargarse desde la página oficial de
[Git](https://git-scm.com/downloads). Una vez que se haya descargado el
archivo, se debe seguir el asistente de instalación para instalar Git en
el equipo. Además, en la página oficial se encuentra un manual completo
sobre el uso de Git.

2. Configurar Git. Una vez que se ha instalado Git, se debe configurar
el nombre de usuario y la dirección de correo electrónico para que los
cambios que se realicen en los repositorios estén correctamente
etiquetados. Para hacer esto, se debe abrir la línea de comandos, Git
Bash o la terminal de RStudio, y ejecutar los siguientes comandos:

<div class="figure" style="text-align: center">
<img src="img/terminal.jpg" alt="Terminal de RStudio" width="60%" />
<p class="caption">(\#fig:c310003002)Terminal de RStudio</p>
</div>


```git
$ git config --global user.name "Su Nombre"
$ git config --global user.email "su.correo@ejemplo.com"
```
  Esto configurará el nombre de usuario y la dirección de correo
  electrónico de forma global en Git.

3. Crear una cuenta en GitHub. Para utilizar GitHub, es necesario crear
una cuenta en la página oficial de GitHub (<https://github.com/join>).
Una vez que se haya creado la cuenta, se debe iniciar sesión en GitHub.

4. Configurar la clave SSH \index{ssh} (protocolo Secure Shell), una credencial de acceso para el protocolo de red que permite el acceso remoto a través de una conexión cifrada. Para autentificar las conexiones con GitHub de
manera segura (véase [capítulo 10](https://happygitwithr.com/ssh-keys.html) de [@happygitwithr]), se recomienda configurar una clave SSH en el equipo y
agregarla a la cuenta de GitHub. Para ello, se debe abrir la línea de
comandos, Git Bash o la terminal de RStudio, y ejecutar los siguientes
comandos:


```git
$ ssh-keygen -t rsa -b 4096 -C "su.correo@ejemplo.com"
```

Esto generará una clave SSH. A continuación, se debe agregar la clave
SSH al agente de SSH:
  

```git
$ eval "$(ssh-agent -s)"
$ ssh-add ~/.ssh/id_rsa
```

Finalmente, se debe copiar la clave SSH al portapapeles:


```git
$ clip < ~/.ssh/id_rsa.pub
```
  
    
y agregarla a la cuenta de GitHub siguiendo las instrucciones en la página de configuración de la cuenta de GitHub:
  

-   En la esquina superior derecha dela página del inicio, haga clic en la
    foto del perfil y, luego, en "Settings" (Configuración).

-   En la sección "Accsess" de la barra lateral, haga clic en "SSH adn GPG keys".

-   Haga clic en "New SSH key" para agregar la clave SSH.

<div class="figure" style="text-align: center">
<img src="img/ssh-add-ssh-key-with-auth.png" alt="Llaves SSH en GitHub" width="60%" />
<p class="caption">(\#fig:c310003008)Llaves SSH en GitHub</p>
</div>

-   En el campo "Title" (Título), agregue una etiqueta descriptiva para
    la clave nueva. Por ejemplo, si está utilizando un portátil
    personal, puede llamar a esta clave "Portátil personal".

-   Seleccione el tipo de clave, ya sea de autentificación o de firma.
    Para obtener más información sobre la firma de una confirmación,
    consulte
    [aquí](https://docs.github.com/es/authentication/managing-commit-signature-verification/about-commit-signature-verification).

-   Pegue su clave pública en el campo "Key".

<div class="figure" style="text-align: center">
<img src="img/ssh-key-paste-with-type.png" alt="Añadir llave SSH en GitHub" width="60%" />
<p class="caption">(\#fig:c310003009)Añadir llave SSH en GitHub</p>
</div>

-   Haga clic en "Add SSH key" para agregar la clave SSH.

-   Si se le solicita, confirme su contraseña en GitHub. Para obtener
    más información, véase [Modo
    sudo](https://docs.github.com/es/authentication/keeping-your-account-and-data-secure/sudo-mode).

Además de utilizar una clave SSH para autentificar las conexiones con
GitHub, también se puede utilizar la autentificación basada en token de
acceso personal, PAT \index{pat}, de GitHub. Esta forma de autentificación es
recomendada por GitHub como una forma segura de autentificar conexiones,
especialmente cuando se trabaja con aplicaciones y herramientas que
requieren acceso a repositorios de GitHub. Para más información sobre el token de aceso personal, PAT, consulte el [capítulo 9](https://happygitwithr.com/https-pat.html) de [@happygitwithr].

A continuación, se describen los pasos para utilizar la autentificación
basada en token de acceso personal de GitHub:

1.  Generar el token PAT: existen dos librerías `usethis` y `gitcreds`
    que facilitan la generación del PAT y almacenarlo, para ello, se
    introduce en la consola de RStudio:


```r
library(usethis)
usethis::create_github_token()
```


2.  Seguir las instrucciones en GitHub: a continuación se abrirá el sito
    web de GitHub, se ingresa mediante el usuario y contraseña, con el
    cuadro para generación del PAT, *New personal access token (classic)*. En *Note* se introduce una nota identificativa al igual
    que en el procedimiento anterior y se selecciona el tiempo de
    validez del PAT en la pestaña *Expiration*, dejando las demás
    opciones por defecto. Se hace clic en *Generate token* para crear el
    token. En la nueva ventana se copia el token para posteriormente introducir en la consola:
    

```r
library(gitcreds)
gitcreds::gitcreds_set()
```

En *password* se pega el token copiado anteriormente.

3.  Para verificar que el nuevo PAT está configurado se introduce en la
    consola:


```r
gitcreds::gitcreds_get(use_cache = FALSE)
```
  

Si la autentificación fue correcta se generará una salida similar a la
  siguiente:
  

```r
 <gitcreds>
   protocol: https
   host    : github.com
   username: mi_usuario
   password: <-- hidden -->
```
Una vez conectado RStudio y GitHub mediante SHH o PAT se puede proceder
con la creación del repositorio en Git.



## Conectar Git y GitHub con Rstudio



### Rstudio primero {#conectar-rstudio-primero}

Este apartado se centra en el enfoque de creación de un nuevo proyecto
en un ordenador local para posteriormente subirlo a GitHub, en remoto.

Una vez instalados y configurado Git en nuestro sistema y con la cuenta
de GitHub hay que seguir los siguientes pasos para conectar Git y GitHub
con RStudio:

1.  Configurar Git en RStudio: Una vez que Git está instalado en el
    sistema, se debe configurar Git en RStudio. Para ello, se debe ir a
    la pestaña "Tools" en la barra de menú principal, seleccionar
    "Global Options" y luego seleccionar "Git/SVN". Desde allí, se debe
    configurar la ubicación del ejecutable de Git en el sistema.

<div class="figure" style="text-align: center">
<img src="img/tools-general-options.png" alt="Tools de Rstudio" width="40%" />
<p class="caption">(\#fig:c310003014)Tools de Rstudio</p>
</div>


2.  Verificar la versión de Git, introduciendo en la Terminal:


```git
$ git --version
```

  Si la salida es la versión de Git entonces la instalación fue ejecutada correctamente.

\pagebreak

3.  Crea un proyecto nuevo desde "File" -> "New project"


<div class="figure" style="text-align: center">
<img src="img/new-project.png" alt="Nuevo proyecto de Rstudio" width="40%" />
<p class="caption">(\#fig:c310003016)Nuevo proyecto de Rstudio</p>
</div>


4.  En el siguiente cuadro se procede dando clic en "New directory"
    y en la siguiente ventana se rellenan los datos como el nombre del
    proyecto y se marca la opción "Create a git repository" para crear
    un nevo proyecto con repositorio de Git.

<div class="figure" style="text-align: center">
<img src="img/new-project-rstudo.png" alt="Nuevo proyecto en un directorio nuevo" width="40%" />
<p class="caption">(\#fig:c310003017)Nuevo proyecto en un directorio nuevo</p>
</div>


\pagebreak

5.  En el icono de Git en la parte superor se accede a la ventana de
    revisión de cambios, se añaden los ficheros pinchando en los ticks,
    se añade el mensaje de confirmación y se hace clic en "commit".

<div class="figure" style="text-align: center">
<img src="img/commit-rstudio.png" alt="Revisión de cambios" width="40%" />
<p class="caption">(\#fig:c310003018)Revisión de cambios</p>
</div>


6.  Alternativamente se puede utilizar la pestaña de Git, marcando los
    ficheros modificados o creados y confirmando mediante clic en "commit"
    tras el cual se abrirá el cuadro de diálogo anterior.

<div class="figure" style="text-align: center">
<img src="img/git-pestana.png" alt="Revisión de cambios" width="40%" />
<p class="caption">(\#fig:c310003019)Revisión de cambios</p>
</div>


7.  Para subir los cambios realizados en el proyecto recién configurado
    en RStudio, habiendo configurado Git y GitHub en los pasos
    anteriores, se ejecuta en la consola los siguientes pasos



```r
library("usethis")
usethis::use_github()
```


La función `usethis::use_github()` en sus valores por defecto creará un
repositorio público con el nombre de proyecto en la cuenta asociada.
Para ver más opciones acuda a la ayuda de la función, ejecutando en la
consola `?usethis::use_github`.



### GitHub Primero


Este apartado se centra en el enfoque de creación de un nuevo proyecto
en un ordenador local a partir de un repositorio disponible en GitHub, en
remoto. Antes de todo hay que verificar si se tiene instalado Git, basta
introduciendo en la terminal de RStudio al igual que en el apartado anterior.


```git
$ git --version
```


Cuando la salida de la terminal arroje la versión de Git entonces la instalación fue correcta. En el caso de que la salida no arroje la versión vuelva la Sec. \@ref(configurar-git-github) o consulte el manual de la página oficial de Git en: [https://git-scm.com](https://git-scm.com).


A continuacción, se describe paso a paso sobre cómo conectar GitHub y
RStudio a partir de un proyecto ya existente en GitHub y con Git
configurado previamente:


1.  Abra RStudio y seleccione la opción "New Project" en la pestaña
    "File" del menú principal. Posteriormente haga clic en la opción
    "Version Control".

<div class="figure" style="text-align: center">
<img src="img/new-project.png" alt="Nuevo proyecto de Rstudio" width="40%" />
<p class="caption">(\#fig:c310003022)Nuevo proyecto de Rstudio</p>
</div>

\pagebreak

2.  En la ventana emergente que aparece, elija "Git".

<div class="figure" style="text-align: center">
<img src="img/new-project-git.png" alt="Crear proyecto desde control de versiones" width="40%" />
<p class="caption">(\#fig:c310003023)Crear proyecto desde control de versiones</p>
</div>


3.  En la siguiente ventana, pegue la URL del repositorio que desee
    clonar y presione "Create Project". RStudio le preguntará en qué
    carpeta desea guardar el proyecto, una vez que hayas elegido una
    ubicación, el proyecto se clonará en tu computadora.


<div class="figure" style="text-align: center">
<img src="img/new-project-git-repo.png" alt="Nuevo proyecto desde un repositorio de Git" width="40%" />
<p class="caption">(\#fig:c310003024)Nuevo proyecto desde un repositorio de Git</p>
</div>


Los cambios realizados en el repositorio se realizan de la misma forma
que en el apartado anterior.

## Flujo de trabajo general de Git y GitHub en RStudio

A continuación se describe un flujo básico de trabajo, comenzando desde
RStudio:

1.  **Iniciar un repositorio local**: Lo primero que hay que hacer es
    inicializar un repositorio local en RStudio. Para ello, abra RStudio
    y seleccione la opción "New Project" en la pestaña "File" del menú
    principal. Luego, seleccione la opción "New Directory" y elija una
    ubicación para su proyecto. A continuación, seleccione "Version
    Control" y luego "Git". RStudio le preguntará si desea inicializar
    un repositorio en este directorio; haga clic en "Yes". Tal y como se
    ha descrito en el punto 1 de la Sec. \@ref(conectar-rstudio-primero). 

2.  **Añadir archivos al repositorio**: Ahora, dene añadir los archivos
    de su proyecto al repositorio. Para ello, haga clic en la pestaña
    "Git" en la parte superior derecha de RStudio, y luego seleccione
    los archivos que desea agregar al repositorio. Haga clic en el botón
    "Stage", y los archivos seleccionados pasarán a la sección "Staged"
    en la parte inferior de la pestaña "Git". Si desea agregar todos los
    archivos del proyecto al repositorio, haz clic en el botón "Stage
    All".

3.  **Hacer un "commit" de los cambios**: Una vez que los archivos están
    en la sección "Staged", debe hacer un "commit" para registrar los
    cambios. Para hacerlo, escriba un mensaje breve que describa los
    cambios que ha realizado en la sección "Commit message". Luego, haga
    clic en el botón "Commit". Los cambios se registrarán en el
    repositorio local.

4.  **Crear una rama (opcional)**: Si desea trabajar en una nueva
    función o corregir un error sin afectar la rama principal (master o
    main), debe crear una nueva rama. Para ello, haga clic en el botón
    "New Branch" en la pestaña "Git". Escriba un nombre para la nueva
    rama y haga clic en "Create". Ahora ya es posible hacer cambios en
    los archivos en la nueva rama sin afectar la rama principal.

5.  **Subir los cambios al repositorio remoto**: Una vez que ha hecho un
    "commit" o confirmación de sus cambios, es hora de subirlos al
    repositorio remoto en GitHub. Para ello, haga clic en el botón
    "Push" en la pestaña "Git". Los cambios se subirán al repositorio
    remoto en GitHub, que fue configurado en la Sec. \@ref(configurar-git-github).

6.  **Solicitar un pull request (opcional)**: Si trabaja en un proyecto
    colaborativo con otros usuarios, debe solicitar un "pull request"
    antes de fusionar los cambios en la rama principal. Para hacerlo,
    haga clic en la pestaña "Pull Requests" en la interfaz de GitHub.
    Luego, haga clic en el botón "New Pull Request" y siga las
    instrucciones para crear la solicitud.

7.  **Fusionar los cambios en la rama principal (opcional)**: Si trabaja
    en una nueva rama y desea fusionar los cambios en la rama principal,
    debe crear una solicitud de "pull request". Si la solicitud es
    aceptada por el propietario del repositorio, los cambios se
    fusionarán en la rama principal.
    
::: {.infobox data-latex=""}
**Repositorio local y remoto:**

-   **Repositorio local** \index{repositorio local} en Git es una copia completa de un proyecto que se
    encuentra en el equipo del usuario. Con un repositorio local, los
    usuarios pueden trabajar en un proyecto sin conexión a Internet y
    luego enviar los cambios al repositorio remoto cuando estén
    conectados.
    
-   **Repositorio remoto** \index{repositorio remoto} en GitHub es una versión en línea del proyecto
    que está almacenada en los servidores de GitHub. Los usuarios pueden
    clonar un repositorio remoto a su equipo para tener una copia local
    del proyecto y trabajar en ella. Los cambios realizados en la copia
    local pueden ser enviados al repositorio remoto para compartirlos
    con otros usuarios.
:::

En resumen, el flujo de trabajo general de Git y GitHub en RStudio
implica inicializar un repositorio local, añadir archivos al
repositorio, hacer un "commit" de los cambios, crear una nueva rama si es
necesario, subir los cambios al repositorio remoto en GitHub.

Todas las operaciones se pueden realizar desde la terminal de RStudio.
Aquí hay algunos de los comandos más comunes que se utilizan en Git:

-   **git init**: Este comando se utiliza para crear un nuevo
    repositorio de Git. Se ejecuta en el directorio raíz del proyecto y
    establece la estructura necesaria para que Git rastree los cambios
    en el código fuente.

-   **git clone**: Este comando se utiliza para clonar un repositorio
    existente de Git. Es útil cuando se desea trabajar en un proyecto
    que ya está en GitHub o en otro servicio de alojamiento de
    repositorios de Git.

-   **git add**: Este comando se utiliza para agregar archivos nuevos o
    modificados al área de preparación "Stage" de Git. La preparación es
    el primer paso para confirmar los cambios en Git.

-   **git commit**: Este comando se utiliza para confirmar los cambios
    realizados en el repositorio de Git. Los cambios confirmados se
    guardan en la base de datos de Git y se etiquetan con un mensaje que
    describe los cambios.

-   **git push**: Este comando se utiliza para enviar los cambios
    confirmados a un repositorio remoto, como GitHub. Esto actualiza el
    repositorio remoto con los cambios realizados en el repositorio
    local.

-   **git pull**: Este comando se utiliza para actualizar el repositorio
    local con los cambios realizados en el repositorio remoto. Es útil
    cuando se está trabajando en un proyecto colaborativo y otros
    colaboradores han realizado cambios en el repositorio remoto.

-   **git branch**: Este comando se utiliza para crear, listar y
    eliminar ramas en el repositorio de Git. Las ramas son una forma de
    trabajar en diferentes versiones del proyecto sin afectar la rama
    principal.

-   **git merge**: Este comando se utiliza para fusionar ramas
    diferentes del repositorio de Git. Esto se utiliza comúnmente cuando
    se trabaja en diferentes ramas y se desea integrar los cambios
    realizados en una rama en la rama principal.

-   **git status**: Este comando se utiliza para verificar el estado del
    repositorio de Git. Proporciona información sobre los archivos que
    se han modificado y los archivos que se han agregado al área de
    preparación.

-   **git log**: Este comando se utiliza para ver un registro detallado
    de los cambios confirmados en el repositorio de Git. Muestra
    información como el autor del cambio, la fecha y la descripción del
    cambio.

Para conocer más a fondo la mecánica de Git es muy recomendable el
manual [@chacon2009] o la hoja resumen proporcionada por GitHub,
disponible en 
[https://training.github.com/downloads/es_ES/github-git-cheat-sheet.pdf](https://training.github.com/downloads/es_ES/github-git-cheat-sheet.pdf).



::: {.infobox_resume data-latex=""}
### Resumen {-}

- Git es un sistema de control de versiones distribuido utilizado para rastrear cambios en archivos a lo largo del tiempo, mientras que GitHub es una plataforma de alojamiento de código que utiliza Git como su sistema de control de versiones subyacente. 

- La instalación y configuración de Git y GitHub es sencilla y permite una colaboración eficiente y un control de versiones en el desarrollo de software. 

- Conectar GitHub y RStudio implica configurar las credenciales de Git, hacer cambios en los archivos y enviar los cambios al repositorio de GitHub. 

- El flujo de trabajo general en Git y GitHub implica inicializar un repositorio local, agregar archivos, comprometer cambios, crear una nueva rama si es necesario, enviar cambios al repositorio remoto, solicitar una solicitud de extracción si se trabaja en colaboración y fusionar cambios en la rama principal.

:::
