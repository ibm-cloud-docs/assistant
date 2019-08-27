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

# Asistentes
{: #assistants}

Un asistente es un bot cognitivo que puede personalizar para adaptarlo a sus necesidades empresariales y desplegar en varios canales para ofrecer ayuda a los clientes donde y cuando la necesiten.
{: shortdesc}

![Conocimientos](images/skill-icon.png) Un asistente direcciona las consultas de sus clientes a un conocimiento, lo que proporciona la respuesta adecuada. Los conocimientos de diálogo devuelven respuestas que son creadas por el usuario para responder a preguntas comunes, mientras que los conocimientos de búsqueda buscan y devuelven pasajes del contenido de autoservicio existente para responder a consultas más complejas.

## Conocimiento de diálogo
{: #assistants-dialog-skill}

Un conocimiento de diálogo que pueda comprender y responder preguntas o solicitudes con las que sus clientes suelen necesitar ayuda. Proporcione información sobre los temas o tareas sobre los que los usuarios realizan preguntas y sobre cómo preguntan sobre las mismas y el producto crea de forma dinámica un modelo de aprendizaje automático que se adapta para comprender las mismas solicitudes de usuario y otras similares.

| Árbol de diálogo | Interfaz gráfica de usuario |
|-------------|-------------------------:|
| Puede utilizar el editor de diálogos gráficos para crear un script de tipos para que lo lea su asistente cuando interactúa con sus clientes. El resultado es un diálogo que simula una conversación real. El diálogo detecta los objetivos comunes de los clientes que le ha enseñado y proporciona respuestas útiles. | ![Un árbol de diálogo de muestra con contenido de ejemplo](images/dialog-depiction.png) |

El propio conocimiento del diálogo se define en el texto, pero puede integrarlo con los servicios Watson Speech to Text y Watson Text to Speech, que permiten a los usuarios interactuar con su asistente verbalmente.

![Datos de entrenamiento listos para ser utilizados](images/oob.png)  Si desea comenzar a trabajar rápidamente, añada datos de entrenamiento preconfigurados al conocimiento de diálogo para que el asistente pueda empezar a ayudar a los clientes en lo básico.

## Conocimiento de búsqueda ![solo el plan Plus o Premium](images/plus.png)
{: #assistants-search-skill}

El conocimiento de búsqueda está disponible solo para usuarios de Plus o Premium.
{: note}

Un conocimiento de búsqueda aprovecha la información de las bases de conocimiento corporativo existentes u otras colecciones de contenido creadas por expertos de la materia para hacer frente a las indagaciones de clientes no anticipadas o más matizadas.

![IBM Cloud](images/cloud.png)  El asistente es un bot alojado completamente que está gestionado por {{site.data.keyword.Bluemix_notm}}, lo que significa que no tiene que preocuparse por configurar o mantener la infraestructura para darle soporte.

| Integraciones       | Canales  |
|--------------------|:----------|
| Puede desplegar el asistente a través de varias interfaces, incluidos canales de mensajería existentes como, por ejemplo, Slack y Facebook Messenger, en solo unos pasos. O bien, si desea diseñar una aplicación personalizada que lo incorpore, puede realizar llamadas directas a las API subyacentes para hacerlo. | ![Métodos de integración que incluyen Slack, Facebook Messenger, una aplicación web o integración de agente humano](images/integrations.png) |

Consulte [Creación de asistentes](/docs/services/assistant?topic=assistant-assistant-add) para empezar.
