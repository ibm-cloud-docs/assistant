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
{:gif: data-image-type='gif'}

# 对话构建提示
{: #dialog-tips}

了解如何构建对话的方法，并获取有关完成更复杂步骤的一些提示。
{: shortdesc}

从有经验的对话设计人员处查看这些提示。

## 规划总体对话
{: #dialog-tips-plan}

- 在工具中添加单个对话节点之前，请规划要构建的对话的设计。根据需要，用纸笔草拟设计。
- 尽可能将设计决策基于来自现实世界证据和行为的数据。不要添加节点来处理有人*认为*可能发生的情况。
- 避免按原样复制业务流程。这些很少是会话式流程。
- 如果人们已使用流程，请检查他们是如何处理流程的。人们通常从会话角度来优化流程。
- 决定助手的语气、个性和定位。在创建的对话中，一致地反映这些选择。
- 绝不要将助手误当成真正的人类。如果用户认为助手是人类，随后发现其实并不是，那么用户很可能会不信任该助手。
- 并不是一切都必须是会话。有时 Web 表单的效果更好。

## 添加节点
{: #dialog-tips-nodes}

- 添加用于描述节点用途的节点名。

  您现在知道节点的用途，但过几个月后，很可能就不会记得了。添加描述性节点名后，未来无论对您自己还是对任何团队成员都有益。节点名会显示在日志中，这可帮助您日后调试会话。
- 要收集执行任务所需的信息，请尝试使用带槽的节点，而不要使用大量单独的节点来从用户探取信息。请参阅[使用槽收集信息](/docs/services/assistant?topic=assistant-dialog-slots)。
- 对于复杂的流程，请在流程开始时指示用户需要提供的任何信息。
- 了解服务如何遍历对话树，以及文件夹、分支、“跳转至”和离题对路径的影响。请参阅[对话流](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-flow)。
- 不要在所有位置添加“跳转至”。“跳转至”会增加对话流的复杂性，并增加日后调试对话的难度。
- 要跳转至当前节点所在分支中的节点，请使用*跳过用户输入*，而不要使用*跳转至*。

  除去要跳转至的子节点或对其重新排序时，此选项会使您不必编辑当前节点的设置。请参阅[定义后续操作](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-jump-to)。
- 允许从节点离题之前，请测试最常见的用户场景。确保将可能离题到的节点配置为返回。请参阅[离题](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions)。

## 添加响应
{: #dialog-tips-responses}

- 使回答保持简短、有用。
- 在响应中反映出用户的意向。

  这样做可让用户确信机器人理解他们的意图，或者如果无法理解，用户有机会立即更正误解之处。
- 仅当回答取决于频繁更改的数据时，才在响应中包含外部站点的链接。
- 避免过度使用按钮。鼓励用户从一组按钮中选择预定义的选项不太像真正的会话，而且会降低您了解用户真正希望做什么的能力。允许真实用户用自己的话进行询问时，可以使用输入来训练系统，并派生出更好的意向。
- 如果能用一个节点解决，就不用一堆节点。例如，根据用户提供的详细信息，将多个条件响应添加到单个节点以返回不同的响应。请参阅[条件响应](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multiple)。
- 认真对响应进行措辞。您可以仅仅根据响应的措辞方式就改变人们对系统的反应方式。更改一行文本可能会使您不必编写多行代码来实施复杂的程序化解决方案。
- 频繁备份技能。请参阅[下载技能](/docs/services/assistant?topic=assistant-skill-add#skill-add-download)。

## 有关从用户输入中捕获信息的提示
{: #dialog-tips-user-input}

很难知道在对话节点中使用什么语法，才能准确捕获要在用户输入中查找的信息。下面是可用于处理常见目标的一些方法。

- **返回用户的输入**：可以捕获用户所说的准确文本，并在响应中返回这些文本。在响应中使用以下 SpEL 表达式以在响应中复述用户指定的文本：

  `您表示：<? input.text ?>.`

- **确定用户输入中的字数**：可以对 input.text 对象执行任何支持的 String 方法。例如，可以使用以下 SpEL 表达式来了解用户发声中的字数：

  `input.text.split(' ').size()`

  要了解可以使用的更多方法，请参阅[用于 String 的表达式语言方法](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings)。

- **处理多个意向**：用户输入的内容表示希望完成两个单独的任务。`我要开立储蓄帐户并申请信用卡。`对话如何识别并处理这两个任务呢？有关可以试用的策略，请参阅 Simon O'Doherty 博客中的 [Compound questions](https://sodoherty.ai/2017/02/06/compound-questions/){: new_window} 部分。（Simon 是 {{site.data.keyword.conversationshort}} 团队的开发者。）

- **处理不明确意向**：用户输入的内容表示的意愿不够明确，因此服务发现两个或更多节点具有潜在解决此问题的意向。对话如何知道该执行哪个对话分支呢？如果启用了消歧，那么可以向用户显示其选项，并要求用户选取正确的选项。有关更多详细信息，请参阅[消歧](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)。

- **处理输入中的多个实体**：如果要仅对实体类型的检测到的第一个实例值求值，那么可以使用语法 `@entity == 'specific-value'`，而不使用 `@entity:(specific-value)` 格式。

  例如，使用 `@appliance == 'air conditioner'` 时，将仅对检测到的第一个 `@appliance` 实体的值求值。但是，使用 `@appliance:air conditioner` 会扩展到 `entity['appliance'].contains('air conditioner')`，只要在用户输入中检测到至少一个值为“空调”的 `@appliance` 实体，即为匹配。

## 条件用法提示
{: #dialog-tips-condition-usage}

- **检查具有特殊字符的值**：如果要检查实体或上下文变量是否包含某个值，并且该值包含特殊字符（如撇号 (')），那么必须使用圆括号将要检查的值括起。例如，要检查实体或上下文变量是否包含名称 `O'Reilly`，必须使用圆括号将该名称括起。

  `@person:(O'Reilly)` 和 `$person:(O'Reilly)`

  服务会将这些简写引用转换为以下完整的 SpEL 表达式：

  `entities['person']?.contains('O''Reilly')` 和 `context['person'] == 'O''Reilly'`

  对于名称中的单个撇号，SpEL 会使用另一个撇号对其转义。
  {: note}

- **检查多个值**：如果要检查多个值，可以创建一个条件，其中使用 OR 运算符 (`||`) 在条件中列出多个值。例如，要定义一个条件，以在上下文变量 `$state` 包含 Massachusetts、Maine 或 New Hampshire 的缩写时求值为 true，那么可以使用以下表达式：

  `$state:MA || $state:ME || $state:NH`

- **检查数字值**：比较数字时，首先确保要检查的实体或变量具有值。如果实体或变量没有数字值，那么在数字比较中，会将其视为空值 (0)。

  例如，您希望检查用户在其输入中指定的美元值是否小于 100。如果使用条件 `@price < 100`，但 `@price` 实体为空，那么条件会求值为 `true`，因为 0 小于 100，虽然从未设置过价格。要避免此类型的不准确结果，请使用诸如 `@price AND @price < 100` 之类的条件。如果 `@price` 没有值，那么此条件会正确返回 false。

- **检查具有特定意图名称模式的意向**：可以使用用于查找与模式相匹配的意向的条件。例如，要查找意向名称以“User_”开头的任何检测到的意向，可以在条件中使用类似以下内容的语法：

  `intents[0].intent.startsWith("User_")`

  但是，如果使用此语法，那么会考虑所有检测到的意向，甚至是置信度低于 0.2 的意向。此外，还会检查是否未返回 Watson 根据其置信度分数视为不相关的意向。为此，请按如下所示更改该条件：

  `!irrelevant && intents[0].intent.startsWith("User_")`

- **模糊匹配如何影响实体识别**：如果将实体用作条件并启用了模糊匹配，那么仅当匹配项的置信度大于 30% 时，`@entity_name` 才会求值为 true。即，仅当 `@entity_name.confidence > .3` 时才会求值为 true。

## 在输入中存储和识别实体模式组
{: #dialog-tips-get-pattern-groups}

要在上下文变量中存储模式实体的值，请将 .literal 附加到实体名称。使用此语法可确保将用户输入中与指定模式匹配的精确范围的文本存储在变量中。

|变量     |值               |
|------------|---------------------|
|email    | <? @email.literal ?>|

要在定义了组的模式实体中存储来自单个组的文本，请指定要存储的组的数组编号。例如，假定为 @phone_number 实体定义的实体模式如下所示。（请记住，圆括号表示模式组）：

`\b((958)|(555))-(\d{3})-(\d{4})\b`

要仅存储用户输入中指定的电话号码中的区号，可以使用以下语法：

|变量     |值               |
|----------------|-------------------------------|
|area_code| <? @phone_number.groups[1] ?>|

这些组由用于定义组模式的正则表达式定界。例如，如果与实体 `@phone_number` 中定义的模式匹配的用户输入是 `958-234-3456`，那么会创建以下组：

|组号         |正则表达式引擎值    |对话值         |说明        |
|--------------|---------------------|----------------|-------------|
|groups[0]    |`958-234-3456`      |`958-234-3456` |第一个组始终是完整的匹配字符串。|
|groups[1]    |`((958)`l`(555))`   |`958`          |与第一个已定义组的正则表达式相匹配的字符串，即 `((958)`l`(555))`。|
|groups[2]    |`(958)`             |`958`          |与包含在 OR 表达式 `((958)`l`(555))` 中作为第一个操作数的组相匹配 |
|groups[3]    |`(555)`             |`null`         |与包含在 OR 表达式 `((958)`l`(555))` 中作为第二个操作数的组不匹配 |
|groups[4]    |`(\d{3})`           |`234`          |与为组定义的正则表达式相匹配的字符串。|
|groups[5]    |`(\d{4})`           |`3456`         |与为组定义的正则表达式相匹配的字符串。|
{: caption="组详细信息" caption-side="top"}

为了帮助解码用于捕获您感兴趣的输入部分的组号，您可以一次抽取有关所有组的信息。使用以下语法创建上下文变量，该上下文变量返回所有分组模式实体匹配项的数组：

|变量     |值               |
|--------------------------|----------------------------|
|array_of_matched_groups| <? @phone_number.groups ?>|

使用“试用”窗格来输入某些测试电话号码值。对于输入 `958-123-2345`，此表达式会将 `$array_of_matched_groups` 设置为 `["958-123-2345","958","958",null,"123","2345"]`。

然后，可以对数组中的每个值从 0 开始进行计数，以获取其组号。

|数组元素值          |数组元素编号         |
|---------------------|----------------------|
|"958-123-2345"      |0 |
|"958"               |1 |
|"958"               |2 |
|null                |3 |
|"123"               |4 |
|"2345"              |5 |
{: caption="数组元素" caption-side="top"}

例如，根据结果，可以确定要捕获电话号码的最后四位数字，您需要组 #5。

要返回创建用于表示分组模式实体的 JSONArray 结构，请使用以下语法：

|变量     |值               |
|----------------------|---------------------------------|
|json_matched_groups| <? @phone_number.groups_json ?>|

此表达式将 `$json_matched_groups` 设置为以下 JSON 数组：

```json
[
  {"group": "group_0","location": [0, 12]},
  {"group": "group_1","location": [0, 3]},
  {"group": "group_2","location": [0, 3]},
  {"group": "group_3"},
  {"group": "group_4","location": [4, 7]},
  {"group": "group_5","location": [8, 12]}
]
```
{: codeblock}

`location` 是实体的属性，使用从零开始的字符偏移量来指示检测到的实体值在输入文本中的开始和结束位置。
{: note}

如果您期望在输入中提供两个电话号码，那么可以检查两个电话号码。例如，如果提供了两个电话号码，请使用以下语法来捕获第二个号码的区号。

|变量     |值               |
|------------------|---------------------------------------------|
|second_areacode| <? entities['phone_number'][1].groups[1] ?>|

如果输入是 `I want to change my phone number from 958-234-3456 to 555-456-5678`，那么 `$second_areacode` 等于 `555`。

## 查看 API 调用详细信息
{: #dialog-tips-inspect-api}

使用“试用”窗格测试对话时，您可能希望了解从服务返回的底层 API 调用的内容。可以使用 Web 浏览器提供的开发者工具来检查这些内容。

例如，在 Chrome 中，打开开发者工具。单击 Network 工具。Name 部分列出了多个 API 调用。单击与测试发声关联的消息调用，然后单击 Response 列以查看 API 响应主体。其中会列出在用户输入中识别到的意向和实体及其置信度分数，以及调用时上下文变量的值。要以结构化格式查看响应主体，请单击 Preview 列。

![显示如何使用 Chrome Web 浏览器开发者工具查看 API 调用详细信息。](images/api-browser-dev.png)
