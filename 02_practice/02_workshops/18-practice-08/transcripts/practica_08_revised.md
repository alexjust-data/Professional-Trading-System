**Cargo estas estrategias a TradeStation desde este file:**

[PRACTICA 2008.ELD](../PRACTICA%2008.ELD)  

1. **Función:** AvgNormalizedTrueRange
2. **Indicador (ShowMe):** Curso_Filtros-ORB
3. **Estrategia:** Curso-ORB-04
4. **Estrategia:** Curso-VB-01
5. **Función:** NarrowRange
6. **Función:** NormalizedTrueRange

Hay funciones, indicadores, filtros y estrategias porque probablemente se exportó:

* Exportas “todo el workspace”  
* Exportas desde File → Export EasyLanguage sin desmarcar indicadores/funciones  
* Exportas desde un proyecto que tiene dependencias  
* La estrategia usa funciones personalizadas que TS incluye automáticamente en la exportación


¿Se puede hacer un archivo con SOLO la estrategia? Sí. 100% posible. Solo tienes que exportar esa única estrategia, sin ningún otro objeto.
<br>

---

<br>

**Configuración del horario del gráfico y diferencias entre “local” y “exchange”**
cuando cargas un símbolo en el gráfico a puedes elegir aquí local o exchange 

![](../img/000.png)

vale dependiendo del tipo de sistema si tú usas horas y tú te refieres a time por ejemplo pues es time es la hora del gráfico entonces tú le has puesto 9,30 y el gráfico va de 15 30 a 10 de la noche que es lo que te pasa en este caso como yo lo tenía ahora en el local lo que pasa que este este sistema en concreto lo veremos más adelante es un `volatilidad breakout` no es `ORB` por lo tanto no usa horas, y no importa, pero si tú usas horas pues puede provocarte eso que no opere, vale, que no opere, porque pues la hora no existe, si tú le dices a partir de las 9 30 hace x cosa, y es ahora no está en el gráfico, pues ese puede ser un motivo

**Configuración del Money Management y exposición del sistema**  
que más comentaba Juan Manuel   

***-Respecto a la configuración previa del MM para el sistema durante la evaluación para saber que exposición neta del sistema en % se recomienda por consenso en un sistema (apalancamiento) y cómo establecerlo por código o configuración en la plataforma. Es decir que pasos debemos seguir una vez queremos optimizar el sistema y ajustar su MM previamente. Por otro lado el MM para ajustar la exposición a la volatilidad y su código que tan comentado es lo veremos próximamente?***

bueno no hay una recomendación de apalancamiento no apalancamiento eso es algo que depende de muchísimos factores desde uno personal, o sea en evaluación preliminar y lo que os comenté es que nosotros aunque también os lo dije, sigue habiendo cierto debate sobre el tema, es decir, nosotros ya ya os explicó siempre os he intentado explicar las cosas que que digamos la industria pues tiene ya establecidas y cosas que al final hacemos nosotros porque nosotros las queremos así, entonces por haber distintas opiniones, nosotros creemos que es mejor usar un `money management`, pero es verdad que si tú tienes el gráfico ajustado, si tú tus stops se ajustan a la volatilidad,,,   
no es grave de acuerdo no es grave que vaya todo el rato con un con un contrato pero es importante que se ajuste y aún así habrán el ejemplo del nasdq que es el que pusimos en el curso es muy paradigmático no porque la variación de precios si miras en muy largo plazo es es bestial y claro un contrato del 2000 no es un contrato del 2009 y mucho menos el 2024 entonces si tú todo el rato vas con un contrato pues ese es el problema entonces realmente usamos un money management que simplemente permita que esos contratos varíen, el que estáis viendo hasta ahora en estos ejercicios que hemos hecho en la práctica es anominal, vale? 

```sh
Contratos = ((Start_Equity * MMVar_Start * 0.01) + (Profits * MMVar_Profits * 0.01)) / Value1;
```
Simplemente utilizamos el importe que asignamos a la cuenta y lo multiplicamos por un factor de 100. Lo único que hacemos es separar el capital inicial del beneficio acumulado, aunque en la práctica ambos se tratan igual, ya que en los dos casos se multiplican por 100 y luego se suman. Por tanto, el resultado final es el mismo.

Esa cantidad se divide entre un valor que corresponde al cierre multiplicado por el Big Point Value, lo que representa el valor nominal del activo. En otras palabras, este cálculo refleja el importe total que controla un contrato en función de su precio y del valor monetario de un punto.

En este caso, la gestión monetaria que estás viendo equivale a operar con una exposición del 100 % del capital, es decir, todo el capital disponible está invertido. Se trata de un modelo muy sencillo, pensado simplemente para evaluar cómo evoluciona el sistema sin añadir complejidad adicional.

ya te digo ese money management no deja de ser ***instrumental*** un poquito para que no estés todo el rato con un contrato y te adaptes a los distintos precios a lo largo del activo se puede ajustar a la volatilidad, sí, pero ya digo el ***money management*** como tal para operar en la práctica uno lo hemos visto y sí en estas prácticas que te comentaba ahora pues lo veremos lo veremos y ahí sí, que lo ajustamos a la volatilidad y sí que os enseñaremos lo que hacemos nosotros que al final pues bueno sí tiene algún detalle propio, pero que como ya os comenté la teoría se basa en ***fixed risk*** el único podemos decir particularidad que tú tienes y mucha gente lo hace así es decir es como mides***risk*** de acuerdo como mides risk es decir al final el risk va en el denominador y ya lo veremos y pueden haber varias formas de hacerlo 


<div style="border-left: 4px solid #f1c40f; background: #fff9e6; padding: 10px 15px; margin: 10px 0;">
<strong>Explicación del cálculo de contratos y lógica de gestión monetaria:</strong><br>
Evitar el daño en mercados adversos.  
<br><br>


```sh
Contratos = ((Start_Equity * MMVar_Start * 0.01) + (Profits * MMVar_Profits * 0.01)) / Value1;
```

representa un modelo de ***dimensionamiento dinámico de posición*** (*position sizing*) basado en la evolución del capital y en un apalancamiento porcentual configurable. En esencia, el sistema determina cuántos contratos debe operar en cada momento en función del capital inicial, las ganancias acumuladas y el valor nominal del activo.

---

**Desglose conceptual**

* `Start_Equity`: es el capital inicial de la cuenta, el punto de partida.
* `MMVar_Start`: es el porcentaje del capital inicial que se desea arriesgar o usar para abrir posiciones (por ejemplo, 100 equivale al 100 %).
* `Profits`: son los beneficios acumulados hasta el momento.
* `MMVar_Profits`: es el porcentaje de los beneficios que se reinvierten en la operativa.
* `Value1`: es el valor nominal de un contrato, calculado normalmente como `Close * BigPointValue`, donde:

  * `Close` es el precio actual o de cierre del activo.
  * `BigPointValue` es el valor monetario de un punto del activo (por ejemplo, en el ES un punto vale 50 USD).

---

**Interpretación práctica**

El numerador de la fórmula suma dos componentes:

1. El ***capital base ponderado*** (`Start_Equity * MMVar_Start * 0.01`).
2. Las ***ganancias acumuladas ponderadas*** (`Profits * MMVar_Profits * 0.01`).

Ambos se multiplican por un factor del 1 % (de ahí el `0.01`) para permitir expresar el riesgo o exposición en términos relativos.

El resultado se divide entre el valor nominal de un contrato, lo que da el número de contratos que el sistema debería operar para mantener una exposición coherente con el tamaño de la cuenta. En la práctica, esto equivale a una exposición del 100 % del capital, porque todo el capital disponible se traduce en posición.

Este método se usa sobre todo en sistemas proporcionales o de apalancamiento lineal, donde el tamaño de la posición crece con las ganancias. Es una aproximación simplificada al ***Fixed Fractional Method*** descrito por Ralph Vince (1989) en *“The Mathematics of Money Management”*, pero sin recurrir a fórmulas de riesgo complejo ni a fracciones óptimas (*Optimal f*).

---

**Ventajas y consideraciones**

* ✅ **Simplicidad:** es fácil de implementar y comprender; permite evaluar la evolución de la estrategia con una gestión monetaria proporcional sin complicaciones.
* ✅ **Escalabilidad:** al reinvertir beneficios, el sistema crece exponencialmente si la estrategia es rentable.
* ⚠️ **Riesgo:** no incorpora control explícito de drawdown o volatilidad, por lo que puede ser agresivo en fases negativas.
* ⚙️ **Equivalencia práctica:** como tanto el capital inicial como las ganancias se multiplican por 100 %, el efecto final es una exposición total al mercado (apalancamiento 1:1). En un entorno real, bastaría ajustar el multiplicador (`MMVar_Start` y `MMVar_Profits`) para controlar el riesgo, por ejemplo, usar 50 % o 25 % para una exposición parcial.

---

</div>

**Clases temáticas y prácticas sobre gestión monetaria**  

en las prácticas probablemente a partir de la de cuando acabemos ya este este volatilidad breakout que confío que va a ser hoy entonces seguramente abordaremos algunas clases como llamarlas temáticas, de no solo de hacer un sistema aunque en los ejemplos pues se pueden hacer sistemas, pero pues ya ya lo comenté durante la teoría y también lo he hecho en las prácticas, pues por ejemplo busqueda de ideas, por ejemplo el aprovechando en la revisión que hemos hecho de apolo, que lo comenté y ha habido algún comentario en el disco, estoy de acuerdo que es interesante, pues haremos una práctica de eso, mirar un sistema real se sale de su zona, los revisamos exponemos un poco esta práctica de un caso real de la revisión de un sistema que está operando entiendes entonces porque creo realmente que es algo que puede aportar mucho mucho mucho valor, y por supuesto gestión monetaria, ahondaremos en la gestión monetaria de manera específica, lógicamente con ejemplos prácticos, pero que la clase estará más enfocada en la gestión monetaria que en el hecho de hacer un sistema como por ejemplo hoy, que estamos más enfocados en hacer un sistema


***-Una vez tenemos un sistema en live o paper ,cómo crear y gestionar un backtest/performance report de los datos de live/paper de nuestro sistema de los trades efectuados.***

no entiendo 

***-Tengo dudas sobre la configuración de slippage según activos, ejemplos en futuros y acciones, ya que he visto que a veces se pone una cantidad fija monetaria y a veces 0.01-2 y cuando usar per trade o per share/contract que vi que a veces usáis uno y otro***

no, realmente siempre usamos la misma forma, es verdad que traystation aunque hizo un cambio hace mucho ha cambiado un poco en eso, lo que pasa es que cambia básicamente dependiendo del tipo de activo,

para fijar la comisión y un sitio para fijar el sleep page 

![](../img/002.png)

vale la comisión es lo que evolucionaron y al final veis te lo explica por trade por acción vale al final no importa qué pones son dólares si tú le pones por trade son cinco dólares por cada trade independientemente si le pones por acción pues cinco dólares por cada acción `total cost` esto es un porcentaje total del coste por si quieres imputar un porcentaje, otra es una parte en fija dólar y una parte en porcentaje de coste, es decir al final es bastante auto explicado si no como siempre la ayuda ,,, 

![](../img/003.png)

ya digo cuando ves ahí 0.01 es porque esto es una acción el spy por lo tanto le penalizamos 001 dólares por acción que es un tic es un tic o sea aquí casi siempre hay un tic cuando es un futuro dices bueno es que a veces veo 10 claro es que el tic son 10 de acuerdo esto depende de cuánto es el tic básicamente nosotros solemos poner per selección y aquí nuevamente ponemos un tic, tic y medio, dos tics, sería un poco ya el límite también depende del futuro si pones el ibex a lo mejor tienes que poner tres tics aquí no puede ser el ibex pero ya me entiendes, entonces depende un poco de los de los futuros, normalmente un tic un tic y medio es bastante pesimista realista y la comisión la que sea que no tiene más. este era por le he puesto porque TradeStation en modo internacional tiene cinco dólares por trade y se lo he puesto, y eso que cierra es limitado o sea que realmente este podría ponerle menos podría ponerle menos

***-Tras repasar vídeos donde comentas este ratio es muy bajito,etc quería saber si es posible sobre el TSI & Robustness, que rangos numéricos son óptimos de este fitness, umbral mínimo aceptable***

me has preguntado varias veces no sé lo iremos viendo el tsi es que depende del tipo de sistema vale es decir hay tipos de sistema que lo normal es moverse en 2.000 3.000 y hay que se mueven más lo explico es decir el tsi nuevamente se mueven miles , podemos entender la expectancia scort es que normalmente se mueve en unidades tres, pero cuando es un diario pues ya es más grande es que depende porque al final no deja de ser fórmulas que se calculan dependiendo del valor del beneficio, del beneficio por trade, del drawdown, entonces depende si hay gestión monetaria, si no la hay, vale es decir lo que importa ahí cuando hablamos de tsi estas cosas en los informes de optimización es un ejercicio de comparación, si te lías y no lo ves claro mira un poco el ***profit factor***, que sí que está podemos decir bastante normalizado y ya sabemos que se va a mover pues en dos, tres, en uno y pico, de acuerdo es decir es un poco donde se va a mover de acuerdo el profit factor es el que te puede decir un poco si vas de manera intuitiva por buen camino o no vale

***-En la configuración del chart para la construcción de vela: natural or session hours, cuál elegir, es relevante?***

esto puede aplicar y aplica a otras plataformas cuando yo tengo un gráfico entre diario no es el caso pero lo pongo ahora aquí tengo aquí se me abren unas opciones que me dice `for bar building` use `natural hours o session hours` 

