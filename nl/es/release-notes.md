---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# Notas del release
{: #release-notes}

## Versiones de la API del servicio
{: #release-notes-api-version}

Las solicitudes de API requieren un parámetro de versión que toma una fecha en el formato `version=AAAA-MM-DD`. Siempre que cambiamos la API de forma que sea compatible con versiones anteriores, ponemos a su disposición una versión menor de la API.

Envíe el parámetro version con cada solicitud de API. El servicio utiliza la versión de la API correspondiente a la fecha que especifique, o la versión más reciente anterior a dicha fecha. No se utiliza como valor predeterminado la fecha actual. Especifique una fecha que coincida con una versión que sea compatible con su app y no la modifique hasta que la app esté lista para una versión posterior.

- La versión actual de V1 es `2019-02-28`.
- La versión actual de V2 es `2019-02-28`.
- El panel "Pruébelo" de la herramienta {{site.data.keyword.conversationshort}} utiliza la versión `2018-07-10`.

## Características beta
{: #release-notes-beta}

IBM proporciona servicios, características y soporte de idioma nacional para su evaluación que se clasifican como versiones beta. Estas características pueden ser inestables, pueden cambiar con frecuencia y pueden dejar de recibir soporte tras un aviso con poca antelación. Las características beta también pueden no proporcionar el mismo nivel de rendimiento o compatibilidad que proporcionan las características con disponibilidad general, y no están pensadas para su uso en un entorno de producción. Únicamente se da soporte a las características beta en [IBM Developer Answers ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://developer.ibm.com/answers/topics/watson-assistant/){: new_window}.

## Modelos actualizados
{: #release-notes-updated-models}

Los algoritmos de {{site.data.keyword.conversationshort}} se pueden perfeccionar y actualizar periódicamente en función de comentarios, mejoras científicas y otros factores, a fin de mejorar continuamente el rendimiento. Las actualizaciones del modelo se comunicarán en estas notas del release.

Los modelos que ha formado no se verán afectados de inmediato, pero los modelos caducados se actualizarán al modelo actual, si todavía no lo ha hecho, transcurridos 60 días desde la disponibilidad de un nuevo modelo.

**Nota:** Esta declaración sobre actualización se aplica solo a características e idiomas disponibles a nivel general (GA).

Están disponibles las siguientes nuevas características y cambios en el servicio. Consulte nuestro [blog ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://medium.com/ibm-watson/assistant/home) para ver información detallada sobre cómo puede beneficiarse su negocio de las nuevas características.

## 1 de marzo de 2019
{: #1March2019}

- **Navegación simplificada**: La barra lateral de navegación que divide los separadores *Crear*,  *Mejorar* y *Desplegar* se ha eliminado. Ahora, puede acceder a todas las herramientas que necesita para crear un conocimiento de diálogo desde la página principal de conocimientos.

- **La página Mejorar ahora se llama Análisis**: Las métricas informativas que genera el servicio a partir de las conversaciones entre los usuarios y el asistente han pasado del separador *Mejorar* de la barra lateral a un nuevo separador de la página principal de conocimientos denominada **Análisis**.

## 28 de febrero de 2019
{: #28February2019}

- **Nueva versión de API**: La versión actual de la API es ahora `2019-02-28`. Con esta versión se han realizado los cambios siguientes:

    - Se ha modificado el orden en el que se evalúan las condiciones en los nodos con ranuras. Anteriormente, si tenía un nodo con ranuras que permitía las digresiones hacia fuera, se activaba el nodo raíz `anything_else` para que se pudieran evaluar las condiciones No encontrado de nivel de ranura. El orden de las operaciones se ha modificado para hacer frente a este comportamiento. Ahora, cuando un usuario realiza una digresión hacia fuera de un nodo con ranuras, se procesan todos los nodos raíz excepto el nodo `anything_else`. A continuación, se evalúan las condiciones No encontrado de nivel de ranura. Finalmente, se procesa el nodo `anything_else` de nivel raíz. Para comprender mejor el orden de las operaciones correspondientes a un nodo con ranuras, consulte [Consejos sobre el uso de ranuras](/docs/services/assistant?topic=assistant-dialog-slots#dialog-slots-node-level-handler).

    - Las series que empiezan por un signo de número (#) en los objetos `context` u `output` de un mensaje ya no se tratan como referencias de intención.
  
      Anteriormente, estas series se trataban automáticamente como intenciones. Por ejemplo, si especificaba una variable de contexto, como por ejemplo `"color":"#FFFFFF"`, el código de color hexadecimal (#FFFFFF) se trataba como una intención. El servicio comprobaba si se había detectado una intención denominada #FFFFFF en la entrada del usuario, y, si no, sustituía #FFFFFF por `false`. Esta sustitución ya no se produce.
  
      De forma similar, si incluía un signo de número (#) en la serie de texto en una respuesta de nodo, para especificar un carácter de escape lo precedía de una barra inclinada invertida (``\`). Por ejemplo, ``We are the \#1 seller of lobster rolls in Maine.`` (Somos los vendedores número 1 de sándwiches de langosta en Maine). Ya no es necesario que utilizar el carácter de escape para el símbolo ``#` en una respuesta de texto.

      Este cambio no se aplica a las condiciones de respuesta de nodo ni condicionales. Las series que empiecen con un signo de número (#) que se especifican en condiciones siguen se siguen tratando como referencias de intención. Además, puede utilizar la sintaxis de expresión de SpEL para obligar al sistema a tratar una serie en los objetos `context` u `output` de un mensaje como una intención. Por ejemplo, especifique la intención como `<? #intent-name ?>`.

## 25 de febrero de 2019
{: #25February2019}

**Mejora en la integración de Slack**: Ahora puede elegir el tipo de suceso que activa su asistente en un canal de Slack. Anteriormente, cuando integraba su asistente con Slack, el asistente interactuaba con los usuarios a través de un canal de mensajes directo. Ahora, puede configurar el asistente para que escuche las menciones y responda cuando se le mencione en otros canales. Puede optar por utilizar uno de los tipos de sucesos o ambos como mecanismo a través del cual el asistente interactúa con los usuarios.

## 11 de febrero de 2019
{: #11February2019}

**Integración con Intercom**: Intercom, una plataforma de mensajería de servicios al cliente líder del sector, se ha asociado con IBM para añadir un nuevo agente al equipo, un asistente virtual de Watson. Puede integrar su asistente con una aplicación Intercom para permitir que la app pase sin problemas conversaciones de usuario entre el asistente y el equipo de agentes de soporte al usuario. Esta integración solo está disponible para los usuarios del plan Plus y Premium. Consulte [Integración con Intercom](/docs/services/assistant?topic=assistant-deploy-intercom) para ver más detalles.

## 8 de febrero de 2019
{: #8February2019}

- **Versiones de sus conocimientos**: Ahora puede capturar una instantánea de los valores de intenciones, entidades, diálogo y configuración correspondientes a un conocimiento en puntos clave durante el proceso de desarrollo. Con las versiones, se protege la creatividad. Puede desplegar nuevos enfoques de diseño en un entorno de prueba para validarlos antes de aplicar las actualizaciones a un despliegue de producción de su asistente. Consulte [Creación de versiones de conocimientos](/docs/services/assistant?topic=assistant-versions) para obtener más detalles.

- **Catálogo de contenido en árabe**: Ahora los usuarios de conocimientos en árabe pueden añadir intenciones predefinidas a sus diálogos. Consulte [Utilización de catálogos de contenido](/docs/services/assistant?topic=assistant-catalog) para obtener más información.

## 17 de enero de 2019
{: #17January2019}

- **El soporte del idioma checo está disponible a nivel general**: El soporte del idioma checo ya no está clasificado como beta; ahora está disponible a nivel general. Consulte [Idiomas soportados](/docs/services/assistant?topic=assistant-language-support) para obtener más información.

- **Mejoras en el soporte de idiomas**: Los componentes de comprensión de idioma se han actualizado para mejorar las características siguientes:

  - Entidades del sistema en alemán y coreano
  - Creación de señales de clasificación de intenciones para el árabe, el holandés, el francés, el italiano, el japonés, el portugués y el español

## 4 de enero de 2019
{: #4January2019}

- **IBM Cloud Functions en las ubicaciones DC y Londres**: Ahora puede realizar llamadas mediante programación a IBM Cloud Functions desde el diálogo de un asistente de una instancia de servicio alojada en los centros de datos Londres y Washington, DC. Consulte [Cómo realizar llamadas mediante programación desde un nodo de diálogo](/docs/services/assistant?topic=assistant-dialog-actions).

- **Nuevos métodos para trabajar con matrices**: Están disponibles los siguientes métodos de expresión SpEL, que facilitan el trabajo con valores de matriz en el diálogo:

  - **JSONArray.filter**: Filtra una matriz comparando cada valor en la matriz con un valor que puede variar en función de la entrada del usuario.
  - **JSONArray.includesIntent**: Comprueba si una matriz `intents` contiene una intención concreta.
  - **JSONArray.indexOf**: Obtiene el número de índice de un valor específico en una matriz.
  - **JSONArray.joinToArray**: Aplica formato a los valores que se devuelven de una matriz.

   Consulte la [documentación del método array](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-arrays) para obtener más información.

## 13 de diciembre de 2018
{: #13December2018}

- **Centro de datos de Londres**: Ahora puede crear instancias del servicio {{site.data.keyword.conversationshort}} alojadas en el centro de datos de Londres sin sindicación. Consulte [Centros de datos](/docs/services/assistant?topic=assistant-services-information#services-information-regions) para obtener más detalles.

- **Cambios en el límite de nodos de un diálogo**: El límite de nodos de un diálogo ha pasado temporalmente de 100.000 a 500 para las nuevas instancias del plan Estándar. Este cambio en el límite se ha revertido posteriormente. Si ha creado una instancia del plan Estándar durante el periodo de tiempo en el que estaba en vigor el límite, es posible que los diálogos se hayan visto afectados. El límite estaba en vigor para los conocimientos creados entre el 10 y el 12 de diciembre de 2018. Los límites inferiores se eliminarán de todas las instancias afectadas en enero. Si necesite que se aumente el límite inferior antes de entonces, abra una incidencia de soporte.

## 1 de diciembre de 2018
{: #1December2018}

   Para determinar el número de nodos de diálogo en un conocimiento de diálogo, realice una de las siguientes acciones:

   - En la herramienta, si aún no está asociado con un asistente, añada el conocimiento de diálogo a un asistente y, a continuación, visualice el mosaico de conocimientos en la página principal del asistente. La sección *datos formados* muestra el número de nodos de diálogo.

   - Envíe una solicitud GET al punto final de la API /dialog_nodes API e incluya el parámetro `include_count=true`. Por ejemplo:

     ```curl
     curl -u "apikey:{apikey}" "https://gateway.watsonplatform.net/assistant/api/v1/workspaces/{workspace_id}/dialog_nodes?version=2018-09-20&include_count=true"
     ```

     En la respuesta, el atributo `total` del objeto `pagination` contiene el número de nodos de diálogo.

     Consulte [Resolución de problemas de importación de conocimientos](/docs/services/assistant?topic=assistant-skill-add#skill-add-import-errors) para obtener información sobre cómo editar los conocimientos que desea seguir utilizando.

## 27 de noviembre de 2018
{: #27November2018}

- **Hay un nuevo plan de servicio disponible, el plan Plus**: El nuevo plan ofrece características de nivel superior a un nivel de precios más bajo. A diferencia de los planes anteriores, el plan Plus es un plan de facturación basada en el usuario. Se mide el uso por parte del número de usuarios exclusivos que interactúan con su asistente durante un periodo de tiempo determinado. Para sacar el máximo provecho del plan, si crea su propia aplicación cliente, diseñe la app de forma que defina un ID exclusivo para cada usuario y pase el ID de usuario con cada llamada de API /message. Para las integraciones incorporadas, se utiliza el ID de sesión para identificar las interacciones de usuario con el asistente. Consulte [Planes basados en el usuario](/docs/services/assistant?topic=assistant-services-information#services-information-user-based-plans) para obtener más información.

  <table>
  <caption>Límites del plan Plus</caption>
    <tr>
      <th>Artefacto</th>
      <th>Límite</th>
    </tr>
    <tr>
      <td>Asistentes</td>
      <td>100</td>
    </tr>
    <tr>
       <td>Entidades contextuales</td>
       <td>20</td>
    </tr>
    <tr>
       <td>Anotaciones de entidades contextuales</td>
       <td>2.000</td>
    </tr>
    <tr>
       <td>Nodos del diálogo</td>
       <td>100.000</td>
    </tr>
    <tr>
       <td>Entidades</td>
       <td>1.000</td>
    </tr>
    <tr>
       <td>Sinónimos de entidad</td>
       <td>100.000</td>
    </tr>
    <tr>
       <td>Valores de entidad</td>
       <td>100.000</td>
    </tr>
    <tr>
       <td>Intenciones</td>
       <td>2.000</td>
    </tr>
    <tr>
       <td>Ejemplos de usuario de intenciones</td>
       <td>25.000</td>
    </tr>
    <tr>
       <td>Integraciones</td>
       <td>100</td>
    </tr>
    <tr>
       <td>Registros</td>
       <td>30 días</td>
    </tr>
    <tr>
       <td>Conocimientos</td>
       <td>50</td>
    </tr>
  </table>

- **Plan Premium basado en usuario**: El plan Premium ahora basa su facturación en el número de usuarios exclusivos activos. Si elige utilizar este plan, diseñe las aplicaciones personalizadas que cree de forma que identifiquen correctamente los usuarios que generan llamadas de API /message. Consulte [Planes basados en el usuario](services-information#user-based-plans) para obtener más información.

  Las instancias existentes del servicio del plan Premium no se ven afectadas por este cambio; siguen utilizando los métodos de facturación basados en API. Solo los usuarios existentes del plan Premium verán el plan basado en API en la opción del plan *Premium (API)*.
  {: note}

  Consulte las {{site.data.keyword.conversationshort}} [opciones del plan de servicio ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window} para obtener más información sobre todos los planes de servicios disponibles.

- **Recomendaciones de ejemplo de usuarios de intenciones ![Solo plan Plus o Premium](images/premium.png)**: Puede cargar un archivo que contiene entradas de usuario sin formato, como consultas de usuarios del registro de un centro de atención al cliente, que el servicio puede analizar para ver detectar candidatos de ejemplos de usuarios de intenciones. Consulte [Adición de ejemplos de archivos de registro](intent-recommendations#intent-recommendations-get-example-recommendations).

## 20 de noviembre de 2018
{: #20November2018}

**Eliminación de la sección Recomendaciones**: Se ha eliminado la sección Recomendaciones del separador Mejorar. Recomendaciones era una característica beta que solo estaba disponible para los usuarios del plan Premium. Recomendaba acciones que los usuarios podían llevar a cabo para mejorar sus datos de entrenamiento. En lugar de consolidar las recomendaciones en un solo lugar, ahora se hacen recomendaciones desde el lugar de la herramienta en el que se realizan los cambios reales en los datos de entrenamiento. Por ejemplo, al añadir sinónimos de entidad, ahora puede optar por ver una lista de sinónimos recomendados por el servicio. Si está buscando otras formas de analizar los registros de conversación de usuario con más detalle, considere la posibilidad de utilizar cuadernos de Jupyter. Consulte [Tareas avanzadas](/docs/services/assistant?topic=assistant-logs-resources) para obtener más detalles.

## 9 de noviembre de 2018
{: #9November2018}

- **Revisión mayor de la interfaz de usuario**: El servicio {{site.data.keyword.conversationshort}} tiene un nuevo aspecto y se han añadido características.

  Esta versión de la herramienta ha sido evaluada por participantes en el programa beta durante los últimos meses.

  - **Conocimientos**: Lo que antes se llamaba *espacio de trabajo* ahora se denomina *conocimiento*. Un *conocimiento de diálogo* es un contenedor de datos y artefactos de entrenamiento de proceso de lenguaje natural que permite al asistente comprender las preguntas de los usuarios y responder a las mismas.

    **¿Dónde están mis espacios de trabajo?** Todos los espacios de trabajo que ha creado anteriormente ahora se muestran en la instancia de servicio como conocimientos. Pulse el separador **Conocimientos** para verlos. Para obtener más información, consulte [Conocimientos](/docs/services/assistant?topic=assistant-skills).

  - **Asistentes**: Ahora puede publicar su conocimiento con tan solo dos pasos. Añada su conocimiento a un asistente y, a continuación, configure una o varias integraciones con las que desplegar el conocimiento. El asistente añade una capa de función sobre el conocimiento que permite al servicio organizar y gestionar automáticamente el flujo de información. Consulte [Asistentes](/docs/services/assistant?topic=assistant-assistants).

  - **Integraciones incorporadas**: En lugar de ir al separador **Desplegar** para desplegar el espacio de trabajo, puede añadir su conocimiento de diálogo a un asistente y añadir integraciones al asistente a través de las cuales el conocimiento se pone a disponibilidad de los usuarios. No es necesario crear una aplicación frontal personalizada y gestionar el estado de la conversación entre una llamada y la siguiente. No obstante, puede hacerlo si lo desea. Consulte [Adición de integraciones](/docs/services/assistant?topic=assistant-deploy-integration-add) para obtener más información.

  - **Nueva versión mayor de API**: Está disponible la versión V2 de la API. Esta versión proporciona acceso a los métodos que puede utilizar para interactuar con un asistente en tiempo de ejecución. Ya no hay que pasar contexto con cada llamada de API; el estado de la sesión se gestiona automáticamente como parte de la capa del asistente.
  
    Lo que se presenta en la herramienta como un conocimiento de diálogo es efectivamente un derivador de un espacio de trabajo V1. Actualmente no hay ningún método de API para la creación de conocimientos y asistentes con la API V2. No obstante, puede seguir utilizando la API V1 para crear espacios de trabajo. Consulte [Visión general de la API](/docs/services/assistant?topic=assistant-api-overview) para obtener más detalles.
    {: note}

  - **Cambio de orígenes de datos**: Ahora es más fácil mejorar el modelo en un conocimiento con registros de conversaciones de usuarios de otro conocimiento. No es necesario que se base en los ID de despliegue; puede sencillamente elegir el nombre del asistente al que se ha añadido y desplegado un conocimiento para utilizar sus datos. Consulte [Mejora entre asistentes](/docs/services/assistant?topic=assistant-logs#logs-deploy-id).

  En el siguiente vídeo se proporciona una visión general de 2 minutos de la herramienta {{site.data.keyword.conversationshort}} actualizada.

  <iframe class="embed-responsive-item" id="youtubeplayer" title="Visión general del producto" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/OkW7gnHrw30?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

  - **Enlaces de vista previa desde instancias de Londres**: Si la instancia de servicio está alojada en Londres, debe editar el URL del enlace de vista previa. El URL incluye un código de región correspondiente a la región en la que está alojada la instancia. Como las instancias de Londres están sindicadas con Dallas, debe sustituir la referencia `eu-gb` en el URL por `us-south` para que la página web de vista previa se muestre correctamente.

## 8 de noviembre de 2018
{: #8November2018}

- **Centro de datos japonés**: Ahora puede crear instancias del servicio {{site.data.keyword.conversationshort}} alojadas en el centro de datos de Tokio. Consulte [Centros de datos](/docs/services/assistant?topic=assistant-services-information#services-information-regions) para obtener más detalles.

## 30 de octubre de 2018
{: #30October2018}

- **Nuevo proceso de autenticación de API**: El servicio {{site.data.keyword.conversationshort}} ha pasado de utilizar Cloud Foundry a utilizar la autenticación de Identity and Access Management (IAM) basada en señal en las siguientes regiones:

  - Dallas (us-south)
  - Frankfurt (eu-de)

  Para nuevas instancias de servicio, utilice IAM para la autenticación. Puede pasar una señal de portadora (bearer) o una clave de API. Las señales dan soporte a solicitudes autenticadas sin incluir credenciales de servicio en cada llamada. Las claves de API utilizan la autenticación básica.

  Para todas las instancias del servicio existentes, siga utilizando las credenciales de servicio (`{nombreusuario}:{contraseña}`) para la autenticación.

  Consulte [Autenticación de llamadas de API](/docs/services/assistant?topic=assistant-services-information#services-information-authenticate-api-calls) para obtener más información.

## 25 de octubre de 2018
{: #25October2018}

- **Las recomendaciones de sinónimos de entidad están disponibles en más idiomas**: Se ha añadido soporte de recomendaciones de sinónimos para los idiomas francés, japonés y español.

## 26 de septiembre de 2018
{: #26September2018}

- **{{site.data.keyword.conversationfull}} está disponible en {{site.data.keyword.icpfull}}**: Consulte la [documentación de {{site.data.keyword.icpfull}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.3/featured_applications/watson_assistant.html) para obtener más información.

## 21 de septiembre de 2018
{: #21September2018}

- **Nueva versión de API**: La versión actual de la API es ahora `2018-09-20`. En esta versión, el atributo `errors[].path` del objeto de error que devuelve la API se expresa como un [puntero JSON ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://tools.ietf.org/html/rfc6901) en lugar de devolverse en el formato de notación con puntos.
- **Soporte de acciones web**: Ahora puede llamar a acciones web de {{site.data.keyword.openwhisk_short}} desde un nodo de diálogo. Consulte [Cómo realizar llamadas mediante programación desde un nodo de diálogo](/docs/services/assistant?topic=assistant-dialog-actions) para obtener más detalles.

## 15 de agosto de 2018
{: #15August2018}

- **Mejoras de soporte de coincidencia aproximada de entidad**: La coincidencia aproximada está totalmente admitida para las entidades en inglés y la característica de corrección ortografía ya no es una característica solo Beta para muchos otros idiomas. Consulte [Idiomas soportados](/docs/services/assistant?topic=assistant-language-support) para obtener más información.

## 6 de agosto de 2018
{: #6August2018}

- **Resolución de conflictos de intenciones ![Solo plan Plus o Premium](images/premium.png)**: La herramienta ahora puede ayudarle a resolver conflictos cuando dos o más ejemplos de usuario de distintas intenciones se parecen. Los ejemplos de usuario que no sean distintos pueden debilitar los datos de entrenamiento y hacer que sea más difícil para el servicio correlacionar la entrada de usuario con la intención adecuada en tiempo de ejecución. Consulte [Resolución
de conflictos de intenciones](/docs/services/assistant?topic=assistant-intents#intents-resolve-conflicts) para obtener más información.

- **Desambiguación** ![Solo plan Plus o Premium](images/premium.png): Habilite la desambiguación para permitir que su asistente solicite ayuda al usuario cuando tenga que decidir entre dos o más nodos de diálogo viables para procesar una respuesta. Consulte [Desambiguación](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation) para obtener más información.

- **Arreglo en Ir a**: Se ha solucionado un problema de la herramienta Diálogos que le impedía configurar una acción jump-to (ir a) destinada a la respuesta de un nodo con la condición especial `anything_else`.

- **Mensaje de retorno de digresión**: Ahora puede especificar el texto que se debe mostrar cuando el usuario vuelve a un nodo después de una digresión. El usuario ya habrá visto la solicitud para el nodo. Puede cambiar ligeramente el mensaje para comunicar a los usuarios que están volviendo a donde lo dejaron. Por ejemplo, puede especificar una respuesta como `¿Dónde estábamos? Ah, sí...` Consulte el apartado sobre [Digresiones](dialog-runtime#digressions) para obtener más información.

## 12 de julio de 2018
{: #12July2018}

- **Tipos de respuesta completa**: Ahora puede añadir respuestas completas que incluyan elementos como, por ejemplo, imágenes o botones, además de texto, en el diálogo. Consulte [Respuestas completas](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia) para obtener más información.

- **Entidades contextuales (Beta)**: Las entidades contextuales son entidades que se definen mediante el etiquetado de las menciones del tipo de entidad que se producen en los ejemplos de usuario de intención. Estos tipos de entidad enseñan al servicio no solo los términos de interés, sino también el contexto en el que suelen aparecer los términos de interés en expresiones de usuario, lo que permite que el servicio reconozca menciones de entidad que no ha visto antes en función únicamente de cómo se hace referencia a las mismas en la entrada de usuario. Por ejemplo, si anota el ejemplo de intención de usuario "I want a flight to Boston" etiquetando "Boston" como una entidad @destination, el servicio puede reconocer "Chicago" como @destination en la entrada de usuario "I want a flight to Chicago." Actualmente esta característica solo está disponible en inglés. Consulte [Adición de entidades contextuales](/docs/services/assistant?topic=assistant-entities#entities-create-annotation-based) para obtener más información.

  Cuando se accede a la herramienta con el navegador web Internet Explorer, no se pueden etiquetar las menciones de entidad en ejemplos de intención de usuario ni editar el texto de ejemplo de usuario.
  {: note}

- **Recomendaciones de entidad**: Ahora el servicio puede recomendar sinónimos para valores de entidad. La función de recomendación busca sinónimos relacionados en función de la similitud contextual extraída de un vasto cuerpo de información existente, que incluye grandes fuentes de texto escrito, y utiliza técnicas de proceso de lenguaje natural para identificar palabras similares a los sinónimos existentes en el valor de entidad. Para obtener más información, consulte [Sinónimos](/docs/services/assistant?topic=assistant-entities#entities-synonyms).

- **Nueva versión de API**: La versión actual de la API es ahora `2018-07-10`. Esta versión presenta los cambios siguientes:

  - El contenido del objeto `output` /message se ha modificado; en lugar de ser un objeto JSON `text`, es una matriz de tipo `generic` que da soporte a varios tipos de respuestas completas, que incluyen `image`, `option`, `pause` y `text`.
  - Se ha añadido soporte para entidades contextuales.

- **Filtro de fecha de la página de visión general**: Utilice los nuevos filtros de fecha para elegir el periodo para el que se visualizan los datos. Estos filtros afectan a todos los datos que se muestran en la página: no solo al número de conversaciones que se muestran en el gráfico, sino también a las estadísticas mostradas junto con el gráfico y a las listas de las principales intenciones y entidades. Consulte [Controles](logs-overview#controls) para obtener más información.

- **Ampliación del límite de patrón**: Cuando se utiliza el campo **Patrones** para [definir patrones específicos para un valor de entidad](/docs/services/assistant?topic=assistant-entities#entities-patterns), el patrón (expresión regular) ahora está limitado a 512 caracteres.

## 2 de julio de 2018
{: #2July2018}

- **Función "ir a" desde respuestas condicionales**: Ahora puede configurar una respuesta condicional para ir directamente a otro nodo. Consulte [Respuestas condicionales](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multiple) para obtener más detalles.

## 21 de junio de 2018
{: #21June2018}

- **Actualizaciones de idiomas para entidades del sistema**: El soporte de los idiomas holandés y chino simplificado ya está disponible a nivel general. El soporte del idioma holandés incluye coincidencia aproximada para errores ortográficos. El soporte de chino tradicional incluye la disponibilidad de [entidades del sistema](/docs/services/assistant?topic=assistant-system-entities) en release beta. Consulte [Idiomas soportados](/docs/services/assistant?topic=assistant-language-support) para obtener más información.

## 14 de junio de 2018
{: #14June2018}

- **Apertura del centro de datos Washington, DC**: Ahora puede crear instancias del servicio {{site.data.keyword.conversationshort}} alojadas en el centro de datos Washington, DC. Consulte [Centros de datos](/docs/services/assistant?topic=assistant-services-information#services-information-regions) para obtener más detalles.

- **Nuevo proceso de autenticación de API**: El servicio {{site.data.keyword.conversationshort}} tiene un nuevo proceso de autenticación de API para instancias de servicio alojadas en las siguientes regiones:

  - Washington, DC (us-east) a partir del 14 de junio de 2018
  - Sídney, Australia (au-syd) a partir del 7 de mayo de 2018

  {{site.data.keyword.cloud_notm}} está migrando a la autenticación de IAM (Identity and Access Management) basada en señales.

  Para las nuevas instancias de servicio de las regiones mencionadas anteriormente, utilice IAM para la autenticación. Puede pasar una señal de portadora (bearer) o una clave de API. Las señales dan soporte a solicitudes autenticadas sin incluir credenciales de servicio en cada llamada. Las claves de API utilizan la autenticación básica.

  Para todas las instancias del servicio nuevas y existentes de otras regiones, siga utilizando las credenciales de servicio (`{nombreusuario}:{contraseña}`) para la autenticación.

  Cuando utilice cualquiera de los SDK de Watson, puede pasar la clave de la API y dejar que el SDK gestione el ciclo de vida de las señales. Para obtener más información y ver ejemplos, consulte [Autenticación ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")]https://{DomainName}/apidocs/assistant-v2#authentication){: new_window} en la consulta de API.

  Si no está seguro sobre el tipo de autenticación que debe utilizar, puede visualizar las credenciales de servicio pulsando la instancia de servicio en la [lista de recursos de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/resources){: new_window}.

## 25 de mayo de 2018
{: #25May2018}

- **Nuevo espacio de trabajo de ejemplo**: El espacio de trabajo de ejemplo que se proporciona para que pueda explorar o utilizar como punto de partida para su propio espacio de trabajo ha cambiado. El ejemplo de **panel de control para coches** se ha sustituido por un ejemplo de **servicio al cliente**. El nuevo ejemplo muestra cómo utilizar intenciones del catálogo de contenido y otras características nuevas para crear un bot. Puede responder a preguntas comunes como, por ejemplo consultas sobre horario y ubicación de tiendas, e ilustra cómo utilizar un nodo con ranuras para planificar citas en la tienda.

- **Se ha añadido la representación en HTML en el panel Pruébelo**: Ahora el panel "Pruébelo" puede mostrar el formato HTML incluido en el texto de una respuesta. Antes, si incluía un enlace de hipertexto como un código HTML en una respuesta de texto, se mostraba el código fuente HTML en el panel "Pruébelo" durante la prueba. Tenía este aspecto:

  `Póngase en contacto con nosotros en <a href="https://www.ibm.com">ibm.com</a>.`

  Ahora, el enlace de hipertexto se representa como si estuviera en una página web. Se muestra de este modo:

  `Póngase en contacto con nosotros en` [ibm.com](https://www.ibm.com){: new_window}.

    Recuerde que debe utilizar el tipo de sintaxis adecuado en las respuestas para la aplicación cliente en la que va a desplegar la conversación. Utilice la sintaxis HTML únicamente si la aplicación cliente puede interpretarla correctamente. Es posible que otros canales de integración esperen otros formatos.

- **Cambios en el despliegue**: Se ha eliminado la opción **Probar en Slack**.

## 11 de mayo de 2018
{: #11May2018}

- **Seguridad de la información**: La documentación incluye algunos detalles nuevos sobre la privacidad de los datos. Encontrará más información en el apartado sobre [Seguridad de la información](/docs/services/assistant?topic=assistant-information-security).

## 7 de mayo de 2018
{: #7May2018}

- **Apertura del centro de datos Sídney, Australia**: Ahora puede crear instancias del servicio {{site.data.keyword.conversationshort}} alojadas en el centro de datos Sídney, Australia. Consulte [Centros de datos globales de IBM Cloud ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo"](https://www.ibm.com/cloud/data-centers/) para obtener más información.

## 4 de abril de 2018
{: #4April2018}

- **Búsqueda en diálogos**: Ahora puede [buscar en nodos de diálogo](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-search) una determinada palabra o frase.

## 15 de marzo de 2018
{: #15March2018}

- **Introducción de {{site.data.keyword.conversationfull}}**: Se ha cambiado el nombre de {{site.data.keyword.ibmwatson}} Conversation. Ahora se llama {{site.data.keyword.conversationfull}}. El cambio de nombre refleja el hecho de que el servicio se está ampliando para proporcionar contenido integrado y herramientas que le ayudarán a compartir más fácilmente los asistentes virtuales que cree. Consulte [esta publicación del blog ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/blogs/watson/2018/03/the-future-of-watson-conversation-watson-assistant/) para obtener más información.

- **Nuevas API REST y SDK disponibles para Watson Assistant**: Las nuevas API son funcionalmente idénticas a las API existentes de la característica Conversation, que siguen recibiendo soporte. Para obtener más información sobre las API de Watson Assistant, vea la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/assistant){: new_window}.

- **Mejoras en diálogos**: Se han añadido las siguientes características a la herramienta de diálogos:

  - Ahora dispone de campos de nombre de variable y valor, que puede utilizar para añadir variables de contexto o para actualizar valores de variable de contexto. No es necesario que abra el editor de JSON a menos que desee hacerlo. Consulte [Definición de una variable de contexto](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-var-define) para obtener más detalles.
  - Organice el diálogo utilizando carpetas para agrupar nodos de diálogo relacionados. Consulte [Organización del diálogo con carpetas](dialog-build#folders) para obtener más información.
  - Se ha añadido soporte para personalizar la forma en que cada nodo de diálogo participa en las digresiones iniciadas por el usuario hacia fuera del flujo del diálogo designado. Consulte [Digresiones](dialog-runtime#digressions) para obtener más información.

- **Búsqueda de intenciones y entidades**: Se ha añadido una nueva característica de búsqueda que le permite [buscar intenciones](intents#searching-intents) para ver ejemplos de usuario, nombres de intención o descripciones, o [buscar valores y sinónimos de entidad](/docs/services/assistant?topic=assistant-entities#entities-search).

- **Catálogos de contenido**: Los nuevos [catálogos de contenido](/docs/services/assistant?topic=assistant-catalog#catalog-add) contienen una sola categoría de intenciones y entidades comunes predefinidas que puede añadir a su aplicación. Por ejemplo, la mayoría de las aplicaciones necesitan una intención #greeting-type general que inicia un diálogo con el usuario. Puede añadirla desde el catálogo de contenido en lugar de crear una propia.

- **Métricas de usuario mejoradas**: El componente Mejorar se ha mejorado con métricas de usuario y estadísticas de registro adicionales. Por ejemplo, la página Visión general incluye varios gráficos nuevos y detallados que resumen las interacciones entre los usuarios y la aplicación, la cantidad de tráfico durante un periodo de tiempo determinado y las intenciones y entidades que se han reconocido con mayor frecuencia en las conversaciones de usuario.

## 12 de marzo de 2018
{: #12March2018}

- **Nuevos métodos de fecha y hora**: Se han añadido métodos que facilitan los cálculos de fechas desde el diálogo. Consulte [Cálculos de fecha y hora](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-date-and-time-calculations) para obtener más detalles.

## 16 de febrero de 2018
{: #16February2018}

- **Rastreo de nodos de diálogo**: Cuando se utiliza el panel "Pruébelo" para probar un diálogo, se visualiza un icono de ubicación junto a cada respuesta. Puede pulsar el icono para resaltar la vía que el servicio ha recorrido a través del árbol del diálogo hasta llegar a la respuesta. Consulte [Creación de un diálogo](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-test) para obtener información más detallada.

- **Nueva versión de API**: La versión actual de la API es ahora `2018-02-16`. Esta versión presenta los cambios siguientes:

  - Ahora se da soporte a un nuevo parámetro, `include_audit`, en la mayoría de las solicitudes GET. Se trata de un parámetro booleano opcional que especifica si la respuesta debería incluir las propiedades de auditoría (indicaciones de fecha y hora de `created` y `updated`). El valor predeterminado es `false`. (Si está utilizando una versión de API anterior a `2018-02-16`, el valor predeterminado es `true`). Para obtener más información, vea la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/assistant){: new_window}.

  - Las respuestas de llamadas API utilizando la nueva versión incluye solo las propiedades con valores no `nulos`.

  - Las propiedades `output.nodes_visited` y `output.nodes_visited_details` de las respuestas de los mensajes ahora incluyen nodos con los siguientes tipos de información, que con anterioridad eran omitidos:

    - Nodos con `type`=`condición_respuesta`
    - Nodos con `type`=`manejador_sucesos` y `event_name`=`entrada`

## 9 de febrero de 2018
{: #9February2018}

- **Entidades de sistema en holandés (Beta)**: El soporte al holandés se ha mejorado para incluir la disponibilidad de las [Entidades de sistema](/docs/services/assistant?topic=assistant-system-entities) en el release beta. Consulte [Idiomas soportados](/docs/services/assistant?topic=assistant-language-support) para obtener más información.

## 29 de enero de 2018
{: #29January2018}

- Ahora la API REST de {{site.data.keyword.conversationshort}} da soporte a nuevos parámetros de solicitud:
  - Utilice el parámetro `append` al actualizar un espacio de trabajo para indicar si los datos del nuevo espacio de trabajo se deberían añadir a los datos existentes, en lugar de reemplazarlos. Para obtener más información, consulte [Actualización del espacio de trabajo ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/apidocs/assistant?curl=#update-workspace){: new_window}.
  - Utilice el parámetro `nodes_visited_details` al enviar un mensaje para indicar si la respuesta debería incluir información de diagnóstico adicional sobre los nodos visitados durante el proceso del mensaje. Para obtener más información, consulte [Enviar mensajes ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}.

## 23 de enero de 2018
{: #23January2018}

- **No se ha podido recuperar la lista de espacios de trabajo**: Si ve este mensaje de error, u otros similares, al trabajar con el conjunto de herramientas, podría indicar que su sesión ha caducado. Finalice la sesión con **Finalizar sesión** desde el icono **Información de usuario** ![Menú de icono de información de usuario](images/user-icon.png) y, a continuación, inicie de nuevo una sesión.

## 8 de diciembre de 2017
{: #8December2017}

- **Acceso a datos de registro a través de instancias (solo usuarios Premium)**: Si es un usuario con una cuenta {{site.data.keyword.conversationshort}} Premium, sus instancias Premium se pueden configurar de forma opcional para permitir el acceso a datos de registro desde espacios de trabajo a través de distintas instancias Premium.

- **Copia de nodos**: Ahora puede duplicar un nodo para hacer una copia del mismo y de sus hijos. Esta característica es útil si crea un nodo con una lógica de interés que desea reutilizar en otro lugar en el diálogo. Consulte [Copia de un nodo de diálogo](dialog-build#copy-node) para obtener más información.

- **Captura de grupos en entidades de patrón**: Puede identificar grupos en el patrón de expresión regular que defina para una entidad. La identificación de grupos es útil si más tarde desea poder hacer referencia a una subsección del patrón. Por ejemplo, la entidad podría tener un patrón de expresión regular para capturar números de teléfono de los EE. UU. Si identifica el segmento del código de área del patrón de número como un grupo, posteriormente puede hacer referencia a dicho grupo para acceder específicamente al segmento de código de un número de teléfono. Consulte [Definición de entidades](/docs/services/assistant?topic=assistant-entities#entities-creating-task) para obtener más información.

## 6 de diciembre de 2017
{: #6December2017}

- **Integración de {{site.data.keyword.openwhisk}} (Beta)**: Llame directamente a acciones de {{site.data.keyword.openwhisk}} (denominado con anterioridad IBM OpenWhisk) desde un nodo de diálogo. Esta característica permite, por ejemplo, llamar a una acción para recuperar información meteorológica desde un nodo de diálogo, y luego condicionar la información devuelta en la respuesta del diálogo. Actualmente, puede llamar a una acción desde una instancia de {{site.data.keyword.openwhisk_short}} alojada en la región EE. UU. sur desde instancias de {{site.data.keyword.conversationshort}} alojadas en la misma región. Consulte [Cómo realizar llamadas mediante programación desde un nodo de diálogo](/doc/services/assistant?topic=assistant-dialog-actions) para obtener más detalles.

## 5 de diciembre de 2017
{: #5December2017}

- **Interfaz de usuario rediseñada para intenciones y entidades**: Los separadores `Intenciones` y `Entidades` se ha rediseñado para proporcionar un flujo de trabajo más sencillo, eficiente al crear y editar entidades e intenciones. Consulte [Definición de intenciones](intents#creating-intents) y [Definición de entidades](/docs/services/assistant?topic=assistant-entities#entities-creating-task) para obtener más información sobre cómo trabajar con estos separadores.

## 30 de noviembre de 2017
{: #30November2017}

- **Soporte numérico para el árabe (Máshrek)**: Ahora se da soporte a los números para el árabe (Máshrek) en las entidades de sistema en árabe.

## 29 de noviembre de 2017
{: #29November2017}

- **Mejora en el entendimiento de entrada del usuario a través de espacios de trabajo**: Ahora es posible mejorar un espacio de trabajo con expresiones que se enviaron a otros espacios de trabajo dentro de su instancia. Por ejemplo, puede tener varias versiones de espacios de trabajo de producción y espacios de trabajo de desarrollo; puede utilizar los mismos datos de las expresiones para mejorar cualquiera de esos espacios de trabajo. Consulte [Mejora entre espacios de trabajo](/docs/services/assistant?topic=assistant-logs#logs-deploy-id).

## 20 de noviembre de 2017
{: #20November2017}

- **Conformidad del estándar GB18030**: GB18030 es un estándar chino que especifica una página de códigos ampliada que se utiliza en el mercado chino. Este estándar de página de códigos es importante para la industria del software porque el Comité técnico de estandarización de tecnología de la información nacional de China obliga desde el 1 de septiembre de 2001 a que todas las aplicaciones de software para el mercado de China estén habilitadas para el estándar GB18030. El servicio {{site.data.keyword.conversationshort}} da soporte a esta codificación, y se ha certificado el cumplimiento del estándar GB18030.

## 9 de noviembre de 2017
{: #9November2017}

- **Ejemplos de intenciones pueden hacer referencia a entidades de forma directa**: Ahora es posible especificar directamente una referencia de entidad en un ejemplo de intención. Dicha referencia de entidad, junto con todos sus valores o sinónimos, es utilizada por el clasificador de servicios de {{site.data.keyword.conversationshort}} para el aprendizaje de la intención. Para obtener más información, consulte [*Entidades como ejemplo*](/docs/services/assistant?topic=assistant-intents#intents-entity-as-example) en el tema [Intenciones](/docs/services/assistant?topic=assistant-intents).

  Actualmente, únicamente se puede hacer referencia de forma directa a entidades cerradas que defina. No es posible hacer directamente referencias a [entidades de patrón](/docs/services/assistant?topic=assistant-entities#entities-pattern) o [entidades de sistema](/docs/services/assistant?topic=assistant-system-entities).
  {: note}

## 8 de noviembre de 2017
{: #8November2017}

- **Conector de {{site.data.keyword.conversationshort}}**: Puede utilizar la nueva herramienta de conector de {{site.data.keyword.conversationshort}} para conectar su espacio de trabajo a una app Slack o Facebook Messenger propia, para ponerla a disposición como un chatbot con el que los usuarios de Slack o Facebook Messenger podrán interactuar. Esta herramienta sólo está disponible para la región EE. UU. sur de {{site.data.keyword.Bluemix_notm}}.

## 3 de noviembre de 2017
{: #3November2017}

- **Actualizaciones en los diálogos**: Las siguientes actualizaciones facilitan la creación de un diálogo. (Consulte [Creación de un diálogo](/docs/services/assistant?topic=assistant-dialog-build) para obtener más detalles).

    - Puede añadir una condición a una ranura para hacerla obligatoria únicamente si se satisfacen determinadas condiciones. Por ejemplo, puede crear una ranura que solicite el nombre de un cónyuge de forma obligatoria sólo si una ranura anterior (obligatoria) que solicita el estado civil indica que el usuario está casado.

    - Ahora puede elegir **Saltar entrada de usuario** como el siguiente paso para un nodo. Cuando elige esta opción, después de procesar el nodo actual, el servicio salta directamente al primer nodo hijo del nodo actual. Esta opción es similar a la opción existente *Ir a*, para ir al siguiente paso, excepto en que ofrece una mayor flexibilidad. No es necesario especificar el nodo exacto al que saltar. En el tiempo de ejecución, el servicio siempre salta al nodo que sea el primer nodo hijo, incluso si se reordenan los nodos hijo o si se añaden nuevos nodos después de que se haya definido el siguiente paso de comportamiento.

    - Es posible añadir respuestas condicionales para las ranuras. Tanto para la respuesta Encontrado como No encontrado, es posible personalizar el servicio con base a si se satisfacen determinadas condiciones. Esta característica le permite comprobar si se presentan posibles malinterpretaciones y corregirlas antes de guardar el valor proporcionado por el usuario en la variable de contexto de la ranura. Por ejemplo, si la ranura guarda la edad del usuario y utiliza `@sys-number` en el campo *Comprobar* para capturarla, puede añadir una condición que compruebe si hay números mayores de 100 para responder con algo similar a *Please provide a valid age in year.* (Proporcione una edad válida). Consulte [Adición de condiciones a las respuestas Encontrado y No encontrado](/docs/services/assistant?topic=assistant-dialog-slots#dialog-slots-handler-next-steps) para obtener más detalles.

    - La interfaz que se utiliza para añadir respuestas condicionales a un nodo se ha rediseñado para facilitar las condiciones y sus respuestas. Para añadir respuestas condicionales a nivel de nodo, pulse **Personalizar** y, a continuación, habilite la opción **Varias respuestas**.

     El conmutador **Varias respuestas** activa o desactiva respuesta únicamente a nivel de nodo. No controla la posibilidad de definir respuestas condicionales para una ranura. La configuración de las distintas respuestas de una ranura se controla de forma separada.
     {: note}

    - Para mantener de forma simple la página donde edita una ranura, seleccione las opciones de menú para a.) añadir una condición que se debe satisfacer para la ranura que se tiene que procesar y, b.) añadir respuestas condicionales para las condiciones Encontrado y No encontrado de una ranura. A no ser que elija añadir esta funcionalidad adicional, no se visualiza la condición de ranura ni los campos de varias respuestas, simplificando la página y facilitando su uso.

## 25 de octubre de 2017
{: #25October2017}

- **Actualizaciones para chino simplificado**: El soporte del idioma se ha mejorado para el chino simplificado. Esto incluye mejoras en la clasificación de intenciones utilizando inclusiones de palabras a nivel de carácter y la disponibilidad de entidades del sistema. Tenga en cuenta que los modelos de aprendizaje del servicio {{site.data.keyword.conversationshort}} pueden haberse actualizado como parte de esta mejora, y cuando se vuelva a formar el modelo se aplicarán los cambios; consulte [Modelos actualizados](#release-notes-updated-models) para obtener más información.
- **Actualizaciones para español** - Se han realizado mejoras en la clasificación de intenciones en español para conjuntos de datos de gran tamaño.

## 11 de octubre de 2017
{: #11October2017}

- **Actualizaciones para coreano**: El soporte del idioma se ha mejorado para el coreano. Tenga en cuenta que los modelos de aprendizaje del servicio {{site.data.keyword.conversationshort}} pueden haberse actualizado como parte de esta mejora, y cuando se vuelva a formar el modelo se aplicarán los cambios; consulte [Modelos actualizados](#release-notes-updated-models) para obtener más información.

## 3 de octubre de 2017
{: #3October2017}

- **Entidades definidas por patrón (Beta)**: Ahora puede definir patrones específicos para una entidad, utilizando expresiones regulares. Esto le puede ayudar a identificar las entidades que siguen un determinado patrón, por ejemplo números de referencia o números de pieza, números de teléfono o direcciones de correo electrónico. Consulte [Entidades definidas por patrón](/docs/services/assistant?topic=assistant-entities#entities-pattern) para ver detalles adicionales.
    - Puede añadir sinónimos o patrones para un solo valor de entidad; no puede añadir ambos.
    - Para cada valor de entidad, puede haber un máximo de 5 patrones.
    - Cada patrón (expresión regular) está limitado a 128 caracteres.
    - La importación o exportación mediante un archivo CSV no da soporte a patrones actualmente.
    - La API REST no da soporte al acceso directo a patrones, pero puede recuperar o modificar patrones utilizando el punto final `/values`.

- **Coincidencia aproximada filtrada por diccionario (solo inglés)**: Ahora dispone de una versión mejorada de la coincidencia aproximada para entidades, en inglés. Esta mejora evita la captura de algunas palabras comunes válidas en inglés como coincidencias aproximadas para una entidad determinada. Por ejemplo, la coincidencia aproximada no hará coincidir el valor de entidad `like` con `hike` ni con `bike`, que son palabras válidas en inglés, pero seguirá coincidiendo con ejemplos como `lkie` u `oike`.

## 27 de septiembre de 2017
{: #27September2017}

- **Actualizaciones en el generador de condiciones**: El control que se muestra para ayudarle a definir una condición en un nodo de diálogo se ha actualizado. Las mejoras incluyen soporte para listar nombres de variables de contexto después de escribir $ para empezar a añadir una variable de contexto.

## 31 de agosto de 2017
{: #31August2017}

- **Reversión de la sección Mejorar**: La métrica de tiempo medio de conversación, y los filtros correspondientes, se han eliminado temporalmente de la página Visión general de la sección Mejorar. Esta eliminación impedirá que el cálculo de determinadas medidas de tiempo medio de conversación y el gráfico de conversaciones en el tiempo muestren información imprecisa. IBM lamenta eliminar esta función de la herramienta, pero se compromete a garantizar que se comunica información precisa a los usuarios.
- **Nombres de nodos del diálogo**: Ahora puede asignar cualquier nombre a un nodo del diálogo; no es necesario que sea exclusivo. Y por lo tanto puede cambiar el nombre de un nodo sin que ello afecte al modo en que se hace referencia al nodo internamente. El nombre que especifique se guarda como un atributo de título del nodo en el archivo JSON del espacio de trabajo y el sistema utiliza un ID exclusivo que se guarda en el atributo de nombre para hacer referencia al nodo.

## 23 de agosto de 2017
{: #23August2017}

- **Actualizaciones en coreano, japonés e italiano**: Se ha mejorado el soporte de los idiomas coreano, japonés e italiano. Tenga en cuenta que los modelos de aprendizaje del servicio {{site.data.keyword.conversationshort}} pueden haberse actualizado como parte de esta mejora, y cuando se vuelva a formar el modelo se aplicarán los cambios; consulte [Modelos actualizados](#release-notes-updated-models) para obtener más información.

## 10 de agosto de 2017
{: #10August2017}

- **Normalización de acentos**: En un entorno conversacional, puede que los usuarios utilicen o no acentos cuando interactúan con el servicio {{site.data.keyword.conversationshort}}. Por lo tanto, se ha actualizado el algoritmo de modo que las versiones acentuadas y no acentuadas de las palabras se traten de la misma manera para la detección de intenciones y el reconocimiento de entidades.

  Sin embargo, para algunos idiomas, como el español, algunos acentos puede alterar el significado de la entidad. Por lo tanto, para la detección de entidades, aunque la entidad original pueda tener implícitamente un acento, el servicio también puede considerar como coincidencia la versión no acentuada de la misma entidad, pero con una puntuación de confianza ligeramente inferior.

  Por ejemplo, para la palabra `barrió`, que lleva acento y corresponde al pretérito indefinido del verbo `barrer`, el servicio también puede considerar como coincidencia la palabra `barrio`, pero con un nivel de confianza ligeramente inferior.

  El sistema proporcionará la mayor puntuación de confianza en entidades con coincidencias exactas. Por ejemplo, `barrio` no se detectará si `barrió` está en el conjunto de datos de entrenamiento; y `barrió` no se detectará si `barrio` está en el conjunto de datos de entrenamiento.

  Se espera que forme el sistema con los caracteres y acentos adecuados. Por ejemplo, si espera `barrió` como respuesta, debe colocar `barrió` en el conjunto de datos de entrenamiento.

  Aunque no lleven acento, lo mismo se aplica a las palabras que utilizan, por ejemplo, la letra `ñ` frente a la letra `n`, como `uña` frente a `una`. En este caso la letra `ñ` no es simplemente una `n` con virgulilla, sino que es una letra específica del español.

  Puede habilitar la coincidencia aproximada si cree que los clientes no utilizarán los acentos correctamente o cometerán errores ortográficos en algunas palabras (por ejemplo, colocando una `n` en lugar de una `ñ`), o bien puede excluirlas de forma explícita de los ejemplos de entrenamiento.

  **Nota:** La normalización de acentos está habilitada para portugués, español, francés y checo.

- **Distintivo opt-out del espacio de trabajo**: Ahora la API REST de {{site.data.keyword.conversationshort}} da soporte a un distintivo opt-out para espacios de trabajo. Este distintivo indica que los datos de entrenamiento del espacio de trabajo, como intenciones y entidades, no serán utilizados por IBM para realizar mejoras en el servicio general. Para obtener más información, vea la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/apidocs/assistant?curl=#data-collection){: new_window}

## 7 de agosto de 2017
{: #7August2017}

- **Interpretación de `Siguiente` y `Última` fecha** : El servicio {{site.data.keyword.conversationshort}} trata las fechas `last` (última) y `next` (siguiente) como si hicieran referencia al último o al siguiente día más inmediato, que puede estar dentro de la misma semana o en la anterior. Consulte el tema sobre [entidades del sistema](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-datetime) para obtener más información.

## 3 de agosto de 2017
{: #3August2017}

- **Coincidencia aproximada para idiomas adicionales (Beta)**: La coincidencia aproximada para entidades ahora está disponible para más idiomas, tal como se indica en el tema [Idiomas soportados](/docs/services/assistant?topic=assistant-language-support).
- **Coincidencia parcial (Beta - solo inglés)**: Ahora la coincidencia aproximada sugerirá automáticamente sinónimos basados en subseries que aparecen en entidades definidas por el usuario y asignará una puntuación de confianza inferior en comparación con la coincidencia de entidad exacta. Consulte [Coincidencia aproximada](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching) para ver detalles.

## 28 de julio de 2017
{: #28July2017}

- Cuando establece preferencias bidireccionales para la herramienta, ahora puede especificar la dirección de la interfaz gráfica de usuario.
- El esquema de colores de la herramienta se ha actualizada para adaptarla a otros servicios y productos Watson.

## 19 de julio de 2017
{: #19July2017}

- Ahora la API REST de {{site.data.keyword.conversationshort}} da soporte al acceso a nodos del diálogo. Para obtener más información, vea la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/apidocs/assistant?curl=#list-dialog-nodes){: new_window}.

## 14 de julio de 2017
{: #14July2017}

- La funcionalidad de ranuras de los diálogos se ha mejorado. Por ejemplo, se ha añadido la propiedad *slot_in_focus* que puede utilizar para definir una condición que solo se aplica a una ranura. Consulte [Obtención de información con ranuras](/docs/services/assistant?topic=assistant-dialog-slots) para ver detalles.

## 12 de julio de 2017
{: #12July2017}

- **Soporte de checo**: Se ha incorporado soporte para el idioma checo; consulte el tema [Idiomas soportados](/docs/services/assistant?topic=assistant-language-support) para ver más detalles.

## 11 de julio de 2017
{: #11July2017}

- **Probar en Slack**: Puede utilizar la nueva herramienta **Probar en Slack** para desplegar rápidamente su espacio de trabajo como usuario de bot de Slack para realizar pruebas. Esta herramienta sólo está disponible para la región EE. UU. sur de {{site.data.keyword.Bluemix_notm}}.
- **Actualizaciones para árabe**: Se ha mejorado el soporte del idioma árabe para incluir puntuación absoluta por intención y la posibilidad de marcar intenciones como irrelevantes; consulte el tema [Idiomas soportados](/docs/services/assistant?topic=assistant-language-support) para ver más detalles. Tenga en cuenta que los modelos de aprendizaje del servicio {{site.data.keyword.conversationshort}} pueden haberse actualizado como parte de esta mejora, y cuando se vuelva a formar el modelo se aplicarán los cambios; consulte [Modelos actualizados](#release-notes-updated-models) para obtener más información.

## 23 de junio de 2017
{: #23June2017}

- **Actualizaciones para coreano**: Se ha mejorado el soporte del idioma coreano; consulte el tema [Idiomas soportados](/docs/services/assistant?topic=assistant-language-support) para ver más detalles. Tenga en cuenta que los modelos de aprendizaje del servicio {{site.data.keyword.conversationshort}} pueden haberse actualizado como parte de esta mejora, y cuando se vuelva a formar el modelo se aplicarán los cambios; consulte [Modelos actualizados](#release-notes-updated-models) para obtener más información.

## 22 de junio de 2017
{: #22June2017}

- **Incorporación de ranuras**: Ahora es más fácil recopilar varios fragmentos de información de un usuario en un solo nodo mediante la adición de ranuras. Anteriormente, se tenían que crear varios nodos de diálogo para que cubrieran todas las combinaciones posibles de formas en que los usuarios pueden proporcionar información. Con las ranuras, puede configurar un solo nodo que guarde cualquier información proporcionada por el usuario y que solicite detalles necesarios que el usuario no ha proporcionado. Consulte [Obtención de información con ranuras](/docs/services/assistant?topic=assistant-dialog-slots) para ver más detalles.
- **Árbol de diálogo simplificado** - Se ha rediseñado el árbol de diálogo para facilitar su uso. La vista del árbol es más compacta para que resulte más fácil ver en qué punto se encuentra uno. Y los enlaces entre nodos se representan de forma que facilitan la comprensión de las relaciones entre los nodos.

## 21 de junio de 2017
{: #21June2017}

- **Soporte de árabe**: Ahora el soporte del idioma árabe está disponible a nivel general. Para obtener más información, consulte [Configuración de idiomas bidireccionales](/docs/services/assistant?topic=assistant-language-support#language-support-configuring-bi-directional).
- **Actualizaciones de idiomas**: Los algoritmos del servicio {{site.data.keyword.conversationshort}} se han actualizado para mejorar el soporte general de idiomas. Consulte el tema sobre [Idiomas soportados](/docs/services/assistant?topic=assistant-language-support) para obtener más información.

## 16 de junio de 2017
{: #16June2017}

- **Recomendaciones (solo usuarios de las versiones Beta - Premium)**: El panel Mejorar también incluye una página **Recomendaciones** que sugiere maneras de mejorar el sistema analizando las conversaciones que los usuarios tienen con el chatbot y teniendo en cuenta los datos de formación y la certeza de respuestas actuales del sistema.

## 14 de junio de 2017
{: #14June2017}

- **Coincidencia aproximada para idiomas adicionales (Beta)**: La coincidencia aproximada para entidades ahora está disponible para más idiomas, tal como se indica en el tema [Idiomas soportados](/docs/services/assistant?topic=assistant-language-support). Puede activar la coincidencia aproximada por entidad para mejorar la capacidad del servicio para reconocer términos de entrada de usuario con una sintaxis parecida a la entidad, pero sin necesidad de que la coincidencia sea exacta. La característica es capaz de correlacionar la entrada de usuario con la entidad adecuada correspondiente, a pesar de que contenga errores ortográficos o ligeras diferencias sintácticas. Por ejemplo, si define giraffe como sinónimo para una entidad animal y la entrada de usuario contiene los términos giraffes o girafe, la coincidencia aproximada es capaz de correlacionar correctamente el término con la entidad animal. Consulte [Coincidencia aproximada](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching) para ver detalles.

## 13 de junio de 2017
{: #13June2017}

- **Conversaciones de usuario**: El panel Mejorar ahora incluye una página **Conversaciones de usuario** que proporciona una lista de interacciones del usuario con el chatbot que se puede filtrar por palabra clave, intención, entidad o número de días. Puede abrir conversaciones individuales para corregir intenciones o para añadir valores de entidad o sinónimos.
- **Cambio de expresiones regulares (Regex)**: Las expresiones regulares que reciben soporte de funciones de SpEL como find, matches, extract, replaceFirst, replaceAll y split se han modificado. Ya no se permite un grupo de construcciones de expresiones regulares, incluidas las construcciones look-ahead, look-behind, possessive repetition y backreference. Este cambio responde a la necesidad de evitar exposiciones de seguridad en la biblioteca de expresiones regulares original.

## 12 de junio de 2017
{: #12June2017}

- El número máximo de espacios de trabajo que puede crear con el plan **Lite** (antes denominado plan Gratuito) ha pasado de 3 a 5.
- Ahora puede asignar cualquier nombre a un nodo del diálogo; no es necesario que sea exclusivo. Y por lo tanto puede cambiar el nombre de un nodo sin que ello afecte al modo en que se hace referencia al nodo internamente. El nombre que especifique se trata como un alias y el sistema utiliza su propio identificador interno para hacer referencia al nodo.
- Ya no se puede cambiar el idioma de un espacio de trabajo después de crearlo editando los detalles del espacio de trabajo. Si necesita cambiar el idioma, puede exportar el espacio de trabajo como un archivo JSON, actualizar la propiedad de idioma, y luego importar el archivo JSON como un nuevo espacio de trabajo.

## 6 de junio de 2017
{: #6June2017}

- **Información**: Dispone de una nueva página *Más información sobre {{site.data.keyword.conversationfull}}* que ofrece información de iniciación y enlaces con la documentación del servicio y otros recursos que le resultarán útiles. Para abrir la página, pulse el icono ![i para información](images/info.png) en la cabecera de la página.
- **Exportación y supresión masivas**: Ahora puede exportar simultáneamente varias intenciones o entidades a un archivo CSV, para luego poderlas importar y reutilizar para otra aplicación de {{site.data.keyword.conversationshort}}. También puede seleccionar simultáneamente varias entidades o intenciones para suprimirlas simultáneamente.
- **Actualizaciones para coreano**: Se han actualizado cuatro señaladores de coreano para dar soporte al lenguaje informal. IBM sigue trabajando para mejorar el reconocimiento y la clasificación de entidades.
- **Soporte de emojis**: Los emojis que se añaden a ejemplos de intenciones, o como valores de entidad, ahora se clasificarán y extraerán correctamente.

  Solo los emojis incluidos en los datos de entrenamiento se identificarán correctamente; es posible que el soporte de emojis no clasifique correctamente emojis parecidos con distintos tonos de colores u otras variaciones.
  {: note}

- **Lematización de entidades (solo versiones Beta - inglés)**: La característica beta de coincidencia aproximada reconoce entidades y las compara en función de la forma del lema del valor de la entidad. Por ejemplo, esta característica reconoce correctamente 'bananas' como similar a 'banana' y 'run' como similar a 'running' ya que comparten un lexema común. Para obtener más información, consulte [Coincidencia aproximada](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching).
- **Progreso de la importación de espacios de trabajo**: Cuando importa un espacio desde un archivo JSON, se muestra inmediatamente un mosaico correspondiente al espacio de trabajo, en el que puede ver información sobre el progreso de la importación.
- **Reducción del tiempo de formación**: Ahora se forman varios modelos en paralelo, lo que reduce significativamente el tiempo de formación para espacios de trabajo de gran tamaño.

## 26 de mayo de 2017
{: #26May2017}

- Ahora la versión de API actual es `2017-05-26`. Esta versión presenta los cambios siguientes:
    - El esquema de objetos ErrorResponse se ha modificado. Este cambio afecta a todos los puntos finales y métodos. Para obtener más información, vea la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/assistant){: new_window}.
    - El esquema interno utilizado para representar nodos del diálogo en el archivo JSON del espacio de trabajo exportado ha cambiado. Si utiliza la API `2017-05-26` para importar un espacio de trabajo que se ha exportado utilizando una versión anterior, es posible que algunos nodos del diálogo no se importen correctamente. Para obtener mejores resultados, importe siempre un espacio de trabajo utilizando la misma versión que utilizó para exportarlo.

## 25 de mayo de 2017
{: #25May2017}

- Ahora puede gestionar variables de contexto en el panel "Pruébelo". Pulse el enlace **Gestionar contexto** para abrir un nuevo panel donde puede establecer y comprobar los valores de las variables de contexto cuando prueba el diálogo. Consulte [Prueba del diálogo](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-test) para obtener más información.

## 16 de mayo de 2017
{: #16May2017}

- Ahora dispone de un espacio de trabajo de ejemplo denominado **Car Dashboard** cuando abre la herramienta. Para utilizar el ejemplo como punto de partida para su propio espacio de trabajo, edite el espacio de trabajo. Si desea utilizarlo para varios espacios de trabajo, duplíquelo. El espacio de trabajo de ejemplo no cuenta en el total de espacios de trabajo de la suscripción, a menos que lo utilice.
- Ahora resulta más sencillo navegar por la herramienta. Las opciones del menú de navegación se muestran en el lateral de la página principal en lugar de en la parte superior. En la parte superior de la página, se muestran los enlaces de la ruta de navegación para mostrarle dónde está. Ahora puede conmutar entre instancias del servicio desde la página Espacios de trabajo. Para hasta allí rápidamente, pulse **Volver a espacios de trabajo** en el menú de navegación. Si tiene varias instancias del servicio, se muestra el nombre de la instancia actual. Puede pulsar el enlace **Cambiar** que hay junto a la instancia para elegir otra instancia.
- Cuando se crea un diálogo, ahora se añaden automáticamente a la misma dos nodos: 1) un nodo **Welcome** en la parte superior del árbol del diálogo que contiene el saludo que se mostrará al usuario y 2) un nodo **Anything else** en la parte inferior del árbol que detecta las consultas de los usuarios que no reconocen otros nodos del diálogo y responde a las mismas. Consulte [Creación de un diálogo](/docs/services/assistant?topic=assistant-dialog-build) para obtener más detalles.
- Cuando pruebe un diálogo en el panel "Pruébelo", ahora puede encontrar y volver a enviar una expresión de prueba reciente pulsando la tecla Subir para recorrer entradas anteriores.
- Ahora dispone de soporte experimental del idioma coreano para 5 entidades del sistema (`@sys-date`, `@sys-time`, `@sys-currency`, `@sys-number`, `@sys-percentage`). Existen problemas conocidos sobre algunas de las entidades numéricas y soporte limitado para entradas en lenguaje informal.
- Dispone de una página Visión en el separador Mejorar. La página proporciona un resumen de las interacciones con el bot. Puede ver la cantidad de tráfico durante un periodo de tiempo determinado, así como las intenciones y entidades que se han reconocido con mayor frecuencia en las conversaciones de los usuarios. Para obtener más información, consulte [Utilización de la página Visión general](/docs/services/assistant?topic=assistant-logs-overview).

## 27 de abril de 2017
{: #27April2017}

- Las siguientes entidades del sistema están ahora disponibles como características beta solo en inglés:
    - sys-location: Reconoce referencias a ubicaciones, como pueblos, ciudades y países, en expresiones de usuario.
    - sys-person: Reconoce referencias a nombres de personas, nombre y apellido, en expresiones de usuario.

    Para obtener más información, vea la [Consulta de entidades del sistema](/docs/services/assistant?topic=assistant-system-entities).
- La coincidencia aproximada para entidades es una característica beta que ahora está disponible en inglés. Puede activar la coincidencia aproximada por entidad para mejorar la capacidad del servicio para reconocer términos de entrada de usuario con una sintaxis parecida a la entidad, pero sin necesidad de que la coincidencia sea exacta. La característica es capaz de correlacionar la entrada de usuario con la entidad adecuada correspondiente, a pesar de que contenga errores ortográficos o ligeras diferencias sintácticas. Por ejemplo, si define **giraffe** como sinónimo para una entidad animal y la entrada de usuario contiene los términos *giraffes* o *girafe*, la coincidencia aproximada es capaz de correlacionar correctamente el término con la entidad animal. Consulte [Definición de entidades](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching) y busque `Coincidencia aproximada` para obtener detalles.

## 18 de abril de 2017
{: #18April2017}

- Ahora la API REST de {{site.data.keyword.conversationshort}} da soporte al acceso a los siguientes recursos:
    - entidades
    - valores de entidad
    - sinónimos de valores de entidad
    - registros

    Para obtener más información, vea la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/assistant){: new_window}.
- El comportamiento del método `POST` /messages ha modificado el manejo de entidades e intenciones especificadas como parte de la entrada del mensaje:
    - Si especifica intenciones en la entrada, el servicio utiliza las intenciones que especifica, pero utiliza el proceso de lenguaje natural para detectar entidades en la entrada de usuario.
    - Si especifica entidades en la entrada, el servicio utiliza las entidades que especifica, pero utiliza el proceso de lenguaje natural para detectar intenciones en la entrada de usuario.

        El comportamiento no ha cambiado para los mensajes que especifican tanto intenciones como entidades ni para los mensajes que no especifican ninguna de ellas.
- La opción para marcar la entrada de usuario como irrelevante ahora está disponible para todos los idiomas soportados. Esta es una característica beta.
- Un nuevo separador Credenciales proporciona una ubicación única en la que puede encontrar toda la información que necesita para conectar la aplicación a un espacio de trabajo (como credenciales de servicio y el ID del espacio de trabajo), así como otras opciones de despliegue. Para acceder al separador Credenciales correspondiente a su espacio de trabajo, pulse el icono ![Menú](images/Menu_16.png) y seleccione **Credenciales**.

## 9 de marzo de 2017
{: #9March2017}

Ahora la API REST de {{site.data.keyword.conversationshort}} da soporte al acceso a los siguientes recursos:

- espacios de trabajo
- intenciones
- ejemplos
- contraejemplos

Para obtener más información, vea la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/assistant){: new_window}.

## 7 de marzo de 2017
{: #7March2017}

- El uso de `.` o `..` como nombre de intención ocasiona problemas y ya no recibe soporte.

    No puede cambiar el nombre ni suprimir una intención con este nombre; para cambiar el nombre, exporte las intenciones a un archivo, cambie el nombre de la intención en el archivo e importar el archivo actualizado en el espacio de trabajo.

    Los clientes de pago pueden ponerse en contacto con el equipo de soporte para un cambio de base de datos.

## 1 de marzo de 2017
{: #1March2017}

- Ahora las entidades del sistema están habilitadas en alemán.

## 22 de febrero de 2017
{: #22February2017}

- Ahora los mensajes están limitados a 2.048 caracteres.

## 3 de febrero de 2017
{: #3February2017}

- Se ha cambiado la forma en la que se puntúan las intenciones y se ha añadido la posibilidad de marcar una entrada como irrelevante para la aplicación. Consulte [Definición de intenciones](/docs/services/assistant?topic=assistant-intents) y busque `Marcar como irrelevante` para obtener más información.

- Este release presenta un cambio sustancial al espacio de trabajo. Para beneficiarse de los cambios, debe actualizar manualmente el espacio de trabajo.

- El proceso de las acciones **Ir a** se ha modificado para evitar los bucles que se pueden producir bajo determinadas circunstancias. Anteriormente, si iba a la condición de un nodo y ni dicho nodo ni ninguno de sus nodos iguales tenían una condición que se evaluara como verdadera, el sistema saltaba al nodo de nivel raíz y buscaba un nodo cuya condición coincidiera con la entrada. En algunas situaciones este proceso creaba un bucle, que impedía el progreso del diálogo.

  Con el nuevo proceso, si ni el nodo de destino ni sus iguales se evalúan como verdaderos, la ronda del diálogo finaliza. Para volver a implementar el modelo anterior, añada un nodo igual final con la condición `true`. En la respuesta, utilice una acción **Ir a** destinada a la condición del primer nodo en el nivel raíz del árbol del diálogo.

## 11 de enero de 2017
{: #11January2017}

- En este release, puede personalizar los títulos de los nodos en el diálogo.

## 22 de diciembre de 2016
{: #22December2016}

- En este release, los nodos del diálogo muestran una nueva sección para `título de nodo`. Ahora se puede personalizar el `título de nodo`. Cuando está contraído, el `título de nodo` muestra la `condición de nodo` del nodo del diálogo. Si no hay ninguna `condición de nodo`, se muestra como título "Nodo sin título".

## 19 de diciembre de 2016
{: #19December2016}

Se han realizado cambios en el editor de diálogos para que resulte más intuitivo:

- Una vista de edición mayor facilita la consulta de todos los detalles de un nodo cuando trabaja en el mismo.
- Un nodo puede contener varias respuestas, cada una activada por una condición distinta. Para obtener más información, consulte [Varias respuestas](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses).

## 5 de diciembre de 2016
{: #5December2016}

- Se da soporte a nuevos idiomas, todos en modalidad experimental: alemán, chino tradicional, chino simplificado y neerlandés.
- Están disponibles dos nuevas entidades del sistema: @sys-date y @sys-time. Para obtener más información, consulte [Entidades del sistema](/docs/services/assistant?topic=assistant-system-entities).

## 21 de octubre de 2016
{: #21October2016}

- Ahora el servicio {{site.data.keyword.conversationshort}} proporciona entidades del sistema, que son entidades comunes que puede utilizar en cualquier caso de uso. Para obtener más información, consulte [Definición de entidades](/docs/services/assistant?topic=assistant-entities) y busque `Habilitación de entidades`.
- Ahora puede ver un historial de conversaciones con usuarios en la página Mejorar. Puede utilizarlo para comprender el comportamiento de su bot. Para obtener más información, consulte [Mejora de su conocimiento](/docs/services/assistant?topic=assistant-logs-intro).
- Ahora puede importar entidades desde un archivo de valores separados por comas (CSV), lo que le resultará de ayuda si tiene un gran número de entidades. Para obtener más detalles, consulte [Definición de entidades](/docs/services/assistant?topic=assistant-entities) y busque `Importación de entidades`.

## 20 de septiembre de 2016
{: #20September2016}

**Nueva versión**: 2016-09-20

Para aprovechar los cambios de una nueva versión, cambie el valor del parámetro `version` por la nueva fecha. Si no está listo para actualizar a esta versión, no cambie la fecha de versión.

- Versión **2016-09-20**: `dialog_stack` ha pasado de una matriz de series a una matriz de objetos JSON.

## 29 de agosto de 2016
{: #29August2016}

- Puede mover nodos del diálogo de una rama a otra, como hermanos o iguales. Para obtener más detalles, consulte [Cómo mover un nodo del diálogo](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-move-node).
- Puede expandir la ventana del editor de JSON.
- Puede ver los registros del chat de las conversaciones del bot para comprender mejor su comportamiento. Puede filtrar por intenciones, entidades, fecha y hora. Para obtener más información, consulte [Mejora de su conocimiento](/docs/services/assistant?topic=assistant-logs-intro)

## 11 de julio de 2016
{: #21July2016}

- Este release de disponibilidad general (GA) le permite trabajar con entidades y diálogos para crear un bot completamente funcional.

## 18 de mayo de 2016
{: #18May2016}

- Este release experimental de {{site.data.keyword.conversationshort}} incorpora la interfaz de usuario y le permite trabajar con espacios de trabajo, intenciones y ejemplos.
