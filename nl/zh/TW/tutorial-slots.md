---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-12"

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

# 指導教學：將含空位的節點新增至對話
{: #tutorial-slots}

在本指導教學中，您將在對話節點中新增空位，以收集單一節點內來自使用者的多個資訊片段。您建立的節點將會收集預約餐廳所需的資訊。
{: shortdesc}

## 學習目標
{: #tutorial-slots-objectives}

在完成本指導教學時，您將瞭解如何執行下列動作：

- 定義對話所需的目的及實體
- 將空位新增至對話節點
- 測試含空位的節點

### 持續期間
{: #tutorial-slots-duration}

本指導教學大約需要 30 分鐘才能完成。

### 必要條件
{: #tutorial-slots-prereqs}

開始之前，請完成[入門指導教學](/docs/services/assistant?topic=assistant-getting-started)。您將使用您所建立的 {{site.data.keyword.conversationshort}} 指導教學技能，並將節點新增至您在開始使用練習時建置的簡易對話中。

如果願意，您也可以從新的對話技能開始。只需要確定在開始本指導教學之前先建立技能即可。
{: note}

## 步驟 1：新增目的及範例
{: #tutorial-slots-add-intent}

請在「目的」標籤上新增目的。目的是使用者輸入中表達的用途或目標。您將新增 #reservation 目的，以辨識指出使用者想要預約餐廳的使用者輸入。

1.  從指導教學技能的**目的**頁面，按一下**新增目的**。
1.  新增下列目的名稱，然後按一下**建立目的**：

    ```json
    reservation
    ```
    {: screen}

    已新增 #reservation 目的。將 `#` 記號加在目的名稱前面，將它標示為目的。此命名慣例可協助您及其他人將目的辨識為目的。它還沒有與其相關聯的範例使用者詞語。
1.  在**新增使用者範例**欄位中，鍵入下列詞語，然後按一下**新增範例**：

    ```json
    i'd like to make a reservation
    ```
    {: screen}

1.  新增下列其他範例，協助 Watson 辨識 `#reservation` 目的。

    ```json
    I want to reserve a table for dinner
    Can 3 of us get a table for lunch?
    do you have openings for next Wednesday at 7?
    Is there availability for 4 on Tuesday night?
    i'd like to come in for brunch tomorrow
    can i reserve a table?
    ```
    {: screen}

1.  按一下**關閉** ![關閉箭頭](images/close_arrow.png) 圖示，以完成新增 `#reservation` 目的及其範例詞語。

## 步驟 2：新增實體
{: #tutorial-slots-add-entity}

實體定義包含一組實體*值*，代表給定目的環境定義中常用的詞彙。定義實體，即可協助助理在使用者輸入中，識別與感興趣之目的相關的參照。在此步驟中，您將啟用可辨識時間、日期及數字參照的系統實體。

1.  按一下**實體**，以開啟「實體」頁面。
1.  啟用可在使用者輸入中辨識日期、時間及數字參照的系統實體。按一下**系統實體**標籤，然後開啟這些實體：

    - `@sys-time`
    - `@sys-date               `
    - `@sys-number               `

您已順利啟用 @sys-date、@sys-time 及 @sys-number 系統實體。您現在可以在對話中使用它們。

## 步驟 3：新增含空位的對話節點
{: #tutorial-slots-add-dialog-with-slots}

對話節點代表助理與使用者之間的對話執行緒的開始。它包含助理處理節點之前必須符合的條件。它最少也會包含回應。例如，節點條件可能會在使用者輸入中尋找 `#hello` 目的，並回應 `Hi. How can I help you?`。此範例是最簡單形式的對話節點，其中包含單一條件及單一回應。將條件式回應新增至單一節點、新增可延長與使用者交流的子節點等等，即可定義複雜對話。（如果您要進一步瞭解複雜對話，則可以完成[建置複雜對話](/docs/services/assistant?topic=assistant-tutorial)指導教學。）

您將在此步驟中新增的節點，是包含空位的節點。空位提供結構化的格式，您可以透過結構化格式詢問及儲存單一節點內來自使用者的多個資訊片段。它們最適合的情況是您心目中有特定作業，而且需要有使用者的重要資訊片段才能執行該作業。如需相關資訊，請參閱[使用空位收集資訊](/docs/services/assistant?topic=assistant-dialog-slots)。

您新增的節點將會收集預約餐廳所需的資訊。

1.  按一下**對話**標籤，以開啟對話樹狀結構。
1.  按一下 **#General_Greetings** 節點上的「其他」圖示 ![其他選項](images/kabob.png)，然後選取**在下面新增節點**。
1.  在條件欄位中開始鍵入 `#reservation`，然後從清單中進行選取。
    如果使用者輸入符合 `#reservation` 目的，則會評估此節點。
1.  按一下**自訂**，按一下**空位**切換開關予以**開啟**，然後按一下**套用**。

    ![顯示您開啟空位的「自訂」對話框。](images/slots-toggle-on.png)
1.  定義下列空位：

    <table>
    <caption>空位詳細資料</caption>
    <tr>
      <th>檢查</th>
      <th>儲存為</th>
      <th>如果不存在，請詢問</th>
    </tr>
    <tr>
      <td>@sys-date               </td>
      <td>$date</td>
      <td>What day would you like to come in?</td>
    </tr>
    <tr>
      <td>@sys-time</td>
      <td>$time</td>
      <td>What time do you want the reservation to be made for?</td>
    </tr>
    </tr>
    <tr>
      <td>@sys-number               </td>
      <td>$guests</td>
      <td>How many people will be dining?</td>
    </tr>
    </table>

1.  回應時，請指定 `OK. I am making you a reservation for $guests on $date at $time.`。

    ![顯示當依指定填入時，每個空位及回應在工具中的樣子。](images/slots-simple-node.png)

1.  按一下 ![關閉](images/close.png)，以關閉節點編輯視圖。

## 步驟 4：測試對話
{: #tutorial-slots-test}

1.  選取 ![詢問 Watson](images/ask_watson.png) 圖示，以開啟聊天窗格。
1.  鍵入 `i want to make a reservation`。

    助理會辨識 #reservation 目的，並以第一個空位的提示 `What day would you like to come in?` 作為回應。

1.  鍵入 `Friday`。

    助理會辨識該值，並使用它來填入第一個空位的 $date 環境定義變數。然後，它會顯示下一個空位 `What time do you want the reservation to be made for?` 的提示。

1.  鍵入 `5pm`。

    助理會辨識該值，並使用它來填入第二個空位的 $time 環境定義變數。然後，它會顯示下一個空位 `How many people will be dining?` 的提示。

1.  鍵入 `6`。

    助理會辨識該值，並使用它來填入第三個空位的 $guests 環境定義變數。既然已填入所有空位，所以會顯示節點回應 `OK. I am making you a reservation for 6 on 2017-12-29 at 17:00:00.`。

![顯示「試用」窗格中已順利填入節點空位的測試對話。](images/slots-test-simple-node.png)

正常運作！恭喜！您已順利建立含空位的節點。

## 摘要
{: #tutorial-slots-summary}

在本指導教學中，您已建立含空位的節點，可擷取在餐廳訂位所需的資訊。

## 後續步驟
{: #tutorial-slots-next-steps}

改善與節點互動的使用者體驗。完成後續指導教學：[改善含空位的節點](/docs/services/assistant?topic=assistant-tutorial-slots-complex)。它會涵蓋簡單改善，例如，如何重新格式化系統所傳回的日期 (2017-12-28) 及時間 (17:00:00) 值。它也涵蓋更複雜的作業，例如，使用者未提供對話預期空位要具有的值類型時要執行的作業。
