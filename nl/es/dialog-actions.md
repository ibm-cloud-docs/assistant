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

# Cómo realizar llamadas mediante programación desde un nodo de diálogo ![BETA](images/beta.png)
{: #dialog-actions}

Defina las acciones que pueden realizar las llamadas de programación a aplicaciones o servicios externos y obtenga un resultado como parte del proceso que se produce en un diálogo.
{: shortdesc}

Puede utilizar un servicio externo para realizar los siguientes tipos de cosas:
- Validar la información que ha recopilado del usuario.
- Realizar cálculos o manipulaciones de series de caracteres en entradas de usuario que son demasiado complejas para que las manejen los métodos de expresión SpEL soportados.
- Interactuar con un servicio web externo para obtener información. Por ejemplo, puede comprobar la hora de llegada esperada de un vuelo desde un servicio de tráfico aéreo o puede obtener una previsión de un servicio meteorológico.
- Enviar solicitudes a una aplicación externa como, por ejemplo, un sitio de reservas de restaurantes, para completar una transacción simple en nombre del usuario.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Cómo llamar a IBM Cloud Functions" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/y0A6X-KNoB8?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Cuando define una llamada mediante programación debe elegir entre uno de los tipos siguientes:

- **client**: Define una llamada mediante programación en un formato estándar que su aplicación de cliente externa puede comprender. La aplicación cliente debe utilizar la información proporcionada para ejecutar la llamada o función de programación y devolver el resultado al diálogo. Básicamente, este tipo de llamada indica al diálogo que haya una pasa en ese punto y que espere a que la aplicación cliente haga algo. El programa en el que se ejecuta la aplicación cliente puede ser el que elija. Asegúrese de especificar el nombre de la llamada y los detalles de los parámetros, así como el nombre de la variable de mensaje de error, según las reglas de formato de JSON que se describen más adelante.

- **cloud_function**: Llama directamente a una acción de {{site.data.keyword.openwhisk_short}} que devuelve el resultado al diálogo. Debe proporcionar una clave de autenticación de {{site.data.keyword.openwhisk_short}} con la llamada. (Este tipo solía recibir el nombre de **server**. El tipo **server** sigue recibiendo soporte).

- **web_action**: Llama a una acción web de {{site.data.keyword.openwhisk_short}}. Las acciones web son acciones de {{site.data.keyword.openwhisk_short}} anotadas que los desarrolladores pueden utilizar para programar lógica de fondo a la que una aplicación web puede acceder de forma anónima, sin que se necesite una clave de autenticación de {{site.data.keyword.openwhisk_short}}. Aunque no se requiere autenticación, las acciones web se pueden proteger de las siguientes formas:

  - Con una señal de autenticación que sea específica de la acción web, y que el propietario de la acción pueda revocar o cambiar en cualquier momento
  - Pasando sus credenciales de {{site.data.keyword.openwhisk_short}}

Todos los tipos de acción de {{site.data.keyword.openwhisk_short}} (web_action y cloud_function o server) incurren en un coste. El coste de activar la acción se carga a la persona propietaria de las credenciales que se especifican en la llamada a la acción. Consulte el apartado [Tarifas ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/openwhisk/learn/pricing){: new_window} para obtener más información. El servicio {{site.data.keyword.openwhisk_short}} no distingue entre las llamadas que se hacen desde el panel "Pruébelo" durante las pruebas y las llamadas que se hacen desde una aplicación en producción. Por lo tanto, las llamadas realizadas durante las pruebas pueden incurrir en cargos.
{: note}

## Procedimiento
{: #dialog-actions-call}

Complete los siguientes pasos para realizar una llamada mediante programación desde un nodo de diálogo:

1.  En el nodo de diálogo desde el que desea realizar la llamada mediante programación, abra el editor JSON.

    - Para realizar una llamada mediante programación que se ejecuta después de que se evalúe la respuesta de un nodo, abra el editor de JSON para la respuesta del nodo.

      ![Muestra cómo acceder al editor JSON asociado con una respuesta de nodo estándar. ](images/contextvar-json-response.png)

      Si el valor de **Varias respuestas** se ha **Activado** para el nodo, debe pulsar el icono **Editar respuesta** ![Editar respuesta](images/edit-slot.png) para visualizar el menú de **Opciones** ![Respuesta avanzada](images/kabob.png).

      ![Muestra cómo acceder al editor de JSON asociado a un nodo estándar que tiene varias respuestas condicionales habilitadas. ](images/contextvar-json-multi-response.png)

    Si desea visualizar o continuar con el proceso de la respuesta desde el servicio externo dentro de la misma ronda del diálogo, debe añadir un segundo para este propósito, y saltar al mismo desde este nodo.
    {: tip}

    - Para realizar una llamada que pueda ser utilizada por una ranura individual, pulse el icono **Editar ranura** ![Icono editar ranura](images/edit-slot.png) y, a continuación:

      - Para realizar una llamada de programación que se ejecute después de que la condición de la ranura se evalúe como verdadera, abra el editor de JSON asociado a la condición de la ranura.

        ![Muestra cómo acceder al editor JSON asociado a una condición de ranura. ](images/contextvar-json-slot-condition.png)

      - Para realizar una llamada mediante programación que se ejecuta después de que la ranura se haya cumplimentado de forma satisfactoria, abra el editor de JSON que está asociado con la respuesta Encontrado. Para ello, desde el menú **Opciones** ![Icono](images/kabob.png) para la ranura, pulse **Habilitar respuestas condicionales**. Para la respuesta de Encontrado, pulse el icono **Editar respuesta** ![Editar respuesta](images/edit-slot.png). Desde el menú **Opciones** ![Icono Opciones](images/kabob.png) para la respuesta de Encontrado, pulse **Abrir el editor JSON**.

        ![Muestra cómo acceder al editor JSON asociado a la respuesta condicional de una ranura. ](images/contextvar-json-slot-multi-response.png)

1.  Utilice la sintaxis siguiente para definir la llamada mediante programación.

    ```json
    {
      "context": {
        "variable_name" : "variable_value"
      },
      "actions": [
        {
          "name":"<actionName>",
          "type":"client | cloud_function | server | web_action",
          "parameters": {
            "<parameter_name>":"<parameter_value>",
            "<parameter_name>":"<parameter_value>"
          },
          "result_variable": "<result_variable_name>",
          "credentials": "<reference_to_credentials>"
        }
      ],
      "output": {
        "text": "response text"
      }
    }
    ```
    {: codeblock}

    La matriz `actions` especifica llamadas mediante programación que se realizarán desde el diálogo. Se pueden definir hasta cinco llamadas distintas mediante programación. Especifique los siguientes pares de nombre y valor en la matriz JSON:

    - `<actionName>`: Necesario. Nombre de la acción o servicio para la llamada. El nombre no puede tener más de 256 caracteres.

       - Para los tipos de acción de cliente, especifique un nombre en la sintaxis que desee. El objetivo es especificar un nombre que su aplicación de cliente pueda reconocer y sepa cómo manejar.

          Por ejemplo: `calculateRate`

       - Para los tipos cloud_function (o server) y web_action, utilice esta sintaxis para proporcionar el nombre completo de la acción: `/<namespace>/[<package-name>]/<action name>`

         - Si una acción estándar es parte de un paquete, entonces se necesita la información del `<package-name>`. De lo contrario, no es necesario un nombre de paquete.
         - Si una acción web es parte de un paquete, entonces se necesita la información del `<package-name>`. De lo contrario, se necesita el nombre de paquete `default`, que se aplica a las acciones web que no tienen un nombre de paquete.
         - Si está llamando a una secuencia de acciones, especifique el `<sequence name>` en lugar del `<action name>`.
         - El espacio de nombres para una acción definida por el usuario habitualmente tiene la sintaxis: `<myIBMCloudOrganizationID>_<myIBMCloudSpace>`. Por ejemplo: `/jdoeorg_prod10/search flights`
         - Las acciones que se proporcionan con {{site.data.keyword.openwhisk_short}} suelen tener el espacio de nombres: `whisk.system`, no obstante, siempre verifique antes el espacio de nombres para asegurarse. Por ejemplo: `/whisk.system/weather/forecast`

           Consulte las [directrices de denominación de funciones de IBM Cloud ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/docs/openwhisk/openwhisk_reference#openwhisk_entities) para obtener más información.

    - `<type>`: Indica el tipo de llamada a hacer. Elija entre uno de los siguientes tipos:

      - **client**: Envía una respuesta de mensaje con información de llamada mediante programación en un formato estándar que entiende la aplicación cliente externa. La aplicación cliente debe utilizar la información proporcionada para ejecutar la llamada o función de programación y devolver el resultado al diálogo. El objeto JSON del cuerpo de la respuesta especifica el servicio o función que se va a llamar, los parámetros asociados que se pasarán con la llamada y el formato del resultado que se devolverá.

      - **cloud_function**: Llama directamente a una o varias acciones de {{site.data.keyword.openwhisk_short}}. Debe definir la propia acción de forma separada con {{site.data.keyword.openwhisk}}. Para obtener más información, consulte [Creación de una acción](#dialog-actions-create). (Este tipo solía recibir el nombre de **server**. El tipo **server** sigue recibiendo soporte).

      - **web_action**: Llama directamente a una o varias acciones de {{site.data.keyword.openwhisk_short}}. Debe definir la propia acción web de forma separada con {{site.data.keyword.openwhisk}}. Para obtener más información, consulte [Creación de una acción](#dialog-actions-create).

      Es opcional especificar el tipo. El valor predeterminado es `client`.

    - `<action_parameters>`: Parámetros que espera el programa externo, que se especifican como un objeto JSON. Los parámetros sólo son necesarios si los precisa el programa externo.

    - `<result_variable_name>`: Nombre que se utiliza para hacer referencia al objeto JSON el servicio o programa externo devuelve. El resultado se añade a la sección de contexto de la respuesta /message. En otras palabras, el resultado se almacena como una variable de contexto de modo que pueda visualizarse en la respuesta del nodo o al que más tarde puedan acceder los nodos del diálogo que se activen. Este valor que devuelve la acción sobrescribe cualquier otro valor existente para la variable de contexto. Puede especificar `result_variable_name` utilizando la siguiente sintaxis:

      - `my_result`
      - `$my_result`

      El nombre no puede tener más de 64 caracteres. El nombre de la variable no puede contener los siguientes caracteres: paréntesis `()`, corchetes (`[]`), comillas simples (`'`), comillas dobles (`"`) o barra inclinada invertida (`\`).

      Si desea guardar el resultado en la sección de salida o entrada de la respuesta de /message, puede añadir una de las siguientes palabras claves de ubicación a `result_variable_name` como prefijo:

       - `output.`: Añade el resultado a la sección de salida de la respuesta /message. Por ejemplo, `output.my_result`.
       - `input.`: Añade el resultado a la sección de entrada de la respuesta /message. Por ejemplo, `input.my_result`.

      También es posible especificar un prefijo de palabra clave de ubicación `context.`. Por ejemplo, `context.my_result`. Sin embargo, no es necesario porque de forma predeterminada el resultado se añade al contexto.

      Puede incluir puntos en el nombre de variable para crear un objeto JSON anidado. Por ejemplo, puede definir estas variables para capturar los resultados de dos solicitudes independientes para un servicio meteorológico para las previsiones para hoy y mañana:

      - `context.weather.today`
      - `context.weather.tomorrow`

      Los resultados (los valores del parámetro `temp` y `rain`) se almacenan en el contexto en esta estructura:

      ```json
      {
        "weather": {
          "today": {
            "temp": "20",
            "rain": "30"
          },
          "tomorrow": {
            "temp": "23",
            "rain": "80"
          }
        }
      }
      ```
      {: codeblock}

      Si varias acciones en una única matriz de acciones JSON añaden el resultado de su llamada mediante programación a la misma variable de contexto, entonces el orden en el que se actualiza el contexto es importante:

      1.  Si tiene una combinación de acciones de servidor (cloud_function o web_action) y de cliente en la matriz, el servicio procesa primero las acciones de tipo servidor (cloud_function o web_action). Como consecuencia, el valor que se calcula para la variable de contexto por la última acción de tipo cliente en la matriz sobrescribe el valor calculado para la misma por cualquier otra acción de tipo servidor.

      1.  Por tipo de acción, el orden en el que las acciones se definen en la matriz determina el orden en el que se establece el valor de la variable de contexto. El valor de la variable de contexto devuelto por la última acción en la matriz sobrescribe los valores calculados por cualquier otra acción.

    - `<reference_to_credentials>`: Nombre del objeto en el que se almacenan las credenciales de {{site.data.keyword.openwhisk_short}}. Solo es necesario para las acciones de tipo servidor o cloud_function.

      Estas credenciales se utilizan para acceder a la instancia de {{site.data.keyword.openwhisk_short}} en la que se ejecuta la acción. No se trata de sus credenciales de {{site.data.keyword.Bluemix_notm}}.

      Para descubrir las credenciales, siga estos pasos:
      1.  Vaya a la página [Clave de API de {{site.data.keyword.openwhisk_short}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/openwhisk/learn/api-key){: new_window}.

          - Si todavía no ha creado una cuenta, hágalo.
          - Si no ha iniciado una sesión, hágalo.

      1.  Pulse el icono **Mostrar clave de autorización** ![Mostrar clave de autorización](images/show-auth-icon.png) para mostrar las credenciales. El segmento anterior a los dos puntos (:) es el ID de usuario. El segmento posterior a los dos puntos es la contraseña.

      Todos los cargos incurridos cuando se ejecuta la acción se cargan a la persona propietaria de dichas credenciales.
      {: note}

      Cuando se utilizan integraciones incorporadas para desplegar el asistente, no hay forma de pasar las credenciales al diálogo. Debe almacenarlas como valores de variable de contexto en un nodo de diálogo que se activará antes de que se haya realizado la llamada mediante programación propiamente dicha. Como resultado, las credenciales resultan visibles en el archivo JSON que representa el conocimiento, que cualquiera con acceso a sus conocimientos puede descargar.
      {: important}

      Puede evitar que se almacene la información que se almacena en los registros de Watson anidando su variable de contexto dentro de la sección $private del contexto del mensaje. Por ejemplo: `$private.my_credentials`. Sin embargo, si se almacenan las credenciales en el objeto privado solo se ocultan de los registros. La información sigue almacenada en el objeto JSON subyacente. No permita que esta información quede expuesta a la aplicación cliente.

      Tenga en cuenta la posibilidad de utilizar uno de estos métodos para proteger las credenciales:

      - Si utiliza una aplicación cliente personalizada, implemente una arquitectura que impida que la aplicación cliente llame directamente a la API. Por ejemplo, utilice un servidor de apps que llame al punto final REST de la API de {{site.data.keyword.conversationshort}} y solo pase el objeto de salida JSON del servidor de apps a la aplicación cliente.
      - Utilice acciones web sin autenticación.
      - Limite la exposición autenticando la llamada con una señal específica únicamente de la acción web.

      El objeto de credenciales que defina debe contener credenciales válidas de {{site.data.keyword.openwhisk_short}}. La forma en que las debe especificar varía en función de la ubicación en la que esté alojado el servicio y del método de autenticación que utilicen las instancias en dicha ubicación. Los métodos incluyen:

      ```json
      {
        "user":"5tj3b41j-bf3j-5d92-24g9-4a7769ab12af",
        "password":"y65gqSTSRzqE..."
      }
      ```
      {: codeblock}

      o

      ```json
      {
        "api_key":"user:password"
      }
      ```
      {: codeblock}

      Al probar el diálogo, puede establecer de forma temporal la variable de contexto `$private.my_credentials` con los valores reales de nombre de usuario y contraseña de {{site.data.keyword.openwhisk_short}} pulsando **Gestionar contexto** en el panel "Pruébelo" en la herramienta.

      ![Muestra cómo la variable de contexto $private.my_credentials se define en la interfaz de gestión de contexto de Pruébelo](images/testing-creds.png)

      {{site.data.keyword.openwhisk_short}} no distingue entre las llamadas que hacen desde el panel "Pruébelo" durante las pruebas y las llamadas que se hacen desde una aplicación en producción. Las llamadas realizadas durante las pruebas pueden incurrir en cargos.
      {: note}

## Creación de una acción
{: #dialog-actions-create}

Si elige definir una llamada mediante programación de tipo acción o acción web, para poder llamarla desde un diálogo debe crearla en {{site.data.keyword.openwhisk}}. Si está definiendo una llamada de programación de tipo de cliente, omita este procedimiento.

**Restricciones de ubicación**: Actualmente, puede llamar a una acción de {{site.data.keyword.openwhisk_short}} desde instancias del servicio {{site.data.keyword.conversationshort}} alojadas únicamente en los centros de datos de las ubicaciones Dallas, Frankfurt, Londres y Washington DC. El servicio {{site.data.keyword.conversationshort}} solo utiliza la instancia de {{site.data.keyword.openwhisk_short}} alojada en la misma ubicación. No comprueba las instancias de {{site.data.keyword.openwhisk_short}} alojadas en otras ubicaciones. Por lo tanto, no llame a una acción desde una instancia de servicio {{site.data.keyword.conversationshort}} alojada en Dallas si la acción se ha definido en una instancia de {{site.data.keyword.openwhisk_short}} alojada en Washington, DC, por ejemplo. Tenga en cuenta que las instancias del servicio {{site.data.keyword.conversationshort}} que se crearon en Londres antes del 13 de diciembre de 2018 se han sindicado al centro de datos de Dallas. Estas instancias solo pueden encontrar acciones de {{site.data.keyword.openwhisk_short}} que también estén alojadas en Dallas.
{: important}

**Límites de tiempo**: Utilice únicamente los tipos **cloud_function**, **server** y
**web_action** para realizar una llamada que sepa que puede devolver información en **menos de 5 segundos**. Se excede el tiempo de espera de las solicitudes para {{site.data.keyword.openwhisk_short}} si una llamada de servicio tarda más que eso. Y si el diálogo realiza más de una llamada a un servicio externo, la cantidad total de tiempo permitido para completar las llamadas es de 7 segundos. Si las tres primeras llamadas se completan en 2 segundos cada una de ellas, y la cuarta tarda más de 1 segundo, entonces se detiene la cuarta llamada y se genera un mensaje de error para dicha llamada indicando que esta no se completó. Si tiene que llamar a servicios menos eficientes, gestione la llamada a través de la aplicación de cliente y pase la información al diálogo como un paso separado.

Siga los siguientes pasos para crear una acción de {{site.data.keyword.openwhisk_short}}:

1.  Vaya al [editor de {{site.data.keyword.openwhisk_short}} en línea ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/openwhisk/create){: new_window}, donde puede escribir directamente su código en su navegador.

    También es posible instalar una [interfaz de línea de mandato ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/openwhisk/learn/cli){: new_window} que permite definir una acción con el código que escribe localmente.

1.  Cree uno de los siguientes tipos de acciones:

    - **Acción de {{site.data.keyword.openwhisk_short}}**: Consulte el apartado [Creación e invocación de acciones ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/openwhisk?topic=cloud-functions-openwhisk_actions){: new_window} para obtener más información.
    - **Acción web de {{site.data.keyword.openwhisk_short}}**: Consulte el apartado [Creación de acciones web ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions){: new_window} para obtener más información.

    Tenga en cuenta lo siguiente:

    - Consulte el [ejemplo de una acción de {{site.data.keyword.openwhisk_short}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/docs/openwhisk/openwhisk_actions#openwhisk_apicall_action){: new_window} para ver cómo llamar a un servicio externo.
    - Para realizar una llamada a un servicio Watson, utilice el [Watson Developer Cloud SDK ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/watson-developer-cloud){: new_window} para el lenguaje de programación que desee utilizar.
    - Asegúrese de que su acción de {{site.data.keyword.openwhisk_short}} acepte los parámetros de entrada como un objeto JSON, y que también devuelva su salida como un objeto JSON.
    - Si está utilizando Node.js para escribir su acción de {{site.data.keyword.openwhisk_short}}, asegúrese de utilizar `Promise` para el proceso asíncrono. También asegúrese de devolver el resultado final de la función `main`.

    Como alternativa, puede crear una secuencia de acciones.
    {: tip}

## Manejo de errores
{: #dialog-actions-handle-errors}

Si la acción de {{site.data.keyword.openwhisk_short}} encuentra un error, el mensaje de error se devuelve al diálogo y se almacena como una propiedad de la variable de respuesta denominada `cloud_functions_call_error`. El error podría darse si su acción de {{site.data.keyword.openwhisk_short}} no puede obtener una respuesta para un servicio externo, o si por ejemplo la acción de Cloud Function falla. Si no se proporcionan las credenciales de Cloud Function o si son incorrectas, se devuelve un error. Esta variable de contexto únicamente se utiliza para acciones de servidor; en la aplicación cliente, considere crear un objeto similar que capture información de error y la devuelva al diálogo como una variable de contexto.

Puede condicionar la respuesta del nodo del diálogo para comprobar en primer lugar si ha habido errores. Por ejemplo, puede asegurarse de que únicamente se muestre una respuesta que haga referencia a una acción de {{site.data.keyword.openwhisk_short}} si no hay errores. Para ello, podría añadir esta expresión a la condición de la respuesta:

```bash
  $forecast_result.cloud_functions_call_error == null
```
{: codeblock}

Para una llamada mediante programación de tipo cliente, podría pasar información sobre el proceso de errores definiendo una variable de contexto como, por ejemplo, `action_error`. Podría pasarla de nuevo al servicio como parte de la variable de resultado. A continuación, únicamente visualizar la respuesta si no se hubiesen encontrado errores definiendo para ello una condición de respuesta como esta:

```bash
  $forecast_result.action_error == null
```
{: codeblock}

## Ejemplo de llamada de cliente
{: #dialog-actions-client-example}

En el siguiente ejemplo se muestra un ejemplo de una llamada a un servicio meteorológico externo. Se añade al editor JSON que está asociado con la respuesta del nodo. En el momento que se desencadenan la respuesta a nivel de nodo, las ranuras han recopilado y almacenado la información de ubicación y fecha del usuario. En este ejemplo se presupone que se llamará al servicio como un punto final denominado `/weather`, con los parámetros `location` y `date`, y que devolverá un objeto JSON, `{"forecast": "<value>"}`.

 ``` json
{
  "actions": [
    {
      "name": "MyWeatherFunction",
      "type": "client",
      "parameters": {
        "date": "$date",
        "location": "$location"
      },
      "result_variable": "context.my_forecast"
    }
  ]
}
```
{: codeblock}

Normalmente, el servicio solo vuelve al cliente desde una solicitud /message POST cuando se necesita una nueva entrada del usuario, como por ejemplo después de ejecutar un nodo padre y antes de ejecutar uno de sus nodos hijo. Sin embargo, si añade una acción de cliente a un nodo, después de la evaluación, el servicio siempre vuelve al cliente para poder devolver el resultado de la llamada de acción. Para evitar esperar una entrada de usuario cuando no debería ser así, como cuando un nodo está configurado para saltar directamente a un nodo hijo, el servicio añade el siguiente valor al contexto del mensaje:

```json
  {
    "context": {
      "skip_user_input": true
    }
  }
```
{: codeblock}

Si desea que el cliente realice una acción, sin obtener una entrada de usuario, puede seguir la misma convención, y añadir la variable de contexto `skip_user_input` al nodo padre para comunicar este hecho a la aplicación de cliente.

Su aplicación cliente siempre debe comprobar la variable `skip_user_input` en el contexto. Si está presente, sabrá que no debe solicitar una nueva entrada por parte del usuario sino ejecutar la acción, y añadir su resultado al mensaje para pasarlo de vuelta al servicio. La nueva solicitud del mensaje POST debería incluir el mensaje devuelto por la respuesta de mensaje POST anterior (es decir, el contexto, la entrada, las intenciones, las entidades y, de forma opcional, la sección de salida) y, en lugar del objeto JSON que define la llamada mediante programación a realizar, debería incluir el resultado devuelto desde la llamada mediante programación.

En un nodo al que salte después de este nodo, añadiría la respuesta para mostrarla al usuario:

``` json
{
  "output": {
    "text": {
      "values": [
        "It will be $my_forecast $date.literal in $location.literal."
      ]
    }
  }
```
{: codeblock}

En el siguiente diagrama de muestra cómo utilizar una llamada de cliente para obtener la información de previsión meteorológica y cómo devolverla al usuario.

![Muestra a un usuario solicitando una previsión meteorológica, y cómo el diálogo envía la solicitud a una app de cliente, la cual la envía a un servicio externo](images/forecast.png)

## Ejemplo de llamada a acción de IBM Cloud Functions
{: #dialog-actions-server-example}

En el siguiente ejemplo se muestra una llamada a una acción {{site.data.keyword.openwhisk_short}}. Este ejemplo muestra cómo utilizar la acción `echo` de {{site.data.keyword.openwhisk_short}} que se definió en el [paquete de programas de utilidad ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/docs/openwhisk/openwhisk_actions#openwhisk_create_action_sequence){: new_window} proporcionado con el servicio. La acción toma una serie de texto y la devuelve.

``` json
{
  "actions": [
    {
      "name": "/whisk.system/utils/echo",
      "type":"cloud_function",
      "parameters": {
        "message": "<?input.text?>"
      },
      "result_variable": "context.my_input_returned",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

Nodos de diálogo posteriores podrán ahora acceder a la salida de la acción de {{site.data.keyword.openwhisk_short}}.

``` json
 {
  "output": {
    "text": {
      "values": [
        "Your input was: $my_input_returned."
      ]
    }
  }
}
```
{: codeblock}

En el siguiente diagrama se muestra cómo llamar a una acción de {{site.data.keyword.openwhisk_short}} con un ejemplo simple que llama al servicio incorporado echo de {{site.data.keyword.openwhisk_short}}. Solicita al usuario una entrada que pasa al servicio echo. El servicio echo devuelve el mismo texto, que se visualiza al usuario.

![Muestra la entrada del usuario, y el diálogo enviando la solicitud al servicio de y luego devolviendo el texto al diálogo.](images/echo-via-cf.png)

### Ejemplo de acción echo
{: #dialog-actions-echo-example}

Para ver un conocimiento de diálogo con un diálogo que ya está configurado para que llame a la acción Echo integrada de {{site.data.keyword.openwhisk_short}}, siga estos pasos:

1.  Descargue el archivo [CloudFunctionsEcho.json ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://raw.githubusercontent.com/watson-developer-cloud/community/master/watson-assistant/cloud-functions-echo.json){: new_window}.
1.  Importe el archivo JSON como un nuevo conocimiento de diálogo.
1.  Revise el diálogo para ver cómo se especifica la llamada a la acción Echo.
1.  Desde el panel "Pruébelo", pulse **Gestionar contexto** y, a continuación establezca de forma temporal las variables de contexto para su nombre de usuario y contraseña de {{site.data.keyword.openwhisk_short}}.

    ```json
    $private.my_credentials
    {
      "user":"<your-CF-instance-username>",
      "password":"<your-CF-instance-password>"
    }
    ```
    {: codeblock}

    o

    ```json
    $private.my_credentials
    {
      "api_key":"user:password"
    }
    ```
    {: codeblock}

1.  Pruebe el diálogo especificando alguna entrada.

    El servicio utilizará la acción Echo de {{site.data.keyword.openwhisk_short}} para repetir de nuevo lo que haya especificado.

## Ejemplo de llamada a acción web de IBM Cloud Functions
{: #dialog-actions-web-action-example}

En el siguiente ejemplo se muestra un ejemplo de una llamada a una acción web. En este ejemplo se muestra cómo pasar un nombre de usuario a un servicio de mensaje de bienvenida sencillo. El servicio devuelve un mensaje de bienvenida que se dirige al usuario por su nombre.

  ``` json
  {
    "actions": [
        {
        "name": "jdoe@example.com_sales/default/hello",
        "type":"web_action",
        "parameters": {
          "name": "<? context['name'] ?>"
        },
        "result_variable": "context.greet_user",
        "credentials": "<See below>"
      }
    ]
  }
  ```
  {: codeblock}

  donde `"credentials"` se especifica de una de las formas siguientes:

  - Sin autenticación:

    ```json
    "credentials": null
    ```
    {: screen}

  - Autenticación mediante credenciales de {{site.data.keyword.openwhisk_short}}

    ```json
    "credentials": "$private.my_credentials"
    ```
    {: screen}

    donde `$private.my_credentials` se define del siguiente modo:

    ```json
    $private.my_credentials
    {
      "user":"<your-CF-instance-username>",
      "password":"<your-CF-instance-password>"
    }
    ```
    {: screen}

    o 

    ```json
    $private.my_credentials
    {
      "api_key":"username:password"
    }
    ```
    {: screen}
  
  - Autenticación con una señal específica únicamente de la acción web

    ```json
    "credentials": "$private.my_credentials"
    ```
    {: screen}

    donde `$private.my_credentials` se define del siguiente modo:

    ```json
    $private.my_credentials
    {
      "web_action_key":"<action-secret>"
    }
    ```
    {: screen}

Ahora los siguientes nodos de diálogo pueden acceder al resultado de la acción web, que se guarda en la variable `context.greet_user`.

``` json
 {
  "output": {
    "text": {
      "values": [
        "$greet_user"
      ]
    }
  }
}
```
{: codeblock}

## Ejemplo avanzado de llamada a acción de IBM Cloud Functions
{: #dialog-actions-advanced-example}

Puede llamar a varias acciones desde un flujo de diálogo individual. De hecho, es posible llamar hasta cinco acciones con un objeto JSON `actions` en un nodo de diálogo individual. Tenga en cuenta que todas las acciones de tipo servidor definidas en la matriz JSON `actions` se procesan en su totalidad en paralelo. Por lo tanto, no puede llamar a una acción de tipo servidor y pasar el resultado desde la misma a una segunda acción de tipo servidor en el mismo bloque `actions`. La mejor manera de llamar a acciones de servidor en una orden específico es utilizar la secuencia de {{site.data.keyword.openwhisk_short}}. En tiempo de ejecución, esta aproximación es más rápida porque el diálogo solo tiene que realizar una llamada externa para completar varias acciones. Para utilizar una secuencia, tan solo haga referencia al nombre de secuencia en lugar de un nombre de acción en la definición del bloque `actions`. Como alternativa, puede llamar a la primera acción de tipo servidor desde un nodo y saltar a un nodo hijo que llame a la siguiente acción de tipo servidor.

Si define una matriz `actions` con una combinación de acciones de tipo servidor y cliente, cuando se ejecuta el nodo de diálogo, las acciones de cliente no se envían al cliente hasta que no se hayan procesado todas las acciones de tipo servidor.
{: tip}

En los siguientes ejemplos se muestra un ejemplo de una llamada a una acción de {{site.data.keyword.openwhisk_short}}.

En este ejemplo, se muestra cómo utilizar la acción de {{site.data.keyword.openwhisk_short}} para llamar a un servicio externo que toma un nombre de ciudad y devuelve las coordenadas de latitud y longitud de la ubicación proporcionada.

``` json
{
  "actions": [
        {
      "name": "/jdoeorg_prod/get coordinates",
      "type":"cloud_function",
      "parameters": {
        "location": "$location"
      },
      "result_variable": "context.my_coordinates",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

La variable de contexto `$my_coordinates` guarda los dos valores que devuelve el servicio `get coordinates`:

```json
{
 "lat":"42.3611",
 "long":"-71.0571"
}
```
{: codeblock}

En este ejemplo se muestra cómo utilizar la acción de {{site.data.keyword.openwhisk_short}} `forecast` definida en el [paquete Weather ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/docs/openwhisk/openwhisk_weather#openwhisk_catalog_weather){: new_window} que se proporciona con el servicio {{site.data.keyword.openwhisk_short}}. La acción espera coordenadas de latitud y longitud, y un periodo de tiempo. Devuelve un objeto JSON con información de previsión para la ubicación especificada durante el periodo de tiempo especificado. Las coordenadas, devueltas por la acción anterior, se especifican como `$my_coordinates.lat` y `$my_coordinates.long`.

``` json
{
  "actions": [
    {
      "name": "/whisk.system/weather/forecast",
      "type":"cloud_function",
      "parameters": {
        "latitude": "$my_coordinates.lat",
        "longitude": "$my_coordinates.long",
        "timePeriod": "$period",
        "username": "$private.my_weather_service.username",
        "password": "$private.my_weather_service.password"
      },
      "result_variable": "context.forecasts",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

Se lista un nombre de usuario y una contraseña como parámetros. Son los únicos que hay porque esta acción concreta los necesita para definir las credenciales que a su vez necesita el servicio Weather externo al que llama la acción proporcionada en un sistema de fondo. Estas credenciales son diferentes de las credenciales de cuenta de IBM Cloud Function. Tome también las medidas necesarias para proteger estas credenciales.
{: note}

Ahora los siguientes nodos de diálogo pueden acceder al resultado de la acción {{site.data.keyword.openwhisk_short}}, que se guarda en la variable `context.forecasts`.

``` json
 {
  "output": {
    "text": {
      "values": [
        "For the next $period in $location, you can expect $forecasts."
      ]
    }
  }
}
```
{: codeblock}

En el siguiente diagrama se muestra una interacción compleja. Un usuario solicita una previsión meteorológica. Las ranuras en el nodo del diálogo solicitarán al usuario la información de ubicación y período de tiempo. Para llamar al servicio de previsión meteorológica de {{site.data.keyword.openwhisk_short}}, se deben proporcionar coordinadas geográficas. Por lo tanto, en primer lugar el diálogo obtiene los detalles de latitud y longitud de la ubicación que proporciona el usuario. Para ello, envía una solicitud al servicio de {{site.data.keyword.openwhisk_short}}, que a su vez, la pasa al servicio externo que devuelve la información de las coordenadas. En un nodo posterior, el diálogo envía la información recién obtenida sobre las coordinadas junto con la información del periodo de tiempo que obtuvo del usuario al servicio de previsión meteorológica incorporado de {{site.data.keyword.openwhisk_short}} para obtener los detalles de dicha previsión. El diálogo responde entonces a la pregunta del usuario mostrando el resultado de la previsión que el servicio de previsión de {{site.data.keyword.openwhisk_short}} ha proporcionado.

![Muestra una persona que pregunta sobre la previsión meteorológica y el diálogo que envía la solicitud al servicio Cloud Function para que la pase al servicio externo.](images/forecast-via-cf.png)
