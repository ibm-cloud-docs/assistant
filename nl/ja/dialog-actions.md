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

# ダイアログ・ノードからのプログラマチック呼び出しの実行![BETA](images/beta.png)
{: #dialog-actions}

外部アプリケーションまたは外部サービスをプログラマチックに呼び出し、その結果を 1 つのダイアログ・ターンの中で行う処理の一部として受け取るアクションを定義します。
{: shortdesc}

外部サービスを使用して、以下のようなタイプのことを行えます。
- ユーザーから収集した情報を検証する。
- サポートされる SpEL 式メソッドでは複雑すぎて扱えないユーザー入力について計算やストリング処理を行う。
- 外部 Web サービスと対話して情報を取得する。例えば、航空交通サービスから飛行機の到着予定時刻を調べたり、気象サービスから天気予報を入手したりすることもできます。
- レストラン予約サイトなどの外部アプリケーションに要求を送信して、ユーザーの代わりに単純な取り引きを行う。

<iframe class="embed-responsive-item" id="youtubeplayer" title="IBM Cloud Functions の呼び出し" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/y0A6X-KNoB8?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

プログラマチック呼び出しを定義するときには、以下のいずれかのタイプを選択します。

- **client**: 外部クライアント・アプリケーションが理解できる標準化された形式でプログラマチック呼び出しを定義します。クライアント・アプリケーションは、提供された情報を使用してプログラマチック呼び出しまたは関数を実行し、結果をダイアログに返す必要があります。このタイプの呼び出しは基本的に、そこで一時停止してクライアント・アプリケーションが何かの処理に進むまで待機するようダイアログに通知します。クライアント・アプリケーションが実行するプログラムとして、どのようなものでも選択できます。JSON 書式設定ルール (概略を後述) に従って、呼び出し名とパラメーター詳細、エラー・メッセージ変数名を必ず指定してください。

- **cloud_function**: {{site.data.keyword.openwhisk_short}} アクションを直接呼び出し、結果をダイアログに返します。この呼び出しで {{site.data.keyword.openwhisk_short}} 認証鍵を指定する必要があります。(このタイプは以前 **server** という名前でした。タイプ **server** は引き続きサポートされています。)

- **web_action**: {{site.data.keyword.openwhisk_short}} Web アクションを呼び出します。Web アクションはアノテーション付きの {{site.data.keyword.openwhisk_short}} アクションであり、開発者はこれを使用して、Web アプリケーションが {{site.data.keyword.openwhisk_short}} 認証鍵を必要とせずに匿名でアクセスできるバックエンド・ロジックをプログラミングできます。認証は必要ないものの、以下のような方法で Web アクションを保護できます。

  - アクション所有者がいつでも取り消しや変更を行える、Web アクションに固有の認証トークンを使用する
  - {{site.data.keyword.openwhisk_short}} 資格情報を渡す

{{site.data.keyword.openwhisk_short}} アクション・タイプ (web_action と cloud_function または server) はどれでも、コストが発生します。アクションをアクティブにするコストは、アクション呼び出しで指定された資格情報を所有する個人に課金されます。詳しくは、[料金 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/openwhisk/learn/pricing){: new_window} を参照してください。{{site.data.keyword.openwhisk_short}} サービスは、テスト中に「Try it out」ペインから行われた呼び出しと、実稼働環境のアプリケーションから行われた呼び出しを区別しません。 したがって、テスト中に行われる呼び出しで課金が発生する場合があります。
{: note}

## 手順
{: #dialog-actions-call}

ダイアログ・ノードからプログラマチック呼び出しを行うには、次の手順を実行します。

1.  プログラマチック呼び出しを行うダイアログ・ノードで JSON エディターを開きます。

    - ノードへの応答が評価された後に実行するプログラマチック呼び出しを作成するには、そのノード応答の JSON エディターを開きます。

      ![標準ノードの応答に関連付けられている JSON エディターにアクセスする方法を示しています。](images/contextvar-json-response.png)

      ノードの**「Multiple responses」**設定が**「On」**の場合、**「Options」**![高度な応答](images/kabob.png) メニューを表示するためには、**「Edit response」**![Edit response](images/edit-slot.png) アイコンをクリックする必要があります。

      ![複数の条件付き応答が有効になっている標準ノードに関連付けられている JSON エディターにアクセスする方法を示しています。](images/contextvar-json-multi-response.png)

    同じダイアログ・ターンで外部サービスからの応答を表示したり応答に処理を加えたりする場合は、それを行う 2 番目のノードを追加し、現在のノードからそのノードにジャンプする必要があります。
    {: tip}

    - 個々のスロットで使用できる呼び出しを行うには、スロットの**「スロットの編集 (Edit slot)」**アイコン ![「スロットの編集 (Edit slot)」アイコン](images/edit-slot.png) をクリックしてから、以下のいずれかを実行します。

      - スロット条件が true と評価された後に実行するプログラマチック呼び出しを作成するには、そのスロット条件に関連付けられている JSON エディターを開きます。

        ![スロット条件に関連付けられている JSON エディターにアクセスする方法を示しています。](images/contextvar-json-slot-condition.png)

      - スロットへの情報の取り込みが成功した後に実行するプログラマチック呼び出しを作成するには、Found 応答に関連付けられている JSON エディターを開きます。 そのためには、スロットの**「オプション」** ![「オプション」アイコン](images/kabob.png) メニューから**「条件付き応答を有効にする (Enable conditional responses)」**をクリックします。 Found 応答で、**「応答の編集 (Edit response)」** ![応答の編集](images/edit-slot.png) アイコンをクリックします。 Found 応答の**「オプション」** ![「オプション」アイコン](images/kabob.png) メニューから、**「JSON エディターを開く (Open JSON editor)」**をクリックします。

        ![スロットの条件付き応答に関連付けられている JSON エディターにアクセスする方法を示しています。](images/contextvar-json-slot-multi-response.png)

1.  プログラマチック呼び出しを定義するには、次の構文を使用します。

    ```json
    {
      "context": {
        "variable_name" : "variable_value"
      },
      "actions": [
        {
          "name":"<actionName>",
          "type":"client | cloud_function | server | web_action",
          "parameters": {
            "<parameter_name>":"<parameter_value>",
            "<parameter_name>":"<parameter_value>"
          },
          "result_variable": "<result_variable_name>",
          "credentials": "<reference_to_credentials>"
        }
      ],
      "output": {
        "text": "response text"
      }
    }
    ```
    {: codeblock}

    `actions` 配列は、ダイアログから行うプログラマチック呼び出しを指定します。 最大 5 つの独立したプログラマチック呼び出しを定義できます。 以下の名前と値のペアを、JSON 配列で指定します。

    - `<actionName>`: 必須。 呼び出すアクションまたはサービスの名前。 256 文字を超える名前を指定することはできません。

       - クライアント・アクション・タイプの場合は、任意の構文で名前を指定します。 目的は、クライアント・アプリケーションが認識して処理方法を判別できる名前を指定することです。

          例: `calculateRate`

       - cloud_function (または server) タイプおよび web_action タイプの場合は、`/<namespace>/[<package-name>]/<action name>` 構文を使用してアクションの完全修飾名を指定します

         - 標準アクションがパッケージに含まれている場合は、`<package-name>` 情報が必要です。そうでない場合、パッケージ名は不要です。
         - Web アクションがパッケージに含まれている場合は、`<package-name>` 情報が必要です。そうでない場合は、パッケージ名のない Web アクションに適用されるパッケージ名 `default` が必要です。
         - 一連のアクションを呼び出す場合は、`<action name>` ではなく `<sequence name>` を指定します。
         - 通常、ユーザー定義アクションの名前空間の構文は `<myIBMCloudOrganizationID>_<myIBMCloudSpace>` です。 例: `/jdoeorg_prod10/search flights`
         - {{site.data.keyword.openwhisk_short}} に用意されているアクションの名前空間はほとんどが `whisk.system` ですが、名前空間が正しいことを最初に確認してください。例: `/whisk.system/weather/forecast`

           詳しくは、[IBM Cloud Functions の命名指針 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/docs/openwhisk/openwhisk_reference#openwhisk_entities) を参照してください。

    - `<type>`: 実行する呼び出しのタイプを示します。 以下のタイプから選択してください。

      - **client**: 外部クライアント・アプリケーションが理解する標準化された形式のプログラマチック呼び出し情報を含めたメッセージ応答を送信します。クライアント・アプリケーションは、提供された情報を使用してプログラマチック呼び出しまたは関数を実行し、結果をダイアログに返す必要があります。応答本体内の JSON オブジェクトは、呼び出すサービスまたは関数、呼び出しで渡す関連パラメーター、送り返す結果の形式を指定します。

      - **cloud_function**: {{site.data.keyword.openwhisk_short}} アクション (1 つ以上) を直接呼び出します。アクション自体は、{{site.data.keyword.openwhisk}} を使用して別途定義する必要があります。 詳しくは、[アクションの作成](#dialog-actions-create)を参照してください。(このタイプは以前 **server** という名前でした。**server** タイプは引き続きサポートされています。)

      - **web_action**: {{site.data.keyword.openwhisk_short}} Web アクション (1 つ以上) を直接呼び出します。Web アクション自体は、{{site.data.keyword.openwhisk}} を使用して別途定義する必要があります。 詳しくは、[アクションの作成](#dialog-actions-create)を参照してください。

      タイプの指定はオプションです。 デフォルト値は `client` です。

    - `<action_parameters>`: 外部プログラムが予期するパラメーター。JSON オブジェクトとして指定します。パラメーターが必要になるのは、外部プログラムにパラメーターが必要な場合だけです。

    - `<result_variable_name>`: 外部サービスまたは外部プログラムから返された JSON オブジェクトを参照するために使用する名前。 結果は、/message 応答のコンテキスト・セクションに追加されます。 つまり、結果はコンテキスト変数として保管されるので、後でトリガーされたダイアログ・ノードで、ノード応答に表示したり、利用したりできます。 コンテキスト変数の既存の値は、アクションから返された値に上書きされます。 `result_variable_name` は、以下の構文を使用して指定できます。

      - `my_result`
      - `$my_result`

      64 文字を超える名前を指定することはできません。 変数名には、括弧 `()`、大括弧 (`[]`)、単一引用符 (`'`)、引用符 (`"`)、円記号 (`\`) のいずれの文字も含めることはできません。

      /message 応答の出力セクションまたは入力セクションに結果を保存する場合は、以下のいずれかのロケーション・キーワードを `result_variable_name` に接頭部として追加できます。

       - `output.`: /message 応答の出力セクションに結果を追加します (例: `output.my_result`)。
       - `input.`: /message 応答の入力セクションに結果を追加します (例: `input.my_result`)。

      `context.` ロケーション・キーワード接頭辞も指定できます (例: `context.my_result`)。 ただし、デフォルトでは結果はコンテキストに追加されるので、指定する必要はありません。

      変数名にピリオドを指定して、ネストされた JSON オブジェクトを作成できます。 例えば、次の変数を定義して、今日と明日の天気予報を求める、気象サービスに対する 2 つの別々の要求の結果を取り込むことができます。

      - `context.weather.today`
      - `context.weather.tomorrow`

      結果 (`temp` と `rain` パラメーター値) は、以下の構造でコンテキストに保管されます。

      ```json
      {
        "weather": {
          "today": {
            "temp": "20",
            "rain": "30"
          },
          "tomorrow": {
            "temp": "23",
            "rain": "80"
          }
        }
      }
      ```
      {: codeblock}

      単一の JSON アクション配列に指定した複数のアクションが、プログラマチック呼び出しの結果を同じコンテキスト変数に追加する場合は、コンテキストが更新される順序が重要になります。

      1.  配列内にサーバー (cloud_function または web_action) アクションとクライアント・アクションを組み合わせて指定した場合、本サービスはサーバー・タイプ (cloud_function または web_action) のアクションを最初に処理します。そのため、サーバー・タイプのアクションで計算されたコンテキスト変数の値は、配列内の最後のクライアント・タイプのアクションで計算された値に上書きされます。

      1.  アクション・タイプごとに、アクションを配列内に定義した順序によって、コンテキスト変数の値が設定される順序が決まります。 配列内の最後のアクションから返されるコンテキスト変数値が、それ以外のアクションで計算される値を上書きします。

    - `<reference_to_credentials>`: {{site.data.keyword.openwhisk_short}} 資格情報が保管されているオブジェクトの名前。 server タイプまたは cloud_function タイプのアクションの場合のみ必要です。

      これらの資格情報は、アクションを実行する {{site.data.keyword.openwhisk_short}} インスタンスにアクセスするために使用されます。 これらは {{site.data.keyword.Bluemix_notm}} の資格情報ではありません。

      資格情報を調べるには、次の手順を実行します。
      1.  [{{site.data.keyword.openwhisk_short}} API キー ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/openwhisk/learn/api-key){: new_window} ページにアクセスします。

          - まだアカウントを作成していない場合は、作成します。
          - まだログインしていない場合は、ログインします。

      1.  **「認証キーの表示 (Show Auth Key)」**アイコン ![認証キーの表示](images/show-auth-icon.png) をクリックして、資格情報を表示します。 コロン (:) の前のセグメントはユーザー ID です。 コロンの後のセグメントはパスワードです。

      アクションの実行時に発生したすべての料金は、これらの資格情報を所有するユーザーに課金されます。
      {: note}

      組み込み統合を使用してアシスタントをデプロイする場合、ダイアログに資格情報を渡す方法はありません。それらは、プログラマチック呼び出し自体が行われる前にトリガーされるダイアログ・ノードにコンテキスト変数値として格納する必要があります。結果として、スキルを表す JSON ファイルで資格情報を参照できるようになり、その JSON ファイルは、スキルにアクセスできる誰もがダウンロードできます。
      {: important}

      メッセージ・コンテキストの $private セクション内にコンテキスト変数をネストすると、この情報が Watson ログに保管されることを防止できます (例: `$private.my_credentials`)。 ただし、プライベート・オブジェクトに資格情報を保管した場合は、ログにおいてのみ隠蔽されます。情報は依然として、基礎となる JSON オブジェクトに保管されています。この情報がクライアント・アプリケーションにさらされないようにしてください。

      資格情報を保護するために以下のアプローチのいずれかを使用することを検討してください。

      - カスタム・クライアント・アプリケーションを使用する場合は、クライアント・アプリケーションが API を直接呼び出せないアーキテクチャーを実装する。例えば、{{site.data.keyword.conversationshort}} API REST エンドポイントを呼び出し、アプリ・サーバーからクライアント・アプリケーションに JSON 出力オブジェクトのみを渡すアプリ・サーバーを使用します。
      - 認証なしで Web アクションを使用する。
      - Web アクションに固有のトークンのみを使用して呼び出しを認証することによって機密漏れを制限する。

      定義する資格情報オブジェクトには、有効な {{site.data.keyword.openwhisk_short}} 資格情報が含まれていなければなりません。それらをどのように指定するかは、本サービスがホストされているロケーションとそのロケーションでインスタンスが使用する認証方式によって異なります。以下のような方法があります。

      ```json
      {
        "user":"5tj3b41j-bf3j-5d92-24g9-4a7769ab12af",
        "password":"y65gqSTSRzqE..."
      }
      ```
      {: codeblock}

      or

      ```json
      {
        "api_key":"user:password"
      }
      ```
      {: codeblock}

      ダイアログのテスト中は、ツールの「Try it out」ペインで**「コンテキストの管理 (Manage context)」**をクリックして、`$ private.my_credentials` コンテキスト変数に実際の {{site.data.keyword.openwhisk_short}} ユーザー名とパスワードの値を一時的に設定できます。

      ![$private.my_credentials コンテキスト変数を「Try it out」コンテキスト管理インターフェースで定義する方法を示しています](images/testing-creds.png)

      {{site.data.keyword.openwhisk_short}} は、テスト中に「Try it out」ペインから行われた呼び出しと、実稼働環境のアプリケーションから行われた呼び出しを区別しません。 テスト中に行われる呼び出しで課金が発生する場合があります。
      {: note}

## アクションの作成
{: #dialog-actions-create}

アクションまたは Web アクション・タイプのプログラマチック呼び出しを定義することを選択した場合、それをダイアログから呼び出すためには、その前に {{site.data.keyword.openwhisk}} で作成しておく必要があります。クライアント・タイプのプログラマチック呼び出しを定義する場合、この手順はスキップしてください。

**ロケーション制限**: 現在、{{site.data.keyword.openwhisk_short}} アクションを呼び出せるのは、ダラス、フランクフルト、ロンドン、ワシントン DC のロケーションにあるデータ・センターでホストされている {{site.data.keyword.conversationshort}} サービス・インスタンスからのみです。{{site.data.keyword.conversationshort}} サービスは、同じロケーションでホストされている {{site.data.keyword.openwhisk_short}} インスタンスのみ使用します。他のロケーションでホストされている {{site.data.keyword.openwhisk_short}} インスタンスは検査しません。したがって、例えば、あるアクションが、ワシントン DC でホストされている {{site.data.keyword.openwhisk_short}} インスタンスで定義されている場合、ダラスでホストされている {{site.data.keyword.conversationshort}} サービス・インスタンスからそのアクションを呼び出さないでください。ロンドンで 2018 年 12 月 13 日より前に作成された {{site.data.keyword.conversationshort}} サービス・インスタンスは、ダラス・データ・センターにシンジケートされたということに留意してください。そのようなインスタンスが見つけることができるのは、ダラスでホストされている {{site.data.keyword.openwhisk_short}} アクションのみです。
{: important}

**時間制限**: **cloud_function**、**server**、**web_action** の各タイプは、**5 秒未満**で返すことがわかっている呼び出しを行う場合にのみ使用してください。1 回のサービス呼び出しにこれ以上の時間がかかると、{{site.data.keyword.openwhisk_short}} への要求はタイムアウトになります。 また、ダイアログで外部サービスへの呼び出しを複数回行う場合は、それらの呼び出しの合計実行時間が 7 秒以下でなければなりません。 最初の 3 回の呼び出しがそれぞれ 2 秒で完了し、4 回目の呼び出しが 1 秒を超えた場合、4 回目の呼び出しは停止され、その呼び出しが完了しなかったことを示すエラー・メッセージが返されます。 あまり効率的でないサービスを呼び出す必要がある場合は、クライアント・アプリケーションで呼び出しを処理し、別のステップとして情報をダイアログに渡してください。

{{site.data.keyword.openwhisk_short}} アクションを作成するには、次の手順を実行します。

1.  [オンライン {{site.data.keyword.openwhisk_short}} エディター ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/openwhisk/create){: new_window} にアクセスします。ここで、ブラウザーにコードを直接書き込むことができます。

    インストール可能な[コマンド・ライン・インターフェース![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/openwhisk/learn/cli){: new_window} もあり、ローカルで作成したコードを使用してアクションを定義できます。

1.  以下のいずれかのタイプのアクションを作成します。

    - **{{site.data.keyword.openwhisk_short}} アクション**: 詳しくは、[アクションの作成と呼び出し ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/openwhisk?topic=cloud-functions-openwhisk_actions){: new_window} を参照してください。
    - **{{site.data.keyword.openwhisk_short}} Web アクション**: 詳しくは、[Web アクションの作成 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/openwhisk?topic=cloud-functions-openwhisk_webactions){: new_window} を参照してください。

    以下のヒントを念頭に置いてください。

    - 外部サービスの呼び出し方法については、[{{site.data.keyword.openwhisk_short}} アクションの例 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/docs/openwhisk/openwhisk_actions#openwhisk_apicall_action){: new_window} を参照してください。
    - Watson サービスを呼び出すには、使用する言語用の [Watson Developer Cloud SDK ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/watson-developer-cloud){: new_window} を使用してください。
    - {{site.data.keyword.openwhisk_short}} アクションは、入力パラメーターを JSON オブジェクトとして受け入れ、出力を JSON オブジェクトとして返すようにしてください。
    - Node.js を使用して {{site.data.keyword.openwhisk_short}} アクションを作成する場合は、非同期処理に必ず `Promise` を使用してください。 また、最終結果は `main` 関数から返すようにしてください。

    また、複数のアクションで構成されるシーケンスを作成することもできます。
    {: tip}

## エラーの処理
{: #dialog-actions-handle-errors}

{{site.data.keyword.openwhisk_short}} アクションでエラーが発生した場合、エラー・メッセージがダイアログに返され、`cloud_functions_call_error` という名前の応答変数のプロパティーとして保管されます。 エラーは、{{site.data.keyword.openwhisk_short}} アクションが外部サービスから応答を取得できない場合や Cloud Function アクションが失敗した場合などに発生する可能性があります。 Cloud Function の資格情報が指定されていない場合や正しくない場合は、エラーが返されます。 このコンテキスト変数はサーバー・アクションでのみ使用されます。クライアント・アプリケーションの場合は、エラー情報をキャプチャーしてコンテキスト変数としてダイアログに返す、類似したオブジェクトを作成することを検討してください。

エラーがないか最初にチェックするように、ダイアログ・ノードの応答を条件付けることができます。 例えば、次の式を応答条件に追加すると、エラーが発生しなかった場合にのみ、{{site.data.keyword.openwhisk_short}} アクションの結果を参照する応答を表示できます。

```bash
  $forecast_result.cloud_functions_call_error == null
```
{: codeblock}

クライアント・タイプのプログラマチック呼び出しの場合は、`action_error` などのコンテキスト変数を定義して、エラー処理に関する情報を渡すことができます。 結果変数の一部として、これをサービスに返すことができます。 そして、以下のような応答条件を定義して、エラーが発生しなかった場合にのみ、応答を表示できます。

```bash
  $forecast_result.action_error == null
```
{: codeblock}

## クライアント呼び出しの例
{: #dialog-actions-client-example}

以下の例は、外部の気象サービスに対する呼び出しを示しています。 これは、ノード応答に関連付けられた JSON エディターに追加されます。 ノード・レベルの応答がトリガーされるまで、スロットがユーザーから日付と場所の情報を収集して保管します。 この例では、呼び出されるサービスにある `/weather` という名前のエンドポイントが、`location` パラメーターと `date` パラメーターを取り、JSON オブジェクト `{"forecast": "<value>"}` を返すことを想定しています。

 ``` json
{
  "actions": [
    {
      "name": "MyWeatherFunction",
      "type": "client",
      "parameters": {
        "date": "$date",
        "location": "$location"
      },
      "result_variable": "context.my_forecast"
    }
  ]
}
```
{: codeblock}

通常、サービスが POST /message 要求からクライアントに戻るのは、新しいユーザー入力が必要な場合のみです。例えば、親ノードを実行した後の子ノードのいずれかを実行する前などに戻ります。 ただし、クライアント・アクションをノードに追加した場合、サービスは、アクション呼び出しの結果を返すために、評価の後に必ずクライアントに戻ります。 子ノードに直接ジャンプするように構成したノードのように、ユーザー入力を待機すべきでない場合は、待機しないようにするために、サービスのメッセージ・コンテキストに次の値を追加します。

```json
  {
    "context": {
      "skip_user_input": true
    }
  }
```
{: codeblock}

クライアントでアクションを実行し、ユーザー入力を取得しないようにする場合も、同じ規則に従い、親ノードに「skip_user_input」 コンテキスト変数を追加して、クライアント・アプリケーションにそのことを伝えることができます。

クライアント・アプリケーションは、必ず、コンテキストに「skip_user_input」変数があるかどうかをチェックする必要があります。 この変数がある場合は、ユーザーに新規入力を要求しないこと、代わりに、アクションを実行し、結果をメッセージに追加してサービスに返すことがわかります。 新しい POST メッセージ要求には、前の POST メッセージ応答 (つまり、コンテキスト、入力、インテント、エンティティー、およびオプションで出力セクション) で返されたメッセージが含まれている必要があります。また、実行するプログラマチック呼び出しを定義する JSON オブジェクトではなく、プログラマチック呼び出しから返された結果が含まれている必要があります。

このノードの後にジャンプする子ノードに、ユーザーに表示する応答を追加します。

``` json
{
  "output": {
    "text": {
      "values": [
        "It will be $my_forecast $date.literal in $location.literal."
      ]
    }
  }
```
{: codeblock}

次の図は、クライアント呼び出しを使用して天気予報の情報を取得し、ユーザーに返す方法を示しています。

![天気予報を尋ねると、ダイアログが要求をクライアント・アプリに送信し、クライアント・アプリがそれを外部サービスに送信することを示しています](images/forecast.png)

## IBM Cloud Functions アクション呼び出しの例
{: #dialog-actions-server-example}

以下の例は、{{site.data.keyword.openwhisk_short}} アクションの呼び出しがどのようなものかを示しています。この例は、サービスに用意されている [Utilities パッケージ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/docs/openwhisk/openwhisk_actions#openwhisk_create_action_sequence){: new_window} に定義されている {{site.data.keyword.openwhisk_short}} の「echo」アクションの使用方法を示しています。 このアクションは、テキスト・ストリングを取り、それを返します。

``` json
{
  "actions": [
    {
      "name": "/whisk.system/utils/echo",
      "type":"cloud_function",
      "parameters": {
        "message": "<?input.text?>"
      },
      "result_variable": "context.my_input_returned",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

これで、「context.my_input_returned」変数に保管された {{site.data.keyword.openwhisk_short}} アクションの出力に、後続のダイアログ・ノードがアクセスできるようになります。

``` json
 {
  "output": {
    "text": {
      "values": [
        "Your input was: $my_input_returned."
      ]
    }
  }
}
```
{: codeblock}

次の図は、{{site.data.keyword.openwhisk_short}} の組み込みの echo サービスを呼び出す単純な例を使用して、{{site.data.keyword.openwhisk_short}} アクションを呼び出す方法を示しています。 ユーザーに入力を要求し、その入力を echo サービスに渡します。 echo サービスが同じテキストを返すと、それがユーザーに表示されます。

![テキストを入力すると、ダイアログがその要求をサービスに送信し、サービスがダイアログにそのテキストを返すことを示しています。](images/echo-via-cf.png)

### Echo アクションの例
{: #dialog-actions-echo-example}

{{site.data.keyword.openwhisk_short}} の組み込み Echo アクションを呼び出すように既にセットアップされているダイアログがあるダイアログ・スキルを表示するには、以下の手順を実行します

1.  [CloudFunctionsEcho.json ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://raw.githubusercontent.com/watson-developer-cloud/community/master/watson-assistant/cloud-functions-echo.json){: new_window} ファイルをダウンロードします。
1.  この JSON ファイルを新規ダイアログ・スキルとしてインポートします。
1.  ダイアログを参照して、Echo アクションに対する呼び出しがどのように指定されているかを確認します。
1.  「Try it out」ペインで「コンテキストの管理 (Manage context)」をクリックし、コンテキスト変数に {{site.data.keyword.openwhisk_short}} のユーザー名とパスワードを (一時的に) 設定します。

    ```json
    $private.my_credentials
    {
      "user":"<your-CF-instance-username>",
      "password":"<your-CF-instance-password>"
    }
    ```
    {: codeblock}

    or

    ```json
    $private.my_credentials
    {
      "api_key":"user:password"
      }
    ```
    {: codeblock}

1.  何かを入力して、ダイアログをテストします。

    サービスが {{site.data.keyword.openwhisk_short}} の Echo アクションを使用して入力内容を繰り返します。

## IBM Cloud Functions Web アクション呼び出しの例
{: #dialog-actions-web-action-example}

次の例は、Web アクションの呼び出しがどのようになるかを示しています。この例は、ユーザー名を単純なグリーティング・サービスに渡す方法を示しています。このサービスはそのユーザー宛てのあいさつを名前を指定して返します。

  ``` json
  {
    "actions": [
    {
        "name": "jdoe@example.com_sales/default/hello",
        "type":"web_action",
        "parameters": {
          "name": "<? context['name'] ?>"
        },
        "result_variable": "context.greet_user",
        "credentials": "<See below>"
      }
    ]
  }
  ```
  {: codeblock}

  ここで、`"credentials"` は以下のいずれかの方法で指定します。

  - 認証なし:

    ```json
    "credentials": null
    ```
    {: screen}

  - {{site.data.keyword.openwhisk_short}} 資格情報で認証

    ```json
    "credentials": "$private.my_credentials"
    ```
    {: screen}

    ここで、`$private.my_credentials` は以下のように定義します。

    ```json
    $private.my_credentials
    {
      "user":"<your-CF-instance-username>",
      "password":"<your-CF-instance-password>"
    }
    ```
    {: screen}

    or 

    ```json
    $private.my_credentials
    {
      "api_key":"username:password"
    }
    ```
    {: screen}
  
  - Web アクションに固有のトークンで認証

    ```json
    "credentials": "$private.my_credentials"
    ```
    {: screen}

    ここで、`$private.my_credentials` は以下のように定義します。

    ```json
    $private.my_credentials
    {
      "web_action_key":"<action-secret>"
    }
    ```
    {: screen}

これで、`context.greet_user` 変数に格納された Web アクションの出力に、後続のダイアログ・ノードがアクセスできるようになります。

``` json
 {
  "output": {
    "text": {
      "values": [
        "$greet_user"
      ]
    }
  }
}
```
{: codeblock}

## 高度な IBM Cloud Functions アクション呼び出しの例
{: #dialog-actions-advanced-example}

1 つのダイアログ・フローから複数のアクションを呼び出すことができます。 実際には、1 つのダイアログ・ノードの 1 つの JSON オブジェクト `actions` で最大 5 つのアクションを呼び出すことができます。 ただし、JSON 配列 `actions` に定義したサーバー・タイプのアクションは、すべて並行して処理されます。 したがって、サーバー・タイプのアクションを 1 つ呼び出した後に、その結果を同じ `actions` ブロック内の 2 番目のサーバー・タイプのアクションに渡すことはできません。 特定の順序でサーバー・アクションを呼び出す最善の方法は、{{site.data.keyword.openwhisk_short}} シーケンスを使用する方法です。 実行時には、ダイアログは外部呼び出しを 1 つ実行するだけで複数のアクションを完了できるので、この方法は高速です。 シーケンスを使用するには、`actions` ブロック定義でアクション名の代わりにシーケンス名を参照するだけです。 代わりに、あるノードから最初のサーバー・タイプのアクションを呼び出した後に、次のサーバー・タイプのアクションを呼び出す子ノードにジャンプするという方法もあります。

クライアント・タイプとサーバー・タイプのアクションが混在する 1 つの `actions` 配列を定義した場合、ダイアログ・ノードを実行すると、サーバー・タイプのすべてのアクションの処理が完了して初めて、クライアント・アクションがクライアントに送信されます。
{: tip}

次の例は、{{site.data.keyword.openwhisk_short}} アクションに対する呼び出しを示しています。

この例は、{{site.data.keyword.openwhisk_short}} アクションを使用して、都市名を取り、その指定された場所の緯度と経度の座標を返す外部サービスを呼び出す方法を示しています。

``` json
{
  "actions": [
    {
      "name": "/jdoeorg_prod/get coordinates",
      "type":"cloud_function",
      "parameters": {
        "location": "$location"
      },
      "result_variable": "context.my_coordinates",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

`$my_coordinates` コンテキスト変数は、`get coordinates` サービスから返される 2 つの値を次のように保存します。

```json
{
 "lat":"42.3611",
 "long":"-71.0571"
}
```
{: codeblock}

この例は、{{site.data.keyword.openwhisk_short}} サービスに用意されている [Weather パッケージ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/docs/openwhisk/openwhisk_weather#openwhisk_catalog_weather){: new_window} で定義されている {{site.data.keyword.openwhisk_short}} `forecast` アクションの使用方法を示しています。このアクションには、緯度と経度の座標、および期間が必要です。 指定した期間にわたる指定した場所の天気予報情報を含む JSON オブジェクトが返されます。 前のアクションから返された座標を、`$my_coordinates.lat` および `$my_coordinates.long` として指定しています。

``` json
{
  "actions": [
    {
      "name": "/whisk.system/weather/forecast",
      "type":"cloud_function",
      "parameters": {
        "latitude": "$my_coordinates.lat",
        "longitude": "$my_coordinates.long",
        "timePeriod": "$period",
        "username": "$private.my_weather_service.username",
        "password": "$private.my_weather_service.password"
      },
      "result_variable": "context.forecasts",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

ユーザー名とパスワードをパラメーターとしてリストしています。 これらを指定しているのは、この特定のアクションのためにこれらのパラメーターが必要だからです。指定したアクションがバックエンドで呼び出す外部 Weather サービスに必要な資格情報を定義しています。 これらは IBM Cloud Function のアカウントの資格情報とは異なります。 これらの資格情報を保護するための手段を講じてください。
{: note}

これで、`context.forecasts` 変数に格納された {{site.data.keyword.openwhisk_short}} アクションの出力に、後続のダイアログ・ノードがアクセスできるようになります。

``` json
 {
  "output": {
    "text": {
      "values": [
        "For the next $period in $location, you can expect $forecasts."
      ]
    }
  }
}
```
{: codeblock}

次の図は複雑な対話を示しています。 ユーザーが天気予報を尋ねます。 ダイアログ・ノードのスロットが、場所と期間の情報を入力するようにユーザーに要求します。 {{site.data.keyword.openwhisk_short}} 予報サービスを呼び出すには、地理座標を提供する必要があります。 そのため、ダイアログはまず、ユーザーから提供された場所の緯度と経度の詳細を取得します。そのために、{{site.data.keyword.openwhisk_short}} サービスに要求を送信すると、そのサービスが、座標情報を返す外部サービスにその要求を渡します。 後続のノードで、ダイアログは、新しく取得した座標情報とユーザーから取得した期間情報を、{{site.data.keyword.openwhisk_short}} の組み込みの予測サービスに送信して、予報の詳細を取得します。 そして、ダイアログは、{{site.data.keyword.openwhisk_short}} 予報サービスから提供された予測結果を示して、ユーザーの質問に答えます。

![天気予報を尋ねると、ダイアログがその要求を Cloud Function サービスに送信して、そこから外部サービスに渡されることを示しています。](images/forecast-via-cf.png)
