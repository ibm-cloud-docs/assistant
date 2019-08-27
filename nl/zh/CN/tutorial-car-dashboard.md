---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-12"

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
{:gif: data-image-type='gif'}

# 教程：构建汽车仪表板对话
{: #tutorial-car-dashboard}

在本教程中，您将使用 {{site.data.keyword.conversationshort}} 服务来创建一个对话，用于帮助用户与智能汽车仪表板进行交互。
{: shortdesc}

## 学习目标
{: #car-dashboard-objectives}

完成本教程后，您将了解如何执行以下操作：

- 定义实体
- 计划对话
- 在对话中使用节点和响应条件

### 持续时间
{: #tutorial-car-dashboard-duration}

完成本教程大约需要 2 到 3 个小时。

### 先决条件
{: #tutorial-car-dashboard-prereqs}

开始之前，请先完成[入门教程](/docs/services/assistant?topic=assistant-getting-started)。

您将使用您所创建的 {{site.data.keyword.conversationshort}} 教程技能，向您在入门练习中构建的简单对话添加节点。

## 步骤 1：添加意向和示例
{: #tutorial-car-dashboard-add-intents}

在“意向”选项卡上添加意向。意向是用户输入中表达的目的或目标。

1.  在 {{site.data.keyword.conversationshort}} 教程技能的**意向**页面中，单击**添加意向**。
1.  添加以下意向名称，然后单击**创建意向**：

    ```
    turn_on
    ```
    {: codeblock}

    这会在指定意向名称的前面附加 `#`。`#turn_on` 意向指示用户希望开启设备，如收音机、挡风玻璃雨刷或前灯。
1.  在**添加用户示例**字段中，输入以下话语，然后单击**添加示例**：

    ```
    我需要灯光
    ```
    {: codeblock}

1.  添加下面 5 个示例，以帮助 Watson 识别 `#turn_on` 意向。

    ```
    放点音乐
    打开收音机
    打开
    请开换气
    把空调开大点
    打开前灯
    ```
    {: codeblock}

1.  单击**关闭** ![关闭箭头](images/close_arrow.png) 图标以完成添加 `#turn_on` 意向的操作。

您现在有 3 个意向：刚才添加的 `#turn_on` 意向以及在作为先决条件步骤完成的*入门教程*中添加的 `#hello` 和 `#goodbye` 意向。每个意向都有一组示例话语，可帮助训练 Watson 识别用户输入中的意向。

## 步骤 2：添加实体
{: #tutorial-car-dashboard-add-entities}

实体定义包含一组可用于触发不同响应的实体*值*。每个实体值可以具有多个*同义词*，用于定义用户输入中指定相同值时可能用到的不同方式。

创建在用户输入中可能会出现的具有 #turn_on 意向的实体，以表示用户要开启的对象。

1.  单击**实体**选项卡以打开“实体”页面。
1.  单击**添加实体**。
1.  添加以下实体名称，然后按 Enter 键：

    ```
    设备
    ```
    {: codeblock}

    在您指定的实体名称前会附加 `@`。`@appliance` 实体表示用户可能希望开启的汽车中的设备。
1.  将以下值添加到**值名称**字段中：

    ```
    收音机
    ```
    {: codeblock}

    该值表示用户可能想开启的具体设备。
1.  在**同义词**字段中添加用于指定收音机设备实体的其他方式。按 **Tab** 键使焦点位于该字段，然后输入以下同义词。每输入一个同义词后，都请按 **Enter** 键。

    ```
    音乐
    乐曲
    ```
    {: codeblock}

1.  单击**添加值**以完成为 `@appliance` 实体定义`收音机`值的操作。
1.  添加其他类型的设备。

    - 值：`前灯`。同义词：`灯`。
    - 值：`空调`。同义词：`冷气`和 `AC`。

1.  单击切换开关以使 `@appliance` 实体的模糊匹配为**开启**。此设置将帮助助手识别用户输入中对实体的引用，即便实体的指定方式与此处使用的语法不完全匹配也能识别。
1.  单击**关闭** ![关闭箭头](images/close_arrow.png) 图标以完成添加 `@appliance` 实体的操作。
1.  重复步骤 2-8 以创建 `@genre` 实体并启用模糊匹配，然后输入以下值和同义词：

    - 值：`古典`。同义词：`交响乐`。
    - 值：`节奏布鲁斯`。同义词：`r&b`。
    - 值：`摇滚乐`。同义词：`rock & roll`、`摇滚乐`和`流行音乐`。

您已定义两个实体：`@appliance`（表示助手可以打开的设备）和 `@genre`（表示用户可以选择收听的音乐类型）。

收到用户的输入时，{{site.data.keyword.conversationshort}} 服务会识别意向和实体。现在，可以定义一个对话，以使用意向和实体来选择正确的响应。

## 步骤 3：创建复杂对话
{: #tutorial-car-dashboard-complex-dialog}

在此复杂对话中，您将创建一个对话分支，用于处理先前定义的 #turn_on 意向。

### 为 #turn_on 添加根节点
{: #tutorial-car-dashboard-add-turn-on}

创建一个对话分支以响应 #turn_on 意向。首先创建根节点：

1.  单击 **#hello** 节点上的“更多”图标 ![“更多”选项](images/kabob.png)，然后选择**在下方添加节点**。
1.  在“条件”字段中开始输入 `#turn_on`，然后从列表中进行相应选择。此条件由与 #turn_on 意向匹配的任何输入触发。
1.  不要在此节点中输入响应。单击 ![关闭](images/close.png) 以关闭节点编辑视图。

### 场景
{: #tutorial-car-dashboard-scenarios}

对话需要确定用户要开启哪个设备。为了处理此问题，请根据其他条件创建多个响应。

根据您定义的意向和实体，有三种可能的场景：

**场景 1**：用户想要打开音乐，在此情况下，助手必须询问音乐的类型。

**场景 2**：用户想要打开任何其他有效设备，在此情况下，助手必须在响应消息中使用所请求设备的名称并指示正在打开该设备。

**场景 3**：用户指定的设备名称无法识别，在此情况下，助手必须请求用户说明清楚。

添加用于按以下顺序检查这些场景条件的节点，使对话能首先对最具体的条件求值。

### 处理场景 1
{: #tutorial-car-dashboard-address-scenario1}

添加节点来处理场景 1，即用户希望打开音乐。在响应中，助手必须询问音乐类型。

#### 添加用于检查设备类型是否为音乐的子节点
{: #tutorial-car-dashboard-add-music-check}

1.  单击 **#turn_on** 节点上的“更多”图标 ![“更多”选项](images/kabob.png)，然后选择**添加子节点**。
1.  在“条件”字段中，输入 `@appliance:radio`。如果 @appliance 实体的值为`收音机`或在“实体”选项卡上定义的某个同义词，那么此条件为 true。
1.  在“响应”字段中，输入`您想听哪种音乐？`
1.  将节点命名为 `Music`。
1.  单击 ![关闭](images/close.png) 以关闭节点编辑视图。

#### 添加从 #turn_on 节点至 Music 节点的跳转
{: #tutorial-car-dashboard-add-jump-to-music}

直接从 `#turn on` 节点跳转至 `Music` 节点，而无需再要求提供更多用户输入。为此，可以使用**跳转至**操作。

1.  单击 **#turn_on** 节点上的“更多”图标 ![“更多”选项](images/kabob.png)，然后选择**跳转至**。
1.  选择 **Music** 子节点，然后选择**如果机器人识别（条件）**，以指示要处理 Music 节点的条件。

![跳转至（之前）](images/tut-dialog-jumpto.png)

请注意，在添加**跳转至**操作之前，必须已创建目标节点（要跳转至的节点）。

创建“跳转至”关系后，在树中会看到新条目：

![跳转至（之后）](images/tut-dialog-jump2.png)

#### 添加用于检查音乐类型的子节点
{: #tutorial-car-dashboard-check-genre}

现在，添加一个节点，用于处理用户请求的音乐类型。

1.  单击 **Music** 节点上的“更多”图标 ![“更多”选项](images/kabob.png)，然后选择**添加子节点**。仅在用户响应有关希望收听的音乐类型的问题后，才会对此子节点求值。因为在此节点之前需要用户输入，所以不需要使用**跳转至**操作。
1.  将 `@genre` 添加到“条件”字段。每当检测到 @genre 实体的有效值时，此条件为 true。
1.  输入`好，播放 @genre。` 作为响应。此响应将重复用户提供的 genre 值。

#### 添加用于处理用户响应中无法识别的类型的节点
{: #tutorial-car-dashboard-catch-genre}

添加一个节点，用于在无法识别用户指定的 @genre 值时进行响应。

1.  单击 *@genre* 节点上的“更多”图标 ![“更多”选项](images/kabob.png)，然后选择**在下方添加节点**以创建对等节点。
1.  在“条件”字段中输入 `true`。true 条件是一种特殊条件，用于指定当对话流到达此节点时，应该始终求值为 true。（如果用户指定有效的 @genre 值，那么将永远不会访问此节点。）
1.  输入`不好意思，我没听懂。我可以播放古典、节奏、蓝调或摇滚乐。` 作为响应。

这就包括了用户要求开启音乐的所有情况。

#### 测试用于音乐的对话
{: #tutorial-car-dashboard-test-music}

1.  选择 ![询问 Watson](images/ask_watson.png) 图标以打开交谈窗格。
1.  输入`播放音乐`。助手识别到 #turn_on 意向和 @appliance:music 实体后，会通过询问音乐类型来进行响应。

1.  输入有效的 @genre 值（例如，`rock`）。助手识别到 @genre 实体后，会进行相应的响应。

    ![显示成功的播放音乐请求](images/tut-test-music.png)

1.  再次输入`播放音乐`，但这次指定无效的类型响应。助手会以不明白进行响应。

### 处理场景 2
{: #tutorial-car-dashboard-address-scenario2}

我们将添加节点来处理场景 2，即用户希望打开其他有效设备。在此情况下，助手必须在响应消息中使用所请求设备的名称并指示正在打开该设备。

#### 添加用于检查任何设备的子节点
{: #tutorial-car-dashboard-check-applicance}

添加一个在用户提供 @appliance 的其他有效值时触发的节点。对于其他 @appliance 值，助手无需要求更多输入。它仅返回一个肯定响应。

1.  单击**音乐**节点上的“更多”图标 ![“更多”选项](images/kabob.png)，然后选择**在下方添加节点**以创建对等节点；此节点会在 @appliance:music 条件求值后进行求值。
1.  输入 `@appliance` 作为节点条件。如果用户输入包含除 music 以外的其他任何可识别 @appliance 实体值，那么将触发此条件。
1.  输入`好，打开 @appliance。` 作为响应。此响应将重复用户提供的设备值。

#### 使用其他设备测试对话
{: #tutorial-car-dashboard-test-other-appliances}

1.  选择 ![询问 Watson](images/ask_watson.png) 图标以打开交谈窗格。
1.  输入`开灯`。

    助手识别到 #turn_on 意向和 @appliance:headlights 实体后，会以`好的，正在打开前灯`进行响应。

    ![显示成功的开灯请求](images/tut-test-lights.png)

1.  输入`打开空调`。

    助手识别到 #turn_on 意向和 @appliance:(air conditioning) 实体后，会以`好的，正在打开空调`进行响应。

1.  根据已定义的示例话语和实体同义词，尝试所有支持命令的变体。

### 处理场景 3
{: #tutorial-car-dashboard-address-scenario3}

现在，添加一个对等节点，此节点在用户未指定有效设备类型的情况下触发。

1.  单击 **@appliance** 节点上的“更多”图标 ![“更多”选项](images/kabob.png)，然后选择**在下方添加节点**以创建对等节点；此节点会在 @appliance 条件求值后进行求值。
1.  在“条件”字段中输入 `true`。（如果用户指定有效的 @appliance 值，那么将永远不会访问此节点。）
1.  输入`抱歉，我不确定听懂了没有。我可以打开音乐、前灯或者空调。`作为响应。

#### 再测试一些变体
{: #tutorial-car-dashboard-test-more}

1.  尝试使用更多的话语变体来测试对话。

    如果助手无法识别正确的意向，那么可以直接从交谈窗口对其进行重新训练。选择不正确的意向旁的箭头，然后从列表中选择正确的意向。

    ![显示选择其他意向并重新训练](images/tut-change-intent.gif){: gif}

您可以选择查看**客户服务 - 样本**技能，以了解如何使用更长的对话和其他功能让这个相同的用例更加充实完整。

1.  单击导航菜单中的**返回到技能**按钮 ![显示菜单中的“返回到技能”按钮](images/workspaces-button.png)。

1.  单击**添加样本**。

    这会将样本技能添加到您的技能列表中。该样本技能尚未与任何助手相关联。

## 后续步骤
{: #tutorial-car-dashboard-deploy}

现在，您已构建并测试了对话技能，可以将其共享给客户了。要部署对话技能，请先将对话技能连接到助手，然后部署助手。有多种方式可以执行此操作。有关更多详细信息，请参阅[添加集成](/docs/services/assistant?topic=assistant-deploy-integration-add)。

您可以从 [GitHub ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://github.com/watson-developer-cloud/car-dashboard) 中访问完整汽车仪表板样本应用程序的源代码。
