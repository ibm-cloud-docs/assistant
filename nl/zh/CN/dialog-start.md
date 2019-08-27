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

# 启动对话
{: #dialog-start}

您不能按相同的方式对所有集成使用内置欢迎节点来启动对话。请改为使用以下变通方法。
{: shortdesc}

系统将显示为对话中的欢迎节点定义的响应，以便可从“试用”窗格以及从“预览链接”集成中的交谈窗口小部件启动会话。但是，在其他许多通道集成中不会显示此响应，因为在用户启动的对话流中会跳过具有 `welcome` 特殊条件的节点。此外，部署的助手通常会等待用户来启动与助手的会话，而不是用户等待助手来启动会话。

`conversation_start` 特殊条件与 `welcome` 特殊条件不同，前者始终在对话启动时触发。您可以将节点与这两个特殊条件（`welcome` 和 `conversation_start`）组合使用，以通过一致的方式来管理对话启动。

要管理对话启动，请完成以下步骤：

1.  创建对话时，在“欢迎”节点上方添加一个对话节点，该节点会自动添加到对话树顶部。

1.  将此新添加节点的节点条件设置为 `conversation_start`，这是前文描述的特殊条件。

1.  在 `conversation_start` 节点中，针对对话，定义要设置缺省值的任何上下文变量。

1.  不要为此节点定义文本响应。

1.  将此节点配置为跳转至对话树中其正下方的`欢迎`节点，然后选择**如果机器人识别（条件）**。

![对话树的屏幕快照，其中的 conversation_start 节点跳转至其下方的欢迎节点。](images/dialog-start.png)

此设计会使对话如下进行工作：

- 无论对于哪种集成类型，都将处理 `conversation_start` 节点，这意味着将初始化在其中定义的任何上下文变量。
- 在助手启动对话流的集成中，将触发`欢迎`节点，并显示其文本响应。
- 在用户启动对话流的集成中，将评估用户的第一个输入，然后由可以提供最佳响应的节点进行处理。
