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

# Migración
{: #migrate}

Migre a una instancia de servicio de {{site.data.keyword.conversationshort}} para que pase de su organización y espacio actuales de Cloud Foundry a un grupo de recursos.
{: shortdesc}

{{site.data.keyword.cloud_notm}} ha pasado de utilizar Cloud Foundry a utilizar grupos de recursos.

Los grupos de recursos ofrecen estas ventajas sobre Cloud Foundry:

- Un mayor control del acceso mediante IBM Cloud Identity and Access Management (IAM)
- Posibilidad de conectar instancias de servicio a apps y servicios en distintas regiones
- Simplifica la captura de datos de uso por grupo

Si ha creado instancias de servicio antes de noviembre de 2018, en función de la ubicación en la que esté alojada la instancia es posible que esté utilizando Cloud Foundry en lugar de grupos de recursos. Consulte [Centros de datos](/docs/services/assistant?topic=assistant-services-information#services-information-regions) para obtener información sobre cuándo cada ubicación ha empezado a utilizar IAM con instancias nuevas.

## Migración de una instancia de servicio
{: #migrate-task}

Si desea obtener más información sobre el proceso de migración antes de empezar, consulte la [documentación sobre migración de IBM Cloud ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/resources?topic=resources-migrate).

Solo la persona o el grupo que ha creado la instancia o que dispone del de rol de `Desarrollador` sobre la instancia puede migrarla.
{: note}

Para migrar la instancia de servicio, siga estos pasos:

1.  En primer lugar, determine el grupo de recursos al que desea mover la instancia de servicio.

    Consulte las [Prácticas recomendadas para organizar recursos en un grupo de recursos ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/resources?topic=resources-bp_resourcegroups) para ver sugerencias.

1.  En la lista de servicios del panel de control de IBM Cloud, pulse el icono de migración ![Migrar](images/migrate.svg) para ver la instancia que desea migrar y luego pulse **Migrar** en la ventana emergente.

    Las instancias de servicio creadas en el centro de datos de Sídney antes del 7 de mayo de 2018 o en el centro de datos de Londres antes del 13 de diciembre de 2018 se han sindicado con el centro de datos de Dallas. Si migra una instancia de servicio de Cloud Foundry alojada en Sídney o en Londres, se convierte en un recurso alojado en Dallas.
    {: note}

1.  Pulse **Continuar** y luego elija un grupo de recursos.

    Si no ha creado un grupo de recursos, puede crear uno ahora. Dispone de un grupo de recursos **predeterminado**. Tómese su tiempo para entender cómo se utilizan los grupos y para crear uno si es necesario. El grupo de recursos que elija aquí *no se puede* cambiar posteriormente.

1.  Pulse **Migrar**.

    Se muestra un mensaje cuando finaliza el proceso. Si tiene que migrar otras instancias de servicio, puede seguir migrando otras instancias de servicio o bien puede pulsar **Listo**.

La instancia de servicio antigua (basada en organización de Cloud Foundry) que ha migrado se sigue mostrando en la lista de la sección de servicios de Cloud Foundry en el panel de control, y ahora se muestra como un *alias* de la nueva versión (basada en grupos de recursos) de la instancia.

![Muestra que la instancia de servicio actual ahora es un alias de una instancia basada en recurso](images/alias.png)

Para obtener más información sobre alias, consulte la documentación de [IBM Cloud Connections ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/docs/resources/connecting_apps#what_is_alias).

Debe abrir la nueva versión basada en grupo de recursos de la instancia de servicio para acceder al botón **Iniciar herramienta**. La nueva instancia aparece en la sección Servicios del panel de control de {{site.data.keyword.Bluemix_notm}}.

## Autenticación
{: #migrate-auth-support}

Si tiene aplicaciones existentes que utilizan la autenticación básica para acceder al servicio, pueden seguir pasando un nombre de usuario y una contraseña para autenticarse con la instancia de servicios después de la migración. Verá la información de credenciales en la página **Credenciales de servicio** del alias.

Su nueva instancia gestiona la autenticación con IBM Cloud Identity and Access Management (IAM), que es un mecanismo mejorado que utiliza claves de API en lugar de credenciales de nombre de usuario y contraseña. Verá la información de claves de API en la página **Credenciales de servicio** de la nueva instancia de servicio.

Tenga en cuenta la posibilidad de actualizar las aplicaciones personalizadas existentes para que utilicen el nuevo método de autenticación para sacar el máximo provecho de la seguridad mejorada que ofrece. Los cambios que realice en los conocimientos de diálogo en la nueva instancia también se ven reflejados en las aplicaciones que utilizan credenciales de autenticación básica. Después de actualizar todas las apps para que utilicen el nuevo enfoque de clave de API, no necesitará el alias y podrá suprimirlo.
