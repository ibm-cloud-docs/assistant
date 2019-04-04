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

# 取得 Watson 協助
{: #intent-recommendations}

如果您有來自客戶支援中心的會談日誌記錄（例如，包含實際使用者話語），請讓服務發掘您現有資料中的目的使用者範例候選項。

## 上傳檔案
{: #intent-recommendations-log-files-add}

開始之前，請先建立要提供給服務的檔案。檔案必須是逗點區隔值 (CSV) 檔案，每行一句使用者話語。理想情況下，話語是簡短的詞組，擷取自您的客服中心記錄，其中包含實際的客戶問題和要求。每個使用者話語檔案的大小上限為 20 MB。

請遵循以下其他準則：

  - 從檔案所包含的話語中，移除所有機密資料。

    機密資料包括與可識別的自然人相關的任何資訊，例如，姓名、電子郵件位址及客戶 ID，以及受管制的資料（例如，受保護的健康資訊）。
  - 請不要包括長度超出 1,024 個字元的使用者話語。過長的話語會被截斷。
  - 檔案必須至少包含 100 句話語；含有 500 句以上話語的檔案將會提供更適當的結果。
  - 如果話語包含逗點，請用引號括住該話語。
  - CSV 只能包括一個直欄。
  - 移除所有不是使用者產生的話語，包括任何真人服務專員的回應或附註。

  例如：

  ```
  What happens to my coverage if I trade in my car?
  i'd like to buy a house.
  How do I add a dependent to my plan?
  "first, i want to know if i am already registered."
  ```

您上傳的所有檔案都會在現行服務實例的所有技能間共用。當您要求取得目的使用者範例建議時，會發掘所有可用檔案中的話語。

## 取得目的使用者範例建議
{: #intent-recommendations-get-example-recommendations}

下列視訊提供 2 分鐘概觀，說明如何取得目的使用者範例建議。

<iframe class="embed-responsive-item" id="youtubeplayer" title="目的使用者範例建議" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/L3FI8KeZfsc?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

您上傳的檔案不需要建立範例與目的的關聯。您只要提供原始使用者話語，然後讓服務來選擇適合現行目的的話語。服務會針對每個目的分析相同的資料，以找出適合該目的之使用者範例。

1.  請遵循[建立目的](/docs/services/assistant?topic=assistant-intents#intents-creating-intents-task)中的步驟來建立目的。

1.  至少新增 5 個使用者範例，說明您預期使用者可能說出以觸發此目的之一般話語的完整範圍。

    這些種子使用者範例會教導服務要在您上傳的檔案中尋找的話語類型。

1.  按一下**顯示建議**。

    在服務實例的技能之間會共用使用者範例原始檔。如果您的同事擁有相同實例中的技能並上傳檔案，則他們的檔案會新增至您共用的*使用者範例原始檔* 集合。

1.  **僅限第一次**：按一下**上傳檔案**，然後按一下**選擇檔案**來瀏覽並選取您先前建立的 CSV 檔。

    如果您上傳的檔案包含所有目的類型的話語，也沒關係。服務會知道您正在使用的目的，並尋找適當的範例，以對這個特定目的提供建議。

    在服務上傳及處理檔案之後，會顯示建議的話語。如果未提供任何建議，則該檔案不包含適合此目的的範例。

1.  如果檔案無法為任何目的提供有用的建議，您可以上傳另一個檔案，嘗試一組不同的話語。如需詳細資料，請參閱[管理上傳的檔案](#intent-recommendations-manage-files)。

1.  在服務顯示建議之後，請選取您要新增為此目的之使用者範例的話語，然後按一下**新增**。或者，按**下一組**來檢閱其他話語。
1.  若要自行搜尋使用者範例的 CSV 檔內容，請按一下**搜尋日誌**標籤，輸入要作為搜尋基礎的關鍵字，然後按 **Enter 鍵**。

    請遵循下列搜尋查詢語法準則：

    - 支援布林運算子（例如，`AND` 及 `OR`）。
    - 新增加上引號的文字，以搜尋完全相符的文字 ("thisstringmustbepresent")。
    - 您可以使用正規表示式，例如 `*ly`，來尋找結尾為 `ly` 的所有術語。
    - 下列字元用作正規表示式運算子：

      `+ - = && || > < ! ( ) { } [ ] ^ " ~ * ? : \ /`

      如果您要在搜尋詞彙中包括一個，而不將它作為運算子進行處理，則必須以反斜線 (`\`) 作為它的字首。

您以此方式新增的使用者範例，會計入目的使用者範例總計中，每個方案對此總計都有所限制。如需詳細資料，請參閱[目的限制](/docs/services/assistant?topic=assistant-intents#intents-limits)。

## 管理已上傳的檔案
{: #intent-recommendations-manage-files}

上傳至少一個檔案之後，您可以開啟*使用者範例原始檔* 集合來管理檔案。上傳的檔案會新增至跨現行 {{site.data.keyword.conversationshort}} 服務實例共用的檔案集合。

1.  若要存取集合，請按一下以開啟目的，按一下**顯示建議**，然後從資訊看板中按一下**檢視檔案**。

1.  您可以採取下列動作：

    - 若要新增檔案，請按一下**新增檔案**，然後瀏覽並選取檔案。
    - 若要刪除檔案，您必須移除已從任何與此服務實例相關聯的技能上傳的所有檔案；您不能只刪除一個檔案。首先，確定沒有其他人正在使用這些檔案，然後按一下**全部刪除**來刪除所有已上傳的檔案。

1.  關閉*使用者範例原始檔* 頁面。
