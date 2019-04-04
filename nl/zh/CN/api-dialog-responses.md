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

# 实现响应
{: #dialog-api-responses}

对话节点可以使用包含文本、图像或交互式元素（如可单击选项）的响应来对用户进行响应。如果要构建您自己的客户机应用程序，那么必须实现对话返回的所有响应类型的正确显示。（有关对话响应的更多信息，请参阅[响应](/docs/services/assistant?topic=assistant-dialog-overview#responses)。）

## 响应输出格式
{: #dialog-api-responses-output}

缺省情况下，对话节点的响应在从 /message API 返回的响应 JSON 中的 `output.generic` 对象中指定。`generic` 对象包含由最多 5 个响应元素组成的数组，这些响应元素可用于任何通道。以下 JSON 示例显示了包含文本和图像的响应：

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "好，这是一张狗的照片。"
      },
      {
        "response_type": "image",
        "source": "http://example.com/dog.jpg"
      }
    ],
    "text" : ["好，这是一张狗的照片。"]
  }
}
```

**注：**如此示例所示，文本响应（`好，这是一张狗的照片。`）还会在 `output.text` 数组中返回。包含此响应是为了使不支持 `output.generic` 格式的应用程序具有向后兼容性。

客户机应用程序负责相应地处理所有响应类型。在本例中，应用程序需要向用户显示指定的文本和图像。

## 响应类型
{: #dialog-api-responses-types}

响应的每个元素都是其中一种支持的响应类型（目前为 `image`、`option`、`pause` 和 `text`）。每种响应类型都使用一组不同的 JSON 属性进行指定，因此为每个响应包含的属性将根据响应类型而变化。有关 /message API 的响应模型的完整信息，请参阅 [API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}。）

此部分描述了可用的响应类型以及这些类型在 /message API 响应 JSON 中的表示方式。（如果使用的是 Watson SDK，那么可以使用为您的语言提供的界面来访问相同的对象。）

**注：**此部分中的示例显示了运行时从 /message API 返回的 JSON 数据的格式。请记住，这与用于定义对话节点内响应的 JSON 格式不同。有关更多信息，请参阅[使用 JSON 编辑器定义响应](/docs/services/assistant?topic=assistant-dialog-responses-json)。

### 图像
{: #dialog-api-responses-image}

`image` 响应类型指示客户机应用程序显示图像，并可选择附带标题和描述：

```json
{
  "output": {
    "generic":[
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "图像示例",
        "description": "这是示例图像"
      }
    ]
  }
}
```

应用程序负责检索 `source` 属性指定的图像，并将其显示给用户。如果提供了可选的 `title` 和 `description`，那么应用程序可以通过任何适当的方式进行显示（例如，在图像下方呈现 title，而将 description 呈现为悬停文本）。

### 选项
{: #dialog-api-responses-option}

`option` 响应类型指示客户机应用程序显示用户界面控件，以支持用户从选项列表中选择，然后根据所选选项将输入发回 {{site.data.keyword.conversationshort}} 服务：

```json
{
  "output": {
    "generic":[
      {
        "response_type": "option",
        "title": "可用选项",
        "description": "请选择下列其中一个选项：",
        "preference": "button",
        "options": [
          {
            "label": "选项 1",
            "value": {
              "input": {
      "text": "选项 1"
              }
            }
          },
          {
            "label": "选项 2",
            "value": {
              "input": {
      "text": "选项 2"
              }
            }
          }
        ]
      }
    ]
  }
}
```

应用程序可以使用任何适用的用户界面控件（例如，一组按钮或下拉列表）来显示指定的选项。如果支持，可选的 `preference` 属性指示应用程序应该使用的首选控件类型（`button` 或 `dropdown`）。

对于每个选项，`label` 属性指定应该在 UI 控件中为选项显示的标签文本。`value` 属性指定用户选择相应的选项时，应该发回 {{site.data.keyword.conversationshort}} 服务的输入（使用 /message API）。请注意，`value` 属性不仅可以包含文本输入，还可以包含其他输入对象，如意向和实体，所有这些对象都应该发送到服务。

### 暂停
{: #dialog-api-responses-pause}

`pause` 响应类型指示应用程序等待指定的时间间隔，然后再显示下一个响应：

```json
{
  "output": {
    "generic":[
      {
        "response_type": "pause",
        "time": 500,
        "typing": false
      }
    ]
  }
}
```

对话可能会请求此暂停，以便有时间完成请求，或者仅仅是模拟可能在响应之间暂停的人工座席的表现。暂停可以持续最长 10 秒的任意时间。

`pause` 响应通常与其他响应组合在一起发送。应用程序应该暂停 `time` 属性指定的时间间隔（以毫秒为单位），然后再显示数组中的下一个响应。可选的 `typing` 属性请求客户机应用程序显示“用户正在输入”指示符（如果支持），以模拟人工座席。

### 文本
{: #dialog-api-responses-text}

`text` 响应类型用于对话中的普通文本响应：

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "好，您要在下周一飞往波士顿。"
      }
    ]
  }
}
```

请注意，为了实现向后兼容性，相同的文本还会包含在对话响应的 `output.text` 数组中（如本页面开头的示例中所示）。
