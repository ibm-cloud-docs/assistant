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

# Tareas avanzadas
{: #logs-resources}

Conozca las API y otras herramientas que puede utilizar para acceder a los datos de registro y analizarlos.
{: shortdesc}

## API
{: #logs-resources-api}

Puede utilizar la API `/logs` para obtener una lista de los sucesos de las transcripciones de conversaciones que se han producido entre los usuarios y el asistente. Para ver la documentación de consulta detallada de la API, consulte [Listado de sucesos de registro ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/apidocs/assistant#list-log-events-in-a-workspace).

El número de días que se almacenan los registros difiere según el tipo de plan de servicio. Consulte [Límites de registro](/docs/services/assistant?topic=assistant-logs#logs-limits) para obtener detalles.

Para un script Python que puede ejecutar para exportar registros y convertirlos al formato CSV, descargue el archivo `export_logs.py`
desde el repositorio [GitHub de Watson Assistant ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://github.com/watson-developer-cloud/community/blob/master/watson-assistant/export_logs.py).

## Terminología relacionada con los registros
{: #logs-resources-terminology}

En primer lugar, revise las definiciones de los términos que asociados con los registros de {{site.data.keyword.conversationshort}}:

- ***Asistente***: aplicación, a la que a veces se denomina 'chat bot', que implementa el contenido de {{site.data.keyword.conversationshort}}.
- ***Conversación***: conjunto de mensajes que consta de los mensajes que envía un usuario individual a su asistente de y los mensajes que devuelve el asistente.
- ***ID de conversación***: identificador exclusivo que se añade a las llamadas de mensajes individuales para relacionar los intercambios de mensajes relacionados entre sí. Los desarrolladores de apps que utilizan la versión V1 de la API de servicio añaden este valor a las llamadas de mensaje en una conversación incluyendo el ID en los metadatos del objeto de contexto.
- ***ID de cliente***: ID exclusivo que se puede utilizar para etiquetar datos de cliente de forma que se puedan suprimir posteriormente si el cliente solicita la eliminación de sus datos.
- ***ID de despliegue***: etiqueta exclusiva que los desarrolladores de apps que utilizan la versión V1 de la API de servicio pasan con cada mensaje de usuario para ayudar a identificar el entorno de despliegue que ha producido el mensaje.
- ***Instancia***: su despliegue de {{site.data.keyword.conversationshort}}, accesible a través de credenciales exclusivas. Una instancia de {{site.data.keyword.conversationshort}} puede contener varios asistentes.
- ***Mensaje***: expresión que un usuario envía al asistente.
- ***ID de conocimiento***: identificador exclusivo de un conocimiento.
- ***Usuario***: un usuario es cualquier persona que interactúe con su asistente; a menudo son sus clientes.
- ***ID de usuario***: etiqueta exclusiva que se utiliza para realizar un seguimiento del nivel de uso del servicio de un usuario específico.
- ***ID de espacio de trabajo***: identificador exclusivo de un espacio de trabajo. Aunque los espacios de trabajo creados antes del 9 de noviembre se muestran como conocimientos en la herramienta, un conocimiento y un espacio de trabajo no son lo mismo. Un conocimiento es en realidad un derivador de un espacio de trabajo de V1.

**Importante**: la propiedad **ID de usuario** *no* equivale a la propiedad **ID de cliente**, aunque ambos se pueden pasar al servicio. El campo **ID de usuario** se utiliza para realizar un seguimiento de los niveles de uso a efectos de facturación, mientras que el campo **ID de cliente** se utiliza para dar soporte al etiquetado y a la posterior supresión de mensajes asociados con usuarios finales. El ID de cliente se utiliza en todos los servicios Watson y se especifica en la cabecera `X-Watson-Metadata`. El ID de usuario se utiliza exclusivamente en el servicio {{site.data.keyword.conversationshort}} y se pasa en el objeto de contexto de cada llamada de la API /message.

## Habilitación de métricas de usuario
{: #logs-resources-user-id}

Las métricas de usuario le permiten ver, por ejemplo, el número de usuarios exclusivos que efectúan operaciones con su asistente, o el número medio de conversaciones por usuario durante un intervalo de tiempo determinado en la [página Visión general](/docs/services/assistant?topic=assistant-logs-overview). Las métricas de usuario se habilitan mediante el parámetro `ID de usuario` exclusivo.

Para especificar el `ID de usuario` para un mensaje enviado mediante la API `/message`, incluya la propiedad `user_id` dentro del objeto de metadatos en el [contexto ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}, como en este ejemplo::

```
"context" : {
  "metadata" : {
       "user_id": "{UserID}"
  }
}
```
{: codeblock}

## Asociación de datos de mensajes con un usuario para suprimirlos
{: #logs-resources-customer_id}

Es posible que en algún momento desee eliminar por completo un conjunto de datos de usuario de una instancia de {{site.data.keyword.conversationshort}}. Cuando se utiliza la característica de supresión, las métricas de Visión general dejan de reflejar los mensajes suprimidos; por ejemplo, tendrán menos Conversaciones totales.

### Antes de empezar
{: #logs-resources-delete-customer-id-prereqs}

Para suprimir mensajes correspondientes a uno o varios usuarios individuales, primero tiene que asociar un mensaje con un **ID de cliente** exclusivo para cada persona. Para especificar el **ID de cliente** para cualquier mensaje enviado mediante la API `/message`,
incluya la propiedad `X-Watson-Metadata: customer_id` en la cabecera. Puede pasar varias entradas de **ID de cliente**
con pares `campo=valor` separados por un punto y coma, utilizando `customer_id`, como en el ejemplo siguiente:

```
curl -X POST -u "apikey:3Df... ...Y7Pc9"
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id={first-customer-ID};customer_id={second-customer-ID}'
 --data '{"input":{"text":"hello"}}' 'https:// gateway-us-south.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

La serie `customer_id` no puede incluir el signo de punto y coma (`;`) ni el signo igual (`=`). Usted es responsable de asegurarse de que cada parámetro `ID de cliente` sea exclusivo entre sus clientes.
{: note}

Para suprimir mensajes utilizando los valores de `customer_id`, consulte el tema [Seguridad de la información](/docs/services/assistant?topic=assistant-information-security#information-security-gdpr-wa).

## Cuadernos de Jupyter
{: #logs-resources-jupyter-notebooks}

IBM ha creado cuadernos de Jupyter que puede utilizar para analizar en detalle sus datos de registro. Un cuaderno de Jupyter es un entorno basado en la web para los cálculos interactivos. Puede ejecutar pequeños fragmentos de código que procesan sus datos y ver los resultados al momento.

Hay un conjunto de cuadernos que puede utilizar con herramientas de Python estándares y un conjunto que está diseñado para un uso óptimo con {{site.data.keyword.DSX_full}}. {{site.data.keyword.DSX_short}} es un producto que proporciona un entorno en el que puede elegir las herramientas que necesita para analizar y visualizar datos, para limpiar y configurar datos, para ingerir datos de modalidad continua o para crear, entrenar y desplegar modelos de aprendizaje de máquina. Consulte la [documentación del producto ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/welcome-main.html){:new_window} para obtener más detalles.

Están disponibles los siguientes cuadernos:

- **Medida**: recopila métricas que se centran en la cobertura (la frecuencia con la que el asistente tiene suficiente confianza como para responder a los usuarios) y la efectividad (cuando el asistente responde, si las respuestas satisfacen los requisitos de los usuarios).

- **Efectividad**: realiza un análisis más profundo de los registros para ayudarle a comprender los pasos que puede llevar a cabo para mejorar su asistente.

En el documento [Watson Assistant Continuous Improvement Best Practices Guide ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/common/ssi/cgi-bin/ssialias?htmlfid=54022554USEN&) se describe cómo sacar el máximo provecho de los cuadernos.

### Utilización de cuadernos con {{site.data.keyword.DSX}}
{: #logs-resources-notebooks-studio}

Si opta por utilizar los cuadernos que se han diseñado para su uso con {{site.data.keyword.DSX}}, a un nivel alto, los pasos son:

1.  Cree una cuenta de {{site.data.keyword.DSX}}, [cree un proyecto ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/projects.html?context=analytics){:new_window} y añada
una cuenta de Cloud Object Storage al mismo.
1.  Desde la comunidad de {{site.data.keyword.DSX}}, obtenga el [cuaderno de medida de rendimiento de Watson Assistant ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")]( https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f635e568).
1   Siga las instrucciones paso a paso que se proporcionan con el cuaderno para analizar un subconjunto de intercambios de diálogos de los registros.

    Los detalles se visualizan de forma que resulte más fácil comprender la cobertura y la eficacia del asistente.
1.  Exporte un conjunto de ejemplo de visualizaciones subyacentes de las conversaciones poco eficaces y analícelas y anótelas.

    Por ejemplo, indique si una respuesta es correcta. Si es correcta, marque si resulta útil. Si una respuesta es incorrecta, identifique la causa raíz, como por ejemplo que se haya detectado una intención o una entidad incorrecta o que se haya activado un nodo de diálogo erróneo. Después de identificar la causa raíz, indique cuál habría sido la elección correcta.
1.  Incorpore la hoja de cálculo anotada al [cuaderno de análisis de efectividad de Watson Assistant](https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f636921c).

Este proceso le ayuda a comprender los pasos que puede llevar a cabo para mejorar su asistente. No hay ninguna manera de aplicar automáticamente lo que aprende a la instancia de servicio. Realice un seguimiento de los cambios que realice para mejorar el sistema, de modo que posteriormente pueda aplicarlos directamente a los datos de entrenamiento de su conocimiento de diálogo.

### Utilización de los cuadernos con herramientas de Python estándares
{: #logs-resources-notebooks-python}

Si opta por utilizar las herramientas de Python estándares para ejecutar los cuadernos, puede obtener los cuadernos del [repositorio GitHub de IBM](https://github.com/watson-developer-cloud/assistant-improve-recommendations-notebook/tree/master/notebook). Asegúrese de ejecutarlos en el orden siguiente:

1.  Medida Notebook.ipynb
1.  Efectividad Notebook.ipynb