![](../img/004.png)

vale esto es importante fijaros que por ejemplo en visual chart cuando teníamos el fondo nos generaba muchos problemas porque ellos van en natural hours es decir ellos no tienen la concepción de TradesStation de sesiones donde yo voy aquí y puedo controlar la sesión del símbolo dependiendo del horario y demás porque hay futuros pues que su sesión no va en un día natural en el caso del spy como este que estáis viendo pues sí va de sus 9.30 a sus 4 de la tarde pero el globex pues va de 11 de 12 a 11 puede coger dos días en muchos casos entonces para este tipo de cosas es donde aplica esto esto lo que quiere decir es si para construir la vela imagínate que tienes una vela de 7 minutos porque la puedes tener no tienes ninguna limitación en ese sentido puedes poner velas de 86 y si quieres en segundos también de 83 segundos, entonces te dice vale yo cómo la hago la vela me baso en el horario natural? que es horario natural pues de las 12 de la noche es decir yo empezaría a construir independientemente de la sesión vale de la sesión que es otro tema, él empezaría a construir las velas de imagínate 7 minutos, a las 12.00, por lo tanto la primera sería a las 12.07, la siguiente sería a las 12.14, la siguiente a las 12.21, y así vale, en cambio si yo le pongo empezar a construir por session hours, que es lo más habitual y más recomendable imagínate este caso el spy empieza a 9.30 vale, cuál será la primera vela pues la primera vela será 9.37, vale se entiende será 9.37, si yo hubiera elegido natural hours sería la que le tocara teniendo en cuenta que él hacer el cálculo aunque el activo empiece 9.30, el cálculo lo empieza a hacer a las 12, entonces a las 9.30 ahora mismo no sé cuál le tocaría pero habría que ver cuál le tocaría, porque él empezaría a construir las velas a las 12.00 es decir horario natural, vale, horario natural, esta es la diferencia

en muchos casos no te va a afectar porque si tú usas velas de 30 y realmente el activo empieza a una hora en punto, que es lo normal, no empiezan al mercado a las 9.37, empiezan a las 9.30, empiezan a las 8.00 empiezan a las 9.00, entonces da igual, pero si usaras un time frame distinto pues sí que podría afectarte, y a nosotros con el oro que lo hemos operado muchas veces en time frames extraños, ahora operamos en 28, pero lo hemos operado en otros, pues podía afectar, 

Y luego en el volumen `For volumen, use:` 

![](../img/005.png)

es un poco lo mismo si usa esto te lo explica mejor en `Ayuda` y es un poco largo porque prefiero no meterme en ese vergenal porque es un tema sobre todo de programación es un tema sobre todo de programación pero es que realmente hay un matiz dependiendo si cargas un entradí además o un diario y luego si cargas que depende qué activo y hay una tabla , pero bueno en términos generales por simplificar es lo que lo que te dice aquí si straightball simplemente es lo obvio con cuenta acciones y acciones o futuros que se cambian de manos y el tick out lo que hace es contar los trades que es lo que es un tick un tick al final es un trait es un trait independientemente del número de acciones que tenga lugar normalmente usamos straightball

**Gestión de performance report y estadísticas en Live Trading**

TradeStation tiene una función para eso que es verdad que en determinados momentos ha dado algunos problemas sobre todo cuando hay multidivisas si no no hay tanto hay aquí un botón apps que es el trade manager analysis 

![](../img/006.png)

<br>

![](../img/007.png)

que eso hace un performance report de la cuenta entonces ahí lo puedes seguir pero también te recomiendo que tú pues vayas llevando tu propio lock de acuerdo bien del programa bien con excel bien con estas herramientas y si ahora funciona bien que nosotros ya digo ahora no la usamos por lo tanto no te puedo decir y los puedes ir cogiendo del ***trade manager*** directamente la verdad que eso sí que va bien de copiar y pegar desde ahí a excel y tienes que ir un poco generándolo tú generándolo tú, nosotros solemos hacerlo así, solemos hacerlo directamente en excel, lo llevamos un poco por nuestra cuenta, pero ya digo esa realmente está ahí y sé que había dado muchos problemas me dijeron que la había arreglado pero yo no lo he probado, pero esto de esto ya te digo puedes sacar performance report de la propia cuenta, o sea de hecho es que si lo haces sale igual.

***-Sobre max bar(MaxBarsBack setting), cómo configurarlo según sistemas diferentes para trabaje correctamente y no salgan errores***

sobre el max bar settings vale esto es más de plataforma pero esto simplemente es el número de barras que necesita el sistema para calcular es decir si tú vas a optimizar si yo aquí tengo ese sistema y tiene esta media por ejemplo, bueno aquí por ejemplo el sistema que luego lo veremos, 

![](../img/008.png)


en un volatility breakout, tiene un número `13` de barras tiene un rango tiene una adx un atr bueno, pues si yo la media de 13 barras esta la voy a utilizar hasta 50 vale pues al max bar la tengo que poner al menos -50 o -55 y ya está sin más no tiene mayor historia vale lo tienes que poner aquí y ya está 

![](../img/011.png)

le pones el valor que le quieras poner o sea que vayas a ponerle más de optimizar, si te salta eso es porque tú le has pedido un cálculo más allá de ese valor 

esto en indicadores es automático y en sistemas no lo es, porque? pues no sé la verdad porque 

***En los tutoriales de TS se recomienda para evitar esto quitar el check de las dos primeras casillas que por defecto vienen marcadas como en la imagen, vostro@s lo tenéis configurado de esa forma que comentan ellos para evitar esto?***

![](../img/009.png)

esta pestaña que comentas de si operas de acuerdo normalmente lo pones para que no recalcule porque no te puede provocar un problema que recalcule, si no, lo puedes activar porque entonces recalcula pero bueno operativa pues si operas directamente con la plataforma en live pues es mejor activarlo 

***-Tras revisar la teoría se comenta que la opti genética es la que más usáis y la que es recomendable hacer generalmente si es posible, pero durante el curso se ha visto también mucha opti exhaustiva en los ejemplos, por lo que en que casos elegiremos exhaustiva sobre genética?En el pdf dice que exhaustiva sólo es adecuada cuando el número de soluciones posibles es limitado. Hay alguna condición del sistema ,umbral mínimo o consenso para saber cual hacer en cada caso? depende del número de parámetros o combinaciones posibles totales de la optimización por ej. +10000 combinaciones→genética? Otra duda es en caso de hacer la genética es que configuración se debe aplicar.***

![](../img/010.png)


En cuanto a la optimización genética, ha habido ya varios comentarios y dudas sobre su uso, y viene muy al caso aclararlo. Hay situaciones en las que **no tiene sentido utilizar la optimización genética**, simplemente porque el número total de combinaciones posibles en una búsqueda exhaustiva es menor que el número de parámetros seleccionados. En esos casos, el propio mensaje del sistema te indica que la búsqueda genética no procede, ya que no hay suficientes combinaciones que justifiquen su aplicación. Es decir, te está diciendo literalmente: “hazla exhaustiva”.

Por tanto, ese es el criterio práctico. La optimización genética es más adecuada cuando el número de parámetros es grande y existen amplias zonas de búsqueda, porque su método evolutivo permite explorar mejor el espacio de soluciones. En cambio, cuando el conjunto de parámetros es pequeño, conviene utilizar la búsqueda exhaustiva, ya que evalúa todas las combinaciones posibles sin dejar huecos.

Por ejemplo, si lo que quieres es generar un mapa de resultados o un *heatmap*, la optimización exhaustiva es preferible, porque ofrece una visión completa y continua del comportamiento del sistema. En cambio, la optimización genética es ideal cuando el espacio de búsqueda es muy amplio y el tiempo de cálculo podría ser excesivo.

En resumen, la elección depende del contexto:

* Si el número de combinaciones es reducido o el tiempo de cómputo lo permite, usa una optimización exhaustiva.
* Si el número de combinaciones es muy grande o el tiempo de cálculo es limitado, la optimización genética suele ser suficiente y eficiente.

No hay una regla rígida ni un umbral fijo; la diferencia es puramente técnica y depende del tamaño del problema y de los recursos disponibles.

***me podeis indicar en el Portfolio Maestro, en Symbol List,  donde se encuentran el grupo de ETF's de indices (SPY, QQQ, etc) y sectoriales (XLF, XLU, etc), he buscado bastante y no acabo de encontrar ninguno.***

***Configuración y gestión de listas de símbolos (Symbol List)*** cuando tú en una strategy group quieres poner un símbolo list es súper primitivo, entonces tú tienes los custom y los suyos, 

![](../img/014.png)

SP500, SP100, NASDAQ100, etcétera, caujones, tiene los típicos, entre stations símbolo list y luego ya digo en index components o en index vale, pero aún así muchas veces pues te haces tú las listas y no te queda otra que hacértela y a veces la tienes que hacer hasta para un símbolo, porque pues si tú quieres a mejor poner un símbolo solo con varios sistemas pues no te queda otra que hacer esto, hacer una lista que se llame QQQ y ponerle el QQQ dentro, 

![](../img/015.png)

las listas se comparten, si las has hecho ***Tradestations*** también están aquí, eso sí que es correcto, 

**Creación manual de listas personalizadas y ejemplos prácticos**
si te fijas ahí, `custom symbol list > create manage`, vale, le das a create y te la creas, te creas una y la añades, pero ya digo el interface es bastante más cómodo y más fácil entre station, porque desde un radar skin los puedes seleccionar, pues al botón derecho y pones add symbol list, 


![](../img/016.png)


lo puedes hacer TradeStation, vale, entonces es bastante más friendly, desde aquí también puedes ir directamente a 

![](../img/017.png)
![](../img/018.png)

ver si me acuerdo el shortcut, no, para que me acuerde, no, no eres aquí, no eres aquí, eres aquí quizá, exacto, aquí tienes edit list, vale, y aquí sale, vale, desde el menú de gráfico del chart, desde aquí de data, pero en un radar skin, lo que te digo, si tú tienes un radar skin abierto, que es la manera más sencilla en mi opinión, 

![](../img/019.png)

y los escribes aquí, 

![](../img/020.png)

lo que quieras, vale, y esto tú ahora lo seleccionas, 

![](../img/022.png)

le das ahí, te abre la ventana y ahí ya te la creas, 

![](../img/023.png)

vale, la puedes añadir a una o le creas una ***normal**, no una histórica, vale, entonces es más, y luego la encontrarás allí, entonces te pones uno, etf, o mis etf, o lo que tú quieras y te la creas, esto es la manera más sencilla

**Configuración genética sugerida y diferencias con el Walk Forward**
a manuel aboga en algún detallito más, pregunta, si se hace genética se ve configurada a mano la configuración o vale, no, dale el sugest, está bien, está bien, de verdad, no te compliques, no te compliques, de verdad, no te compliques, el único caso donde tiene algún sentido es en el world forward, en el world forward, vale, pero por lo demás, dale al sugest y es correcto, es correcto, vale, bien.


## ORB

Al final tú con un volatility breakout tal como lo hemos definido hasta ahora, que por supuesto es opcional, todo es opcional en un sistema, es decir, nosotros fijamos que a la fin de día, pero podías quedarte a más de un día, digamos, pero lo tratamos de plantear así, de esta manera, claro, tú estás operando un activo que es el que estáis viendo hoy en pantalla, que es el SPY, que es, yo diría que un poco alcista, de largo plazo, es un poco alcista de largo plazo, 

![](../img/024.png)

que lógicamente tiene sus períodos bajistas, pero en el largo plazo es un poco alcista, subido un poco, subido un poco, entonces, claro, cualquier cosa que frene, que vaya contra esa tendencia, esto puede parecer contraintuitivo, pero verás que no lo es, vale, es complicado y entonces tú dices, claro, pero a mí eso me hace pensar que es más fácil ganar en el largo que en el corto, y sí que lo es, pero ***si quitas la restricción de cerrar a fin de día***, entiendes, si quitas la restricción de cerrar a fin de día, porque acuérdate que ***el mercado sube con poca volatilidad y baja con mucha volatilidad***, tú justamente lo que estás buscando es un volatility breakout, entiendes, entonces, claro, al ***alza hay menos volatilities breakouts***, hay mucha tendencia, hay mucho seguimiento de tendencia, hay mucha subida, pero no necesariamente volatilities breakouts, 

en cambio abajo es todo lo contrario, abajo es relativamente sencilla, aunque parezca contraintuitivo, pillar roturas de volatilidad porque el mercado cuando cae, cae con volatilidad, de acuerdo, es verdad que no es fácil, ¿por qué? porque la tendencia de fondo prevalece y por lo tanto tienes que buscar, como ya has hecho tú y por eso también estamos limitados en intradía, pues objetivos cercanos, ¿de acuerdo? ¿por qué? porque al final cuando tú tienes la tendencia en contra, pues ahí es mejor ir rápido, ¿de acuerdo? salir rápido, 

en cambio en el lado largo verás que te resultará mucho más sencillo si eliminas la restricción de cerrar a fin de día, entonces verás que es muy fácil encontrar roturas de volatilidad, porque entonces ya renta, ¿de acuerdo? deja correr más. porque la mayoría de sesiones al alza, me refiero cuando el mercado entra en una tendencia alcista, 

imagínate este tramo aquí, mira esto, 

![](../img/025.png)

