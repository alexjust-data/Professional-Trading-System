# sersans

## 00:01 - 00:06
la emisión está comenzando todos los asistentes están en modo de sólo

## 00:06 - 00:27
escucha qué tal muy buenas tardes cómo estáis os saluda ser y sánchez una

## 00:27 - 00:37
clase más del curso de trading algorímico así que vamos a comenzar con

## 00:37 - 00:45
ello como siempre voy a escribirlo

## 00:46 - 00:54
los que me oís ya veo que me oís porque me estáis contestando pero si alguien no

## 00:54 - 01:00
me oye pues ya lo ya lo he escrito ya lo he escrito por ahí vale venga como

## 01:00 - 01:07
siempre empezamos atendiendo un poquito las las preguntas que hay en él sobre

## 01:07 - 01:13
todo en el disc or también voy a hacer algún comentario algún algún email y

## 01:13 - 01:20
luego vamos a trabajar bueno enseñaros cositas ideas que hemos trabajado

## 01:20 - 01:29
bastante en profundidad durante esta durante esa semana vale a ver el disco

## 01:29 - 01:42
el disco que está bueno ya ha visto ya lo que habíamos dejado comentado bueno

## 01:42 - 01:48
aquí antes voy la verdad que comentabas aquí en el disco que no conseguías que

## 01:48 - 01:57
te genera ningún trade y pues no no te sé no te sé decir con los datos que

## 01:57 - 02:04
pones ahí la verdad tienes la de arriba están estatus son la de abajo están

## 02:04 - 02:13
estatus no sé no sé si alberto se le ocurre algo que pueda provocar que la

## 02:13 - 02:20
estrategia no opere bueno quizás la sesión que lo tenga en el horario local a

## 02:20 - 02:26
lo mejor cargado el símbolo pudiera ser pudiera ser fíjate que lo tengas

## 02:26 - 02:34
cargado en echange el símbolo vale te enseño lo que lo que quiero decir

## 02:34 - 02:43
espera que vamos a sacar ya esto enseño lo que quiero decir vale es decir tú

## 02:43 - 02:47
cuando cargas un símbolo en el gráfico

## 02:47 - 02:57
puedes elegir aquí local o echange vale dependiendo del tipo de sistema si tú

## 02:57 - 03:06
usas horas y tú te refieres a time por ejemplo pues es time ese es la hora del

## 03:06 - 03:12
gráfico entonces tú le has puesto 9 30 y el gráfico va de 15 30 a 10 de la

## 03:12 - 03:16
noche que es lo que te pasa en este caso como yo lo tenía ahora en el local lo

## 03:16 - 03:20
que pasa que este este sistema en concreto lo veremos más adelante es un

## 03:20 - 03:25
volatility breakout no es un rv por lo tanto no usa horas y no importa pero si

## 03:25 - 03:31
tú usas horas pues puede ser puede provocarte eso que no opere vale que no

## 03:31 - 03:35
opere porque pues la hora no existe si tú le dices a partir de las 9 30 hace

## 03:35 - 03:43
x cosa y esa hora no está en el gráfico pues ese puede ser un motivo vale se me

## 03:43 - 03:47
ocurre que se puede ser eso es probable que sea eso es probable que sea eso

## 03:47 - 03:55
porque ya te digo es algo usual ahora ha caído vale bien qué más comentaba

## 03:55 - 04:02
Juan Manuel ya las de Raúl quedó la semana pasada voy voy leyendo respecto a

## 04:02 - 04:07
la configuración del money management para el sistema durante la evaluación

## 04:07 - 04:10
para saber qué exposición neta del sistema en porcentaje se recomienda por

## 04:10 - 04:14
consenso hay que apalancamiento y cómo establecerlo en código bueno no hay una

## 04:14 - 04:20
recomendación de apalancamiento no apalancamiento eso es algo que depende

## 04:20 - 04:24
de muchísimos factores desde uno personal vale desde el riesgo o sea en

## 04:24 - 04:30
evaluación preliminar lo que os comenté es que nosotros aunque también os lo

## 04:30 - 04:34
dije y os digo levantó que sigue habiendo cierto debate sobre el tema es

## 04:34 - 04:39
decir nosotros ya ya os explicó siempre os he intentado explicar las cosas que

