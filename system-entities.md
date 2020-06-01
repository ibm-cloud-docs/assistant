---

copyright:
  years: 2015, 2020
lastupdated: "2020-05-28"

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

# Legacy system entities
{: #system-entities}

Learn about system entities that are provided by IBM for you to use out of the box. These built-in utility entities help your assistant recognize terms and references that are commonly used by customers in conversation, such as numbers and dates. 
{: shortdesc}

New and improved versions of the numeric system entities are generally available for dialog skills in all languages. The new versions of the numeric system entities provide superior number recognition with higher precision. This topic describes the legacy versions of the numeric system entities. The legacy versions are being deprecated. Support will end on 15 July 2020. When support ends, mentions in input that are detected as person or location system entities today will no longer be detected. Switch to using the new system entities now so you have time to test your dialog before support ends. For more information, see [New system entities](/docs/assistant?topic=assistant-new-system-entities).
{: deprecated}

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

### @sys-currency usage tips
{: #system-entities-currencty-usage-tips}

- Currency values are recognized as instances of @sys-number entities as well. If you are using separate conditions to check for both currency values and numbers, place the condition that checks for currency before the one that checks for a number.

  This workaround is not necessary if you are using the revised system entities. For more information, see [New system entities](/docs/assistant?topic=assistant-new-system-entities).
  {: note}

- If you use the @sys-currency entity as a node condition and the user specifies `$0` as the value, the value is recognized as a currency properly, but the condition is evaluated to the number zero, not the currency zero. As a result, the `null` in the condition is evaluated to false and the node is not processed. To check for currency values in a way that handles zeros properly, use the expression `@sys-currency OR @sys-currency == 0` in the node condition instead.

## @sys-date and @sys-time entities
{: #system-entities-sys-date-time}

The `@sys-date` system entity extracts mentions such as `Friday`, `today`, or `November 1`. The value of this entity stores the corresponding inferred date as a string in the format `yyyy-MM-dd`. For example, `"2016-11-21"`. The system augments missing elements of a date (such as the year for `"November 21"`) with the current date values.

For the English locale only, the format for date input is MM/DD/YYYY. The format for date input changes to DD/MM/YYYY only if the first two numbers are greater than 12. The value that is stored has the format `yyyy-MM-dd`.
{: note}

The `@sys-time` system entity extracts mentions such as `2pm`, `at 4`, or `15:30`. The value of this entity stores the time as a string in the format `HH:mm:ss`. For example, `"13:00:00"`.

### Date-time mentions
{: #system-entities-date-time-mentions}

Mentions of date and time, such as `now` or `two hours from now` are extracted as two separate entity mentions - one `@sys-date` and one `@sys-time`. These two mentions are not linked to one another except that they share the same literal string that spans the complete date-time mention.

Multi-word date-time mentions such as `on Monday at 4pm` are also extracted as two @sys-date and @sys-time mentions. When mentioned together consecutively they also share a single literal string that spans the complete date-time mention.

### Date and time ranges
{: #system-entities-date-time-ranges}

Mentions of a date range such as `the weekend`, `next week`, or `from Monday to Friday` are extracted as a pair of `@sys-date` entity mentions that show the start and end of the range. Similarly, mentions of time ranges such as `from 2 to 3` are extracted as two `@sys-time` entities, showing the start and end times. The two entities in the pair share a literal string that corresponds to the full date or time range mention.

### `Last` and `Next` dates and times
{: #system-entities-last-next}

In some locales, a phrase like "last Monday" is used to specify the Monday of the previous week only. In contrast, other locales use `last Monday` to specify the last day that was a Monday. However, the Monday might have been either in the same week or the previous week.

As an example, for `Friday June 16`, in some locales `last Monday` refers to either `June 12` or to `June 5`. In other locales, it refers only to `June 5` (the previous week). This same logic holds true for a phrase like `next Monday`.

The {{site.data.keyword.conversationshort}} service treats `last` and `next` dates as references to the most immediate last or next day that is referenced, which might be in either the same or a previous week.

For time phrases like `for the last 3 days` or `in the next 4 hours`, the logic is equivalent. For example, if the input includes, `in the next 4 hours`, two `@sys-time` entities are found: one for the current time, and one for the time four hours later than the current time.

### Time zones
{: #system-entities-time-zones}

Mentions of a date or time that are relative to the current time are resolved for a chosen time zone. By default, the time zone is Greenwich mean time. Therefore, REST API clients that are located in different time zones get the current Coordinated Universal Time when `now` is mentioned in input.

Optionally, the REST API client can add the local time zone as the context variable `$timezone`. This context variable must be sent with every client request. For example, the `$timezone` value can be `America/Los_Angeles`, `EST`, or `UTC`. For a full list of supported time zones, see [Supported time zones](/docs/assistant?topic=assistant-time-zones).

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
- \* Returns the next matching date. If the date passed for the current year, next year's date is returned.

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

Available as a beta feature for only languages noted in the [Supported languages](/docs/assistant?topic=assistant-language-support) topic. 

This system entity is being deprecated entirely. There is no new version of this system entity available with the new system entities. A powerful alternative to using the `@sys-location` system entity is to use a contextual entity for identifying proper nouns, such as locations. For more information, see [Annotation-based method](https://cloud.ibm.com/docs/assistant?topic=assistant-entities#entities-annotations-overview).
{: deprecated}

The @sys-location system entity extracts place names (country, state or province, city, town, and so on) from the user's input.

### Recognized formats
{: #system-entities-sys-location-formats}

- Boston
- U.S.A.
- New South Wales

For more information about processing String values, see the [Strings method reference](/docs/assistant?topic=assistant-dialog-methods#dialog-methods-strings).
{: tip}

## @sys-number entity
{: #system-entities-sys-number}

The @sys-number system entity detects numbers that are written with either numerals or words. In either case, a numeric value is returned.

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

### @sys-number usage tips
{: #system-entities-sys-number-usage-tips}

- If you use the @sys-number entity as a node condition and the user specifies zero as the value, the 0 value is recognized properly as a number. However, the 0 is interpreted as a `null` value for the condition, which results in the node not being processed. To check for numbers in a way that handles zeros properly, use the expression `@sys-number == 0` in the node condition also. The full expression to use is `@sys-number OR @sys-number == 0`.

- If you use @sys-number to compare number values in a condition, be sure to separately include a check for the presence of a number itself. If no number is found, @sys-number evaluates to null. Your comparison might evaluate to true even when no number is present.

  For example, do not use `@sys-number<4` alone because if no number is found, `@sys-number` evaluates to null. Because null is less than 4, the condition evaluates to true even though no number is present.

  Use `(@sys-number OR @sys-number == 0) AND @sys-number < 4` instead. If no number is present, the first condition evaluates to false. As a result, the whole condition evaluates to false.

For more information about processing number values, see the [Numbers method reference](/docs/assistant?topic=assistant-dialog-methods#dialog-methods-numbers).
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

### @sys-percentage usage tips
{: #system-entities-sys-percentage-usage-tips}

- Percentage values are recognized as instances of @sys-number entities as well. If you are using separate conditions to check for both percentage values and numbers, place the condition that checks for a percentage before the one that checks for a number.

  This workaround is not necessary if you are using the revised system entities. For more information, see [New system entities](/docs/assistant?topic=assistant-new-system-entities).
  {: note}

- If you use the @sys-percentage entity as a node condition and the user specifies `0%` as the value, the value is recognized as a percentage properly, but the condition is evaluated to the number zero not the percentage 0%. As a result, the `null` in the condition is evaluated to false and the node is not processed. To check for percentages in a way that handles zero percentages properly, use the expression `@sys-percentage OR @sys-percentage == 0` in the node condition instead.

- If you input a value like `1-2%`, the values `1%` and `2%` are returned as system entities.

## @sys-person entity ![Beta feature](images/beta.png)
{: #system-entities-sys-person}

Available as a beta feature for only languages noted in the [Supported languages](/docs/assistant?topic=assistant-language-support) topic. 

This system entity is being deprecated entirely. There is no new version of this system entity available with the new system entities. A powerful alternative to using the `@sys-person` system entity is to use a contextual entity for identifying proper nouns, such as names. For more information, see [Annotation-based method](https://cloud.ibm.com/docs/assistant?topic=assistant-entities#entities-annotations-overview).
{: deprecated}

The @sys-person system entity extracts names from the user's input. Names are recognized individually, so that "Joe" is not treated as "Joseph", or vice versa.

### Recognized formats
{: #system-entities-sys-person-formats}

- Ronald
- Jane Doe
- Vijay

For more information about processing String values, see the [Strings method reference](/docs/assistant?topic=assistant-dialog-methods#dialog-methods-strings).
{: tip}
