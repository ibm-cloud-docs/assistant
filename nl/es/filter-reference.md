---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-29"

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

# Referencia de las consultas de filtro
{: #filter-reference}

La API REST del servicio {{site.data.keyword.conversationshort}} ofrece potentes funciones de búsqueda por las consultas de filtro. Puede utilizar el parámetro `filter` de la API /logs para buscar en el registro del conocimiento los sucesos que coinciden con una consulta especificada.

El parámetro `filter` es una consulta que se puede colocar en memoria caché que limita los resultados a aquellos que coinciden con el filtro especificado. Puede filtrar según varios objetos que formen parte del modelo de respuesta JSON (por ejemplo, el texto de entrada de usuario, las intenciones y entidades detectadas o la puntuación de confianza).

Para ver ejemplos de consultas de filtro, consulte [Ejemplos](#filter-reference-examples).

Para obtener más información sobre el método /logs `GET` y su modelo de respuesta, revise la [Consulta de API ![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/apidocs/assistant?curl=#list-log-events-in-a-workspace){: new_window}.

## Sintaxis de las consultas de filtro
{: #filter-reference-syntax}

En el ejemplo siguiente se muestra el formato general de una consulta de filtro:

| Ubicación           | Operador de consulta | Término         |
|:------------------:|:--------------:|:------------:|
|`request.input.text`|`::`            |`"IBM Watson"`|

- La _ubicación_ identifica el campo según el que desea filtrar (en este ejemplo, `request.input.text`).
- El _operador de consulta_ especifica el tipo de coincidencia que desea utilizar (coincidencia aproximada o coincidencia exacta).
- El _término_ especifica la expresión o el valor que desea utilizar para evaluar el campo para ver si hay coincidencia. El término puede contener texto literal y operadores, tal como se describe en la [siguiente sección](#filter-reference-operators).

El filtrado por intención o entidad requiere una sintaxis ligeramente diferente de la del filtrado por otros campos. Para obtener más información, consulte [Filtrado por intención o entidad](#filter-reference-intent-entity).

**Nota:** En la sintaxis de la consulta de filtro se utilizan algunos caracteres que no están permitidos en consultas HTTP. Asegúrese de que todos los caracteres especiales, incluidos espacios y comillas, estén codificados en URL cuando se envíen como parte de una consulta HTTP. Por ejemplo, el filtro `response_timestamp<2016-11-01` se especificaría como
`response_timestamp%3C2016-11-01`.

## Operadores
{: #filter-reference-operators}

Puede utilizar los operadores siguientes en la consulta de filtro.

| Operador | Descripción |
|:-------------------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `:` | Operador de consulta de coincidencia aproximada. Utilice el prefijo `:` antes del término de consulta si desea que haya coincidencia con cualquier valor que contenga el término de consulta o una variante gramatical del mismo. La coincidencia aproximada está disponible para texto de entrada de usuario, texto de salida de respuesta y valores de entidad. |
| `::` | Operador de consulta de coincidencia exacta. Utilice el prefijo `::` antes del término de consulta si desea que haya coincidencia únicamente con los valores que sean exactamente iguales al término de consulta. |
| `:!` | Operador de consulta de coincidencia aproximada negativa. Utilice el prefijo `:!` antes del término de consulta si desea que haya coincidencia únicamente con los valores que _no_ contengan el término de consulta ni una variante gramatical del mismo. |
| `::!` | Operador de consulta de coincidencia exacta negativa. Utilice el prefijo `::!` antes del término de consulta si desea que haya coincidencia únicamente con los valores que _no_ coincidan exactamente con el término de consulta. |
| `<=`, `>=`, `>`, `<` | Operadores de comparación. Preceda el término de consulta con estos operadores para encontrar coincidencias basadas en una comparación aritmética. |
| `\` | Operador de escape. Utilícelo en consultas que incluyan caracteres de control que de lo contrario se analizarían como operadores. Por ejemplo, `\!hello` coincidiría con el texto `!hello`. |
| `""` | Frase literal. Se utiliza para especificar un término de consulta que contiene espacios u otros caracteres especiales. La API no analiza los caracteres especiales contenidos dentro de las comillas. |
| `~` | Coincidencia aproximada. Añada este operador seguido de un `1` o de un `2` al final del término de consulta para especificar el número permitido de diferencias de un solo carácter entre la serie de la consulta y una coincidencia en el objeto de respuesta. Por ejemplo, `car~1` coincidiría con `car`, `cat` o `cars`, pero no coincidiría con `cats`. Este operador no es válido cuando se realiza el filtrado sobre `ID_registro` o cualquier campo de fecha u hora, o con coincidencia aproximada. |
| `*` | Operador de carácter comodín. Coincide con cualquier secuencia de cero o más caracteres. Este operador no es válido cuando se realiza el filtrado sobre `ID_registro` o cualquier campo de fecha u hora. |
| `()`, `[]` | Operadores de agrupación. Utilícelo para especificar una agrupación lógica de varias expresiones mediante operadores booleanos. |
| `|` | Operador _or_ booleano. |
| `,` | Operador _and_ booleano. |

### Filtrado por intención o entidad
{: #filter-reference-intent-entity}

Debido a las diferencias en la forma en que las intenciones y las entidades se almacenan internamente, la sintaxis del filtrado sobre una determinada intención o entidad es diferente a la sintaxis utilizada para otros campos en el JSON devuelto. Para especificar un campo de `intención` o `entidad` dentro de un grupo de `intenciones` o `entidades`, debe utilizar el operador de comparación `:` en lugar de un punto.

Por ejemplo, esta consulta coincide con cualquier suceso registrado en la respuesta que incluya una intención detectada denominada `hello`:

`response.intents:intent::hello`

Observe que se ha especificado el operador `:` en lugar de un punto (intents:intent)

Utilice el mismo patrón para comparar según cualquier campo de una intención o entidad detectada en la respuesta. Por ejemplo, esta consulta coincide con cualquier suceso registrado en el que la respuesta incluya una entidad detectada con el valor `soda`:

`response.entities:value::soda`

Asimismo, puede filtrar según intenciones o entidades enviados como parte de la solicitud, como en este ejemplo:

`request.intents:intent::hello`

El filtrado según intenciones funciona en todas las intenciones detectadas. Para filtrar sólo según la intención detectada con la mayor confianza, puede utilizar la sintaxis abreviada `response.top_intent`. Por ejemplo:

`response.top_intent::goodbye`

### Filtrado por ID de cliente
{: #filter-reference-customer-id}

Para filtrar por ID de cliente, utilice la ubicación especial `customer_id`. (Para obtener más información sobre el etiquetado de mensajes con un ID de cliente, consulte [Seguridad de la información](/docs/services/assistant?topic=assistant-information-security)).

### Filtrado por otros campos
{: #filter-reference-fields}

Para filtrar por cualquier otro campo de los datos de registro, especifique la ubicación como una vía de acceso que identifique los niveles de objetos anidados en la respuesta JSON de la API /logs. Utilice puntos (`.`) para especificar niveles sucesivos de anidamiento en los datos JSON. Por ejemplo, la ubicación `request.input.text` identifica el campo de texto de entrada de usuario tal como se muestra en el siguiente fragmento de JSON:

```json
  "request": {
    "input": {
      "text": "Good morning"
    }
  }
```
<!-- {data-copy=false} -->

El filtrado no está disponible para todos los campos. Puede filtrar por los campos siguientes:

- request.context.metadata.deployment
- request.input.text
- response.entities
- response.input.text
- response.intents
- response.top_intent
- meta.message.entities_count

Actualmente no hay soporte para el filtrado por otros campos.

## Ejemplos
{: #filter-reference-examples}

En los ejemplos siguientes se ilustran distintos tipos de consultas que utilizan esta sintaxis.

| Descripción | Consulta |
|---------|-----------|
| La fecha de la respuesta está en el mes de julio de 2017. | `response_timestamp>=2017-07-01,response_timestamp<2017-08-01` |
| La indicación de fecha y hora de la respuesta es anterior a `2016-11-01T04:00:00.000Z`. | `response_timestamp<2016-11-01T04:00:00.000Z` |
| El mensaje está etiquetado con el ID de cliente `my_id`. | `customer_id::my_id` |
| El texto de entrada de usuario contiene la palabra "order" o una variante gramatical (por ejemplo, `orders` y `ordering`. | `request.input.text:order` |
| Un nombre de intención de la respuesta coincide exactamente con `place_order`. | `response.intents:intent::place_order` |
| Un nombre de entidad de la respuesta coincide exactamente con `beverage`.  | `response.entities:entity::beverage` |
| El texto de entrada de usuario no contiene la palabra "order" ni ninguna variante gramatical. | `request.input.text:!order` |
| El nombre de la intención detectada con la confianza más alta no coincide exactamente con `hello`. | `response.top_intent::!hello` |
| El texto de entrada de usuario contiene la serie `!hello`. | `request.input.text:\!hello` |
| El texto de entrada de usuario contiene la serie `IBM Watson`. | `request.input.text:"IBM Watson"` |
| El texto de entrada de usuario contiene una serie que no tiene más de 2 diferencias de un solo carácter con `Watson`. | `request.input.text:Watson~2` |
| El texto de entrada de usuario contiene una serie consistente en `comm`, seguido de cero o más caracteres adicionales, seguidos de `s`. | `request.input.text:comm*s` |
| El ID de despliegue del contexto coincide con `my_app`. | `request.context.metadata.deployment::my_app` |
| Una intención de la respuesta tiene un valor de confianza mayor que 0,8. | `response.intents:confidence>0.8` |
| Un nombre de intención de la respuesta coincide exactamente con `hello` o con `goodbye`. | `response.intents:intent::(hello|goodbye)` |
| Una intención de la respuesta tiene el nombre `hello` y un valor de confianza igual o mayor que 0,8. | `response.intents:(intent:hello,confidence>=0.8)` |
| Un nombre de intención de la respuesta coincide exactamente con `order`, y un nombre de entidad de la respuesta coincide exactamente con `beverage`. | `[response.intents:intent::order,response.entities:entity::beverage]` |
<!-- -->
