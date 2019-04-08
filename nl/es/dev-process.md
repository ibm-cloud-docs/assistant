---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# Proceso de desarrollo
{: #dev-process}

Utilice la herramienta {{site.data.keyword.conversationshort}} para aprovechar la IA al crear, desplegar y mejorar de forma incremental un asistente de conversación.
{: shortdesc}

![Muestra el flujo de los pasos de desarrollo, desde el desarrollo de datos de entrenamiento hasta el despliegue en producción](images/dev-process.png)

## Flujo de trabajo
{: #dev-process-workflow}

Un flujo de trabajo típico para un proyecto de asistente incluye los pasos siguientes:

1.  Defina un conjunto limitado de requisitos principales del cliente que desea que el asistente gestione en su nombre, incluidos procesos empresariales que pueda iniciar o completar para los clientes.
1.  Cree intenciones que representen las necesidades del cliente que ha identificado en el paso anterior. Por ejemplo, intenciones como `About_company` o `#Place_order`.

    Utilice la característica de recomendación de ejemplo de intención para explorar los registros existentes del centro de atención al cliente en busca de ejemplos óptimos de intenciones de usuario.
    {: tip}

1.  Cree un diálogo que detecte las intenciones definidas y ofrezca respuestas a las mismas, ya sea con respuestas sencillas o con un flujo de diálogo que recopile más información en primer lugar.
1.  Defina las entidades necesarias para comprender mejor el significado del usuario.

    Explore los ejemplos de intenciones de usuario existentes en busca de menciones de valores comunes de entidades. El uso de anotaciones para definir entidades captura no solo el texto del valor de la entidad, sino también el contexto en el que se suele utilizar el valor de la entidad en una frase.

    En el caso de entidades basadas en diccionario, utilice las recomendaciones de sinónimos para ampliar las definiciones de entidad.
    {: tip}

1.  Pruebe cada una de las funciones que añada al asistente en el panel "Pruébelo", de forma incremental, a medida que avance.
1.  Cuando tenga un asistente en funcionamiento que puede manejar correctamente las tareas principales, añada una integración que despliegue el asistente en un entorno de desarrollo. Pruebe el asistente desplegado y realice ajustes.

1.  Después de crear un asistente eficaz, tome una instantánea del conocimiento de diálogo y guárdelo como una versión.

    El hecho de guardar una versión cuando alcance un objetivo de desarrollo le proporciona un punto de referencia al que poder volver si los cambios posteriores que realice en el conocimiento reducen su efectividad. Consulte [Creación de versiones de conocimientos](/docs/services/assistant?topic=assistant-versions).
1.  Despliegue la versión del asistente en un entorno de prueba y pruébela.

    Si utiliza el widget de conversación alojado en la web, puede compartir el URL con otras personas para obtener su ayuda con las pruebas.
1.  Utilice las métricas del separador Análisis para encontrar áreas de mejora y realizar ajustes.

    Si tiene que probar enfoques alternativos para tratar un problema, cree una versión para cada solución, de modo que pueda desplegar y probar cada una de forma independiente y comparar los resultados.
1.  Cuando esté satisfecho con el rendimiento del asistente, despliegue la mejor versión del mismo en un entorno de producción.
1.  Supervise los registros de las conversaciones que mantienen los usuarios con el asistente desplegado.

    Puede ver los registros de una versión de un conocimiento que se ejecuta en producción en el separador Análisis de una versión de desarrollo del conocimiento. A medida que encuentra errores de clasificación u otros problemas, puede corregirlos en la versión de desarrollo del conocimiento y luego desplegar la versión mejorada en producción después de la prueba. Consulte [Mejora entre asistentes](/docs/services/assistant?topic=assistant-logs#logs-deploy-id) para obtener más información.

El proceso de analizar los registros y de mejorar el conocimiento de es continuo. Con el tiempo, es posible que desee ampliar las tareas que el asistente puede gestionar automáticamente. El cliente también tiene que cambiar. A medida que surgen nuevas necesidades, las métricas generadas por los asistentes desplegados pueden ayudarle a identificarlas y a solucionarlas en posteriores iteraciones.
