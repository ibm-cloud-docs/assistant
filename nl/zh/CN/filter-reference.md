---

copyright:
  years: 2015, 2017
lastupdated: "2017-09-18"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# 过滤器查询参考

{{site.data.keyword.conversationshort}} 服务 REST API 通过过滤器查询，提供功能强大的日志搜索功能。可以使用 /logs API `filter` 参数来搜索工作空间日志，以查找与指定查询匹配的事件。

`filter` 参数是可高速缓存的查询，用于将结果限制为与指定过滤器匹配的结果。可以过滤属于 JSON 响应模型的任何对象（例如，用户输入文本、检测到的意向和实体或置信度分数）。

要查看各种类型的过滤器查询的示例，请参阅[示例](#examples)。

有关 /logs `GET` 方法及其响应模型的更多信息，请参阅 [API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#get_logs){: new_window}。

## 过滤器查询语法

以下示例显示过滤器查询的一般格式：

| 位置| 查询运算符| 项|
|:------------------:|:--------------:|:------------:|
|`request.input.text`|`::`            |`"IBM Watson"`|

- _位置_确定要过滤的字段（在此示例中为 `request.input.text`）。
- _查询运算符_用于指定要使用的匹配类型（模糊匹配或完全匹配）。
- _项_指定要用于对字段求值以进行匹配的表达式或值。项可以包含字面值文本和运算符，如[下一节](#operators)中所述。

按意向或实体进行过滤需要的语法与过滤其他字段的略有不同。有关更多信息，请参阅[按意向或实体过滤](#intent_entity_filter)。

**注：**过滤器查询语法使用了 HTTP 查询中不允许的一些字符。请确保在作为 HTTP 查询的一部分发送时，对所有特殊字符（包括空格和引号）都进行了 URL 编码。例如，过滤器 `response_timestamp<2016-11-01` 将指定为 `response_timestamp%3C2016-11-01`。

## 运算符
<!-- {#operators} -->

可以在过滤器查询中使用以下运算符。

| 运算符| 描述|
|:-------------------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `:` | 模糊匹配查询运算符。如果要匹配包含查询项或查询项语法变体的任何值，请使用 `:` 作为查询项的前缀。模糊匹配可用于用户输入文本、响应输出文本和实体值。|
| `::` | 完全匹配查询运算符。如果要仅匹配完全等于查询项的值，请使用 `::` 作为查询项的前缀。|
| `:!` | 否定模糊匹配查询运算符。如果要仅匹配_不_包含查询项或查询项语法变体的值，请使用 `:!` 作为查询项的前缀。|
| `::!` | 否定完全匹配查询运算符。如果要仅匹配与查询项_不_完全匹配的值，请使用 `::!` 作为查询项的前缀。|
| `<=`, `>=`, `>`, `<` | 比较运算符。要基于算术比较进行匹配，请使用这些运算符作为查询项的前缀。|
| `\` | 转义运算符。用于包含控制字符的查询，不然这些控制字符会解析为运算符。例如，`\!hello` 将与文本 `!hello` 匹配。|
| `""` | 字面值短语。用于将包含空格或其他特殊字符的查询项括起。API 不会解析引号括起的特殊字符。|
| `~` | 近似匹配。将后跟 `1` 或 `2` 的此运算符附加到查询项末尾，以指定允许查询字符串与响应对象中的匹配项之间的单字符差异数。例如，`car~1` 将与 `car`、`cat` 或 `cars` 匹配，但不会与 `cats` 匹配。在对 `log_id` 或任何日期或时间字段进行过滤时，或者使用模糊匹配时，此运算符无效。|
| `*` | 通配符运算符。与零个或零个以上字符组成的任意序列匹配。在对 `log_id` 或任何日期或时间字段进行过滤时，此运算符无效。|
| `()`, `[]` | 分组运算符。用于通过布尔运算符将多个表达式的逻辑分组括起。|
| \| | 布尔值_或_运算符。|
| `,` | 布尔值_和_运算符。|

### 按意向或实体过滤
<!-- {#intent_entity_filter} -->

由于意向和实体的内部存储方式不同，因此用于过滤特定意向或实体的语法与返回的 JSON 中用于其他字段的语法不同。要在`意向`或`实体`集合内指定`意向`或`实体`字段，必须使用 `:` 匹配运算符，而不是使用点。

例如，以下查询匹配的记录事件是，响应包含检测到的名为 `hello` 的意向：

`response.intents:intent::hello`

请注意，`:` 运算符代替了点 (intents:intent)

使用同一模式可匹配响应中检测到的意向或实体的任何字段。例如，以下查询匹配的记录事件是，响应包含检测到的值为 `soda` 的实体：

`response.entities:value::soda`

同样，可以对作为请求的一部分发送的意向或实体进行过滤，如以下示例所示：

`request.intents:intent::hello`

**注**：对意向进行过滤会作用于所有检测到的意向。要仅过滤检测到的置信度最高的意向，可以使用 `response.top_intent` 简写语法，如以下示例中所示：

`response.top_intent::goodbye`

### 按其他字段过滤

要过滤日志数据中的其他任何字段，请将位置指定为路径，以确定来自 /logs API 的 JSON 响应中嵌套对象的级别。使用点 (`.`) 来指定 JSON 数据中的后续嵌套级别。例如，位置 `request.input.text` 确定用户输入文本字段，如以下 JSON 片段中所示：

```json
  "request": {
    "input": {
      "text": "Good morning"
    }
  }
```
<!-- {data-copy=false} -->

## 示例

以下示例说明使用此语法的各种类型的查询。

| 描述| 查询|
|---------|-----------|
| 响应日期为 2017 年 7 月。| `response_timestamp>=2017-07-01,response_timestamp<2017-08-01` |
| 响应的时间戳记早于 `2016-11-01T04:00:00.000Z`。| `response_timestamp<2016-11-01T04:00:00.000Z` |
| 用户输入文本包含单词“order”或语法变体（例如，`orders` 或 `ordering`）。| `request.input.text:order`|
| 响应中的意向名称与 `place_order` 完全匹配。| `response.intents:intent::place_order`|
| 响应中的实体名称与 `beverage` 完全匹配。| `response.entities:entity::beverage`|
| 用户输入文本不包含单词“order”或语法变体。| `request.input.text:!order`|
| 检测到的置信度最高的意向的名称与 `hello` 不完全匹配。| `response.top_intent::!hello`|
| 用户输入文本包含字符串 `!hello`。| `request.input.text:\!hello`|
| 用户输入文本包含字符串 `IBM Watson`。| `request.input.text:"IBM Watson"`|
| 用户输入文本包含的字符串与 `Watson` 的差异不超过 2 个单字符。| `request.input.text:Watson~2`|
| 用户输入文本包含的字符串由 `comm`，后跟零个或多个其他字符，再后跟 `s` 组成。| `request.input.text:comm*s`|
| 上下文中的部署标识与 `my_app` 匹配。| `request.context.metadata.deployment::my_app`|
| 响应中实体的置信度值大于 0.8。| `response.intents:confidence>0.8`|
| 响应中的意向名称与 `hello` 或 `goodbye` 完全匹配。| <code>response.intents:intent::(hello\|goodbye)</code> |
| 响应中意向的名称为 `hello` 且置信度值等于或大于 0.8。| `response.intents:(intent:hello,confidence>=0.8)`|
| 响应中的意向名称与 `order` 完全匹配，并且响应中的实体名称与 `beverage` 完全匹配。| `[response.intents:intent::order,response.entities:entity::beverage]` |
<!-- -->
