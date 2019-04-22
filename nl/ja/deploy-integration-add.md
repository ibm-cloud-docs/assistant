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

# 統合の追加
{: #deploy-integration-add}

スキルをデプロイするには、そのスキルをアシスタントに追加してから、お客様の顧客が支援を求めているチャネルにボットを公開する統合をアシスタントに追加します。
{: shortdesc}

## 統合の追加
{: #deploy-integration-add-task}

アシスタントに統合を追加するには、以下の手順を実行します。

1.  **「アシスタント (Assistants)」**タブをクリックします。

1.  デプロイするアシスタントのタイルをクリックして開きます。

1.  「Integrations」セクションに移動します。

    **プレビュー・リンク統合とは何ですか?** アシスタントを作成すると、テスト Web サイトが自動的にプロビジョンされます (無効にした場合を除く)。これは、テスト目的でのアシスタントとの対話に使用できる単純なチャット・ウィジェット・インターフェースを備えています。この IBM ブランドのサイトへの URL をチーム・メンバーと共有することもできます。

1.  **「Add Integration」**をクリックします。

1.  アシスタントと統合するチャネルのタイルをクリックします。オプションは以下のとおりです。

    - [カスタム・アプリケーション](/docs/services/assistant?topic=assistant-deploy-custom-app)
    - [Facebook Messenger](/docs/services/assistant?topic=assistant-deploy-facebook)
    - [Intercom](/docs/services/assistant?topic=assistant-deploy-intercom)  ![プラス・プランとプレミアム・プランのみ](images/premium.png)
    - [プレビュー・リンク](/docs/services/assistant?topic=assistant-deploy-web-link)
    - [Slack](/docs/services/assistant?topic=assistant-deploy-slack)
    - [Voice Agent (Telephony)  ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/catalog/services/voice-agent-with-watson){: new_window}

      {{site.data.keyword.cloud_notm}} で**「Voice Agent with Watson」**サービス概要ページを開きます。
    - [WordPress プラグイン ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://wordpress.org/plugins/conversation-watson/){: new_window}

      WordPress サイトで IBM Watson Assistant プラグイン概要ページを開きます。

1.  画面に示された指示に従って、統合プロセスを実行します。

アシスタントを統合したら、ターゲットのチャネルからテストを行って、アシスタントが正常に機能することを確認します。

すべての統合タイプでダイアログを一貫した方法で開始するためのヒントについては、[ダイアログの開始](/docs/services/assistant?topic=assistant-dialog-start)を参照してください。

## 統合の制限
{: #deploy-integration-add-limits}

1 つのサービス・インスタンスで作成できる統合の数は、ご利用の {{site.data.keyword.conversationshort}} プランによって異なります。

| サービス・プラン     | アシスタントごとの統合数 |
|------------------|---------------------------:|
| プレミアム          |                        100 |
| プラス             |                        100 |
| 標準         |                        100 |
| ライト             |                        100 |
{: caption="サービス・プランの詳細" caption-side="top"}
