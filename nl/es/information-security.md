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

# Seguridad de la información
{: #information-security}

IBM se ha comprometido a proporcionar a nuestros clientes y socios soluciones innovadoras de privacidad, seguridad y gobierno de datos.
{: shortdesc}

**Aviso:** Los clientes son responsables de garantizar su propio cumplimiento con diversas leyes y reglamentos, incluido el Reglamento General de Protección de Datos de la Unión Europea (GDPR). Los clientes son los únicos responsables de obtener asesoramiento legal competente referente a la identificación y la interpretación de cualquier ley relevante que pudiera afectar a su negocio, así como cualquier medida que debieran tomar para cumplir con dichas leyes y normativas.

Los productos, los servicios y otras prestaciones descritas en este documento no son adecuados para todas las situaciones de los clientes y su disponibilidad puede estar restringida. IBM no proporciona recomendaciones legales, contables o de auditoría ni garantiza que sus servicios o productos garantizarán que los clientes cumplan ninguna legislación o normativa.

Si tiene que solicitar soporte de GDPR para los recursos de {{site.data.keyword.cloud}} {{site.data.keyword.watson}} que se crean

- En la Unión Europea, consulte [Solicitud de soporte para recursos de IBM Cloud Watson creados en la Unión Europea![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/docs/services/watson/getting-started-gdpr-sar#request-EU){: new_window}.
- Fuera de la Unión Europea, consulte [Solicitud de soporte para recursos fuera de la Unión Europea![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/docs/services/watson/getting-started-gdpr-sar#request-non-EU){: new_window}.

## Reglamento General de Protección de Datos de la Unión Europea (GDPR)
{: #information-security-gdpr}

IBM se ha comprometido a proporcionar a nuestros clientes y socios soluciones innovadoras de privacidad, seguridad y gobierno de datos para ayudarles a cumplir con el GDPR.

Obtenga más información sobre la implantación del GDPR en IBM y las prestaciones y las ofertas de GDPR para ayudarle a cumplirlo [aquí ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](../../icons/launch-glyph.svg "Icono de enlace externo")](http://www.ibm.com/gdpr){: new_window}.

## Cómo etiquetar y suprimir datos en {{site.data.keyword.conversationshort}}
{: #information-security-gdpr-wa}

No incluya datos personales en los datos de entrenamiento (entidades e intenciones, incluidos ejemplos de usuario) que cree. En particular, asegúrese de eliminar cualquier información de identificación personal de los archivos que contengan expresiones de usuario reales que cargue para que se examinen a fin de generar recomendaciones de ejemplos de usuario.

**Nota:** Las características experimentales y beta no están pensadas para su uso en un entorno de producción y, por lo tanto, no se garantiza que funcionen como se espera al etiquetar y suprimir datos. Las características experimentales y beta no deben utilizarse al implementar una solución que requiera el etiquetado y supresión de datos.

Si tiene que eliminar datos de mensajes de un cliente de una instancia de {{site.data.keyword.conversationshort}}, puede hacerlo basándose en el ID del cliente, siempre que asocie el mensaje con un ID de cliente cuando el mensaje se envíe al servicio.

**Nota:** el enlace de vista previa y las características de integración automática de Facebook no dan soporte al etiquetado y, por lo tanto, a la supresión de datos en función del ID de cliente. Estas características no se deben utilizar en una solución que requiera la posibilidad de suprimir basándose en el ID de cliente.

### Antes de empezar
{: #information-security-delete-user-data-prereqs}

Para poder suprimir datos de mensajes asociados con un usuario específico, primero debe asociar todos los mensajes con un **ID de cliente** exclusivo para cada usuario. Para especificar el **ID de cliente** para los mensajes enviados mediante la API `/message`, incluya la propiedad `X-Watson-Metadata: customer_id` en la cabecera. Por ejemplo:

```
curl -X POST -u "apikey:3Df... ...Y7Pc9"
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id=abc'
 --data
   '{"input":{"text":"hello"}}'
  'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

La serie `customer_id` no puede incluir el signo de punto y coma (`;`) ni el signo igual (`=`). Usted es responsable de asegurarse de que cada propiedad de `ID de cliente` sea exclusiva entre sus clientes.
{: note}

Puede pasar varios valores de **ID de cliente** con pares `customer_id={value}` separados por signos de punto y coma. Por ejemplo: `'X-Watson-Metadata: customer_id=abc;customer_id=xyz'`

Si añade un conocimiento de búsqueda a un asistente, la entrada de usuario que se envía al asistente se pasa al servicio {{site.data.keyword.discoveryshort}} como una consulta de búsqueda. Si la integración de {{site.data.keyword.conversationshort}} proporciona un ID de cliente, la solicitud de la API /message resultante incluye el ID de cliente en la cabecera y el ID se pasa mediante la solicitud de la API /query de {{site.data.keyword.discoveryshort}}. Para suprimir los datos de consulta asociados a un cliente específico, debe enviar una solicitud de supresión directamente a la instancia de servicio de {{site.data.keyword.discoveryshort}} enlazada al asistente. Consulte el tema sobre [seguridad de la información](https://cloud.ibm.com/docs/services/discovery/information-security#gdpr-discovery) de {{site.data.keyword.discoveryshort}} para obtener más información.

### Consulta de datos de usuario
{: #information-security-query-customer-id}

Utilice el parámetro `filter` del método de v1 `/logs` para buscar datos de usuario específicos en el registro de una aplicación. Por ejemplo, para buscar datos específicos de un `customer_id` que coincidan con `my_best_customer`, la consulta podría ser:

``` curl
curl -X GET -u "apikey:3Df... ...Y7Pc9"
'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/logs?version=2018-09-20&filter=customer_id::my_best_customer'
```
{: codeblock}

Revise el tema sobre [Filtrado de consultas](/docs/services/assistant?topic=assistant-filter-reference) para obtener más información.

### Supresión de datos
{: #information-security-delete-data}

Para suprimir los datos de registros de mensajes asociados con un usuario específico que el servicio puede haber guardado, utilice el método de la API v1 `DELETE /user_data`. Especifique el ID de cliente del usuario pasando un parámetro `customer_id` con la solicitud.

Con este método de supresión solo se pueden suprimir los datos que se han añadido utilizando el punto final de API `POST /message` con un ID de cliente asociado. Los datos añadidos mediante otros métodos no se pueden suprimir en función del ID de cliente. Por ejemplo, las entidades y las intenciones añadidas de conversaciones de clientes no se pueden suprimir con este método. No se da soporte a datos personales para estos métodos.

**IMPORTANTE**: si se especifica un `customer_id`, se suprimirán *todos* los mensajes con ese `customer_id` que se hayan recibido antes que la solicitud de supresión en toda la instancia de {{site.data.keyword.conversationshort}}, no solo dentro de un solo conocimiento.

Por ejemplo, para suprimir los datos de mensajes asociados a un usuario con el ID de cliente `abc` de la instancia de {{site.data.keyword.conversationshort}}, envíe el siguiente mandato cURL:

```
curl -X DELETE -u "apikey:3Df... ...Y7Pc9"
 'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/user_data?customer_id=abc&version=2018-09-20'
```
{: codeblock}

Se devuelve un objeto JSON vacío `{}`.

Para obtener más información, examine la [Consulta de API](https://cloud.ibm.com/apidocs/assistant?curl=#delete-labeled-data).

**Nota:** las solicitudes de supresión se procesan en lotes y pueden tardar hasta 24 horas en completarse.
