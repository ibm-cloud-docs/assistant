---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-09"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:table: .aria-labeledby="caption"}

# Visión general del diálogo
{: #dialog-overview}

El diálogo utiliza las intenciones y entidades que se identifican en la entrada del usuario, además del contexto de la aplicación, para interactuar con el usuario y finalmente proporcionar una respuesta útil.
{: shortdesc}

La respuesta puede ser la respuesta a una pregunta como `¿Dónde puedo poner gasolina?` o la ejecución de un mandato, como encender la radio. La intención y la entidad podrían ser suficiente información para determinar la respuesta correcta. Otra posibilidad es que el diálogo solicite más información para responder correctamente. Por ejemplo, si un usuario pregunta `¿Dónde puedo comprar comida?` es posible que desee aclarar si busca un restaurante o una tienda de comestibles, para cenar o para llevar, etc. Puede pedir más detalles en una respuesta de texto y crear uno o varios nodos hijo para procesar la nueva entrada.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/oQUpejt6d84?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

El diálogo se representa de forma gráfica en la herramienta de {{site.data.keyword.conversationshort}} como un árbol. Cree una rama para procesar cada intención que desea que maneje la conversación. Una rama se compone de varios nodos.

## Nodos del diálogo

Cada nodo del diálogo contiene, como mínimo, una condición y una respuesta.

![Muestra la entrada del usuario que va a parar a un recuadro que contiene la sentencia If: CONDITION, Then: RESPONSE](images/node1-empty.png)

- Condición: Especifica la información que debe aparecer en la entrada del usuario para este nodo en el diálogo que se va a activar. La información puede ser una intención específica, un valor de entidad o un valor de variable de contexto. Consulte [Condiciones](dialog-runtime.html#conditions) para obtener más información.
- Respuesta: La expresión que el servicio utiliza para responder al usuario. La respuesta también puede configurarse para que active acciones programáticas. Consulte [Respuestas](#responses) para obtener más información.

Puede pensar en el nodo como si tuviera una construcción de tipo si/entonces (if/then): si esta condición se cumple, entonces devolver esta respuesta.

Por ejemplo, el siguiente nodo se activa si la función de proceso de lenguaje natural del servicio determina que la entrada de usuario contiene la intención `#cupcake-menu`. Como consecuencia de que se active el nodo, el servicio responde con una respuesta adecuada.

![Muestra al usuario que pregunta sobre sabores de magdalenas. La condición If es #cupcake-menu y la respuesta Then es una lista de sabores de magdalenas.](images/node1-simple.png)

Un solo nodo con una condición y respuesta puede gestionar solicitudes sencillas de usuario. Pero generalmente los usuarios tienen preguntas más sofisticadas o desean ayuda con tareas más complejas. Puede añadir nodos hijo que soliciten al usuario que proporcione cualquier información adicional que necesita el servicio.

![Muestra que el primer nodo del diálogo pregunta qué tipo de magdalena desea el usuario, normal o sin gluten, y tiene dos nodos hijo que proporcionan distintas respuestas en función de la respuesta del usuario.](images/node1-children.png)

## Flujo del diálogo

El servicio procesa el diálogo que cree desde el primer nodo en el árbol hasta el último. 

![Las flechas apuntan hacia abajo a los 3 nodos para mostrar que el flujo de diálogo empieza en el primer nodo y acaba en el último](images/node-flow-down.png)

A medida que baja por el árbol, si el servicio encuentra una condición que se cumple, activa dicho nodo. Luego se mueve por el nodo que ha sido activado para comparar la entrada del usuario con las condiciones de los nodos hijo. A media que se comprueban los nodos hijos, pasa de nuevo desde el primer nodo hijo hasta el último. 

Los servicios siguen funcionando de esta forma a través del árbol del diálogo, desde el primer al último nodo, por cada nodo activado, y luego desde el primer al último nodo hijo, por cada nodo hijo activado hasta alcanzar el último nodo de la rama que se está siguiendo.  

![Muestra la flecha 1 apuntando desde el primer nodo raíz al último, la flecha 2 apuntando por todo el nodo desencadenado y la flecha 3 apuntando desde el primer al último de los nodos hijos del nodo activado. ](images/node-flow.png)

Cuando empiece a crear el diálogo, debe determinar las ramas para desea incluir y dónde colocarlas. El orden de las ramas es importante porque los nodos se evalúan de primero a último. Se utiliza el primer nodo raíz cuya condición coincida con la entrada; los nodos que hay posteriormente por árbol no se activan. 

Cuando el servicio alcanza el final de una rama, o cuando no puede encontrar una condición que se evalúe como verdadera desde el conjunto actual de nodos hijos que está evaluando, salta de nuevo a la base del árbol. Y una vez más, el servicio procesa los nodos raíz del primero al último. Si ninguna de las condiciones se evalúa como verdadera, se devuelve la respuesta del último nodo en el árbol, que normalmente tiene la condición especial `anything_else` que siempre se evalúa como verdadera. 

Existe la posibilidad de alterar el flujo estándar de primero a último personalizando lo que ocurre después de que se haya procesado un nodo. Por ejemplo, puede configurar un nodo para saltar directamente a otro nodo después de que se haya procesado, incluso si el otro nodo está situado con anterioridad en el árbol. Consulte [Definición de lo que hay que hacer a continuación](dialog-overview.html#jump-to) para obtener más detalles. 

La forma en que se configuran los valores de digresión para cada nodo también afectan a la forma en la que los usuarios se mueven en tiempo de ejecución a través de los nodos. Si habilita alejar digresiones de la mayoría de la mayoría de los nodos, los usuarios pueden saltar de un nodo a otro y volver de nuevo más fácilmente. Consulte [Digresiones](dialog-runtime.html#digressions) para obtener más información. 

## Condiciones
{: #conditions}

Una condición de nodo determina si el nodo se utiliza en la conversación. Las condiciones de respuesta determinan qué respuesta se debe mostrar al usuario.


- [Artefactos de condición](dialog-overview.html#condition-artifacts)
- [Detalles de sintaxis de condición](dialog-overview.html#condition-syntax)
- [Sugerencias sobre el uso de condiciones](dialog-overview.html#condition-tips)

### Artefactos de condición
{: #condition-artifacts}

Puede utilizar uno o varios de los siguientes artefactos en cualquier combinación para definir una condición:

- **Variable de contexto**: El nodo se utiliza si la expresión de la variable de contexto que especifique es verdadera. Utilice la sintaxis, `$nombre_variable:valor` o `$nombre_variable == 'valor'`. Por ejemplo, `$city:Boston` comprueba si la variable de contexto `$city` contiene el valor `Boston`. Si es así, se procesa el nodo o la respuesta. 

  No defina un nodo o condición de respuesta en función del valor de una variable de contexto en el mismo nodo del diálogo en el que ha establecido el valor de la variable de contexto.
  {: tip}

  Para obtener más información sobre las variables de contexto, consulte [Variables de contexto](dialog-runtime.html#context). 

- **Entidad**: El nodo se utiliza cuando cualquier valor o sinónimo de la entidad se reconoce en la entrada de usuario. Utilice la sintaxis, `@nombre_entidad`. Por ejemplo, `@city` comprueba si se detecta en la entrada de usuario alguno de los nombres de ciudad definidos para la entidad @city. Si es así, se procesa el nodo o la respuesta. 

  Asegúrese de crear un nodo igual para manejar los casos en los que no se reconoce ninguno de los valores o sinónimos de la entidad.
  {: tip}

  Para obtener más información sobre las entidades, consulte [Definición de entidades](entities.html).

- **Valor de entidad**: El nodo se utiliza si se detecta el valor de entidad en la entrada de usuario. Utilice la sintaxis, `@nombre_entidad:valor` y especifique un valor definido para la entidad, no un sinónimo. Por ejemplo, `@city:Boston` comprueba si se ha detectado en la entrada de usuario el nombre de ciudad específico `Boston`. 

  Si la entidad es una entidad de patrón con grupos de captura, puede comprobar la coincidencia con un determinado valor de grupo. Por ejemplo, puede utilizar la sintaxis: `@us_phone.groups[1] == '617'`. Consulte [Almacenamiento de valores de entidad de patrón en variables de contexto](dialog-runtime.html#context-pattern-entities) para obtener más información. 

  Si comprueba la presencia de la entidad, sin especificar un valor en particular para la misma, en un nodo de igual, asegúrese de colocar este nodo (que realiza la comprobación de un valor de entidad concreto) antes del nodo de igual que comprueba únicamente la presencia de la entidad. De lo contrario este nodo nunca se evaluará.
  {: tip}

- **Intención**: La condición más sencilla es una sola intención. El nodo se utiliza si la entrada del usuario se correlaciona con dicha intención. Utilice la sintaxis, `#nombre_intención`. Por ejemplo, `#weather` comprueba si la intención detectada en la entrada de usuario es `weather`. Si es así, el nodo se procesa.

  Para obtener más información sobre las intenciones, consulte [Definición de intenciones](intents.html).

- **Condición especial**: Condiciones que se proporcionan con el servicio y que puede utilizar para realizar funciones comunes de diálogo.

| Sintaxis de la condición| Descripción |
|----------------------|-------------|
| `anything_else`      | Puede utilizar esta condición al final de un diálogo para que se procese cuando la entrada de usuario no coincida con ningún otro nodo del diálogo. Esta condición activa el nodo **Anything else**.|
| `conversation_start` | Al igual que **welcome**, esta condición se evalúa como verdadera durante la primera ronda del diálogo. A diferencia de **welcome**, es verdadera tanto si la solicitud inicial de la aplicación contiene la entrada de usuario como si no. Se puede utilizar un nodo con la condición **conversation_start** para inicializar variables de contexto o para realizar otras tareas al principio del diálogo.|
| `false`              | Esta condición siempre se evalúa como falsa. Puede utilizarla al principio de una rama que esté en proceso de desarrollo para impedir que se utilice, o como condición para un nodo que proporciona una función común y solo se utiliza como destino de una acción **Ir a**.|
| `irrelevant`         | Esta condición se evaluará como verdadera si el servicio {{site.data.keyword.conversationshort}} determina que la entrada del usuario es irrelevante.|
| `true`               | Esta condición siempre se evalúa como verdadera. Puede utilizarla al final de una lista de nodos o respuestas para detectar las respuestas que no coincide con ninguna de las condiciones anteriores.|
| `welcome`            | Esta condición se evalúa como verdadera durante la primera vuelta del diálogo (cuando se inicia la conversación), solo si la solicitud inicial procedente de la aplicación no contiene ninguna entrada de usuario. Se evalúa como falsa en todas las demás rondas del diálogo. Esta condición activa el nodo **Welcome**. Normalmente se utiliza un nodo con esta condición para saludar al usuario, por ejemplo para mostrar un mensaje como `Welcome to our Pizza ordering app.` (Bienvenido a nuestra app para pedir pizzas.). |
{: caption="Condiciones especiales" caption-side="top"}

### Detalles de sintaxis de condición
{: #condition-syntax}

Utilice una de estas opciones sintaxis para crear expresiones válidas en condiciones:

- Claves de notaciones para hacer referencia a intenciones, entidades y variables de contexto. Consulte [Acceso y evaluación de objetos](expression-language.html).

- Lenguaje Spring Expression (SpEL), que es un lenguaje de expresión que da soporte a la consulta y manipulación de un gráfico de objetos en el momento de la ejecución. Consulte [Lenguaje Spring Expression Language (SpEL) ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window} para obtener más información.

Utilice expresiones regulares para comprobar los valores con los que comparar la condición.  Para encontrar una serie coincidente, por ejemplo, puede utilizar el método `String.find`. Consulte [Métodos](dialog-methods.html) para obtener más detalles.

### Sugerencias sobre el uso de condiciones
{: #condition-tips}

- **Comprobación de valores con caracteres especiales**: Si desea comprobar si una variable de contexto o entidad contiene un valor, y el valor incluye un carácter especial, como un apóstrofo ('), debe delimitar con paréntesis el valor que desea comprobar. Por ejemplo, para comprobar si una variable de contexto o entidad contiene el nombre `O'Reilly`, debe delimitar el nombre con paréntesis. 

  `@person:(O'Reilly)` y `$person:(O'Reilly)`

  El servicio convierte estas referencias en estas expresiones SpEL completas: 

  `entities['person']?.contains('O''Reilly')` y `context['person'] == 'O''Reilly'`

  **Nota**: SpEL utilizan un segundo apóstrofo para codificar como escape el apóstrofo individual en el nombre. 

- **Comprobación de valores numéricos**: Cuando utilice variables numéricas, asegúrese de que las variables tengan valores. Si una variable no tiene un valor, se trata como si tuviera un valor nulo (0) en una comparación numérica. 

  Por ejemplo, si comprueba el valor de una variable con la condición `@price < 100`, y la entidad @price es nula, la condición se evalúa como `true` porque 0 es menor que 100, aunque nunca se haya definido el precio. Para evitar la comprobación de variables nulas, utilice una condición como `@price AND @price < 100`. Si `@price` no tiene ningún valor, esta condición devuelve correctamente false.

- **Comprobación de intenciones con un patrón de nombre de intención específico**: Puede utilizar una condición que busca intenciones que coinciden con un patrón. Por ejemplo, para encontrar los nombres de las intenciones detectadas que empiezan por 'User_', puede utilizar una sintaxis como la siguiente en la condición: 

  `intents[0].intent.startsWith("User_")`

  Sin embargo, cuando lo hace, todas las intenciones detectadas se consideran, incluso aquellas con una confianza inferior a 0,2. También se comprueba que no se devuelvan las intenciones que Watson considera irrelevantes con base a su confianza. Para ello, cambie la condición tal como se indica: 

  `!irrelevant && intents[0].intent.startsWith("User_")`

- **Como la coincidencia aproximada afecta al reconocimiento de entidades**: Si utiliza una entidad como condición y la coincidencia aproximada está habilitada, `@nombre_entidad` se evalúa como true solo si la confianza de la coincidencia es mayor que 30%. Es decir, solo si `@nombre_entidad.confidence > .3`. 

- **Manejo de varias entidades en la entrada**: Si solo desea evaluar el valor de la primera instancia detectada de un tipo de entidad, puede utilizar la sintaxis `@entidad == 'valor-específico'` en lugar del formato `@entidad:(valor-específico)`. 

  Por ejemplo, si utiliza `@appliance == 'air conditioner'`, se evalúa solo el valor de la primera entidad `@appliance` detectada. Pero, si utiliza `@appliance:(air conditioner)`, se expande a `entity['appliance'].contains('air conditioner')`, que coincide siempre que se detecta al menos una entidad `@appliance` con el valor 'air conditioner' en la entrada de usuario.

## Respuestas
{: #responses}

La respuesta del diálogo define cómo responder al usuario.

Puede responder con uno de estos tipos de respuesta:

- [Respuesta de texto simple](#simple-text)
- [Respuestas condicionales](#multiple)
- [Respuesta compleja](#complex)
- [Respuesta multimedia](#multimedia)

### Respuesta de texto simple
{: #simple-text}

Si desea proporcionar una respuesta de texto, simplemente especifique el texto que desea que el servicio muestre al usuario.

![Muestra un nodo que muestra una pregunta de usuario, Where are you located (Dónde está ubicado), y la respuesta del diálogo es, We have no brick and mortar stores! But, with an internet connection, you can shop us from anywhere. (¡No tenemos tiendas físicas! Pero con una conexión a internet, puede comprar en nosotros desde cualquier lugar). ](images/response-simple.png)

Si se incluye una dirección de correo electrónico en la respuesta, debe codificar como escape el símbolo (`@`) con una barra inclinada invertida (`\`).  Por ejemplo, `Send us your feedback at feedback\@example.com.` (Envíenos sus comentarios a feedback\@example.com). De forma similar, si incluye un signo numérico (`#`) en la respuesta, también lo debe codificar como escape. Por ejemplo, `We are the \#1 seller of lobster rolls in Maine.` (Somos los vendedores número 1 de sándwiches de langosta en Maine). Los nombres de entidad empiezan con `@` y los nombres de intención con `#`. Codificar con escape estos símbolos permitirá que el servicio los lea correctamente en el texto de las respuestas.
{: tip}

#### Adición de una variedad
{: #variety}

Si los usuarios vuelven con frecuencia a su servicio de conversación, es posible que se cansen de escuchar siempre el mismo saludo y las mismas respuestas.  Puede añadir *variaciones* a las respuestas para que la conversación puede responder a la misma condición de diferentes maneras.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/nAlIW3YPrAs?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

En este ejemplo, la respuesta que proporciona el servicio en respuesta a las preguntas sobre las ubicaciones de las tiendas difiere entre una interacción y la siguiente:

![Muestra un nodo que muestra una pregunta de usuario, Where are you located (Dónde está ubicado), y el diálogo tiene definidas tres respuestas diferentes.](images/variety.png)

Puede optar por rotar entre las variaciones de respuestas secuencialmente o en orden aleatorio. De forma predeterminada, las respuestas se rotan secuencialmente, como si fueran seleccionadas en una lista ordenada.

### Respuestas condicionales
{: #multiple}

Un solo nodo de diálogo puede proporcionar distintas respuestas, cada uno activada por una condición diferente.  Utilice este enfoque para abordar varios escenarios en un solo nodo.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/KcvVQAsnhLM?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

El nodo sigue teniendo una condición principal, que es la condición para utilizar el nodo y procesar las condiciones y respuestas que contiene.

En este ejemplo, el servicio utiliza la información recopilada anteriormente sobre la ubicación del usuario para adaptar su respuesta y proporcionar información sobre la tienda más cercana al usuario. Consulte [Variables de contexto](dialog-runtime.html#context) para obtener más información sobre cómo almacenar la información recopilada por el usuario.

![Muestra un nodo que muestra una pregunta de usuario, Where are you located (Dónde está ubicado), y el diálogo tiene tres respuestas distintas dependiendo de las condiciones que utilizan información de la variable de contexto $state para especificar ubicaciones en estos estados.](images/multiple-responses.png)

Este nodo único ahora proporciona la función equivalente de cuatro nodos separados.

Para añadir respuestas condicionales a un nodo, pulse **Personalizar** y, a continuación, pulse **Varias respuestas** para **activarlo**. 

Las condiciones de un nodo se evalúan en orden, como lo hacen los nodos.  Asegúrese de que sus condiciones y las respuestas aparecen listadas en el orden correcto. Si necesita cambiar el orden, seleccione una condición y súbala o bájela en la lista utilizando las flechas que se visualizan. Si desea actualizar el contexto, debe hacerlo en el editor JSON para cada respuesta individual. No hay un editor JSON común para todas las respuestas. Si asoció una acción **Ir a** con el nodo, el salto se produce una vez se hayan procesado las respuestas.
{: tip}

### Una respuesta compleja
{: #complex}

Para especificar una respuesta más compleja, puede utilizar el editor de JSON para especificar la respuesta en la propiedad `"output":{}`.

Para incluir un valor de variable de contexto en la respuesta, utilice la sintaxis `$variable-name` para especificarlo. Consulte [Variables de contexto](dialog-runtime.html#context) para obtener más información.

```json
{
  "output": {
    "text": "Hello $user"
  }
}
```
{: codeblock}

Para especificar más de una sentencia que desea visualizar en líneas separadas, defina la salida como una matriz JSON.

```json
{
  "output": {
    "text": ["Hello there.", "How are you?"]
  }
}
```
{: codeblock}

La primera frase se muestra en una línea, y la segunda frase se muestra como una línea nueva después de la anterior.

Para implementar un comportamiento más complejo, puede definir el texto de salida como un objeto JSON complejo. Por ejemplo, puede utilizar un objeto complejo en la salida de JSON para imitar el comportamiento de añadir variaciones respuesta al nodo. Puede incluir las siguientes propiedades en el objeto complejo:

- **values**: Una matriz JSON de series que contiene varias versiones de texto de salida que puede devolver este nodo de diálogo. El orden en el que se devuelven los valores de la matriz depende del atributo `selection_policy`.

- **selection_policy**: Se aceptan los siguientes valores:

    - **ramdom**: El sistema selecciona aleatoriamente texto de salida de la matriz `values` y no los repite consecutivamente. Por ejemplo, supongamos que output.text contiene tres valores. Las tres primeras veces se selecciona un valor aleatorio, pero no se repite otra vez. Una vez suministrados todos los valores de salida, el sistema selecciona aleatoriamente otro valor y repite el proceso.

        ```json
        {
            "output":{
                "text":{
                    "values":["Hello.","Hi.","Howdy!"],
                    "selection_policy":"random"
                }
            }
        }
        ```
        {: codeblock}

    El sistema devuelve un saludo de entre estas tres opciones, que escoge al azar. La siguiente vez que se activa la respuesta, se muestra otro saludo de la lista. El saludo se elige de nuevo de manera aleatoria, excepto en que el saludo utilizado anteriormente no se repite intencionadamente.

    - **sequential**: El sistema devuelve el primer texto de salida la primera vez que se activa el nodo del diálogo, el segundo texto de salida la segunda vez que se activa el nodo, y así sucesivamente.

        ```json
        {
            "output":{
                "text":{
                    "values":["Hello.", "Hi.", "Howdy!"],
                    "selection_policy":"sequential"
                }
            }
        }
        ```
        {: codeblock}

- **append**: Especifica si se debe añadir un valor a una matriz o se deben sobrescribir los valores de la matriz con el nuevo valor o valores. Cuando se establece en false, la salida recopilada en los nodos de diálogo ejecutados anteriormente se sobrescribe con el valor de texto especificado en este nodo particular.

    ```json
    {
        "output":{
            "text":{
                "values": ["Hello."],
                "append":false
            }
        }
    }
    ```
    {: codeblock}

    En este caso, el resto del texto de salida se sobrescribe con este texto de salida.

El comportamiento predeterminado es aceptar `selection_policy = random` y `append = true`. Cuando la matriz de valores contiene más de un elemento, el texto de salida se selecciona aleatoriamente entre sus elementos.

### Respuesta multimedia
{: #multimedia}

Si tiene previsto integrar el diálogo con Slack o Facebook Messenger utilizando el conector de {{site.data.keyword.conversationshort}}, puede especificar respuestas de nodo del diálogo que incluyan elementos interactivos o multimedia como, por ejemplo, botones que es posible pulsar. 

Consulte [Respuestas multimedia](dialog-multimedia.html) para obtener más información. 

## Definición de lo que hay que hacer a continuación
{: #jump-to}

Después de ofrecer la respuesta especificada, puede indicar al servicio para haga una de estas cosas:

- **Esperar una entrada de usuario**: El servicio espera a que el usuario especifique una nueva entrada que obtiene la respuesta. Por ejemplo, la respuesta puede realizar al usuario una pregunta de tipo sí o no. El diálogo no avanzará hasta que el usuario proporcione más información.
- **Saltar entrada de usuario**: Utilice esta opción cuando desee omitir la espera de la entrada de usuario y en su lugar desee ir directamente al primer nodo hijo del nodo actual. 

  **Nota**: Para que esta opción esté disponible, el nodo actual debe tener al menos un nodo hijo. 

- **Saltar a otro nodo del diálogo**: Utilice esta opción cuando desee omitir la espera de información del usuario y desee que la conversación vaya directamente a un nodo de diálogo totalmente diferente. Puede utilizar una acción *Ir a* para dirigir el flujo a un nodo de diálogo común, por ejemplo desde varias ubicaciones del árbol. 

  **Nota**: El nodo de destino al que desea ir debe existir para configurar la opción Ir a para que lo utilice. 

### Configuración de la acción Ir a
{: #jump-to-config}

Si elige ir a otro nodo, debe especificar si el destino de la acción es la **respuesta** o la **condición** del nodo de diálogo seleccionado. 

- **Respuesta**: Si el destino de la sentencia es la sección de la respuesta del nodo de diálogo seleccionado, se ejecuta inmediatamente. Esto es, el sistema no evalúa la condición del nodo de diálogo seleccionado sino que procesa inmediatamente la respuesta del nodo de diálogo seleccionado. 

  Elegir la respuesta como destino resulta útil para encadenar varios nodos del diálogo. La respuesta se procesa como si la condición de este nodo de diálogo fuera verdadera. Si el nodo del diálogo seleccionado tiene otra acción **Ir a**, dicha acción también se ejecuta inmediatamente.

- **Condición**: Si el destino de la sentencia es la sección de la condición del nodo de diálogo seleccionado, el servicio comprueba primero si la condición del nodo de destino se evalúa como verdadera. 
    - Si la condición se evalúa como verdadera, el sistema procesa de forma inmediata el nodo de destino. 
    - Si la condición no se evalúa como verdadera, el sistema pasa al siguiente nodo hermano del nodo de destino para evaluar su condición, y repite este proceso hasta que encuentra un nodo de diálogo con una condición que se evalúe como verdadera. 
    - Si el sistema procesa todos los hermanos y ninguna de las condiciones se evalúa como verdadera, se utiliza la estrategia básica de volver atrás, de forma que el diálogo evalúa los nodos a nivel base del árbol del diálogo. 

    Elegir la condición como destino resulta útil para encadenar las condiciones de los nodos del diálogo. Por ejemplo, supongamos que desea comprobar primero si la entrada contiene una intención, como por ejemplo `#turn_on`, y, si es así, es posible que desee comprobar si la entrada contiene entidades, como `@lights`, `@radio` o `@wipers`. El encadenamiento de condiciones ayuda a estructurar los árboles del diálogo.

## Más información

Para obtener información sobre el lenguaje de expresión que utiliza el diálogo, más métodos, entidades del sistema y otros detalles útiles, consulte la sección de **Referencia** del panel de navegación. 
