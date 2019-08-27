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

# 存取環境定義資料
{: #api-client-get-context}

*環境定義* 是一個物件，包含在交談中持續保存的變數，並可供對話和用戶端應用程式共用。如果您的應用程式使用第 2 版 API，則助理會根據每個階段作業自動維護環境定義。對話及用戶端應用程式都可以讀取及寫入環境定義變數。依預設，環境定義不會傳回至用戶端應用程式，但您可以選擇性地要求將它包含在每個 `/message` 要求的回應中。

**重要事項：**環境定義的其中一個用途是針對與助理互動的每個一般使用者指定唯一的使用者 ID。若為使用者型方案，此 ID 用於計費目的。（如需相關資訊，請參閱[使用者型方案](/docs/services/assistant?topic=assistant-services-information#user-based-plans)。）

環境定義有以下兩種類型：

- **廣域環境定義**：助理使用的所有技能所共用的環境定義變數，其中包括用來管理交談流程的內部系統變數。

- **技能特有的環境定義**：特定技能特有的環境定義變數，包括應用程式所需的所有使用者定義變數。目前，只支援一種技能（名為 `main skill`）。

## 範例

下列範例顯示 `/message` 要求，其中同時包括廣域環境定義變數和技能特有的環境定義變數；它也會使用 `options.return_context` 內容，來要求傳回環境定義與回應。

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

在此範例要求中，應用程式會為 `user_id` 指定一個值，作為廣域環境定義。此外，它還會設定一個使用者定義的環境定義變數 (`account_number`)，作為技能特有的環境定義一部分。此環境定義變數可被對話節點當作 `$account_number` 存取。（如需在對話中使用環境定義的相關資訊，請參閱[對話處理方式](/docs/services/assistant?topic=assistant-dialog-runtime)。）

您可以指定您要用於使用者定義的環境定義變數的任何變數名稱。如果指定的變數已存在，則會以新值改寫該變數；如果沒有，則會在環境定義中新增一個變數。

此要求的輸出不僅包括一般輸出，也包括環境定義，顯示已新增指定的值。

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

如需如何使用 API 來存取環境定義變數的詳細資訊，請參閱[第 2 版 API 參考資料 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}。
