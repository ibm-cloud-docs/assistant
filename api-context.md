---

copyright:
  years: 2015, 2020
lastupdated: "2020-06-11"

subcollection: assistant


---

{:curl: .ph data-hd-programlang='curl'}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: data-hd-programlang='swift'}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:download: .download}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# Accessing context data
{: #api-client-get-context}

The *context* is an object containing variables that persist throughout a conversation and can be shared by the dialog and the client application. If your application is using the v2 API, the context is automatically maintained by the assistant on a per-session basis. Both the dialog and the client application can read and write context variables. By default, the context is not returned to a client application, but you can optionally request that it be included in the response to each `/message` request.

**Important:** One use of the context is to specify a unique user ID for each end user who interacts with the assistant. For user-based plans, this ID is used for billing purposes. (For more information, see [User-based plans](/docs/assistant?topic=assistant-services-information#services-information-user-based-plans).)

There are two types of context:

- **Global context**: context variables that are shared by all skills used by an assistant, including internal system variables used to manage conversation flow.

- **Skill-specific context**: context variables specific to a particular skill, including any user-defined variables needed by your application. Currently, only one skill (named `main skill`) is supported.

## Example

The following example shows a `/message` request that includes both global and skill-specific context variables; it also uses the `options.return_context` property to request that the context be returned with the response.

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
userDefinedContext.put("account_num","123456");
MessageContextSkill mainSkillContext = new MessageContextSkill.Builder()
  .userDefined(userDefinedContext)
  .build();
MessageContextSkills skillsContext = new MessageContextSkills();
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

The output from this request includes not only the usual output, but also the context, showing that the specified values have been added.

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

For detailed information about how to access context variables using the API, see the [v2 API Reference](https://{DomainName}/apidocs/assistant/assistant-v2#send-user-input-to-assistant){: external}.)
