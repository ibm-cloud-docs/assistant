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

# アップグレード
{: #upgrade}

サービス・プランをアップグレードする方法について説明します。
{: shortdesc}

## プランのアップグレード
{: #upgrade-plan}

{{site.data.keyword.conversationshort}} [サービス・プラン・オプション ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window} を検討して、自分に最適なプランを決定できます。

Cloud Foundry ベースのインスタンスをプラス・プランにアップグレードすることはできません。インスタンスをマイグレーションする必要があるため、アップグレードする前にリソース・グループを使用します。詳しくは、[マイグレーション](/docs/services/assistant?topic=assistant-migrate)を参照してください。
{: note}

プランをアップグレードするには、以下の手順を実行します。

1.  {{site.data.keyword.Bluemix_notm}} メニューから、**「プランのアップグレード (Upgrade Plan)」**を選択します。
    ここで、現在のプランと他の使用可能なプランのオプションを確認し、変更を行うことができます。

サブスクリプションに関する一般的な質問への回答については、[請求および使用量の管理![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/billing-usage?topic=billing-usage-charges){: new_window} を参照してください。

まだご不明な点がありますか? [IBM 営業部門 ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window} にお問い合わせください。

## ダイアログ・スキルのアップグレード
{: #upgrade-skill}

{{site.data.keyword.conversationshort}} サービスでは、機能が定期的に追加および更新されます。 これらの変更の一部はダイアログ・スキルに自動的に適用されますが、大規模な影響を与える更新の場合は、スキルで使用される機械学習モデルの手動更新が必要です。

アップグレードは、アップグレード・アイコン (![アップグレード・アイコン](images/upgrade.png)) が表示されている場合にのみダイアログ・スキルに対して使用できます。

ダイアログ・スキルをアップグレードするには、以下の手順を実行します。

1.  [ダイアログ・スキルのコピーをダウンロードし](/docs/services/assistant?topic=assistant-skill-add#skill-add-download-skill)、新規スキルとしてインポートします。
2.  ダイアログ・スキルの新規コピーをアップグレードします。

    スキルをアップグレードした場合、API の最新バージョンがツールで使用可能になり、「試行する (Try it out)」ペインで最新の機能が使用されるようになります。
3.  アップグレードされたスキルをテストします。
4.  アップグレードされたスキルを評価して、アップグレードがアプリケーションに及ぼす影響について理解したら、アップグレードを 1 次ダイアログ・スキルに適用します。
5.  アプリケーションをアップグレードします。そのためには、最新の API バージョンを指定するように、使用するメッセージ API 呼び出しを変更します。API バージョンの詳細は、[リリース・ノート](/docs/services/assistant?topic=assistant-release-notes)を参照してください。
