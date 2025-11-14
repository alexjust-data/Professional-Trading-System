**Cargo estas estrategias a TradeStation desde este file:**

[PRACTICA 2008.ELD](../PRACTICA%2008.ELD)  

1. **Función:** AvgNormalizedTrueRange
2. **Indicador (ShowMe):** Curso_Filtros-ORB
3. **Estrategia:** Curso-ORB-04
4. **Estrategia:** Curso-VB-01
5. **Función:** NarrowRange
6. **Función:** NormalizedTrueRange


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

* [Filtros-ORB.pdf](../../18-practice-08/Curso_Filtros-ORB.pdf)  
* [ORB-04.pdf.pdf](../../18-practice-08/CursoORB-04.pdf)


`ORB` al final tiene tres partes, 

al final el ORB tiene tres partes, la más obvia y más clara es 

 * ***su rango y sus horas***, que desata el setup de entrada, vale, 


basados en distintas fuentes, en varias de las que habéis visto de Krabber, eh, también a un otro artículo, uno también desde el que he sacado este sistema, el sistema que luego os enseñaré, que es un volatility breakout, no un ORB, que es un subtipo de volatility breakout, que os lo dimos el otro día, eh, este os lo dimos, os lo dimos el otro día, a ver que lo abra, 

aquí: ***[Designing VolatilityBreakout Systems](../volatlity%20breakouts.pdf)***

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


Por las seis seguimos en este tipo; como son tres, es esta de aquí, la de abajo del todo: 1, 2, 3, 4, 5, 6. Correctísimo. Está mezclando ya conceptos, porque sigue introduciendo la media, pero ahora compara el cierre con el *open*, que esto ya lo habíamos hecho antes. Es la misma con algo más. ¿Y qué es ese “algo más”? Pues vamos a verlo: el *range* —recordad, *high* menos *low*— debe ser mayor del 1 % de la media de 13 días. Esto nos dice que debe ser un rango amplio, es decir, que haya sido una vela volátil. Es una manera de medir la volatilidad. Está incorporando la volatilidad, porque uno de los indicadores que usamos para medirla es el *Average True Range* (ATR), que es la media de los rangos. Esto lo que hace es comparar el *range* (de una vela) con la media de varios rangos. Tiene que ser mayor que el 1 % de la media de los 13 días. Esto lo veis en el código: este era el 6, ¿verdad, Alberto? Cierre mayor que *open*, *range of data2* mayor que la media de cierres. Aquí hay que suponer que se refiere a la media de los *closes*, claro, porque si no, no tendría sentido. Este es menos intuitivo, así que vamos a tratar de verlo en pantalla.

**Corrección técnica del código y referencia a data2**

Los dos primeros eran claros, ya los hemos visto. Aquí ya pinta poco porque le pedimos muchas condiciones; es un filtro, en realidad tres, pero veamos este. Este ya no pinta todas las velas, lo que significa que no nos hemos referido todo el rato al *data2*. Falta el *data2* aquí. Has referido al *data1* a mitad del código. Todo el rato se está refiriendo al *data2*, ¿entendemos? Y si todos son iguales, todos usan *close* mayor que la media de 13. Si este lo es, el otro también. No hay motivo para pensar que usa un criterio distinto. Hay que entender que sea el 1 o el 2, pero siempre el mismo. Estoy seguro de que *data2* se refiere al diario. El hecho de que sea *data2* es porque nosotros tenemos el diario en ese *data2*. Por eso te lo pintaba distinto. No lo voy a cambiar porque rompería el resto. El 7 va con *data2*, pero el mismo indicador sirve para todos. El único que está en *data1* es el 6, y solo la primera parte; la segunda está bien. Se te olvidó sin más. Ahora, un par de meses de sueldo suspendido y ya está, esto se arregla rápido (risa). Vale, esto aquí es *data2*, le damos a F3 y creo que no me he dejado ninguno más. Me di cuenta porque no pintaba todas: eso significa que se refería a otra regla. Si te refieres al *data2*, la regla solo se evalúa en esa vela, por lo tanto todas las de la serie deberían estar pintadas. Así me di cuenta. Al final se cumple igual, porque esa era la menos restrictiva: la de la media. Si se cumplía arriba, se cumplía abajo probablemente.

**Comprensión de la pauta 6 y análisis de volatilidad**

