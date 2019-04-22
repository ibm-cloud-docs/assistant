---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-11"

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

# ダイアログの開始
{: #dialog-start}

すべての統合で同じ方法で、組み込みの Welcome ノードを使用してダイアログを開始することはできません。代わりに、回避策を使用します。
{: shortdesc}

ダイアログの Welcome ノードに定義した応答は、ツール内の「試行する (Try it out)」ペイン、およびプレビュー・リンク統合のチャット・ウィジェットから会話を開始するために表示されます。ただし、ユーザーが開始したダイアログ・フローでは `welcome` 特殊条件を持つノードはスキップされるため、他の多くのチャネル統合からは表示されません。また、デプロイ済みのアシスタントでは、通常、ユーザーが会話を開始することを待機し、アシスタントからは開始しません。

`welcome` 特殊条件とは異なり、`conversation_start` 特殊条件は、常にダイアログの開始時にトリガーされます。これら 2 つの特殊条件 (`welcome` と `conversation_start`) を持つノードの組み合わせを使用して、一貫性のある方法でダイアログの開始を管理します。

ダイアログの開始を管理するには、以下の手順を実行します。

1.  ダイアログの作成時にダイアログ・ツリーの上部に自動的に追加された Welcome ノードの上に、ダイアログ・ノードを追加します。

1.  新しく追加されたノードのノード条件に、前述の特殊条件である `conversation_start` を設定します。

1.  `conversation_start` ノードで、ダイアログのデフォルト値で設定するコンテキスト変数を定義します。

1.  このノードのテキスト応答は定義しないでください。

1.  このノードを、ダイアログ・ツリーの直下の `Welcome` ノードにジャンプするように構成し、**「ボットが認識する場合 (条件) (If bot recognizes (condition))」**を選択します。

![その下の Welcome ノードにジャンプする conversation_start ノードを含むダイアログ・ツリーのスクリーン・ショット](images/dialog-start.png)

この設計では、ダイアログは次のように機能します。

- 統合タイプに関係なく、`conversation_start` ノードが処理されます。つまり、そこで定義したコンテキスト変数が初期化されます。
- アシスタントがダイアログ・フローを開始する統合では、`Welcome` ノードがトリガーされて、テキスト応答が表示されます。
- ユーザーがダイアログ・フローを開始する統合では、ユーザーの最初の入力が評価されて、最適な応答を提供できるノードによって処理されます。
