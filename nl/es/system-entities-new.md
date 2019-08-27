---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-17"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:gif: data-image-type='gif'}

# Nuevas entidades del sistema ![Beta](images/beta.png)
{: #beta-system-entities}

Habilite las nuevas entidades del sistema para aprovechar las mejoras que se han realizado en las entidades del sistema basadas en número que proporciona IBM.

Actualmente, este valor solo se puede habilitar para los conocimientos de diálogo que se escriben en inglés o en alemán.
{: note}

Las nuevas entidades del sistema pueden reconocer menciones más matizadas en la entrada del usuario. Por ejemplo, `@sys-date` puede calcular la fecha de un día festivo nacional, como por ejemplo `Nochebuena`, cuando se menciona por su nombre. Y `@sys-date` puede reconocer cuando se especifica un año como parte de una fecha mencionada en la entrada del usuario. Las mejoras también hacen que sea más fácil para su asistente distinguir entre las muchas entidades de sistema basadas en números. Por ejemplo, una mención a fecha, como `May 10`, que se reconoce como un `@sys-date` no se identifica además como mención de `@sys-number`.

Las entidades de sistema `@sys-location` y `@sys-person` no han cambiado.
{: note}

## Habilitación de las nuevas entidades del sistema
{: #beta-system-entities-enable}

Para habilitar las nuevas entidades del sistema, complete los pasos siguientes:

1.  En la página Conocimientos, abra su conocimiento.
1.  Pulse en el separador **Opciones**.
1.  Pulse en **Entidades del sistema** y, a continuación, seleccione **Probar beta**.
1.  Pulse el separador **Entidades** y luego pulse **Entidades del sistema**.
1.  Active las entidades del sistema que desea utilizar en el diálogo.

Pruebe las nuevas entidades del sistema añadiendo una o más a las condiciones del nodo de diálogo o en la condición de respuestas condicionales del nodo de diálogo. A continuación, en el panel "Pruébelo", envíe expresiones de usuario que activen los nodos que ha añadido.

Si prefiere el comportamiento de la versión anterior de las entidades del sistema, puede dejar de utilizar la versión beta en cualquier momento. Vuelva a la página **Entidades del sistema** en el separador **Opciones** y seleccione **Conservar existente**. Watson necesita algo de tiempo para volver a entrenar.

Si decide continuar utilizando la versión beta, revise los nodos de diálogo existentes en dicha condición o que devuelvan valores de entidad del sistema para determinar si tiene que realizar cambios. Por ejemplo, antes se clasificaban algunas menciones como más de un tipo de entidad de sistema. Ahora, si se menciona una moneda, por ejemplo, sólo se clasifica como `@sys-currency`. Es posible que pueda simplificar la lógica que utilizaba antes como método alternativo a dicho comportamiento. O es posible que tenga que revisar la lógica que ha añadido y que se basa en el comportamiento antiguo.

### Nuevas propiedades de entidad del sistema
{: #beta-system-entities-props}

Para mejorar las entidades del sistema, se han añadido nuevas propiedades a los objetos de entidad de las entidades del sistema basadas en números.

En la tabla siguiente se resumen las nuevas propiedades que se han añadido. Para ver las propiedades que están asociadas con las entidades del sistema actual, consulte [Detalles de entidad del sistema](/docs/services/assistant?topic=assistant-system-entities).

Tabla 1. Nuevas propiedades de entidad del sistema

| Entidad del sistema | Propiedad | Descripción |
|---------------|----------|-------------|
| `@sys-date` | `alternatives` | Captura las fechas que no sean las guardadas como el valor `@sys-date` que el usuario pueda haber querido indicar cuando la fecha no está claramente indicada. Por ejemplo, si la fecha de hoy es el 10 de marzo de 2019, y el usuario especifica `el 1 de marzo`, la fecha del 1 de marzo para el año siguiente se guarda como `@sys-date` (`2020-03-01`). Sin embargo, el usuario podría haber querido referirse al 1 de marzo de 2019. El sistema también guarda la fecha de este año (`2019-03-01`) como un valor de fecha alternativo. Los valores alternativos se guardan como un objeto que contiene un valor y una confianza para cada fecha alternativa y que se almacenan en una matriz JSON. Para obtener un valor alternativo, utilice la sintaxis: `@sys-date.alternative`. En la mayoría de los casos, la fecha futura se guarda como valor de `@sys-date`, y la pasada como la alternativa. Esto es válido para los días de semana cuando se especifica el día de la semana sin un artículo, como `el` o un adjetivo como `este` o `que viene`. Por ejemplo, `¿Abre el lunes?`. Sin embargo, si un usuario especifica un día de la semana como parte de una frase como `el lunes` o `el lunes que viene`, se supone que la fecha es una fecha futura, y no se crea ninguna fecha alternativa para el mismo. Por ejemplo, si hoy es martes y un usuario escribe `este martes` o `el martes próximo`, `@sys-date` se establece en la fecha del martes de la semana que viene, y no se crean fechas alternativas. |
| `@sys-date` | `datetime_link` | Si está presente, indica que la entrada del usuario menciona una fecha y una hora juntas, lo que implica que la fecha y la hora están relacionadas entre sí. Los valores de índice de inicio y final de la serie que incluye la fecha y la hora se guardan como parte del nombre de enlace. Por ejemplo, si la entrada es `¿Está abierto hoy a las 17:00?`, `hoy` se reconoce como una referencia de fecha en la ubicación `[13,18]` y `a las 17:00` se reconoce como una referencia temporal en la ubicación `[19,23]`. El valor de ubicación `[13,23]`, que abarca tanto la fecha como la hora, se añade al datetime_link resultante. Se le llama `datetime_link_13_23` y se incluye en la salida de las entidades del sistema `@sys-date` y `@sys-time` que participan en la relación. |
| `@sys-date` | `festival` | Reconoce los nombres de días festivos específicos del entorno local y puede devolver la fecha de los próximos festivos, como por ejemplo `Día de la Constitución`, `Navidad` o `Día de todos los Santos`. Si quiere especificar el nombre de un festivo, utilice minúsculas en la condición; por ejemplo, (`@sys-date.festival == 'navidad'`). |
| `@sys-date` | `granularity` | Reconoce las menciones a intervalos de tiempo, como `el año que viene` (=`año`). Las opciones son `day`, `weekend`, `week`, `fortnight`, `month`, `quarter` y `year` (día, fin de semana, semana, quincena, mes, trimestre y año). |
| `@sys-date` | `interpretation` | El objeto devuelto por el asistente que contiene campos nuevos que aumentan la precisión del clasificador de entidad del sistema. Puede omitir `interpretation` de la expresión que utiliza para hacer referencia a sus campos. Por ejemplo, puede acceder al valor del campo `festival` en el objeto de interpretación utilizando la sintaxis abreviada `<? @sys-date.festival ?>`. |
| `@sys-date` | `range_link` | Si está presente, indica que la entrada del usuario contiene una sintaxis que sugiere que se ha especificado un rango de fechas. Por ejemplo, la entrada podría ser `Voy a viajar del 15 de marzo al 22 de marzo`. Cada una de las dos entidades del sistema `@sys-date` que se detectan tienen una propiedad `range_link` en su salida. Se proporciona información adicional, incluyendo el rol que cada `@sys-date` juega en la relación de rango. Por ejemplo, la fecha de inicio tiene el tipo de rol de `date_from` y la fecha de finalización tiene el tipo de rol `date_to`. Para comprobar un valor de rol, puede utilizar la sintaxis `@sys-date.role?.type == 'date_from'`. Si la entrada de usuario implica un rango, pero sólo se especifica una fecha, la propiedad `range_link` no se devuelve, pero se devuelve un tipo de rol. Por ejemplo, si el usuario pregunta, `¿Ha estado abierto desde ayer?` La fecha de ayer se reconoce como la mención `@sys-date`, y se devuelve el rol de tipo `date_from`. También se devuelve un `range_modifier` que indica la palabra en la entrada que ha propiciado la identificación de un rango. En este ejemplo, el modificador es `desde`. |
| `@sys-date` | `relative_year`, `relative_month`, `relative_week`, `relative_weekend`, `relative_day` | Reconoce y captura menciones de valores de fecha relativa en la entrada del usuario, como `hace dos semanas` (`relative_week = -2`), o `este fin de semana (relative_weekend = 0)` u `hoy` (`relative_day = 0`). |
| `@sys-date` | `specific_year`, `specific_quarter`, `specific_month`, `specific_day`, `specific_day_of_week` | Reconoce y captura menciones de valores de fecha específicos en la entrada de usuario, como `2020` (`specific_year = 2020`), `Q2` (`specific_quarter = 2`), `March`  (`specific_month = 3`), `Monday` (`specific_day_of_week = lunes`) o `3rd` (`specific_day = 3`). |
| `@sys-number` | `interpretation` | El objeto devuelto por el asistente que contiene campos nuevos que aumentan la precisión del clasificador de entidad del sistema. Puede omitir `interpretation` de la expresión que utiliza para hacer referencia a sus campos. Por ejemplo, puede acceder al valor del campo `range_link` en el objeto de interpretación utilizando la sintaxis abreviada `<? @sys-date.range_link ?>`. |
| `@sys-number` | `range_link` | Si está presente, indica que la entrada del usuario contiene una sintaxis que sugiere que se ha especificado un rango de números. Por ejemplo, la entrada podría ser `Estoy pensando alojarme de 5 a 7 días`. Cada una de las dos entidades del sistema `@sys-number` que se detectan tienen una propiedad `range_link` en su salida. Se proporciona información adicional, incluyendo el rol que cada `@sys-number` juega en la relación de rango. Por ejemplo, el número de inicio tiene un tipo de rol de `number_from` y el número final tiene un tipo de rol de `number_to`. Para comprobar un valor de rol, puede utilizar la sintaxis `@sys-number.role?.type == 'number_from'`. |
| `@sys-time` | `alternatives` | Captura las horas que no sean las guardadas como el valor `@sys-time` que el usuario pueda haber querido indicar cuando la hora no está claramente indicada. Por ejemplo, cuando un usuario dice `La reunión es a las 5`, el sistema calcula el tiempo como 5:00:00 o 5 AM y lo guarda como `@sys-time`. Sin embargo, es posible que el usuario quisiera decir 17:00:00 o 5 PM. El sistema también guarda ésta última opción como un valor de hora alternativo. Si el usuario especifica `La reunión es a las 5pm`, `@sys-time` se establece en 17:00:00, y no se crea ningún valor alternativo porque no hay ninguna confusión de AM y PM. Los valores alternativos se guardan como un objeto que contiene un valor y una confianza para cada hora alternativa y que se almacenan en una matriz JSON. Para obtener un valor alternativo, utilice la sintaxis: `@sys-time.alternative`. |
| `sys-time` | `datetime_link` | Si está presente, indica que la entrada del usuario menciona una fecha y una hora juntas, lo que implica que la fecha y la hora están relacionadas entre sí. Los valores de índice de inicio y final de la serie que incluye la fecha y la hora se guardan como parte del nombre de enlace. Por ejemplo, si la entrada es `¿Está abierto hoy a las 17:00?`, `hoy` se reconoce como una referencia de fecha en la ubicación `[13,18]` y `a las 17:00` se reconoce como una referencia temporal en la ubicación `[19,23]`. El valor de ubicación `[13,23]`, que abarca tanto la fecha como la hora, se añade al datetime_link resultante. Se le llama `datetime_link_13_23` y se incluye en la salida de las entidades del sistema `@sys-date` y `@sys-time` que participan en la relación. |
| `@sys-time` | `granularity` | Reconoce las menciones de los plazos, como `now` (=`instant`), `3 o'clock`, `noon` (=`hour`) o `17:00:00` (=`second`). Las opciones son `hour`, `minute`, `second` e `instant`. |
| `@sys-time` | `interpretation` | El objeto devuelto por el asistente que contiene campos nuevos que aumentan la precisión del clasificador de entidad del sistema. Puede omitir `interpretation` de la expresión que utiliza para hacer referencia a sus campos. Por ejemplo, puede acceder al valor del campo `granularity` en el objeto de interpretación utilizando la sintaxis abreviada `<? @sys-time.granularity ?>`. |
| `@sys-time` | `part_of_day` | Reconoce los términos que representan la hora del día, como por ejemplo `morning`, `afternoon`, `evening`, `night` o `now` (mañana, mediodía, tarde, noche o ahora). También establece horarios arbitrarios, como `9:00:00` para la mañana, `12:00:00` para mediodía, `18:00:00` para la tarde y `22:00:00` para la noche. |
| `@sys-time` | `range_link` | Si está presente, indica que la entrada del usuario contiene una sintaxis que sugiere que se ha especificado un rango de horas. Por ejemplo, la entrada podría ser `¿Abren de 9AM a 11AM?`. Cada una de las dos entidades del sistema `@sys-time` que se detectan tienen una propiedad `range_link` en su salida. Se proporciona información adicional, incluyendo el rol que cada `@sys-time` juega en la relación de rango. Por ejemplo, la hora de inicio tiene el tipo de rol de `time_from` y la hora de finalización tiene el tipo de rol `time_to`. Para comprobar un valor de rol, puede utilizar la sintaxis `@sys-time.role?.type == 'time_from'`. Si la entrada de usuario implica un rango, pero sólo se especifica una hora, la propiedad `range_link` no se devuelve, pero se devuelve un tipo de rol. Por ejemplo, si el usuario pregunta, `¿Está abierto hasta las 9PM?`, se reconoce 9PM como la mención `@sys-time` y se devuelve un rol de tipo `time_to`. También se devuelve un `range_modifier` que indica la palabra en la entrada que ha propiciado la identificación de un rango. En este ejemplo, el modificador es `hasta`. |
| `@sys-time` | `relative_hour`, `relative_minute`, `relative_second` | Reconoce las menciones de tiempo relativas, como `hace 5 horas` (`relative_hour = -5`),`en dos minutos`(`relative_minute = 2`) o `en un segundo` (`relative_second = 1`). |
| `@sys-time` | `specific_hour`, `specific_minute`, `specific_second` | Reconoce menciones específicas de hora, como `a las 5 en punto` (`specific_hour = 5`), `a las 2:30`(`specific_minute = 30`) o `23:30:22` (`specific_second = 22`). |
{: caption="Nuevas propiedades de entidad del sistema" caption-side="top"}

Los valores de la tabla siguiente no son propiedades del objeto de entidad. Son valores de ayuda que se pueden utilizar en el producto para extraer rápidamente detalles de fecha u hora de las nuevas entidades del sistema.

Tabla 2. Funciones del ayudante de entidades del sistema

| Entidad del sistema | Valor del ayudante | Descripción |
|---------------|----------|-------------|
| `@sys-date` | `day` | Devuelve el día que se especifica en la fecha como un valor numérico. Por ejemplo, si la fecha es `5 de diciembre de 2019`, devuelve `5`. |
| `@sys-date` | `day_of_week` | Evalúa la fecha para determinar el día de la semana en que se encuentra. Almacena el día de la semana en minúsculas. Por ejemplo, `Domingo` se almacena como `domingo`. Utilice una letra inicial en minúsculas para comprobar el nombre de un día de semana en una condición. Por ejemplo: `@sys-date.day_of_week == 'domingo'` |
| `@sys-date` | `month` | Devuelve el mes que se especifica en la fecha como un valor numérico. Por ejemplo, si la fecha es `5 de diciembre de 2019`, devuelve `12`. |
| `@sys-date` | `year` | Devuelve el año que se especifica en la fecha como un valor numérico. Por ejemplo, si la fecha es `5 de diciembre de 2019`, devuelve `2019`. |
| `@sys-time` | `hour` | Devuelve la hora que se especifica en el formato de hora como un valor numérico. Por ejemplo, si la hora es `5:30:10 PM`, devuelve`17` para representar las 5 PM. |
| `@sys-time` | `minute` | Devuelve el valor de minuto que se especifica en la hora. `30`. Si no se especifica ningún minuto, este ayudante devuelve `0`.|
| `@sys-time` | `second` | Devuelve el valor de segundos que se especifica en la hora. `10`. Si no se especifican segundos, este ayudante devuelve `0`. |
{: caption="Funciones del ayudante de entidades del sistema" caption-side="top"}

### Ejemplos de uso
{: #beta-system-entities-examples}

Puede utilizar los siguientes tipos de expresiones para aprovechar de las nuevas propiedades en las entidades del sistema mejoradas. La tabla muestra los resultados que se devuelven cuando un usuario menciona la fecha `4 de julio de 2019` y la hora `3:30 PM`. Puede utilizar estas expresiones en las respuestas de texto de diálogo sin tener que incluirlas entre los códigos de sintaxis `<? ?>`.

Tabla 3. Utilización de las nuevas entidades del sistema para obtener detalles de fecha y hora

| Sintaxis de la expresión SpEL | Resultado |
|------------------------|---------|
| `@sys-date.year` | `2019` |
| `@sys-date.month` | `7` |
| `@sys-date.day` | `4` |
| `@sys-date.day_of_week` | `thursday` |
| `@sys-time.hour` | `15` |
| `@sys-time.minute` | `30` |
| `@sys-time.second` | `0` |
{: caption="Uso de las nuevas entidades del sistema para obtener detalles de fecha y hora" caption-side="top"}

En un nodo que condiciones en el intento `#Customer_Care_Store_Hours`, puede añadir respuestas condicionales que utilizan nuevas propiedades de entidad de sistema para proporcionar respuestas ligeramente distintas sobre el horario de la tienda, en función de lo que el usuario solicite. 

Es probable que no quiera utilizar todas estas respuestas condicionales en un diálogo real; se describen aquí simplemente para mostrar las posibilidades.
{: tip}

Tabla 4. Utilización de nuevas entidades del sistema en respuestas condicionales

| Sintaxis de condición de la respuesta condicional | Descripción | Ejemplo de texto de respuesta |
|--------------------------------|-------------|----------|
| `@sys-date.festival == 'thanksgiving'` | Comprueba si el cliente está preguntando sobre horas en el día de Día de Acción de Gracias, concretamente. | Damos comidas gratis a las personas necesitadas en el Día de Acción de Gracias. |
| `@sys-date.festival` | Comprueba si el cliente está preguntando sobre horario en otro festivo concreto. | Estamos cerrados el día de Navidad, el día de Reyes y el Viernes Santo. En el resto de los festivos, estamos abiertos de 10:00 a 17:00. |
| `@sys-time.part_of_day == 'night'` | Comprueba si el usuario incluye algún término que mencionen la noche (night) como hora del día en la entrada. | No abrimos hasta tarde; cerramos a las 21:00 la mayoría de los días. |
| `@sys-date.datetime_link && input.text.contains('today') && now().sameOrAfter(@sys-time)` | Comprueba si la entrada contiene una frase como, por ejemplo, `hoy a las 8`. También comprueba si el `@sys-time` que se detecta es anterior a la hora actual. Puede añadir una variable de contexto a la respuesta condicional que crea una variable `$new_time` con el valor `<? @sys-time.plusHours(12) ?>`. Puede utilizar este enfoque para crear una variable de fecha que capture la hora más probable a la que se refiere un usuario que, a mediodía, pregunta `¿Está abierto hoy a las 8`? El sistema presupone que el usuario se refiere a las 8 de la tarde, no de la mañana. | ¿Se refiere a hoy a las $new_time, ¿verdad? |
| `@sys-time.range_link && entities[0].role?.type == 'time_from' && entities[1].role?.type == 'time_to'` | Comprueba si la entrada contiene un intervalo de tiempo. Antes de incluirlas en la respuesta, la condición también se asegura de que la primera entidad del sistema `@sys-time` listada en la matriz de entidades es la hora de inicio y que la segunda es la hora de finalización. | Desea saber si estamos abiertos entre `<? entities[0] ?>` y `<? entities[1] ?>`, ¿verdad? |
| `@sys-time && entities.size() < 2 && entities[0].role?.type == 'time_to'` | Comprueba si la entrada contiene una mención `@sys-time` que tiene un tipo de rol `time_to`. Si la entrada de usuario coincide con esta condición, sugiere que el usuario ha especificado la hora de cierre de un intervalo de horario de apertura-cierre. Por ejemplo, la respuesta se desencadenará mediante la entrada `¿Está abierto hasta las 9pm?`. Esta entrada coincide porque sólo identifica una hora en un intervalo temporal, y la mención tiene un rol `time_to`. | ¿Quiere decir desde ahora (`<? now().reformatDateTime('h:mm a') ?>`) hasta las `<? @sys-time.reformatDateTime('h:mm a') ?>`? |
| `@sys-date.day_of_week == 'domingo'` | Comprueba si una fecha específica que el usuario pregunta cae en domingo. | Cerramos los domingos. |
| `@sys-date.specific_day_of_week == 'lunes' && @sys-date.alternative` | Comprueba si el usuario ha mencionado el `lunes` en su consulta. Por ejemplo, `¿Abren los lunes?`. La condición también comprueba si se han almacenado fechas alternativas. Las fechas alternativas se crean cuando el asistente no está completamente seguro a qué lunes se refiere el usuario, por lo que también almacena las fechas de los lunes alternativos. Cuando un usuario especifica un día de semana, el asistente presupone que el usuario se refiere a un día en el futuro (el lunes que viene). Puede añadir una respuesta que confirme la fecha que se pretende, utilizando el valor alternativo detectado. En este caso, la fecha alternativa es la fecha del lunes pasado. | ¿Se refiere a `@sys-date` o `@sys-date.alternative`? |
| `true` | Responde a cualquier otra solicitud de información del horario de la tienda. | Estamos abiertos de 9:00 a 21:00, de lunes a sábado. |
{: caption="Uso de las nuevas entidades de sistema en respuestas condicionales" caption-side="top"}
