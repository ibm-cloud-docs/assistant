---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-15"

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

# 대화 상자 노드에서 프로그래밍 방식 호출 작성![베타](images/beta.png)
{: #dialog-actions}

외부 애플리케이션 또는 서비스를 프로그래밍 방식으로 호출하고 대화 상자 턴 내에서 발생하는 처리의 일부로 결과를 되돌릴 수 있는 동작을 정의합니다.
{: shortdesc}

외부 서비스를 사용하여 사용자가 수집한 정보의 유효성을 검증하거나 지원되는 SpEL 표현식 및 메소드를 사용하여 처리하기에 너무 복잡한 입력에 대한 계산 또는 문자열 조작을 수행할 수 있습니다. 또는 외부 웹 서비스와 상호작용하여 항공편의 예상 도착 시간을 확인하는 항공 교통 서비스 또는 날씨 예측 서비스와 같은 정보를 얻을 수 있습니다. 식당 예약 사이트와 같은 외부 애플리케이션과 상호작용하여 사용자 대신 간단한 트랜잭션을 완료할 수도 있습니다. 

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/y0A6X-KNoB8?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

프로그래밍 방식 호출을 정의할 때, 다음 유형 중 하나를 선택하십시오.

- **클라이언트**: 외부 클라이언트 애플리케이션이 프로그래밍 방식의 호출 또는 함수를 수행하고 결과를 대화 상자에 리턴하는 데 사용할 수 있는 표준화된 형식의 프로그래밍 방식 호출을 정의합니다.
- **서버**: {{site.data.keyword.openwhisk_short}} 조치를 직접 호출하고 대화 상자에 결과를 리턴합니다.

    현재는 미국 남부나 독일 지역에서 호스팅되는 {{site.data.keyword.conversationshort}} 인스턴스에서 {{site.data.keyword.openwhisk_short}} 조치를 호출할 수 있습니다.

    **참고**: 사용되는 {{site.data.keyword.openwhisk_short}} 인스턴스는 동일한 위치(미국 남부 또는 독일)에서 호스팅되는 인스턴스입니다. 따라서 예를 들어, 미국 남부에서 호스팅되는 {{site.data.keyword.conversationshort}} 서비스 인스턴스에서 액세스하려는 경우 독일에서 호스팅되는 {{site.data.keyword.openwhisk_short}} 인스턴스에 조치를 정의하지 마십시오. 

    **중요**: 이 메소드를 사용하여 **5 초 이내**에 리턴할 수 있는 {{site.data.keyword.openwhisk_short}} 조치를 호출하십시오. {{site.data.keyword.openwhisk_short}}에 대한 요청은 개별 서비스 호출에 걸리는 시간이 오래 걸리는 경우, 시간 초과됩니다. 대화 상자에서 두 번 이상 외부 서비스를 호출하는 경우 호출이 완료될 수 있는 총 시간은 7 초입니다. 처음 3 번의 호출이 각각 2 초씩 완료되고 네 번째 호출이 1 초 이상 걸리면 네 번째 호출이 중지되고 호출에 대한 오류 메시지는 호출이 완료되지 않았음을 나타냅니다. 호출이 필요한 효율성이 떨어지는 서비스의 경우 클라이언트 애플리케이션을 통해 호출을 관리하고 정보를 대화 상자에 별도의 단계로 전달합니다. 

## 프로시저
{: #call-action}

대화 상자 노드에서 프로그래밍 방식 호출을 작성하려면, 다음 단계를 완료하십시오.

1.  프로그래밍 방식 호출을 작성하려는 대화 상자 노드에서 JSON 편집기를 여십시오.

    - 노드에 대한 응답이 평가된 후에 실행되는 프로그래밍 방식 호출을 작성하려면 노드 응답에 대한 JSON 편집기를 여십시오.

      ![표준 노드 응답과 관련된 JSON 편집기에 액세스하는 방법 표시.](images/contextvar-json-response.png)

      노드에 대해 **다중 응답** 설정이 **켜져**있으면, **옵션**![고급 응답](images/kabob.png) 메뉴가 표시되기 전에 **응답 편집**![응답 편집](images/edit-slot.png) 아이콘을 클릭하십시오. 

      ![여러 조건부 응답을 사용하도록 설정된 표준 노드와 연결된 JSON 편집기에 액세스하는 방법 표시.](images/contextvar-json-multi-response.png)

    동일한 대화 상자 턴 내에서 외부 서비스의 응답을 표시하거나 추가로 처리하려면 이 조치를 수행하는 두 번째 노드를 추가하고 이 노드에서 점프하십시오.
    {: tip}

    - 개별 슬롯에서 사용할 수 있는 호출을 작성하려면 슬롯에 대한 **슬롯 편집** 아이콘 ![슬롯 편집 아이콘](images/edit-slot.png)을 클릭하고 다음 중 하나를 수행하십시오.

      - 슬롯 조건이 true로 평가된 후에 실행되는 프로그래밍 방식 호출을 작성하려면 슬롯 조건과 관련된 JSON 편집기를 여십시오.

        ![슬롯 조건과 관련된 JSON 편집기에 액세스하는 방법 표시.](images/contextvar-json-slot-condition.png)

      - 슬롯이 채워진 후 실행되는 프로그래밍 방식의 호출을 수행하려면 찾음 응답과 관련된 JSON 편집기를 여십시오. 이를 수행하려면, 슬롯에 대한 **옵션** ![옵션 아이콘](images/kabob.png) 메뉴에서, **조건부 응답 사용**을 클릭하십시오. 찾음 응답의 경우, **응답 편집** ![응답 편집](images/edit-slot.png) 아이콘을 클릭하십시오. 찾음 응답에 대한 **옵션** ![옵션 아이콘](images/kabob.png) 메뉴에서 **JSON 편집기 열기**를 클릭하십시오. 

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
          "type":"client | server",
          "parameters": {
            "<parameter_name>":"<parameter_value>",
            "<parameter_name>":"<parameter_value>"
          },
          "result_variable": "<result_variable_name>",
          "credentials": "<reference_to_credentials>"
        }
      ],
      "output": {
        "text": "response text"
      }
    }
    ```
    {: codeblock}

    `actions` 배열은 대화 상장에서 작성할 프로그래밍 방식 호출을 지정합니다. 이것은 프로그래밍 방식 호출을 5개까지 정의할 수 있습니다. JSON 배열에서 다음과 같은 이름 및 값 쌍을 지정하십시오.

    - `<actionName>`: 필수. 호출할 조치 및 서비스의 이름입니다. 이름은 최대 64자를 초과할 수 없습니다.

       - 클라이언트 조치 유형의 경우, 사용자 원하는 이름으로 지정하십시오. 목표는 클라이언트 애플리케이션을 인식하고 처리 방법을 알 수 있는 이름을 지정하는 것입니다.

          예: `calculateRate`

       - {{site.data.keyword.openwhisk_short}} (서버) 조치 유형의 경우, 다음과 같은 구문을 사용하여 조치의 완전한 이름을 제공하십시오.`/<namespace>/[<package-name>]/<action name>`

         - 조치가 패키지의 일부인 경우, `<package-name>` 정보는 필수이며 패키지의 일부가 아닌 경우, 필수가 아닙니다.
         - 연속적인 조치를 호출하는 경우, `<action name>` 대신 `<sequence name>`을 지정하십시오.
         - 사용자 정의 조치에 대한 네임스페이스 구문은 다음과 같습니다. `<myIBMCloudOrganizationID>_<myIBMCloudSpace>`. 예: `/jdoeorg_prod10/search flights`
         - {{site.data.keyword.openwhisk_short}}에 제공되는 조치는 다음과 같은 네임스페이스(`whisk.system`)를 가지고 있지만, 항상 네임스페이스를 확인해야 합니다. 예: `/whisk.system/weather/forecast`

    - `<type>`: 작성할 호출 유형을 나타냅니다. 다음 유형에서 선택하십시오.

      - **클라이언트**: 외부 클라이언트 애플리케이션이 호출 또는 함수를 수행하고 대화 상자를 대신하여 결과를 확보하는 데 사용할 수 있는 표준화된 형식의 프로그래밍 방식 호출 정보를 사용하여 메시지 응답을 보냅니다. 응답 본문에서 JSON 오브젝트를 호출할 서비스 또는 함수, 호출과 함께 전달할 관련 매개 변수 및 결과를 리턴하는 방법을 지정합니다. 

      - **서버**: {{site.data.keyword.openwhisk_short}} 조치(하나 이상)를 직접 호출합니다. {{site.data.keyword.openwhisk}}를 사용하여 조치 자체를 별도로 정의해야 합니다. 자세한 정보는 [{{site.data.keyword.openwhisk_short}}조치 작성](dialog-actions.html#create-action)을 참조하십시오. 

      유형을 지정하는 것은 선택사항입니다. 기본값은 `client`입니다. 

    - `<action_parameters>`: 외부 프로그램에서 예상하는 모든 매개변수는 JSON 오브젝트로 지정됩니다. 매개변수가 외부 프로그램에서 요구하는 경우에만 필요합니다.

    - `<result_variable_name>`: 외부 서비스 또는 프로그램에서 리턴되는 JSON 오브젝트를 참조하기 위해 사용하는 이름입니다. 결과는 /message 응답의 컨텍스트 섹션에 추가됩니다. 즉, 결과는 컨텍스트 변수로 저장되므로 노드 응답에 표시되거나 나중에 트리거되는 대화 상자 노드가 액세스할 수 있습니다. 컨텍스트 변수의 모든 기존 값이 조치에서 리턴하는 값으로 대체됩니다. 다음 구문을 사용하여 `result_variable_name`을 지정할 수 있습니다. 

      - `my_result`
      - `$my_result`

      이름은 최대 64자를 초과할 수 없습니다. 변수 이름은 다음 문자를 포함할 수 없습니다. 소괄호 `()`, 대괄호 (`[]`), 작은 따옴표(`'`), 인용 부호(`"`) 또는 백슬래시(`\`).

      /message 응답의 출력 또는 입력 섹션에 결과를 저장하는 경우, 다음 위치 키워드 중 하나를 `result_variable_name` 앞에 추가할 수 있습니다.

       - `output.`: /message 응답의 출력 섹션에 결과를 추가합니다. 예: `output.my_result`.
       - `input.`: /message 응답의 입력 섹션에 결과를 추가합니다. 예: `input.my_result`.

      `context.`를 지정할 수 있습니다. 위치 키워드 접두부는 다음과 같습니다. 예: `context.my_result`. 그러나, 결과가 기본적으로 컨텍스트에 추가되므로 수행할 필요는 없습니다.

      변수 이름에 마침표를 포함하여 중첩된 JSON 오브젝트를 작성할 수 있습니다. 예를 들어, 오늘과 내일의 날씨 서비스 예측에 대한 두개의 개별 요청에서 결과를 캡처하기 위해 다음 변수를 정의할 수 있습니다.

      - `context.weather.today`
      - `context.weather.tomorrow`

      결과(`temp` 및 `rain` 매개변수 값)는 이 구조의 컨텍스트에 저장됩니다.
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

      단일 JSON 조치 배열의 다중 조치가 동일한 컨텍스트 변수에 프로그래밍 방식 호출의 결과를 추가하면 컨텍스트가 업데이트되는 순서가 중요합니다.

      1. 배열의 서버 및 클라이언트 조합이 있는 경우, 서비스는 서버 유형 조치를 먼저 처리합니다. 그 결과, 배열의 마지막 클라이언트 유형 조치로 컨텍스트 변수에 대해 계산된 값은 모든 서버 유형 조치로 계산한 값을 겹쳐 씁니다.
      1. 조치 유형별로 배열에 조치가 정의된 순서에 따라 컨텍스트 변수의 값이 설정되는 순서가 결정됩니다. 배열의 마지막 조치로 리턴한 컨텍스트 변수 값은 다른 조치로 계산한 값을 겹쳐 씁니다.

    - `<reference_to_credentials>`: {{site.data.keyword.openwhisk_short}} 신임 정보의 오브젝트 이름이 저장됩니다. 서버 조치에만 필요합니다. 이러한 신임 정보는 조치가 실행되는 {{site.data.keyword.openwhisk_short}} 인스턴스에 액세스하는 데 사용됩니다. 이것은 사용자의 {{site.data.keyword.Bluemix_notm}} 신임 정보가 아닙니다. 

      신임 정보를 검색하려면, 다음 단계를 완료하십시오.
      1.  [{{site.data.keyword.openwhisk_short}} API 키 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/openwhisk/learn/api-key){: new_window} 페이지로 이동하십시오. 

          - 아직 계정을 작성하지 않은 경우, 계정을 작성하십시오.
          - 로그인하지 않은 경우, 로그인하십시오.

      1.  신임 정보를 표시하려면, Click the **Auth 키 표시** 아이콘 ![Auth 키 표시](images/show-auth-icon.png)을 클릭하십시오. 콜론(:) 앞의 세그먼트는 사용자 ID입니다. 콜론 다음의 세그먼트는 사용자의 비밀번호입니다. 

      **주의**: 조치가 실행될 때 발생하는 모든 비용은 이 신임 정보를 소유한 사람에게 부과됩니다.

      신임 정보를 보호하려면, {{site.data.keyword.conversationshort}} 작업영역에 신임 정보를 저장하지 마십시오. 대신, 컨텍스트의 일부로 클라이언트 애플리케이션에서 전달하십시오. 컨텍스트 변수를 메시지 컨텍스트의 $private 섹션에 중첩하여 정보가 Watson 로그에 저장되는 것을 방지할 수 있습니다. 예: `$private.my_credentials`.

      사용자가 정의하는 신임 정보 오브젝트는 `user` 및 `password` 매개변수를 포함해야 합니다.

      ```json
      {
        "user":"5tj3b41j-bf3j-5d92-24g9-4a7769ab12af",
        "password":"y65gqSTSRzqE..."
      }
      ```
      {: codeblock}

      대화 상자를 테스트하는 동안 도구의 "연습" 분할창에서 **컨텍스트 관리**를 클릭하여 실제 {{site.data.keyword.openwhisk_short}} 사용자 이름과 비밀번호 값으로 `$private.my_credentials` 컨텍스트 변수를 임시로 설정할 수 있습니다. 

      ![연습 컨텍스트 관리 인터페이스에서 $ private.my_credentials 컨텍스트 변수가 정의된 방법 표시](images/testing-creds.png)