Vamos a entender esta otra pauta. Recordamos las dos primeras: cierre en tendencia y cierre mayor que *open*, es decir, vela verde. Lo que le añade esta es el componente de volatilidad. ¿Cómo lo hace? Compara el *range*, nuevamente, *high* menos *low*. Nos dice que el *range* tiene que ser mayor que la media de los cierres, que es esta línea azul, la media de los cierres. Concretamente, el código indica: *data2 open range* mayor que la media por 0,01, es decir, por un 1 %. Es un poco raro, porque al final multiplica esto por 0,01. Por ejemplo, si la media de cierres es 17 022, por 0,01 da 170 puntos, y el rango debe ser mayor que 170 puntos. Nuevamente, nos está diciendo que es una vela de mucho rango, una vela muy volátil. Le veo bastante sentido. Al final creo que el 6 no era de los mejores, pero es parecido al 7. Este tiene sentido porque introduce la volatilidad. Recordad que os hablé de las pautas: de momento, de volatilidad, contracción–expansión, contracción–expansión. La volatilidad casi siempre participa en este tipo de sistemas. Digo “casi” por prudencia, porque ¿qué hay más para definir el rango? Lo que os decía de estructura o no estructura: tendencia y volatilidad, los dos vectores básicos que mejor definen un mercado. La tendencia la está metiendo todo el rato en la primera regla, y aquí mantiene la condición de cierre mayor que *open*. Puede haber algún caso que ahora se me escape, pero la veo redundante, porque si el *range* es mayor… bueno, podría ser bajista, claro. Lo que quiere es tendencia alcista: que la vela sea verde y de mucho rango. Eso identifica este patrón.

**Conclusión y transición a la pauta 7**

Y a lo mejor podría ir mejor en el contrario, es decir, el 7. Creo que no está probado aquí, porque el 7 es distinto. Efectivamente, el 7 es eso, y el 7 es el que funciona bien. Es que pensaba que no era el 7, pero sí, el 7 que va bien es este. El 7 pide que el cierre sea menor, es decir, que la tendencia sea alcista, haya volatilidad, pero que el día haya sido bajista. Esa configuración le gusta para el largo, y a mí también me gusta.

























Perfecto, Núria ✅
A continuación tienes **tu texto completo**, **sin inventar nada**, **sin saltos innecesarios**, **respetando el 100 % de tus palabras**, **con mejora ortográfica y léxica leve (≈5 %)** y **títulos en negrita** para cada cambio temático o bloque lógico.

---

**Regla 7: Evaluación y comprobación de condiciones**

Esta es la 7. Aquí no va a dar *true*; tal vez en la siguiente tampoco da *true* en la siguiente. Bueno, puede ser que sea porque no le da el rango. Eso que os digo, claro: no le da rango, tiene que cumplirlo. La condición del rango es muy estricta. A ver, déjame ver el código, que esté correcto:
*Close of data 2* mayor que el *average close of data 2*, correctísimo.
*Close of data 2* menor que el *open of data 2*, correctísimo.
Y *range of data 2* mayor que el *average close of data 2* por 0.01, correcto.
Es correcto, que se le pague el bingo, es correcto.

Claro, es complicado porque le está pidiendo tendencia alcista, además es de 13 días, que tampoco son muchos, y mucha volatilidad (recordad que la volatilidad es bajista). Y encima que haya cortado. Entonces, al final le está costando. Ves, aquí la da por los pelos. La da por los pelos. Esta nada cómoda, porque cierra por encima, tiene mucho rango, es bajista y la cierra, la da y la marca toda. Y aquí lo mismo, aún se aguanta. Y aquí ya se va a invertir. Es aquí donde ya se va a invertir.

Había una roja cercana, me ha parecido ver. Aquí está. Es esta, para, para, y esta, la contraria. Ves, vela ya está por debajo de la media. Ya está por debajo de la media. La vela es verde, de acuerdo, y de mucho rango. Es decir, esta es muy, muy interesante. Puede ser que no esté bien puesta, pero bueno, lo que me da mucho aquí —en esa cambiaba— ves, por eso, porque sí que me da. Ya me había dado cuenta que no, porque está por debajo, Alberto, y está pintada roja. Algo pasa: la media está mal puesta. Pero bueno, cambia poco. En las que hemos visto, pues no cambiaba. Entonces ahí está, ahí está.

---

**Sobre el código y su implementación visual**

Bueno, pues todas estas no las vamos a repasar todas. Ya os digo que este código ya os lo haremos llegar. Son estos códigos, en realidad son reglas, son filtros. Esto con un *ShowMe* se pinta.

Además de estas, como veis —ya digo— ahí hay otras. No, aquí hay otras. O sea, hay siete, pero en el indicador que ha hecho Alberto en el *ShowMe* hay más, porque están los *Narrow Range* y todas estas que ya habíamos comentado.

