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

# Expresiones para acceder a objetos
{: #expression-language}

Puede escribir expresiones que accedan a objetos y propiedades de objetos mediante el lenguaje Spring Expression (SpEL). Para obtener más información, consulte el [Lenguaje Spring Expression (SpEL)![Icono de enlace externo](../../icons/launch-glyph.svg "Icono de enlace externo")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}.
{: shortdesc}

## Sintaxis de evaluación
{: #expression-language-long-syntax}

Para expandir valores de variables dentro de otras variables o para invocar a métodos en propiedades y objetos globales, utilice la sintaxis de expresión `<? expression ?>`. Por ejemplo:

- **Expansión de una propiedad**
    - `"output":{"text":"Your name is <? context.userName ?>"}`

- **Invocación de métodos en propiedades de objetos globales**
    - `"context":{"email": "<? @email.literal ?>"}`

## Sintaxis abreviada
{: #expression-language-shorthand-syntax}

Aprenda con rapidez a hacer referencia a los siguientes objetos utilizando la sintaxis abreviada SpEL:

- [Variables de contexto](#expression-language-shorthand-context)
- [Entidades](#expression-language-shorthand-entities)
- [Intenciones](#expression-language-shorthand-intents)

### Sintaxis abreviada de las variables de contexto
{: #expression-language-shorthand-context}

En la tabla siguiente se muestran ejemplos de la sintaxis abreviada que puede utilizar para escribir variables de contexto en expresiones de condición.

| Sintaxis abreviada           | Sintaxis completa en SpEL                     |
|----------------------------|-----------------------------------------|
| `$card_type`               | `context['card_type']`                  |
| `$(card-type)`             | `context['card-type']`                  |
| `$card_type:VISA`          | `context['card_type'] == 'VISA'`        |
| `$card_type:(MASTER CARD)` | `context['card_type'] == 'MASTER CARD'` |

Puede incluir caracteres especiales, como guiones o puntos, en los nombres de las variables de contexto. Sin embargo, hacerlo podría ocasionarle problemas al evaluar la expresión SpEL. Por ejemplo, el guión se podría interpretar como un signo menos. Para evitar este tipo de problemas, haga referencia a la variable utilizando la sintaxis de expresión completa o la sintaxis abreviada `$(variable-name)` y no utilice los siguientes caracteres especiales en el nombre:

- Paréntesis ()
- Más de un apóstrofo ''
- Comillas dobles "

### Sintaxis abreviada de las entidades
{: #expression-language-shorthand-entities}

En la tabla siguiente se muestran ejemplos de la sintaxis abreviada que puede utilizar cuando haga referencia a entidades.

| Sintaxis abreviada    | Sintaxis completa en SpEL                      |
|---------------------|------------------------------------------|
| `@year`             | `entities['year']?.value`                |
| `@year == 2016`     | `entities['year']?.value == 2016`        |
| `@year != 2016`     | `entities['year']?.value != 2016`        |
| `@city == 'Boston'` | `entities['city']?.value == 'Boston'`    |
| `@city:Boston`      | `entities['city']?.contains('Boston')`   |
| `@city:(New York)`  | `entities['city']?.contains('New York')` |

En SpEL, el signo de interrogación `(?)` impide que se active una excepción de puntero nulo cuando un objeto de entidad es nulo.

Si el valor de entidad que desea comprobar contiene un carácter `)`, no puede utilizar el operador `:` para la comparación.  Por ejemplo, si desea comprobar si la entidad city es `Dublin (Ohio)`, debe utilizar `@city == 'Dublin (Ohio)'` en lugar de `@city:(Dublin (Ohio))`.

### Sintaxis abreviada de las intenciones
{: #expression-language-shorthand-intents}

En la tabla siguiente se muestran ejemplos de la sintaxis abreviada que puede utilizar cuando haga referencia a intenciones.

<table>
  <caption>Sintaxis abreviada de intenciones</caption>
  <tr>
    <th>Sintaxis abreviada</th>
    <th>Sintaxis completa en SpEL</th>
  </tr>
  <tr>
    <td>`#help`</td>
    <td>`intent == 'help'`</td>
  </tr>
  <tr>
    <td>`! #help`</td>
    <td>`intent != 'help'`</td>
  </tr>
  <tr>
    <td>`NOT #help`</td>
    <td>`intent != 'help'`</td>
  </tr>
  <tr>
    <td>`#help` or `#i_am_lost`</td>
    <td>`(intent == 'help' || intent == 'I_am_lost')`</td>
  </tr>
</table>

## Variables globales incorporadas
{: #expression-language-builtin-vars}

Utilice el lenguaje de expresiones para extraer información de propiedades para las siguientes variables globales:

| Variable global      | Definición |
|----------------------|------------|
| *context*            | Parte del objeto JSON del mensaje de la conversación procesada. |
| *entities[ ]*        | Lista de entidades que da soporte al acceso predeterminado al primer elemento. |
| *input*              | Parte del objeto JSON del mensaje de la conversación procesada. |
| *intents[ ]*         | Lista de intenciones da soporte al acceso predeterminado al primer elemento. |
| *output*             | Parte del objeto JSON del mensaje de la conversación procesada. |

## Acceso a entidades
{: #expression-language-access-entity}

La matriz de entidades contiene una o varias entidades reconocidas en la entrada del usuario.

Cuando se prueba el diálogo, puede ver los detalles de las entidades que se reconocen en la entrada del usuario especificando esta expresión en una respuesta del nodo del diálogo:

```json
<? entities ?>
```
{: codeblock}

Para la entrada de usuario, *Hello now*, el servicio reconoce las entidades @sys-date y @sys-time, de modo que la respuesta contiene estos objetos de entidad:

```json
[
{"entity":"sys-date","location":[6,9],"value":"2017-08-07",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}},
{"entity":"sys-time","location":[6,9],"value":"15:01:00",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}}
]
```
{: codeblock}

### Cuando es importante colocar entidades en la entrada
{: #expression-language-placement-matters}

Cuando utilice la expresión abreviada, `@city.contains('Boston')`, en una condición, el nodo de diálogo devuelve true **únicamente si** `Boston` es la primera entidad detectada en la entrada del usuario. Utilice esta sintaxis solo si la ubicación de las entidades en la entrada es importante para usted y solo desea comprobar la primera mención.

Utilice la expresión completa de SpEL si desea que la condición devuelva true cada vez que se mencione el término en la entrada del usuario, independientemente del orden en el que se mencionen las entidades. La condición `entities['city']?.contains('Boston')` devuelve true cuando se encuentra al menos una entidad city 'Boston' en todas las entidades @city, independientemente de su colocación.

Por ejemplo, supongamos que un usuario envía `"I want to go from Toronto to Boston."` Las entidades `@city:Toronto` y `@city:Boston` se detectan y se representan en la matriz devuelva del siguiente modo:

- `entities.city[0].value = 'Toronto'`
- `entities.city[1].value = 'Boston'`

El orden de las entidades de la matriz que se devuelve coincide con el orden en el que se mencionan en la entrada del usuario.
{: note}

### Propiedades de una entidad
{: #expression-language-entity-props}

Cada entidad tiene un conjunto de propiedades asociadas. Puede acceder a información sobre una entidad a través de sus propiedades.

| Propiedad              | Definición | Consejos de uso |
|-----------------------|------------|------------|
| *confidence*          | Un porcentaje decimal que representa la confianza del servicio en la entidad reconocida. La confianza de una entidad puede ser 0 o 1, a no ser que haya activado la coincidencia aproximada de entidades. Cuando la coincidencia aproximada está habilitada, el umbral de nivel de confianza predeterminado es 0.3. Tanto si la coincidencia aproximada está habilitada como si no, las entidades del sistema siempre tienen el nivel de confianza 1.0. | Puede utilizar esta propiedad en una condición para que devuelva false si el nivel de confianza no es superior al porcentaje que especifique. |
| *location*            | Un desplazamiento de carácter basado en cero que indica dónde empiezan y terminan los valores de entidad detectados en el texto de entrada. | Utilice `.literal` para extraer la parte de texto comprendida entre los valores de índice de inicio y fin almacenados en la propiedad location. |
| *value*               | La entidad value identificada en la entrada. | Esta propiedad devuelve el valor de entidad tal como está definido en los datos de entrenamiento, aunque la comparación se haya realizado sobre uno de los sinónimos asociados. Puede utilizar `.values` para capturar varias apariciones de una entidad que pueda estar presente en la entrada de usuario. |

### Ejemplos de uso de propiedades de entidades
{: #expression-language-entity-props-example}

En los ejemplos siguientes, el conocimiento contiene una entidad airport que incluye el valor JFK y el sinónimo "Kennedy Airport". La entrada del usuario es *I want to go to Kennedy Aiport*.

- Para devolver una respuesta específica si se reconoce la entidad 'JFK' en la entrada del usuario, podría añadir esta expresión a la condición de respuesta: `entities.airport[0].value == 'JFK'`
  o
  `@airport = "JFK"`
- Para devolver el nombre de la entidad tal como la ha especificado el usuario en la respuesta del diálogo, utilice la propiedad .literal: `So you want to go to <?entities.airport[0].literal?>...`
  o
  `So you want to go to @airport.literal ...`

  Ambos formatos se evalúan como `So you want to go to Kennedy Airport...' en la respuesta.

- Las expresiones como `@airport:(JFK)` o `@airport.contains('JFK')` siempre hacen referencia al **valor** de la entidad (`JFK` en este ejemplo).
- Para ser más restrictivo sobre los términos identificados como aeropuertos en la entrada cuando la coincidencia aproximada está habilitada, puede especificar esta expresión en una condición de nodo, por ejemplo: `@airport && @airport.confidence > 0.7`. El nodo solo se ejecutará si el servicio tiene una confianza del 70% en que el texto de entrada contiene una referencia a airport.

En este ejemplo, la entrada del usuario es *Are there places to exchange currency at JFK, Logan, and O'Hare?*

- Para capturar varias apariciones de un tipo de entidad en la entrada del usuario, utilice una sintaxis como la siguiente:

    ```json
    "context":{
      "airports":"@airport.values"
    }
    ```

  Para hacer referencia posteriormente a la lista capturada de una respuesta de diálogo, utilice esta sintaxis: `You asked about these airports: <? $airports.join(', ') ?>.`
  Se muestra del siguiente modo:
  `You asked about these airports: JFK, Logan, O'Hare.`

## Acceso a intenciones
{: #expression-language-intent}

La matriz de intenciones contiene una o varias intenciones que se han reconocido en la entrada del usuario, clasificadas en orden descendente de confianza.

Cada intención tiene una única propiedad: la propiedad `confidence`. La propiedad confidence es un porcentaje decimal que representa la confianza del servicio en la intención reconocida.

Cuando se prueba el diálogo, puede ver los detalles de las intenciones que se reconocen en la entrada del usuario especificando esta expresión en una respuesta del nodo del diálogo:

```json
<? intents ?>
```
{: codeblock}

Para la entrada de usuario, *Hello now*, el servicio encuentra una coincidencia exacta con la intención #greeting. Por lo tanto, lista en primer lugar los detalles del objeto de intención #greeting. La respuesta también incluye las otras 10 primeras intenciones definidas en el conocimiento independientemente de su puntuación de confianza. (En este ejemplo, su confianza en las otras intenciones se establece en 0 porque la primera intención es una coincidencia exacta). Se devuelven las otras 10 primeras intenciones porque el panel "Pruébelo" envía el parámetro `alternate_intents:true` con su solicitud. Si está utilizando directamente la API y desea ver los primeros 10 resultados, asegúrese de especificar este parámetro en su llamada. Si `alternate_intents` es false, el valor predeterminado, únicamente se devolverán en la matriz las intenciones con una confianza por encima de 0,2.

```json
[{"intent":"greeting","confidence":1},
{"intent":"yes","confidence":0},
{"intent":"pizza-order","confidence":0}]
```
{: codeblock}

En los ejemplos siguientes se muestra cómo comprobar el valor de una intención:

- `intents[0] == 'Help'`
- `intent == 'Help'`

`intent == 'help'` difiere de `intents[0] == 'help'` porque `intent == 'help'` no genera una excepción si no se detecta ninguna intención. Solo se evalúa como true si la confianza de la intención supera un umbral.  Si lo desea, puede especificar un nivel de confianza personalizado para una condición, por ejemplo `intents.size() > 0 && intents[0] == 'help' && intents[0].confidence > 0.1`

## Acceso de la entrada
{: #expression-language-intent-props}

El objeto JSON de entrada solo contiene una propiedad: la propiedad text. La propiedad text representa el texto de la entrada del usuario.

### Ejemplos de uso de propiedades de entrada
{: #expression-language-intent-props-example}

En el ejemplo siguiente se muestra cómo acceder a la entrada:

- Para ejecutar un nodo si la entrada del usuario es "Yes", añada esta expresión a la condición del nodo: `input.text == 'Yes'`

Puede utilizar cualquiera de los [métodos String](/docs/services/conversation/dialog-methods#dialog-methods-strings) para evaluar o manipular texto de la entrada del usuario. Por ejemplo:

- Para comprobar si la entrada del usuario contiene "Yes", utilice: `input.text.contains( 'Yes' )`.
- Devuelve true si la entrada del usuario es un número: `input.text.matches( '[0-9]+' )`.
- Para comprobar si la serie de entrada contiene diez caracteres, utilice `input.text.length() == 10`.
