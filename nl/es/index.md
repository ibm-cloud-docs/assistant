---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-26"

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

# Acerca de

Con el servicio {{site.data.keyword.conversationfull}}, puede crear una solución que comprenda la entrada en lenguaje natural y utilice el aprendizaje de máquina para responder a los clientes de modo que simule una conversación entre humanos.
{: shortdesc}

## Cómo funciona 

En este diagrama se muestra la arquitectura general de una solución completa:![Diagrama de flujo del servicio](images/conversation_arch_overview.png)

- Los usuarios interactúan con la aplicación mediante la **interfaz** de usuario que implemente. Por ejemplo, una simple ventana de conversación o una app móvil, o incluso un robot con una interfaz de voz.

- La **aplicación** envía la entrada de usuario al servicio {{site.data.keyword.conversationshort}}.
    - La aplicación se conecta a un *espacio de trabajo*, que es un contenedor para el flujo de diálogo y los datos de entrenamiento.
    - El servicio interpreta la entrada de usuario, dirige el flujo de la conversación y obtiene la información que necesita.
    - Puede conectar servicios {{site.data.keyword.watson}} adicionales para analizar la entrada de usuario, como por ejemplo {{site.data.keyword.toneanalyzershort}} o {{site.data.keyword.speechtotextshort}}.

- La aplicación puede interactuar con los **sistemas de fondo** en función de la intención del usuario y de la información adicional. Por ejemplo, puede responder una pregunta, abrir incidencias, actualizar información de cuentas o realizar pedidos. No hay límite en lo referente a lo que puede hacer.

## Implementación

Así es como se implementa la conversación:

- **Configure un espacio de trabajo.** Con el entorno gráfico de fácil utilización, configure los datos de entrenamiento y diálogo correspondiente a la conversación.

    Los datos de entrenamiento constan de los siguientes artefactos:
    - **Intenciones**: Objetivos que cree que tendrán los usuarios cuando interactúen con el servicio. Defina una intención para cada objetivo que se pueda identificar en la entrada de usuario. Por ejemplo, puede definir una intención denominada *store_hours* que responde a preguntas sobre el horario de almacén. Para cada intención puede añadir expresiones de ejemplo que reflejen lo que pueden preguntar los clientes para obtener la información que necesitan, como por ejemplo `What time do you open?` (¿A qué hora abren?).
    - **Entidades**: una entidad representa un término u objeto que proporciona contexto para una intención. Por ejemplo, una entidad podría ser un nombre de ciudad que ayude al diálogo a distinguir el diálogo la tienda sobre la que el usuario desea saber el horario de apertura.

      A medida que añade datos de entrenamiento, se añade automáticamente un clasificador de lenguaje natural al espacio de trabajo, y se va formando para comprender los tipos de preguntas que ha indicado que debe escuchar y a las que debe responder el servicio.

    Utilice la herramienta de diálogo para crear un flujo de diálogo que incorpore sus intenciones y entidades. El flujo de diálogo se representa gráficamente en la herramienta como un árbol. Puede añadir una rama para procesar cada una de las intenciones que desea que gestione el servicio. Luego puede añadir nodos de rama que gestionen muchas posibles permutaciones de una pregunta en función de otros factores, como las entidades que se encuentran en la entrada de usuario o información que se pasa al servicio desde la aplicación o desde otro servicio externo.

- **Despliegue su espacio de trabajo.** Despliegue el espacio de trabajo configurado a los usuarios conectándolo a una interfaz de usuario frontal, a redes sociales o un canal de mensajería. La instancia desplegada en el servicio {{site.data.keyword.conversationshort}} la aloja {{site.data.keyword.cloud_notm}}, la plataforma de computación en la nube de IBM. 
(Consulte la [Visión general de la plataforma ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/docs/overview/ibm-cloud.html#overview) para obtener más información). 

Obtenga más información sobre estas implementaciones en estos enlaces: 

- [Planificación de intenciones y entidades](intents-entities.html#planning-your-entities)
- [Visión general del diálogo](dialog-overview.html)
- [Visión general del despliegue](deploy.html)

## Soporte de navegador

Las herramientas de servicio de {{site.data.keyword.conversationshort}} precisan del mismo nivel de software de navegador que el que {{site.data.keyword.Bluemix_notm}} necesita. Consulte {{site.data.keyword.Bluemix_notm}} [Requisitos previos ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://console.bluemix.net/docs/overview/prereqs.html#browsers){: new_window} para obtener más información. 

## Soporte de idiomas

Encontrará información sobre el soporte de idiomas en el apartado [Idiomas soportados](lang-support.html).

## Siguientes pasos

- [Comience a utilizar](getting-started.html) el servicio
- Pruebe algunas [demos](sample-applications.html).
- Consulta la lista de [SDK ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/watson/developercloud/developer-tools.html){: new_window}.

¿Le queda alguna duda? Póngase en contacto con el equipo de [ventas de IBM ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}. 
