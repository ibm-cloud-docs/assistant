---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-01"

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

# Integración con Slack
{: #deploy-slack}

Slack es una aplicación de mensajería basada en la nube que ayuda a las personas a colaborar entre sí.
{: shortdesc}

Después de configurar un conocimiento de diálogo y de añadirlo a un asistente, puede integrar el asistente con Slack.

1.  En el separador Asistentes, pulse para abrir el mosaico del asistente que desea desplegar.

1.  En la sección Integraciones, pulse **Añadir integración**.

1.  Pulse el botón **Seleccionar integración** correspondiente a *Slack*.

1.  Siga las instrucciones que se proporcionan en la pantalla para completar el proceso de integración.

Si desea ver los pasos que sigue otro usuario para realizar el despliegue, vea este vídeo.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Proceso paso a paso del despliegue de Slack" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/RBGBPJ8h4HQ?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Duración: 9,5 minutos

## Consideraciones sobre el diálogo
{: #deploy-slack-dialog}

Las respuestas completas que añade a un diálogo se visualizan en un canal de Slack según lo esperado, con las siguientes excepciones:

- **Conectar con un agente humano**: este tipo de respuesta se pasa por alto.

- **Imagen**: se necesita un título de imagen. Si no especifica un título, no se muestra la imagen.

- **Opción**: este tipo de respuesta muestra una lista de opciones entre las que el usuario puede elegir.

  - El título es **obligatorio** y se muestra antes de la lista de opciones.
  - No se muestra una descripción, tanto si la especifica como si no.
  - Después de que un usuario pulse uno de los botones, las opciones de botón desaparecen y se sustituyen por la entrada de usuario que genera la opción del usuario. Si incluye varios tipos de respuesta en una sola respuesta, coloque el tipo de respuesta de la opción en último lugar. De lo contrario, la salida contendrá una combinación de respuestas y entradas de usuario que podrían confundir al usuario.

- **Pausa**: este tipo de respuesta hace una pausa en la actividad del canal de Slack. Sin embargo, no se muestra ningún indicador visible para indicar que el asistente está en pausa, tanto si elige mostrar la escritura como si no.

Consulte [Respuestas completas](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia) para obtener más información sobre los tipos de respuesta.

## Conversación con el asistente
{: #deploy-slack-try}

Para iniciar una conversación con el asistente, siga los pasos siguientes:

1.  Abra Slack y vaya al espacio de trabajo asociado a la app.
1.  Pulse la aplicación que ha creado en la sección Apps.
1.  Chatee con el asistente.

La integración de Slack no procesa el nodo de bienvenida de su diálogo. El mensaje de bienvenida no se muestra en el canal de slack, al contrario que en el panel "Pruébelo" dentro de la herramienta o en la página web de integración de enlace de vista previa. No se activa desde aquí porque los nodos con la condición especial `welcome` se omiten en los flujos de diálogo iniciados por usuarios. Slack espera a que el usuario inicie la conversación. Si tiene que definir valores predeterminados para las variables de contexto al principio de la conversación, no las defina en el nodo de bienvenida. Consulte [Inicio del diálogo](/docs/services/assistant?topic=assistant-dialog-start) para obtener más información.
{: note}

El flujo de diálogo correspondiente a la sesión actual se reinicia después de 60 minutos de inactividad (5 minutos para los planes Lite y Estándar). Esto significa que si un usuario deja de interactuar con el asistente, después de 60 (o de 5) minutos, cualquier valor de variable de contexto que se haya definido durante la conversación anterior se establece en nulo o en sus valores predeterminados.