## {{site.data.keyword.openwhisk_short}} 조치 작성
{: #create-action}

서버 유형 프로그래밍 방식 호출을 정의하도록 선택한 경우, 대화 상자에서 호출하기 전에 {{site.data.keyword.openwhisk}}에서 조치를 작성해야 합니다. 클라이언트 유형 프로그래밍 방식 호출을 정의하는 경우, 이 프로시저를 건너 뛰십시오.

**참고:**{{site.data.keyword.openwhisk_short}} 조치를 실행하면 비용이 발생할 수 있습니다. 자세한 정보는 [가격 책정 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/openwhisk/learn/pricing){: new_window}을 참조하십시오. {{site.data.keyword.openwhisk_short}}는 테스트 중에 "연습" 분할 창에서 작성된 호출과 프로덕션의 애플리케이션에서 작성된 호출을 구분하지 않습니다. 

{{site.data.keyword.openwhisk_short}} 조치를 작성하려면, 다음 단계를 완료하십시오.

1.  브라우저에서 직접 코드를 작성할 수 있는 [온라인 {{site.data.keyword.openwhisk_short}}편집기 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.ng.bluemix.net/openwhisk/create){: new_window}으로 이동하십시오. 

    또한 로컬로 작성하는 코드를 사용하여 조치를 정의할 수 있는 [명령행 인터페이스![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.bluemix.net/openwhisk/learn/cli){: new_window}도 설치할 수 있습니다. 

1.  지원되는 프로그래밍 언어 중 하나를 사용하여 {{site.data.keyword.openwhisk_short}} 조치를 작성하십시오. 세부사항은 [{{site.data.keyword.openwhisk_short}} 문서 ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://console.ng.bluemix.net/docs/openwhisk/openwhisk_actions.html){: new_window}를 참조하십시오. 

    다음 팁을 염두에 두십시오.

    - 외부 서비스를 호출하는 방법은 [{{site.data.keyword.openwhisk_short}} 조치![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘") 예제](https://console.ng.bluemix.net/docs/openwhisk/openwhisk_actions.html#openwhisk_apicall_action){: new_window}를 참조하십시오. 
    - Watson 서비스를 호출하려면, 사용하려는 언어로 [Watson Developer Cloud SDK ![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/watson-developer-cloud){: new_window}를 사용하십시오.
    - {{site.data.keyword.openwhisk_short}} 조치가 모든 입력 매개 변수를 JSON 오브젝트로 승인하고 모든 출력을 JSON 오브젝트로 리턴하는지 확인하십시오.
    - Node.js를 사용하여 {{site.data.keyword.openwhisk_short}} 조치를 작성하는 경우, `Promise`를 사용해야 합니다. 또한, `main` 함수에서 최종 결과를 리턴하는지 확인하십시오.

    대체적인 방법으로 연속적인 조치를 작성할 수 있습니다.
    {: tip}

## 핸들링 오류

{{site.data.keyword.openwhisk_short}} 조치에서 오류가 발생하면, 오류 메시지가 대화 상자로 리턴되고 `cloud_functions_call_error`라는 응답 변수의 특성으로 저장됩니다. {{site.data.keyword.openwhisk_short}} 조치가 외부 서비스에서 응답을 받지 못하거나 Cloud Function 조치가 실패한 경우, 오류가 발생할 수 있습니다. Cloud Function 신임 정보가 제공되지 않거나 올바르지 않은 경우, 오류가 리턴됩니다. 이 컨텍스트 변수는 서버 조치에만 사용됩니다. 클라이언트 애플리케이션에서 오류 정보를 캡처하여 컨텍스트 변수로 대화 상자에 리턴하는 유사한 오브젝트를 작성하는 것을 고려하십시오. 

먼저 오류를 확인하기 위해 대화 상자 노드 응답을 조건화할 수 있습니다. 예를 들어, 이 표현식을 응답 조건에 추가하여 오류가 발생하지 않은 경우에만 {{site.data.keyword.openwhisk_short}} 조치 결과를 참조하는 응답이 표시되도록 할 수 있습니다.

```json
  $forecast_result.cloud_functions_call_error == null
```
{: codeblock}

클라이언트 유형 프로그래밍 방식 호출의 경우, 컨텍스트 변수(`action_error`)를 정의하여 오류 처리에 대한 정보를 전달할 수 있습니다. 결과 변수의 일부로 서비스에 다시 전달할 수 있습니다. 그러면, 다음과 같은 응답 조건을 정의하여 오류가 발생하지 않은 경우에만 응답을 표시할 수 있습니다. 

```json
  $forecast_result.action_error == null
```
{: codeblock}

## 클라이언트 호출 예제
{: #action-client-example}

다음 예제는 외부 날씨 서비스에 대한 호출을 보여줍니다. 노드 응답과 연관된 JSON 편집기에 추가됩니다. 노드 레벨 응답이 트리거될 때까지 슬롯은 사용자로부터 날짜 및 위치 정보를 수집하여 저장합니다. 이 예제에서는 호출할 서비스가 `/weather`라고 이름 지정된 엔드포인트를 가지고 있고, `location` 및 `date` 매개변수를 사용하고 JSON 오브젝트(`{"forecast": "<value>"}`)를 리턴합니다.

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

일반적으로, 이 서비스는 새로운 사용자 입력이 필요한 경우(예:상위 노드를 실행한 후 해당 하위 노드 중 하나를 실행하기 전에)에만 POST /message 요청을 통해 클라이언트에게 리턴됩니다. 그러나, 노드에 클라이언트 조치를 추가한 경우, 평가 후에 서비스는 항상 클라이언트로 리턴되어 조치 호출의 결과가 리턴될 수 있습니다. 하위 노드로 직접 점프하도록 구성된 노드와 같이 사용자 입력을 대기하지 않도록 하기 위해 서비스는 다음 값을 메시지 컨텍스트에 추가합니다.

```json
  {
    "context": {
      "skip_user_input": true
    }
  }
```
{: codeblock}

클라이언트가 사용자 입력을 받지 않고 조치 수행을 원하는 경우, 동일한 규칙에 따라 상위 노드에 'skip_user_input` 컨텍스트 변수를 추가하여 클라이언트 애플리케이션과 통신할 수 있습니다.

클라이언트 애플리케이션은 컨텍스트에서 항상 `skip_user_input` 변수를 검사해야 합니다. 존재하는 경우, 사용자로부터 새로운 입력을 요청하지 않고, 그 대신에 조치를 실행하고, 결과를 메시지에 추가한 다음, 서비스로 다시 전달합니다. 새 POST 메시지 요청에는 이전 POST 메시지 응답(즉, 컨텍스트, 입력, 인텐트, 엔티티 및 선택적으로 출력 섹션)으로 리턴한 메시지가 포함되어야 하며 프로그래밍 방식의 호출을 정의하는 JSON 오브젝트 대신 프로그래밍 방식 호출에서 리턴된 결과를 포함해야합니다.

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

다음 다이어그램에서는 클라이언트 호출을 사용하여 일기 예보 정보를 가져오고 이를 사용자에게 리턴하는 방법을 보여줍니다.

![일기 예보를 요청하고, 대화 상자가 클라이언트 앱에 요청을 보내고, 클라이언트 앱이 외부 서비스에 요청을 전송하는 것을 표시](images/forecast.png)

## 서버 호출 예제
{: #action-server-example}

다음 예제는 {{site.data.keyword.openwhisk_short}} 조치에 대한 호출을 표시합니다. 이 예제는 [Utilities package ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/openwhisk/openwhisk_actions.html#openwhisk_create_action_sequence){: new_window}에 정의된 {{site.data.keyword.openwhisk_short}} `echo` 조치를 사용하는 방법을 표시합니다. 조치는 텍스트 문자열을 사용하여 리턴합니다.

 ``` json
{
  "actions": [
    {
      "name": "/whisk.system/utils/echo",
      "type":"server",
      "parameters": {
        "message": "<?input.text?>"
      },
      "result_variable": "context.my_input_returned",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

`context.my_input_returned` 변수에 저장된 {{site.data.keyword.openwhisk_short}} 조치의 결과물은 이제 후속 대화 노드에서 액세스할 수 있습니다.

 ``` json
 {
  "output": {
    "text": {
      "values": [
        "Your input was: $my_input_returned."
      ]
    }
  }
}
```
{: codeblock}

다음 다이어그램은 내장된 {{site.data.keyword.openwhisk_short}} 에코 서비스를 호출하는 간단한 예제를 사용하여 {{site.data.keyword.openwhisk_short}} 조치를 호출하는 방법을 보여줍니다. 이는 사용자에게 입력을 요청하고 에코 서비스에 전달합니다. 에코 서비스는 사용자에게 표시되는 동일한 텍스트를 리턴합니다.

![텍스트를 입력하는 사용자를 표시하고 대화 상자가 요청을 {{site.data.keyword.openwhisk_short}} 서비스로 보낸 다음 해당 텍스트를 대화 상자로 리턴합니다.](images/echo-via-cf.png)

### Echo 조치 예제

이미 {{site.data.keyword.openwhisk_short}} 내장 Echo 조치를 호출하도록 설정된 대화 상자가 있는 작업영역을 보려면 다음 단계를 완료하십시오.

1.  [CloudFunctionsEcho.json ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud/community/raw/master/conversation/cloud-functions-echo.json){: new_window} 파일을 다운로드하십시오.
1.  JSON 파일을 새 작업공간으로 가져오십시오.
1.  대화 상자를 검토하여 Echo 조치 호출이 지정된 방식을 확인하십시오.
1.  "연습" 분할 창에서, **컨텍스트 관리**를 클릭하고, 그런 다음, 컨텍스트 변수를 {{site.data.keyword.openwhisk_short}} 사용자 이름 및 비밀번호에 설정하십시오(임시).

    ```json
    $private.my_credentials
    {
      "user":"<your-CF-instance-username>",
      "password":"<your-CF-instance-password>"
    }
    ```
    {: codeblock}

1.  일부 사항을 입력하여 대화 상자를 테스트하십시오.

    서비스는 {{site.data.keyword.openwhisk_short}} Echo 조치를 사용하여 사용자가 입력한 내용을 반복합니다.

## 고급 서버 호출 예제
{: #advanced-action-server-example}

단일 대화 상자 플로우 내에서 다중 조치를 호출할 수 있습니다. 단일 대화 상자 노드에서 하나의 `actions` JSON 오브젝트당 5개까지의 조치를 호출할 수 있습니다. 그러나, `actions` JSON 배열에 정의된 모든 서버 유형 조치는 모두 병렬로 처리됩니다. 따라서, 사용자는 하나의 서버 유형 조치를 호출하여 그 결과를 동일한 'actions` 블록의 두 번째 서버 유형 조치로 전달할 수 없습니다. 특정 순서의 서버 조치를 호출하는 가장 좋은 방법은 {{site.data.keyword.openwhisk_short}} 순서를 사용하는 것입니다. 실행 시에, 이 접근법은 대화 상자가 하나의 외부 호출만으로 다중 조치를 완료하기 때문에 더 빠릅니다. 순서를 사용하려면, `actions` 블록 정의에 있는 조치 이름 대신 순서 이름을 참조하십시오. 또는 한 노드에서 첫 번째 서버 유형 조치를 호출하고 다음 서버 유형 조치를 호출하는 하위 노드로 이동할 수 있습니다.

클라이언트 유형과 서버 유형 조치가 혼합된 하나의`actions` 배열을 정의하는 경우, 대화 상자 노드가 실행될 때 클라이언트 조치는 모든 서버 유형 조치가 처리될 때까지 클라이언트로 전송되지 않습니다.
{: tip}

다음 예제는 {{site.data.keyword.openwhisk_short}} 조치에 대한 호출을 표시합니다.

이 예제에서는 {{site.data.keyword.openwhisk_short}} 조치를 사용하여 도시 이름을 가져와서 제공된 위치의 위도와 경도 좌표를 리턴하는 외부 서비스를 호출하는 방법을 표시합니다.

``` json
{
  "actions": [
        {
      "name": "/jdoeorg_prod/get coordinates",
      "type":"server",
      "parameters": {
        "location": "$location"
      },
      "result_variable": "context.my_coordinates",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

`$my_coordinates` 컨텍스트 변수는 `get coordinates` 서비스로 리턴한 두 개의 값을 다음과 같이 저장합니다.

```json
{
 "lat":"42.3611",
 "long":"-71.0571"
}
```
{: codeblock}

이 예제에서는 {{site.data.keyword.openwhisk_short}} 서비스와 함께 제공되는 [Weather package ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/openwhisk/openwhisk_weather.html#openwhisk_catalog_weather){: new_window}에 정의된 {{site.data.keyword.openwhisk_short}} `forecast` 조치를 사용하는 방법을 표시합니다. 이 조치는 위도 및 경도 좌표와 시간 주기가 필요합니다. 지정된 기간 동안 지정된 위치에 대한 예측 정보가 있는 JSON 오브젝트를 리턴합니다. 이전 조치로 리턴하는 좌표는 `$my_coordinates.lat` 및 `$my_coordinates.long`으로 지정됩니다.

 ``` json
{
  "actions": [
    {
      "name": "/whisk.system/weather/forecast",
      "type":"server",
      "parameters": {
        "latitude": "$my_coordinates.lat",
        "longitude": "$my_coordinates.long",
        "timePeriod": "$period",
        "username": "$private.my_weather_service.username",
        "password": "$private.my_weather_service.password"
      },
      "result_variable": "context.forecasts",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

**참고**: 사용자 이름 및 비밀번호는 매개변수로 표시됩니다. 이러한 정보는 이 특정 조치가 요청할 때에만 존재하며, 또한, 백엔드에서 제공된 조치가 호출하는 외부 날씨 서비스에 필요한 신임 정보를 정의합니다. 이것은 IBM Cloud Function 계정 신임 정보와는 다릅니다. 이 신임 정보를 비공개로 유지하려면 다음 단계를 수행하십시오.

`context.forecasts` 변수에 저장된 {{site.data.keyword.openwhisk_short}} 조치의 출력은 이제 후속 대화 상자 노드에서 액세스가 가능합니다.

 ``` json
 {
  "output": {
    "text": {
      "values": [
        "For the next $period in $location, you can expect $forecasts."
      ]
    }
  }
}
```
{: codeblock}

다음 다이어그램은 복합 상호작용을 보여줍니다. 사용자가 날씨 예보를 요청합니다. 대화 상자 노드의 슬롯은 사용자 위치 및 기간 정보를 프롬프트합니다. {{site.data.keyword.openwhisk_short}} 예보 서비스를 호출하려면, 지리적 좌표를 제공해야 합니다. 따라서, 대화 상자는 {{site.data.keyword.openwhisk_short}} 서비스에 요청을 전송하여 사용자가 제공한 위치에 대한 위도 및 경도 정보를 먼저 가져와서 좌표 정보를 리턴하는 외부 서비스로 전달합니다. 후속 노드에서, 대화 상자는 사용자로 부터 확보한 기간 정보와 함께 새로 확보한 좌표 정보를 {{site.data.keyword.openwhisk_short}} 내장된 예측 서비스로 보내 예측 세부 정보를 얻습니다. 대화 상자는 {{site.data.keyword.openwhisk_short}} 예측 서비스에서 제공한 예측 결과를 표시하여 사용자의 질문에 응답합니다.

![날씨 예보를 요청하고 대화 상자가 Cloud Function 서비스에 요청을 전송하여 외부 서비스에 전달하는 것을 표시.](images/forecast-via-cf.png)
