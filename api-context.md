---

copyright:
  years: 2015, 2023
lastupdated: "2022-12-14"

subcollection: assistant


---

{{site.data.keyword.attribute-definition-list}}

Documentation for the **classic {{site.data.keyword.assistant_classic_short}}** experience has moved. For the most up-to-date version, see [Accessing context data in dialog](/docs/watson-assistant?topic=watson-assistant-api-client-get-context){: external}.
{: attention}

# Accessing context data
{: #api-client-get-context}

The *context* is an object containing variables that persist throughout a conversation and can be shared by the dialog and the client application. Both the dialog and the client application can read and write context variables.

You can choose whether you want the context to be maintained by your application or by the {{site.data.keyword.assistant_classic_short}} service:

- If you use the stateful v2 `message` API, the context is automatically maintained by the assistant on a per-session basis. Your application must explicitly create a session at the beginning of each conversation; the context is stored by the service as part of the session and is not returned in message responses unless you request it. For more information, see the [v2 API Reference](https://cloud.ibm.com/apidocs/assistant/assistant-v2#message){: external}.

- If you use the stateless v2 `message` API (or the legacy v1 `message` API) your application is responsible for storing the context after each conversation turn and sending it back to the service with the next message. For a complex application, or an application that needs to store personally identifiable information, you might choose to store the context in a database.

  A session ID is automatically generated at the beginning of the conversation, but no session data is stored by the service. With the stateless `message` API, the context is always included with each message response. For more information, see the [v2 API Reference](https://cloud.ibm.com/apidocs/assistant/assistant-v2#messagestateless){: external}.

**Important:** One use of the context is to specify a unique user ID for each end user who interacts with the assistant. For user-based plans, this ID is used for billing purposes. (For more information, see [User-based plans](/docs/assistant?topic=assistant-admin-managing-plan#admin-managing-plan-user-based).)

There are two types of context:

- **Global context**: context variables that are shared by all skills used by an assistant, including internal system variables used to manage conversation flow. The global context includes the user ID, as well as other global values such as the time zone and language of the assistant.

- **Skill-specific context**: context variables specific to a particular skill, including any user-defined variables needed by your application. Currently, only one skill (named `main skill`) is supported.

User-defined context variables that you specify in a dialog node are part of the `user_defined` object within the skill context when accessed using the API. Note that this structure differs from the `context` structure that appears in the JSON editor in the {{site.data.keyword.assistant_classic_short}} user interface. For example, you might specify the following in the JSON editor:

```json
"context": {
  "my_context_var": "this is the value"
}
```

In the v2 API, you would access this user-defined variable as follows:

```json
"context": {
  "skills": {
    "main skill": {
      "user_defined": {
        "my_context_var": "this is the value"
      }
    }
  }
}
```

For detailed information about how to access context variables using the API, see the [v2 API Reference](https://{DomainName}/apidocs/assistant/assistant-v2){: external}.

## Example
{: #api-context-example}

The following example shows a stateful `/message` request that includes both global and skill-specific context variables; it also uses the `options.return_context` property to request that the context be returned with the response. Note that this option is applicable only if you are using the stateful `message` method, because the stateless `message` method always returns the context.

```javascript
service
  .message({
    assistant_id: '{assistant_id}',
    session_id: '{session_id}',
    input: {
      message_type: 'text',
      text: 'Hello',
      options: {
        'return_context': true
      }
    },
    context: {
      'global': {
        'system': {
          'user_id': 'my_user_id'
        }
      },
      'skills': {
        'main skill': {
          'user_defined': {
            'account_number': '123456'
          }
        }
      }
    }
  })
  .then(res => {
    console.log(JSON.stringify(res, null, 2));
  })
  .catch(err => {
    console.log(err);
  });
```
{: codeblock}
{: javascript}

```python
response=service.message(
    assistant_id='{assistant_id}',
    session_id='{session_id}',
    input={
        'message_type': 'text',
        'text': 'Hello',
        'options': {
            'return_context': True
        }
    },
    context={
        'global': {
            'system': {
                'user_id': 'my_user_id'
            }
        },
        'skills': {
            'main skill': {
                'user_defined': {
                    'account_number': '123456'
                }
            }
        }
    }
).get_result()

print(json.dumps(response, indent=2))
```
{: codeblock}
{: python}

```java
MessageInputOptions inputOptions = new MessageInputOptions.Builder()
  .returnContext(true)
  .build();

MessageInput input = new MessageInput.Builder()
  .messageType("text")
  .text("Hello")
  .options(inputOptions)
  .build();

// create global context with user ID
MessageContextGlobalSystem system = new MessageContextGlobalSystem.Builder()
  .userId("my_user_id")
  .build();
MessageContextGlobal globalContext = new MessageContextGlobal.Builder()
  .system(system)
  .build();

// build user-defined context variables, put in skill-specific context for main skill
Map<String, Object> userDefinedContext = new HashMap<>();
userDefinedContext.put("account_number","123456");
MessageContextSkill mainSkillContext = new MessageContextSkill.Builder()
  .userDefined(userDefinedContext)
  .build();
Map<String, MessageContextSkill> skillsContext = new HashMap<>();
skillsContext.put("main skill", mainSkillContext);

MessageContext context = new MessageContext.Builder()
  .global(globalContext)
  .skills(skillsContext)
  .build();

MessageOptions options = new MessageOptions.Builder()
  .assistantId("{assistant_id}")
  .sessionId("{session_id}")
  .input(input)
  .context(context)
  .build();

MessageResponse response = service.message(options).execute().getResult();

System.out.println(response);
```
{: codeblock}
{: java}

In this example request, the application specifies a value for `user_id` as part of the global context. In addition, it sets one user-defined context variable (`account_number`) as part of the skill-specific context. This context variable can be accessed by dialog nodes as `$account_number`. (For more information about using the context in your dialog, see [How the dialog is processed](/docs/assistant?topic=assistant-dialog-runtime).)

You can specify any variable name you want to use for a user-defined context variable. If the specified variable already exists, it is overwritten with the new value; if not, a new variable is added to the context.

The output from this request includes not only the usual output, but also the context, showing that the specified values have been added. If you are using the stateless `message` method, this context data must be stored locally and sent back to the {{site.data.keyword.assistant_classic_short}} service as part of the next message. If you are using the stateful `message` method, this context is stored automatically and will persist for the life of the session.

```json
{
  "output": {
    "generic": [
      {
        "response_type": "text",
        "text": "Welcome to the Watson Assistant example!"
      }
    ],
    "intents": [
      {
        "intent": "hello",
        "confidence": 1
      }
    ],
    "entities": []
  },
  "user_id": "my_user_id",
  "context": {
    "global": {
      "system": {
        "turn_count": 1,
        "user_id": "my_user_id"
      }
    },
    "skills": {
      "main skill": {
        "user_defined": {
          "account_number": "123456"
        }
      }
    }
  }
}
```

## Restoring conversation state
{: #api-context-restore-state}

In some situations, you might want the ability to restore a conversation to a previous state.

You can use the `export` option on stateful `message` requests to specify that you want the context object in the response to include complete session state data. If you specify `true` for this option, the returned skill context includes an encoded `state` property that represents the current conversation state.

If you are using the stateful `message` API, the service stores conversation state data only for the life of the session. However, if you save this context data (including `state`) and send it back to the service with a subsequent message request, you can restore the conversation to the same state, even if the original session has expired or has been deleted.

If you are using the stateless `message` API, the `state` property is always included in responses (along with the rest of `context`). Although stateless sessions do not expire, you can still use this state data to reset a conversation to a previous state.