esto es un volatilities breakout al uso, que está tal como está, yo creo que es decente, ¿de acuerdo? que es decente y verás que se también gana en el largo y en el corto, pero gana más en el corto, gana más en el corto, ¿vale? y también buscando entradas muy rápidas y demás, pero este entra en corrección, ¿de acuerdo? busca la volatilities breakout, pero tras corregir, muy habitual en sistemas de acciones, ¿de acuerdo? de hecho, por ejemplo ***Artemisa*** hace eso, Artemisa hace eso, ¿de acuerdo? es algo, lo que no es tan rápido es saliendo como este, pero porque este busca solo el breakout y se sale, ¿de acuerdo? cambiar Artemisa, digamos que su setup de entrada es de este estilo, pero se queda, ¿vale? lo que te decía un poco ahora, ¿no? entonces ya digo, si te quedas es más fácil, entonces no es tan raro, no es tan raro como te parece por el hecho de obligarle a salir a fin de día y porque el mercado sube con poca volatilidad, aún así es posible, aún así es posible, no digo que no sea posible, pero es más complicado si no te quedas.

de hecho por ejemplo en los libros de Krabbel tenía varios apuntes donde no todo el mundo plantea los volatilities breakouts o los ORB para ir a un sub-tipo obligatoriamente cerrando a fin de día, ¿de acuerdo? eso es una, de hecho en nuestro código había un input que lo controlaba, pero sí que es verdad que yo hasta ahora lo he dejado bloqueado en uno, pero que podías plantearlo y darle dos o darle tres o darle dos o también podías trabajar otra variante porque ahora voy a hablar un poco de las distintas filtros y demás, vamos a trabajar un poco en eso y de verdad es el mundo es infinito los filtros, pero ahora se me ocurre uno que de hecho no hemos trabajado, que es que dependiendo del día de la semana que entres quedarte o no quedarte, ¿de acuerdo? quedarte o no quedarte, esto tiene mucho sentido, es muy importante el sentido común en los filtros porque si no, no se acaba nunca, tú le puedes poner 74 filtros al sistema, no hay que hacerlo, entonces hay que usar pocos pero coherentes, entonces **este es muy coherente**, este es muy coherente, ***dependiendo del día de la semana cerrar a fin de día o no***, si es lunes, martes, miércoles, jueves o viernes cerrar a fin de día o no.

**Evidencias estadísticas sobre rendimiento por día de la semana**

a ver si esto es el retorno de el overnight, el overnight en SP, ahora no recuerdo en qué año está hecho, esto es de sentiment trader, 

![](../img/026.png)

el overnight por día de la semana, veis las enormes diferencias, ya os la pasaré esa imagen, y lo mismo pero durante el daytime, la sesión normal, 

![](../img/027.png)

es decir este que veíamos ahora de 9.3 a 16 que sigue siendo bastante heavy, el viernes el más lojo, el miércoles el mejor y se pueden combinar uno de otro, mejor diurno, aquí tenemos algunas notas, estrategias nuestras analizadas, esto no deja de ser un blog de notas donde tomamos notas, pero a raíz de esto, lo que os decía, imaginaros, dependiendo si yo sé que es martes, si yo sé que estoy en el operando el martes y sé que el nocturno del martes es el mejor nocturno que hay, pues a lo mejor me cierro en la apertura de miércoles, por ejemplo, me lo acabo de inventar, pero me entendéis ahí, cosas de este tipo, vale, entonces, vamos a ver un poquito todo el tema de los filtros.

### Valoración final sobre filtros, flexibilidad y criterios personales

ya he comentado lo que quería comentarle a Zenén, era esto verdad, no tenía nada más, comentaba el del, bueno él comentaba que le había quedado un sistema bastante decente de cortos, estoy de acuerdo lo que has enseñado ahí, además él decía que había seguido un procedimiento paso a paso, variable a variable y demás, pues es interesante, es interesante, sí que veo que tienes tu take profit en dólares, ya sabéis que no soy amigo de ello, pero oye, ningún problema, quiero decir que muchos grandes autores lo usan, aunque ya os he comentado que no estoy de acuerdo, creo que se están haciendo trampas al solitario, pero oye, al final, como te decía antes en otro tema, esto hay opiniones, hay cosas que son bastante claras, hay cosas que hay debate y os los explico, al final, tenéis que encontrar, en eso insisto mucho, vuestra manera, evidentemente lo que nosotros decimos son cosas que no son tonterías, que son correctas, pero tenéis que encontrar vuestra manera, vuestra manera al final puede ser, pues debe ser distinta a la nuestra, poco a poco, vale.

**Estructura del ORB y materiales de referencia**

* [Filtros-ORB.pdf](../docs/Curso_Filtros-ORB.pdf)
* [ORB-04.pdf](../docs/CursoORB-04.pdf)


`ORB` al final tiene tres partes, 

al final el ORB tiene tres partes, la más obvia y más clara es 

 * ***su rango y sus horas***, que desata el setup de entrada, vale, 


basados en distintas fuentes, en varias de las que habéis visto de Krabber, eh, también a un otro artículo, uno también desde el que he sacado este sistema, el sistema que luego os enseñaré, que es un volatility breakout, no un ORB, que es un subtipo de volatility breakout, que os lo dimos el otro día, eh, este os lo dimos, os lo dimos el otro día, a ver que lo abra, 

aquí: ***[Designing VolatilityBreakout Systems](../docs/volatlity%20breakouts.pdf)***

designing volatility breakout system, y bueno, hace una explicación básica de lo general, que es lo que estaba diciendo ahora, es que no tiene, o sea, un volatility breakout, o decimos en catalán, son fabas juntadas, vale, porque sí que es verdad que en el, ***en el campo del filtro, en los ORBs, donde especialmente está la clave***, vale, ¿por qué?, porque al final, eh, tienes que marcar de alguna manera, porque al final tú aquí ves sesiones, sesiones, sesiones, ¿de acuerdo?, y dices bueno, lo fácil es, que es lo fácil, es lo intuitivo, es decir, pues bueno, un ranguito, yo ahí en sus roturas opero,

![](../img/028.png)

vale, bien, es una solución, en algunos activos puede funcionar, pero en la mayoría, directamente así, como que falta algo, vale, y que falta, bueno, pues falta algún filtro que, mirando lo que ha pasado en las anteriores sesiones, puede ser que el anterior, puede ser que a veces algunas más, aumente las probabilidades de que la rotura de este día sea favorable, vale, es tan fácil como eso.

![](../img/029.png)

**Tipos de filtros aplicables y primeros ejemplos**

¿En qué se basan esas, esos filtros?, bien, en la, en el que vimos de base había uno típico de tendencia, 
* ***Momentum***, es decir, si el precio está mayor que N, pues este, en esta versión que había del, del modelo de Rupert Tacho era este, vale, simplemente el cierre, el cierre es mayor que un precio de referencia, que es el cierre diario de 14 días, esto que es un Momentum, 

```sh
# Curso-VB-01 Strategy
if time >= 1030 and marketposition=0 Then
Begin
    if (close > RefPrice) then //filtro momentum positivo: solo posiciones largas
        Buy Next Bar at HHH stop;


    if (close < RefPrice) then 
        Sellshort Next Bar at LLL stop; //filtro momentum negativo: solo posiciones cortas
End;
```

vale, es decir, el precio está subiendo, viene subiendo, vale, otro que implementamos en el nuestro, ahora ya estamos en la versión 4, que es la que hoy vamos a usar, pero bueno, más o menos, el final de la estructura es la misma,  basado en un filtro de tendencia, de acuerdo, una media de cierres diarios también, que esté por encima de esa media, 

```sh
FiltroTendencia (0), # Media de cierres diarios, si es 0 no actúa
```
pues eso encajaría en los filtros de Momentum, vale, pero los que suelen ser más eficaces son los que hablan un poco de la pauta, ***de la estructura del mercado***, vale, de la estructura del mercado y la estructura muchas veces se mira mejor en el gráfico diario, que en el propio intradía, donde vas a hacer el trade.

y esto, pues hay, como os digo, muchas figuras, Krabbel habrá de varias, las más conocidas son, por las que habréis oído hablar, el Narrow Range, vale, que está, Serén, por ejemplo, que hablaba, él le había usado, ahora no recuerdo si de 4 o de 7, Krabbel hablaba bastante de esto, un Narrow Range, ahora lo vamos, lo voy a poner ya, 

![](../img/030.png)

pues empezamos por el 1, que empezamos por el 1, y allá vamos...

![](../img/031.png)


**Análisis del primer filtro: tendencia y momentum**

empezamos en uno que me lo está marcando casi todo,

![](../img/033.png)

estos son del artículo, como me decías, verdad, si, ese es como el artículo, cierre mayor que la average, cierre mayor que la average y hike mayor que hike anterior, vale, 

![](../img/037.png)

bien, este tipo de pautas que hay, hay muchísimas, aquí solo en ese artículo hay algunas, vale, hay algunas, pero creo que hay muchísimas, de acuerdo, hay muchísimas, aquí hay alguna que hemos mirado, ya os diremos cuáles, hemos visto que van mejor, pero que hay muchísimas, de acuerdo, muchísimas, como os decía antes también, los narrow range, los inside range, ya hablamos de ellos, también hay mucha gente que tiene especificaciones para los gaps, hay distintas variaciones, hablaba antes de los días, cualquier indicador de momentum, es decir, cualquier variación, vale, yo por ejemplo, siguiendo este artículo, he utilizado también hablaba, si no recuerdo mal, krabbel de ello, el ADX, aunque lo he hecho un poco distinto a como lo hacía él, pero he utilizado al final el ***movimiento direccional***, he utilizado también la ***volatilidad***, de acuerdo, al final son estos los vectores que hay, no hay más, 

lo que pasa que sí que es verdad que la manera en que marcamos estas figuras, que nos hablan un poco de la estructura de precio, son bastante definitoria, es decir, por mi experiencia lo que más define que uno de ORB acabe funcionando es este tipo de estructuras, de acuerdo, este tipo de estructuras, que al final denotan, y como os digo, estructura de precios, vale, 

bien, esto como os decía, lo hemos, lo Alberto lo ha programado en un show me, que cuando se cumple la condición lo marca, como estáis viendo en el caso de la primera, pues es un filtro bastante al uso de tendencia, el 1 es muy poco restrictivo, 

![](../img/034.png)


esto os lo recomiendo muchísimo hacerlo, este código os lo pasaremos   
* [`filtros-ORB:Showme`](../PRACTICA%2008.ELD),   

esto os recomiendo mucho porque ya en la teoría os hablaba de ello y que también os hablaba que me sorprende porque he hablado con colegas que dicen que ellos miran pocos gráficos y a mí realmente me sorprende muchísimo porque realmente es donde se ven las pautas, de acuerdo, donde se ven las pautas, entonces al final por mucho que tú te puedas hacer una idea y ya sabes lo que es un cierre por encima de una media y todo lo que queráis, de esta manera entiendes muy bien el filtro, sabes, entiendes muy bien el filtro y le coges lo que os he hablado siempre de la sensibilidad, vale, entonces es bastante útil, ves incluso a qué nivel, a qué frecuencia actúa, de acuerdo, a qué frecuencia actúa, así es, porque ya veréis que hay algunos que constantemente dan, bueno dan, nos queda en señal, constantemente cumple las condiciones para pintar, de acuerdo, sin más, esto es tan sencillo como si cumple pintas y no pues no pintas, vale, en este caso en el mismo el `case 1` pinta tanto el plot 1 como el plot 2, ¿verdad Alberto? exactamente, exactamente, 

es importante para entender un poco el, la idea, el setup y os lo recomiendo en general con todos los setups, de acuerdo, es decir, verlos, mirar los setups en la pantalla, vale, mirar los setups, porque ahí verás cuando actúa o cuando no, vale, 

bien, ***este era el uno, ***

$$
c > \text{average}(13) \\
\text{ and } \\
h > h[1] \\
\\
66.94\%
$$

$$
\text{cierre mayor que la media de 13 días y el máximo mayor que el máximo del día anterior}
$$ 

este buen hombre pues había hecho unos, unos, unos estudios aquí, por ello, vale, ¿cuál es el, cuál es el segundo? Este, como os digo, es un posible filtro, este claramente es de Momentum, vale, este claramente es de Momentum y si no recuerdo mal, poco efectivo, ¿verdad Alberto? Este es poco, poco efectivo, ¿de acuerdo? Este era poco efectivo, como yo creo que ya sospechabais porque al final es muy poco restrictivo, por sí solo, puede que a lo mejor en combinación con otro, ¿me entendéis? Es decir, es decir, a lo mejor este, este para simplemente filtro de Momentum y luego una pauta de volatilidad, pues a lo mejor ya funciona, ¿me entendéis? Pero por sí solo, pues es, es, es poca cosa, vale, es poca cosa.

***Bien, ahora le ponemos el 2***

$$
c > \text{average}(13) \\
\text{ and } \\
close > open \\
\\
63.43\%
$$

$$
\text{cierre mayor que la media de 13 días y el máximo mayor que el máximo del día anterior}
$$ 

