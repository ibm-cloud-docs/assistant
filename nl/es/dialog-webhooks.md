---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-09"

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

# Realizar una llamada mediante programa desde un nodo de diálogo
{: #dialog-webhooks}

Para realizar una llamada mediante programa, defina un webhook (enganche web) que envíe una llamada a una solicitud POST a una aplicación externa que realice una función mediante programa. A continuación, puede invocar el webhook desde uno o más nodos de diálogo.

Un webhook es un mecanismo que permite llamar a un programa externo basado en algo que está ocurriendo en tu programa. Cuando se utiliza en un conocimiento de diálogo, se desencadena un webhook cuando el asistente procesa un nodo que tiene habilitado un webhook. El webhook recopila los datos que especifique o que se recopilen del usuario durante la conversación, y se guardan en las variables de contexto. Envía los datos como parte de una solicitud POST de HTTP al URL que especifique como parte de su definición de webhook. El URL que recibe el webhook es el elemento de escucha. Realiza una acción predefinida utilizando la información que se le pasa tal como se especifica en la definición de webhook y, opcionalmente, puede devolver una respuesta.

Vea este vídeo para obtener más información.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Demostración de webhooks (enganches de web)" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/j8TBqD2rx2o?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Puede utilizar un webhook para realizar los siguientes tipos de cosas:

- Validar la información que ha recopilado del usuario.
- Interactuar con un servicio web externo para obtener información. Por ejemplo, puede comprobar la hora de llegada esperada de un vuelo desde un servicio de tráfico aéreo o puede obtener una previsión de un servicio meteorológico.
- Enviar solicitudes a una aplicación externa como, por ejemplo, un sitio de reservas de restaurantes, para completar una transacción simple en nombre del usuario.
- Desencadenar una notificación SMS.
- Desencadenar una acción web de {{site.data.keyword.openwhisk}}.

No se puede utilizar un webhook para llamar a una acción de {{site.data.keyword.openwhisk_short}} que utiliza la autenticación de IAM (Identity and Access Management) basada en señales.
{: note}

Para obtener información sobre cómo llamar a una aplicación cliente, consulte [Cómo llamar a una aplicación cliente desde un nodo de diálogo](/docs/services/assistant?topic=assistant-dialog-actions-client).

## Definición del webhook
{: #dialog-webhooks-create}

Puede definir un URL de webhook para un conocimiento de diálogo y, a continuación, llamar al webhook desde uno o más nodos de diálogo.

La llamada mediante programa al servicio externo debe cumplir estos requisitos:

- La llamada debe ser una solicitud POST HTTP.
- El formato de la solicitud y la respuesta debe estar en JSON. Por ejemplo: `Content-Type: application/json`.
- La llamada debe volver en **5 segundos o menos**.

  Si tiene que llamar a servicios menos eficientes, puede gestionar la llamada a través de la aplicación de cliente y pase la información al diálogo como un paso separado. Para obtener más información, consulte [Cómo llamar a una aplicación cliente desde un nodo de diálogo](/docs/services/assistant?topic=assistant-dialog-actions-client).
  {: tip}

Para añadir los detalles de webhook, complete los pasos siguientes:

1.  Desde el conocimiento en el que desea añadir el webhook, pulse el separador **Opciones**.

1.  Pulse **Webhooks**.

1.  En el campo **URL**, añada el URL de la aplicación externa a la que desea enviar llamadas de solicitud POST de HTTP.

    Por ejemplo, para llamar al servicio de Traductor de idiomas (Language Translator), especifique el URL de la instancia de servicio.

    ```bash
    https://gateway.watsonplatform.net/language-translator/api/v3/translate?version=2018-05-01
    ```
    {: codeblock}

    Si la aplicación externa a la que llama devuelve una respuesta, debe ser capaz de devolver una respuesta en formato JSON. Para el servicio Traductor de idiomas, por ejemplo, debe especificar el formato en el que desea que se devuelva el resultado. Puede hacerlo pasando una cabecera al servicio.

1.  En la sección Headers, añada las cabeceras que quiera pasar al servicio de una en una, pulsando **Añadir cabecera**.

    Por ejemplo, añada una cabecera que indique que quiere que el valor resultante se devuelva en formato JSON.

    <table>
    <caption>Ejemplo de cabecera</caption>
      <tr>
      <th>Header name</th>
      <th>Header value</th>
      </tr>
      <tr>
      <td>Content-Type</td>
      <td>application/json</td>
      </tr>
    </table>
    
1.  Si el servicio externo requiere que pase las credenciales de autenticación básica con la solicitud, proporciónelas. Pulse **Añadir autorización**, añada sus credenciales a los campos **Nombre de usuario** y **Contraseña** y, a continuación, pulse **Guardar**. 

    El producto crea una serie ASCII codificada en base 64 a partir de las credenciales y genera una cabecera que se añade a la página. 

    <table>
    <caption>Ejemplo de cabecera</caption>
      <tr>
      <th>Header name</th>
      <th>Header value</th>
      </tr>
      <tr>
      <td>Authorization</td>
      <td>Basic `<encoded-api-key>`</td>
      </tr>
    </table>
    
    Para probar, puede enviar una clave de API sobre autenticación básica. Pulse **Añadir autorización** y, a continuación, añada `apikey` al campo **Nombre de usuario** y pegue el valor de la clave de API en el campo **Contraseña**. Pulse **Guardar**.
    {: tip}

Los detalles de su webhook se guardan de forma automática.

## Agregar una llamada de webhook a un nodo de diálogo
{: #dialog-webhooks-dialog-node-callout}

Para utilizar un webhook desde un nodo de diálogo, debe habilitar los webhooks en el nodo y, a continuación, añadir detalles a la llamada.

1.  Pulse el separador **Diálogo**.

1.  Busque el nodo de diálogo en el que desea añadir una llamada. La llamada al webhook se producirá siempre que este nodo se desencadene durante una conversación con un usuario.

    Por ejemplo, es posible que desee enviar una llamada a un webhook desde el nodo `#General_Greetings`.

1.  Pulse para abrir el nodo de diálogo y, a continuación, pulse **Personalizar**.

1.  Desplácese hacia abajo hasta la sección *Webhooks*, cambie el conmutador a **Activado** y, a continuación, pulse **Aplicar**.

    Si no lo ha habilitado ya, el valor de *Varias respuestas condicionales* se cambia automáticamente y no puede inhabilitarlo. Este valor se habilita para permitir añadir distintas respuestas, según el éxito o fracaso de la llamada al webhook. Si ya ha especificado una respuesta para el nodo, se convierte en la primera respuesta condicional.
    {: note}

1.  Añada los datos que quiera pasar a la aplicación externa como pares de clave y valor, en la sección *Parámetros*.

    Por ejemplo, si llama al servicio Traductor de idiomas, debe proporcionar valores para los parámetros siguientes:

    <table>
    <caption>Ejemplo de parámetro</caption>
      <tr>
        <th>Clave</th>
        <th>Valor</th>
        <th>Descripción</th>
      </tr>
      <tr>
        <td>model_id</td>
        <td>"en-es"</td>
        <th>Identifica los idiomas de entrada y salida. En este ejemplo, se solicita que el texto en inglés (en) se traduzca a español (es).</th>
      </tr>
      <tr>
        <td>text</td>
        <td>"How are you?"</td>
        <th>Este parámetro contiene la serie de texto que desea que el servicio traduzca. Puede codificar este valor, pasar una variable de contexto, por ejemplo $saved_text, o pasar la entrada de usuario al servicio directamente, especificando `<? input.text ?>` como este valor.</th>
      </tr>
    </table>

    En casos de uso más complejos, podría recopilar información durante una conversación con un usuario acerca de sus planes de viaje, por ejemplo. Puede recopilar información de fechas y destinos, y guardarla en las variables de contexto que puede pasar a una aplicación externa como parámetros.

    <table>
    <caption>Ejemplo de parámetros de viaje</caption>
      <tr>
        <th>Clave</th>
        <th>Valor</th>
      </tr>
      <tr>
        <td>depart_date</td>
        <td>$departure</td>
      </tr>
    <tr>
        <td>arrive_date</td>
        <td>$arrival</td>
      </tr>
    <tr>
        <td>origin</td>
        <td>$origin</td>
      </tr>
    <tr>
        <td>destination</td>
        <td>$destination</td>
      </tr>
    </table>

1.  Cualquier respuesta realizada por la llamada se guarda en la variable de retorno. Puede cambiar el nombre de la variable que se añade automáticamente al campo **Variable de retorno**. Si la llamada da como resultado un error, esta variable se establece en `null`.

    El nombre de variable generado tiene la sintaxis `webhook_result_n`, donde el `_n` que se añade se incrementa cada vez que añade una llamada de webhook a un nodo de diálogo. Este convenio de denominación garantiza que los nombres de las variables de contexto sean exclusivos en todo el conocimiento de diálogo. Si cambia el nombre, asegúrese de utilizar un nombre exclusivo.

1.  En la sección de respuestas condicionales, se añaden automáticamente dos condiciones de respuesta, una respuesta que se muestran al llamar correctamente al webhook y se devuelve una variable de retorno. Y una respuesta que se muestra cuando la llamada falla. Puede editar estas respuestas y añadir más respuestas condicionales al nodo.

    Si la llamada devuelve una respuesta y conoce el formato de la respuesta JSON, puede editar la respuesta del nodo de diálogo para que incluya sólo la sección de la respuesta que desea compartir con los usuarios. 
    
    Por ejemplo, el servicio Traductor de idiomas devuelve un objeto como este:
    
    ```json
       {
       "translations":[
          {"translation":"¿Cómo estás?"}
       ],
       "word_count":3,
       "character_count":12
       }
    ```
    {: codeblock}
    
    Utilice una expresión SpEL que extraiga sólo el valor de texto traducido.

    <table>
    <caption>Ejemplo de respuestas condicionales</caption>
      <tr>
        <th>Condición</th>
        <th>Respuesta</th>
      </tr>
      <tr>
        <td>$webhook_result_1</td>
        <td>Su texto en español: <? $webhook_result_1.translations[0].translation ?>.</td>
      </tr>
      <tr>
        <td>anything_else</td>
        <td>La llamada a la aplicación externa ha fallado. Vuelva a intentarlo más tarde.</td>
      </tr>
    </table>

    Si utiliza el formato recomendado para la respuesta y se devuelve la respuesta de traducción mostrada anteriormente, la respuesta del asistente al usuario sería: `Su texto en español: ¿Cómo estás?`

1.  Cuando haya terminado, pulse en la X para cerrar el nodo. Los cambios se guardan automáticamente.

## Prueba de los webhooks
{: #dialog-webhooks-test}

Cuando se añade una llamada de webhook por primera vez, puede ser útil ver exactamente lo que se devuelve en la respuesta de la aplicación externa, los datos y su formato. Para ello, añada esta expresión como la respuesta de texto para la respuesta condicional de llamada satisfactoria: `$webhook_result_n` donde `n` es el número adecuado para el webhook que está probando.

Esta respuesta devuelve el cuerpo completo de la variable de retorno, de modo que puede ver lo que devuelve la llamada y decidir qué compartir con el usuario. A continuación, puede utilizar los métodos documentados en [Métodos de lenguaje de expresiones](/docs/services/assistant?topic=assistant-dialog-methods) para extraer sólo la información que le interesa de la respuesta.

Probar si determinadas entradas de usuario pueden generar errores en la llamada e incorporar maneras de manejar tales situaciones. Los errores generados por la aplicación externa se almacenan en `output.webhook_error.<result_variable>`. Puede utilizar una respuesta condicional como esta mientras prueba la captura de dichos errores:

| Condición | Respuesta |
|-----------|----------|
| output.webhook_error | La llamada ha generado este error: <? output.webhook_error.webhook_result_1 ?>. |

Por ejemplo, podría no autenticar la solicitud adecuadamente (401), o bien podría intentar pasar un parámetro con un nombre que ya se esté utilizando por parte de una aplicación externa. Pruebe el webhook para descubrir y arreglar estos tipos de errores antes de desplegar el webhook.

## Eliminar un webhook
{: #dialog-webhooks-delete}

Si decide que no desea realizar una llamada de webhook desde un nodo de diálogo, abra la página *Personalizar* del nodo y, a continuación, cambie Webhooks a **Desactivado**.

La sección *Parámetros* y el campo **Variable de retorno** se eliminan del editor del nodo de diálogo. Sin embargo, las respuestas condicionales que se han añadido automáticamente o que usted mismo haya añadido, se conservan.

La sección *Varias respuestas condicionadas* vuelve a ser editable. Puede elegir desactivar la característica. Si lo hace, sólo se guardará la primera respuesta condicional como la única respuesta de texto del nodo.

Para cambiar el servicio externo que llama desde los nodos de diálogo, edite los detalles de webhook definidos en la página de Webhooks del separador **Opciones**. Si el nuevo servicio espera que se le pasen parámetros distintos, asegúrese de actualizar los nodos de diálogo que lo llaman.

## Cómo llamar a IBM Cloud Functions
{: #dialog-webhooks-cf}

Puede escribir el URL de webhook y proporcionar cabeceras de forma diferente en función de si está llamando a una acción estándar o a una acción web.

### Cómo llamar a una acción web
{: #dialog-webhooks-cf-web-action}

Las sugerencias siguientes le ayudarán a llamar a una acción web de {{site.data.keyword.openwhisk_short}} desde su diálogo. 

1.  Abra la página **Opciones** para el conocimiento y, a continuación, pulse en **Webhooks**.

1.  En el campo **URL**, añada el URL de la aplicación externa a la que desea enviar llamadas de solicitud POST de HTTP.

    Por ejemplo, para llamar a una acción web de {{site.data.keyword.openwhisk_short}}, especifique el URL para la acción web pública. Por ejemplo:

    ```bash
    https://us-south.functions.cloud.ibm.com/api/v1/web/my_org_dev/default/Hello%20World.json
    ```
    {: codeblock}

    Si la aplicación externa a la que llama devuelve una respuesta, debe ser capaz de devolver una respuesta en formato JSON.

    Tenga en cuenta que el URL de la solicitud en este ejemplo finaliza en `.json`. Al especificar esta extensión, aprovecha una característica de acciones web que le permite especificar el tipo de contenido que quiera de la respuesta. La especificación de este tipo de extensión garantiza que, si las acciones web pueden devolver respuestas en más de un formato, se devolverá una respuesta JSON. Para obtener más detalles, consulte [Características adicionales](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_extra){: new_window}.
    {: tip}

1.  No es necesario añadir cabeceras.

    Las acciones web de {{site.data.keyword.openwhisk_short}} no necesitan ser autenticadas, por lo que no es necesario que defina una cabecera de autorización.

    Los detalles de su webhook se guardan de forma automática.

1.  Pulse el separador **Diálogo**.

1.  Pulse esta opción para abrir el nodo de diálogo desde el que desea llamar a la acción web y, a continuación, pulse **Personalizar**.

1.  Desplácese hacia abajo hasta la sección *Webhooks*, cambie el conmutador a **Activado** y, a continuación, pulse **Aplicar**.

1.  Añada los datos que quiera pasar a la aplicación externa como pares de clave y valor, en la sección *Parámetros*.

    Por ejemplo, si llama a la acción web Hello World de {{site.data.keyword.openwhisk_short}}, es posible que desee añadir la siguiente información para pasarla en el parámetro de mensaje que dicha aplicación acepta:

    <table>
    <caption>Ejemplo de parámetro</caption>
      <tr>
        <th>Clave</th>
        <th>Valor</th>
      </tr>
      <tr>
        <td>message</td>
        <td>"hello"</td>
      </tr>
    </table>

    Al llamar a una acción web de {{site.data.keyword.openwhisk_short}}, no puede pasar parámetros con la misma clave que los parámetros que se definen como parte de la acción web. Consulte [Parámetros protegidos](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_protect){: new_window} para obtener más detalles.
    {: note}

1.  Puede editar la respuesta del nodo de diálogo para que incluya sólo la sección de la respuesta que quiere mostrar a los usuarios. 

    La acción web Hello World de {{site.data.keyword.openwhisk_short}} incluye un nombre de mensaje y un par de valores en su respuesta, junto con otra información. Para mostrar solo el contenido del mensaje, puede utilizar la sintaxis `<return-variable>.message` para extraer la sección del mensaje solo del objeto de respuesta.

    <table>
    <caption>Ejemplo de respuestas condicionales</caption>
      <tr>
        <th>Condición</th>
        <th>Respuesta</th>
      </tr>
      <tr>
        <td>$webhook_result_1</td>
        <td>La aplicación ha devuelto "$webhook_result_1.message".</td>
      </tr>
      <tr>
        <td>anything_else</td>
        <td>La llamada a la aplicación externa ha fallado. Vuelva a intentarlo más tarde.</td>
      </tr>
    </table>

1.  Cuando haya terminado, pulse en la X para cerrar el nodo. Los cambios se guardan automáticamente.

### Cómo llamar a una acción estándar
{: #dialog-webhooks-cf-action}

Puede realizar una llamada a una acción gestionada por Cloud Foundry, pero no a una acción que utilice la autenticación de IAM (Identity and Access Management) basada en señales (tokens).

Las acciones de {{site.data.keyword.openwhisk_short}} admiten llamadas síncronas o asíncronas. Sin embargo, no puede realizar una llamada asíncrona a una acción de {{site.data.keyword.openwhisk_short}} con un webhook. Cuando se envía una solicitud asíncrona, sólo se devuelve un ID de activación.

Para realizar una llamada síncrona a una acción de {{site.data.keyword.openwhisk_short}} gestionada por Cloud Foundry, realice los pasos siguientes:

1.  Desde el conocimiento en el que desea añadir el webhook, pulse el separador **Opciones**.

1.  Pulse **Webhooks**.

1.  En el campo **URL**, especifique el URL para la acción. 

    Añada un parámetro `?blocking=true` al URL de acción para forzar que se realice una llamada síncrona.

    Esta sintaxis envía una solicitud síncrona:

    ```bash
    https://us-south.functions.cloud.ibm.com/api/v1/namespaces/my_org_dev/actions/Hello%20World?blocking=true
    ```
    {: codeblock}

    Cuando llama a una acción, debe proporcionar la clave de API que está asociada a la acción como una cabecera, que se describe en el paso siguiente.

1.  Proporcione detalles de autorización con la solicitud.

    Para llamar a una acción de {{site.data.keyword.openwhisk_short}} gestionada por Cloud Foundry, realice los pasos siguientes:

    1.  Obtenga la clave de API. En la página Punto final de {{site.data.keyword.openwhisk_short}}, busque la sección CURL y pulse el icono de ojo para mostrar los detalles de la clave. Copie la clave `user ID:password` que hay tras el parámetro `-u` en el mandato curl.
    
    1.  Pulse **Añadir autorización**, añada sus credenciales a los campos **Nombre de usuario** y **Contraseña** y, a continuación, pulse **Guardar**. 

    Las credenciales están codificadas y se genera una cabecera que se añade a la página automáticamente. 
    
    ![Muestra los campos del URL y la sección Headers de la página Opciones.](images/webhook-to-cfaction.png)

    <!-- - If you are calling a {{site.data.keyword.openwhisk_short}} action that is managed by IBM Cloud Identity and Access Management (IAM) instead of CLoud Foundry, then you must provide an IAM bearer token to authenticate the request. 
    
      1. Follow the instructions in [Passing an IBM Cloud IAM token to authenticate with a service's API](/docs/iam?topic=iam-iamapikeysforservices#token_auth). 
      
      1. To pass the IAM bearer token, use a header like this:
    
         <table>
         <caption>Header example</caption>
           <tr>
             <th>Header name</th>
             <th>Header value</th>
           </tr>
           <tr>
             <td>Authorization</td>
             <td>Bearer `<IAM token>`</td>
           </tr>
         </table>

        For more information about how to manage IAM tokens, see this [IBM Developer article](https://developer.ibm.com/tutorials/accessing-iam-based-services-from-ibm-cloud-functions/){: external}.
    -->
    Los detalles de su webhook se guardan de forma automática.

1.  Pulse el separador **Diálogo**.

1.  Pulse esta opción para abrir el nodo de diálogo desde el que desea llamar a la acción web y, a continuación, pulse **Personalizar**.

1.  Desplácese hacia abajo hasta la sección *Webhooks*, cambie el conmutador a **Activado** y, a continuación, pulse **Aplicar**.

1.  Añada los datos que quiera pasar a la aplicación externa como pares de clave y valor, en la sección *Parámetros*.

    Cuando se llama a una acción de {{site.data.keyword.openwhisk_short}}, *puede* pasar parámetros con la misma clave que los parámetros que se definen como parte de la acción. Los parámetros no están reservados como si fueran para acciones web.

1.  Puede editar la respuesta del nodo de diálogo para que incluya sólo la sección de la respuesta que quiere mostrar a los usuarios. 

    Por ejemplo, en la sección de respuestas condicionales, puede utilizar una expresión con la sintaxis `$webhook_result.response.result.message` para extraer sólo el mensaje devuelto.

    <table>
    <caption>Ejemplo de respuestas condicionales</caption>
      <tr>
        <th>Condición</th>
        <th>Respuesta</th>
      </tr>
      <tr>
        <td>$webhook_result_1</td>
        <td>La aplicación ha devuelto "$webhook_result.response.result.message".</td>
      </tr>
      <tr>
        <td>anything_else</td>
        <td>La llamada a la aplicación externa ha fallado. Vuelva a intentarlo más tarde.</td>
      </tr>
    </table>

1.  Cuando haya terminado, pulse en la X para cerrar el nodo. Los cambios se guardan automáticamente.
