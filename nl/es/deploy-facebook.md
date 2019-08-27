---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-28"

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

# Integración con Facebook Messenger
{: #deploy-facebook}

Facebook Messenger es una aplicación de mensajería móvil que ayuda a empresas y a clientes a comunicarse directamente entre sí.
{: shortdesc}

Después de configurar un conocimiento de diálogo y de añadirlo a un asistente, puede integrar el asistente con Facebook Messenger.

Actualmente no hay ningún mecanismo para identificar a los usuarios que interactúan con el asistente a través de Facebook Messenger, lo que significa que no hay forma de identificar o suprimir los datos asociados con un usuario específico. No utilice este método de integración para despliegues que deban cumplir con GDPR. Consulte [Seguridad de la información](/docs/services/assistant?topic=assistant-information-security) para obtener más información.
{: important}

1.  En el separador Asistentes, pulse para abrir el mosaico del asistente que desea desplegar.

1.  En la sección Integraciones, pulse **Añadir integración**.

1.  Pulse **Facebook Messenger**.

1.  Siga las instrucciones que se proporcionan en la pantalla para completar el proceso de integración.

Si desea ver los pasos que sigue otro usuario para realizar el despliegue, vea este vídeo.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Proceso paso a paso del despliegue de Facebook " type="text/html" width="640" height="390" src="https://www.youtube.com/embed/8o-FFU5sYNM?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Duración: 8 minutos

## Consideraciones sobre el diálogo
{: #deploy-facebook-dialog}

Las respuestas completas que añade a un diálogo se visualizan en una app de Facebook según lo esperado, con las siguientes excepciones:

- **Conectar con un agente humano**: este tipo de respuesta se pasa por alto.

- **Imagen**: este tipo de respuesta incorpora una imagen en la respuesta. No se muestran un título y ni una descripción antes de la imagen, tanto si las especifica como si no.

- **Opción**: este tipo de respuesta muestra una lista de opciones entre las que el usuario puede elegir.

  - El título es **obligatorio** y se muestra antes de la lista de opciones.
  - No se muestra una descripción, tanto si la especifica como si no.
  - Después de que un usuario pulse uno de los botones, las opciones de botón desaparecen y se sustituyen por la entrada de usuario que genera la opción del usuario. Si el asistente o el usuario especifica una entrada nueva, desaparece la entrada generada por el botón. Por lo tanto, si incluye varios tipos de respuesta en una sola respuesta, coloque el tipo de respuesta de la opción en último lugar. De lo contrario, el contenido de las respuestas posteriores, como el texto de un tipo de respuesta de texto, sustituirá el texto generado por el botón.

- **Pausa**: este tipo de respuesta hace una pausa en la actividad del asistente en Messenger. Sin embargo, la actividad no se reanuda después de la pausa a no ser que se active otro tipo de respuestas tras la misma. Siempre que incluya este tipo de respuesta, añada otro tipo de respuesta distinto, como por ejemplo una respuesta de texto, y colóquelo después de este.

Consulte [Respuestas completas](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia) para obtener más información sobre los tipos de respuesta.

## Conversación con el asistente
{: #deploy-facebook-try}

Para iniciar una conversación con el asistente, siga los pasos siguientes:

1.  Abra Facebook Messenger.
1.  Escriba el nombre de la página que ha creado anteriormente.
1.  Cuando aparezca la página, pulse en la misma y empiece a chatear con el asistente.

La integración de Facebook Messenger no procesa el nodo de bienvenida de su diálogo. El mensaje de bienvenida no se muestra en la conversación de Facebook, al contrario que en el panel "Pruébelo" o en la página web de integración de enlace de vista previa. No se activa desde aquí porque los nodos con la condición especial `welcome` se omiten en los flujos de diálogo iniciados por usuarios. Facebook Messenger espera a que el usuario inicie la conversación. Si tiene que definir valores predeterminados para las variables de contexto al principio de la conversación, no las defina en el nodo de bienvenida. Consulte [Inicio del diálogo](/docs/services/assistant?topic=assistant-dialog-start) para obtener más información.
{: note}

El flujo de diálogo correspondiente a la sesión actual se reinicia después de 60 minutos de inactividad (5 minutos para los planes Lite y Estándar). Esto significa que si un usuario deja de interactuar con el asistente, después de 60 (o de 5) minutos, cualquier valor de variable de contexto que se haya definido durante la conversación anterior se establece en nulo o en sus valores predeterminados.
