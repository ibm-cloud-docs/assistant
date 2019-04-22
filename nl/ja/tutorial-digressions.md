---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-12"

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

# チュートリアル: 脱線の説明
{: #tutorial-digressions}

このチュートリアルでは、脱線の動作を直接ご覧いただきます。
{: shortdesc}

## 学習目標
{: #tutorial-digressions-objectives}

このチュートリアルを完了すると、以下のことを理解できます。

- 脱線の動作の設計方法
- 脱線設定がダイアログのフローに与える影響
- ダイアログの脱線設定のテスト方法

### 所要時間
{: #tutorial-digressions-duration}

このチュートリアルを完了するための所要時間は約 20 分です。

### 前提条件
{: #tutorial-digressions-prereqs}

{{site.data.keyword.conversationshort}} インスタンスがない場合は、[概説チュートリアル](/docs/services/assistant?topic=assistant-getting-started#getting-started-prerequisites)の『**始める前に**』の手順を完了して作成してください。

## ステップ 1: 脱線ショーケース・ダイアログ・スキルのインポート
{: #tutorial-digressions-import-json}

まず、*脱線ショーケース*・ダイアログ・スキルを {{site.data.keyword.conversationshort}} インスタンスにインポートする必要があります。

1.  [digression-showcase.json](https://github.com/watson-developer-cloud/community/raw/master/watson-assistant/digression-showcase.json) ファイルをダウンロードします。
1.  {{site.data.keyword.conversationshort}} インスタンスで、![インポート](images/workspace_import.png) アイコンをクリックします。
1.  **「ファイルの選択 (Choose a file)」**をクリックして、上記でダウンロードした **digression-showcase.json** ファイルを選択します。
1.  **「インポート (Import)」**をクリックしてダイアログ・スキルのインポートを終了します。

## ステップ 2: ダイアログからの一時的な脱線
{: #tutorial-digressions-temporarily-digress-away}

脱線により、ユーザーはダイアログ・ブランチから抜けてトピックを一時的に変更し、その後元のダイアログ・フローに戻ることができます。このステップでは、まずレストランを予約し、脱線してレストランの営業時間を尋ねます。サービスは、営業時間の情報を提供した後、レストラン予約ダイアログ・フローに戻ります。

1.  **「ダイアログ (Dialog)」**をクリックして、インテントを含むページからダイアログ・ツリーのビューに切り替えます。

1.  ![「試行する (Try it)」](images/ask_watson.png) アイコンをクリックして、「試行する (Try it out)」ペインを開きます。
1.  テキスト・フィールドに `Book me a restaurant` と入力します。

    サービスは、`When do you want to go?` という予約日の入力を求めるプロンプトで応答します。

1.  応答の横にある**「場所 (Location)」** ![場所](images/location.png) アイコンをクリックして、応答をトリガーしたノード (ダイアログ・ツリーの **Restaurant booking** ノード) を強調表示します。

    ![「試行する (Try it out)」ペインの強調表示された Restaurant booking ノードおよび進行中のダイアログを示しています。](images/tut-dig-location.png)
1.  `Tomorrow` と入力します。

    サービスは、`What time do you want to go?` という予約時間の入力を求めるプロンプトで応答します。

1.  レストランがいつ閉まるかわからないため、`What time do you close?` と尋ねます。

    ボットは restaurant booking ノードから脱線して **Restaurant opening hours** ノードを処理します。`The restaurant is open from 8:00 AM to 10:00 PM.` と応答します。サービスは restaurant booking ノードに戻り、予約時間を再度尋ねます。

    ![「試行する (Try it out)」ペインで発生した脱線を示しています。](images/tut-dig-digression.png)
1.  **オプション**: ダイアログ・フローを完了するために、予約時間については `8pm` と入力し、人数については `2` と入力します。

これで完了です。 正常にダイアログ・フローから脱線し、ダイアログ・フローに戻りました。

## ステップ 3: スロット脱線の無効化
{: #tutorial-digressions-disable-slot}

このステップでは、ユーザーが脱線しないように restaurant booking ノードの脱線設定を編集し、設定の変更がダイアログ・フローに与える影響を確認します。

1.  **Restaurant booking** ノードの現在の脱線設定を見てみましょう。ノードをクリックして、編集ビューで開きます。

1.  **「カスタマイズ (Customize)」**をクリックし、**「脱線 (Digressions)」**タブをクリックします。

    ![Restaurant booking ノードの脱線設定を示しています。](images/tut-dig-resto-settings.png)

1.  **「脱線を許可 (Allow digressions away)」**トグルを「オン (on)」から**「オフ (off)」**に変更して、**「適用 (Apply)」**をクリックします。

1.  ノード編集ビューを閉じるには、![閉じる](images/close.png) をクリックします。

1.  「試行する (Try it out)」ペインで**「クリア」**をクリックして、ダイアログをリセットします。

1.  `Book me a restaurant` と入力します。

    サービスは、`When do you want to go?` という予約日の入力を求めるプロンプトで応答します。

1.  `Tomorrow` と入力します。

    サービスは、`What time do you want to go?` という予約時間の入力を求めるプロンプトで応答します。

1.  `What time do you close?` と尋ねます。

    サービスは、質問によって #restaurant_opening_hours インテントがトリガーされたことを認識しますが、それを無視して、@sys-time スロットに関連付けられているプロンプトを再度表示します。

ユーザーがレストラン予約プロセスから脱線するのを正常に防止しました。

## ステップ 4: 戻らないノードへの脱線
{: #tutorial-digressions-digress-without-return}

処理対象の現在のノードについて、サービスが脱線したノードに戻らないようにダイアログ・ノードを構成できます。これを実証するには、restaurant hours ノードの脱線設定を変更します。ステップ 2 では、restaurant booking ノードから脱線して restaurant opening hours ノードに移動した後、サービスは restaurant booking ノードに戻って予約プロセスを続行しました。この演習では、設定の変更後、**Job opportunities** ダイアログから脱線してレストランの営業時間を尋ね、サービスが処理を中断した場所に戻らないことを確認します。

1.  **Restaurant opening hours** ノードをクリックして開きます。

1.  **「カスタマイズ (Customize)」**をクリックし、**「脱線 (Digressions)」**タブをクリックします。

1.  **「脱線はこのノードに戻ることができる (Digressions can come into this node)」**セクションを展開し、**「脱線後に戻る (Return after digression)」**チェック・ボックスを選択解除します。**「適用 (Apply)」**をクリックしてから、![閉じる](images/close.png) をクリックして、ノード編集ビューを閉じます。

1.  「試行する (Try it out)」ペインで**「クリア」**をクリックして、ダイアログをリセットします。

1.  **Job opportunities** ダイアログ・ノードを使用するには、`I'm looking for a job` と入力します。

    サービスは、`We are always looking for talented people to add to our team. What type of job are you interested in?` と応答します。

1.  この質問に答えずに、ボットに関係のない質問をします。`What time do you open?` と入力します。

    サービスは Job opportunities ノードから Restaurant opening hours ノードに脱線し、質問に答えます。サービスは `The restaurant is open from 8:00 AM to 10:00 PM.` と応答します。

    前のテストとは異なり、今回はダイアログは **Job opportunities** ノードの処理を中断したところから再開しません。戻らないように **Restaurant opening hours** ノードの設定を変更したため、サービスは処理中だったダイアログに戻りません。

    ![脱線後に戻らない会話を示しています。](images/tut-dig-noreturn.png)

これで完了です。 戻ることなく正常にダイアログから脱線しました。

## サマリー
{: #tutorial-digressions-summary}

このチュートリアルでは、脱線の動作を体験し、個々のダイアログ・ノードの設定が脱線の動作に与える影響を確認しました。

## 次のステップ
{: #tutorial-digressions-next-steps}

独自のダイアログの脱線を構成するときに支援が必要な場合は、[脱線](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions)を参照してください。
