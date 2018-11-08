---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-15"

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

# 从对话节点发起程序化调用 ![BETA](images/beta.png)
{: #dialog-actions}

定义可以对外部应用程序或服务发起程序化调用的操作，并返回结果作为一轮对话内所执行处理的一部分。
{: shortdesc}

可以使用外部服务来验证从用户那里收集的信息，或者对输入执行太复杂而无法使用受支持的 SpEL 表达式和方法进行处理的计算或字符串操作。或者，您可以与外部 Web Service（例如用于检查航班预计到达时间的空中交通服务，或用于获取天气预报的天气服务）进行交互以获取相关信息。您甚至可以与外部应用程序（例如，餐馆预订站点）进行交互，以代表用户完成简单的交易。

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/y0A6X-KNoB8?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

定义程序化调用时，请选择以下其中一种类型：

- **client**：定义标准化格式的程序化调用，外部客户机应用程序可以使用该格式来执行程序化调用或函数，并将结果返回给对话。
- **server**：直接调用 {{site.data.keyword.openwhisk_short}} 操作，并将结果返回给对话。

    目前，可以从美国南部或德国区域中托管的 {{site.data.keyword.conversationshort}} 实例调用 {{site.data.keyword.openwhisk_short}} 操作。

    **注**：使用的 {{site.data.keyword.openwhisk_short}} 实例是在同一位置（美国南部或德国）托管的实例。因此，例如，如果计划从在美国南部托管的 {{site.data.keyword.conversationshort}} 服务实例访问某个操作，那么不要在德国托管的 {{site.data.keyword.openwhisk_short}} 实例中定义该操作。

    **重要信息**：仅使用此方法来调用您确信可以在 **5 秒内**返回的 {{site.data.keyword.openwhisk_short}} 操作。如果单个服务调用所用时间长于 5 秒，对 {{site.data.keyword.openwhisk_short}} 的请求会超时。如果对话对某个外部服务发起多个调用，那么允许这些调用完成的总时间为 7 秒。如果前 3 个调用各自耗费 2 秒来完成，并且第 4 个调用耗费的时间超过 1 秒，那么第 4 个调用会停止，并且该调用的错误消息指示该调用未完成。对于需要调用的效率较低的服务，请通过客户机应用程序来管理调用，并作为单独的步骤将相应信息传递给对话。

## 过程
{: #call-action}

要从对话节点发起程序化调用，请完成以下步骤：

1.  在要从中发起程序化调用的对话节点中，打开 JSON 编辑器。

    - 要发起在对节点响应求值后执行的程序化调用，请打开节点响应的 JSON 编辑器。

      ![显示如何访问与标准节点响应关联的 JSON 编辑器。](images/contextvar-json-response.png)

      如果节点的**多个响应**设置为**打开**，那么必须单击**编辑响应** ![编辑响应](images/edit-slot.png) 图标后，才会显示**选项** ![高级响应](images/kabob.png) 菜单。

      ![显示如何访问与启用了多个条件响应的标准节点关联的 JSON 编辑器。](images/contextvar-json-multi-response.png)

    如果要在同一轮对话中显示或进一步处理来自外部服务的响应，那么必须添加第二个节点来执行此操作，然后从此节点跳转至第二个节点。
    {: tip}

    - 要发起可由单个槽使用的调用，请单击该槽的**编辑槽**图标 ![“编辑槽”图标](images/edit-slot.png)，然后执行以下其中一个操作：

      - 要发起在槽条件求值为 true 后执行的程序化调用，请打开与该槽条件关联的 JSON 编辑器。

        ![显示如何访问与槽条件关联的 JSON 编辑器。](images/contextvar-json-slot-condition.png)

      - 要发起在槽成功填充后执行的程序化调用，请打开与“已找到”响应关联的 JSON 编辑器。为此，请在该槽的**选项** ![“选项”图标](images/kabob.png) 菜单中，单击**启用条件响应**。对于“已找到”响应，请单击**编辑响应** ![编辑响应](images/edit-slot.png) 图标。从“已找到”响应的**选项** ![“选项”图标](images/kabob.png) 菜单中，单击**打开 JSON 编辑器**。

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
          "type":"client | server",
          "parameters": {
            "<parameter_name>":"<parameter_value>",
            "<parameter_name>":"<parameter_value>"
          },
          "result_variable": "<result_variable_name>",
          "credentials": "<reference_to_credentials>"
        }
      ],
      "output": {
        "text": "response text"
      }
    }
    ```
    {: codeblock}

    `actions` 数组指定要从对话发起的程序化调用。此数组可以定义最多 5 个不同的程序化调用。在 JSON 数组中指定以下名称和值对：

    - `<actionName>`：必需。要调用的操作或服务的名称。名称长度不能超过 64 个字符。

       - 对于 client 操作类型，请使用所需的任何语法指定名称。目标是指定客户机应用程序可识别并知道如何处理的名称。

          例如：`calculateRate`

       - 对于 {{site.data.keyword.openwhisk_short}} (server) 操作类型，请使用以下语法来提供操作的标准名称：`/<namespace>/[<package-name>]/<action name>`

         - 如果操作是包的一部分，那么 `<package-name>` 信息是必需的；否则，此信息不是必需的。
         - 如果要调用操作序列，那么指定 `<sequence name>` 以代替 `<action name>`。
         - 用户定义的操作的名称空间通常具有以下语法：`<myIBMCloudOrganizationID>_<myIBMCloudSpace>`。例如：`/jdoeorg_prod10/search flights`
         - 随 {{site.data.keyword.openwhisk_short}} 一起提供的操作通常具有名称空间 `whisk.system`，但您应该始终验证名称空间才能确定。例如：`/whisk.system/weather/forecast`

    - `<type>`：指示要发起的调用的类型。从以下类型中进行选择：

      - **client**：发送具有标准化格式的程序化调用信息的消息响应，外部客户机应用程序可使用该格式来执行调用或函数，并代表对话获取结果。响应主体中的 JSON 对象指定要调用的服务或函数、要与该调用一起传递的任何关联参数，以及应该如何发回结果。

      - **server**：直接调用 {{site.data.keyword.openwhisk_short}} 操作（一个或多个）。必须使用 {{site.data.keyword.openwhisk}} 来单独定义操作本身。有关更多信息，请参阅下面的[创建 {{site.data.keyword.openwhisk_short}} 操作](dialog-actions.html#create-action)。

      指定类型是可选操作。缺省值为 `client`。

    - `<action_parameters>`：外部程序预期的任何参数，指定为 JSON 对象。仅当外部程序需要时，这些参数才是必需的。

    - `<result_variable_name>`：用于引用由外部服务或程序返回的 JSON 对象的名称。结果会添加到 /message 响应的 context 部分。换句话说，结果会存储为一个上下文变量，以便可以在节点响应中显示或由将来触发的对话节点进行访问。上下文变量的任何现有值都会被该操作返回的值覆盖。可以使用以下语法来指定 `result_variable_name`：

      - `my_result`
      - `$my_result`

      名称长度不能超过 64 个字符。变量名称不能包含以下字符：括号 `()`、方括号 (`[]`)、单引号 (`'`)、双引号 (`"`) 或反斜杠 (`\`)。

      如果要将结果保存到 /message 响应的 output 或 input 部分，那么可以将以下其中一个位置关键字插入到 `result_variable_name` 开头：

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

      如果单个 JSON 操作数组中的多个操作将其程序化调用的结果添加到同一个上下文变量中，那么更新上下文的顺序就很重要：

      1. 如果数组中有 server 和 client 操作的组合，那么服务会首先处理 server 类型操作。因此，数组中最后一个 client 类型操作为上下文变量计算的值将覆盖由任何 server 类型操作为其计算的值。
      1. 根据操作类型，数组中定义操作的顺序决定了设置上下文变量值的顺序。数组中最后一个操作返回的上下文变量值将覆盖由其他任何操作计算的值。

    - `<reference_to_credentials>`：存储 {{site.data.keyword.openwhisk_short}} 凭证的对象的名称。仅对于 server 操作，此项是必需的。这些凭证用于访问运行操作的 {{site.data.keyword.openwhisk_short}} 实例。这些不是您的 {{site.data.keyword.Bluemix_notm}} 凭证。

      要发现凭证，请完成以下步骤：
      1.  转至 [{{site.data.keyword.openwhisk_short}} API 密钥 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/openwhisk/learn/api-key){: new_window} 页面。

          - 如果您尚未创建帐户，请进行创建。
          - 如果您未登录，请登录。

      1.  单击**显示认证密钥**图标 ![显示认证密钥](images/show-auth-icon.png) 以显示凭证。冒号 (:) 前的分段是用户标识。冒号后的分段是密码。

      **注意**：运行操作时发生的任何费用均由拥有这些凭证的人员支付。

      为了保护凭证，不要将其存储在 {{site.data.keyword.conversationshort}} 工作空间中。请改为将凭证作为上下文的一部分从客户机应用程序传递。通过将上下文变量嵌套在消息上下文的 $private 部分中，可以阻止信息存储在 Watson 日志中。例如：`$private.my_credentials`。

      定义的凭证对象必须包含 `user` 和 `password` 参数。

      ```json
      {
        "user":"5tj3b41j-bf3j-5d92-24g9-4a7769ab12af",
        "password":"y65gqSTSRzqE..."
      }
      ```
      {: codeblock}

      测试对话时，可以通过单击工具中“试用”窗格中的**管理上下文**，使用您的真实 {{site.data.keyword.openwhisk_short}} 用户名和密码值来临时设置 `$private.my_credentials` 上下文变量。

      ![显示如何在“试用”上下文管理界面中定义 $private.my_credentials 上下文变量](images/testing-creds.png)