## 04:39 - 04:43
que digamos la industria pues tiene ya establecidas cosas que al final hacemos

## 04:43 - 04:46
nosotros porque nosotros las queremos así entonces por haber distintas

## 04:46 - 04:52
opiniones nosotros creemos que es mejor usar

## 04:52 - 04:58
un money management pero es verdad que si tú tienes el gráfico ajustado si tú

## 04:58 - 05:04
tus esto se ajustan a la volatilidad puedes no es grave de acuerdo no es

## 05:04 - 05:09
grave que vaya todo el rato con un con un cotado pero es importante que se

## 05:09 - 05:13
ajuste y aún así habrán el ejemplo del nasda que es el que pusimos en el curso

## 05:13 - 05:18
es muy paradigmático no porque la variación de precios y miras en muy

## 05:18 - 05:25
largo plazo es es bestial claro un contrato del 2000 no es un contrato del

## 05:25 - 05:31
2009 y mucho menos el 2024 entonces si tú todo el rato vas con un contrato pues

## 05:31 - 05:35
ese es el problema entonces realmente usamos un money management que

## 05:35 - 05:40
simplemente permita que esos contratos varíen el que estáis viendo hasta ahora

## 05:40 - 05:45
en estos ejercicios que hemos hecho en la práctica es anominal vale que es

## 05:45 - 05:52
este tengo aquí un sistema que os enseñaré luego bueno igual de hecho da

## 05:52 - 06:00
igual porque todos iguales vale aquí simplemente usamos el importe que le

## 06:00 - 06:03
ponemos nosotros en la cuenta se multiplica por un multiplicador que es

## 06:04 - 06:11
vale que por cien vale lo único que está separado el dinero inicial la

## 06:11 - 06:14
cuenta inicial y el que vas ganando pero al final le ponemos cien iguales y la

## 06:14 - 06:19
realidad no está separado es lo mismo porque en ambos casos multiplicamos por

## 06:19 - 06:26
cien y lo sumamos por lo tanto queda que da igual y eso se divide por un valor

## 06:26 - 06:33
que al final es el cierre multiplicado por big point value vale que es el cierre

## 06:33 - 06:38
multiplicado por big point value el valor nominal de un activo vale es decir

## 06:38 - 06:42
en este caso yo estoy invirtiendo este este money management que ves tú

## 06:42 - 06:48
equivale a ir a cien por cien de exposición es eso pero simplemente es

## 06:48 - 06:53
porque para para mirar un poco cómo va el sistema pues pues usamos uno sencillo

## 06:53 - 06:59
que no tiene mucha mucha historia en las prácticas

## 06:59 - 07:03
probablemente a partir de la de cuando acabemos ya este este volatilidad

## 07:03 - 07:08
breakout o revés y demás que confío que va a ser hoy

## 07:08 - 07:15
entonces seguramente abordaremos algunas clases como llamarlas temáticas de

## 07:15 - 07:20
acuerdo es decir no sólo de hacer un sistema aunque en los ejemplos pues por

## 07:20 - 07:25
supuesto se pueden hacer sistemas pero pues ya ya lo comenté durante la teoría

## 07:25 - 07:29
y también lo he dicho en las prácticas pues por ejemplo pues queda de ideas por

## 07:29 - 07:37
ejemplo el aprovechando en la revisión que hemos hecho de apolo que lo comenté

## 07:37 - 07:41
y ha habido un comentario en el disco estoy de acuerdo que es interesante pues

## 07:41 - 07:46
bueno pues haremos una práctica de eso mirar un sistema real se sale de su zona

## 07:47 - 07:52
si pondremos un poco de esta práctica de un caso real de la revisión de un

## 07:52 - 07:56
sistema que está operando entiendes entonces creo realmente que es algo que

## 07:56 - 08:01
puede aportar mucho mucho valor

## 08:01 - 08:06
claro y por supuesto que es algo que iba ahora gestión monetaria ah ah ah

## 08:06 - 08:10
ah ah undaremos en la gestión monetaria de manera específica la

## 08:12 - 08:16
que la clase estará más enfocada en la gestión monetaria que en el hecho de

## 08:16 - 08:24
hacer un sistema como por ejemplo hoy estamos más enfocados en hacer en hacer un sistema que más
