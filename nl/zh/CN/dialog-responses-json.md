---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-11"

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

# 使用 JSON 编辑器定义响应
{: #dialog-responses-json}

在某些情况下，您可能需要使用 JSON 编辑器来定义响应。（有关对话响应的更多信息，请参阅[响应](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)。）通过编辑响应 JSON，您可以直接访问将返回到通信通道或定制应用程序的数据。

## 通用 JSON 格式
{: #dialog-responses-json-generic}

响应的通用 JSON 格式用于指定针对任何通道的响应。此格式可以适应 Slack 和 Facebook 集成支持的各种响应类型，也可以由定制客户机应用程序来实现。（这是缺省情况下，对通过 {{site.data.keyword.conversationshort}} 工具定义的对话响应使用的格式。）

有关如何通过工具打开 JSON 编辑器以编辑对话节点响应的信息，请参阅 [JSON 编辑器中的上下文变量](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-var-json)。

要以通用 JSON 格式指定交互式响应，请将相应的 JSON 对象插入到对话节点响应的 `output.generic` 字段中。以下示例显示可如何发送包含多种响应类型（文本、图像和可单击选项）的响应：

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "values": [
          {
            "text": "这些是离您最近的门店。"
          }
        ]
      },
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "示例图像",
        "description": "图像的一些描述。"
      },
      {
        "response_type": "option",
        "title": "单击以下其中一项",
        "options": [
          {
            "label": "位置 1",
            "value": {
              "input": {
      "text": "位置 1"
              }
            }
          },
          {
            "label": "位置 2",
            "value": {
              "input": {
      "text": "位置 2"
              }
            }
          },
          {
            "label": "位置 3",
            "value": {
              "input": {
      "text": "位置 3"
              }
            }
          }
        ]
      }
    ]
  }
}
```

有关如何使用 JSON 对象指定每个受支持响应类型的更多信息，请参阅[响应类型](#dialog-responses-json-response-types)。

如果使用的是 {{site.data.keyword.conversationshort}} 连接器，那么在运行时响应会转换为通道（Slack 或 Facebook Messenger）所需的格式。如果响应包含多种媒体类型或附件，那么通用响应会根据需要转换为一系列不同的消息有效内容。然后，连接器在单独的消息中将每个消息有效内容发送到通道。

**注**：一个响应拆分成多条消息时，{{site.data.keyword.conversationshort}} 连接器会将这些消息按顺序发送到通道。由通道负责将这些消息传递给最终用户；这可能会受网络或服务器问题的影响。

如果要构建您自己的客户机应用程序，那么应用程序必须根据需要实现每种响应类型。有关更多信息，请参阅[实现响应](/docs/services/assistant?topic=assistant-api-dialog-responses)。

## 本机 JSON 格式
{: #dialog-responses-json-native}

除了通用 JSON 格式外，对话节点 JSON 还支持使用本机 Slack 和 Facebook Messenger 格式编写的特定于通道的响应。此外，{{site.data.keyword.conversationshort}} 连接器也支持这些格式。如果您确定工作空间将仅与一种通道类型集成，并且需要指定通用 JSON 格式当前不支持的响应类型，您可能要使用本机 JSON 格式。

可以使用对话节点响应中的相应字段为 Slack 或 Facebook 指定本机 JSON：

- `output.slack`：插入要包含在 Slack 响应的 `attachment` 字段中的任何 JSON。有关 Slack JSON 格式的更多信息，请参阅 Slack [文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://api.slack.com/docs/message-attachments){: new_window}。

- `output.facebook`：插入要包含在 Facebook 响应的 `message.attachment.payload` 字段中的任何 JSON。有关 Facebook JSON 格式的更多信息，请参阅 Facebook [文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: new_window}。

## 响应类型
{: #dialog-responses-json-response-types}

通用 JSON 格式支持以下响应类型。

### 图像
{: #dialog-responses-json-image}

显示 URL 指定的图像。

#### 字段
{: #{: #dialog-responses-json-image-fields}

|名称|类型|描述|必需？|
|---------------|--------|------------------------------------|-----------|
|response_type |枚举值|`image`|是|
|source        |字符串|图像的 URL。指定的图像必须为 .jpg、.gif 或 .png 格式。|是|
|title         |字符串|要在图像前面显示的标题。|否|
|description   |字符串|图像附带的描述的文本。|否|

#### 示例
{: #dialog-responses-json-image-example}

此示例显示带有标题和描述性文本的图像。

```json
{
  "output": {
    "generic":[
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "示例图像",
        "description": "作为多媒体响应的一部分返回的示例图像。"
      }
    ]
  }
}
```

### 选项
{: #dialog-responses-json-option}

显示用户可用于选择选项的一组按钮或下拉列表。然后，指定的值会作为用户输入发送到工作空间。

#### 字段
{: #dialog-responses-json-option-fields}

|名称|类型|描述|必需？|
|---------------|--------|-------------------------------------|-----------|
|response_type |枚举值|`option`|是|
|title         |字符串|要在选项前面显示的标题。|是|
|description   |字符串|选项附带的描述的文本。|否|
|preference    |枚举值|要显示的首选类型的控件（如果通道支持）（`dropdown` 或 `button`）。{{site.data.keyword.conversationshort}} 连接器当前仅支持 `button`。|否|
|options       |列表   |指定用户可以从中进行选择的选项的键/值对列表。|是|
|options[].label |字符串|面向用户的选项标签。|是|
|options[].value |对象|定义用户选择选项时将发送到 {{site.data.keyword.conversationshort}} 服务的响应的对象。|是|
|options[].value.input |对象|输入对象，包含与选项对应的输入文本。|否|
|options[].value.input.text |字符串|针对选项将发送到助手的文本。|否|

#### 示例
{: #dialog-responses-json-option-example}

此示例显示两个选项：

- 选项 1（标注为`购物`）发送简单的字符串消息（`下单`），该消息将作为输入文本发送到工作空间。
- 选项 2（标注为`退出`）发送包含输入文本和意向数组的复杂消息。响应可以包含作为 {{site.data.keyword.conversationshort}} 消息有效部分的任何字段。（有关消息输入结构的更多信息，请参阅 [API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}。）

```json
{
  "output": {
    "generic":[
      {
        "response_type": "option",
        "title": "从以下选项中选择：",
        "preference": "button",
        "options": [
          {
            "label": "购物",
            "value": {
              "input": {
      "text": "下单"
              }
            }
          },
          {
            "label": "退出",
      "value": {
        "input": {
      "text": "退出"
        },
              "intents": [
                {
                  "intent": "exit_app",
            "confidence": 1.0
          }
        ]
            }
          }
        ]
      }
    ]
  }
}
```

### 暂停
{: #dialog-responses-json-pause}

在将下一条消息发送到通道之前暂停，并可选择发送“用户正在输入”事件（对于支持此事件的通道）。

#### 字段
{: #dialog-responses-json-pause-fields}

|名称|类型|描述|必需？|
|---------------|--------|--------------------|-----------|
|response_type |枚举值|`pause`|是|
|time          |整数|暂停持续时间（以毫秒为单位）。|是|
|typing        |布尔值|暂停期间是否发送“用户正在输入”事件。如果通道不支持此事件，将忽略此事件。|否|

#### 示例
{: #dialog-responses-json-pause-example}

此示例在暂停持续 5 秒期间发送“用户正在输入”事件。

```json
{
  "output": {
    "generic":[
      {
        "response_type": "pause",
        "time": 5000,
        "typing": true
      }
    ]
  }
}
```

### 文本
{: #dialog-responses-json-text}

显示文本。要增加多样性，可以指定多个备用文本响应。如果指定多个响应，那么可以选择按顺序在列表中循环，随机选择响应，或输出所有指定的响应。

#### 字段
{: #dialog-responses-json-text-fields}

|名称|类型|描述|必需？|
|---------------|--------|--------------------|-----------|
|response_type |枚举值|`text`|是|
|values        |列表   |定义文本响应的一个或多个对象的列表。|是|
|values.[_n_].text   |字符串|响应的文本。这可以包含换行符 (`\n`) 和 Markdown 标记（如果通道支持）。（通道不支持的任何格式设置都会被忽略。）|否|
|selection_policy |字符串|指定了多个响应时，如何从列表中选择响应。可能的值为 `sequential`、`random` 和 `multiline`。|否|
|delimiter     |字符串|输出作为响应之间分隔符的定界符。 仅当 `selection_policy`=`multiline` 时使用。缺省定界符为换行符 (`\n`)。|否|

#### 示例
{: #dialog-responses-json-text-example}

此示例向用户显示问候消息。

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "values": [
          { "text": "您好！" },
          { "text": "嗨，您好！" }
        ],
        "selection_policy": "random"
      }
    ]
  }
}  
```
