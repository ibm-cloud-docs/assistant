---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

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

# Información sobre los servicios de IBM Cloud
{: #services-information}

El asistente es un bot alojado completamente que está gestionado por {{site.data.keyword.cloud}}, lo que significa que no tiene que preocuparse por configurar o mantener la infraestructura para darle soporte.
{: shortdesc}

## Información sobre el plan de servicio
{: #services-information-plans}

Consulte las {{site.data.keyword.conversationshort}} [opciones del plan de servicio ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window}.

Antes de crear una instancia de servicio, decida cómo desea organizar los recursos en la cuenta de {{site.data.keyword.cloud_notm}}. Si no define su propio grupo de recursos, se utiliza el grupo de recursos **default**, que *no puede* cambiar posteriormente. Consulte las [Prácticas recomendadas para organizar recursos en un grupo de recursos ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/docs/resources/bestpractice_rgs#bp_resourcegroups){: new_window} para obtener más información. Todos los usuarios deben tener un rol de acceso de plataforma de Operador. ({{site.data.keyword.conversationshort}} no utiliza los roles de acceso al servicio).

Para averiguar el plan de servicio al que pertenece la instancia actual, siga estos pasos:

1.  Anote el nombre de la instancia que está utilizando actualmente. (Puede encontrar y cambiar la instancia de las páginas principales Conocimientos o Asistentes).
1.  Acceda a la página [lista de recursos de IBM Cloud ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/resources).
1.  Expanda la sección **Servicios**, busque el nombre de instancia que ha anotado anteriormente y pulse en él para ver la información de plan asociada.

### Límites de plan por tipo de artefacto
{: #services-information-limits}

Dispone de información sobre los límites de artefactos por plan en los temas en los que se describe cómo crear artefactos para que pueda consultar los límites cuando lo necesite. Estos son los enlaces a los temas:

- [Asistentes](/docs/services/assistant?topic=assistant-assistant-add#assistant-add-limits)
- [Nodos del diálogo](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-node-limits)
- [Entidades](/docs/services/assistant?topic=assistant-entities#entities-limits)
- [Tiempo de espera excedido de inactividad](/docs/services/assistant?topic=assistant-assistant-settings#assistant-settings-session-limits)
- [Intenciones](/docs/services/assistant?topic=assistant-intents#intents-limits)
- [Integraciones](/docs/services/assistant?topic=assistant-deploy-integration-add#deploy-integration-add-limits)
- [Registros](/docs/services/assistant?topic=assistant-logs#logs-limits)
- [Conocimientos](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)
- [Versiones](/docs/services/assistant?topic=assistant-versions#versions-limits)

### Límites en las llamadas de API
{: #services-information-api-limits}

El número de llamadas de API permitidas por instancia depende de su plan de servicio. Consulte la descripción de su plan para obtener detalles.

Si tiene un plan Lite y alcanza el límite de llamadas de API, pero los registros muestran que ha realizado menos llamadas de lo esperado, recuerde que el plan Lite almacena la información de registro solo durante 7 días.

Si desea actualizar de un plan a otro, consulte el apartado sobre [Actualización](/docs/services/assistant?topic=assistant-upgrade).

### Características de los planes Plus y Premium ![solo el plan Plus y Premium](images/plus.png)
{: #services-information-premium}

Las siguientes características sólo están disponibles para los usuarios de los planes Plus o Premium.

- [Desambiguación](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)
- [Resolución de conflictos de intenciones](/docs/services/assistant?topic=assistant-intents#intents-resolve-conflicts)
- [Recomendaciones de intención y recomendaciones de ejemplo de usuario de intención](/docs/services/assistant?topic=assistant-intent-recommendations)
- [Integración de Intercom](/docs/services/assistant?topic=assistant-deploy-intercom)
- [Conocimiento de búsqueda](/docs/services/assistant?topic=assistant-skill-search-add)

### Planes basados en el usuario
{: #services-information-user-based-plans}

A diferencia de los planes basados en API, que miden el uso por el número de llamadas de API realizadas durante un periodo de tiempo especificado, el nuevo plan Plus y el plan Premium actualizado utilizan una facturación basada en el usuario. Miden el uso por el número de usuarios exclusivos que han interactuado con el asistente durante un periodo de tiempo especificado.

{{site.data.keyword.conversationshort}} comprueba la información siguiente de las solicitudes de API en este orden para la facturación:

  1.  **user_id**: propiedad definida en la API que se envía en el objeto de contexto de una llamada de API /message. Esta propiedad constituye la mejor forma de asegurarse de que se envían llamadas de API /message a usuarios exclusivos. Para obtener más información sobre la propiedad de ID de usuario, consulte la documentación de referencia de la API:
  
    - `context.global.system.user_id`: [API v2](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant)
    - `context.metadata.user_id`: [API v1](https://cloud.ibm.com/apidocs/assistant#get-response-to-user-input)

  1.  **session_id**: propiedad definida en la API v2 que identifica una sola conversación entre un usuario y el asistente. Se proporciona un ID de sesión en las llamadas de API /message que generan las integraciones incorporadas. La sesión finaliza cuando un usuario cierra la ventana de conversación o después de que la conversación esté inactiva durante 60 minutos.

  1.  **conversation_id**: propiedad definida en la API v1 que se almacena en el objeto de contexto de una llamada de API /message. Esta propiedad se puede utilizar para identificar varias llamadas de API /message asociadas a un solo intercambio de conversaciones con un usuario. Sin embargo, solo se utiliza el mismo ID si se retiene explícitamente el ID y se pasa de nuevo con cada solicitud que se realiza como parte de la misma conversación. De lo contrario, se genera un nuevo ID para cada nueva llamada de API /message.

Para sacar el máximo provecho de los nuevos planes de servicio basados en el usuario, diseñe las aplicaciones personalizadas que utilice para desplegar su asistente de modo que capturen un ID de usuario o un ID de sesión exclusivo y pasen la información a {{site.data.keyword.conversationshort}}.

## Autenticación de llamadas de API
{: #services-information-authenticate-api-calls}

El mecanismo de autenticación que utiliza la instancia de servicio afecta a la forma en que debe proporcionar las credenciales al realizar una llamada de API.

1.  Obtenga las credenciales de servicio.

    - Busque y pulse la instancia de servicio en la [Lista de recursos de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com){: new_window}.

    - Pulse para abrir la instancia de servicio, pulse **Credenciales de servicio** y luego pulse **Ver credenciales**.

      **Credenciales de Cloud Foundry**

      ![Muestra la página de credenciales de servicio correspondiente a instancias alojadas en Cloud Foundry.](images/cf-cred-ui.png)

      **Credenciales de IAM**

      ![Muestra la página de credenciales de servicio para instancias alojadas por IAM.](images/iam-creds.png)

1.  Utilice estas credenciales en la llamada de API.

    **Llamada de API de Cloud Foundry**

    Especifique sus credenciales de nombre de usuario y contraseña.

    ```curl
    curl -X GET \
    --user {username}:{password} \
    'https://gateway.watson.net/assistant/api/v1/workspaces?version=2018-09-20'
    ```
    {: codeblock}

     **Llamada de API de IAM**

    - El URL base debe incluir la ubicación. Utilice la sintaxis `gateway-<location>.watsonplatform.net` para especificar la ubicación en la que ha creado la instancia de servicio. Encontrará los códigos de ubicación en la tabla *Ubicaciones de centros de datos*.
    - Especifique el tipo de señal adecuado en la cabecera. Puede pasar una señal de portadora (bearer) o una clave de API.

      - Las señales dan soporte a solicitudes autenticadas sin incluir credenciales de servicio en cada llamada. En el ejemplo siguiente se muestra el uso de una señal de portadora.

        ```curl
        curl -X GET \
        'https://gateway-syd.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        --header 'Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs'
        ```
        {: codeblock}

      - Las claves de API utilizan la autenticación básica. En el ejemplo siguiente se muestra el uso de una clave de API.

        ```curl
        curl -X GET -u "apikey:3Df... ...Y7Pc9" \
        'https://gateway-us-east.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        ```
        {: codeblock}

        Cuando utilice cualquiera de los SDK de Watson, puede pasar la clave de la API y dejar que el SDK gestione el ciclo de vida de las señales.
        {: note}

        Los recursos de IAM no se pueden gestionar con la interfaz de línea de mandatos (CLI) de Cloud Foundry. Por ejemplo, los mandatos de CLI de Cloud Foundry (que empiezan por `cf`) que crean o gestionan instancias de servicio no funcionan con instancias alojadas en ubicaciones que utilizan IAM. Debe utilizar la CLI de {{site.data.keyword.cloud_notm}} y sus mandatos asociados. Consulte [Cómo trabajar con recursos y con grupos de recursos ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource) para ver más detalles.

        Consulte el apartado [Autenticación con señales de IAM ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/services/watson?topic=watson-iam){: new_window} para obtener más información.

    Por ejemplo, consulte [Autenticación ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/apidocs/assistant-v2#authentication){: new_window} correspondiente a su idioma en la consulta de API.

### Centros de datos
{: #services-information-regions}

{{site.data.keyword.cloud_notm}} tiene una red de centros de datos globales que proporcionan ventajas en cuanto a rendimiento a sus servicios de nube. Consulte [Centros de datos globales de {{site.data.keyword.cloud_notm}}
![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/data-centers/){: new_window} para ver más detalles.

{{site.data.keyword.cloud_notm}} ha pasado de gestionar el acceso de los usuarios con Cloud Foundry a utilizar la autenticación de IAM (Identity and Access Management) basada en señales. IAM se ha ido incorporando a distintas ubicaciones en distintos momentos. Puede migrar una instancia de servicio para que pase de su organización y espacio actuales de Cloud Foundry a un grupo de recursos. Consulte [Migración](/docs/services/watson?topic=watson-migrate) para obtener más detalles.

Puede crear instancias del servicio {{site.data.keyword.conversationshort}} alojadas en los siguientes centros de datos:

| Ubicación    | Código de ubicación | Tipo de autenticación | Fecha de adopción de IAM | Notas |
|-------------|---------------|---------------------|-------------------|-------|
| Dallas      | us-south      | IAM                 | 30 de octubre de 2018 | N/D |
| Frankfurt   | eu-de         | IAM                 | 30 de octubre de 2018 | N/D |
| Sídney      | au-syd        | IAM                 | 7 de mayo de 2018 | Las instancias creadas antes del 7 de mayo se han sindicado con Dallas |
| Tokio       | jp-tok        | IAM                 | 8 de noviembre de 2018 | N/D |
| Londres      | eu-gb, lon    | IAM                 | 13 de diciembre de 2018 | Las instancias creadas en la región del Reino Unido antes del 13 de diciembre se han sindicado con la región EE. UU. sur |
| Washington DC  | us-east    | IAM                 | 14 de junio de 2018 | N/D |
{: caption="Ubicaciones de centros de datos" caption-side="top"}

Para obtener información sobre los centros de datos en los que están alojados otros servicios de {{site.data.keyword.cloud_notm}}, consulte [Servicios por región ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/docs/resources/services_region#services_region){: new_window}.

## Términos y seguridad
{: #services-information-terms}

Para obtener más información sobre los términos de servicio y la seguridad de los datos, lea la información siguiente:

- [Términos de servicio ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/home?OpenDocument){: new_window}
- [Seguridad y privacidad de los datos ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/software/sla/sladb.nsf/sla/csdsp?OpenDocument){: new_window}
- [Seguridad de la información](/docs/services/assistant?topic=assistant-information-security)

Consulte [Visión general de la plataforma ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/overview?topic=overview-whatis-platform){: new_window} para obtener más información sobre {{site.data.keyword.cloud_notm}}.

## ¿Le queda alguna duda? 
{: #services-information-sales}

Póngase en contacto con el equipo de [ventas de IBM ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.
