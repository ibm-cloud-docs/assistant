---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-30"

subcollection: assistant


---

{:curl: #curl .ph data-hd-programlang='curl'}
{:external: target="_blank" .external}
{:javascript: #javascript .ph data-hd-programlang='javascript'}
{:java: #java .ph data-hd-programlang='java'}
{:python: #python .ph data-hd-programlang='python'}
{:swift: data-hd-programlang='swift'}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:download: .download}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# 迁移到 V2 API
{: #api-migration}

Assistant V2 运行时 API 已于 2018 年 11 月推出，支持使用助手和技能。此 API 相比于 V1 运行时 API，提供了多项显著优势，包括自动状态管理、轻松部署、技能版本控制以及可用的新功能（如搜索技能）。

V2 API 可供所有用户使用，与服务套餐无关，也无需额外费用。

V2 API 目前仅支持与现有助手的运行时交互。用于创建或修改工作空间的编写应用程序应该继续使用 V1 API。
{: note}

## 概述

通过 V2 API，客户机应用程序可与助手进行通信，而不是直接与工作空间进行通信。助手是一个新的编排层，提供了多种新功能，包括自动状态管理、技能版本控制、更轻松的部署以及（适用于增强版和高端套餐）搜索技能。现有工作空间（现在称为_对话技能_）会继续像以前一样运行，但新的辅助层提供了新功能。

与助手的所有通信都在_会话_的上下文中进行，这会使其在会话期间保持会话状态。{{site.data.keyword.conversationshort}} 会自动存储状态数据（包括对话或客户机应用程序定义的任何上下文变量），无需应用程序方面执行任何操作。

状态数据会持久存储，直到您显式删除会话，或者由于没有活动而导致会话超时为止。

如果您有现有应用程序是使用 V1 API 将用户输入直接发送到工作空间，那么迁移应用程序以使用 V2 API 是一个简单的过程。

## 设置助手

V2 运行时 API 向助手发送消息，然后助手将消息路由到对话技能（原先称为工作空间）。要设置助手，请使用 {{site.data.keyword.conversationshort}} 用户界面：

1. 单击**技能**选项卡。验证工作空间是否显示为可用技能。（服务实例的所有现有工作空间都会自动转换为 {{site.data.keyword.conversationshort}} 用户界面中的技能。此转换不会对底层工作空间进行任何更改。）

1. 单击**助手**选项卡。单击**创建助手**以创建新的助手。提示添加技能时，单击**添加对话技能**，并选择对应于工作空间的对话技能。

  有关创建助手的更多信息，请参阅[创建助手](https://cloud.ibm.com/docs/services/assistant?topic=assistant-assistant-add)。

1. 创建新助手后，单击 ![菜单](images/kebab-react.png) 菜单，然后选择**设置**。

1. 在**助手设置**页面上，查找助手标识。应用程序将使用此标识（而不是工作空间标识）来与助手通信。V1 和 V2 API 的服务凭证相同。

  目前，API 不支持检索助手标识。要查找助手标识，必须使用 {{site.data.keyword.conversationshort}} 用户界面。
  {: note}

## 调用 V2 运行时 API

创建助手后，可以更新客户机应用程序以使用 V2 运行时 API，而不使用 V1 运行时 API。

1. 在会话中发送第一条消息之前，请使用 V2 [**创建会话**](https://cloud.ibm.com/apidocs/assistant-v2#create-a-session){: external}方法来创建会话。保存返回的会话标识：

  ```javascript
  service
  .createSession({
    assistant_id: assistantId,
  })
  .then(res => {
    sessionId = res.session_id;
  })
  ```
  {: codeblock}
  {: javascript}

  ```python
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

```
  {: codeblock }
  {: python }

  ```java
  CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
  SessionResponse session = service.createSession(createSessionOptions).execute().getResult();
  String sessionId = session.getSessionId();
  ```
  {: codeblock}
  {: java}

1. 使用 V2 [**向助手发送用户输入**](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant){: external}方法向助手发送用户输入。不要像使用 V1 API 那样指定工作空间标识，请改为指定助手标识和会话标识：

  ```javascript
  service
    .message({
      assistant_id: assistantId,
      session_id: sessionId,
      input: messageInput
    })
  ```
  {: codeblock}
  {: javascript}

  ```python
  response = service.message(
      assistant_id,
      session_id,
      input = message_input
  ).get_result()
  ```
  {: codeblock}
  {: python}

  ```java
MessageInput input = new MessageInput.Builder().text(inputText).build();
      MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId)
                                                  .input(input)
                                                  .build();
  MessageResponse response = service.message(messageOptions)
    .execute()
    .getResult();
  ```
  {: codeblock}
  {: java}

  基本消息结构未更改；尤其是，用户输入仍作为 `input.text` 发送。

1. 会话结束后，请使用 V2 [**删除会话**](https://cloud.ibm.com/apidocs/assistant-v2#delete-session){: external}方法来删除会话。

  ```javascript
  service
    .deleteSession({
      assistant_id: assistantId,
      session_id: sessionId,
    })
  ```
  {: codeblock}
  {: javascript}

  ```python
service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
```
  {: codeblock}
  {: python}

  ```java
  DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId
    .build();
  service.deleteSession(deleteSessionOptions).execute();
  ```
  {: codeblock}
  {: java}

   如果您未显式删除会话，那么系统将在配置的超时时间间隔后自动删除该会话。（超时持续时间取决于套餐；有关更多信息，请参阅[会话限制](/docs/services/assistant?topic=assistant-assistant-settings#assistant-settings-session-limits)。）

要查看简单客户机应用程序上下文中 V2 API 的示例，请参阅[构建客户机应用程序](/docs/services/assistant?topic=assistant-api-client)。

## 处理 V2 响应格式

应用程序可能需要更新才能处理 V2 运行时响应格式，具体取决于应用程序需要访问响应的哪些部分：

- 所有响应类型（例如，`text` 和 `option`）的输出仍在 `output.generic` 对象中返回。用于处理这些响应的应用程序代码应该无需修改就能正常工作。

- 现在，检测到的意向和实体会作为 `output` 对象的一部分返回，而不是在响应 JSON 的根返回。

- 现在，会话上下文组织成两个对象：

  - **全局上下文**包含由助手使用的所有技能共享的系统级别上下文数据。

  - **技能上下文**包含对话技能使用的任何用户定义的上下文变量。

  但是，请谨记，状态数据（包括会话上下文）现在由助手进行维护，因此应用程序可能根本不需要访问上下文（请参阅[让助手维护状态](#api-migration-state)）。

有关 V2 响应格式的完整文档，请参阅 V2 [API 参考](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant){: external}。

## 让助手维护状态
{: #api-migration-state}

现在，可以除去大多数应用程序为了维护状态而包含的任何代码。不必再保存上下文并针对每轮会话将其发送回 {{site.data.keyword.conversationshort}}。上下文将由 {{site.data.keyword.conversationshort}} 自动维护，并且对话可以像以前一样访问上下文。

请注意，使用 V2 API 时，缺省情况下，上下文并不会包含在对客户机应用程序的响应中。但是，如果需要，代码仍可以访问上下文变量：

- 您仍然可以将 `context` 对象作为消息输入的一部分发送。包含的任何上下文变量都会作为 {{site.data.keyword.conversationshort}} 维护的上下文的一部分存储。（如果发送的上下文变量在上下文中已存在，那么新值会覆盖先前存储的值。）

  确保发送的 context 对象符合 V2 格式。应用程序发送的所有用户定义的上下文变量都应该是技能上下文的一部分；通常，可能需要设置的唯一全局上下文变量是 `system.user_id`，该变量由增强版和高端套餐用于计费目的。

- 您仍可以从全局或技能上下文中检索上下文变量。要使 `context` 对象包含在消息响应中，请使用消息输入选项中的 **return_context** 属性。有关更多信息，请参阅[访问上下文数据](/docs/services/assistant?topic=assistant-api-client-get-context)。
