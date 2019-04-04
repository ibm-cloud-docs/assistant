---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# 表达式语言方法
{: #dialog-methods}

可以处理从用户发声中抽取的值，这些值将根据您的需要在上下文变量、条件或响应中的其他地方进行引用。
{: shortdesc}

## 求值语法
{: #dialog-methods-evaluation-syntax}

要在其他变量内部扩展变量值，或要将方法应用于输出文本或上下文变量，请使用 `<? expression ?>` 表达式语法。例如：

- **递增数字属性**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **对对象调用方法**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

以下各部分描述可用于处理值的方法。这些值按数据类型进行组织：

- [数组](#dialog-methods-arrays)
- [日期和时间](#dialog-methods-date-time)
- [数字](#dialog-methods-numbers)
- [对象](#dialog-methods-objects)
- [字符串](#dialog-methods-strings)

## 数组
{: #dialog-methods-arrays}

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

### JSONArray.clear()

此方法用于清除数组中的所有值并返回 null。

在输出中使用以下表达式来定义一个字段，用于清除保存到其值的上下文变量的数组 ($toppings_array)。

```json
{
  "output": {
    "array_eraser": "<? $toppings_array.clear() ?>"
  }
}
```
{: codeblock}

如果后续引用 $toppings_array 上下文变量，那么仅返回“[]”。

### JSONArray.contains(Object value)

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

### JSONArray.containsIntent(String intent_name, Double min_score, [Integer top_n])
{: #dialog-methods-array-containsIntent}

如果 `intents` JSONArray 具体包含指定的意向，并且该意向的置信度分数等于或高于指定的最小分数，那么此方法会返回 `true`。（可选）可以指定一个数字，以指示该意向必须包含在数组中排名前几位（该数字）元素内。

如果指定的意向不在数组中，其置信度分数低于最小置信度分数，或者该意向在数组中的位置低于指定的索引位置，那么会返回 `false`。

服务会自动生成 `intents` 数组，其中列出每当提交用户输入时，服务都会在输入中检测到的意向。该数组会按置信度从高到低列出服务检测到的所有意向。

可以在节点条件中使用此方法，以便不仅可检查是否存在意向，还可设置置信度分数阈值，必须达到此阈值后，才会处理节点并返回其响应。

例如，如果要在仅当满足以下条件时才触发对话节点，请在节点条件中使用以下表达式：

- `#General_Ending` 意向存在。
- `#General_Ending` 意向的置信度分数高于 80%。
- `#General_Ending` 意向是 intents 数组中排名前 2 位的意向之一。

```bash
intents.containsIntent("General_Ending", 0.8, 2)
```
{: codeblock}

### JSONArray.filter(temp, "temp.property operator comparison_value")
{: #dialog-methods-array-filter}

通过将每个数组元素值与指定的值进行比较来过滤数组。此方法类似于[集合投影](#collection-projection)。集合投影返回基于数组元素名称/值对中的名称过滤的数组。filter 方法返回基于数组元素名称/值对中的值过滤的数组。

过滤表达式由以下值组成：

- `temp`：对每个数组元素求值时临时使用的变量的名称。例如，`city`。
- `property`：要与 `comparison_value` 进行比较的元素属性。将 property 指定为在第一个参数中命名的临时变量的属性。请使用语法：`temp.property`。例如，如果 `latitude` 是数组中名称/值对的有效元素名称，请将 property 指定为 `city.latitude`。
- `operator`：要用于将属性值与 `comparison_value` 进行比较的运算符。

    支持的运算符如下：

    <table>
    <caption>支持的过滤器运算符</caption>
    <tr>
      <th>运算符</th>
      <th>描述</th>
    </tr>
    <tr>
      <td>`==`</td>
      <td>等于</td>
    </tr>
    <tr>
      <td>`>`</td>
      <td>大于</td>
    </tr>
    <tr>
      <td>`<`</td>
      <td>小于</td>
    </tr>
    <tr>
      <td>`>=`</td>
      <td>大于或等于</td>
    </tr>
    <tr>
      <td>`<=`</td>
      <td>小于或等于</td>
    </tr>
    <tr>
      <td>`!=`</td>
      <td>不等于</td>
    </tr>
    </table>

- `comparison_value`：要与每个数组元素属性值进行比较的值。要指定可以根据用户输入变化的值，请将上下文变量或实体用作值。如果指定了可以变化的值，请添加逻辑以保证 `comparison_value` 值在求值时有效，否则将发生错误。

#### 过滤器示例 1

例如，可以使用 filter 方法来对包含一组城市名称及其人口数字的数组求值，以返回仅包含人口超过 500 万的城市的更小数组。

以下 `$cities` 上下文变量包含对象的数组。每个对象都包含 `name` 和 `population` 属性。

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Rome",
      "population":2868104
   },
   {
      "name":"Beijing",
      "population":20693000
   },
   {
      "name":"Paris",
      "population":2241346
   }
]
```
{: codeblock}

在以下示例中，任意临时变量名称为 `city`。SpEL 表达式会过滤 `$cities` 数组，以仅包含人口超过 500 万的城市：

```bash
$cities.filter("city", "city.population > 5000000")
```
{: codeblock}

此表达式将返回以下过滤后的数组：

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Beijing",
      "population":20693000
   }
]
```
{: codeblock}

可以使用集合投影来创建新的数组，该数组仅包含 filter 方法返回的数组中的城市名称。然后，可以使用 `join` 方法将数组中的两个 name 元素值显示为字符串，并使用逗号和空格分隔各值。

```bash
The cities with more than 5 million people include <?  T(String).join(", ",($cities.filter("city", "city.population > 5000000")).![name]) ?>.
```
{: codeblock}

生成的响应为：`The cities with more than 5 million people include Tokyo, Beijing.`

#### 过滤器示例 2

filter 方法的强大之处在于，无需对 `comparison_value` 值进行硬编码。在此示例中，硬编码的值 5000000 会改为替换为上下文变量。

在此示例中，`$population_min` 上下文变量包含数字 `5000000`。任意临时变量名称为 `city`。SpEL 表达式会过滤 `$cities` 数组，以仅包含人口超过 500 万的城市：

```bash
$cities.filter("city", "city.population > $population_min")
```
{: codeblock}

此表达式将返回以下过滤后的数组：

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Beijing",
      "population":20693000
   }
]
```
{: codeblock}

比较数字值时，请确保在触发 filter 方法之前，将比较中涉及的上下文变量设置为有效值。请注意，如果要与其进行比较的数组元素可能包含 null，那么 `null` 可以是有效值。例如，如果东京的人口名称和值对为 `"population":null`，比较表达式为 `"city.population == $population_min"`，那么 `null` 将是 `$population_min` 上下文变量的有效值。
{: tip}

可以使用类似于以下内容的对话节点响应表达式：

```bash
The cities with more than $population_min people include <?  T(String).join(", ",($cities.filter("city", "city.population > $population_min")).![name]) ?>.
```
{: codeblock}

生成的响应为：`The cities with more than 5000000 people include Tokyo, Beijing.`

#### 过滤器示例 3

在此示例中，实体名称用作 `comparison_value`。用户输入为：`What is the population of Tokyo?`。任意临时变量名称为 `y`。您创建了名为 `@city` 的实体，用于识别城市名称，包括 `Tokyo`。

```bash
$cities.filter("y", "y.name == @city")
```

此表达式将返回以下数组：

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   }
]
```
{: codeblock}

您可以使用集合项目来获取只包含原始数组中 population 元素的数组，然后使用 `get` 方法来返回 population 元素的值。

```bash
The population of @city is: <? ($cities.filter("y", "y.name == @city").![population]).get(0) ?>.
```
{: codeblock}

表达式会返回：`The population of Tokyo is 9273000.`

### JSONArray.get(Integer)

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
    "generic":[
      {
        "values": [
        {
        "text" : "The first item in the array is <?$nested.array.get(0)?>"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
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
    "generic":[
      {
        "values": [
        {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
}
```
{: codeblock}

结果：`"ham is a great choice!"`、`"onion is a great choice!"` 或 `"olives is a great choice!"`

**注：**生成的输出文本是随机选择的。

### JSONArray.indexOf(value)
{: #dialog-methods-array-indexOf}

此方法返回数组中与指定为参数的值相匹配的元素的下标号，如果在数组中找不到该值，那么会返回 `-1`。值可以是字符串 (`"School"`)、整数 (`8`) 或双精度值 (`9.1`)。值必须是完全匹配项，并且区分大小写。

例如，以下上下文变量包含数组：

```json
{
  "context": {
   "array1": ["Mary","Lamb","School"],
    "array2": [8,9,10],
    "array3": [8.1,9.1,10.1]
  }
}
```

以下表达式可用于确定在其中指定值的数组下标：

```bash
<? $array1.indexOf("Mary") ?> returns `0`
<? $array2.indexOf(9) ?> returns `1`
<? $array3.indexOf(10.1) ?> returns `2`
```

例如，要获取 intents 数组中某个元素的下标，此方法会非常有用。您可以将 `indexOf` 方法应用于在每次对用户输入求值时生成的 intents 数组，以确定特定意向的数组下标号。

```bash
intents.indexOf("General_Greetings")
```
{: codeblock}

如果您希望了解特定意向的置信度分数，那么可以使用语法 `intents[`*`index`*`].confidence` 将上面的表达式作为 *`index`* 值传递给表达式。 例如：

```bash
intents[intents.indexOf("General_Greetings")].confidence
```
{: codeblock}

### JSONArray.join(String delimiter)

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
    "generic":[
      {
        "values": [
        {
    "text": "This is the array: <? $toppings_array.join(';') ?>"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
}
```
{: codeblock}

结果：

```json
This is the array: onion;olives;ham;
```
{: codeblock}

如果定义了用于在一个 JSON 数组中存储多个值的变量，那么可以返回该数组中的值的子集，然后使用 join() 方法对其正确设置格式。

#### 集合投影
{: #dialog-methods-collection-projection}

`集合投影` SpEL 表达式从包含对象的数组中抽取子集合。集合投影的语法为 `array_that_contains_value_sets.![value_of_interest]`。

例如，以下上下文变量定义用于存储航班信息的 JSON 数组。每个航班有两个数据点：时间和航班代码。

```json
"flights_found": [
  {
    "time": "10:00",
    "flight_code": "OK123"
  },
  {
    "time": "12:30",
    "flight_code": "LH421"
  },
  {
    "time": "16:15",
    "flight_code": "TS4156"
  }
]
```
{: codeblock}

要仅返回航班代码，可以使用以下语法来创建集合投影表达式：

```
<? $flights_found.![flight_code] ?>
```

此表达式将 `fight_code` 值的数组作为 `["OK123"，"LH421"，"TS4156"]` 返回。有关更多详细信息，请参阅 [SpEL 集合投影文档](https://docs.spring.io/spring/docs/3.0.x/reference/expressions.html)。

如果将 `join()` 方法应用于返回的数组中的值，那么航班代码将以逗号分隔列表形式显示。例如，可以在响应中使用以下语法：

```
The flights that fit your criteria are:
  <? T(String).join(",", $flights_found.![flight_code]) ?>.
```
{: codeblock}

结果：`The flights that match your criteria are: OK123,LH421,TS4156.`

### JSONArray.joinToArray(template)
{: #dialog-methods-joinToArray}

此方法会将您在模板中定义的格式应用于数组，并返回根据规范设置了格式的数组。例如，要将格式应用于您希望在对话响应中返回的数组值，此方法非常有用。

模板可以指定为字符串、JSON 对象或 JSON 数组。要引用您正在模板中编辑的数组中的值，请遵循以下语法约定：

- `%`：表示要从所编辑数组返回的元素或元素属性的开头或结尾。
- `e`：临时表示要对其应用格式的数组元素。此临时变量名称 `e` 无法更改。

例如，您有一个上下文变量，其中包含一个数组，数组中含有三个航班的航班详细信息列表。

```json
"flights": [
      {
        "flight": "DL1040",
        "origin": "JFK",
        "carrier": "Alitalia",
        "duration": 485,
        "destination": "FCO",
        "arrival_date": "2019-02-03",
        "arrival_time": "07:00",
        "departure_date": "2019-02-02",
        "departure_time": "16:45"
      },
      {
        "flight": "DL1710",
        "origin": "JFK",
        "carrier": "Delta",
        "duration": 379,
        "destination": "LAX",
        "arrival_date": "2019-02-02",
        "arrival_time": "10:19",
        "departure_date": "2019-02-02",
        "departure_time": "07:00"
      },
      {
        "flight": "DL4379",
        "origin": "BOS",
        "carrier": "Virgin Atlantic",
        "duration": 385,
        "destination": "LHR",
        "arrival_date": "2019-02-03",
        "arrival_time": "09:05",
        "departure_date": "2019-02-02",
        "departure_time": "21:40"
      }
    ]
```
{: codeblock}

您希望仅返回航班代码列表。要仅从每个数组中抽取 `flight` 元素的值，并将其以列表形式返回，可以使用以下表达式：

```
The available flights are <? $flights.joinToArray("%e.flight%"). ?>
```
{: codeblock}

对话节点响应为 `The available flights are ["DL1040","DL1710","DL4379"].`

要将数组显示为文本，请在表达式中使用 `join` 方法，类似于：

```
The available flights are <? $flights.joinToArray("%e.flight%").join(", "). ?>
```
{: codeblock}

响应为 `The available flights are DL1040, DL1710, DL4379.`

#### 复杂模板
{: #dialog-methods-complex-template}

要创建更复杂的模板，您可以创建上下文变量，而不是直接在方法参数中指定模板详细信息。

此模板上下文变量包含数组元素的子集，并在元素前面添加标签，因此这些信息在响应中将以可阅读列表形式显示：

```json
"template": "<br/>Flight number: %e.flight% <br/> Airline: %e.carrier% <br/> Departure date: %e.departure_date% <br/> Departure time: %e.departure_time% <br/> Arrival time: %e.arrival_time% <br/>"
```
{: codeblock}

某些集成通道（包括 Facebook 和 Slack）*不会*呈现表示换行符的 `<br/>` HTML 标记。
{: note}

在对话节点响应中使用以下表达式，以将 `$template` 中定义的模板应用于 `$flights` 中的数组。

```
The flight info is <? $flights.joinToArray($template).join(" ") ?>
```
{: codeblock}

响应类似于：

```
The flight info is
Flight number: DL1040
Airline: Alitalia
Departure date: 2019-02-02
Departure time: 16:45
Arrival time: 07:00

Flight number: DL1710
Airline: Delta
Departure date: 2019-02-02
Departure time: 07:00
Arrival time: 10:19

Flight number: DL4379
Airline: Virgin Atlantic
Departure date: 2019-02-02
Departure time: 21:40
Arrival time: 09:05
```
{: screen}

使用此方法的优点是，数组中值的更改频率或数组中元素的数量是否增加都无关紧要。只要每个数组元素至少包含模板引用的属性的子集，表达式就有效。

#### JSON 对象模板示例
{: #dialog-methods-object-template}

在此示例中，模板上下文变量定义为 JSON 对象，用于从 `$flight` 上下文变量的数组中指定的每个 flight 元素中抽取航班号、到达和出发日期与时间。例如，可以使用此方法将标准格式应用于由两家不同航空公司管理的航班的航班详细信息，如航空公司在其 Web 服务中以不同方式设置航班信息格式。

```json
"template": {
      "departure": "Flight %e.flight% departs on %e.departure_date% at %e.departure_time%.",
      "arrival": "Flight %e.flight% arrives on %e.arrival_date% at %e.arrival_time%."
    }
```
{: codeblock}

您可能希望设计定制客户机应用程序，以从返回的数组中读取对象，并针对聊天机器人的响应正确设置值的格式。对话节点响应可以使用以下表达式将航班到达详细信息对象作为数组返回：

```
<? $flights.joinToArray($template) ?>
```
{: screen}

下面是对话节点响应：

```json
[
  {
    "arrival":"Flight DL1040 arrives on 2019-02-03 at 07:00.",
    "departure":"Flight DL1040 departs on 2019-02-02 at 16:45."
    },
  {
    "arrival":"Flight DL1710 arrives on 2019-02-02 at 10:19.",
    "departure":"Flight DL1710 departs on 2019-02-02 at 07:00."
    },
  {
    "arrival":"Flight DL4379 arrives on 2019-02-03 at 09:05.",
    "departure":"Flight DL4379 departs on 2019-02-02 at 21:40."
    }
  ]
  ```

请注意，`arrival` 和 `departure` 元素的顺序在响应中进行了交换。服务通常会对 JSON 对象中的元素进行重新排序。如果要以特定顺序返回元素，请改为使用 JSON 数组或字符串值来定义模板。

### JSONArray.remove(Integer)

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

### JSONArray.set(Integer index, Object value)

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
{: #dialog-methods-com.google.gson.JsonArray}

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
{: #dialog-methods-date-time}

有多种方法可用于处理日期和时间。

有关如何在用户输入中识别和抽取日期和时间信息的信息，请参阅 [@sys-date 和 @sys-time 实体](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-date-time)。

### .after(String date or time)

确定日期/时间值是否晚于 date/time 自变量。

### .before(String date or time)
确定日期/时间值是否早于 date/time 自变量。

例如：
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')

- 如果比较不同的项（例如，`时间与日期`、`日期与时间`以及`时间与日期和时间`），那么此方法会返回 false，并且会在响应 JSON 日志 `output.log_messages` 中输出异常。

  例如，`@sys-date.before(@sys-time)`。
- 如果比较`日期和时间与时间`，那么此方法会忽略日期而仅比较时间。

### now()

返回 `yyyy-MM-dd HH:mm:ss` 格式的当前日期和时间的字符串。

- 静态函数。
- 可以对此函数返回的日期/时间值调用其他日期/时间方法，并且可以将其作为这些方法的自变量传递。
- 如果设置了上下文变量 `$timezone`，那么此函数会返回客户机时区中的日期和时间。

输出字段中使用了 `now()` 的对话节点示例：

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "<? now() ?>"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

节点条件中的 `now()` 示例（用于确定现在是否仍是上午）：

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Good morning!"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
        }
      ]
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

### today()

返回 `yyyy-MM-dd` 格式的当前日期的字符串。

- 静态函数。
- 可以对此函数返回的日期值调用其他日期方法，并且可以将其作为这些方法的自变量传递。
- 如果设置了上下文变量 `$timezone`，那么此函数会返回客户机时区中的日期。否则，将使用 `GMT` 时区。

输出字段中使用了 `today()` 的对话节点示例：

```json
{
  "conditions": "#what_day_is_it",
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Today's date is <? today() ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

结果：`Today's date is 2018-03-09.`

## 日期和时间计算
{: #dialog-methods-calculations}

使用以下方法可计算日期。

|方法|描述|
|-------------------------|-------------|
| `<date>.minusDays(n)`   |返回指定日期之前 n 天的日期。|
| `<date>.minusMonths(n)` |返回指定日期之前 n 个月的日期。|
| `<date>.minusYears(n)`  |返回指定日期之前 n 年的日期。|
| `<date>.plusDays(n)`   |返回指定日期之后 n 天的日期。|
| `<date>.plusMonths(n)` |返回指定日期之后 n 个月的日期。|
| `<date>.plusYears(n)`  |返回指定日期之后 n 年的日期。|

其中，`<date>` 指定为格式 `yyyy-MM-dd` 或 `yyyy-MM-dd HH:mm:ss`。

要获取明天的日期，请指定以下表达式：

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Tomorrow's date is <? today().plusDays(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

如果今天是 2018 年 3 月 9 日，那么结果为：`Tomorrow's date is 2018-03-10.`

要获取从今天起一周之后的日期，请指定以下表达式：

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Next week's date is <? @sys-date.plusDays(7) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

如果 @sys-date 实体捕获的日期是今天的日期，即 2018 年 3 月 9 日，那么结果为：`Next week's date is 2018-03-16.`

要获取上个月的日期，请指定以下表达式：

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Last month the date was <? today().minusMonths(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

如果今天是 2018 年 3 月 9 日，那么结果为：`Last month the date was 2018-02-9.`

使用以下方法可计算时间。

|方法|描述|
|-------------------------|-------------|
| `<time>.minusHours(n)`   |返回指定时间之前 n 小时的时间。|
| `<time>.minusMinutes(n)` |返回指定时间之前 n 分钟的时间。|
| `<time>.minusSeconds(n)`  |返回指定时间之前 n 秒的时间。|
| `<time>.plusHours(n)`   |返回指定时间之后 n 小时的时间。|
| `<time>.plusMinutes(n)` |返回指定时间之后 n 分钟的时间。|
| `<time>.plusSeconds(n)`  |返回指定时间之后 n 秒的时间。|

其中，`<time>` 按格式 `HH:mm:ss` 进行指定。

要获取从现在起一小时后的时间，请指定以下表达式：

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "One hour from now is <? now().plusHours(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

如果现在是早上 8 点，那么结果为：`One hour from now is 09:00:00.`

要获取 30 分钟前的时间，请指定以下表达式：

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "A half hour before @sys-time is <? @sys-time.minusMinutes(30) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

如果 @sys-time 实体捕获的时间为早上 8 点，那么结果为：`A half hour before 08:00:00 is 07:30:00.`

要重新设置所返回时间的格式，可以使用以下表达式：

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "6 hours ago was <? now().minusHours(6).reformatDateTime('h:mm a') ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

如果现在是下午 2:19，那么结果为：`6 hours ago was 8:19 AM.`

### 使用时间范围
{: #dialog-methods-time-spans}

要根据今天的日期是否位于特定时间范围内来显示响应，可以使用与时间相关的方法的组合。例如，如果您在每年的假期期间运行特殊报价，那么可以检查今天的日期是否位于今年的 11 月 25 日与 12 月 24 日之间。首先，将相关日期定义为上下文变量。

在以下开始日期和结束日期上下文变量表达式中，通过将动态派生的当前年份值与硬编码的月份和日期值并置来构造日期。

```json
"context": {
   "end_date": "<? now().reformatDateTime('Y') + '-12-24' ?>",
   "start_date": "<? now().reformatDateTime('Y') + '-11-25' ?>"
 }
```

在响应条件中，可以指示仅在当前日期位于定义为上下文变量的开始日期和结束日期之间时，才显示响应。

`now().after($start_date) && now().before($end_date)`

### java.util.Date 支持
{: #dialog-methods-java.util.Date}

除了内置方法外，还可以使用 `java.util.Date` 类的标准方法。

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
{: #dialog-methods-numbers}

这些方法可帮助您获取数字值并重新设置其格式。

有关可以在用户输入中识别和抽取数字的系统实体的信息，请参阅 [@sys-number 实体](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-number)。

如果您希望服务识别用户输入中的特定数字格式（例如，订单号引用），请考虑创建模式实体来进行捕获。有关更多详细信息，请参阅[创建实体](/docs/services/assistant?topic=assistant-entities)。

如果要更改数字的小数位数（例如，用于将数字格式重新设置为货币值），请参阅 [String format() 方法](#dialog-methods-java.lang.String)。

### toDouble()

  将对象或字段转换为 Double 数字类型。可以对任何对象或字段调用此方法。如果转换失败，会返回 *null*。

### toInt()

  将对象或字段转换为 Integer 数字类型。可以对任何对象或字段调用此方法。如果转换失败，会返回 *null*。

### toLong()

  将对象或字段转换为 Long 数字类型。可以对任何对象或字段调用此方法。如果转换失败，会返回 *null*。

  如果在 SpEL 表达式中指定了长整型数字类型，那么必须在该数字后附加 `L` 进行标识。例如，`5000000000L`。对于任何不适合 32 位整数类型的数字，此语法都是必需的。例如，大于 2^31 (2,147,483,648) 或小于 -2^31 (-2,147,483,648) 的数字被视为长整型数字类型。长整型数字类型的最小值为 -2^63，最大值为 2^63-1。

### Java 数字支持
{: #dialog-methods-java.lang.Number}

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
    "generic":[
      {
        "values": [
          {
          "text": "The bigger number is $bigger_number."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
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
    "generic":[
      {
        "values": [
          {
          "text": "The smaller number is $smaller_number."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
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
    "generic":[
      {
        "values": [
          {
          "text": "Your number $base to the second power is $power_of_two."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
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
    "generic":[
      {
        "values": [
          {
          "text": "Here's a random number between 0 and @sys-number.literal: $answer."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
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
{: #dialog-methods-objects}

### JSONObject.clear()

此方法用于清除 JSON 对象中的所有值并返回 null。

例如，您希望清除 $user 上下文变量中的当前值。

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

在输出中使用以下表达式来定义一个字段，用于清除其值的对象。

```json
{
  "output": {
    "object_eraser": "<? $user.clear() ?>"
  }
}
```
{: codeblock}

如果后续引用 $user 上下文变量，那么仅返回 `{}`。

可以在 API `/message` 调用主体中的 `context` 或 `output` JSON 对象上使用 `clear()` 方法。

#### 清除上下文
{: #dialog-methods-clearing-context}

使用 `clear()` 方法清除 `context` 对象时，会清除**所有**变量，但以下变量除外：

 - `context.conversation_id`
 - `context.timezone`
 - `context.system`

**警告**：所有上下文变量值是指：

  - 在当前会话期间触发的节点中为变量设置的所有缺省值。
  - 使用用户或外部服务在当前会话期间提供的信息对缺省值进行的任何更新。

要使用此方法，可以在输出对象中定义的变量内的表达式中指定该方法。例如：

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Response for this node."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ],
    "context_eraser": "<? context.clear() ?>"
  }
}

```

#### 清除输出
{: #dialog-methods-clearing-output}

使用 `clear()` 方法来清除 `output` 对象时，将清除所有变量，但用于清除在当前节点中定义的输出对象和任何文本响应的变量除外。该方法也不会清除以下变量：

- `output.nodes_visited`
- `output.nodes_visited_details`

要使用此方法，可以在输出对象中定义的变量内的表达式中指定该方法。例如：

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Have a great day!"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ],
    "output_eraser": "<? output.clear() ?>"
  }
}
```

如果树中较早的节点定义了文本响应 `I'm happy to help.`，然后跳转至如上所述定义了 JSON 输出对象的节点，那么只会将 `Have a great day.` 显示为响应。不会显示 `I'm happy to help.` 输出，因为此输出已被清除并替换为调用 `clear()` 方法的节点中的文本响应。

### JSONObject.has(String)

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

### JSONObject.remove(String)

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
{: #dialog-methods-com.google.gson.JsonObject}

除了内置方法外，还可以使用 `com.google.gson.JsonObject` 类的标准方法。

## 字符串
{: #dialog-methods-strings}

这些方法有助于您处理文本。

有关如何识别和抽取用户输入中特定类型 String（例如，人名和位置）的信息，请参阅[系统实体](/docs/services/assistant?topic=assistant-system-entities)。

**注：**有关涉及正则表达式的方法，请参阅 [RE2 语法参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/google/re2/wiki/Syntax){: new_window}，以了解有关指定正则表达式时使用的语法的详细信息。

### String.append(Object)

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

### String.contains(String)

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

### String.endsWith(String)

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

### String.find(String regexp)

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

### String.matches(String regexp)

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

### String.startsWith(String)

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

### String.substring(Integer beginIndex, Integer endIndex)

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

可以将标准 Java String `format()` 方法应用于文本。有关用于指定格式详细信息的语法的信息，请参阅 [java.util.formatter 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter#syntax){: new_window}。

例如，以下表达式采用 3 个十进制整数（1、1 和 2）并将它们添加到语句中。

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d equals %d', 1, 1, 2) ?>"
}
```
{: codeblock}

结果：`1 + 1 equals 2`。

要更改数字的小数位数，请使用以下语法：

```
{
  <? T(String).format('%.2f',<number to format>) ?>
}
```
{: codeblock}

例如，如果需要将格式设置为美元的 $number 变量为 `4.5`，那么诸如 `Your total is $<? T(String).format('%.2f',$number) ?>` 之类的响应将返回 `Your total is $4.50`。

## 间接数据类型转换
{: #dialog-methods-indirect-type-conversion}

例如，在文本中包含表达式作为节点响应的一部分时，该值会呈现为 String。如果希望表达式以其原始数据类型呈现，那么不要将其包含在文本内。

例如，可以将以下表达式添加到对话节点响应，以便以 String 格式返回用户输入中识别到的实体：

```json
实体为 <? entities ?>。
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

可以随后对 $array 变量（例如，`<? $array.removeValue('two') ?>`）执行数组方法，而不能对 $array_in_string 变量执行。