es muy parecido, es muy parecido, lo único que en vez de comparar, o sea, además del cierre por encima de la media, esto es igual, tiene el Momentum, pero además la pauta de estos, si os fijáis es parecido al análisis candlesticks, a aquellos que habéis hecho velas, ¿no? Y el cuerpo real, ¿de acuerdo? Todo este tipo de cosas, el candlestick al final, cuerpo real es cierre menos open, open menos high, el rango es high menos low, ¿vale? Es decir, todo este tipo es muy usual trabajar este tipo de pautas, ¿vale? En este caso comparaba el high con el high, muy obvio, pero aquí lo que haces es comparar el cierre con el open, ¿vale? Aquí comparar el cierre con el open, ¿qué haces comparando el cierre con el open? Bueno, quiere decir que ha sido una vela alcista, no quiere decir que el día ha subido, fijaros, porque el día si ha subido es cierre contra cierre anterior, vale, cierre contra open es que la vela es alcista, ¿entendéis? Es decir, que durante la jornada ha subido desde que ha abierto, pero ha podido abrir con un gap, ¿entendéis? Ha podido abrir con un gap, como por ejemplo hoy el mercado ha abierto con un gap al alza, el mercado americano, pues y desde ahí puede caer y entonces el cierre no sería mayor que el open, aunque la sesión sea alcista, ¿entendéis? Entonces esto al final es una figura de velas, esto quiere decir que el cuerpo, el cuerpo de la vela es azul, ¿entendéis? No es que sepáis candlestick, ¿vale? Y entonces ya marca distinto, porque como tiene que cumplir las dos, 

![](../img/035.png)

antes todo esto era igual, bueno no porque era de 50, hay cuidado, y ahora pues aquí ya tiene algunos días donde esto no se da, vale, esto no se da también, vale, bueno, es otro otro filtro, bien, entonces este es otro nuevamente, pues podemos, como os digo, va bien para para verlo en el gráfico, vale.

***Bien, ahora le ponemos el 3***

![](../img/036.png)

$$
c > \text{average}(13) \\
\text{ and } \\
close > close[1] \\
\\
63.43\%
$$

![](../img/037.png)

¿Qué más? El tercero, ya vais a ver que algunos cambia un poco más, estos son bastante parecidos, al final tenemos los de narro y demás, que son los que más utiliza la industria, aquí viene el 3, que nuevamente es muy parecido, todos estos todos estos primeros mantienen el mismo criterio de cierre mayor que lo que cambia es un poco la segunda pauta, vale, este es lo que os decía, es parecido al anterior, porque el anterior es cierre mayor que open, y la mayoría de veces que pase esto va a pasar que el cierre sea mayor que el cierre anterior, pero no siempre, entonces bueno, es ese matiz probar uno contra otro, la cual es probar las diferencias, y luego también dependerá de qué activo, habrán activos que cambie más más que otros, vale, pero bueno, este realmente es parecido, ¿ves? Aquí por ejemplo, pues ya hay días más rojos, vamos al siguiente, porque esos son muy muy muy similares, vale.

***Bien, ahora le ponemos el 3***

![](../img/038.png)

El cuarto ya es más interesante y de esto lo veréis, es muy habitual, de acuerdo, y la primera vez que lo ves es como raro, pero ya os lo explico y veréis que no está raro, es que no está raro, eso ya cambia más, aquí ya la cosa ya no está tan clara, veis, ya hay días que sí, aquí sube, sube, sube, en cambio no lo ha marcado, vale, no se le cumplía la condición, 

![](../img/039.png)

¿qué condición es esa? ¿Qué condición es esa? 

$$
c > \text{average}(13) \\
\text{ and } \\
open > Low + 0.5* \text{range}[1]\\
\\
63.43\%
$$


`c > \text{average}(13)` esta no es el problema, la que está marcando un poco la diferencia es la segunda parte, ¿no? Se tienen que cumplir las dos, vale, es que el open sea mayor, la voy a leer mejor en el código está, que el open sea mayor que el `open > Low + una cantidad` low más una cantidad, más una cantidad, ¿de acuerdo? No es simplemente el open contra el low, es que el open sea mayor que el low más una cantidad, y ***esta cantidad es la mitad de la vela, de todo el rango de la vela***, esto es muy habitual, lo veréis muchas veces con un `0.5`, con la mitad y también a veces lo he visto yo bastante en tercios, ¿de acuerdo? Es decir, al final, esto, ya digo, proviene bastante del candlestick, esto hablaba bastante de ello, se llamaba el autor este que se hizo tan popular de nNison de velas, vale, es decir, al final tú tienes una vela y una vela al final...

![](../img/040.png)

pues casi todas estas figuras en el fondo vienen de esto, de acuerdo, de entender esto y de entender que esto, que es verdad, que por sí solos tienen poco poder predictivo anualizadas solas, como complementos de filtros son bastante útiles, porque al final esto te está hablando de la estructura del mercado y por añadido de las fuerzas del mercado, de acuerdo, el mercado es oferta y demanda y al final una vela por sí sola, sobre todo esos cinco minutos, pero evidentemente como siempre hay más ruido tal, en diario, al final te está diciendo mucho sobre la estructura y quien controla el mercado, la vela en sí, todo el rato, todo el rato hasta las más tontas y de hecho las más tontas usualmente son las que más información y claro que no hay ciencias ciertas, pero fijaros que tras impulso muchas veces hay velas con mechas, con cuerpos pequeños, lo veis, esto claramente está marcando un rango lateral, tú por ejemplo tienes esta vela que es un tirón fuerte y automáticamente luego que te viene, ves una cuerpa con un velito de nada, un cuerpito de nada, mechita, mechita, que es lo más probable de esto, es lo más probable es continuación, es así que lo más probable ya indica que no es seguro, pero es así, pues esto al final es una estructura que a mí me hace, me puede hacer decir, yo puedo utilizarlo a lo mejor en sistemas, si puedo medirlo de alguna manera, que es probable que la siguiente rotura sea fuerte, ¿por qué? porque las congestiones de precios provocan, son un anticipo de expansiones y esto es siempre esta pauta que hay, que hay muchas veces que pues la contracción dura una teronidad y te duermes, es decir, y esto pasa y aquí en toda esta estructura dices, mira pues aquí esta puede ser buena y al final no lo ves y se va abajo, claro que hay fallos.

**Relación entre estructuras de velas, breakout y tendencia**


pero ligando un poquito con lo que decía antes de Sénén, antes del lado largo, ¿qué pasa en el lado largo? tú en el lado en el lado largo, ¿por qué es más complicado? porque esto es muy aprovechable para un volatility breakout, pero ves no marca tendencia, 

![](../img/041.png)

en cambio esto para un volatility breakout no es aprovechable, 

![](../img/042.png)

esto es aprovechable para un tendencial, tenemos, eso es diario ahora, entonces es otro tipo de sistema, yo para operar aquí iré mejor en tendencial, ¿puedo aprovechar el volatility breakout del lado largo? sí que puedo, pero entiendes, es distinto, porque si puedo coger esta vela y me salgo, 

![](../img/043.png)

pero luego ya está, aquí ya es complicado entrar muchas veces, en muchos casos será complicado entrar, habrá muchos volatility breakout, de hecho el otro que tenemos preparado os lo entregaré al final, si no os lo entrego el código hoy os lo subiremos mañana, pero ya os enseñaré seguro, al final aquí no entra ya, no entra, porque su manera de identificar el breakout requiere descanso, cuando ya está tendencia no es su partida, pues todo esto que al final son figuras de velas, todas estas que os digo que vienen aquí que son el rango, una parte del rango, lo que hay detrás  `open > Low + 0.5* \text{range}[1]\\` al final es una vela, es una figura de vela.

**Interpretación práctica y cálculo de la regla de mitad del rango**

es decir, cuando yo estoy midiendo la mitad del rango, esto si os cuesta, la mejor manera es pasarlo a números, un poco cuenta la vieja, te coges unas cuantas velas y lo haces y te haces el cálculo, entonces lo entiendes, te haces el cálculo de lo que está haciendo, porque así dices la mitad del rango más el low, como que suena raro, pero vamos a intentar, vamos a intentar, a ver donde tengo ahora el código, aquí no, vamos a intentar de esto que está aquí escrito `open > Low + 0.5* \text{range}[1]`, pintarlo, a ver si lo consigo yo pintar en una vela, en una vela que haya, bueno, como lo tengo puesto, bueno, en cualquiera de estas, claro, cualquiera de estas me está dando truco, lo cual está pintada, la alcista, esto lo pinta para todo el día, como usa el daily, se lo pintamos arriba, pero en realidad usa datos de abajo, se entiende esto, usa datos de abajo, 

porque esas son pautas que se han analizadas en el diario, se han analizados en el diario, es decir, en esta sesión toda ella cumple, quiere decir que en la anterior cumple, 

![](../img/044.png)


es decir, esta al cierre es la que desata la sesión, entonces voy a ver si aquí abajo lo voy a hacer candel, stick, ah no, me lo he hecho arriba, venga, perfecto, perfecto, lo he hecho arriba, pues entro aquí y me lo hago yo, aquí no me lo pone azul, este servidor no es culé Alberto, qué pasa aquí, esto tiene que ser rojo y azul y grana, esto no se repita, bastante valestamos para al menos bajarlos del barco, hay que apoyar ahí, bueno, esta es la vela, espérate que ya me acerco, ahí llegamos, esta es la vela que aparte de ahí pinta, porque ese es el cierre, este es el close, 530 las velas salieron al cierre y ahí empieza a pintar, 

![](../img/045.png)

entonces cuál es la regla que dice Fran?  

la media es obvio que está por encima del 13, pero luego además tenemos la otra condición que es que, os la enseño aquí para que la veáis, 

***Bien, ahora le ponemos el 4***

```sh
	Case 4: 
		if Close of data2 > average(Close of data2, FiltroTendencia) and Open of data2 > Low of data2 + 0.5 * range[1] of data2 then 
			Plot1( High, !("Filtro1 Lng") )
		Else
			Noplot(1); 
		if Close of data2 < average(Close of data2, FiltroTendencia) and Open of data2 < High of data2 - 0.5 * range[1] of data2 then 
			Plot2( Low, !("Filtro1 Shrt") )
		Else
			Noplot(2); 
```

porque es que en el powerpoint está como que se ve muy rara, porque pone la ola en esa minúscula y yo creo que casi no se entiende, vale, a ver si aquí lo veis más fácil, la primera es obvia, la segunda es esta, es esta, de acuerdo, perdón, si el `data 2` es el daily, o sea, olvidaros del data 2 no aporta nada al entendimiento, simplemente que es el gráfico de abajo que está en diario, es decir, `Open of data2 > Low of data2 + 0.5 * range[1]` el open es mayor que low más la mitad del rango, el open tiene que ser mayor que low, a ver si lo pinto yo aquí, es decir, que participa aquí, participa el open, participa el low y participa el range que es esto, concretamente participa la mitad de esto, es decir, este trozo, bueno, este es igual, se entiende, ¿no?, 


![](../img/046.png)

¿cómo?, perdón, tenemos un low que es 1769, tenemos un open que es 17000, esto es un low, a ver que lo he perdido, 17103, esto es el open y tenemos un rango que es la resta entre el high y el low, de acuerdo, siempre que veáis rango es high menos low, es exactamente lo mismo, que es 17136 menos 17069, eso es el rango, vale, yo calculo eso, eso me da 67 

y eso me dice que lo multiplique por 05, que es lo mismo que dividirlo por 2, estamos de acuerdo, ¿no?, la mitad, que es 33,5, el 05 es 33,5 puntos, vale, 

y entonces a mí la regla para ser true, tiene que ser que el open, o sea, le tengo que sumar esto al low, el low era 1769 más 33,5 y me da 17102,5, el open era 17103, por medio punto es true, vale, medio punto, 


pero lo que quiero que entendáis es lo que significa, ¿de acuerdo?, fijaros que esto a mí me está diciendo, lo que en el fondo estoy comparando es la apertura, lo que me está diciendo es que la apertura está lejos del mínimo, 

![](../img/047.png)

se entiende un poco la idea, ¿por qué?, porque si el mínimo estuviera más cerca, si el mínimo estuviera más cerca, sumado a la mitad del rango al mínimo, superaría el open, entendéis un poco cómo juegan este tipo de figuras, porque veréis muchas así, que tú la lees, que tú la lees, es joder, esto que es la mitad del low más el high, no sé qué, hay muchas versiones de este tipo.

**Reflexión sobre la importancia de entender los cálculos de los filtros**

en el fondo es muy recomendable que hagáis este ejercicio de entenderlo, ¿entendéis?, cuando ya estén a consumirlo pues es igual, pero es muy muy importante en cuantitativo entender el porqué de las cosas, 

esto en la teoría os lo decía mucho a partir de los indicadores, todo oye, ya nada es el MACD, el RSI, el stock, o sea, hay que entender qué hace cada cosa y lo mismo esto, o sea, no caer en meter filtros y meter y no saber ni qué metéis, ¿entendéis?, es decir, coger una lista de 50 que podéis encontrar por ahí un montón y meterlos, ¿vale?, o ir metiendo varios, puedes probarlos pero entendiendo qué hacer, ¿vale?, entendiendo qué hacer, ¿vale?, 

bien, este es uno de ellos que pues da true en determinados casos, ¿vale?, bueno, porque esto lo que está indicando es esto, ¿vale?, lo que os digo, al final, al comparar, al comparar el open, a ver, estoy aquí arriba, aquí, al comparar, bueno, primero, estoy en tendencia alcista, 

$$
c > \text{average}(13) \\
$$

todos parten de eso, ¿vale?, estoy en tendencia alcista porque cierre mayor que una media, ¿vale?, estoy en tendencia alcista, pero además de estar en tendencia alcista, yo pido esto, 


$$
\text{ and } \\
open > Low + 0.5* \text{range}[1]\\
\\
$$


