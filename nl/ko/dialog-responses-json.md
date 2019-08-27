---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-11"

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

# JSON 편집기를 사용하여 응답 정의
{: #dialog-responses-json}

경우에 따라 JSON 편집기를 사용하여 응답을 정의해야 할 수도 있습니다. (대화 응답에 대한 자세한 정보는 [응답](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)을 참조하십시오.) 응답 JSON을 편집하면 통신 채널 또는 사용자 정의 애플리케이션에 리턴될 데이터에 대한 직접 액세스가 부여됩니다.

## 일반 JSON 형식
{: #dialog-responses-json-generic}

응답에 대한 일반 JSON 형식은 모든 채널을 대상으로 하는 응답을 지정하는 데 사용됩니다. 이 형식은 Slack 및 Facebook 통합에서 지원되는 다양한 응답 유형을 포함할 수 있으며 사용자 정의 클라이언트 애플리케이션에서 구현될 수도 있습니다. (이는 {{site.data.keyword.conversationshort}} 도구를 사용하여 정의된 대화 응답에 대해 기본적으로 사용되는 형식입니다.)

도구에서 대화 노드 응답에 대한 JSON 편집기를 여는 방법에 대한 정보는 [JSON 편집기의 컨텍스트 변수](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-var-json)를 참조하십시오.

일반 JSON 형식의 대화식 응답을 지정하려면 대화 노드 응답의 `output.generic` 필드에 적절한 JSON 오브젝트를 삽입하십시오. 다음 예제는 여러 응답 유형(텍스트, 이미지 및 클릭 가능한 옵션)을 포함하는 응답을 전송하는 방법을 보여줍니다.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "values": [
          {
            "text": "Here are your nearest stores."
          }
        ]
      },
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Example image",
        "description": "Some description for the image."
      },
      {
        "response_type": "option",
        "title": "Click on one of the following",
        "options": [
          {
            "label": "Location 1",
            "value": {
              "input": {
                "text": "Location 1"
              }
            }
          },
          {
            "label": "Location 2",
            "value": {
              "input": {
                "text": "Location 2"
              }
            }
          },
          {
            "label": "Location 3",
            "value": {
              "input": {
                "text": "Location 3"
              }
            }
          }
        ]
      }
    ]
  }
}
```

JSON 오브젝트를 사용하여 지원되는 각 응답 유형을 지정하는 방법에 대한 자세한 정보는 [응답 유형](#dialog-responses-json-response-types)을 참조하십시오.

{{site.data.keyword.conversationshort}} 커넥터를 사용 중인 경우 런타임 시 응답이 채널(Slack 또는 Facebook Messenger)에서 예상되는 형식으로 변환됩니다. 응답에 여러 미디어 유형 또는 첨부 파일이 포함된 경우 필요에 따라 일반 응답이 일련의 개별 메시지 페이로드로 변환됩니다. 그런 다음 커넥터는 별도의 메시지로 채널에 각 메시지 페이로드를 전송합니다.

**참고:**응답이 여러 메시지로 분할되면, {{site.data.keyword.conversationshort}} 커넥터는 이러한 메시지를 순서대로 채널에 발송합니다. 이러한 메시지를 일반 사용자에게 전달하는 것은 채널의 책임입니다. 이는 네트워크 또는 서버 문제로 인해 영향을 받을 수 있습니다.

고유 클라이언트 애플리케이션을 빌드하는 경우 앱이 각 응답 유형을 적절히 구현해야 합니다. 자세한 정보는 [응답 구현](/docs/services/assistant?topic=assistant-api-dialog-responses)을 참조하십시오.

## 원시 JSON 형식
{: #dialog-responses-json-native}

대화 노드 JSON은 일반 JSON 형식 이외에 네이티브 Slack 및 Facebook Messenger 형식을 사용하여 작성된 채널별 응답도 지원합니다. 이러한 형식은 {{site.data.keyword.conversationshort}} 형식화에서도 지원됩니다. 작업공간이 하나의 채널 유형과만 통합됨을 알고 있고 현재 일반 JSON 형식에서 지원되지 않는 응답 유형을 지정해야 하는 경우 네이티브 JSON 형식을 사용할 수 있습니다.

대화 노드 응답의 적절한 필드를 사용하여 Slack 또는 Facebook에 대해 원시 JSON을 지정할 수 있습니다.

- `output.slack`: Slack 응답의 `attachment` 필드에 포함될 JSON을 삽입합니다. Slack JSON 형식에 관한 자세한 정보는 Slack [문서![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://api.slack.com/docs/message-attachments){: new_window}를 참조하십시오.

- `output.facebook`: Facebook 응답의 `message.attachment.payload` 필드에 포함하려는 JSON을 삽입합니다. Facebook JSON 형식에 관한 자세한 정보는 Facebook [문서![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: new_window}를 클릭하십시오.

## 응답 유형
{: #dialog-responses-json-response-types}

다음 응답 유형은 일반 JSON 형식으로 지원됩니다.

### 이미지
{: #dialog-responses-json-image}

URL로 지정된 이미지를 표시합니다.

#### 필드
{: #{: #dialog-responses-json-image-fields}

| 이름          | 유형   | 설명                        | 필수? |
|---------------|--------|------------------------------------|-----------|
| response_type | enum   | `image`                            | Y         |
| source        | 문자열 | 이미지의 URL. 지정된 이미지는 .jpg, .gif 또는 .png 형식이어야 합니다. | Y |
| title         | 문자열 | 이미지 전에 표시하는 제목.| N         |
| description   | 문자열 | 이미지와 함께 제공되는 설명 텍스트. | N |

#### 예제
{: #dialog-responses-json-image-example}

이 예제는 제목과 설명 텍스트가 있는 이미지를 표시합니다.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Example image",
        "description": "An example image returned as part of a multimedia response."
      }
    ]
  }
}
```

