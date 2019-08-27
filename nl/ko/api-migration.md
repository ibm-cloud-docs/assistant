---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-30"

subcollection: assistant


---

{:curl: #curl .ph data-hd-programlang='curl'}
{:external: target="_blank" .external}
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

# v2 API로 마이그레이션
{: #api-migration}

어시스턴트 및 스킬의 사용을 지원하는 Assistant v2 런타임 API가 2018년 11월에 도입되었습니다. 이 API는 자동 상태 관리, 용이한 배치, 스킬 버전화 및 검색 스킬과 같은 새로운 기능의 가용성을 포함하여 v1 런타임 API에 비해 상당한 장점을 제공합니다. 

서비스 플랜에 관계없이 모든 사용자가 추가 비용 없이 v2 API를 사용할 수 있습니다. 

현재 v2 API는 기존의 어시스턴트와 런타임 상호작용만 지원합니다. 작업공간을 작성하거나 수정하는 작성 애플리케이션은 계속해서 v1 API를 사용해야 합니다.
{: note}

## 개요

사용자의 클라이언트 앱은 v2 API를 사용하여 작업공간과 직접 통신하는 대신 어시스턴트와 통신합니다. 어시스턴트는 자동 상태 관리, 스킬 버전화, 더 용이한 배치 및 (Plus 및 Premium 플랜) 검색 스킬을 포함한 몇 가지 새로운 기능을 제공하는 새 오케스트레이션 계층입니다. 기존 작업공간(이제는 _대화 스킬_이라고 함)은 이전과 같이 계속 작동하지만 새 어시스턴트 계층에서 새 기능을 제공합니다.

어시스턴트와의 모든 통신은 대화의 지속 기간 동안 대화 상태를 유지하는 _세션_ 컨텍스트 내에서 수행됩니다. 대화 또는 클라이언트 애플리케이션에서 정의한 컨텍스트 변수를 포함한 상태 데이터는 애플리케이션 부분에서 요구하는 어떠한 조치 없이{{site.data.keyword.conversationshort}}에서 자동으로 저장합니다. 

상태 데이터는 세션을 명시적으로 삭제할 때까지 또는 비활성으로 인해 세션이 제한시간 초과될 때까지 지속됩니다. 

v1 API를 사용하여 사용자 입력을 직접 작업공간에 전송하는 기존 애플리케이션이 있다면, v2 API를 사용하도록 앱을 마이그레이션하는 것은 간단한 프로세스입니다. 

## 어시스턴트 설정

v2 런타임 API는 메시지를 사용자의 대화 스킬(이전의 작업공간)에 라우팅하는 어시스턴트에 메시지를 전송합니다. 어시스턴트를 설정하려면 {{site.data.keyword.conversationshort}} 사용자 인터페이스를 사용하십시오. 

1. **스킬** 탭을 클릭하십시오. 작업공간이 사용 가능한 스킬로 표시되어 있는지 확인하십시오. (서비스 인스턴스에 대한 모든 기존 작업공간은 {{site.data.keyword.conversationshort}} 사용자 인터페이스에서 스킬로 자동 변환됩니다. 이 변환은 기본 작업공간을 변경하지 않습니다.)

1. **어시스턴트** 탭을 클릭하십시오. **어시스턴트 작성**을 클릭하여 새 어시스턴트를 작성하십시오. 스킬을 추가하도록 프롬프트가 표시되면 **대화 스킬 추가**를 클릭하고 작업공간에 해당하는 대화 스킬을 선택하십시오.

  어시스턴트 작성에 대한 자세한 정보는 [어시스턴트 작성](https://cloud.ibm.com/docs/services/assistant?topic=assistant-assistant-add)을 참조하십시오.

1. 새 어시스턴트가 작성된 후 ![Menu](images/kebab-react.png) 메뉴를 클릭하고 **설정**을 선택하십시오. 

1. **어시스턴트 설정** 페이지에서 어시스턴트 ID를 찾으십시오. 애플리케이션은 이 ID(워크스페이스 ID 대신)를 사용하여 어시스턴트와 통신합니다. 서비스 인증 정보는 v1과 v2 API 모두 동일합니다. 

  현재, 어시스턴트 ID를 검색하기 위한 API 지원은 없습니다. 어시스턴트 ID를 찾으려면 {{site.data.keyword.conversationshort}} 사용자 인터페이스를 사용해야 합니다. {: note}

## v2 런타임 API 호출

어시스턴트를 작성한 후에는 v1 런타임 API 대신 v2 런타임 API를 사용하도록 클라이언트 애플리케이션을 업데이트할 수 있습니다.

1. 대화에서 첫 번째 메시지를 보내기 전에 v2 [**세션 작성**](https://cloud.ibm.com/apidocs/assistant-v2#create-a-session){: external} 메소드를 사용하여 세션을 작성하십시오. 리턴된 세션 ID를 저장하십시오. 

  ```javascript
  service
  .createSession({
    assistant_id: assistantId,
  })
  .then(res => {
    sessionId = res.session_id;
  })
  ```
  {: codeblock}
  {: javascript}

  ```python
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']
  ```
  {: codeblock }
  {: python }

  ```java
  CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
  SessionResponse session = service.createSession(createSessionOptions).execute().getResult();
  String sessionId = session.getSessionId();
  ```
  {: codeblock}
  {: java}

1. v2 [**사용자 입력을 어시스턴트에 전송**](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant){: external} 메소드를 사용하여 사용자 입력을 어시스턴트에 전송하십시오. v1 API를 사용할 때처럼 작업공간 ID를 지정하는 대신에, 어시스턴트 ID와 세션 ID를 지정하십시오. 

  ```javascript
  service
    .message({
      assistant_id: assistantId,
      session_id: sessionId,
      input: messageInput
    })
  ```
  {: codeblock}
  {: javascript}

  ```python
  response = service.message(
      assistant_id,
      session_id,
      input = message_input
  ).get_result()
  ```
  {: codeblock}
  {: python}

  ```java
MessageInput input = new MessageInput.Builder().text(inputText).build();
      MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId)
    .input(input)
    .build();
  MessageResponse response = service.message(messageOptions)
    .execute()
    .getResult();
  ```
  {: codeblock}
  {: java}

  기본 메시지 구조는 변경되지 않았으며 특히 사용자 입력은 여전히 `텍스트`로 전송됩니다. 

1. 대화가 종료된 후 v2 [**세션 삭제**](https://cloud.ibm.com/apidocs/assistant-v2#delete-session){: external} 메소드를 사용하여 세션을 삭제하십시오.

  ```javascript
  service
    .deleteSession({
      assistant_id: assistantId,
      session_id: sessionId,
    })
  ```
  {: codeblock}
  {: javascript}

  ```python
service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
  ```
  {: codeblock}
  {: python}

  ```java
  DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId
    .build();
  service.deleteSession(deleteSessionOptions).execute();
  ```
  {: codeblock}
  {: java}

   세션을 명시적으로 삭제하지 않으면 구성된 제한시간 간격 이후에 자동으로 삭제됩니다. (제한시간 지속 기간은 플랜에 따라 결정됩니다. 자세한 정보는 [세션 한계](/docs/services/assistant?topic=assistant-assistant-settings#assistant-settings-session-limits)를 참조하십시오.)

간단한 클라이언트 애플리케이션 컨텍스트에서 v2 API 예제를 보려면 [클라이언트 애플리케이션 빌드](/docs/services/assistant?topic=assistant-api-client)를 참조하십시오.

## v2 응답 형식 처리

애플리케이션에서 액세스하는 데 필요한 응답 파트에 따라 애플리케이션을 v2 런타임 응답 형식을 처리하도록 업데이트해야 할 수 있습니다. 

- 모든 응답 유형에 대한 출력(예: `text` 및 `option`)은 여전히 `output.generic` 오브젝트에 리턴됩니다. 이러한 응답을 처리하기 위한 애플리케이션 코드는 수정 없이 작동해야 합니다.

- 발견된 인텐트와 엔티티는 이제 응답 JSON의 루트가 아니라 `output`의 일부로 리턴됩니다. 

- 대화 컨텍스트는 이제 두 개의 오브젝트로 구성됩니다. 

  - **글로벌 컨텍스트**는 어시스턴트 사용하는 모든 스킬에서 공유하는 시스템 레벨 컨텍스트 데이터를 포함합니다.

  - **스킬 컨텍스트**는 대화 스킬에서 사용하는 사용자 정의 컨텍스트 변수를 포함합니다. 

  그러나, 대화 컨텍스트를 포함하는 상태 데이터가 현재 어시스턴트에 의해 유지보수되므로, 사용자의 애플리케이션은 모든 컨텍스트에 액세스할 필요가 없을 수도 있습니다([어시스턴트가 상태 유지보수](#api-migration-state) 참조).

v2 응답 형식에 대한 완전한 문서는 v2 [API 참조](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant){: external}를 참조하십시오.

## 어시스턴트가 상태 유지보수
{: #api-migration-state}

이제 대부분의 애플리케이션에서 상태 유지보수 목적으로 포함된 모든 코드를 제거할 수 있습니다. 더 이상 컨텍스트를 저장하고 차례대로 다시 {{site.data.keyword.conversationshort}}로 전송하는 것이 필요하지 않습니다. 컨텍스트는 자동으로 {{site.data.keyword.conversationshort}}에 의해 유지보수되며 이전과 같이 대화에서 액세스할 수 있습니다.

v2 API에서는 컨텍스트가 클라이언트 애플리케이션에 대한 응답에 기본적으로 포함되지 않는다는 점에 유의하십시오. 그러나 필요한 경우 코드는 여전히 컨텍스트 변수에 액세스할 수 있습니다. 

- 메시지 입력의 일부로 `context` 오브젝트를 여전히 전송할 수 있습니다. 사용자가 포함하는 모든 컨텍스트 변수는 {{site.data.keyword.conversationshort}}에서 유지보수하는 컨텍스트의 일부로 저장됩니다. (전송한 컨텍스트 변수가 이미 컨텍스트에 있는 경우, 새 값이 이전에 저장된 값을 겹쳐씁니다.)

  전송하는 컨텍스트 오브젝트가 v2 형식에 부합하는지 확인하십시오. 사용자 애플리케이션에서 전송한 모든 사용자 정의 컨텍스트 변수는 스킬 컨텍스트의 일부여야 합니다. 일반적으로 설정해야 하는 글로벌 컨텍스트 변수는 `system.user_id`가 유일하며 청구 용도로 Plus 및 Premium 플랜에서 사용됩니다. 

- 글로벌 또는 스킬 컨텍스트에서 컨텍스트 변수를 계속 검색할 수 있습니다. 메시지 응답에 `context` 오브젝트를 포함하려면 메시지 입력 옵션에서 **return_context**특성을 사용하십시오. 자세한 정보는 [컨텍스트 데이터 액세스](/docs/services/assistant?topic=assistant-api-client-get-context)를 참조하십시오. 
