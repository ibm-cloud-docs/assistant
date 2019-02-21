---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-15"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Configuración de un espacio de trabajo de {{site.data.keyword.conversationshort}}
{: #skill-add}

El proceso de lenguaje natural para el servicio {{site.data.keyword.conversationshort}} ocurre dentro de un *espacio de trabajo*, que es un contenedor para todos los artefactos que definen el flujo de conversación para una aplicación.
{: shortdesc}

Una sola instancia del servicio {{site.data.keyword.conversationshort}} puede contener varios espacios de trabajo. Un espacio de trabajo contiene los siguientes tipos de artefactos:

- [**Intenciones**](intents.html): Una *intención* representa la finalidad de una entrada de usuario, como por ejemplo una pregunta sobre las ubicaciones de un negocio o el pago de una factura. El usuario define una intención para cada tipo de solicitud de usuario a la que desea que la aplicación dé soporte. En la herramienta, el nombre de una intención siempre tiene como prefijo el carácter `#`. Para preparar el espacio para que reconozca sus intenciones, debe especificar muchos ejemplos de entradas de usuario e indicar con qué intenciones se correlacionan.
- [**Entidades**](entities.html): Una *entidad* representa un objeto que sea relevante para las intenciones y que ofrezca un contexto específico para una intención. Por ejemplo, una entidad podría representar una ciudad donde el usuario desea buscar la ubicación de un negocio o el importe del pago de una factura. En la herramienta, el nombre de una entidad siempre tiene como prefijo el carácter `@`. Para preparar el espacio para que reconozca sus entidades, debe obtener una lista de los posibles valores para cada entidad y sinónimos que puedan especificar los usuarios.
- [**Diálogo**](dialog-build.html): Un *diálogo* es un flujo de conversación que define la forma en que la aplicación responde cuando reconoce intenciones y entidades definidas. Puede utilizar el creador de diálogos en la herramienta para crear conversaciones con los usuarios, proporcionando respuestas basadas en intenciones y entidades que se reconoce en su entrada.

A medida que añade entrenamiento, el espacio de trabajo se va formando a sí mismo, de modo que no tiene que hacer nada para iniciar el entrenamiento.

## Límites del espacio de trabajo
{: #workspace-limits}

El número de espacios de trabajo que puede crear en una sola instancia de servicio depende de su plan de servicio de {{site.data.keyword.conversationshort}}. El espacio de trabajo de ejemplo no cuenta en el límite de espacios de trabajo, a menos que edite o duplique el ejemplo:

| Plan de servicio     | Espacios de trabajo por instancia de servicio |
|------------------|--------------------------------:|
| Estándar/Premium |                              20 |
| Lite*            |                               5 |

*Después de 30 días de inactividad, un espacio de trabajo Lite no utilizado se puede suprimir para liberar espacio.

## Creación de espacios de trabajo
{: #creating-workspaces}

Puede crear un espacio de trabajo desde cero, utilizar el espacio de trabajo de ejemplo proporcionado o importar un espacio de trabajo desde un archivo JSON. También puede duplicar un espacio de trabajo existente dentro de la misma instancia de servicio.

Utilice la herramienta {{site.data.keyword.conversationshort}} para crear espacios de trabajo. Siga estos pasos para crear un espacio de trabajo:

1.  Si la página "Detalles de servicio" no está ya abierta, pulse la instancia de servicio de {{site.data.keyword.conversationshort}} en la consola. (Cuando se crea una instancia de servicio, se muestra la página "Detalles de servicio".)

1.  Pulse **Iniciar herramienta**.

1.  Realice una de las acciones siguientes:
    - Para crear un espacio de trabajo desde cero, pulse **Crear**.
    - Para utilizar el ejemplo como punto de partida para su propio espacio de trabajo, edite el espacio de trabajo de ejemplo. Si desea utilizarlo para varios espacios de trabajo, duplíquelo.
    - Para importar un espacio de trabajo desde un archivo JSON, pulse el icono ![Importar espacio de trabajo](images/workspace_import.png) y seleccione el archivo JSON del que lo desea importar.

        **Importante:**

        - El archivo JSON importado debe utilizar la codificación UTF-8.
        - El tamaño máximo de un archivo JSON de espacio de trabajo es 10 MB. Si tiene que importar un espacio más grande, considere la posibilidad de importar las intenciones y las entidades por separado después de haber importado el espacio de trabajo. (También puede importar espacios de trabajo grandes utilizando la API REST. Para obtener más información, consulte la [Referencia de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#create_workspace){: new_window}).
        - El archivo JSON no puede contener tabuladores, saltos de página ni retornos de carro.

        Especifique los datos que desea incluir:

        - Seleccione **Todo (intenciones, entidades y diálogo)** si desea importar una copia completa del espacio de trabajo exportado, incluido el diálogo.
        - Seleccione **Intenciones y entidades** si desea utilizar las intenciones y entidades del espacio de trabajo exportado, pero tiene previsto crear un nuevo diálogo.

1.  Especifique los detalles del nuevo espacio de trabajo:
    - **Nombre**: Un nombre no puede contener más de 64 caracteres. Este valor es obligatorio.
    - **Descripción**: Una descripción no puede contener más de 128 caracteres.
    - **Lenguaje**: El lenguaje de la entrada de usuario para el que se ha formado el espacio de trabajo.

Después de crear el espacio de trabajo, aparece como un mosaico en la página Espacios de trabajo.

## Exportación y copia de espacios de trabajo
{: #exporting-and-copying-workspaces}

Puede utilizar la página Espacios de trabajo para exportar espacios de trabajo y copiar un espacio de trabajo en un nuevo espacio de trabajo.

Cuando se exporta un espacio de trabajo, se guarda una copia de todos los datos del espacio de trabajo en un archivo JSON. Utilice esta opción si desea importar el espacio de trabajo en otra instancia de servicio o guardar una copia de los datos del espacio de trabajo fuera de línea. Cuando importa un espacio de trabajo, puede optar por importar sólo las intenciones y entidades, lo que puede resultar útil si desea crear un nuevo diálogo utilizando los mismos datos de entrenamiento.

Cuando se copia un espacio de trabajo, se hace una copia completa del espacio de trabajo dentro de la misma instancia de servicio. Utilice esta opción si desea adaptar un espacio de trabajo existente conservando la versión original.

- Para exportar un espacio de trabajo existente a un archivo JSON, pulse el icono de menú en el mosaico del espacio de trabajo y luego seleccione **Descargar como JSON**. 
- Para crear una copia de un espacio de trabajo existente dentro de la misma instancia de servicio, pulse el icono de menú en el mosaico del espacio de trabajo y seleccione **Duplicar**. 

    Especifique el nombre, la descripción y el lenguaje para el nuevo espacio de trabajo. Todos los datos (incluidos intenciones, entidades y diálogo) se incluyen en la copia.

## Cómo compartir el espacio de trabajo con miembros del equipo
{: #invite-others}

Después de crear la instancia de servicio, puede dar acceso a otras personas. Juntos, pueden definir los datos de entrenamiento y construir el diálogo. 

**Importante**: Únicamente una persona puede editar una intención, una entidad o un nodo de diálogo al mismo tiempo. Si varias personas trabajan en el mismo elemento al mismo tiempo, entonces únicamente se aplican los cambios realizados por la última persona en guardarlos. Se pierden los cambios realizados en el mismo intervalo temporal por las personas que lo hicieron en primer lugar. Coordine las actualizaciones que tiene previsto realizar con los miembros de su equipo para evitar que nadie pierda su trabajo. 

Para compartir un espacio con otras personas, debe darles acceso de desarrollador a la instancia de servicio que aloja el espacio de trabajo. Si la instancia que comparte aloja otros espacios de trabajo que no desea que otras personas editen, asegúrese de no invitarlas. 

1.  Vaya a la página [Proyectos ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.{DomainName}/developer/watson/projects) de {{site.data.keyword.watson}} Developer Console e inicie una sesión. Pulse **Gestionar > Cuenta > Usuarios** en el menú. 
1.  Pulse **Invitar a usuarios** y, a continuación, especifique las direcciones de correo electrónico de las personas de su equipo a quienes desea otorgar acceso. 
1.  En la sección de *Acceso de Cloud Foundry*, elija su organización de la lista de *Organización*. 

   El campo *Roles de organización* se cumplimenta de forma automática con *Auditor*. Puede mantener el valor predeterminado en el campo.
1.  Opcionalmente, limite el acceso que está otorgando a los espacios de trabajo a una única región y espacio. 
1.  En el campo *Roles de espacio*, elija **Desarrollador**. 

    Consulte [Roles de Cloud Foundry ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/docs/iam/cfaccess.html#cfroles) para obtener más información sobre los roles.
1.  Pulse **Invitar a usuarios**.

La próxima vez que inicien una sesión en {{site.data.keyword.cloud_notm}} las personas a quienes ha invitado, verán incluida su organización en su lista de cuentas. 

Cuantas más personas contribuyan al desarrollo del espacio de trabajo, más probable es que se produzcan cambios no intencionados, incluida las supresiones del espacio de trabajo. Considere crear copias de seguridad de su espacio de trabajo de forma regular, de modo que pueda retrotraer a una versión anterior si fuese necesario. Para crear una copia, simplemente exporte el espacio de trabajo como un archivo JSON. 
