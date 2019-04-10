---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

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

# 응답 구현
{: #dialog-api-responses}

대화 노드는 텍스트, 이미지 또는 대화식 요소(예: 클릭 가능한 옵션)를 포함하는 응답으로 사용자에게 응답할 수 있습니다. 고유 클라이언트 애플리케이션을 빌드하는 경우 대화에서 리턴된 모든 응답 유형의 올바른 표시를 구현해야 합니다. (대화 응답에 대한 자세한 정보는 [응답](/docs/services/assistant?topic=assistant-dialog-overview#responses)을 참조하십시오.)

## 응답 출력 형식
{: #dialog-api-responses-output}

기본적으로 대화 노드의 응답은 /message API에서 리턴된 응답 JSON의 `output.generic` 오브젝트에 지정됩니다. `generic` 오브젝트에는 모든 채널에 사용할 수 있는 최대 5개의 응답 요소로 이루어진 배열이 포함되어 있습니다. 다음 JSON 예제는 텍스트 및 이미지를 포함하는 응답을 보여줍니다.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "OK, here's a picture of a dog."
      },
      {
        "response_type": "image",
        "source": "http://example.com/dog.jpg"
      }
    ],
    "text" : ["OK, here's a picture of a dog."]
  }
}
```

**참고:** 이 예제에 표시된 대로 텍스트 응답(`OK, here's a picture of a dog.`)이 `output.text` 배열에도 리턴됩니다. 이는 `output.generic` 형식을 지원하지 않는 애플리케이션의 이전 버전과의 호환성을 위해 포함됩니다.

모든 응답 유형을 적절히 처리하는 것은 클라이언트 애플리케이션의 책임입니다. 이 경우 애플리케이션이 사용자에게 지정된 텍스트 및 이미지를 표시해야 할 수 있습니다.

## 응답 유형
{: #dialog-api-responses-types}

응답의 각 요소는 지원되는 응답 유형(현재 `image`, `option`, `pause` 및 `text`) 중 하나입니다. 각 응답 유형은 다른 JSON 특성 세트를 사용하여 지정되므로 각 응답에 대해 포함된 특성은 응답 유형에 따라 다릅니다. /message API의 응답 모델에 대한 전체 정보는 [API 참조 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}를 참조하십시오.

이 섹션에서는 사용 가능한 응답 유형과 이러한 응답 유형이 /message API 응답 JSON에 표시되는 방법에 대해 설명합니다. (Watson SDK를 사용 중인 경우 사용자 언어에 제공된 인터페이스를 사용하여 동일한 오브젝트에 액세스할 수 있습니다.)

**참고:** 이 섹션의 예제는 런타임 시 /message API에서 리턴된 JSON 데이터의 형식을 보여줍니다. 이 형식은 대화 노드 내에서 응답을 정의하는 데 사용되는 JSON 형식과 다르다는 점을 기억하십시오. 자세한 정보는 [JSON 편집기를 사용하여 응답 정의](/docs/services/assistant?topic=assistant-dialog-responses-json)를 참조하십시오.

### 이미지
{: #dialog-api-responses-image}

`image` 응답 유형은 선택적으로 제목과 설명이 수반된 이미지를 표시하도록 클라이언트 애플리케이션에 지시합니다.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Image example",
        "description": "This is an example image"
      }
    ]
  }
}
```

애플리케이션은 `source` 특성에 지정된 이미지를 검색하여 사용자에게 표시하는 작업을 담당합니다. 선택적 `title` 및 `description`이 제공되는 경우 애플리케이션이 이러한 특성을 적절한 방식으로 표시할 수 있습니다(예: 이미지 아래에 제목을 렌더링하고 설명을 풍선 텍스트로 렌더링). 

### 옵션
{: #dialog-api-responses-option}

`option` 응답 유형은 사용자가 옵션 목록에서 선택할 수 있도록 하는 사용자 인터페이스 제어를 표시하도록 클라이언트 애플리케이션에 지시한 후 선택한 옵션에 따라 {{site.data.keyword.conversationshort}} 서비스에 입력을 반송합니다.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "option",
        "title": "Available options",
        "description": "Please select one of the following options:",
        "preference": "button",
        "options": [
          {
            "label": "Option 1",
            "value": {
              "input": {
                "text": "option 1"
              }
            }
          },
          {
            "label": "Option 2",
            "value": {
              "input": {
                "text": "option 2"
              }
            }
          }
        ]
      }
    ]
  }
}
```

앱은 적합한 사용자 인터페이스 제어(예: 단추 세트 또는 드롭 다운 목록)를 사용하여 지정된 옵션을 표시할 수 있습니다. 선택적 `preference` 특성은 지원되는 경우 앱에서 사용해야 하는 기본 제어 유형(`button` 또는 `dropdown`)을 표시합니다.

각 옵션에서 `label` 특성은 UI 제어의 옵션에 대해 표시되어야 하는 레이블 텍스트를 지정합니다. `value` 특성은 사용자가 해당 옵션을 선택할 때 /message API를 사용하여 {{site.data.keyword.conversationshort}} 서비스에 반송되어야 하는 입력을 지정합니다. `value` 특성에는 텍스트 입력뿐만 아니라 인텐트 및 엔티티와 같은 다른 입력 오브젝트도 포함될 수 있으며, 모두 서비스에 전송되어야 합니다.

### 일시정지
{: #dialog-api-responses-pause}

`pause` 응답 유형은 다음 응답을 표시하기 전에 지정된 간격 동안 대기하도록 애플리케이션에 지시합니다.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "pause",
        "time": 500,
        "typing": false
      }
    ]
  }
}
```

이 일시중지는 요청이 완료될 때까지 기다리거나 단순히 응답 사이에 일시중지할 수 있는 휴먼 에이전트의 모양을 가장하기 위해 대화에서 요청될 수 있습니다. 일시중지 기간은 최대 10초까지 가능합니다.

`pause` 응답은 일반적으로 다른 응답과 결합하여 전송됩니다. 애플리케이션이 배열에 있는 다음 응답을 표시하기 전에 `time` 특성(밀리초 단위)에 지정된 간격 동안 일시정지해야 합니다. 선택적 `typing` 특성은 클라이언트 애플리케이션이 휴먼 에이전트를 시뮬레이션하기 위해 "사용자가 입력 중" 표시기를 표시하도록 요청합니다(지원되는 경우).

### 텍스트
{: #dialog-api-responses-text}

`text` 응답 유형은 대화의 일반 텍스트 응답에 사용됩니다.

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "OK, you want to fly to Boston next Monday."
      }
    ]
  }
}
```

이전 버전과의 호환성을 위해 동일한 텍스트가 대화 응답의 `output.text` 배열(이 페이지의 시작 부분에 있는 예제에 표시됨)에도 포함됩니다.
