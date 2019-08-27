---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-29"

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

# 更改不活动状态超时设置 ![仅限增强版或高端套餐](images/plus.png)
{: #assistant-settings}

用户通过其中一个内置集成与助手进行交互时，如果用户在特定时间段内未与助手进行交互，那么交谈会话会结束。可以通过更改不活动状态超时设置，指定等待多长时间后重置交谈会话。
{: shortdesc}

重置交谈会话时，对话会失去先前与用户交流期间保存的任何上下文信息。例如，如果对话询问用户的姓名，然后在对话的其余部分中通过该名称调用用户，那么在交谈会话结束并开始新的会话之后，对话将再次从询问用户的姓名开始。

## 会话限制
{: #assistant-settings-session-limits}

允许的不活动状态超时长度根据服务实例套餐类型而有所不同。下表列出了限制。

|服务套餐|交谈会话缺省不活动时间段|交谈会话最长不活动时间段|
|--------------|--------------------------------:|----------------------------:|
|高端                                 |60 分钟|24 小时|
|增强版                               |60 分钟|24 小时|
|标准                                 |5 分钟|5 分钟|
|增强试用版                           |5 分钟|5 分钟|
|轻量*            |5 分钟|5 分钟|
{: caption="服务套餐详细信息" caption-side="top"}

## 更改设置
{: #assistant-settings-timeout-task}

1.  单击助手的菜单，然后选择**设置**。

1.  更改**超时限制**字段中指定的时间。

1.  关闭“设置”页面。这将自动保存您的更改。
