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

# マイグレーション
{: #migrate}

{{site.data.keyword.conversationshort}} サービス・インスタンスをマイグレーションして、現在の Cloud Foundry の組織およびスペースからリソース・グループに移動します。
{: shortdesc}

{{site.data.keyword.cloud_notm}} は、Cloud Foundry の使用からリソース・グループの使用に移行しました。

リソース・グループには、Cloud Foundry と比べて以下の利点があります。

- IBM Cloud Identity and Access Management (IAM) を使用することで、より細かいアクセス制御が可能になる
- サービス・インスタンスをさまざまな地域にまたがるアプリケーションとサービスに接続できる
- グループあたりの使用量データの収集が簡易化される

2018 年 11 月より前にサービス・インスタンスを作成した場合は、インスタンスがホストされている場所によっては、そのサービス・インスタンスではリソース・グループの代わりに Cloud Foundry が使用されている可能性があります。それぞれの場所で新しいインスタンスで IAM の使用が開始された時期については、[データ・センター](/docs/services/assistant?topic=assistant-services-information#services-information-regions)を参照してください。

## サービス・インスタンスのマイグレーション
{: #migrate-task}

作業を開始する前にマイグレーション・プロセスの詳細を確認するには、[IBM Cloud のマイグレーション資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/resources?topic=resources-migrate) を参照してください。

対象のインスタンスを作成したユーザーまたはグループ、または対象のインスタンスに対する `Developer` ロール・アクセス権が付与されたユーザーまたはグループのみが、そのインスタンスをマイグレーションできます。
{: note}

サービス・インスタンスをマイグレーションするには、以下の手順を実行します。

1.  最初にサービス・インスタンスの移動先となるリソース・グループを決定します。

    ヒントについては、[リソースをリソース・グループに編成するためのベスト・プラクティス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/resources?topic=resources-bp_resourcegroups) を参照してください。

1.  IBM Cloud ダッシュボードのサービス・リストから、マイグレーションするインスタンスのマイグレーション・アイコン ![マイグレーション](images/migrate.svg) をクリックして、ポップアップから**「マイグレーション (Migrate)」**をクリックします。

    2018 年 5 月 7 日より前にシドニーのデータ・センターで作成されたサービス・インスタンスと、2018 年 12 月 13 日より前にロンドンのデータ・センターで作成されたサービス・インスタンスは、ダラスのデータ・センターに転送されて保管されました。シドニーまたはロンドンを拠点にした Cloud Foundry サービス・インスタンスをマイグレーションする場合は、そのサービス・インスタンスはダラスでホストされているリソースに変換されます。
    {: note}

1.  **「続行 (Continue)」**をクリックして、リソース・グループを選択します。

    リソース・グループをまだ作成していない場合は、この時点で作成できます。**デフォルト**のリソース・グループを使用できます。しばらく時間をかけてグループの使用方法を理解して、必要に応じてグループを作成してください。ここで選択したリソース・グループを後で変更することは*できません*。

1.  **「マイグレーション (Migrate)」**をクリックします。

    プロセスが完了すると、メッセージが表示されます。マイグレーションする他のサービス・インスタンスがある場合は、引き続き他のサービス・インスタンスをマイグレーションできます。ない場合は、**「完了 (Done)」**をクリックします。

マイグレーションした古い (Cloud Foundry 組織ベースの) サービス・インスタンスは、ダッシュボードの「Cloud Foundry サービス (Cloud Foundry Services)」セクションに引き続き一覧表示され、現在はそのインスタンスの新しい (リソース・グループ・ベースの) バージョンの*別名* として表示されます。

![現在のサービス・インスタンスはリソース・ベース・インスタンスの別名になっているという表示](images/alias.png)

別名について詳しくは、[IBM Cloud Connections の資料 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/docs/resources/connecting_apps#what_is_alias) を参照してください。

**「ツールの起動 (Launch tool)」**ボタンにアクセスするには、新しいリソース・グループ・ベース・バージョンのサービス・インスタンスを開く必要があります。新しいインスタンスは {{site.data.keyword.Bluemix_notm}} ダッシュボードの「サービス (Services)」セクションに一覧表示されています。

## 認証
{: #migrate-auth-support}

基本認証を使用して当サービスにアクセスする既存のアプリケーションがある場合は、それらのアプリケーションは引き続きユーザー名とパスワードを渡して、マイグレーション後のサービス・インスタンスから認証を受けることができます。資格情報は、別名の**「サービス資格情報 (Service credentials)」**ページで参照できます。

新しいインスタンスは、IBM Cloud Identity and Access Management (IAM) を使用して認証を管理します。IBM Cloud IAM は、ユーザー名とパスワードの資格情報の代わりに API キーを使用する拡張メカニズムです。API キーの情報は、新しいサービス・インスタンスの **「サービス資格情報 (Service credentials)」**ページで参照できます。

セキュリティーが強化された新しい認証方式を使用するように既存のカスタム・アプリケーションを更新することを検討してください。新しいインスタンスでダイアログ・スキルに加える変更はすべて、基本認証の資格情報を使用するアプリケーションにも反映されます。新しい API キー方式を使用するようにすべてのアプリケーションを更新した後は、別名は不要になるため削除してかまいません。