luego veréis que hay otro que es con el close, o sea, no, no, ¿por qué el open? Bueno, es porque hay varias, ¿vale?, hay varias, el open de a mentir la importancia, ahora me gusta más el close, ¿vale?, perfecto, ahora en la siguiente lo vemos, ¿vale?, este es con el open, este lo que está comparando es el open con el low, pero no directamente con el low, le suma la mitad del rango, ¿vale?, pero esto ***es una manera de decir que está lejos***, ¿entendéis?, está lejos porque si, si no estuviera lejos lo sobrepasea y de hecho no lo sobrepasa por medio punto, con relación, o de hecho de otra manera, el open con relación al cuerpo, ¿vale?, el, el, el, bueno, no el cuerpo, perdón, porque el cuerpo es esto, ¿vale?, todo el rango, el open está por encima de la mitad, 

![](../img/048.png)


**Preparación para el siguiente filtro: comparación con el cierre y cambios en la lógica**

ahora vamos a hacer lo mismo pero con el cierre, 

***Bien, ahora le ponemos el 5***

```sh
	Case 5:
		if Close of data2 > average(Close of data2, FiltroTendencia) and Close of data2 > Low of data2 + 0.5 * range[1] of data2 then 
			Plot1( High, !("Filtro1 Lng") )
```
en el 5 es muy parecido al anterior, seguimos comparando el, o sea, seguimos estando en tendencia al cista y un cierre por encima de la media, que esto podría haber hecho separado, pero bueno, hemos respetado al autor, tienes, pero, o sea, aquí hay dos filtros del uno, uno de momentum, cierre por encima de la vera y además de pauta, ¿vale?, hay dos, momentum y pauta de precio, es súper habitual, súper habitual este, este concepto de uno de momentum y otro, o sea, ¿por qué?, porque yo voy a favor de tendencia, pero además de favor de tendencia quiero algo, ¿no?, y también puede ser en contra de tendencia, cuidado, es decir, es muy habitual lo que os decía antes, que esto no acertemisa, es decir, lo contrario, es decir, el cierre es menor que el cierre anterior, o sea, yo estoy en tendencia al cista, pero el cierre es menor al anterior, es decir, estoy en una corrección, sin llegar a perder la tendencia estoy en una corrección, esto también es muy muy habitual,   
pero aquí de momento es tendencia y una figura de velas que vamos tratando de analizar cada una, ¿no?, 

en este caso lo que os decía, lo que compara es el close con el low más el rango `Close of data2 > Low of data2 + 0.5 * range[1]`, entonces nuevamente tenemos los mismos elementos, lo que pasa es que aquí ya no es el open, sino el close, 


![](../img/049.png)

pero claro, aquí otra vez me está diciendo que lo que compara es el close sumándole la mitad del rango, si le sumo la mitad del rango al close of data 2 mayor que el low más la mitad del rango exactamente, es..., yo al *low* nuevamente tengo el rango aquí, y al *low* le sumo la mitad, poco más o menos, así a ojo. 


Es ahí, ¿no? Cuidado, que estoy ya mejorando. Entonces, yo al mínimo le sumo esta mitad y me quedo aquí donde estaba antes, 

![](../img/050.png)

lógicamente, ahí me quedo en esta línea. ***¿El cierre es mayor que esta línea?***, por mucho más que antes; antes el *open* ya lo era, pero por poco, ahora es mucho más. ¿Entendéis un poco? 

Entonces, yo le estoy pidiendo ***un cierre muy alejado del mínimo***, igual que antes con el *open*. Es más estricta con el *open*, el cierre es más fácil. Podemos decirlo así: con el *open* es más estricta, pero entendéis un poco la diferencia entre una y otra, una con el *open* y la otra con el *close*. Esta vela es claramente alcista, le estoy pidiendo una vela que sea muy alcista o, para ser más ortodoxos, que cierre muy alejada del mínimo. Eso es lo que le está pidiendo esta pauta, y así ya vamos entrando en la mentalidad de entender las pautas.

**Uso de las pautas y su función como filtros**

Esto no solo se usa en el *reversal*, cuidado. Este tipo de figuras os las estoy introduciendo aquí porque se utilizan mucho a nivel de filtros, y es muy típico en este tipo de *RV* por lo que os decía: para aumentar la probabilidad de acierto. 

Aunque, cuidado, no siempre ganan más. Mira, vuelvo a referirme al sistema, porque lo has pasado y se me olvidó comentarlo; al leer tu email lo he pensado. Tú lo comentabas: creo que decías que no ganaba más, pero sí que aumentaba la probabilidad. Esto es muy habitual. En el sistema que veréis ahora siempre aparece este dilema: filtrar más o filtrar menos. ¿Hasta dónde filtro? Porque si filtro más, aumento el acierto y el *profit factor*, pero pierdo *trades*, gana menos. Ahí está el equilibrio. Y no hay una respuesta única, no hay una fórmula que diga “hay que hacer esto y esto”. Depende de la cartera, depende de qué busque cada uno. Ahí está esa parte de interpretación.

**El componente humano en la operativa cuantitativa**

Esto me lo comentaba un colega que además está apuntado al curso. Me decía: “Yo pensaba que esto del trading algorítmico era como sota, caballo y rey, muy cuantitativo y cerrado, y estoy viendo que hay cierto grado de discreción”. Y le respondí que sí, cierto grado hay, pero muy pequeño, porque todo está bastante estandarizado. Aun así, es verdad que al final hay cierto margen, y si no lo hubiera, todo sería igual: todo el mundo ganaría o todo el mundo perdería. No habría margen para que existieran operadores buenos que destaquen técnicamente, no habría diferencias entre los resultados. Si todo fuera tan rígido, no habría espacio para la excelencia. Tratamos de protocolizar y estandarizar los procesos, pero siempre queda un pequeño margen, porque al final siempre hay una persona detrás. Eliminar eso al cien por cien es difícil, aunque tratamos de minimizarlo al máximo. Si me estás viendo en directo, salúdame por el chat, que ya sabes quién eres. Me he ido del tema, pero volvamos.

**Análisis del patrón número 5**

Ya estaba mirando el 5, creo. El 5 es el que hemos visto del cierre, aunque no hemos analizado el lado negativo. Lo pongo un momento aquí simplemente para que lo veáis, pero vaya, es un poco lo mismo. Lo único es que en un mercado alcista cuesta más, porque recordad que se tienen que dar las dos condiciones: el cierre tiene que ser inferior a la media de cierres; si esa ya no se da, da igual si se cumple la de velas. Se tienen que cumplir ambas. Aquí, por ejemplo, se da, ya lo veis. 

![](../img/052.png)

Tiene que ser una vela que cierre en la parte baja del rango. Eso es: al final es la mitad del rango; al máximo le sumo la mitad del rango y me quedo más o menos ahí. Esa raya tiene que estar por debajo del cierre o por encima, y lo está: el cierre tiene que ser menor. Esta con el *open* no lo cumpliría, ¿veis? Porque el *open* está arriba. Esta con el *open* no estaría pintada roja; la número 4 no estaría pintada porque la mitad del rango cae por debajo del *open*. Por el cierre sí, lo veis: el cierre está debajo de esta línea, pero el *open* está por encima. Así entendéis cómo funcionan estas figuras.

**Interpretación de las velas y la lógica detrás del candlestick**

Como os decía, al final detrás hay un *candlestick*, no en el sentido estético, sino técnico, aunque la estética ayuda mucho a entenderlo. El *candlestick* gusta porque es visual: esto por sí solo me da mucha información. Me está diciendo que hemos cerrado muy alejados de la apertura, que ha sido una sesión de rango importante, que también hay bastante mecha en ambos lados y que tiene cierta indefinición, porque el precio ha vuelto bastante desde el mínimo. En otras velas hay indefinición absoluta: ha habido mucho rango, pero al final ha cerrado donde abrió. Las velas dan muchísima información y se usan mucho en este tipo de análisis.


***Bien, ahora le ponemos el 6***



Está mezclando ya conceptos, porque sigue introduciendo la media, pero ahora compara el cierre con el *open*, que esto ya lo habíamos hecho antes. Es la 2a con algo más. ¿Y qué es ese “algo más”? Pues vamos a verlo: 

$$
c > \text{average}(13) \text{ and } close > open \\
\text{ and } \\
\text{ range > 1\% of average(13)} \\
\\
55.93\%
$$

el *range* —recordad, *high* menos *low*— debe ser mayor del 1 % de la media de 13 días. Esto nos dice que debe ser un rango amplio, es decir, que haya sido una vela volátil. Es una manera de medir la volatilidad. ***Está incorporando la volatilidad***, porque uno de los indicadores que usamos para medirla es el *Average True Range* (ATR), que es la media de los rangos. Esto lo que hace es comparar el *range* (de una vela) con la media de varios rangos. Tiene que ser mayor que el 1 % de la media de los 13 días. 

Esto lo veis en el código: este era el 6, 

```sh
Case 6:
    if Close of data2 > average(Close of data2, FiltroTendencia) and Close of data2 > Open of data2 and range of data2 > average(Close of data2, FiltroTendencia) * 0.01 then 
        Plot1( High, !("Filtro1 Lng") )
```

Aquí lo tienes **en HTML**, con **las negritas convertidas a cursivas** correctamente:

<div style="border-left: 4px solid #f1c40f; background: #fff9e6; padding: 10px 15px; margin: 10px 0;">
    <strong>Explicación :</strong><br>
    Este fragmento de código define una <em>condición de filtro para entradas largas (long)</em> basada en tres requisitos simultáneos aplicados sobre <em>data2</em> (normalmente el timeframe diario). Primero exige <em>tendencia alcista</em>, comprobando que el cierre actual está por encima de la media de cierres de los últimos <em>FiltroTendencia</em> días. Después pide una <em>vela alcista real</em>, es decir, que el cierre sea mayor que la apertura. Y, por último, exige <em>volatilidad suficiente</em>, verificando que el <em>range</em> de la vela (high–low) supere al menos el 1% de esa misma media de cierres. Si las tres condiciones se cumplen a la vez, entonces el sistema dibuja un marcador visual (Plot1) en el <em>high</em> de la barra, señalando que esta vela cumple el filtro técnico “Filtro1 Lng”.
</div>


Aquí hay que suponer que se refiere a la media de los *closes*, claro, porque si no, no tendría sentido. Este es menos intuitivo, así que vamos a tratar de verlo en pantalla. Los dos primeros eran claros, ya los hemos visto. Aquí ya pinta poco porque le pedimos muchas condiciones; Si te refieres al *data2*, la regla solo se evalúa en esa vela, por lo tanto todas las de la serie deberían estar pintadas. 

![](../img/054.png)

Compara el *range*, nuevamente, *high* menos *low*. Nos dice que el *range* tiene que ser mayor que la media de los cierres, que es esta línea azul, la media de los cierres. Concretamente, el código indica: *data2 open range* mayor que la media por 0,01, es decir, por un 1 %. 

![](../img/055.png)

Es un poco raro, porque al final multiplica esto por 0,01. Por ejemplo, si la media de cierres es 17 022, por 0,01 da 170 puntos, y el rango debe ser mayor que 170 puntos. Nuevamente, ***nos está diciendo que es una vela de mucho rango, una vela muy volátil***. Le veo bastante sentido. Al final creo que el 6 no era de los mejores, pero es parecido al 7. Este tiene sentido porque introduce la volatilidad. 

Recordad que os hablé de las pautas: de ***momentum***, de ***volatilidad***, ***contracción–expansión***. La volatilidad casi siempre participa en este tipo de sistemas. Digo “casi” por prudencia, porque ¿qué hay más para definir el rango? Lo que os decía de estructura o no estructura: tendencia y volatilidad, los dos vectores básicos que mejor definen un mercado. 

La tendencia la está metiendo todo el rato $cierre > \text{average}(13)$ en la primera regla, y aquí mantiene la condición de cierre mayor que *open* es decir la vela podría tener ese rango enorme y ser bajista, claro. Lo que quiere es tendencia alcista: que la vela sea verde y de mucho rango. Eso identifica este patrón. Y a lo mejor podría ir mejor en el contrario, es decir, el 7. 

***Bien, ahora le ponemos el 7***

El 7 pide que el cierre sea menor, es decir, que la tendencia sea alcista, haya volatilidad, pero que el día haya sido bajista. Esa configuración le gusta para el largo, y a mí también me gusta.

$$
c > \text{average}(13) \text{ and } close < open \\
\text{ and } \\
\text{ range > 1\% of average(13)} \\
\\
47.06\%
$$

```sh
Case 7:
		if Close of data2 > average(Close of data2, FiltroTendencia) and Close  of data2 < Open of data2 and range of data2 > average(Close of data2,FiltroTendencia) * 0.01 then 
			Plot1( High, !("Filtro1 Lng") )
```

Claro, es complicado porque le está pidiendo tendencia alcista, además es de 13 días, que tampoco son muchos, y mucha volatilidad (recordad que la volatilidad es bajista). Y encima que haya cortado. Entonces, al final le está costando. 

Ves, aquí la da por los pelos. La da por los pelos. la da y la marca toda. 

![](../img/056.png)


Esta la da cómoda, porque cierra por encima, tiene mucho rango, es bajista y la cierra, 

![](../img/059.png)
Es aquí donde ya se va a invertir.

![](../img/060.png)

Es esta vela ya está por debajo de la media. La vela es verde, de acuerdo, y de mucho rango. Es decir, esta es muy, muy interesante. 

![](../img/057.png)


Bueno, pues todas estas no las vamos a repasar todas. Ya os digo que este código ya os lo haremos llegar. Son estos códigos, en realidad son reglas, son filtros. Esto con un *ShowMe* se pinta.

