---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-26"

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

Para obtener más información sobre lo que es un asistente en primer lugar, consulte
[Asistentes](/docs/services/assistant?topic=assistant-assistants).

Siga estos pasos para crear un asistente:

1.  Pulse **Crear asistente**.

1.  Añada detalles sobre el nuevo asistente:

    - **Nombre**: un nombre no puede contener más de 100 caracteres. El nombre es obligatorio.
    - **Descripción**: una descripción no puede contener más de 200 caracteres.

    Se crea automáticamente una página web de IBM para que el usuario y su equipo puedan probar su asistente. Si no desea que se cree la página web de vista previa, anule la selección del recuadro de selección **Habilitar enlace de vista previa**.

1.  Pulse **Crear asistente**.

1.  Añada un conocimiento al asistente eligiendo uno de los siguientes tipos de conocimientos a añadir.

    **Nota**: Puede optar por añadir un conocimiento existente o crear uno nuevo.

    - **Añadir Conocimiento de diálogo**: Utiliza el procesamiento de lenguaje natural de Watson y las tecnologías de aprendizaje automático para comprender las preguntas y las solicitudes de los usuarios, y responder a ellas con las respuestas que usted haya creado.

      Cuando se añade un conocimiento de diálogo desde aquí, se obtiene la versión de desarrollo. Si quiere añadir una versión de conocimiento de diálogo específica, añádala mejor desde la página *Versiones* del conocimiento.

    - **Añadir conocimiento de búsqueda** ![Solo en el plan Plus o Premium](images/plus.png): para una consulta de usuario determinada, utiliza el servicio {{site.data.keyword.discoveryfull}} para recuperar información de un origen de datos que identifique y comparte cualquier información relevante que encuentre como respuesta al usuario.

      Esta opción sólo es visible si se trata de un usuario del plan Plus o Premium.
      {: note}

    Consulte [Creación de un conocimiento](/docs/services/assistant?topic=assistant-skill-add).

## Límites del asistente
{: #assistant-add-limits}

El número de asistentes que puede crear depende del tipo de plan de {{site.data.keyword.conversationshort}}.

| Plan | Asistentes por instancia de servicio |
|--------------|--------------------------------:|
| Premium      |                             100 |
| Plus         |                             100 |
| Estándar     |                             100 |
| Plus Trial   |                               5 |
| Lite*        |                             100 |
{: caption="Detalles del plan" caption-side="top"}

*Después de 30 días de inactividad, es posible que un asistente no utilizado de una instancia de servicio del Plan se suprima para liberar espacio.

Consulte [Cambio del valor de tiempo de espera de inactividad](/docs/services/assistant?topic=assistant-assistant-settings) para obtener más información sobre el asunto.

Puede conectar un conocimiento de cada tipo a su asistente. El número de conocimientos que puede crear difiere en función del plan que tenga. Consulte [Límites de conocimientos](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits) para obtener más información.

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

Puede añadir un conocimiento de cada tipo de conocimiento a un asistente. Si desea cambiar un conocimiento que utiliza su asistente, puede intercambiar un conocimiento por otro.

1.  En el separador Asistentes, abra el asistente.

1.  Pulse el icono ![abrir y cerrar lista de opciones](images/kabob-beta.png) en el conocimiento que quiere intercambiar y seleccione **Cambiar conocimiento**.

    Para cambiar el conocimiento de diálogo actual por otra versión del conocimiento, elija **Cambiar versión de conocimiento**.

1.  Elija un conocimiento existente para utilizarlo en lugar del otro o bien [cree un conocimiento](/docs/services/assistant?topic=assistant-skill-add).

### Cambio entre instancias de servicio
{: #assistant-add-switch-instance}

Si tiene más de una instancia de servicio, puede comprobar la cabecera de la página para averiguar la instancia que está utilizando actualmente. Si está trabajando en un conocimiento, pulse primero el enlace del rastro de navegación **Conocimientos**. El mensaje de cabecera muestra el nombre de la instancia actual. Para cambiar a otra instancia de servicio, pulse **Cambiar** y elija la instancia que desee.
