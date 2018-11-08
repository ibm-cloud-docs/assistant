---

copyright:
  years: 2015, 2018
lastupdated: "2018-03-19"

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

# Creación de un diálogo
{: #dialog-build}

Utilice la herramienta de {{site.data.keyword.conversationshort}} para crear su diálogo.
{: shortdesc}

## Límites del nodo del diálogo
{: #dialog-node-limits}

El número de nodos de diálogo que puede crear depende de su plan de servicio.

| Plan de servicio     | Nodos de diálogo por espacio de trabajo |
|------------------|---------------------------:|
| Estándar/Premium |                    100.000 |
| Lite             |                     25.000 |
{: caption="Detalles del plan de servicio" caption-side="top"}

Límite de profundidad de árbol: El servicio da soporte a 2.000 nodos de diálogo descendientes; las herramientas funcionan mejor con 20 o menos.

## Procedimiento
{: #dialog-procedure}

Para crear un diálogo, siga estos pasos:

1.  Abra la página **Compilar** en la barra de navegación, pulse el separador **Diálogo** y pulse **Crear**.

    Cuando se abra el creador de diálogos por primera vez, se crean automáticamente los nodos siguientes:
    - **Welcome**: El primer nodo. Contiene un saludo que se muestra a los usuarios la primera vez que interactúan con el servicio. Puede editar el mensaje.
    - **Anything else**: El nodo final. Contiene frases que se utilizan para responder a los usuarios cuando no se reconoce la información que especifican. Puede sustituir las respuestas que se proporcionan o añadir más respuestas con un significado similar a añadir variedad a la conversación. También puede elegir si desea que el servicio para devuelva cada respuesta definida por orden o si desea devolverlas en orden aleatorio.
1.  Para añadir más nodos al árbol de diálogo, pulse **Más** ![icono Más](images/kabob.png) en el nodo **Welcome** y seleccione **Añadir nodo debajo**.
1.  Especifique una condición que, si se cumple, active el servicio para que procese el nodo.

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

    La condición que defina debe tener menos de 500 caracteres de longitud.

Para obtener más información sobre cómo probar valores en condiciones, consulte [Condiciones](dialog-overview.html#conditions).
1.  **Opcional**: Si desea recopilar varios fragmentos información del usuario en este nodo, pulse **Personalizar** y habilite **Ranuras**. Consulte [Obtención de información con ranuras](dialog-slots.html) para ver más detalles.
1.  Escriba una respuesta.
    - Añada el texto que desea que el servicio muestre al usuario como respuesta.
    - Si desea definir diferentes respuestas basándose en determinadas condiciones, pulse **Personalizar** y habilite **Varias respuestas**. 
    - Para obtener información sobre las respuestas condicionales o cómo añadir distintas respuestas, consulte [Respuestas](dialog-overview.html##responses). 

1.  Especifique qué hacer después de que se haya procesado el nodo actual. Elija entre las siguientes opciones: 

    - **Esperar una entrada de usuario**: Se detiene el servicio hasta que el usuario proporcione una nueva entrada. 
    - **Saltar entrada de usuario**: El servicio salta directamente al primer nodo hijo. Esta opción únicamente está disponible si el nodo actual tiene al menos un nodo hijo. 
    - **Ir a**: El servicio sigue el proceso de diálogo en el nodo que especifique. Puede elegir si el servicio debe evaluar la condición del nodo de destino o ir directamente a la respuesta del nodo de destino. Consulte la [Configuración de la acción Ir a](dialog-overview.html#jump-to-config) para obtener más detalles. 

1.  **Opcional**: Asigne un nombre al nodo.

    El nombre del nodo del diálogo puede contener letras (en Unicode), números, espacios, signos de subrayado, guiones y puntos.

    La asignación de un nombre al nodo le permite recordar su finalidad y localizar el nodo cuando está minimizado. Si no especifica ningún nombre, se utiliza la condición como nombre.

1.  Para añadir más nodos, seleccione un nodo del árbol y pulse el icono **Más** ![icono Más](images/kabob.png).
    - Para crear un nodo igual que se compruebe a continuación si no se cumple la condición correspondiente al nodo existente, seleccione **Añadir nodo debajo**.
    - Para crear un nodo igual que se compruebe antes que la condición correspondiente al nodo existente, seleccione **Añadir nodo encima**.
    - Para crear un nodo hijo del nodo seleccionado, seleccione **Añadir nodo hijo**. Un nodo hijo se procesa después que su nodo padre.
    - Para copiar el nodo actual, seleccione **Duplicar**.

Para obtener más información sobre el orden en que se procesan los nodos del diálogo, consulte [Visión general del diálogo](dialog-overview.html#dialog-flow).
1.  Pruebe el diálogo a medida que lo crea.
   Consulte [Prueba del diálogo](#test) para obtener más información.

## Prueba del diálogo
{: #test}

A medida que realiza cambios en el diálogo, puede probarlo en cualquier momento para ver cómo responde ante una entrada.

1.  En el separador Diálogo, pulse el icono ![Preguntar a Watson](images/ask_watson.png).
1.  En el panel de la conversación, escriba el texto que desee y pulse Intro.

    Asegúrese de que el sistema haya terminado de formarse con los cambios más recientes antes de empezar a probar el diálogo. Si el sistema continúa entrenándose, se visualizará un mensaje en el panel *Pruébelo*:
    {: tip}

    ![Captura de pantalla del mensaje de entrenamiento](images/training.png)
1.  Compruebe la respuesta para ver si el diálogo ha interpretado correctamente la entrada y ha elegido la respuesta apropiada. 

    La ventana de la conversación indica las intenciones y entidades que se han reconocido en la entrada:

    ![Captura de pantalla de la salida del diálogo de prueba](images/test_dialog_output.png)
1.  Si desea saber qué nodo en el árbol de diálogo desencadenó una respuesta, pulse el icono **Ubicación** ![Ubicación](images/location.png) junto a la misma. Si todavía no está en el separador Diálogo, ábralo. 

    Se le da el foco al nodo de origen y se resalta la ruta que el servicio ha recorrido a través del árbol para llegar al final del mismo. La ruta permanece resaltada hasta que lleve a cabo otra acción como, por ejemplo, especificar una nueva entrada de prueba.
1.  Para comprobar o establecer el valor de una variable de contexto, pulse el enlace **Gestionar contexto**.

    Se muestran las variables de contexto que ha definido en el diálogo.

    Además, se muestra una variable de contexto `$timezone`. La interfaz de usuario del panel *Pruébelo* obtiene la información sobre el entorno local del usuario del navegador web y la utiliza para establecer la variable de contexto `$timezone`. Esta variable de contexto facilita el manejo de las referencias de tiempo en los intercambios del diálogo de prueba. Considere la posibilidad de hacer algo similar en la aplicación de usuario. Si no se especifica, se utiliza la hora media de Greenwich (GMT).

    Puede añadir una variable y establecer su valor para ver cómo responde el diálogo en la siguiente ronda del diálogo de prueba. Esta función es útil si, por ejemplo, el diálogo está configurado para mostrar diferentes respuestas en función de un valor de variable de contexto que proporciona el usuario.

    1.  Para añadir una variable de contexto, especifique el nombre de la variable y pulse **Intro**.
    1.  Para definir un valor predeterminado para la variable de contexto, busque la variable de contexto que ha añadido en la lista y especifique un valor para la misma.

    Consulte [Variables de contexto](dialog-runtime.html#context) para obtener más información.

1.  Continúe interactuando con el diálogo para ver cómo fluye la conversación.
    - Para encontrar y volver a enviar una expresión de prueba, puede pulsar la tecla Subir para recorrer las entradas recientes.
    - Para eliminar expresiones de prueba anteriores del panel de la conversación y volver a empezar, pulse el enlace **Borrar**. No solo se eliminan las expresiones de prueba y las respuestas; esta acción también elimina los valores de las variables de contexto establecidas como resultado de sus interacciones con el diálogo. Los valores de las variables de contexto que ha establecido o modificado de forma explícita no se borran.

### Qué hacer a continuación

Si determina que se están reconociendo intenciones o entidades erróneas, es posible que tenga que modificar las definiciones de las intenciones o entidades.

Si se están reconociendo las intenciones y entidades correctas, pero se activan los nodos erróneos en el diálogo, asegúrese de que las condiciones se han escrito adecuadamente. 

## Copia de un nodo de diálogo
{: #copy-node}

Puede duplicar un nodo para crear una copia exacta del mismo como un nodo de igual directamente debajo de dicho nodo en el árbol de diálogo. Al nodo copiado se le da el mismo nombre que el nombre del nodo original, añadiendo `- copy`*`n`* al nombre, donde *`n`* es un número que empieza por 1. Si duplica el mismo nodo más de una vez, el valor de *`n`* en el nombre se incrementa en una unidad en cada copia para ayudarle a distinguir a las copias entre sí. Si el nodo no tiene un nombre, se da el nombre `copy`*`n`*. 

Cuando se duplica un nodo que tiene nodos hijo, los nodos hijo también se duplican. Los nodos hijo copiados tienen exactamente los mismos nombres que los nodo hijo originales. La única forma de distinguir un nodo hijo copiado desde un nodo hijo original es la referencia `copy` en el nombre de nodo padre. 

1.  En el nodo que desea copiar, pulse el icono **Más** ![icono Más](images/kabob.png) y luego seleccione **Duplicar**.
1.  Considere cambiar los nodos copiados o editar sus condiciones para hacerlos distintos.

## Cómo mover un nodo del diálogo
{: #move-node}

Cada nodo que cree se puede moverse en el árbol del diálogo.

Es posible que desee mover un nodo creado anteriormente a otra área del flujo para cambiar la conversación. Puede mover nodos para convertirlos en hermanos o iguales en otra rama.

1.  En el nodo que desea mover, pulse el icono **Más** ![icono Más](images/kabob.png) y luego seleccione **Mover**.
1.  Seleccione un nodo de destino que se encuentre en el árbol próximo a donde desea mover este nodo. Elija si desea colocar este nodo antes o después del nodo de destino, o si desea convertirlo en hijo del nodo de destino.

## Organización de un diálogo con carpetas
{: #folders}

Los nodos del diálogo se pueden agrupar en carpetas. Hay muchas razones para agrupar nodos, entre otras: 

- Para mantener agrupados los nodos que tratan un tema similar para poder encontrarlos con más facilidad. Por ejemplo, podría agrupar los nodos que tratan preguntas sobre las cuentas de usuario en la carpeta *Cuenta de usuario* y los nodos que tratan de consultas relacionadas con pagos en la carpeta de *Pagos*. 
- Para agrupar un conjunto de nodos que desea que el diálogo procese únicamente si se satisface una determinada condición. Utilice una condición como, por ejemplo, `$isPlatinumMember`, para agrupar nodos que ofrecen servicios adicionales que únicamente se deberían procesar si el usuario actual estuviese autorizado a recibir dichos servicios adicionales. 
- Para ocultar nodos en el tiempo de ejecución mientras trabaja en ellos. Podría añadir los nodos a una carpeta con una condición `false` para evitar procesarlos. 
- Para aplicar a varios nodos raíz al mismo tiempo los mismos valores de configuración para las digresiones. Consulte [Digresiones](dialog-runtime.html#digressions) para obtener más información. 

Estas características de la carpeta afectan la forma en la que se procesan los nodos en una carpeta: 

- Condición: Si se especifica, el servicio primero evalúa la condición de la carpeta para determinar si hay que procesar los nodos que contiene. 
- Personalización: Los valores de configuración que se aplican a la carpeta los heredan los nodos en la carpeta.  Si cambia los valores de digresión de la carpeta, por ejemplo, los cambios los heredan todos los nodos en la carpeta. 
- Jerarquía de árbol: Los nodos en la carpeta se tratan como nodos raíz o hijo con base a si la carpeta se añadió al árbol de diálogo a nivel de raíz o hijo. Todos los nodos a nivel raíz que se añaden a una carpeta de nivel raíz continúan funcionando como nodos raíz; por ejemplo, no se convierten en nodos hijo de la carpeta.  Sin embargo, si mueve nodos a nivel raíz a una carpeta que es un hijo de otro nodo, entonces los nodos raíz se convierten en hijos de dicho otro nodo. 

Las carpetas no afectan al orden en que se evalúan los nodos. Los nodos continúan procesándose del primero al último. A medida que el servicio recorre el árbol en sentido descendente, cuando encuentra una carpeta, si la condición de la carpeta es de verdadero, procesa de forma inmediata el primer nodo de la carpeta, y recorre el árbol en sentido descendente a partir de ese punto. Si una carpeta no tiene una condición de carpeta, es transparente para el servicio. 

### Adición de una carpeta
{: #folders-add}

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
{: #folders-delete}

Puede suprimir una carpeta sola o una carpeta con todos los nodos de diálogo que contenga. 

Para suprimir una carpeta, siga los siguientes pasos:

1.  En la vista de árbol del separador **Diálogo**, encuentre la carpeta que desea suprimir. 

1.  Pulse el icono **Más** ![Icono Más](images/kabob.png) en la carpeta y, a continuación, seleccione **Suprimir**.  

1.  Realice una de las acciones siguientes:

    - Para únicamente suprimir una carpeta y mantener los nodos de diálogo que contiene la carpeta, no seleccione el recuadro de selección **Suprimir nodos dentro de la carpeta** y, a continuación, pulse **Sí, suprimirla**.
    - Para suprimir la carpeta y todos los nodos del diálogo que contiene, pulse **Sí, suprimirla**.

Si únicamente suprime la carpeta, los nodos que contenía se visualizan en el árbol del diálogo en el lugar en el que se encontraba la carpeta. 

## Búsqueda de un nodo del diálogo por su ID de nodo
{: #get-node-id}

Es posible que desee buscar el nodo del diálogo que está asociado a un ID de nodo conocido por uno de estos motivos:

- Está revisando registros y el registro hace referencia a una sección del diálogo por su ID de nodo.
- Desea correlacionar los ID de nodo que aparecen en la lista de la propiedad `nodes_visited` de la salida del mensaje de la API con nodos que pueda ver en el árbol del diálogo.
- Un mensaje de error en tiempo de ejecución del diálogo le informa sobre un error de sintaxis y utiliza un ID de nodo para identificar el nodo que tiene que corregir.

Para descubrir un nodo basándose en su ID de nodo, siga estos pasos:

1.  En el separador Diálogo de la herramienta, seleccione cualquier nodo del árbol del diálogo.
1.  Cierre la vista de edición si está abierta para el nodo actual.
1.  En el campo de ubicación del navegador web, debería mostrarse un URL con la siguiente sintaxis:

    `    https://watson-conversation.ng.bluemix.net/space/instance-id/workspaces/workspace-id/build/dialog#node=node-id
    `

1.  Edite el URL y sustituya el valor `node-id` actual por el ID del nodo que desea encontrar y envíe el nuevo URL.
1.  Si es necesario, vuelva a resaltar el URL editado y envíelo de nuevo.

La herramienta se renueva y el foco pasa a estar en el nodo del diálogo con el ID de nodo que ha especificado. Si el ID de nodo es para una ranura, un manejador de ranura o una respuesta condicional, entonces el nodo en que se definió la respuesta condicional o la ranura obtiene el foco y se visualiza la correspondiente sección modal. 

**Nota**: Si sigue sin poder encontrar el nodo, exporte el espacio de trabajo y utilice el editor JSON para buscarlo en el archivo JSON del espacio de trabajo. 
