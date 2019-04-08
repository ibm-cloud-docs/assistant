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

# Acerca de
{: #index}

{{site.data.keyword.conversationfull}} es un bot cognitivo que puede personalizar para adaptarlo a sus necesidades empresariales y desplegar en varios canales para ofrecer ayuda a los clientes donde y cuando la necesiten.
{: shortdesc}

## Cómo funciona
{: #index-how-it-works}

En este diagrama se muestra la arquitectura general:

![Diagrama de flujo del servicio](images/arch-overview.png)

- Los usuarios interactúan con el asistente a través de uno o más de estos puntos de **integración**:

  - Un bot de conversación que publique directamente en una plataforma de mensajería de redes sociales existente, como Slack o Facebook Messenger.
  - Una interfaz de usuario sencilla de bot de conversación alojada en IBM Cloud.
  - Aplicación personalizada que desarrolle, como una app móvil o un robot con una interfaz de voz.

- El **asistente** recibe la entrada del usuario y la direcciona al conocimiento de diálogo.

- El **conocimiento** de diálogo interpreta la entrada del usuario, luego dirige el flujo de la conversación y recopila la información que necesita para responder o realizar una transacción en nombre del usuario.

## Implementación
{: #index-mplementation}

Así es como se implementa el asistente:

- **Crear un conocimiento de diálogo**. Utilice la herramienta gráfica intuitiva para definir los datos de entrenamiento y el diálogo para la conversación entre su asistente y sus clientes.

  Los datos de entrenamiento constan de los siguientes artefactos:

  - **Intenciones**: Objetivos que cree que tendrán los usuarios cuando interactúen con el servicio. Defina una intención para cada objetivo que se pueda identificar en la entrada de usuario. Por ejemplo, puede definir una intención denominada *store_hours* que responde a preguntas sobre el horario de almacén. Para cada intención puede añadir expresiones de ejemplo que reflejen lo que pueden preguntar los clientes para obtener la información que necesitan, como por ejemplo `What time do you open?` (¿A qué hora abren?).

    O bien puede utilizar los **catálogos de contenido** predefinidos que proporciona IBM para empezar a trabajar con datos pensados para responder ante objetivos comunes de los clientes.

  - **Diálogo**: Utilice la herramienta de diálogo para crear un flujo de diálogo que incorpore sus intenciones. El flujo de diálogo se representa gráficamente en la herramienta como un árbol. Puede añadir una rama para procesar cada una de las intenciones que desea que gestione el servicio.

  - **Entidades**: una entidad representa un término u objeto que proporciona contexto para una intención. Por ejemplo, una entidad podría ser un nombre de ciudad que ayude al diálogo a distinguir el diálogo la tienda sobre la que el usuario desea saber el horario de apertura. Después de añadir entidades, actualice el diálogo para utilizarlas. Utilice nodos de diálogo que gestionen muchas posibles permutaciones de una pregunta en función de otros factores de la entrada de usuario.

    A medida que añade datos de entrenamiento, se añade automáticamente un clasificador de lenguaje natural al conocimiento, y se va formando para comprender los tipos de preguntas que ha indicado que debe escuchar y a las que debe responder el servicio.

- **Cree un asistente**

- **Añada el conocimiento de diálogo a su asistente.**

- **Integre el asistente.** Cree una integración de canal para desplegar el asistente configurado directamente en un canal de mensajería o un canal de redes sociales.

  El asistente desplegado se aloja en {{site.data.keyword.cloud_notm}}, la plataforma de cálculo en la nube de IBM. (Consulte la [Visión general de la plataforma ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/docs/overview/ibm-cloud#overview) para obtener más información).

Obtenga más información sobre estas implementaciones en estos enlaces:

- [Visión general de la creación de intenciones](/docs/services/assistant?topic=assistant-intents#intents-described)
- [Visión general del diálogo](/docs/services/assistant?topic=assistant-dialog-overview)
- [Visión general de la creación de entidades](/docs/services/assistant?topic=assistant-entities#entities-described)
- [Visión general del asistente](/docs/services/assistant?topic=assistant-assistant-add)
- [Adición de integraciones](/docs/services/assistant?topic=assistant-deploy-integration-add)

## ¿Dónde están mis espacios de trabajo?
{: #index-existing-customers}

Si ha creado un *espacio de trabajo* en una versión anterior del servicio, no sufra; todavía puede acceder al mismo. Ahora los espacios de trabajo se denominan *conocimientos*. Para acceder a su espacio de trabajo, pulse el separador **Conocimientos**.

## Soporte de navegador
{: #index-browser-support}

La herramienta de servicio de {{site.data.keyword.conversationshort}} necesita el mismo nivel de software de navegador que el que necesita {{site.data.keyword.Bluemix_notm}}. Consulte {{site.data.keyword.Bluemix_notm}} [Requisitos previos ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/docs/overview/prereqs#browsers){: new_window} para obtener más información.

## Soporte de idiomas
{: #index-lang-support}

Encontrará información sobre el soporte de idiomas en el apartado [Idiomas soportados](/docs/services/assistant?topic=assistant-language-support).

## Términos y avisos
{: #index-notices}

Consulte [Términos y avisos de IBM Cloud ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](/docs/overview/terms-of-use?topic=overview-terms) para obtener más información sobre las condiciones de servicio.

## Siguientes pasos
{: #index-next-steps}

- [Empiece a trabajar](/docs/services/assistant?topic=assistant-getting-started) con el servicio.
- Consulta la lista de [recursos del desarrollador ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/watson/developer-resources/){: new_window}.

¿Le queda alguna duda? Póngase en contacto con el equipo de [ventas de IBM ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.