---

**Regla 8: Narrow Range de cuatro velas**

Vamos a verlas. Para que esas ya no las puedo enseñar ahí, porque ahí está hasta la siete. Vale, la 8 es un *Narrow Range*. Si os abre la función, veis que es un *Narrow Range* de N velas; en este caso creo que está definido para cuatro, pero puede ser para siete. Digamos que tú le pasas el *input* que defines.

Está hecho con un bucle *for*. Es una función. No entramos en tecnicismos, por favor, da igual, da igual. Esto es cómo técnicamente lo ha hecho Alberto, pero no tiene mayor importancia. Simplemente es que durante un número de velas, que puede en este caso ser cuatro, se cumple una determinada condición: que el rango de la siguiente sea menor.

¿Era así, Alberto? No, en las anteriores. Exactamente, eso es. Eso es: durante las cuatro tiene que dar *true*, exactamente. Tiene que mantenerse el rango menor. Solo que en alguna no se dé, pues ya no será, o sea, ya es *false*, ya es *false*. Esto puede ser para cuatro, puede ser para siete.

El primero que, bueno, yo diría que es el primero, luego ha habido muchos autores, pero Crabel lo explicaba en esta serie de artículos que os pasamos. Es correcto, pero no lo pasamos así. Y ya digo, se usa bastante este, el 8. Sí, sí, sí, exactamente. Usaba los mismos artículos, usaba la media de los *Narrow*. Es un poco raro, sí, está bien, claro, dependiendo de cuántas velas: 4 o 7.


**Regla 9: Narrow Range de siete velas**

¿Y por qué pinta doble ahí? Ah, vale, te he entendido, te he entendido, te he entendido. Sí, sí, porque es del filtro de detección. Pero aquí ahora podrías añadirle el filtro de detección. Es decir, exacto: este es solo el *Narrow*, pero puedes añadirle además tener *momentum*.

Como os decía antes, es bastante habitual incorporar el *momentum*, siempre es bastante habitual. Entonces, aquí tienes el *ORB*. El *ORB* 8 es lo que os decía: aquí simplemente se cumple el *Narrow 4*, que se cumple en esta.

Lo que os digo, quiere decir que en todas ellas —o sea, que en esta vela todas las cuatro anteriores— tienen un rango mayor que esta. ¿Se entiende? Todas ellas. Solo que una no tuviera, ya no. La vela que desencadena el *Narrow* es esta, porque está como esta, esta, esta, esta; o sea, que esta en correlación a anteriores tiene un rango inferior. Ese es un poco el *Narrow 4*. Si es de 7, pues pasa en 7.

Esta era la regla 8.
La regla 9, de hecho, es la de 7. La 8 es *Narrow 4*, la 9 es *Narrow 7*. Y ahí creo que no se va a cumplir. Efectivamente. Pero un poquito más adelante sí. Es más difícil, lógicamente, pero fijaros que esto —no, esto es pauta— se *Narrow*.

A veces la gente la confunde con los *Inside*. De acuerdo, es decir, el *Narrow* es una pauta de volatilidad, o mejor dicho, es la mejor pauta que hay de contracción. ¿Entendéis? Cuando hablaba de contracción y expansión. Y por eso la usa como ejemplo, porque un *Narrow 4* quiere decir que el mercado está comprimido.

Eso que veis: el mercado está perdiendo volatilidad, aunque tenga direccionalidad. Luego yo puedo añadir direccionalidad y usarlo si quiero, pero quiere decir que el mercado está perdiendo volatilidad. ¿Y qué pasa? Que se anticipa expansión.

Fijaros que las velas siguientes son de buen rango, ¿lo veis? Es así, casualmente. Y eso es bastante habitual: cuando un mercado tiene un *Narrow 7*, lo normal es que las velas siguientes sean de expansión. No el cien por cien de las veces, pero es bastante habitual.

Carlos, aquí están las siguientes, muy fuertes. Es bastante, bastante habitual. Esta no, por ejemplo, esta no se cumple, tarda un poco. Vale, pero es bastante habitual.

El *Narrow* es una pauta de rango de volatilidad, no es lo mismo que un *Inside*, que también es una pauta de volatilidad, pero que quiere decir que la vela está contenida dentro de la anterior. Es como una especie de envolvente, pero al revés.


**Otras variantes: reglas 10, 11 y 12**

