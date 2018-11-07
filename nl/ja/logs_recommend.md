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

# レコメンデーション (Recommendations)
このページには、システムを改善するために推奨される方法が表示されます。
{: shortdesc}

![「レコメンデーション (Recommendations)」タブ](images/RecommendTop.png)

この機能はベータ版のみです。
{: tip}

この機能はプレミアム・ユーザーにのみ提供されています。
{: tip}

ユーザーとワークスペースの会話内容を分析し、システムの現在のトレーニング・データと応答の確実性を考慮したうえで、ワークスペースを簡単かつ効率的に改善するために実行できるアクションが提示されます。

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/scMu66AvZtY" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

レコメンデーションは夜間に生成されます。50 以上といった大量のユーザー・メッセージが必要になります。
{: tip}

## 既存のインテントを改善する (Improve existing intents)
このレコメンデーションでは、ユーザー入力のうちシステムで認識されない個々のフレーズが取得され表示されるので、フレーズごとにインテントを選択します。 これにより、ユーザーの発言をワークスペースがより正確に理解できるようになります。

**「開始 (Start)」**をクリックして、インテントの特定を開始します。
![「既存のインテントを改善する (Improve existing intents)」ページ](images/rec_improve_intent.png)

**「既存のインテントを改善する (Improve existing intents)」**を開始または終了するときには、その日の未処理のフレーズ総数に対して、現行セッションで対処したフレーズ数を示す進行状況表示バーが表示されます。 終了して再開した場合、進行状況表示バーは再び`空`の状態から始まりますが、前回の作業が失われたわけではないので注意してください。前回の作業は、現行セッションの進行状況にカウントされないだけです。

表示されたリストから、フレーズに対して最適なインテントを選択するか、*「不適当としてマークを付ける (Mark as irrelevant)」*を選択します。 **「保存 (Save)」**をクリックするとすぐに、フレーズがサンプル (トレーニング・データ) としてインテントに追加されます。

*「次へスキップ (Skip to Next)」*ボタンを使用すると、 現在のフレーズをスキップして次に移動できます。 スキップしたフレーズは、同日中に**「既存のインテントを改善する (Improve existing intents)」**を終了して再開しても表示されなくなりますが、翌日以降は再び表示されるようになります。

![「既存のインテントを改善する (Improve existing intents)」の編集ページ](images/rec_improve_intent2.png)
