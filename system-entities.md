---

copyright:
  years: 2015, 2020
lastupdated: "2020-02-14"

keywords: system entity, sys-number, sys-date, sys-time

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

# System entity details
{: #system-entities}

Learn about system entities that are provided by IBM for you to use out of the box. These built-in utility entities help your assistant recognize terms and references that are commonly used by customers in conversation, such as numbers and dates. 
{: shortdesc}

A new and improved version of the numeric system entities is generally available. For more details, see [New system entities](/docs/assistant?topic=assistant-beta-system-entities).

For information about language support for system entities, see [Supported languages](/docs/assistant?topic=assistant-language-support).

For more information about how to use system entities from dialog, see [Creating entities](/docs/assistant?topic=assistant-entities#entities-enable-system-entities).

## @sys-currency entity
{: #system-entities-sys-currency}

The @sys-currency system entity detects monetary currency values that are expressed in an utterance with a currency symbol or currency-specific terms. A numeric value is returned.

### Recognized formats
{: #system-entities-sys-currency-formats}

- 20 cents
- Five dollars
- $10

### Metadata
{: #system-entities-sys-currency-metadata}

- `.numeric_value`: the canonical numeric value as an integer or a double, in base units
- `.unit`: the base unit currency code (for example, 'USD' or 'EUR')

### Returns
{: #system-entities-sys-currency-returns}

For the input `twenty dollars` or `$1,234.56`, @sys-currency returns these values:

| Attribute                   | Type   | Returned for `twenty dollars` | Returned for `$1,234.56` |
|-----------------------------|--------|-------------------------------|-------------------------:|
| @sys-currency               | string | 20                            |                  1234.56 |
| @sys-currency.literal       | string | twenty dollars                |                $1,234.56 |
| @sys-currency.numeric_value | number | 20                            |                  1234.56 |
| @sys-currency.location      | array  | [0,14]                        |                    [0,9] |
| @sys-currency.unit          | string | USD*                          |                      USD |
{: caption="@sys-currency examples" caption-side="top"}

*@sys-currency.unit always returns the 3-letter ISO currency code.

For the input `veinte euro` or <code>&euro;1.234,56</code>, in Spanish, @sys-currency returns these values:

| Attribute                   | Type   | Returned for  `veinte euro` | Returned for <code>&euro;1.234,56</code> |
|-----------------------------|--------|-----------------------------|-------------------------:|
| @sys-currency               | string | 20                          |                  1234.56 |
| @sys-currency.literal       | string | veinte euro                 |                &euro;1.234,56 |
| @sys-currency.numeric_value | number | 20                          |                  1234.56 |
| @sys-currency.location      | array  |[0,11]                       |                     [0,9]|
| @sys-currency.unit          | string | EUR*                        |                     EUR  |
{: caption="More @sys-currency examples" caption-side="top"}

*@sys-currency.unit always returns the 3-letter ISO currency code.

You get equivalent results for other supported languages and national currencies.

### @system-currency usage tips
{: #system-entities-currencty-usage-tips}

- Currency values are recognized as instances of @sys-number entities as well. If you are using separate conditions to check for both currency values and numbers, place the condition that checks for currency above the one that checks for a number.

  This workaround is not necessary if you are using the revised system entities. For more details, see [New system entities](/docs/assistant?topic=assistant-beta-system-entities).
  {: note}

- If you use the @sys-currency entity as a node condition and the user specifies `$0` as the value, the value is recognized as a currency properly, but the condition is evaluated to the number zero, not the currency zero. As a result, the `null` in the condition is evaluated to false and the node is not processed. To check for currency values in a way that handles zeros properly, use expression `@sys-currency >=0` in the node condition instead.

## @sys-date and @sys-time entities
{: #system-entities-sys-date-time}

The `@sys-date` system entity extracts mentions such as `Friday`, `today`, or `November 1`. The value of this entity stores the corresponding inferred date as a string in the format "yyyy-MM-dd" e.g. "2016-11-21". The system augments missing elements of a date (such as the year for "November 21") with the current date values.

For English locale only, the default system behavior for date input is MM/DD/YYYY. This will change to DD/MM/YYYY only if the first two numbers are greater than 12. The value stored will still be in the format "yyyy-MM-dd".
{: note}

The `@sys-time` system entity extracts mentions such as `2pm`, `at 4`, or `15:30`. The value of this entity stores the time as a string in the format "HH:mm:ss". For example, "13:00:00."

### Date-time mentions
{: #system-entities-date-time-mentions}

Mentions of date and time, such as `now` or `two hours from now` are extracted as two separate entity mentions - one `@sys-date` and one `@sys-time`. These two mentions are not linked to one another except that they share the same literal string that spans the complete date-time mention.

Multi-word date-time mentions such as `on Monday at 4pm` are also extracted as two @sys-date and @sys-time mentions. When mentioned together consecutively they also share a single literal string that spans the complete date-time mention.

### Date and time ranges
{: #system-entities-date-time-ranges}

Mentions of a date range such as `the weekend`, `next week`, or `from Monday to Friday` are extracted as a pair of `@sys-date` entity mentions that show the start and end of the range. Similarly, mentions of time ranges such as `from 2 to 3` are extracted as two `@sys-time` entities, showing the start and end times. The two entities in the pair share a literal string that corresponds to the full date or time range mention.

### `Last` and `Next` dates and times
{: #system-entities-last-next}

In some locales, a phrase like "last Monday" is used to specify the Monday of the previous week only. In contrast, other locales use "last Monday" to specify the last day which was a Monday, but which may have been either in the same week or the previous week.

As an example, for Friday June 16, in some locales "last Monday" could refer to either June 12 or to June 5, while in other locales it refers only to June 5 (the previous week). This same logic holds true for a phrase like "next Monday".

The {{site.data.keyword.conversationshort}} service treats "last" and "next" dates as referring to the most immediate last or next day referenced, which may be in either the same or a previous week.

For time phrases like "for the last 3 days" or "in the next 4 hours", the logic is equivalent. For example, in the case of "in the next 4 hours", this results in two `@sys-time` entities: one of the current time, and one of the time four hours later than the current time.

### Time zones
{: #system-entities-time-zones}

Mentions of a date or time that are relative to the current time are resolved with respect to a chosen time zone. By default, this is UTC (GMT). This means that by default, REST API clients located in time zones different from UTC will observe the value of `now` extracted according to the current UTC time.

Optionally, the REST API client can add the local timezone as the context variable `$timezone`. This context variable should be sent with every client request. For example, the `$timezone` value should be `America/Los_Angeles`, `EST`, or `UTC`. For a full list of supported time zones, see [Supported time zones](/docs/assistant?topic=assistant-time-zones).

When the `$timezone` variable is provided, the values of relative @sys-date and @sys-time mentions are computed based on the client time zone instead of UTC.

#### Examples of mentions relative to time zones
{: #system-entities-time-zone-examples}

- now
- in two hours
- today
- tomorrow
- 2 days from now

### Recognized formats
{: #system-entities-sys-date-time-formats}

- November 21
- 10:30
- at 6 pm
- this weekend

### Returns
{: #system-entities-sys-date-time-returns}

For the input `November 21` @sys-date returns these values:

| Attribute               | Type   | Returned for `November 21` |
|-------------------------|--------|---------------------------:|
| @sys-date.literal       | string |                November 21 |
| @sys-date               | string |                20xx-11-21 *|
| @sys-date.location      | array |                     [0,11]  |
| @sys-date.calendar_type | string |                  GREGORIAN |
{: caption="@sys-date examples" caption-side="top"}

- @sys-date always returns the date in this format: yyyy-MM-dd.
- \* Returns the next matching date. If that date has already passed this year, this returns next year's date.

For the input `at 6 pm` @sys-time returns these values:

| Attribute               | Type   | Returned for `at 6 pm` |
|-------------------------|--------|-----------------------:|
| @sys-time.literal       | string |                at 6 pm |
| @sys-time               | string |               18:00:00 |
| @sys-time.location      | array |                   [0,7]|
| @sys-time.calendar_type | string |              GREGORIAN |
{: caption="More @sys-date examples" caption-side="top"}

- @sys-time always returns the time in this format: HH:mm:ss.

For information about processing date and time values, see the [Date and time method reference](/docs/assistant?topic=assistant-dialog-methods#dialog-methods-date-time).
{: tip}

## @sys-location entity  ![Beta feature](images/beta.png)
{: #system-entities-sys-location}

Available as a beta feature for only languages noted in the [Supported languages](/docs/assistant?topic=assistant-language-support) topic. A powerful alternative to using this system entity is to use a contextual entity for identifying proper nouns, such as locations. For more information, see [Annotation-based method](https://cloud.ibm.com/docs/assistant?topic=assistant-entities#entities-annotations-overview).
{: tip}

The @sys-location system entity extracts place names (country, state/province, city, town, etc.) from the user's input.

### Recognized formats
{: #system-entities-sys-location-formats}

- Boston
- U.S.A.
- New South Wales

For information about processing String values, see the [Strings method reference](/docs/assistant?topic=assistant-dialog-methods#dialog-methods-strings).
{: tip}

## @sys-number entity
{: #system-entities-sys-number}

The @sys-number system entity detects numbers that are written using either numerals or words. In either case, a numeric value is returned.

### Recognized formats
{: #system-entities-sys-number-formats}

- 21
- twenty one
- 3.13

### Metadata
{: #system-entities-sys-number-metadata}

- `.numeric_value` - the canonical numeric value as an integer or a double

### Returns
{: #system-entities-sys-number-returns}

For the input `twenty` or `1,234.56`, @sys-number returns these values:

| Attribute                   | Type   | Returned for `twenty` | Returned for `1,234.56` |
|-----------------------------|--------|-------------------|------------------------:|
| @sys-number               | string | 20                |                 1234.56 |
| @sys-number.literal       | string | twenty            |                1,234.56 |
| @sys-number.location      | array |  [0,6]             |                    [0,8]|
| @sys-number.numeric_value | number | 20                |                 1234.56 |
{: caption="@sys-number examples" caption-side="top"}

For the input `veinte` or `1.234,56`, in Spanish, @sys-number returns these values:

| Attribute                   | Type   | Returned for `veinte` | Returned for `1.234,56` |
|-----------------------------|--------|-----------------------|------------------------:|
| @sys-number               | string | 20                    |                 1234.56 |
| @sys-number.literal       | string | veinte                |                1.234,56 |
| @sys-number.location      | array  | [0,6]                 |                   [0,8] |
| @sys-number.numeric_value | number | 20                    |                 1234.56 |
{: caption="More @sys-number examples" caption-side="top"}

You get equivalent results for other supported languages.

### @system-number usage tips
{: #system-entities-sys-number-usage-tips}

- If you use the @sys-number entity as a node condition and the user specifies zero as the value, the 0 value is recognized properly as a number. However, the 0 is interpreted as a `null` value for the condition, which results in the node not being processed. To check for numbers in a way that handles zeros properly, use the expression `@sys-number >= 0` in the node condition instead.

- If you use @sys-number to compare number values in a condition, be sure to separately include a check for the presence of a number itself. If no number is found, @sys-number evaluates to null, which might result in your comparison evaluating to true even when no number is present.

  For example, do not use `@sys-number<4` alone because if no number is found, `@sys-number` evaluates to null. Because null is less than 4, the condition evaluates to true even though no number is present.

  Use `@sys-number AND @sys-number<4` instead. If no number is present, the first condition evaluates to false, which appropriately results in the whole condition evaluating to false.

For information about processing number values, see the [Numbers method reference](/docs/assistant?topic=assistant-dialog-methods#dialog-methods-numbers).
{: tip}

## @sys-percentage entity
{: #system-entities-sys-percentage}

The @sys-percentage system entity detects percentages that are expressed in an utterance with the percent symbol or written out using the word `percent`. In either case, a numeric value is returned.

### Recognized formats
{: #system-entities-sys-percentage-formats}

- 15%
- 10 percent

### Metadata
{: #system-entities-sys-percentage-metadata}

`.numeric_value`: the canonical numeric value as an integer or a double

### Returns
{: #system-entities-sys-percentage-returns}

For the input `1,234.56%`, @sys-percentage returns these values:

| Attribute                     | Type   | Returned for `1,234.56%` |
|-------------------------------|--------|-------------------------:|
| @sys-percentage               | string |                  1234.56 |
| @sys-percentage.literal       | string |                1,234.56% |
| @sys-percentage.location      | array  |                    [0,9] |
| @sys-percentage.numeric_value | number |                  1234.56 |
{: caption="@sys-percentage examples" caption-side="top"}

For the input `1.234,56%`, in Spanish, @sys-currency returns these values:

| Attribute                     | Type   | Returned for `1.234,56%` |
|-------------------------------|--------|-------------------------:|
| @sys-percentage               | string |                  1234.56 |
| @sys-percentage.literal       | string |                1.234,56% |
| @sys-percentage.location      | array  |                    [0,9] |
| @sys-percentage.numeric_value | number |                  1234.56 |
{: caption="More @sys-percentage examples" caption-side="top"}

You get equivalent results for other supported languages.

### @system-percentage usage tips
{: #system-entities-sys-percentage-usage-tips}

- Percentage values are recognized as instances of @sys-number entities as well. If you are using separate conditions to check for both percentage values and numbers, place the condition that checks for a percentage above the one that checks for a number.

  This workaround is not necessary if you are using the revised system entities. For more details, see [New system entities](/docs/assistant?topic=assistant-beta-system-entities).
  {: note}

- If you use the @sys-percentage entity as a node condition and the user specifies `0%` as the value, the value is recognized as a percentage properly, but the condition is evaluated to the number zero not the percentage 0%. As a result, the `null` in the condition is evaluated to false and the node is not processed. To check for percentages in a way that handles zero percentages properly, use the expression `@sys-percentage >= 0` in the node condition instead.

- If you input a value like `1-2%`, the values `1%` and `2%` are returned as system entities. The index will be the whole range between 1% and 2%, and both entities will have the same index.

## @sys-person entity ![Beta feature](images/beta.png)
{: #system-entities-sys-person}

Available as a beta feature for only languages noted in the [Supported languages](/docs/assistant?topic=assistant-language-support) topic. A powerful alternative to using this system entity is to use a contextual entity for identifying proper nouns, such as names. For more information, see [Annotation-based method](https://cloud.ibm.com/docs/assistant?topic=assistant-entities#entities-annotations-overview).
{: tip}

The @sys-person system entity extracts names from the user's input. Names are recognized individually, so that "Joe" is not treated as "Joseph", or vice versa.

### Recognized formats
{: #system-entities-sys-person-formats}

- Ronald
- Jane Doe
- Vijay

For information about processing String values, see the [Strings method reference](/docs/assistant?topic=assistant-dialog-methods#dialog-methods-strings).
{: tip}
