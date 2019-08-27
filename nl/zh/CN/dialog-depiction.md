---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-28"

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

# 对话图
{: #dialog-depiction}

![包含示例内容的样本对话树](images/dialog-depiction-full.png) 

此图显示了使用图形用户界面对话编辑器构建的对话树的实体模型。它包含两个根对话节点。典型的对话树可能有更多节点，但通过此图，可一窥其中一小部分节点可能是什么样的。

- 第一个根节点以意向值为条件。它有两个子节点，每个子节点均以一个实体值为条件。第二个子节点定义了两个响应。如果上下文变量的值与条件中指定的值相匹配，那么将向用户返回第一个响应。否则，将返回第二个响应。

  要捕获有关特定主题的问题，然后在根响应中询问由子节点处理的跟进问题，此标准类型的节点会很有用。例如，它可能会识别用户有关折扣的问题，然后提出跟进问题：用户是否属于与公司有特殊折扣协议的任何组织。子节点会根据用户对有关组织成员资格问题的回答来提供不同的响应。

- 第二个根节点是带槽的节点。它也以意向值为条件。此节点定义了一组槽，其中要向用户收集的每条信息一个槽。每个槽会提出一个问题，以从用户那里探取回答。它会在用户对提示的回复中查找特定实体值，然后将其保存在槽上下文变量中。

  对于收集代表用户执行事务时可能需要的详细信息，此类型的节点很有用。例如，如果用户的意向是预订航班，那么槽可以收集出发地和目的地位置信息、出行日期等。
