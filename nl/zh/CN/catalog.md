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

# 使用内容目录
{: #catalog}

通过***内容目录***，可轻松将常见意向添加到 {{site.data.keyword.conversationshort}} 对话技能。
{: shortdesc}

从目录添加的意向旨在用作起点。添加或编辑目录意向，以针对您的用例对这些意向进行定制。

## 向对话技能添加内容目录
{: #catalog-add}

1.  打开对话技能，然后单击**内容目录**选项卡。

1.  选择内容目录（如*银行*）以查看随该目录一起提供的意向。

    ![显示可用目录的截屏](images/catalog_overview.png)

    您将看到有关该目录中包含的意向的信息。

    ![显示“银行”类别意向的截屏](images/catalog_open.png)

    从内容目录添加的意向可以通过其名称与其他意向区分开来。每个意向名称的开头都会附加有内容目录名称。

1.  选择 ![关闭箭头](images/close_arrow.png) 以返回到**内容目录**选项卡。

1.  接下来，通过单击`添加到技能`按钮将内容目录添加到对话技能。

1.  现在，选择**意向**选项卡，并验证该目录中的意向是否已添加并可用。

    ![显示“意向”选项卡上列出的“银行”意向的截屏](images/catalog_intents.png)

系统会开始对新数据进行自我训练。

将目录添加到技能后，意向会成为训练数据的一部分。如果 IBM 后续对内容目录进行更新，这些更改不会自动应用于从目录添加的任何意向。
{: note}

## 编辑内容目录示例
{: #catalog-edit-content}

与其他任何意向一样，将内容目录意向添加到技能后，可以对其进行以下更改：

- 重命名意向。
- 删除意向。
- 添加、编辑或删除示例。
- 将示例移至其他意向。