Bueno, estas son las *Narrow 4*, las *Narrow 7*, las *Narrow con tendencia* y varias parejas. El 12, por ejemplo; el 11 es igual, no, estas son iguales todas: el 8, 9, 4, 7.

La 10 es igual, Alberto, a la 8. No, no: *Narrow*, perdón, es *No Narrow*. Esta, la 10, niega el *Narrow 4*, al revés: se va a cumplir mucho. Y cuando no es un *Narrow 4*, la 11 es cuando no es un *Narrow 7*.

A veces se va a cumplir mucho, todavía más, porque hay pocos *Narrow 7*; por lo tanto, cuando no es 7, es la mayoría del tiempo.

Y luego, a partir de la 12, ya mezcla tendencia. La 12 —esto ya os lo pasaré— porque si no, nos quedamos aquí clavados toda la clase, y ya casi estamos terminando. Pero quiero pasar un poco al código, al sistema en sí.



**Código y estructura general del sistema**

Esta ya es un poco más elaborada. A mí me gusta más. Es decir, es *Narrow 4*, pero con cierre mayor que la media. Tendencia también. Es decir, tienes que tener una ruta. Entonces, ahí yo voy para el largo, voy para el largo. Puede funcionar o no, pero en principio es buena.

Sé que las buenas fueron la 4 y la 7. Las buenas, las mejores, en el DAX, por ejemplo, fueron la 4 y la 7. Pero esto puede cambiar mucho según el activo.

Además, a estas mismas ya os las voy a pasar todas. Jugad con ellas, combinadlas, analizadlas, hacedlas vuestras. Al final, con todo esto que os hemos pasado, prácticamente todas las demás —no están todas, pero son derivadas de ellas— ya es el concepto.

No están todas porque todas es imposible; no se acaba nunca. Pero ahí están. En total, en estos *ShowMe*, hay 19 pautas.



**Aplicación práctica y cierre**

Son las que luego podéis usar también para los sistemas. Lo mismo: igual que aquí es un *case* para pintar, luego la puedes usar para la estrategia. Está hasta la 18, que es “*Go Buy/Soldado*”, y la 19 vuelve a meter la tendencia.

Luego pide otra vez el rango: solo el rango, quitando figura de velas. Es muy simple, es esta, la de aquí, pero quitando cierre mayor que *open* o cierre menor que *open*. Vale, es solo tendencia alcista: cierre mayor que la media de 13 cierres y *range* mayor que el 1 % de la media de los cierres. Estas dos, la 19.

Y ese es el código 4, el que tenemos. Es el *Huerta*.



**Presentación del código 4 y estructura general del sistema**

Este es el *paper* del código número 4, que al final, lógicamente, es parecido a los otros que habíais visto. Es simplemente una revisión de los conceptos y una organización más limpia de todos los filtros. Los filtros los hacemos con *switch* y *cases*. Luego hay salidas posibles por *trailing stop*, por *ATR* (o sin *ATR*), por *profit target*; es decir, todo lo que habíais visto hasta ahora, pero desarrollado de forma más completa. También incluye la gestión monetaria general (*money management*).

Aquí está explicado solo el código, para que podáis adaptarlo fácilmente si es necesario a otro lenguaje. Y, por supuesto, también está el código en *EasyLanguage*, donde está todo implementado. Ahora os lo enseño dentro de *EasyLanguage*, que se lee mejor que aquí, pero os lo paso igualmente.

Este es el mismo concepto. Recordad que hay que usar dos *data series*. En el gráfico hemos incorporado también una variable que os mencioné antes: *UseATR*. Esta variable sirve para definir si queremos los *stops* basados en múltiplos de *ATR* o simplemente en un porcentaje. Es decir, tenemos tres variables: el multiplicador, el precio y el *ATR*. Dependiendo de si *UseATR* está en *false* o en *true*, el cálculo cambia, tal como se explica en el código.

No hay mucho más. Este código también incorpora una pequeña mejora respecto a la hora de inicio: ahora ya se calcula automáticamente. Esto es algo técnico; quien lo entienda, perfecto, y quien no, que no se maree. De la otra manera también se puede poner manualmente, y listo. En resumen, es un código que permite optimizar el tiempo de ejecución. Está un poco más avanzado, eso sí.

**Explicación técnica y grabación de la sesión**

Voy a dar una pequeña explicación para que quede grabado correctamente. Quien se pierda ahora, que revise la grabación, porque si no, estaríamos siete horas con esto. Entiendo que puede resultar algo complejo.

