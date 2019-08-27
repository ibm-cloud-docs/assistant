---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-06"

keywords: import workspace, import JSON, export JSON

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

# Creación de un conocimiento de diálogo
{: #skill-dialog-add}

El proceso de lenguaje natural para el servicio {{site.data.keyword.conversationshort}} se define en un *conocimiento de diálogo*,
que es un contenedor para todos los artefactos que definen un flujo de conversación.
{: shortdesc}

Puede añadir un conocimiento de diálogo a un asistente. Consulte [Límites de conocimientos](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits) para obtener información sobre los límites por plan.

## Crear un conocimiento de diálogo
{: #skill-dialog-add-task}

Puede crear un conocimiento desde cero, utilizar un conocimiento de ejemplo proporcionado por IBM o importar un conocimiento desde un archivo JSON.

Para añadir un conocimiento, siga los siguientes pasos:

1.  Pulse en el separador **Diálogo** y luego en **Crear conocimiento**.

1.  Pulse el icono *Conocimiento de diálogo* y luego en **Siguiente**.

1.  Realice una de las acciones siguientes:

    - Para crear un conocimiento desde cero, pulse **Crear conocimiento**.
    - Para añadir un conocimiento de ejemplo que se proporciona con el producto como punto de partida para su propio conocimiento o como ejemplo que explorar antes de crear uno, pulse **Utilizar conocimiento de ejemplo** y luego pulse el ejemplo que desea utilizar.

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

      Si tiene problemas para importar un conocimiento, consulte el apartado sobre [Resolución de problemas de importación de conocimientos](#skill-dialog-add-import-errors).

1.  Especifique los detalles del conocimiento:

    - **Nombre**: un nombre no puede contener más de 100 caracteres. El nombre es obligatorio.
    - **Descripción**: una descripción no puede contener más de 200 caracteres.
    - **Idioma**: el idioma de la entrada de usuario para el que se ha entrenado el conocimiento. El valor predeterminado es el inglés.

Después de crear el conocimiento de diálogo, aparece como un mosaico en la página Conocimientos. Ahora puede empezar a identificar los objetivos de usuario que desea que el conocimiento de diálogo pueda abordar.

- Para añadir intenciones predefinidas al conocimiento, consulte [Utilización de catálogos de contenido](/docs/services/assistant?topic=assistant-catalog).
- Para definir sus propias intenciones, consulte [Definición de intenciones](/docs/services/assistant?topic=assistant-intents).

El conocimiento de diálogo no puede interactuar con los clientes hasta que se añada a un asistente y este se despliegue. Consulte [Creación de un asistente](/docs/services/assistant?topic=assistant-assistant-add).

### Resolución de problemas de importación de conocimientos
{: #skill-dialog-add-import-errors}

Si recibe un mensaje que indica que el conocimiento contiene artefactos que superan los límites impuestos por el plan de servicio, siga estos pasos para importar correctamente el conocimiento:

1.  Adquiera un plan con límites de artefactos más altos.
1.  Cree una instancia de servicio en el nuevo plan.
1.  Importe el conocimiento en la nueva instancia de servicio.
1.  Realice cambios en el conocimiento para que se ajuste a los requisitos de límite de artefactos para el plan que desee utilizar de ahora en adelante. Es posible que tenga que reducir el número de nodos de diálogo utilizados en el árbol de diálogo, por ejemplo:
1.  Exporte el conocimiento editado descargándolo.
1.  Intente de nuevo importar el conocimiento en la instancia de servicio original en el plan que desee.

### Adición del conocimiento a un asistente
{: #skill-dialog-add-to-assistant}

Puede añadir un conocimiento de diálogo a un asistente. Debe abrir el mosaico del asistente y añadir el conocimiento al asistente desde la página de configuración del asistente; no puede elegir el asistente que utilizará el conocimiento desde dentro de la página de configuración del conocimiento. Un conocimiento de diálogo puede ser utilizado por más de un asistente.

1.  En el separador Asistente, pulse para abrir el mosaico correspondiente al asistente al que desea añadir el conocimiento.

1.  Pulse **Añadir conocimiento de diálogo**.

1.  Pulse **Añadir conocimiento existente**.

    Pulse el conocimiento que desea añadir entre los conocimientos que se muestran.

Cuando se añade un conocimiento de diálogo desde aquí, se obtiene la versión de desarrollo. Si quiere añadir una versión de conocimiento específica, añádala mejor desde el separador *Versiones* del conocimiento.

## Descarga de un conocimiento de diálogo
{: #skill-dialog-add-download}

Puede descargar un conocimiento de diálogo en formato JSON. Es posible que desee descargar un conocimiento si desea utilizar el mismo conocimiento de diálogo en otra instancia distinta del servicio {{site.data.keyword.conversationshort}}, por ejemplo. Puede descargarlo desde una instancia e importarlo en otra instancia como un nuevo conocimiento de diálogo.

Para descargar un conocimiento de diálogo, siga los siguientes pasos:

1.  Localice el mosaico del conocimiento en la página Conocimientos o en la página de configuración de un asistente que utiliza el conocimiento.

1.  Pulse el icono ![abrir y cerrar lista de opciones](images/kabob-beta.png) y luego seleccione **Descargar JSON**.

1.  Especifique un nombre para el archivo JSON y dónde guardarlo y pulse **Guardar**.

También puede exportar un conocimiento mediante la API. Incluya el parámetro `export=true` con la solicitud. Revise la [Consulta de API](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace) para obtener más información.

## Cómo compartir un conocimiento de diálogo con miembros del equipo
{: #skill-dialog-add-invite-others}

Después de crear la instancia de servicio, puede dar acceso a otras personas. Juntos, pueden definir los datos de entrenamiento y construir el diálogo.

Únicamente una persona puede editar una intención, una entidad o un nodo de diálogo al mismo tiempo. Si varias personas trabajan en el mismo elemento al mismo tiempo, entonces únicamente se aplican los cambios realizados por la última persona en guardarlos. Se pierden los cambios realizados en el mismo intervalo temporal por las personas que lo hicieron en primer lugar. Coordine las actualizaciones que tiene previsto realizar con los miembros de su equipo para evitar que nadie pierda su trabajo.
{: important}

Para compartir un conocimiento de diálogo con otras personas, debe darles acceso a la instancia de servicio que aloja el conocimiento. Tenga en cuenta que la persona a la que invite podrá acceder a cualquier conocimiento alojado por esta instancia de servicio.

1.  Anote el nombre de la instancia actual, que se muestra en la parte superior de la página actual.
1.  Pulse el icono Usuario ![Usuario](images/user-icon2.png) en la cabecera de la página y seleccione **Gestionar usuarios** en el menú desplegable.

1.  En el panel de navegación, pulse **Usuarios**.

    Si ha dado a alguien acceso a una instancia de servicio con anterioridad, es posible que la persona ya aparezca en la lista como usuario invitado. Para cambiar el nivel de acceso de la persona a la instancia, pulse el menú situado junto a su nombre, seleccione **Asignar acceso** y, a continuación, pulse **Asignar acceso a los recursos**.
    
1.  Pulse **Invitar a usuarios**. 

1.  En la sección *Servicios*, seleccione el {{site.data.keyword.conversationshort}} servicio.

1.  Seleccione al menos una región y, como mínimo, una instancia de servicio para compartir con este usuario.

1.  Añada a este usuario las asignaciones siguientes como mínimo:
 
    - **Roles de acceso de plataforma**: Operador
    - **Roles de acceso de servicio**: Escritor

    Para obtener más información sobre los roles, consulte [Acceso de IAM ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/docs/iam?topic=iam-userroles).

    Para instancias antiguas gestionadas por Cloud Foundry, debe expandir la sección *Acceso de Cloud Foundry*, elegir su organización y asignar la persona al rol **Desarrollador** del espacio.
    {: note}

1.  Pulse **Invitar a usuarios**.

    Si está editando el acceso para un usuario existente, pulse **Asignar acceso**.

La próxima vez que inicien una sesión en {{site.data.keyword.cloud}} las personas a quienes ha invitado, verán incluida su cuenta en su lista de cuentas. Si seleccionan su cuenta, podrán ver su instancia de servicio y abrir y editar sus conocimientos.

Cuantas más personas contribuyan al desarrollo del conocimiento de diálogo, más probable es que se produzcan cambios no intencionados, incluida la supresión del conocimiento. Considere la posibilidad de crear copias de seguridad de su conocimiento de diálogo de forma regular, de modo que pueda retrotraer a una versión anterior si fuese necesario. Para crear una copia de seguridad, simplemente [descargue el conocimiento como un archivo JSON](#skill-dialog-add-download).
