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

# Facebook Messenger との統合
{: #deploy-facebook}

Facebook Messenger は、企業や個人のお客様が相互に直接やり取りできるようにするモバイル・メッセージング・アプリケーションです。
{: shortdesc}

ダイアログ・スキルを構成してアシスタントに追加した後に、そのアシスタントを Facebook Messenger と統合できます。

現時点では、Facebook Messenger を使用してアシスタントと対話するユーザーを特定するためのメカニズムはありません。このことは、特定のユーザーに関連するデータを識別したり削除したりする方法がないことを意味します。この統合方法は、GDPR に準拠する必要があるデプロイメントでは使用しないでください。詳しくは、[機密保護](/docs/services/assistant?topic=assistant-information-security)を参照してください。
{: important}

1.  「アシスタント (Assistants)」タブから、デプロイするアシスタントのタイルをクリックして開きます。

1.  「Integrations」セクションから、**「Add Integration」**をクリックします。

1.  *Facebook Messenger* の**「Select Integration」**ボタンをクリックします。

1.  画面に示された指示に従って、統合プロセスを実行します。

他のユーザーがデプロイメント・ステップを実行する様子を見て手順を確認したい場合は、次の動画をご覧ください。

<iframe class="embed-responsive-item" id="youtubeplayer" title="Walkthrough of the Facebook deployment steps" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/8o-FFU5sYNM?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

所要時間: 8 分

## ダイアログについての考慮事項
{: #deploy-facebook-dialog}

ダイアログに追加するリッチ応答は Facebook アプリに正常に表示されますが、以下の例外があります。

- **Connect to human agent**: この応答タイプは無視されます。

- **Image**: この応答タイプでは、イメージが応答に埋め込まれます。タイトルと説明は、指定したかどうかに関係なく、イメージの前に表示されません。

- **Option**: この応答タイプでは、ユーザーが選択できるオプションのリストが表示されます。

  - タイトルは**必須**で、オプションのリストの前に表示されます。
  - 説明は、指定したかどうかに関係なく表示されません。
  - ユーザーがいずれかのボタンをクリックすると、ボタンの選択項目が非表示になり、ユーザーの選択によって生成されるユーザー入力に置き換わります。アシスタントまたはユーザーが新しい入力データを入力すると、ボタンで生成される入力は非表示になります。そのため、1 つの応答に複数の応答タイプを含める場合は、オプションの応答タイプを最後に配置してください。そのようにしないと、後続の応答のコンテンツ (テキスト応答タイプのテキストなど) により、ボタン生成テキストが置き換えられてしまいます。

- **Pause**: この応答タイプは、Messenger でのアシスタントのアクティビティーを一時停止します。しかし、その後に別の応答タイプがトリガーされなければ、アクティビティーは一時停止後に再開しません。この応答タイプを含める場合は、必ず別の異なる応答タイプ (テキスト応答など) がこの応答タイプの後に配置されるように追加してください。

応答タイプについて詳しくは、[リッチ応答](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)を参照してください。

## アシスタントとのチャット
{: #deploy-facebook-try}

アシスタントとチャットを開始するには、以下の手順を実行します。

1.  Facebook Messenger を開きます。
1.  以前に作成したページの名前を入力します。
1.  ページが表示されたら、そこをクリックして、アシスタントとのチャットを開始します。

ダイアログの「Welcome」ノードは、Facebook Messenger 統合では処理されません。ウェルカム・メッセージは、ツール内の「Try it out」ペインやプレビュー・リンク統合 Web ページとは異なり、Facebook チャットでは表示されません。このノードがここからトリガーされない理由は、`welcome` という特殊条件を持つノードは、ユーザーによって開始されたダイアログ・フローではスキップされるためです。Facebook Messenger は、ユーザーが会話を開始するのを待ちます。会話の開始時にコンテキスト変数のデフォルト値を設定する必要がある場合、「welcome」ノードにはそれらの値を設定しないでください。詳しくは、[ダイアログの開始](/docs/services/assistant?topic=assistant-dialog-start)を参照してください。
{: note}

現行セッションのダイアログ・フローは、60 分間 (ライト・プランおよび標準プランの場合は 5 分間) 何の操作も行われないと再開されます。つまり、ユーザーがアシスタントとの対話を停止し、60 分 (または 5 分) 経過すると、以前の会話中に設定されたコンテキスト変数の値はすべてヌルに設定されるか、デフォルト値に戻されます。
