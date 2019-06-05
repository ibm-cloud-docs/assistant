---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-04"

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

# Expressions for accessing objects
{: #expression-language}

You can write expressions that access objects and properties of objects by using the Spring Expression (SpEL) language. For more information, see [Spring Expression Language (SpEL) ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}.
{: shortdesc}

## Evaluation syntax
{: #expression-language-long-syntax}

To expand variable values inside other variables or invoke methods on properties and global objects, use the `<? expression ?>` expression syntax. For example:

- **Expanding a property**
    - `"output":{"text":"Your name is <? context.userName ?>"}`

- **Invoking methods on properties of global objects**
    - `"context":{"email": "<? @email.literal ?>"}`

## Shorthand syntax
{: #expression-language-shorthand-syntax}

Learn how to quickly reference the following objects by using the SpEL shorthand syntax:

- [Context variables](#expression-language-shorthand-context)
- [Entities](#expression-language-shorthand-entities)
- [Intents](#expression-language-shorthand-intents)

### Shorthand syntax for context variables
{: #expression-language-shorthand-context}

The following table shows examples of the shorthand syntax that you can use to write context variables in condition expressions.

| Shorthand syntax           | Full syntax in SpEL                     |
|----------------------------|-----------------------------------------|
| `$card_type`               | `context['card_type']`                  |
| `$(card-type)`             | `context['card-type']`                  |
| `$card_type:VISA`          | `context['card_type'] == 'VISA'`        |
| `$card_type:(MASTER CARD)` | `context['card_type'] == 'MASTER CARD'` |

You can include special characters, such as hyphens or periods, in context variable names. However, doing so can lead to problems when the SpEL expression is evaluated. The hyphen could be interpreted as a minus sign, for example. To avoid such problems, reference the variable by using either the full expression syntax or the shorthand syntax `$(variable-name)` and do not use the following special characters in the name:

- Parentheses ()
- More than one apostrophe ''
- Quotation marks "

### Shorthand syntax for entities
{: #expression-language-shorthand-entities}

The following table shows examples of the shorthand syntax that you can use when referring to entities.

| Shorthand syntax    | Full syntax in SpEL                      |
|---------------------|------------------------------------------|
| `@year`             | `entities['year']?.value`                |
| `@year == 2016`     | `entities['year']?.value == 2016`        |
| `@year != 2016`     | `entities['year']?.value != 2016`        |
| `@city == 'Boston'` | `entities['city']?.value == 'Boston'`    |
| `@city:Boston`      | `entities['city']?.contains('Boston')`   |
| `@city:(New York)`  | `entities['city']?.contains('New York')` |

In SpEL, the question mark `(?)` prevents a null pointer exception from being triggered when an entity object is null.

If the entity value that you want to check for contains a `)` character, you cannot use the `:` operator for comparison.  For example, if you want to check whether the city entity is `Dublin (Ohio)`, you must use `@city == 'Dublin (Ohio)'` instead of `@city:(Dublin (Ohio))`.

### Shorthand syntax for intents
{: #expression-language-shorthand-intents}

The following table shows examples of the shorthand syntax that you can use when referring to intents.

<table>
  <caption>Intents shorthand syntax</caption>
  <tr>
    <th>Shorthand syntax</th>
    <th>Full syntax in SpEL</th>
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

## Built-in global variables
{: #expression-language-builtin-vars}

You can use the expression language to extract property information for the following global variables:

| Global variable      | Definition |
|----------------------|------------|
| *context*            | JSON object part of the processed conversation message. |
| *entities[ ]*        | List of entities that supports default access to 1st element. |
| *input*              | JSON object part of the processed conversation message. |
| *intents[ ]*         | List of intents that supports default access to first element. |
| *output*             | JSON object part of the processed conversation message. |

## Accessing entities
{: #expression-language-access-entity}

The entities array contains one or more entities that were recognized in user input.

While testing your dialog, you can see details of the entities that are recognized in user input by specifying this expression in a dialog node response:

```json
<? entities ?>
```
{: codeblock}

For the user input, *Hello now*, your assistant recognizes the @sys-date and @sys-time system entities, so the response contains these entity objects:

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

### When placement of entities in the input matters
{: #expression-language-placement-matters}

When you use the shorthand expression, `@city.contains('Boston')`, in a condition, the dialog node returns true **only if** `Boston` is the first entity detected in the user input. Only use this syntax if the placement of entities in the input matters to you and you want to check the first mention only.

Use the full SpEL expression if you want the condition to return true any time the term is mentioned in the user input, regardless of the order in which the entities are mentioned. The condition, `entities['city']?.contains('Boston')` returns true when at least one 'Boston' city entity is found in all the @city entities, regardless of placement.

For example, a user submits `"I want to go from Toronto to Boston."` The `@city:Toronto` and `@city:Boston` entities are detected and are represented in the array that is returned as follows:

- `entities.city[0].value = 'Toronto'`
- `entities.city[1].value = 'Boston'`

The order of entities in the array that is returned matches the order in which they are mentioned in the user input.
{: note}

### Entity properties
{: #expression-language-entity-props}

Each entity has a set of properties associated with it. You can access information about an entity through its properties.

| Property              | Definition | Usage tips |
|-----------------------|------------|------------|
| *confidence*          | A decimal percentage that represents your assistant's confidence in the recognized entity. The confidence of an entity is either 0 or 1, unless you have activated fuzzy matching of entities. When fuzzy matching is enabled, the default confidence level threshold is 0.3. Whether or not fuzzy matching is enabled, system entities always have a confidence level of 1.0. | You can use this property in a condition to have it return false if the confidence level is not higher than a percent you specify. |
| *location*            | A zero-based character offsets that indicates where the detected entity values begin and end in the input text. | Use `.literal` to extract the span of text between start and end index values that are stored in the location property. |
| *value*               | The entity value identified in the input. | This property returns the entity value as defined in the training data, even if the match was made against one of its associated synonyms. You can use `.values` to capture multiple occurrences of an entity that might be present in user input. |

### Entity property usage examples
{: #expression-language-entity-props-example}

In the following examples, the skill contains an airport entity that includes a value of JFK, and the synonym 'Kennedy Airport". The user input is *I want to go to Kennedy Aiport*.

- To return a specific response if the 'JFK' entity is recognized in the user input, you could add this expression to the response condition:
  `entities.airport[0].value == 'JFK'`
  or
  `@airport = "JFK"`
- To return the entity name as it was specified by the user in the dialog response, use the .literal property:
  `So you want to go to <?entities.airport[0].literal?>...`
  or
  `So you want to go to @airport.literal ...`

  Both formats evaluate to `So you want to go to Kennedy Airport...' in the response.

- Expressions like `@airport:(JFK)` or `@airport.contains('JFK')` always refer to the **value** of the entity (`JFK` in this example).
- To be more restrictive about which terms are identified as airports in the input when fuzzy matching is enabled, you can specify this expression in a node condition, for example: `@airport && @airport.confidence > 0.7`. The node will only execute if your assistant is 70% confident that the input text contains an airport reference.

In this example, the user input is *Are there places to exchange currency at JFK, Logan, and O'Hare?*

- To capture multiple occurrences of an entity type in user input, use syntax like this:

    ```json
    "context":{
      "airports":"@airport.values"
    }
    ```

  To later refer to the captured list in a dialog response, use this syntax:
  `You asked about these airports: <? $airports.join(', ') ?>.`
  It is displayed like this:
  `You asked about these airports: JFK, Logan, O'Hare.`

## Accessing intents
{: #expression-language-intent}

The intents array contains one or more intents that were recognized in the user input, sorted in descending order of confidence.

Each intent has one property only: the `confidence` property. The confidence property is a decimal percentage that represents your assistant's confidence in the recognized intent.

While testing your dialog, you can see details of the intents that are recognized in user input by specifying this expression in a dialog node response:

```json
<? intents ?>
```
{: codeblock}

For the user input, *Hello now*, your assistant finds an exact match with the #greeting intent. Therefore, it lists the #greeting intent object details first. The response also includes the top 10 other intents that are defined in the skill regardless of their confidence score. (In this example, its confidence in the other intents is set to 0 because the first intent is an exact match.) The top 10 intents are returned because the "Try it out" pane sends the `alternate_intents:true` parameter with its request. If you are using the API directly and want to see the top 10 results, be sure to specify this parameter in your call. If `alternate_intents` is false, which is the default value, only intents with a confidence above 0.2 are returned in the array.

```json
[{"intent":"greeting","confidence":1},
{"intent":"yes","confidence":0},
{"intent":"pizza-order","confidence":0}]
```
{: codeblock}

The following examples show how to check for an intent value:

- `intents[0] == 'Help'`
- `intent == 'Help'`

`intent == 'help'` differs from `intents[0] == 'help'` because `intent == 'help'` does not throw an exception if no intent is detected. It is evaluated as true only if the intent confidence exceeds a threshold.  If you want to, you can specify a custom confidence level for a condition, for example, `intents.size() > 0 && intents[0] == 'help' && intents[0].confidence > 0.1`

## Accessing input
{: #expression-language-intent-props}

The input JSON object contains one property only: the text property. The text property represents the text of the user input.

### Input property usage examples
{: #expression-language-intent-props-example}

The following example shows how to access input:

- To execute a node if the user input is "Yes", add this expression to the node condition:
  `input.text == 'Yes'`

You can use any of the [String methods](/docs/services/assistant/dialog-methods#dialog-methods-strings) to evaluate or manipulate text from the user input. For example:

- To check whether the user input contains "Yes", use: `input.text.contains( 'Yes' )`.
- Returns true if the user input is a number: `input.text.matches( '[0-9]+' )`.
- To check whether the input string contains ten characters, use: `input.text.length() == 10`.
