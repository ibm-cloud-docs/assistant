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

# 컨텍스트 데이터에 액세스
{: #api-client-get-context}

*컨텍스트*는 대화를 통해 지속되는 변수를 포함하는 오브젝트이며 대화와 클라이언트 애플리케이션에서 공유할 수 있습니다. 애플리케이션이 v2 API를 사용하는 경우에는 세션별로 어시스턴트에서 컨텍스트를 자동으로 관리합니다. 대화와 클라이언트 애플리케이션은 둘 다 컨텍스트 변수를 읽고 쓸 수 있습니다. 기본적으로 컨텍스트는 클라이언트 애플리케이션에 리턴되지 않지만, 선택적으로 각각의 `/message` 요청에 대한 응답에 포함되도록 요청할 수 있습니다.

**중요:** 어시스턴트 사용은 어시스턴트와 상호작용하는 각각의 최종 사용자에 대해 고유한 사용자 ID를 지정하는 것입니다. 사용자 기반 플랜의 경우 이 ID는 청구용으로 사용됩니다. (자세한 정보는 [사용자 기반 플랜](/docs/services/assistant?topic=assistant-services-information#user-based-plans)을 참조하십시오.)

다음과 같은 두 가지 유형의 컨텍스트가 있습니다.

- **글로벌 컨텍스트**: 대화 플로우를 관리하는 데 사용되는 내부 시스템 변수를 포함하여, 어시스턴트에서 사용하는 모든 스킬이 공유하는 컨텍스트 변수입니다.

- **스킬 특정 컨텍스트**: 애플리케이션에서 필요로 하는 사용자 정의 변수를 포함하여 특정 스킬에 고유한 컨텍스트 변수입니다. 현재, 하나의 스킬(이름: `main skill`)만 지원됩니다.

## 예제

다음 예제는 글로벌 및 스킬 특정 컨텍스트 변수를 포함하는 `/message` 요청을 표시합니다. 또한 이 요청은 컨텍스트가 응답과 함께 리턴되도록 요청하기 위해 `options.return_context` 속성을 사용합니다 `

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

이 예제 요청에서 애플리케이션은 글로벌 컨텍스트의 일부로 `user_id`에 대한 값을 지정합니다. 또한, 스킬 특정 컨텍스트의 일부로 하나의 사용자 정의 컨텍스트 변수(`account_number`)를 설정합니다. 이 컨텍스트 변수는
대화 노드에서 `$account_number`로 액세스할 수 있습니다. (대화에서 컨텍스트를 사용하는 방법에 대한 자세한 정보는
[대화 처리 방법](/docs/services/assistant?topic=assistant-dialog-runtime)을 참조하십시오.)

사용자 정의 컨텍스트 변수에 사용할 변수 이름을 지정할 수 있습니다. 지정된 변수가 이미 존재하는 경우 새 값이 겹쳐씁니다. 그렇지 않은 경우, 새 변수가 컨텍스트에 추가됩니다.

이 요청의 출력에는 일반적인 출력뿐만 아니라 지정된 값이 추가되었음을 표시하는 컨텍스트도 포함됩니다.

```json
{
  "output": {
    "generic":[
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

API를 사용하여 컨텍스트 변수에 액세스하는 방법에 대한 자세한 정보는 [v2 API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}를 참조하십시오.
