---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-15"

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

# ダイアログ・ノードからのプログラマチック呼び出しの実行![BETA](images/beta.png)
{: #dialog-actions}

外部アプリケーションまたは外部サービスをプログラマチックに呼び出し、その結果を 1 つのダイアログ・ターンの中で行う処理の一部として受け取るアクションを定義します。
{: shortdesc}

外部サービスを使用することで、ユーザーから収集した情報を検証したり、サポートされている SpEL 式やメソッドでは複雑すぎて処理できない入力に対して計算やストリング操作を実行したりできます。また、外部 Web サービスと対話して情報を取得することもできます。例えば、航空会社のサービスと対話してフライトの到着予定時刻を調べたり、気象サービスと対話して天気予報を調べたりできます。レストラン予約サイトなどの外部アプリケーションと対話して、ユーザーの代わりに単純な取り引きを行うこともできます。

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/y0A6X-KNoB8?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

プログラマチック呼び出しを定義するときには、以下のいずれかのタイプを選択します。

- **クライアント**: 外部クライアント・アプリケーションがプログラマチックに呼び出しまたは関数を実行してダイアログに結果を返せるように標準化された形式で、プログラマチック呼び出しを定義します。
- **サーバー**: {{site.data.keyword.openwhisk_short}} アクションを直接呼び出し、その結果をダイアログに返します。

    現在、{{site.data.keyword.openwhisk_short}} アクションは、米国南部地域またはドイツ地域でホストされている {{site.data.keyword.conversationshort}} インスタンスから呼び出すことができます。

    **注意**: 使用される {{site.data.keyword.openwhisk_short}} インスタンスは、同じ場所 (米国南部またはドイツ) でホストされているインスタンスです。したがって、例えば、米国南部でホストされている {{site.data.keyword.conversationshort}} サービス・インスタンスからアクセスするアクションを、ドイツでホストされている {{site.data.keyword.openwhisk_short}} インスタンスに定義しないでください。

    **重要**: この方法は、**5 秒未満**で返すことがわかっている {{site.data.keyword.openwhisk_short}} アクションを呼び出す場合にのみ使用してください。1 回のサービス呼び出しにこれ以上の時間がかかると、{{site.data.keyword.openwhisk_short}} への要求はタイムアウトになります。また、ダイアログで外部サービスへの呼び出しを複数回行う場合は、それらの呼び出しの合計実行時間が 7 秒以下でなければなりません。最初の 3 回の呼び出しがそれぞれ 2 秒で完了し、4 回目の呼び出しが 1 秒を超えた場合、4 回目の呼び出しは停止され、その呼び出しが完了しなかったことを示すエラー・メッセージが返されます。あまり効率的でないサービスを呼び出す必要がある場合は、クライアント・アプリケーションで呼び出しを処理し、別のステップとして情報をダイアログに渡してください。

## 手順
{: #call-action}

ダイアログ・ノードからプログラマチック呼び出しを行うには、次の手順を実行します。

1.  プログラマチック呼び出しを行うダイアログ・ノードで JSON エディターを開きます。

    - ノードへの応答が評価された後に実行されるプログラマチック呼び出しを作成するには、そのノード応答の JSON エディターを開きます。

      ![標準ノードの応答に関連付けられている JSON エディターにアクセスする方法を示しています。](images/contextvar-json-response.png)

      ノードの**「複数の応答 (Multiple responses)」**設定が**「オン (On)」**の場合は、**「応答の編集 (Edit response)」** ![応答の編集](images/edit-slot.png) アイコンをクリックしないと **「オプション」**  ![高度な応答](images/kabob.png) メニューは表示されません。

      ![複数の条件付き応答が有効になっている標準ノードに関連付けられている JSON エディターにアクセスする方法を示しています。](images/contextvar-json-multi-response.png)

    同じダイアログ・ターンで外部サービスからの応答を表示したり応答に処理を加えたりする場合は、それを行う 2 番目のノードを追加し、現在のノードからそのノードにジャンプする必要があります。
{: tip}

    - 個々のスロットで使用できる呼び出しを行うには、スロットの**「スロットの編集 (Edit slot)」**アイコン ![「スロットの編集 (Edit slot)」アイコン](images/edit-slot.png) をクリックしてから、以下のいずれかを実行します。

      - スロット条件が true と評価された後に実行されるプログラマチック呼び出しを作成するには、そのスロット条件に関連付けられている JSON エディターを開きます。

        ![スロット条件に関連付けられている JSON エディターにアクセスする方法を示しています。](images/contextvar-json-slot-condition.png)

      - スロットへの情報の取り込みが成功した後に実行されるプログラマチック呼び出しを作成するには、Found 応答に関連付けられている JSON エディターを開きます。そのためには、スロットの**「オプション」** ![「オプション」アイコン](images/kabob.png) メニューから**「条件付き応答を有効にする (Enable conditional responses)」**をクリックします。Found 応答で、**「応答の編集 (Edit response)」** ![応答の編集](images/edit-slot.png) アイコンをクリックします。Found 応答の**「オプション」** ![「オプション」アイコン](images/kabob.png) メニューから、**「JSON エディターを開く (Open JSON editor)」**をクリックします。

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
          "type":"client | server",
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

    `actions` 配列は、ダイアログから行うプログラマチック呼び出しを指定します。最大 5 つの独立したプログラマチック呼び出しを定義できます。以下の名前と値のペアを、JSON 配列で指定します。

    - `<actionName>`: 必須。呼び出すアクションまたはサービスの名前。64 文字を超える名前を指定することはできません。

       - クライアント・アクション・タイプの場合は、任意の構文で名前を指定します。目的は、クライアント・アプリケーションが認識して処理方法を判別できる名前を指定することです。

          例: `calculateRate`

       - {{site.data.keyword.openwhisk_short}} (サーバー) アクション・タイプの場合は、次の構文を使用してアクションの完全修飾名を指定します。`/<namespace>/[<package-name>]/<action name>`

         - アクションがパッケージに含まれている場合は、`<package-name>` 情報が必要です。そうでない場合、この情報は不要です。
         - 一連のアクションを呼び出す場合は、`<action name>` ではなく `<sequence name>` を指定します。
         - 通常、ユーザー定義アクションの名前空間の構文は `<myIBMCloudOrganizationID>_<myIBMCloudSpace>` です。例: `/jdoeorg_prod10/search flights`
         - {{site.data.keyword.openwhisk_short}} に用意されているアクションの名前空間はほとんどが `whisk.system` ですが、必ず名前空間が正しいことを確認してください。 例: `/whisk.system/weather/forecast`

    - `<type>`: 実行する呼び出しのタイプを示します。以下のタイプから選択してください。


      - **クライアント**: 外部クライアント・アプリケーションが呼び出しや関数を実行してダイアログの代わりに結果を取得できるように標準化された形式のプログラマチック呼び出し情報を含む、メッセージ応答を送信します。応答の本文の JSON オブジェクトによって、呼び出すサービスまたは関数、呼び出しで渡す関連パラメーター、結果を戻す方法を指定します。

      - **サーバー**: {{site.data.keyword.openwhisk_short}} アクション (1 つ以上) を直接呼び出します。アクション自体は、{{site.data.keyword.openwhisk}} を使用して別途定義する必要があります。詳しくは、後述の [{{site.data.keyword.openwhisk_short}} アクションの作成](dialog-actions.html#create-action)を参照してください。

      タイプの指定はオプションです。デフォルト値は `client` です。

    - `<action_parameters>`: 外部プログラムが予期する任意のパラメーター。JSON オブジェクトとして指定します。パラメーターが必要になるのは、外部プログラムにパラメーターが必要な場合だけです。

    - `<result_variable_name>`: 外部サービスまたは外部プログラムから返された JSON オブジェクトを参照するために使用する名前。結果は、/message 応答のコンテキスト・セクションに追加されます。つまり、結果はコンテキスト変数として保管されるので、後でトリガーされたダイアログ・ノードで、ノード応答に表示したり、利用したりできます。コンテキスト変数の既存の値は、アクションから返された値に上書きされます。 `result_variable_name` は、以下の構文を使用して指定できます。

      - `my_result`
      - `$my_result`

      64 文字を超える名前を指定することはできません。変数名に、括弧`()`、大括弧 (`[]`)、単一引用符 (`'`)、引用符 (`"`)、円記号 (\) を含めることはできません。

      /message 応答の入出力セクションに結果を保存する場合は、以下のいずれかのロケーション・キーワードを `result_variable_name` の前に付加することができます。

       - `output.`: /message 応答の出力セクションに結果を追加します (例: `output.my_result`)。
       - `input.`: /message 応答の入力セクションに結果を追加します (例: `input.my_result`)。

      `context.` ロケーション・キーワード接頭辞も指定できます (例: `context.my_result`)。ただし、デフォルトでは結果はコンテキストに追加されるので、指定する必要はありません。

      変数名にピリオドを指定して、ネストされた JSON オブジェクトを作成できます。例えば、次の変数を定義して、今日と明日の天気予報を求める、気象サービスに対する 2 つの別々の要求の結果を取り込むことができます。

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

      1. 配列内にサーバー・アクションとクライアント・アクションを組み合わせて指定した場合、サービスはサーバー・タイプのアクションを先に処理します。 そのため、サーバー・タイプのアクションで計算されたコンテキスト変数の値は、配列内の最後のクライアント・タイプのアクションで計算された値に上書きされます。
      1. アクション・タイプごとに、アクションを配列内に定義した順序によって、コンテキスト変数の値が設定される順序が決まります。配列内の最後のアクションから返されるコンテキスト変数値が、それ以外のアクションで計算される値を上書きします。

    - `<reference_to_credentials>`: {{site.data.keyword.openwhisk_short}} 資格情報が保管されているオブジェクトの名前。サーバー・アクションの場合にのみ必要です。これらの資格情報は、アクションを実行する {{site.data.keyword.openwhisk_short}} インスタンスにアクセスするために使用されます。これらは {{site.data.keyword.Bluemix_notm}} の資格情報ではありません。

      資格情報を調べるには、次の手順を実行します。
      1.  [{{site.data.keyword.openwhisk_short}} API キー ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/openwhisk/learn/api-key){: new_window} ページにアクセスします。

          - まだアカウントを作成していない場合は、作成します。
          - まだログインしていない場合は、ログインします。

      1.  **「認証キーの表示 (Show Auth Key)」**アイコン ![認証キーの表示](images/show-auth-icon.png) をクリックして、資格情報を表示します。コロン (:) の前のセグメントはユーザー ID です。コロンの後のセグメントはパスワードです。

      **注意**: アクションの実行時に発生したすべての料金は、これらの資格情報を所有するユーザーに課金されます。

      資格情報を保護するために、資格情報は {{site.data.keyword.conversationshort}} ワークスペースに保管しないでください。代わりに、コンテキストの一部としてクライアント・アプリケーションから渡してください。メッセージ・コンテキストの $private セクション内にコンテキスト変数をネストすると、この情報が Watson ログに保管されることを防止できます (例: `$private.my_credentials`)。

      定義する資格情報オブジェクトには、パラメーター `user` および `password` が含まれている必要があります。

      ```json
      {
        "user":"5tj3b41j-bf3j-5d92-24g9-4a7769ab12af",
        "password":"y65gqSTSRzqE..."
      }
      ```
      {: codeblock}

      ダイアログのテスト中は、ツールの「Try it out」ペインで**「コンテキストの管理 (Manage context)」**をクリックして、`$ private.my_credentials` コンテキスト変数に実際の {{site.data.keyword.openwhisk_short}} ユーザー名とパスワードの値を一時的に設定できます。

      ![$private.my_credentials コンテキスト変数を「Try it out」コンテキスト管理インターフェースで定義する方法を示しています](images/testing-creds.png)

## {{site.data.keyword.openwhisk_short}} アクションの作成
{: #create-action}

サーバー・タイプのプログラマチック呼び出しを定義する場合は、ダイアログから呼び出す前に、{{site.data.keyword.openwhisk}} でアクションを作成する必要があります。 クライアント・タイプのプログラマチック呼び出しを定義する場合、この手順はスキップしてください。

**注意:** {{site.data.keyword.openwhisk_short}} アクションを実行すると、料金が発生する場合があります。詳細については、[料金 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/openwhisk/learn/pricing){: new_window} を参照してください。 {{site.data.keyword.openwhisk_short}} は、テスト中に「Try it out」ペインから行われた呼び出しと、実稼働環境のアプリケーションから行われた呼び出しを区別しません。

{{site.data.keyword.openwhisk_short}} アクションを作成するには、次の手順を実行します。

1.  [オンライン {{site.data.keyword.openwhisk_short}} エディター ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.ng.bluemix.net/openwhisk/create){: new_window} にアクセスします。ここで、ブラウザーにコードを直接書き込むことができます。

    インストール可能な[コマンド・ライン・インターフェース![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/openwhisk/learn/cli){: new_window} もあり、ローカルで作成したコードを使用してアクションを定義できます。

1.  サポートされるいずれかのプログラミング言語を使用して、{{site.data.keyword.openwhisk_short}} アクションを作成します。詳しくは、[{{site.data.keyword.openwhisk_short}} の資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.ng.bluemix.net/docs/openwhisk/openwhisk_actions.html){: new_window} を参照してください。

    以下のヒントを念頭に置いてください。

    - 外部サービスの呼び出し方法については、[{{site.data.keyword.openwhisk_short}} アクションの例 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.ng.bluemix.net/docs/openwhisk/openwhisk_actions.html#openwhisk_apicall_action){: new_window} を参照してください。
    - Watson サービスを呼び出すには、使用する言語用の [Watson Developer Cloud SDK ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/watson-developer-cloud){: new_window} を使用してください。
    - {{site.data.keyword.openwhisk_short}} アクションは、入力パラメーターを JSON オブジェクトとして受け入れ、出力を JSON オブジェクトとして返すようにしてください。
    - Node.js を使用して {{site.data.keyword.openwhisk_short}} アクションを作成する場合は、非同期処理に必ず `Promise` を使用してください。 また、最終結果は `main` 関数から返すようにしてください。

    また、複数のアクションで構成されるシーケンスを作成することもできます。
{: tip}

## エラーの処理

{{site.data.keyword.openwhisk_short}} アクションでエラーが発生した場合、エラー・メッセージがダイアログに返され、`cloud_functions_call_error` という名前の応答変数のプロパティーとして保管されます。エラーは、{{site.data.keyword.openwhisk_short}} アクションが外部サービスから応答を取得できない場合や Cloud Function アクションが失敗した場合などに発生する可能性があります。Cloud Function の資格情報が指定されていない場合や正しくない場合は、エラーが返されます。このコンテキスト変数はサーバー・アクションでのみ使用されます。クライアント・アプリケーションの場合は、エラー情報をキャプチャーしてコンテキスト変数としてダイアログに返す、類似したオブジェクトを作成することを検討してください。

エラーがないか最初にチェックするように、ダイアログ・ノードの応答を条件付けることができます。例えば、次の式を応答条件に追加すると、エラーが発生しなかった場合にのみ、{{site.data.keyword.openwhisk_short}} アクションの結果を参照する応答を表示できます。

```json
  $forecast_result.cloud_functions_call_error == null
```
{: codeblock}

クライアント・タイプのプログラマチック呼び出しの場合は、`action_error` などのコンテキスト変数を定義して、エラー処理に関する情報を渡すことができます。 結果変数の一部として、これをサービスに返すことができます。そして、以下のような応答条件を定義して、エラーが発生しなかった場合にのみ、応答を表示できます。

```json
  $forecast_result.action_error == null
```
{: codeblock}

## クライアント呼び出しの例
{: #action-client-example}

以下の例は、外部の気象サービスに対する呼び出しを示しています。これは、ノード応答に関連付けられた JSON エディターに追加されます。ノード・レベルの応答がトリガーされるまで、スロットがユーザーから日付と場所の情報を収集して保管します。 この例では、呼び出されるサービスにある `/weather` という名前のエンドポイントが、`location` パラメーターと `date` パラメーターを取り、JSON オブジェクト `{"forecast": "<value>"}` を返すことを想定しています。

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

通常、サービスが POST /message 要求からクライアントに戻るのは、新しいユーザー入力が必要な場合のみです。例えば、親ノードを実行した後の子ノードのいずれかを実行する前などに戻ります。ただし、クライアント・アクションをノードに追加した場合、サービスは、アクション呼び出しの結果を返すために、評価の後に必ずクライアントに戻ります。子ノードに直接ジャンプするように構成したノードのように、ユーザー入力を待機すべきでない場合は、待機しないようにするために、サービスのメッセージ・コンテキストに次の値を追加します。
```json
  {
    "context": {
      "skip_user_input": true
    }
  }
```
{: codeblock}

クライアントでアクションを実行し、ユーザー入力を取得しないようにする場合も、同じ規則に従い、親ノードに「skip_user_input」 コンテキスト変数を追加して、クライアント・アプリケーションにそのことを伝えることができます。
クライアント・アプリケーションは、必ず、コンテキストに「skip_user_input」変数があるかどうかをチェックする必要があります。この変数がある場合は、ユーザーに新規入力を要求しないこと、代わりに、アクションを実行し、結果をメッセージに追加してサービスに返すことがわかります。新しい POST メッセージ要求には、前の POST メッセージ応答 (つまり、コンテキスト、入力、インテント、エンティティー、およびオプションで出力セクション) で返されたメッセージが含まれている必要があります。また、実行するプログラマチック呼び出しを定義する JSON オブジェクトではなく、プログラマチック呼び出しから返された結果が含まれている必要があります。
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
![天気予報を尋ねると、ダイアログがその要求をクライアント・アプリケーションに送信し、クライアント・アプリケーションがそれを外部サービスに送信することを示しています](images/forecast.png)

## サーバー呼び出しの例
{: #action-server-example}

次の例は、{{site.data.keyword.openwhisk_short}} アクションに対する呼び出しを示しています。この例は、サービスに用意されている [Utilities パッケージ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/docs/openwhisk/openwhisk_actions.html#openwhisk_create_action_sequence){: new_window} に定義されている {{site.data.keyword.openwhisk_short}} の「echo」アクションの使用方法を示しています。このアクションは、テキスト・ストリングを取り、それを返します。
``` json
{
  "actions": [
    {
      "name": "/whisk.system/utils/echo",
      "type":"server",
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

次の図は、{{site.data.keyword.openwhisk_short}} の組み込みの echo サービスを呼び出す単純な例を使用して、{{site.data.keyword.openwhisk_short}} アクションを呼び出す方法を示しています。ユーザーに入力を要求し、その入力を echo サービスに渡します。echo サービスが同じテキストを返すと、それがユーザーに表示されます。
![テキストを入力すると、ダイアログがその要求を {{site.data.keyword.openwhisk_short}} サービスに送信し、サービスがダイアログにそのテキストを返すことを示しています。](images/echo-via-cf.png)

### Echo アクションの例
{{site.data.keyword.openwhisk_short}} の組み込みの Echo アクションを呼び出すようにセットアップ済みのダイアログがあるワークスペースを表示するには、次の手順を実行します。
1.  [CloudFunctionsEcho.json ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://github.com/watson-developer-cloud/community/raw/master/conversation/cloud-functions-echo.json){: new_window} ファイルをダウンロードします。
1.  この JSON ファイルを新規ワークスペースとしてインポートします。
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

1.  何かを入力して、ダイアログをテストします。
サービスが {{site.data.keyword.openwhisk_short}} の Echo アクションを使用して入力内容を繰り返します。
## 高度なサーバー呼び出しの例
{: #advanced-action-server-example}

1 つのダイアログ・フローから複数のアクションを呼び出すことができます。実際には、1 つのダイアログ・ノードの 1 つの JSON オブジェクト「actions」で最大 5 つのアクションを呼び出すことができます。ただし、JSON 配列「actions」に定義したサーバー・タイプのアクションは、すべて並行して処理されます。したがって、サーバー・タイプのアクションを 1 つ呼び出した後に、その結果を同じ「actions」ブロック内の 2 番目のサーバー・タイプのアクションに渡すことはできません。 特定の順序でサーバー・アクションを呼び出す最善の方法は、{{site.data.keyword.openwhisk_short}} シーケンスを使用する方法です。実行時には、ダイアログは外部呼び出しを 1 つ実行するだけで複数のアクションを完了できるので、この方法は高速です。 シーケンスを使用するには、「actions」ブロック定義でアクション名の代わりにシーケンス名を参照するだけです。代わりに、あるノードから最初のサーバー・タイプのアクションを呼び出した後に、次のサーバー・タイプのアクションを呼び出す子ノードにジャンプするという方法もあります。
クライアント・タイプとサーバー・タイプのアクションが混在する 1 つの「actions」配列を定義した場合、ダイアログ・ノードを実行すると、サーバー・タイプのすべてのアクションの処理が完了して初めて、クライアント・アクションがクライアントに送信されます。
{: tip}

次の例は、{{site.data.keyword.openwhisk_short}} アクションに対する呼び出しを示しています。
この例は、{{site.data.keyword.openwhisk_short}} アクションを使用して、都市名を取り、その指定された場所の緯度と経度の座標を返す外部サービスを呼び出す方法を示しています。
``` json
{
  "actions": [
    {
      "name": "/jdoeorg_prod/get coordinates",
      "type":"server",
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

「$my_coordinates」コンテキスト変数は、「get coordinates」サービスから返される 2 つの値を次のように保存します。
```json
{
 "lat":"42.3611",
 "long":"-71.0571"
}
```
{: codeblock}

この例は、{{site.data.keyword.openwhisk_short}} サービスに用意されている [Weather パッケージ ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://console.bluemix.net/docs/openwhisk/openwhisk_weather.html#openwhisk_catalog_weather){: new_window} に定義されている {{site.data.keyword.openwhisk_short}} の「forecast」アクションの使用方法を示しています。このアクションには、緯度と経度の座標、および期間が必要です。指定した期間にわたる指定した場所の天気予報情報を含む JSON オブジェクトが返されます。前のアクションから返された座標を、「$my_coordinates.lat」および「$my_coordinates.long」として指定しています。
``` json
{
  "actions": [
    {
      "name": "/whisk.system/weather/forecast",
      "type":"server",
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

**注意**: ユーザー名とパスワードをパラメーターとしてリストしています。これらを指定しているのは、この特定のアクションのためにこれらのパラメーターが必要だからです。指定したアクションがバックエンドで呼び出す外部 Weather サービスに必要な資格情報を定義しています。これらは IBM Cloud Function のアカウントの資格情報とは異なります。これらの資格情報を保護するための手段を講じてください。
これで、「context.forecasts」変数に格納された {{site.data.keyword.openwhisk_short}} アクションの出力に、後続のダイアログ・ノードがアクセスできるようになります。
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

次の図は複雑な対話を示しています。ユーザーが天気予報を尋ねます。ダイアログ・ノードのスロットが、場所と期間の情報を入力するようにユーザーに要求します。{{site.data.keyword.openwhisk_short}} 予報サービスを呼び出すには、地理座標を提供する必要があります。そのため、ダイアログはまず、ユーザーから提供された場所の緯度と経度の詳細を取得します。そのために、{{site.data.keyword.openwhisk_short}} サービスに要求を送信すると、そのサービスが、座標情報を返す外部サービスにその要求を渡します。後続のノードで、ダイアログは、新しく取得した座標情報とユーザーから取得した期間情報を、{{site.data.keyword.openwhisk_short}} の組み込みの予測サービスに送信して、予報の詳細を取得します。そして、ダイアログは、{{site.data.keyword.openwhisk_short}} 予報サービスから提供された予測結果を示して、ユーザーの質問に答えます。
![天気予報を尋ねると、ダイアログがその要求を外部サービスに渡すために Cloud Function サービスに送信することを示しています。](images/forecast-via-cf.png)
