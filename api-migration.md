---

copyright:
  years: 2015, 2023
lastupdated: "2021-01-07"

subcollection: assistant


---

{{site.data.keyword.attribute-definition-list}}

Documentation for the **classic {{site.data.keyword.assistant_classic_short}}** experience has moved. For the most up-to-date version, see [Migrating to the v2 API](/docs/watson-assistant?topic=watson-assistant-api-migration){: external}.
{: attention}

# Migrating to the v2 API
{: #api-migration}

The Assistant v2 runtime API, which supports the use of assistants and skills, was introduced in November 2018. This API offers significant advantages over the v1 runtime API, including automatic state management, ease of deployment, skill versioning, and the availability of new features such as the search skill.

The v2 API is available for all users, regardless of service plan, at no additional cost.

The v2 API currently supports only runtime interaction with an existing assistant. Authoring applications that create or modify workspaces should continue to use the v1 API.
{: note}

## Overview

With the v2 API, your client app communicates with an assistant, rather than directly with a workspace. An assistant is a new orchestration layer that offers several new capabilities, including automatic state management, skill versioning, easier deployment, and (for Plus and Premium plans) search skills. Your existing workspace (now referred to as a _dialog skill_) continues to function as before, but the new capabilities are provided by the new assistant layer.

All communication with an assistant takes place within the context of a _session_, which maintains conversation state throughout the duration of the conversation. State data, including any context variables that are defined by your dialog or client application, are automatically stored by {{site.data.keyword.assistant_classic_short}}, without any action required on the part of your application.

State data persists until you explicitly delete the session, or until the session times out because of inactivity.

If you prefer to manage state yourself, the v2 API also provides a stateless `message` method that functions more like the v1 API. If you use the stateless `message` method, you do not need to explicitly create or delete sessions, and your app is responsible for maintaining context. For more information about the stateless `message` method, see the [API Reference](https://{DomainName}/apidocs/assistant/assistant-v2#messagestateless).
{: note}

If you have an existing application that uses the v1 API to send user input directly to a workspace, migrating your app to use the v2 API is a straightforward process.

## Set up an assistant

The v2 runtime API sends messages to an assistant, which routes the messages to your dialog skill (formerly workspace). To set up an assistant, use the {{site.data.keyword.assistant_classic_short}} user interface:

1. Click the **Skills** tab. Verify that your workspace is shown as an available skill. (All existing workspaces for your service instance are automatically converted to skills in the {{site.data.keyword.assistant_classic_short}} user interface. This conversion does not make any change to the underlying workspace.)

1. Click the **Assistants** tab. Click **Create assistant** to create a new assistant. When prompted to add skills, click **Add dialog skill** and select the dialog skill that corresponds to your workspace.

  For more information about creating assistants, see [Creating an assistant](/docs/assistant?topic=assistant-assistant-add).

1. After your new assistant is created, click the ![Menu](images/kebab.png) menu and then select **Settings**.

1. On the **Assistant Settings** page, find the assistant ID. Your application will use this ID (instead of a workspace ID) to communicate with the assistant. The service credentials are the same for both the v1 and v2 APIs.

  Currently, there is no API support for retrieving an assistant ID. To find the assistant ID, you must use the {{site.data.keyword.assistant_classic_short}} user interface.
  {: note}

## Call the v2 runtime API

After you have created an assistant, you can update your client application to use the v2 runtime API instead of the v1 runtime API.

1. Before sending the first message in a conversation, use the v2 [**Create a session**](https://{DomainName}/apidocs/assistant/assistant-v2#createsession){: external} method to create a session. Save the returned session ID:

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

1. Use the v2 [**Send user input to assistant**](https://{DomainName}/apidocs/assistant/assistant-v2#message){: external} method to send user input to the assistant. Instead of specifying the workspace ID as you did with the v1 API, you specify the assistant ID and the session ID:

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

  The basic message structure has not changed; in particular, the user input is still sent as `input.text`.

1. After a conversation ends, use the v2 [**Delete session**](https://{DomainName}/apidocs/assistant/assistant-v2#deletesession){: external} method to delete the session.

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

   If you do not explicitly delete the session, it will be automatically deleted after the configured timeout interval. (The timeout duration depends on your plan; for more information, see [Session limits](/docs/assistant?topic=assistant-assistant-settings#assistant-settings-session-limits).)

To see examples of the v2 APIs in the context of a simple client application, see [Building a client application](/docs/assistant?topic=assistant-api-client).

## Handle the v2 response format

Your application might need to be updated to handle the v2 runtime response format, depending on which parts of the response your application needs to access:

- Output for all response types (such as `text` and `option`) are still returned in the `output.generic` object. Application code for handling these responses should work without modification.

- Detected intents and entities are now returned as part of the `output` object, rather than at the root of the response JSON.

- The conversation context is now organized into two objects:

  - The **global context** contains system-level context data shared by all skills used by the assistant.

  - The **skill context** contains any user-defined context variables used by your dialog skill.

  However, keep in mind that state data, including conversation context, is now maintained by the assistant, so your application might not need to access the context at all (see [Let the assistant maintain state](#api-migration-state)).

Refer to the v2 [API Reference ](https://{DomainName}/apidocs/assistant/assistant-v2#message){: external} for complete documentation of the v2 response format.

## Let the assistant maintain state
{: #api-migration-state}

For most applications, you can now remove any code included for the purpose of maintaining state. It is no longer necessary to save the context and send it back to {{site.data.keyword.assistant_classic_short}} with each turn of the conversation. The context is automatically maintained by {{site.data.keassistant_classic_shortnshort}} and can be accessed by your dialog as before.

Note that with the v2 API, the context is by default not included in responses to the client application. However, your code can still access context variables if necessary:

- You can still send a `context` object as part of the message input. Any context variables you include are stored as part of the context maintained by {{site.data.keyword.assistant_classic_short}}. (If the context variable you send already exists in the context, the new value overwrites the previously stored value.)

  Make sure the context object you send conforms to the v2 format. All user-defined context variables sent by your application should be part of the skill context; typically, the only global context variable you might need to set is `system.user_id`, which is used by Plus and Premium plans for billing purposes.

- You can still retrieve context variables from either the global or skill context. To have the `context` object included with message responses, use the **return_context** property in the message input options. For more information, see [Accessing context data](/docs/assistant?topic=assistant-api-client-get-context).
