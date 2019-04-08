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

# Obtención de ayuda de Watson
{: #intent-recommendations}

Si tiene transcripciones de registros de conversaciones, por ejemplo de un centro de soporte al cliente, que contienen expresiones reales de usuarios, deje que el servicio examine los datos existentes en busca de candidatos de ejemplos de usuario de intenciones.

## Carga de archivos
{: #intent-recommendations-log-files-add}

Antes de empezar, cree un archivo que proporcionará al servicio. El archivo debe ser un archivo de valores separados por comas (CSV), con una expresión de usuario por línea. Lo ideal es que las expresiones sean frases cortas extraídas de transcripciones del centro de atención al cliente que contengan preguntas y solicitudes reales de clientes. Cada archivo de expresiones de usuario puede tener un máximo de 20 MB.

Siga las siguientes directrices adicionales:

  - Elimine los datos confidenciales de las expresiones que incluya en el archivo.

    Los datos confidenciales incluyen cualquier información relacionada con una persona física identificable, como nombres, direcciones de correo electrónico e ID de cliente, y datos regulados, como información protegida sobre salud.
  - No incluya expresiones de usuario que superen los 1.024 caracteres de longitud. Las expresiones más largas se truncan.
  - El archivo debe contener al menos 100 expresiones; un archivo con 500 o más expresiones le dará mejores resultados.
  - Si una expresión contiene una coma, especifique la expresión entre comillas.
  - El archivo CSV debe incluir solo una columna.
  - Elimine todo lo que no sea una expresión generada por el usuario, incluidas las respuestas o notas de agentes humanos.

  Por ejemplo:

  ```
  What happens to my coverage if I trade in my car?
  i'd like to buy a house.
  How do I add a dependent to my plan?
  "first, i want to know if i am already registered."
  ```

Los archivos que cargue se comparten entre todos los conocimientos de la instancia de servicio actual. Se examinan las expresiones de todos los archivos disponibles cuando solicita recomendaciones de ejemplo de intenciones de usuarios.

## Obtención de recomendaciones de ejemplo de intenciones de usuarios
{: #intent-recommendations-get-example-recommendations}

En el siguiente vídeo se proporciona una visión general de 2 minutos de cómo obtener recomendaciones de ejemplo de intenciones de usuarios.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Recomendaciones de ejemplo de intenciones de usuarios" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/L3FI8KeZfsc?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

No es necesario que los archivos que cargue asocien ejemplos con intenciones. Simplemente proporcione las expresiones de usuario y deje que el servicio realice el trabajo de elegir las que resulten apropiadas para la intención actual. Para cada intención, el servicio analiza los mismos datos para encontrar ejemplos de usuario que resulten apropiados para dicha intención.

1.  Siga los pasos que se indican en [Creación de intenciones](/docs/services/assistant?topic=assistant-intents#intents-creating-intents-task) para crear una intención.

1.  Añada al menos 5 ejemplos de usuario que ilustren el rango completo de expresiones típicas que prevé que los usuarios puedan utilizar para activar esta intención.

    Estos ejemplos de expresiones de usuarios enseñan al servicio los tipos de expresiones que debe buscar en los archivos que cargue.

1.  Pulse **Mostrar recomendaciones**.

    Los archivos de origen de ejemplos de usuario se comparten entre los conocimientos de una instancia de servicio. Si sus compañeros de trabajo poseen conocimientos en la misma instancia y cargan archivos, sus archivos se añaden a la colección de *Archivos de origen de ejemplos de usuario* compartidos.

1.  **Solo la primera vez**: pulse **Cargar archivos** y luego pulse **Elegir un archivo** para examinar el archivo CSV que ha creado anteriormente y selecciónelo.

    No hay problema si el archivo que ha cargado contiene expresiones correspondientes a todos los tipos de intenciones. El servicio sabe en qué intención está trabajando y localiza ejemplos adecuados para realizar recomendaciones sobre esta intención específica.

    Después de cargar el archivo y de que el servicio lo procese, se muestran las expresiones recomendadas. Si no se realiza ninguna recomendación, significa que el archivo no contiene ejemplos que resulten adecuados para esta intención.

1.  Si el archivo no puede proporcionar recomendaciones útiles para ninguna de sus intenciones, puede probar otro conjunto de expresiones cargando otro archivo. Consulte [Gestión de archivos cargados](#intent-recommendations-manage-files) para obtener más información.

1.  Después de que el servicio muestre recomendaciones, seleccione las expresiones que desea añadir como ejemplos de usuario para esta intención y luego pulse **Añadir**. O bien pulse **Siguiente conjunto** para examinar más expresiones.
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

1.  Puede llevar a cabo las acciones
siguientes:

    - Para añadir un archivo, pulse **Añadir archivos** y luego busque un archivo y selecciónelo.
    - Para suprimir un archivo, debe eliminar todos los archivos que se han cargado desde cualquier conocimiento asociado con esta instancia de servicio; no se puede suprimir un solo archivo. En primer lugar, asegúrese de que nadie más esté utilizando los archivos y, a continuación, pulse **Suprimir todo** para suprimir todos los archivos cargados.

1.  Cierre la página *Archivos de origen de ejemplos de usuario*.