Además de estas, como veis —ya digo— ahí hay otras. No, aquí hay otras. O sea, hay siete, pero en el indicador que ha hecho Alberto en el *ShowMe* hay más, porque están los *Narrow Range* y todas estas que ya habíamos comentado.


***Bien, ahora le ponemos el 8 - funcion : Narrow Range de cuatro velas***

```sh
inputs: Length( numericsimple ) ;

condition1 = True;
for value1=1 to length
Begin
	condition1 = condition1 and range[value1] of Data2 > range of Data2;
	#condition1 = condition1 and ((HighSession(0, value1) - LowSession(0, value1)) > (HighSession(0, 0) - LowSession(0, 0))); //rangos diarios
end; 

Narrowrange = condition1; 
```

La 8 es un *Narrow Range*. Si os abre la función, veis que es un *Narrow Range* de N velas; en este caso creo que está definido para cuatro, pero puede ser para siete... 

```sh
# Filtros_ORB : ShowMe
	Case 8:
		if NarrowRange(4) of Data2 then  # NarrowRange(4)
			Plot1( High, !("Filtro1 Lng") )
		Else
			Noplot(1); 
		if NarrowRange(4) of Data2 then  # NarrowRange(4)
			Plot2( Low, !("Filtro1 Shrt") )
		Else
			Noplot(2); 
	Case 9: 
		if NarrowRange(7) of Data2 then # NarrowRange(7)
			Plot1( High, !("Filtro1 Lng") )
		Else
			Noplot(1); 
		if NarrowRange(7) of Data2 then # NarrowRange(7)
			Plot2( Low, !("Filtro1 Shrt") )
		Else
			Noplot(2);
```

Digamos que tú le pasas el *input* que defines. Simplemente es que durante un número de velas, que puede en este caso ser cuatro, se cumple una determinada condición: que el rango de la siguiente sea menor.

Durante las cuatro tiene que dar *true*, exactamente. Tiene que mantenerse el rango menor. Solo que en alguna no se dé, pues ya no será, o sea, ya es *false*, ya es *false*. Esto puede ser para cuatro, puede ser para siete.

El primero que, bueno, yo diría que es el primero, luego ha habido muchos autores, pero Crabel lo explicaba en esta serie de artículos que os pasamos. Es correcto, pero no lo pasamos así. Y ya digo, se usa bastante este, el 8. 

![](../img/061.png)

este es solo el *Narrow*, pero puedes añadirle además tendencia y narrow, *momentum*.

Como os decía antes, es bastante habitual incorporar el *momentum*, siempre es bastante habitual. Entonces, aquí tienes el *ORB*. El *ORB* 8 es lo que os decía: aquí simplemente se cumple el *Narrow 4*, que se cumple en esta

![](../img/062.png)

quiere decir que todas las cuatro velas anteriores— tienen un rango mayor que esta. Todas ellas. Solo que una no tuviera, ya no. La vela que desencadena el *Narrow* es esta, porque está en correlación a anteriores y tiene un rango inferior. Ese es un poco el *Narrow 4*. Si es de 7, pues pasa en 7 velas.


***Bien, ahora le ponemos el 9 - funcion : Narrow Range de 7 velas***

A veces la gente la confunde con los *Inside*. De acuerdo, es decir, el *Narrow* es una pauta de ***volatilidad***, o mejor dicho, es ***la mejor pauta que hay de contracción***. ¿Entendéis? Cuando hablaba de contracción y expansión. Y por eso la usa como ejemplo, porque un *Narrow 4* quiere decir que el mercado está comprimido, el mercado está perdiendo ***volatilidad***, aunque tenga direccionalidad. Luego yo puedo añadir ***direccionalidad*** y usarlo si quiero, pero quiere decir que 

> ---
> el mercado está perdiendo volatilidad`.   
> ¿Y qué pasa? Que se anticipa expansión.  
>
> ---

![](../img/063.png)

Fijaros que las velas siguientes son de buen rango, ¿lo veis? Es así, casualmente. Y eso es bastante habitual: cuando un mercado tiene un *Narrow 7*, lo normal es que las velas siguientes sean de expansión. No el cien por cien de las veces, pero es bastante habitual.

> ---
> El *Narrow* es una pauta de rango de volatilidad,   
no es lo mismo que un *Inside*, que también es una pauta de volatilidad, pero que quiere decir que la vela está contenida dentro de la anterior. Es como una especie de envolvente, pero al revés.
>
> ---


**Otras variantes: reglas 10, 11 y 12**

La 10, niega el *Narrow 4*, al revés: se va a cumplir mucho. 

![](../img/064.png)
![](../img/065.png)


`la 11` es cuando no es un *Narrow 7*.

![](../img/066.png)

A veces se va a cumplir mucho, todavía más, porque hay pocos *Narrow 7*; por lo tanto, cuando no es 7, es la mayoría del tiempo.

`la 12`

Y luego, a partir de la 12, ya mezcla tendencia. La 12 —esto ya os lo pasaré— porque si no, nos quedamos aquí clavados toda la clase, y ya casi estamos terminando. Pero quiero pasar un poco al código, al sistema en sí.

![](../img/068.png)

Esta ya es un poco más elaborada. A mí me gusta más. Es decir, es *Narrow 4*, pero con cierre mayor que la media. Tendencia también. Es decir, tienes que tener una ruta. Entonces, ahí yo voy para el largo, voy para el largo. Puede funcionar o no, pero en principio es buena.

---

<br>

**Las buenas, las mejores**  

Sé que las buenas fueron la 4 y la 7. Las buenas, las mejores, en el DAX, por ejemplo, fueron la 4 y la 7. Pero esto puede cambiar mucho según el activo.

Además, a estas mismas ya os las voy a pasar todas. Jugad con ellas, combinadlas, analizadlas, hacedlas vuestras. Al final, con todo esto que os hemos pasado, prácticamente todas las demás —no están todas, pero son derivadas de ellas— ya es el concepto.

[Filtros_ORB : ShowMe](../PRACTICA%2008.ELD)

No están todas porque todas es imposible; no se acaba nunca. Pero ahí están. En total, en estos *ShowMe*, hay 19 pautas.

Son las que luego podéis usar también para los sistemas. Lo mismo: igual que aquí es un *case* para pintar, luego la puedes usar para la estrategia. Está hasta la 18, que es “*Go Buy/Soldado*”, y la 19 vuelve a meter la tendencia.

Luego pide otra vez el rango: solo el rango, quitando figura de velas. Es muy simple, es esta, la de aquí, pero quitando cierre mayor que *open* o cierre menor que *open*. Vale, es solo tendencia alcista: cierre mayor que la media de 13 cierres y *range* mayor que el 1 % de la media de los cierres. Estas dos, la 19.

Y ese es el código 4, el que tenemos. Es el *Huerta*.


## Presentación del código 4 y estructura general del sistema

Este es el *paper* del código número 4, 

- [*paper* del código número 4](../docs/CursoORB-04.pdf)  
- [STRATEGY_ORB_V4.ELD](../code/STRATEGY_ORB_V4.ELD)

<div style="border-left: 4px solid #f1c40f; background: #fff9e6; padding: 10px 15px; margin: 10px 0;">
<strong>Explicación:</strong>  
<br>
<br>
Esta estrategia es un **Open Range Breakout (ORB)** en un gráfico de *10 minutos*, reforzado con filtros procedentes del *timeframe diario (Data2)*. Combina tres pilares:

1. un rango inicial calculado a una hora concreta,
2. filtros de tendencia/volatilidad provenientes del diario,
3. un sistema de gestión monetaria y salidas configurable.

---

**1. Inputs: el panel de control completo**  
Los inputs permiten ajustar el comportamiento de la estrategia sin tocar el código:

* **HoraInicio / HoraFin** → delimitan la ventana en la que puede abrir y cerrar posiciones.
* **PrecioAlto / PrecioBajo** → campos para decidir qué precios usar en la construcción del rango.
* **Prc_ATR_Open** y **Prc_Open** → amplían el rango inicial usando ATR o un porcentaje fijo.
* **ATR_Per** → periodo del ATR usado para trailing y stops si se activa.
* **eleccionfiltro y FiltroTendencia** → activan filtros basados en Data2 (diario): tendencia, NR4/NR7, rango mínimo, etc.
* **TradesDia** → limita el número de operaciones por día.
* **UsoATR, Prc_Trail, Prc_Stop, Prc_Profit** → definen si los stops/take profit se calculan con ATR o porcentaje puro.
* **Money management** (Start_Equity, MMVar_Start, MMVar_Profits, Min/Max_Size, RoundTo) → determina cuántos contratos abrir según capital inicial y beneficios acumulados.
* **minutosOptimizacion** → desplaza la hora de inicio para optimizaciones de la ventana ORB.

---

**2. Construcción del rango inicial**  
En la hora exacta definida por *HoraInicioTrading*, la estrategia calcula un **RangoAlto** y un **RangoBajo** a partir de las últimas barras desde la apertura:

* Si se usa **Prc_ATR_Open**, el rango se expande con ATR.
* Si se usa **Prc_Open**, se expande con un porcentaje fijo.

Este rango será el nivel para lanzar las órdenes stop de ruptura.

---

**3. Filtros del timeframe diario (Data2)**  
Según *eleccionfiltro*, la estrategia puede activar diferentes condiciones que definen si solo se permiten largos, cortos o ambos. Estos filtros combinan:

* Tendencia diaria (cierre vs. media de cierres).
* Comportamiento de la vela diaria (cierre vs. apertura).
* Volatilidad mínima (range diario > 1% de la media).
* Narrow Range (NR4, NR7).

Si el filtro da señal alcista → se activa **Condition4**.
Si es bajista → **Condition5**.

---

**4. Entrada ORB**  
Dentro de la ventana de tiempo permitida y si no se han superado los trades diarios:

* Si **Condition4** (filtro largo) es verdadero →
  **Buy stop** en *RangoAlto*.

* Si **Condition5** (filtro corto) es verdadero →
  **SellShort stop** en *RangoBajo*.

Las órdenes no son a mercado, sino **orden stop de ruptura**.

---

**5. Gestión monetaria dinámica**  
El número de contratos se calcula así:

* Se suma:
  capital inicial + beneficio acumulado.
* Se aplican multiplicadores configurables (*MMVar_Start*, *MMVar_Profits*).
* Se divide por el valor nominal del activo (*Close × BigPointValue*).
* Se redondea y se limita a mínimos y máximos.

Resultado: la exposición crece o se reduce según rendimiento, simulando un position sizing proporcional.

---

**6. Gestión de salidas**

* **Trailing stop**  
Puede funcionar de dos formas:  
    **Con ATR** → trailing dinámico según la volatilidad.  
    **Sin ATR** → trailing en porcentaje puro sobre High/Low.  

* **Stop Loss y Profit Target**  
Igual que el trailing, pueden basarse en:  
    ATR × porcentaje,    
    Precio de entrada × porcentaje  

* **Cierre forzado por tiempo**  
Al llegar a *HoraFin*, se cierra cualquier posición al mercado.

* **SetExitOnClose**  
Añade un cierre extra al final de la sesión si algo quedase abierto.


</div>


que al final, lógicamente, es parecido a los otros que habíais visto. Es simplemente una revisión de los conceptos y una organización más limpia de todos los filtros. Los filtros los hacemos con *switch* y *cases*. Luego hay salidas posibles por *trailing stop*, por *ATR* (o sin *ATR*), por *profit target*; es decir, todo lo que habíais visto hasta ahora, pero desarrollado de forma más completa. También incluye la gestión monetaria general (*money management*).

Aquí está explicado solo el código, para que podáis adaptarlo fácilmente si es necesario a otro lenguaje. Y, por supuesto, también está el código en *EasyLanguage*, donde está todo implementado. Ahora os lo enseño dentro de *EasyLanguage*, que se lee mejor que aquí, pero os lo paso igualmente.

Este es el mismo concepto. Recordad que hay que usar dos *data series*. En el gráfico hemos incorporado también una variable que os mencioné antes: *UseATR*. Esta variable sirve para definir si queremos los *stops* basados en múltiplos de *ATR* o simplemente en un porcentaje. Es decir, tenemos tres variables: el multiplicador, el precio y el *ATR*. Dependiendo de si *UseATR* está en *false* o en *true*, el cálculo cambia, tal como se explica en el código.


**Continuación del análisis del código**

Bueno, entonces seguimos hablando del código que estábamos comentando. Acaba de cargar. Ya está cargado.

Ok, ok. Lo que os decía: estamos comentando las distintas *inputs*, que ya habíais visto ahí. La elección del filtro, que no es más que los filtros que habéis visto.

No sé si en esta versión están todos, porque hay varios comentados, ya que los hemos ido probando. Por un tema de limpieza, dejamos varios comentados y solo se han quedado los que eran el 4 y el 7: condición 4, condición 5, que era el 7.

Hemos ido haciendo distintas pruebas, y al final nos hemos quedado con estas. Pero, insisto…


**Experimentación y variaciones del canal**

Al final podéis —y debéis— experimentar. En la parte del canal hay una variación importante: si queréis usar el *ATR* o no, también está comentado arriba en el código. Podéis añadirle algo más al canal y experimentar con las horas, que es lo que iba a explicar ahora. Para hacerlo, necesito abrir el archivo dedicado al rango temporal. No lo tengo abierto aquí, así que lo abriré. Es igual, ya lo hemos visto muchas veces.

**Optimización de horas de inicio y final**

- [STRATEGY_ORB_V4.ELD](../code/STRATEGY_ORB_V4.ELD)

El código v4, como os decía, plantea sobre todo una mejora: es optimizable. Se puede marcar una hora de inicio y una de final, que aparecen como *inputs*, igual que en versiones anteriores. En el código, esto se utiliza para que puedas optimizar esas dos variables.

La variable que se optimiza es *MinutosOptimización*. Esta es la variable clave. ¿Cómo funciona? Si cargas el gráfico, por ejemplo, en velas de 10 minutos, al optimizar, esa variable se va incrementando: si partes de las 9:00, se probarán valores como 9:10, 9:20, 9:30, 9:40, 9:50, 10:00, y así sucesivamente. Es decir, el sistema irá desplazando el inicio según el incremento definido, de 10 a 240 minutos.

![](../img/069.png)

Si no hay ningún *bug*, funcionará correctamente. Esto se hace así porque, al optimizar, el sistema debe manipular las horas, y como el tiempo no es una variable numérica al uso, hay que convertirlo. Hay funciones en *EasyLanguage* que permiten hacerlo (*MinutesToTime*, *TimeToMinutes*). Son algo avanzadas, pero están explicadas en la documentación. Quien tenga el manual, puede consultarlas y ver cómo están implementadas.

![](../img/070.png)

**Manipulación del tiempo y cálculo del rango**

Este código está preparado para manipular el tiempo correctamente y poder modificar las horas de inicio o final según se desee. En caso de no querer hacerlo, simplemente se deja el valor por defecto. El rango no hace falta configurarlo manualmente porque se calcula de forma automática.

Por ejemplo, si cargas datos desde las 8:00 y el mercado abre a las 9:30, y usas velas de 30 minutos, el sistema calcula tres barras antes de apertura. Ya sabe cuándo abre el mercado y lo calcula correctamente. Si quisieras usar el día anterior, este código no está preparado para ello, pero podría adaptarse fácilmente.

El código está pensado para calcular el rango desde que el mercado abre hasta la hora que se indique, y esa hora es optimizable a través de la variable *MinutosOptimización*.

**Optimización práctica y resultados en DAX**

Hemos hecho algunas pruebas. Por ejemplo, Alberto ha estado trabajando con una optimización de minutos de tiempo, cambiando la barra. Si exportas los datos a Excel, verás que los valores aparecen en minutos (por ejemplo, 90 significa una hora y media). Al valor de inicio le añade esos minutos para probar distintos escenarios.

![](../img/071.png) 

En el DAX, el sistema estaba funcionando bastante bien. 
Sin hacer grandes ajustes, los resultados eran decentes. Al ordenar los resultados en Excel y buscar equilibrio entre beneficios y consistencia, se encuentra un punto óptimo.

![](../img/074.png) 
![](../img/075.png) 
![](../img/077.png) 

![](../img/078.png) 
![](../img/079.png) 

El *setup* no es perfecto, pero es sólido. En muchos casos, el lado corto ofrece mejores resultados que el largo, lo cual no es raro en un ORB, porque al cerrar siempre a fin de día, no lo olvidéis, ***el largo paga esa restricción más que el corto***. Por eso, un camino interesante en el lado largo es dejar correr las posiciones, no cerrarlas siempre al final de la sesión.

**Posibles ajustes y comportamiento por tipo de activo**

Se podrían diseñar variaciones: por ejemplo, dejar correr las posiciones largas cuando hay una pauta muy fiable (como un *Narrow 7*), o dependiendo del día de la semana o de la hora... en estos puntos el largo va a salir ganando de quedarse varios días y el corto no necesariamente.

El sistema tiene unas 1.000 operaciones, con comisiones incluidas. 

![](../img/080.png) 

El *setup* está configurado con tres barras de rango en velas de 10 minutos, usando *high* y *low*, lo que produce un rango bastante estrecho. Esto puede mejorarse. 

![](../img/081.png)

Probad también el *Typical Price* o modelos como el de **Rupert Tacho**, que usaba el mayor valor entre el *open* y el *close*.

```sh
Begin
   HHH = Highest(Maxlist(open, close), 8); //en chicago CME abre a las 0830, en NY a las 0930
   LLL = Lowest(Minlist(open, close), 8);
   # HHH = Highest(TypicalPrice, 8); //en chicago CME abre a las 0830, en NY a las 0930
   # LLL = Lowest(TypicalPrice, 8);
