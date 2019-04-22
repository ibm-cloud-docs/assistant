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

# データのバックアップとリストア
{: #backup}

データをエクスポートしてからインポートすることによって、データをバックアップおよびリストアします。
{: shortdesc}

{{site.data.keyword.conversationshort}} サービス・インスタンスから以下のデータをエクスポートできます。

- ダイアログ・スキル・トレーニング・データ (インテントとエンティティー)
- ダイアログ・スキル・ダイアログ

以下のデータはエクスポートできません。

<!--- Search skill -->
- アシスタント (構成済みの統合を含む)

## ログの保持
{: #backup-retain-logs}

各ユーザーがアシスタントとやり取りした会話のログを保管する場合は、`/logs` API を使用して、ログ・データをエクスポートできます。詳しくは、[API リファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/apidocs/assistant#list-log-events-in-a-workspace) を参照してください。

スキル・タイルからスキルのワークスペース ID を取得するには、![オプション・リストのオープンとクローズ](images/kabob-beta.png) アイコンをクリックしてから、**「View API Details」**を選択します。
{: tip}

ログが保管される期間は、サービス・プランに応じて異なります。例えば、ライト・プランでは、過去 7 日間のログのみ提供します。詳しくは、[ログの制限](/docs/services/assistant?topic=assistant-logs#logs-limits)を参照してください。

あるスキルから別のスキルにログをインポートすることはできません。

## ダイアログ・スキルのエクスポート
{: #backup-export-skill}

ダイアログ・スキル・データをバックアップするには、スキルを JSON ファイルとしてエクスポートして、その JSON ファイルを保管します。

1.  「スキル」ページで、またはスキルを使用するアシスタントの構成ページで、そのダイアログ・スキルのタイルを見つけます。

1.  ![オプション・リストのオープンとクローズ](images/kabob-beta.png) アイコンをクリックしてから、**「JSON のダウンロード (Download JSON)」**を選択します。

1.  JSON ファイルの名前と保存場所を指定して、**「保存 (Save)」**をクリックします。

別の方法として、`/workspaces` API を使用して、ダイアログ・スキルをエクスポートすることもできます。GET ワークスペース要求に `export=true` パラメーターを組み込みます。詳しくは、[API リファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace) を参照してください。

## ダイアログ・スキルのインポート
{: #backup-import-skill}

別のサービス・インスタンスまたは環境からエクスポートしたダイアログ・スキルのバックアップ・コピーを復元するには、エクスポートしたダイアログ・スキルの JSON ファイルをインポートすることによって、新しいダイアログ・スキルを作成します。

クラウド・ホスティングの継続的デリバリー環境内のインスタンスに定期的に適用される機能更新によって、スキルをエクスポートしてからインポートするまでの間に {{site.data.keyword.conversationshort}} サービスが変更されると、インポートしたスキルの機能が以前とは異なる場合があります。
{: important}

1.  **「スキル (Skills)」**タブをクリックします。

1.  **「新規作成 (Create new)」**をクリックします。

1.  **「スキルのインポート (Import skill)」**をクリックしてから**「JSON ファイルの選択 (Choose JSON File)」**をクリックし、インポートする JSON ファイルを選択します。

    **重要:**

    - インポートする JSON ファイルは、バイト・オーダー・マーク (BOM) エンコードなしの UTF-8 エンコード方式を使用している必要があります。
    - スキルの JSON ファイルの最大サイズは 10MB です。 これよりも大きなスキルをインポートする必要がある場合は、スキルをインポートした後にインテントとエンティティーを別々にインポートする方法を検討してください。 (サイズの大きなスキルは、REST API を使用してインポートすることもできます。 詳しくは、『[API リファレンス ![「外部リンク」アイコン](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/apidocs/assistant#create-workspace){: new_window}』を参照してください。)
    - JSON ファイルにはタブ、改行、復帰を含めることができません。

    エクスポートされたスキルの完全なコピーをインポートする場合は、**「Everything (Intents, Entities, and Dialog)」**を選択します。

    **「インポート」**をクリックします。

    スキルのインポート時に問題が発生した場合は、[スキルのインポートに関する問題のトラブルシューティング](/docs/services/assistant?topic=assistant-skill-add#skill-add-import-errors)を参照してください。

1.  スキルの詳細を指定します。

    - **名前 (Name)**: 長さ 100 文字以下の名前です。 名前は必須です。
    - **説明 (Description)**: 長さ 200 文字以下のオプションの説明です。
    - **言語 (Language)**: トレーニングの対象となるスキルのユーザー入力の言語です。 デフォルト値は英語です。

スキルは、作成された後、「スキル (Skills)」ページにタイルとして表示されます。

## アシスタントの再作成
{: #backup-recreate-assistant}

アシスタントを再作成できるようになりました。続いて、インポートしたダイアログ・スキルをアシスタントにリンクして、アシスタント用の統合を構成できます。

詳しくは、[アシスタントの作成](/docs/services/assistant?topic=assistant-assistant-add)および[統合の追加](/docs/services/assistant?topic=assistant-deploy-integration-add#deploy-integration-add-task)を参照してください。

## クライアント・アプリケーションの更新
{: #backup-update-api}

エクスポートしたダイアログ・スキルをインポートすると、新しいスキルが作成されます。新しいスキルには新しいワークスペース ID があります。v1 の API を使用してこのスキルにアクセスする既存のクライアント・アプリケーションがある場合は、代わりに新しいワークスペース ID を使用するようにすべてのワークスペース ID 参照を更新する必要があります。

アシスタントを再作成するときは、新しいアシスタント ID が付与されます。v2 の API を使用してアシスタントにアクセスする既存のクライアント・アプリケーションがある場合は、代わりに新しいアシスタント ID を使用するようにすべてのアシスタント ID 参照を更新する必要があります。
