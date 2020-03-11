---

copyright:
  years: 2015, 2020
lastupdated: "2020-03-11"

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
{:gif: data-image-type='gif'}

# New system entities
{: #new-system-entities}

The numeric system entities have been updated. The new system entities can recognize more nuanced mentions in user input. For example, `@sys-date` can calculate the date of a national holiday, such as `Thanksgiving`, when it is mentioned by name. And `@sys-date` can recognize when a year is specified as part of a date mentioned in the user's input. The improvements also make it easier for your assistant to distinguish among the many number-based system entities. For example, a date mention, such as `May 10`, that is recognized to be a `@sys-date` is not also identified as a `@sys-number` mention.

The `@sys-location` and `@sys-person` system entities did not change.
{: note}

The new system entities are enabled automatically in English, Brazilian Portuguese, Czech, Dutch, French, German, Italian, and Spanish dialog skills. For more information, see [Supported languages](/docs/assistant?topic=assistant-language-support).

## Enabling the new system entities
{: #new-system-entities-enable}

Arabic, Chinese, Korean, and Japanese dialog skills or skills in service instances that were created before 10 March 2020 use the legacy version of the system entities unless you enable the new version.

To enable the new system entities, complete the following steps:

1.  Click the **Skills** icon ![Skills menu icon](images/nav-skills-icon.png), and then open your skill.
1.  From the Skills menu, click **Options**, and then click **System Entities**.
1.  Select **Use new system entities**.
1.  From the Skills menu, click **Entities**, and then click **System entities**.
1.  Turn on the individual system entitites that you want to use in your dialog.

Test the new system entities by adding one or more of them to dialog node conditions or in the condition of a dialog node's conditional responses. Then, from the "Try it out" pane, submit user utterances that trigger the nodes you added.

If you decide you prefer the behavior of the previous version of the system entities, you can stop using the new version at any time. Return to the **Options>System entities** page, and then select **Use legacy version**. Give Watson time to retrain.

If you decide to continue using the new version, review any existing dialog nodes that condition on or return system entity values to determine if you need to make changes. For example, some mentions were classified as more than one system entity type before. Now, if a currency is mentioned, for example, it is classified as a `@sys-currency` only. You might be able to simplify logic you used before to work around the prior behavior. Or you might need to revise logic that you added that relies on the old behavior.

### New system entity properties
{: #new-system-entities-props}

To improve the system entities, new properties were added to the entity objects of number-based system entities.

The following table summarizes the new properties that were added. To see the properties that are associated with the current system entities, see [System entity details](/docs/assistant?topic=assistant-system-entities).

Table 1. New system entity properties

| System entity | Property | Description |
|---------------|----------|-------------|
| `@sys-date` | `alternatives` | Captures dates other than the one saved as the `@sys-date` value that the user might have meant when the date is not clearly indicated. For example, if today's date is 10 March 2019, and the user enters `on March 1`, then the March 1st date for next year is saved as the `@sys-date` (`2020-03-01`). However, the user might have meant March 1st of 2019. The system also saves this year's date (`2019-03-01`) as an alternative date value. The alternative values are saved as an object that contains a value and confidence for each alternative date and is stored in a JSON Array. To get an alternative value, use the syntax: `@sys-date.alternative`. In most cases, the date of the future date is saved as the `@sys-date` value, and the past date is saved as the alternative. This is true for weekdays when the weekday is specified without a preposition such as `on` or an adjective such as `this` or `next`. For example, `Are you open Monday?`. However, if a user specifies a weekday as part of a phrase such as `on Monday` or `next Monday`, then the date is assumed to be a future date, and no alternative date is created for it. For example, if today is Tuesday and a user enters, `this Tuesday` or `next Tuesday`, then `@sys-date` is set to the date of Tuesday of next week and no alternative dates are created. |
| `@sys-date` | `datetime_link` | If present, indicates that the user's input mentions a date and time together, which implies that the date and time are related to one another. The start and end index values of the string that includes the date and time are saved as part of the link name. For example, if the input is, `Are you open today at 5?`, then `today` is recognized as a date reference at location `[13,18]` and `at 5` is recognized as a time reference at location `[19,23]`. A location value of `[13,23]`, which spans both the date and time mentions, is appended to the resulting datetime_link. It is named `datetime_link_13_23` and it is included in the output of the `@sys-date` and `@sys-time` system entities that participate in the relationship. |
| `@sys-date` | `festival` | Recognizes locale-specific holiday names and can return the date of the upcoming holiday, such as `Thanksgiving`, `Christmas`, and `Halloween`. Use all lowercase letters if you need to specify a holiday name, in a condition for example (`@sys-date.festival == 'thanksgiving'`). |
| `@sys-date` | `granularity` | Recognizes mentions of time frames, such as `next year` (=`year`). Options are `day`, `weekend`, `week`, `fortnight`, `month`, `quarter`, and `year`. |
| `@sys-date` | `interpretation` | The object that is returned by your assistant which contains new fields that increase the precision of the system entity classifier. You can omit `interpretation` from the expression that you use to refer to its fields. For example, you can access the value of the `festival` field in the interpretation object by using the shorhand syntax `<? @sys-date.festival ?>`. |
| `@sys-date` | `range_link` | If present, indicates that the user's input contains syntax that suggests a date range is specified. For example, the input might be `I will travel from March 15 to March 22`. Each of the two `@sys-date` system entities that are detected has a `range_link` property in its output. Additional information is provided, including the role that each `@sys-date` plays in the range relationship. For example, the start date has a role type of `date_from` and the end date has a role type of `date_to`. To check a role value, you can use the syntax `@sys-date.role?.type == 'date_from'`. If the user input implies a range, but only one date is specified, then the `range_link` property is not returned, but one role type is returned. For example, if the user asks, `Have you been open since yesterday`. Yesterday's date is recognized as the `@sys-date` mention, and a role of type `date_from` is returned for it. A `range_modifier` that identifies the word in the input that triggers the identification of a range is also returned. In this example, the modifier is `since`. |
| `@sys-date` | `relative_year`, `relative_month`, `relative_week`, `relative_weekend`, `relative_day` | Recognizes and captures mentions of relative date values in the user's input, such as `two weeks ago` (`relative_week = -2`), or `this weekend (relative_weekend = 0)`, or `today` (`relative_day = 0`). |
| `@sys-date` | `specific_year`, `specific_quarter`, `specific_month`, `specific_day`, `specific_day_of_week` | Recognizes and captures mentions of specific date values in the user's input, such as `2020` (`specific_year = 2020`), `Q2` (`specific_quarter = 2`), `March`  (`specific_month = 3`), `Monday` (`specific_day_of_week = monday`), or `3rd` (`specific_day = 3`). |
| `@sys-number` | `interpretation` | The object that is returned by your assistant which contains new fields that increase the precision of the system entity classifier. You can omit `interpretation` from the expression that you use to refer to its fields. For example, you can access the value of the `range_link` field in the interpretation object by using the shorhand syntax `<? @sys-date.range_link ?>`. |
| `@sys-number` | `range_link` | If present, indicates that the user's input contains syntax that suggests a number range is specified. For example, the input might be `I'm interested in 5 to 7.`. Each of the two `@sys-number` system entities that are detected have a `range_link` property in their output. Additional information is provided, including the role that each `@sys-number` plays in the range relationship. For example, the start number has a role type of `number_from` and the end number has a role type of `number_to`. To check a role value, you can use the syntax `@sys-number.role?.type == 'number_from'`. |
| `@sys-time` | `alternatives` | Captures times other than the one saved as the `@sys-time` value that the user might have meant when the time is not clearly indicated. For example, when a user says `The meeting is at 5`, the system calculates the time as 5:00:00 or 5 AM and saves that as the `@sys-time`. However, the user might have meant 17:00:00 or 5 PM. The system also saves the later time as an alternative time value. If the user enters `The meeting is at 5pm`, then `@sys-time` is set to 17:00:00, and no alternative value is created because there is no AM and PM confusion. The alternative values are saved as an object that contains a value and confidence for each alternative time and is stored in a JSON Array. To get an alternative value, use the syntax: `@sys-time.alternative`. |
| `sys-time` | `datetime_link` | If present, indicates that the user's input mentions a date and time together, which implies that the date and time are related to one another. The start and end index values of the string that includes the date and time are saved as part of the link name. For example, if the input is, `Are you open today at 5?`, then `today` is recognized as a date reference at location `[13,18]` and `at 5` is recognized as a time reference at location `[19,23]`. A location value of `[13,23]`, which spans both the date and time mentions, is appended to the resulting datetime_link. It is named `datetime_link_13_23` and it is included in the output of the `@sys-date` and `@sys-time` system entities that participate in the relationship. |
| `@sys-time` | `granularity` | Recognizes mentions of time frames, such as `now` (=`instant`) or `3 o'clock` or `noon` (=`hour`), or `17:00:00` (=`second`). Options are `hour`, `minute`, `second` and `instant`. |
| `@sys-time` | `interpretation` | The object that is returned by your assistant which contains new fields that increase the precision of the system entity classifier. You can omit `interpretation` from the expression that you use to refer to its fields. For example, you can access the value of the `granularity` field in the interpretation object by using the shorhand syntax `<? @sys-time.granularity ?>`. |
| `@sys-time` | `part_of_day` | Recognizes terms that represent the time of day, such as `morning`, `afternoon`, `evening`, `night`, or `now`. Also sets a time range for each part of the day. The response contains two `@sys-time` values, one for the start and one for the end of the time range. The entities array contains a `range_link` object with `time-from` and `time-to` role types. The time range for morning is `6:00:00` to `12:00:00`. Afternoon is `12:00:00` to `18:00:00`. Evening is `18:00:00` to `22:00:00`. Night is `22:00:00` to `23:59:59`. Night ends just before midnight because otherwise it would overlap with a new day, which is measured by `@sys-date`. |
| `@sys-time` | `range_link` | If present, indicates that the user's input contains syntax that suggests a time range is specified. For example, the input might be `Are you open from 9AM to 11AM`. Each of the two `@sys-time` system entities that are detected has a `range_link` property in its output. Additional information is provided, including the role that each `@sys-time` plays in the range relationship. For example, the start time has a role type of `time_from` and the end time has a role type of `time_to`. To check a role value, you can use the syntax `@sys-time.role?.type == 'time_from'`. If the user input implies a range, but only one time is specified, then the `range_link` property is not returned, but one role type is returned. For example, if the user asks, `Are you open until 9PM`, 9PM is recognized as the `@sys-time` mention, and a role of type `time_to` is returned for it. A `range_modifier` that identifies the word in the input that triggers the identification of a range is also returned. In this example, the modifier is `until`. |
| `@sys-time` | `relative_hour`, `relative_minute`, `relative_second` | Recognizes relative mentions of time, such as `5 hours ago` (`relative_hour = -5`),`in two minutes`(`relative_minute = 2`), or `in a second` (`relative_second = 1`). |
| `@sys-time` | `specific_hour`, `specific_minute`, `specific_second` | Recognizes specific mentions of time, such as `at 5 o'clock` (`specific_hour = 5`), `at 2:30`(`specific_minute = 30`), or `23:30:22` (`specific_second = 22`). |
{: caption="New system entity properties" caption-side="top"}

The values in the following table are not properties of the entity object. They are helper values that you can use in the product to quickly extract date or time details from the new system entities.

Table 2. System entities helper functions

| System entity | Helper value | Description |
|---------------|----------|-------------|
| `@sys-date` | `day` | Returns the day that is specified in the date as a numeric value. For example, if the date is `5 December 2019`, it returns `5`. |
| `@sys-date` | `day_of_week` | Evaluates the date to determine which day of the week it is. Stores the day of the week in lower case. For example, `Sunday` is stored as `sunday`. Use a lowercase initial letter to check for a weekday name in a condition. For example: `@sys-date.day_of_week == 'sunday'` |
| `@sys-date` | `month` | Returns the month that is specified in the date as a numeric value. For example, if the date is `5 December 2019`, it returns `12`. |
| `@sys-date` | `year` | Returns the year that is specified in the date as a numeric value. For example, if the date is `5 December 2019`, it returns `2019`. |
| `@sys-time` | `hour` | Returns the hour that is specified in the time as a numeric value. For example, if the time is `5:30:10 PM`, it returns`17` to represent 5 PM. |
| `@sys-time` | `minute` | Returns the minute value that iss specified in the time. `30`. If no minutes are specified, then this helper returns `0`.|
| `@sys-time` | `second` | Returns the second value that is specified in the time. `10`. If no second value is specified, then this helper returns `0`. |
{: caption="System entities helper functions" caption-side="top"}

### Usage examples
{: #new-system-entities-examples}

You can use the following types of expressions to take advantage of the new properties in the improved system entities. The table shows the results are returned when a user mentions the date `4 July 2019` and time `3:30 PM`. You can use these expressions in dialog text responses without surrounding them in `<? ?>` syntax.

Table 3. Using the new system entities to get date and time details

| SpEL expression syntax | Result |
|------------------------|---------|
| `@sys-date.year` | `2019` |
| `@sys-date.month` | `7` |
| `@sys-date.day` | `4` |
| `@sys-date.day_of_week` | `thursday` |
| `@sys-time.hour` | `15` |
| `@sys-time.minute` | `30` |
| `@sys-time.second` | `0` |
{: caption="Use the new system entities to get data and time details" caption-side="top"}

In a node that conditions on the `#Customer_Care_Store_Hours` intent, you can add conditional responses that use new system entity properties to provide slightly different answers about store hours depending on what the user asks. 

You probably do not want to use all of these conditional responses in a real dialog; they are described here merely to illustrate what's possible.
{: tip}

Table 4. Using new system entities in conditional responses

| Conditional response condition syntax | Description | Example response text |
|--------------------------------|-------------|----------|
| `@sys-date.festival == 'thanksgiving'` | Checks whether the customer is asking about hours on Thanksgiving day in particular. | We give out free meals to those in need on Thanksgiving day. |
| `@sys-date.festival` | Checks whether the customer is asking about hours on another specific holiday. | We are closed on Christmas Day, July 4th, and President's Day. On other holidays, we are open from 10AM to 5PM. |
| `@sys-time.part_of_day == 'night'` | Checks whether the user includes any terms that mention night as the time of day in the input. | We are not open late; we close at 9PM most days. |
| `@sys-date.datetime_link && input.text.contains('today') && now().sameOrAfter(@sys-time)` | Checks whether the input contains a phrase such as `today at 8`. It also checks whether the `@sys-time` that is detected is earlier than the current time of day. You can add a context variable to the conditional response that creates a `$new_time` variable with the value `<? @sys-time.plusHours(12) ?>`. You can use this approach to create a date variable that captures the more likely time meant by a user who, at noontime asks, `Are you open today at 8`. The system assumes the user means 8AM instead of 8PM. | You mean today at $new_time, right? |
| `@sys-time.range_link && entities[0].role?.type == 'time_from' && entities[1].role?.type == 'time_to'` | Checks whether the input contains a time range. The condition also makes sure that the first `@sys-time` system entity listed in the entities array is the start time and that the second one is the end time before including them in the response. | You want to know if we're open between `<? entities[0] ?>` and `<? entities[1] ?>`, is that right? |
| `@sys-time && entities.size() < 2 && entities[0].role?.type == 'time_to'` | Checks whether the input contains one `@sys-time` mention that has a `time_to` role type. If the user input matches this condition, it suggests that the user specified the end time of an open-ended time range. For example, the response would be triggered by the input, `Are you open until 9pm?`. This input matches because it identifies only one time in a time span, and the mention has a `time_to` role. | Do you mean from now (`<? now().reformatDateTime('h:mm a') ?>`) until `<? @sys-time.reformatDateTime('h:mm a') ?>`? |
| `@sys-date.day_of_week == 'sunday'` | Checks whether a specific date that the user is asking about falls on a Sunday. | We are closed on Sundays. |
| `@sys-date.specific_day_of_week == 'monday' && @sys-date.alternative` | Checks whether the user mentioned the weekday `Monday` in their query. For example, `Are you open Monday`. The condition also checks whether any alternative dates were stored. Alternative dates are created when your assistant isn't entirely sure which Monday the user means, so it stores the dates of alternative Mondays also. When a user specifies a weekday, your assistant assumes that the user means the future occurrence of the day (the coming Monday). You can add a response that double checks the intended date, by using the detected alternative value. In this case, the alternative date is the previous Monday's date. | Do you mean `@sys-date` or `@sys-date.alternative`? |
| `true` | Responds to any other requests for store hour information. | We're open from 9AM to 9PM Monday through Saturday. |
{: caption="Use the new system entities in conditional responses" caption-side="top"}

## Disabling the new system entities
{: #new-system-entities-disable}

To disable the new system entities, complete the following steps:

1.  Click the **Skills** icon ![Skills menu icon](images/nav-skills-icon.png), and then open your skill.
1.  From the Skills menu, click **Options**, and then click **System Entities**.
1.  Select **Use legacy version**.