end;
```

El *high* y *low* es la opción más intuitiva, pero a veces hace que el sistema entre tarde en activos muy volátiles, porque ya parte de una expansión. Si buscas aprovechar la contracción antes de la expansión, anclarte al *high* puede ser contraproducente.

También se podrían diseñar *canales basados en medias de los máximos*, no necesariamente en los extremos de las velas. Esto cambiaría el enfoque: ya no sería un *reversal breakout* clásico, pero sí una versión más flexible.

En este caso, el filtro que aportaba más valor era el 4, era este, el que usaba el *open*. Era más restrictivo que el del *close*. También el 7 dio buenos resultados. Las combinaciones entre *take profit* y *stop loss* cambian mucho el comportamiento, pero estas versiones con estos filtros ya hay bastante aprovechables, especialmente en el DAX y en el S&P.

**En el petróleo**,  
sin embargo, la cosa se complica más. Como los futuros de energía tienen sesión *Globex*, hay que estudiar las horas activas. 

Primero deberíamos trabajar antes estas tres partes que dije:

1. setup clásico de entrada:

    ```sh
    inputs:
        HoraInicio (0930),
        HoraFin (1530),
        //BarrasRango (6), // Anulamos este input al 
        PrecioAlto (High),
        PrecioBajo (Low),
        Prc_ATR_Open (0.0),
        Prc_Open (0.0), //en tanto por ciento, 0 no actúa
        ATR_Per (14),
    ```

    Esta parte consiste en definir la hora, el rango, si utilizo el High o el Low, si aplico algún filtro, o si establezco un pequeño rango adicional para la entrada. Eso constituye la entrada.
    En activos como el petróleo, donde las horas son muy relevantes, hay muchísimo trabajo por hacer, porque todos los futuros que operan en Globex no deben analizarse únicamente en su sesión oficial. Hay que identificar cuál es la **sesión operativa efectiva**.

    Por tanto, el primer trabajo consiste en localizar esa sesión real. Con este código podéis hacerlo: podéis investigar, cargar históricos empezando en una hora u otra, y a partir de ahí intentar encontrar una optimización genérica, utilizando algún filtro que ya hayáis comprobado mínimamente y sin modificar nada más, es decir, algo sencillo.
    
    A partir de ahí, vais comparando diferentes configuraciones para ver qué rango presenta más contracciones o expansiones, observad esos gráficos. También os puede resultar muy útil el código del *Show Me* porque es muy visual y permite ver si realmente existen sesiones diferenciadas o patrones de comportamiento ligados a determinadas horas. Recordad, por ejemplo, que las noticias tienen mucho peso en esto. ¿A qué hora se publican los inventarios de petróleo? Pues en ese momento aparece volatilidad. Quizá un rango previo a esa hora puede construirse.
    
    Lo mismo ocurre en acciones: en lugar de aplicar un *Open Range Breakout* al SPY —como había hecho Senet, o como hacemos nosotros en el futuro— utilizando únicamente el horario regular, quizá podéis trabajar con todo el continuo (el Globex completo), pero definiendo un rango entre la 1 y las 2, porque sabéis que a las 2:30 salen noticias. Sabéis que hay volatilidad, y buscáis exactamente esa expansión. 
    
    Es decir: podéis buscar vuestras propias sesiones. No es obligatorio utilizar la sesión oficial que marca el mercado. Esto aplica sobre todo a los mercados que funcionan casi 23 horas, como petróleo, NASDAQ, oro. Dentro de una sesión oficial, en realidad existen muchas micro-sesiones, y no tenéis por qué ceñiros siempre a la misma.

2. Trabajo los filtros, stops, tp´s etc

    ```sh
        eleccionfiltro(0),
        FiltroTendencia (0),//Media de cierres diarios, si es 0 no actúa
        TradesDia (1),
        
        UsoATR (false),//False: stops/profits % sobre precio. True: porcentaje ATR
        Prc_Trail (0),//en tanto por 100, 0 no actúa
        Prc_Stop (0),
        Prc_Profit (0),//stop y profit no trailing en tanto por 100, 0 no actúa
    ```


### [Strategy : VB-01](../code/STRATEGY_VB_01.ELD)

Este codigo está basado en este mismo artículo

- [Volatility BreakOut](../docs/volatlity%20breakouts.pdf)

Para este son reglas muy sencillas que son estas de aquí:

<img src="../img/082.png" width="500">

<br>

Un *breakout* al uso. Esto está mal expresado en lenguaje porque es antiguo, pero es algo similar. Y, al final, he explicado cierre mayor que cierre anterior, cierre menor que cierre anterior y cierre mayor que la media. Y la regla de compra es una regla que lleva implícita una expansión.
Esto está mal expresado en el lenguaje porque es antiguo, pero es algo similar. Y, al final, he explicado cierre mayor que cierre anterior, cierre menor que cierre anterior y cierre mayor que la media. Y la regla de compra es una regla que lleva implícita una expansión.

```sh
Input:
	Barras (13),
	Rango (0.5),
	Nivel_ADX (16),
	Nivel_ATR (1.15),
	
	Prc_Stop (0.0),
	
	//Gestión Monetaria
	Start_Equity (100000),
	MMVar_Start (100),
	MMVar_Profits (100),
	Min_Size (1),
	Max_Size (100000),
	RoundTo (1);
var:
	FiltroDMIPos (false),
	FiltroDMINeg (false),
	FiltroATR (false),
	Profits (0),
	Contratos (0),
	ATR (0);

{ Money Management }
Profits = NetProfit + OpenPositionProfit;

If AbsValue(Close * BigPointValue) > 0 Then
	Value1 = AbsValue(Close * BigPointValue)
Else
	Value1 = 0.01;
	
Contratos = ((Start_Equity * MMVar_Start * 0.01) + (Profits * MMVar_Profits * 0.01)) / Value1;
Contratos = IntPortion(Contratos / RoundTo) * RoundTo;
Contratos = MaxList(Contratos, Min_Size);
Contratos = MinList(Contratos, Max_Size);

If Nivel_ADX > 0 Then
Begin
	FiltroDMIPos = DmiPlus(Barras) > Nivel_ADX;
	FiltroDMINeg = DmiMinus(Barras) > Nivel_ADX;
end Else
Begin
	FiltroDMIPos = True;
	FiltroDMINeg = True;
End;

If Nivel_ATR > 0 Then
	//FiltroATR = AvgTrueRange(Barras) / Average(Close, Barras) > (Nivel_ATR/100);
	FiltroATR = AvgNormalizedTrueRange(Barras) > (Nivel_ATR)
Else
	FiltroATR = True;
	
If Close < Close[1] and 
	Close > Average(Close, Barras) and
	FiltroATR and
	FiltroDMIPos and
	(Close + Rango*Range) < High and //si el precio es menor que el high abrimos y cerramos al mismo precio
	Open of next bar < High Then //si el precio abre por encima del high abrimos y cerramos al mismo precio
		
		Buy Contratos contracts next bar at Close + Rango*Range Stop;
	
Sell next bar at High limit;

If Close > Close[1] and
	Close < Average(Close, Barras) and
	FiltroATR and
	FiltroDMINeg and
	(Close - Rango*Range) > Low and //si el precio es mayor que el low abrimos y cerramos al mismo precio
	Open of next bar > Low Then //si el precio abre por encima del low abrimos y cerramos al mismo precio
		
		SellShort Contratos contracts next bar at Close - Rango*Range Stop;

BuyToCover next bar at Low limit;

Setstopshare;
ATR = AvgTrueRange(Barras);

if Prc_Stop > 0 then
	SetStopLoss(ATR * Prc_Stop * Bigpointvalue);
