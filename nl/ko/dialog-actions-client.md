---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-01"

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

# 클라이언트 애플리케이션 호출 ![베타](images/beta.png)
{: #dialog-actions-client}

클라이언트 측 프로세스를 실행할 수 있도록 대화를 일시정지하는 대화 노드에 클라이언트 조치를 추가하고 대화 턴(turn) 내에서 발생하는 처리의 일부로 결과를 리턴하십시오.
{: shortdesc}

클라이언트 조치는 외부 클라이언트 애플리케이션이 이해할 수 있는 표준화된 형식으로 프로그래밍 방식의 호출을 정의합니다. 외부 클라이언트 애플리케이션은 제공된 정보를 사용하여 프로그래밍 방식의 호출 또는 함수를 실행한 후 결과를 대화로 리턴해야 합니다.

이 조치는 직접 호출하지 않습니다. 기본적으로 대화가 여기에서 일시정지하고 외부 클라이언트 애플리케이션이 작업을 수행하는 것을 대기하도록 지시합니다. 클라이언트 애플리케이션이 실행하는 프로그램은 사용자가 선택하는 것일 수 있습니다. 이 주제의 후반부에서 설명하는 JSON 형식 규칙에 따라 호출 이름과 매개변수 세부사항 및 오류 메시지 변수 이름을 지정해야 합니다.

외부 애플리케이션을 호출하여 다음 유형의 작업을 수행할 수 있습니다.

- 사용자로부터 수집한 정보를 유효성 검증합니다.
- 지원되는 SpEL 표현식 메소드를 처리하기에 너무 복잡한 사용자 입력에 대해 계산 또는 문자열 조작을 수행합니다.
- 다른 애플리케이션 또는 서비스에서 데이터를 가져오십시오.

외부 서비스(예: {{site.data.keyword.openwhisk_short}} 웹 액션)를 호출하는 방법에 대한 자세한 정보는 [대화 노드에서 프로그래밍 방식 호출 작성](/docs/services/assistant?topic=assistant-dialog-webhooks)을 참조하십시오.

다음 다이어그램에서는 클라이언트 호출을 사용하여 일기 예보 정보를 가져오고 이를 사용자에게 리턴하는 방법을 보여줍니다.

![일기 예보를 요청하는 사람, 클라이언트 앱에 요청을 보내는 대화, 클라이언트 앱이 외부 서비스에 요청을 전송하는 것을 표시](images/forecast.png)

클라이언트 조치는 클라이언트 애플리케이션이 실행하는 프로그램인 `MyWeatherFunction` 프로그램을 호출합니다. 클라이언트 애플리케이션은 외부 웹 서비스(`/weather`)에 대한 호출을 작성하여 실제 예측 정보를 가져옵니다. 그러면 클라이언트 애플리케이션이 대화에 응답을 리턴합니다. 클라이언트 조치를 대화에 추가하는 경우, 처리 자체를 수행하거나 대화와 사용하려는 외부 백엔드 서비스 사이에 정보를 전달하는 클라이언트 애플리케이션이 있어야 합니다. 

## 프로시저
{: #dialog-actions-client-call}

대화 노드에서 클라이언트 애플리케이션을 프로그래밍 방식으로 호출하려면 다음 단계를 완료하십시오.

1.  프로그래밍 방식 호출을 작성하려는 대화 노드에서 JSON 편집기를 여십시오.

    - 노드에 대한 응답이 평가된 후에 실행하는 프로그래밍 방식 호출을 작성하려면 노드 응답에 대한 JSON 편집기를 여십시오.

      ![표준 노드 응답과 관련된 JSON 편집기에 액세스하는 방법 표시.](images/contextvar-json-response.png)

      노드에 대해 **다중 응답** 설정이 **켜져**있는 경우, **옵션**![고급 응답](images/kabob.png) 메뉴가 표시되도록 하려면 **응답 편집**![응답 편집](images/edit-slot.png) 아이콘을 클릭하십시오.

      ![활성화된 다중 조건부 응답이 있는 표준 노드와 연관된 JSON 편집기에 액세스하는 방법을 표시합니다.](images/contextvar-json-multi-response.png)

    동일한 대화 턴(turn) 내에서 응답을 표시하거나 추가로 처리하려면 이 액션을 수행하는 두 번째 노드를 추가하고 이 노드에서 점프하십시오.
    {: tip}

    - 개별 슬롯에서 사용할 수 있는 호출을 작성하려면 슬롯에 대한 **슬롯 편집** 아이콘 ![슬롯 편집 아이콘](images/edit-slot.png)을 클릭하고 다음 중 하나를 수행하십시오.

      - 슬롯 조건이 true로 평가된 후에 실행하는 프로그래밍 방식 호출을 작성하려면 슬롯 조건과 관련된 JSON 편집기를 여십시오.

        ![슬롯 조건과 관련된 JSON 편집기에 액세스하는 방법 표시.](images/contextvar-json-slot-condition.png)

      - 슬롯이 채워진 후 실행하는 프로그래밍 방식의 호출을 수행하려면 찾음 응답과 관련된 JSON 편집기를 여십시오. 이를 수행하려면, 슬롯에 대한 **옵션** ![옵션 아이콘](images/kabob.png) 메뉴에서, **조건부 응답 사용**을 클릭하십시오. 찾음 응답의 경우, **응답 편집** ![응답 편집](images/edit-slot.png) 아이콘을 클릭하십시오. 찾음 응답에 대한 **옵션** ![옵션 아이콘](images/kabob.png) 메뉴에서 **JSON 편집기 열기**를 클릭하십시오.

        ![슬롯에 대한 조건부 응답과 관련된 JSON 편집기에 액세스하는 방법 표시.](images/contextvar-json-slot-multi-response.png)

1.  프로그래밍 방식 호출을 정의하려면 다음 구문을 사용하십시오.

    ```json
    {
      "context": {
        "variable_name" : "variable_value"
      },
      "actions": [
        {
          "name":"<actionName>",
          "type":"client",
          "parameters": {
            "<parameter_name>":"<parameter_value>",
            "<parameter_name>":"<parameter_value>"
          },
          "result_variable": "<result_variable_name>"
        }
      ],
      "output": {
        "text": "response text"
      }
    }
    ```
    {: codeblock}

    `actions` 배열은 대화에서 작성할 프로그래밍 방식 호출을 지정합니다. 이것은 프로그래밍 방식 호출을 5개까지 정의할 수 있습니다. JSON 배열에서 다음과 같은 이름 및 값 쌍을 지정하십시오.

    - `<actionName>`: 필수. 호출할 액션 및 서비스의 이름입니다. 원하는 구문으로 이름을 지정하십시오. 목표는 클라이언트 애플리케이션을 인식하고 처리 방법을 알 수 있는 이름을 지정하는 것입니다. 이름은 최대 256자를 초과할 수 없습니다.

      예: `calculateRate`

    - `<type>`: 작성할 호출 유형을 나타냅니다. 선택적으로, 기본값인 `client`를 지정하십시오.

      외부 클라이언트 애플리케이션이 판별하는 표준화된 형식으로 프로그래밍 방식의 호출 정보를 포함한 메시지 응답을 보냅니다. 클라이언트 애플리케이션은 제공된 정보를 사용하여 프로그래밍 방식의 호출 또는 함수를 실행하고 결과를 대화로 리턴해야 합니다. 응답 본문에서 JSON 오브젝트를 호출할 서비스 또는 함수, 호출과 함께 전달할 관련 매개 변수 및 다시 보낼 결과의 형식을 지정합니다.

    - `<action_parameters>`: 외부 프로그램에서 예상하는 모든 매개변수는 JSON 오브젝트로 지정됩니다. 매개변수가 외부 프로그램에서 요구하는 경우에만 필요합니다.

    - `<result_variable_name>`: 외부 서비스 또는 프로그램에서 리턴되는 JSON 오브젝트를 참조하기 위해 사용하는 이름입니다. 결과는 `/message` 응답의 컨텍스트 섹션에 추가됩니다. 즉, 결과는 컨텍스트 변수로 저장되므로 노드 응답에 표시되거나 나중에 트리거되는 대화 노드가 액세스할 수 있습니다. 컨텍스트 변수의 모든 기존 값이 액션에서 리턴하는 값으로 대체됩니다. 다음 구문을 사용하여 `result_variable_name`을 지정할 수 있습니다.

      - `my_result`
      - `$my_result`

      이름은 최대 64자를 초과할 수 없습니다. 변수 이름은 다음 문자를 포함할 수 없습니다. 소괄호 `()`, 대괄호 (`[]`), 작은 따옴표(`'`), 인용 부호(`"`) 또는 백슬래시(`\`).

      `/message` 응답의 출력 또는 입력 섹션에 결과를 저장하는 경우, 다음 위치 키워드 중 하나를 `result_variable_name` 앞에 접두부로 추가할 수 있습니다.

       - `output.`: /message 응답의 출력 섹션에 결과를 추가합니다. 예: `output.my_result`.
       - `input.`: /message 응답의 입력 섹션에 결과를 추가합니다. 예: `input.my_result`.

      `context.`를 지정할 수 있습니다. 위치 키워드 접두부는 다음과 같습니다. 예: `context.my_result`. 그러나, 결과가 기본적으로 컨텍스트에 추가되므로 수행할 필요는 없습니다.

      변수 이름에 마침표를 포함하여 중첩된 JSON 오브젝트를 작성할 수 있습니다. 예를 들어, 오늘과 내일의 날씨 서비스 예측에 대한 두 개의 개별 요청에서 결과를 캡처하기 위해 다음 변수를 정의할 수 있습니다.

      - `context.weather.today`
      - `context.weather.tomorrow`

      결과(`temp` 및 `rain` 매개변수 값)는 다음 구조의 컨텍스트로 저장됩니다.

      ```json
      {
        "weather": {
          "today": {
            "temp": "20",
            "rain": "30"
          },
          "tomorrow": {
            "temp": "23",
            "rain": "80"
          }
        }
      }
      ```
      {: codeblock}

      단일 JSON 액션 배열의 다중 액션이 동일한 컨텍스트 변수에 프로그래밍 방식 호출의 결과를 추가하는 경우 컨텍스트가 업데이트되는 순서가 중요합니다. 액션 유형별로 배열에 액션이 정의된 순서에 따라 컨텍스트 변수의 값이 설정되는 순서가 결정됩니다. 배열의 마지막 액션으로 리턴한 컨텍스트 변수 값은 다른 액션으로 계산한 값을 겹쳐 씁니다.

## 클라이언트 호출 예제
{: #dialog-actions-client-example}

다음 예제는 외부 날씨 서비스에 대한 호출을 보여줍니다. 노드 응답과 연관된 JSON 편집기에 추가됩니다. 노드 레벨 응답이 트리거될 때까지 슬롯은 사용자로부터 날짜 및 위치 정보를 수집하여 저장합니다. 이 예제에서는 호출할 프로그램의 이름을 `MyWeatherFunction`이라고 가정하고, `location` 및 `date` 매개변수를 사용하고 JSON 오브젝트(`{"forecast": "<value>"}`)를 리턴합니다.

 ``` json
{
  "actions": [
    {
      "name": "MyWeatherFunction",
      "type": "client",
      "parameters": {
        "date": "$date",
        "location": "$location"
      },
      "result_variable": "context.my_forecast"
    }
  ]
}
```
{: codeblock}

일반적으로, 이 서비스는 새로운 사용자 입력이 필요한 경우(예:상위 노드를 실행한 후 해당 하위 노드 중 하나를 실행하기 전에)에만 POST `/message` 요청을 통해 클라이언트에게 리턴됩니다. 그러나, 노드에 클라이언트 액션을 추가한 경우, 평가 후에 서비스는 항상 클라이언트로 리턴되어 액션 호출의 결과가 리턴될 수 있습니다. 하위 노드로 직접 점프하도록 구성된 노드와 같이 사용자 입력을 대기하지 않도록 하기 위해 서비스는 다음 값을 메시지 컨텍스트에 추가합니다.

```json
  {
    "context": {
      "skip_user_input": true
    }
  }
```
{: codeblock}

클라이언트가 사용자 입력을 받지 않고 액션 수행을 원하는 경우, 동일한 규칙에 따라 상위 노드에 'skip_user_input` 컨텍스트 변수를 추가하여 클라이언트 애플리케이션과 통신할 수 있습니다.

클라이언트 애플리케이션은 컨텍스트에서 항상 `skip_user_input` 변수를 검사해야 합니다. 존재하는 경우, 사용자로부터 새로운 입력을 요청하지 않고, 그 대신에 액션을 실행하고, 결과를 메시지에 추가한 다음, 서비스로 다시 전달합니다. 새 POST 메시지 요청에는 이전 POST 메시지 응답(즉, 컨텍스트, 입력, 인텐트, 엔티티 및 선택적으로 출력 섹션)으로 리턴한 메시지가 포함되어야 하며 프로그래밍 방식의 호출을 정의하는 JSON 오브젝트 대신 프로그래밍 방식 호출에서 리턴된 결과를 포함해야합니다.

이 노드 후에 점프하는 하위 노드에서, 사용자에게 표시할 응답을 추가하십시오.

``` json
{
  "output": {
    "text": {
      "values": [
        "It will be $my_forecast $date.literal in $location.literal."
      ]
    }
  }
```
{: codeblock}
