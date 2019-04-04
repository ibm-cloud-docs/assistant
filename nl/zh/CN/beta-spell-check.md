---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-11"

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

# 更正用户输入
{: #beta-spell-check}

此功能仅可供 Beta 程序参与者使用。要了解如何请求访问权，请参阅[参与 Beta 程序](/docs/services/assistant?topic=assistant-feedback#feedback-beta)。

![Beta](images/beta.png) IBM 发布的供您评估的服务、功能和语言支持分类为 Beta。这些功能可能不太稳定，可能经常会更改，还可能会临时通知中断使用。此外，Beta 功能可能无法提供与一般可用功能所提供级别相同的性能或兼容性，并且 Beta 功能不适用于生产环境。

启用*拼写检查*功能，以修正用户作为用户输入提交的发声中所犯的拼写错误。启用了拼写检查后，会自动更正拼写错误的词。评估输入时，使用的是更正后的词。提供的输入越精确，服务识别到实体提及项并理解用户意向的频率越高。

目前，只能为英语对话技能启用此设置。
{: note}

启用拼写检查后，用户输入将按以下方式进行更正：

- 原始输入：`letme applt for a memberdhip`
- 更正后的输入：`let me apply for a membership`

服务在评估是否更正某个词的拼写时，并不是依赖简单的字典查找过程，而是使用自然语言处理和概率模型的组合来评估一个词汇实际上是否拼写错误而应该更正。

## 启用拼写检查
{: #beta-spell-check-enable}

要启用拼写检查功能，请完成以下步骤：

1.  在“技能”页面中，找到您的技能。
1.  单击 ![打开和关闭选项列表](images/kabob-beta.png) 图标，然后选择**设置**。
1.  在*拼写检查*设置页面中，开启**拼写检查自动更正**。

### 测试拼写更正
{: #beta-spell-check-test}

1.  在“试用”窗格中，提交包含某些拼写错误的词的发声。

    如果输入中的词拼写错误，那么会自动更正这些词，并且会显示 ![自动更正](images/auto-correct.png) 图标。更正后的发声会加下划线。
1.  将鼠标悬停在带下划线的发声上可查看原始拼写。

如果有拼写错误的词汇，并且您预期服务会予以更正，但服务并未更正，那么请复查服务用于决定是否要更正某个词的规则，以确定该词是否属于服务有意不更改的词的类别。

为了避免过度更正，服务不会更正以下类型输入的拼写：

- 首字母大写的单词
- 表情符号
- 位置实体，例如状态和街道地址
- 数字和计量单位或时间
- 专有名词，例如常见的名字或公司名称
- 用引号括起的文本
- 包含特殊字符（例如连字符 (-)、星号 (*) 和 & 符号）的词
- *属于*此技能的词，即因为出现在实体值、实体同义词或意向用户示例中而具有隐含意义的词。

如果未更正的词明显不是上述其中一种输入类型，那么可能应该检查实体是否启用了模糊匹配。

#### 拼写更正是如何与模糊匹配相关的？
{: #beta-spell-check-vs-fuzzy-matching}

模糊匹配可帮助服务识别用户输入中基于字典的实体提及项。它使用字典查找方法将用户输入中的词与技能训练数据中的现有实体值或同义词相匹配。例如，如果用户输入了 `books` ，并且训练数据包含实体同义词 `book`，那么模糊匹配会识别这两个词汇的含义是否相同。

同时启用拼写检查和模糊匹配时，模糊匹配功能会在触发拼写检查之前运行。如果模糊匹配找到一个词汇可以与现有字典实体值或同义词相匹配，那么它会将该词汇添加到*属于*该技能，因此不会进行更正的词的列表中。同样，如果用户输入类似 `I want to buy a boook` 的语句，那么模糊匹配会识别到词汇 `boook` 与实体同义词 `book` 的含义相同，从而将其添加到受保护词列表中。因此，服务*不会*更正 `boook` 的拼写。

#### 工作方式
{: #beta-spell-check-how-it-works}

通常，用户输入会按原样保存在消息的 `input` 对象的 `text` 字段中。当且仅当以某种方式更正了用户输入时，才会在 `input` 对象中创建名为 `original_text` 的新字段。此字段会存储包含任何拼写错误的词的用户原始输入。更正后的文本会添加到 `input.text` 字段。
