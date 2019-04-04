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

# 添加集成
{: #deploy-integration-add}

要部署技能，请将其添加到助手，然后向助手添加集成，以用于将机器人发布到客户寻求帮助的通道。
{: shortdesc}

## 添加集成
{: #deploy-integration-add-task}

要向助手添加集成，请执行以下步骤：

1.  单击**助手**选项卡。

1.  单击以打开要部署的助手的磁贴。

1.  转至“集成”部分。

    **什么是“预览链接”集成？**创建助手时，会自动为您供应测试 Web 站点（除非您将其禁用）。该站点具有一个简单的交谈窗口小部件界面，可以用于与助手进行交互以进行测试。此外，还可以与团队成员共享此 IBM 品牌站点的 URL。

1.  单击**添加集成**。

1.  单击要集成助手的通道的磁贴。选项包括：

    - [定制应用程序](/docs/services/assistant?topic=assistant-deploy-custom-app)
    - [Facebook Messenger](/docs/services/assistant?topic=assistant-deploy-facebook)
    - [Intercom](/docs/services/assistant?topic=assistant-deploy-intercom)  ![仅限增强版或高端套餐](images/premium.png)
    - [预览链接](/docs/services/assistant?topic=assistant-deploy-web-link)
    - [Slack](/docs/services/assistant?topic=assistant-deploy-slack)
    - [Voice Agent（电话）![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://cloud.ibm.com/catalog/services/voice-agent-with-watson){: new_window}

      这将打开 {{site.data.keyword.cloud_notm}} 上的 **Voice Agent with Watson** 服务概述页面。
    - [WordPress 插件 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://wordpress.org/plugins/conversation-watson/){: new_window}

      这将打开 WordPress 站点上的 IBM Watson Assistant 插件概述页面。

1.  按照屏幕上提供的指示信息完成集成过程。

集成助手后，通过目标通道进行测试，以确保助手按预期工作。

有关如何在所有集成类型之间以一致的方式启动对话的提示，请参阅[启动对话](/docs/services/assistant?topic=assistant-dialog-start)。

## 集成限制
{: #deploy-integration-add-limits}

可以在单个服务实例中创建的集成数取决于 {{site.data.keyword.conversationshort}} 套餐。

|服务套餐          |每个助手的集成数|
|------------------|---------------------------:|
|高级                                 |100 |
|增强版            |100 |
|标准                                 |100 |
|Lite              |100 |
{: caption="服务套餐详细信息" caption-side="top"}
