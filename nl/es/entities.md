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

# Creación de entidades
{: #entities}

Las ***entidades*** representan información en la entrada de usuario que es relevante para la finalidad del usuario.

Si las intenciones representan verbos (la acción que un usuario quiere llevar a cabo), las entidades representan nombres (el objeto o el contexto de una acción). Por ejemplo, si la *intención* es para obtener una previsión meteorológica, se necesitan las *entidades* relevantes correspondientes a ubicación y a fecha para que la aplicación pueda devolver una previsión precisa.

El hecho de reconocer entidades en la entrada del usuario le ayuda a crear respuestas más útiles y específicas. Por ejemplo, supongamos que tiene una intención `#buy_something`. Cuando un usuario realiza una solicitud que activa la intención `#buy_something`,
la respuesta del asistente debe reflejar que comprende que *something* es lo que el usuario quiere comprar. Puede añadir una entidad `@product` y utilizarla para extraer información de la entrada de usuario sobre el producto en el que está interesado el cliente. (El símbolo `@` antes del nombre de la entidad ayuda a identificar claramente que se trata de una entidad.)

Por último, puede añadir varias respuestas al árbol de diálogo con términos que difieren en función del valor `@product` que se detecta en la solicitud del usuario.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Cómo trabajar con entidades" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/o-uhdw6bIyI" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Visión general de la evaluación de entidades
{: #entities-described}

El servicio detecta entidades en la entrada de usuario utilizando uno de los siguientes métodos de evaluación:

### Método basado en diccionario
{: #entities-dictionary-overview}

El servicio busca términos en la entrada del usuario que coincidan con los valores, sinónimos o patrones que defina para la entidad.

- **Entidad de sinónimo**: El usuario defina una categoría de términos como una entidad (`color`) y luego uno o varios valores en dicha categoría (`blue`). Para cada valor, especifique un grupo de sinónimos (`aqua`, `navy`). También puede seleccionar sinónimos para añadirlos de las recomendaciones que le ha hecho el servicio.

    En el momento de la ejecución, el servicio reconoce los términos en la entrada de usuario que coinciden exactamente con los valores o con los sinónimos que ha definido para la entidad como menciones de dicha entidad.
- **Entidad de patrón**: El usuario define una categoría de términos como entidad (`contact_info`) y luego uno o varios valores en dicha categoría (`email`). Para cada valor, el usuario especifica una expresión regular que define el patrón textual de las menciones de dicho tipo de valor. En el caso del valor de la entidad `email`, es posible que desee especificar una expresión regular que defina un patrón `text@text.com`.

    En el momento de la ejecución, el servicio busca patrones que coincidan con su expresión regular en la entrada de usuario e identifica las coincidencias como menciones de dicha entidad.
- **Entidad del sistema**: Entidades de sinónimos predefinidas por IBM. Cubren las categorías de uso común, tales como números, fechas y horas. Solo tiene que habilitar una entidad del sistema para empezar a usarla.

### Método basado en contexto
{: #entities-annotations-overview}

Cuando define una entidad contextual, se entrena un modelo sobre el *término anotado* y el *contexto* en el que se utiliza el término en la frase que anota. Este nuevo modelo de entidad contextual permite al servicio calcular una puntuación de confianza que identifica la probabilidad de que una palabra o frase sea una instancia de una entidad, en función de cómo se utiliza en la entrada del usuario.

- **Entidad contextual**: En primer lugar, el usuario define una categoría de términos como una entidad (`product`). A continuación, va a la página *Intenciones* y examina los ejemplos de usuario de intenciones existentes para encontrar menciones de la entidad y las etiqueta como tales. Por ejemplo, puede ir a la intención `#buy_something` y buscar un ejemplo de usuario como `I want to buy a Coach bag`. Puede etiquetar `Coach bag` como una mención de la entidad `@product`.

    A efectos de entrenamiento, el término que ha anotado, `Coach bag`, se añade como valor de la entidad `@product`.

    En el momento de la ejecución, el servicio evalúa los términos en función del contexto en el que se utilizan solo en la sentencia. Si la estructura de una solicitud de usuario que menciona el término coincide con la estructura de un ejemplo de usuario de intención en el que se etiqueta una mención, el servicio interpreta el término de modo que sea una mención de ese tipo de entidad. Por ejemplo, la entrada de usuario puede incluir la expresión `I want to buy a Gucci bag`. Debido a la similitud de la estructura de esta frase con el ejemplo de usuario que ha anotado (`I want to buy a Coach bag`), el servicio reconoce `Gucci bag` como mención de entidad `@product`.

    Cuando se utiliza un modelo de entidad contextual para una entidad, el servicio *no* busca coincidencias exactas de texto o de patrón para la entidad en la entrada de usuario, sino que se centra en el contexto de la frase en la que se menciona la entidad.

    Si opta por definir valores de entidad utilizando anotaciones, añada al menos 10 anotaciones por entidad para dar al modelo de entidad de contexto suficientes datos para que resulten fiables.

## Creación de entidades
{: #entities-creating-task}

Utilice la herramienta {{site.data.keyword.conversationshort}} para crear entidades.

1.  En la herramienta {{site.data.keyword.conversationshort}}, abra el conocimiento de diálogo y luego pulse el separador **Entidades**. Si **Entidades** no está visible, utilice el menú ![Menú](images/Menu_16.png) para abrir la página.

1.  Pulse **Añadir entidad**.

    También puede pulsar **Utilizar entidades del sistema** para seleccionar entre una lista de entidades comunes, proporcionadas por {{site.data.keyword.IBM_notm}}, que se pueden aplicar a cualquier caso de uso. Consulte [Habilitación de entidades del sistema](#entities-enable-system-entities) para ver más detalles.

1.  En el campo **Nombre de entidad**, escriba un nombre descriptivo para la entidad.

    El nombre de entidad puede contener letras (en Unicode), números, signos de subrayado y guiones. Por ejemplo:
    - `@location`
    - `@menu_item`
    - `@product`

    No incluya espacios en el nombre. El nombre no puede tener más de 64 caracteres. El nombre no debe comenzar con la serie `sys-` porque está reservada para las entidades del sistema.

    La herramienta incluye automáticamente el carácter @ en el nombre de entidad, de modo que no tiene que añadirlo.
    {: tip}

1.  Pulse **Crear entidad**.

    ![Captura de pantalla sobre la creación de una entidad](images/create_entity.png)

1.  Para esta entidad, elija si desea que el servicio utilice un enfoque basado en diccionario o uno basado en contexto para encontrar menciones del mismo y luego siga el procedimiento correspondiente.

    **Para cada entidad que cree, elija un solo tipo de entidad.** En cuando añada una anotación para una entidad, el modelo contextual se inicializa y se convierte en el enfoque principal para analizar la entrada de usuario para encontrar menciones de dicha entidad. El contexto en el que se utiliza la mención en la entrada de usuario tiene prioridad sobre las coincidencias exactas que pueda haber. Consulte [Visión general de la evaluación de entidades](#entities-described) para obtener más información sobre la forma en que se evalúa cada tipo.

    - [Entidades basadas en diccionario](#entities-create-dictionary-based)
    - [Entidades basadas en contexto](#entities-create-annotation-based)

## Adición de entidades basadas en diccionario
{: #entities-create-dictionary-based}

Las entidades basadas en diccionario son aquellas para los que se definen términos, sinónimos o patrones específicos. En el momento de la ejecución, el servicio solo encuentra menciones de la entidad cuando un término en la entrada de usuario coincide exactamente (o se parece mucho si la coincidencia aproximada está habilitada) con el valor de uno de sus sinónimos.

1.  En el campo **Nombre de valor**, escriba el texto de un posible valor para la entidad y pulse `Intro`. El valor de una entidad puede ser cualquier serie de un máximo de 64 caracteres de longitud.

    **Importante:** No incluya información confidencial o personal en los nombres o valores de entidad. Los nombres y valores pueden ser expuestas en URL en una app.

1.  Si desea que el servicio reconozca términos con una sintaxis parecida al valor de entidad y a los sinónimos que especifique, pero sin necesidad de que haya una coincidencia exacta, pulse el conmutador **Coincidencia aproximada** para activarlo.

    Esta característica está disponible para los idiomas indicados en el tema [Idiomas soportados](/docs/services/assistant?topic=assistant-language-support).

    **Coincidencia aproximada**
    {: #entities-fuzzy-matching}

    La coincidencia aproximada tiene estos componentes:

    - *Lematización* - La característica reconoce la forma del lexema de los valores de entidad que tienen varias formas gramaticales. Por ejemplo, el lexema de 'bananas' sería 'banana', mientras que el lexema de 'running' sería 'run'.
    - *Errores ortográficos* - La característica es capaz de correlacionar la entrada de usuario con la entidad adecuada correspondiente, a pesar de que contenga errores ortográficos o ligeras diferencias sintácticas. Por ejemplo, si define *giraffe* como sinónimo para una entidad animal y la entrada de usuario contiene los términos *giraffes* o *girafe*, la coincidencia aproximada es capaz de correlacionar correctamente el término con la entidad animal.
    - *Coincidencia parcial* - Esta característica sugiere automáticamente sinónimos basados en subseries presenten en las entidades definidas por el usuario y asigna un ámbito de confianza bajo en comparación con una coincidencia de entidad exacta.

    En el caso del inglés, la coincidencia aproximada evita la captura de algunas palabras en inglés válidas y comunes como coincidencias aproximadas para una determinada entidad. Esta característica utiliza palabras del diccionario inglés estándar. También puede definir un valor/sinónimo de entidad inglés, y la coincidencia aproximada encontrará coincidencias únicamente del valor/sinónimo definido de su entidad. Por ejemplo, una coincidencia aproximada podría encontrar coincidencias del término `unsure` (inseguro) con `insurance` (seguro); sin embargo, si define `unsure` como una valor/sinónimo de una entidad como `@option`, entonces `unsure` siempre se emparejaría con `@option`, y no con `insurance`.
    {: note}

    El valor de coincidencia aproximada no afecta a las recomendaciones de sinónimos. Aunque la coincidencia aproximada esté habilitada, solo se sugieren sinónimos para el valor exacto que especifique, no para las ligeras variaciones del valor.

1.  Cuando haya especificado un nombre de valor, puede añadir cualquier sinónimo o definir patrones específicos para dicho valor de entidad seleccionando `Sinónimos` o `Patrones` en el menú desplegable *Tipo*.

    ![Selector de tipo para el valor](images/value_type.png)

    **Nota:** Puede añadir sinónimos *o* patrones para un solo valor de entidad, pero no ambos.

    ***Sinónimos***
    {: #entities-synonyms}

    - En el campo **Sinónimos**, escriba cualquier sinónimo para el valor de entidad. Un sinónimo puede ser cualquier serie de un máximo de 64 caracteres de longitud.

      ![Captura de pantalla de la definición de una entidad](images/define_entity.png)

      El servicio {{site.data.keyword.conversationshort}} también puede recomendar sinónimos para los valores de entidad. La función de recomendación busca sinónimos relacionados en función de la similitud contextual extraída de un vasto cuerpo de información existente, que incluye grandes fuentes de texto escrito, y utiliza técnicas de proceso de lenguaje natural para identificar palabras similares a los sinónimos existentes en el valor de entidad.

    - Pulse **Mostrar recomendaciones**.

    - El servicio {{site.data.keyword.conversationshort}} hará varias recomendaciones para los sinónimos. Los términos se muestran en minúsculas, pero el servicio reconoce las menciones de los sinónimos tanto si se especifican en minúsculas como en mayúsculas.

      Cuanto más coherentes sean los sinónimos de los valores de las entidades, más relevantes y precisas serán las recomendaciones. Por ejemplo, si tiene varias palabras sobre un tema, obtendrá mejores sugerencias que si tiene una o dos palabras aleatorias.
      {: tip}

      ![Pantalla 2 de recomendaciones de sinónimos](images/synonym_2.png)

    - Seleccione los sinónimos que desee incluir y pulse **Añadir seleccionados**.

      Debe pulsar el botón **Añadir seleccionados** para los sinónimos que va a añadir. Si pasa al siguiente conjunto sin pulsar primero este botón, las selecciones se pierden.

      ![Pantalla 3 de recomendaciones de sinónimos](images/synonym_3.png)

    - El servicio {{site.data.keyword.conversationshort}} añade estos sinónimos a la entidad y sugiere sinónimos adicionales.

      Si no recibe ninguna recomendación de sinónimos adicionales, podría deberse a que la entidad ya está bien definida, o bien a que contiene contenido que el recomendador no es capaz de ampliar en este momento.
      {: tip}

      Si elige no seleccionar un sinónimo recomendado, el sistema lo tratará como un término que no le interesa y modificará el siguiente conjunto de recomendaciones que verá cuando pulse `Añadir seleccionados` o `Siguiente conjunto`. Esta inferencia solo se aplica mientras está seleccionando sinónimos; el servicio no utiliza la información sobre los sinónimos omitidos para ninguna otra finalidad.
      {: note}

      ![Pantalla 4 de recomendaciones de sinónimos](images/synonym_4.png)

      Siga añadiendo los sinónimos que desee. Cuando termine de aceptar recomendaciones, pulse **X** para cerrar.

    ***Patrones***
    {: #entities-patterns}

    - El campo **Patrones** le permite definir patrones específicos para un valor de entidad. Un patrón **debe** especificarse como una expresión regular en el campo.

      - Para cada valor de entidad, puede haber un máximo de 5 patrones.
      - Cada patrón (expresión regular) está limitado a 512 caracteres.

      ![Captura de pantalla para definir una entidad de patrón](images/patternents1.png)
      {: #entities-pattern-entities}

      En este ejemplo, para la entidad *ContactInfo*, los patrones correspondientes a teléfono (phone), correo electrónico (email) y sitio web (website) se pueden definir del siguiente modo:
      - Phone
        - `localPhone`: `(\d{3})-(\d{4})`, por ejemplo 426-4968
        - `fullUSphone`: `(\d{3})-(\d{3})-(\d{4})`, por ejemplo 800-426-4968
        - `internationalPhone`: `^(\(?\+?[0-9]*\)?)?[0-9_\- \(\)]*$`, por ejemplo +44 1962 815000
      - `email`: `\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}\b`, por ejemplo name@ibm.com
      - `website`: `(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$`, por ejemplo https://www.ibm.com

      Cuando se utiliza entidades de patrón, generalmente es necesario almacenar el texto que coincida con el patrón en una variable de contexto (o variable de acción), desde dentro del árbol de diálogo. Para obtener más información, consulte [Definición de una variable de contexto](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-var-define).

      Supongamos que quiere preguntar al usuario su dirección de correo electrónico. La condición del nodo de diálogo contendrá una condición parecida a `@contactInfo:email`. Para asignar el correo electrónico especificado por el usuario como una variable de contexto, se puede utilizar la siguiente sintaxis para capturar la coincidencia de patrón dentro de la sección de respuesta del nodo de diálogo:

      <table>
      <caption>Cómo guardar un patrón</caption>
        <tr>
          <th>Variable</th>
          <th>Valor</th>
        </tr>
        <tr>
          <td>email</td>
          <td>`<? @contactInfo.literal ?>`</td>
        </tr>
      </table>

      ***Capturar grupos***
      {: #entities-capture-group}

      En las expresiones regulares, cualquier patrón dentro de unos paréntesis normales se capturará como un grupo. Por ejemplo, la entidad `@ContactInfo` tiene un valor de patrón llamado `fullUSphone` que contiene tres grupos capturados:

      - `(\d{3})` - Código de área estadounidense
      - `(\d{3})` - Prefijo
      - `(\d{4})` - Número de línea

      La agrupación es de utilidad si, por ejemplo, desea que el servicio {{site.data.keyword.conversationshort}} solicite a los usuarios su número de teléfono, y a continuación solo desee utilizar el código de área en el número que se ha proporcionado como respuesta.

      Para asignar el código de área especificado por el usuario como una variable de contexto, se puede utilizar la siguiente sintaxis para capturar la coincidencia de grupo dentro de la sección de respuesta del nodo de diálogo:

      <table>
      <caption>Cómo guardar un grupo de captura</caption>
        <tr>
          <th>Variable</th>
          <th>Valor</th>
        </tr>
        <tr>
          <td>area_code</td>
          <td>`<? @ContactInfo.groups[1] ?>`</td>
        </tr>
      </table>

      Para obtener más información sobre el uso de grupos de captura en el diálogo, consulte [Almacenamiento y reconocimiento de grupos de patrones de entidad en la entrada](/docs/services/assistant?topic=assistant-dialog-tips#dialog-tips-get-pattern-groups).

      El motor de comparación de patrones que utiliza el servicio {{site.data.keyword.conversationshort}} tiene ciertas limitaciones de sintaxis, necesarias para evitar problemas de rendimiento que se pueden producir cuando se utilizan otros motores de expresión regular.

      - Los patrones de entidad no pueden contener:
        - Repeticiones positivas (por ejemplo, `x*+`)
        - Referencias anteriores (por ejemplo, `\g1`)
        - Ramas condicionales (por ejemplo, `(?(cond)true)`)
      - Cuando una entidad de patrón empieza o finaliza con un carácter Unicode, e incluye límites de palabra, por ejemplo `\bš\b`, la coincidencia de patrón no coincide correctamente con el límite de palabra. En este ejemplo, para la entrada `š zkouška`, la coincidencia devuelve `Grupo 0: 6-7 š` (`š zkou`_**`š`**_`ka`), en lugar de la respuesta correcta `Grupo 0: 0-1 š` (_**`š`**_ `zkouška`).

      El motor de expresión regular se basa en parte en el motor de expresión regular de Java. El servicio {{site.data.keyword.conversationshort}} generará un error si intenta cargar un patrón no soportado, ya sea mediante la API o desde dentro de la IU de herramientas del servicio {{site.data.keyword.conversationshort}}.

1.  Pulse **Añadir valor** y repita el proceso para añadir más valores de entidad.

1.  Cuando termine de añadir valores de entidad, pulse ![Flecha de cerrar](images/close_arrow.png) para terminar de crear la entidad.

La entidad que ha creado se añade al separador **Entidades** y el sistema empieza a formarse a sí mismo con los nuevos datos.

## Adición de entidades contextuales
{: #entities-create-annotation-based}

Las entidades basadas en contexto son aquellas para los que se anotan las apariciones de la entidad en frases de ejemplo para enseñar al servicio el contexto en el que se utiliza normalmente la entidad.

Para entrenar un modelo de entidad contextual, puede aprovechar sus ejemplos de intenciones, que ofrecen frases preparadas para que las anote.

La utilización de ejemplos de usuario de intención para definir entidades contextuales no afecta a la clasificación de dicha intención. Sin embargo, las menciones de entidad que etiquete también se añaden a dicha entidad como sinónimos. Y la clasificación de intenciones utiliza las menciones de sinónimos en los ejemplos de usuario de intención para establecer una referencia débil entre una intención y una entidad.
{: note}

1.  En la herramienta {{site.data.keyword.conversationshort}}, abra el conocimiento y pulse el separador **Intenciones**. Si no ve **Intenciones**, utilice el menú ![Menú](images/Menu_16.png) para abrir la página.

1.  Pulse una intención para abrirla.

    En este ejemplo, la intención `#place_order` define la función de pedido para un minorista con venta en línea.

    ![Seleccione la intención #place_order](images/oe-intent.png)

1.  Revise los ejemplos de intención para ver posibles menciones de entidad. Resalte una mención de entidad potencial en los ejemplos de intención.

    En este ejemplo, `computer` es la mención de entidad.

    ![Revisar ejemplos de intención](images/oe-intent-review.png)

    El icono Editar ![Icono Editar](images/oe-intent-edit.png) se utiliza para editar un ejemplo de usuario de intención; no está relacionado con la adición de anotaciones.
    {: tip}

1.  Se abre un recuadro de búsqueda que puede utilizar para buscar la entidad de la que la palabra o frase resaltada es una mención.

    ![Estado inicial del recuadro de búsqueda](images/oe-intent-search1.png)

    En este ejemplo, la búsqueda de `prod` detecta coincidencias para la entidad `@product`.

    ![Recuadro de búsqueda con el parámetro de búsqueda prod](images/oe-intent-search2.png)

    Si la entidad tiene valores de entidad existentes, se muestran solo con fines informativos. Está añadiendo la anotación a la entidad, no a ningún valor de entidad específico.

    Si desea enseñar el modelo que la mención es sinónimo de un valor de entidad existente, puede asociarla con un valor de entidad específico.
    {: important}

    Para asociar la mención con un valor de entidad específico, siga estos pasos:

    1.  Escriba el nombre de entidad completo y el valor en el campo de búsqueda. Por ejemplo, escriba `@product:IT`.
    1.  Cuando se muestre el valor de la entidad en el menú desplegable, selecciónelo.

1.  Seleccione la entidad a la que desea añadir la anotación.

    En este ejemplo, se está añadiendo `computer` como anotación para la entidad `@product`.

    Cree *al menos* 10 anotaciones para cada entidad contextual; se recomiendan más anotaciones para el uso de producción.
    {: important}

1.  Si ninguna de las entidades resulta adecuada, puede crear una entidad nueva seleccionando **@(crear una entidad nueva)**.

1.  Repita este proceso para cada mención de entidad que desee anotar.

    Asegúrese de anotar cada mención de un tipo de entidad que aparezca en los ejemplos de usuario que edite. Consulte [Lo que no anote es importante](#entities-counter-examples) para obtener más detalles.
    {: important}

1.  Ahora pulse la anotación que acaba de crear. Se abre un recuadro con la indicación `Ir a: <entity-name>`. Si pulsa dicho enlace irá directamente a la entidad.

    ![Verificar el valor computer para la entidad product](images/oe-verify-value.png)

    La anotación se añade a la entidad con la que la ha asociado y el sistema empieza a formarse a sí mismo con los nuevos datos.

    El término que ha anotado se añade a la entidad como un nuevo valor de diccionario. Si ha asociado el término anotado con un valor de entidad existente, el término se añade como sinónimo de dicho valor de entidad en lugar de como valor de entidad independiente.
    {: important}

1.  Para ver todas las menciones que ha anotado para una determinada entidad, en la página de configuración de la entidad pulse el separador **Anotación**.

    ![Selector de vista de anotación resaltado](images/oe-annotate2.png)

    Las entidades contextuales entienden los valores que no ha definido explícitamente. El sistema realiza predicciones sobre valores de entidad adicionales en función de cómo se anotan los ejemplos de usuario y utiliza estos valores para entrenar a otras entidades. Todos los ejemplos de usuario similares se añaden a la vista *Anotación*, de modo que puede ver cómo afecta esta opción al entrenamiento.
    {: note}

    Si no desea que las entidades contextuales utilicen esta compresión ampliada de valores de entidad, seleccione todos los ejemplos de usuario en la vista *Anotación* para dicha entidad y luego pulse **Suprimir**.

En el siguiente vídeo se muestra cómo anotar menciones de entidad.

<iframe class="embed-responsive-item" id="youtubeplayer0" title="Menciones de entidad de anotación" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/3WjzJpLsnhQ" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Si desea ver una guía de aprendizaje que muestra cómo definir entidades contextuales antes de añadir la suya propia, vaya a la [Guía de aprendizaje: Definición de entidades contextuales ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://www.ibm.com/cloud/garage/demo/try-watson-assistant-contextual-entities){: new_window}.

### Lo que no anota es importante
{: #entities-counter-examples}

Si tiene un ejemplo de intención con una anotación y otra palabra de dicho ejemplo coincide con el valor o con un sinónimo de la misma entidad, pero el valor *no* está anotado, esta omisión tiene un impacto. El modelo también aprende del contexto del término que no ha anotado. Por lo tanto, si etiquete un término como una mención de una entidad en un ejemplo de usuario, asegúrese de etiquetar también cualquier otra mención aplicable.

1.  La intención `#Customer_Care_Appointments` incluye dos ejemplos de intención con la palabra `visit`.

    ![Ejemplos de la intención Visit](images/oe-counter-1.png)

1.  En el primer ejemplo, desea anotar la palabra `visit` como valor de entidad de la entidad `@meeting`. Esto hace que `visit` sea equivalente a otros valores de la entidad `@meeting`, como `appointment`, como en los ejemplos "I'd like to make an appointment" (Quiero celebrar una reunión) o "I'd like to schedule a visit" (Quiero planificar una visita).

    ![Entidad @meeting](images/oe-counter-2.png)

1.  En el segundo ejemplo, la palabra `visit` se utiliza en un contexto que no es meeting (reunión). En este caso, puede seleccionar la palabra `appointment` del ejemplo de intención y anotarla como valor de entidad de la entidad `@meeting`. El modelo aprende del hecho de que la palabra `visit` en el mismo ejemplo no está anotada.

    ![Visit sin seleccionar](images/oe-counter-3.png)

## Habilitación de entidades del sistema
{: #entities-enable-system-entities}

El servicio {{site.data.keyword.conversationshort}} proporciona una serie de *entidades del sistema*, que son entidades comunes que puede utilizar para cualquier aplicación. La habilitación de una entidad del sistema permite llenar rápidamente su conocimiento con datos de entrenamiento que son comunes a muchos casos de uso.

Las entidades del sistema se pueden utilizar para reconocer una amplia gama de valores correspondientes a los tipos de objetos que representan. Por ejemplo, la entidad del sistema `@sys-number` coincide con cualquier valor numérico, incluidos números enteros, fracciones decimales o incluso números escritos como palabras.

Las entidades del sistema se mantienen de forma centralizada, de modo que cualquier actualización está disponible automáticamente. No puede modificar las entidades del sistema.

1.  En el separador Entidades, pulse **Entidades del sistema**.

    ![Captura de pantalla del separador "Entidades del sistema"](images/system_entities_1.png)

1.  Examine la lista de entidades del sistema para elegir las que resulten útiles para la aplicación.
    - Para ver más información sobre una entidad del sistema, incluidos ejemplos de entada coincidente, pulse la entidad en la lista.
    - Para ver detalles sobre las entidades del sistema disponibles, consulte [Entidades del sistema](/docs/services/assistant?topic=assistant-system-entities).

1.  Pulse el conmutador que hay junto a una entidad del sistema para habilitarla o inhabilitarla.

Después de habilitar las entidades del sistema, el servicio {{site.data.keyword.conversationshort}} comienza el formarse. Una vez finalizado el entrenamiento, puede utilizar las entidades.

## Límites de las entidades
{: #entities-limits}

El número de entidades, los valores de las entidades y los sinónimos que puede crear dependen del plan del servicio {{site.data.keyword.conversationshort}}:

| Plan de servicio      | Entidades por conocimiento | Valores de entidad por conocimiento | Sinónimos de entidad por conocimiento |
|-------------------|-------------------:|------------------------:|--------------------------:|
| Premium           |               1000 |                 100.000 |                   100.000 |
| Plus              |               1000 |                 100.000 |                   100.000 |
| Estándar          |               1000 |                 100.000 |                   100.000 |
| Lite              |                 25 |                 100.000 |                   100.000 |
{: caption="Detalles del plan de servicio" caption-side="top"}

Las entidades del sistema cuyo uso habilite cuentan en los totales de uso de su plan.

| Plan de servicio | Entidades contextuales y anotaciones |
|--------------|------------------------------------:|
| Premium      |        30 entidades contextuales con 3000 anotaciones |
| Plus         |        20 entidades contextuales con 2000 anotaciones |
| Estándar     |        20 entidades contextuales con 2000 anotaciones |
| Lite         |        10 entidades contextuales con 1000 anotaciones |
{: caption="Detalles de los planes de servicio, continuación" caption-side="top"}

## Edición de entidades
{: #entities-edit}

Puede pulsar cualquier entidad de la lista para abrirla a fin de editarla. Puede cambiar el nombre o suprimir entidades, y puede añadir, editar o suprimir valores, sinónimos o patrones.

Si cambia el tipo de entidad de `synonym` por `pattern`, o viceversa, se convierten los valores existentes, sin embargo, podría no resultar útil tal como está.
{: note}

## Búsqueda de entidades
{: #entities-search}

La característica de búsqueda sirve para encontrar nombres, valores y sinónimos de entidad.

1.  En la página **Entidades**, pulse el icono Buscar.

    ![Icono Buscar en la cabecera de la página Intenciones](images/header-search-icon.png)

    No es posible realizar búsquedas con las entidades de sistema.
    {: note}

1.  Especifique una frase o término de búsqueda.

    ![Término de búsqueda de entidad](images/searchent_1.png)

Se mostrarán las entidades que contienen su término de búsqueda, con los correspondientes ejemplos.

  ![Devolución de búsqueda de entidades](images/searchent_2.png)

## Exportación de entidades
{: #entities-export}

Puede exportar varias entidades a un archivo CSV para luego importarlas y reutilizarlas para otra aplicación de {{site.data.keyword.conversationshort}}.

- La información de patrón se incluye en la exportación de CSV. Las series delimitadas con `/` se considerarán un patrón (en contraposición a un sinónimo).
- Las anotaciones asociadas con entidades contextuales no se exportan. Debe exportar todo el conocimiento de diálogo para capturar tanto el valor de entidad como las anotaciones asociadas.

1.  Seleccione las entidades que desea y luego pulse **Exportar**.

    ![Botón Exportar entidad](images/ExportEntity.png)

## Importación de entidades
{: #entities-import}

Si tiene un gran número de entidades, puede que le resulte más fácil importarlas desde un archivo CSV (valores separados por comas) que definirlas una por una en la herramienta {{site.data.keyword.conversationshort}}.

Las anotaciones de entidad no se incluyen en la importación de un archivo CSV de entidad. Debe importar todo el conocimiento de diálogo para conservar las anotaciones asociadas para una entidad contextual en dicho conocimiento. Si solo exporta e importa entidades, las entidades contextuales que haya exportado se tratan como entidades basadas en diccionario después de importarlas.
{: note}

1.  Recopile las entidades en un archivo CSV o expórtelas desde una hoja de cálculo a un archivo CSV. El formato necesario para cada línea del archivo es el siguiente:

    ```
    <entity>,<value>,<synonyms>
    ```
    {: screen}

    donde &lt;entity&gt; es el nombre de una entidad, &lt;value&gt; es un valor para la entidad y &lt;synonyms&gt; es una lista separada por comas de sinónimos del valor.

    ```
    weekday,Monday,Mon
    weekday,Tuesday,Tue,Tues
    weekday,Wednesday,Wed
    weekday,Thursday,Thur,Thu,Thurs
    weekday,Friday,Fri
    weekday,Saturday,Sat
    weekday,Sunday,Sun
    month,January,Jan
    month,February,Feb
    month,March,Mar
    month,April,Apr
    month,May
    ```
    {: screen}

    También se da soporte a los patrones en la importación de archivos CSV. Las series delimitadas con `/` se considerarán un patrón (en contraposición a un sinónimo).

    ```
    ContactInfo,localPhone,/(\d{3})-(\d{4})/
    ContactInfo,fullUSphone,/(\d{3})-(\d{3})-(\d{4})/
    ContactInfo,internationalPhone,/^(\(?\+?[0-9]*\)?)?[0-9_\- \(\)]*$/
    ContactInfo,email,/\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}\b/
    ContactInfo,website,/(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/
    ```
    {: screen}

    Guarde el archivo CSV con codificación UTF-8 y sin marca de orden de bytes (BOM). El tamaño máximo del archivo CSV es 10 MB. Si el archivo CSV es mayor, considere la posibilidad de dividirlo en varios archivos y de importarlos por separado.  En la herramienta {{site.data.keyword.conversationshort}}, abra el conocimiento de diálogo y luego pulse el separador **Entidades**.
    {: tip}

1.  Pulse ![Importar](images/importGA.png) y luego arrastre un archivo o examine para seleccionar un archivo del sistema. El archivo se valida y se importa y el sistema empieza a formarse a sí mismo con los datos nuevos.

Puede ver las entidades importadas en el separador Entidades. Es posible que deba renovar la página para ver las entidades nuevas.

## Supresión de entidades
{: #entities-delete}

Puede seleccionar varias entidades para suprimirlas.

**IMPORTANTE**: Si suprime entidades, también suprime todos los valores, sinónimos o patrones asociados y estos elementos no se pueden recuperar más tarde. Todos los nodos de diálogo que hacen referencia a estas entidades o valores se deben actualizar manualmente para que dejen de hacer referencia al contenido suprimido.

1.  Seleccione las entidades que desea suprimir y pulse **Suprimir**.

    ![Botón Suprimir entidad](images/DeleteEntity.png)
