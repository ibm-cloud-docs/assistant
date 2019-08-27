---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-29"

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

# Cambio del valor de tiempo de espera de inactividad ![Solo en el plan Plus o Premium](images/plus.png)
{: #assistant-settings}

Cuando un usuario interactúa con su asistente a través de una de las integraciones incorporadas, la sesión de conversación finaliza después de un periodo de tiempo específico en el que el usuario no interactúa con el asistente. Puede especificar el tiempo que se debe esperar antes de que se restablezca una sesión de conversación cambiando el valor de tiempo de espera de inactividad.
{: shortdesc}

Cuando se restablece una sesión de conversación, el diálogo pierde cualquier información de contexto que guardó durante el intercambio anterior con el usuario. Por ejemplo, si el diálogo solicita el nombre del usuario y, a continuación, llama al usuario por ese nombre a lo largo del resto de la conversación, después de que termine la sesión de chat y comienza una nueva, el diálogo empezará preguntando otra vez por el nombre del usuario.

## Límites de sesión
{: #assistant-settings-session-limits}

La longitud permitida para un tiempo de espera de inactividad difiere según el tipo de plan de instancia de servicio. En la tabla siguiente se listan los límites.

| Plan de servicio | Período de inactividad predeterminado de sesión de conversación | Período máximo de inactividad de sesión de conversación |
|--------------|--------------------------------:|----------------------------:|
| Premium      |                      60 minutos |                    24 horas |
| Plus         |                      60 minutos |                    24 horas |
| Estándar     |                       5 minutos |                   5 minutos |
| Plus Trial   |                       5 minutos |                   5 minutos |
| Lite*        |                       5 minutos |                   5 minutos |
{: caption="Detalles del plan de servicio" caption-side="top"}

## Cambio del valor
{: #assistant-settings-timeout-task}

1.  Pulse el menú correspondiente a su asistente y, a continuación, seleccione **Valores**.

1.  Cambie el tiempo especificado en el campo **Límite de tiempo de espera excedido**.

1.  Cierre la página de valores. El cambio se guarda automáticamente.
