---

copyright:
  years: 2015, 2019
lastupdated: "2018-02-09"

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

# {{site.data.keyword.conversationshort}} コネクターを使用したチャネルへのデプロイ
{: #conversation-connector}

ワークスペースを開発し終えたら、{{site.data.keyword.conversationshort}} コネクターを使用して、Slack や Facebook Messenger などの通信チャネルにすぐにワークスペースを接続できます。{{site.data.keyword.conversationshort}} コネクターは、{{site.data.keyword.openwhisk}} ワークスペースと Slack アプリや Facebook アプリとの間の通信を仲介し、セッション・データを Cloudant データベースに保管する、{{site.data.keyword.conversationshort}} コンポーネントのセットです。

ワークスペースをチャネル・アプリに接続すると、Slack または Facebook Messenger のユーザーが対話できるチャット・ボットとして使用できるようになります。ダイアログからの応答には、マルチメディア応答や対話式の応答 (イメージやクリック可能なボタンなど) を含めることができます。(マルチメディア応答の定義について詳しくは、[マルチメディア応答](dialog-multimedia.html)を参照してください。)

![{{site.data.keyword.openwhisk_short}} デプロイメントの総括ダイアグラム](images/deploytochannel_diagram.png)

{{site.data.keyword.conversationshort}} コネクターには、次の制限事項があります。

- Slack アプリか Facebook アプリを作成するか、使用しようとしているアプリを変更するための管理権限を持っている必要があります。
- {{site.data.keyword.openwhisk_short}} の制約事項のために、このツールが現在使用できるのは {{site.data.keyword.Bluemix_notm}} の米国南部地域のみです。

## {{site.data.keyword.conversationshort}} ツールを使用した Slack へのデプロイ

{{site.data.keyword.conversationshort}} ツールを使用すると、{{site.data.keyword.conversationshort}} コネクターを使用して素早くボットをデプロイできます。このツールでは、チャネル・アプリとコネクター・コンポーネントの構成に必要な情報を収集するプロセスの手順が表示され、それらの手順に従うことで、自動的に必要なコンポーネントが IBM Cloud にデプロイされます。

**注:** {{site.data.keyword.conversationshort}} ツール・インターフェースは現在 Slack のみサポートしていますが、自動化された {{site.data.keyword.Bluemix_short}} プロセスを使用して Facebook Messenger へのデプロイを実行できます。Facebook Messenger へのデプロイについて詳しくは、{{site.data.keyword.conversationshort}} コネクターの [GitHub リポジトリー![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/watson-developer-cloud/conversation-connector/blob/master/channels/facebook/README.md){: new_window} 内の資料を参照してください。

自動化されたデプロイメント・ツールを使用してボットをデプロイするには、以下のようにします。

1. {{site.data.keyword.conversationshort}} ツールで、デプロイしようとしているワークスペースを開きます。
1. 左上隅にあるメニュー・アイコンをクリックして、**「デプロイ (Deploy)」**を選択します。

   ![クイック・デプロイのメニュー・オプション](images/deploy_menu_testdeploy.png)

1. **「{{site.data.keyword.openwhisk_short}} を使用してデプロイ (Deploy with Cloud Functions)」**で、**「Slack にデプロイ (Deploy to Slack)」**をクリックします。
1. 「Slack」ページで、**「Slack アプリにデプロイ (Deploy to Slack app)」**をクリックし、指示に従います。

   ![「Slack アプリにデプロイ (Deploy to Slack app)」ボタン](images/deploy_deploytoslack.png)

## 手動デプロイ

自動化されたツールを使用してワークスペースをボットとしてデプロイする代わりに、必要なコンポーネントを手動で構成して IBM Cloud にデプロイできます。このことは、以下のような場合に行います。

- **部分的な再デプロイメント**。構成を変更したり、問題を訂正したり、新バージョンからフィックスを適用したりするために、既存のデプロイメントの一部のコンポーネントを再デプロイすることもできます。
- **新しい機能によるボットの拡張**。提供されているコンポーネントを変更して新機能 (例えば、ユーザー入力の前処理を行った後に {{site.data.keyword.conversationshort}} ワークスペースに送信する機能など) を追加できます。
- **新しいチャネルへのデプロイ**。ボットを Slack や Facebook Messenger 以外のチャネルにデプロイする場合には、既存のコンポーネントのパターンに従って独自のコンポーネントを開発できます。

{{site.data.keyword.conversationshort}} コネクターのコンポーネントは、パブリック GitHub リポジトリー内にホストされており、そこからダウンロードまたは複製することができます。詳しくは、この[リポジトリー ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/watson-developer-cloud/conversation-connector){: new_window} 内で提供されている資料を参照してください。

## ボットとのチャット

デプロイメント・プロセスを完了すると、Slack や Facebook Messenger の他のボットと対話する場合と同様に、ボットのユーザー名を使用して {{site.data.keyword.conversationshort}} ワークスペースと対話することができます。

{{site.data.keyword.conversationshort}} コネクターは、ボットと対話しているユーザーごとに個別に状態を保持することに注意してください。(Slack の場合は、1 人のユーザーがボットとの複数の固有の会話 (直接メッセージによる会話と、各チャネルでの会話) を持つことができます。)つまり、ダイアログ・コンテキストに格納したすべての変数は、クリアしない限り、無期限に維持されます。

会話を既知の開始状態にリセットする必要がある場合、ダイアログ内でリセットできます。ダイアログには、会話の終わりまたは最初からやり直すことが必要な他の場合に実行されるノードが必要です。 すべてのコンテキスト変数を適切な開始値にリセットするように、このノードの JSON を更新します。 (必要な場合は、各**「ジャンプ先 (Jump to)」**アクション、またはこのノードを実行するための特別な「end conversation (会話の終了)」インテントを使用できます。)

例えば、ワークスペースで `drink_order` というコンテキスト変数を使用してユーザーの飲み物の選択を格納している場合、会話の終了時に次に示すように `context.remove` メソッドを使用してこの変数を削除できます。

```json
"context": {
   "reset_drink_order": "<?context.remove('drink_order')?>"
 }
```
{: codeblock}

コンテキスト変数値の変更について詳しくは、[コンテキスト変数値の更新](dialog-runtime.html#context-update)を参照してください。

デプロイした {{site.data.keyword.openwhisk_short}} アクションを編集して、コンテキストをクリアしたり、ボットの動作にその他の変更を加えたりすることもできます。詳しくは、[GitHub リポジトリー ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/watson-developer-cloud/conversation-connector){: new_window} 内で提供されている資料を参照してください。
