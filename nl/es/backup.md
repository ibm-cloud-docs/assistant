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

# Copia de seguridad y restauración de datos
{: #backup}

Realice una copia de seguridad y restaure los datos exportando y luego importando los datos.
{: shortdesc}

Puede exportar los siguientes datos desde una instancia de servicio de {{site.data.keyword.conversationshort}}:

- Datos de entrenamiento de conocimientos de diálogo (intenciones y entidades)
- Diálogo de conocimientos de diálogo

No puede exportar los siguientes datos:

<!--- Search skill -->
- Asistente, incluidas las integraciones configuradas

## Conservación de registros
{: #backup-retain-logs}

Si desea guardar registros de conversaciones que los usuarios han mantenido con su asistente, puede utilizar la API `/logs` para exportar los datos de registro. Revise la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/apidocs/assistant#list-log-events-in-a-workspace) para obtener más detalles.

Para ver el ID del espacio de trabajo de un conocimiento, en el mosaico del conocimiento pulse el icono ![abrir y cerrar lista de opciones](images/kabob-beta.png) y luego seleccione **Ver detalles de API**.
{: tip}

Los registros se guardan durante un periodo de tiempo que depende de su plan de servicio. Por ejemplo, los planes Lite solo proporcionan registros de los últimos 7 días. Consulte [Límites de registro](/docs/services/assistant?topic=assistant-logs#logs-limits) para obtener más información.

No se pueden importar registros de un conocimiento en otro conocimiento.

## Exportación de un conocimiento de diálogo
{: #backup-export-skill}

Para hacer una copia de seguridad de los datos de un conocimiento de diálogo, exporte el conocimiento como archivo JSON y luego guarde el archivo JSON.

1.  Localice el mosaico del conocimiento en la página Conocimientos o en la página de configuración de un asistente que utiliza el conocimiento.

1.  Pulse el icono ![abrir y cerrar lista de opciones](images/kabob-beta.png) y luego seleccione **Descargar JSON**.

1.  Especifique un nombre para el archivo JSON y dónde guardarlo y pulse **Guardar**.

Como alternativa, puede utilizar la API `/workspaces` para exportar un conocimiento de diálogo. Incluya el parámetro `export=true`
con la solicitud de espacio de trabajo GET. Revise la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace) para obtener más detalles.

## Importación de un conocimiento de diálogo
{: #backup-import-skill}

Para crear una instancia de una copia de seguridad de un conocimiento de diálogo que ha exportado desde otra instancia de servicio o entorno, cree un nuevo conocimiento de diálogo importando el archivo JSON del conocimiento de diálogo que ha exportado.

Si el servicio {{site.data.keyword.conversationshort}} cambia entre el momento en que exporta el conocimiento y el momento en que lo importa, debido a las actualizaciones funcionales que se aplican con regularidad a instancias en entornos de entrega continua alojadas en la nube, el conocimiento importado podría funcionar de forma distinta al que tenía antes.
{: important}

1.  Pulse el separador **Conocimientos**.

1.  Pulse **Crear nuevo**.

1.  Pulse **Importar conocimiento** y luego pulse **Elegir archivo JSON** y seleccione el archivo JSON que desea importar.

    **Importante:**

    - El archivo JSON importado debe utilizar la codificación UTF-8, sin la codificación de marca de orden de bytes (BOM).
    - El tamaño máximo de un archivo JSON de conocimiento es 10 MB. Si tiene que importar un conocimiento mayor, considere la posibilidad de importar las intenciones y las entidades por separado después de haber importado el conocimiento. (También puede importar conocimientos grandes utilizando la API REST. Para obtener más información, consulte la [Referencia de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/apidocs/assistant#create-workspace){: new_window}).
    - El archivo JSON no puede contener tabuladores, saltos de página ni retornos de carro.

    Seleccione **Todo (intenciones, entidades y diálogo)** para importar una copia completa del conocimiento exportado.

    Pulse **Importar**.

    Si tiene problemas para importar un conocimiento, consulte el apartado sobre [Resolución de problemas de importación de conocimientos](/docs/services/assistant?topic=assistant-skill-add#skill-add-import-errors).

1.  Especifique los detalles del conocimiento:

    - **Nombre**: un nombre no puede contener más de 100 caracteres. El nombre es obligatorio.
    - **Descripción**: una descripción no puede contener más de 200 caracteres.
    - **Idioma**: el idioma de la entrada de usuario para el que se ha entrenado el conocimiento. El valor predeterminado es el inglés.

Después de crear el conocimiento, aparece como un mosaico en la página Conocimientos.

## Cómo volver a crear su asistente
{: #backup-recreate-assistant}

Ahora puede volver a crear su asistente. Luego puede enlazar el conocimiento de diálogo importado en el asistente y configurar integraciones para el mismo.

Consulte [Creación de un asistente](/docs/services/assistant?topic=assistant-assistant-add) y [Adición de integraciones](/docs/services/assistant?topic=assistant-deploy-integration-add#deploy-integration-add-task) para obtener más detalles.

## Actualización de aplicaciones cliente
{: #backup-update-api}

Cuando importa un conocimiento de diálogo que ha exportado, se crea un nuevo conocimiento. El nuevo conocimiento tiene un nuevo ID de espacio de trabajo. Si tiene aplicaciones cliente que utilizan la API v1 para acceder a este conocimiento, debe actualizar las referencias al ID de espacio de trabajo para que utilicen el nuevo ID de espacio de trabajo.

Cuando vuelva a crear el asistente, se le asignará un nuevo ID de asistente. Si tiene aplicaciones cliente que utilizan la API v2 para acceder al conocimiento, debe actualizar las referencias al ID del asistente para que utilicen el nuevo ID de asistente.
