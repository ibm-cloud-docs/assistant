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

# IBM Cloud サービス情報
{: #services-information}

アシスタントは、{{site.data.keyword.cloud_notm}} で管理される完全にホストされたボットであるため、サポートするためにインフラストラクチャーの設定や保守について懸念する必要がありません。
{: shortdesc}

## サービス・プラン情報
{: #services-information-plans}

{{site.data.keyword.conversationshort}} [サービス・プランのオプション ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window} を検討してください。

サービス・インスタンスを作成する前に、{{site.data.keyword.cloud_notm}} アカウントでのリソースの編成方法を決定します。独自のリソース・グループを定義しない場合は、**デフォルト**のリソース・グループが使用され、後で変更することは*できません*。詳しくは、[リソースをリソース・グループに編成するためのベスト・プラクティス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/docs/resources/bestpractice_rgs#bp_resourcegroups){: new_window} を参照してください。すべてのユーザーがオペレーターのプラットフォーム・アクセス役割を持っている必要があります。(サービス・アクセス役割は {{site.data.keyword.conversationshort}} では利用されません。)

### 成果物タイプ別のプランの制限
{: #services-information-limits}

プランごとの成果物の制限に関する情報は、成果物の作成方法を説明するトピックに示されているため、必要に応じて制限を参照できます。以下に、トピックのリンクを示します。

- [アシスタント](/docs/services/assistant?topic=assistant-assistant-add#assistant-add-limits)
- [ダイアログ・ノード](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-node-limits)
- [エンティティー](/docs/services/assistant?topic=assistant-entities#entities-limits)
- [インテント](/docs/services/assistant?topic=assistant-intents#intents-limits)
- [統合](/docs/services/assistant?topic=assistant-deploy-integration-add#deploy-integration-add-limits)
- [ログ](/docs/services/assistant?topic=assistant-logs#logs-limits)
- [スキル](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)
- [バージョン](/docs/services/assistant?topic=assistant-versions#versions-limits)

### API 呼び出しの制限
{: #services-information-api-limits}

インスタンス当たりに許可される API 呼び出しの回数は、サービス・プランによって異なります。 詳細については、プランの説明を参照してください。

ライト・プランをお持ちで、その API 呼び出しの制限回数に達したにもかかわらず、ログに示される実行した呼び出しの回数が予想よりも少ない場合、ライト・プランではログ情報を 7 日間しか保管しないことに注意してください。

プランを別のプランにアップグレードする場合は、[アップグレード](/docs/services/assistant?topic=assistant-upgrade)を参照してください。

### プラス・プラントプレミアム・プランの機能
{: #services-information-premium}

以下の機能は、プレミアム・プランのユーザーのみが使用できます。

- [明確化](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)
- [インテント競合解決](/docs/services/assistant?topic=assistant-intents#intents-resolve-conflicts)
- [インテント・ユーザー例の推奨](/docs/services/assistant?topic=assistant-intent-recommendations)
- [Intercom の統合](/docs/services/assistant?topic=assistant-deploy-intercom)

### ユーザーベースのプラン
{: #services-information-user-based-plans}

指定された時間フレームの間に行われた API 呼び出しの数によって使用量を計測する API ベースのプランとは異なり、新しいプラス・プランおよび更新されたプレミアム・プランでは、ユーザーベースの請求が使用されます。指定された時間フレームの間にアシスタントと対話した一意のユーザーの数によって使用量が計測されます。

サービスによって、請求のために API 要求からの以下の情報が以下の順序で確認されます。

  1.  **user_id**: /message API 呼び出しのコンテキスト・オブジェクトで送信される、API で定義されたプロパティー。/message API 呼び出しが一意のユーザーに帰属することを正確に確認するには、このプロパティーを使用する方法が最適です。ユーザー ID プロパティーについて詳しくは、API リファレンス資料を参照してください。
  
    - `context.global.system.user_id`: [v2 API](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant)
    - `context.metadata.user_id`: [v1 API](https://cloud.ibm.com/apidocs/assistant#get-response-to-user-input)

  1.  **session_id**: ユーザーとアシスタントの間の単一の会話を識別する、v2 API で定義されたプロパティー。セッション ID は、組み込み統合によって生成される /message API 呼び出しで提供されます。セッションは、ユーザーがチャット・ウィンドウを閉じるか、またはチャットが 60 分を超えて非アクティブな場合に終了します。

  1.  **conversation_id**: /message API 呼び出しのコンテキスト・オブジェクトに保管される、v1 API で定義されたプロパティー。このプロパティーを使用して、1 名のユーザーとの単一の会話型のやり取りに関連付けられている複数の /message API 呼び出しを識別できます。ただし、同じ ID は、その ID を明示的に保持し、同じ会話の一部として作成された各要求とともに渡した場合にのみ使用されます。それ以外の場合は、新しい /message API 呼び出しごとに新しい ID が生成されます。

新しいユーザーベースのサービス・プランを最大限に活用するには、アシスタントをデプロイするために使用するカスタム・アプリケーションを、一意のユーザー ID またはセッション ID を取得して情報をサービスに渡すように設計します。

## API 呼び出しの認証
{: #services-information-authenticate-api-calls}

サービス・インスタンスによって使用される認証メカニズムは、API 呼び出しを行う際に提供する必要がある資格情報に影響を与えます。

1.  サービス資格情報を取得します。

    - [{{site.data.keyword.Bluemix_notm}} Resource List ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/resources){: new_window} でサービス・インスタンスを見つけてクリックします。

    - サービス・インスタンスをクリックして開き、**「サービス資格情報 (Service credentials)」**をクリックして、**「資格情報の表示 (View credentials)」**をクリックします。

      **Cloud Foundry 資格情報**

      ![Cloud Foundry でホストされているインスタンスのサービス資格情報ページを表示します。](images/cf-cred-ui.png)

      **IAM 資格情報**

      ![IAM でホストされているインスタンスのサービス資格情報ページを表示します。](images/iam-creds.png)

1.  API 呼び出しでこれらの資格情報を使用します。

    **Cloud Foundry API 呼び出し**

    資格情報としてユーザー名とパスワードを指定します。

    ```curl
    curl -X GET \
    --user {username}:{password} \
    'https://gateway.watson.net/assistant/api/v1/workspaces?version=2018-09-20'
    ```
    {: codeblock}

     **IAM API 呼び出し**

    - 基本 url には場所が含まれている必要があります。`gateway-<location>.watsonplatform.net` という構文を使用して、サービス・インスタンスを作成した場所を指定します。場所コードは、*データ・センターの場所* の表にリストされています。
    - ヘッダーで適切なタイプのトークンを指定します。ベアラー・トークンを渡すか、または API キーを渡すことができます。

      - トークンを使用すれば、認証済み要求がサポートされるので、呼び出すたびにサービス資格情報を埋め込む必要がありません。 次の例では、ベアラー・トークンが使用されています。

        ```curl
        curl -X GET \
        'https://gateway-syd.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        --header 'Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs'
        ```
        {: codeblock}

      - API キーは基本認証を使用します。 次の例では、apikey が使用されています。

        ```curl
        curl -X GET -u "apikey:3Df... ...Y7Pc9" \
        'https://gateway-us-east.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        ```
        {: codeblock}

        Watson SDK を使用する場合は、API キーを渡して、SDK にトークンのライフサイクルを管理させることができます。
        {: note}

        IAM リソースは、Cloud Foundry コマンド・ライン・インターフェース (CLI) で管理することはできません。例えば、サービス・インスタンスを作成または管理する Cloud Foundry CLI コマンド (先頭が `cf`) は、IAM を使用する場所でホストされているインスタンスでは機能しません。代わりに、{{site.data.keyword.cloud_notm}} CLI およびその関連コマンドを使用する必要があります。詳しくは、[リソースおよびリソース・グループの処理 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource) を参照してください。

        詳しくは、[IAM トークンを使用した認証![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/services/watson?topic=watson-iam){: new_window} を参照してください。

    例えば、API リファレンスで、ご使用の言語の[認証 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/assistant-v2#authentication){: new_window} を参照してください。

### データ・センター
{: #services-information-regions}

{{site.data.keyword.cloud_notm}} には、そのクラウド・サービスにパフォーマンス上のメリットをもたらすグローバル・データ・センターのネットワークがあります。詳しくは、[{{site.data.keyword.cloud_notm}} グローバル・データ・センター ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/data-centers/){: new_window} を参照してください。

{{site.data.keyword.cloud_notm}} では、Cloud Foundry を使用したユーザー・アクセスの管理からトークンベースの Identity and Access Management (IAM) 認証の使用に変更されました。IAM は異なる時間に異なる場所でロールアウトされました。サービス・インスタンスをマイグレーションし、現在の Cloud Foundry の組織とスペースからリソース・グループに移動できます。詳しくは、[マイグレーション](/docs/services/assistant?topic=assistant-migrate)を参照してください。

以下のデータ・センターの場所でホストされる {{site.data.keyword.conversationshort}} サービス・インスタンスを作成できます。

| Location    | 場所コード | 認証タイプ | IAM 採用日 | 注 |
|-------------|---------------|---------------------|-------------------|-------|
| ダラス      | us-south      | IAM                 | 2018 年 10 月 30 日 | N/A |
| フランクフルト   | eu-de         | IAM                 | 2018 年 10 月 30 日 | N/A |
| シドニー      | au-syd        | IAM                 | 2018 年 5 月 7 日 | 5 月 7 日より前に作成されたインスタンスはダラスにシンジケートされます |
| 東京       | jp-tok        | IAM                 | 2018 年 11 月 8 日 | N/A |
| ロンドン      | eu-gb, lon    | IAM                 | 2018 年 12 月 13 日 | 12 月 13 日より前に英国地域で作成されたインスタンスは米国南部地域にシンジケートされます |
| ワシントン DC  | us-east    | IAM                 | 2018 年 6 月 14 日 | N/A |
{: caption="データ・センターの場所" caption-side="top"}

他の {{site.data.keyword.cloud_notm}} サービスがホストされているデータ・センターについて詳しくは、[地域別のサービス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/docs/resources/services_region#services_region){: new_window} を参照してください。

## 使用条件とセキュリティー
{: #services-information-terms}

サービスの使用条件とデータ・セキュリティーについて詳しくは、以下の情報を参照してください。

- [Service terms ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/home?OpenDocument){: new_window}
- [Data security and privacy ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/software/sla/sladb.nsf/sla/csdsp?OpenDocument){: new_window}
- [機密保護](/docs/services/assistant?topic=assistant-information-security)

{{site.data.keyword.cloud_notm}} について詳しくは、[プラットフォームの概要 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/overview?topic=overview-whatis-platform){: new_window} を参照してください。

## まだご不明な点がありますか? 
{: #services-information-sales}

[IBM 営業部門 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window} にお問い合わせください。
