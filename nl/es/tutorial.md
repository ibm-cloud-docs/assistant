---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-24"

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

# Guía de aprendizaje: Creación de un diálogo complejo
{: #tutorial}

En esta guía de aprendizaje, utilizará el servicio {{site.data.keyword.conversationshort}} para crear un diálogo que ayude a los usuarios a interactuar con un panel de control de coche cognitivo.
{: shortdesc}

## Objetivos del aprendizaje

Cuando termine la guía de aprendizaje, habrá aprendido a:

- Definir entidades
- Planificar un diálogo
- Utilizar condiciones de nodo y respuesta en un diálogo

### Duración
Le llevará entre 2 y 3 horas completar esta guía de aprendizaje.

### Requisito previo

Antes de empezar, complete la [Guía de aprendizaje de iniciación](getting-started.html). 

Utilizará el espacio de trabajo de la guía de aprendizaje de {{site.data.keyword.conversationshort}} que ha creado y añadirá nodos al diálogo simple que ha creado como parte del ejercicio de iniciación.

## Paso 1: Añadir intenciones y ejemplos
{: #intents}

Añada una intención en el separador Intenciones. Una intención es la finalidad u objetivo expresado en la entrada del usuario.

1.  En la página de Intenciones del espacio de trabajo de la guía de aprendizaje de {{site.data.keyword.conversationshort}}, pulse **Añadir intención**. 
1.  Añada el siguiente nombre de intención y, a continuación, pulse **Crear intención**:

    ```
    turn_on
    ```
    {: codeblock}

    Se añade el signo `#` al nombre de intención que especifique. La intención `#turn_on` indica que el usuario desea activar un dispositivo, como la radio, los limpiaparabrisas o los faros.
1.  En **Añadir ejemplo de usuario**, escriba la siguiente expresión y, a continuación, pulse **Añadir ejemplo**:  

    ```
    I need lights
    ```
    {: codeblock}

1.  Añada estos 5 ejemplos para ayudar a Watson a reconocer la intención `#turn_on`.

    ```
    Play some tunes
    Turn on the radio
    turn on
    Air on please
    Crank up the AC
    Turn on the headlights
    ```
    {: codeblock}

1.  Pulse el icono **Cerrar** ![Flecha Cerrar](images/close_arrow.png) para finalizar la adición de la intención `#turn_on`. 

Ahora tendrá tres intenciones, la intención `#turn_on` que acaba de añadir, y las intenciones `#hello` y `#goodbye` que se añadieron en la *guía aprendizaje de iniciación* que debería haber completado como un paso de requisito previo para este procedimiento. Cada intención tiene un conjunto de expresiones de ejemplo que ayuda a entrenar a Watson para que reconozca las intenciones de la entrada de usuario. 

## Paso 2: Añadir entidades
{: #entities}

Una definición de entidad incluye un conjunto de *valores* de entidad que se pueden utilizar para activar distintas respuestas. Cada valor de entidad puede tener varios *sinónimos*, que definen distintas formas en que se puede especificar el mismo valor en la entrada de usuario.

Cree entidades que se puedan producir en la entrada de usuario que tiene la intención #turn_on para representar lo que el usuario desea activar.

1.  Pulse el separador **Entidades** para abrir la página Entidades.
1.  Pulse **Añadir entidad**.
1.  Añada el siguiente nombre de entidad y pulse Intro:

    ```
    appliance
    ```
    {: codeblock}

    Se antepone un signo `@` al nombre de entidad que especifique. La entidad `@appliance` representa un dispositivo del coche que es usuario puede desear activar.
1.  Añada el siguiente valor al campo **Nombre de valor**: 

    ```
    radio
    ```
    {: codeblock}

    El valor representa un dispositivo específicos que el usuario puede desear activar.
1.  Añada otras formas de especificar la entidad del dispositivo radio en el campo **Sinónimos**. Pulse el **Tabulador** para colocarse en el campo y especifique los siguientes sinónimos. Pulse **Intro** después de cada sinónimo.

    ```
    music
    tunes
    ```
    {: codeblock}

1.  Pulse **Añadir valor** para acabar de definir el valor `radio` para la entidad `@appliance`. 
1.  Añada otros tipos de dispositivos.

    - Valor: `headlights`. Sinónimo: `lights`.
    - Valor: `air conditioning`. Sinónimos: `air` y `AC`.

1.  Pulse para **Activar** la coincidencia aproximada para la entidad `@appliance`. Este valor ayuda al servicio a reconocer referencias a entidades en la entrada de usuario aunque la entidad se especifique de forma que no coincida exactamente con la sintaxis utilizada aquí.
1.  Pulse el icono **Cerrar** ![Flecha Cerrar](images/close_arrow.png) para completar la adición de la entidad `@appliance`. 
1.  Repita los pasos 2-8 para crear la entidad @`genre` con la coincidencia aproximada activada y estos valores y sinónimos:

    - Valor: `classical`. Sinónimo: `symphonic`.
    - Valor: `rhythm and blues` Sinónimo: `r&b`.
    - Valor: `rock`. Sinónimos: `rock & roll`, `rock and roll` y `pop`.

Ha definido dos entidades: `@appliance` (representa un dispositivo que el bot puede activar) y `@genre` (representa un género musical que el usuario puede elegir para escucharlo).

Cuando se recibe la entrada del usuario, el servicio {{site.data.keyword.conversationshort}} identifica las intenciones y entidades. Ahora puede definir un diálogo que utilice intenciones y entidades para elegir la respuesta correcta.

## Paso 3: Crear un diálogo complejo
{: #complex-dialog}

En este diálogo complejo, creará ramas del diálogo que manejarán la intención #turn_on que ha definido anteriormente.

### Añada un nodo raíz para #turn_on
Cree una rama del diálogo que responda a la intención #turn_on. Empiece creando el nodo raíz:

1.  Pulse el icono Más ![Más opciones](images/kabob.png) en el nodo **#hello** y seleccione **Añadir nodo debajo**.
1.  Empiece escribiendo `#turn_on` en el campo de condición y luego selecciónelo en la lista.
    Activa esta condición cualquier entrada que coincida con la intención #turn_on.
1.  No especifique una respuesta en este nodo. Pulse ![Cerrar](images/close.png) para cerrar la vista de edición del nodo.

### Escenarios
El diálogo debe determinar qué dispositivo desea activar el usuario. Para manejar esta situación, cree varias respuestas en función de condiciones adicionales.

Existen tres posibles escenarios, que dependen de las intenciones y entidades que ha definido:

**Escenario 1**: El usuario desea encender la música, en cuyo caso el bot debe pedir el género.

**Escenario 2**: El usuario desea activar algún otro dispositivo válido, en cuyo caso el bot hace eco del nombre del dispositivo solicitado en un mensaje que indica que se está activando.

**Escenario 3**: El usuario no especifica ningún nombre de dispositivo que se reconozca, en cuyo caso el bot debe pedir una aclaración.

Añada nodos que comprueben estas condiciones del escenario en este orden para que el diálogo evalúe en primer lugar la condición más específica.

### Para el Escenario 1

Añada nodos al escenario 1, que es aquel en el que el usuario desea encender la música. Como respuesta, el bot debe pedir al género musical.

#### Añada un nodo hijo que compruebe si el tipo de dispositivo es música

1.  Pulse el icono Más ![Más opciones](images/kabob.png) en el nodo **#turn_on** y seleccione **Añadir nodo hijo**.
1.  En el campo de condición, especifique `@appliance:radio`.
    Esta condición es verdadera si el valor de la entidad @appliance es `radio` o uno de sus sinónimos, que se han definido en el separador Entidades.
1.  En el campo de respuesta, escriba `What kind of music would you like to hear?` (¿Qué tipo de música le gustaría escuchar?).
1.  Asigne al nodo el nombre `Music`.
1.  Pulse ![Cerrar](images/close.png) para cerrar la vista de edición del nodo.

#### Añadir un salto desde el nodo #turn_on al nodo Music

Vaya directamente desde el nodo `#turn on` al nodo `Music` sin solicitar ninguna otra entrada de usuario. Para ello, puede utilizar una acción **Ir a**.

1.  Pulse el icono Más ![Más opciones](images/kabob.png) en el nodo **#turn_on** y seleccione **Ir a**.
1.  Seleccione el nodo hijo **Music** y luego seleccione **Si el bot reconoce (condición)** para indicar que desea procesar la condición del nodo Music.

![Ir a antes](images/tut-dialog-jumpto.png)

Tenga en cuenta que tiene que crear el nodo de destino (el nodo al que desea ir) antes de añadir la acción **Ir a**.

Después de crear la relación Ir a, verá una nueva entrada en el árbol:

![Ir a después](images/tut-dialog-jump2.png)

#### Añadir un nodo hijo que compruebe el género musical

Ahora añada un nodo para procesar el tipo de música que solicita el usuario.

1.  Pulse el icono Más ![Más opciones](images/kabob.png) en el nodo **Music** y seleccione **Añadir nodo hijo**.
    Este nodo hijo solo se evalúa después de que el usuario responda a la pregunta sobre el tipo de música que desea escuchar. Puesto que necesitamos una entrada de usuario antes de este nodo, no es necesario utilizar una acción **Ir a**.
1.  Añada `@genre` al campo de condición.  Esta condición es verdadera siempre que se detecta un valor válido para la entidad @genre.
1.  Escriba `OK! Playing @genre.` como respuesta. Esta respuesta reitera el valor del género que especifica el usuario.

#### Añada un nodo que maneja los tipos de género no reconocidos en las respuestas del usuario

Añadir un nodo para responder cuando el usuario no especifica un valor reconocido por @genre.

1.  Pulse el icono Más ![Más opciones](images/kabob.png) en el nodo *@genre* y seleccione **Añadir nodo debajo** para crear un nodo igual.
1.  Escriba `true` en el campo de condición.
    La condición true es una condición especial. Se especifica que si el flujo del diálogo alcanza este nodo, siempre se debe evaluar como true. (Si el usuario especifica un valor de @genre válido, nunca se alcanzará este nodo.)
1.  Escriba `I'm sorry, I don't understand. I can play classical, rhythm and blues, or rock music.` como respuesta.

Eso se encarga de todos los casos en que el usuario solicita que se active la música.

#### Pruebe el diálogo correspondiente a música

1.  Seleccione el icono ![Preguntar a Watson](images/ask_watson.png) para abrir el panel de la conversación.
1.  Escriba `Play music`.
    El robot reconoce la intención #turn_on y la entidad @appliance:music y responde preguntando el género musical.

1.  Escriba un valor de @genre válido (por ejemplo, `rock`).
    El bot reconoce la entidad @genre y responde correctamente.

    ![Muestra una solicitud correcta de reproducir música](images/tut-test-music.png)

1.  Vuelva a escribir `Play music`, pero esta vez especifique una respuesta no válida para el género. El bot responde que no entiende.

### Para el Escenario 2

Añadiremos nodos al escenario 2, que es aquel en el que el usuario desea activar otro dispositivo válido. En este caso, el bot hace eco del nombre del dispositivo solicitado en un mensaje que indica que se está activando.

#### Añadir un nodo hijo que compruebe si hay más dispositivos

Añada un nodo que se active cuando el usuario especifique cualquier otro valor válido para @appliance.
Para los otros valores de @appliance, el bot no necesita solicitar ninguna otra entrada. Simplemente devuelve una respuesta positiva.

1.  Pulse el icono Más ![Más opciones](images/kabob.png) en el nodo **Music** y luego seleccione **Añadir nodo debajo** para crear un nodo igual que se evalúe después de que se evalúe la condición @appliance:music.
1.  Escriba `@appliance` como condición de nodo.
    Esta condición se activa si la entrada de usuario incluye cualquier valor reconocido por la entidad @appliance además de music.
1.  Escriba `OK! Turning on the @appliance.` como respuesta.
    Esta respuesta reitera el valor del dispositivo que ha especificado el usuario.

#### Pruebe el diálogo con otros dispositivos

1.  Seleccione el icono ![Preguntar a Watson](images/ask_watson.png) para abrir el panel de la conversación.
1.  Escriba `lights on`.

    El robot reconoce la intención #turn_on y la entidad @appliance:headlights y responde con `OK, turning on the headlights` (De acuerdo, encendiendo los faros).

    ![Muestra una solicitud correcta de encender los faros](images/tut-test-lights.png)

1.  Escriba `turn on the air`.

    El robot reconoce la intención #turn_on y la entidad @appliance(air conditioning) y responde con `OK, turning on the air conditioning` (De acuerdo, encendiendo el aire acondicionado).

1.  Intente variaciones en todos los mandatos soportados en función de las expresiones de ejemplo y los sinónimos de entidades que ha definido.

### Para el Escenario 3

Ahora añada un nodo igual que se active si el usuario no especifica un tipo de dispositivo válido.

1.  Pulse el icono Más ![Más opciones](images/kabob.png) en el nodo **@appliance** y luego seleccione **Añadir nodo debajo** para crear un nodo igual que se evalúe después de que se evalúe la condición @appliance.
1.  Escriba `true` en el campo de condición.
    (Si el usuario especifica un valor de @appliance válido, nunca se alcanzará este nodo.)
1.  Escriba `I'm sorry, I'm not sure I understood you. I can turn on music, headlights, or air conditioning.` como respuesta.

#### Pruebe algunos más

1.  Pruebe más variaciones de expresiones para probar el diálogo.

    Si el bot no reconoce la intención correcta, puede volverlo a formar directamente desde la ventana de la conversación. Seleccione la flecha que hay junto a la intención incorrecta y elija la correcta de la lista.

    ![Muestra cómo seleccionar otra intención y volver a formar](images/tut-change-intent.gif)

Si lo desea, puede revisar el espacio de trabajo **Car Dashboard - Sample (Panel de control del coche - Ejemplo)** para ver este mismo caso de uso ampliado con un diálogo más largo y funciones adicionales.

1.  Pulse el botón **Volver a espacios de trabajo** ![Muestra el botón Volver a espacios de trabajo del menú](images/workspaces-button.png) desde el menú de navegación.

1.  En el mosaico de **Car Dashboard - Sample**, pulse **Editar ejemplo**.

## Siguientes pasos
{: #deploy}

Ahora que ha creado y probado su espacio de trabajo, puede desplegarlo conectándolo a una interfaz de usuario. Lo puede hacer de varias formas.

### Probar en Slack

Puede utilizar la herramienta de despliegue de prueba para [desplegar su espacio de trabajo](test-deploy.html) como un chatbot en un canal Slack en solo unos pasos. Esta opción constituye la forma más rápida y sencilla de desplegar el espacio de trabajo para probarlo, pero tiene limitaciones.

### Cree su propia aplicación frontal

Puede utilizar los SDK de Watson para [crear su propia](develop-app.html) aplicación frontal que se conecte al espacio de trabajo utilizando la API REST de {{site.data.keyword.conversationshort}}. 

### Despliegue en redes sociales o canales de mensajería

Puede utilizar la [infraestructura de Botkit](integrations.html) para crear una aplicación de bot que puede integrar con redes sociales y canales de mensajería como Slack, Facebook y Twilio.
