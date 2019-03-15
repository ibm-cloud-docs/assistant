---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-15"

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

# Beta: New system entities
{: #beta-system-entities}

This feature is available for use by participants in the beta program only. To find out how to request access, see [Participate in the beta program](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

![Beta](images/beta.png) IBM releases services, features, and language support for your evaluation that are classified as beta. These features might be unstable, might change frequently, and might be discontinued with short notice. Beta features also might not provide the same level of performance or compatibility that generally available features provide and are not intended for use in a production environment.

Enable the new system entities to check out the improvements that have been made to the number-based system entities.

Currently, this setting can be enabled for English-language dialog skills only.
{: note}

The new system entities can recognize more nuanced mentions in user input. For example, `@sys-date` can calculate the date of a national holiday that is mentioned by name, such as `Thanksgiving`, or can recognize when a year is specified as part of a date mentioned in the user's input. The improvements also make it easier for the service to distinguish among the many number-based system entities. For example, a date mention that is recognized as a `@sys-date` is not also identified as a `@sys-number` mention.

The `@sys-location` and `@sys-person` system entities have not been changed.
{: note}

## Enabling the new system entities
{: #beta-system-entities-enable}

To enable the new system entities, complete the following steps:

1.  From the Skills page, open your skill.
1.  Click the **Options** tab.
1.  From the *System entities* page, choose **Try beta**.
1.  Click the **Entities** tab to open the *Entities* page, and then click the **System Entities** tab.
1.  Turn on the system entitites that you want to use in your dialog.

Test the new system entities by adding one or more of them to dialog node conditions or in the condition of a dialog node's conditional responses. Then, from the "Try it out" pane, submit user utterances that trigger the nodes you added.

### New system entity properties
{: #beta-system-entities-props}

To improve the system entities, new properties were added to the entity objects for certain system entity types.

The following table summarizes the new properties that were added. To see the properties that are associated with the current system entities, see [System entity details](/docs/services/assistant?topic=assistant-system-entities).

| System entity | Property | Description |
|---------------|----------|-------------|
| `@sys-date` | `alternatives` | Captures dates other than the one saved as the `@sys-date` value that the user might have meant when the date is not clearly indicated. For example, if today's date is 10 March 2019, and the user enters `on March 1`, then the March 1st date for next year is saved that as the `@sys-date` (`2020-03-01`). However, the user might have meant March 1st of 2019. The system also saves this year's date (`2019-03-01`) as an alternative date value. The alternative values are saved as an object that contains a value and confidence for each alternative date and is stored in a JSON Array. To get an alternative value, use the syntax: `<? @sys-date.alternatives[0].value ?>`. In most cases, the date of the future date is saved as the `@sys-date` value, and the past date is saved as the alternative. However, weekdays are always assumed to be future dates, and no alternative dates are created for them. For example, if today is Tuesday and a user enters, `Tuesday`, `this Tuesday`, and `next Tuesday`, then `@sys-date` is set to the date of Tuesday of next week and no alternative dates are created. |
| `@sys-date` | `datetime_link` | If present, indicates that the user's input mentions a date and time together, which implies that the date and time are related to one another. The start and end index values of the string that includes the date and time are saved as part of the link name. For example, if the input is, `Are you open today at 5?`, then `today` is recognized as a date reference at location `[13,18]` and `at 5` is recognized as a time reference at location `[19,23]`. A location value of `[13,23]`, which spans both the date and time mentions, is appended to the resulting datetime_link. It is named `datetime_link_13_23` and it is included in the output of the `@sys-date` and `@sys-time` system entities that participate in the relationship. |
| `@sys-date` | `festival` | Recognizes locale-specific holiday names and can return the date of the upcoming holiday, such as `Thanksgiving`, `Christmas`, and `Halloween`. Use all lower-case letters if you need to specify a holiday name, in a condition for example (`@sys-date.festival == 'thanksgiving'`). |
| `@sys-date` | `granularity` | Recognizes mentions of time frames, such as `next year` (=`year`). Options are `day`, `weekend`, `week`, `fortnight`, `month`, `quarter`, and `year`. |
| `@sys-date` | `range_link` | If present, indicates that the user's input contains syntax that suggests a date range is specified. For example, the input might be `I will travel from March 15 to March 22`. Each of the two @sys-date system entities that are detected have a `range_link` property in their output. Additional information is provided, including the role that each `@sys-date` plays in the range relationship. For example, the start date has a role type of `date_from` and the end date has a role type of `date_to`. To check a role value, you can use the syntax `@sys-date.role?.type == 'date_from'`. |
| `@sys-date` | `relative_year`, `relative_month`, `relative_week`, `relative_weekend`, `relative_day` | Recognizes and captures mentions of relative date values in the user's input, such as `two weeks ago` (`relative_week = -2`), or `this weekend (relative_weekend = 0)`, or `today` (`relative_day = 0`). |
| `@sys-date` | `specific_year`, `specific_quarter`, `specific_month`, `specific_day` | Recognizes and captures mentions of specific date values in the user's input, such as `2020` (`specific_year = 2020`), `Q2` (`specific_quarter = 2`), `March`  (`specific_month = March`), or `3rd` (`specific_day = 3`). |
| `@sys-number` | `range_link` | If present, indicates that the user's input contains syntax that suggests a number range is specified. For example, the input might be `I'm interested in 5 to 7.`. Each of the two `@sys-number` system entities that are detected have a `range_link` property in their output. Additional information is provided, including the role that each `@sys-number` plays in the range relationship. For example, the start number has a role type of `number_from` and the end number has a role type of `number_to`. To check a role value, you can use the syntax `@sys-number.role?.type == 'number_from'`. If the user input implies a range, but only one date is specified, then no range_link is returned. For example, `Have you been open since yesterday`. Instead, a role of type `open_range_end` or `open_range_start` is returned. A `range_modifier` that identifies the word in the input that triggers the identification of a range is also returned. For example, `since`. |
| `@sys-time` | `alternatives` | Captures times other than the one saved as the `@sys-time` value that the user might have meant when the time is not clearly indicated. For example, when a user says `The meeting is at 5`, the system calculates the time as 5:00:00 or 5 AM and saves that as the `@sys-time`. However, the user might have meant 17:00:00 or 5 PM. The system also saves the later time as an alternative time value. If the user enters `The meeting is at 5pm`, then `@sys-time` is set to 17:00:00, and no alternative value is created because there is no AM and PM confusion. The alternative values are saved as an object that contains a value and confidence for each alternative time and is stored in a JSON Array. To get an alternative value, use the syntax: `<? @sys-time.alternatives[0].value ?>` |
| `sys-time` | `datetime_link` | If present, indicates that the user's input mentions a date and time together, which implies that the date and time are related to one another. The start and end index values of the string that includes the date and time are saved as part of the link name. For example, if the input is, `Are you open today at 5?`, then `today` is recognized as a date reference at location `[13,18]` and `at 5` is recognized as a time reference at location `[19,23]`. A location value of `[13,23]`, which spans both the date and time mentions, is appended to the resulting datetime_link. It is named `datetime_link_13_23` and it is included in the output of the `@sys-date` and `@sys-time` system entities that participate in the relationship.  |
| `@sys-time` | `granularity` | Recognizes mentions of time frames, such as `now` (=`instant`) or `3 o'clock` (=`hour`) or `17:00:00` (=`second`). Options are `hour`, `minute`, `second` and `instant`. |
| `@sys-time` | `part_of_day` | Recognizes terms that represent the time of day, such as `morning`, `afternoon`, `evening`, `night`, or `now`. Also sets arbitrary times, such as `9:00:00` for morning, `15:00:00` for afternoon, `18:00:00` for evening, and `22:00:00` for night. |
| `@sys-time` | `range_link` | If present, indicates that the user's input contains syntax that suggests a time range is specified. For example, the input might be `Are you open from 9AM to 11AM`. Each of the two `@sys-time` system entities that are detected have a `range_link` property in their output. Additional information is provided, including the role that each `@sys-time` plays in the range relationship. For example, the start time has a role type of `time_from` and the end time has a role type of `time_to`. To check a role value, you can use the syntax `@sys-time.role?.type == 'time_from'`. If the user input implies a range, but only one time is specified, then no range_link is returned. For example, `Are you open until 9PM`. Instead, a role of type `open_range_end` or `open_range_start` is returned. A `range_modifier` that identifies the word in the input that triggers the identification of a range is also returned. For example, `until`. |
| `@sys-time` | `relative_hour`, `relative_minute`, `relative_second` | Recognizes relative mentions of time, such as `5 hours ago` (`relative_hour = -5`),`in two minutes`(`relative_minute = 2`), or `in a second` (`relative_second = 1`). |
| @sys-time | `specific_hour`, `specific_minute`, `specific_second` | Recognizes specific mentions of time, such as `at 5 o'clock` (`specific_hour = 5`), `at 2:30`(`specific_minute = 30`), or `23:30:22` (`specific_second = 22`). |

### Usage examples
{: #beta-system-entities-examples}

You can use the following types of conditions to take advantage of the new and improved system entities.

In a node that conditions on the `#Customer_Care_Store_Hours` intent, you can add conditional responses that provide slightly different answers about store hours depending on what the user asks.

| Conditional response condition | Description | Example response text |
|--------------------------------|-------------|----------|
| `@sys-date.festival == 'thanksgiving'` | Checks whether the customer is asking about hours on Thanksgiving day in particular. | We give out free meals to those in need on Thanksgiving day. |
| `@sys-date.festival` | Checks whether the customer is asking about hours on another specific holiday. | We are closed on Christmas Day, July 4th, and President's Day. On other holidays, we are open from 10AM to 5PM. |
| `@sys-time.part_of_day == 'night'` | Checks whether the user includes any terms that mention night as the time of day in the input. | We are not open late; we close at 9PM most days. |
| `@sys-date.datetime_link && input.text.contains('today') && now().sameOrAfter(@sys-time)` | Checks whether the input contains a phrase such as `today at 8`. It also checks whether the `@sys-time` that is detected is earlier than the current time of day. You can add a context variable to the conditional response that creates a `$new_time` variable with the value `<? @sys-time.plusHours(12) ?>`. You can use this approach to create a date variable that captures the more likely time meant by a user who, at noontime asks, `Are you open today at 8`. The system incorrectly assumes the user means 8AM instead of 8PM. | You mean today at $new_time, right? |
| `@sys-time.range_link && entities[0].role?.type == 'time_from' && entities[1].role?.type == 'time_to'` | Checks whether the input contains a time range. The condition also makes sure that the first `@sys-time` system entity listed in the entities array is the start time and that the second one is the end time before including them in the response. | You want to know if we're open between <? entities[0] ?> and <? entities[1] ?>, is that right? |
| ``@sys-time && entities[0].role?.type == 'open_range_end'` | Checks whether the input contains an open-ended time range. If a user asks, `Are you open until 9pm?`, because the input identifies only one date or time in a time span, a role of type `open_range_end` is returned. | Do you mean from now (`<? now().reformatDateTime('h:mm a') ?>`) until <? @sys-time.reformatDateTime('h:mm a') ?>? |
| true | Responds to any other requests for store hour information. | We're open from 9AM to 9PM Monday through Saturday. |

<!-- | @sys-date  | specific_day_of_week | Recognizes mentions of days by name, and stores it in lower case. For example, `Sunday` is stored as `sunday`. |

and examples:

| @sys-date.specific_day_of_week == 'sunday' | Checks whether the customer is asking about the store hours for Sunday in particular. | We are closed on Sundays. |
| @sys-date.specific_day_of_week == 'monday' | Gets confirmation from the user that the date the system assumed the user meant (next Monday), is the Monday they are asking about by using the detected alternative value, which in this case is last Monday's date. | Do you mean `@sys-date` or `<? @sys-date.alternatives[0].value ?>`? |

left out | `@sys-time` | `granularity` | Recognizes mentions of time frames, such as `noon` (= `hour`) |
-->
