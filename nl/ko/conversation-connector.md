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

# {{site.data.keyword.conversationshort}} 커넥터로 채널에 배치

작업공간을 개발한 후 {{site.data.keyword.conversationshort}} 커넥터를 사용하여 Slack이나 Facebook Messenger와 같은 통신 채널에 신속하게 연결할 수 있습니다. {{site.data.keyword.conversationshort}} 커넥터는 {{site.data.keyword.conversationshort}}작업영역과 Slack 또는 Facebook 앱 간의 통신을 조정하여 Cloudant 데이터베이스에 세션 데이터를 저장하는 {{site.data.keyword.openwhisk}} 컴포넌트 세트입니다. 

작업공간을 채널 앱에 연결하면 Slack 또는 Facebook Messenger 사용자가 상호 작용할 수 있는 채팅 봇으로 사용할 수 있습니다. 사용자 대화 상자의 응답은 이미지 및 클릭 가능한 단추 등과 같은 대화식 응답 및 멀티미디어를 포함할 수 있습니다. (멀티미디어 응답 정의에 관한 자세한 정보는 [멀티미디어 응답](dialog-multimedia.html)을 참조하십시오.)

![{{site.data.keyword.openwhisk_short}} 배치 개요 다이어그램](images/deploytochannel_diagram.png)

{{site.data.keyword.conversationshort}} 커넥터는 일부 제한사항이 있습니다.

- Slack 또는 Facebook 앱을 작성하거나 사용하려는 앱을 수정하려면 관리 권한이 있어야합니다.
- {{site.data.keyword.openwhisk_short}} 제한사항으로 인해 이 도구는 현재 {{site.data.keyword.Bluemix_notm}} 미국 남부 지역에서만 사용 가능합니다.

## {{site.data.keyword.conversationshort}} 도구를 사용하여 Slack 배치

{{site.data.keyword.conversationshort}} 도구는 {{site.data.keyword.conversationshort}} 커넥터를 사용하여 봇을 신속하게 배치하는 방법을 제공합니다. 이 도구는 채널 앱과 커넥터 컴포넌트를 구성하는 데 필요한 정보를 수집하는 과정을 단계별로 설명하고 필수 컴포넌트를 IBM Cloud에 자동으로 배치합니다.

**참고:** {{site.data.keyword.conversationshort}} 도구 인터페이스는 현재 Slack만 지원하지만 자동화된 {{site.data.keyword.Bluemix_short}} 프로세스를 사용하여 Facebook Messenger에 배치할 수 있습니다. Facebook Messenger 배치에 관한 자세한 정보는 {{site.data.keyword.conversationshort}} 커넥터에 있는 문서 [GitHub 저장소![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/watson-developer-cloud/conversation-connector/blob/master/channels/facebook/README.md){: new_window}를 참조하십시오.

자동화된 배치 도구를 사용하여 봇을 배치하려면 다음을 수행하십시오. 

1. {{site.data.keyword.conversationshort}} 도구에서, 배치하려는 작업공간을 여십시오.
1. 왼쪽 상단 구석에 있는 메뉴 아이콘을 클릭한 다음 **배치**를 선택하십시오.

   ![빠른 배치 메뉴 옵션](images/deploy_menu_testdeploy.png)

1. **{{site.data.keyword.openwhisk_short}}와 함께 배치** 아래에서, **Slack에 배치**를 클릭하십시오.
1. Slack 페이지에서, **Slack 앱에 배치**를 클릭하고 다음 지시사항을 수행하십시오.

   ![Slack 앱에 배치 단추](images/deploy_deploytoslack.png)

## 수동으로 배치

자동화된 도구를 사용하여 작업공간을 봇으로 배치하는 대신 필요한 컴포넌트를 수동으로 구성하여 IBM 클라우드에 배치할 수 있습니다. 다음과 같이 수행할 수 있는 여러 가지 상황이 있습니다.

- **부분 재배치**. 구성을 변경하거나 문제 해결 또는 최신 버전의 수정 사항을 적용하려면 기존 배치의 일부 컴포넌트를 다시 배치해야 할 수 있습니다. 
- **신규 기능으로 봇 확장**. 제공된 컴포넌트를 수정하여 {{site.data.keyword.conversationshort}} 작업공간으로 보내기 전에 사용자 입력을 사전 처리하는 등의 새 기능을 추가 할 수 있습니다.
- **새 채널에 배치**. Slack 또는 Facebook Messenger가 아닌 다른 채널에 봇을 배치하려는 경우 기존 컴포넌트의 패턴을 따라 자신만의 컴포넌트를 개발할 수 있습니다. 

{{site.data.keyword.conversationshort}} 커넥터 컴포넌트는 공용 GitHub 저장소에서 호스팅되며 다운로드하거나 복제할 수 있습니다. 자세한 정보는 [저장소![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/watson-developer-cloud/conversation-connector){: new_window}에서 제공되는 문서를 참조하십시오.

## 봇과의 대화

배치 프로세스를 완료한 후에는 다른 Slack 또는 Facebook Messenger 봇의 경우와 같이 bot 사용자 이름을 사용하여 {{site.data.keyword.conversationshort}} 작업영역과 상호작용할 수 있습니다.

{{site.data.keyword.conversationshort}} 커넥터는 봇과 상호작용하는 각 사용자에 대해 개별적으로 상태를 유지한다는 점을 주의하십시오. (Slack의 경우, 단일 사용자가 봇과 직접 대화하고 각 채널에 하나씩 여러 개의 고유 대화를 가질 수 있습니다.) 즉 사용자가 대화 상자 컨텍스트에 저장하는 모든 변수는 지우기 전에는 무기한으로 유지됨을 의미합니다. 

대화를 알려진 시작 상태로 재설정해야 하는 경우 대화 상자 내에서 이를 수행할 수 있습니다. 대화 상자에 대화 종료 시 또는 다시 시작해야 하는 때 실행되는 노드가 있는지 확인하십시오. 모든 컨텍스트 변수를 적절한 시작 값으로 재설정하려면 이 노드의 JSON을 업데이트하십시오. (필요한 경우, **점프** 조치 또는 특수 "종료 대화" 인텐트를 사용하여 이 노드를 실행할 수 있습니다.)

예를 들어, 작업공간이 컨텍스트 변수 `drink_order`를 사용하여 사용자의 음료 선택을 저장하는 경우, 대화 종료 시 `context.remove` 메소드를 사용하여 이 변수를 삭제할 수 있습니다.

```json
"context": {
   "reset_drink_order": "<?context.remove('drink_order')?>"
 }
```
{: codeblock}

컨텍스트 변수 값 수정에 대한 자세한 정보는 [컨텍스트 변수 값 업데이트](dialog-runtime.html#context-update)를 참조하십시오.

배치된 {{site.data.keyword.openwhisk_short}} 조치의 편집을 통해 컨텍스를 지우거나 봇의 동작을 변경할 수도 있습니다. 자세한 정보는 [GitHub 저장소![외부 링크 아이콘](../../icons/launch-glyph.svg "외부 링크 아이콘")](https://github.com/watson-developer-cloud/conversation-connector){: new_window}에서 제공되는 문서를 참조하십시오. 
