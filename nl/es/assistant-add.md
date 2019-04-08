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

# Creación de un asistente
{: #assistant-add}

Cree un asistente con los conocimientos que necesita para dar respuesta a los objetivos empresariales de sus clientes.
{: shortdesc}

Siga estos pasos para crear un asistente:

1.  Pulse el separador **Asistentes**.

1.  Realice una de las acciones siguientes:

    - Para crear un asistente de ejemplo que puede revisar y del que puede aprender, pulse **Añadir un ejemplo** y luego elija el asistente de ejemplo que desea crear.

      Se añade el asistente de ejemplo. Puede saltarse los pasos restantes de este procedimiento.

      Se proporciona un conocimiento de ejemplo con el asistente de ejemplo y se añade a la lista de conocimientos. Si ya ha creado un conocimiento de ejemplo del mismo tipo, el conocimiento existente se asocia automáticamente con este asistente de ejemplo nuevo.
      {: note}

    - Para crear un asistente desde cero, pulse **Crear nuevo** y, a continuación, siga los pasos restantes de este procedimiento.

1.  Especifique los detalles del nuevo asistente:
    - **Nombre**: un nombre no puede contener más de 100 caracteres. El nombre es obligatorio.
    - **Descripción**: una descripción no puede contener más de 200 caracteres.

    Se crea automáticamente una página web de IBM para que el usuario y su equipo puedan probar su asistente. Si no desea que se cree la página web de vista previa, anule la selección del recuadro de selección **Habilitar enlace de vista previa**.

1.  Pulse **Crear**.

1.  Añada un conocimiento al asistente pulsando **Añadir conocimiento**. Puede optar por añadir un conocimiento existente o crear uno nuevo.

    Cuando se añade un conocimiento desde aquí, se obtiene la versión de desarrollo. Si desea añadir una versión de conocimiento específica, añádala desde el separador *Historial de versiones* del conocimiento.

    Si ha creado o si ha recibido acceso con el rol de desarrollador a los espacios de trabajo creados con la versión disponible a nivel general del servicio {{site.data.keyword.conversationshort}} (antes llamado Watson Conversation), los verá en la lista como conocimientos de diálogo.
    {: note}

    Consulte [Creación de un conocimiento](/docs/services/assistant?topic=assistant-skill-add) para obtener más información sobre cómo crear un conocimiento.

## Límites del asistente
{: #assistant-add-limits}

El número de asistentes que puede crear en una sola instancia de servicio depende de su plan de {{site.data.keyword.conversationshort}}.

| Plan de servicio | Asistentes por instancia de servicio | Integraciones por asistente  | Periodo de inactividad de sesión de conversación |
|--------------|--------------------------------:|----------------------------:|-----------------:|
| Premium      |                             100 |                         100 |       60 minutos |
| Plus         |                             100 |                         100 |       60 minutos |
| Estándar     |                             100 |                         100 |        5 minutos |
| Lite*        |                             100 |                         100 |        5 minutos |
{: caption="Detalles del plan de servicio" caption-side="top"}

*Después de 30 días de inactividad, es posible que un asistente no utilizado de una instancia de servicio del Plan se suprima para liberar espacio.

Puede conectar un conocimiento a su asistente. El número de conocimientos que puede crear difiere en función del plan que tenga. Consulte [Límites de conocimientos](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits) para obtener más información.

## Supresión de un asistente
{: #assistant-add-delete}

Cuando se suprime un asistente, las integraciones que ha definido para el asistente también se suprimen automáticamente. Los conocimientos que ha añadido al asistente no se suprimen.

Para suprimir un asistente, siga estos pasos:

1.  En el separador Asistentes, busque el asistente que desea suprimir.

1.  Pulse el icono ![abrir y cerrar lista de opciones](images/kabob-beta.png) y seleccione **Suprimir**. Confirme la supresión.

## Cambio del nombre de un asistente
{: #assistant-add-rename}

Después de crear el asistente, puede cambiar el nombre de un asistente y su descripción asociada.

Para cambiar el nombre de un asistente, siga estos pasos:

1.  En el separador Asistentes, busque el asistente cuyo nombre desea cambiar.

1.  Pulse el icono ![abrir y cerrar lista de opciones](images/kabob-beta.png) y seleccione **Renombrar**.

1.  Edite el nombre y luego pulse **Renombrar** para guardar los cambios.

### Cambio del conocimiento asociada con el asistente
{: #assistant-add-swap-skill}

Puede añadir un conocimiento a un asistente. Si desea cambiar el conocimiento que utiliza su asistente, puede intercambiar un conocimiento por otro.

1.  En el separador Asistente, pulse para abrir el mosaico correspondiente al asistente para el que desea cambiar el conocimiento.

1.  Pulse el icono ![abrir y cerrar lista de opciones](images/kabob-beta.png) y seleccione **Cambiar conocimiento**. Para cambiar el conocimiento actual por otra versión del conocimiento, elija **Cambiar versión de conocimiento**.

1.  Elija un conocimiento existente para utilizarlo en lugar del otro o bien [cree un conocimiento](/docs/services/assistant?topic=assistant-skill-add).

### Cambio entre instancias de servicio
{: #assistant-add-switch-instance}

Si tiene más de una instancia de servicio, puede comprobar la cabecera de la página para averiguar la instancia que está utilizando actualmente. Si está trabajando en un conocimiento, pulse primero el enlace del rastro de navegación **Conocimientos**. El mensaje de cabecera muestra el nombre de la instancia actual. Para cambiar a otra instancia de servicio, pulse **cambiar** y elija la instancia que desee.
