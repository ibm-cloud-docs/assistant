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

# 高度なタスク
{: #logs-resources}

ログ・データのアクセスと分析のために使用できる API および他のツールについて説明します。
{: shortdesc}

## API
{: #logs-resources-api}

`/logs` API を使用して、ユーザーとアシスタントの間で行われた会話の書き起こしデータに含まれるイベントを一覧表示できます。詳細な API リファレンス資料については、[ログ・イベントのリスト ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/apidocs/assistant#list-log-events-in-a-workspace) を参照してください。

ログの保管日数はサービス・プラン・タイプによって異なります。詳しくは、[ログの制限](/docs/services/assistant?topic=assistant-logs#logs-limits)を参照してください。

ログをエクスポートして CSV 形式に変換するための Python スクリプトについては、`export_logs.py` ファイルを [Watson Assistant GitHub ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/watson-developer-cloud/community/blob/master/watson-assistant/export_logs.py) リポジトリーからダウンロードしてください。

## ログ関連の用語
{: #logs-resources-terminology}

まず、{{site.data.keyword.conversationshort}} のログに関する用語の定義を確認してください。

- ***アシスタント***: {{site.data.keyword.conversationshort}} のコンテンツを実装するアプリケーション (チャット・ボットと呼ばれることもあります)。
- ***会話***: 個別ユーザーがアシスタントに送信するメッセージと、アシスタントが返信するメッセージからなる一連のメッセージ。
- ***会話 ID***: 個別のメッセージ呼び出しに追加される固有 ID であり、関連するメッセージ交換同士をリンクします。V1 バージョンのサービス API を使用しているアプリケーション開発者は、context オブジェクトの metadata に ID を含めることで、単一会話内の複数のメッセージ呼び出しにこの値を追加します。
- ***お客様 ID***: お客様データにラベル付けするための固有 ID。このラベル付けにより、お客様が自身のデータの削除を要望した場合に後でお客様データを削除できるようにします。
- ***デプロイメント ID***: V1 バージョンのサービス API を使用しているアプリケーション開発者が各ユーザー・メッセージとともに渡す固有ラベルであり、そのメッセージが作成されたデプロイメント環境を識別するのに役立ちます。
- ***インスタンス***: 固有の資格情報を使用してアクセスできる {{site.data.keyword.conversationshort}} のデプロイメント。 単一の {{site.data.keyword.conversationshort}} インスタンスに複数のアシスタントが含まれていることがあります。
- ***メッセージ***: 1 件のメッセージは、ユーザーがアシスタントに送信する単一の発話です。
- ***スキル ID***: スキルの固有 ID。
- ***ユーザー***: ユーザーは、アシスタントと対話する任意の人物であり、多くの場合はお客様です。
- ***ユーザー ID***: 特定ユーザーのサービス使用量のレベルを追跡するために使用される固有ラベル。
- ***ワークスペース ID***: ワークスペースの固有 ID。 11 月 9 日より前に作成されたワークスペースはスキルとしてツールに表示されますが、スキルとワークスペースは同じものではありません。スキルは実質的に V1 ワークスペースのラッパーです。

**重要**: **「ユーザー ID (User ID)」**プロパティーは**「お客様 ID (Customer ID)」**プロパティーと同等のものでは*ありません* (両方とも当サービスに渡すことは可能ですが)。**「ユーザー ID (User ID)」**フィールドの用途は、請求処理のために使用量のレベルを追跡することであるのに対して、**「お客様 ID (Customer ID)」**フィールドの用途は、エンド・ユーザーに関連付けられたメッセージのラベル付けと後からの削除を可能にすることです。お客様 ID はすべての Watson サービスにまたがって一貫して使用され、`X-Watson-Metadata` ヘッダーで指定されます。ユーザー ID は {{site.data.keyword.conversationshort}} サービスのみで使用され、各 /message API 呼び出しの context オブジェクトに格納して渡されます。

## ユーザー・メトリックの有効化
{: #logs-resources-user-id}

ユーザー・メトリックを使用すると、アシスタントとやり取りしたユニーク・ユーザーの数や、指定した期間中のユーザーあたりの平均会話数などを[「概要 (Overview)」ページ](/docs/services/assistant?topic=assistant-logs-overview)で表示できます。ユーザー・メトリックを有効にするには、固有の `User ID` パラメーターを使用します。

`/message` API を使用して送信されたメッセージの `User ID` を指定するには、次の例のように `user_id` プロパティーを [context ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window} 内の metadata オブジェクト内に含めます。

```
"context" : {
  "metadata": {
       "user_id": "{UserID}"
  }
}
```
{: codeblock}

## 削除のためのメッセージ・データとユーザーの関連付け
{: #logs-resources-customer_id}

場合によっては、{{site.data.keyword.conversationshort}} インスタンスから一連のユーザー・データを完全に削除したいことがあります。削除機能を使用すると、「概要 (Overview)」ページに表示されるメトリックにはそれらの削除されたメッセージは反映されなくなります (例えば、会話総数が少なくなります)。

### 始める前に
{: #logs-resources-delete-customer-id-prereqs}

1 人以上のユーザーのメッセージを削除するには、まず各ユーザーの固有の**お客様 ID **にメッセージを関連付ける必要があります。`/message` API を使用して送信されたメッセージに対して**お客様 ID **を指定するには、`X-Watson-Metadata: customer_id` プロパティーをヘッダーに含めます。複数の**お客様 ID **エントリーを渡すには、次の例のように `customer_id` を使用して、`field=value` のペアをセミコロンで区切って指定します。

```
curl -X POST -u "apikey:3Df... ...Y7Pc9"
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id={first-customer-ID};customer_id={second-customer-ID}'
 --data '{"input":{"text":"hello"}}' 'https:// gateway-us-south.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

`customer_id` ストリングにセミコロン (`;`) や等号 (`=`) の文字を含めることはできません。各 `Customer ID` パラメーターがお客様の間で重複していないことを確認する必要があります。
{: note}

`customer_id` の値を使用してメッセージを削除するには、[機密保護](/docs/services/assistant?topic=assistant-information-security#information-security-gdpr-wa)のトピックを参照してください。

## Jupyter ノートブック
{: #logs-resources-jupyter-notebooks}

IBM が作成した Jupyter ノートブックを使用すると、ログ・データをさらに詳しく分析できます。Jupyter ノートブックは、対話式計算処理のための Web ベースの環境です。データを処理する小規模のコードを実行して、計算結果を即時に表示することができます。 

標準の Python ツールで使用できる一連のノートブックが用意されているとともに、{{site.data.keyword.DSX_full}} で最適に使用できるように設計された一連のノートブックが用意されています。{{site.data.keyword.DSX_short}} は、さまざまな操作を実行するために必要なツールを選択できる環境を提供する製品です。これらの操作としては、データの分析と視覚化、データのクレンジングと加工、ストリーミング・データの取り込み、機械学習モデルの作成、トレーニング、およびデプロイが挙げられます。詳しくは、[製品資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/welcome-main.html){:new_window} を参照してください。

次のノートブックを使用できます。

- **測定 (Measure)**: 適応範囲 (アシスタントがユーザーに応答するのに十分な自信を持っている頻度) と有効性 (アシスタントが応答する際に、応答内容がユーザーのニーズを満たしているかどうか) に重点を置いたメトリックを収集します。

- **有効性 (Effectiveness)**: ログをさらに詳しく分析して、アシスタントを改善するために実行できる手順を理解することを支援します。

[Watson Assistant Continuous Improvement Best Practices Guide ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/common/ssi/cgi-bin/ssialias?htmlfid=54022554USEN&) では、ノートブックを最大限に活用する方法を説明しています。

### {{site.data.keyword.DSX}} でのノートブックの使用
{: #logs-resources-notebooks-studio}

{{site.data.keyword.DSX}} で使用するように設計されたノートブックを使用することを選択する場合は、大まかな手順は次のとおりです。

1.  {{site.data.keyword.DSX}} のアカウントを作成して、[プロジェクトを作成して ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/projects.html?context=analytics){:new_window}、Cloud Object Storage のアカウントをこのプロジェクトに追加します。
1.  {{site.data.keyword.DSX}} のコミュニティーから、[Measure Watson Assistant Performance ノートブック ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")]( https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f635e568) を取得します。
1   ノートブックで提示されている手順に従って、ログ内のダイアログ交換のサブセットを分析します。

    アシスタントの適応範囲と有効性を理解しやすくなるような形で洞察情報が視覚化されます。
1.  効果的でない会話から得られる視覚化のベースとなっているログのサンプル・セットをエクスポートしてから、それらのログを分析して、それらのログに注釈を付けます。

    例えば、応答が正しいかどうかを示します。正しい場合は、応答が有用であるかどうかをマーキングします。応答が正しくない場合は、根本原因を明らかにします。例えば、間違ったインテントやエンティティーが検出されたことや、間違ったダイアログ・ノードがトリガーされたことなどが考えられます。根本原因を特定できたら、正しい選択肢が何であったかを示します。
1.  注釈付きのスプレッドシートを [Analyze Watson Assistant Effectiveness ノートブック](https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f636921c)に提供します。

このプロセスは、アシスタントを改善するために実行できる手順を理解するのに役立ちます。自身が学習した内容をサービス・インスタンスに自動的に適用し戻す方法はありません。システムを改善するために加えた変更を随時記録することで、後でそれらの変更をダイアログ・スキルのトレーニング・データに直接適用できるようにします。

### 標準の Python ツールでのノートブックの使用
{: #logs-resources-notebooks-python}

標準の Python ツールを使用してノートブックを実行することを選択した場合は、[IBM GitHub リポジトリーから](https://github.com/watson-developer-cloud/assistant-improve-recommendations-notebook/tree/master/notebook)ノートブックを取得できます。必ず次の順序でノートブックを実行してください。

1.  Measure Notebook.ipynb
1.  Effectiveness Notebook.ipynb
