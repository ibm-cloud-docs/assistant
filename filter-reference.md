---

copyright:
  years: 2015, 2020
lastupdated: "2020-08-12"

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

# Filter query reference
{: #filter-reference}

The {{site.data.keyword.conversationshort}} service REST API offers powerful log search capabilities through filter queries. You can use the v2 /logs API `filter` parameter to search your skill log for events that match a specified query.

The `filter` parameter is a cacheable query that limits the results to those matching the specified filter. You can filter on various objects that are part of the JSON response model (for example, the user input text, the detected intents and entities, or the confidence score).

To see examples of filter queries, see [Examples](#filter-reference-examples).

For more information about the /logs `GET` method and its response model, refer to the [API Reference](https://cloud.ibm.com/apidocs/assistant/assistant-v2?code=dotnet-standard#list-log-events-for-an-assistant){: external}.

## Filter query syntax
{: #filter-reference-syntax}

The following example shows the general form of a filter query:

| Location           | Query operator | Term         |
|:------------------:|:--------------:|:------------:|
|`request.input.text`|`::`            |`"IBM Watson"`|

- The _location_ identifies the field that you want to filter on (in this example, `request.input.text`).
- The _query operator_, which specifies the type of matching you want to use (fuzzy matching or exact matching).
- The _term_ specifies the expression or value you want to use to evaluate the field for matching. The term can contain literal text and operators, as described in the [next section](#filter-reference-operators).

Filtering by intent or entity requires slightly different syntax from filtering on other fields. For more information, see [Filtering by intent or entity](#filter-reference-intent-entity).

**Note:** The filter query syntax uses some characters that are not allowed in HTTP queries. Make sure that all special characters, including spaces and quotation marks, are URL encoded when sent as part of an HTTP query. For example, the filter `response_timestamp<2020-01-01` would be specified as `response_timestamp%3C2020-01-01`.

## Operators
{: #filter-reference-operators}

You can use the following operators in your filter query.

| Operator | Description |
|:-------------------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `:` | Fuzzy match query operator. Prefix the query term with `:` if you want to match any value that contains the query term, or a grammatical variant of the query term. Fuzzy matching is available for user input text, response output text, and entity values. |
| `::` | Exact match query operator. Prefix the query term with `::` if you want to match only values that exactly equal the query term. |
| `:!` | Negative fuzzy match query operator. Prefix the query term with `:!` if you want to match only values that do _not_ contain the query term or a grammatical variant of the query term. |
| `::!` | Negative exact match query operator. Prefix the query term with `::!` if you want to match only values that do _not_ exactly match the query term. |
| `<=`, `>=`, `>`, `<` | Comparison operators. Prefix the query term with these operators to match based on arithmetic comparison. |
| `\` | Escape operator. Use in queries that include control characters that would otherwise be parsed as operators. For example, `\!hello` would match the text `!hello`. |
| `""` | Literal phrase. Use to encloses a query term that contains spaces or other special characters. No special characters within the quotation marks are parsed by the API. |
| `~` | Approximate match. Append this operator followed by a `1` or `2` to the end of the query term to specify the allowed number of single-character differences between the query string and a match in the response object. For example, `car~1` would match `car`, `cat`, or `cars`, but it would not match `cats`. This operator is not valid when filtering on `log_id` or any date or time field, or with fuzzy matching. |
| `*` | Wildcard operator. Matches any sequence of zero or more characters. This operator is not valid when filtering on `log_id` or any date or time field. |
| `()`, `[]` | Grouping operators. Use to enclose a logical grouping of multiple expressions using Boolean operators. |
| <code>&#124;</code> | Boolean _or_ operator. |
| `,` | Boolean _and_ operator. |

### Filtering by intent or entity
{: #filter-reference-intent-entity}

Because of differences in how intents and entities are stored internally, the syntax for filtering on a specific intent or entity is different from the syntax used for other fields in the returned JSON. To specify an `intent` or `entity` field within an `intents` or `entities` collection, you must use the `:` match operator instead of a dot.

For example, this query matches any logged event where the response includes a detected intent named `hello`:

`response.output.intents:intent::hello`

Note the `:` operator in place of a dot (intents:intent)

Use the same pattern to match on any field of a detected intent or entity in the response. For example, this query matches any logged event where the response includes a detected entity with the value `soda`:

`response.output.entities:value::soda`

Similarly, you can filter on intents or entities sent as part of the request, as in this example:

`request.input.intents:intent::hello`

<!--Filtering on intents operates on all detected intents. To filter only on the detected intent with the highest confidence, you can use the `response.top_intent` shorthand syntax. For example:

`response.top_intent::goodbye`
-->
### Filtering by other fields
{: #filter-reference-fields}

To filter on another field in the log data, specify the location as a path identifying the levels of nested objects in the JSON response from the /logs API. Use dots (`.`) to specify successive levels of nesting in the JSON data. For example, the location `request.input.text` idenfities the user input text field as shown in the following JSON fragment:

```json
  "request": {
    "input": {
      "text": "Good morning"
    }
  }
```
<!-- {data-copy=false} -->

Filtering is not available for all fields. You can filter on the following fields:

- `assistant_id`
- `customer_id`
- `language`
- `request.context.global.system.user_id`
- `request.input.text`
- `request_timestamp`
- `response.context.global.system.user_id`
- `response.output.entities`
- `response.output.intents`
- `response_timestamp`
- `skill_id`
- `snapshot`

Filtering on other fields is not currently supported.

## Examples
{: #filter-reference-examples}

The following examples illustrate various types of queries using this syntax.

| Description | Query |
|---------|-----------|
| The date of the response is in the month of July 2020. | `response_timestamp>=2020-07-01,response_timestamp<2020-08-01` |
| The timestamp of the response is earlier than `2019-11-01T04:00:00.000Z`. | `response_timestamp<2019-11-01T04:00:00.000Z` |
| The message is labeled with the customer ID `my_id`. | `customer_id::my_id` |
| The message was sent to a specific assistant. | `assistant_id::dcd5c5ad-f3a1-4345-89c5-708b0b5ff4f7` |
| The user input text contains the word "order" or a grammatical variant (for example, `orders` or `ordering`. | `request.input.text:order` |
| An intent name in the response exactly matches `place_order`. | `response.output.intents:intent::place_order` |
| An entity name in the response exactly matches `beverage`.  | `response.output.entities:entity::beverage` |
| The user input text does not contain the word "order" or a grammatical variant. | `request.input.text:!order` |
<!--| The name of the detected intent with the highest confidence does not exactly match `hello`. | `response.top_intent::!hello` |-->
| The user input text contains the string `!hello`. | `request.input.text:\!hello` |
| The user input text contains the string `IBM Watson`. | `request.input.text:"IBM Watson"` |
| The user input text contains a string that has no more than 2 single-character differences from `Watson`. | `request.input.text:Watson~2` |
| The user input text contains a string consisting of `comm`, followed by zero or more additional characters, followed by `s`. | `request.input.text:comm*s` |
| The user input text is not empty. | `request.input.text::!""` |
| An intent name in the response exactly matches either `hello` or `goodbye`. | <code>response.output.intents:intent::(hello&#124;goodbye)</code> |
| An intent name in the response exactly matches `order`, and an entity name in the response exactly matches `beverage`. | `[response.output.intents:intent::order,response.output.entities:entity::beverage]` |

## Filtering v1 logs

If your application is still using the v1 API, you can query and filter logs using the v1 /logs method. The filtering syntax is the same, but the structure of v1 logs and message requests is different. For more information, see [API Reference](https://cloud.ibm.com/apidocs/assistant/assistant-v1#list-log-events-in-a-workspace){: external}.

With the v1 /logs API, you can filter on the following fields:

- `request.context.metadata.deployment`
- `request.context.system.assistant_id`
- `request.input.text`
- `response.entities`
- `response.input.text`
- `response.intents`
- `response.top_intent`
- `meta.message.entities_count`
- `workspace_id`

Filtering on other fields is not currently supported.
