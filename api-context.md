---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-13"

subcollection: assistant


---

{:curl: #curl .ph data-hd-programlang='curl'}
{:javascript: #javascript .ph data-hd-programlang='javascript'}
{:java: #java .ph data-hd-programlang='java'}
{:python: #python .ph data-hd-programlang='python'}
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

**Important:** One use of the context is to specify a unique user ID for each end user who interacts with the assistant. For user-based plans, this ID is used for billing purposes. (For more information, see [User-based plans](/docs/services/assistant?topic=assistant-services-information#user-based-plans).)

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
MessageInputOptions inputOptions = new MessageInputOptions();
inputOptions.setReturnContext(true);

MessageInput input = new MessageInput.Builder()
  .messageType("text")
  .text("Hello")
  .options(inputOptions)
  .build();

// create global context with user ID
MessageContextGlobalSystem system = new MessageContextGlobalSystem();
system.setUserId("my_user_id");
MessageContextGlobal globalContext = new MessageContextGlobal();
globalContext.setSystem(system);
  
// build user-defined context variables, put in skill-specific context for main skill
Map<String, String> userDefinedContext = new HashMap<>();
userDefinedContext.put("account_num","123456");
Map<String, Map> mainSkillContext = new HashMap<>();
mainSkillContext.put("user_defined", userDefinedContext);
MessageContextSkills skillsContext = new MessageContextSkills();
skillsContext.put("main skill", mainSkillContext);

MessageContext context = new MessageContext();
context.setGlobal(globalContext);
context.setSkills(skillsContext);

MessageOptions options = new MessageOptions.Builder("{assistant_id}", "{session_id}")
  .input(input)
  .context(context)
  .build();

MessageResponse response = service.message(options).execute().getResult();

System.out.println(response);
```
{: codeblock}
{: java}

In this example request, the application specifies a value for `user_id` as part of the global context. In addition, it sets one user-defined context variable (`account_number`) as part of the skill-specific context. This context variable can be accessed by dialog nodes as `$account_number`. (For more information about using the context in your dialog, see [How the dialog is processed](/docs/services/assistant?topic=assistant-dialog-runtime).)

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
