---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-24"

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

# Slack에서 테스트

테스트 배치 도구를 사용하여 {{site.data.keyword.conversationshort}} 작업공간을 Slack 팀에 봇 사용자로 통합할 수 있습니다. Slack 봇을 작업공간의 사용자 인터페이스로 사용하여 신속하게 테스트하려는 경우 이 방법을 사용하십시오.

테스트 배치 도구는 {{site.data.keyword.openwhisk}} 서비스를 사용하여 미리 빌드된 Slack 애플리케이션을 팀에 봇 사용자로 배치합니다. 이 애플리케이션은 {{site.data.keyword.conversationshort}} 작업공간과의 통신을 처리합니다.

![테스트 배치 개요 다이어그램](images/testdeploy_diagram.png)

테스트 배치 도구에 몇 가지 제한사항이 있습니다.

- 다른 팀이 사용할 애플리케이션을 공개하는 데 이 도구를 사용할 수 없습니다.
- 이 방법을 사용하여 동일한 팀에 둘 이상의 작업공간을 배치하는 경우 모든 작업공간이 `@ibmwatson_bot` 사용자 이름에 응답합니다. 각 Slack 팀에 한 번에 하나의 작업공간만 배치하려면 이 도구를 사용하는 것이 좋습니다.
- Slack 팀에 앱을 설치하는 권한이 있어야 합니다. 이 권한이 있는지 확실하지 않은 경우 Slack 관리자에게 확인하십시오.
- 미리 빌드된 Slack 애플리케이션은 오로지 테스트용이며 항상 사용 가능한 것은 아닙니다.
- {{site.data.keyword.openwhisk_short}} 제한사항으로 인해 이 도구는 현재 {{site.data.keyword.Bluemix_notm}} 미국 남부 지역에서만 사용 가능합니다.

애플리케이션을 봇 사용자로 설치하려면 다음을 수행하십시오.

1. {{site.data.keyword.conversationshort}} 도구의 경우 Slack에서 테스트할 작업공간을 여십시오.
1. 왼쪽 상단 구석에 있는 메뉴 아이콘을 클릭한 다음 **배치**를 선택하십시오. 배치 옵션 페이지가 열립니다.

   ![빠른 배치 메뉴 옵션](images/deploy_menu_testdeploy.png)

1. **{{site.data.keyword.openwhisk_short}}와 함께 배치**아래에서, **Slack에서 테스트**를 클릭하고 다음 지시사항을 따르십시오.

   ![Slack 테스트 작성 단추](images/testdeploy_testinslack.png)

## 봇과의 대화

배치 프로세스를 완료한 후, 다른 Slack 봇의 경우와 같이 `@ibmwatson_bot` 사용자 이름을 사용하여 {{site.data.keyword.conversationshort}} 작업공간과 상호작용할 수 있습니다.

팀에 배치된 봇은 특정 채널 내 각 사용자의 상태를 보존합니다. 즉, 대화 상자 컨텍스트에 저장한 변수는 대화 상자가 지워지지 않는 한 무기한 유지됩니다.

대화를 알려진 시작 상태로 재설정해야 하는 경우 대화 상자 내에서 이를 수행해야 합니다. 대화 상자에 대화 종료 시 또는 다시 시작해야 하는 때 실행되는 노드가 있는지 확인하십시오. 모든 컨텍스트 변수를 적절한 시작 값으로 재설정하려면 이 노드의 JSON을 업데이트하십시오. (필요한 경우, **점프** 조치 또는 특수 "종료 대화" 인텐트를 사용하여 이 노드를 실행할 수 있습니다.)

예를 들어, 작업공간이 컨텍스트 변수 `drink_order`를 사용하여 사용자의 음료 선택을 저장하는 경우, 대화 종료 시 `context.remove` 메소드를 사용하여 이 변수를 삭제할 수 있습니다.

```json
"context": {
   "reset_drink_order": "<?context.remove('drink_order')?>"
 }
```
{: codeblock}

컨텍스트 변수 값 수정에 대한 자세한 정보는 [컨텍스트 변수 값 업데이트](dialog-overview.html#updating-a-context-variable-value)를 참조하십시오.

**참고:** 작업공간 테스트를 완료하면 테스트 배치 도구로 돌아가 **테스트 삭제**를 클릭하여 테스트 배치를 삭제할 수 있습니다. Slack 팀에서 봇 애플리케이션의 권한을 별도로 해제해야 합니다.
