---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-02"

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

Personalice su asistente añadiéndole los conocimientos que necesita para satisfacer los objetivos de sus clientes.
{: shortdesc}

Puede crear el siguiente tipo de conocimiento:

- **Conocimiento de diálogo**: Utiliza el procesamiento de lenguaje natural de Watson y las tecnologías de aprendizaje automático para comprender las preguntas y las solicitudes de los usuarios, y responder a ellas con las respuestas que usted haya creado.

- **Conocimiento de búsqueda** ![Solo en el plan Plus o Premium](images/plus.png): para una consulta de usuario determinada, utiliza el servicio {{site.data.keyword.discoveryfull}} para buscar un origen de datos de su contenido de autoservicio y devolver una respuesta.

  Sólo los usuarios de los planes Plus o Premium pueden crear este tipo de conocimientos.
  
  Por lo general, primero se crea un conocimiento de cada tipo. A continuación, a medida que se crea un diálogo para el conocimiento de diálogo, se decide cuándo iniciar el conocimiento de búsqueda. Para algunas preguntas o solicitudes, es suficiente una respuesta codificada o derivada mediante programación (que se define en el conocimiento de diálogo). Para otros, es posible que desee proporcionar una respuesta más robusta devolviendo un pasaje completo de la información relacionada (que se extrae de un origen de datos externo utilizando el conocimiento de búsqueda).

## Crear el conocimiento
{: #skill-add-task}

Puede añadir un conocimiento de cada tipo de conocimiento a un asistente.

Para crear un conocimiento, siga este paso:

1.  Pulse en el separador **Diálogo** y luego en **Crear conocimiento**.

1.  Elija el tipo de conocimiento que desea crear y, a continuación, pulse **Siguiente**.

    Siga los pasos restantes en el procedimiento adecuado para completar el proceso de creación de conocimiento.

      - [Conocimiento de diálogo](/docs/services/assistant?topic=assistant-skill-dialog-add)
      - [Conocimiento de búsqueda](/docs/services/assistant?topic=assistant-skill-search-add)

## Límites de los conocimientos
{: #skill-add-limits}

El número de conocimientos que puede crear depende del tipo de plan de {{site.data.keyword.conversationshort}}. Los conocimientos de diálogo de ejemplo que están disponibles para que los utilice no se cuentan en el límite, a menos que los utilice. Una versión de conocimiento no cuenta como un conocimiento.

| Plan     | Conocimientos por instancia de servicio |
|------------------|----------------------------:|
| Premium          |                          50 |
| Plus             |                          50 |
| Estándar         |                          20 |
| Lite`*`, Plus Trial |                        5 |
{: caption="Detalles del plan" caption-side="top"}

`*` Después de 30 días de inactividad, es posible que un conocimiento no utilizado de una instancia de servicio del Plan se suprima para liberar espacio.

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
