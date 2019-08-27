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

# 用于访问对象的表达式
{: #expression-language}

可以使用 Spring 表达式 (SpEL) 语言来编写用于访问对象及其属性的表达式。有关更多信息，请参阅 [Spring 表达式语言 (SpEL) ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}。
{: shortdesc}

## 求值语法
{: #expression-language-long-syntax}

要在其他变量内部扩展变量值，或要对属性和全局对象调用方法，请使用 `<? expression ?>` 表达式语法。例如：

- **扩展属性**
    - `"output":{"text":"Your name is <? context.userName ?>"}`

- **对全局对象的属性调用方法**
    - `"context":{"email": "<? @email.literal ?>"}`

## 简写语法            
{: #expression-language-shorthand-syntax}

了解如何使用 SpEL 简写语法快速引用以下对象：

- [上下文变量](#expression-language-shorthand-context)
- [实体](#expression-language-shorthand-entities)
- [意向](#expression-language-shorthand-intents)

### 上下文变量的简写语法
{: #expression-language-shorthand-context}

下表显示了可用于在条件表达式中编写上下文变量的简写语法示例。

|简写语法                   |使用 SpEL 的完整语法                     |
|----------------------------|-----------------------------------------|
|`$card_type`               |`context['card_type']`                  |
|`$(card-type)`             |`context['card-type']`                  |
|`$card_type:VISA`          |`context['card_type'] == 'VISA'`        |
|`$card_type:(MASTER CARD)` |`context['card_type'] == 'MASTER CARD'` |

可以在上下文变量名称中包含特殊字符，例如连字符或句点。但是，这样做会导致对 SpEL 表达式求值时出现问题。例如，连字符可能解释为减号。要避免此类问题，请使用完整的表达式语法或简写语法 `$(variable-name)` 来引用该变量，并且不要在名称中使用以下特殊字符：

- 圆括号 ()
- 多个单引号 ''
- 引号 "

### 实体的简写语法
{: #expression-language-shorthand-entities}

下表显示了可以在引用实体时使用的简写语法示例。

|简写语法                   |使用 SpEL 的完整语法                     |
|---------------------|------------------------------------------|
|`@year`             |`entities['year']?.value`                |
|`@year == 2016`     |`entities['year']?.value == 2016`        |
|`@year != 2016`     |`entities['year']?.value != 2016`        |
|`@city == 'Boston'` |`entities['city']?.value == 'Boston'`    |
|`@city:Boston`      |`entities['city']?.contains('Boston')`   |
|`@city:(New York)`  |`entities['city']?.contains('New York')` |

在 SpEL 中，问号 `(?)` 会阻止实体对象为空时触发空指针异常。

如果要检查的实体值包含 `)` 字符，那么不能使用 `:` 运算符进行比较。例如，如果要检查 city 实体是否为 `Dublin (Ohio)`，那么必须使用 `@city == 'Dublin (Ohio)'`，而不能使用 `@city:(Dublin(Ohio))`。

### 意向的简写语法
{: #expression-language-shorthand-intents}

下表显示了可以在引用意向时使用的简写语法示例。

<table>
  <caption>意向简写语法</caption>
  <tr>
    <th>简写语法            </th>
    <th>使用 SpEL 的完整语法</th>
  </tr>
  <tr>
    <td>`#help`</td>                 <td>`intent == 'help'`</td>  </tr>
  <tr>
    <td>`! #help`</td>               <td>`intent != 'help'`</td>  </tr>
  <tr>
    <td>`NOT #help`</td>             <td>`intent != 'help'`</td>  </tr>
  <tr>
    <td>`#help` 或 `#i_am_lost`</td> <td>`(intent == 'help' || intent == 'I_am_lost')`</td>
  </tr>
</table>

## 内置全局变量
{: #expression-language-builtin-vars}

可以使用表达式语言抽取以下全局变量的属性信息：

|全局变量             |定义       |
|----------------------|------------|
|*context*            |已处理会话消息的 JSON 对象部分。|
|*entities[ ]*        |支持缺省访问第一个元素的实体的列表。|
|*input*              |已处理会话消息的 JSON 对象部分。|
|*intents[ ]*         |支持缺省访问第一个元素的意向的列表。|
|*output*             |已处理会话消息的 JSON 对象部分。|

## 访问实体
{: #expression-language-access-entity}

实体数组包含在用户输入中识别的一个或多个实体。

测试对话时，通过在对话节点响应中指定以下表达式，可以查看在用户输入中识别到的实体的详细信息：

```json
<? entities ?>
```
{: codeblock}

对于用户输入 *Hello now*，助手可识别到 @sys-date 和 @sys-time 系统实体，因此响应包含以下实体对象：

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

### 在输入内容中放置实体
{: #expression-language-placement-matters}

使用简写表达式 `@city.contains('Boston')` 时，在条件中，**仅当** `Boston` 是在用户输入中检测到的第一个实体时，对话节点才会返回 true。仅当输入中实体的位置对您很重要，并且您希望只检查第一个提及项时，才使用此语法。

如果希望任何时候用户输入中提及该词汇时，条件都返回 true（与这些实体提及的顺序无关），请使用完整的 SpEL 表达式。如果在所有 @city 实体中至少有一个“Boston”city 实体，那么无论该实体放置在哪，条件 `entities['city']?.contains('Boston')` 都会返回 true。

例如，用户提交 `"I want to go from Toronto to Boston."`。系统将检测到 `@city:Toronto` 和 `@city:Boston` 实体，并在返回的数组中表示，如下所示：

- `entities.city[0].value = 'Toronto'`
- `entities.city[1].value = 'Boston'`

返回的数组中实体的顺序与用户输入中提及这些实体的顺序相匹配。
{: note}

### 实体属性
{: #expression-language-entity-props}

每个实体都有一组与之关联的属性。可以通过实体的属性访问有关实体的信息。

|属性                  |定义       |用法提示   |
|-----------------------|------------|------------|
|*confidence*          |表示助手对已识别实体的置信度的小数百分比。除非激活了实体模糊匹配，否则实体的置信度为 0 或 1。启用模糊匹配后，缺省置信度级别阈值为 0.3。无论是否启用模糊匹配，系统实体的置信度级别都始终为 1.0。|可以在条件中使用此属性，在置信度级别不高于您指定的百分比时，使其返回 false。|
|*location*            |从零开始的字符偏移量，用于指示检测到的实体值在输入文本中的起始和结束位置。|使用 `.literal` 可抽取 location 属性中存储的起始索引值与结束索引值之间的文本范围。|
|*value*               |在输入中识别到的实体值。|此属性返回在训练数据中定义的实体值，即便已针对其某个关联同义词进行了匹配也不例外。可以使用 `.values` 来捕获可能在用户输入出现多次的实体。|

### 实体属性用法示例
{: #expression-language-entity-props-example}

在以下示例中，技能包含 airport 实体，此实体包含值 JFK 和同义词“Kennedy Airport”。用户输入为 *I want to go to Kennedy Aiport*。

- 要在用户输入中识别到“JFK”实体时返回特定响应，可以将以下表达式添加到响应条件：`entities.airport[0].value == 'JFK'` 或 `@airport = "JFK"`
- 要返回由用户在对话响应中指定的实体名称，请使用 .literal 属性：
  `So you want to go to <?entities.airport[0].literal?>...`
  或
  `So you want to go to @airport.literal ...`

  这两种格式都会在响应中求值为“So you want to go to Kennedy Airport...”。

- 像 `@airport:(JFK)` 或 `@airport.contains('JFK')` 这样的表达式始终引用实体（在此示例中为 `JFK`）的**值**。
- 例如，要在启用模糊匹配时对输入中识别为机场的词汇进行更强的限制，可以在节点条件中指定以下表达式：`@airport && @airport.confidence > 0.7`。仅当助手对输入文本包含机场引用的置信度超过 70% 时，才会执行此节点。

在此示例中，用户输入为 *Are there places to exchange currency at JFK, Logan, and O'Hare?*

- 要捕获在用户输入中多次出现的实体类型，请使用如下语法：

    ```json
    "context":{
      "airports":"@airport.values"
    }
    ```

  要稍后在对话响应中引用捕获的列表，请使用以下语法：
  `You asked about these airports: <? $airports.join(', ') ?>.`
  显示的响应类似于：`You asked about these airports: JFK, Logan, O'Hare.`

## 访问意向
{: #expression-language-intent}

意向数组包含在用户输入中已识别到的一个或多个意向，按置信度降序排序。

每个意向只有一个属性：`confidence` 属性。confidence 属性为小数百分比，表示助手对识别到的意向的置信度。

测试对话时，通过在对话节点响应中指定以下表达式，可以查看在用户输入中识别到的意向的详细信息：

```json
<? intents ?>
```
{: codeblock}

对于用户输入 *Hello now*，助手找到了与 #greeting 意向的完全匹配。因此，服务首先列出 #greeting 意向对象的详细信息。响应还包含在技能中定义的排名前 10 的其他意向，这些意向与置信度分数无关。（在此示例中，由于第一个意向是完全匹配，因此它在其他意向中的置信度设置为 0。）由于“试用”窗格在其请求中发送了 `alternate_intents:true` 参数，因此返回了排名前 10 的意向。如果要直接使用 API，并希望查看排名前 10 的结果，请确保在调用中指定此参数。如果 `alternate_intents` 为 false（这是缺省值），那么在数组中仅返回置信度高于 0.2 的意向。

```json
[{"intent":"greeting","confidence":1},
{"intent":"yes","confidence":0},
{"intent":"pizza-order","confidence":0}]
```
{: codeblock}

以下示例显示如何检查意向值：

- `intents[0] == 'Help'`
- `intent == 'Help'`

`intent == 'help'` 不同于 `intents[0] == 'help'`，因为如果未检测到意向，`intent == 'help'` 不会抛出异常。仅当意向置信度超过阈值时，它才会求值为 true。如果需要，可以为条件指定定制置信度级别，例如 `intents.size() > 0 && intents[0] == 'help' && intents[0].confidence > 0.1`

## 访问输入
{: #expression-language-intent-props}

输入 JSON 对象仅包含一个属性：text 属性。text 属性表示用户输入的文本。

### 输入属性用法示例
{: #expression-language-intent-props-example}

以下示例显示如何访问输入：

- 要在用户输入为“Yes”时执行节点，请向节点条件添加以下表达式：`input.text == 'Yes'`

可以使用任何[字符串方法](/docs/services/assistant/dialog-methods#dialog-methods-strings)对用户输入的文本进行求值或处理。例如：

- 要检查用户输入是否包含“Yes”，请使用：`input.text.contains( 'Yes' )`。
- 如果用户输入的是数字，那么返回 true：`input.text.matches( '[0-9]+' )`。
- 要检查输入字符串是否包含 10 个字符，请使用：`input.text.length() == 10`。
