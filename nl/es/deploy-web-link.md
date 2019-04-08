---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-11"

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

# Integración con un widget de conversación alojado en la web
{: #deploy-web-link}

Si no inhabilita el enlace de vista previa, el asistente estará disponible inmediatamente para realizar pruebas desde una página web.
{: shortdesc}

El asistente se implementa automáticamente como un widget de conversación integrado en una página web sencilla de IBM. Puede probar el conocimiento de diálogo que ha añadido al asistente especificando el texto en el widget de conversación. También puede compartir el URL de la página con otras personas para obtener ayuda en las pruebas y para recibir comentarios sobre el asistente.

A diferencia de cuando se prueba utilizando el panel "Pruébelo" de la herramienta, cualquier llamada de API resultante de las interacciones con el asistente alojado por el URL de enlace de vista previa no incurre en ningún cargo.

## Utilización de la integración de enlace de vista previa para probar el asistente
{: #deploy-web-link-try}

Para probar el asistente desde un widget de conversación alojado en la web, siga estos pasos:

1.  En el separador Asistentes, pulse para abrir el mosaico del asistente que desea probar.

1.  En la sección *Integraciones*, pulse el icono **Enlace de vista previa**.

    Si no ha habilitado el enlace de vista previa cuando ha creado el asistente, pulse **Añadir integración**, pulse el mosaico de integración **Enlace de vista previa** y luego pulse **Crear**.

1.  **Opcional**: cambie el nombre y la descripción de la página web de la vista previa.

1.  Pulse el enlace de URL que se muestra para abrir la página de prueba.

    Se abre otro separador del navegador web que contiene una implementación del widget de conversación del asistente.

    Si la instancia de servicio se ha creado en el centro de datos de Londres antes del 13 de diciembre de 2018 o en Sídney antes del 7 de mayo de 2018, debe editar el URL del enlace de vista previa. El URL incluye un código de ubicación correspondiente a la ubicación en la que está alojada la instancia. Como las instancias de Londres y Sídney estaban sindicadas con Dallas antes de las fechas mencionadas, debe sustituir la referencia `eu-gb` o `au-syd` del URL por `us-south` para que la página web de vista previa se muestre correctamente.
    {: important}

1.  Envíe las expresiones de prueba para ver cómo responde el asistente.

    No se devuelve ninguna respuesta hasta después de crear un conocimiento de diálogo y de añadirlo al asistente.

    El flujo de diálogo correspondiente a la sesión actual se reinicia después de 60 minutos de inactividad (5 minutos para los planes Lite y Estándar). Esto significa que si un usuario deja de interactuar con el asistente, después de 60 (o de 5) minutos, cualquier valor de variable de contexto que se haya definido durante la conversación anterior se establece en nulo o en sus valores predeterminados.

1.  Después de la prueba, puede cerrar el separador del navegador para salir de la página web pública.

1.  En la herramienta, pulse **Guardar cambios** para guardar las ediciones que haya realizado en la integración del enlace de vista previa y cerrar la página o bien pulse **X** para cerrar la página sin guardar.

## Consideraciones sobre el diálogo
{: #deploy-web-link-dialog}

Las respuestas completas que a un diálogo se muestran en un widget de conversación basado en la web según lo esperado, con las siguientes excepciones:

- **Conectar con un agente humano**: este tipo de respuesta se pasa por alto.

- **Pausa**: este tipo de respuesta hace una pausa en la actividad del widget de conversación. Sin embargo, la actividad no se reanuda después de la pausa hasta que se active otra respuesta. Si incluye un tipo de respuesta `pause`, añada otro tipo de respuesta distinto, como por ejemplo `text`, tras el mismo.

Consulte [Respuestas completas](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia) para obtener más información sobre los tipos de respuesta.

## Adición de una integración de enlace de vista previa
{: #deploy-web-link-add-more}

Si ha suprimido accidentalmente la integración de enlace de vista previa que se crea automáticamente o si desea crear otra página web pública, puede añadir una integración de enlace de vista previa.

1.  En el separador Asistente, pulse para abrir el mosaico correspondiente al asistente que desea desplegar.

1.  Vaya a la sección **Integraciones** y luego pulse **Añadir integración**.

1.  Pulse el icono de integración **Enlace de vista previa**.

1.  Si lo desea, puede editar el nombre y la descripción de integración del enlace de vista previa y luego pulsar **Crear**.

    Se añade un URL a la página. Pulse sobre el mismo para abrir la página web de vista previa.
