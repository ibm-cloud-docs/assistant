---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-26"

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

# 製品情報
{: #index}

{{site.data.keyword.conversationfull}} サービスを使用すると、自然言語の入力を理解し、機械学習を使用して人間同士の会話を模した方法でお客様に応答するソリューションを構築できます。
{: shortdesc}

## 処理の流れ

この図は完全なソリューションのアーキテクチャー全体を示します。![サービスのフロー図](images/conversation_arch_overview.png)

- 実装したユーザー・**インターフェース**を介して、ユーザーはアプリケーションと対話します。 例えば、シンプルなチャット・ウィンドウやモバイル・アプリ、あるいは音声インターフェースを備えたロボットなどです。

- **アプリケーション**はユーザー入力を {{site.data.keyword.conversationshort}} サービスに送信します。
    - アプリケーションは*ワークスペース*に接続します。ワークスペースはダイアログ・フローとトレーニング・データのコンテナーです。
    - サービスはユーザー入力を解釈し、会話のフローを指図し、必要な情報を収集します。
    - 追加の {{site.data.keyword.watson}} サービス ({{site.data.keyword.toneanalyzershort}} や {{site.data.keyword.speechtotextshort}} など) を接続してユーザー入力を分析できます。

- アプリケーションはユーザーのインテントおよび追加情報に基づいて、 **バックエンド・システム**と対話します。 例えば、質問に答える、チケットをオープンする、アカウント情報を更新する、注文を行う、などです。 実行できるものに制限はありません。

## 実装

ここでは会話を実装する方法について説明します。

- **ワークスペースを構成します。** 使いやすいグラフィカル環境で、会話のためのトレーニング・データとダイアログをセットアップします。

    トレーニング・データは次の成果物で構成されます。
    - **インテント**: ユーザーがサービスと対話したときに得ると予想される目標です。 ユーザー入力で識別可能なそれぞれの目標に対して 1 つのインテントを定義します。 例えば、店の営業時間についての質問に答える *store_hours* という名前のインテントを定義できます。 各インテントについて、「`何時に開店しますか?`」など、お客様が必要とする情報を尋ねるために、使用する可能性がある入力を反映したサンプル発話を追加します。
    - **エンティティー**: 「エンティティー」はインテントのコンテキストを提供する用語またはオブジェクトを表します。 例えば、お客様が開店時間を知ろうとする店を区別するためにダイアログで役に立つ都市名などのエンティティーが考えられます。

      トレーニング・データを追加するにつれ、自然言語分類子が自動的にワークスペースに追加され、サービスがお客様の要求を聞き、応答しなければならないと指示されたタイプの要求を理解するためのトレーニングが行われます。

    ダイアログ・ツールを使用して、インテントとエンティティーを取り込むダイアログ・フローを構築します。 ダイアログ・フローはツール内でグラフィカルなツリー構造で表されます。 サービスで扱う各インテントを処理するために、ブランチを追加できます。 その後に、ユーザー入力内で検出されたエンティティーや、構築したアプリケーションや他の外部サービスからサービスに渡された情報などの他の要因に基づいて、考えられる多くの要求の組み合わせを処理するためのブランチ・ノードを追加できます。

- **ワークスペースをデプロイします。** 構成したワークスペースをフロントエンド・インターフェース、ソーシャル・メディア、またはメッセージング・チャネルに接続してユーザーにデプロイします。 {{site.data.keyword.conversationshort}} サービスのデプロイ済みのインスタンスは、IBM クラウド・コンピューティング・プラットフォームの {{site.data.keyword.cloud_notm}} でホスティングされます。(詳しくは、[プラットフォームの概要 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/docs/overview/ibm-cloud.html#overview) を参照してください。)

これらの実装ステップについて詳しくは、以下のリンクを参照してください。

- [インテントとエンティティーの計画をたてる](intents-entities.html#planning-your-entities)
- [ダイアログの概要](dialog-overview.html)
- [デプロイメントの概要](deploy.html)

## ブラウザーのサポート

{{site.data.keyword.conversationshort}} サービス・ツールには、{{site.data.keyword.Bluemix_notm}} で必要なものと同じレベルのブラウザー・ソフトウェアが必要です。詳しくは、{{site.data.keyword.Bluemix_notm}} の [前提条件 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/docs/overview/prereqs.html#browsers){: new_window} のトピックを参照してください。

## 言語サポート

機能別の言語サポートの詳細情報は [サポートされる言語](lang-support.html)のトピックで説明されています。

## 次のステップ

- サービスの[概説](getting-started.html)
- いくつかの[デモ](sample-applications.html)をお試しください。
- [SDK ![外部リンクのアイコン](../../icons/launch-glyph.svg "外部リンクのアイコン")](https://www.ibm.com/watson/developercloud/developer-tools.html){: new_window} のリストを参照します。

まだご不明な点がありますか? [IBM 営業部門 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window} にお問い合わせください。
