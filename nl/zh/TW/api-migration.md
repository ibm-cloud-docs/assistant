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

# 移轉至第 2 版 API
{: #api-migration}

Assistant 第 2 版運行環境 API 已在 2018 年 11 月引進，支援使用助理和技能。此 API 所提供的顯著優勢優於第 1 版運行環境 API，包括自動狀態管理、輕鬆部署、技能版本化以及新特性的可用性（如搜尋技能）。

第 2 版 API 可供所有使用者使用，與服務方案無關，也不需要額外費用。

第 2 版 API 目前僅支援與現有助理的運行環境互動。用於建立或修改工作區的編寫應用程式應該繼續使用第 1 版 API。
{: note}

## 概觀

使用第 2 版 API，用戶端應用程式可以與助理進行通訊，而不是直接與工作區進行通訊。助理是一個提供多種新功能的新編排層，包括自動狀態管理、技能版本化、更輕鬆的部署以及（適用於加值和超值方案）搜尋技能。現有工作區（現在稱為_對話技能_）會繼續像以前一樣地運作，但新的助理層提供新功能。

與助理的所有通訊都在_階段作業_ 的環境定義中進行，這會使其在階段作業期間保持階段作業狀態。{{site.data.keyword.conversationshort}} 會自動儲存狀態資料（包括對話或用戶端應用程式定義的任何環境定義變數），而不需要應用程式方面執行任何動作。

狀態資料會持續保存，直到您明確刪除階段作業，或者由於沒有活動而導致階段作業逾時為止。

如果您的現有應用程式使用第 1 版 API 將使用者輸入直接傳送到工作區，則移轉應用程式以使用第 2 版 API 是一個簡單的處理程序。

## 設定助理

第 2 版運行環境 API 會將訊息傳送給助理，而助理會將訊息遞送至對話技能（先前稱為工作區）。若要設定助理，請使用 {{site.data.keyword.conversationshort}} 使用者介面：

1. 按一下**技能**標籤。驗證工作區是否顯示為可用技能（服務實例的所有現有工作區都會自動轉換為 {{site.data.keyword.conversationshort}} 使用者介面中的技能。此轉換不會對基礎工作區進行任何變更）。

1. 按一下**助理**標籤。按一下**建立助理**以建立新的助理。系統提示新增技能時，請按一下**新增對話技能**，並選取對應於工作區的對話技能。

  如需建立助理的相關資訊，請參閱[建立助理](https://cloud.ibm.com/docs/services/assistant?topic=assistant-assistant-add)。

1. 建立新的助理後，請按一下 ![功能表](images/kebab-react.png) 功能表，然後選取**設定**。

1. 在**助理設定**頁面上，尋找助理 ID。應用程式將使用此 ID（而不是工作區 ID）來與助理通訊。第 1 版及第 2 版 API 的服務認證相同。

  目前，不支援使用 API 來擷取助理 ID。若要尋找助理 ID，您必須使用 {{site.data.keyword.conversationshort}} 使用者介面。
  {: note}

## 呼叫第 2 版運行環境 API

建立助理後，可以更新用戶端應用程式以使用第 2 版運行環境 API，而不使用第 1 版運行環境 API。

1. 在階段作業中傳送第一則訊息之前，請使用第 2 版[**建立階段作業**](https://cloud.ibm.com/apidocs/assistant-v2#create-a-session){: external}方法來建立階段作業。請儲存傳回的階段作業 ID：

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

1. 使用第 2 版[**將使用者輸入傳送給助理**](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant){: external}方法，將使用者輸入傳送給助理。請指定助理 ID 和階段作業 ID，而不要像使用第 1 版 API 那樣地指定工作區 ID：

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

  基本訊息結構未變更；尤其是，使用者輸入仍作為 `input.text` 傳送。

1. 階段作業結束後，請使用第 2 版[**刪除階段作業**](https://cloud.ibm.com/apidocs/assistant-v2#delete-session){: external}方法來刪除階段作業。

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

   如果您未明確刪除階段作業，則系統將在配置的逾時間隔後自動刪除該階段作業（逾時持續時間取決於方案；如需相關資訊，請參閱[階段作業限制](/docs/services/assistant?topic=assistant-assistant-settings#assistant-settings-session-limits)）。

若要查看簡單用戶端應用程式環境定義中第 2 版 API 的範例，請參閱[建置用戶端應用程式](/docs/services/assistant?topic=assistant-api-client)。

## 處理第 2 版回應格式

應用程式可能需要更新才能處理第 2 版運行環境回應格式，具體取決於應用程式需要存取回應的哪些部分：

- 所有回應類型（例如 `text` 和 `option`）的輸出都還是透過 `output.generic` 物件傳回。用於處理這些回應的應用程式碼應該不需要修改就能正常運作。

- 現在，偵測到的目的和實體會作為 `output` 物件的一部分傳回，而不是在回應 JSON 的根傳回。

- 現在，交談環境定義組織成兩個物件：

  - **廣域環境定義**包含助理所使用的所有技能共用的系統層次環境定義資料。

  - **技能環境定義**包含對話技能所使用的任何使用者定義環境定義變數。

  不過，請記住，狀態資料（包括交談環境定義）現在由助理進行維護，因此應用程式可能根本不需要存取環境定義（請參閱[讓助理維護狀態](#api-migration-state)）。

如需第 2 版回應格式的完整文件，請參閱第 2 版 [API 參考資料](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant){: external}。

## 讓助理維護狀態
{: #api-migration-state}

現在，可以移除大多數應用程式為了維護狀態而包含的任何程式碼。不再需要儲存環境定義以及針對每輪交談將其送回 {{site.data.keyword.conversationshort}}。環境定義將由 {{site.data.keyword.conversationshort}} 自動維護，並且對話可以像以前一樣存取環境定義。

請注意，使用第 2 版 API 時，依預設不會包含環境定義，以回應用戶端應用程式。不過，必要的話，程式碼仍然可以存取環境定義變數：

- 您仍然可以將 `context` 物件作為訊息輸入的一部分傳送。包含的任何環境定義變數都會作為 {{site.data.keyword.conversationshort}} 維護的環境定義的一部分儲存（如果環境定義中已存在傳送的環境定義變數，則新值會改寫先前儲存的值）。

  請確定傳送的 context 物件符合第 2 版格式。應用程式傳送的所有使用者定義環境定義變數都應該是技能環境定義的一部分；通常，可能需要設定的唯一廣域環境定義變數是 `system.user_id`，該變數由加值和超值方案用於計費目的。

- 您仍然可以從廣域或技能環境定義中擷取環境定義變數。若要將 `context` 物件包含在訊息回應中，請使用訊息輸入選項中的 **return_context** 內容。如需相關資訊，請參閱[存取環境定義資料](/docs/services/assistant?topic=assistant-api-client-get-context)。
