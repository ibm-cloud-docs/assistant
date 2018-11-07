---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-30"

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

# Definición de intenciones

Las ***intenciones*** son objetivos o propósitos expresados en una entrada de cliente, como por ejemplo responder a una pregunta o procesar el pago de una factura. Al reconocer la intención expresada en una entrada de cliente, el servicio {{site.data.keyword.conversationshort}} puede elegir el flujo de diálogo correcto para responder a la misma.
{: shortdesc}

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/6HAZpBHqX8M" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Límites de las intenciones
{: #intent-limits}

El número de intenciones y los ejemplos que puede crear dependen de su plan de servicio {{site.data.keyword.conversationshort}}:

| Plan de servicio     | Intenciones por espacio de trabajo | Ejemplos por espacio de trabajo |
|------------------|----------------------:|-----------------------:|
| Estándar/Premium |                 2.000 |                 25.000 |
| Lite             |                   100 |                 25.000 |

## Creación de intenciones
{: #creating-intents}

Utilice la herramienta {{site.data.keyword.conversationshort}} para crear intenciones.

1.  En la herramienta {{site.data.keyword.conversationshort}}, abra el espacio de trabajo y seleccione el separador **Intenciones** en la barra de navegación. Si no ve **Intenciones**, utilice el menú ![Menú](images/Menu_16.png) para abrir la página.

1.  Seleccione **Crear nueva**.

1.  En el campo **Nombre de intención**, escriba un nombre para la intención.
    - El nombre de la intención puede contener letras (en Unicode), números, signos de subrayado, guiones y puntos.
    - El nombre no puede consistir únicamente en `..` ni en cualquier otra serie formada solo por puntos.
    - Los nombres de intención no pueden contener espacios y no deben superar los 128 caracteres. A continuación se muestran algunos ejemplos de nombres de intención:
        - `#weather_conditions`
        - `#pay_bill`
        - `#escalate_to_agent`

    Las herramientas incluyen automáticamente el carácter `#` en los nombres de intención, de modo que no tiene que añadirlo.
    {: tip}

    Añada una descripción de la intención en el campo **Descripción**. 

1.  Seleccione **Crear intención** para guardar el nombre de la intención.

    ![Captura de pantalla que muestra una nueva definición de intención](images/create_intent.png)

1.  A continuación, en el campo **Añadir ejemplos de usuario**, escriba el texto de un ejemplo de usuario para la intención. Un ejemplo podría ser cualquier serie de hasta 1024 caracteres de longitud. Estos serían ejemplos correspondientes a la intención `#pay_bill`:
    - `I need to pay my bill.`
    - `Pay my account balance`
    - `make a payment`

    **Hacer referencia a entidades y sinónimos como ejemplos de intenciones**

    Si ha definido, o piensa definir, entidades correspondientes a esta intención, consulte las entidades, o sus sinónimos asociados, en algunos de los ejemplos. Esto ayuda a establecer una relación entre la intención y las entidades.

    ![Captura de pantalla que muestra una definición de intención](images/define_intent.png)
    {: #entity-as-example}

    *Importante*:

      - Los datos del ejemplo de la intención deberían ser representativos y típicos de los datos que los usuarios finales proporcionarán. Se pueden recopilar ejemplos de datos de usuario reales, o de personas expertas del campo específico. Es importante la naturaleza de la representatividad y precisión de los datos. 
      - Tanto los datos de aprendizaje como los de prueba (a efectos de evaluación) deberían reflejar la distribución de las intenciones en el uso real. Habitualmente, las intenciones más frecuentes tienen relativamente más ejemplos, y una mejor cobertura de respuestas. 
      - Puede incluir puntuación en el texto de ejemplo, en la medida que sea natural. Si cree que algunos usuarios expresarán sus intenciones con ejemplos que incluyan puntuación, mientras que otros usuarios no lo harán, incluya dos versiones. En general, cuanto mayor sea la cobertura de los distintos patrones, mejor será la respuesta. 

    **Hacer referencia directamente a una @Entidad como un ejemplo de intención**

    También puede elegir hacer directamente referencia a las entidades en los ejemplos de las intenciones. Por ejemplo, supongamos que tiene una entidad denominada `@PhoneModelName`, que contiene los valores *Galaxy S8*, *Moto Z2*, *LG G6* y *Google Pixel 2*. Al crear una intención, por ejemplo, `#order_phone`, podría proporcionar los siguientes datos de entrenamiento:  
    - Can I get a `@PhoneModelName`?
    - Help me order a `@PhoneModelName`.
    - Is the `@PhoneModelName` in stock?
    - Add a `@PhoneModelName` to my order.

    ![Captura de pantalla que muestra una definición de intención](images/define_intent_entity.png)
    **Nota**: Actualmente, únicamente se puede hacer referencia de forma directa a entidades cerradas que defina (se ignoran los valores de patrón). 
No es posible utilizar [entidades de sistema](system-entities.html).

    Si elige hacer referencia a una entidad como un ejemplo de una intención (por ejemplo, `@PhoneModelName`) *en un lugar cualquiera* de los datos de entrenamiento, cancelará el valor utilizado en una referencia directa (por ejemplo, *Galaxy S8*) en un ejemplo de intención en cualquier otro lugar. Todas las intenciones utilizarán entonces una aproximación de ejemplo de entidad como una intención. No podrá seleccionar esta aproximación para una intención específica únicamente. 

    En la práctica, esto significa que si anteriormente ha entrenado la mayoría de sus intenciones basándose en referencias directas (*Galaxy S8*), y ahora se utilizan las referencias de entidad (`@PhoneModelName`), ni que sea para una única intención, afectará a todo el entrenamiento anterior. Si elige utilizar referencias de `@Entidad`, deberá ser cuidadoso reemplazando todas las referencias directas con referencias de `@Entidad`. 

    **Nota**: La definición de una intención de ejemplo con una `@Entidad` que tiene 10 valores definidos para la misma **no** equivale a especificar dicho ejemplo 10 veces. El servicio de {{site.data.keyword.conversationshort}} no da tanto peso a dicha sintaxis de intención de ejemplo. 

    **Importante**: Los nombres de intención y el texto de ejemplo pueden ser expuestos en los URL cuando una aplicación interactúa con el servicio. No incluya información confidencial o personal en estos artefactos.

    Pulse **Añadir ejemplo** para guardar el ejemplo. 

1.  Repita el mismo proceso para añadir más ejemplos. Puede tabular entre cada ejemplo. Especifique al menos 5 ejemplos para cada intención. Cuantos más ejemplos proporcione, más precisa podrá ser la aplicación.

1.  Cuando haya terminado de añadir ejemplos, seleccione ![Flecha de cierre](images/close_arrow.png) para dar por finalizada la creación de la intención. 

### Resultados

La intención que ha creado se añade al separador Intenciones y el sistema empieza a formarse a sí mismo con los nuevos datos.

## Edición de intenciones

Puede seleccionar cualquier intención de la lista para abrirla a fin de editarla. Puede realizar
los siguientes cambios:

- Cambiar el nombre de la intención.
- Suprimir la intención.
- Añadir, editar o suprimir ejemplos.
- Mover un ejemplo a otra intención.

Puede tabular desde el nombre de la intención a cada ejemplo, editando los ejemplos si así lo desea.

Para mover o suprimir un ejemplo, seleccione el ejemplo seleccionando el recuadro de selección y, a continuación, seleccione **Mover** o **Suprimir**. 

  ![Captura de pantalla que muestra cómo mover o suprimir un ejemplo](images/move_example.png)

## Búsqueda de intenciones

Utilice la característica de búsqueda para encontrar ejemplos, nombres de intenciones y descripciones. 

1.  Seleccione el separador **Intenciones** en la barra de navegación. 

    ![Visión general del separador Intenciones](images/intent_oview.png)

1.  Seleccione el icono de búsqueda: ![Icono de búsqueda](images/search_icon.png). 

1.  Especifique una frase o término de búsqueda. 

    ![Término de búsqueda de intención](images/searchint_1.png)

    **Nota**: La primera vez que efectúa la búsqueda, se crea un índice por lo que es posible que vea un mensaje para que espere mientras se indexa el contenido. 

### Resultados

Se mostrarán las intenciones que contienen su término de búsqueda, con los correspondientes ejemplos. Seleccione un resultado para editarlo. 

  ![Devolución de búsqueda de intenciones](images/searchint_2.png)

## Importación de intenciones y ejemplos

Si tiene un gran número de intenciones y ejemplo, puede que le resulte más fácil importarlos desde un archivo CSV (valores separador por comas) que definirlos uno por uno en la herramienta {{site.data.keyword.conversationshort}}.

1.  Recopile las entidades y ejemplos en un archivo CSV o expórtelos desde una hoja de cálculo a un archivo CSV. El formato necesario para cada línea del archivo es el siguiente:

    ```
    <example>,<intent>
    ```
    {: screen}

    donde `<example>` es el texto de un ejemplo de usuario y `<intent>` es el nombre de la intención con la que desea que coincida el ejemplo. Por ejemplo:

    ```
    Tell me the current weather conditions.,weather_conditions
    Is it raining?,weather_conditions
    What's the temperature?,weather_conditions
    Where is your nearest location?,find_location
    Do you have a store in Raleigh?,find_location
    ```
    {: screen}

    > **Importante:** Guarde el archivo CSV con codificación UTF-8 y sin marca de orden de bytes (BOM).

1.  En la herramienta {{site.data.keyword.conversationshort}}, abra el espacio de trabajo y seleccione el separador **Intenciones** en la barra de navegación. Si no ve **Intenciones**, utilice el menú ![Menú](images/Menu_16.png) para abrir la página.

1.  Seleccione el icono *Importar* ![Icono de importar](images/importGA.png). A continuación, arrastre un archivo o selecciónelo en su sistema. El archivo se valida y se importa y el sistema empieza a formarse a sí mismo con los datos nuevos.

    ![Opción de importar](images/ImportIntent.png)

    > **Importante:** El tamaño máximo del archivo CSV es 10 MB. Si el archivo CSV es mayor, considere la posibilidad de dividirlo en varios archivos y de importarlos por separado.

### Resultados

Puede ver las intenciones importadas y los ejemplos correspondientes en el separador **Intenciones**. Es posible que deba renovar la página para ver los nuevos ejemplos e intenciones.

## Exportación de intenciones
{: #export_intents}

Puede exportar varias intenciones a un archivo CSV para luego importarlas y reutilizarlas para otra aplicación de {{site.data.keyword.conversationshort}}. 

1.  En el separador Intenciones, seleccione las intenciones que desea de la lista y elija *Exportar*.

    ![Opción de exportar](images/ExportIntent.png)

## Supresión de intenciones
{: #delete_intents}

Puede seleccionar varias intenciones para suprimirlas.

**IMPORTANTE**: Si suprime intenciones y también suprime todos los ejemplos asociados, estos elementos no se pueden recuperar más tarde. Todos los nodos de diálogo que hacen referencia a estas intenciones se deben actualizar manualmente para que dejen de hacer referencia al contenido suprimido.

1.  En el separador Intenciones, seleccione las intenciones que desea de la lista y elija *Suprimir*.

    ![Opción de suprimir](images/DeleteIntent.png)

## Prueba de las intenciones
{: #testing-your-intents}

Cuando termine de crear nuevas intenciones, puede probar el sistema para ver si reconoce las intenciones tal como espera.

1.  En la herramienta {{site.data.keyword.conversationshort}}, seleccione el icono ![Preguntar a Watson](images/ask_watson.png).

1.  En el panel *Pruébelo*, escriba una pregunta u otra serie de texto y pulse Intro para ver si se reconoce la intención. Si se reconoce la intención errónea, puede mejorar el modelo añadiendo este texto como ejemplo a la intención correcta.

    Si ha realizado cambios recientemente en el espacio de trabajo, es posible que vea un mensaje que indica que el sistema continúa formándose. Si ve este mensaje, espere hasta que finalice la entrenamiento antes de realizar la prueba:
    {: tip}

    ![Captura de pantalla que muestra el mensaje de entrenamiento](images/training.png)

    La respuesta indica la intención que se ha reconocido de la entrada.

    ![Captura de pantalla de prueba de intenciones](images/test_intents.png)

1.  Si el sistema no ha reconocido la intención correcta, puede corregirla. Para corregir la intención reconocida, seleccione la intención mostrada y luego seleccione la intención correcta de la lista. Una vez enviada la corrección, el sistema se forma a sí mismo automáticamente para incorporar los nuevos datos.

    ![Captura de pantalla de corrección de una intención reconocida](images/correct_intent.png)

1.  Si la entrada no está relacionada con la aplicación, puede indicarlo. Seleccione la intención mostrada y elija **Marcar como irrelevante**.

    ![Captura de pantalla de Marcar como irrelevante](images/irrelevant.png)

Si las intenciones no se reconocen correctamente, considere la posibilidad de realizar los siguientes tipos de cambios:

- Añada el texto que no se reconoce como ejemplo a la intención correcta.
- Mueva los ejemplos existentes de una intención de otra.
- Si considera que las intenciones se parecen demasiado, vuelva a definirlas.

## Puntuación absoluta y Marcar como irrelevante

A partir de febrero de 2017, hay un nuevo algoritmo para la puntuación de confianza de la intención y para devolver intenciones. También puede marcar entradas como *irrelevantes*. Estos cambios pueden requerir que [actualice su espacio de trabajo![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](upgrading.html){: new_window}.

### Puntuación absoluta

El servicio {{site.data.keyword.conversationshort}} ahora puntúa la confianza de cada intención por separado, no en relación con otras intenciones. Esto ofrece la flexibilidad de devolver varias intenciones. También significa que es posible que el sistema no devuelva ninguna intención. Si la intención principal tiene una baja confianza de que haya intenciones relacionadas con la entrada del usuario (inferior a 0,2), la API todavía la incluirá en la salida de la matriz de intenciones, pero condicionando a que dicha #intención devolverá false. Si desea detectar el caso en el que no se detectaron intenciones con una buena confianza, puede condicionar con `irrelevante`. 

A medida que cambiar las puntuaciones de confianza de las intenciones, es posible que se deban reestructurar los diálogos. Por ejemplo, si el diálogo está condicionado por una intención que ahora tiene un nivel de confianza bajo, la respuesta del sistema dejará de ser correcta.

### Marcar como irrelevante
{: #mark-irrelevant}

Consulte [idiomas soportados](lang-support.html) para ver la disponibilidad de esta característica.

Después de actualizar el espacio de trabajo, puede [probar la entrada](#testing-your-intents) en el panel *Pruébelo* para ver los cambios. Puede utilizar **Marcar como irrelevante** para indicar que la entrada no está relacionada con la aplicación. 

Si tiene una intención, como #off_topic, para aquellas entradas que quedan fuera del ámbito de un tema, suprima la intención y pruebe el espacio de trabajo marcando las entradas son irrelevantes.

**Importante**: las entradas que están marcadas como irrelevante se almacenan en el espacio de trabajo y se incluyen como parte de los datos de entrenamiento. Asegúrese de que desea realizar este cambio.

- No se puede acceder a las entradas ni se pueden modificar más adelantes con las herramientas.
- La única forma de eliminar la etiqueta **irrelevante** es utilizando la misma entrada en el panel *Pruébelo* y modificando la intención. 