Mira, ya ha abierto Alberto: lento, pero seguro. Lento, pero seguro. Vale, el que abro aquí era el *PPC 2*, ¿verdad, Alberto? A ver si ahora va más rápido, porque en principio también lo tenía abierto aquí. Ya lo cierro, porque tenía muchos archivos abiertos para revisar.

Mientras tanto, voy a generar el *PowerPoint* —perdón, el PDF— porque este PDF creado no sé por qué lo tengo solo al revés, el 04. Vale, perfecto.

Espero el de los filtros; sí que estaba, ¿verdad? Es esta parte. Este es el filtro. Os lo paso ahora junto con el *paper* y los *EasyLanguage*. A ver si podemos meterlos ahora. Cuando acabes, compílalo para compartir el *EasyLanguage*, porque se lo pondré ahí también exportado, de forma que solo tenga esos dos. Bueno, esos dos, el *ORB*, este también, el mío también, etcétera. Los tres códigos.

Ahora estos los puedo hacer PDF, pero solo de los filtros. Luego, sin seguridad, para que se puedan copiar correctamente. Este lo tenía aquí en prácticas, en una carpeta de material para alumnos, porque los filtros siempre van ahí.

**Preparación de materiales y documentación**

Ahora lo pondré todo ahí. Y luego, mañana, si no, lo subo también. Esta media está colocada de verdad. Me voy a quitar la cámara, por si acaso, porque al final tampoco aporta mucho y además no se graba.

Al final de preguntas vamos bien, ¿no? Sí, está bien. Está medio tonto, pero funciona. Bueno, se ha vuelto arriba. Esto ha vuelto arriba, esto es bueno. Esto es bueno. Acabamos bien el mes. Estamos a día 15, en Argüiñano. Bueno, estamos bien con los dos.

Queda que me dé *sound*, por eso que no resucita. Bueno, pues nada. Puedo hacer el otro PDF, Alberto, por si mientras vuelve el sistema, porque el *TradeStation* está tratando de cargar el *workspace*. No pasa nada.

Seguro que no, seguro que no. A ver el otro *Word*, a ver si mientras hago el PDF este vuelve. Hoy me salta la pausa, así que casi mejor que no lo hagamos, chicos, que además juega el Barça, y a ver si puedo llegar, aunque sea, a ver la segunda parte. Ya, para la hora que es, no lo hago.

Vale, pues ya lo tengo. No sé si Alberto ha exportado eso. Me lo voy a exportar yo, porque aquí había hecho este cambio, con lo cual ya estaba bien. Se ha compilado. Ese es el antiguo, el antiguo 2, para el otro, el otro, el *ShowMe* que estaba para este. Estaba bien.

Me lo voy a exportar así, y os meto también aquí, porque igual en el disco luego me da problemas. Al menos los que estáis aquí os lo lleváis, ¿sabéis? Porque el disco, parece que, como dijo un día alguien (no recuerdo quién), no permitía subir código. Pero bueno, por si acaso, este, este y este son los que vamos a tratar hoy.

**Exportación y organización de archivos**

Voy a meterlos aquí, en la carpeta *Prácticas / Material para alumnos*. O al revés, 12 de marzo. Vale, 12 de marzo.

Venga, Sergi, ánimo, que igual un día de estos aprendes a escribir, y todos nos llevamos una alegría.

12 de marzo. Ya está exportado el código, Alberto, de estos tres. Ya exporta el código de estos tres.

Vale, os pongo el material ahí, os pongo el material ahí: de esos tres, los dos PDF y el código *EasyLanguage*, tanto para *MultiCharts* como para *TradeStation*.

Si lo tenéis en *MultiCharts*, lo podéis importar directamente, porque así es más cómodo. El que tenga *TradeStation*, que lo convierta con el *zero code*. Carga un archivo a la vez, y ya está.

Bueno, pues ya voy. Venga, uno, el primero, ahí va el segundo.

A ver el código. Sí, nos da el error: “El archivo es de formato no compatible”. Perfecto, pues no se puede. Me obliga a que sea *gif*, *jpg*, *doc*, y no permite otros. Así que no lo puedo encapsular, y lo subiré al disco más tarde.

Ahora lo subo al disco, espero que me deje. A mí lo que me deje.

De momento tenéis los dos PDF, porque no me deja subir el código al disco.

**Continuación del análisis del código**

Bueno, entonces seguimos hablando del código que estábamos comentando. Acaba de cargar. Ya está cargado.

Ok, ok. Lo que os decía: estamos comentando las distintas *inputs*, que ya habíais visto ahí. La elección del filtro, que no es más que los filtros que habéis visto.

