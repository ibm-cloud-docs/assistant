---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-13"

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

# 使用目录
{: #catalog}

通过***目录***，可轻松将常见意向添加到 {{site.data.keyword.conversationshort}} 服务工作空间。
{: shortdesc}

## 向工作空间添加目录
{: #add-catalog}

使用 {{site.data.keyword.conversationshort}} 工具来添加目录。

1.  在 {{site.data.keyword.conversationshort}} 工具中，打开工作空间，然后选择导航栏中的**目录**选项卡。如果**目录**未显示，请使用 ![菜单](images/Menu_16.png) 菜单来打开该页面。

1.  选择目录（如*帐单*）以查看随该目录一起提供的意向。

    ![显示可用目录的截屏](images/catalog_overview.png)

    您将看到有关在*帐单*类别中定义的意向的信息。

    ![显示“帐单”类别意向的截屏](images/catalog_open.png)

    从目录添加的意向可以通过其命名约定与其他意向区分开来；在本例中为 `#Billing_ . . .`

1.  选择 ![关闭箭头](images/close_arrow.png) 以返回到**目录**选项卡。

1.  接下来，通过单击`添加到机器人`按钮将*帐单*目录添加到工作空间。您将看到一条消息，指示*帐单*意向已添加到工作空间。

    ![显示“添加到机器人”按钮的截屏](images/catalog_addtobot.png)

1.  现在，选择**意向**选项卡，并验证*帐单*意向是否已添加到工作空间。

    ![显示“意向”选项卡上列出的“帐单”意向的截屏](images/catalog_intents.png)

### 结果

*帐单*目录中的意向已添加到工作空间的**意向**选项卡，系统会开始针对新数据进行自我培训。

## 编辑目录示例

与其他任何意向一样，将*帐单*目录意向添加到工作空间后，即可进行以下更改：

- 重命名意向。
- 删除意向。
- 添加、编辑或删除示例。
- 将示例移至其他意向。

可以通过 Tab 键从意向名称切换到每个示例，并根据需要编辑示例。

要移动或删除示例，请通过选中示例的对应复选框来选择该示例，然后选择**移动**或**删除**。

  ![显示如何移动或删除示例的截屏](images/catalog_edit.png)
