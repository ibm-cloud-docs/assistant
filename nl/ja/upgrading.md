---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-09"

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

# アップグレード
{: #upgrading}

サービス・プランをアップグレードする方法について説明します。
{: shortdesc}

## プランのアップグレード
{: #plan-upgrade}

{{site.data.keyword.conversationshort}} サービスは、ライト・プランで幅広く探索することができます。 ただし、実行できることには限度があります。


### 成果物による制限
プランごとの成果物の制限については、次の各トピックを参照してください。

- [ワークスペース](configure-workspace.html#workspace-limits)
- [対話ノード](dialog-build.html#dialog-node-limits)
- [インテント](intents.html#intent-limits)
- [エンティティー](entities.html#entity-limits)
- [ログ](logs_convo.html#log-limits)

### API 呼び出しの制限
インスタンス当たりに許可される API 呼び出しの回数は、サービス・プランによって異なります。 詳細については、プランの説明を参照してください。

ライト・プランをお持ちで、その API 呼び出しの制限回数に達したにもかかわらず、ログに示される実行した呼び出しの回数が予想よりも少ない場合、ライト・プランではログ情報を 7 日間しか保管しないことに注意してください。

プランをアップグレードするには、以下の手順を実行します。

1.  {{site.data.keyword.Bluemix_notm}} メニューから、**「サービス (Services)」** > **「ダッシュボード (Dashboard)」**を選択します。
1.  アップグレードするサービス・インスタンスを選択して開きます。
1.  ナビゲーション・ペインで**「プラン (Plan)」**をクリックします。
   ここで、現在のプランと他の使用可能なプランのオプションを確認し、変更を行うことができます。

サブスクリプションに関する一般的な質問への回答については、[請求および使用量の管理![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/billing-usage/how_charged.html){: new_window} を参照してください。

IBM Cloud によってホストされるサービスの詳細については、以下のリンクを参照してください。

- [クラウド・サービスのご利用条件](http://www.ibm.com/software/sla/sladb.nsf/sla/saas)
- [クラウド・サービスのデータ・セキュリティーとプライバシー](http://www.ibm.com/software/sla/sladb.nsf/sla/csdsp)

## ワークスペースのアップグレード
{: #upgrade-workspace}

{{site.data.keyword.conversationshort}} サービスでは、機能が定期的に追加および更新されます。これらの変更の一部はワークスペースに自動的に適用されますが、大規模な影響を与える更新の場合は手動更新が必要です。

アップグレードは、アップグレード・アイコン (![upgrade icon](images/upgrade.png)) が表示されている場合にのみ実行できます。

**注**: ワークスペースをアップグレードした後で、ワークスペースを前のバージョンに戻すことはできません。

ワークスペースをアップグレードするには、以下の手順を実行します。
1.  [ワークスペースを複製します](configure-workspace.html#exporting-and-copying-workspaces)。
2.  複製したワークスペースをアップグレードします。

    ワークスペースをアップグレードした場合、API の最新バージョンがツールで使用可能になり、「試行する (Try it out)」ペインで最新の機能が使用されるようになります。
3.  アップグレードされたワークスペースをテストします。
4.  複製したワークスペースを評価して、アップグレードがアプリケーションに及ぼす影響について理解したら、アップグレードを 1 次ワークスペースに適用します。
5.  最新の API バージョンを使用するようにメッセージ API 呼び出しを変更して、アプリケーションをアップグレードします。
