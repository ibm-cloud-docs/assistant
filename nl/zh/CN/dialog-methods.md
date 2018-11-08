---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-05"

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

# 表达式语言方法

可以处理从用户发声中抽取的值，这些值将根据您的需要在上下文变量、条件或响应中的其他地方进行引用。
{: shortdesc}

## 求值语法

要在其他变量内部扩展变量值，或要将方法应用于输出文本或上下文变量，请使用 `<? expression ?>` 表达式语法。例如：

- **递增数字属性**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **对对象调用方法**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

以下各部分描述可用于处理值的方法。这些值按数据类型进行组织：

- [数组](dialog-methods.html#arrays)
- [日期和时间](dialog-methods.html#date-time)
- [数字](dialog-methods.html#numbers)
- [对象](dialog-methods.html#objects)
- [字符串](dialog-methods.html#strings)

## 数组
{: #arrays}

要检查某个节点条件或响应条件中是否存在某个数组中的值，不能在设置了这些数组值的同一节点中使用以下方法进行检查。

### JSONArray.append(object)

此方法用于将新值附加到 JSONArray，并返回修改后的 JSONArray。

对于以下对话运行时上下文：

```json
{
  "context": {
   "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

进行以下更新：

```json
{
  "context": {
   "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
  }
}
```
{: codeblock}

结果：

```json
{
  "context": {
   "toppings_array": ["onion", "olives", "ketchup", "tomatoes"]
  }
}
```
{: codeblock}

### JSONArray.contains(object value)

如果输入 JSONArray 包含输入值，那么此方法会返回 true。

对于由前一个节点或外部应用程序设置的以下对话运行时上下文：

```json
{
  "context": {
   "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

对话节点或响应条件：

```json
$toppings_array.contains('ham')
```
{: codeblock}

结果：`True`，因为数组包含元素 ham。

### JSONArray.get(integer)

此方法用于返回 JSONArray 中的输入索引。

对于由前一个节点或外部应用程序设置的以下对话运行时上下文：

```json
{
  "context": {
   "name": "John",
    "nested": {
      "array": [ "one", "two" ]
    }
  }
}
```
{: codeblock}

对话节点或响应条件：

```json
$nested.array.get(0).getAsString().contains('one')
```
{: codeblock}

结果：`True`，因为嵌套数组包含 `one` 作为值。

响应：

```json
"output": {
    "text": {
      "values": [
        "The first item in the array is <?$nested.array.get(0)?>"
      ],
      "selection_policy": "sequential"
    }
  }
```
{: codeblock}

### JSONArray.getRandomItem()

此方法用于返回输入 JSONArray 中的随机项。

对于以下对话运行时上下文：

```json
{
  "context": {
   "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

对话节点输出：

```json
{
  "output": {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
  }
}
```
{: codeblock}

结果：`"ham is a great choice!"`、`"onion is a great choice!"` 或 `"olives is a great choice!"`

**注：**生成的输出文本是随机选择的。

### JSONArray.join(string delimiter)

此方法用于将此数组中的所有值连接成一个字符串。值将转换为字符串，并由输入定界符进行定界。

对于以下对话运行时上下文：

```json
{
  "context": {
   "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

对话节点输出：

```json
{
  "output": {
    "text": "This is the array: <? $toppings_array.join(';') ?>"
  }
}
```
{: codeblock}

结果：

```json
This is the array: onion;olives;ham;
```
{: codeblock}

### JSONArray.remove(integer)

此方法用于从 JSONArray 除去索引位置中的元素，并返回更新后的 JSONArray。

对于以下对话运行时上下文：

```json
{
  "context": {
   "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

进行以下更新：

```json
{
  "context": {
   "toppings_array": "<? $toppings_array.remove(0) ?>"
  }
}
```
{: codeblock}

结果：

```json
{
  "context": {
   "toppings_array": ["olives"]
  }
}
```
{: codeblock}

### JSONArray.removeValue(object)

此方法用于从 JSONArray 中除去第一次出现的值，并返回更新后的 JSONArray。

对于以下对话运行时上下文：

```json
{
  "context": {
   "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

进行以下更新：

```json
{
  "context": {
   "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
  }
}
```
{: codeblock}

结果：

```json
{
  "context": {
   "toppings_array": ["olives"]
  }
}
```
{: codeblock}

### JSONArray.set(integer index, object value)

此方法用于将 JSONArray 的输入索引设置为输入值，并返回修改后的 JSONArray。

对于以下对话运行时上下文：

```json
{
  "context": {
   "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

对话节点输出：

```json
{
  "context": {
   "toppings_array": "<? $toppings_array.set(1,'ketchup')?>"
  }
}
```
{: codeblock}

结果：

```json
{
  "context": {
   "toppings_array": ["onion", "ketchup", "ham"]
  }
}
```
{: codeblock}

### JSONArray.size()

此方法用于以整数形式返回 JSONArray 的大小。

对于以下对话运行时上下文：

```json
{
  "context": {
   "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

进行以下更新：

```json
{
  "context": {
   "toppings_array_size": "<? $toppings_array.size() ?>"
  }
}
```
{: codeblock}

结果：

```json
{
  "context": {
   "toppings_array_size": 2
  }
}
```
{: codeblock}

### JSONArray split(String regexp)

此方法用于通过输入正则表达式来拆分输入字符串。结果为字符串的 JSONArray。

对于以下输入：

```
"bananas;apples;pears"
```
{: codeblock}

以下语法：

```json
{
  "context": {
   "array": "<?input.text.split(";")?>
  }
}
```
{: codeblock}

将生成以下输出：

```json
{
  "context": {
   "array": [ "bananas", "apples", "pears" ]
  }
}
```
{: codeblock}

### com.google.gson.JsonArray 支持
{: #com.google.gson.JsonArray}

除了内置方法外，还可以使用 `com.google.gson.JsonArray` 类的标准方法。

#### 新增数组

新增 JsonArray().append('value')

要定义将使用用户提供的值进行填充的新数组，可以对数组实例化。对数组实例化后，还必须向数组中添加一个占位符值。可以使用以下语法来执行此操作：

```json
{
  "context":{
    "answer": "<? output.answer?:new JsonArray().append('temp_value') ?>"
  }
```
{: codeblock}

## 日期和时间
{: #date-time}

有多种方法可用于处理日期和时间。

有关如何在用户输入中识别和抽取日期和时间信息的信息，请参阅 [@sys-date 和 @sys-time 实体](system-entities.html#sys-datetime)。

### .after(String date/time)

- 确定日期/时间值是否晚于 date/time 自变量。
- 类似于 `.before()`。

### .before(String date or time)

- 例如：
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')
- 确定日期/时间值是否早于 date/time 自变量。
- 如果比较不同的项（例如，`时间与日期`、`日期与时间`以及`时间与日期和时间`），那么此方法会返回 false，并且会在响应 JSON 日志 `output.log_messages` 中输出异常。
    - 例如，`@sys-date.before(@sys-time)`。
- 如果比较`日期和时间与时间`，那么此方法会忽略日期而仅比较时间。

### now()

- 静态函数。
- 返回 `yyyy-MM-dd HH:mm:ss` 格式的当前日期和时间的字符串。
- 可以对此函数返回的日期/时间值调用其他日期/时间方法，并且可以将其作为这些方法的自变量传递。
- 如果设置了上下文变量 `$timezone`，那么此函数会返回客户机时区中的日期和时间。

在输出字段中使用了 `now()` 的对话节点示例：

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "text": "<? now() ?>"
   }
}
```
{: codeblock}

节点条件中的 `now()` 示例（用于确定现在是否仍是上午）：

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
    "text": "Good morning!"
   }
}
```
{: codeblock}

### .reformatDateTime(String format)

将日期和时间字符串的格式设置为用户输出所需的格式。

根据指定格式返回已设置格式的字符串：

- `MM/dd/yyyy` 返回 12/31/2016
- `h a` 返回 10pm

要返回星期几：

- `E` 表示星期二
- `u` 表示星期几索引 (1 = 星期一、...、7 = 星期日）

例如，此上下文变量定义会创建 $time 变量，用于将值 17:30:00 保存为 *5:30 PM*。

```json
{
  "context": {
   "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

格式遵循 Java [SimpleDateFormat ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html){: new_window} 规则。

**注**：尝试仅设置时间的格式时，日期会视为 `1970-01-01`。

### .sameMoment(String date/time)

- 确定日期/时间值是否等于 date/time 自变量。

### .sameOrAfter(String date/time)

- 确定日期/时间值是否晚于或等于 date/time 自变量。
- 类似于 `.after()`。

### .sameOrBefore(String date/time)

- 确定日期/时间值是否早于或等于 date/time 自变量。

### java.util.Date 支持
{: #java.util.Date}

除了内置方法外，还可以使用 `java.util.Date` 类的标准方法。

#### 日期计算

要获取从今天起一周之后的日期，可以使用以下语法。

```json
{
  "context": {
   "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))) ?>"
  }
}
```
{: codeblock}

此表达式首先获取当前日期，以毫秒为单位（自 1970 年 1 月 1 日 00:00:00 GMT 起）。此表达式还会计算 7 天的毫秒数。（`(24*60*60*1000L)` 表示一天的毫秒数。）然后，将当前日期加上 7 天。结果就是从今天起一周之后的完整日期。例如，`Fri Jan 26 16:30:37 UTC 2018`。请注意，时间采用 UTC 时区。您始终可以将 7 更改为可以传入的变量（例如，`$number_of_days`）。只需确保在对此表达式求值之前已设置该变量的值。

如果您希望能够随后将日期与服务生成的另一日期进行比较，那么必须重新设置日期的格式。系统实体 (`@sys-date`) 和其他内置方法 (`now()`) 会将日期转换为 `yyyy-MM-dd` 格式。

```json
{
  "context": {
   "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))).format('yyyy-MM-dd') ?>"
  }
}
```
{: codeblock}

重新设置日期的格式后，结果为 `2018-01-26`。现在，您可以在响应条件中使用表达式（如 `@sys-date.after($week_from_today)`）将用户输入中指定的日期与保存在上下文变量中的日期进行比较。

以下表达式计算从此刻起 3 小时之后的时间。

```json
{
  "context": {
   "future_time": "<? new Date(new Date().getTime() + (3 * (60*60*1000L)) -
      (5 * (60*60*1000L))).format('h:mm a') ?>"
  }
}
```
{: codeblock}

`(60*60*1000L)` 值表示一小时的毫秒数。此表达式将当前时间加上 3 小时。然后，通过从结果中减去 5 小时，将 UTC 时区的时间重新计算为 EST 时区的时间。此表达式还会重新设置日期值的格式，以包含小时和分钟以及“上午”或“下午”。

## 数字
{: #numbers}

这些方法可帮助您获取数字值并重新设置其格式。

有关可以在用户输入中识别和抽取数字的系统实体的信息，请参阅 [@sys-number 实体](system-entities.html#sys-number)。

如果您希望服务识别用户输入中的特定数字格式（例如，订单号引用），请考虑创建模式实体来进行捕获。有关更多详细信息，请参阅[创建实体](entities.html#creating-entities)。

### toDouble()

  将对象或字段转换为 Double 数字类型。可以对任何对象或字段调用此方法。如果转换失败，会返回 *null*。

### toInt()

  将对象或字段转换为 Integer 数字类型。可以对任何对象或字段调用此方法。如果转换失败，会返回 *null*。

### toLong()

  将对象或字段转换为 Long 数字类型。可以对任何对象或字段调用此方法。如果转换失败，会返回 *null*。

  如果在 SpEL 表达式中指定了长整型数字类型，那么必须在该数字后附加 `L` 进行标识。例如，`5000000000L`。对于任何不适合 32 位整数类型的数字，此语法都是必需的。例如，大于 2^31 (2,147,483,648) 或小于 -2^31 (-2,147,483,648) 的数字被视为长整型数字类型。长整型数字类型的最小值为 -2^63，最大值为 2^63-1。

### Java 数字支持
{: #java.lang.Number}

### java.lang.Math()

执行基本数字运算。

可以使用 Class 方法，包括以下方法：

- max()

```json
{
  "context": {
   "bigger_number": "<? T(Math).max($number1,$number2) ?>"
  },
  "output": {
    "text": {
      "values": [
        "最大的数字是 $bigger_number。"
      ],
      "selection_policy": "sequential"
    }
  }
}
```
{: codeblock}

- min()

```json
{
  "context": {
   "smaller_number": "<? T(Math).min($number1,$number2) ?>"
  },
  "output": {
    "text": {
      "values": [
        "最小的数字是 $smaller_number。"
      ],
      "selection_policy": "sequential"
    }
  }
}
```
{: codeblock}

- pow()

```json
{
  "context": {
   "power_of_two": "<? T(Math).pow($base.toDouble(),2.toDouble()) ?>"
  },
  "output": {
    "text": {
      "values": [
        "您的数字 $base 的二次幂等于 $power_of_two。"
      ],
      "selection_policy": "sequential"
    }
  }
}
```
{: codeblock}

有关其他方法的信息，请参阅 [java.lang.Math 参考文档](https://docs.oracle.com/javase/7/docs/api/java/lang/Math.html)。

### java.util.Random()

返回一个随机数。可以使用以下其中一个语法选项：

- 要返回随机布尔值（true 或 false），请使用 `<?new Random().nextBoolean()?>`.
- 要返回 0（含 0）和 1（不含 1）之间的随机双精度数，请使用 `<?new Random().nextDouble()?>`
- 要返回 0（含 0）和指定数字之间的随机整数，请使用 `<?new Random().nextInt(n)?>`，其中 n 等于您需要的数字范围上限 + 1。例如，如果要返回 0 到 10 之间的随机数，请指定 `<?new Random().nextInt(11)?>`.
- 要返回完整 Integer 值范围（-2147483648 到 2147483648）内的随机整数，请使用 `<?new Random().nextInt()?>`.

例如，可以创建由 #random_number 意向触发的对话节点。第一个响应条件可能类似于：

```json
Condition = @sys-number
{
  "context": {
   "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
  },
  "output": {
    "text": {
      "values": [
        "Here's a random number between 0 and @sys-number.literal: $answer."
      ],
      "selection_policy": "sequential"
    }
  }
}
```
{: codeblock}

有关其他方法的信息，请参阅 [java.util.Random 参考文档](https://docs.oracle.com/javase/7/docs/api/java/util/Random.html)。

还可以使用以下类的标准方法：

- `java.lang.Byte`
- `java.lang.Integer`
- `java.lang.Long`
- `java.lang.Double`
- `java.lang.Short`
- `java.lang.Float`

## 对象
{: #objects}

### JSONObject.has(string)

如果复杂 JSONObject 具有输入名称的属性，那么此方法会返回 true。

对于以下对话运行时上下文：

```json
{
  "context": {
   "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

对话节点输出：

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

结果：条件为 true，因为用户对象包含属性 `first_name`。

### JSONObject.remove(string)

此方法用于从输入 `JSONObject` 中除去名称的属性。此方法返回的 `JSONElement` 是要除去的 `JSONElement`。

对于以下对话运行时上下文：

```json
{
  "context": {
   "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

对话节点输出：

```json
{
  "context": {
   "attribute_removed": "<? $user.remove('first_name') ?>"
  }
}
```
{: codeblock}

结果：

```json
{
  "context": {
   "user": {
      "last_name": "Snow"
    },
    "attribute_removed": {
      "first_name": "John"
    }
  }
}
```
{: codeblock}

### com.google.gson.JsonObject 支持
{: #com.google.gson.JsonObject}

除了内置方法外，还可以使用 `com.google.gson.JsonObject` 类的标准方法。

## 字符串
{: #strings}

这些方法有助于您处理文本。

有关如何识别和抽取用户输入中特定类型 String（例如，人名和位置）的信息，请参阅[系统实体](system-entities.html)。

**注：**有关涉及正则表达式的方法，请参阅 [RE2 语法参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/google/re2/wiki/Syntax){: new_window}，以了解有关指定正则表达式时使用的语法的详细信息。

### String.append(object)

此方法用于将输入对象作为字符串附加到字符串，并返回修改后的字符串。

对于以下对话运行时上下文：

```json
{
  "context": {
   "my_text": "This is a text."
  }
}
```
{: codeblock}

以下语法：

```json
{
  "context": {
   "my_text": "<? $my_text.append(' More text.') ?>"
  }
}
```
{: codeblock}

将生成以下输出：

```json
{
  "context": {
   "my_text": "This is a text. More text."
  }
}
```
{: codeblock}

### String.contains(string)

如果字符串包含输入子字符串，那么此方法会返回 true。

输入："Yes, I'd like to go."

以下语法：

```json
{
  "conditions": "input.text.contains('Yes')"
}
```
{: codeblock}

结果：条件为 `true`。

### String.endsWith(string)

如果字符串以输入子字符串结尾，那么此方法会返回 true。

对于以下输入：

```
"What is your name?".
```
{: codeblock}

以下语法：

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{: codeblock}

结果：条件为 `true`。

### String.extract(String regexp, Integer groupIndex)

此方法用于返回由输入正则表达式的指定组索引所抽取的字符串。

对于以下输入：

```
"Hello 123456".
```
{: codeblock}

以下语法：

```json
{
  "context": {
   "number_extract": "<? input.text.extract('[\\d]+',0) ?>"
  }
}
```
{: codeblock}

  **重要信息：**要将 `\\d` 作为正则表达式处理，必须通过再添加一组 `\\` 对原来的两个反斜杠进行转义：`\\\\d`

结果：

```json
{
  "context": {
   "number_extract": "123456"
  }
}
```
{: codeblock}

### String.find(string regexp)

如果字符串的任何分段与输入正则表达式匹配，那么此方法会返回 true。可以对 JSONArray 或 JSONObject 元素调用此方法，在进行比较之前，此方法会将数组或对象转换为字符串。

对于以下输入：

```
"Hello 123456".
```
{: codeblock}

以下语法：

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

结果：条件为 true，因为输入文本的数字部分与正则表达式 `^[^\d]*[\d]{6}[^\d]*$` 匹配。

### String.isEmpty()

如果字符串为空字符串，但不是 null，那么此方法会返回 true。

对于以下对话运行时上下文：

```json
{
  "context": {
   "my_text_variable": ""
  }
}
```
{: codeblock}

以下语法：

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{: codeblock}

结果：条件为 `true`。

### String.length()

此方法用于返回字符串的字符长度。

对于以下输入：

```
"Hello"
```
{: codeblock}

以下语法：

```json
{
  "context": {
   "input_length": "<? input.text.length() ?>"
  }
}
```
{: codeblock}

将生成以下输出：

```json
{
  "context": {
   "input_length": 5
  }
}
```
{: codeblock}

### String.matches(string regexp)

如果字符串与输入正则表达式匹配，那么此方法会返回 true。

对于以下输入：

```
"Hello".
```
{: codeblock}

以下语法：

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{: codeblock}

结果：条件为 true，因为输入文本与正则表达式 `\^Hello\$` 匹配。

### String.startsWith(string)

如果字符串以输入子字符串开头，那么此方法会返回 true。

对于以下输入：

```
"What is your name?".
```
{: codeblock}

以下语法：

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{: codeblock}

结果：条件为 `true`。

### String.substring(int beginIndex, int endIndex)

此方法用于获取子字符串，其第一个字符位于 `beginIndex`，最后一个字符设置为 `endIndex` 之前的索引。不包括 endIndex 字符。

对于以下对话运行时上下文：

```json
{
  "context": {
   "my_text": "This is a text."
  }
}
```
{: codeblock}

以下语法：

```json
{
  "context": {
   "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{: codeblock}

将生成以下输出：

```json
{
  "context": {
   "my_text": "is a text."
  }
}
```
{: codeblock}

### String.toLowerCase()

此方法用于返回原始 String 转换为小写字母之后的结果。

对于以下输入：

```
"This is A DOG!"
```
{: codeblock}

以下语法：

```json
{
  "context": {
   "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{: codeblock}

将生成以下输出：

```json
{
  "context": {
   "input_upper_case": "this is a dog!"
  }
}
```
{: codeblock}

### String.toUpperCase()

此方法用于返回原始 String 转换为大写字母之后的结果。

对于以下输入：

```
"hi there".
```
{: codeblock}

以下语法：

```json
{
  "context": {
   "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{: codeblock}

将生成以下输出：

```json
{
  "context": {
   "input_upper_case": "HI THERE"
  }
}
```
{: codeblock}

### String.trim()

此方法用于删除字符串开头和结尾的所有空格，并返回修改后的字符串。

对于以下对话运行时上下文：

```json
{
  "context": {
   "my_text": "   something is here    "
  }
}
```
{: codeblock}

以下语法：

```json
{
  "context": {
   "my_text": "<? $my_text.trim() ?>"
  }
}
```
{: codeblock}

将生成以下输出：

```json
{
  "context": {
   "my_text": "something is here"
  }
}
```
{: codeblock}

### java.lang.String 支持
{: #java.lang.String}

除了内置方法外，还可以使用 `java.lang.String` 类的标准方法。

#### java.lang.String.format()

可以将标准 Java String `format()` 方法应用于文本。有关用于指定格式详细信息的语法的信息，请参阅 [java.util.formatter 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter.html#syntax){: new_window}。

例如，以下表达式采用 3 个十进制整数（1、1 和 2）并将它们添加到语句中。

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d equals %d', 1, 1, 2) ?>"
}
```
{: codeblock}

结果：`1 + 1 equals 2`。

## 间接数据类型转换

例如，在文本中包含表达式作为节点响应的一部分时，该值会呈现为 String。如果希望表达式以其原始数据类型呈现，那么不要将其包含在文本内。

例如，可以将以下表达式添加到对话节点响应，以便以 String 格式返回用户输入中识别到的实体：

```json
  The entities are <? entities ?>.
```
{: codeblock}

如果用户将 *Hello now* 指定为输入，那么 `now` 引用将触发 @sys-date 和 @sys-time 实体。实体对象是一个数组，但由于表达式包含在文本中，因此实体以 String 格式返回，如下所示：

```json
  The entities are 2018-02-02, 14:34:56.
```
{: codeblock}

如果响应中不包含文本，那么会改为返回数组。例如，如果响应仅指定为表达式，而不是包含在文本内。

```
  <? entities ?>
```
{: codeblock}

实体信息作为数组以其本机数据类型返回。

```json
[
  {
    "entity":"sys-date","location":[6,9],"value":"2018-02-02","confidence":1,"metadata":{"calendar_type":"GREGORIAN","timezone":"America/New_York"}
  },
  {
    "entity":"sys-time","location":[6,9],"value":"14:33:22","confidence":1,"metadata":{"calendar_type":"GREGORIAN","timezone":"America/New_York"}
  }
  ]
```
{: codeblock}

再例如，以下 $array 上下文变量是数组，但 $string_array 上下文变量是字符串。

```json
{
  "context": {
   "array": [
      "one",
      "two"
    ],
    "array_in_string": "this is my array: $array"
  }
}
```
{: codeblock}

如果在“试用”窗格中检查这些上下文变量的值，那么将看到其值指定为如下所示：

**$array** : `["one","two"]`

**$array_in_string** : `"this is my array: [\"one\",\"two\"]"`

可以随后对 $array 变量执行数组方法（例如，`<? $array.removeValue('two') ?>`），但不能对 $array_in_string 变量执行数组方法。
