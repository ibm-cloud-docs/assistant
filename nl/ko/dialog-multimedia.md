---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-09"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# 멀티미디어 응답

[{{site.data.keyword.conversationshort}} 커넥터](conversation-connector.html)를 사용하여 Slack 또는 Facebook Messenger와 작업공간을 통합하려는 경우, 클릭 가능한 단추와 같은 멀티미디어 또는 대화식 요소를 포함하는 대화 상자 노드 응답을 지정할 수 있습니다. 대화식 응답을 지정하려면 대화 상자 노드의 출력에 JSON 데이터 블록을 삽입하십시오.

**참고:** Slack에서 대화식 메시지를 사용하려면, 대화식 메시지 지원을 사용하도록 설정했는지 확인하십시오. 자세한 정보는 Slack 배치 [README ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/watson-developer-cloud/conversation-connector/blob/master/channels/slack/README.md#interactive-messages){: new_window}를 참조하십시오. 

## 일반 JSON 형식

Slack 또는 Facebook Messenger 배치를 지원하는 일반 JSON 형식을 사용하여 대화식 응답을 지정할 수 있습니다. 일반 JSON 형식의 대화식 응답을 지정하려면, 대화 상자 노드 응답의 `output.generic` 필드에 JSON을 삽입하십시오. 다음 예제는 여러 응답 유형(텍스트, 이미지 및 클릭 가능한 옵션)을 포함하는 응답을 전송하는 방법을 보여줍니다.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "Here are your nearest stores."
      },
      {
        "response_type": "image",
        "source": "http://...",
        "title": "Image title",
        "description": "Some description for the image"
      },
      {
        "response_type": "option",
        "title": "Click on one of the following",
        "options": [
          {
            "label": "Location 1",
            "value:" "Location 1"
          },
          {
            "label": "Location 2",
            "value:" "Location 2"
          },
          {
            "label": "Location 3",
            "value:" "Location 3"
          }
        ]
      }
    ]
  }
}
```

지원되는 응답 유형 및 지정하는 방법에 대한 자세한 정보는 [응답 유형](#response-types)을 참조하십시오.

실행 중에, {{site.data.keyword.conversationshort}} 커넥터는 이 응답을 채널(Slack 또는 Facebook Messenger)에서 예상한 형식으로 변환합니다. 응답에 여러 미디어 유형 또는 첨부 파일이 포함된 경우 일반 응답은 일련의 개별 메시지 페이로드로 변환됩니다. 그런 다음 커넥터는 별도의 메시지로 채널에 각 메시지 페이로드를 전송합니다.

**참고:**응답이 여러 메시지로 분할되면, {{site.data.keyword.conversationshort}} 커넥터는 이러한 메시지를 순서대로 채널에 발송합니다. 이러한 메시지를 일반 사용자에게 전달하는 것은 채널의 책임입니다. 이는 네트워크 또는 서버 문제로 인해 영향을 받을 수 있습니다.

## 원시 JSON 형식

일반 JSON 형식외에도, {{site.data.keyword.conversationshort}} 커넥터는 또한 원시 Slack 및 Facebook Messenger 형식을 사용하여 작성된 채널별 응답을 지원합니다. 사용자가 현재 일반 JSON 형식에서 지원되지 않는 응답 유형을 지정해야 하는 경우, 원시 JSON 형식을 사용하도록 할 수 있습니다.

대화 상자 노드 응답의 적절한 필드를 사용하여 Slack 또는 Facebook에 대해 원시 JSON을 지정할 수 있습니다.

- `output.slack`: Slack 응답의 `attachment` 필드에 포함하려는 JSON을 삽입합니다. Slack JSON 형식에 관한 자세한 정보는 Slack [문서![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://api.slack.com/docs/message-attachments){: new_window}를 참조하십시오.

- `output.facebook`: Facebook 응답의 `message.attachment.payload` 필드에 포함하려는 JSON을 삽입합니다. Facebook JSON 형식에 관한 자세한 정보는 Facebook [문서![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: new_window}를 클릭하십시오.

## 응답 유형

다음 응답 유형은 일반 JSON 형식으로 지원됩니다.

### 이미지

URL로 지정된 이미지를 표시합니다.

#### 필드

| 이름          | 유형   | 설명                        | 필수? |
|---------------|--------|------------------------------------|-----------|
| response_type | enum   | `image`                            | Y         |
| source        | 문자열 | 이미지의 URL. 지정된 이미지는 .jpg, .gif 또는 .png 형식이어야 합니다. | Y |
| title         | 문자열 | 이미지 전에 표시하는 제목.| N         |
| description   | 문자열 | 이미지와 함께 제공되는 설명 텍스트. | N |

#### 예제

이 예제는 제목과 설명 텍스트가 있는 이미지를 표시합니다. 

```json
{
  "response_type": "image",
  "source": "http://example.com/image.jpg",
  "title": "Example image",
  "description": "An example image returned as part of a multimedia response."
}
```

### 옵션

사용자가 옵션을 선택하기 위해 클릭할 수 있는 단추 세트를 표시합니다. 지정한 값은 사용자 입력으로 작업공간에 전송됩니다.

#### 필드

| 이름          | 유형   | 설명                         | 필수? |
|---------------|--------|-------------------------------------|-----------|
| response_type | enum   | `option`                            | Y         |
| title         | 문자열 | 옵션 전에 표시하는 제목. | Y       |
| description   | 문자열 | 옵션과 함께 제공되는 설명 텍스트. | N |
| options       | list   | 사용자가 선택할 수 있는 옵션을 지정하는 키/값 쌍의 목록. | Y |
| options[].label | 문자열 | 옵션의 사용자 대상 레이블. | Y     |
| options[].value | 문자열<br/>오브젝트 | 사용자가 옵션을 선택할 경우, 이 값은 {{site.data.keyword.conversationshort}} 서비스로 발송됩니다. 이는 문자열(간단한 응답에 대한) 또는 다중 필드를 포함하는 오브젝트일 수 있습니다. 아래 예제를 참조하십시오. | Y |

#### 예제

이 예제에서는 두 개의 옵션이 표시됩니다.

- 옵션 1 ('Buy something'으로 레이블됨) 메시지의 `input.text` 필드를 사용하여 작업공간으로 보내는 간단한 문자열 메시지(`option_1`)를 발송합니다.
- 옵션 2 ('Exit'로 레이블됨) 입력 텍스트와 인텐트 배열을 모두 포함하는 복합 메시지를 보냅니다. 응답은 {{site.data.keyword.conversationshort}} 메시지의 올바른 파트인 모든 필드를 포함할 수 있습니다. 메시지 입력의 구조에 관한 자세한 정보는 [API 참조![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://www.ibm.com/watson/developercloud/conversation/api/v1/?curl#send_message){: new_window}를 참조하십시오.)

```json
{
  "response_type": "option",
  "title": "Choose from the following options:",
  "options": [
    {
      "label": "Buy something",
      "value": "Place order"
    },
    {
      "label": "Exit",
      "value": {
        "input": {
          "text": "Exit"
        },
        "intent": [
          {
            "intent": "exit_app",
            "confidence": 1.0
          }
        ]
      }
    }
  ]
}
```

### 텍스트

텍스트를 표시합니다.

#### 필드

| 이름          | 유형   | 설명        | 필수? |
|---------------|--------|--------------------|-----------|
| response_type | enum   | `text`             | Y         |
| text          | 문자열 | 표시할 텍스트. 채널에서 지원하는 경우, 줄 바꾸기 문자(`\n`) 및 마크 다운 태그 지정을 포함할 수 있습니다. (채널에서 지원하지 않는 형식은 무시됩니다.) | Y |

#### 예제

이 예제는 사용자에게 텍스트 메시지를 표시합니다.

```json
{
  "response_type": "text",
  "text": "This is a text response."
}
```
