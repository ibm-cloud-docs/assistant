---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-07"

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

# 支持的语言
{{site.data.keyword.conversationshort}} 服务支持列出的语言。每种语言或多或少会支持服务的各个功能部件。
{: shortdesc}

在下表中，通过以下代码指示了语言和功能部件的支持级别：

- **GA** - 此功能部件一般可用且此语言支持。请注意，即便功能部件一般可用后，也仍然可能会继续更新。
- **Beta** - 此功能部件仅作为 Beta 发行版支持，并且仍在进行测试，要在测试后才可在此语言中一般可用。
- **空白** - 缺少任何代码都指示功能部件在此语言中不可用。

|                  | **[定义意向](intents.html)**、**[实体](entities.html)**和**[对话](dialog-build.html)**| **[绝对评分和“标记为不相关”](intents.html#mark-irrelevant)**| **系统实体（[number](system-entities.html#sys-number)、[currency](system-entities.html#sys-currency)、[percentage](system-entities.html#sys-percentage) 和 [date, time](system-entities.html#sys-datetime)）**| **[实体模糊匹配](entities.html#fuzzy-matching)**|
|:---|:---:|:---:|:---:|:---:|
| **英语 (en)**| GA| GA| GA</br> Beta（[location](system-entities.html#sys-location) 和 [person](system-entities.html#sys-person)）| Beta（词干提取、拼写错误检查和部分匹配）|
| **阿拉伯语 (ar)**| GA| Beta| Beta| Beta（仅用于拼写错误）|
| **简体中文 (zh-cn)**| Beta| Beta| Beta|  |
| **繁体中文 (zh-tw)**| Beta| Beta|  |  |
| **捷克语 (cs)**| Beta| Beta| Beta| Beta（仅用于拼写错误）|
| **荷兰语 (nl)**| Beta| Beta| Beta|  |
| **法语 (fr)**| GA| GA| GA| Beta（仅用于拼写错误）|
| **德语 (de)**| GA| GA| GA| Beta（仅用于拼写错误）|
| **意大利语 (it)**| GA| GA| GA| Beta（仅用于拼写错误）|
| **日语 (ja)**| GA| GA| GA| Beta（仅用于拼写错误）|
| **韩国语 (ko)**| GA| GA| GA| Beta（仅用于拼写错误）|
| **巴西葡萄牙语 (pt-br)**| GA| GA| GA| Beta（仅用于拼写错误）|
| **西班牙语 (es)**| GA| GA| GA| Beta（仅用于拼写错误）||

**注：**{{site.data.keyword.conversationshort}} 服务支持注明的多种语言，但工具界面本身（描述、标签等）是英语。所有支持的语言都可以通过英语界面进行输入和培训。

**GB18030 合规性**：GB18030 是一项中国标准，规定了用于中国市场的扩展代码页。此代码页标准对于软件行业来说非常重要，因为中国国家信息技术标准化技术委员会要求，在 2001 年 9 月 1 日之后向中国市场发布的任何软件应用程序都应支持 GB18030。{{site.data.keyword.conversationshort}} 服务支持此编码，并且已通过 GB1 8030 合规性认证。

## 更改工作空间语言

一旦创建了工作空间，就无法修改其语言。如果有必要更改工作空间的受支持语言，用户应该下载该工作空间。然后，在文本编辑器中编辑生成的 JSON 文件，搜索名为 `language` 的 JSON 属性。

`language` 属性应该设置为工作空间的原始语言；例如，英语将为 `en`。修改此属性的值，将其更改为所需语言（`fr` 表示法语，`de` 表示德语等）. 保存对 JSON 文件的更改，然后将修改后的文件导入到 {{site.data.keyword.conversationshort}} 服务实例中。

## 配置双向语言
{: #configuring-bi-directional}

对于双向语言（例如，阿拉伯语），可以相应地更改工作空间首选项。在“工作空间”选项卡中，选择*操作*下拉菜单，然后选择**双向首选项**（此选项仅可用于设置为双向语言的工作空间）：

![双向首选项](images/bidi_prefs.png)

从工作空间的以下选项中进行选择：

- **GUI 方向**：指定图形用户界面中元素（例如，按钮或菜单）的布局方向。选择 `LTR`（从左到右）或 `RTL`（从右到左）。如果未指定，那么工具会遵循 Web 浏览器 GUI 方向设置。
- **文本方向**：指定所输入文本的方向。选择 `LTR`（从左到右）或 `RTL`（从右到左），或者选择`自动`以自动根据系统设置选择文本方向。`无`选项将显示从左到右的文本。
- **数字塑形**：指定显示常规数字时要使用的数字格式。请选择`名义`、`阿拉伯语-印度语`或`阿拉伯语-欧洲`。`无`选项将显示西方数字。
- **日历类型**：指定如何在工作空间 UI 中选择过滤日期。选择`伊斯兰希吉来历`、`伊斯兰历（表格式）`、`伊斯兰历（乌姆艾尔古拉历）`或`公历`。**注**：此设置不适用于“试用”面板。

![双向选项](images/bidi_opts.png)

完成选择后，单击**更新**以保存并返回到“工作空间”选项卡。

## 处理有重音的字符
{: #working-with-accents}

在会话设置中，用户在与 {{site.data.keyword.conversationshort}} 服务进行交互时，有可能会使用重音符。因此，对于意向检测和实体识别，对单词的有重音和无重音版本进行相同的处理。

但是，对于某些语言（例如，西班牙语），一些重音符可以改变实体的含义。因此，对于实体检测，尽管原始实体可能隐式具有重音符，但服务还是可以与同一实体的无重音版本相匹配，但置信度分数略低。

例如，对于单词“barrió”，该词具有重音符，并且对应于动词“barrer”（打扫）的过去时，那么服务还可能匹配单词“barrio”（邻居），但置信度略低。

系统将在具有完全匹配项的实体中提供最高置信度分数。例如，如果培训集内有 `barrió`，那么不会检测到 `barrio`；如果培训集内有 `barrio`，那么不会检测到 `barrió`。

您应该使用正确的字符和重音符来培训系统。例如，如果期望 `barrió` 作为响应，那么应该将 `barrió` 放入培训集。

同样的规则也适用于除重音符号标记以外带其他标记的单词，例如西班牙语字母 `ñ` 和字母 `n`（如“uña”与“una”）。在此例中，字母 `ñ` 并非只是带有重音符的 `n`；它是一个特定于西班牙语的独特字母。

如果您认为客户不会使用正确的重音符或者单词会拼写错误（例如，包括输入 `n` 而不是 `ñ`），那么可以启用模糊匹配，或者在培训示例中显式包含这些情况。
