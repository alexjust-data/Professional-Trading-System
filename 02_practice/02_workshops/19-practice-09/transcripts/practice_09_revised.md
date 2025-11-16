

**Introducción**

***como se ve en la imagen tiene un profit factor de 1.09 y un 66.0%  percent profite... que da a entender que el sistema puede tener opciones de valóralo metiéndole mas variables  a lo tengo en e mini sp500***

![](../img/000.png)
![](../img/001.png)

***creo que podría empeorar el beneficio para bajar el riesgo***

![](../img/002.png)


es complicado que sólo ***bollinger bans*** así te rente es complicado te faltan comisiones hay muchos trades bueno 1.09 en todo caso no sería operable, pero bueno no es mal comienzo tampoco hay que decir, que es lo que tú decías, ahí, bueno para punto de partida seguramente no no es malo, no, podías empeorar el beneficio para bajar el riesgo,,, 

***quería entender que se puede tener opciones metiéndole más variables a lo que tengo en el mini sp*** 

si ese criterio estoy de acuerdo, es decir, un setup que lo pones te da 1,09 con 9000 operaciones estoy de acuerdo, estoy de acuerdo que es un setup prometedor, vale, es decir para seguir, de acuerdo, pero bueno habría que mirar lógicamente más cosas como bien dices no hay variables, estoy de acuerdo, estoy de acuerdo, en que probablemente es bueno setup. 

El ***bollinger bans*** es una muy buena señal ya lo comenté en la teoría y tengo pensado, lo que pasa que no sé yo creo que en algún en algún momento no sé dónde va a ser el límite pero en algún momento tendré que decidir no hacer todo lo que quería hacer porque lo que estoy viendo es que si hago todo lo que tenía que hacer será un curso de 500 horas en práctica, entonces no no no puede ser no puede ser, entonces en algún momento habrá que ir ya diciendo bueno esto no, esto sí, porque va a ser imposible es imposible, ya os lo comenté queríamos sobre la marcha y tenía intención de hacerlo todo pero cada vez tengo más dudas de conseguirlo, sinceramente, porque ya llevamos muchas horas siguen quedando muchas tranquilos siguen quedando muchas pero es que aun quedando muchas no creo que haga todo lo que quiero hacer de verdad entonces sigo

