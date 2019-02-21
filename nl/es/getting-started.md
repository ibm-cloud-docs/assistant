---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-16"

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
{:download: .download}

# Guía de aprendizaje de iniciación
{: #getting-started}

En esta breve guía de aprendizaje presentamos una introducción a la herramienta {{site.data.keyword.conversationshort}} y al proceso de crear la primera conversación.
{: shortdesc}

## Antes de empezar
{: #prerequisites}

Para poder empezar necesitará una instancia de servicio. 

<!-- Remove the text marked `download` after there's no g-s tab in the catalog dashboard -->

Ha creado la instancia de servicio. Pulse **Gestionar** y, a continuación **Iniciar herramienta**. Vaya al paso 2.
{: download tip}

Si ha creado un proyecto con el servicio {{site.data.keyword.conversationshort}}, habrá completado los requisitos previos. Vaya al paso 1. 

1.  Vaya a la página [Servicios ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.{DomainName}/developer/watson/services){: new_window} de {{site.data.keyword.watson}} Developer Console. 
1.  Seleccione {{site.data.keyword.conversationshort}}, pulse **Añadir servicios** y regístrese a una cuenta gratuita de {{site.data.keyword.Bluemix_notm}} o inicie una sesión. 
1.  Cambie el nombre del proyecto a `conversation-tutorial` (guía de aprendizaje de conversación) y, a continuación, pulse **Crear proyecto**. 

<!-- Remove this text after dedicated instances have the developer console: begin -->

Si utiliza {{site.data.keyword.Bluemix_dedicated_notm}}, cree su instancia de servicio desde la página [{{site.data.keyword.conversationshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.{DomainName}/catalog/services/conversation/){: new_window} en el Catálogo. 

<!-- Remove this text after dedicated instances have the developer console: end -->

## Paso 1: Iniciar la herramienta
{: #launch-tool}

Después de crear un proyecto que incluya el servicio {{site.data.keyword.conversationshort}}, deberá ver la página de detalles del proyecto. Inicie desde aquí la herramienta {{site.data.keyword.conversationshort}}.

Pulse **Iniciar herramienta** para {{site.data.keyword.conversationshort}} bajo **Servicios**.

<!-- To do: Add screenshot for developer console -->

Si se le solicita iniciar una sesión en la herramienta, proporcione sus credenciales de {{site.data.keyword.Bluemix_notm}}. 

Si no está en la página de detalles del proyecto para el servicio de {{site.data.keyword.conversationshort}}, vaya a la página [Proyectos ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.{DomainName}/developer/watson/projects) de {{site.data.keyword.watson}} Developer Console y seleccione el proyecto.
{: tip}

<!-- Remove this text after dedicated instances have the developer console: begin -->

{{site.data.keyword.Bluemix_dedicated_notm}}: Seleccione la instancia de servicio en el panel de control para iniciar el conjunto de herramientas. 

<!-- Remove this text after dedicated instances have the Developer Console: end -->

## Paso 2: Crear un espacio de trabajo
{: #create-workspace}

El primer paso en la herramienta {{site.data.keyword.conversationshort}} consiste en crear un espacio de trabajo.

Un [*espacio de trabajo*](configure-workspace.html) es un contenedor para los artefactos que definen el flujo de la conversación.

1.  En la herramienta {{site.data.keyword.conversationshort}}, pulse **Crear**.
1.  Proporcione el nombre `{{site.data.keyword.conversationshort}} tutorial` (guía de aprendizaje) a su espacio de trabajo. Si el diálogo que tiene previsto crear utilizará un idioma distinto del inglés, elija el idioma adecuado en la lista. Pulse **Crear**. Irá al separador **Intenciones** del nuevo espacio de trabajo.

![Muestra una animación de un usuario creando el espacio de trabajo {{site.data.keyword.conversationshort}} tutorial.](images/gs-create-workspace-animated.gif)

## Paso 3: Crear intenciones
{: #create-intents}

Una [intención](intents.html) representa la finalidad de la entrada de usuario. Puede considerar las intenciones como las acciones que pueden desear llevar a cabo los usuarios con la aplicación.

Para este ejemplo, vamos a simplificar y vamos a definir solo dos intenciones: uno para decir hola y uno para decir adiós.

1.  Asegúrese de que está en el separador Intenciones. (Debería estar allí si acaba de crear el espacio de trabajo.)
1.  Pulse **Añadir intención**.
1.  Asigne a la intención el nombre `hello` y a continuación pulse **Crear intención**. 
1.  Escriba `hello` en el campo **Añadir ejemplo de usuario** y, a continuación pulse **Intro**. 

   Los *ejemplos* indican al servicio {{site.data.keyword.conversationshort}} los tipos de entrada de usuario que desea comparar con la intención. Cuantos más ejemplos proporcione, más precisa será la forma en que el servicio reconocerá las intenciones de los usuarios.
1.  Añada cuatro ejemplos más: 
    - `good morning`
    - `greetings`
    - `hi`
    - `howdy`

1.  Pulse el icono **Cerrar** ![Flecha Cerrar](images/close_arrow.png) para finalizar la creación de la intención #hello. 
1.  Cree otra intención denominada #goodbye con estos cinco ejemplos:
    - `bye`
    - `farewell`
    - `goodbye`
    - `I'm done`
    - `see you later`

Ha creado dos intenciones, #hello y #goodbye, y ha proporcionado entrada de usuario de ejemplo para formar el servicio {{site.data.keyword.watson}} para que reconozca estas intenciones en la entrada de usuario.

![Muestra la página Intenciones con las intenciones #goodbye y #hello](images/gs-add-intents-result.png)

## Paso 4: Añadir intenciones de un catálogo
{: #add-catalog}

Añada datos de entrenamiento creados por IBM a su espacio de trabajo añadiendo intenciones de un catálogo. En concreto, dará acceso a su asistente al catálogo `Business Information` de forma que su diálogo pueda utilizar solicitudes de información de contacto de empresas. 

1.  En la herramienta {{site.data.keyword.conversationshort}}, pulse el separador **Catálogo**. 
1.  Busque **Business Information** en la lista y, a continuación, pulse **Añadir a bot**. 
1.  Abra el separador **Intenciones** para revisar las intenciones y las expresiones de ejemplo asociadas que se añadieron a los datos de entrenamiento. Puede reconocerlas porque cada nombre de intención empieza con el prefijo `#Business_Information_`. 
En un paso posterior añadirá la intención `#Business_Information_Contact_Us` a su diálogo. 

Ha añadido de forma satisfactoria información suplemental a sus datos de entrenamiento con contenido creado de forma previa por parte de IBM. 

## Paso 5: Crear un diálogo
{: #build-dialog}

Un [diálogo](dialog-build.html) define el flujo de la conversación en forma de árbol lógico. Cada nodo del árbol tiene una condición que lo activa, en función de la entrada de usuario.

Vamos a crear un diálogo sencillo que maneje nuestras intenciones #hello y #goodbye, cada una con un solo nodo.

### Adición de un nodo de inicio

1.  En la herramienta {{site.data.keyword.conversationshort}}, pulse el separador **Diálogo**.
1.  Pulse **Crear**. Verá dos nodos:
    - **Welcome**: Contiene un saludo que se muestra a los usuarios la primera vez que interactúan con el servicio.
    - **Anything else**: Contiene frases que se utilizan para responder a los usuarios cuando no se reconoce la información que especifican.

    ![Muestra el árbol del diálogo con los nodos Welcome y Anything else](images/gs-add-dialog-node-animated-cover.png)
1.  Pulse el nodo **Welcome** para abrirlo en la vista de edición.
1.  Sustituya la respuesta predeterminada por el texto `Welcome to the {{site.data.keyword.conversationshort}} tutorial!`. 

    ![Muestra el nodo Welcome abierto en la vista de edición](images/gs-edit-welcome-node.png)
1.  Pulse ![Cerrar](images/close.png) para cerrar la vista de edición.

Ha creado un nodo de diálogo que se activa mediante la condición `welcome`, que es una condición especial que indica que el usuario ha iniciado una nueva conversación. El nodo especifica que cuando se inicia una nueva conversación, el sistema debe responder con el mensaje de bienvenida.

### Prueba del nodo de inicio

Puede probar el diálogo en cualquier momento para verificarlo. Vamos a probarlo ahora.

- Pulse el icono ![Preguntar a Watson](images/ask_watson.png) para abrir el panel "Pruébelo". Debería ver el mensaje de bienvenida.

    ![Prueba del nodo de diálogo](images/gs-tryitout-welcome-node.png)

### Adición de nodos para manejar intenciones

Ahora vamos a añadir nodos para manejar nuestras intenciones entre el nodo `Welcome` y el nodo `Anything else`.

1.  Pulse el icono Más ![Más opciones](images/kabob.png) del nodo **Welcome** y seleccione **Añadir nodo debajo**.
1.  Escriba `#hello` en el campo **Especificar una condición** de este nodo. Luego seleccione la opción **#hello**.
1.  Añada la respuesta, `Good day to you`.
1.  Pulse ![Cerrar](images/close.png) para cerrar la vista de edición.

   ![Muestra una animación de un usuario que está añadiendo un nodo hello al diálogo](images/gs-add-dialog-node-animated.gif)
1.  Pulse el icono Más ![Más opciones](images/kabob.png) en este nodo y luego seleccione **Añadir nodo debajo** para crear un nodo igual. En el nodo de igual, especifique `#Business_Information_Contact_Us` como la condición. 
1.  Añada el siguiente texto como la respuesta. 

    `Call us at 800-426-4968 or give us your feedback at https://www.ibm.com/scripts/contact/contact/us/en.
(Llámenos al 800-426-4968 o proporcione sus comentarios en
https://www.ibm.com/scripts/contact/contact/us/en.)
`
1.  Pulse el icono Más ![Más opciones](images/kabob.png) en este nodo y luego seleccione **Añadir nodo debajo** para crear otro nodo de igual. En el nodo de igual, especifique `#goodbye` como condición y `OK. See you later!` como respuesta.

### Prueba de reconocimiento de intención

Ha creado un diálogo sencillo para reconocer y responder a las entradas hello y goodbye. Vamos a ver si funcionan.

1.  Pulse el icono ![Preguntar a Watson](images/ask_watson.png) para abrir el panel "Pruébelo". Aparece este mensaje de bienvenida tranquilizador.
1.  En la parte inferior del panel, escriba `Hello` y pulse Intro. La salida indica que se ha reconocido la intención #hello y aparece la respuesta adecuada (`Good day to you`).
1.  Intente la entrada siguiente:
    - `bye`
    - `howdy`
    - `see ya`
    - `good morning`
    - `sayonara`

   ![Muestra una animación del usuario probando el diálogo en el panel de 'Pruébelo'.](images/gs-test-dialog-animated.gif)
1.  Especifique `Who can I call if I have questions?`(¿A quién puedo llamar si tengo preguntas?) y pulse Intro. La salida indica que se reconoció la intención `#Business_Information_Contact_Us` y se visualiza la respuesta que ha añadido. 

{{site.data.keyword.watson}} reconoce las intenciones incluso cuando la entrada no coincide exactamente con los ejemplos que ha incluido. El diálogo utiliza intenciones para identificar la finalidad de la entrada de usuario, independientemente de los términos precisos utilizados, y responde en la forma que especifique.

### Resultado de crear un diálogo

Eso es todo. Ha creado una conversación sencilla con dos intenciones y un diálogo para reconocerlas.

## Paso 6: Revisar el espacio de trabajo de ejemplo
{: #review-sample-workspace}

Abra el espacio de trabajo de ejemplo para ver intenciones similares a las que acaba de crear y muchas más, y para ver cómo se utilizan en un diálogo complejo.

1.  Vuelva a la página espacios de trabajo.
   Puede pulsar el botón ![Botón Volver a espacios de trabajo](images/workspaces-button.png) en el menú de navegación.
1.  En el mosaico del espacio de trabajo **Car Dashboard - Sample**, pulse el botón **Editar ejemplo**.

    ![Muestra el mosaico de ejemplo de Car Dashboard en la página Espacios de trabajo](images/gs-workspace-car-sample.png)

## Siguientes pasos
{: #next-steps}

Esta guía de aprendizaje se basa en un ejemplo sencillo. Para una aplicación real, deberá algunas intenciones más interesantes, algunas entidades y un diálogo más complejo.

- Pruebe la [guía de aprendizaje](tutorial.html) avanzada para añadir entidades y aclarar una finalidad de usuario.
- Para [desplegar](deploy.html) el espacio de trabajo, conéctelo a una interfaz de usuario frontal, red social o canal de mensajería.
- Consulte las [apps de ejemplo](sample-applications.html).
