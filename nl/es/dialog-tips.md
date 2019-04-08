---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

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

# Sugerencias sobre la creación de diálogos
{: #dialog-tips}

Aprenda a adoptar el enfoque adecuado en la creación de un diálogo y vea algunas sugerencias sobre cómo llevar a cabo pasos más complejos.
{: shortdesc}

Consulte estas sugerencias de diseñadores de diálogos con una amplia experiencia.

## Planificación del diálogo general
{: #dialog-tips-plan}

- Planifique el diseño del diálogo que desea crear antes de añadir un nodo de diálogo individual en la herramienta. Si es necesario, haga un boceto en papel.
- Siempre que sea posible, base sus decisiones de diseño en datos de comportamientos reales. No añada nodos para manejar una situación que alguien *piense* que se podría producir.
- Evite copiar procesos empresariales tal como están. Rara vez son conversacionales.
- Si la gente ya utiliza un proceso, examine cómo lo enfocan. La gente suele optimizar el proceso desde una perspectiva conversacional.
- Decida el tono, la personalidad y el posicionamiento de su asistente. Refleje estas opciones de forma coherente en el diálogo que cree.
- No olvide que el asistente no es humano. Si los usuarios piensan que el asistente es una persona y luego descubren que no lo es, es probable que desconfíen de él.
- No todo tiene que ser una conversación. A veces un formato web funciona mejor.

## Adición de nodos
{: #dialog-tips-nodes}

- Añada un nombre de nodo que describa la finalidad del nodo.

  Ya sabe lo que hace el nodo en este momento, pero es posible que no lo sepa en unos meses. Usted mismo y los miembros del equipo le agradecerán en el futuro que haya añadido un nombre de nodo descriptivo. Y el nombre de nodo se muestra en el registro, lo que le puede ayudar a depurar una conversación más adelante.
- Para obtener la información necesaria para llevar a cabo una tarea, intente utilizar un nodo con ranuras en lugar de un grupo de nodos separados para recopilar información de los usuarios. Consulte [Obtención de información con ranuras](/docs/services/assistant?topic=assistant-dialog-slots).
- En el caso de un flujo de proceso complejo, indique a los usuarios la información que deben proporcionar al principio del proceso.
- Comprenda cómo avanza el servicio a través del árbol de diálogo y el impacto que tienen las carpetas, las ramas, los saltos y las digresiones sobre la ruta. Consulte [Flujo de diálogo](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-flow).
- No añada saltos en todas partes. Aumentan la complejidad del flujo de diálogo y dificultan la depuración del diálogo más adelante.
- Para saltar a un nodo que esté en la misma rama que el nodo actual, utilice *Saltar entrada de usuario* en lugar de *Ir a*.

  Esta opción evita que tenga que editar los valores del nodo actual cuando elimine o reordene los nodos hijo a los que se salta. Consulte [Definición de lo que hay que hacer a continuación](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-jump-to).
- Antes de habilitar las digresiones hacia fuera de un nodo, pruebe los casos de ejemplo de usuario más comunes. Y asegúrese de que los nodos a los que probablemente se desvíe estén configurados para volver. Consulte [Digresiones](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions).

## Adición de respuestas
{: #dialog-tips-responses}

- Cree respuestas cortas y útiles.
- Refleje la intención del usuario en la respuesta.

  Esto asegura a los usuarios que el bot les está entendiendo, o, si no es así, les ofrece la oportunidad de corregir un malentendido de inmediato.
- Incluya únicamente enlaces a sitios externos en las respuestas si la respuesta depende de datos que cambian con frecuencia.
- Evite el uso excesivo de botones. El hecho de animar a los usuarios a elegir opciones predefinidas entre un conjunto de botones se asemeja menos a una conversación real y disminuye su capacidad para aprender lo que los usuarios realmente desean hacer. Si deja que los usuarios reales soliciten cosas con sus propias palabras, se puede utilizar la entrada para entrenar el sistema y generar mejores intenciones.
- Evite utilizar un grupo de nodos cuando un nodo sea suficiente. Por ejemplo, añada varias respuestas condicionales a un solo nodo para que devuelva distintas respuestas en función de los detalles que especifique el usuario. Consulte [Respuestas condicionales](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multiple).
- Redacte las respuestas cuidadosamente. Puede cambiar la forma en que alguien reacciona ante su sistema en función simplemente de la forma en que redacta una respuesta. El hecho de modificar una línea de texto puede evitar que tenga que escribir varias líneas de código para implementar una solución programática compleja.
- Haga copias de seguridad frecuentes de su conocimiento. Consulte [Descarga de un conocimiento](/docs/services/assistant?topic=assistant-skill-add#skill-add-download).

## Sugerencias para capturar información de la entrada de usuario
{: #dialog-tips-user-input}

Puede resultar difícil saber la sintaxis que se utiliza en el nodo de diálogo para capturar con precisión la información que desea encontrar en la entrada del usuario. Estos son algunos enfoques que puede utilizar para abordar objetivos comunes.

- **Devolución de la entrada de usuario**: Puede capturar el texto exacto utilizado por el usuario y devolverlo en su respuesta. Utilice la siguiente expresión SpEL en una respuesta para repetir el texto que el usuario ha especificado en la respuesta:

  `You said: <? input.text ?>.`

- **Determinación del número de palabras en la entrada de usuario**: Puede utilizar cualquiera de los métodos String admitidos en el objeto input.text. Por ejemplo, puede averiguar cuántas palabras hay en una expresión de usuario, utilice la siguiente expresión SpEL:

  `input.text.split(' ').size()`

  Consulte [Métodos de lenguaje de expresión para String](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings) para obtener más información sobre los métodos que puede utilizar.

- **Manejo de varias intenciones**: Un usuario especifica una entrada que expresa el deseo de llevar a cabo dos tareas separadas. `I want to open a savings account and apply for a credit card (Quiero abrir una cuenta de ahorro y conseguir una tarjeta de crédito).` ¿Cómo reconoce y aborda el diálogo ambas intenciones? Consulte la entrada [Preguntas compuestas](https://sodoherty.ai/2017/02/06/compound-questions/){: new_window} en el blog de Simon O'Doherty para ver las estrategias que puede probar. (Simon es un desarrollador del equipo de {{site.data.keyword.conversationshort}}).

- **Manejo de intenciones ambiguas**: Una entrada de usuario expresa un deseo que es lo suficientemente ambiguo como para que el servicio encuentre dos o más nodos con intenciones que podrían darle respuesta. ¿Cómo sabe el diálogo qué rama de diálogo debe seguir? Si habilita la desambiguación, puede mostrar a los usuarios sus opciones y pedir al usuario que elija la correcta. Consulte [Desambiguación](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation) para obtener más información.

- **Manejo de varias entidades en la entrada**: Si solo desea evaluar el valor de la primera instancia detectada de un tipo de entidad, puede utilizar la sintaxis `@entidad == 'valor-específico'` en lugar del formato `@entidad:(valor-específico)`.

  Por ejemplo, si utiliza `@appliance == 'air conditioner'`, se evalúa solo el valor de la primera entidad `@appliance` detectada. Pero, si utiliza `@appliance:(air conditioner)`, se expande a `entity['appliance'].contains('air conditioner')`, que coincide siempre que se detecta al menos una entidad `@appliance` con el valor 'air conditioner' en la entrada de usuario.

## Sugerencias sobre el uso de condiciones
{: #dialog-tips-condition-usage}

- **Comprobación de valores con caracteres especiales**: Si desea comprobar si una variable de contexto o entidad contiene un valor, y el valor incluye un carácter especial, como un apóstrofo ('), debe delimitar con paréntesis el valor que desea comprobar. Por ejemplo, para comprobar si una variable de contexto o entidad contiene el nombre `O'Reilly`, debe delimitar el nombre con paréntesis.

  `@person:(O'Reilly)` y `$person:(O'Reilly)`

  El servicio convierte estas referencias en estas expresiones SpEL completas:

  `entities['person']?.contains('O''Reilly')` y `context['person'] == 'O''Reilly'`

  SpEL utiliza un segundo apóstrofo para codificar como escape el apóstrofo individual en el nombre.
  {: note}

- **Comprobación de varios valores**: SI desea comprobar más de un valor, puede crear una condición que utilice operadores OR (`||`) para obtener una lista de varios valores en la condición. Por ejemplo, para definir una condición que sea verdadera si la variable de contexto `$state` contiene las abreviaturas de Massachusetts, Maine o New Hampshire, puede utilizar esta expresión:

  `$state:MA || $state:ME || $state:NH`

- **Comprobación de valores de número**: Cuando se comparen números, primero asegúrese de que la entidad o variable que está comprobando tiene un valor. Si la entidad o variable no tiene un valor de número, se trata como si tuviera un valor nulo (0) en una comparación numérica.

  Por ejemplo, supongamos que desea comprobar si un valor de dólar que un usuario especificado en la entrada de usuario es inferior a 100. Si utiliza la condición `@price < 100`, y la entidad `@price` es nula, la condición se evalúa como `true` porque 0 es menor que 100, aunque nunca se haya definido el precio. Para evitar este tipo de resultado impreciso, utilice una condición como `@price AND @price < 100`. Si `@price` no tiene ningún valor, esta condición devuelve correctamente false.

- **Comprobación de intenciones con un patrón de nombre de intención específico**: Puede utilizar una condición que busca intenciones que coinciden con un patrón. Por ejemplo, para encontrar los nombres de las intenciones detectadas que empiezan por 'User_', puede utilizar una sintaxis como la siguiente en la condición:

  `intents[0].intent.startsWith("User_")`

  Sin embargo, cuando lo hace, todas las intenciones detectadas se consideran, incluso aquellas con una confianza inferior a 0,2. También se comprueba que no se devuelvan las intenciones que Watson considera irrelevantes con base a su confianza. Para ello, cambie la condición tal como se indica:

  `!irrelevant && intents[0].intent.startsWith("User_")`

- **Cómo la coincidencia aproximada afecta al reconocimiento de entidades**: Si utiliza una entidad como condición y la coincidencia aproximada está habilitada, `@nombre_entidad` se evalúa como true solo si la confianza de la coincidencia es mayor que 30%. Es decir, solo si `@nombre_entidad.confidence > .3`.

## Almacenamiento y reconocimiento de grupos de patrones de entidad en una entrada
{: #dialog-tips-get-pattern-groups}

Para almacenar el valor de una entidad de patrón en una variable de contexto, añada .literal al nombre de la entidad. Con esta sintaxis se asegura de que en la variable se almacena el texto exacto de la entrada del usuario que coincide con el patrón especificado.

| Variable   | Valor               |
|------------|---------------------|
| email      | <? @email.literal ?> |

Para almacenar el texto de un grupo individual en una entidad de patrón donde se hayan definido grupos, especifique el índice de la matriz del grupo que desea almacenar. Por ejemplo, supongamos que el patrón de entidad se define como tal como sigue para la entidad @phone_number. (Recuerde, los paréntesis denotan grupos de patrón):

`\b((958)|(555))-(\d{3})-(\d{4})\b`

Para almacenar solamente el código de área del número de teléfono que se especifica en la entrada de usuario, puede utilizar la sintaxis siguiente:

| Variable       | Valor                         |
|----------------|-------------------------------|
| area_code      | <? @phone_number.groups[1] ?> |

Los grupos se delimitan mediante la expresión regular que se utiliza para definir el patrón de grupo. Por ejemplo, si la entrada de usuario que coincide con el patrón definido en la entidad `@phone_number` es: `958-234-3456`, se crearían los siguientes grupos:

| Número de grupo | Valor de motor de expresión regular  | Valor de diálogo   | Explicación |
|--------------|---------------------|----------------|-------------|
| groups[0]    | `958-234-3456`      | `958-234-3456` | El primer grupo es siempre la serie coincidente completa. |
| groups[1]    | `((958)`l`(555))`   | `958`          | Serie que coincide con la expresión regular del primer grupo definido, que es `((958)`l`(555))`. |
| groups[2]    | `(958)`             | `958`          | Coincidencia con relación al grupo que se incluye como el primer operando en la expresión OR `((958)`l`(555))` |
| groups[3]    | `(555)`             | `null`         | Sin coincidencia con relación al grupo que se incluye como el segundo operando en la expresión OR `((958)`l`(555))` |
| groups[4]    | `(\d{3})`           | `234`          | Serie que coincide con la expresión regular que se ha definido para el grupo. |
| groups[5]    | `(\d{4})`           | `3456`         | Serie que coincide con la expresión regular que se ha definido para el grupo. |
{: caption="Detalles de grupo" caption-side="top"}

Para ayudarle a averiguar qué número de grupo se debe utilizar para capturar la sección de la entrada en la que está interesado, puede extraer información sobre todos los grupos al mismo tiempo. Utilice la sintaxis siguiente para crear una variable de contexto que devuelve una matriz con todas las coincidencias de entidad de patrón agrupadas:

| Variable                 | Valor                      |
|--------------------------|----------------------------|
| array_of_matched_groups  | <? @phone_number.groups ?> |

Utilice el panel "Pruébelo" para especificar algunos valores de prueba como número de teléfono. Para la entrada `958-123-2345`, esta expresión establece `$array_of_matched_groups` en `["958-123-2345","958","958",null,"123","2345"]`.

Puede contar entonces cada valor en la matriz, empezando desde 0, para obtener el número de grupo para la misma.

| Valor de elemento de matriz | Número de elemento de matriz |
|---------------------|----------------------|
| "958-123-2345"      | 0 |
| "958"               | 1 |
| "958"               | 2 |
| null                | 3 |
| "123"               | 4 |
| "2345"              | 5 |
{: caption="Elementos de matriz" caption-side="top"}

A partir del resultado puede determinar que, por ejemplo, para capturar los últimos cuatro dígitos del número de teléfono, necesita el grupo núm. 5.

Para devolver la estructura JSONArray que se ha creado para representar la entidad del patrón agrupado, utilice la sintaxis siguiente:

| Variable             | Valor                           |
|----------------------|---------------------------------|
| json_matched_groups  | <? @phone_number.groups_json ?> |

Esta expresión establece `$json_matched_groups` en la siguiente matriz JSON:

```json
[
  {"group": "group_0","location": [0, 12]},
  {"group": "group_1","location": [0, 3]},
  {"group": "group_2","location": [0, 3]},
  {"group": "group_3"},
  {"group": "group_4","location": [4, 7]},
  {"group": "group_5","location": [8, 12]}
]
```
{: codeblock}

`location` es una propiedad de una entidad que utiliza un desplazamiento de caracteres que empieza por cero para indicar dónde empieza y acaba el valor de la entidad detectada en el texto de entrada.
{: note}

Si espera recibir dos números de teléfono en la entrada, puede comprobar los dos números de teléfono. Si están presentes, utilice la siguiente sintaxis para capturar, por ejemplo, el código de área del segundo número.

| Variable         | Valor                                       |
|------------------|---------------------------------------------|
| second_areacode  | <? entities['phone_number'][1].groups[1] ?> |

Si la entrada es `I want to change my phone number from 958-234-3456 to 555-456-5678` (Deseo cambiar mi número de teléfono de 958-234-3456 a 555-456-5678), `$second_areacode` es `555`.

## Visualización de detalles de llamadas de API
{: #dialog-tips-inspect-api}

A medida que prueba el diálogo con el panel "Pruébelo", quizás desee saber cómo son las llamadas de API subyacentes que se devuelven desde el servicio. Puede utilizar las herramientas de desarrollador que proporciona el navegador web para inspeccionarlas.

Desde Chrome, por ejemplo, abra las herramientas de desarrollador. Pulse la herramienta Red. En la sección Nombre se muestran varias llamadas de API. Pulse la llamada de mensaje asociada con su expresión de prueba y luego pulse la columna Respuesta para ver el cuerpo de la respuesta de la API. Contiene las intenciones y las entidades que se han reconocido en la entrada del usuario con sus puntuaciones de confianza, y los valores de las variables de contexto en el momento de la llamada. Para ver el cuerpo de la respuesta en formato estructurado, haga clic en la columna Vista previa.

![Muestra cómo ver los detalles de las llamadas de API mediante herramientas del desarrollador del navegador web Chrome.](images/api-browser-dev.png)
