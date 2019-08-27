---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-06"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
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

Utilice {{site.data.keyword.conversationfull}} para crear un asistente con su propia marca en cualquier dispositivo, aplicación o canal. Su asistente se conecta a los recursos de relación del cliente que ya utiliza para ofrecer una experiencia de relación y una resolución de problemas unificada y atractiva a sus clientes.
{: shortdesc}

## Cómo funciona
{: #index-how-it-works}

En este diagrama se muestra cómo funciona el producto:

![Diagrama de flujo del servicio](images/simple-overview.png)

- Los usuarios interactúan con el asistente a través de uno o más de estos puntos de **integración**:

  - Un asistente virtual que publique directamente en una plataforma de mensajería de redes sociales existente, como Slack o Facebook Messenger.
  - Una aplicación personalizada que desarrolle, como una app móvil o un robot con una interfaz de voz.

- El **asistente** recibe la entrada del usuario y la direcciona al conocimiento de diálogo.

- El **conocimiento de diálogo** interpreta aún más la entrada del usuario y, a continuación, dirige el flujo de la conversación. El diálogo recopila la información que necesita para responder o realizar una transacción en nombre del usuario.

- Las preguntas que el cuadro de diálogo no puede responder se envían a la **buscar conocimiento**, que encuentra las respuestas relevantes buscando en las bases de conocimiento de la empresa que configure a tal efecto.

## Implementación
{: #index-implementation}

Este diagrama muestra la implementación con más detalles:

![Diagrama de flujo del servicio](images/arch-overview-search.png)

Así es como se implementa el asistente:

- **Cree un asistente**

- **Añada un conocimiento a su asistente**.

  En función de su plan de servicios, puede añadir los siguientes tipos de conocimientos:

  - **Añada un conocimiento de diálogo**.  
  
    Utilice el producto gráfico intuitivo para definir los datos de entrenamiento y el diálogo para la conversación entre su asistente y sus clientes. Los datos de entrenamiento constan de los siguientes artefactos:

    - **Intenciones**: Objetivos que cree que tienen los usuarios cuando interactúan con su asistente. Defina una intención para cada objetivo que se pueda identificar en la entrada de usuario. Por ejemplo, puede definir una intención denominada *store_hours* que responde a preguntas sobre el horario de almacén. Para cada intención puede añadir expresiones de ejemplo que reflejen lo que pueden preguntar los clientes para obtener la información que necesitan, como por ejemplo `What time do you open?` (¿A qué hora abren?).

      O bien puede utilizar los **catálogos de contenido** predefinidos que proporciona IBM para empezar a trabajar con datos pensados para responder ante objetivos comunes de los clientes.

    - **Diálogo**: Utilice el editor de diálogos para crear un flujo de diálogo que incorpore sus intenciones. El flujo de diálogo se representa gráficamente como un árbol. Puede añadir una rama para procesar cada una de las intenciones que desea que gestione su asistente.

    - **Entidades**: una entidad representa un término u objeto que proporciona contexto para una intención. Por ejemplo, una entidad podría ser un nombre de ciudad que ayude al diálogo a distinguir el diálogo la tienda sobre la que el usuario desea saber el horario de apertura. Después de añadir entidades, actualice el diálogo para utilizarlas. Utilice nodos de diálogo que gestionen muchas posibles permutaciones de una pregunta en función de otros factores presentes en la entrada del usuario.

    A medida que se añaden datos de entrenamiento, se añade automáticamente un clasificador de lenguaje natural al conocimiento. El modelo de clasificador se entrena para que entienda los tipos de solicitudes que quiere que su asistente atienda y responda.

  - **Añada un conocimiento de búsqueda**. ![Solo en el plan Plus o Premium](images/plus.png)

    Aproveche las recopilaciones de datos que cree en {{site.data.keyword.discoveryfull}} para proporcionar respuestas a las preguntas de los clientes. Cuando un cliente hace una pregunta que el diálogo no está diseñado para responder, el asistente puede buscar información relevante de los orígenes de datos configurados, extraer la información y devolverla como respuesta del asistente.

- **Integre el asistente.** Añada una integración de canal incorporado para desplegar el asistente configurado directamente en un canal de mensajería o un canal de redes sociales. O bien cree su propia aplicación cliente como la interfaz de usuario para el asistente.

  El asistente desplegado se aloja en {{site.data.keyword.cloud}}, la plataforma de cálculo en la nube de IBM. (Para obtener más información, consulte [Descripción general de la plataforma](/docs/overview/ibm-cloud#overview){: external}).

Obtenga más información sobre estas implementaciones en estos enlaces:

- [Visión general del asistente](/docs/services/assistant?topic=assistant-assistants)
- [Descripción general de la búsqueda de conocimiento](/docs/services/assistant?topic=assistant-skill-add-search)
- [Visión general de la creación de intenciones](/docs/services/assistant?topic=assistant-intents#intents-described)
- [Visión general del diálogo](/docs/services/assistant?topic=assistant-dialog-overview)
- [Visión general de la creación de entidades](/docs/services/assistant?topic=assistant-entities#entities-described)
- [Adición de integraciones](/docs/services/assistant?topic=assistant-deploy-integration-add)

## Soporte de navegador
{: #index-browser-support}

{{site.data.keyword.conversationshort}} necesita el mismo nivel de software de navegador que el que necesita {{site.data.keyword.Bluemix_notm}}. Para obtener más información, consulte {{site.data.keyword.Bluemix_notm}} [Requisitos previos](/docs/overview?topic=overview-prereqs-platform#browsers-platform){: external}.

## Soporte de idiomas
{: #index-lang-support}

Encontrará información sobre el soporte de idiomas en el apartado [Idiomas soportados](/docs/services/assistant?topic=assistant-language-support).

## Términos y avisos
{: #index-notices}

Para obtener información sobre los términos del servicio, consulte [Términos y avisos de IBM Cloud](/docs/overview/terms-of-use?topic=overview-terms){: external}.

Hay soporte para la Ley de Portabilidad y Responsabilidad de Seguro Sanitario de los Estados Unidos (Health Insurance Portability and Accountability Act, HIPAA) para los planes Premium alojados en la ubicación de Washington, DC creada el 1 de abril de 2019 o después de éste. Para obtener más información, consulte [Habilitación de valores admitidos en la UE e HIPAA](/docs/account?topic=account-eu-hipaa-supported#eu-hipaa-supported){: external}.

## Siguientes pasos
{: #index-next-steps}

- [Empiece a trabajar](/docs/services/assistant?topic=assistant-getting-started) con el producto.
- Ver una lista de los [recursos de desarrollador)](https://www.ibm.com/watson/developer-resources/){: external}.

¿Tiene preguntas? Póngase en contacto con el equipo de [Ventas de IBM](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: external}.
