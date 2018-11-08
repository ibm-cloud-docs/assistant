---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-09"

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

# 多媒体响应

如果工作空间已使用 [{{site.data.keyword.conversationshort}} 连接器](conversation-connector.html)与 Slack 或 Facebook Messenger 集成，那么可以指定对话节点响应，其中包含多媒体或交互式元素，如可单击的按钮。要指定交互式响应，可以将 JSON 数据块插入到对话节点的输出中。

**注**：如果要使用与 Slack 的交互式消息，请确保已启用交互式消息支持。有关更多信息，请参阅 Slack 部署[自述文件 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/watson-developer-cloud/conversation-connector/blob/master/channels/slack/README.md#interactive-messages){: new_window}。

## 通用 JSON 格式

可以使用支持 Slack 或 Facebook Messenger 部署的通用 JSON 格式来指定交互式响应。要以通用 JSON 格式指定交互式响应，请将 JSON 插入到对话节点响应的 `output.generic` 字段中。以下示例显示可如何发送包含多种响应类型（文本、图像和可单击选项）的响应：

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "这些是离您最近的门店。"
      },
      {
        "response_type": "image",
        "source": "http://...",
        "title": "图像标题",
        "description": "图像的一些描述"
      },
      {
        "response_type": "option",
        "title": "单击以下其中一项",
        "options": [
          {
            "label": "位置 1",
            "value:" "位置 1"
          },
          {
            "label": "位置 2",
            "value:" "位置 2"
          },
          {
            "label": "位置 3",
            "value:" "位置 3"
          }
        ]
      }
    ]
  }
}
```

有关支持的响应类型以及如何指定这些类型的更多信息，请参阅[响应类型](#response-types)。

在运行时，{{site.data.keyword.conversationshort}} 连接器将此响应转换为通道（Slack 或 Facebook Messenger）所需的格式。如果响应包含多种媒体类型或附件，那么通用响应将转换为一系列单独的消息有效内容。然后，连接器在单独的消息中将每个消息有效内容发送到通道。

**注**：一个响应拆分成多条消息时，{{site.data.keyword.conversationshort}} 连接器会将这些消息按顺序发送到通道。由通道负责将这些消息传递给最终用户；这可能会受网络或服务器问题的影响。

## 本机 JSON 格式

除了通用 JSON 格式外，{{site.data.keyword.conversationshort}} 连接器还支持使用本机 Slack 和 Facebook Messenger 格式编写的特定于通道的响应。如果需要指定通用 JSON 格式当前不支持的响应类型，您可能要使用本机 JSON 格式。

可以使用对话节点响应中的相应字段为 Slack 或 Facebook 指定本机 JSON：

- `output.slack`：插入要包含在 Slack 响应的 `attachment` 字段中的任何 JSON。有关 Slack JSON 格式的更多信息，请参阅 Slack [文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://api.slack.com/docs/message-attachments){: new_window}。

- `output.facebook`：插入要包含在 Facebook 响应的 `message.attachment.payload` 字段中的任何 JSON。有关 Facebook JSON 格式的更多信息，请参阅 Facebook [文档 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: new_window}。

## 响应类型

通用 JSON 格式支持以下响应类型。

### 图像

显示 URL 指定的图像。

#### 字段

| 名称| 类型| 描述| 必需？|
|---------------|--------|------------------------------------|-----------|
| response_type | 枚举值 | `image`| 是|
| source        | 字符串 | 图像的 URL。指定的图像必须为 .jpg、.gif 或 .png 格式。| 是|
| title         | 字符串 | 要在图像前面显示的标题。| 否|
| description   | 字符串 | 图像附带的描述的文本。| 否|

#### 示例

此示例显示带有标题和描述性文本的图像。

```json
{
  "response_type": "image",
  "source": "http://example.com/image.jpg",
  "title": "示例图像",
  "description": "作为多媒体响应的一部分返回的示例图像。"
}
```

### 选项

显示用户可以单击以选择选项的一组按钮。然后，指定的值会作为用户输入发送到工作空间。

#### 字段

| 名称| 类型| 描述| 必需？|
|---------------|--------|-------------------------------------|-----------|
| response_type | 枚举值 | `option`| 是|
| title         | 字符串 | 要在选项前面显示的标题。| 是|
| description   | 字符串 | 选项附带的描述的文本。| 否|
| options       | 列表   | 指定用户可以从中进行选择的选项的键/值对列表。| 是|
| options[].label | 字符串 | 面向用户的选项标签。| 是|
| options[].value | 字符串 <br/>对象| 用户选择选项时将发送到 {{site.data.keyword.conversationshort}} 服务的值。这可以是字符串（对于简单响应）或包含多个字段的对象。请参阅下面的示例。| 是|

#### 示例

此示例显示两个选项：

- 选项 1（标记为“购物”）发送简单的字符串消息 (`option_1`)，该消息使用消息的 `input.text` 字段发送到工作空间。
- 选项 2（标记为“退出”）发送包含输入文本和意向数组的复杂消息。响应可以包含作为 {{site.data.keyword.conversationshort}} 消息有效部分的任何字段。（有关消息输入结构的更多信息，请参阅 [API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://www.ibm.com/watson/developercloud/conversation/api/v1/?curl#send_message){: new_window}。）

```json
{
  "response_type": "option",
  "title": "从以下选项中选择",
  "options": [
    {
      "label": "购物",
      "value": "下单"
    },
    {
      "label": "退出",
      "value": {
        "input": {
      "text": "退出"
        },
        "intent": [
          {
            "intent": "exit_app",
            "confidence": 1.0
          }
        ]
      }
    }
  ]
}
```

### 文本

显示文本。

#### 字段

| 名称| 类型| 描述| 必需？|
|---------------|--------|--------------------|-----------|
| response_type | 枚举值 | `text`| 是|
| text          | 字符串 | 要显示的文本。这可以包含换行符 (`\n`) 和 Markdown 标记（如果通道支持）。（通道不支持的任何格式设置都会被忽略。）| 是|

#### 示例

此示例向用户显示文本消息。

```json
{
  "response_type": "text",
  "text": "这是文本响应。"
}
```
