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

# Improve コンポーネントの概要

{{site.data.keyword.conversationshort}} の Improve コンポーネントは、ワークスペースのユーザーとの対話の履歴を提供します。この履歴を使用して、ユーザー入力に対するワークスペースの理解能力を改善できます。
{: shortdesc}

**「改善 (Improve)」**パネルのセクション:

* [「概要 (Overview)」](logs_oview.html): ユーザーとワークスペースの対話の概要。
* [「ユーザーの会話 (User conversations)」](logs_convo.html): ユーザー発話のリスト。 個々のユーザー発話を表示しながら、インテントとエンティティーを更新できます。**注**: 単一のユーザーの会話は、複数の発話で構成されている可能性があります。
* [「推奨 (Recommendations)」](logs_recommend.html): システムを改善する方法。 プレミアム・ユーザーのみが利用できます。

## ワークスペース間での改善
{: #deploy_id}

発話データを使用してワークスペース間で改善を行う方法を理解するために、{{site.data.keyword.conversationshort}} サービスに関連する以下の定義を確認しておくことは有益です。

* ***インスタンス***: 固有の資格情報を使用してアクセスできる {{site.data.keyword.conversationshort}} のデプロイメント。{{site.data.keyword.conversationshort}} インスタンスは、複数のワークスペースで構成される場合があります。
* ***ワークスペース***: ワークスペースは {{site.data.keyword.conversationshort}} コンテンツの 1 つのモデルです。多くの場合、これはボットに相当します。
* ***ワークスペース ID***: ワークスペースの固有 ID。
* ***デプロイメント ID***: デプロイメント ID は、ユーザー発話とともに渡される固有のラベルであり、この ID によって、発話の発生元のデプロイメント環境を識別できます。
* ***発話***: 発話は、ユーザーがワークスペースに送信する単一のメッセージです。

ワークスペースの作成は非常に反復の多いプロセスです。ワークスペースを開発するときには、*「Try it out」*ペインを使用して、ワークスペースがテスト入力から正しいインテントとエンティティーを認識することを確認し、必要に応じて修正することができます。

**「改善 (Improve)」**パネルでは、ユーザーとの実際の対話に関する情報を表示し、同様の修正を加えて、ワークスペースによるインテントとエンティティーの認識の正確性を改善することができます。ユーザーの質問の*仕方*や、ランダムな発話の内容を正確に把握することは難しいため、ワークスペースを改善するためには、頻繁に**「改善 (Improve)」**パネルにアクセスすることが重要です。

{{site.data.keyword.conversationshort}} インスタンスに複数のワークスペースが含まれている場合は、ある 1 つのワークスペースの発言データを、同じインスタンス内の別のワークスペースを改善するために使用することが有効な場合があります。**注**: {{site.data.keyword.conversationshort}} プレミアム・ユーザーの場合は、オプションで、さまざまなプレミアム・インスタンスのワークスペースからのログ・データにアクセスできるようにプレミアム・インスタンスを構成することができます。

例えば、*HelpDesk* という {{site.data.keyword.conversationshort}} インスタンスがあるとします。HelpDesk インスタンスには、実動ワークスペースと開発ワークスペースの両方が含まれているとします。開発ワークスペースで作業しているときに、実動ワークスペースの`デプロイメント ID` に基づいて発話をフィルターに掛けることができ、そのようにして、実動ワークスペースの発話を使用して開発ワークスペースを改善することができます。

![データ・ソース・リンク](images/data_source_1.png)

開発ワークスペース内で行った編集は、実動ワークスペースの発話を使用している場合でも、開発ワークスペースにのみ影響します。詳細については、[データ・ソースの選択](logs_convo.html#select-source)を参照してください。

`/message` API を使用して送信する発話のデプロイメント ID を指定するには、デプロイメント・プロパティーを、次の例のように、[コンテキスト ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#send_message){: new_window} 内のメタデータ・オブジェクト内に含めます。

```
"context" : {
  "username" : "jane_doe@myemail.com",
  "member_type" : "gold",
  "metadata" : {
       "deployment": "HelpDesk-Production"
  }
}
```
{: #codeblock}
