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
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:download: .download}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# 访问上下文数据
{: #api-client-get-context}

*context* 是一个包含变量的对象，这些变量在整个会话中持久存储，并且可以由对话和客户机应用程序共享。如果应用程序使用的是 V2 API，那么上下文由助手逐个会话自动进行维护。对话和客户机应用程序都可以读取和写入上下文变量。缺省情况下，上下文不会返回给客户机应用程序，但您可以选择请求将其包含在对每个 `/message` 请求的响应中。

**重要信息：**上下文的一个用途是指定与助手进行交互的每个最终用户的唯一用户标识。对于基于用户的套餐，此标识用于计费。（有关更多信息，请参阅[基于用户的套餐](/docs/services/assistant?topic=assistant-services-information#user-based-plans)。）

有两种类型的上下文：

- **全局上下文**：由助手使用的所有技能共享的上下文变量，包括用于管理会话流的内部系统变量。

- **特定于技能的上下文**：特定于某种技能的上下文变量，包括应用程序所需的任何用户定义的变量。目前，仅支持一种技能（名为 `main skill`）。

## 示例

以下示例显示了包含全局上下文变量和特定于技能的上下文变量的 `/message` 请求；此示例还使用 `options.return_context` 属性来请求随响应返回上下文。

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

    // 使用用户标识创建全局上下文
    MessageContextGlobalSystem system = new MessageContextGlobalSystem();
    system.setUserId("my_user_id");
    MessageContextGlobal globalContext = new MessageContextGlobal();
    globalContext.setSystem(system);

    // 构建用户定义的上下文变量，并为 main skill 放入特定于技能的上下文
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

在此示例请求中，应用程序将 `user_id` 的值指定为全局上下文的一部分。此外，应用程序还将一个用户定义的上下文变量 (`account_number`) 设置为特定于技能的上下文的一部分。此上下文变量可由对话节点作为 `$account_number` 进行访问。（有关在对话中使用上下文的更多信息，请参阅[如何处理对话](/docs/services/assistant?topic=assistant-dialog-runtime)。）

可以指定要用于用户定义的上下文变量的任何变量名称。如果指定的变量已存在，那么将使用新值覆盖该变量；如果不存在，会将新变量添加到上下文中。

此请求的输出不仅包括通常的输出，还包括上下文，其中显示添加了指定值。

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "欢迎使用 Watson Assistant 示例！"
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

有关如何使用 API 访问上下文变量的详细信息，请参阅 [V2 API 参考 ![外部链接图标](../../icons/launch-glyph.svg "外部链接图标")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}。