## 创建 {{site.data.keyword.openwhisk_short}} 操作
{: #create-action}

如果选择定义 server 类型程序化调用，那么必须在 {{site.data.keyword.openwhisk}} 中创建操作后，才能从对话进行调用。如果要定义 client 类型程序化调用，请跳过此过程。

**注**：运行 {{site.data.keyword.openwhisk_short}} 操作可能会发生成本。有关更多信息，请参阅[定价 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/openwhisk/learn/pricing){: new_window}。{{site.data.keyword.openwhisk_short}} 不会区分在测试期间从“试用”窗格发起的调用与从生产中的应用程序发起的调用。

要创建 {{site.data.keyword.openwhisk_short}} 操作，请完成以下步骤：

1.  转至[在线 {{site.data.keyword.openwhisk_short}} 编辑器 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.ng.bluemix.net/openwhisk/create){: new_window}，在其中可以直接在浏览器中编写代码。

    此外，还有一个[命令行界面 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/openwhisk/learn/cli){: new_window}，您可以进行安装，以支持使用您本地编写的代码来定义操作。

1.  使用其中一种支持的编程语言来创建 {{site.data.keyword.openwhisk_short}} 操作。有关详细信息，请参阅 [{{site.data.keyword.openwhisk_short}} 文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.ng.bluemix.net/docs/openwhisk/openwhisk_actions.html){: new_window}。

    请记住以下提示：

    - 要了解如何调用外部服务，请参阅 [{{site.data.keyword.openwhisk_short}} 操作的示例 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.ng.bluemix.net/docs/openwhisk/openwhisk_actions.html#openwhisk_apicall_action){: new_window}。
    - 要对 Watson 服务发起调用，请使用与您要使用的语言对应的 [Watson Developer Cloud SDK ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/watson-developer-cloud){: new_window}。
    - 确保 {{site.data.keyword.openwhisk_short}} 操作将任何输入参数作为 JSON 对象接受，并将任何输出作为 JSON 对象返回。
    - 如果是使用 Node.js 来编写 {{site.data.keyword.openwhisk_short}} 操作，请确保使用 `Promise` 进行异步处理。另请确保从 `main` 函数返回最终结果。

    或者，可以创建操作序列。
    {: tip}

## 处理错误

如果 {{site.data.keyword.openwhisk_short}} 操作遇到错误，错误消息会返回到对话，并存储为名为 `cloud_functions_call_error` 的响应变量的属性。例如，如果 {{site.data.keyword.openwhisk_short}} 操作无法获得来自外部服务的响应，或者如果 Cloud Function 操作失败，那么可能会发生该错误。如果 Cloud Function 凭证未提供或不正确，那么会返回错误。此上下文变量仅用于 server 操作；在客户机应用程序中，请考虑创建类似的对象来捕获错误信息，并将其作为上下文变量返回给对话。

您能以对话节点响应为条件来首先检查是否有错误。例如，通过将以下表达式添加到响应条件，可以确保仅当没有遇到任何错误时，才会显示引用 {{site.data.keyword.openwhisk_short}} 操作结果的响应：

```json
  $forecast_result.cloud_functions_call_error == null
```
{: codeblock}

对于 client 类型的程序化调用，可以通过定义上下文变量（例如 `action_error`）来传递有关错误处理的信息。可以将其作为结果变量的一部分传递回服务。然后，通过定义如下所示的响应条件，仅当没有遇到任何错误时才能显示响应：

```json
  $forecast_result.action_error == null
```
{: codeblock}

## 客户机调用示例
{: #action-client-example}

以下示例显示了对外部天气服务的调用可能是什么样的。该调用会添加到与节点响应关联的 JSON 编辑器中。在触发节点级别的响应时，槽已收集并存储来自用户的日期和位置信息。此示例假定将调用的服务具有名为 `/weather` 的端点，接受 `location` 和 `date` 参数，并返回 JSON 对象：`{"forecast": "<value>"}`。

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

通常，仅当需要新的用户输入时（例如在执行父节点之后且在执行其中某个子节点之前），服务才会从 POST /message 请求返回到客户机。但是，如果将 client 操作添加到节点，那么在求值后，服务将始终返回到客户机，以便可以返回操作调用的结果。为了避免在不应该等待用户输入的时候（例如，对于配置为直接跳转至子节点的节点）进行等待，服务会将以下值添加到消息上下文中：

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

下图说明了可以如何使用客户机调用来获取天气预报信息，并将其返回给用户。

![显示有人在询问天气预报，然后对话将该请求发送到客户机应用程序，而客户机应用程序接着将其发送到外部服务](images/forecast.png)

## 服务器调用示例
{: #action-server-example}

以下示例显示了对 {{site.data.keyword.openwhisk_short}} 操作的调用可能是什么样的。此示例显示了如何使用在随服务一起提供的[实用程序包 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/docs/openwhisk/openwhisk_actions.html#openwhisk_create_action_sequence){: new_window} 中定义的 {{site.data.keyword.openwhisk_short}} `echo` 操作。该操作采用文本字符串，并返回该文本字符串。

 ``` json
{
  "actions": [
    {
      "name": "/whisk.system/utils/echo",
      "type":"server",
      "parameters": {
        "message": "<?input.text?>"
      },
      "result_variable": "context.my_input_returned",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

现在，{{site.data.keyword.openwhisk_short}} 操作的输出（存储在 `context.my_input_returned` 变量中）可以由随后的对话节点进行访问。

 ``` json
 {
  "output": {
    "text": {
      "values": [
        "您的输入是：$my_input_returned。"
      ]
    }
  }
}
```
{: codeblock}

下图使用简单的示例说明了如何调用 {{site.data.keyword.openwhisk_short}} 操作，在该示例中将调用内置 {{site.data.keyword.openwhisk_short}} 回传服务。该操作要求用户进行输入，然后将其传递到回传服务。回传服务会返回相同的文本并向用户显示。

![显示有人在输入文本，然后对话将请求发送到 {{site.data.keyword.openwhisk_short}} 服务，该服务再将文本返回给对话。](images/echo-via-cf.png)

### Echo 操作示例

要查看具有已设置为调用 {{site.data.keyword.openwhisk_short}} 内置 Echo 操作的对话的工作空间，请完成以下步骤：

1.  下载 [CloudFunctionsEcho.json ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/watson-developer-cloud/community/raw/master/conversation/cloud-functions-echo.json){: new_window} 文件。
1.  将 JSON 文件作为新的工作空间导入。
1.  查看对话以了解如何指定对 Echo 操作的调用。
1.  在“试用”窗格中，单击**管理上下文**，然后（临时）将上下文变量设置为您的 {{site.data.keyword.openwhisk_short}} 用户名和密码。

    ```json
    $private.my_credentials
    {
      "user":"<your-CF-instance-username>",
      "password":"<your-CF-instance-password>"
    }
    ```
    {: codeblock}

1.  通过输入一些输入内容来测试对话。

    服务将使用 {{site.data.keyword.openwhisk_short}} Echo 操作将您输入的任何内容原封不动地返回给您。

## 高级服务器调用示例
{: #advanced-action-server-example}

可以从单个对话流中调用多个操作。实际上，可以在单个对话节点中的一个 `actions` JSON 对象内最多调用 5 个操作。但是，在 `actions` JSON 数组中定义的所有 server 类型操作将并行处理。因此，不能调用一个 server 类型操作，并将其结果传递到同一个 `actions` 块中的另一个 server 类型操作。以特定顺序调用 server 操作的最佳方法是使用 {{site.data.keyword.openwhisk_short}} 序列。在运行时，这种方法速度更快，因为对话只需要发起一次外部调用就能完成多个操作。要使用序列，只需引用序列名称，而不是 `actions` 块定义中的操作名称。或者，也可以从一个节点调用第一个 server 类型操作，然后跳转至调用下一个 server 类型操作的子节点。

如果定义一个混合了 client 类型和 server 类型操作的 `actions` 数组，那么在执行对话节点时，仅当所有 server 类型操作都处理完之后，client 操作才会发送到客户机。
{: tip}

以下示例显示了对 {{site.data.keyword.openwhisk_short}} 操作的调用可能是什么样的。

此示例显示了如何使用 {{site.data.keyword.openwhisk_short}} 操作来调用用于获取城市名称并返回所提供位置的纬度和经度坐标的外部服务。

``` json
{
  "actions": [
        {
      "name": "/jdoeorg_prod/get coordinates",
      "type":"server",
      "parameters": {
        "location": "$location"
      },
      "result_variable": "context.my_coordinates",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

`$my_coordinates` 上下文变量保存 `get coordinates` 服务返回的两个值，如下所示：

```json
{
 "lat":"42.3611",
 "long":"-71.0571"
}
```
{: codeblock}

此示例显示了如何使用在随 {{site.data.keyword.openwhisk_short}} 服务一起提供的 [Weather 包 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://console.bluemix.net/docs/openwhisk/openwhisk_weather.html#openwhisk_catalog_weather){: new_window} 中定义的 {{site.data.keyword.openwhisk_short}} `forecast` 操作。该操作需要纬度和经度坐标以及时间段。它会返回具有指定时间段内指定位置的预报信息的 JSON 对象。由之前操作返回的坐标指定为 `$my_coordinates.lat` 和 `$my_coordinates.long`。                                                     

 ``` json
{
  "actions": [
    {
      "name": "/whisk.system/weather/forecast",
      "type":"server",
      "parameters": {
        "latitude": "$my_coordinates.lat",
        "longitude": "$my_coordinates.long",
        "timePeriod": "$period",
        "username": "$private.my_weather_service.username",
        "password": "$private.my_weather_service.password"
      },
      "result_variable": "context.forecasts",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

**注**：username 和 password 列为参数。提供这两个参数仅仅是因为此特定操作需要；这两个参数一起定义了所提供操作在后端调用的外部天气服务所需的凭证。它们与 IBM Cloud Function 帐户凭证不同。另请采取措施使这些凭证保持私密性。

现在，{{site.data.keyword.openwhisk_short}} 操作的输出（存储在 `context.forecasts` 变量中）可以由随后的对话节点进行访问。

 ``` json
 {
  "output": {
    "text": {
      "values": [
        "在$location 的未来$period，天气应该为$forecasts。"
      ]
    }
  }
}
```
{: codeblock}

下图说明了一个复杂交互。用户询问了天气预报。对话节点中的槽提示用户输入位置和时间段信息。要调用 {{site.data.keyword.openwhisk_short}} 预报服务，必须提供地理坐标。因此，对话首先通过向 {{site.data.keyword.openwhisk_short}} 服务发送请求来获取用户所提供位置的纬度和经度详细信息，此服务会将该请求传递给用于返回坐标信息的外部服务。在随后的节点中，对话将新获得的坐标信息与从用户那里获取的时间段信息一起发送到 {{site.data.keyword.openwhisk_short}} 内置预报服务，以获取预报详细信息。然后，对话通过显示 {{site.data.keyword.openwhisk_short}} 预报服务提供的预报结果来回答用户的问题。

![显示有人在询问天气预报，然后对话将该请求发送到 Cloud Function 服务，以将其传递给外部服务。](images/forecast-via-cf.png)
