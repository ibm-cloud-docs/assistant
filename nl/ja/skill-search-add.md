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

# 検索スキルの構築
{: #skill-search-add}

アシスタントでは、*検索スキル* を使用して、お客様の複雑な問い合わせを {{site.data.keyword.discoveryfull}} サービスにルーティングします。{{site.data.keyword.discoveryshort}} では、ユーザー入力が検索照会として処理されます。照会に関連する情報が外部データ・ソースから検索され、アシスタントに返されます。
{: shortdesc}

この機能は、ベータ・プログラムの参加者のみ使用できます。アクセス権を要求する方法については、[ベータ・プログラムへの参加](/docs/services/assistant?topic=assistant-feedback#feedback-beta)を参照してください。

![ベータ](images/beta.png) IBM は、ベータに分類される評価のためのサービス、機能、および言語サポートをリリースしています。 これらの機能は不安定な場合があり、頻繁に変更される場合があり、十分な通知期間を設けずに中止される場合があります。 また、ベータ機能では、通常使用できる機能と同レベルのパフォーマンスや互換性が提供されない場合があり、実稼働環境での使用が意図されていません。

1 つのアシスタントに 1 つの検索スキルを追加できます。プランごとの制限について詳しくは、[スキルの制限](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)を参照してください。

検索スキルでは、{{site.data.keyword.discoveryshort}} サービスを使用して作成したデータ・コレクションから情報が検索されます。{{site.data.keyword.discoveryshort}} は、非構造化データをクロール、変換、および正規化するサービスです。このサービスでは、データ分析やコグニティブな直感を適用してデータをエンリッチすることで、後でそのデータから意味のある情報を簡単に検出して取り出すことができるようにします。{{site.data.keyword.discoveryshort}} について詳しくは、[製品資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/discovery?topic=discovery-about) を参照してください。

{{site.data.keyword.discoveryfull}} サービスは、以下の方法でトリガーされます。

- **Anything else ノード**: どのダイアログ・ノードでもユーザーの照会に対処できない場合に、関連する応答を外部データ・ソースで検索します。アシスタントは、`I don't know how to help you with that.` などの標準メッセージを表示する代わりに、`Maybe this information can help:` と表示し、検索によって返されたパッセージをこの後に続けます。検索スキルがアシスタントにリンクされている場合は、`anything_else` ノードがトリガーされると常に、ノード応答を表示する代わりに、検索が実行されます。アシスタントは、照会としてユーザー入力を検索スキルに受け渡し、応答として検索結果を返します。
- **検索応答タイプ**: ダイアログ・ノードに検索応答タイプを追加すると、サービスは、外部データ・ソースからパッセージを取得し、特定の質問に対する応答としてそれを返します。このタイプの検索は、個別のダイアログ・ノードが処理されている場合にのみ発生します。このアプローチは、検索を実行する前にユーザー照会を絞り込む場合に役立ちます。例えば、お客様が購入を希望するデバイスのタイプについての情報をダイアログ・ブランチで収集するとします。メーカーとモデルがわかっている場合、検索スキルに送信する照会でモデルのキーワードを送信でき、より適切な結果を得ることができます。
- **検索スキルのみ**: 検索スキルのみがアシスタントにリンクされていて、ダイアログ・スキルがアシスタントにリンクされていない場合、ユーザー入力がアシスタントの統合チャネルのいずれかから受信されると、検索照会が {{site.data.keyword.discoveryshort}} サービスに送信されます。

## 検索スキルの作成
{: #skill-search-add-task}

まだ[概説チュートリアル](/docs/services/assistant?topic=assistant-getting-started#getting-started-prerequisites)の前提条件ステップを実行していない場合は、それを実行して、{{site.data.keyword.conversationshort}} サービス・インスタンスを作成し、{{site.data.keyword.conversationshort}} ツールを起動します。

{{site.data.keyword.conversationshort}} ツールを使用してスキルを作成します。 検索スキルを作成するには、以下のステップに従います。

1.  **「スキル (Skills)」**タブをクリックします。

1.  **「新規作成 (Create new)」**をクリックします。

1.  **「追加 (Add)」**をクリックして、*検索スキル* を作成します。

1.  新規スキルの詳細を指定します。
    - **名前 (Name)**: 長さ 100 文字以下の名前です。 名前は必須です。
    - **説明 (Description)**: 長さ 200 文字以下のオプションの説明です。

1.  **「作成」**をクリックします。

残りのステップは、コレクションが作成されている既存の {{site.data.keyword.discoveryshort}} サービス・インスタンスにアクセスするかどうかによって異なります。状態に応じて、該当する手順に従ってください。

- [既存の Watson Discovery インスタンスへの接続](#skill-search-add-connect-discovery)
- [Watson Discovery インスタンスの作成](#skill-search-add-create-discovery)

## 既存の Watson Discovery サービス・インスタンスへの接続
{: #skill-search-add-connect-discovery}

1.  情報を抽出する {{site.data.keyword.discoveryshort}} サービス・インスタンスを選択します。
{: #choose-d-instance}

    アクセス権限のある {{site.data.keyword.discoveryshort}} サービス・インスタンスがリストに表示されます。

    一部の {{site.data.keyword.discoveryshort}} サービス・インスタンスに資格情報が設定されていないという警告が表示される場合、{{site.data.keyword.cloud_notm}} ダッシュボードから直接開いたことがない 1 つ以上のインスタンスへのアクセス権限があることを意味しています。サービス・インスタンスにアクセスして、資格情報が作成されるようにする必要があります。資格情報は、{{site.data.keyword.conversationshort}} が {{site.data.keyword.discoveryshort}} サービス・インスタンスへの接続を確立する前に存在している必要があります。リストされる必要がある {{site.data.keyword.discoveryshort}} サービス・インスタンスがリストされていない場合は、{{site.data.keyword.cloud_notm}} ダッシュボードからそのインスタンスを直接開いて、資格情報が生成されるようにしてください。

1.  以下のいずれかを実行して、使用するデータ・コレクションを指定します。
{: #pick-data-collection}

    - 既存のデータ・コレクションを選択します。

      使用するデータ・コレクションを決定する前に、*Discovery で開く (Open in Discovery)* リンクをクリックして、データ・コレクションの構成を検討できます。

      [検索の構成](#beta-search-skill-add-configure)に進みます。

    - コレクションがない場合、またはリストされているデータ・コレクションを使用しない場合は、**「新規コレクションの作成 (Create a new collection)」**をクリックして、コレクションを追加します。[データ・コレクションの作成](#beta-search-skill-add-create-discovery-collection)の手順に従います。

      {{site.data.keyword.discoveryshort}} サービス・プランに基づいて作成が許可されているコレクションの制限数に達している場合は、**「新規コレクションの作成 (Create a new collection)」**ボタンは表示されません。プランの制限詳細について詳しくは、[{{site.data.keyword.discoveryshort}} の料金プラン ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/discovery/discovery-about?topic=discovery-discovery-pricing-plans) を参照してください。

## Watson Discovery サービス・インスタンスの作成
{: #skill-search-add-create-discovery}

1.  {{site.data.keyword.discoveryshort}} サービス・インスタンスを作成するには、**「新規コレクションの作成 (Create a new collection)」**をクリックします。

    {{site.data.keyword.discoveryshort}} サービスのインスタンスが作成され、新規 {{site.data.keyword.discoveryshort}} サービス・インスタンスに対する構成ページが開きます。

    どの {{site.data.keyword.conversationshort}} サービス・プランを使用している場合でも、ライト・プランのサービス・インスタンスが {{site.data.keyword.Bluemix_notm}} にプロビジョンされます。別のプランの一部として {{site.data.keyword.discoveryshort}} サービス・インスタンスを作成する場合は、ここで作業を停止してください。[{{site.data.keyword.Bluemix_notm}} カタログ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/catalog/services/discovery) から直接サービス・インスタンスを作成して、代わりに、[既存の {{site.data.keyword.discoveryshort}} サービス・インスタンスへの接続](#skill-search-add-connect-discovery)手順に従います。
    {: important}

1.  インスタンスを使用するためのご使用条件を確認してから、**「同意する (Accept)」**をクリックして続行します。

1.  [データ・コレクションを作成します](#skill-search-add-create-discovery-collection)。

## データ・コレクションの作成
{: #skill-search-add-create-discovery-collection}

*Watson Discovery News* と呼ばれる、事前にエンリッチされたデータ・ソースをインスタンスに追加しないでください。このデータ・タイプは、{{site.data.keyword.conversationshort}} からは検索できません。
{: important}

1.  {{site.data.keyword.discoveryshort}} コレクションを作成するには、以下のいずれかを実行します。

      - {{site.data.keyword.discoveryshort}} の組み込みサポートとして提供されているデータ・ソースのタイプで保管されているデータからコレクションを作成するには、**「データ・ソースへの接続 (Connect to data source)」**をクリックします。

        1.  データ・ソース・タイプを選択します。
        1.  選択するデータ・ソースの必須情報を指定して、**「接続 (Connect)」**をクリックします。

            詳しくは、[データ・ソースへの接続 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/discovery?topic=discovery-sources) を参照してください。
        1.  データ・ソースのデータと {{site.data.keyword.discoveryshort}} で作成中のコレクションを同期する頻度を指定します。
        1.  データ・ソースから抽出して {{site.data.keyword.discoveryshort}} コレクションに組み込む情報を指定します。

            表示されるオプションは、データ・ソース・タイプによって異なります。

            - Salesforce データ・ソースの場合、ソース文書から抽出するオブジェクト・タイプを選択します。例えば、[Case オブジェクト・タイプ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_case.htm#!) (これは、*事例* つまりお客様の問題を表します) などを選択します。
            - Sharepoint データ・ソースの場合、パスを指定します。
            - ファイル・リポジトリーの場合、ディレクトリーまたはファイルを指定します。

        1.  **「データの保存と同期 (Save and sync data)」**をクリックします。

            データ・コレクションが作成されます。処理が完了すると、別個の Web ブラウザー・タブの {{site.data.keyword.discoveryshort}} ツールに要約ページが表示されます。
        1.  **「{{site.data.keyword.conversationshort}} でのスキルの構成 (Configure your skill in Watson Assistant)」**をクリックして、{{site.data.keyword.conversationshort}} ツールに戻ります。

      - 文書をアップロードしてコレクションを作成するには、**「独自データのアップロード (Upload your own data)」**をクリックします。

        1.  最初にコレクションを定義してから文書をアップロードします。以下の情報を指定します。

            - コレクション名。このサービス・インスタンスに固有の名前にする必要があります。
            - 構成。デフォルトの構成テンプレートの使用、または保存された構成の使用を選択できます。構成について詳しくは、[サービスの構成 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/discovery?topic=discovery-configservice) を参照してください。
            - 言語。このコレクションに追加するファイルの言語を選択します。{{site.data.keyword.discoveryshort}} でサポートされる言語について詳しくは、[言語サポート ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/discovery?topic=discovery-language-support) を参照してください。
        1.  文書をアップロードします。

            サポートされるファイル・タイプには、PDF ファイル、HTML ファイル、JSON ファイル、および DOC ファイルが含まれます。詳しくは、[コンテンツの追加 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/discovery?topic=discovery-addcontent) を参照してください。
            {: note}

            アップロードされる文書を同時進行で同期することはできません。文書の変更内容を反映する場合は、後のバージョンの文書をアップロードしてください。

コレクションが完全に取り込まれるまで待ってから、{{site.data.keyword.conversationshort}} に戻ります。

## 検索の構成
{: #skill-search-add-configure}

1.  {{site.data.keyword.conversationshort}} の検索スキル・ページで、**「構成 (Configure)」**をクリックします。

1.  検索の成功度に応じて、ユーザーと共有するさまざまなメッセージのドラフトを作成します。

    <table>
    <caption>検索結果メッセージ</caption>
    <tr>
      <th>フィールド名</th>
      <th>シナリオ</th>
      <th>メッセージの例</th>
    </tr>
    <tr>
      <td>メッセージ</td>
      <td>検索結果が返された</td>
      <td>次のような役立ちそうな情報が見つかりました: </td>
    </tr>
    <tr>
      <td>検出結果なし (No results found)</td>
      <td>検索結果が見つからない</td>
      <td>照会に対処する情報を知識ベースで検索しましたが、役立つ共有情報は見つかりませんでした。</td>
    </tr>
    <tr>
      <td>エラー・メッセージ</td>
      <td>何らかの理由によりサービスで検索を実行できない</td>
      <td>照会の対処に役立つ情報があるかもしれませんが、現在、知識ベースを検索できません。</td>
    </tr>
    </table>

1.  テキストを抽出する {{site.data.keyword.discoveryshort}} コレクション・フィールドを選択します。

    使用可能なフィールドは、取り込んだデータおよび取り込むために使用した構成に基づいて異なります。

    各検索結果は、以下のような情報から構成されます。

    - **タイトル**: 検索結果のタイトル。検索結果タイトルとして、コレクション・フィールドの title、name、または同様のタイプを使用します。

      Facebook 統合および Slack 統合で応答を表示するには、`None` 以外を選択する必要があります。
    - **本文**: 検索結果の説明。検索結果の本文として、コレクション・フィールドの abstract、summary、または highlight を使用します。

      Facebook 統合および Slack 統合で応答を表示するには、`None` 以外を選択する必要があります。
    - **URL**: ネイティブ・データ・ソースのオリジナル・データ・オブジェクトへのハイパーテキスト・リンク。多くのオンライン・データ・ソースでは、直接アクセスをサポートするために、保管されているオブジェクトの自己参照の公開 URL を指定しています。

      結果 URL は、Slack 統合の応答で URL が組み込まれ、Facebook 統合で応答が表示されるように、有効かつ到達可能である必要があります。Facebook 統合および Slack 統合で、`None` の選択は許容されます。

    詳しくは、[コレクション・フィールド選択のヒント](#skill-search-add-field-tips)を参照してください。
  
    1 つ以上のオプションで、`None` 以外の値を選択する必要があります。

    ドロップダウン・フィールドで使用可能なオプションがない場合、{{site.data.keyword.discoveryshort}} でコレクションの作成が完了するまでさらに時間が必要な可能性があります。それ以外の場合は、コレクションに文書が含まれていないか、対処する必要のある取り込みエラーが発生している可能性があります。

1.  プレビュー・ペインでテスト・メッセージを入力して、構成の選択が検索に適用された場合に返される結果を確認します。必要に応じて調整してください。

1.  **「作成」**をクリックします。

後で構成を変更する場合、検索スキルを再度開いて編集します。加えた変更は、保存する必要はなく、自動的に適用されます。検索結果に満足したら、**「保存 (Save)」**をクリックして、検索スキルの構成を終了します。

## 次のステップ
{: #skill-search-add-next-steps}

スキルは、作成された後、「スキル (Skills)」ページにタイルとして表示されます。

検索スキルは、アシスタントに追加されてそのアシスタントがデプロイされるまでは、お客様と対話できません。詳しくは、[アシスタントの作成](/docs/services/assistant?topic=assistant-assistant-add)を参照してください。

ダイアログ・スキルと検索スキルの両方がアシスタントにリンクされている場合に、ユーザー入力がダイアログ・スキルで処理されてそのダイアログ・ノードのいずれでも対処できない場合は、検索スキルが自動的にトリガーされます。`anything_else` ノードの汎用応答で返信するのではなく、ユーザー入力を照会ストリングとして使用した検索が開始されます。

必要に応じて、特定のノード条件に対応して特定の検索照会が呼び出されるように定義できます。それを行うには、ダイアログ・ノードに検索応答タイプを追加します。詳しくは、[応答](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)を参照してください。

ダイアログ・スキルから任意のタイプの検索を開始する場合、ダイアログをテストして、検索が期待どおりにトリガーされることを確認します。例えば、検索応答タイプを使用していない場合は、既存のダイアログ・ノードでユーザー入力に対処できない場合にのみ検索がトリガーされることをテストします。検索がトリガーされた場合は常に、意味のある結果が返されることを確認します。

### コレクション・フィールド選択のヒント
{: #skill-search-add-field-tips}

データの抽出に適切なコレクション・フィールドは、コレクションのデータ・ソース、およびデータ・ソースの取り込みに使用した構成に応じて変わります。コレクションの文書構造 (抽出する情報を含むフィールドの名前など) について詳しくは、{{site.data.keyword.discoveryshort}} ツールでコレクションを開き、**「データ・スキーマの表示 (View data schema)」**をクリックしてください。

以下の表に、始めに試すのに適したコレクション・フィールドを示します。この提案は、コレクションの作成時にデフォルトの構成テンプレートを使用することが前提となっています。

| データ・ソース・タイプ | タイトル | 本文 | URL |
|--------------------|-------|------|-----|
| アップロードされた PDF 文書 | enriched_text.concepts.text | text | なし |
| Box                | name | description | listing_url |

コレクションが作成されるときに、コレクション・フィールドが作成されます。取り込みにデフォルトの構成テンプレートを使用した場合に生成されるフィールド (`enriched_text.concepts.text` など) について詳しくは、[サービスの構成 > エンリッチメントの追加 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/discovery?topic=discovery-configservice#adding-enrichments) を参照してください。

### アシスタントへのスキルの追加
{: #skill-search-add-to-assistant}

1 つのアシスタントに 1 つのスキルを追加できます。アシスタント・タイルを開いて、アシスタント構成ページからアシスタントにスキルを追加する必要があります。スキル構成ページ内からは、スキルを使用するアシスタントを選択できません。

1 つの検索スキルは、複数のアシスタントで使用できます。

1.  「アシスタント (Assistants)」タブから、スキルを追加するアシスタントのタイルをクリックして開きます。

1.  **「検索スキルの追加 (Add Search Skill)」**をクリックします。

1.  **「既存のスキルの追加 (Add existing skill)」**をクリックします。

    表示されている使用可能なスキルから、追加するスキルをクリックします。

少なくとも 1 つのテスト統合チャネルを構成します。「試行する (Try it out)」ペインからは検索スキルをテストできません。統合チャネルで、検索をトリガーする照会を入力して、スキルをテストします。検索が適切にトリガーされること、および関連する結果が返されることを確認します。

共有可能リンク統合は、検索スキルを使用するアシスタントでは現在機能しません。
{: important}
