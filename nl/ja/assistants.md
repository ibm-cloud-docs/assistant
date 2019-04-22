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

# アシスタント
{: #assistants}

アシスタントは、ビジネス・ニーズに合わせてカスタマイズし、必要な場面で顧客にヘルプを提供するために複数のチャネルに展開できるコグニティブ・ボットです。
{: shortdesc}

![スキル](images/skill-icon.png)  顧客の目標を達成するために必要なスキルを追加することで、アシスタントをカスタマイズします。

顧客が一般にサポートを求める質問や要求事項を理解して、それに対処することができる対話スキルを追加します。ユーザーの質問の対象の事象やタスクと、それらについてのユーザーの質問の内容に関する情報を提供すると、サービスは、同じ類似したユーザー要求を理解するように調整された機械学習モデルを動的に構築します。

| ダイアログ・ツリー |グラフィカル・ユーザー・インターフェース|
|-------------|-------------------------:|
| グラフィカル・ツールを使用して、アシスタントがユーザーと対話するときに読み取るダイアログ、実際の会話をシミュレートするダイアログを作成できます。ダイアログは、認識するように学習させられた一般的な顧客目標に基づいて、有用な応答を返します。| ![サンプル・コンテンツを含むサンプル・ダイアログ・ツリー](images/dialog-depiction.png) |

ダイアログ・スキル自体はテキストで定義されていますが、これを Watson Speech to Text サービスや Watson Text to Speech サービスと統合して、ユーザーが口頭でアシスタントと対話できるようにすることができます。

![すぐに使用できるトレーニング・データ](images/oob.png)  すぐに作業を開始したい場合は、アシスタントが基礎的なデータで顧客のサポートを開始できるように、事前作成されたトレーニング・データをダイアログ・スキルに追加してください。

![IBM Cloud](images/cloud.png)  アシスタントは、{{site.data.keyword.cloud_notm}} によって管理される完全ホスト型ボットです。つまり、これをサポートするためのインフラストラクチャーの設定や保守について心配する必要はありません。

| 統合       |チャネル|
|--------------------|:----------|
| アシスタントは、Slack や Facebook Messenger などの既存のメッセージング・チャネルを含む複数のインターフェースを使用して、わずか数ステップでデプロイできます。または、アシスタントを取り込んだカスタム・アプリケーションを設計する場合は、基礎 API を直接呼び出して設計できます。| ![Slack、Facebook Messenger、Web アプリケーション、ヒューマン・エージェント統合などの統合方法](images/integrations.png) |


開始するには、[アシスタントの作成](/docs/services/assistant?topic=assistant-assistant-add)を参照してください。