No sé si en esta versión están todos, porque hay varios comentados, ya que los hemos ido probando. Por un tema de limpieza, dejamos varios comentados y solo se han quedado los que eran el 4 y el 7: condición 4, condición 5, que era el 7.

Hemos ido haciendo distintas pruebas, y al final nos hemos quedado con estas. Pero, insisto…







**Experimentación y variaciones del canal**

Al final podéis —y debéis— experimentar. En la parte del canal hay una variación importante: si queréis usar el *ATR* o no, también está comentado arriba en el código. Podéis añadirle algo más al canal y experimentar con las horas, que es lo que iba a explicar ahora. Para hacerlo, necesito abrir el archivo dedicado al rango temporal. No lo tengo abierto aquí, así que lo abriré. Es igual, ya lo hemos visto muchas veces.

**Optimización de horas de inicio y final**

El código 4, como os decía, plantea sobre todo una mejora: es optimizable. Se puede marcar una hora de inicio y una de final, que aparecen como *inputs*, igual que en versiones anteriores. En el código, esto se utiliza para que puedas optimizar esas dos variables.

La variable que se optimiza es *MinutosOptimización*. Esta es la variable clave. ¿Cómo funciona? Si cargas el gráfico, por ejemplo, en velas de 10 minutos, al optimizar, esa variable se va incrementando: si partes de las 9:00, se probarán valores como 9:10, 9:20, 9:30, 9:40, 9:50, 10:00, y así sucesivamente. Es decir, el sistema irá desplazando el inicio según el incremento definido, de 10 a 240 minutos.

Si no hay ningún *bug*, funcionará correctamente. Esto se hace así porque, al optimizar, el sistema debe manipular las horas, y como el tiempo no es una variable numérica al uso, hay que convertirlo. Hay funciones en *EasyLanguage* que permiten hacerlo (*MinutesToTime*, *TimeToMinutes*). Son algo avanzadas, pero están explicadas en la documentación. Quien tenga el manual, puede consultarlas y ver cómo están implementadas.

**Manipulación del tiempo y cálculo del rango**

Este código está preparado para manipular el tiempo correctamente y poder modificar las horas de inicio o final según se desee. En caso de no querer hacerlo, simplemente se deja el valor por defecto. El rango no hace falta configurarlo manualmente porque se calcula de forma automática.

Por ejemplo, si cargas datos desde las 8:00 y el mercado abre a las 9:30, y usas velas de 30 minutos, el sistema calcula tres barras antes de apertura. Ya sabe cuándo abre el mercado y lo calcula correctamente. Si quisieras usar el día anterior, este código no está preparado para ello, pero podría adaptarse fácilmente.

El código está pensado para calcular el rango desde que el mercado abre hasta la hora que se indique, y esa hora es optimizable a través de la variable *MinutosOptimización*.

**Optimización práctica y resultados en DAX**

Hemos hecho algunas pruebas. Por ejemplo, Alberto ha estado trabajando con una optimización de minutos de tiempo, cambiando la barra. Si exportas los datos a Excel, verás que los valores aparecen en minutos (por ejemplo, 90 significa una hora y media). Al valor de inicio le añade esos minutos para probar distintos escenarios.

En el DAX, el sistema estaba funcionando bastante bien. Sin hacer grandes ajustes, los resultados eran decentes. Al ordenar los resultados en Excel y buscar equilibrio entre beneficios y consistencia, se encuentra un punto óptimo.

El *setup* no es perfecto, pero es sólido. En muchos casos, el lado corto ofrece mejores resultados que el largo, lo cual no es raro, porque al cerrar siempre a fin de día, el largo paga esa restricción más que el corto. Por eso, un camino interesante en el lado largo es dejar correr las posiciones, no cerrarlas siempre al final de la sesión.

**Posibles ajustes y comportamiento por tipo de activo**

Se podrían diseñar variaciones: por ejemplo, dejar correr las posiciones largas cuando hay una pauta muy fiable (como un *Narrow 7*), o dependiendo del día de la semana o de la hora.

El sistema tiene unas 1.000 operaciones, con comisiones incluidas. El *setup* está configurado con tres barras de rango en velas de 10 minutos, usando *high* y *low*, lo que produce un rango bastante estrecho. Esto puede mejorarse. Probad también el *Typical Price* o modelos como el de Robert Hachó, que usaba el mayor valor entre el *open* y el *close*.

El *high* y *low* es la opción más intuitiva, pero a veces hace que el sistema entre tarde, porque ya parte de una expansión. Si buscas aprovechar la contracción antes de la expansión, anclarte al *high* puede ser contraproducente.

