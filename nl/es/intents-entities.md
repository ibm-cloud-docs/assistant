---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-09"

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

# Planificación de intenciones y entidades
{: #planning-your-entities}

Para planificar las *intenciones* para su aplicación, debe tener en cuenta lo que cree que desearán hacer los clientes y lo que desea que la aplicación sea capaz de manejar. Elegir la intención correcta para la entrada de un usuario es el primer paso para proporcionar una respuesta útil. Las intenciones que identifique para su aplicación determinarán los flujos de diálogo que tiene que crear; también pueden determinar los sistemas de fondo con los que se debe integrar la aplicación para dar respuesta a las solicitudes de los clientes (como por ejemplo bases de datos de clientes o sistemas de proceso de pagos).

Una *entidad* representa un término o un objeto en la entrada del usuario que proporciona aclaraciones o un contexto específico para una determinada intención. Si las intenciones representan verbos (algo un usuario quiere hacer), las entidades representan nombres (como el objeto o el contexto de una acción). Las entidades permiten que una sola intención represente varias acciones concretas. Una entidad define una clase de objetos, con valores específicos que representan posibles objetos de dicha clase.

Básicamente, si desea capturar una solicitud o llevar a cabo una acción, utilice una intención. Si desea capturar información que puede afectar a la forma de responder a una solicitud o acción, utilice una entidad.

Por ejemplo, supongamos que desea crear una aplicación para el panel de control cognitivo de un coche que permita a los usuarios activar o desactivar accesorios:

1.  Recopile tantas preguntas, mandatos u otras entradas reales clientes como le sea posible. El uso de entradas de usuarios reales ofrece una imagen mucho más precisa de la entrada que se puede esperar que utilizar listas de posibles expresiones creadas por expertos. Recuerde que los clientes pueden realizar el mismo tipo de solicitud de muchas maneras diferentes. Por ejemplo, todas estas expresiones representan una solicitud de desactivar algo:

    - `Headlights off (Apagar faros)`
    - `Turn the radio off (Apagar la radio)`
    - `Stop the air conditioner (Parar el aire acondicionado)`

1.  Cuando tenga una lista de ejemplos, clasifíquelos en categorías en función de la capacidad a la que la aplicación desea dar soporte; estas categorías representan las intenciones que definirá, mientras que los ejemplos ayudarán a la aplicación a identificar dichas intenciones en nuevas entradas.

    No cree intenciones demasiado parecidas. Las intenciones similares pueden resultar difíciles de distinguir para el servicio {{site.data.keyword.conversationshort}}. Si cree que tiene varias intenciones cuyo significado se parece, tenga en cuenta la posibilidad de combinarlas en una sola intención y luego utilizar entidades para ofrecer varias posibles respuestas para dicha intención.
    {: tip}

    Puesto que los ejemplos anteriores representan la misma intención (desactivar algo), puede llamar a esta categoría `#turn_off`. Recuerde que no es necesario que la entrada de un cliente coincida exactamente con ninguno de los ejemplos; las intenciones se reconocen mediante un proceso de lenguaje natural.
    {: tip}

1.  Luego debería utilizar una entidad que representara lo que el usuario desea desactivar. Para ello, puede crear una entidad denominada `@accessory` y asignarle los siguientes valores posibles:

    - `headlights (faros)`
    - `radio`
    - `air conditioner (aire acondicionado)`

    Para cada valor también puede especificar sinónimos. Por ejemplo, puede especificar `AC` y `cooling` (refrigeración) como sinónimos de `air conditioner`; estas tres series se tratarán como el mismo valor. Hacer una lista de las diversas formas que pueden utilizar los usuarios para referirse al mismo valor ayuda a mejorar la precisión de la aplicación.
1.  Cuando se recibe la entrada del usuario, la conversación reconoce tanto intenciones como entidades. Entonces el flujo del diálogo puede utilizarlas para ofrecer la mejor respuestas. Solo debe crear una entidad para algo importante en lo referente a cómo cambia la forma en que la aplicación responde frente a una intención.
    Además de las entidades que defina, el servicio proporciona un conjunto de entidades del sistema que ya están definidas y listas para que las utilice. Para obtener más información, consulte [Habilitación de entidades del sistema](entities.html#enable_system_entities).
    {: tip}

1.  Siga perfilando las intenciones, entidades y ejemplos según necesite. No piense en su conjunto de intenciones y entidades como productos acabados. Es probable que cuando [cree sus diálogos](dialog-build.html) identifique intenciones y entidades adicionales que deberá añadir. También puede seguir recopilando entradas de nuevos clientes y utilizarlas para añadir nuevos ejemplos; este proceso iterativo mejora la capacidad de su aplicación de reconocer intenciones y entidades con precisión.
