---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-01"

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

# Slack との統合
{: #deploy-slack}

Slack は、ユーザー同士のコラボレーションを支援するクラウド・ベースのメッセージング・アプリケーションです。
{: shortdesc}

ダイアログ・スキルを構成してアシスタントに追加した後に、そのアシスタントを Slack と統合できます。

1.  「アシスタント (Assistants)」タブから、デプロイするアシスタントのタイルをクリックして開きます。

1.  「Integrations」セクションから、**「Add Integration」**をクリックします。

1.  *Slack* の**「Select Integration」**ボタンをクリックします。

1.  画面に示された指示に従って、統合プロセスを実行します。

他のユーザーがデプロイメント・ステップを実行する様子を見て手順を確認したい場合は、次の動画をご覧ください。

<iframe class="embed-responsive-item" id="youtubeplayer" title="Walkthrough of the Slack deployment steps" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/RBGBPJ8h4HQ?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

所要時間: 9.5 分

## ダイアログについての考慮事項
{: #deploy-slack-dialog}

ダイアログに追加するリッチ応答は Slack チャネルに正常に表示されますが、以下の例外があります。

- **Connect to human agent**: この応答タイプは無視されます。

- **Image**: イメージのタイトルは必須です。タイトルを指定しない場合、そのイメージは表示されません。

- **Option**: この応答タイプでは、ユーザーが選択できるオプションのリストが表示されます。

  - タイトルは**必須**で、オプションのリストの前に表示されます。
  - 説明は、指定したかどうかに関係なく表示されません。
  - ユーザーがいずれかのボタンをクリックすると、ボタンの選択項目が非表示になり、ユーザーの選択によって生成されるユーザー入力に置き換わります。1 つの応答に複数の応答タイプを含める場合は、オプションの応答タイプを最後に配置してください。そのようにしないと、応答とユーザー入力が出力に混在するため、ユーザーに混乱を招く可能性があります。

- **Pause**: この応答タイプは、Slack チャネルでのアシスタントのアクティビティーを一時停止します。ただし、入力の表示を選択したかどうかに関係なく、アシスタントが一時停止していることを示す可視インディケーターは表示されません。

応答タイプについて詳しくは、[リッチ応答](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)を参照してください。

## アシスタントとのチャット
{: #deploy-slack-try}

アシスタントとチャットを開始するには、以下の手順を実行します。

1.  Slack を開き、アプリに関連するワークスペースに移動します。
1.  「アプリ」セクションで作成したアプリケーションをクリックします。
1.  アシスタントとチャットします。

ダイアログの「Welcome」ノードは、Slack 統合では処理されません。ウェルカム・メッセージは、ツール内の「Try it out」ペインやプレビュー・リンク統合 Web ページとは異なり、Slack チャネルでは表示されません。このノードがここからトリガーされない理由は、`welcome` という特殊条件を持つノードは、ユーザーによって開始されたダイアログ・フローではスキップされるためです。Slack は、ユーザーが会話を開始するのを待ちます。会話の開始時にコンテキスト変数のデフォルト値を設定する必要がある場合、「welcome」ノードにはそれらの値を設定しないでください。詳しくは、[ダイアログの開始](/docs/services/assistant?topic=assistant-dialog-start)を参照してください。
{: note}

現行セッションのダイアログ・フローは、60 分間 (ライト・プランおよび標準プランの場合は 5 分間) 何の操作も行われないと再開されます。つまり、ユーザーがアシスタントとの対話を停止し、60 分 (または 5 分) 経過すると、以前の会話中に設定されたコンテキスト変数の値はすべてヌルに設定されるか、デフォルト値に戻されます。
