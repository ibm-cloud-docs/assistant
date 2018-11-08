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

# 使用 {{site.data.keyword.conversationshort}} 连接器部署到通道

开发工作空间后，可以使用 {{site.data.keyword.conversationshort}} 连接器将其快速连接到通信通道，例如 Slack 或 Facebook Messenger。{{site.data.keyword.conversationshort}} 连接器是一组 {{site.data.keyword.openwhisk}} 组件，用于调解 {{site.data.keyword.conversationshort}} 工作空间与您拥有的 Slack 或 Facebook 应用程序之间的通信，并将会话数据存储在 Cloudant 数据库中。

通过将工作空间连接到通道应用程序，可以使其作为 Slack 或 Facebook Messenger 用户可以与之交互的聊天机器人使用。对话中的响应可以包括多媒体和交互式回复，例如图像和可单击的按钮。（有关定义多媒体响应的更多信息，请参阅[多媒体响应](dialog-multimedia.html)。）

![{{site.data.keyword.openwhisk_short}} 部署概览图](images/deploytochannel_diagram.png)

{{site.data.keyword.conversationshort}} 连接器有一些限制：

- 您必须创建 Slack 或 Facebook 应用程序，或者具有修改要使用的应用程序的管理许可权。
- 由于 {{site.data.keyword.openwhisk_short}} 限制，此工具目前仅可用于 {{site.data.keyword.Bluemix_notm}} 美国南部区域。

## 使用 {{site.data.keyword.conversationshort}} 工具部署到 Slack

通过 {{site.data.keyword.conversationshort}} 工具，可使用 {{site.data.keyword.conversationshort}} 连接器快速部署机器人。该工具将引导您逐步完成收集必需信息来配置通道应用程序和连接器组件的过程，然后会自动将必需的组件部署到 IBM Cloud。

**注**：{{site.data.keyword.conversationshort}} 工具界面目前仅支持 Slack，但您可以使用自动化的 {{site.data.keyword.Bluemix_short}} 过程为 Facebook Messenger 进行部署。有关为 Facebook Messenger 进行部署的更多信息，请参阅 {{site.data.keyword.conversationshort}} 连接器 [GitHub 存储库 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/watson-developer-cloud/conversation-connector/blob/master/channels/facebook/README.md){: new_window} 中的文档。

要使用自动化部署工具来部署机器人，请执行以下操作：

1. 在 {{site.data.keyword.conversationshort}} 工具中，打开要部署的工作空间。
1. 单击左上角的“菜单”图标，然后选择**部署**。

   ![“快速部署”菜单选项](images/deploy_menu_testdeploy.png)

1. 在**使用 {{site.data.keyword.openwhisk_short}} 部署**下，单击**部署到 Slack**。
1. 在 Slack 页面上，单击**部署到 Slack 应用程序**，然后按照指示信息操作。

   ![“部署到 Slack 应用程序”按钮](images/deploy_deploytoslack.png)

## 手动部署

可以手动配置必需的组件并将其部署到 IBM Cloud，而不使用自动化工具将工作空间部署为机器人。在下面几种情况下，您可能希望执行手动部署：

- **部分重新部署**。您可能需要重新部署现有部署的某些组件，以便更改配置、更正问题或应用来自较新版本的修订。
- **使用新功能扩展机器人**。可以修改提供的组件以添加新功能，例如对用户输入进行预处理后，再将其发送到 {{site.data.keyword.conversationshort}} 工作空间。
- **部署到新通道**。如果要将机器人部署到除 Slack 或 Facebook Messenger 以外的其他通道，可以遵循现有组件的模式来开发自己的组件。

{{site.data.keyword.conversationshort}} 连接器组件在公共 GitHub 存储库中进行托管，您可以下载或克隆这些组件。有关更多信息，请参阅[存储库 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/watson-developer-cloud/conversation-connector){: new_window} 中提供的文档。

## 与机器人交谈

完成部署过程后，可以使用机器人用户名与 {{site.data.keyword.conversationshort}} 工作空间进行交互，就像使用其他任何 Slack 或 Facebook Messenger 机器人一样。

请记住，{{site.data.keyword.conversationshort}} 连接器会为与机器人交互的每个用户分别保存状态。（对于 Slack，单个用户可能会与机器人进行多种唯一会话，一种通过直接消息进行，一种在每个通道中进行。）这意味着存储在对话上下文中的任何变量都将无限期保留，除非您将其清除。

如果需要能够将会话重置为已知的起始状态，那么可以在对话中执行此操作。确保对话有一个节点可以在会话结束时执行或者在需要从头开始的任何时候执行。更新此节点的 JSON，以将所有上下文变量重置为相应的起始值。（如果需要，可以使用**跳转至**操作或特殊的“结束会话”意向来执行此节点。）

例如，如果工作空间使用名为 `drink_order` 的上下文变量来存储用户的饮料选择，那么在会话结束时，可以使用 `context.remove` 方法删除此变量：

```json
"context": {
   "reset_drink_order": "<?context.remove('drink_order')?>"
 }
```
{: codeblock}

有关修改上下文变量值的更多信息，请参阅[更新上下文变量值](dialog-runtime.html#context-update)。

您还可以通过编辑已部署的 {{site.data.keyword.openwhisk_short}} 操作来清除上下文，或对机器人的行为进行其他更改。有关更多信息，请参阅 [GitHub 存储库 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/watson-developer-cloud/conversation-connector){: new_window} 中提供的文档。
