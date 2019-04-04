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

# 支持的语言
{: #language-support}

{{site.data.keyword.conversationshort}} 服务支持列出的语言。每种语言或多或少会支持服务的各个功能部件。
{: shortdesc}

在以下各表中，通过以下代码指示了语言和功能的支持级别：

- **GA** - 此功能部件一般可用且此语言支持。请注意，即便功能部件一般可用后，也仍然可能会继续更新。
- **Beta** - 此功能部件仅作为 Beta 发行版支持，并且仍在进行测试，要在测试后才可在此语言中一般可用。
- **不可用** - 指示功能在此语言中不可用。

第一个表显示了对所有功能的支持级别，但与意向和实体相关的功能除外，这些功能的支持级别在第二个表和第三个表中显示。

**表 1. 功能支持详细信息**

|语言|**定义[意向](/docs/services/assistant?topic=assistant-intents)**、**[实体](/docs/services/assistant?topic=assistant-entities)**和**[对话](/docs/services/assistant?topic=assistant-dialog-build)**|**搜索**|
|:---:|:---:|:---:|
|**英语 (en)**|GA|GA|
|**阿拉伯语 (ar)**|GA|不可用|
|**简体中文 (zh-cn)**|GA|Beta|
|**繁体中文 (zh-tw)**|Beta|Beta|
|**捷克语 (cs)**|GA|Beta|
|**荷兰语 (nl)**|GA|Beta|
|**法语 (fr)**|GA|Beta|
|**德语 (de)**|GA|Beta|
|**意大利语 (it)**|GA|Beta|
|**日语 (ja)**|GA|Beta|
|**韩国语 (ko)**|GA|Beta|
|**巴西葡萄牙语 (pt-br)**|GA|Beta|
|**西班牙语 (es)**|GA|Beta|
{: caption="功能支持详细信息" caption-side="top"}

**表 2. 意向功能支持详细信息**

|语言|**[绝对评分和“标记为不相关”](/docs/services/assistant?topic=assistant-intents#intents-mark-irrelevant)**|**[内容目录](/docs/services/assistant?topic=assistant-catalog)**|**[用户示例建议](/docs/services/assistant?topic=assistant-intent-recommendations#intent-recommendations-get-example-recommendations)**|
|:---:|:---:|:---:|:---:|
|**英语 (en)**|GA|GA|Beta|
|**阿拉伯语 (ar)**|Beta|GA|不可用|
|**简体中文 (zh-cn)**|GA|不可用|不可用|
|**繁体中文 (zh-tw)**|Beta|不可用|不可用|
|**捷克语 (cs)**|GA|不可用|不可用|
|**荷兰语 (nl)**|GA|不可用|不可用|
|**法语 (fr)**|GA|GA|不可用|
|**德语 (de)**|GA|GA|不可用|
|**意大利语 (it)**|GA|GA|不可用|
|**日语 (ja)**|GA|GA|不可用|
|**韩国语 (ko)**|GA|不可用|不可用|
|**巴西葡萄牙语 (pt-br)**|GA|GA|不可用|
|**西班牙语 (es)**|GA|GA|不可用|
{: caption="意向功能支持详细信息" caption-side="top"}

**表 3. 实体功能支持详细信息**

|语言|**系统实体（[数字](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-number)、[货币](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-currency)、[百分比](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-percentage)、[日期和时间](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-date-time)）**|**[实体模糊匹配](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching)**|**[上下文实体](/docs/services/assistant?topic=assistant-entities#entities-create-annotation-based)**|**[同义词建议](/docs/services/assistant?topic=assistant-entities#entities-synonyms)**
|:---|:---:|:---:|:---:|:---:|
|**英语 (en)**|GA，Beta（[位置](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-location)和[人员](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-person)）|GA|Beta|GA|
|**阿拉伯语 (ar)**|Beta|GA（仅用于拼写错误）|不可用|不可用|
|**简体中文 (zh-cn)**|GA|不可用|不可用|不可用|
|**繁体中文 (zh-tw)**|Beta|不可用|不可用|不可用|
|**捷克语 (cs)**|GA|GA（仅用于拼写错误）|不可用|不可用|
|**荷兰语 (nl)**|GA|GA（仅用于拼写错误）|不可用|不可用|
|**法语 (fr)**|GA|GA（仅用于拼写错误）|不可用|GA|
|**德语 (de)**|GA|GA（仅用于拼写错误）|不可用|不可用|
|**意大利语 (it)**|GA|GA（仅用于拼写错误）|不可用|不可用|
|**日语 (ja)**|GA|GA（仅用于拼写错误）|不可用|GA|
|**韩国语 (ko)**|GA|GA（仅用于拼写错误）|不可用|不可用|
|**巴西葡萄牙语 (pt-br)**|GA|GA（仅用于拼写错误）|不可用|不可用|
|**西班牙语 (es)**|GA|GA（仅用于拼写错误）|不可用|GA|
{: caption="实体功能支持详细信息" caption-side="top"}

**注：**{{site.data.keyword.conversationshort}} 服务支持注明的多种语言，但工具界面本身（描述、标签等）采用英语。所有支持的语言都可以通过英语界面进行输入和训练。

**GB18030 合规性**：GB18030 是一项中国标准，规定了用于中国市场的扩展代码页。此代码页标准对于软件行业来说非常重要，因为中国国家信息技术标准化技术委员会要求，在 2001 年 9 月 1 日之后向中国市场发布的任何软件应用程序都应支持 GB18030。{{site.data.keyword.conversationshort}} 服务支持此编码，并且已通过 GB1 8030 合规性认证。

## 更改技能语言
{: #language-support-change-language}

创建了技能后，就无法修改其语言。如果有必要更改技能的受支持语言，用户应该下载该技能。然后，在文本编辑器中编辑生成的 JSON 文件，搜索名为 `language` 的 JSON 属性。

`language` 属性应该设置为技能的原始语言；例如，英语将为 `en`。修改此属性的值，将其更改为所需语言（`fr` 表示法语，`de` 表示德语等）. 保存对 JSON 文件的更改，然后将修改后的文件导入到 {{site.data.keyword.conversationshort}} 服务实例中。

## 配置双向语言
{: #language-support-configure-bi-directional}

对于双向语言（例如，阿拉伯语），可以相应地更改技能首选项。在“技能”磁贴中，选择*操作*下拉菜单，然后选择**双向首选项**（此选项仅可用于设置为双向语言的技能）：

![双向首选项](images/bidi_prefs.png)

从技能的以下选项中进行选择：

- **GUI 方向**：指定图形用户界面中元素（例如，按钮或菜单）的布局方向。选择 `LTR`（从左到右）或 `RTL`（从右到左）。如果未指定，那么工具会遵循 Web 浏览器 GUI 方向设置。
- **文本方向**：指定所输入文本的方向。选择 `LTR`（从左到右）或 `RTL`（从右到左），或者选择`自动`以自动根据系统设置选择文本方向。`无`选项将显示从左到右的文本。
- **数字塑形**：指定显示常规数字时要使用的数字格式。请选择`名义`、`阿拉伯语-印度语`或`阿拉伯语-欧洲`。`无`选项将显示西方数字。
- **日历类型**：指定如何在技能 UI 中选择过滤日期。选择`伊斯兰希吉来历`、`伊斯兰历（表格式）`、`伊斯兰历（乌姆艾尔古拉历）`或`公历`。

  此设置不适用于“试用”面板。
  {: note}

![双向选项](images/bidi_opts.png)

完成选择后，单击**更新**以保存并返回到“技能”磁贴。

## 处理有重音的字符
{: #language-support-accents}

在会话设置中，用户在与 {{site.data.keyword.conversationshort}} 服务进行交互时，有可能会使用重音符。因此，对于意向检测和实体识别，对单词的有重音和无重音版本进行相同的处理。

但是，对于某些语言（例如，西班牙语），一些重音符可以改变实体的含义。因此，对于实体检测，尽管原始实体可能隐式具有重音符，但服务还是可以与同一实体的无重音版本相匹配，但置信度分数略低。

例如，对于单词“barrió”，该词具有重音符，并且对应于动词“barrer”（打扫）的过去时，那么服务还可能匹配单词“barrio”（邻居），但置信度略低。

系统将在具有完全匹配项的实体中提供最高置信度分数。例如，如果训练集内有 `barrió`，那么不会检测到 `barrio`；如果训练集内有 `barrio`，那么不会检测到 `barrió`。

您应该使用正确的字符和重音符来训练系统。例如，如果期望 `barrió` 作为响应，那么应该将 `barrió` 放入训练集。

同样的规则也适用于除重音符号标记以外带其他标记的单词，例如西班牙语字母 `ñ` 和字母 `n`（如“uña”与“una”）。在此例中，字母 `ñ` 并非只是带有重音符的 `n`；它是一个特定于西班牙语的独特字母。

如果您认为客户不会使用正确的重音符或者单词会拼写错误（例如，包括输入 `n` 而不是 `ñ`），那么可以启用模糊匹配，或者在训练示例中显式包含这些情况。