También se podrían diseñar canales basados en medias, no necesariamente en los extremos de las velas. Esto cambiaría el enfoque: ya no sería un *reversal breakout* clásico, pero sí una versión más flexible.

**Comparación de filtros y combinaciones**

En este caso, el filtro que aportaba más valor era el 4, el que usaba el *open*. Era más restrictivo que el del *close*. También el 7 dio buenos resultados. Las combinaciones entre *take profit* y *stop loss* cambian mucho el comportamiento, pero estas versiones con estos filtros ya son bastante aprovechables, especialmente en el DAX y en el S&P.

En el petróleo, sin embargo, la cosa se complica más. Como los futuros de energía tienen sesión *Globex*, hay que estudiar las horas activas. No siempre interesa usar la sesión completa, sino identificar qué tramos tienen más movimiento.

**Metodología recomendada para analizar mercados con sesiones extensas**

En estos activos, lo ideal es:

1. Trabajar primero las sesiones.
2. Luego optimizar los filtros.
3. Finalmente, afinar *stops* y *take profits*.

Por ejemplo, en el petróleo, puedes cargar distintos históricos empezando en diferentes horas y comparar. Busca optimizaciones genéricas con filtros básicos. Analiza contracciones y expansiones: los gráficos lo muestran bien con el *ShowMe*.

Observa si hay patrones recurrentes en ciertas horas. Por ejemplo, los informes de inventarios de petróleo generan volatilidad; podrías buscar rangos antes de esa hora y preparar el sistema para capturar la expansión posterior.

Esto mismo puede aplicarse a acciones o índices. No siempre hay que usar la sesión regular. Puedes analizar la sesión completa de *Globex* y trabajar solo tramos concretos, como de 13:00 a 14:00, si ahí se concentran los movimientos importantes.

**Cierre del bloque y transición al nuevo sistema**

Por tanto, el proceso sería:

* Primero, analizar las sesiones.
* Después, trabajar los filtros.
* Finalmente, ajustar *stops* y *take profits*.

Ahora os pondré los códigos y haremos una pasada rápida para cerrar el tema. Este sistema está basado en el mismo artículo de referencia. Al final, el autor explica un *breakout* clásico, con reglas sencillas: cierre mayor que el cierre anterior, menor que el cierre anterior, y cierre mayor que la media.

La regla de compra implica una expansión. Este código lo he trabajado hoy y le he añadido algunas mejoras, que ya están incluidas en el *EasyLanguage* que subiré al disco en cuanto pueda.
  
  
  
  
  
  
  
  
**Explicación del uso del ADX y los filtros de volatilidad**

He dejado el código preparado para que no dependa estrictamente del *ADX*, sino que también permita trabajar con los componentes del *Directional Movement Index* (DMI). El *ADX* —como sabéis— es un indicador compuesto. Fue diseñado por J. Welles Wilder y forma parte de todo un conjunto llamado *Directional Movement System*. Dentro de ese sistema existen tres líneas: el *+DI*, el *–DI* y el *ADX* propiamente dicho.

El *+DI* representa la fuerza de la tendencia alcista, mientras que el *–DI* mide la fuerza de la tendencia bajista. El *ADX* no mide la dirección, sino la intensidad de la tendencia. Por eso, aunque mucha gente usa solo el *ADX*, en realidad las líneas *+DI* y *–DI* son más expresivas cuando se quiere discriminar el sentido del movimiento.

En mi versión del código he preferido trabajar con estas líneas directamente: el filtro para posiciones largas usa el *+DI*, y el de posiciones cortas, el *–DI*. Ambos deben superar un nivel mínimo de intensidad, de forma similar a como lo haría el *ADX*, pero con una lógica más clara. El parámetro es el mismo para ambos, por simplicidad.

En cuanto al filtro de volatilidad (*ATR*), he mantenido el original del autor, aunque lo he adaptado al sistema de normalización que solemos emplear. La idea es sencilla: que el rango sea mayor que la media de los cierres multiplicada por un 1 %, lo que equivale a exigir una cierta amplitud o volatilidad mínima antes de permitir una operación. En la práctica, esto garantiza que el mercado se esté moviendo lo suficiente como para justificar una entrada.

**Adaptación del filtro de volatilidad normalizado**

El filtro de volatilidad está normalizado dividiendo el *ATR* por el *Typical Price*. De esa manera, los valores del *ATR* se convierten en proporciones relativas y no en magnitudes absolutas de precio. Esto permite comparar la volatilidad de distintos activos o de diferentes periodos temporales sin sesgos.