### 옵션
{: #dialog-responses-json-option}

사용자가 옵션을 선택하는 데 사용할 수 있는 단추 또는 드롭 다운 목록 세트를 표시합니다. 지정한 값은 사용자 입력으로 작업공간에 전송됩니다.

#### 필드
{: #dialog-responses-json-option-fields}

| 이름          | 유형   | 설명                         | 필수? |
|---------------|--------|-------------------------------------|-----------|
| response_type | enum   | `option`                            | Y         |
| title         | 문자열 | 옵션 전에 표시하는 제목. |Y       |
| description   | 문자열 | 옵션과 함께 제공되는 설명 텍스트. | N |
| preference    | enum   | 채널에서 지원되는 경우 표시할 기본 제어 유형(`dropdown` 또는 `button`). {{site.data.keyword.conversationshort}}커넥터는 현재 `button`만 지원합니다.| N |
| options       | list   | 사용자가 선택할 수 있는 옵션을 지정하는 키/값 쌍의 목록. | Y |
| options[].label | 문자열 | 옵션의 사용자 대상 레이블. | Y     |
| options[].value | 오브젝트 | 사용자가 옵션을 선택하는 경우 {{site.data.keyword.conversationshort}} 서비스로 전송될 응답을 정의하는 오브젝트. | Y |
| options[].value.input | 오브젝트 | 옵션에 해당하는 입력 텍스트를 포함하는 입력 오브젝트. | N |
| options[].value.input.text | 문자열 | 옵션에 대한 어시스턴트로 전송될 텍스트. | N |

#### 예제
{: #dialog-responses-json-option-example}

이 예제에서는 두 개의 옵션이 표시됩니다.

- 옵션 1(`Buy something`으로 레이블 지정됨)은 작업공간에 입력 텍스트로 전송되는 단순 문자열 메시지(`Place order`)를 전송합니다.
- 옵션 2(`Exit`로 레이블 지정됨)는 입력 텍스트와 인텐트 배열을 모두 포함하는 복합 메시지를 전송합니다. 응답은 {{site.data.keyword.conversationshort}} 메시지의 올바른 파트인 모든 필드를 포함할 수 있습니다. 메시지 입력의 구조에 관한 자세한 정보는 [API 참조![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}를 참조하십시오.)

```json
{
  "output": {
    "generic":[
      {
        "response_type": "option",
        "title": "Choose from the following options:",
        "preference": "button",
        "options": [
          {
            "label": "Buy something",
            "value": {
              "input": {
                "text": "Place order"
              }
            }
          },
          {
            "label": "Exit",
      "value": {
              "input": {
                "text": "Exit"
              },
              "intents": [
                {
                  "intent": "exit_app",
            "confidence": 1.0
          }
              ]
            }
          }
        ]
      }
    ]
  }
}
```

### 일시정지
{: #dialog-responses-json-pause}

채널에 다음 메시지를 전송하기 전에 일시정지하고 선택적으로 "사용자 입력 주예 이벤트를 전송합니다(이 이벤트를 지원하는 채널의 경우).

#### 필드
{: #dialog-responses-json-pause-fields}

| 이름          | 유형   | 설명        | 필수? |
|---------------|--------|--------------------|-----------|
| response_type | enum   | `pause`            | Y         |
| time          | 정수    | 일시정지할 기간(밀리초). | Y |
| typing        | 부울 | 일시정지 중에 "사용자 입력 중" 이벤트를 전송할지 여부. 채널이 이 이벤트를 지원하지 않는 경우 무시됩니다. | N |

#### 예제
{: #dialog-responses-json-pause-example}

이 예제는 5초 동안 일시정지하는 중에 "사용자 입력 중" 이벤트를 전송합니다.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "pause",
        "time": 5000,
        "typing": true
      }
    ]
  }
}
```

### 텍스트
{: #dialog-responses-json-text}

텍스트를 표시합니다. 다양성을 추가하기 위해 여러 대체 텍스트 응답을 지정할 수 있습니다. 여러 응답을 지정하는 경우 목록을 순차적으로 순환하거나, 랜덤으로 응답을 선택하거나, 지정된 모든 응답을 출력할 수 있습니다.

#### 필드
{: #dialog-responses-json-text-fields}

| 이름          | 유형   | 설명        | 필수? |
|---------------|--------|--------------------|-----------|
| response_type | enum   | `text`             | Y         |
| values        | list   | 텍스트 응답을 정의하는 하나 이상의 오브젝트 목록. | Y |
| values.[_n_].text   | 문자열 | 응답의 텍스트. 채널에서 지원하는 경우, 줄 바꾸기 문자(`\n`) 및 마크 다운 태그 지정을 포함할 수 있습니다. (채널에서 지원하지 않는 형식은 무시됩니다.) | N |
| selection_policy | 문자열 | 둘 이상의 응답이 지정된 경우 목록에서 응답을 선택하는 방법. 가능한 값은 `sequential`, `random` 및 `multiline`입니다. | N |
| delimiter     | 문자열 | 응답 사이에 구분자로 출력할 구분 기호. `selection_policy`=`multiline`인 경우에만 사용됩니다. 기본 구분 기호는 줄 바꾸기(`\n`)입니다. | N |

#### 예제
{: #dialog-responses-json-text-example}

이 예제는 사용자에게 인사 메시지를 표시합니다.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "values": [
          { "text": "Hello." },
          { "text": "Hi there." }
        ],
        "selection_policy": "random"
      }
    ]
  }
}  
```
