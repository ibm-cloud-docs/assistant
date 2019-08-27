---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-17"

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

# 新系统实体 ![Beta](images/beta.png)
{: #beta-system-entities}

通过启用新系统实体，可利用对 IBM 提供的基于数字的系统实体所做的改进。

目前，只能为以英语或德语编写的对话技能启用此设置。
{: note}

新系统实体可以识别用户输入中更细致的提及项。例如，如果提及国定假期（如 `Thanksgiving`）的名称，那么 `@sys-date` 可以计算假期的日期。`@sys-date` 还可以识别在用户输入中所提及日期中指定的年份。通过这些改进，助手能更轻松地区分大量基于数字的系统实体。例如，识别为 `@sys-date` 的日期提及项（如 `May 10`）不会同时识别为 `@sys-number` 提及项。

`@sys-location` 和 `@sys-person` 系统实体未更改。
{: note}

## 启用新系统实体
{: #beta-system-entities-enable}

要启用新系统实体，请完成以下步骤：

1.  在“技能”页面中，打开技能。
1.  单击**选项**选项卡。
1.  单击**系统实体**，然后选择**试用 Beta**。
1.  单击**实体**选项卡，然后单击**系统实体**。
1.  开启要在对话中使用的系统实体。

通过将一个或多个新系统实体添加到对话节点条件或添加到对话节点条件响应的条件中，以测试新系统实体。然后，在“试用”窗格中，提交用户话语以触发添加的节点。

如果您决定更希望使用系统实体先前版本的行为，那么可以随时停止使用 Beta 版本。返回到**选项**选项卡上的**系统实体**页面，然后选择**保持现有**。给 Watson 些时间来进行重新训练。

如果决定继续使用 Beta 版本，请复查以系统实体值为条件或返回系统实体值的所有现有对话节点，以确定是否需要进行更改。例如，某些提及项先前被分类为多个系统实体类型。但现在仅分类为一个系统实体类型，例如货币提及项只会分类为 `@sys-currency`。您可能有能力简化先前用于处理该行为的逻辑。或者，可能需要修改原先添加的依赖于旧行为的逻辑。

### 新系统实体属性
{: #beta-system-entities-props}

为了改进系统实体，向基于数字的系统实体的实体对象添加了新属性。

下表概述了添加的新属性。要查看与当前系统实体关联的属性，请参阅[系统实体详细信息](/docs/services/assistant?topic=assistant-system-entities)。

表 1. 新系统实体属性

|系统实体|属性                  |描述|
|---------------|----------|-------------|
|`@sys-date`|`alternatives`|捕获的日期是在用户未明确指示日期时，除了保存为 `@sys-date` 值的日期以外，用户可能所指的其他日期。例如，如果今天的日期为 2019 年 3 月 10 日，但用户输入的是 `on March 1`，那么会将明年 3 月 1 日保存为 `@sys-date` (`2020-03-01`)。但是，用户可能指的是 2019 年 3 月 1 日。因此，系统还会将今年的日期 (`2019-03-01`) 保存为备用日期值。备用值会保存为对象，其中包含每个备用日期的值和置信度，并以 JSON 数组格式存储。要获取备用值，请使用语法：`@sys-date.alternative`。在大多数情况下，未来的日期会保存为 `@sys-date` 值，而过去的日期会保存为备用日期。这同样适用于指定了星期几，但未指定介词（如 `on`）或形容词（如 `this` 或 `next`）的情况。例如，`Are you open Monday?`。但是，如果用户在短语中指定星期几，例如 `on Monday` 或 `next Monday`，那么会假定日期为未来的日期，并且不会为其创建备用日期。例如，如果今天是星期二，用户输入了 `this Tuesday` 或 `next Tuesday`，那么 `@sys-date` 会设置为下周的星期二，并且不会创建备用日期。|
|`@sys-date`|`datetime_link`|如果提供，指示用户的输入同时提及了日期和时间，这表示日期和时间彼此相关。包含日期和时间的字符串的开始和结束索引值会保存为链接名称的一部分。例如，如果输入为 `Are you open today at 5?`，那么会将 `today` 识别为位置 `[13,18]` 处的日期引用，将 `at 5` 识别为位置 `[19,23]` 处的时间引用。同时涵盖日期和时间提及项的位置值 `[13,23]` 会附加到生成的 datetime_link。此链接会命名为 `datetime_link_13_23`，并包含在参与该关系的 `@sys-date` 和 `@sys-time` 系统实体的输出中。|
|`@sys-date`|`festival`|识别特定于语言环境的假期名称，并可以返回即将来临的假期的日期，例如 `Thanksgiving`、`Christmas` 和 `Halloween`。如果需要在条件中指定假期名称，请全部使用小写字母，例如 (`@sys-date.festival == 'thanksgiving'`)。|
|`@sys-date`|`granularity`|识别时间范围的提及项，例如 `next year` (=`year`)。选项包括 `day`、`weekend`、`week`、`fortnight`、`month`、`quarter` 和 `year`。|
|`@sys-date`|`interpretation`|助手返回的对象，其中包含增加系统实体分类器精度的新字段。可以在用于引用其字段的表达式中省略 `interpretation`。例如，可以使用简短语法 `<? @sys-date.festival ?>` 来访问 interpretation 对象中 `festival` 字段的值。|
|`@sys-date`|`range_link`|如果提供，指示用户的输入包含的语法表明指定了日期范围。例如，输入可能为 `I will travel from March 15 to March 22`。对于检测到的两个 `@sys-date` 系统实体，其输出中都会有 `range_link` 属性。这提供了额外的信息，包括每个 `@sys-date` 在范围关系中扮演的角色。例如，开始日期的角色类型为 `date_from`，结束日期的角色类型为 `date_to`。要检查角色值，可以使用语法 `@sys-date.role?.type == 'date_from'`。如果用户输入隐含范围，但仅指定了一个日期，那么不会返回 `range_link` 属性，而只会返回一个角色类型。例如，如果用户询问 `Have you been open since yesterday`，那么会将 yesterday 的日期识别为 `@sys-date` 提及项，并且会为其返回类型为 `date_from` 的角色。此外，还会返回 `range_modifier`，用于标识输入中触发识别范围的词。在此示例中，修饰符为 `since`。 |
|`@sys-date`|`relative_year`、`relative_month`、`relative_week`、`relative_weekend`、`relative_day`|识别并捕获用户输入中相对日期值的提及项，例如 `two weeks ago` (`relative_week = -2`)、`this weekend (relative_weekend = 0)` 或 `today` (`relative_day = 0`)。|
|`@sys-date`|`specific_year`、`specific_quarter`、`specific_month`、`specific_day`、`specific_day_of_week`|识别并捕获用户输入中特定日期值的提及项，例如 `2020` (`specific_year = 2020`)、`Q2` (`specific_quarter = 2`)、`March` (`specific_month = 3`)、`Monday` (`specific_day_of_week = monday`) 或 `3rd` (`specific_day = 3`)。|
|`@sys-number`|`interpretation`|助手返回的对象，其中包含增加系统实体分类器精度的新字段。可以在用于引用其字段的表达式中省略 `interpretation`。例如，可以使用简短语法 `<? @sys-date.range_link ?>` 来访问 interpretation 对象中 `range_link` 字段的值。|
|`@sys-number`|`range_link`|如果提供，指示用户的输入包含的语法表明指定了数字范围。例如，输入可能为 `I'm interested in 5 to 7.`。对于检测到的两个 `@sys-number` 系统实体，其输出中都会有 `range_link` 属性。这提供了额外的信息，包括每个 `@sys-number` 在范围关系中扮演的角色。例如，开始数字的角色类型为 `number_from`，结束数字的角色类型为 `number_to`。要检查角色值，可以使用语法 `@sys-number.role?.type == 'number_from'`。|
|`@sys-time`|`alternatives`|捕获的时间是在用户未明确指示时间时，除了保存为 `@sys-time` 值的时间以外，用户可能所指的其他时间。例如，用户说 `The meeting is at 5` 时，系统会将时间计算为 5:00:00 或 5 AM，并将其保存为 `@sys-time`。但是，用户可能指的是 17:00:00 或 5 PM。因此，系统还会将后一个时间保存为备用时间值。如果用户输入 `The meeting is at 5pm`，那么 `@sys-time` 会设置为 17:00:00，并且不会创建备用值，因为不存在 AM 和 PM 混淆的问题。备用值会保存为对象，其中包含每个备用时间的值和置信度，并以 JSON 数组格式存储。要获取备用值，请使用语法：`@sys-time.alternative`。|
|`sys-time`|`datetime_link`|如果提供，指示用户的输入同时提及了日期和时间，这表示日期和时间彼此相关。包含日期和时间的字符串的开始和结束索引值会保存为链接名称的一部分。例如，如果输入为 `Are you open today at 5?`，那么会将 `today` 识别为位置 `[13,18]` 处的日期引用，将 `at 5` 识别为位置 `[19,23]` 处的时间引用。同时涵盖日期和时间提及项的位置值 `[13,23]` 会附加到生成的 datetime_link。此链接会命名为 `datetime_link_13_23`，并包含在参与该关系的 `@sys-date` 和 `@sys-time` 系统实体的输出中。|
|`@sys-time`|`granularity`|识别时间范围的提及项，例如 `now` (=`instant`)、`3 o'clock`、`noon` (=`hour`) 或 `17:00:00` (=`second`)。选项包括 `hour`、`minute`、`second` 和 `instant`。|
|`@sys-time`|`interpretation`|助手返回的对象，其中包含增加系统实体分类器精度的新字段。可以在用于引用其字段的表达式中省略 `interpretation`。例如，可以使用简短语法 `<? @sys-time.granularity ?>` 来访问 interpretation 对象中 `granularity` 字段的值。|
|`@sys-time`|`part_of_day`|识别表示一天中时间的词汇，例如 `morning`、`afternoon`、`evening`、`night` 或 `now`。此外，可设置任意时间，例如 `9:00:00` 表示 morning，`15:00:00` 表示 afternoon，`18:00:00` 表示 evening，`22:00:00` 表示 night。|
|`@sys-time`|`range_link`|如果提供，指示用户的输入包含的语法表明指定了时间范围。例如，输入可能为 `Are you open from 9AM to 11AM`。对于检测到的两个 `@sys-time` 系统实体，其输出中都会有 `range_link` 属性。这提供了额外的信息，包括每个 `@sys-time` 在范围关系中扮演的角色。例如，开始时间的角色类型为 `time_from`，结束时间的角色类型为 `time_to`。要检查角色值，可以使用语法 `@sys-time.role?.type == 'time_from'`。如果用户输入隐含范围，但仅指定了一个时间，那么不会返回 `range_link` 属性，而只会返回一个角色类型。例如，如果用户询问 `Are you open until 9PM`，那么会将 9PM 识别为 `@sys-time` 提及项，并且会为其返回类型为 `time_to` 的角色。此外，还会返回 `range_modifier`，用于标识输入中触发识别范围的词。在此示例中，修饰符为 `until`。|
|`@sys-time`|`relative_hour`、`relative_minute`、`relative_second`|识别时间的相对提及项，例如 `5 hours ago` (`relative_hour = -5`)、`in two minutes`(`relative_minute = 2`) 或 `in a second` (`relative_second = 1`)。|
|`@sys-time`|`specific_hour`、`specific_minute`、`specific_second`|识别时间的具体提及项，例如 `at 5 o'clock` (`specific_hour = 5`)、`at 2:30`(`specific_minute = 30`) 或 `23:30:22` (`specific_second = 22`)。|
{: caption="新系统实体属性" caption-side="top"}

下表中的值不是实体对象的属性。这些属性是可以在产品中使用的助手函数值，用于快速从新系统实体中抽取日期或时间详细信息。

表 2. 系统实体助手函数

|系统实体|助手函数值|描述|
|---------------|----------|-------------|
|`@sys-date`|`day`|将日期中指定的天作为数字值返回。例如，如果日期为 `5 December 2019`，那么会返回 `5`。|
|`@sys-date`|`day_of_week`|对日期求值，以确定这是星期几。以小写格式存储星期几。例如，`Sunday` 会存储为 `sunday`。使用小写首字母来检查条件中的星期几名称。例如：`@sys-date.day_of_week == 'sunday'`|
|`@sys-date`|`month`|将日期中指定的月份作为数字值返回。例如，如果日期为 `5 December 2019`，那么会返回 `12`。|
|`@sys-date`|`year`|将日期中指定的年份作为数字值返回。例如，如果日期为 `5 December 2019`，那么会返回 `2019`。|
|`@sys-time`|`hour`|将时间中指定的小时作为数字值返回。例如，如果时间为 `5:30:10 PM`，那么会返回 `17` 以表示 5 PM。|
|`@sys-time`|`minute`|返回时间中指定的分钟值。例如，上例中的 `30`。如果未指定分钟，那么此助手函数会返回 `0`。|
|`@sys-time`|`second`|返回时间中指定的秒值。例如，上例中的 `10`。如果未指定秒值，那么此助手函数会返回 `0`。|
{: caption="系统实体助手函数" caption-side="top"}

### 用法示例
{: #beta-system-entities-examples}

可以使用以下类型的表达式来利用改进的系统实体中的新属性。下表显示了用户在提及日期 `4 July 2019` 和时间 `3:30 PM` 时返回的结果。可以在对话文本响应中使用这些表达式，而无需采用 `<? ?>` 语法将其括起。

表 3. 使用新系统实体来获取日期和时间详细信息

|SpEL 表达式语法|结果|
|------------------------|---------|
| `@sys-date.year` | `2019` |
| `@sys-date.month` | `7` |
| `@sys-date.day` | `4` |
| `@sys-date.day_of_week` | `thursday` |
| `@sys-time.hour` | `15` |
| `@sys-time.minute` | `30` |
| `@sys-time.second` | `0` |
{: caption="使用新系统实体来获取日期和时间详细信息" caption-side="top"}

在以 `#Customer_Care_Store_Hours` 意向为条件的节点中，可以添加使用新系统实体属性的条件响应，以根据用户的询问来提供有关门店营业时间的略有不同的回答。 

您可能并不希望在实际对话中使用所有这些条件响应；此处描述这些条件响应仅仅是为了说明可以使用的条件响应。
{: tip}

表 4. 在条件响应中使用新的系统实体

|条件响应条件语法|描述|示例响应文本|
|--------------------------------|-------------|----------|
|`@sys-date.festival == 'thanksgiving'`|检查客户是否特别询问感恩节的营业时间。|我们在感恩节为有需要的人提供免费餐。|
|`@sys-date.festival`|检查客户是否询问其他特定假期的营业时间。|我们在圣诞节、7 月 4 日和总统日不营业。在其他假期，营业时间为上午 10 点到下午 5 点。|
|`@sys-time.part_of_day == 'night'`|检查用户是否在输入中包含将 night 作为一天中的时间提及的任何词汇。|我们不会营业到很晚；大多数时候结束营业的时间为晚上 9 点。|
| `@sys-date.datetime_link && input.text.contains('today') && now().sameOrAfter(@sys-time)` |检查输入是否包含诸如 `today at 8` 之类的短语。此外，还会检查检测到的 `@sys-time` 是否早于当前时刻。可以将上下文变量添加到条件响应，用于创建值为 `<? @sys-time.plusHours(12) ?>` 的 `$new_time` 变量。可以使用此方法来创建日期变量，用于捕获用户在中午时间询问 `Are you open today at 8` 时更有可能所指的时间。系统会假定用户指的是 8AM，而不是 8PM。|您是指今天 $new_time 点，对吗？|
|`@sys-time.range_link && entities[0].role?.type == 'time_from' && entities[1].role?.type == 'time_to'`|检查输入是否包含时间范围。该条件还会先确保在 entities 数组中列出的第一个 `@sys-time` 系统实体是开始时间，第二个是结束时间，然后才将这些系统实体包含在响应中。|您想知道我们的营业时间是否为 `<? entities[0] ?>` 到 `<? entities[1] ?>`，对吗？|
| `@sys-time && entities.size() < 2 && entities[0].role?.type == 'time_to'` |检查输入是否包含一个具有 `time_to` 角色类型的 `@sys-time` 提及项。如果用户输入与此条件相匹配，那么表明用户指定了开放式时间范围的结束时间。例如，响应将由输入 `Are you open until 9pm?` 来触发。此输入与此条件相匹配，因为它仅在时间范围内识别到一个时间，并且该提及项具有 `time_to` 角色。|您是指从现在 (`<? now().reformatDateTime('h:mm a') ?>`) 一直到 `<? @sys-time.reformatDateTime('h:mm a') ?>` 吗？|
|`@sys-date.day_of_week == 'sunday'`|检查用户询问的特定日期是否为某个星期日。|我们星期日不营业。|
|`@sys-date.specific_day_of_week == 'monday' && @sys-date.alternative`|检查用户是否在其查询中提及一星期中的 `Monday`（星期一）。例如，`Are you open Monday`。此条件还会检查是否存储了任何备用日期。助手不完全确定用户指的是哪个星期一时，会创建备用日期，因此还会存储备用星期一的日期。用户指定星期几时，助手会假定用户指的是未来的日期（下一个星期一）。可以使用检测到的备用值来添加用于复查目标日期的响应。在本例中，备用日期是上一个星期一的日期。|您是指 `@sys-date` 还是 `@sys-date.alternative`？|
|`true`|响应其他任何询问门店营业时间信息的请求。|我们的营业时间是星期一到星期六，早上 9:00 到晚上 9 点|
{: caption="在条件响应中使用新的系统实体" caption-side="top"}