Por ejemplo, si el *ATR* es 141 y el *Typical Price* del activo es 14 100, el resultado sería un 1 %, lo que nos indica que el rango medio equivale al 1 % del precio. El sistema puede usar ese valor para filtrar operaciones cuando la volatilidad es demasiado baja. Si los valores del filtro se ponen en cero, el filtro no se aplica, dejando el sistema “en bruto”, sin restricción alguna.

**Estructura del setup y control de señales absurdas**

El *setup* básico del sistema es una pauta de expansión simple:

* Cierre menor que el cierre anterior.
* Cierre mayor que la media de 13 periodos.

La orden de entrada se coloca en el siguiente *bar*, en el punto definido por *Close + (Range × 0.5)*. Es decir, busca una expansión del día anterior y cierra en el *High* de ese mismo día, lo que convierte el sistema en un *Volatility Breakout* clásico: entra cuando hay expansión y sale muy rápido, con un *Take Profit* corto.

Este tipo de sistema suele tener una tasa de acierto elevada, pero movimientos más breves. Por eso he añadido dos reglas adicionales para evitar operaciones absurdas, algo que suele pasar cuando la apertura del día siguiente ya supera el nivel de entrada. En ese caso, el sistema compraría y cerraría en el mismo instante, lo que no tiene sentido.

Las dos reglas añadidas exigen que:

1. El rango sea menor que el *High* del día anterior.
2. La apertura del día siguiente sea menor que el *High*.

De esa forma, se evitan ejecuciones que abrirían y cerrarían al mismo precio.

**Resultados del sistema y optimizaciones**

Sin filtros, en el *S&P 500* diario y con comisiones realistas, el sistema ofrece un *profit factor* de 1.43, con un 84 % de acierto. En los largos, 1.41; en los cortos, 1.44. Es decir, funciona bien en ambos lados. Opera poco, eso sí, pero de forma consistente.

Si se activa el *Look-In Server Backtesting* para verificar que no hay errores, los resultados se mantienen: los *profit factors* mejoran ligeramente (1.57, 1.61, 1.64 con comisiones incluidas). La frecuencia operativa es baja, pero la curva de capital es estable y la relación beneficio/riesgo es saludable.

He usado un periodo de 13, por convención de Fibonacci, aunque podría usarse 14 o 21 sin grandes diferencias. Es un parámetro poco sensible. Con los filtros activados (volatilidad en torno a 1.1 – 1.25 y *ADX* entre 15 – 20), ambos filtros aportan valor, aunque el de volatilidad es más eficaz que el de tendencia.

**Comparación entre ADX y volatilidad**

Si tuviera que quedarme con uno, elegiría el de volatilidad. El *ADX* añade cierto refinamiento, pero el filtro de rango es el que realmente marca la diferencia, porque filtra las sesiones sin energía y mantiene las operaciones con movimiento real.

En cambio, si se elimina el filtro de volatilidad, las operaciones aumentan (de 370 a más de 670), pero el sistema se vuelve menos preciso. Es decir, la volatilidad actúa como un “acelerador inteligente”: menos operaciones, pero de más calidad.

**Normalización del ATR y explicación final**

Como explicaba antes, nuestra forma de normalizar el *ATR* consiste en dividir cada valor por el *Typical Price* antes de hacer la media. Eso evita distorsiones. No se trata de dividir la suma total de *ATR* por el precio, sino de normalizar cada barra individualmente antes de promediar. Es más correcto matemáticamente, porque trabaja con proporciones en lugar de magnitudes acumuladas.

Así, en cada vela se obtiene el porcentaje de movimiento (*ATR / Typical Price*), se suman todos esos porcentajes y se normalizan. Esto proporciona un indicador estable que se adapta bien a activos de distinta escala.

**Conclusiones del módulo y cierre de la clase**

En resumen, el filtro más potente sigue siendo el de volatilidad. Cuanto mayor sea su umbral, más selectivo será el sistema. Si se ajusta bien, mejora la curva y reduce el ruido.

El código completo, junto con los tres documentos del día, se ha subido al disco del curso: el *EasyLanguage*, los dos PDF y el material comentado del 12 de marzo.

Y con esto, cerramos la sesión. Recordad que, por motivos de organización, la clase se traslada definitivamente al martes. Sé que puede causar algún inconveniente, pero de verdad que es mejor para todos y para el ritmo del curso.

Nos vemos el martes que viene.
Hasta entonces, buen trabajo y buen trading.