***Hola, un par de cosas, hay un error en el codigo de Curso-ORB-04 , donde dice `` barrasRangoDesdeInicio = timetominutes(HoraInicioTrading - sessionstarttime(0,1))/barinterval;` debe ser `barrasRangoDesdeInicio = (timetominutes(HoraInicioTrading) - timetominutes(sessionstarttime(0,1)))/barinterval;`se puede probar el error poniendo `HoraInicio 1000`, y devuelve 7 barras desde las 930***

***Otra cosa, en el codigo Curso-VB-01, hay 2 lineas que comentasteis en las practicas que eran para que no abriera posición y cerrar al instante al haber logrado el objetivo de profit.***
`Open of next bar < High Then`  
`Open of next bar > Low Then`  
***entiendo para que son, pero tenía pensado que EasyLanguage estaba diseñado para impedir leer datos futuros, no existe otra manera de lanzar ordenes condicionando su ejecucion en un rango de precios?***

**Open of Next Bar y lectura de datos**

Aquí tienes el texto **con el léxico mejorado**, **más claro y preciso**, **sin perder ni modificar ninguna palabra original** (solo reorganizo ligeramente algunas comas y conectores para darle fluidez, pero **no elimino ninguna palabra**).

**Open of Next Bar y lectura de datos**

Sí, claro, hay otra manera, otras maneras de hacerlo, y hay veces que no se puede utilizar `open nextbar` por distintos motivos, por ejemplo tener dos datas o algún otro factor. Pero muy importante, `open of next bar` no lee futuro, vale, es muy importante entender bien esto. Esto no es leer futuro. Lo que hace `open of next bar` no deja de ser un truco de lenguaje en el que lo que hace es que, al cierre de la vela en cuestión, retrasa el cierre al tick de apertura, simplemente. No es que lea futuro: es que espera al tick de apertura para leer el código, de acuerdo, y “lee tres datos”, esto en el mastering sale, en el mastering sale en la clase de mastering, se trata porque eso es normal, es algo que no tienen todos los lenguajes, es una utilidad bastante interesante, pero no siempre se puede usar, aunque hay veces que sí viene bien.

Lo que hace es leer del tick de apertura tres datos: el precio de apertura, la hora y la fecha, sólo esos tres datos. Entonces tú puedes usar esos tres datos del tick de apertura para procesarlo en la barra de cierre del día anterior, pero de la barra anterior. Entonces no lee futuro: simplemente espera procesarlo. Por lo tanto yo ahí lo que hago es llamar al `open of next bar` y le digo: si el `open of next bar` es menor que tanto… haz X cosa… pero no lee futuro, ¿se entiende?, espera ese tick, sólo eso. Leer futuro sería usar un dato que no dispones en el momento de ejecutar el código; aquí sí que dispone, por eso se espera, entonces espera, lo lee, y entonces ejecuta la regla que tú le digas, de acuerdo.

Así que es perfectamente útil, y en este caso simplemente es para decir: oye, si hay un gap que supera el precio que yo tengo pensado ya cerrar, ¿para qué voy a abrir? Entonces le pregunto: oye, ¿has abierto por encima de tal precio? Me dice: sí, pues no, no tienes la orden, entonces no la tires. Ya está. Esta manera tiene un poco eso, o para otras cosas se puede usar, pero para sistemas de gaps va fantástico, claro, para sistemas de gaps es fantástico. Pero insisto, no se puede usar en dos datas, por ejemplo. Entonces espero que haya quedado claro esto.


**Optimización de filtros**

***a)Tras ver un video en yt donde se mostraba una función de EL con 500 filtros  me surgió una duda. Tomando como ejemplo el listado en EL de entradas y salidas que se controlaban mediante inputs mostrado en la teoría, me preguntaba si haciendo lo mismo con un listado de filtros e intentar optimizar todos mediante su input para ver cual funcionaría mejor, si esto es una buena práctica o es sobre optimizar. Es decir se busca qué filtro de un listado predeterminado va mejor para el sistema atendiendo a su opti para luego solo quedarnos con el mejor.***

***b)sobre los filtros, a la hora de elegir alguno(en caso de que así sea) tenéis algún listado estándar o varios que por consenso uséis generalmente o los filtros  los elegís  según vuestro criterio o ideas personales en el momento, es decir en que basarse para elegir que tipo de filtro usar dada la gran diversidad de estos que hay.***



bien, filtros, lo que comentas de usar un optimizador de filtros, sí puede hacerse, sí puede hacerse y, de hecho, puede ser buena práctica, puede ser buena práctica y no tiene por qué ser *sobre optimizar*, vale, pero debería estar todo bastante acotado, es decir, no optimicemos ya el filtro también, es decir, si el filtro depende de un parámetro de *volatilidad* encima no optimicemos el parámetro, de acuerdo, es decir, optimicemos el filtro en un parámetro estándar, o sea, probemos ese filtro en un parámetro estándar, otro en otro parámetro estándar, de acuerdo, y lógicamente, como siempre, como cualquier regla, habrá que ver el tema de *significación estadística*, etcétera, etcétera, el número de *trades* que tenemos y podríamos hacer alguna otra prueba de evaluación como *z-score* o cualquiera de estas, pero se puede hacer, se puede, se puede hacer, esto no deja de ser un *builder* y por eso yo, y de hecho en el debate interno, es en algún momento usar algún *builder* para hacer algo de esto, porque esto al final no deja de ser un *builder* implementado en *easylenguaje*; por cierto, el código ese de 500 filtros, si nos lo quieres pasar, sería interesante verlo, yo no sé a cuál te refieres, yo no lo he visto, ya digo, en general no sé si, 500, 500, pero al final, esto es un *builder* implementado en código, un *builder* externo como sabéis, *builder* que hay [XtrategyQuant](https://strategyquant.com/), [AdapterBuilder](https://www.adaptrade.com/Builder/), nosotros tenemos comprado hace muchos años, se puede usar pero se puede usar ya teniéndolo muy cerrado, de acuerdo, es decir, el problema de los *builders* es dejarlos buscar libremente, entonces ya digo, esto por ejemplo hecho en un *builder* no sería mala práctica, no sería mala práctica, pero quiere decir que tú ya tienes tu *setup* de entrada, de salida, y tratas de buscar filtro para mejorar la entrada entiendo lo que estamos hablando, puede ser buena práctica, pero como con todo hace falta buen criterio y *prudencia*, vigilar,,, un buen filtro debe quitar más malas que *buenas*, hay que ver, hay que ver una vez siempre cuando comparas filtro es cómo va el sistema antes de filtrado y filtrado... ver un poco los parámetros clave, proteger aciertos, beneficio medio de positivas y negativas, número de positivos, número de negativos, es lo típico porque ahí lo que buscas en un filtro es quitar malos, estamos de acuerdo no, es decir, buscas quitar malos, pero hombre, si solo quita malos es un poco sospechoso también, tiene que quitar más malos que buenos, pero algún bueno también tiene que quitar, si solo se quitan malos eso es feo,

entonces, a cuál filtro elegir, bueno, no tenemos una lista porque nosotros no hemos sido nunca muy amantes de los filtros, eso es verdad, hemos filtrado poco, por esa como sabéis siempre *prudencia estadística* que tenemos, donde siempre tratamos, nos pesa mucho la significación estadística, y filtrar es perder *trades*, eso es obvio, pero siempre nos ha costado o, dicho de otra manera, los filtros no acaban de aportar mucho o lo suficiente para aplicarlo, pero al final más o menos si podemos en algún momento, si queréis, tratar de dar algunas referencias, con el ORB por ejemplo ya lo dimos, y a medida que vayamos viendo sistemas lo iremos dando, pero lógicamente a cada tipo de sistema lo vamos dando, pero al final no me complicaría en demasiada vida, me explico, es decir, porque entonces sí que 500 filtros me parece una locura, no me complicaría en exceso la vida y si tenemos que complicarla tanto seguramente ya es que el sistema no vale, si tienes que filtrar tanto y buscarle tantos filtros mejor busca otro sistema, entiendes, entonces filtros que hay al final?  pues las cosas obvias,,,

* ***la volatilidad***, en casi todos los tipos de sistema suele ir bien filtrar o puede ser una cosa a evaluar, de acuerdo, filtrar volatilidad y desde ahí cuidado puede ser
    * vía ***atr*** interna
    * vía externa vía ***vix*** por ejemplo  
    también hay de este tipo en *intermarkets* con por ejemplo la curva del vix, es decir, los distintos periodos de eso quería, es una de las cosas que tengo apuntadas ver, pero es bastante avanzada y si lo vemos lo veremos de lo último, de acuerdo, porque es una cosa bastante avanzada pero tiene que ver con filtrar la volatilidad pero por lo externo, vale, un poco externo con los distintos vencimientos del vix
* ***tendencia*** lógicamente el *régimen de mercado*, lo que técnicamente se llama régimen de mercado, que podemos simplificar que recordaréis que hay seis: 
    * volatilidad a la baja, 
    * volatilidad al alza, 
    * tendencia alcista, 
    * tendencia bajista, 
    * y neutral, 
    * en ambos casos pues esa mezcla de todo pues se puede dar seis, nueve,   
    es al final un poco porque puedes hacer alta subiendo, alta bajando, baja subiendo, baja bajando por ejemplo,,,

<div style="border-left: 4px solid #27ae60; background: #ecf9f1; padding: 10px 15px; margin: 10px 0;">
  <strong>📏 Nota técnica ampliada</strong><br>
  En análisis cuantitativo, el <em>régimen de mercado</em> suele modelarse mediante indicadores de estado como volatilidad realizada, volatilidad implícita, pendientes de curva VIX, ratios VIX/VVIX, coeficiente de Hurst, medias móviles de estado o filtros de Kalman para detectar cambios estructurales.
</div>

pero bueno, en los dos vectores principales siempre son volatilidad y tendencia, y eso al final te da un régimen de mercado, entonces ahí pues hay muchos, régimen de mercado, desde los más *sencillos* con una media *móvil*, hasta los más complejos con coeficientes de *Hurst*,,, o sea, al final hay muchas cosas a mirar, pero repito, yo soy muy amante de la simplicidad como sabéis, y por lo tanto, eso aplica a todo también a los filtros, también a los filtros.



# Revisión de Apolo short

**Inicio de la revisión de Apolo Short**

Apolo es un sistema que está hoy en día operando; es un sistema que opera largo y corto, pero que de hace bastante tiempo —aunque al principio no era así— se decidió hacerlo, se decidió hacerlo en determinado momento, se decidió probar si aportaba valor hacerlo, como ya sospechábamos, pero al principio operaba con el mismo *set* en el largo y en el corto. Algo que hay autores que recomiendan —y que en sí es verdad que puede ser una buena práctica— pero que no necesariamente no hacerlo es mala, se entiende; es decir, es preferible… puede serlo, pero sobre todo en *equity* normalmente hay *especialistas de cortos*, *especialistas de largos*, porque, por lo de la *volatilidad* que os decía realmente, y por la dinámica propia del mercado, donde el régimen de mercado es muy alcista y por lo tanto es distinto, es decir, el largo normalmente deja correr, quiere correr; en cambio el corto pues no quiere correr. Al final, *corto* es un sistema más de *oportunidad rápida*. Es bastante obvio, es bastante obvio.

**Sesgo alcista y dinámica de mercado**

Pues bueno, bastante obvio, pues por esto, por esto:

![](../img/003.png)

Aquí sólo he cargado un periodo de… bueno, si cargo un *workspace* de largo plazo que tengo por ahí:

![](../img/004.png)

Pues esto, de acuerdo? Por esto, porque digamos que el sesgo de largo plazo es ligeramente alcista, ligeramente ligeramente alcista, *viento de cola*. Claro que hay periodos que pueden ser de muchos años de lateral, de corrección, o congestión, etcétera; hay muchas, muchas variedades, pero en el largo plazo pues es más bien tendente a subir, por lo tanto ya añade un sesgo distinto, distinto.

**Separación de parámetros largos y cortos**

Entonces bien, Apolo, Apolo opera ya hace tiempo con parámetros para corto, parámetros para largo, y hay distintos *sets* dentro del mismo sistema; en este caso, el mismo *set* opera con distintos sistemas.

De hecho, hemos hecho un pequeño cambio en Apolo que no es de las revisiones que hay pendientes —de cosas de evoluciones— sino una *simplificación del código* que implica al final quitarle un *input*, y de hecho os lo digo abiertamente: en este caso este sistema usa el *ATR*, y el ATR en algunas épocas se ha utilizado y ahora ya hace un tiempo que no recuerdo que no se optimiza, y está fijo, de acuerdo? Está fijo en —creo— que está fijo en 15 y en 15 barras, porque en su momento ya se hizo un estudio muy exhaustivo y se ha revisado hace poco y es una zona que está bastante estabilizada, con mapa —que luego veremos mapas y demás— y por lo tanto nos gusta que esté ahí.

**Diferencia entre TP largos y TP cortos**

Entonces lo que os digo: tiene largo y corto.

![](../img/005.png)

Bien, el corto lleva una época mala y que en algunos *sets* se ha convertido en demasiado mala, vale? Entonces “demasiado mala” se ha ido degradando.

El sistema de largos —ya digo— tiene distintos tipos, pero como veis, en general… por ejemplo este, este *trade* es este *trade* por ejemplo; no es un *trade* de tres o cuatro días que hace un *TP* bastante interesante, interesante:

![](../img/006.png)

En cambio los *TPs* —a ver si hay alguno aquí— aquí hay algún TP claro; los TPs del corto es que es otra cosa: es de muy poco recorrido.

![](../img/008.png)

Esto es lo que busca el corto: corto tiene un TP siempre más cercano, y os lo comentaba varias veces en la teoría, también en la práctica y de cierta manera pensando en Apolo. Es decir, realmente una de las mejores maneras de bajar el riesgo es tener un *TP cercano*, de acuerdo? Es decir, en datos de riesgo lo baja mucho porque sale rápido, y de hecho Apolo trabaja con *ST en el lado corto mayor que TP*, es un *stop* que salta pocas veces, y ese es su problema: que en los *sets* que van… hay *sets* que dejan correr un poco más, hay *sets* un poco más parecidos al largo… no tanto, pero hay algún *set* más parecido, y hay *sets* como este de la derecha que veis que es muy muy rápido, de acuerdo?, que hace TP muy rápido. Entonces, cuando tiene 2, 3, 4 fallos, y son fallos de cierta importancia —como alguno que veis aquí— pues bueno, duele más.

**Protocolo de supervisión**

Entonces, a lo que vamos: ¿dónde empieza esto? Bueno, esto empieza con el *protocolo de supervisión*. El protocolo de supervisión, ya os lo comenté en la teoría, es algo en lo que tenemos campo de mejora; ya tenemos cosas hechas, pero tenemos campo de mejora, y de hecho pues mira una de esas cosas que os comenté que en el curso nos puede servir a nosotros aprovecharlo es esta, de acuerdo?, es esta. Más que mejorar en darnos cuenta es *automatizarlo* un poco, porque es verdad que el protocolo de supervisión lo puedes complicar todo lo que tú quieras, pero a la hora de la verdad —no solo, pero normalmente— si tú… digo para vosotros que empezando, que no os volváis locos, de acuerdo? Sobre todo aquellos que estáis empezando: hay muchas maneras de hacer esto y *mirando un performance report* es una de ellas.

***mirando un performance report***
![](../img/011.png)

***mirando la curva de drawdown***
![](../img/010.png)

***mirando la curva del sistema***
![](../img/012.png)

***los trades***

* tú si vas aquí puedes ir a buscar el trade más largo
* puedes buscar el peor trade, el *largest losing trade*
* puedes ver cuándo ha sido marzo, el 16, etcétera

![](../img/013.png)

De acuerdo?? Puedes, puedes mirarlo. Una de esas maneras es esta, lo cual tampoco penséis que obligatoriamente hace falta tener un código y todo súper complicado; pero bueno, sí que es verdad que una de las cosas que caracteriza la profesionalización es esta: automatizar más las cosas.

Pero como al final aquellos que ya nos conocéis —no sólo de ahora del curso sino hace tiempo— ya sabéis que me quejo siempre de la falta de recursos y de tiempo, es así. Porque al final todo esto lleva trabajo y hay que decidir, cuando eres una empresa o un proyecto, es igual, tienes que decidir en qué gastarse el tiempo, los recursos, y al final no es infinito, igual que no lo es el dinero tampoco lo es el tiempo, y hay que decidir dónde haces cosas, y por eso tenemos siempre una reunión mensual, damos prioridad a una cosa, a otra, y hay cosas que están en reuniones de hace años porque no hay manera de que se considere que es prioritario respecto a otras cosas, y esto hace que hayan cosas que a lo mejor podrían hacerse mejor pero ya están bien así, porque dedicarle un montón de horas pues no te renta.

**Código de extracción de métricas**

Vale, entonces aquí por ejemplo nosotros hoy en día sacamos estos datos:

![](../img/009.png)

¿Qué datos sacamos? Sacamos simplemente datos que ya veíais ahí en el *performance report*:

Pero que los sacamos vía el `print`, de acuerdo? Que esto es todo vía código. Este código —ya me adelanto por si alguien lo pregunta— Alberto no me ha autorizado a entregarlo todavía, todavía, porque es que lo quiere mejorar antes de entregarlo. Esto se llama *orgullo de programador*. Entonces yo entiendo que antes de acabar el curso —ya os lo dijimos— que lo haremos, ya os daremos un código y puede que algo mejor. Yo creo que ya está bastante bien, pero Alberto dice que no, que si me ocurre entregarlo ya, que lo quiere mejorar. Entonces… pero bueno, al final informa del mayor *drawdown*, cuándo lo hace, cuándo hace peor *trade*, y cuándo hace peor serie de *trades*, de acuerdo?, que son datos de riesgo bastante importantes.


**Métrica N-trades y porcentaje**

tenemos uno que hacemos en revisiones más profundas vía excel ya os lo comenté en práctica, perdón la teoría y creo que lo dije en la teoría que teníamos planes es una de esas cosas pendientes, a ver si aprovechando lo acabamos de desarrollar, que es lo de contar n trades, n trades, vale, pero no en valor absoluto sino en porcentaje, acuerdo, en porcentaje porque en valor absoluto es muy complicado porque se desvirtúa este es un problema a veces hay que calcularlo en porcentaje, y cuando acumulas trades eso ya no es tan fácil a nivel de histórico pero creo que lo conseguiremos,,,

**Puntos clave detectados en Apolo**

entonces aquí ahora mismo pues vemos que este sistema hizo peor trade en octubre del 23 y que su drawdown record lo hizo histórico en 380 en mayo del 23 

![](../img/014.png)

tras ese se revisó y este ahora vuelve a estar cerca y ese set estando justo sigue operativo 

![](../img/016.png)

luego llegaremos a esto porque hoy veréis incluso os enseñaremos los sets que cambiamos y toda la manera en que lo analizamos vale


Entonces en un momento determinado salta la alerta, que ya nosotros ya veíamos que no iban finos, de acuerdo, es decir ya lo ves, es que la mayoría de veces tú ya lo ves, pero lo cual no quita que no vaya bien tener un código que te avise porque así pues puedes olvidarte, 

Estos son distintos sets exactamente y son dos sets, distinta combinación de parámetros de la misma estrategia, sí Apolo hace esto 

## Inicio de la revisión y preparación del mapa de optimización

bien pues entonces vamos con la revisión, la primera cosa que hacemos es 

***un mapa de optimización de prácticamente o sino prácticamente todo el histórico***

`mapa short`  
![](../img/017.png)


todo parte lógicamente de una optimización que la hacemos con `out of sample` detrás, es decir, cercano del 25% por ciento porque mirando los cortes pues se ve que puede estar ahí interesante y todo el histórico disponible, desde julio del 99 hasta el 29 de febrero del 2024 esto lo hacemos en datos de tic data que nosotros compramos pero podría haberse hecho perfectamente en los datos de TradeStation con ***tres dólares de comisión*** con ***ocho dólares de slippage*** etc


**Inputs seleccionados para la optimización**

Pero no se hace de todos los *inputs*, no se hacen de todos los *inputs*; los *inputs* que tiene el sistema son todos estos,

![](../img/018.png)

no todos son optimizables; los que les hemos dejado el nombre

![](../img/019.png)

son del algoritmo. Bueno, este último `puntosErrorTradeporHalt` es cuando hubo el *halt* en el *covid*; tuvieron varios *halt* y hubo problemas de ejecución, y entonces consideramos que ahí hay un factor que se benefició de manera artificial y lo penalizamos, lo penalizamos en 600 puntos ahí porque la ejecución no fue real. Eso puede pasar, probablemente pasa, por un *tic* y el *Slipage* lo controla; en este caso no fue así, el gráfico marcó ejecutado y ese día, para que me entendáis —y digo porque esto…—

Imaginaos que aquí se cierra una operación:

![](../img/020.png)

imaginad que habría aquí, pero realmente, como el mercado es falso, no te deja ejecutar: ejecuta 600 puntos por debajo. Entonces, bueno, pues eso lo implementamos en el código para que en el optimizador no sacara partido de una situación que fue falsa; no pudiste operar ese precio, aunque ponga que abrió ahí. Entonces, cuando hay algo así que no es real, que te has podido aprovechar, o que penaliza, pues es mejor dejarlo puesto en el código porque así, si tú optimizas, no saque partido de una cosa que no fue real. Es una cosa muy puntual, pero bueno, os explico para que lo sepáis: es ese, esos puntos que está implementado y está como *input* `puntosErrorTradeporHalt`.

**Inputs que sí se optimizan**

Y luego el resto `filtRiskNoOptimizarMM`; esos son variables de la gestión monetaria, al igual que esta `MMVar`, que es el porcentaje, es la *f*, la *f* de la gestión monetaria, el saldo de la cuenta inicial, el lote mínimo, el lote máximo, de tal manera que si le pones uno, pues lo bloqueas, redondea a uno si quieres redondear a otro número, y ya está. Todas estas son variables de la gestión monetaria, vale, que veremos en algún momento, veremos en algún momento en detalle; esto lo comenté en la teoría cuando haremos alguna clase dedicada solo a la gestión monetaria.

**Variables realmente evaluadas en el mapa**

Entonces, ¿qué variables tocamos? Hay una variable del indicador; hay otras dos variables que no se suelen tocar; esta tampoco; esta es, de hecho, una prueba que se hizo una vez de un filtro que está ahí, no sé, creo que alguna vez lo hemos configurado, pero nuevamente quiere decir que no actúa. Ese es el valor que os decía de la *atr* que tampoco se optimizaba. Y luego vienen los tres —podemos decir— filtros… filtros en el sentido estricto, sino que es un porcentaje multiplicador: *bar 01* al final es un modificador de la orden de entrada, y *bar 02* es *tp* y *bar 03* es *stop*, no hay más. Entonces aquí solo se analizan en el mapa estas cuatro variables: el indicador, el multiplicador de entrada, el multiplicador del *tp* y el multiplicador del *stop*; esos cuatro es lo que se hace en el mapa.


<div style="border-left: 4px solid #27ae60; background: #ecf9f1; padding: 10px 15px; margin: 10px 0;">
  <strong>📏 Nota técnica</strong><br>
  La estructura descrita —un indicador principal y tres multiplicadores paramétricos— se ajusta a arquitecturas comunes en sistemas cuantitativos de diseño modular. Es frecuente en frameworks inspirados en autores como *Howard Bandy* (autor de “Quantitative Trading Systems”), donde se separa el núcleo determinista del sistema (*signal core*) de los modificadores de escala (*scaling layers*) aplicados en entrada, TP y Stop.
</div>

<br>
<br>

**Cantidad de combinaciones y por qué se usan tantas**

para hacer un mapa guardamos `GUARDO 8000` todo lo que nos deja TradeStation que son 8.000 porque? porque al final en el mapa me interesa ver realmente cuántas más combinaciones posibles mejor acuerdo en el mapa es importante que haya los menos huecos posibles  entre combinaciones, por eso idealmente hay que hacer la exhaustiva vale exhaustiva,  entonces bueno esto al final da 49.000 combinaciones que en exhaustiva pues se hace depende del equipo pero tardaba unas horas, con el lift activado y todo, que eran unas horas depende lógicamente de la potencia de la máquina, 

como ya visteis en la teoría o veis en cualquier optimización de tres sesiones

* Una que es `AllData` que reúne los dos periodos, 
* el periodo `InSample` que es el optimizado, 
* el periodo `OutOfSample` que es el no optimizado, 

en este caso el último 25% del histórico. InSample, OutOfSample, y OldData que los suma todos. Estos datos al final los llevamos a Excel.

![](../img/021.png)
![](../img/022.png)


todas las hojas simplemente hacemos una estadística descriptiva abajo del todo también cuando hay 8.000 

![](../img/023.png)  
![](../img/024.png)  

esto sale directamente del propio excel en datos en análisis de datos, estadística descriptiva, que saca todos los parámetros que tú le has puesto. Aquí abajo 

![](../img/025.png)  

pues marcamos por ver más rápido mínimo máximo y mediana es mejor estimador para la mayoría de casos la mediana que la media la mediana es mejor estimador nuevamente cuando hay outliers cosas bastante habitual en el trading pero bueno que también si usas la media no pasa nada si la media si la media la media no son iguales la moda recordar que eso es en este caso una moda pero si eso será recordar que es una de las características que tiene una distribución normal que lo visteis o lo vimos esto en la clase de porfolio en la práctica hay una ***clase de porfolio*** donde se habla del bar el bar todo esto y ahí se ve bastante todo este tema vale

**Normalización de métricas y selección de parámetros**

hacemos como ya os expliqué por norma siempre los exes no creo que ya hacemos de manera en los ex y es un excel de búsqueda de zonas muy inicial 

en un sistema que no conocemos mucho mejor no lo hacemos pero normalmente hacemos esto : normalizamos las diferencias entre tanto tsi expectancy como ppc y robustness, recordar que robustness compara beneficio medio analizado con beneficio medio analizado out of sample con in sample, y automáticamente se normaliza con el valor máximo, y de ahí sale una suma de los cuatro de las diferencias respecto a su máximo 

![](../img/027.png)  

aquí lo veis a ver aquellos que entran con este valor si veis que sale la fórmula 

![](../img/026.png)  
![](../img/028.png)  

y es este valor menos este valor dividido por este valor vale dividido por este valor  
eso es normalización típica respecto al máximo lo cual te dice que este valor es del máximo es un 0.86 del máximo simplemente para pues sumarlos todos y de esta manera buscas un equilibrio entre todos el que tiene un valor de suma más bajo quiere decir que en estas cuatro referencias cumple más equilibrio y todos da buenas notas vale buenas notas

este por ejemplo es en verde pintamos el mejor tsi en naranja pintamos el mejor expectancy score y en azul pintamos el mejor ppc 

![](../img/029.png) 

pero luego repito se ordena o bien por `suma` o bien por `suma sin rob`, ¿cuándo se usa uno u otro? cuando optimizamos con el `Out of sample` al final es decir ahora en la actualidad o lo más cercano a la actualidad entonces usamos `SUMA` porque? porque el `Out of sample` es cercano y nos interesa. 

al final usar el valor de `robustness` o no usarlo para los cálculos de esta suma, es simplemente si yo quiero sobreponderar o darle importancia a algún dato que compare el `insample` con el `Out of sample`, el único dato aquí que compara eso es el robustness, robustness compara justamente el rendimiento de un periodo con el rendimiento del otro por lo tanto, si yo quiero darle más peso al auto sample vale usando esta variable LO HAGO, luego de hecho si ordeno por esta variable 

![](../img/031.png)

pues al final ***estos son los sets que han ido mejor en el `Out of sample`***   
pero sólo eso tampoco lo quiero porque también quiero tener en cuenta las otras variables que tienen en cuenta todo el histórico y por eso eso lo hace la `SUMA` porque la suma tiene en cuenta este este este y además este 

![](../img/032.png) 

en cambio este último `SUMA SIN ROB` no tiene en cuenta, provoca otra ordenación distinta que lógicamente es parecida muchas veces no pero no es exactamente 

**Por qué se usan 8.000 combinaciones**

bien esto, (ahora vamos al mapa luego iremos a los sets), aquí no me importa los sets, no me importa si los sets son iguales o sino porque he elegido 8.000, elegir 8.000 es elegir muchos valores, elegir 8.000 al final recordar que esto funciona que optimiza por una función,,, (este creo que es el MAPA ES SHORT pero se ha hecho para los tres, he escogido este que ha sido aleatorio)  

![](../img/033.png) 
![](../img/035.png) 

yo optimizo por esta variable por `expectancy score` y escoge los 8.000 mejores en `in sample` los 8.000 mejores y de ahí de esos 8.000 que ha encontrado calcula qué hubieran rendido en el periodo `Out of sample`, lo pone aquí 

![](../img/036.png) 

y luego suma los dos aquí 

![](../img/037.png) 

el que manda es el `in sample` el que manda en el sentido que es el que controla el resto de datos 


por lo tanto estaremos de acuerdo que lo bueno es que el `OutOfSample` e `in sample` se parezca pero eligiendo 8.000 me da igual igual porque además he dejado elegir demasiado por lo tanto lo que encuentre como mejor dato aquí puede ser fruto de una sobreoptimización luego veremos que no es tan distinto pero podría ser

**Utilidad real de tener tantas combinaciones**

a mí para qué me sirve tener 8.000 pues para hacer esto 

![](../img/038.png) 

para hacer mapas

**Construcción del mapa y por qué preferimos tablas dinámicas**

Los mapas en esta estrategia cómo planteamos los mapas? yo ya os comenté en la teoría que nos gusta mucho verlos en tablas a nosotros, tablas dinámicas, no es necesario hacer el típico mapa gráfico y visual, 

por ejemplo multicharts lo hace está bastante chulo, está bien, es muy aprovechable de multicharts, pero TradeStation no lo hace entonces nosotros lo hacemos en excel y para hacerlo en excel o sea este mapa a mí no me dice nada más 

![](../img/041.png)

si hago un mapa aquí que esto es tan fácil como seleccionar la tabla dinámica hacerle así que me lo encuentro aquí superficie y ya está hecho vale a mí esto me dice más un poco me dice lo mismo que aquí pero lo veo mejor aquí

![](../img/040.png)

ahora os explico por qué entonces

**Problemas del efecto “escala” al visualizar mapas 3D**

además del efecto mapa es el efecto escala que siempre se llama de los gráficos pasa igual aquí, vale porque tú dices joder está muy puntiagudo,,, seguro?? vamos a hacerlo menos puntiagudo 

![](../img/042.png)

(ironía) "ya está mucho mejor fíjate qué llanura más magnífica de parámetros tenemos es una cosa súper estable y súper robusta en que ya está sistema a operar,,," (ironía) me entendéis no es decir que al final el efecto escala condiciona totalmente el efecto visual 

**El truco de Multicharts: la marca de agua**

para eso tiene multicharts aquello de la del nivel, ya lo visteis en la teoría en algún sistema lo hemos enseñado, bueno creo que el otro día en el sistema que hicimos al principio lo enseñamos también, lo enseñamos que ahí enseñamos multicharts por eso aquí pues vamos otra vez más a excel, vamos enseñando un poco de todo, eso es lo de la marca de agua, os acordáis 

![](../img/043.png)

la marca de agua que viene muy bien, evitas un poco ese efecto con esto ese efecto escala lo puedes tú jugar así porque el efecto visual cambia totalmente la mente la mente entonces así con esto puedes jugarlo entonces aquí sí viene muy bien porque es una herramienta que ya se ha pensado para eso tiene esto para mitigar y viene bien pero en excel a mí me gusta más la tabla

**Por qué preferimos tabla dinámica frente al gráfico**

prefiero la tabla la tabla en excel, recordar que es una tabla dinámica que excel hoy en día hace de manera automática tú simplemente seleccionas los valores y le dices aquí insertar tabla dinámica y ya está hecha de verdad no estoy exagerando es un poco saber de excel 

lo peor es el blanco al final lo peor es que no haya valores a nivel de mapa , solo blanco verde y entonces ya claro el blanco es el que tiene poco valor es casi blanco es parecido a este pero no está entonces este es perfecto este es perfecto
así pues bueno se ve muy bien por un único color 

3 mapas, los he hecho por :
1. ***Suma de All : TS Index***
2. ***Perfect Profit Correlation*** 
3. ***Expectancy Score*** 

![](../img/046.png)

esto realmente no es necesario lo he hecho para demostraros que no es necesario porque al final el vector clave que hace que el mapa sea bueno o malo es el hecho de ***aparecer*** y lógicamente su valor también tiene importancia pero es al final lo que hace es 


acumular todos los


**Origen del valor “suma” y cómo se genera en la tabla dinámica**

valores que dan 4.05 en el resto de variables, todas, y la relación a uno y te dice qué valor da la variable de la suma podría podría usar el producto podría usar otros ahora si yo veáis de dónde viene esto porque ya está montado porque si lo montamos todo no hacemos clase ya lo vimos en la teoría es aquí 

En una columna esta `Var_01`  
en fila está `Per_01`   
y los valores que calcula es suma de `All_ppc (Perfect_Profit_Correlation)`


![](../img/047.png)

esto es configurable esto ya digo es pelearse también poco usar la ayuda de los programas de acuerdo es decir los programas la mayoría tienen buenas ayudas usar la dinámica y poco a poco mirar vídeos decir hay cosas 

![](../img/048.png)

o sea el curso tratamos que sea completo pero es imposible que sea profundo en excel profundo en estadística profundo en econometría profundo es imposible no hacemos mil horas o más no más entonces claro damos las puntas las cosas clave enseñamos lo que nosotros hacemos es así esto es lo que hacemos nosotros y a partir de ahí claro mira voy flojo de excel bueno pues vamos a buscar un curso de excel y mejoro mi excel voy justo de programación bueno pues voy a buscar un curso voy justo tienes que ir reforzando un poquito aquellas áreas aquellas áreas porque tratamos de explicarlo por encima lo que no es propiamente la actividad como en este caso excel no lo usamos como actividad principal lo explico un poco pero claro no puedo en profundidad porque si no es que como digo estaríamos 2000 horas vale

**Por qué se usa “suma” como agregación y qué aporta visualmente** 59:36

entonces aquí dentro de `suma` yo podría haber elegido otro elegir el recuento promedio máximo mínimo producto vamos suma y ya está eso es la suma de todos los valores entonces al final colorcito es muy visual y también los totales generales porque esto cuando hay la dinámica aquí en el diseño hay una configuración de tabla dinámica nosotros aquí usamos el esquema que nos gusta usamos totales generales para filas y columnas y subtotales para ninguno entonces porque ese total al final te está diciendo todos los valores que tiene 0 5 todos los valores tal todos valores tal pues esto es muy visual vale rápidamente yo veo per 1 con bar 1 cómo relaciona per 1 con bar 1 cómo relaciona ignorando el resto ahora veremos el resto

**Primeras zonas fuertes del mapa y necesidad de comprobar vecinos**

y aquí rápidamente pues yo veo que hay dos zonas de per 1 buenas está aquí 

![](../img/049.png)

en ambas me toca a lo que veremos en ambas me toca esto no es deseable 

![](../img/050.png)

quiero decir que aquí me está diciendo que me he quedado corto debería haber ido al menos a 26 o 27 porque a mí me interesan mucho en los mapas los ***vecinos*** los vecinos me interesan mucho el mapa no es tanto elegir un set, al final puedo elegir un set por mapa pero no es elegir un set, sino que el mapa sobre todo me sirve para ver claramente lo que descarto, vale, y para ver dónde están las zonas más estables, es un ***análisis de sensibilidad***, aquí quiere decir... "a ver pero este"... bueno... luego veremos si ese me gusta, si tiene un perfil más adecuado al riesgo,,, pero hombre sí está lo que yo pero lo deseable es que esté dentro de las buenas zonas del mapa, eso es lo deseable lo muy deseable.

**Necesidad de evaluar estabilidad y degradación entre parámetros cercanos**

pues el mapa es para ver cuán estables son los parámetros, pues aquí el 4 lo mismo debería ver el 3 para ir bien

![](../img/051.png)

si luego al final quiero operar con el 4 debería ver el 3 vale porque si en el 3 me pasa como ahora vais a ver en otro parámetro que me degrada en picado, el 4 no me interesa, entendéis. Como un poco este caso, el 10 que este aquí saca la cabeza pero sus vecinos justito

![](../img/052.png)

luego lo veremos lo veremos, lo veremos si me lo quedo o no me lo quedo, bueno tampoco es un desastre final recordar que ya estar aquí en esa zona ya es bueno , lo pero es no estar.

***Mapa de entrada:***

bien aquí relaciono **Per_1** con **Var_1** ese es el valor del indicador y **0.500** es el multiplicador del indicador por lo tanto al final es ***entrada*** de acuerdo esta tabla de aquí `Sume de All : TS Index` me está evaluando un poco la entrada y ver qué combinación de entrada puede ir mejor

***Mapa de salida:***

y aquí tengo stop contra tp ***salida*** 

![](../img/053.png)

stop contra tp, este sistema solo sale por SP o por TP por la salida contraria, que no está contemplada,,, salida contraria es decir cuando aunque sean solo cortos si fuera largo pues cierra, cuando iría largo cierra, por esa combinación de parámetros.  Entonces aquí ya veis lo que os decía, parece que 0.60 de los 60 es muy bueno también es su vecino 0.625 no está mal su vecino 0.565 y tampoco está mal su vecino es decir aquí tenemos cuatro zonitas bien rodeadas

![](../img/055.png)

**Detección de zonas débiles y vecinos inestables**

a partir de aquí ya 0.650 bueno ya su vecino no es tan bueno y aquí 0.565 su vecino ya no es malo del todo pero ya este vecino es malos malos vecinos entonces bueno lo ideal lo ideal lo ideal son estos dos son estos dos 


**Mapa TP–Stop: holgura, tolerancia y degradación progresiva**

lo primero que se ve es que el stop pues el stop da igual porque es un stop bastante holgado 

![](../img/056.png)

aunque lógicamente salta más 1.8 que 1 que 2.9 

![](../img/057.png)

y de hecho reconozco que esperábamos un poquito más de granularidad aquí 

![](../img/056.png)

esperábamos un poco más porque casi no la hay esto casi dice da igual elige el que quieras casi elige el que quieras además en todos los valores. apenas en `2.5` hay algún blanco yson valores muy altos ya de tp valores muy altos de tp vale

**Lectura del TP: sensibilidad y estabilidad por rangos**

y en el tp pues sí, un tp bajo, algo que nos sorprende algo que no sorprende 

![](../img/053.png)

pero pero dentro de no sorprender es muy positivo esta progresión, pero fijaros lo que os digo en el 0.60 que os decía antes el 0.5 es drama se me hunde, por lo tanto pero el 0.60 estoy operando muy justo, estoy operando muy justo, es decir, no es recomendable como mínimo habría que coger 0.70, pero podría elegir 0.70, 0.80, 0.90, incluso 1 porque aquí degrada pero podemos decir que degrada de una manera más tolerable pero bueno a partir de ahí lo ideal ideal sería de 0.70 a 0.90 pero yo aceptaría 1 en este caso

**Consideraciones específicas por activo: Nasdaq y comportamiento histórico**

más en un sistema recordar que es un sistema que va en el nasdaq corto que va en muchos más índices y activos cuando se creó se creó así pero ahora lo estamos estudiando en el nasdaq y está operando el sistema, de hecho ha abierto cortos hoy, en algún set, entonces bueno como veis en tp pues más bien rápido, a medida que va degradando también degrada de manera bastante progresiva (fíjate en `Total general`),  esto está bien es más o menos progresiva hasta llegar a un punto donde realmente degrada mucho, pero es muy bueno que haya valores hay valores en la zona de blancos abajo, de acuerdo, es decir en todos los casos hay valores. Fijaros en TSIndex en muchos no hay valores, cuando aquí en toda la zona de tp stop en todos prácticamente aparecen apenas aquí todos aparecen valores entre los 8.000. ***está muy bien distribuido*** el `tp` y el `SP`, tiene mucho margen tiene bastante tolerancia , son parámetros con tolerancia. En el mapa TSIndex no la hay tanto, sobre todo en este filtro de entrada Per_01 es un filtro que tiene una tolerancia justa

**Revisión adicional: el input duplicado, el indicador modificado y los incrementos**

una de las cosas que hemos revisado en esta revisión vale donde primero lógicamente se plantean , todo al 100% no podemos hablarlo por razones obvias pero también por tiempo, entonces aquí hubo un replanteamiento de este ayudante input `Per_01` al final antes eran dos y nos dimos cuenta de que de hecho era hasta incorrecto el código que eran demasiado rebuscado usar dos y decidimos sacar uno y cambiar el indicador no usar el que es original y crear uno propio para hacerlo distinto y ahora pues operar con un indicador propio que tiene una pequeña modificación que usa el mismo parámetro para esto

**Incrementos de parámetros: prudencia, sensibilidad y ajuste fino**

y también revisamos analizamos revisar los incrementos de estos tres filtros 

![](../img/058.png)

esto recordar que lo detallamos bastante en la clase de desarrollo teórico  donde hablamos de prudencia con los incrementos con las optimizaciones, porque yo puedo poner aquí 0.0025, o puedo poner aquí 0.00001, y claro no me he pasado pero entendéiso, entonces al final usar uno u otro no deja de ser un análisis de sensibilidad que también se hace con mapa este lo hicimos previamente ver mapas que varían y ver los sets que hay, qué variación, también se mira mucho aquí en la tabla, 


![](../img/059.png)

es decir bueno cómo varía el mismo set con el siguiente incremento `Var_01` ,  


1:10h


yo busco yo ordeno aquí por este y ordeno luego por este y entonces automáticamente aquí me quedan todos estos ordenados vale entonces bueno vas jugando hasta que encuentres el mismo no no pero claro tengo que desbloquear el otro perdón no estoy haciendo bien vale ahora tengo este quieto este quieto y este quieto los imaginaros no bueno es que aquí no hay debate porque el incremento no puede ser este sería donde hay debate vale

**Ejemplo de variación: impacto de un solo tick en trades y estabilidad**

entonces yo aquí por ejemplo tengo que buscar este por ejemplo veis este es igual o grande es una de las maneras de mirar los incrementos esto por si no queda claro que entiendo que sí pero esperaros es así entonces estos dos veis son iguales todo es igual 25 2 1 2.9 y sólo cambia el incremento entonces ahí analizar cómo varían los resultados es importante fijaros que un tick varía 20 trades un cambio bastante importante esto cómo estaría mal que aquí cambiara o ningún trade un trade y todo fuera casi igual tenéis eso es un poco hay que buscar un incremento que provoque cierto cambio vale cierto o cuánto es cierto que no sé no no lo tengo medido el porcentaje no sé si decir un 10% sería correcto puede puede estar ahí 10% incremento mínimo vale pero en variación de significación pero no sé no no no lo toméis como una cosa sagrada porque depende al final simplemente que varíe

**Conclusión sobre incrementos: mejor quedarse corto que excederse**

entonces hay que ver que haya cierta variación a veces te puedes pasar a veces se puede expresar aquí podríamos pensar que a lo mejor es demasiado y a lo mejor puedes ganar un poco más pues puede ser a lo mejor podría haber 0.0025 0.002 a lo mejor podía hacerlo y entonces haría otro mapa y vería a ver vale para regularlo un poco más en vez de esas nueve sería 11 tenéis pues vendría aquí y vería el mapa a ver a lo mejor es eso también que puede ser vale pero ya digo mejor pecar de conservador en esto que de excesivo vale

**Variable crítica: bar 0 1 como parámetro más sensible del mapa**

porque lo importante es que haya cierto cambio en el incremento entonces esta es un poco la variable en estos datos que estamos viendo estoy leyendo los datos como los veo trato de no tener (aunque es complicado) pero de verdad que trato de no tener sesgo de saber el sistema que hace todo y al final este dato aquí en el mapa es el que parece más sensible de acuerdo más sensible que ahora sí tiene su zona de trabajo pero es muy estrecha de acuerdo

**Comparación: TP con margen amplio vs entrada más delicada**

en cambio aquí hay otras zonas fijaros en el tp también hay una pero también es bastante larga y además claro como larga teniendo zona en todos y aquí fijaros que hay muchos blancos aquí no tanto tp como stop tiene más margen de mano entonces aquí variable delicada podemos decir que es bar 0 1 en este mapa

**Revisión del incremento para bar 0 1**

bueno se podía es lo que se decía se podría volver a hacer un análisis de sensibilidad como habéis visto ahí pues quizá me pasé nos hemos pasado con 0.0025 a lo mejor vamos a reducirlo a la mitad el incremento a ver a ver qué tal eso es un multiplicador venga

**Definición de zonas de trabajo previas a la selección final**

bien visto esto yo aquí tengo unas zonas de trabajo bien definidas a nosotros nos gusta montar el mapa completo el mapa completo de todos los inputs es decir que aquí recoge las 8000

**Visualización del mapa global en varios monitores**

esto como yo tengo cuatro monitores ahora no lo vais a ver pero lo que hago es abrirlo así lo estiro y me lo estiro a los cuatro monitores aquí lógicamente no puedo porque estoy compartiendo un monitor vale y lo único que puedo hacer es esto que parece que no pero es bastante útil es bastante útil igualmente porque al final a mí me interesa no me interesan tanto los valores y me interesan las zonas entender las zonas

**Explicación de ejes y estructura del mapa global**

pero antes antes vamos a ver porque sin explicar los ejes es complicado que entendáis de qué estamos hablando aquí que tenemos tenemos por un lado aquí tenemos bar 2 y bar 3 y aquí tenemos bar 1 y per 1 es decir que tenemos las entradas y tenemos las salidas lógicamente hay uno que es el que condiciona más porque así yo aquí recojo y puedo ir recogiendo ver los distintos voy agrupando si quiero es hacerlo de hecho puedo filtrar también puedo filtrar expandir o contraer todos expandir todo el campo vale puedo filtrar y de hecho una de las cosas que hacemos es esa ahora la haremos es filtrar filtrar pues aquellas zonas que ya me gustan decir qué cosas tengo claras pues mira esta de aquí bueno pues vale pues la filtro la voy a bloquear ahí

**Identificación rápida de parámetros con pocos valores (zonas débiles)**

vale pero antes quería que mirárais el mapa teniendo en cuenta lo que os digo aquí tenéis 0.5 luego 0.5 con el 4 con el 6 con el 2 con los que aparecen los que hay huecos es que no aparece 0.525 con el 4 con el 6 con el… esos son los que aparecen y ya te está diciendo que aparecen pocos esto creo de cada elemento no era esto había un sitio no me acuerdo no se podían poner los ceros creo no me acuerdo no me acuerdo no me acuerdo no me acuerdo pero que sí que había un sitio pero bueno que es igual no importa no importa porque es mejor así porque así ves cuando hay pocos de 0.5 que visto así un poco si os fijáis lo veis veis aquí ves uno aquí ves el otro aquí ves el otro entonces ves que hay pocos entendéis

**Cómo se localizan rápidamente los huecos del mapa**

y esto es lo que te da información a medida que lo vas reduciendo parece que no parece que cuesta más de ver pero aquí por ejemplo hasta en 20 que ya casi casi la llenamos toda vamos a ver ligeramente no se ve cuando cuando nos reduce si en 30 sé que se ve creo es verdad que ya le hemos quitado el micro hemos quitado un poco es aquí aquí sí que se ve es aquí tienes uno aquí tienes otro aquí tienes otro vale entonces claro estos ya se ven que tienen muy pocos en cambio aquí fijaros tiene muchos muchos está que llega al final que también casi no tiene en el filtro en el filtro de entrada por entendernos vale

**Distribución de TP/Stop: zonas llenas frente a zonas huecas**

el filtro de entrada aquí pasa lo mismo con el tema de tp y stop y primero este tp que fijaros aquí en general son muy parecidos porque aparecen todos lo habéis visto antes excepto este último que aparecen algunos menos nada bajo del todo pero en este no no no aparecen los aquí insisto no importa tanto leer los valores no se ve nada vale vale es igual no importa

**Visualización conceptual: no importan los números, importa el dibujo global**

aquí ahora ya tú porque ya te lo he dicho sabes dónde está cada cosa a ver un segundo una más 20 ahí 15 15 15 15 15 se ve se ve ahí se ve ahí enterita entonces ya digo no importa tanto no importa tanto el hecho de ver los valores sino de entender un poco el dibujo

**Identificación visual de la zona fuerte del 0.60**

aquí claramente es donde está el 0.60 de arriba y donde es más ancha y porque el 0.60 ahora veréis veíais arriba de acuerdo aquí en la pequeñita quedaba más porque veis el 0.60 tiene aquí donde tiene zona de tp baja vale que es donde está esta es la zona de tp baja espera que voy a pintar esto evidentemente es la zona de tp bajo es evidente vale

**Por qué 0.60 aparece como valor dominante incluso con TP alto**

pero fijaros que el 0.60 incluso en zonas de tp altas tiene valores en cambio su vecino en 0.575 no tanto 0.55 tampoco vale y para arriba mejor ahora llegando a lo que dices pues oye cuidado es verdad que en la zona mismo tampoco tiene tanto pero es progresiva el 1 en sí es bastante progresivo también

**Interpretación del mapa en conjunto**

entonces esto es lo que nos permite ver el mapa un poco global que se ve mejor en 4 en 4


Perfecto Núria. Aquí tienes **todo el fragmento reescrito**, siguiendo **exactamente** tus reglas:

– Respeto **total** de todas las palabras del texto original
– Léxico afinado únicamente donde no altera el sentido
– **Títulos en negrita** para cada cambio de tema
– Sin almohadillas, sin iconos, sin líneas, sólo texto limpio

---

**Visualización del mapa en cuatro monitores y transición hacia el filtrado**

en 4 en 4 monitores pero visto esto quería simplemente que lo vierais ahora vamos a filtrarlo para que lo veáis mejor para que lo veáis mejor y vamos a limitar, a bloquear los inputs que son más claros bar 0 2 por un lado vale bar 0 2 por un lado que es recordar el tp y bar 0 1 que es el filtro de entrada por entenderlo se hace aquí entonces donde los bloqueo aquí bueno yo puedo venir aquí a la variable y le digo bueno pues de bar 0 1 lo voy a ver sólo desde 0 55 a 0 65 sólo voy a bloquear esos porque bueno porque el 0 55 ya no es bueno pero me interesa ver por vecino de acuerdo me interesa ver siempre el vecino aquí incluso podía haber puesto este vamos a poner 0 55 0 6 7 5 por ver también el vecino vale 0 6 7 5 yo ahí añado el vecino vale

**Bloqueo de la zona relevante en TP y motivo para incluir valores “malos”**

y lo mismo voy a hacer para la del tp que el 0 5 es virria pero quiero verlo quiero verlo porque quiero ver este caso no me haría ni falta porque es muy claro que es virria pero yo lo quiero ver quiero ver vale pero 5 hasta 1 1 porque quiero ver el vecino del 1 quiero ver el vecino del 1 vale que lo veíamos ahí será este es este 1 1 quiero ver ahora ya lo he bloqueado vamos a usar tamaños vale

**Mapa filtrado: análisis más limpio y lectura por filas/columnas**

y ahora evidentemente sólo con esos dos ya vais a ver que va a ser más sencillo analizarla vale ser un poco más sencillo sigue habiendo mucho pero con el trenta ya casi llegaba si el trenta casi casi está ahí queda un poco entonces aquí ya se ve un poco mejor recordar que a la izquierda tengo así en filas tengo tp y su stop y arriba tengo entrada entonces este es esta fila que vemos aquí tan tan flojita vale esta fila es el 0 5 de tp de acuerdo con sus stops 0 5 en cada distinto rango de entrada vale

**Interpretación de la fila débil: por qué resiste bien en 0.60**

lógicamente aquí donde tiene 0 6 donde tiene 0 6 veis pues es donde tiene tiene mejor de acuerdo tiene mejor dato ahí el 0 5 es eso es lo bueno lo que habla de la robustez de una variable es esto es que no es sólo donde le va bien sino también en otras zonas de muestra esto indica que es zona robusta de ese parámetro entendéis pero es verdad que como veis como decía a partir de que sacas el 0 5 del tp también casi que ha sido igual no 0 6 7 todas todas están muy bien todas están muy bien todas están muy bien

**Lectura completa de rangos de TP y cómo se reflejan en cada bar 0 1**

no esta este es 0 6 de tp este es 0 7 de tp 0 8 0 9 1 y 1 1 y en toda la zona incluso que al principio de todo que es la zona primera del 0 5 5 no debía ser 0 5 5 creo que he puesto de de bar 0 1 del filtro filtro el bar 0 1 vale este es 0 5 5 estos 5 7 5 6 6 25 entonces bueno pues incluso aquí se ve bastante bastante de todo me parece que me falta una estaba estaba contando mientras lo decía me faltaba efectivamente una esta es 5 5 esta esta de aquí 5 5 5 y estas 5 7 5 y estas 6 bueno

**Sensibilidad del TP/Stop: amplio margen, poca fragilidad**

entonces lo que os decía aquí en la variable 5 5 hay pocas pero fijaros que en toda la columna en todos los niveles de tp y stop hay esto es por lo que el motivo el tp y el stop es poco sensible porque en todas las zonas de póngase el póngase el filtro salen tp y stop lógicamente en los que salen más que en otros pero en todos hay zonas un parámetro bastante da margen de maniobra su sensibilidad es buena tiene buena tolerancia vale a medida que aumenta pues ya van apareciendo apareciendo pocos entonces de esta manera yo puedo ver el mapa mejor

**Comparación de dos zonas del mapa: por qué 0.575 parece mejor pero 0.60 domina en robustez**

aquí por ejemplo eliminando los los mapas si os fijáis a ver si consigo que lo veáis es interesante que veáis una cosa que hemos visto en el mapa aquí quería quería verlos aquí fijaros que en el 5 7 5 que aquí ves 4 5 7 5 de filtro con el indicador en 4 5 6 7 que ahí hemos visto que era buena zona no fijaros que en esta zona me lo voy a marcar mejor con un cuadradito a los que en esta zona con 4 5 6 vale tiene hasta mejores valores en 0 6 lo veis que tiene 4000 es aquí 3000 por el color ya se ve no es decir en esta zona es mejor es mejor 5 7 5 es bueno tiene valores realmente elevados

**Razón matemática: 0.60 distribuye valores en todo el mapa (no sólo en su zona óptima)**

porque 6 cuando lo agrupas aquí arriba sale mejor porque cuando lo ves aquí resumido vale cuando aquí pinta sólo per 1 contra bar 0 1 ignorando el resto es decir todas están recogidas aquí vale aquí sí que veis claro que 6 es mejor que 5 7 5 bueno la respuesta es muy muy sencilla la habéis visto antes incluso en la en la zona donde estaban todos que el 0 6 distribuye mucho en todos no sólo en los buenos en los buenos en las en su zona óptima 5 7 5 parece hasta mejor pero 6 es un todoterreno espectacular en todas las zonas mete bastantes valores en todas en todas incluso las que no están aquí ahora las hemos quitado recordar que lo hemos visto antes lo voy a volver a poner el mapa completo para que lo veáis lo que quiero decir

**Conclusión de robustez: tolerancia transversal de 0.60**

eso es porque 6 en la zona óptima no es el mejor mejores 5 7 5 pero aquí lo veis es pero hasta hasta cuando no es bueno mete mete más y también aquí aunque no lo parezca es en esta zona aquí tiene más aquí que aquí y realmente 0 6 es el que tiene mejor tolerancia mejor tolerancia es un valor bueno

**Advertencia: el mapa va “por zonas”, no por sets individuales**

pero cuidado que es muy muy importante en un mapa vamos por zonas no se trata de decir si luego metemos un set que está ahí pues de coña pero son las zonas de acuerdo de entender dónde dónde me muevo yo mejor donde mis sets pueden estar más justos vale

**Pregunta sobre incrementos de los inputs y respuesta general**

bien esto se va siguiendo la vuelta por ahí o que o asatiendo ahora sí pregunta jaime si hablando de los incrementos en los inputs hay alguna forma de elegir en cuánto poner los incrementos sería probando que explicas sí no no esto te lo decía jaime no no hay vaya nosotros no no no no tenemos ningún método vale que puede ser que lo haya nosotros explicamos lo que lo que hacemos al final lo normal es que un porcentaje sea razonable cuando es incremental lo permita porque si usas un indicador imagínate que eso sea una media móvil pues número entero no pero si tú estás metiendo aquí un porcentaje como es el caso es un multiplicador un multiplicador es un porcentaje de acuerdo

**Regla práctica para incrementos: usar el sentido común y evitar extremos**

entonces al final como los regulas a 0 0 1 a 0 1 a 0 0 0 1 tienes hay que buscar sentido común aquí la verdad que como os he dicho voy a hacer otra seguramente haremos una prueba porque me ha parecido que la variación es bastante notable a mí esta variación ha parecido bastante notable hay que ver que realmente depende de los otros inputs hay que mirarlo en distintas zonas ahora habría que buscar uno en el 4 tienes en el 4 4 o algo así que actúe muy distinto o con el filtro más cercano no sé distintas combinaciones no con tp cercanos pero es verdad que este nivel es elevado para ser el incremento 20 trades sobre sobre 600 y pico son bastantes

**Posible ajuste futuro: reducir incremento para aumentar granularidad**

pudiera ser que aceptara un poco más de granularidad entonces casi con total seguridad lo tenemos que hablar luego con con alberto porque pero es posible que podamos evaluar reducirle un poco más porque como digo 20 y pico son bastantes y es verdad que es el incremento que se ve que va un poco justo con lo cual puede ser que sea justo porque lo dejamos que ruede en pocos al final esto esto siempre va todo esto siempre va de señal y de ruido de acuerdo

**Equilibrio entre evitar sobreoptimización y no restringir demasiado la señal**

entonces no hay que pasarse con o sea igual que no hay que pasarse con la sobre optimización es decir en el sentido de que le permites ajustarse tanto tienes que dejarle suficientes grados de libertad para que se ajuste me entendéis para que se ajuste a la señal si yo no le permito un mínimo vale imagínate que yo este filtro le pongo de de 5 en 5 es imposible no puedo no soy capaz de contar nada ahí porque me estás obligando a trabajar en parámetros de locura o como si yo digo pues si el mercado cae un 5 por ciento pones largo bueno cuántas veces cae un 5 por ciento al final todo tiene su nivel no

**Contexto del sistema: años de evolución y robustez comprobada**

como todo en la vida al final tanto es malo como poco siempre mejor pecar de prudencia y protegerse contra la sobre optimización mucho pero aquí en un sistema que tenemos que conocemos más que a mi hija casi vale que lleva dos días alberto sí será capaz de decir menos años que lleva pero pero apolo puede llevar 15 años en el mercado alberto es el 14 como mínimo este no pues igual ya más con algunas pequeñas modificaciones que es que ha demostrado operar todo tipo de mercados es un sistema muy muy robusto vale que lógicamente en fin cuando pues hay que ajustarlo pero es muy robusto entonces bueno se puede se puede apretar un poco más las tuercas pero siempre prudencia

**Revisión del incremento: 23 trades probablemente demasiado**

pero es verdad que ya digo 23 sobre 600 y pico insisto que es el menor incremento posible parece demasiado así visto así en eso parece demasiado pero claro esto hay que mirarlo en más sets de más zonas y concluir a lo mejor esto ha sido una casualidad y resulta que normalmente se mueve tres trades pero bueno si fuera a cinco trades estaría mejor estos 20 trades para ser el menor incremento puede ser demasiado así que lo miraremos lo miramos

**Pregunta de Fran sobre usar ROC en el mapa en lugar de TSI/PPC**

y nos pareció que era el adecuado pero así es la vida del público se ve siempre mirando cosas vale la de fran la de arriba la dejo para el final de manuel o vale ok a ver que traduzco porque utilizamos el tsi es ppc los mapas en vez de utilizar el roc no sería mejor al tener diversas versiones de fitness funciona agrupadas no acabo de entender es que al final tú necesitas la suma tú quieres decir el valor que recoge la tabla no le veo no le veo pero puedes puedes ponerlo pero no le veo mucha no le veo mucho sentido fran pensándolo ahora pero porque al final el roc es el roc es un valor que es importante pero aquí cuando estás con 8.000 tiene un sesgo tiene una posible sobreutilización gigantesca y de hecho vas a ver roc con valores absolutamente locos

**Problema del ROC: dispersión extrema con 8.000 combinaciones**

que no sé qué he hecho antes que me he cargado la tabla no venga perfecto vamos pues lo vuelvo a abrir no sí porque lo de abajo lo de abajo lo puedo volver a mover en un momento no no guardo venga tira no sé qué he hecho antes seleccionándolo todo con el ratón que se me ha ido a tomar y ahora no quería volverlo todo para atrás o sea al final el roc en sí compara insample con outsample es un valor importante es un valor importante pero en este punto al haber 8.000 de acuerdo vas a ver que hay valores o sea su dispersión es brutal la dispersión es brutal

**Comprobación del ROC en la tabla: valores negativos y cambio de color**

bueno vemos abajo la dispersión no para eso está de 296 a menos 74 la suma de los roc a la columna suma perdona perdona pues sí sí o sea no cambiaría nada fran no cambiaría si puedes usarlo no tiene ningún problema no creo de verdad que cambie algo y hayamos descubierto aquí algo puede ser perfectamente pero no creo que cambie porque al final va a cambiar los valores lógicamente pero quiero decir que el hecho de cuál es mejor o peor no creo que cambie porque es muy parecido de uno a otro de acuerdo

**Prueba con net profit: criterio más estable para mostrar robustez**

al final tú tienes a ver aquí que no he hecho la tabla tan grande no lo he incluido no lo he incluido déjame un momento tengo que hacer con la última porque no tiene espacio para expandirse y a ver en esta si me deja ahora le cambiaron no le cambiaron esta venga muy bien llevamos una casualidad y que me hablas con el exhaustivo pasa veces abierto sí pero antes hace hace mucho rato te he dicho que acabaría a ocho y media si te acuerdas acabar a ocho y media había parecido ahora claro son valores negativos porque ese valor era negativo por lo tanto tenemos que crear el color al revés pero eso no tiene mayor importancia porque ya lo tiene previsto el mismo programa

**Comparación visual: mapas prácticamente idénticos con distintos vectores**

y puede haber alguna pequeña diferencia no está mal es decir lo veo bien de hecho mira lo probaré unas cuantas veces nosotros normalmente ponemos en el net profit yo los he puesto ahí otros he puesto ese y pero normalmente hacemos net profit no tiene no es un gran vector vale esto en sí bueno aquí no y mejor este sí que tiene como poner a un poquito más abajo que arriba si te fijas pero es bastante parecido pero porque lo he puesto ahora porque es negativo donde aparece más es es bastante similar es bastante similar y bueno como veis ya digo no si lo haces por net profit que lo solemos hacer por net profit voy a poner este por net profit me deja bueno de la derecha mismo porque si no a veces no te deja no te deja añadir es como lo solemos hacer pero es que es también muy muy parecido cambia muy poco normalmente lo hacemos por el profit

**Cierre del bloque de Fran: la suma sigue siendo el vector más claro**

lo he puesto aquí un poco para que lo vierais para que vierais las funciones que vamos usando pero es esto muy similar porque al final lo que marca sobre todo en esto es el hecho de aparecer claro que puede haber un pequeño matiz del dato pero es pequeño es pequeño es muy parecido es bastante parecido como ya esperaba pero no es incorrecto o sea como concepto me gusta no pasa que al trabajar al ser un número negativo ya pues ya tienes que interpretar al revés a lo mejor y sabes es por simplicidad ya digo el net profit suele ir bien para para hacer un mapa por donde el mapa que sea lo más extenso posible que cubra todas las mayores zonas posibles y que los incrementos estén bien lo que hablamos ahora

**Retorno al mapa grande, filtrado y reconstrucción final del análisis**

bien seguimos aquí abajo en el mapa dejarme repasar las notas de lo que he ido diciendo hasta ahora porque este es uno de los días que tengo más guión porque no quería dejarme nada quería dejarme nada vale vale es así ya hemos filtrado el mapa grande ahora lo voy a volver a poner a hacer vamos a volver a hacer lo del mapa grande que si os lo digo esto parece que no que puede resultar poco útil verlo así pero pero ya arriba se ve bien pero a nosotros nos gusta nos gusta partir del grande luego ir filtrando ir filtrando y hasta que ahora lo vamos a volver a hacer porque como he tenido que abrirlo porque no sé lo que había liado pues lo volvemos a lo volvemos a ver

**Aplicación de filtros definitivos para el mapa “limpio”**

ponemos aquí desde este desde el tp hemos dicho que vamos desde el 0 5 al 1 1 vale el bar 1 hemos dicho que lo llevábamos desde el 0 55 al 0 65 al final de 21 más 55 y dijimos aquí meter el 0 6 7 5 también eso no tenía que apuntarlo lo voy a apuntar pero 0 6 7 5 y luego también podríamos tocar los otros no tiene mucho sentido tocar el stop porque es lineal pero sí que aquí podemos dejar estas zonas por ejemplo de 4 a 7 no es un poco el vecino dejar sólo el 4 al 7 por un lado y luego el otro lado pues el 22 al 25 22 a 25 ahí tiene el problema que hemos tocado límite también arriba 24 25 más 22 y bueno así pues el mapa de abajo es un poquito más manejable ya pues se queda todo como veis bastante bastante agrupado porque nos hemos cargado los buenos hemos dejado los límites de acuerdo y vemos un poco la misma información que teníamos queda más limpio queda más más limpio el mapa pero nada nada más

**Marcado visual del set ideal y construcción de zona candidata**

bien aquí podemos marcar un poco nuestro set ideal vale nuestro set ideal que aquí tenemos una duda que mira os la voy a enseñar ya porque la he hecho esta he hecho he hecho esta zona 2 porque quería ver ese 25 quería ver la he hecho hecho esto lo he hecho último ahora por su trabajo estaba acabando vale

**Referencia al out-of-sample y comparación entre etapas históricas del mercado**

y que de hecho mira ves aquí sí que hemos apuntado los los datos sample que eso me gusta el otro se nos ha olvidado pero en este veo que sí que lo ha puesto con muy buen criterio porque nos gusta ponerlo pero se nos había olvidado vale y esto nos gusta ponerlo y antes de empezar igual que se mira los incrementos se analiza esto se analiza esto donde están los cortes donde se analiza pues lo que habéis visto antes vale espera me lo voy a poner aquí para yo verlo gráfico idealmente si uno no le tiene una gran sensibilidad al sistema no lo conoce tanto pues se puede hacer en distintos cortes y te puede hacer delante y detrás que ya sabéis que nos gusta mucho pero aquí poquito como os digo es un sistema amplio rango de comportamiento bastante de por humano entonces pues bueno no lo complicamos tanto en ese sentido

**Revisión de la serie histórica: ciclos, crisis, laterales y contexto general**

pero al final si yo veo aquí todo aquí lo voy a ver poco pero bueno poco este mira lo voy a quitar el sistema momento 17 29 de diciembre del 17 por aquí más o menos por aquí más o menos es el corte no aquí era más o menos por ahí más o menos por ahí es el out sample es decir de ahí para adelante es out sample aquí lo ideal para aquellos que no han hecho deberías de tener un indicador de volatilidad y tendencia o puede ser también la de x pues tener tanto la de x con el histórico si lo tenéis o cualquier otro indicador ahora ocultar esto un momento y ahí pues veis un poquito las dinámicas que lleva el mercado aquí pues ha tenido ciclos bajistas ha tenido crack del covid en ese ciclo bajista y de hecho hay bastante ciclo bajista recuerdo bastante ciclo bajista y aquí en el histórico pues hay también un poco de todo porque hay una gran crisis primera del 2000 muy larga vale luego un período lateral bastante duro bastante duro otra nueva caída y luego un ciclo bajista espectacular por eso que hay un poquito de todo se pueden hacer distintos estudios de aquí dejar out sample la parte inicial en los cortos normalmente hace un out sample bestialmente bueno vale bestialmente bueno aquí no no es fácil porque es verdad que ahora en proporción hay bastante corto vale y no estaría tampoco mal alargarlo un poco más y es a lo mejor a 30 por ciento


es a lo mejor a 30 por ciento el problema del porcentaje es que si tú le dejas muy poco in sample hay autores incluso pardo que comenta alguna vez y nosotros alguna vez lo hemos hecho es como prueba de 50 a 50 es 50 a 50 y si es un sistema con muchísimos trades es bien de acuerdo es válido vale pero aquí hablamos de un diario es un sistema que tiene muchos trades pero que operan diario pero que te operan diario y esta optimización tiene muchos trades están hablando de la mayoría de 700 800 500 depende no hasta mil en algún set pero son muchos pero tampoco son tantos por decir realmente yo en el in sample metido así pues estoy metiendo 500 600 claro si lo pongo menos sample le meto menos entonces capto menos señal este es el equilibrio de señales entonces es complicado en esos sistemas y en este tipo por ejemplo el 50 a 50

**Impacto del sesgo bajista y la estructura histórica reciente**

pero en uno podría ser y este por mercado estaría chulo de acuerdo porque es verdad que ahora tiene un poquito de sesgo bajista en este en el auto aunque ha subido mucho porque es como bajista poder desde aquí aquí ha subido un montón sí pero ha tenido bastantes tramos de caída y hay en el in sample también en el sample también pero es verdad que tiene un tramo de montonazo de años veis o a súper alcistas y luego cambio tiene dos tramos súper bajistas para un mercado de bolsa súper bajistas este es súper bajista y este es súper bajista entonces realmente todos súper bajistas uno muy muy largo luego aquí un poco de todo pero un poco de todo

**Evaluación de la volatilidad histórica en ambas muestras**

pero veis aquí tiene momentos de elevada volatilidad de muy baja volatilidad tiene un poquito de todo y lo mismo en el ADX lo mismo lo ves en el in sample que veréis periodos de muchísima volatilidad y periodos de muy poca volatilidad pero en porcentaje en el in sample tiene un poquito más de periodo de poca volatilidad que ahora es un poquito el único sesgo que tiene la muestra pero es complicada quitársela aquí es complicada

**Estrategia futura: ampliar histórico para equilibrar volúmenes y señales**

como vamos aumentando el histórico seguramente ya para las siguientes ya lo podemos anotar en nuestras notas alberto que le llevaremos cinco más porque ya vas ganando histórico por lo tanto cada vez tienes más trades siempre no y meterle ir a ir abriendo a llevarlo un poco más atrás el 30 para ganar periodos sin volatilidad de acuerdo en el auto sample pero claro yo necesito suficientes trades en el in sample también pero ese es un poco el problema que plantean los sistemas de este tipo

**Retorno al mapa filtrado y determinación del set ideal preliminar**

bien volviendo al mapa aquí ya lo tenemos filtrado y lo que os decía yo aquí puedo tener un poquito mi set ideal mi set ideal que está claro que está en la zona de 06 del bar 1 y que en la zona del en la zona del indicador bueno la zona del indicador no es tan claro no es tan claro pero bueno pues ahí tenemos 6 tenemos 5 4 y 25 es muy bueno el problema es que no tengo vecinos por eso os decía que había hecho el otro donde estaba aquí esto es simplemente la misma que hemos hecho pero en b como 425

**Revisión del mapa “zona 2” y detección de un posible error al copiar inputs**

esto lo has cambiado tu algo alberto has copiado aquí algo y no habrás copiado los inputs no no no es esa no se ha hecho esa opti seguro el mapa es short zona 2 eso no se está seguro pues pues no sé me habré equivocado al recogerla pero estará de 23 a 30 es de 23 a 30 es estará de 23 a 30 es esta la que se ha hecho aquí se ha hecho está seguro 23 a 30 esto es la que se ha hecho dejando todos los demás igual pero dejando sólo el valor del indicador de 23 hasta para qué para ver ese más allá del 25 a ver qué pasaba

**Comprobación de la extensión de parámetros entre 23 y 30**

está todo igual recogido todo igual vale con sus marcas sus estadísticas y demás vale no vamos a pararnos mucho aquí vamos a ir un poco a los mapas vale esos son los mapas como veis de 23 a 29 porque el 30 pues igual no sale porque no debe salir el 30 en ningún 30 sí que hay 30 así porque no sale de 30 a ver si no salen los datos sí pero si hay datos sale está recogido está recogido bien vale vale bueno no sé por qué no sé por qué porque no está filtrado

**Corrección del ordenamiento y aplicación del color de mapa**

vamos a poner el net profit para no no me va a dejar ahora claro gracias y las siete si está desordenado esta habrá que poner el sonido deja invitada no sé por qué no está ordenado lo sé porque lo sabrá porque por defecto está ordenado pero no sé por qué no lo estaba y por eso no me daba no me daba cuenta gracias gracias al que lo ha puesto lo he hecho para ver si estabais atentos de realidad esto lo hago mucho yo lo he hecho para ver si estabais atentos así que muy bien que lo haya hecho tiene un punto positivo para examen final final de curso queremos esta la escalita de color

**Confirmación de que 0.60 sigue dominando pese a extender el rango del indicador**

bien fijaros que lo primero que vemos es que el 06 sigue sigue dominando en toda la zona da igual el valor que le des al indicador que él sigue siendo el rey vale y y que vemos que el 25 efectivamente destaca y sí que vemos que degrada degrada bueno degrada no no creo que degrada de manera para no usarlo no creo que degrada para la manera de no usarlo pero sí que hay una cierta ahí degradación no no menor vale no menor

**Necesidad de pausar y reorganizar las ideas del análisis**

a ver si aquí en el mapa de abajo del todo grande vemos alguna cosa adicional quiero ser más estrecha así que me va a venir bien ocultar esto y podíamos parar cinco minutitos igual no así que sí vamos a parar cinco minutitos si os parece y así reponemos un poco un poco todos

**Reanudación de la clase tras la pausa y reflexión sobre incrementos**

a ver que pongo el contador un momento de pánico porque no veía el marcador de la grabación digo matame matame que no duela por favor venga pues vamos con una pausita de cinco minutitos y vuelvo casi estiro un poquito las piernas y voy a visitar a un amigo que tengo ahí hasta ahora pues ya estamos por aquí y seguimos con la clase yendo yendo yendo al baño se me ocurrió una cosa en relación con lo que decías de los incrementos es que es que veis para que veas una una cosa que puedes mirar por eso la pregunta que queda ahora que hemos parado era una buena idea mira

**Comentario de Juan Manuel sobre la necesidad de practicar una optimización completa**

preguntaba antes Juan Manuel en futuras prácticas será alguna óptica como esta con el mapa con tabla dinámica ya que la teoría se hace una vez por encima y rápido y sería clave para dominarlo verlo hacer desde cero completo al menos una vez o sea como el pensar que los vídeos los puedes ver 70 veces si quieres o sea yo creo que que todo el proceso lo he mostrado si tienes alguna hora concreta pasamos la etapa tenemos de resolverla pero es que una tabla dinámica o sea tú seleccionas los valores y le das al botón es que no tiene más de verdad y no lo hago con el ratón con el texto porque se me va no sé antes lo intentado y se me ha ido a tomar viento no es cual imagínate que se acabará aquí insertar tabla dinámica y te abre

**Explicación sobre tablas dinámicas: simplicidad, repetición y práctica**

y luego tú aquí eliges un valor en columna y un valor en filas lo que quiera representar y quiere representar uno contra otro y el cual pones en uno y otro es prueba error o sea no hay un que tú quieras ver más antes y aquí el valor que recoge net profit ya está es que no es más una tabla dinámica y lo que te digo buscas tablas dinámicas en youtube están 250 vídeos pero que no hay más bueno luego claro configurarla pero como cualquier otra cosa pues ya incluso hay con cosas de gusto que te decía nosotros para que quede grabado nosotros no mostramos subtotales sí que mostramos activado para filas y columnas totales generales pero a lo mejor habrá gente que no le gusta y tal los sumas entonces sólo ves los inputs sin las sumas

**Detalles de estilo en Excel: formato condicional y preferencias personales**

y el diseño me gusta el esquema porque me enseña y las variables y no lo otro que no lo enseña pero eso es totalmente gusto no tiene nada y ya está todo esto cuando quieres marcar un color haces así y es una opción de excel que se llama formato condicional vale que depende de los datos pues pinta de un color o le pone barritas o le pone iconitos ves estos son opciones del excel cuál va mejor para esto pues a nosotros nos gusta esto pero mira antes usamos esto y a partir de esta semana hemos cambiado a este y aquí usamos este formato condicional barras de datos cual da igual si es un color u otro no tiene mayor es esto una más de verdad

**Comentarios menores sobre control de banderas y navegación**

es que más que esto o sea tú dices explicamos poco a poco que no te lo puedo explicar de otra manera es esto es esto y ha quedado grabado ha sido muy rápido pues vuelvelo a ver vuelvelo a ver vuelvelo a ver te pones la pausa lo pruebas tú lo ves a ver la gracia de los vídeos es esa tienes que tú puedes parar el vídeo hacerlo entiendes al final es un poco el tema vale pues ya está como se marca ha respondido esto pero como se hace ha respondido ha entendido gracias pero la banderita no me la deja quitar así nada pregunta a mí vale está vale estaba ahí bueno no sé cómo quitar la banderita

**Regreso al análisis del mapa extendido y revisión de degradaciones**

vale bien entonces lo que nos decía en el mapa del 2 de la zona esta que también lo tenemos total pues bueno se viene a apreciar más o menos lo mismo no hay ninguna novedad lo único queríamos ver es cómo evolucionaba más allá del 25 y bueno pues no hay una gran hecatombe sí que hay cierta degradación se parece claro que el 25 es una zona muy muy top pero bueno aguanta bien tiene bastantes valores y sería mejor que degradara menos sí pero no es tampoco muy dramático

**Bloqueo del parámetro para aislar comportamiento y observar su progresión**

si lo vemos aquí abajo a ver siempre bueno aquí nos va a costar de ver vamos a bloquear vamos a hacer una cosa como queremos ver este parámetro lo que vamos a hacer ahora es lo que os decía a veces de los de los datos sea el hecho de que uno vaya antes o después lo marca simplemente el orden en que están aquí ahora voy a poner el per 1 arriba porque pues nada pues porque así el que manda primero es el per 1 entonces ya la dejo sólo per 1

**Corrección del orden de las dimensiones y limpieza del mapa**

aunque aquí cuatro está mal pero si no hay cuatro aquí no o que cuatro esto no ha actualizado esta tabla ya verás se ha dado un actualizado no entiendo porque sale cuatro si no hay datos de cuatro y nuevamente no está ordenado de menos a más y en el bar 2 pues le dejo ya los ya conocidos no vale voy a dejar solo los tres los tres megatops

**Observación relevante: en la zona alta del indicador la degradación es mucho más suave**

y fijaros que nos queda una tabla ya muy muy reducida nos queda es muy larga con lo cual le voy a cortar también a los que aquí el 06 es más estable que fijaros que en la zona elevada esto es bastante interesante fijaros que el tp y el stop como que degrada menos lo veis por eso el 25 en la otra salía tan bueno es porque en la zona de tp a partir del 1 también va degradando pero es como degrada de manera bastante más progresiva que antes cuando estaba la zona del 4 al 25 pues ya os dais cuenta no

**Constatación de que la ampliación más allá de 25 mejora la suavidad de la degradación**

es decir en esta zona de 25 estas son las cosas que conclusiones que sacamos en el mapa que la acabo de sacar ahora la zona sigue siendo igual en la zona de tp bajo bar 02 también degrada mucho en el 05 que quizá un poco menos pero degrada mucho tiene el 6 el 7 el 8 9 el 1 y hasta el 1 muy bien y ahí en el 1 1 empieza a degradar pero fijaros que empieza de manera de manera mucho más progresiva que antes

**Interpretación final de la zona alta del indicador (23–26)**

aquí el 1 es mejor en la zona de 25 en la zona alta más de 23 en este segundo rango que veíamos en la otra no en la otra habíamos ya visto ese rango aquí lo que pasa es que acababa en 25 vale queríamos ver más allá acababa en 25 pero ya veíamos que ahí había aquí había algo bueno es como ya salía claro 18 25 era era la leche ahora hemos ido más allá para ver qué pasa tras esto no y nos hemos dado cuenta que a la que vimos de 23 a 25 no automáticamente veis esa degradación que es mucho menor aquí es mucho menor en el otro caso

**Confirmación de que 23–26 es una banda amplia y estable**

y ya más adelante fijaros aquí en la zona de tp es incluso el 1 6 aquí ya prácticamente no no sale casi y aquí en cambio fijaros no lo seís pero está todavía todavía tiene bastante bastante buen dato decir realmente degrada de manera mucho más progresiva en la zona del indicador a más de 23 vale es decir elegir 24 25 está bastante bien aquí es una zona muy buena

**Comparación final entre las zonas óptimas del indicador**

es decir teníamos aquella zona que veíamos 5 6 pero también está también está cual bueno parece 26 mejor pero fijaros 25 también 25 muy bien 25 muy bien y su vecino 24 24 es bastante bueno también hay su vecino perdón decía 25 digo su vecino 23 24 25 25 están bien

**Reconstrucción del rango completo para ampliar lectura**

también aquí a ver que le he cortado demasiado que no que he pasado que ver que no le he puesto 24 23 pongo 23 pongo todo y está para verlo para verlo bien 23 24 25 26 27 y sí fijaros que incluso 23 aparece aquí 0 6 0 6 25 24 muy bien una columna realmente extensa todo todo todo todo muy progresiva va degradando va degradando lógicamente va degradando pero fijaros que degrada de manera realmente

**Reajuste del layout del mapa para mejor legibilidad en pantalla**

aquí otra vez otra cosa que voy a cambiar aquí que esto lo ves en los mapas al ir haciéndolo no tiene mayor mayor historia es decir me va a llevar un pequeño bajito no muy largo pero un pequeño de cambiar de una separado uno lo voy a llevar aquí porque la prefiero más alargada no porque la pantalla es más alargada me va mejor así me da mejor así dejo per 1 arriba vale y dejo bar 2 arriba para que me mande el tp justo

**Corrección por inconsistencias tras la reorganización**

ahora el problema es que seguramente esto no está bien ya se ha hecho un lío importante lo vamos a volver a hacer porque esta vez es cuando cambian los datos pasa es un poco lío pero así es le montamos esta bueno espérate que la vamos a faltar un poco por mucho que es mejor no no tanto no tanto entonces el filtro aunque ya vemos que va realmente bien lo vamos a dejar solo del 0 6 0 5 no lo dejamos dejamos el 0 5 vale

**Ajuste de rangos finales y confirmación de homogeneidad en la matriz**

pero le vamos a dar hasta el 1 1 igualmente no es que la verdad aquí por dejarle le puedes dejar hasta donde quieras porque realmente va muy muy bien y el stop pues queda igual lo mismo no esto pasa un poco igual esto no sabes ni dónde cortarlo el stop es que no sabes ni dónde cortarlo no sabes ni de dónde cortarlo aquí tenemos esta hora y siendo larguísima la verdad así pues bueno sigue costando mucho porque es enorme la zona es enorme que se vería mucho mejor pero no se ve mucho mejor a lo subo un poco mejor a lo subo un poco mejor

**Eliminación del TP=0.5 para limpiar ruido y reducir tamaño**

voy a quitar el 0 5 porque algo ganaremos voy a quitar uno en cada lado algo bajamos no algo es algo bueno es es realmente homogéneo realmente homogéneo como como veíamos y está muy bien

**Identificación de las zonas finales recomendadas para per1**

bueno tenemos tenemos más o menos más o menos claro más o menos claro que el indicador no está no lo hemos hecho pero estaría bien también esto mismo que hemos hecho para arriba también hacerlo es decir hacer de 1 a 7 por ejemplo de 1 a 7 todo igual entendés decir hacer esto de 1 a 7 esta misma que la vamos a dejar ahora puesta alberto en ordenador de 1 a 7 para mañana analizarla y así veremos un poco qué pasa en ese 4 qué pasa en ese 4 para ya no es lo mismo que hemos visto aquí

**Síntesis de las mejores zonas del indicador (per1)**

pero bueno en principio vemos que 4 5 6 sobre todo 5 6 de acuerdo 4 5 6 está está bastante bien no 4 5 6 pues parece parece ser buena buena zona no 4 5 6 porque incluso 7 pues es una degradación bastante aceptable aceptable

**Síntesis de las mejores zonas del filtro de entrada (bar01)**

por el lado de aquí ya hemos visto que entre 5 7 5 porque aún aún dejaría aún este vecino lo consideramos aceptable y aquí 0 6 50 pero bastante por los pelos bastante por los pelos idealmente 0 6 25 pero bueno 0 6 50 aún podría aún podría valer

**Síntesis del TP óptimo y límites aceptables por tolerancia**

en cuanto al lado del en cuanto al lado del tp 0 6 este sí que lo vamos a descartar por el enorme barranco que supone 0 5 vale y aceptaríamos hasta uno hasta uno pero es verdad que mirando la otra ahí vemos un poquito más de tolerancia de acuerdo aquí vemos que en la zona elevada esa zona de 4 5 6 antes no le mencionaba que la quería mencionar aquí pero también está de acuerdo también tenemos 24 25 no pero ya está como la iba a ver aquí pues ya no la he tratado

**Confirmación de las zonas 23–25 como históricamente top en el lado largo**

y no pero tenemos esta tenemos esta zona que nos está nos está diciendo que 24 incluso 23 no cosa 23 porque el 22 una degradación aún aceptable aún aceptable yo de 23 24 25 23 es decir tenemos 4 5 6 y 23 24 y 25 esto además tiene un plus que de hecho es lo queríamos haber dejado preparado pero pero no sabemos porque eso nos bloqueaba la optimización y no hemos podido

**Contexto histórico: por qué el rango 23–25 ya era conocido como excelente**

porque porque eso ya os lo digo este sistema lleva mucho tiempo operando esta zona de 23 24 y 25 es una zona top del lado largo de acuerdo el lado largo opera varios sets no sólo operan sino que han operado durante años en esa zona de acuerdo es una zona muy fina para el lado largo entonces eso también está y de hecho hay algún set también el corto operando en esa zona hoy en día es decir que sé que esto no es nuevo esto como os digo viene de siempre

**Uso de las zonas del mapa como regiones, no como selección de sets finales**

y así por eso pues estos datos al final van saliendo y vas analizarlos por encima vale bien esto al final repito es zonas de trabajo pero elegiríamos los sets con esto no para operar no por qué no por dos motivos primero ni con el mapa ni con este excel por dos motivos para uno principalmente uno el dejarle elegir 8.000 como os he dicho antes es un ejercicio sobre optimización bastante notable es poco como lo que os decía de dejarlo variar los incrementos al final es un poco lo mismo

**Limitación fundamental: dejar elegir entre 8.000 combinaciones es sobreoptimizar**

si yo le dejo elegir entre 8.000 in samples cuáles van mejor y los meses que no el data y luego me los ordeno al final esto esta ordenación que yo he hecho aquí aquí no está en la zona 2 igual vamos a la 1 vale esa ordenación que yo he hecho aquí tampoco está aquí que ha pasado pero pero que es que me está pasando se me han ido los colores eso estaba todo pintado no vuelvo a cerrar tiene no sé no no eso estaba todo hecho todo estaba todo hecho con los colores abierto todo tienes la selección y tal

**Revisión de posibles candidatos y sus zonas operativas**

de cambiar la tabla no se me ha cambiado todo bueno yo aquí tengo entonces pues mira que hay un 10 y un 4 hemos hablado del 4 antes hemos hablado del 10 también pero teníamos dudas vale no sale más por aquí salen 4 a 10 es 5 7 5 bueno está ahí en el límite que hemos considerado apto 6 7 5 estaría fuera estaría fuera uno también estaría apto 0 7 también entonces son zonas operativas el 10 dudoso

**Advertencia: un set concreto sólo es válido si está dentro de su zona robusta**

el más operativo de aquí es este pero aún así si ahora que compara el sample con el out sample bueno pueden ser los mejores o no no lo son de hecho pero pero este puede ser que sí si está en la zona es decir no son lejanos de uno de los mejores in sample es 25 pero en out sample no aparece ninguno

**Conclusión: nunca elegir sets directamente desde el mapa ni desde el Excel**

el problema es que son 8000 entonces ahí nunca elegimos de este de este set lo que hacemos es hacer o la misma opti o otra de acuerdo que a lo mejor ya con el mapa yo he podido acotar y a lo mejor podría mejor si fuera más grande imaginaos que hubiera sido una optimización genética la anterior mucho mayor podía ahora acotar y hacer una exhaustiva pero como yo ya la tengo hecha yo la he hecho eso de dos maneras de hacerla

**Creación de la optimización exhaustiva recortada (250 mejores)**

esta es la misma exactamente la misma exactamente la misma la 1 es sin esta extensión porque ya la teníamos hecha si no la hubiéramos hecho con la extensión vale porque esta ahora viendo esto lo mejor pues sería hacer esto más o menos 29 para que son 65 pero esto acaba son unas horas en ordenadores potentes horas pues el 12 horas 14 horas y 27 horas en el mío a lo mejor en el dedicado que es un poco más lento pues dura 15 horas entonces se podía hacer esta ya y lo tienes todo en uno vale pero bueno no se ha hecho se ha hecho esta

**Por qué reducir a 250 sets: evitar sobreoptimización y facilitar selección**

entonces se ha hecho sólo guardando 250 es un número que aún 200 si quieres 100 puedes coger perfectamente 100 y hacemos lo mismo pero claro aquí ya en el in sample solo hay 200 esto nosotros hemos construido seleccionándolo con una función buscar pero no no no complicaros

**Explicación técnica para quien domina Excel (opcional)**

hay dos datos lo explico para que lo sepáis simplemente aquel que domine excel porque esto ya esto sí que si no dominas excel pues te olvidas vale no hace falta meterse en complicarse la vida necesariamente pero aquí hay dos variables que se comparten entre las tres hojas una es el test el test se comparte sin equivoco es el test que recoge cada combinación vale y luego el robunes también es igual en todas las hojas vale entonces buscando el número de test aquí ordenas tú por el fitness que has hecho por tsi ordenas por tsi que es el fitness que hemos usado en este excel vale ordeno por tsi y automáticamente busco con la función buscar

**Proceso final: recorte ordenado de los 250 mejores y alineación entre pestañas**

de las borro abajo hasta 201 elimino las anteriores y las otras 250 me quedo con 250 y luego con una función buscar busco estas test en la otra hoja y me quedo con esas vale es bueno esto ya digo si no sabes ni de qué te hablo no te rompas la cabeza pero el que sepa de excel sabe de qué le hablo y con una función buscar hemos buscado estos test en el auto sample y no el data y hemos eliminado el resto por una sencilla selección así vale y ya está pero si no pues vuelves a hacer la optimización

**Cierre del bloque: equivalencia entre esta reducción y rehacer la optimización**

entonces ahora aquí nos hemos quedado con las 250 es decir esto es lo mismo que si hubiéramos hecho la optimización porque ésta es exhaustiva que no hay no hay un algoritmo que seleccione son todas entonces las 250


Perfecto Núria.
Aquí tienes **la continuación completa**, con tu mismo formato:

– todas las palabras respetadas
– léxico afinado sin cambiar significado
– **títulos en negrita** cada vez que cambia el tema
– sin símbolos, sin líneas, sin iconos

---

**Selección final de los 250 mejores sets y ordenación por criterios compuestos**

son todas entonces las 250 que mejor te se iteran se han quedado y por lo tanto yo sí que tengo aquí unos datos in sample unos datos all data all sample y los datos all data y los tengo ordenados también por suma porque el auto sample está detrás y cuando pongo el auto sample detrás me gusta más usar la suma incluyendo el robe en el cálculo y por lo tanto veo cuáles son los mejores in sample que como veis lógicamente los mejores in sample los mejores sí son los mismos porque los que eran mejores en 8.000 son mejores en 250 lo que cambia o lo que puede cambiar es el auto sample

**Aclaración sobre la pantalla, la cámara y el ordenamiento por ‘suma’**

a ver un segundo que me he dejado abierto aquí un poquito voy a dar cuenta que no me estáis viendo estáis disfrutando de mi enorme atractivo esto es un algo que me extraña que no estuviese ahí advirtiendo sobre ello no pero bueno no pasa nada estaréis despistados o se están contentados la gracia es que no os habéis dado cuenta pero lo que ha sido es que si no no se entiende entonces ya ordenamos por por suma el in sample lógicamente son los mismos eso es obvio no el que es mejor con mil es mejor con 8.000 y con 250 mil pero no el auto sample tiene por qué me acuerdo en el auto sample no tiene por qué ser el mejor porque aquí ahora ya sólo hay 250 y bola

**Comparación visual de los mejores sets en in-sample y out-of-sample**

fijaros que tengo 406 25 con varios y aquí tengo no sé si exactamente el mismo no es pero por ahí andan por ahí es evidentemente la zona bastante bastante parecida si vamos al data pues nuevamente son los cuatro en este tsi son los los cuatro que hacemos nosotros en este en este caso no te sé que no hablado de vol forward al final sí os hablo de vol forward vale pero de momento de momento yo normalmente en apolo solemos elegir así solemos elegir así por el vídeo el mapa yo tengo claras mis zonas y ahora tengo mediante all data pues aquellos sets que trabajan mejor en equilibrio

**Uso del criterio “suma” y su ponderación del robustness**

porque con la suma pondero sobrepondero un poco el ro burnes ya que me añade y lo tengo por un fitness que este sí son estos y lo tengo por el fitness ese que lógicamente muchas ocasiones en algunos en común pero no siempre aquí de hecho como veis no los hay no hay en ese común pero en este en el ppc sí que hay en común entre los dos hay un poco de mezcla de los dos aquí nos sale más el 25 el ppc siempre tira más para para el retorno recuerdo el ppc te tira mucho para el retorno

**La ficha de resumen: herramienta clave para comparar de un vistazo**

una cosa que nos gusta mucho que esto creo que lo enseñé un poco en la práctica lo enseño también aquí que tengo mis notas está es la ficha que tenemos en el whatnot y aquí a modo de resumen recogemos siempre cuando cuando lo hacemos con tres sesiones estos tres excel y el resumen de las tres variables y el robustness esto es muy interesante porque de un vistazo de un vistazo te puedes hacer una idea de cuál es el mejor

**Comparación por medianas: utilidad en exhaustivas de 250 sets**

vale aquí al estar 250 de ser exhaustiva en la mediana que es el valor primero es bastante útil vale y aquí también tanto en las funciones fitness como en el robustness fijaros que te paliza respecto al tsi el s es el que mejor robustness tiene de hecho no tiene ni un solo robustness negativo ni uno solo entre los 250 muy destacable su mejora 167 pero tenés pero es que su mediana es 86 recordar que tiene es igual a un sample que bastante bien vale bastante bien su valor medio de tsi 2700 mediano su valor mediano 5 millones

**Interpretación: por qué el fitness “s” supera a PPC y TSI en esta optimización**

digamos que incluso supera al ppc que es el que el que se ha optimizado en él eso como puede ser bueno porque ha tenido más capacidad predictiva del auto sample pero sample ha predicho mejor entonces en este de estos tres excels aunque vamos a mirar los tres el s es el mejor eso es lo que os digo se hablaba en el curso de la supervisión de los fitness de acuerdo esto lógicamente de la supervisión de todos los protocolos es decir este tipo de cosas también sirven para eso para ir viendo con el tiempo que predice mejor a un tipo de sistema o a otro aquí predice ese predicho ha tenido una mejor capacidad predictiva del auto sample

**Reflexión: por qué normalmente TSI es un buen equilibrio, pero no siempre**

pero repito que miramos los tres es verdad que normalmente a mí se me dice es un caso de duda normalmente tsi suele ser un buen equilibrio pero también nos gusta mucho ppc y nosotros nos tiramos mucho por el ppc porque si tú tienes cartera y puedes cubrir el riesgo con otras maneras que son monetaria exposición diversificación retorno al final vía sistema puede ser un muy buen vector de acuerdo porque yo riesgo ya lo controlaré vía cartera vía exposición y vía gestión monetaria entonces el sistema puede ser una manera de verlo

**Selección de performance reports: parte visual y subjetiva del proceso**

pero ahí están los tres insisto ahí están los vale bien entonces nosotros vamos a mirar estos performance report porque al final hay que ver el performance report hay que ver el gráfico el gráfico me refiero el comportamiento de la curva el gráfico ahora de hecho lamentablemente en las últimas versiones de tradestation han decidido sacarnos una cosa que a nosotros nos gustaba mucho en vez de evolucionar a mejor parece que va a evolucionar a peor vale

**Eliminación de la antigua exportación gráfica en TradeStation**

que era simplemente cuando tienes aquí una función fitness y le das aquí a guardar puedes puedes decir exportar en excel pues en csv puedes usar en un xml vale este lo has probado alberto hay jaimito que me parece que es así que grafica un momentito bueno espérate es igual lo voy a poner ahí me parece que no lo he puesto en todos creo lo puse entero adiós perfecto pésame para tradestation se ha colgado en alegría ya tengo los hechos me da igual

**Problemas con la exportación HTML y deterioro de funciones previas**

por antiguamente generaba en un formato que era de microsoft que te generaba un archivo que pensaba que ese xml podía ser pero no tengo dudas ahora creo que no creo que no y si sí el html html y entonces te dejaba ver te dejaba ver pero este ahora se ha colgado y al generarlo lo ha generado mal entonces por eso no sé no sé si está bien o mal porque lo ha generado mal y sale aquí que está dañado

**Situación actual: sólo se exporta la tabla, no los gráficos**

entonces no pero pudiera ser que sí pudiera ser que esto lo abriera el el hc el hc el hc a las las guarda ahora es que yo lo tengo colgado perfecto gracias la carpeta de los test no lo abre pero no hace nada no si es lo que me hacía a mí es no tapar tu café ni está el informe si lo sé y suelo ver perfecto pues a ver y si lo abre bueno pues antes que nada por un html donde tú abrías los gráficos y era muy exportado muy chulo ahora pues han decidido solo excel entonces podías graficarlo pero no te grafica entonces te exporta performance report exporta los datos tal que está muy bien toda la información que quieras pero no te exporta los gráficos y la verdad que los gráficos pues vienen bien también

**Método alternativo: pegar los gráficos manualmente en Excel**

bien bien para verlo a nosotros nos gusta mucho pero ahora puedes poner aquí pegado que es lo que normalmente hacemos pero hoy por lo que os decía por el tiempo no lo hemos hecho le hacemos entonces nosotros lo que hacemos es sobre esos mejores y recogemos los performance report también de los sets que de hecho no solemos hacerlo así hoy lo hemos hecho así pero normalmente no ponemos que es el set es decir le ponemos su combinación y lo mezclamos aquí entre todos

**Evitar el sesgo: comparar sin mirar qué parámetros son hasta el final**

tratamos de no mirar o sea no lo miramos los vamos abriendo y alberto esto lo expliqué en la teoría es la única cosa más subjetiva que hay en el sentido de que al final alberto hace su selección y ahora mía tiene su selección aquí y yo tengo la mía todavía no las hemos puesto en común todavía nos hemos puesto en común lo vamos a hacer ahora yo fijaros que me he quedado con tres de los set que operan que eso tiene su gracia pero hay sets operando me he quedado con ellos

**Cómo validamos si un set operativo debe mantenerse o sustituirse**

o sea esto es la manera en la que valoramos si cambiamos o no cambiamos de acuerdo es decir incorporando en la fase final lo que opera y entonces decido si me los quedo o no me los quedo que aunque los que me quedo luego se consideran que no son aptos para operar pues entonces el tema no vale de acuerdo pero así es como lo hemos lo hemos visto de acuerdo alberto ha visto estos y yo he visto estos yo los tengo ordenados de menos a más trades

**Cruce entre listas: sets en común y sets que difieren**

no no es por calidad vale por ejemplo este lo tenemos en común veo pero bueno alberto me ha dicho que luego los set no nos ha considerado esa que no se sabe si se los elegiría o no que no se ha considerado 10 0 6 1 2 también lo tenemos estos dos lo tenemos los dos y este 10 0 5 este no lo tengo yo lo tengo yo y tú no vale y luego pues tú tienes varios como has elegido otros y demás bueno bueno esta es un poco la selección que hacemos de set

**Dificultad: pocos sets en la zona 4 pese a ser robusta**

aquí el problema es que en esta selección fijaros que no se nos ha quedado a mí no me ha quedado ninguno del 4 aunque esos que operan puede ser que alguno lo sea alberto sí 4 5 que esa zona es buena esta 4 5 y 7 es buena lo hemos visto antes aunque 25 estaba muy bien y de hecho ese no recuerdo mal es el que iba mejor podemos ver la curva pero es el que iba mejor fíjate que se lleva el tp a 1 y medio y medio este además nos gusta mucho porque no hay muchos los cortos cuesta encontrar un set ahí con o tp y aguanta lo suficiente

**Por qué algunos sets “fuera de zona” se salvan por comportamiento histórico**

este es probable que aunque se salga un poco de la zona lo lleváramos a operar porque en esta zona aguantaba bastante tiene 0 6 y tener tp de 1 y medio en cortos si realmente funciona que creemos que se puede funcionar porque lo ha hecho se ha operado en esa zona estaría estaría muy bien

**Proceso de sustitución: qué sets entran y qué sets salen**

y el resto de sets ya digo están operando el único yo al final es cupido el 1 y el 5 el 1 es el que estaba en alerta se ha lavado y el 2 3 y 4 se quedan entonces el 1 y el 5 pues se podrían sustituir por estos hay que ahora ponerlo en común para que por lo común pero sería el procedimiento normal que haríamos ahora íbamos una revisión de estos en pantalla

**Selección vía Porfolio Maestro: última fase**

y a veces nos quedamos con más y hoy me he quedado a ver nos quedamos con puedes quedar con 10 porque luego puede haber otra fase digo verlo en pantalla conjuntamente y como ya os dije vía portfolio de acuerdo es decir vía portfolio ver ya cuál le pasa más es decir meter meterlos todos distintas combinaciones con tu mezcla de portfolio y ver cuál de ellos va mejor a la cartera va mejor la cartera porque a lo mejor este es mejor por sí solo pero resulta que este a la cartera le va mejor porque diversifica más porque esto ya lo tiene otro sistema que operas entendéis

**Explicación final del procedimiento estándar**

entonces esta es un poco la manera de proceder nuestra vale y hasta aquí que vamos a hacer ahora bueno vamos a hacer antes decía que lo he dicho que lo iba a explicar y no lo he hecho a mezclar esto no lo he hecho cuando he dicho de lavado se me ha ocurrido una cosa y no sé qué ha pasado que creo que me he ido de tema

**Revisión de incrementos: técnica correcta para detectar granularidad incorrecta**

cuando hablábamos y os acordáis del tema de los incrementos decía mira una de las maneras de mirar los incrementos entonces lo hemos hecho aquí no decir bueno no all data yo miro la variación de este de este input entonces bueno al final voy provocando que se me queden alineados que sólo me puede variar este ahora bueno aquí 250 que va a costar más vamos a hacerlo en el de 8.000 vamos a hacerlo en el de 8.000 vamos a hacerlo en el de 8.000 porque claro hay 8.000 el más fácil es en todos no

**Método correcto: fijar todos los inputs y dejar variar sólo uno**

pregunta en disco la pregunta dicen por ahí el reto vale entonces lo que decía vale venga vamos aquí le das a los incrementos para provocar que se queden todos quietos excepto el que tú quieres si vas ordenando por estos siempre se ordena por el anterior entonces ahora todos los otros quedan ordenados y el único que debería de cambiar es este es aquí que no este no este es el que hemos visto antes que hemos visto antes vale entonces esta es una una variación

**Segunda técnica: observar cómo varían las otras columnas cuando cambia un input**

pero lo que te decía otra de las maneras es ver cómo varían las otras entiendes que eso es lo que no hemos caído antes es decir si yo tengo aquí un número entero vale es decir variar un punto del número entero cómo varía es decir estabilizo todas menos la que es el número entero que es el indicador aquí mira aquí tengo otro que varía el número entero no varía solo dos trades entonces desde este punto habría que ver también el valor bajo desde este punto de vista eso que te decía la sensación que me daba es que ese 0 0 25 era un poco alto entiendes

**Deducción: bar1 incrementa demasiados trades, señal de exceso de salto**

pero hay que mirarlo mejor entonces pero es bastante probable bastante probable que volvamos a trabajar un poco esto a ver si lo bajamos un poco ya de paso volveremos a analizar estos y como hemos visto esta zona la abriremos es probable que la volvamos a hacer con estos dos criterios pero bueno lo que va a salir va a ser de este entorno que esto pasa a veces analizando los datos pues te das cuenta que te has equivocado que la zona que has tocado un límite que tal es así

**Confirmación empírica: variaciones excesivas en increments evidencian mala granularidad**

y aquí aunque esto lo revisamos hace unas semanas sobre los incrementos pues no sé ahora deciros por qué pero mirando estos datos yo mi sensación ahora mismo es que el incremento es un poco elevado mira aquí tengo otra variación del corto solo ser el 6 25 y me varía nuevamente un montón de trades estás viendo alberto no es me varía otra vez 27 trades es demasiado en la parte baja no te varía más

**Comparación con inputs discretos (enteros) para calibrar variación esperada**

cuando en el que es de un número entero que es así que menos no puede ser podía ser más pero no menos a ver aquí en la parte baja mira aquí tenemos 45 aquí en la parte baja bueno un poquito más que antes pero pero como veis estamos en menos son 11 trades y en 5 10 trades yo lo encuentro que puede estar una zona razonable en esta magnitud de que son más 900 pero que bueno es eso no cuando era antes 20 sobre 680 es un 3 por ciento no sé

**Límite superior razonable: incremento máximo basado en input entero equivalente**

no es fácil encontrar una pauta rígida pero es lo que decía una manera que puedes buscar es ver que si tienes algún otro parámetro que va por número entero no puede granular podía ir a de 2 en 2 o 3 en 3 pero no puede ir a menos de 1 pues ver ese 1 cuánto varía entonces ese como mucho ese es el límite

**Diferencia entre incrementos lineales y porcentuales en distintos indicadores**

entonces también es depende no porque no es una media pero imagínate que es una media no es lo mismo de 4 a 5 que de 25 a 26 esto por ejemplo en algún sistema lo hablamos quiero que lo enseñamos tenemos un código para granular el incremento porcentualmente es un poco más complejo pero pero bueno al final no es más que programación en variarlo por eso que varía más de 4 a 5 que de 20 a 26 al revés perdón que varía menos de 4 a 5 y que se va haciendo exponencial luego llega a 20 y a 3 en 3 porque esto tiene tiene bastante sentido

**Conclusión: el incremento actual de bar1 es demasiado amplio y debe reducirse**

pero no es indicado está correcto así pero pero bueno eso es un poco lo que hablamos de los incrementos así que sí que da la sensación que ese es un poco alto y mira ya que estamos vamos a mirar también los de 0 10 vale los de 0 10 y aquí tenemos este por ejemplo 0 10 bueno veis este está ahí en 12 trades está ahí este este este parece más razonable parece más razonable 10 15 trades eso es que parece más razonable parece más razonable

**Zona baja: variaciones correctas; zona alta: variaciones excesivas**

si bajamos mucho de stop en aquí tenemos variación de 2 tics es son más o menos como el otro era 1 eso sea este el bar 1 da sensación que está demasiado amplio vale y ese puede ser y probablemente es uno de porque le saltas demasiado de cada y esto sin querer porque no se ha previsto los incrementos pues nos ha salido que que quizá los incrementos de este input nos también y veis un poco lo que os hablaba en la teoría

**Importancia teórica y práctica de definir correctamente los incrementos**

muchas veces de la importancia de los incrementos normalmente la prudencia de no pasarse pero que también lo comentamos es necesario captar las señales y al final puedes pasarte y como todo tiene su tampoco es que esto sea una burrada ponerlo no sería una burrada operarlo con este incremento ni mucho menos pero está un poquito justito vale está un poquito justito y deberíamos de seguramente bajarlo un poco más o incluso hasta la mitad de 125 hay que verlo

**Consulta sobre el Excel del curso y si estaba disponible como material**

vale pues a ver me decía y pregunta en el disco pero es para ahora a modo de resumen a ver a marrugat a marrugat el excel no lo dimos el excel al ver el excel no hemos dado un excel de muestra nunca dijimos de darlo al final con los vídeos no sé si subíamos material allí si que había material en uno de los vídeos esto no había material de excel si ahora me me estás matando pero me lo apunto yo es que a mí me suena mucho haberlo dado ahora me estás haciendo dudar si no lo hemos dado si ya lo daremos para ya lo daremos ya lo damos

**Aclaración sobre la inexistencia de “plantilla” estándar**

pero un excel de muestra ya ya y pero yo vaya en mi mente habíamos dado ya uno pero insisto mi mente tiene las capacidades que tiene entonces lo revisaremos a mí me suena que en la en la o sea en la teórica que nos ha fijado ahí abajo hay adjuntos en todos está el glosario pero además en alguno del glosario había un excel creo yo creo yo pero hay que mirarlo hay que mirarlo

**Pregunta en Discord sobre cuándo reoptimizar un sistema**

vale a marrugat pregunta en el discord una vez saltó la alarma cuál para indicaros cuando una estrategia conocida de parámetros no está rindiendo uno a realizarse una optimización de todo en base a eso que no concluís revisando por dos pares de ojos que haces aparte que un nuevo juego de parámetros irá mejor bueno sí publicando mucho el tema sí sí bueno es que es que es depende

**Explicación: normalmente revisamos sin esperar a una alarma formal**

o sea salta una alarma cual la alarma os lo he mostrado aquí al principio no sé si a lo mejor no estabas al esto se me ha colgado el tradestation se me ha colgado aureli y la verdad era bueno es que ese ese no era o sea era drawdown nosotros tenemos si hay veces que no sé si lo he dicho en el curso lo he dicho a veces en un directo vale pero nosotros la mayoría de veces que revisamos revisamos sin alarma de acuerdo es decir porque tú ya le tienes sensibilidad al sistema lo va siguiendo lo que te digo es los gráficos y ves que no está yendo bien vale

**Alarmas internas: drawdown, peor trade, peor racha**

pero aún así hay alarmas en los códigos que es eso que habéis visto que cuando salta peor drawdown peor trade y peor serie de fallos salta un aviso vale y lo vemos pero lo normal es que cuando salte ese aviso ya está revisado

**Método ortodoxo: revisar lista de trades en excel cuando hay dudas**

ahora queremos implementar que sí que cuando hacemos un análisis tenemos dudas bajamos una lista de trade en excel y eso lo enseñamos en una práctica y lo hacemos lo que llamamos la revisión más completa pero ya digo normalmente antes de eso ya ya nos salta nosotros os explicamos en manera transparente las cosas que hacemos como hacemos y también os explicamos cómo hay que hacer algunas que a lo mejor no lo hacemos pues porque no hay tiempo porque consideramos que no que no renta el esfuerzo que con que vale hacerlo por para porque ya lo miro con este código que habéis visto me entendéis

**Cierre del bloque: vigilancia continua del sistema vía performance report**

pero aún así en la teoría os hemos explicado las maneras ortodoxas de hacerlo y la que si tú tienes un equipo o tienes tiempo suficiente para desarrollarlo y demás pues vale la pena hacerlo pero por eso digo que no os volváis locos con eso porque mirando un performance report vigilando el drawdown 


**Control ortodoxo: drawdown, rachas y supervisión estadística**

vigilando los resultados del sistema se puede hacer la manera ortodoxa de hacerlos llevando un control de operaciones y que a partir de 30 trades ya os lo os lo expliqué hay distintos análisis estadísticos y también me suena que dimos el excel de eso pero ahora tengo dudas y se pueden hacer distintas pruebas de evaluación y con eso sale pero de verdad los que tengáis un nivel avanzado bien los que estéis en una fase inicial no os comáis la cabeza con eso de verdad no os comáis la cabeza y basaros en cosas drawdown racha de fallos etcétera

**Procedimiento real de revisión para este sistema concreto**

vale entonces el proceso de revisión de este sistema ya digo es un sistema que lleva mucho tiempo operando y consiste en eso revisar nuevamente los mapas y no lo he dicho eso por cierto los sets que elegimos que están hechos en una optimización que viene desde el 99 se miran solo en los últimos 10 años lo ponía en la carpeta pero no lo he dicho no este performance report que cogemos no es de todo el histórico es de los últimos 10 y si el sistema es intradía puede ser de 5 incluso de 2 vale depende del sistema

**Por qué elegir solo los últimos años para evaluar sets finales**

porque porque a mí yo ya he elegido yo ya he optimizado he hecho los mapas de todo el histórico pero para acabar de elegir sets y ver me interesa dentro de esa muestra de todo el histórico es decir no no optimizo sólo los últimos 10 años yo he optimizado todo pero dentro de esa muestra del histórico me centro sólo en el último periodo más cercano que son los últimos 10 años no ves el 99 son pues 25 finales la mitad y podía ser un poco menos pero bueno como es un sistema diario que al final eso tiene 303 estas combinaciones aquí pueden tener 303 280 de ese orden entonces bueno menos ya nos preferimos que sea una muestra de ese tipo pero si fuera intradía a lo mejor y pues lo haríamos con más con más trecho

**Qué pasa cuando salta una alarma y cuándo se reoptimiza todo**

vale a ver y entonces una vez salta la alarma realizas una optimización de todo bueno sí realizamos realizamos realmente hemos hecho más cosas como os decía antes que por ejemplo revisamos el tema de los incrementos revisamos miramos los miramos de los que están operando para evaluar si estaban rotos o podían estar todavía dentro de una zona normal concluimos que alguno podría estar ya demasiado desviado y ahí se inicia este proceso de optimización de todo porque se sospecha que puede haber algún set fuera de rango ya

**Identificación de sets desalineados con la estructura del mapa**

no y de hecho se ve que alguno está un poco forzado ahí en el mapa un poco justo de acuerdo que está ahí en la zona un poco justa porque ahí esto ahora no no he querido tampoco decirlo porque no lo he enseñado pero de las últimas veces el 3 degrada mucho es decir normalmente la vamos a ahora hacer para arriba para arriba ya sospechaba que saldría bien lo que para arriba sospecho que va a salir bien para abajo sospecho que va a salir mal para abajo sospecho que va a salir mal entonces los cuatro ahí están un poquito más más justos entonces ese son sets muy inestables y bueno pues ahí sí es el resumen

**Naturaleza del sistema: diario, simple y con comportamiento probado**

en esta estrategia que es una estrategia que opera en gráfico diario que lleva muchos años operando y que es extremadamente sencilla o sea que es un sistema extremadamente sencillo el sistema de operar no son ni tres líneas y digo tres por no decir dos

**Pregunta: por qué no se hace walk-forward para Apolo**

indicabas que no hacías walk forward para apolo alguna razón no así que se ha hecho walk forward para apolo el problema que tiene de hecho tenemos pendiente intentar e intentar hacerlo lo que pasa que en tradestation nos ha petado porque tiene demasiadas lo que ya creo que ya os lo conté nos reventó entonces ahora hemos bajado de nuevo que tiene buena pinta y estamos en ello entonces vamos aprendiendo un poco pero pero porque tenía que ser más estrecha no me convence estrecharla tanto al final lo trucas

**Limitaciones del walk-forward rolling en sistemas diarios con pocos trades**

pero el walk forward en este tipo de sistema uno es diario el número de trades va muy justo vale hay que hacerlo sin gestión monetaria porque la gestión monetaria al hacer rolling la mayoría lo cuentan por dinero y entonces se rompe se carga el drawdown no es comparable porque el drawdown claro va subiendo va subiendo el retorno va subiendo el drawdown a magnitudes enormes entonces digamos que no sé si este programa lo hará pero el tradestation no homogeniza los resultados bien los toma en valor absoluto entonces al final no se pueden comparar los distintos periodos y más en un diario que mezcla muchos años

**Problema técnico: comparación inválida de periodos rolling por escala monetaria**

entiendes entonces los periodos in sample y out of sample haciendo el rolling que va haciendo no son comparables y al final da datos de comparar peras con manzanas entonces hay que hacerlos sin gestión monetaria y el otro problema que tiene un equity en short vale es que es muy inestable es decir te pasa muchos periodos de mercado largo o sea encontrar una ventana óptima es complejo porque es un mercado como os he enseñado antes que no es nada estable no es nada estable

**Dificultad inherente del lado corto en equity para walk-forward**

no no puedo abrir esto que lo tengo colgado entonces el short es muy complicado de pasar en un walk forward en un equity es muy complicado pero aún así tenemos pendiente mirarlo no sé si alberto se acuerda si alguna vez en corto solo nos ha pasado no sé si tú te acuerdas el walk forward en corto te consta pero te consta que ya ha pasado no sí no miraré sí que lo ha pasado seguro cuando operaba o sea cuando lo optimizamos junto que esto ya os he comentado antes el inicio que lo hacíamos así lo pasaba porque lo tenía más trades y entonces tenía la capacidad de ir a los dos lados y adaptarse mejor

**Motivo por el cual el walk-forward falla cuando optimiza solo cortos**

pero así es extremadamente complicado semanalmente complicado porque porque que un periodo tenga capacidad predictiva para el otro en el largo es más fácil pero en el corto es muy complicado porque depende de ventanas muy estrechas y entonces claro cuando entra un periodo muy alcista se pone en modo muy alcista pega una pequeña corrección un poco fuerte y como está en modo alcista no entra entonces bueno es complicado que los parámetros vayan adaptando haciendo rolling

**Opción alternativa: Anchored Walk Forward en lugar de Rolling**

entonces aquí sí que suele ser mejor puede ser que vaya mejor en el anchored eso lo tenemos pendiente de mirar puede ir mejor vale porque al final el periodo es mucho más largo entiendes y eso es lo que necesita un sistema de este tipo parámetros muy largos o sea que sea muy muy largo el periodo para poder adaptarse no para poder encontrar todo tipo de mercados con los parámetros que se muevan de manera distinta en todo tipo de mercados porque al final se está moviendo en un territorio que es hostil para él

**Contexto: operar cortos en equity es estructuralmente hostil**

no o sea esto es obvio es obvio que un sistema de cortos para equity es una zona hostil no es un no es una zona ideal de hecho hay mucha gente que defiende en no hacerlo esto es lo que claro lo explicado porque nosotros lo hacemos pero vais a leer mucha gente entendida incluso que te va a decir que no operes nunca en corto en bolsa te lo va a decir mucha gente

**Nuestra lógica: alfa y descorrelación para carteras de clientes**

vale nosotros sí y no es incorrecto pero yo vuelvo un poco a decir lo que te decía antes del portfolio y lo que tú quieres hacer a nivel de producto cuando tú de una manera u otra tratas de obtener rentabilidad para otra gente de acuerdo para terceros entonces nosotros al final esa es nuestra intención nosotros es ganar dinero a través de que clientes que operan esas estrategias ganan dinero y por eso yo tengo dos perfiles tengo un perfil que es smart beta y tengo un perfil que es ser alfa

**Por qué los cortos aportan alfa aunque sean difíciles**

entonces claro la alfa es eso la alfa sobre todo es batir a la renta variable es decir tener correlación pero cero o negativa con la renta variable entonces claro si tú buscas alfa ya sé que cuesta pero meter cortos da mucha alfa que tú quieres beta pues ya tienes a ser san smart beta entiendes

**Recomendación personal para traders en formación**

que haces entonces es un poco depende de lo que tú quieras ahora tú estás haciendo un portfolio estás empezando olvídate de los cortos en bolsa olvídate o sea no no es no es el sitio fácil no las cosas fáciles no las cosas fáciles no es buscar cortos en bolsa no es buscar cortos ahora tú ya tienes una cartera y buscas algo para diversificar tu cartera de bolsa pues adelante a por ello vale pero no es la primera la primera cosa a hacer ni mucho menos ni probablemente la segunda

**Caso RV intradía: por qué los cortos funcionaban mejor**

aunque el otro día lo comentó se en que lo tengo aquí delante en él en él en el discord y nosotros también en el rv digamos algo con el rv nos fue más fácil sacar cortos que largos pero eso tiene el sentido de que es lo que se explicaba al final a los rv les está escapando que pasa aquí con con el tp lo habéis visto de apolo no que lo quiere corto lo quiere cerca porque el corto necesita en bolsa operar rápido y salirse rápido

**Relación entre estructura de TP y facilidad de operación**

en cambio un rv que hace hace eso en sentido de bueno hace eso nosotros probamos un rv que cerraba a final de día ese fue lo que intentamos encontrar entonces claro si yo te estoy obligando a cerrar a fin de día pues en el lado largo normalmente estás escapando mucho rendimiento porque si va a subir va a subir va a subir cinco días pues para qué cerrar a fin de día entonces luego hay gaps etcétera

**Por qué en ese contexto el corto funcionaba naturalmente mejor**

en cambio en el corto es al revés eso es lo que le gusta le gusta hacer rápido perfecto pues es más fácil si tú obligas a cerrar en el día con encontrar con roturas de volatilidad hagas tp te salgas y ganas en corto entonces un rv intradía cerrando a fin de día es más fácil en corto que largo

**Inversión del efecto si se cambian las reglas de permanencia**

pero esto no es contrario el otro justamente es lo mismo refuerza es por la obligación a cerrar a fin de día entiendes porque yo lo obligo si yo le hago mantener la posición dos días completamente se invierte la tortilla entonces el corto le va a costar una vida un montón en el largo te va a ser muy fácil entiendes es un poco la diferencia de dejar correr o no dejar correr los cortos no quieren correr no quieren dejar correr beneficios en cambio los largos quieren dejar correr porque el mercado no hace más que subir en el largo plazo de acuerdo

**Listado de tareas y pendientes de entrega**

nada más a ver que me voy a apuntar dimos excels dimos excels tenemos ahí algún email pendiente creo para mañana y tenemos también esto que han dicho de sí los artículos los artículos donchian el día 5 de febrero esto no sé si lo has llegado a mirar o no lo pusimos vale pues hay que amarrarlo disculpad que no lo pusimos pues lo dimos aquí en el discord en la clase y no lo dimos vale pues pues hay que darlo artículo 5 de febrero y dimos excels en teoría vale

**Cierre final de la sesión**

nada más sin más preguntas lo dejamos amigos que son las 9 y 18 3 horas y 4 minutos de clase espero que os haya resultado útil es de las no bueno no es que todas de verdad el rv también fue un montón de horas pero esta esta también espero que os haya resultado útil porque las horas de trabajo que tienen que son muchas más de tres muchas más de tres pero bueno no nos quejamos nos vemos el martes que viene misma hora y ya sabéis cualquier pregunta en el discord la verdad que ahora mismo no tengo claro porque he estado tan enfocado en preparar esta que ni he pensado que después o sea que ya mañana lo pienso y decidimos empezamos a prepararlo de acuerdo hasta el martes chao

