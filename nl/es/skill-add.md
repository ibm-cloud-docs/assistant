---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-08"

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

# Creación de un conocimiento
{: #skill-add}

El proceso de lenguaje natural para el servicio {{site.data.keyword.conversationshort}} se define en un *conocimiento de diálogo*,
que es un contenedor para todos los artefactos que definen un flujo de conversación.
{: shortdesc}

Puede añadir un conocimiento a un asistente.

Puede crear un conocimiento desde cero, utilizar un conocimiento de ejemplo proporcionado por IBM o importar un conocimiento desde un archivo JSON. Para añadir un conocimiento, siga los siguientes pasos:

1.  Si no ha creado una instancia del servicio {{site.data.keyword.conversationshort}}, realice primero este procedimiento de una sola vez. De lo contrario, omita este paso.

    1.  Vaya a la página [{{site.data.keyword.conversationshort}} ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/catalog/services/watson-assistant) en el catálogo de
{{site.data.keyword.cloud_notm}}.

    1.  Regístrese para obtener una cuenta de {{site.data.keyword.cloud_notm}} o inicie una sesión.

        La instancia de servicio se creará en el grupo de recursos **default** si no elige otro y *no se puede* cambiar posteriormente. El valor *default* suele ser suficiente. No obstante, si desea obtener más información sobre los grupos de recursos, consulte la [documentación de {{site.data.keyword.Bluemix_notm}}![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://{DomainName}/docs/resources?topic=resources-bp_resourcegroups#bp_resourcegroups){: new_window}.

        Puede utilizar una instancia de servicio para desarrollar, probar y desplegar un asistente, de modo que no cree grupos de recursos distintos para distintos tipos de entornos de despliegue.
        {:tip}

        Con un plan Premium, puede crear varias instancias. Todas las instancias se deben crear en el mismo grupo de recursos.

    1.  Pulse **Crear**.

    1.  Pulse **Iniciar herramienta**. Si se le solicita que inicie una sesión en la herramienta, proporcione sus credenciales de {{site.data.keyword.cloud_notm}}.

1.  Pulse el separador **Conocimientos**.

    Si ha creado o si ha recibido acceso con el rol de desarrollador a los espacios de trabajo creados con la versión disponible a nivel general del servicio {{site.data.keyword.conversationshort}} (antes llamado Watson Conversation), los verá en la lista como conocimientos de diálogo.
    {: note}

1.  Pulse **Crear nuevo**.

1.  Realice una de las acciones siguientes:

    - Para crear un conocimiento desde cero, pulse **Crear conocimiento**.
    - Para añadir un conocimiento de ejemplo que se proporciona con el servicio como punto de partida para su propio conocimiento o como ejemplo que explorar antes de crear uno, pulse **Utilizar conocimiento de ejemplo** y luego pulse el ejemplo que desea utilizar.

      El conocimiento de ejemplo se añade a la lista de conocimientos. No está asociado con ningún asistente. Sáltese el resto de los pasos de este procedimiento.

    - Para añadir un conocimiento existente a esta instancia de servicio, puede importarla como un archivo JSON. Pulse **Importar conocimiento** y luego pulse **Elegir archivo JSON** y seleccione el archivo JSON que desea importar.

      **Importante:**

      - El archivo JSON importado debe utilizar la codificación UTF-8, sin la codificación de marca de orden de bytes (BOM).
      - El tamaño máximo de un archivo JSON de conocimiento es 10 MB. Si tiene que importar un conocimiento mayor, considere la posibilidad de importar las intenciones y las entidades por separado después de haber importado el conocimiento. (También puede importar conocimientos grandes utilizando la API REST. Para obtener más información, consulte la [Referencia de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/apidocs/assistant?curl=#create-workspace){: new_window}).
      - El archivo JSON no puede contener tabuladores, saltos de página ni retornos de carro.

      Especifique los datos que desea incluir:

        - Seleccione **Todo (intenciones, entidades y diálogo)** si desea importar una copia completa del conocimiento exportado, incluido el diálogo.
        - Seleccione **Intenciones y entidades** si desea utilizar las intenciones y entidades del conocimiento exportado, pero tiene previsto crear un nuevo diálogo.

      Pulse **Importar**.

      Si tiene problemas para importar un conocimiento, consulte el apartado sobre [Resolución de problemas de importación de conocimientos](#skill-add-import-errors).

1.  Especifique los detalles del conocimiento:

    - **Nombre**: un nombre no puede contener más de 100 caracteres. El nombre es obligatorio.
    - **Descripción**: una descripción no puede contener más de 200 caracteres.
    - **Idioma**: el idioma de la entrada de usuario para el que se ha entrenado el conocimiento. El valor predeterminado es el inglés.

Después de crear el conocimiento, aparece como un mosaico en la página Conocimientos. Ahora puede empezar a identificar los objetivos de usuario que desea que el conocimiento de diálogo pueda abordar.

- Para añadir intenciones predefinidas al conocimiento, consulte [Utilización de catálogos de contenido](/docs/services/assistant?topic=assistant-catalog).
- Para definir sus propias intenciones, consulte [Definición de intenciones](/docs/services/assistant?topic=assistant-intents).

El conocimiento de diálogo no puede interactuar con los clientes hasta que se añada a un asistente y este se despliegue. Consulte [Creación de un asistente](/docs/services/assistant?topic=assistant-assistant-add).

## Resolución de problemas de importación de conocimientos
{: #skill-add-import-errors}

Si recibe un mensaje que indica que el conocimiento contiene artefactos que superan los límites impuestos por el plan de servicio, siga estos pasos para importar correctamente el conocimiento:

1.  Adquiera un plan con límites de artefactos más altos.
1.  Cree una instancia de servicio en el nuevo plan.
1.  Importe el conocimiento en la nueva instancia de servicio.
1.  Realice cambios en el conocimiento para que se ajuste a los requisitos de límite de artefactos para el plan que desee utilizar de ahora en adelante. Es posible que tenga que reducir el número de nodos de diálogo utilizados en el árbol de diálogo, por ejemplo:
1.  Exporte el conocimiento editado descargándolo.
1.  Intente de nuevo importar el conocimiento en la instancia de servicio original en el plan que desee.

## Adición del conocimiento a un asistente
{: #skill-add-to-assistant}

Puede añadir un conocimiento a un asistente. Debe abrir el mosaico del asistente y añadir el conocimiento al asistente desde la página de configuración del asistente; no puede elegir el asistente que utilizará el conocimiento desde dentro de la página de configuración del conocimiento. Un conocimiento de diálogo puede ser utilizado por más de un asistente.

1.  En el separador Asistente, pulse para abrir el mosaico correspondiente al asistente al que desea añadir el conocimiento.

1.  Pulse **Añadir conocimiento de diálogo**.

1.  Pulse **Añadir conocimiento existente**.

    Pulse el conocimiento que desea añadir entre los conocimientos que se muestran.

    Cuando se añade un conocimiento desde aquí, se obtiene la versión de desarrollo. Si desea añadir una versión de conocimiento específica, añádala desde el separador *Historial de versiones* del conocimiento.

## Límites de los conocimientos
{: #skill-add-limits}

El número de conocimientos que puede crear en una sola instancia de servicio depende del plan de {{site.data.keyword.conversationshort}}. Los conocimientos de diálogo de ejemplo que están disponibles en la instancia de servicio no se cuentan en el límite, a menos que los edite o los duplique.

| Plan de servicio     | Conocimientos por instancia de servicio |
|------------------|----------------------------:|
| Premium          |                          50 |
| Plus             |                          50 |
| Estándar         |                          20 |
| Lite*            |                           5 |
{: caption="Detalles del plan de servicio" caption-side="top"}

*Después de 30 días de inactividad, es posible que un conocimiento no utilizado de una instancia de servicio del Plan se suprima para liberar espacio.

## Descarga de un conocimiento de diálogo
{: #skill-add-download}

Puede descargar un conocimiento de diálogo en formato JSON. Es posible que desee descargar un conocimiento si desea utilizar el mismo conocimiento de diálogo en otra instancia distinta del servicio {{site.data.keyword.conversationshort}}, por ejemplo. Puede descargarlo desde una instancia e importarlo en otra instancia como un nuevo conocimiento de diálogo.

Para descargar un conocimiento de diálogo, siga los siguientes pasos:

1.  Localice el mosaico del conocimiento en la página Conocimientos o en la página de configuración de un asistente que utiliza el conocimiento.

1.  Pulse el icono ![abrir y cerrar lista de opciones](images/kabob-beta.png) y luego seleccione **Descargar JSON**.

1.  Especifique un nombre para el archivo JSON y dónde guardarlo y pulse **Guardar**.

También puede exportar un conocimiento mediante la API. Incluya el parámetro `export=true` con la solicitud. Revise la [Consulta de API](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace) para obtener más información.

## Supresión de un conocimiento
{: #skill-add-delete}

Puede suprimir cualquier conocimiento al que pueda acceder, a menos que esté siendo utilizado por un asistente. Si se está utilizando, debe eliminarlo del asistente que lo está utilizando para poder suprimirlo.

Asegúrese de comprobar con cualquier otra persona que pueda estar utilizando el conocimiento antes de suprimirlo.
{: tip}

Para suprimir un conocimiento, siga los siguientes pasos:

1.  Averigüe si algún asistente está utilizando el conocimiento. En el separador Conocimientos, busque el mosaico correspondiente al conocimiento que desea suprimir. El campo **Asistentes** muestra los asistentes que utilizan actualmente el conocimiento.

1.  Si el conocimiento que desea suprimir está asociada con un asistente, elimínelo del asistente mediante los pasos siguientes:

    - Consulte con el propietario del asistente que está utilizando el conocimiento antes de eliminar el conocimiento del mismo.
    - Abra el separador Asistentes y pulse para abrir el mosaico del asistente.
    - Busque el mosaico del conocimiento que desea suprimir. Pulse el icono ![abrir y cerrar lista de opciones](images/kabob-beta.png) y seleccione **Eliminar**.
    - Repita los pasos anteriores para cualquier otro asistente que utilice conocimiento.
    - Vuelva al separador Conocimientos y busque el mosaico correspondiente al conocimiento que desea suprimir.

1.  Pulse el icono ![abrir y cerrar lista de opciones](images/kabob-beta.png) y seleccione **Suprimir**. Confirme la supresión.

## Cómo compartir un conocimiento de diálogo con miembros del equipo
{: #skill-add-invite-others}

Después de crear la instancia de servicio, puede dar acceso a otras personas. Juntos, pueden definir los datos de entrenamiento y construir el diálogo.

**Importante**: Únicamente una persona puede editar una intención, una entidad o un nodo de diálogo al mismo tiempo. Si varias personas trabajan en el mismo elemento al mismo tiempo, entonces únicamente se aplican los cambios realizados por la última persona en guardarlos. Se pierden los cambios realizados en el mismo intervalo temporal por las personas que lo hicieron en primer lugar. Coordine las actualizaciones que tiene previsto realizar con los miembros de su equipo para evitar que nadie pierda su trabajo.

Para compartir un conocimiento de diálogo con otras personas, debe darles acceso a la instancia de servicio que aloja el conocimiento. Tenga en cuenta que la persona a la que invite podrá acceder a cualquier conocimiento alojado por esta instancia de servicio.

1.  Anote el nombre de la instancia actual, que se muestra en la parte superior de la página actual.
1.  Pulse el icono Usuario ![Usuario](images/user-icon2.png) en la cabecera de la página y seleccione **Gestionar usuarios** en el menú desplegable.

1.  Pulse **Invitar a usuarios** y, a continuación, especifique las direcciones de correo electrónico de las personas de su equipo a quienes desea otorgar acceso.

    Si ha dado a alguien acceso a una instancia de servicio cuando se gestionaba mediante Cloud Foundry, es posible que la persona ya aparezca en la lista como usuario invitado. Pulse el nombre de la persona para abrir los valores de gestión de acceso para el usuario. Pulse **Asignar acceso** y, a continuación, elija **Asignar acceso a los recursos**.

1.  En la sección *Servicios*, realice como mínimo las siguientes selecciones:

    - **Servicios**: {{site.data.keyword.conversationshort}}
    - **Asignar roles de acceso a la plataforma**: Operador

    Para obtener más información sobre los roles de gestión de la plataforma, consulte [Acceso IAM ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/iam?topic=iam-userroles). (De forma predeterminada, {{site.data.keyword.conversationshort}} no utiliza los roles de acceso al servicio).

    Para instancias antiguas gestionadas por Cloud Foundry, debe expandir la sección *Acceso de Cloud Foundry*, elegir su organización y asignar la persona al rol **Desarrollador** del espacio.
    {: note}

1.  Pulse **Invitar a usuarios**.

    Si está editando el acceso para un usuario existente, pulse **Asignar acceso**.

La próxima vez que inicien una sesión en {{site.data.keyword.cloud_notm}} las personas a quienes ha invitado, verán incluida su cuenta en su lista de cuentas. Si seleccionan su cuenta, podrán ver su instancia de servicio y abrir y editar sus conocimientos.

Cuantas más personas contribuyan al desarrollo del conocimiento de diálogo, más probable es que se produzcan cambios no intencionados, incluida la supresión del conocimiento. Considere la posibilidad de crear copias de seguridad de su conocimiento de diálogo de forma regular, de modo que pueda retrotraer a una versión anterior si fuese necesario. Para crear una copia de seguridad, simplemente [descargue el conocimiento como un archivo JSON](#skill-add-download).
