---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-11"

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
{:gif: data-image-type='gif'}

# 更正使用者輸入
{: #beta-spell-check}

此特性僅供測試版程式的參與者使用。若要瞭解如何要求存取權，請參閱[參與測試版程式](/docs/services/assistant?topic=assistant-feedback#feedback-beta)。

![測試版](images/beta.png) IBM 發行分類為測試版的評估的服務、特性及語言支援。這些特性可能不穩定、經常變更，且可能接到簡短通知就中斷服務。測試版特性也可能未提供正式發行特性所提供但不要用於正式作業環境的相同層次的效能或相容性。

啟用*拼字檢查* 特性，以修正使用者在其提交作為使用者輸入之話語中所造成的拼字錯誤。啟用拼字檢查時，會自動更正拼錯的單字。它是用來評估輸入的已更正單字。提供更精確的輸入時，服務會更常辨識實體提及項目，並瞭解使用者的目的。

目前，此設定只能用於英文語言對話技能。
{: note}

一旦啟用拼字檢查，就會以下列方式更正使用者輸入：

- 原始輸入：`letme applt for a memberdhip`
- 已更正的輸入：`let me apply for a membership`

當服務評估是否要更正單字的拼寫時，它不是根據簡單的字典查閱處理程序。相反地，它會使用「自然語言處理程序」和機率模型的組合，來評量詞彙實際上是否拼錯，應該予以更正。

## 啟用拼字檢查
{: #beta-spell-check-enable}

若要啟用拼字檢查特性，請完成下列步驟：

1.  從「技能」頁面中，尋找您的技能。
1.  按一下 ![開啟及關閉選項清單](images/kabob-beta.png) 圖示，然後選擇**設定**。
1.  從*拼字檢查* 設定頁面中，開啟**拼字檢查自動更正**。

### 測試拼字更正
{: #beta-spell-check-test}

1.  從「試用」窗格中，提交包含一些拼字錯誤的話語。

    如果輸入中的單字拼錯，則會自動予以更正，並顯示 ![自動更正](images/auto-correct.png) 圖示。已更正的話語會畫上底線。
1.  將游標移至畫底線的話語上方來查看原始用字。

如果您預期有需要服務更正的拼字錯誤詞彙，但沒有，請檢閱服務用來決定是否要更正單字的規則，以查看該單字是否屬於服務故意不變更的單字種類中。

為了避免過度更正，服務不會更正下列輸入類型的拼字：

- 大寫單字
- 表情符號
- 位置實體，例如州/省及地址
- 度量或時間的數字及單位
- 適當的名詞，例如常用的名字或公司名稱
- 引號內的文字
- 包含特殊字元的單字，例如連字號 (-)、星號 (*) 和 & 符號
- *屬於* 此技能的單字，表示這些單字具有隱含的重要性，因為它們出現在實體值、實體同義字或目的使用者範例中。

如果未更正的單字未明顯地屬於其中一種輸入類型，則可能值得檢查實體是否已對它啟用模糊符合。

#### 拼字更正與模糊符合有何相關？
{: #beta-spell-check-vs-fuzzy-matching}

模糊符合可協助服務在使用者輸入中辨識字典型實體提及項目。它會使用字典查閱方法，將使用者輸入中的單字與技能訓練資料中的現有實體值或同義字進行比對。例如，如果使用者輸入 `books`，而且您的訓練資料包含實體同義字 `book`，則模糊符合會認識到這兩個詞彙表示相同的事物。

啟用拼字檢查和模糊符合時，會先執行模糊符合功能，然後再觸發拼字檢查。如果它找到一個詞彙，它符合現有字典實體值或同義字，則會將這個詞彙新增至*屬於* 技能的單字清單中，因此不會予以更正。同樣地，如果使用者輸入類似 `I want to buy a boook` 的句子，模糊符合會認識到詞彙 `boook` 表示與實體同義字 `book` 相同的事物，並將它新增至受保護的單字清單。因此，服務*不會* 更正 `boook` 的拼字。

#### 運作方式
{: #beta-spell-check-how-it-works}

一般來說，使用者輸入會依現狀儲存在訊息 `input` 物件的 `text` 欄位中。若且只有在以某種方式更正使用者輸入時，才會在 `input` 物件中建立新欄位，稱為 `original_text`。此欄位會儲存使用者的原始輸入，其中包含任何拼錯的單字。已更正的文字則會新增至 `input.text` 欄位。
