---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-24"

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

# 在 Slack 中测试

可以使用测试部署工具将 {{site.data.keyword.conversationshort}} 工作空间作为机器人用户集成到 Slack 团队。如果要使用 Slack 机器人作为工作空间的用户界面进行快速测试，请使用此方法。

测试部署工具使用 {{site.data.keyword.openwhisk}} 服务将预构建的 Slack 应用程序作为机器人用户部署到您的团队。此应用程序会处理与 {{site.data.keyword.conversationshort}} 工作空间的通信。

![测试部署概述图](images/testdeploy_diagram.png)

请注意，测试部署工具有一些限制：

- 不能使用此工具来发布应用程序以供其他团队使用。
- 如果使用此方法将多个工作空间部署到同一团队，那么所有工作空间都将响应 `@ibmwatson_bt` 用户名。建议使用此工具时，一次仅将一个工作空间部署到每个 Slack 团队。
- 您必须有权将应用程序安装到 Slack 团队。如果不确定自己是否具有此许可权，请与 Slack 管理员核实。
- 预构建的 Slack 应用程序仅用于测试，并且可能不一定始终可用。
- 由于 {{site.data.keyword.openwhisk_short}} 限制，此工具目前仅可用于 {{site.data.keyword.Bluemix_notm}} 美国南部区域。

要将应用程序安装为机器人用户，请执行以下操作：

1. 在 {{site.data.keyword.conversationshort}} 工具中，打开要在 Slack 中测试的工作空间。
1. 单击左上角的“菜单”图标，然后选择**部署**。这将打开“部署选项”页面。

   ![“快速部署”菜单选项](images/deploy_menu_testdeploy.png)

1. 在**使用 {{site.data.keyword.openwhisk_short}} 部署**下，单击**在 Slack 中测试**并按照指示信息操作。

   ![“创建 Slack 测试”按钮](images/testdeploy_testinslack.png)

## 与机器人交谈

完成部署过程后，可以使用 `@ibmwatson_Bot` 用户名与 {{site.data.keyword.conversationshort}} 工作空间进行交互，就像使用其他任何 Slack 机器人一样。

请记住，部署到团队的机器人会为特定通道中的每个用户保留状态。这意味着存储在对话上下文中的任何变量都将无限期保留，除非对话将其清除。

如果需要能够将会话重置为已知的起始状态，那么必须在对话中执行此操作。确保对话有一个节点可以在会话结束时执行或者在需要从头开始的任何时候执行。更新此节点的 JSON，以将所有上下文变量重置为相应的起始值。（如果需要，可以使用**跳转至**操作或特殊的“结束会话”意向来执行此节点。）

例如，如果工作空间使用名为 `drink_order` 的上下文变量来存储用户的饮料选择，那么在会话结束时，可以使用 `context.remove` 方法删除此变量：

```json
"context": {
   "reset_drink_order": "<?context.remove('drink_order')?>"
 }
```
{: codeblock}

有关修改上下文变量值的更多信息，请参阅[更新上下文变量值](dialog-overview.html#updating-a-context-variable-value)。

**注：**完成对工作空间的测试后，可以通过返回到测试部署工具并单击**删除测试**来删除测试部署。请记住，还必须单独对 Slack 团队中的机器人应用程序取消授权。
