
# El procesamiento del lenguaje natural para tendencias de moda en textil {#nlp-textil}

*Ambrosio Nguema Ansue*

Diverger



## Introducción
\index{procesamiento del lenguaje natural, NLP}
\index{modelado de temas}
\index{análisis de tendencias}

El **procesamiento del lenguaje natural** (NLP, por sus siglas en inglés) es una rama de la inteligencia artificial que ayuda a los ordenadores a entender, interpretar y manipular el lenguaje humano. Abarca una amplia gama de técnicas y algoritmos, que incluyen: la categorización de contenido, el descubrimiento y modelado de temas, la extracción contextual, el análisis de sentimientos, la conversión de habla a texto y de texto a habla, el resumen de documentos y la traducción automática de texto o habla de un idioma a otro.

La técnica de NLP sobre la que descansa el caso de uso que se analiza en este capítulo, aunque no la única, es el  modelado de temas, que no es un modelo de predicción en sí mismo sino una técnica de aprendizaje no supervisado que tiene como objetivo descubrir estructuras ocultas (temas) dentro de un conjunto de documentos o textos.  

En este capítulo, se muestra cómo el modelado de temas y otras técnicas de NLP pueden aplicarse al análisis de tendencias en el mundo de la moda. El modelado de temas aplicado a la industria textil puede proporcionar información muy valiosa sobre las preferencias y opiniones de los clientes, lo que puede mejorar la toma de decisiones y la experiencia del cliente en el ámbito del comercio electrónico de ropa.





## Análisis de tendencias de moda en textil

En este caso de uso se analiza el conjunto de datos `clothes` incluido en el paquete `CDR`. Dicho conjunto contiene datos, reseñas y calificaciones proporcionadas por personas que compraron ropa de mujer a través del comercio electrónico. 

```r
library("CDR")
library("readr")
library("tidyverse")
library("tidytext")
```


Las variables incluidas pueden verse con la ejecutando `names(clothes)`. La descripción de dichas variables se lleva a cabo con el comando `??clothes`. El primer registro presenta la siguiente información:

```r
head(clothes)[1, ]
#>    ID Age Title                                                Review Rating
#> 1 767  33  <NA> Absolutely wonderful - silky and sexy and comfortable      4
#>   Recommend Liked  Division     Dept     Class
#> 1         1     0 Initmates Intimate Intimates
```

El conjunto de datos consta de 23.486 entradas que incluyen información acerca de la edad del cliente, las calificaciones otorgadas, sus opiniones, el rating que le asigna al producto comprado, el departamento y las recomendaciones sobre la ropa comprada en comercios electrónicos para mujeres. Estas variables se organizan en columnas. `Rating` y `Recommend` contienen valores enteros y el resto almacena caracteres. Todas las columnas con valores enteros están completas, mientras que algunas columnas de caracteres presentan valores faltantes (NA). La variable con la mayor cantidad de valores NA es `Title` (se refiere al encabezado del comentario). 


\index{bigrama}
\index{análisis!de texto}
\index{latent Dirichlet allocation, LDA}

El objetivo de este capítulo es explorar la aplicación de técnicas de análisis de texto en un conjunto de datos de opiniones, reseñas y calificaciones sobre ropa de comercio electrónico para mujeres. En primer lugar, se realiza el análisis del porcentaje de reseñas y calificaciones por departamento, destacándose aquellos con mayor y menor porcentaje. Posteriormente, se lleva a cabo un **análisis de bigramas** para identificar las frases asociadas con diferentes calificaciones. Finalmente, se utiliza el modelado de temas con el algoritmo **_latent Dirichlet allocation_** (LDA) para explorar las características clave de las opiniones correspondientes al departamento de "Tendencias". Los resultados del análisis proporcionan información muy valiosa para las empresas sobre el grupo demográfico objetivo, las preferencias de los clientes y las características clave de las prendas.

En cuanto al porcentaje de revisiones en cada departamento, el código necesario es el siguiente:

```r
clothes |>
  count(Dept) |>
  mutate(prop = n / sum(n)) |>
  ggplot(aes(x = Dept, y = prop * 100)) +
  geom_bar(stat = "identity", fill="blue") +
  xlab("Departamento") +
  ylab("Porcentaje de opiniones (%)") +
  geom_text(aes(label = round(prop * 100, 2)), vjust = -0.25) 
```

