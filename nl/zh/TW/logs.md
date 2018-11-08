---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-13"

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

# 關於改善元件

{{site.data.keyword.conversationshort}} 的「改善」元件提供與工作區使用者的互動歷程。您可以使用此歷程來改善工作區對使用者輸入的瞭解。
{: shortdesc}

**改善**畫面的各區段：

* [概觀](logs_oview.html)：使用者與工作區互動的摘要。
* [使用者交談](logs_convo.html)：使用者詞語清單。您可以在檢視個別使用者詞語時更新目的及實體。**附註**：單一使用者交談可能包含多個詞語。
* [建議](logs_recommend.html)：系統改善方式。僅適用於「進階」使用者。

## 跨工作區改善
{: #deploy_id}

若要瞭解如何使用詞語資料來跨工作區進行改善，則最好檢閱下列與 {{site.data.keyword.conversationshort}} 服務相關聯的定義：

* ***實例***：您的 {{site.data.keyword.conversationshort}} 部署，可使用唯一的認證來進行存取。{{site.data.keyword.conversationshort}} 實例可能是由多個工作區所構成。
* ***工作區***：工作區是 {{site.data.keyword.conversationshort}} 內容的一個模型；通常，這相當於機器人
* ***工作區 ID***：工作區的唯一 ID
* ***部署 ID***：部署 ID 是與使用者詞語一起傳遞的唯一標籤，協助識別詞語來自的部署環境
* ***詞語***：詞語是使用者傳送給工作區的單一訊息

建立工作區是反覆運算極頻繁的處理程序。開發工作區時，請使用*試用* 窗格來驗證工作區辨識測試輸入中的正確目的及實體，並視需要進行更正。

在**改善**畫面中，您可以檢視與使用者的實際互動資訊，並進行類似的更正，以改善工作區辨識目的及實體的正確性。很難確切得知使用者*如何* 詢問問題，或他們可能產生的隨機詞語，因此請務必頻繁造訪**改善**畫面，以改善工作區。

針對包含多個工作區的 {{site.data.keyword.conversationshort}} 實例，有時最好使用某個工作區中的詞語資料來改善該相同實例內的另一個工作區。**附註**：如果您是「{{site.data.keyword.conversationshort}} 超值」使用者，則超值實例可以選擇性地配置成容許跨不同的超值實例來存取工作區中的日誌資料。

例如，假設您有名為 *HelpDesk* 的 {{site.data.keyword.conversationshort}} 實例。在 HelpDesk 實例中，您可能同時會有「正式作業」工作區及「開發」工作區。使用「開發」工作區時，您可以根據`部署 ID` 來過濾「正式作業」工作區的詞語，因此您將使用「正式作業」工作區詞語來改善「開發」工作區。

![資料來源鏈結](images/data_source_1.png)

任何您在「開發」工作區內進行的編輯都只會影響「開發」工作區，即是您是使用「正式作業」工作區詞語。如需相關資訊，請參閱[選取資料來源](logs_convo.html#select-source)。

若要指定使用 `/message` API 所傳送之詞語的部署 ID，請將 meta 資料物件內的部署內容包含在[環境定義 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#send_message){: new_window} 中，如此範例所示：

```
"context" : {
            "username" : "jane_doe@myemail.com",
  "member_type" : "gold",
  "metadata" : {
       "deployment": "HelpDesk-Production"
  }
}
```
{: #codeblock}
