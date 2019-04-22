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

# ベータ
{: #beta}

![ベータ](images/beta.png) IBM は、ベータに分類される評価のためのサービス、機能、および言語サポートをリリースしています。 これらの機能は不安定な場合があり、頻繁に変更される場合があり、十分な通知期間を設けずに中止される場合があります。 また、ベータ機能では、通常使用できる機能と同レベルのパフォーマンスや互換性が提供されない場合があり、実稼働環境での使用が意図されていません。 ベータ機能は、[IBM Developer Answers ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.ibm.com/answers/topics/watson-assistant/){: new_window} でのみサポートされています。

## 使用可能なベータ機能
{: #beta-features}

以下の機能は、ベータ・プログラムの参加者のみ使用できます。アクセス権を要求する方法については、[ベータ・プログラムへの参加](/docs/services/assistant?topic=assistant-feedback#feedback-beta)を参照してください。

- 検索スキルの使用方法が変更されました。1 つの検索スキルと 1 つのダイアログ・スキルを同じアシスタントに追加できるようになりました。両方を追加すると、ダイアログ・スキルのダイアログ内のいずれかのノードでユーザー入力に対処できない場合に、検索がトリガーされます。詳しくは、以下のトピックを参照してください。

  - [検索スキル](/docs/services/assistant?topic=assistant-skill-search-add)
  - [ダイアログ・スキル](/docs/services/assistant?topic=assistant-beta-skill-dialog-add)

  この機能がリリースされると、Plus プランまたは Premium プランのユーザーのみがこれを使用できるようになります。

- ダイアログ・ビルダーのユーザー・インターフェースが、React JavaScript ライブラリーを使用するように更新されました。ダイアログ機能が、その独自の状態を管理するカプセル化されたコンポーネントで提供されるようになり、ユーザー・エクスペリエンスの応答性が向上しました。

- ユーザー入力時のミススペルを訂正するスキルを構成できます。詳しくは、[ユーザー入力の修正](/docs/services/assistant?topic=assistant-beta-spell-check)を参照してください。

- 既存のお客様サポートのチャット・トランスクリプトを利用して、アシスタントのトレーニングに使用する適切なインテントとユーザー例のセットを見つけます。詳しくは、[インテント作成時のヘルプの利用](/docs/services/assistant?topic=assistant-beta-intent-recommendations)を参照してください。
