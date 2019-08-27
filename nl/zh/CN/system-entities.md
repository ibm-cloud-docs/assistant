---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-06"

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

# 系统实体详细信息
{: #system-entities}

了解有关 IBM 提供的可供您开箱即用的系统实体。这些内置实用程序实体可帮助助手识别会话中客户常用的词汇和引用，例如数字和日期。
{: shortdesc}

系统实体可用于[支持的语言](/docs/services/assistant?topic=assistant-language-support)主题中注明的语言。

如果对话技能为英语或德语，那么可以试用已更新的系统实体。有关更多详细信息，请参阅[新系统实体](/docs/services/assistant?topic=assistant-beta-system-entities)。

有关如何使用实体的更多信息，请参阅[创建实体](/docs/services/assistant?topic=assistant-entities#entities-enable-system-entities)。

## @sys-currency 实体
{: #system-entities-sys-currency}

@sys-currency 系统实体用于检测话语中使用货币符号或特定于货币的词汇表示的货币值。这将返回数字值。

### 可识别的格式
{: #system-entities-sys-currency-formats}

- 20 cents
- Five dollars
- $10

### 元数据
{: #system-entities-sys-currency-metadata}

- `.numeric_value`：整数或双精度值的规范数字值（基本单位）
- `.unit`：基本单位货币代码（例如，“USD”或“EUR”）

### 返回项
{: #system-entities-sys-currency-returns}

对于输入 `twenty dollars` 或 `$1,234.56`，@sys 会返回以下值：

|属性|类型|针对 `twenty dollars` 的返回项|针对 `$1,234.56` 的返回项|
|-----------------------------|--------|-------------------------------|-------------------------:|
|@sys-currency               |字符串 |20                            |1234.56 |
|@sys-currency.literal       |字符串 |twenty dollars                |$1,234.56 |
|@sys-currency.numeric_value |数字   |20                            |1234.56 |
|@sys-currency.location      |数组   |[0,14]                        |[0,9] |
|@sys-currency.unit          |字符串 |USD*                          |USD |

*@sys-currency.unit 始终返回 3 个字母的 ISO 货币代码。

对于输入 `veinte euro` 或 <code>&euro;1.234,56</code>（西班牙语），@sys-currency 会返回以下值：

|属性|类型|针对 `veinte euro` 的返回项|针对 <code>&euro;1.234,56</code> 的返回项|
|-----------------------------|--------|-----------------------------|-------------------------:|
|@sys-currency               |字符串 |20                            |1234.56 |
|@sys-currency.literal       |字符串 |veinte euro                 |&euro;1.234,56 |
|@sys-currency.numeric_value |数字   |20                            |1234.56 |
|@sys-currency.location      |数组   |[0,11]                       |[0,9]|
|@sys-currency.unit          |字符串 |EUR*                        |EUR  |
*@sys-currency.unit 始终返回 3 个字母的 ISO 货币代码。

对于其他支持的语言和国家货币，将获得同样的结果。

### @system-currency 用法提示
{: #system-entities-currencty-usage-tips}

- 货币值会识别为 @sys-number 实体的实例。如果要使用单独的条件来检查货币值和数字，请将用于检查货币的条件放在用于检查数字的条件上方。

  如果使用的是已修改的系统实体，那么此变通方法不是必需的。有关更多详细信息，请参阅[新系统实体](/docs/services/assistant?topic=assistant-beta-system-entities)。
  {: note}

- 如果将 @sys-currency 实体用作节点条件，并且用户将 `$0` 指定为值，那么该值会正确识别为货币，但该条件会求值为数字零，而不是货币零。因此，条件中的 `null` 会求值为 false，并且不会处理节点。要检查货币值是否以正确方式处理零，请改为在节点条件中使用表达式 `@sys-currency >=0`。

## @sys-date 和 @sys-time 实体
{: #system-entities-sys-date-time}

`@sys-date` 系统实体用于抽取日期提及项，例如 `Friday`、`today` 或 `November 1`。此实体的值将相应的推断日期存储为“yyyy-MM-dd”格式（例如，“2016-11-21”）的字符串。系统使用当前日期值扩充日期的缺少元素（例如“11 月 21 日”的年份）。

**注：**- 仅对于英语语言环境，日期输入的缺省系统行为是 MM/DD/YYYY。仅当前两个数字大于 12 时，才会更改为 DD/MM/YYYY。存储的值仍为“yyyy-MM-dd”格式。

`@sys-time` 系统实体用于抽取时间提及项，例如 `2pm`、`at 4` 或 `15:30`。此实体的值将时间存储为“HH:mm:ss”格式的字符串。例如，“13:00:00”。

### 日期/时间提及项
{: #system-entities-date-time-mentions}

日期和时间的提及项，例如 `now` 或 `two hours from now` 会作为两个单独的实体提及项抽取：一个是 `@sys-date`，一个是 `@sys-time`。这两个提及项不相互链接，但共享跨完整日期/时间提及项的同一个文字串。

多词日期/时间提及项（例如，`on Monday at 4pm`）也会作为两个提及项抽取：@sys-date 和 @sys-time。这两个项若连续提及时，还会共享跨完整日期/时间提及项的单个文字串。

### 日期和时间范围
{: #system-entities-date-time-ranges}

日期范围提及项（例如，`the weekend`、`next week` 或 `from Monday to Friday`）作为一对 `@sys-date` 实体提及项抽取，以显示范围的开始日期和结束日期。同样，时间范围提及项（例如，`from 2 to 3`）也会作为两个 `@sys-time` 实体进行抽取，以显示开始时间和结束时间。该对中的两个实体共享一个与完整日期或时间范围提及项相对应的文字串。

### `Last` 和 `Next` 日期/时间
{: #system-entities-last-next}

在一些语言环境中，诸如“last Monday”这样的短语仅用于指定前一周的星期一。与之相对的，另一些语言环境使用“last Monday”指定最近的星期一，但这可能是同一周的星期一，也可能是前一周的星期一。

例如，对于 6 月 16 日星期五，在一些语言环境中“last Monday”可能指 6 月 12 日或 6 月 5 日，而在另一些语言环境中，仅指 6 月 5 日（前一周）。这种逻辑同样适用于像“next Monday”这样的短语。

{{site.data.keyword.conversationshort}} 服务将“last”和“next”日期视为是指所引用的最近的上一个日期或下一个日期（可能是同一周，也可能是前一周）。

对于像“for the last 3 days”或“in the next 4 hours”这样的时间短语，该逻辑同样适用。例如，对于“in the next 4 hours”，这将生成两个 `@sys-time` 实体：一个是当前时间，一个是晚于当前时间四小时的时间。

### 时区
{: #system-entities-time-zones}

相对于当前时间的日期或时间的提及项会针对所选时区进行解析。缺省情况下，时区为 UTC (GMT)。这意味着缺省情况下，位于非 UTC 时区中的 REST API 客户机将根据当前 UTC 时间来观察所抽取的 `now` 值。

REST API 客户机可以选择将本地时区添加为上下文变量 `$timezone`。此上下文变量应该随每个客户机请求一起发送。例如，`$timezone` 值应该为 `America/Los_Angeles`、`EST` 或 `UTC`。有关受支持时区的完整列表，请参阅[支持的时区](/docs/services/assistant?topic=assistant-time-zones)。

如果提供 `$timezone` 变量，那么将根据客户机时区（而不是 UTC）来计算相对 @sys-date 和 @sys-time 提及项的值。

#### 相对于时区的提及项示例
{: #system-entities-time-zone-examples}

- now
- in two hours
- today
- tomorrow
- 2 days from now

### 可识别的格式
{: #system-entities-sys-date-time-formats}

- November 21
- 10:30
- at 6 pm
- this weekend

### 返回项
{: #system-entities-sys-date-time-returns}

对于输入 `November 21`，@sys-date 会返回以下值：

|属性|类型|针对 `November 21` 的返回项|
|-------------------------|--------|---------------------------:|
|@sys-date.literal       |字符串 |November 21 |
|@sys-date               |字符串 |20xx-11-21 *|
|@sys-date.location      |数组   |[0,11]  |
|@sys-date.calendar_type |字符串 |GREGORIAN |

- @sys-date 始终返回以下格式的日期：yyyy-MM-dd。
- \* 返回下一个匹配的日期。如果该日期超过今年，那么将返回下一年的日期。

对于输入 `at 6 pm`，@sys-time 会返回以下值：

|属性|类型|针对 `at 6 pm` 的返回项|
|-------------------------|--------|-----------------------:|
|@sys-time.literal       |字符串 |at 6 pm |
|@sys-time               |字符串 |18:00:00 |
|@sys-time.location      |数组   |[0,7]|
|@sys-time.calendar_type |字符串 |GREGORIAN |

- @sys-time 始终返回以下格式的时间：HH:mm:ss。

有关处理日期和时间值的信息，请参阅[日期和时间方法参考](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-date-time)。
{: tip}

## @sys-location 实体
{: #system-entities-sys-location}

**BETA - 针对[支持的语言](/docs/services/assistant?topic=assistant-language-support)主题中注明的语言**：@sys-location 系统实体用于从用户的输入中抽取位置名称（国家或地区、州/省、城市、城镇等）。该实体的值不是位置的系统标准值。

### 可识别的格式
{: #system-entities-sys-location-formats}

- Boston
- U.S.A.
- New South Wales

有关处理字符串值的信息，请参阅[字符串方法参考](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings)。
{: tip}

## @sys-number 实体
{: #system-entities-sys-number}

@sys-number 系统实体用于检测使用数字或文字编写的数字。在两种情况下，都会返回数字值。

### 可识别的格式
{: #system-entities-sys-number-formats}

- 21
- twenty one
- 3.13

### 元数据
{: #system-entities-sys-number-metadata}

- `.numeric_value` - 作为整数或双精度值的规范数字值

### 返回项
{: #system-entities-sys-number-returns}

对于输入 `twenty` 或 `1,234.56`，@sys-number 会返回以下值：

|属性|类型|针对 `twenty` 的返回项|针对 `1,234.56` 的返回项|
|-----------------------------|--------|-------------------|------------------------:|
|@sys-number               |字符串 |20                            |1234.56 |
|@sys-number.literal       |字符串 |twenty            |1,234.56 |
|@sys-number.location      |数组   |[0,6]                 |[0,8]|
|@sys-number.numeric_value |数字   |20                            |1234.56 |

对于输入 `veinte` 或 `1.234,56`（西班牙语），@sys-number 会返回以下值：

|属性|类型|针对 `veinte` 的返回项|针对 `1.234,56` 的返回项|
|-----------------------------|--------|-----------------------|------------------------:|
|@sys-number               |字符串 |20                            |1234.56 |
|@sys-number.literal       |字符串 |veinte                |1.234,56 |
|@sys-number.location      |数组   |[0,6]                 |[0,8] |
|@sys-number.numeric_value |数字   |20                            |1234.56 |

对于其他支持的语言，将获得同样的结果。

### @system-number 用法提示
{: #system-entities-sys-number-usage-tips}

- 如果将 @sys-number 实体用作节点条件，并且用户将零指定为值，那么会将 0 值正确识别为数字。但是，0 会被解释为条件的 `null` 值，这会导致不处理节点。要检查数字是否以正确方式处理零，请改为在节点条件中使用表达式 `@sys-number >= 0`。

- 如果使用 @sys-number 来比较条件中的数字值，请确保单独包含一项检查来确认是否存在数字本身。如果找不到数字，那么 @sys-number 会求值为 null，这可能导致即便不存在任何数字，比较也会求值为 true。

  例如，不要单独使用 `@sys-number<4`，因为如果找不到数字，`@sys-number` 会求值为 null。由于 null 小于 4，因此即便不存在任何数字，条件也会求值为 true。

  请改为使用 `@sys-number AND @sys-number<4`。如果不存在任何数字，那么第一个条件求值为 false，这会相应地将整个条件求值为 false。

有关处理数字值的信息，请参阅[数字方法参考](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-numbers)。
{: tip}

## @sys-percentage 实体
{: #system-entities-sys-percentage}

@sys-percentage 系统实体用于检测在话语中使用百分号表示的百分比或使用文字 `percent` 书写的百分比。在两种情况下，都会返回数字值。

### 可识别的格式
{: #system-entities-sys-percentage-formats}

- 15%
- 10 percent

### 元数据
{: #system-entities-sys-percentage-metadata}

`.numeric_value`：作为整数或双精度值的规范数字值

### 返回项
{: #system-entities-sys-percentage-returns}

对于输入 `1,234.56%`，@sys-percentage 会返回以下值：

|属性|类型|针对 `1,234.56%` 的返回项|
|-------------------------------|--------|-------------------------:|
|@sys-percentage               |字符串 |1234.56 |
|@sys-percentage.literal       |字符串 |1,234.56% |
|@sys-percentage.location      |数组   |[0,9] |
|@sys-percentage.numeric_value |数字   |1234.56 |

对于输入 `1.234,56%`（西班牙语），@sys-currency 会返回以下值：

|属性|类型|针对 `1.234,56%` 的返回项|
|-------------------------------|--------|-------------------------:|
|@sys-percentage               |字符串 |1234.56 |
|@sys-percentage.literal       |字符串 |1.234,56% |
|@sys-percentage.location      |数组   |[0,9] |
|@sys-percentage.numeric_value |数字   |1234.56 |

对于其他支持的语言，将获得同样的结果。

### @system-percentage 用法提示
{: #system-entities-sys-percentage-usage-tips}

- 百分比值还会识别为 @sys-number 实体的实例。如果要使用单独的条件来检查百分比值和数字，请将用于检查百分比的条件放在用于检查数字的条件上方。

  如果使用的是已修改的系统实体，那么此变通方法不是必需的。有关更多详细信息，请参阅[新系统实体](/docs/services/assistant?topic=assistant-beta-system-entities)。
  {: note}

- 如果将 @sys-percentage 实体用作节点条件，并且用户将 `0%` 指定为值，那么该值会正确识别为百分比，但该条件会求值为数字零，而不是百分比 0%。因此，条件中的 `null` 会求值为 false，并且不会处理节点。要检查百分比是否以正确方式处理 0%，请改为在节点条件中使用表达式 `@sys-percentage >= 0`。

- 如果输入像 `1-2%` 这样的值，那么值 `1%` 和 `2%` 会作为系统实体返回。索引将是 1% 到 2% 之间的整个范围，并且这两个实体具有相同的索引。

## @sys-person 实体
{: #system-entities-sys-person}

**BETA - 针对[支持的语言](/docs/services/assistant?topic=assistant-language-support)主题中注明的语言**：@sys-person 系统实体用于从用户的输入中抽取姓名。姓名会分别识别，因此不会将“Joe”视为“Joseph”，也不会将“Joseph”视为“Joe”。该实体的值不是姓名的系统标准值。

### 可识别的格式
{: #system-entities-sys-person-formats}

- Ronald
- Jane Doe
- Vijay

有关处理字符串值的信息，请参阅[字符串方法参考](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings)。
{: tip}
