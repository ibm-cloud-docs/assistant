---

copyright:
  years: 2015, 2021
lastupdated: "2021-04-16"

subcollection: assistant

keywords: system entity, sys-number, sys-date, sys-time

---

{{site.data.keyword.attribute-definition-list}}

Documentation for the **classic {{site.data.keyword.assistant_classic_short}}** experience has moved. For the most up-to-date version, see [System entities](/docs/watson-assistant?topic=watson-assistant-system-entities){: external}.
{: attention}

# System entities
{: #system-entities}

Learn about system entities that are provided by IBM for you to use out of the box. These built-in utility entities help your assistant recognize terms and references that are commonly used by customers in conversation, such as numbers and dates. 
{: shortdesc}

For information about how to add system entities to your dialog skill, see [Creating entities](/docs/assistant?topic=assistant-entities#entities-enable-system-entities).

In January 2020, a new version of the system entities was introduced. As of April 2021, only the new version of the system entities is supported for all languages. The option to switch to using the legacy version is no longer available. 
{: note}

For information about how to use contextual entities to identify people and location names, see the [Detecting Names And Locations With Watson Assistant](https://medium.com/ibm-watson/detecting-names-and-locations-with-watson-assistant-e3e1fa2a8427){: external} blog post on Medium.

## @sys-currency entity
{: #system-entities-sys-currency}

The `@sys-currency` system entity detects mentions of monetary currency values in user input. The currency can be expressed with a currency symbol or currency-specific terms. In either case, a number is returned.

### Recognized formats
{: #system-entities-sys-currency-formats}

- 20 cents
- Five dollars
- $10

### Attributes
{: #system-entities-sys-currency-metadata}

- `.literal`: Exact phrase in input that is interpreted to be a currency mention.
- `.numeric_value`: Canonical numeric value as an integer or a double, in base units.
- `.location`: Lists the index element values of the first and last letters in the text string for the phrase that is interpreted to be a currency mention.
- `.unit`: Base unit of currency specified as a 3-letter ISO currency code. For example, `USD` or `EUR`.

The following table illustrates the information that each attribute captures from the user input.

| Attribute                   | Type   | Example (input = `twenty dollars`) | Example (input = `$1,234.56`) |
|-----------------------------|--------|-------------|---------|
| @sys-currency               | string |  `20` | `1234.56` |
| @sys-currency.literal       | string | `twenty dollars` | `$1,234.56` |
| @sys-currency.numeric_value | number | `20` | `1234.56` |
| @sys-currency.location      | array  | `[0,14]` | `[0,9]` |
| @sys-currency.unit          | string | `USD` | `USD` |
{: caption="@sys-currency examples" caption-side="top"}

For the input `veinte euro` (`twenty euro` in Spanish) or <code>&euro;1.234,56</code>, @sys-currency returns these values:

| Attribute                   | Type   | Input is  `veinte euro` | Input is <code>&euro;1.234,56</code> |
|-----------------------------|--------|-----------------------------|-------------------------:|
| @sys-currency               | string | 20                          |                  1234.56 |
| @sys-currency.literal       | string | veinte euro                 |                &euro;1.234,56 |
| @sys-currency.numeric_value | number | 20                          |                  1234.56 |
| @sys-currency.location      | array  |[0,11]                       |                     [0,9]|
| @sys-currency.unit          | string | EUR                         |                     EUR  |
{: caption="More @sys-currency examples" caption-side="top"}

Equivalent results are returned for other supported languages and national currencies.

### @sys-currency usage tips
{: #system-entities-currency-usage-tips}

If you use the `@sys-currency` entity as a node condition and the user specifies `$0` as the value, the value is recognized as a currency properly, but the condition is evaluated to the number zero, not the currency zero. As a result, the `null` in the condition is evaluated to false and the node is not processed. To check for currency values in a way that handles zeros properly, use the expression `@sys-currency OR @sys-currency == 0` in the node condition instead.

For more information about currencies that are recognized per language, see [Currency support](/docs/assistant?topic=assistant-sys-currency-details).

## @sys-date
{: #system-entities-sys-date}

The `@sys-date` system entity detects mentions of dates in user input. The date value is stored as a string in the format `yyyy-MM-dd`. For example, the mention `May 8` is stored as `"2020-05-08"`. The system augments missing elements of a date (such as the year for `"May 8"`) with the current date values.

If you select English as the skill language, the system uses US English (`en-us`) as the locale. For the US English locale only, the format of the date is `MM/DD/YYYY`. The format of the date changes to `DD/MM/YYYY` only if the first two numbers are greater than 12. The value that is stored has the format `yyyy-MM-dd`.

### Recognized formats
{: #system-entities-sys-date-formats}

- Friday
- today
- May 8

### Attributes
{: #system-entities-sys-date-metadata}

- `.alternatives`: Saves alternative date values when the input is open to interpretation. Typically the future date is returned as the `@sys-date` value. If there is ambiguity, the past date is saved as an alternative date. For example, for the input `in January`, a future date is returned. However, if it's February (January passed for this year), then the past January is returned also as an alternative date. An alternative date is also provided for weekdays when the weekday is specified without a modifier. For example, for the input `Are you open Wednesday?`, the future date is stored, but an alternative date with the date of the previous Wednesday is created also. Alternative dates are created for phrases that contain modifiers such as `next`, `this`, or `last`, which can have different meanings in different locales. The {{site.data.keyword.assistant_classic_short}} service treats `last` and `next` dates as references to the most immediate last or next day that is referenced, which might be in either the same or a previous week. For some modifiers, an alternative date is created or not depending on the current day of the week. For example, maybe the input contains `this Wednesday`. If it's Monday, the future date is stored and no alternative date is created. However, if it's Wednesday, then the date is assumed to be the Wednesday of the coming week, but today's date is stored as an alternative date. Each alternative date is saved as an object that contains a value and confidence for each alternative date. The alternative date objects are stored in a JSON Array. To get an alternative value, use the syntax: `@sys-date.alternative`.
- `.calendar_type`: Specifies the calendar type for the timezone in use. `GREGORIAN` is the only supported type.
- `.datetime_link`: See [Date and time mentions](#system-entities-sys-date-time).
- `.day`: Helper method that returns the day in the date.
- `.day_of_week`: Returns the day of the week in the date as a lowercase string.
- `.festival`: Recognizes locale-specific holiday names and can return the date of the upcoming holiday, such as `Thanksgiving`, `Christmas`, and `Halloween`. Use all lowercase letters if you need to specify a holiday name, in a condition for example (`@sys-date.festival == 'thanksgiving'`). For more information about recognized holidays, see [Festivals](/docs/assistant?topic=assistant-sys-date-festivals).
- `.granularity`: Recognizes time frame mentions. Options are `day`, `weekend`, `week`, `fortnight`, `month`, `quarter`, and `year`.
- `.interpretation`: The object that is returned by your assistant which contains new fields that increase the precision of the system entity classifier. You can omit `interpretation` from the expression that you use to refer to its fields. For example, you can access the value of the `festival` field in the interpretation object by using the shorhand syntax `<? @sys-date.festival ?>`.
- `.literal`: Exact phrase in input that is interpreted to be the date.
- `.location`: Index element values of the first and last letters in the text string for the phrase that is interpreted to be a date. 
- `.month`: Helper method that returns the month in the date. 
- `.range_link`: If present, indicates that the user's input contains a date range. The start and end index values of the string that includes the date range are saved as part of the link name. Additional information is provided, including the role that each `@sys-date` plays in the range relationship. For example, the start date has a role type of `date_from` and the end date has a role type of `date_to`. To check a role value, you can use the syntax `@sys-date.role?.type == 'date_from'`. If the user input implies a range, but only one date is specified, then the `range_link` property is not returned, but one role type is returned. For example, if the user asks, `Have you been open since yesterday`. Yesterday's date is recognized as the `@sys-date` mention, and a role of type `date_from` is returned for it. A `range_modifier` that identifies the word in the input that triggers the identification of a range is also returned. In this example, the modifier is `since`.
- `.relative_{timeframe}`: Recognizes and captures mentions of relative date values in the user's input. Returns a number that shows the number of units between now and the specified date. The {timeframe} units can be `year`, `month`, `week`, `weekend`, or `day`.
- `.specific_{timeframe}`: Recognizes and captures mentions of specific date units in the user's input. The {timeframe} units can be `year`, `quarter`, `month`, `day`, or `day_of_week`.
- `.year`: Helper method that returns the year in the date.

The following table illustrates the information that each attribute captures from the user input.

| Attribute                   | Type   | Example |
|-----------------------------|--------|---------|
| @sys-date                   | string | If the input is `May 13, 2020`, returns `2020-05-13`. |
| @sys-date.alternatives      | array | For example, if today's date is `10 March 2019`, and the user enters `on March 1`, then the March 1st date for next year is saved as the `@sys-date` (`2020-03-01`). However, the user might have meant March 1st of 2019. The system also saves this year's date (`2019-03-01`) as an alternative date value. As another example, if today is Tuesday and a user enters, `this Tuesday` or `next Tuesday`, then `@sys-date` is set to the date of Tuesday of next week and no alternative dates are created. |
| @sys-date.calendar_type | string | For any date mention, returns `GREGORIAN`. |
| @sys-date.datetime_link | string | N/A |
| @sys-date.day | string | If the input is `13 May 2020`, returns `13`. |
| @sys-date.day_of_week | string | If the input is `13 May 2020` and it's a Wednesday, returns `wednesday`. |
| @sys-date.festival      | string | If the input contains `Thanksgiving Day`, then `2020-11-26` is returned. |
| @sys-date.granularity   | string | If the input mentions `next year`, returns `year`. |
| @sys-date.literal       | string | If the input is, `I plan to leave on Saturday.`, then `on Saturday` is returned. |
| @sys-date.location      | array  | If the input is `My vacation starts tomorrow.`, returns `[19, 27]`. |
| @sys-date.month | string | If the input is `13 May 2020`, returns `5`. |
| @sys-date.range_link | string | For example, the input might be `I will travel from March 15 to March 22`. Each `@sys-date` entity that is detected has a `range_link` property in its output that is named `date_range_14_39` where the location of the date range text is `[14,39]`. The start date (`2020-05-15`) has a role type of `date_from`. The end date (`2020-05-22`) has a role type of `date_to`. |
| @sys-date.relative_{timeframe} | number | If the input is `two weeks ago`, returns `relative_week = -2`. If the input is `this weekend`,  returns `relative_weekend = 0`. If the input is `today`, returns `relative_day = 0`. |
| @sys-date.specific_{timeframe} | string | For `2020`, returns `specific_year = 2020`. For `Q2 2020` or `second quarter 2020`, returns`specific_quarter = 2`. For `March`, returns `specific_month = 3`. For `Monday`, returns `specific_day_of_week = monday`. For `May 3rd`, returns `specific_day = 3`. |
| @sys-date.year | string | If the input is `13 May 2020`, returns `2020`. |
{: caption="@sys-date attributes" caption-side="top"}

## @sys-time
{: #system-entities-sys-time}

The `@sys-time` system entity detects mentions of times in user input. Returns the time that is specified in user input in the format `HH:mm:ss`. For example, `"13:00:00"` for `1pm`.

### Recognized formats
{: #system-entities-sys-time-formats}

- 2pm
- at 4
- 15:30

### Attributes
{: #system-entities-sys-time-metadata}

- `.alternatives`: Captures times other than the one saved as the `@sys-time` value that the user might have meant when the time is not clearly indicated. The alternative values are saved as an object that contains a value and confidence for each alternative time and is stored in a JSON Array. To get an alternative value, use the syntax: `@sys-time.alternative`.
- `.calendar_type`: Specifies the calendar type for the timezone in use. `GREGORIAN` is the only supported type.
- `.granularity`: Recognizes mentions of time frames. Options are `hour`, `minute`, `second` and `instant`.
- `.hour`: Helper method that returns the hour that is specified in the time as a numeric value.
- `.interpretation`: The object that is returned by your assistant which contains new fields that increase the precision of the system entity classifier. You can omit `interpretation` from the expression that you use to refer to its fields. For example, you can access the value of the `granularity` field in the interpretation object by using the shorhand syntax `<? @sys-time.granularity ?>`.
- `.literal`: Exact phrase in input that is interpreted to be the time.
- `.location`: Lists the index element values of the first and last letters in the text string for the phrase that is interpreted to be a time mention.
- `.minute`: Helper method that returns the minute value that is specified in the time.
- `.part_of_day`: Recognizes terms that represent the time of day, such as `morning`, `afternoon`, `evening`, `night`, or `now`. Also sets a time range for each part of the day. The response contains two `@sys-time` values, one for the start and one for the end of the time range. The entities array contains a `range_link` object with `time-from` and `time-to` role types. The time ranges for different parts of the day can differ by locale. For US English, morning is `6:00:00` to `12:00:00`, afternoon is `12:00:00` to `18:00:00`, evening is `18:00:00` to `22:00:00`, and night is `22:00:00` to `23:59:59`. Night ends just before midnight because otherwise it would overlap with a new day, which is measured by `@sys-date`.
- `.range_link`: If present, indicates that the user's input contains a time range. The start and end index values of the string that includes the time range are saved as part of the link name. Additional information is provided, including the role that each `@sys-time` plays in the range relationship. For example, the start time has a role type of `time_from` and the end time has a role type of `time_to`. To check a role value, you can use the syntax `@sys-time.role?.type == 'time_from'`. If the user input implies a range, but only one time is specified, then the `range_link` property is not returned, but one role type is returned. A `range_modifier` that identifies the word in the input that triggers the identification of a range is also returned.
- `.relative_{timeframe}`: Recognizes relative mentions of time and returns a number that shows the number of units between now and the specified time. The {timeframe} units can be `hour`, `minute`, or `second`.
- `.second`: Helper method that returns the second value that is specified in the time.
- `.specific_{timeframe}`: Recognizes and captures mentions of specific time units in the user input. The {timeframe} units can be `hour`, `minute`, or `second`.

The following table illustrates the information that each attribute captures from the user input.

| Attribute                   | Type   | Example |
|-----------------------------|--------|---------|
| @sys-time | string | For the input `6:30 PM`, returns `18:30:00`. |
| @sys-time.alternatives | array | If a user says `The meeting is at 5`, the system calculates the time as `5:00:00` or `5 AM` and saves that as the `@sys-time`. However, the user might have meant `17:00:00` or `5 PM`. The system also saves the later time as an alternative time value. If the user enters `The meeting is at 5pm`, then `@sys-time` is set to `17:00:00`, and no alternative value is created because there is no AM and PM confusion. |
| @sys-time.calendar_type | string | For any time mention, returns `GREGORIAN`. |
| @sys-time.granularity   | string | If the input is `now`, returns `instant`. For `3 o'clock` or `noon`, returns `hour`. For `17:00:00`, returns `second`. |
| @sys-time.hour | number | If the input contains `5:30:10 PM`, it returns`17` to represent 5 PM. |
| @sys-time.literal  | string | For the input, `The store closes at 8PM`, returns `at 8PM`. |
| @sys-time.location | array  | For the input, `The store closes at 8PM`, returns `[17, 23]`. |
| @sys-time.minute | number | If the input mentions the time `5:30:10 PM`, returns `30`. If no minutes are specified, then this helper returns `0`. |
| @sys-time.part_of_day | string | If the input is `this morning`, returns two @sys-time values, `6:00:00` and `12:00:00`. Also returns a `range_link` object with `time-from` and `time-to` role types. |
| @sys-time.range_link | string | For the input `Are you open from 9AM to 11AM`, two `@sys-time` entities are detected. Each `@sys-time` entity that is detected has a `range_link` property in its output. If a range is implied but only one time value is provided, then only one `@sys-time` value is returned. For the input `Are you open until 9PM`, 9PM is recognized as the `@sys-time` mention, and a role of type `time_to` is returned for it. The `range_modifier` in this example is `until`. |
| @sys-time.relative_{timeframe} | number | For `5 hours ago`, returns `relative_hour = -5`. For `in two minutes`, returns `relative_minute = 2`. For `in a second`, returns `relative_second = 1`. |
| @sys-time.second | number | If the input mentions the time `5:30:10 PM`, returns `10`. If no second value is specified, then this helper returns `0`. |
| @sys-time.specific_{timeframe} | string | For `at 5 o'clock`, returns `specific_hour = 5`. For `at 2:30`, returns `specific_minute = 30`. For `23:30:22`, returns `specific_second = 22`. |
{: caption="@sys-time attributes" caption-side="top"}

## Date and time mentions
{: #system-entities-sys-date-time}

Some user input contains information that includes both date and time information. Your assistant captures both values and saves them as two separate entity mentions, one `@sys-date` and one `@sys-time`.

The relationship between the date and time values is established in two ways:

- Each entity that participates in the relationship contains the same `datetime_link` attribute.
- The literal string that spans the complete date and time mention, when they are mentioned together in input, is the same for both entities. The location information is appended to the `datetime_link` name.

### Recognized formats
{: #system-entities-sys-date-time-formats}

- now
- two hours from now
- on Monday at 4pm

### Attributes
{: #system-entities-sys-date-time-metadata}

- `.datetime_link`: If present, indicates that the user's input mentions a date and time together, which implies that the date and time are related to one another. The start and end index values of the string that includes the date and time are saved as part of the link name.

The following table illustrates the information that each attribute captures from the user input.

| Attribute                   | Type   | Example |
|-----------------------------|--------|---------|
| `@sys-date.datetime_link` and `@sys-time.datetime_link` | string | If the input is, `Are you open today at 5?`, then `today` is recognized as a date mentionn(`2020-05-21`) at location `[13,18]` and `at 5` is recognized as a time mention (`05:00:00`) at location `[19,23]`. A location value of `[13,23]`, which spans both the date and time mentions, is appended to the resulting `datetime_link`. It is named `datetime_link_13_23` and it is included in the output of the `@sys-date` and `@sys-time` entities that participate in the relationship. |
{: caption="Date and time attributes" caption-side="top"}

### Time zones
{: #system-entities-time-zones}

Mentions of a date or time that are relative to the current time are resolved for a chosen time zone. By default, the time zone is Greenwich mean time. Therefore, REST API clients that are located in different time zones get the current Coordinated Universal Time when `now` is mentioned in input.

Optionally, the REST API client can add the local time zone as the context variable `$timezone`. This context variable must be sent with every client request. For example, the `$timezone` value can be `America/Los_Angeles`, `EST`, or `UTC`. For a full list of supported time zones, see [Supported time zones](/docs/assistant?topic=assistant-time-zones).

When the `$timezone` variable is provided, the values of relative `@sys-date` and `@sys-time` mentions are computed based on the client time zone instead of UTC.

For information about processing date and time values, see the [Date and time method reference](/docs/assistant?topic=assistant-dialog-methods#dialog-methods-date-time).
{: tip}

## @sys-number entity
{: #system-entities-sys-number}

The `@sys-number` system entity detects mentions of numbers in user input. The number can be written with either numerals or words. In either case, a number is returned.

### Recognized formats
{: #system-entities-sys-number-formats}

- 21
- twenty one
- 3.13

### Attributes
{: #system-entities-sys-number-metadata}

- `.literal`: Exact phrase in input that is interpreted to be the number.
- `.location`: Index element values of the first and last letters in the text string that is interpreted to be a number.
- `.numeric_value`: Canonical numeric value as an integer or a double.
- `.range_link`: If present, indicates that the user's input contains a number range. The location value of the text that specifies the range is appended to the link name. Additional information is provided, including the role that each `@sys-number` plays in the range relationship. For example, the start number has a role type of `number_from` and the end number has a role type of `number_to`. To check a role value, you can use the syntax `@sys-number.role?.type == 'number_from'`.

The following table illustrates the information that each attribute captures from the user input.

| Attribute                   | Type   | Example  |
|-----------------------------|--------|----------|
| @sys-number               | string | If the input is `twenty`, returns `20`. If the input is  `1,234.56`, returns `1234.56`. |
| @sys-number.literal       | string | If the input is `twenty`, returns `twenty`. If the input is `1,234.56` returns `1,234.56`. |
| @sys-number.location      | array | If the input is `twenty`, returns [0,6]. If the input is  `1,234.56`, returns [0,8]|
| @sys-number.numeric_value | number | If the input is `twenty`, returns `20` . If the input is  `1,234.56`, returns `1234.56` |
| @sys-number.range_link | string | If the input is `I'm interested in 5 to 7.`. Each of the two `@sys-number` system entities that are detected have a `range_link` of `number_range_18_24` in their output. The first `@sys-number` entity (`5`) has a `number_from` role type. The last `@sys-number` entity (`7`) has a `number_to` role type. |
{: caption="@sys-number examples" caption-side="top"}

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

The `@sys-percentage` system entity detects mentions of percentages in user input. The percentage can be expressed in an utterance with the percent symbol or written out using the word `percent`. In either case, a numeric value is returned.

### Recognized formats
{: #system-entities-sys-percentage-formats}

- 15%
- 10 percent

### Attributes
{: #system-entities-sys-percentage-metadata}

- `.literal`: Exact phrase in input that is interpreted to be a percentage.
- `.location`: Index element values of the first and last letters in the text string that is interpreted to be a percentage mention.
- `.numeric_value`: Canonical numeric value as an integer or a double
- `.range_link`: If present, indicates that the user's input contains a range, such as `from 2 to 3 percent`. The location value of the text that specifies the range is appended to the link name. Additional information is provided, including the role that each `@sys-percentage` plays in the range relationship. For example, the start number has a role type of `number_from` and the end number has a role type of `number_to`. To check a role value, you can use the syntax `@sys-percentage.role?.type == 'number_from'`.

The following table illustrates the information that each attribute captures from the user input.

| Attribute                       | Type   | Example |
|---------------------------------|--------|-------------------------:|
| @sys-percentage                | string | If input contains `1,234.56%`, returns `1234.56`. |
| @sys-percentage.literal        | string | If input contains `50 percent` returns `50 percent`. |
| @sys-percentage.location       | array  | If input contains `from 2 to 3 percent`, returns `[0,19]`. |
| @sys-percentage.numeric_value  | number | If input contains `50 percent` returns `50`. |
| @sys-percentage.range_link     | string | If input contains `from 2 to 3 percent`, returns `number_range_0_19`. |
{: caption="@sys-percentage examples" caption-side="top"}

### @sys-percentage usage tips
{: #system-entities-sys-percentage-usage-tips}

- If you use the @sys-percentage entity as a node condition and the user specifies `0%` as the value, the value is recognized as a percentage properly, but the condition is evaluated to the number zero not the percentage 0%. As a result, the `null` in the condition is evaluated to false and the node is not processed. To check for percentages in a way that handles zero percentages properly, use the expression `@sys-percentage OR @sys-percentage == 0` in the node condition instead.

- If you input a value like `1-2%`, the values `1%` and `2%` are returned as system entities.

## Using system entities in conditions
{: #system-entities-conditions}

In a node that conditions on the `#Customer_Care_Store_Hours` intent, you can add conditional responses that use new system entity properties to provide slightly different answers about store hours depending on what the user asks. 

You probably do not want to use all of these conditional responses in a real dialog; they are described here merely to illustrate what's possible.
{: tip}

| Conditional response condition syntax | Description | Example response text |
|--------------------------------|-------------|----------|
| `@sys-date.festival == 'thanksgiving'` | Checks whether the customer is asking about hours on Thanksgiving day in particular. | We give out free meals to those in need on Thanksgiving Day. |
| `@sys-date.festival` | Checks whether the customer is asking about hours on another specific holiday. | We are closed on Christmas Day, July 4th, and President's Day. On other holidays, we are open from 10AM to 5PM. |
| `@sys-time.part_of_day == 'night'` | Checks whether the user includes any terms that mention night as the time of day in the input. | We are not open late; we close at 9PM most days. |
| `@sys-date.datetime_link && input.text.contains('today') && now().sameOrAfter(@sys-time)` | Checks whether the input contains a phrase such as `today at 8`. It also checks whether the `@sys-time` that is detected is earlier than the current time of day. You can add a context variable to the conditional response that creates a `$new_time` variable with the value `<? @sys-time.plusHours(12) ?>`. You can use this approach to create a date variable that captures the more likely time meant by a user who, at noontime asks, `Are you open today at 8`. The system assumes the user means 8AM instead of 8PM. | You mean today at $new_time, correct? |
| `@sys-time.range_link && entities[0].role?.type == 'time_from' && entities[1].role?.type == 'time_to'` | Checks whether the input contains a time range. The condition also makes sure that the first `@sys-time` system entity listed in the entities array is the start time and that the second one is the end time before including them in the response. | You want to know if we're open between `<? entities[0] ?>` and `<? entities[1] ?>`, is that correct? |
| `@sys-time && entities.size() < 2 && entities[0].role?.type == 'time_to'` | Checks whether the input contains one `@sys-time` mention that has a `time_to` role type. If the user input matches this condition, it suggests that the user specified the end time of an open-ended time range. For example, the response would be triggered by the input, `Are you open until 9pm?`. This input matches because it identifies only one time in a time span, and the mention has a `time_to` role. | Do you mean from now (`<? now().reformatDateTime('h:mm a') ?>`) until `<? @sys-time.reformatDateTime('h:mm a') ?>`? |
| `@sys-date.day_of_week == 'sunday'` | Checks whether a specific date that the user is asking about falls on a Sunday. | We are closed on Sundays. |
| `@sys-date.specific_day_of_week == 'monday' && @sys-date.alternative` | Checks whether the user mentioned the weekday `Monday` in their query. For example, `Are you open Monday`. The condition also checks whether any alternative dates were stored. Alternative dates are created when your assistant isn't entirely sure which Monday the user means, so it stores the dates of alternative Mondays also. When a user specifies a weekday, your assistant assumes that the user means the future occurrence of the day (the coming Monday). You can add a response that double checks the intended date, by using the detected alternative value. In this case, the alternative date is the previous Monday's date. | Do you mean `@sys-date` or `@sys-date.alternative`? |
| `true` | Responds to any other requests for store hour information. | We're open from 9AM to 9PM Monday through Saturday. |
{: caption="System entities in conditional responses" caption-side="top"}
