---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-01"

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

# 调用客户机应用程序 ![BETA](images/beta.png)
{: #dialog-actions-client}

将客户机操作添加到用于暂停对话的对话节点，以便客户机端进程可以运行，并将结果作为一轮对话内处理的一部分返回。
{: shortdesc}

客户机操作以外部客户机应用程序可以理解的标准化格式定义程序化调用。外部客户机应用程序必须使用提供的信息来运行程序化调用或函数，然后将结果返回给对话。

此操作不会直接调用自身。此操作主要指示对话在此处暂停，等待外部客户机应用程序执行某些操作。客户机应用程序运行的程序可以是您选择的任何程序。确保根据在本主题中稍后概述的 JSON 格式设置规则，指定调用名称和参数详细信息以及错误消息变量名称。

可以调用客户机应用程序来执行以下类型的操作：

- 验证从用户那里收集的信息。
- 对用户输入执行对于受支持 SpEL 表达式方法来说太复杂而无法处理的计算或字符串操作。
- 从其他应用程序或服务获取数据。

有关如何调用外部服务（例如，{{site.data.keyword.openwhisk_short}} Web 操作）的更多信息，请参阅[从对话节点发起程序化调用](/docs/services/assistant?topic=assistant-dialog-webhooks)。

下图说明了可以如何使用客户机调用来获取天气预报信息，并将其返回给用户。

![显示有人询问天气预报时，对话向客户机应用程序发送请求，然后客户机应用程序将该请求发送到外部服务](images/forecast.png)

请注意，客户机操作调用的是 `MyWeatherFunction` 程序，这是客户机应用程序运行的程序。客户机应用程序将调用外部 Web Service (`/weather`) 来获取实际的预报信息。然后，客户机应用程序会将响应返回给对话。将客户机操作添加到对话时，客户机应用程序必须存在，该应用程序要么自行执行处理，要么在对话和要使用的任何外部后端服务之间传递信息。

## 过程
{: #dialog-actions-client-call}

要从对话节点向客户机应用程序发起程序化调用，请完成以下步骤：

1.  在要从中发起程序化调用的对话节点中，打开 JSON 编辑器。

    - 要发起在对节点响应求值后运行的程序化调用，请打开该节点响应的 JSON 编辑器。

      ![显示如何访问与标准节点响应关联的 JSON 编辑器。](images/contextvar-json-response.png)

      如果节点的**多个响应**设置为**打开**，那么必须单击**编辑响应** ![编辑响应](images/edit-slot.png) 图标，才会显示**选项** ![高级响应](images/kabob.png) 菜单。

      ![显示如何访问与具有多个启用的条件响应的标准节点关联的 JSON 编辑器。](images/contextvar-json-multi-response.png)

    如果要在同一轮对话中显示或进一步处理响应，那么必须添加第二个节点来执行此操作，然后从此节点跳转至第二个节点。
    {: tip}

    - 要发起可由单个槽使用的调用，请单击该槽的**编辑槽**图标 ![“编辑槽”图标](images/edit-slot.png)，然后执行以下其中一个操作：

      - 要发起在槽条件求值为 true 后运行的程序化调用，请打开与该槽条件关联的 JSON 编辑器。

        ![显示如何访问与槽条件关联的 JSON 编辑器。](images/contextvar-json-slot-condition.png)

      - 要发起在槽成功填充后运行的程序化调用，请打开与“已找到”响应关联的 JSON 编辑器。为此，请在该槽的**选项** ![“选项”图标](images/kabob.png) 菜单中，单击**启用条件响应**。对于“已找到”响应，请单击**编辑响应** ![编辑响应](images/edit-slot.png) 图标。从“已找到”响应的**选项** ![“选项”图标](images/kabob.png) 菜单中，单击**打开 JSON 编辑器**。

        ![显示如何访问与槽的条件响应关联的 JSON 编辑器。](images/contextvar-json-slot-multi-response.png)

1.  使用以下语法来定义程序化调用。

    ```json
    {
      "context": {
   "variable_name" : "variable_value"
      },
      "actions": [
        {
          "name":"<actionName>",
          "type":"client",
          "parameters": {
            "<parameter_name>":"<parameter_value>",
            "<parameter_name>":"<parameter_value>"
          },
          "result_variable": "<result_variable_name>"
        }
      ],
      "output": {
        "text": "response text"
      }
    }
    ```
    {: codeblock}

    `actions` 数组指定要从对话发起的程序化调用。此数组可以定义最多 5 个不同的程序化调用。在 JSON 数组中指定以下名称和值对：

    - `<actionName>`：必需。要调用的操作或服务的名称。请使用所需的任何语法指定名称。目标是指定客户机应用程序可识别并知道如何处理的名称。名称长度不能超过 256 个字符。

      例如：`calculateRate`

    - `<type>`：指示要发起的调用的类型。（可选）指定 `client`，这是缺省值。

      发送消息响应，其中包含外部客户机应用程序可以理解的标准化格式的程序化调用信息。客户机应用程序必须使用提供的信息来运行程序化调用或函数，并将结果返回给对话。响应主体中的 JSON 对象指定要调用的服务或函数、要与该调用一起传递的任何关联参数，以及要返回的结果的格式。

    - `<action_parameters>`：外部程序预期的任何参数，这些参数指定为 JSON 对象。仅当外部程序需要时，这些参数才是必需的。

    - `<result_variable_name>`：用于引用由外部服务或程序返回的 JSON 对象的名称。结果会添加到 `/message` 响应的 context 部分。换句话说，结果会存储为一个上下文变量，以便可以在节点响应中显示或由将来触发的对话节点进行访问。上下文变量的任何现有值都会被该操作返回的值覆盖。可以使用以下语法来指定 `result_variable_name`：

      - `my_result`
      - `$my_result`

      名称长度不能超过 64 个字符。变量名称不能包含以下字符：圆括号 `()`、方括号 (`[]`)、单引号 (`'`)、双引号 (`"`) 或反斜杠 (`\`)。

      如果要将结果保存到 `/message` 响应的 output 或 input 部分，那么可以将下列其中一个位置关键字作为前缀添加到 `result_variable_name`：

       - `output.`：将结果添加到 /message 响应的 output 部分。例如，`output.my_result`。
       - `input.`：将结果添加到 /message 响应的 input 部分。例如，`input.my_result`。

      还可以指定 `context.` 位置关键字前缀。例如，`context.my_result`。但是，这样做不是必需的，因为缺省情况下会将结果添加到上下文。

      可以在变量名称中包含句点以创建嵌套的 JSON 对象。例如，可以定义以下变量，以捕获对天气服务发出的两个独立请求（用于预测今天和明天天气）的结果：

      - `context.weather.today`
      - `context.weather.tomorrow`

      结果（`temp` 和 `rain` 参数值）会按以下结构存储在上下文中：

      ```json
      {
        "weather": {
          "today": {
            "temp": "20",
            "rain": "30"
          },
          "tomorrow": {
            "temp": "23",
            "rain": "80"
          }
        }
      }
      ```
      {: codeblock}

      如果单个 JSON 操作数组中的多个操作将其程序化调用的结果添加到同一个上下文变量中，那么更新上下文的顺序就很重要。根据操作类型，数组中定义操作的顺序决定了设置上下文变量值的顺序。数组中最后一个操作返回的上下文变量值将覆盖由其他任何操作计算的值。

## 客户机调用示例
{: #dialog-actions-client-example}

以下示例显示了对外部天气服务的调用可能是什么样的。该调用会添加到与节点响应关联的 JSON 编辑器中。在触发节点级别的响应时，槽已收集并存储来自用户的日期和位置信息。此示例假定将调用的程序名为 `MyWeatherFunction`，接受 `location` 和 `date` 参数，并返回 JSON 对象：`{"forecast": "<value>"}`。

 ``` json
{
  "actions": [
    {
      "name": "MyWeatherFunction",
      "type": "client",
      "parameters": {
        "date": "$date",
        "location": "$location"
      },
      "result_variable": "context.my_forecast"
    }
  ]
}
```
{: codeblock}

通常，仅当需要新的用户输入时（例如在执行父节点之后且在执行其中某个子节点之前），服务才会从 POST `/message` 请求返回到客户机。但是，如果将 client 操作添加到节点，那么在求值后，服务将始终返回到客户机，以便可以返回操作调用的结果。为了避免在不应该等待用户输入的时候（例如，对于配置为直接跳转至子节点的节点）进行等待，服务会将以下值添加到消息上下文中：

```json
  {
    "context": {
   "skip_user_input": true
    }
  }
```
{: codeblock}

如果希望客户机执行操作，但不获取用户输入，那么可以遵循相同的约定，并将 `skip_user_input` 上下文变量添加到父节点，以将其传递给客户机应用程序。

客户机应用程序应该始终检查上下文中的 `skip_user_input` 变量。如果存在，那么客户机应用程序就会知道不要向用户请求新的输入，而是改为执行操作，将其结果添加到消息中，并将其传递回服务。新的 POST 消息请求应该包含前一个 POST 消息响应返回的消息（即 context、input、intents、entities 和可选的 output 部分），还应该包含从程序化调用返回的结果，而不是用于定义要发起的程序化调用的 JSON 对象。
在此节点之后跳转至的子节点中，添加要向用户显示的响应：

``` json
{
  "output": {
    "text": {
      "values": [
        " $date.literal $location.literal 的天气将为$my_forecast。"
      ]
    }
  }
```
{: codeblock}
