---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-07"

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

# 支援的語言
{{site.data.keyword.conversationshort}} 服務支援列出的語言。每一種語言都或多或少支援服務的個別特性。
{: shortdesc}

在下表中，語言及特性支援的層次由下列程式碼指出：

- **GA 版** - 此特性已正式發行並支援該語言。請注意，特性即使在正式發行之後，仍然可能會繼續進行更新。
- **測試版** - 此特性僅支援作為「測試版」，而且在正式發行該語言版本之前仍在進行測試。
- **空白** - 沒有任何代碼表示此特性不支援該語言。

|                  | **[定義目的](intents.html)**、**[實體](entities.html)**及**[對話](dialog-build.html)** | **[絕對評分及「標示為不相關」](intents.html#mark-irrelevant)** | **系統實體（[數字](system-entities.html#sys-number)、[貨幣](system-entities.html#sys-currency)、[百分比](system-entities.html#sys-percentage)、[日期和時間](system-entities.html#sys-datetime)）** | **[實體模糊符合](entities.html#fuzzy-matching)** |
|:---|:---:|:---:|:---:|:---:|
| **英文 (en)**                      | GA 版 | GA 版 | GA 版 </br> 測試版（[位置](system-entities.html#sys-location)、[人員](system-entities.html#sys-person)）| 測試版（詞幹分析、拼字錯誤及部分符合）|
| **阿拉伯文 (ar)**                  | GA 版 | 測試版| 測試版| 測試版（僅限拼字錯誤）|
| **簡體中文 (zh-cn)**               | 測試版| 測試版| 測試版|  |
| **繁體中文 (zh-tw)**               | 測試版| 測試版|  |  |
| **捷克文 (cs)**                    | 測試版| 測試版| 測試版| 測試版（僅限拼字錯誤）|
| **荷蘭文 (nl)**                    | 測試版| 測試版| 測試版|  |
| **法文 (fr)**                      | GA 版 | GA 版 | GA 版 | 測試版（僅限拼字錯誤）|
| **德文 (de)**                      | GA 版 | GA 版 | GA 版 | 測試版（僅限拼字錯誤）|
| **義大利文 (it)**                  | GA 版 | GA 版 | GA 版 | 測試版（僅限拼字錯誤）|
| **日文 (ja)**                      | GA 版 | GA 版 | GA 版 | 測試版（僅限拼字錯誤）|
| **韓文 (ko)**                      | GA 版 | GA 版 | GA 版 | 測試版（僅限拼字錯誤）|
| **葡萄牙文（巴西）(pt-br)**        | GA 版 | GA 版 | GA 版 | 測試版（僅限拼字錯誤）|
| **西班牙文 (es)**                  | GA 版 | GA 版 | GA 版 | 測試版（僅限拼字錯誤）||

**附註：**{{site.data.keyword.conversationshort}} 服務如所述支援多種語言，但工具介面本身（說明、標籤等）為英文。所有支援的語言都可以透過英文介面輸入及訓練。

**GB18030 法規遵循**：GB18030 是指定用於中文市場之擴充碼頁面的中文標準。此字碼頁標準對於軟體產業而言十分重要，因為 China National Information Technology Standardization Technical Committee 已管制 2001 年 9 月 1 日之後針對中文市場發行的任何軟體應用程式啟用 GB18030。{{site.data.keyword.conversationshort}} 服務支援此編碼，而且符合官方認證的 GB18030 標準

## 變更工作區語言

在建立工作區之後，即無法修改其語言。如果需要變更工作區的支援語言，使用者應該下載工作區。然後，在文字編輯器中編輯產生的 JSON 檔案，並搜尋稱為 `language` 的 JSON 內容。

`language` 內容應該設為工作區的原始語言；例如，英文會是 `en`。修改此內容的值，並將它變更為所要的語言（`fr` 代表法文，`de` 代表德文等）。儲存對 JSON 檔案的變更，並將修改過的檔案匯入至 {{site.data.keyword.conversationshort}} 服務實例。

## 配置雙向語言
{: #configuring-bi-directional}

針對雙向語言（例如阿拉伯文），您可以相應地變更工作區喜好設定。從工作區標籤中，選取*動作* 下拉功能表，然後選取**雙向喜好設定**（此選項只適用於設為雙向語言的工作區）：

![雙向喜好設定](images/bidi_prefs.png)

從工作區的下列選項中進行選取：

- **GUI 方向**：指定圖形使用者介面中元素（例如按鈕或功能表）的佈置方向。選擇 `LTR`（從左到右）或 `RTL`（從右到左）。如果未指定，工具會遵循 Web 瀏覽器的 GUI 方向設定。
- **文字方向**：指定所鍵入文字的方向。選擇 `LTR`（從左到右）或 `RTL`（從右到左），或者選取 `Auto`，以根據系統設定自動選擇文字方向。`None` 選項將會顯示從左到右的文字。
- **數值整理**：指定要在呈現一般數字時使用的數字形式。選擇 `Nominal`、`Arabic-Indic` 或 `Arabic-European`。`None` 選項將會顯示西方數字。
- **行事曆類型**：指定如何在工作區使用者介面中選擇過濾日期。選擇 `Islamic-Civil`、`Islamic-Tabular`、`Islamic-Umm al-Qura` 或 `Gregorian`。**附註**：此設定不適用於「試用」畫面。

![雙向選項](images/bidi_opts.png)

在完成選擇後，請按一下**更新**以儲存並回到工作區標籤。

## 使用重音字元
{: #working-with-accents}

在交談式設定中，與 {{site.data.keyword.conversationshort}} 服務互動時，使用者不一定會使用重音符號。因此，在進行目的偵測及實體辨識時，會將重音及無重音版本的單字視為相同。

不過，針對部分語言（例如西班牙文），有些重音符號可能會變更實體的意義。因此，進行實體偵測時，雖然原始實體可能隱含地具有重音符號，但是服務也可能符合相同實體的非重音版本，但具有略低的信任評分。

例如，針對具有重音符號並對應於動詞 "barrer"（清理）過去式的 "barrió" 單字，服務也可能符合 "barrio"（鄰近地區）單字，但具有略低的信任。

系統將提供具有完全相符項之實體中的最高信任評分。例如，如果 `barrió` 位於訓練集中，則偵測不到 `barrio`；而如果 `barrio` 位於訓練集中，則偵測不到 `barrió`。

您應該使用適當的字元及重音符號來訓練系統。例如，如果您預期 `barrió` 作為回應，則應該將 `barrió` 放入訓練集中。

雖然不是重音符號，但是同樣適用於使用西班牙文字母 `ñ` 與字母 `n` 的這類單字，例如 "uña" 與 "una"。在此情況下，字母 `ñ` 不只是具有重音符號的 `n`，還是唯一的西班牙文特有字母。

如果您認為客戶不會使用適當的重音符號或錯字（例如 including，放置 `n`，而非 `ñ`），則可以啟用模糊符合，或者，您可以在訓練範例中明確包含它們。