```

Este lo he trabajado un poco hoy y le he añadido algunas cosas. Os lo he incluido en EasyLanguage, que ahora voy a subir a Discord si me deja. Este código os lo he incluido.
No he optimizado nada más que los filtros. He dejado por defecto el *setup* de entrada, pero sí he estado trabajando en los filtros.

Él recomendaba usar —bueno, “recomendaba”— decía que había muchos más, pero había probado un par. Uno era el ADX y el ATR. Ambos los he modificado un poco porque el ADX en sí —esto no sé si lo sabéis, pero si no os lo explico ahora mismo— es un indicador compuesto que yo tengo aquí. El ADX es la línea marrón que veis abajo. En la mayoría de plataformas también lo podéis insertar de forma independiente.

Voy a insertarlo y veréis que el ADX… aquí está. Hay varios. Hay que revisar también el ADXR.
Esto procede del *Directional Move*.
Os recomiendo —como ya he dicho por activa, por pasiva y por perifrástica— que reviséis los códigos y, como mínimo, si no conocemos el código interno, que leamos qué es el ADX.
Si no es aquí, buscad por internet.

El ADX es un indicador compuesto. Al final tiene dos líneas que forman su base conceptual. Su creador, Wilder —inventor de muchísimos indicadores— desarrolló lo que llamó el movimiento direccional. El ADX era solo una parte, pero, por algún motivo, se ha popularizado muchísimo, quizá por su simplicidad (solo una línea).
Pero en realidad era parte de todo un conjunto que se llamaba *Directional Move*. De ahí viene esta DM.

Ahora veis aquí el ADX, y como podéis observar, tiene el mismo valor que la línea naranja de abajo. No sé si se aprecia bien, pero creedme: tiene el mismo valor.

![](../img/083.png)

El ADX es la suma de las dos líneas inferiores, azul y roja, que son los componentes del DM positivo y del DM negativo.
El DM positivo identifica la tendencia alcista, el DM negativo la bajista. Y la suma de ambos da el ADX. Por eso se considera que el ADX no muestra dirección. Es cierto, pero las líneas sí la muestran.

Yo, al final, prefiero usar las líneas; me parecen una señal más limpia.
Pero eso no significa que usar el ADX esté mal: simplemente son componentes distintos.

Entonces yo lo he planteado así:
– para largos, el filtro es el DM+,
– para cortos, el filtro es el DM–,
y ambos por encima de un nivel, igual que se haría con el ADX.
Es la misma idea, pero con un único parámetro, el mismo para ambos, usando la línea azul y la roja.


**Adaptación del filtro de volatilidad normalizado**

En cuanto al filtro de volatilidad (*ATR*), he mantenido el original del autor, aunque lo he adaptado al sistema de normalización que solemos emplear. La idea es sencilla: que el rango sea mayor que la media de los cierres multiplicada por un 1 %, lo que equivale a exigir una cierta amplitud o volatilidad mínima antes de permitir una operación. En la práctica, esto garantiza que el mercado se esté moviendo lo suficiente como para justificar una entrada.

El filtro de volatilidad está normalizado dividiendo el *ATR* por el *Typical Price*. De esa manera, los valores del *ATR* se convierten en proporciones relativas y no en magnitudes absolutas de precio. Esto permite comparar la volatilidad de distintos activos o de diferentes periodos temporales sin sesgos.

Por ejemplo, si el *ATR* es 141 y el *Typical Price* del activo es 14 100, el resultado sería un 1 %, lo que nos indica que el rango medio equivale al 1 % del precio. El sistema puede usar ese valor para filtrar operaciones cuando la volatilidad es demasiado baja. 

Si los valores del filtro se ponen en cero, el filtro no se aplica, dejando el sistema “en bruto”, sin restricción alguna.

![](../img/084.png)

![](../img/085.png))

Utiliza un apauta de entrada muy similar a la que hemos visto antes: 

utiliza cierre más el rango por 0.5. Le suma al cierre el rango por 0.5 `Close - Rango*Range Stop;`. Es decir, busca una expansión del día anterior y cerrar en el *high* del día anterior `Sell next bar at High limit;`. Fijaos lo restrictivo que es. Es un *volatility breakout* al uso. Es decir, entro y salgo rápido. Entro cuando hay una expansión y me voy rapidísimo. Me voy rapidísimo. Casi siempre hace *take profit*. Tiene un porcentaje de aciertos muy elevado.

```sh
If Close < Close[1] and 
	Close > Average(Close, Barras) and
	FiltroATR and
	FiltroDMIPos and
	(Close + Rango*Range) < High and 
    #si el precio es menor que el high abrimos y cerramos al mismo precio
	Open of next bar < High Then 
    #si el precio abre por encima del high abrimos y cerramos al mismo precio
		
		Buy Contratos contracts next bar at Close + Rango*Range Stop;
	
Sell next bar at High limit;
```

Aquí yo he incorporado dos reglas que él no había incorporado, que son para evitar señales absurdas.

```sh
	(Close - Rango*Range) > Low and 
    #si el precio es mayor que el low abrimos y cerramos al mismo precio
	Open of next bar > Low Then 
    #si el precio abre por encima del low abrimos y cerramos al mismo precio
```
Esto se detecta observando el gráfico. ¿Por qué? Porque muchas veces el rango, la apertura ya supera al precio al que vas a vender. Entonces no tiene sentido comprar, porque compra y cierra directamente. Seguro que habéis visto algún sistema al que le ocurre esto: compra, abre y cierra al mismo precio. Eso es un error de programación. No podemos permitirlo. Es absurdo hacer eso. Pues no hagamos absurdidades.

Entonces, lo que he hecho es añadir una regla que evite este problema. Es decir, le exijo que para poder comprar, el rango sea menor que el *high* y le digo que la apertura del día siguiente sea menor que el *high* también. Porque si ya va a abrir por encima del objetivo, no compres. Para abrir y cerrar al mismo precio, no compres. Así que, simplemente, son dos reglas añadidas para evitar *trades* —digamos— absurdos, porque implican abrir y cerrar al mismo precio. De hecho, pasaba: abrir y cerrar al mismo precio. Para eso no abro, ¿no? Estamos de acuerdo, ¿no?

Entonces realmente esas reglas son esas. La regla en sí, el *setup* en sí, es esto: 

```sh
If Close < Close[1] and 
	Close > Average(Close, Barras)
```
cierre menor que cierre anterior y cierre mayor que la media de 13, lo que habíais visto antes. Y la expansión está delimitada en un *stop*. `Close + Rango*Range Stop`

```sh
Buy Contratos contracts next bar at Close + Rango*Range Stop;
```
Es decir, yo lanzo la orden *next bar at close más rango por range*, que es 0.5. Y no he optimizado nada de esto; lo he dejado todo por defecto.

Esto, ya sin filtros, en el SPY en diario, en todo el histórico, con comisiones realistas, da esto. Da esto. 

![](../img/086.png)

Un 1.43 de *profit factor*, 41 en el largo, 44 en el corto. Bastante bien. Bastante bien.
Creo que tenía las comisiones realistas; 

![](../img/087.png)

ahora no tiene el LEAP, pero se lo había puesto. De hecho, creo que hasta ha ido mejor con el LEAP. A ver.

*Backtest* delimitado negativo, porque cierran limitada, es decir, que sobrepase el precio. 

![](../img/088.png)

Yo siempre *backteste*o así. O es muy pesimista. Bueno, pues no uses limitadas. Ya está. Si no quieres ser pesimista, no usas limitadas. Vas a mercado y ya está. Si quieres usar limitadas, pues vas pesimista. Y ya está. Cuenta con que será mejor que eso y ya está. Oye, al final será mejor que eso. Es decir, el sistema irá mejor. Porque ejecutarás algún día mejor. Y seguro, seguro, que todos los *trades* los haces. Harás algún *trade* que será bueno y que él no contempla, seguro, porque no habrá hecho el TP. Y tú sí lo harás.

Le añado el LEAP `Use Look Inside Bar testing`

![](../img/089.png)

Esto va a tardar un poco. 

Ya digo que no tenéis el *paper* de esto. Pero sí tenéis el código. Y el código está bastante comentado. Pero el *paper* intentaré hacéroslo mañana. Intentaré hacéroslo mañana.

Y ya está. Las salidas, ya digo, simplemente tienen el *stop* y, si queréis usarlo o no, con ATR. Que, en realidad, no aporta nada porque casi nunca… o sea, casi siempre sale.
Ahora ya tiene el *Looking Server Backtesting* para estar seguros de que no hemos caído en ningún error. Porque en un minuto diario es 100% que va bien.

Y todavía ha mejorado. Es lo que me suena: 1.61. 1.57 y 1.64 con comisiones.

![](../img/090.png)

Esto está bastante bien. Esto está bastante bien. Opera poco. Opera poco.
Pero aquí, ya digo, hay una optimización implícita, que es el periodo que él ha usado: 13. 13 y 0.5. Pero ya lo habéis visto, lo digo y lo sabéis, que es muy común. Multiplica por 0.5 y usa 13.

![](../img/091.png)

A mí también me gusta el 13. ¿Por qué? Porque no soy supersticioso. Es un número de Fibonacci y, para usar el 14, uso el 13. Pero da igual, de verdad. Si usáis 21, el sistema también funciona. ¿Entendéis? No cambia nada. Es un valor muy poco restrictivo.

Y si activáis los filtros en el orden de 1 o 1.25, que es lo que salía —este creo que era 1.25 o por ahí— y el del ADX, no sé si era 15, 16, por ahí. Por ahí era. 

![](../img/092.png)

Ya veréis que aportan. Los dos los he mirado por separado. Los dos aportan.
Aporta más la volatilidad que el ADX. Si quisiera quedarme con uno, me quedaría con la volatilidad y prescindiría del ADX. ¿Vale?

Entonces, ya os digo que está bastante bien. ¿Vale?

Claro, aquí filtras, y es lo que os decía. “Hostia, es que he operado muy poco!!”. Ya, ya, claro, pero es que esto no es una expansión de volatilidad. 

![](../img/093.png)

Es lo que os decía: hay que entender que, si esto te sabe mal porque te quedas fuera, lo entiendo, lo comparto, pero entonces tienes que operar un tendencial, no un *volatility breakout*. Entonces es otra cosa. Al final, un *volatility breakout* es eso, un sistema de oportunidades. ¿Que aquí son demasiado pocas? 

![](../img/094.png)

Bueno, perfecto. Si le quitas el filtro, opera más, ¿vale? Pero cuando hay mucha tendencia, le cuesta más. Cuando hay mucha tendencia, le cuesta más. ¿Vale?

Para acabar de ver esto, a ver, ahora voy con las preguntas. Aquí, ¿cómo mejoraba esto? Bueno, estos 12, 205, 217… tenemos 369 *trades*. Son muy pocos *trades*. 74% de acierto, está muy filtrado. Pero ya habéis visto que sin filtros iba hacia el pico. La curva está bastante correcta. 


![](../img/095.png)

Sin *stop*, por cierto. Sin *stop*. 

![](../img/096.png)
![](../img/097.png)

Pero es que sale, ya veis que no engancha. Pero puedes ponerle *stop*, está preparado para el *stop*, está preparado para ponérselo. Degrada, ya os lo digo, pero eso es casi siempre así. Y ya está, ¿vale?

Y eso que os digo: si queréis —creo que el del ADX… a ver, ¿cuál es el más restrictivo?— lo voy a dejar aquí para que se vea. Tenemos 369 *trades*. Creo que si el del ADX lo ponemos en cero… si lo ponéis en cero, no actúa, ¿vale? 

![](../img/100.png)

Solemos poner las curvas así porque es muy incómodo. Antes 369 *trades*. Ahora 370 *trades*. 

![](../img/103.png)

El que filtra más es el de la volatilidad entonces, ¿no? El que filtra más es el de la volatilidad.

Este de ATR… bueno, claro, también depende del valor que le pongas aquí… 

![](../img/104.png)


ahora verás cómo tiene más de 3,70 al quitarle el filtro de volatilidad. Seguramente también empeorará. ¿eh? El de la volatilidad es muy bueno.

![](../img/105.png)

Cada vez que quitamos el filtro de volatilidad, nos hemos ido a 677. O sea, el que filtra más es el filtro de volatilidad. También depende del valor que le des, claro. Porque el ADX, si ahora le meto 20, veréis que filtra más, claro.

![](../img/106.png)

Y ahora es: oye, si no pasa de 20, no verás largo o corto, la línea, cada una de ellas. Entonces lo estoy filtrando más. Depende del valor que le des: filtra más o filtra menos. Eso está claro. ¿Vale?





## Preguntas : 




***¿El ATR medido al *peak* es una forma de normalizar el ATR?***

Sí, , es una manera de normalizar el ATR. Sí, sí, correctísimo.

De hecho, nuestra forma de normalizar el ATR te la enseño inmediatamente, que para eso te has gastado las panojas en el curso. 

```sh
# VB-01 : Strategy
If Nivel_ATR > 0 Then
	//FiltroATR = AvgTrueRange(Barras) / Average(Close, Barras) > (Nivel_ATR/100);
	FiltroATR = AvgNormalizedTrueRange(Barras) > (Nivel_ATR)
```

![](../img/101.png)

```sh
# AvgNormalizedTrueRange()
inputs: Length (numericsimple);
var:	double NATR (0);
NATR = Average (NormalizedTrueRange , Length);
AvgNormalizedTrueRange = NATR;
```

Esto es un ATR normal. O sea, el ATR es igual. 

![](../img/102.png)

```sh
#NormalizedTrueRang
var: double NATR (0);
NATR = MaxList (H - L, C[1] - L, H - C[1]) / TypicalPrice * 100;
	# NormalizedTrueRange = TrueRange / TypicalPrice * 100;
NormalizedTrueRange = NATR;
```

El único cambio que hay es que nosotros dividimos por `*typical price*`. Pero sí, puedes dividir por *close* igual. O sea, sí, es lo mismo, de verdad. Esto es el ATR normal y corriente  `MaxList (H - L, C[1] - L, H - C[1])`. Eso es el ATR. Lo único que lo hacemos en cada cálculo. ¿Vale? Es mejor hacerlo así, porque así en cada cálculo te lo normaliza. Y luego hace la suma del average de cada uno de ellos. Ya te lo hace en porcentaje cada una de ellas.

Es decir, al final, cada vela… ¿qué vela es esta? ¿1,2? Vale, pues 1,2. ¿1,3? Vale, 1,3. ¿1,3? Pues 1,2 más 1,3, ... Eso normaliza. ¿Entiendes? No normaliza todos los valores de ATR si lo divide por el valor de ATR, porque eso no es del todo correcto. En valores largos cambia un poco eso. ¿Me explico? Es decir, yo sumo 100 puntos, 200 puntos, 600 puntos y normalizo eso. No. Es mejor calcular el porcentaje y luego normalizar los porcentajes, que es como se ha hecho aquí. ¿Vale?

Es sencillo. No tiene más. Pero sí, sí, normalizar el ATR es eso. Esto, al final, si lo ves en un gráfico… muchos gráficos hemos visto aquí. 