<div class="figure" style="text-align: center">
<img src="212058_cd_nlp_textil_files/figure-html/rev-departamento-1.png" alt="Porcentaje de revisiones por departamento." width="60%" />
<p class="caption">(\#fig:rev-departamento)Porcentaje de revisiones por departamento.</p>
</div>

La Fig. \@ref(fig:rev-departamento) muestra que un gran porcentaje de las opiniones y calificaciones corresponden a los departamentos o secciones de tops (44,57%) y vestidos (*dresses*) (26,91%). Por el contrario, el departamento de chaquetas (*jackets*) y la sección de tendencias (*trend*) apenas si reciben, entre ambos, el 5% de dichas opiniones y calificaciones. Dado que la sección de tendencias presenta una mezcla de ropa que puede pertenecer a varias secciones y únicamente representa el 0,51% del conjunto de datos (opiniones y calificaciones), por simplicidad, se ha decidido excluirla del análisis. 

A continuación se analiza la calificación (`Rating`) que recibe cada uno de estos departamentos. Esta calificación varía de 1 (muy mala) a 5 (muy buena).


```r
clothes |>
  filter(!is.na(Dept), Dept != "Trend") |>
  mutate(Dept = factor(Dept)) |>
  group_by(Dept, Rating) |>
  summarize(n = n()) |>
  mutate(perc = n / sum(n)) |>
  ggplot(aes(x = Rating, y = perc * 100, fill = Dept)) +
  geom_bar(stat = "identity", show.legend = FALSE) +
  facet_wrap(~Dept) +
  ylab("Calificaciones por departamento (%)") +
  geom_text(aes(label = round(perc * 100, 2)), vjust = -.2) +
  scale_y_continuous(limits = c(0, 65))
```

<div class="figure" style="text-align: center">
<img src="212058_cd_nlp_textil_files/figure-html/rating-departamento-1.png" alt="Distribución porcentual de las calificaciones por departamento." width="60%" />
<p class="caption">(\#fig:rating-departamento)Distribución porcentual de las calificaciones por departamento.</p>
</div>

En la Fig. \@ref(fig:rating-departamento) se observa que en todos los departamentos la calificación de 5 estrellas es, de largo, la más común. Aunque las chaquetas solo tienen un 4,39 % de reseñas (Fig. \@ref(fig:rev-departamento)), este departamento es el que tiene la mayor proporción de calificaciones de 5 estrellas. Una posible razón de esto es que las chaquetas suelen ser más fáciles de ajustar a diferentes formas corporales en comparación con vestidos y blusas, que pueden ser más difíciles de adaptarse correctamente, especialmente cuando se compran on line.

Si se analizan las reseñas u opiniones en cada departamento por grupos de edad (véase Fig. \@ref(fig:coment-edad-dto)), se observa, en primer lugar, que las personas de 30 a 40 años realizan la mayoría de las reseñas, seguidas por las de 40 a 50 y las de 50 a 60 años. Esto les da a las empresas una idea de quién es el grupo demográfico objetivo y qué tipo de ropa (camisas, vestidos) tiene más demanda. También se aprecia que la distribución del número de
reseñas por departamento se mantienen dentro de cada uno de los grupos de edad.





```r
ages <- clothes |>
  filter(!is.na(Age), !is.na(Dept), Dept != "Trend") |>
  select(ID, Age, Dept) |>
  mutate(Age_group = ifelse(Age < 30, "18-29", ifelse(Age < 40,
"30-39", ifelse(Age < 50, "40-49", ifelse(Age < 60, "50-59", ifelse(Age <
70, "60-69", ifelse(Age < 80, "70-79", ifelse(Age < 90, "80-89",
"90-99")
)))))))

ages |>
  filter(Age < 80) |>
  group_by(Age_group) |>
  count(Dept) |>
  ggplot(aes(Dept, n, fill = Age_group)) +
  geom_bar(stat = "identity", show.legend = FALSE) +
  facet_wrap(~Age_group, scales = "free") +
  xlab("Departmento") +
  ylab("Número de opiniones") +
  geom_text(aes(label = n), hjust = 1) +
  scale_y_continuous(expand = c(.1, 0)) +
  coord_flip()
```

<div class="figure" style="text-align: center">
<img src="212058_cd_nlp_textil_files/figure-html/coment-edad-dto-1.png" alt="Número de opiniones en cada departamento, por grupo de edad." width="60%" />
<p class="caption">(\#fig:coment-edad-dto)Número de opiniones en cada departamento, por grupo de edad.</p>
</div>




También se aprecia que la tendencia en la distribución de reseñas por departamentos (es decir, tops con el mayor número de reseñas y vestidos con el segundo mayor número) es similar en la mayoría de los grupos de edad. Esto indica que, en general, la popularidad de los diferentes tipos de ropa se mantiene en gran medida constante entre los grupos de edad más jóvenes y de mediana edad.



### Análisis de bigramas para el modelado de temas

\index{bigrama}
El análisis de bigramas es una técnica útil para identificar patrones y tendencias en el lenguaje utilizado en las opiniones sobre los productos. Un bigrama es un par de palabras consecutivas en un texto. El análisis de bigramas puede proporcionar una valiosa información sobre la frecuencia con la que ciertas palabras aparecen juntas y las combinaciones de palabras que son relevantes en las opiniones de los clientes. Lógicamente, el análisis de bigramas puede ser de gran ayuda para entender mejor las opiniones de los clientes.




```r
clothesr <- clothes |> filter(!is.na(Review))
notitle <- clothesr |>
  filter(is.na(Title)) |>
  select(-Title)
wtitle <- clothesr |>
  filter(!is.na(Title)) |>
  unite(Review, c(Title, Review), sep = " ")

main <- bind_rows(notitle, wtitle)
```

Para llevar a cabo el análisis de bigramas, se procede al procesamiento de las palabras incluidas en las opiniones, eliminando las palabras vacías, que son palabras de uso común sin un significado contextual importante. Una vez procesadas las palabras, se agrupan según sus calificaciones y se representan gráficamente (véase Fig. \@ref(fig:bigramas)) los 10 bigramas más comunes para cada nivel de calificación. De esta forma, se pueden identificar y comprender mejor las combinaciones de palabras relevantes en las opiniones de los clientes para cada nivel de calificación.
\index{bigrama}



```r
bigramming <- function(data) {
  cbigram <- data |> unnest_tokens(bigram, Review, token = "ngrams", n = 2)
  cbigram_sep <- cbigram |> separate(bigram, c("first", "second"), sep = " ")
  cbigram2 <- cbigram_sep |>
    filter(!first %in% stop_words$word, !second %in% stop_words$word, 
           !str_detect(first, "\\d"), !str_detect(second, "\\d")) |>
    unite(bigram, c(first, second), sep = " ")
  return(cbigram2)
}
```



```r
top_bigrams <- bigramming(main) |>
  mutate(Rating = factor(Rating, levels <- c(5:1))) |>
  mutate(bigram = factor(bigram, levels = rev(unique(bigram)))) |>
  group_by(Rating) |>
  count(bigram, sort = TRUE) |>
  top_n(10, n) |>
  ungroup()

top_bigrams |> ggplot(aes(bigram, n, fill = Rating)) +
  geom_col(show.legend = FALSE) +
  facet_wrap(~Rating, ncol = 3, scales = "free") +
  labs(x = NULL, y = "frequency") +
  coord_flip()
```

<div class="figure" style="text-align: center">
<img src="212058_cd_nlp_textil_files/figure-html/bigramas-1.png" alt="Bigramas más frecuentes (por nivel de calificación)" width="60%" />
<p class="caption">(\#fig:bigramas)Bigramas más frecuentes (por nivel de calificación)</p>
</div>
No hace falta decir que los bigramas tienen un tono positivo en las calificaciones más altas y negativo en las más bajas.



Finalmente, se genera una nube de palabras para mostrar
las palabras compartidas por los bigramas más comunes. Se analizan sólo las reseñas de 1
y 5 estrellas, para visualizar lo malo y lo bueno, respectivamente, de las opiniones (o prendas de vestir).


```r
onerating <- main |> filter(Rating == 1)
fiverating <- main |> filter(Rating == 5)
one <- bigramming(onerating) |> count(bigram, sort = TRUE)
five <- bigramming(fiverating) |> count(bigram, sort = TRUE)
```

Nube de palabras para 1 estrella (Fig. \@ref(fig:nube1)):

```r
library(wordcloud2)
wordcloud2(one |>
  filter(n > 5) |>
  mutate(n = sqrt(n)), size = .4 )
```

<div class="figure" style="text-align: center">

```{=html}
<div id="htmlwidget-e800dde012ff88e79b48" style="width:60%;height:480px;" class="wordcloud2 html-widget"></div>
<script type="application/json" data-for="htmlwidget-e800dde012ff88e79b48">{"x":{"word":["poor quality","cold water","cute top","beautiful dress","feels cheap","lay flat","maternity top","poor fit","sale price","usual size","cute online","dry clean","odd fit","potato sack","quality control","quality material","short waisted","arm holes","bad quality","body type","extra fabric","extremely disappointed","horrible fit","local retailer","love retailer","real life","retailer tops","shirt runs","weird fit"],"freq":[6.557438524302,3.74165738677394,3.16227766016838,3,3,3,3,2.82842712474619,2.82842712474619,2.82842712474619,2.64575131106459,2.64575131106459,2.64575131106459,2.64575131106459,2.64575131106459,2.64575131106459,2.64575131106459,2.44948974278318,2.44948974278318,2.44948974278318,2.44948974278318,2.44948974278318,2.44948974278318,2.44948974278318,2.44948974278318,2.44948974278318,2.44948974278318,2.44948974278318,2.44948974278318],"fontFamily":"Segoe UI","fontWeight":"bold","color":"random-dark","minSize":0,"weightFactor":10.9798970639475,"backgroundColor":"white","gridSize":0,"minRotation":-0.785398163397448,"maxRotation":0.785398163397448,"shuffle":true,"rotateRatio":0.4,"shape":"circle","ellipticity":0.65,"figBase64":null,"hover":null},"evals":[],"jsHooks":{"render":[{"code":"function(el,x){\n                        console.log(123);\n                        if(!iii){\n                          window.location.reload();\n                          iii = False;\n\n                        }\n  }","data":null}]}}</script>
```

<p class="caption">(\#fig:nube1)Nube de palabaras para la peor calificaión (1 estreslla).</p>
</div>


Nube de palabras para 5 estrellas (Fig. \@ref(fig:nube5)):

```r
wordcloud2(five |>
             filter(n > 10) |>
             mutate(n = sqrt(n)), size = .4)
```

<div class="figure" style="text-align: center">

```{=html}
<div id="htmlwidget-9e565088cb3979026562" style="width:60%;height:480px;" class="wordcloud2 html-widget"></div>
<script type="application/json" data-for="htmlwidget-9e565088cb3979026562">{"x":{"word":["love love","fit perfectly","fits perfectly","highly recommend","super cute","usual size","super soft","absolutely love","skinny jeans","fits true","beautiful dress","perfect fit","light weight","regular size","normal size","super comfortable","summer dress","super flattering","perfect length","local retailer","super comfy","sale price","fit perfect","runs true","fits perfect","cute top","form fitting","perfect summer","flattering fit","typically wear","local store","retailer tops","size medium","beautiful top","dress fits","green color","spring summer","arm holes","fits tts","broad shoulders","absolutely beautiful","petite size","cami underneath","pleasantly surprised","gorgeous dress","denim jacket","retailer store","soft fabric","summer top","blue color","body type","beautiful color","fall winter","xs petite","medium fit","figure flattering","top love","tank top","nice weight","perfect dress","fit true","nice quality","size xs","extremely comfortable","dress love","loose fitting","pencil skirt","absolutely gorgeous","beautiful sweater","bathing suit","tank underneath","flattering cut","layering piece","medium fits","retailer dresses","soft comfortable","tiny bit","xs fit","incredibly soft","dress runs","jean jacket","cute dress","loose fit","low cut","extremely flattering","drapes beautifully","fits beautifully","pink color","dress online","navy blue","regular xs","sweater coat","drapes nicely","hot summer","regular length","top fits","wear size","athletic build","body types","fit tts","nice touch","nude bra","previous reviewer","quality fabric","runs tts","xs fits","perfect fall","versatile piece","dry clean","favorite dress","flattering dress","run true","warm weather","white jeans","fits nicely","hard time","machine washable","petite sizes","fabric feels","medium weight","black pants","larger size","lovely dress","dress fit","lovely top","red color","sweater dress","beautiful colors","flip flops","soft material","summer days","wine color","bit loose","pear shaped","petite frame","petite xs","wardrobe staple","casual dress","flattering love","fun dress","eye catching","gorgeous top","nice fit","short torso","sleeve length","beautiful blouse","black leggings","comfortable dress","date night","lace detail","maxi dress","perfect top","perfect weight","previous reviews","slip underneath","swing dress","usual xs","bit snug","real life","strapless bra","white top","wide leg","casual top","cream color","hourglass figure","jeans love","short waisted","soft comfy","summer staple","comfortable flattering","cute love","incredibly flattering","pretty top","size xl","tee shirt","bit tight","black dress","elastic waist","pretty true","received compliments","top runs","versatile dress","beautiful detail","beautiful piece","comfortable fit","fall dress","hourglass shape","orange color","perfect amount","running errands","xxs petite","basic tee","blue motif","dark grey","faux fur","favorite retailer","flows nicely","grey color","hangs nicely","incredibly comfortable","length hits","neutral color","nice top","olive green","pretty dress","relaxed fit","shirt underneath","size larger","time finding","absolutely stunning","beautiful design","beautiful skirt","bra straps","cooler weather","excellent quality","hangs beautifully","leather jacket","nice fabric","post baby","tall boots","typical size","unique design","usual medium","xs regular","ankle length","beautiful fabric","byron lars","coral color","extra fabric","fit beautifully","medium petite","mid rise","natural waist","petite fit","petite sizing","recommend sizing","transition piece","waist band","absolutely perfect","comfortable love","easy breezy","favorite jeans","flattering top","fun top","hand wash","knee length","pilcro jeans","quality material","retailer clothes","statement piece","swing top","upper arms","wear xs","absolutely adore","ag jeans","bell sleeves","cute shirt","floral pattern","hot weather","polka dots","pretty color","skin tone","statement necklace","warmer weather","beautiful love","dress pants","dress perfect","ivory color","jeans fit","jeans shorts","lovely sweater","navy color","previous reviewers","absolutely adorable","amp amp","beautiful print","bit short","black jeans","bought size","cozy sweater","dark blue","dress size","fit love","fit nicely","highly recommended","holiday party","looser fit","neck line","nice drape","nice length","perfect spring","pilcro pants","regular bra","size fit","size fits","soft cotton","sweater love","tracy reese","white blouse","white shirt","beautiful shirt","boiled wool","bridal shower","casual wear","cute comfy","dark gray","fit fine","love wearing","nice flow","pear shape","perfect love","picture online","soft love","summer day","tad bit","top online","transitional piece","white pants","added bonus","ankle boots","bit sheer","black tights","bottom half","comfortable fabric","comfy love","dress beautiful","easily dressed","easy dress","fall sweater","favorite top","feet tall","hei hei","lays nicely","months ago","pants fit","pencil skirts","polka dot","price tag","received tons","rib cage","skinny pants","slightly loose","slightly sheer","store yesterday","summer nights","summer wardrobe","versatile top","beautiful details","beautiful soft","black motif","boyfriend jeans","bra underneath","color love","comfortable material","cute detail","cute skirt","deal breaker","holding horses","hot days","jacket love","light airy","nice soft","numerous compliments","peasant top","petite length","pictures online","retailer size","reviewer mentioned","run slightly","runs slightly","shirt fits","shorter length","staple piece","stunning dress","summer pants","sweater jacket","totally worth","unique dress","unique piece","weeks ago","worn casually","yoga pants","beige color","bright colors","business casual","casual day","classic retailer","cold water","cold weather","comfy top","dark navy","dress justice","dress makes","extra length","extremely soft","fall top","fall wardrobe","falls nicely","feel comfortable","flowy top","huge fan","light blue","light grey","light sweater","linen pants","mid thigh","months pregnant","moss color","muffin top","nice surprise","perfect sweater","received lots","regular fit","regular medium","retailer purchases","reviewers mentioned","shirt dress","shirt love","swing style","thicker fabric","top fit","vibrant colors","adorable dress","adorable top","beautiful lace","beautiful quality","bit boxy","bit shorter","black color","bra size","bra strap","bust line","camisole underneath","cloth stone","cute sweater","dress hits","fall colors","favorite sweater","feminine touch","finally found","fits fine","floral print","free shipping","gorgeous color","gray color","larger chest","length sleeves","love pilcro","low rise","material feels","meadow rue","nice color","nice detail","online photo","originally bought","pants love","peach color","perfect white","pretty blouse","soft light","special occasion","summer wedding","tee love","top heavy","top underneath","vegan leather","waisted jeans","wide legs","amazing dress","beautiful flattering","belly button","bit pricey","blue green","blue jeans","bright red","burnt orange","bust size","comfortable wearing","curvy figure","cute casual","cute summer","cute tee","denim shirt","favorite pair","favorite purchases","flattering comfortable","late summer","line shape","lovely soft","mid section","mock neck","model shot","narrow shoulders","nice addition","nice shape","normal bra","pale skin","perfect color","previous review","ran true","retailer clothing","retailer purchase","short waist","simply beautiful","skirt fits","skirt love","slim fit","spring dress","summer tank","surprisingly flattering","sweater underneath","thick material","top portion","weight sweater","ag stevie","beautiful pattern","bit wide","black friday","color combination","color combo","comfortable soft","comfortable stylish","comfortable top","cowl neck","fall weather","feel beautiful","feels soft","flat sandals","flattering shape","hem falls","light fabric","lighter weight","medium size","nice details","online photos","online picture","purple color","rich color","size love","sky blue","sleep pants","slimming effect","soft cozy","soft sweater","stretchy material","tad tight","tunic length","upper body","usual retailer","white button","white color","white dress","add bulk","awesome dress","baby weight","beautiful unique","bigger size","black lace","black top","black white","body skimming","button detail","casual summer","chest size","closet staple","customer service","cut outs","darker color","dress comfortable","dress looked","fabric love","fabulous dress","fall season","favorite purchase","favorite summer","feel pretty","feels amazing","flattering style","flows beautifully","gorgeous print","gorgeous skirt","gorgeous sweater","holiday season","looked amazing","lounge pants","love retailer","machine wash","maeve tops","maxi dresses","midi length","moss green","nice light","perfect pair","perfect shirt","perfect size","perfect tee","perfect winter","petite fits","pretty love","recently purchased","rehearsal dinner","rise jeans","sale rack","size chart","skin tight","slightly tight","soft warm","stop thinking","super easy","swing dresses","thin material","unique style","waist line","wear medium","white tank","white tee","write reviews","absolutely loved","amazingly soft","ankle jeans","arm length","bathing suits","beautiful comfortable","beautiful embroidery","beautiful fit","beautiful shade","bit low","black tank","bottom hem","boxy shape","brown color","burgundy color","comfy casual","cropped pants","cute fit","cute pants","dark denim","dark jeans","dark wash","dress cute","dry cleaning","easily dress","fall spring","favorite pants","favorite shirt","fits pretty","fits wonderfully","flare jeans","fun summer","hot humid","immediately drawn","love ag","lovely color","material makes","mid calf","nice dress","nice stretch","nice summer","peplum top","peplum tops","perfect casual","perfect denim","perfect lightweight","perfect piece","pilcro denim","purchased size","received numerous","reviewer noted","sales associate","season dress","short sleeve","short sleeves","sleeves hit","sleeves rolled","soft knit","straight leg","stretchy fabric","suggest sizing","summer months","super cozy","sweater fits","sweater knit","tall girls","wear heels","white denim","amazing fit","amazing quality","appears online","baby doll","beautiful beautiful","bit lower","blue version","boho chic","casual office","comfortable jeans","cool weather","cute comfortable","dark green","darling dress","day dress","denim skirt","dress bought","dress ran","dress yesterday","drop waist","easy fit","everyday dress","fall days","faux leather","fingers crossed","fits comfortably","gold color","gorgeous colors","hang dry","heavier weight","jean shorts","jeans leggings","jeans pants","lace top","layer underneath","leg pants","light material","lovely fabric","maternity dress","maternity top","mid weight","multiple compliments","nice material","nice pair","nice skirt","originally purchased","perfectly love","plum color","pretty colors","pretty short","recommend wearing","scoop neck","shift dress","shirt runs","simple top","skinny jean","slightly oversized","slim pants","soft flattering","soft stretchy","substantial fabric","summer fall","summer tee","super happy","sweater material","teal color","top perfect","true size","wear mine","weight fabric","white background","white version","winter dress","wonderfully soft"],"freq":[22.248595461287,19.6468827043885,19.4679223339318,18.411952639522,18.3575597506858,16.7630546142402,16.6433169770932,16.2788205960997,15.9373774505092,13.856406460551,13.5646599662505,12.8840987267251,12.7279220613579,12.5299640861417,12.369316876853,11.9582607431014,11.6619037896906,11.4017542509914,11.3137084989848,11.1803398874989,11.1803398874989,11,10.6770782520313,10.4403065089106,10.3923048454133,10.3440804327886,10.1980390271856,10,9.9498743710662,9.8488578017961,9.74679434480896,9.53939201416946,9.48683298050514,9.32737905308882,9.2736184954957,9.21954445729289,9.21954445729289,9.16515138991168,9.16515138991168,9.1104335791443,8.94427190999916,8.94427190999916,8.88819441731559,8.77496438739212,8.71779788708135,8.66025403784439,8.66025403784439,8.60232526704263,8.54400374531753,8.48528137423857,8.48528137423857,8.42614977317636,8.36660026534076,8.36660026534076,8.30662386291807,8.24621125123532,8.24621125123532,8.12403840463596,8.06225774829855,8.06225774829855,8,8,7.87400787401181,7.81024967590665,7.54983443527075,7.54983443527075,7.54983443527075,7.48331477354788,7.48331477354788,7.41619848709566,7.41619848709566,7.34846922834953,7.34846922834953,7.34846922834953,7.34846922834953,7.34846922834953,7.34846922834953,7.28010988928052,7.21110255092798,7.14142842854285,7.14142842854285,7.07106781186548,7.07106781186548,7.07106781186548,7,6.92820323027551,6.92820323027551,6.92820323027551,6.85565460040104,6.85565460040104,6.85565460040104,6.85565460040104,6.78232998312527,6.78232998312527,6.78232998312527,6.78232998312527,6.78232998312527,6.70820393249937,6.70820393249937,6.70820393249937,6.70820393249937,6.70820393249937,6.70820393249937,6.6332495807108,6.6332495807108,6.6332495807108,6.557438524302,6.557438524302,6.48074069840786,6.48074069840786,6.48074069840786,6.48074069840786,6.48074069840786,6.48074069840786,6.40312423743285,6.40312423743285,6.40312423743285,6.40312423743285,6.32455532033676,6.32455532033676,6.2449979983984,6.2449979983984,6.2449979983984,6.16441400296898,6.16441400296898,6.16441400296898,6.16441400296898,6.08276253029822,6.08276253029822,6.08276253029822,6.08276253029822,6.08276253029822,6,6,6,6,6,5.91607978309962,5.91607978309962,5.91607978309962,5.8309518948453,5.8309518948453,5.8309518948453,5.8309518948453,5.8309518948453,5.74456264653803,5.74456264653803,5.74456264653803,5.74456264653803,5.74456264653803,5.74456264653803,5.74456264653803,5.74456264653803,5.74456264653803,5.74456264653803,5.74456264653803,5.74456264653803,5.65685424949238,5.65685424949238,5.65685424949238,5.65685424949238,5.65685424949238,5.56776436283002,5.56776436283002,5.56776436283002,5.56776436283002,5.56776436283002,5.56776436283002,5.56776436283002,5.47722557505166,5.47722557505166,5.47722557505166,5.47722557505166,5.47722557505166,5.47722557505166,5.3851648071345,5.3851648071345,5.3851648071345,5.3851648071345,5.3851648071345,5.3851648071345,5.3851648071345,5.29150262212918,5.29150262212918,5.29150262212918,5.29150262212918,5.29150262212918,5.29150262212918,5.29150262212918,5.29150262212918,5.29150262212918,5.19615242270663,5.19615242270663,5.19615242270663,5.19615242270663,5.19615242270663,5.19615242270663,5.19615242270663,5.19615242270663,5.19615242270663,5.19615242270663,5.19615242270663,5.19615242270663,5.19615242270663,5.19615242270663,5.19615242270663,5.19615242270663,5.19615242270663,5.19615242270663,5.09901951359278,5.09901951359278,5.09901951359278,5.09901951359278,5.09901951359278,5.09901951359278,5.09901951359278,5.09901951359278,5.09901951359278,5.09901951359278,5.09901951359278,5.09901951359278,5.09901951359278,5.09901951359278,5.09901951359278,5,5,5,5,5,5,5,5,5,5,5,5,5,5,4.89897948556636,4.89897948556636,4.89897948556636,4.89897948556636,4.89897948556636,4.89897948556636,4.89897948556636,4.89897948556636,4.89897948556636,4.89897948556636,4.89897948556636,4.89897948556636,4.89897948556636,4.89897948556636,4.89897948556636,4.79583152331272,4.79583152331272,4.79583152331272,4.79583152331272,4.79583152331272,4.79583152331272,4.79583152331272,4.79583152331272,4.79583152331272,4.79583152331272,4.79583152331272,4.69041575982343,4.69041575982343,4.69041575982343,4.69041575982343,4.69041575982343,4.69041575982343,4.69041575982343,4.69041575982343,4.69041575982343,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.58257569495584,4.47213595499958,4.47213595499958,4.47213595499958,4.47213595499958,4.47213595499958,4.47213595499958,4.47213595499958,4.47213595499958,4.47213595499958,4.47213595499958,4.47213595499958,4.47213595499958,4.47213595499958,4.47213595499958,4.47213595499958,4.47213595499958,4.47213595499958,4.47213595499958,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.35889894354067,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.24264068711928,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4.12310562561766,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,4,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.87298334620742,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.74165738677394,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.60555127546399,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.46410161513775,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554,3.3166247903554],"fontFamily":"Segoe UI","fontWeight":"bold","color":"random-dark","minSize":0,"weightFactor":3.23615933982356,"backgroundColor":"white","gridSize":0,"minRotation":-0.785398163397448,"maxRotation":0.785398163397448,"shuffle":true,"rotateRatio":0.4,"shape":"circle","ellipticity":0.65,"figBase64":null,"hover":null},"evals":[],"jsHooks":{"render":[{"code":"function(el,x){\n                        console.log(123);\n                        if(!iii){\n                          window.location.reload();\n                          iii = False;\n\n                        }\n  }","data":null}]}}</script>
```

<p class="caption">(\#fig:nube5)Nube de palabaras para la mejor calificaión (5 estresllas).</p>
</div>



### Modelado de temas con LDA

El enfoque de modelado de temas con LDA es una técnica ampliamente utilizada en NLP para extraer temas latentes de un corpus de texto. LDA es un algoritmo no supervisado que utiliza el aprendizaje automático para identificar patrones en grandes conjuntos de datos de texto, agrupando palabras similares en temas y asignando probabilidades a cada tema en cada documento.


En este caso de uso se ha utilizado el enfoque de modelado de temas de LDA para explorar las 118 opiniones vertidas sobre prendas del departamento de Tendencias. Se ajusta un modelo LDA utilizando muestreo de Gibbs y se elige $k = 5$ (donde *k* es el número de palabras principales de cada tema) para los departamentos de *Bottoms* (pantalones/faldas), *Dresses* (vestidos), *Intimate* (ropa interior), *Jackets* (chaquetas) y Tops. Como resultado se identifican las 5 palabras principales de cada tema. De esta forma, se obtiene una información muy valiosa sobre las preferencias y opiniones de los clientes en diferentes departamentos.




```r
library("topicmodels")
library("tm")
library("LDAvis")

trend_count <- main |>
  filter(Dept == "Trend") |>
  unnest_tokens(word, Review) |>
  anti_join(stop_words, by = "word") |>
  filter(!str_detect(word, "\\d")) |>
  count(ID, word, sort = TRUE) |>
  ungroup()

trend_dtm <- trend_count |> cast_dtm(ID, word, n)
trendy <- tidy(LDA(trend_dtm, k = 5, method = "GIBBS", 
                   control = list(seed = 4444, alpha = 1)), matrix = "beta")

top_trendy <- trendy |>
  group_by(topic) |>
  top_n(5, beta) |>
  ungroup() |>
  arrange(topic, desc(beta))

top_trendy |>
  mutate(term = reorder(term, beta)) |>
  ggplot(aes(term, beta, fill = factor(topic))) +
  geom_col(show.legend = FALSE) +
  facet_wrap(~topic, scales = "free") +
  coord_flip()
```

<div class="figure" style="text-align: center">
<img src="212058_cd_nlp_textil_files/figure-html/lda-1.png" alt="Modelo LDA (k=5)." width="60%" />
<p class="caption">(\#fig:lda)Modelo LDA (k=5).</p>
</div>

En el modelo LDA (Fig. \@ref(fig:lda)) cada tema se representa por un conjunto de palabras que aparecen juntas con mayor frecuencia en las opiniones de los clientes. Por ejemplo, el tema 3 se caracteriza por palabras como *colors* (colores), *wear* (vestir), *bit* (un poco), *jacket* (chaqueta) y *price* (precio), lo que sugiere que los clientes pueden estar comentando sobre la variedad de colores disponibles, la durabilidad de la prenda y su precio. Por otro lado, el tema 1 se caracteriza por palabras como *love* (amor), *fit* (ajuste), *fabric* (fábrica), *wear* (vestir) y *length* (largo), lo que sugiere que los clientes pueden estar hablando sobre su experiencia con la prenda en términos de comodidad, ajuste y calidad de la tela. Al identificar estos temas, se pueden obtener ideas valiosas sobre las opiniones y preferencias de los clientes que ayuden a mejorar la calidad de la ropa y satisfacer sus necesidades y deseos. Esto permite a las empresas tomar decisiones informadas para satisfacer las necesidades de sus clientes y mejorar la experiencia del usuario en el ámbito del comercio electrónico de ropa.



Como conclusión, destacar que el análisis de este conjunto de datos proporciona información valiosa sobre las preferencias y opiniones de los clientes en cuanto a la ropa femenina. Las reseñas de 5 estrellas son dominantes en cada departamento, y las chaquetas son las prendas que obtienen la proporción más alta de calificaciones positivas. Además, se ha observado que los clientes de entre 30 y 40 años dejan la mayoría de las reseñas y que factores como el ajuste, la comodidad/calidad del material y la estética de la prenda influyen en la calificación. La realización de análisis de datos exploratorios y de bigramas puede ayudar a las empresas a comprender mejor lo que funciona y lo que no. Por último, el modelado de temas con LDA es una herramienta útil en situaciones en las que se tienen reseñas sin marcar y puede proporcionar mucha información sobre las características clave de las opiniones. En general, estos análisis pueden ayudar a las empresas a tomar decisiones informadas y mejorar la experiencia del usuario en el ámbito del comercio electrónico de ropa.



<img src="img/LogoCDR_transparente.png" width="15%" style="display: block; margin: auto;" />
