---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-01"

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

# Cómo llamar a una aplicación cliente ![BETA](images/beta.png)
{: #dialog-actions-client}

Añada una acción de cliente al nodo de diálogo que pausa el diálogo para que se pueda ejecutar un proceso del lado del cliente y devolver un resultado como parte del procedimiento que tiene lugar dentro de un turno diálogo.
{: shortdesc}

Una acción de cliente define una llamada programática en un formato estandarizado que su aplicación de cliente externo puede entender. La aplicación cliente externo debe utilizar la información proporcionada para ejecutar la llamada o función de programación y devolver el resultado al diálogo.

Esta acción no realiza una llamada directa propiamente. Básicamente, indica al diálogo que haga una pausa en ese punto y que espere a que la aplicación cliente externa haga algo. El programa en el que se ejecuta la aplicación cliente puede ser el que elija. Asegúrese de especificar el nombre de la llamada y los detalles de los parámetros, así como el nombre de la variable de mensaje de error, según las reglas de formato de JSON que se describen más adelante en este tema.

Puede invocar una aplicación de cliente para realizar los siguientes tipos de cosas:

- Validar la información que ha recopilado del usuario.
- Realizar cálculos o manipulaciones de series de caracteres en entradas de usuario que son demasiado complejas para que las manejen los métodos de expresión SpEL soportados.
- Obtener datos de otra aplicación o servicio.

Para obtener información sobre cómo llamar a un servicio externo, como por ejemplo una acción web de {{site.data.keyword.openwhisk_short}}, consulte [Realización de una llamada programática desde un nodo de diálogo](/docs/services/assistant?topic=assistant-dialog-webhooks).

En el siguiente diagrama de muestra cómo utilizar una llamada de cliente para obtener la información de previsión meteorológica y cómo devolverla al usuario.

![Muestra una persona que pregunta sobre la previsión meteorológica, el diálogo que envía la solicitud a una app cliente, que la envía al servicio externo](images/forecast.png)

Tenga en cuenta que la acción de cliente llama al programa `MyWeatherFunction`, que es un programa que se ejecuta en la aplicación cliente. La aplicación cliente realiza la llamada al servicio web externo (`/weather`) para obtener la información de previsión real. A continuación, la aplicación cliente devuelve la respuesta al diálogo. Cuando añade una acción de cliente a un diálogo, debe existir una aplicación cliente que realiza el procesamiento en sí o que pasa información entre su diálogo y cualquier servicio de fondo externo que desee utilizar.

## Procedimiento
{: #dialog-actions-client-call}

Complete los pasos siguientes para realizar una llamada a una aplicación cliente mediante programación desde un nodo de diálogo:

1.  En el nodo de diálogo desde el que desea realizar la llamada mediante programación, abra el editor JSON.

    - Para realizar una llamada mediante programación que se ejecuta después de que se evalúe la respuesta de un nodo, abra el editor de JSON para la respuesta del nodo.

      ![Muestra cómo acceder al editor JSON asociado con una respuesta de nodo estándar. ](images/contextvar-json-response.png)

      Si el valor de **Varias respuestas** se ha **Activado** para el nodo, debe pulsar el icono **Editar respuesta** ![Editar respuesta](images/edit-slot.png) para visualizar el menú de **Opciones** ![Respuesta avanzada](images/kabob.png).

      ![Muestra cómo acceder al editor de JSON asociado a un nodo estándar que tiene varias respuestas condicionales habilitadas. ](images/contextvar-json-multi-response.png)

    Si desea visualizar o continuar con el proceso de la respuesta desde dentro de la misma ronda del diálogo, debe añadir un segundo para este propósito, y saltar al mismo desde este nodo.
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
          "type":"client",
          "parameters": {
            "<parameter_name>":"<parameter_value>",
            "<parameter_name>":"<parameter_value>"
          },
          "result_variable": "<result_variable_name>"
        }
      ],
      "output": {
        "text": "response text"
      }
    }
    ```
    {: codeblock}

    La matriz `actions` especifica llamadas mediante programación que se realizarán desde el diálogo. Se pueden definir hasta cinco llamadas distintas mediante programación. Especifique los siguientes pares de nombre y valor en la matriz JSON:

    - `<actionName>`: Necesario. Nombre de la acción o servicio para la llamada. Especifique un nombre en la sintaxis que desee. El objetivo es especificar un nombre que su aplicación de cliente pueda reconocer y sepa cómo manejar. El nombre no puede tener más de 256 caracteres.

      Por ejemplo: `calculateRate`

    - `<type>`: Indica el tipo de llamada a hacer. Opcionalmente, especifique `client`, que es el valor predeterminado.

      Envía una respuesta de mensaje con información de llamada mediante programación en un formato estándar que entiende la aplicación cliente externa. La aplicación cliente debe utilizar la información proporcionada para ejecutar la llamada o función de programación y devolver el resultado al diálogo. El objeto JSON del cuerpo de la respuesta especifica el servicio o función que se va a llamar, los parámetros asociados que se pasarán con la llamada y el formato del resultado que se devolverá.

    - `<action_parameters>`: Parámetros que espera el programa externo, que se especifican como un objeto JSON. Los parámetros sólo son necesarios si los precisa el programa externo.

    - `<result_variable_name>`: Nombre que se utiliza para hacer referencia al objeto JSON el servicio o programa externo devuelve. El resultado se añade a la sección de contexto de la respuesta `/message`. En otras palabras, el resultado se almacena como una variable de contexto de modo que pueda visualizarse en la respuesta del nodo o al que más tarde puedan acceder los nodos del diálogo que se activen. Este valor que devuelve la acción sobrescribe cualquier otro valor existente para la variable de contexto. Puede especificar `result_variable_name` utilizando la siguiente sintaxis:

      - `my_result`
      - `$my_result`

      El nombre no puede tener más de 64 caracteres. El nombre de la variable no puede contener los siguientes caracteres: paréntesis `()`, corchetes (`[]`), comillas simples (`'`), comillas dobles (`"`) o barra inclinada invertida (`\`).

      Si desea guardar el resultado en la sección de salida o entrada de la respuesta de `/message`, puede añadir una de las siguientes palabras claves de ubicación a `result_variable_name` como prefijo:

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

      Si varias acciones en una única matriz de acciones JSON añaden el resultado de su llamada mediante programación a la misma variable de contexto, entonces el orden en el que se actualiza el contexto es importante. Por tipo de acción, el orden en el que las acciones se definen en la matriz determina el orden en el que se establece el valor de la variable de contexto. El valor de la variable de contexto devuelto por la última acción en la matriz sobrescribe los valores calculados por cualquier otra acción.

## Ejemplo de llamada de cliente
{: #dialog-actions-client-example}

En el siguiente ejemplo se muestra un ejemplo de una llamada a un servicio meteorológico externo. Se añade al editor JSON que está asociado con la respuesta del nodo. En el momento que se desencadenan la respuesta a nivel de nodo, las ranuras han recopilado y almacenado la información de ubicación y fecha del usuario. En este ejemplo se presupone que el programa que se invocará se llama `MyWeatherFunction`, y que acepta los parámetros `location` (ubicación) y `date` (fecha) y devuelve un objeto JSON, `{"forecast": "<value>"}`.

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

Normalmente, el servicio solo vuelve al cliente desde una solicitud `/message` POST cuando se necesita una nueva entrada del usuario, como por ejemplo después de ejecutar un nodo padre y antes de ejecutar uno de sus nodos hijo. Sin embargo, si añade una acción de cliente a un nodo, después de la evaluación, el servicio siempre vuelve al cliente para poder devolver el resultado de la llamada de acción. Para evitar esperar una entrada de usuario cuando no debería ser así, como cuando un nodo está configurado para saltar directamente a un nodo hijo, el servicio añade el siguiente valor al contexto del mensaje:

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
