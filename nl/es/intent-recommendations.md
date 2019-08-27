---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

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

# Obtener ayuda para definir intenciones ![solo el plan Plus o Premium](images/plus.png)
{: #intent-recommendations}

Si tiene datos de transcripciones de conversaciones soporte de cliente de empresa existentes, deje que Watson analice los datos para comprender las necesidades del cliente a las que el equipo de soporte dedica la mayor parte de su tiempo. A continuación, Watson puede recomendar intenciones y ejemplos de usuario que puede utilizar para entrenar a su asistente para que pueda reconocer las mismas solicitudes (y similares) en el futuro.
{: shortdesc}

Esta característica está disponible para los usuarios de los planes Plus y Premium.
{: note}

Las necesidades del cliente se representan en {{site.data.keyword.conversationshort}} como *intenciones*. Si todavía no ha definido intenciones, puede empezar a hacerlo más rápidamente solicitando ayuda a Watson. Cargue archivos con expresiones de cliente de las transcripciones del centro de atención al cliente para que el servicio {{site.data.keyword.conversationshort}} las analice. Basándose en la información que descubra, Watson recomienda un conjunto base de intenciones que debe crear para cubrir las necesidades más comunes de sus clientes.

A medida que cambien los temas sobre los que hablan sus clientes, puede utilizar la característica de recomendaciones de ejemplo de usuario de intención como ayuda para mantener las intenciones actualizadas y relevantes a lo largo del tiempo.

En el siguiente vídeo se proporciona una visión general de 3,5 minutos sobre recomendaciones de intención y ejemplos de usuarios de intenciones.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Descripción general de recomendaciones de intención" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/64h59KqDY98?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Para obtener más información sobre la forma en la que las recomendaciones de intención pueden ayudar al bot a ser más rápido, [lea esta publicación de blog ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://medium.com/ibm-watson/let-watson-train-your-virtual-assistant-57bd53b12bc3){: new_window}.

Examine los datos existentes para realizar una de las tareas siguientes:

- [Obtención de recomendaciones de intenciones](#intent-recommendations-get-intent-recommendations)
- [Obtención de recomendaciones de ejemplo de intenciones de usuarios](#intent-recommendations-get-example-recommendations)

Consulte [Idiomas admitidos](/docs/services/assistant?topic=assistant-language-support) para obtener información sobre el soporte de idiomas para esta característica.

## Creación de un archivo de origen de ejemplo de usuario
{: #intent-recommendations-log-files-add}

Para que Watson pueda analizar los datos, antes debe proporcionar los datos en el formato correcto. Cree un archivo de valores separados por coma (CSV) con una expresión de cliente por línea. Lo ideal es que las expresiones sean frases cortas extraídas de transcripciones del centro de atención al cliente que contengan preguntas y solicitudes reales de clientes. Cada archivo de origen de ejemplo de usuario puede tener un máximo de 20 MB.

Siga las siguientes directrices adicionales:

  - Elimine los datos confidenciales de las expresiones que incluya en el archivo.

    Los datos confidenciales incluyen cualquier información relacionada con una persona física identificable, como nombres, direcciones de correo electrónico e ID de cliente, y datos regulados, como información protegida sobre salud.
  - No incluya expresiones que superen los 1.024 caracteres de longitud. Las expresiones más largas se truncan.
  - El archivo debe contener al menos 100 expresiones; un archivo con 500 o más expresiones le dará mejores resultados.
  - Si una expresión contiene una coma, especifique la expresión entre comillas.
  - El archivo CSV debe incluir solo una columna.
  - Elimine todo lo que no sea una expresión generada por el cliente, incluidas las respuestas o notas de agentes humanos.

  Por ejemplo:

  ```
  What happens to my coverage if I trade in my car?
  i'd like to buy a house.
  How do I add a dependent to my plan?
  "first, i want to know if i am already registered."
  ```

Los archivos que cargue se comparten entre todos los conocimientos de la instancia de servicio actual. Se examinan las expresiones de todos los archivos disponibles cuando solicita recomendaciones de intenciones y recomendaciones de ejemplo de intenciones de usuarios.

## Obtención de recomendaciones de intenciones de Watson
{: #intent-recommendations-get-intent-recommendations}

Deje que Watson analice las transcripciones de conversaciones del centro de atención al cliente y recomiende algunas intenciones iniciales con las que pueda empezar a trabajar. Si ya ha creado algunas intenciones, deje que Watson analice sus registros y compare sus hallazgos con las intenciones existentes para identificar lagunas en los datos de entrenamiento y sugerir nuevas intenciones para cubrirlas.

Para utilizar esta característica, cargue un archivo con expresiones de cliente. Watson evalúa estas expresiones e identifica áreas de problemas comunes que los clientes mencionan con frecuencia. {{site.data.keyword.conversationshort}} muestra un conjunto de intenciones concretas que capturan las necesidades comunes de los clientes. Puede revisar cada intención recomendada y sus ejemplos de usuario correspondientes para elegir los que desea añadir a los datos de entrenamiento.

## Obtención de recomendaciones de intenciones
{: #intent-recommendations-get-intent-recommendations-task}

Antes de empezar, cree un archivo CSV con los datos. Consulte [Creación de un archivo de origen de ejemplo de usuario](#intent-recommendations-log-files-add).

Para obtener recomendaciones de intenciones, siga los pasos siguientes:

1.  Abra el conocimiento de diálogo. El conocimiento se abre en la página **Intenciones**.

1.  Pulse **Obtener recomendaciones**.

1.  **Solo la primera vez**: pulse **Añadir archivo** y luego pulse **Elegir un archivo** para examinar el archivo CSV que ha creado anteriormente y selecciónelo.

    Dé tiempo a Watson para analizar sus datos y agrupar las expresiones.

1.  Revise las intenciones recomendadas por Watson.

    Las recomendaciones de intento se ordenan por:

    1.  Intenciones con las expresiones más frecuentes. La frecuencia de las expresiones asociadas con estas intenciones sugiere que identifican las preocupaciones más comunes de los clientes.
    1.  Diferencia entre las intenciones y aquellas que usted ya ha añadido a su conocimiento. Su exclusividad indica que pueden atender necesidades del cliente que aún no se estén atendiendo.
    1.  Similitud entre los ejemplos de usuario en cuanto a intención, lo que indica la fortaleza de la misma.

    Puede pasar el puntero del ratón sobre una intención para ver algunos ejemplos de sus expresiones asociadas. Si desea ver una lista de todas las agrupaciones de intenciones, pulse **Mostrar todas las recomendaciones**.

1.  Pulse una intención para ver el conjunto completo de ejemplos de usuario asociados a la misma y lleve a cabo una de las acciones siguientes:

    Deseleccione las expresiones que no desee añadir como ejemplos de usuario.

    - Para añadir la intención recomendada con las expresiones seleccionadas como ejemplos de usuario, pulse **Crear intención**.
    - Para añadir las expresiones seleccionadas de la intención recomendada a una de las intenciones existentes como ejemplos de usuario, pulse **Añadir a intención existente**, elija la intención y luego pulse **Añadir**.

Las intenciones y los ejemplos de usuario que añada de esta forma no cuentan en los totales de intenciones y de ejemplos de usuario de intención, para los que existen límites según el plan. Consulte [Límites de intenciones](/docs/services/assistant?topic=assistant-intents#intents-limits) para obtener más información.

A medida que cambien los temas sobre los que hablan sus clientes, puede utilizar la característica de recomendaciones de ejemplo de usuario de intención como ayuda para mantener las intenciones actualizadas y relevantes a lo largo del tiempo.

## Obtención de recomendaciones de ejemplo de intenciones de usuarios
{: #intent-recommendations-get-example-recommendations}

Los datos que proporcione a Watson para que busque ejemplos de usuario de intención no necesitan asociar los ejemplos con las intenciones. Simplemente proporcione las expresiones de cliente y deje que Watson realice el trabajo de elegir las que resulten apropiadas para la intención actual. Para cada intención, Watson analiza los mismos datos para encontrar ejemplos de usuario que resulten apropiados para dicha intención.

Antes de empezar, cree un archivo CSV con los datos. Consulte [Creación de un archivo de origen de ejemplo de usuario](#intent-recommendations-log-files-add).

1.  Siga los pasos que se indican en [Creación de intenciones](/docs/services/assistant?topic=assistant-intents#intents-creating-intents-task) para crear una intención.

1.  Añada al menos 5 ejemplos de usuario que ilustren el rango completo de expresiones típicas que prevé que los clientes puedan utilizar para activar esta intención.

    Estos ejemplos de expresiones de usuarios enseñan a Watson los tipos de expresiones que debe buscar en los archivos que cargue.

1.  Pulse **Mostrar recomendaciones**.

    Los archivos de origen de ejemplos de usuario se comparten entre los conocimientos de una instancia de servicio. Si sus compañeros de trabajo poseen conocimientos en la misma instancia y cargan archivos, sus archivos se añaden a la colección de *Archivos de origen de ejemplos de usuario* compartidos.

1.  **Solo la primera vez**: pulse **Cargar archivos** y luego pulse **Elegir un archivo** para examinar el archivo CSV que ha creado anteriormente y selecciónelo.

    No hay problema si el archivo que ha cargado contiene expresiones correspondientes a todos los tipos de intenciones. Watson sabe en qué intención está trabajando y localiza ejemplos adecuados para realizar recomendaciones sobre esta intención específica.

    Después de cargar el archivo y de que Watson lo procese, se muestran las expresiones recomendadas. Si no se realiza ninguna recomendación, significa que el archivo no contiene ejemplos que resulten adecuados para esta intención.

1.  Si el archivo no puede proporcionar recomendaciones útiles para ninguna de sus intenciones, puede probar otro conjunto de expresiones cargando otro archivo. Consulte [Gestión de archivos cargados](#intent-recommendations-manage-files) para obtener más información.

1.  Después de que Watson muestre recomendaciones, seleccione las expresiones que desea añadir como ejemplos de usuario para esta intención y luego pulse **Añadir**. O bien pulse **Siguiente conjunto** para examinar más expresiones.
1.  Si desea buscar usted mismo ejemplos de usuario en el contenido del archivo CSV, pulse el separador **Buscar en registros**, especifique una palabra clave en la que basar la búsqueda y luego pulse **Intro**.

    Siga estas directrices sobre la sintaxis de la consulta de búsqueda:

    - Se da soporte a operadores booleanos (por ejemplo, `AND` y `OR`).
    - Añada texto entre comillas para buscar una coincidencia de texto exacta ("thisstringmustbepresente").
    - Puede utilizar expresiones regulares, como por ejemplo `*ly` para encontrar todos los términos que terminan por `ly`.
    - Los siguientes caracteres se utilizan como operadores de expresiones regulares:

      `+ - = && || > < ! ( ) { } [ ] ^ " ~ * ? : \ /`

      Si desea incluir uno en un término de búsqueda sin que se procese como operador, debe añadirle como prefijo una barra inclinada invertida (`\`).

Los ejemplos de usuario que añada de esta forma no cuentan en los totales de ejemplos de usuario de intención, para los que existen límites según el plan. Consulte [Límites de intenciones](/docs/services/assistant?topic=assistant-intents#intents-limits) para obtener más información.

## Gestión de los archivos cargados
{: #intent-recommendations-manage-files}

Después de cargar al menos un archivo, puede abrir la colección de *Archivos de origen de ejemplos de usuario* para gestionar los archivos. Los archivos cargados se añaden a una colección de archivos que se comparte en la instancia de servicio actual de {{site.data.keyword.conversationshort}}.

1.  Para acceder a la colección, pulse para abrir una intención, pulse **Mostrar recomendaciones** y luego pulse **Ver archivos** en la barra lateral.

    Si todavía no tiene ninguna intención añadida, desde la página principal de Intenciones amplíe la sección *Recomendaciones de intención*, pulse **Obtener recomendaciones**, pulse **Mostrar todas las recomendaciones** y, a continuación, pulse **Ver archivos**.

1.  Puede llevar a cabo las acciones
siguientes:

    - Para añadir un archivo, pulse **Añadir archivos** y luego busque un archivo y selecciónelo.
    - Para suprimir un archivo, debe eliminar todos los archivos que se han cargado desde cualquier conocimiento asociado con esta instancia de servicio; no se puede suprimir un solo archivo. En primer lugar, asegúrese de que nadie más esté utilizando los archivos y, a continuación, pulse **Suprimir todo** para suprimir todos los archivos cargados.

1.  Cierre la página *Archivos de origen de ejemplos de usuario*.
