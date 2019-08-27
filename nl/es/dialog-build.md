---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-29"

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
{:table: .aria-labeledby="caption"}

# Creación de un diálogo
{: #dialog-build}

El diálogo define lo que dice su asistente en respuesta a los clientes, en base a lo que cree que quiere el cliente.
{: shortdesc}

## Creación de un diálogo
{: #dialog-build-task}

Para crear un diálogo, siga estos pasos:

1.  Pulse en el separador **Diálogo** y luego en **Crear diálogo**.

    Cuando se abra el editor de diálogos por primera vez, se crean automáticamente los nodos siguientes:

    - **Welcome**: El primer nodo. Contiene un saludo que se muestra a los usuarios la primera vez que interactúan con su asistente. Puede editar el mensaje.

    Este nodo no se desencadena en los flujos de diálogo que inician los usuarios. Por ejemplo, los diálogos que se utilizan en las integraciones con canales como Facebook o Slack se saltan los nodos con la condición especial `welcome`. Consulte [Inicialización de un diálogo](/docs/services/assistant?topic=assistant-dialog-start) para obtener más información.
    {: note}

    - **Anything else**: El nodo final. Contiene frases que se utilizan para responder a los usuarios cuando no se reconoce la información que especifican. Puede sustituir las respuestas que se proporcionan o añadir más respuestas con un significado similar a añadir variedad a la conversación. También puede elegir si desea que su asistente para devuelva cada respuesta definida por orden o si desea devolverlas en orden aleatorio.
1.  Para añadir más nodos al árbol de diálogo, pulse **Más** ![icono Más](images/kabob.png) en el nodo **Welcome** y seleccione **Añadir nodo debajo**.
1.  En el campo **Si el asistente reconoce**, especifique una condición que, cuando se cumpla, hace que su asistente procese el nodo. 

    Para empezar, normalmente habrá que añadir una intención como la condición. Por ejemplo, si añade `#open_account` aquí, quiere decir que quiere que se devuelva al usuario la respuesta que especificará en este nodo, si la entrada del usuario indica que éste quiere abrir una cuenta.

    Cuando empieza a definir una condición, se muestra un recuadro que muestra sus opciones. Puede especificar uno de los siguientes caracteres, y luego seleccionar un valor de la lista de opciones que se visualiza.

    <table>
    <caption>Sintaxis del generador de condiciones</caption>
    <tr>
      <th>Carácter</th>
      <th>Muestra una lista de los valores definidos para estos tipos de artefacto</th>
    </tr>
    <tr>
      <td>`#`</td>
      <td>intenciones</td>
    </tr>
    <tr>
      <td>`@`</td>
      <td>entidades</td>
    </tr>
    <tr>
      <td>`@{entity-name}:`</td>
      <td>valores de {entity-name}</td>
    </tr>
    <tr>
      <td>`$`</td>
      <td>Variables de contexto que ha definido o a las que ha hecho referencia en algún otro lugar del diálogo</td>
    </tr>
    </table>

    Puede crear una nueva intención, entidad, valor de entidad o variables de contexto definiendo una nueva condición que lo utilice. Si crea un artefacto de este modo, asegúrese de retroceder y completar cualquier otro paso necesario para crear el artefacto, como por ejemplo definir expresiones de ejemplo para una intención.

    Para definir un nodo que se active en función de más de una condición, especifique una condición y pulse el icono de signo más (+) situado junto a la misma. Si desea aplicar un operador `OR` a varias condiciones en lugar de `AND`, pulse `and` que se visualiza entre los campos para cambiar el tipo de operador. Las operaciones AND se ejecutan antes que las operaciones OR, pero puede cambiar el orden utilizando paréntesis. Por ejemplo:
    `$isMember:true AND ($memberlevel:silver OR $memberlevel:gold)`

    La condición que defina debe tener menos de 2.048 caracteres de longitud.

    Para obtener más información sobre cómo probar valores en condiciones, consulte [Condiciones](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-conditions).
1.  **Opcional**: Si desea recopilar varios fragmentos información del usuario en este nodo, pulse **Personalizar** y habilite **Ranuras**. Consulte [Obtención de información con ranuras](/docs/services/assistant?topic=assistant-dialog-slots) para ver más detalles.
1.  Escriba una respuesta.
    - Añada el texto o los elementos multimedia que desea que su asistente muestre al usuario como respuesta.
    - Si desea definir diferentes respuestas basándose en determinadas condiciones, pulse **Personalizar** y habilite **Varias respuestas**.
    - Para obtener información sobre respuestas condicionales, sobre respuestas completas o sobre cómo añadir distintas respuestas, consulte [Respuestas](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses).

1.  Especifique qué hacer después de que se haya procesado el nodo actual. Elija entre las siguientes opciones:

    - **Esperar una entrada de usuario**: Se detiene su asistente hasta que el usuario proporcione una nueva entrada.
    - **Saltar entrada de usuario**: Su asistente salta directamente al primer nodo hijo. Esta opción únicamente está disponible si el nodo actual tiene al menos un nodo hijo.
    - **Ir a**: Su asistente sigue el proceso de diálogo en el nodo que especifique. Puede elegir si su asistente debe evaluar la condición del nodo de destino o ir directamente a la respuesta del nodo de destino. Consulte la [Configuración de la acción Ir a](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-jump-to-config) para obtener más detalles.

1.  **Opcional**: si desea que este nodo se tenga en cuenta cuando se muestre a los usuarios un conjunto de opciones de nodo en el momento de la ejecución y si les solicite que elijan la que mejor se ajuste a su objetivo, añada una breve descripción del objetivo de usuario que gestiona este nodo en el campo **nombre de nodo externo**. Por ejemplo, *Abrir una cuenta*.

    ![Solo plan Plus o Premium](images/plus.png) el campo *nombre de nodo externo* solo se muestra a los usuarios del plan Plus o Premium. Consulte [Desambiguación](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation) para obtener más información.

1.  **Opcional**: Asigne un nombre al nodo.

    El nombre del nodo del diálogo puede contener letras (en Unicode), números, espacios, signos de subrayado, guiones y puntos.

    La asignación de un nombre al nodo le permite recordar su finalidad y localizar el nodo cuando está minimizado. Si no especifica ningún nombre, se utiliza la condición como nombre.

1.  Para añadir más nodos, seleccione un nodo del árbol y pulse el icono **Más** ![icono Más](images/kabob.png).
    - Para crear un nodo igual que se compruebe a continuación si no se cumple la condición correspondiente al nodo existente, seleccione **Añadir nodo debajo**.
    - Para crear un nodo igual que se compruebe antes que la condición correspondiente al nodo existente, seleccione **Añadir nodo encima**.
    - Para crear un nodo hijo del nodo seleccionado, seleccione **Añadir nodo hijo**. Un nodo hijo se procesa después que su nodo padre.
    - Para copiar el nodo actual, seleccione **Duplicar**.

    Para obtener más información sobre el orden en que se procesan los nodos del diálogo, consulte [Visión general del diálogo](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-flow).
1.  Pruebe el diálogo a medida que lo crea.
   Consulte [Prueba del diálogo](#dialog-build-test) para obtener más información.

## Prueba del diálogo
{: #dialog-build-test}

A medida que realiza cambios en el diálogo, puede probarlo en cualquier momento para ver cómo responde ante una entrada.

Las consultas que envíe a través del panel "Pruébelo" generan llamadas de API `/message`, pero no se registran y no incurren en cargos.

1.  En el separador Diálogo, pulse el icono ![Pruébelo](images/ask_watson.png).
1.  En el panel de la conversación, escriba el texto que desee y pulse Intro.

    Asegúrese de que el sistema haya terminado de formarse con los cambios más recientes antes de empezar a probar el diálogo. Si el sistema continúa entrenándose, se visualizará un mensaje en el panel *Pruébelo*:
    {: tip}

    ![Captura de pantalla del mensaje de entrenamiento](images/training.png)
1.  Compruebe la respuesta para ver si el diálogo ha interpretado correctamente la entrada y ha elegido la respuesta apropiada.

    La ventana de la conversación indica las intenciones y entidades que se han reconocido en la entrada.

    ![Captura de pantalla de la salida del diálogo de prueba](images/test_dialog_output.png)

    Si desea editar una entidad que se reconoce de la entrada, pulse el nombre de la entidad para abrirla en la página Entidades. Si se reconoce el intento incorrecto, puede pulsar en la flecha situada junto al nombre de intento para corregirla o marcar el tema como irrelevante. Para obtener más información, consulte
[Cómo realizar mejora en los datos de entrenamiento](/docs/services/assistant?topic=assistant-logs#logs-fix-data).

1.  Si desea saber qué nodo en el árbol de diálogo desencadenó una respuesta, pulse el icono **Ubicación** ![Ubicación](images/location.png) junto a la misma. Si todavía no está en el separador Diálogo, ábralo.

    Se le da el foco al nodo de origen y se resalta la ruta que su asistente ha recorrido a través del árbol para llegar al final del mismo. La ruta permanece resaltada hasta que lleve a cabo otra acción como, por ejemplo, especificar una nueva entrada de prueba.

1.  Para comprobar o establecer el valor de una variable de contexto, pulse el enlace **Gestionar contexto**.

    Se muestran las variables de contexto que ha definido en el diálogo.

    Además, se muestra una variable de contexto `$timezone`. La interfaz de usuario del panel *Pruébelo* obtiene la información sobre el entorno local del usuario del navegador web y la utiliza para establecer la variable de contexto `$timezone`. Esta variable de contexto facilita el manejo de las referencias de tiempo en los intercambios del diálogo de prueba. Considere la posibilidad de hacer algo similar en la aplicación de usuario. Si no se especifica, se utiliza la hora media de Greenwich (GMT).

    Puede añadir una variable y establecer su valor para ver cómo responde el diálogo en la siguiente ronda del diálogo de prueba. Esta función es útil si, por ejemplo, el diálogo está configurado para mostrar diferentes respuestas en función de un valor de variable de contexto que proporciona el usuario.

    1.  Para añadir una variable de contexto, especifique el nombre de la variable y pulse **Intro**.
    1.  Para definir un valor predeterminado para la variable de contexto, busque la variable de contexto que ha añadido en la lista y especifique un valor para la misma.

    Consulte [Variables de contexto](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context) para obtener más información.

1.  Continúe interactuando con el diálogo para ver cómo fluye la conversación.
    - Para encontrar y volver a enviar una expresión de prueba, puede pulsar la tecla Subir para recorrer las entradas recientes.
    - Para eliminar expresiones de prueba anteriores del panel de la conversación y volver a empezar, pulse el enlace **Borrar**. No solo se eliminan las expresiones de prueba y las respuestas; esta acción también elimina los valores de las variables de contexto establecidas como resultado de sus interacciones con el diálogo.

### Qué hacer a continuación
{: #dialog-build-next-steps}

Si determina que se están reconociendo intenciones o entidades erróneas, es posible que tenga que modificar las definiciones de las intenciones o entidades.

Si se están reconociendo las intenciones y entidades correctas, pero se activan los nodos erróneos en el diálogo, asegúrese de que las condiciones se han escrito adecuadamente.

Consulte los [Consejos para crear diálogos](/docs/services/assistant?topic=assistant-dialog-tips) para ver sugerencias que pueden ayudarle a empezar a crear diálogos.

Si está listo para poner la conversación en funcionamiento para ayudar a los usuarios, integre su asistente con una plataforma de mensajería o una aplicación personalizada. Consulte [Adición de integraciones](/docs/services/assistant?topic=assistant-deploy-integration-add).

## Límites del nodo del diálogo
{: #dialog-build-node-limits}

El número de nodos de diálogo que puede crear por conocimiento depende de su tipo de plan.

| Plan     | Nodos de diálogo por conocimiento     |
|------------------|---------------------------:|
| Premium          |                    100.000 |
| Plus             |                    100.000 |
| Estándar         |                    100.000 |
| Plus Trial       |                     25.000 |
| Lite             |                     100`*` |
{: caption="Detalles del plan" caption-side="top"}

Los nodos de diálogo welcome y anything_else que ya contienen información en el árbol no cuentan al calcular el total.

Límite de profundidad de árbol: el diálogo admite 2000 nodos de diálogo descendientes; el diálogo funciona mejor con 20 o menos.

`*` Los límites han pasado de 25.000 a 100 para los planes Lite el 1 de diciembre de 2018. Los usuarios de instancias de servicio que se han creado antes de la modificación del límite tienen hasta el 1 de junio de 2019 para actualizar su plan o para editar los diálogos en los conocimientos de las instancias de servicio existentes para que se ajusten a los nuevos requisitos de límite.

Para ver el número de nodos de diálogo en un conocimiento de diálogo, realice una de las siguientes acciones:

- Si aún no está asociado con un asistente, añada el conocimiento de diálogo a un asistente y, a continuación, visualice el mosaico de conocimientos en la página principal del asistente. La sección *datos formados* muestra el número de nodos de diálogo.
- Envíe una solicitud GET al punto final de la API /dialog_nodes API e incluya el parámetro `include_count=true`. Por ejemplo:

  ```curl
  curl -u "apikey:{apikey}" "https://gateway.watsonplatform.net/assistant/api/v1/workspaces/{workspace_id}/dialog_nodes?version=2018-09-20&include_count=true"
  ```

  En la respuesta, el atributo `total` del objeto `pagination` contiene el número de nodos de diálogo.

Si el total parece mayor de lo que esperaba, podría deberse a que el diálogo que crea desde la aplicación se convierte en un objeto JSON. Algunos campos que parecen ser parte de un nodo individual están realmente estructurados como nodos de diálogo separados en el objeto JSON subyacente.

  - Cada nodo y carpeta se representa como su propio nodo.
  - Cada respuesta condicional que está asociada con un solo nodo de diálogo se representa como un nodo individual.
  - Para un nodo con ranuras, cada ranura, respuesta de ranura encontrada, respuesta de ranura no encontrada, manejador de ranura y, si está definida, la respuesta "solicitar todo" es un nodo individual. De hecho, un nodo con tres ranuras puede equivaler a once nodos de diálogo.

## Búsqueda en diálogos
{: #dialog-build-search}

Puede buscar en el diálogo para encontrar uno o varios nodos de diálogo que mencionen una palabra o frase determinada.

1.  Seleccione el icono de búsqueda: ![Icono de búsqueda](images/search_icon.png).

1.  Especifique una frase o término de búsqueda.

    La primera vez que efectúa la búsqueda, se crea un índice. Es posible que se le pida que espere mientras se indexa el texto de los nodos de diálogo.
    {: note}

Se mostrarán los nodos que contienen su término de búsqueda, con los correspondientes ejemplos. Seleccione un resultado para editarlo.

  ![Devolución de búsqueda de intenciones](images/search_dialog.png)

## Búsqueda de un nodo del diálogo por su ID de nodo
{: #dialog-build-get-node-id}

Puede buscar un nodo de diálogo por su ID de nodo. Especifique el ID de nodo completo en el campo de búsqueda. Es posible que desee buscar el nodo del diálogo que está asociado a un ID de nodo conocido por uno de estos motivos:

- Está revisando registros y el registro hace referencia a una sección del diálogo por su ID de nodo.
- Desea correlacionar los ID de nodo que aparecen en la lista de la propiedad `nodes_visited` de la salida del mensaje de la API con nodos que pueda ver en el árbol del diálogo.
- Un mensaje de error en tiempo de ejecución del diálogo le informa sobre un error de sintaxis y utiliza un ID de nodo para identificar el nodo que tiene que corregir.

Otra forma de descubrir un nodo en función de su ID de nodo es mediante estos pasos:

1.  En el separador Diálogo, seleccione cualquier nodo del árbol del diálogo.
1.  Cierre la vista de edición si está abierta para el nodo actual.
1.  En el campo de ubicación del navegador web, debería mostrarse un URL con la siguiente sintaxis:

    `https://assistant-location.watsonplatform.net/location/instance-id/workspaces/workspace-id/build/dialog#node=node-id`

1.  Edite el URL y sustituya el valor `node-id` actual por el ID del nodo que desea encontrar y envíe el nuevo URL.
1.  Si es necesario, vuelva a resaltar el URL editado y envíelo de nuevo.

La página se renueva y el foco pasa a estar en el nodo del diálogo con el ID de nodo que ha especificado. Si el ID de nodo es para una ranura, una condición de ranura encontrada o no encontrada, un manejador de ranura o una respuesta condicional, entonces el nodo en que se definió la respuesta condicional o la ranura recibe el foco y se visualiza la correspondiente sección modal.

Si sigue sin poder encontrar el nodo, exporte el conocimiento del diálogo y utilice el editor JSON para buscar el archivo JSON del conocimiento.
{: tip}

## Copia de un nodo de diálogo
{: #dialog-build-copy-node}

Puede duplicar un nodo para crear una copia exacta del mismo como un nodo de igual directamente debajo de dicho nodo en el árbol de diálogo. Al nodo copiado se le da el mismo nombre que el nombre del nodo original, añadiendo `- copy`*`n`* al nombre, donde *`n`* es un número que empieza por 1. Si duplica el mismo nodo más de una vez, el valor de *`n`* en el nombre se incrementa en una unidad en cada copia para ayudarle a distinguir a las copias entre sí. Si el nodo no tiene un nombre, se da el nombre `copy`*`n`*.

Cuando se duplica un nodo que tiene nodos hijo, los nodos hijo también se duplican. Los nodos hijo copiados tienen exactamente los mismos nombres que los nodos hijo originales. La única forma de distinguir un nodo hijo copiado desde un nodo hijo original es la referencia `copy` en el nombre de nodo padre.

1.  En el nodo que desea copiar, pulse el icono **Más** ![icono Más](images/kabob.png) y luego seleccione **Duplicar**.
1.  Considere cambiar los nodos copiados o editar sus condiciones para hacerlos distintos.

## Cómo mover un nodo del diálogo
{: #dialog-build-move-node}

Cada nodo que cree se puede moverse en el árbol del diálogo.

Es posible que desee mover un nodo creado anteriormente a otra área del flujo para cambiar la conversación. Puede mover nodos para convertirlos en hermanos o iguales en otra rama.

1.  En el nodo que desea mover, pulse el icono **Más** ![icono Más](images/kabob.png) y luego seleccione **Mover**.
1.  Seleccione un nodo de destino que se encuentre en el árbol próximo a donde desea mover este nodo. Elija si desea colocar este nodo antes o después del nodo de destino, o si desea convertirlo en hijo del nodo de destino.

## Organización de un diálogo con carpetas
{: #dialog-build-folders}

Los nodos del diálogo se pueden agrupar en carpetas. Hay muchas razones para agrupar nodos, entre otras:

- Para mantener agrupados los nodos que tratan un tema similar para poder encontrarlos con más facilidad. Por ejemplo, podría agrupar los nodos que tratan preguntas sobre las cuentas de usuario en la carpeta *Cuenta de usuario* y los nodos que tratan de consultas relacionadas con pagos en la carpeta de *Pagos*.
- Para agrupar un conjunto de nodos que desea que el diálogo procese únicamente si se satisface una determinada condición. Utilice una condición como, por ejemplo, `$isPlatinumMember`, para agrupar nodos que ofrecen servicios adicionales que únicamente se deberían procesar si el usuario actual estuviese autorizado a recibir dichos servicios adicionales.
- Para ocultar nodos en el tiempo de ejecución mientras trabaja en ellos. Podría añadir los nodos a una carpeta con una condición `false` para evitar procesarlos.

Estas características de la carpeta afectan la forma en la que se procesan los nodos en una carpeta:

- Condición: si no se especifica ninguna condición, su asistente procesa directamente los nodos dentro de la carpeta. Si se especifica una condición, su asistente evalúa en primer lugar la condición de la carpeta para determinar si hay que procesar los nodos que contiene.
- Personalización: Los valores de configuración que se aplican a la carpeta los heredan los nodos en la carpeta. Si cambia los valores de digresión de la carpeta, por ejemplo, los cambios los heredan todos los nodos en la carpeta.
- Jerarquía de árbol: Los nodos en la carpeta se tratan como nodos raíz o hijo con base a si la carpeta se añadió al árbol de diálogo a nivel de raíz o hijo. Todos los nodos a nivel raíz que se añaden a una carpeta de nivel raíz continúan funcionando como nodos raíz; por ejemplo, no se convierten en nodos hijo de la carpeta. Sin embargo, si mueve un nodo de nivel raíz a una carpeta que es un hijo de otro nodo, el nodo raíz se convierte en hijo de ese otro nodo.

Las carpetas no afectan al orden en que se evalúan los nodos. Los nodos continúan procesándose del primero al último. A medida que su asistente recorre el árbol en sentido descendente, cuando encuentra una carpeta, si la carpeta no tiene ninguna condición o si su condición es verdadero, procesa de forma inmediata el primer nodo de la carpeta y recorre el árbol en sentido descendente a partir de ese punto. Si una carpeta no tiene una condición de carpeta, la carpeta es transparente para su asistente y cada nodo de la carpeta se trata como cualquier otro nodo individual del árbol.

### Adición de una carpeta
{: #dialog-build-folders-add}

Para añadir una carpeta a un árbol de diálogo, siga estos pasos:

1.  En la vista de árbol del separador **Diálogo**, pulse **Añadir carpeta**.

    La carpeta se añade al final del árbol del diálogo, justo antes del nodo **Anything else**. A no ser que se seleccione un nodo existente en el árbol, en cuyo caso, se añade bajo el nodo seleccionado.

    Si desea añadir la carpeta en cualquier otro lugar en árbol, en el nodo por encima del punto donde desea añadirla, pulse el icono **Más** ![Icono Más](images/kabob.png) y, a continuación, seleccione **Añadir carpeta**.

    Puede añadir una carpeta bajo un nodo hijo dentro de una rama del diálogo existente. Para ello, pulse el icono **Más** ![Icono Más](images/kabob.png) en el nodo hijo y, a continuación, seleccione **Añadir carpeta**.

    La carpeta se abre en la vista de edición.

1.  **Opcional**: Nombre de la carpeta.

1.  **Opcional**: Defina una condición para la carpeta.

    Si no especifica una condición, se utiliza `true`, indicando que los nodos en la carpeta siempre se procesan.

1.  Añada nodos de diálogo a la carpeta.

    - Para añadir nodos de diálogo existentes a la carpeta, debe moverlos a la carpeta de uno en uno.

      En el nodo que desea mover, pulse el icono **Más** ![Icono Más](images/kabob.png), seleccione **Mover** y, a continuación, pulse la carpeta. Seleccione **A carpeta** como destino de la acción de mover.

      A medida que mueva los nodos, se añadirán al inicio del árbol dentro de la carpeta. Por lo tanto, si desea conservar el orden de un conjunto de nodos de diálogo raíz consecutivos, por ejemplo, muévalos empezando en primer lugar por el último nodo.
      {: tip}

    - Para añadir un nodo de diálogo nuevo a la carpeta, pulse el icono **Más** ![Icono Más](images/kabob.png) en la carpeta y, a continuación, seleccione **Añadir nodo a carpeta**.

      El nodo de diálogo se añade al final del árbol del diálogo dentro de la carpeta.

### Supresión de una carpeta
{: #dialog-build-folders-delete}

Puede suprimir una carpeta sola o una carpeta con todos los nodos de diálogo que contenga.

Para suprimir una carpeta, siga los siguientes pasos:

1.  En la vista de árbol del separador **Diálogo**, encuentre la carpeta que desea suprimir.

1.  Pulse el icono **Más** ![Icono Más](images/kabob.png) en la carpeta y, a continuación, seleccione **Suprimir**.

1.  Realice una de las acciones siguientes:

    - Para únicamente suprimir una carpeta y mantener los nodos de diálogo que contiene la carpeta, no seleccione el recuadro de selección **Suprimir nodos dentro de la carpeta** y, a continuación, pulse **Sí, suprimirla**.
    - Para suprimir la carpeta y todos los nodos del diálogo que contiene, pulse **Sí, suprimirla**.

Si únicamente suprime la carpeta, los nodos que contenía se muestran en el árbol del diálogo en el lugar en el que se encontraba la carpeta.
