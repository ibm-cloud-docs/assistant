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

# カタログの使用
{: #catalog}

***カタログ***を使用すると、一般的なインテントを {{site.data.keyword.conversationshort}} サービス・ワークスペースに簡単に追加できます。
{: shortdesc}

## ワークスペースへのカタログの追加
{: #add-catalog}

{{site.data.keyword.conversationshort}} ツールを使用してカタログを追加します。

1.  {{site.data.keyword.conversationshort}} ツールで、ワークスペースを開き、ナビゲーション・バーの**「カタログ (Catalog)」**タブを選択します。**「カタログ (Catalog)」**が表示されていない場合は、![メニュー](images/Menu_16.png) メニューを使用してページを開きます。

1.  *「請求処理 (Billing)」*などのカタログを選択して、そのカタログに提供されているインテントを表示します。

    ![使用可能なカタログを示す画面キャプチャー](images/catalog_overview.png)

    *「請求処理 (Billing)」*カテゴリー内で定義されているインテントに関する情報が表示されます。

    ![「請求処理 (Billing)」カテゴリーのインテントを示す画面キャプチャー](images/catalog_open.png)

    特定のカタログから追加したインテントは、命名規則によって他のインテントと区別できます。この例では、`#Billing_ . . .` という名前にしています。

1.  ![閉じる矢印](images/close_arrow.png) を選択して、**「カタログ (Catalog)」**タブに戻ります。

1.  次に、*「ボットに追加 (Add to bot)」* ボタンをクリックして、`「請求処理 (Billing)」`カタログをワークスペースに追加します。*「請求処理 (Billing)」*インテントがワークスペースに追加されたことを示すメッセージが表示されます。

    ![「ボットに追加 (Add to bot)」ボタンを示す画面キャプチャー](images/catalog_addtobot.png)

1.  次に、**「インテント (Intents)」**タブを選択し、*「請求処理 (Billing)」*インテントがワークスペースに追加されていることを確認します。

    ![「インテント (Intents)」タブに「請求処理 (Billing)」インテントがリストされていることを示す画面キャプチャー](images/catalog_intents.png)

### 結果

*「請求処理 (Billing)」*カタログからのインテントがワークスペースの**「インテント (Intents)」**タブに追加され、新しいデータについてのシステムのトレーニングが開始されます。

## カタログの例の編集

他のインテントと同様に、*「請求処理 (Billing)」*カタログのインテントがワークスペースに追加されたら、以下の変更を加えることができます。

- インテントの名前を変更する。
- インテントを削除する。
- 例を追加、編集、または削除する。
- 例を別のインテントに移動する。

インテント名からそれぞれの例にタブで移動して選択すれば、例を編集できます。

例を移動したり削除したりするには、チェック・ボックスを選択してその例を選択してから**「移動 (Move)」**または**「削除」**を選択します。

  ![例の移動または削除の方法を示す画面キャプチャー](images/catalog_edit.png)
