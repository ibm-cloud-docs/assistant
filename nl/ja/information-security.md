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

# 機密保護
{: #information-security}

IBM は、お客様やパートナーに、データ・プライバシー、セキュリティー、およびガバナンスに関する革新的なソリューションを提供することに努めています。
{: shortdesc}

**注意:**
お客様は、EU 一般データ保護規則 (GDPR) を含む各法律および規制の遵守をお客様ご自身で確保する責任があります。お客様のビジネスに影響を及ぼす可能性のある関連法令の特定およびそれらの解釈、ならびにかかる関連法令を遵守するためにお客様が講ずるべき必要措置に関する助言は、お客様の責任により適格な弁護士から得るものとします。

本書に記載の製品、サービス、および他の機能が、すべてのお客様の状況に適しているとは限らず、使用する際に制約を受ける場合があります。 IBM は、法律、会計または監査に関する助言を提供することはしませんし、IBM のサービスまたは製品が、お客様のあらゆる法令遵守の裏付けとなる表明または保証もいたしません。

以下の場所で作成された {{site.data.keyword.cloud}} {{site.data.keyword.watson}} リソースの GDPR サポートを要請する必要がある場合の手順

- EU 内の場合、[Requesting support for IBM Cloud Watson resources created in the European Union![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/docs/services/watson/getting-started-gdpr-sar#request-EU){: new_window} を参照してください。
- EU 外の場合、[Requesting support for resources outside the European Union![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://cloud.ibm.com/docs/services/watson/getting-started-gdpr-sar#request-non-EU){: new_window} を参照してください。

## EU 一般データ保護規則 (GDPR)
{: #information-security-gdpr}

IBM は、お客様やパートナーに、データ・プライバシー、セキュリティー、およびガバナンスに関する革新的なソリューションを提供して、GDPR  に対する準拠が完了するまでの過程を支援することに努めています。

GDPR に対する準備を整えるための IBM 独自の過程と、準拠が完了するまでの過程をサポートする弊社の GDPR 機能とオファリングについて詳しくは、[ここ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](../../icons/launch-glyph.svg "外部リンク・アイコン")](http://www.ibm.com/gdpr){: new_window} を参照してください。

## {{site.data.keyword.conversationshort}} でのデータのラベル付けと削除
{: #information-security-gdpr-wa}

作成するトレーニング・データ (ユーザー例を含むエンティティーとインテント) に個人データを追加しないでください。特に、ユーザー例の推奨事項をマイニングするために実際のユーザー発話が含まれたファイルをアップロードする場合は、個人情報を必ず削除してください。

**注:** 試験およびベータの機能は、実稼働環境での使用を意図していないため、データのラベル付けおよび削除時に期待どおりに機能することは保証されません。 データのラベル付けと削除を必要とするソリューションを実装する場合は、試験およびベータの機能を使用しないでください。

お客様のメッセージ・データを {{site.data.keyword.conversationshort}} インスタンスから削除する必要がある場合は、クライアントのお客様 ID に基づいて削除できます。ただし、そのためには、そのメッセージがこのサービスに送信されるとき、そのメッセージをお客様 ID に関連付ける必要があります。

**注:** プレビュー・リンク機能と自動 Facebook 統合機能ではラベル付けがサポートされていないため、お客様 ID に基づいたデータ削除もサポートされていません。お客様 ID に基づいた削除機能を必要とするソリューションでは、これらの機能を使用しないでください。

### 始める前に
{: #information-security-delete-user-data-prereqs}

特定のユーザーに関連付けられたメッセージ・データを削除できるようにするには、まずすべてのメッセージを各ユーザーの固有の**お客様 ID **に関連付ける必要があります。`/message` API を使用して送信されるメッセージの**お客様 ID **を指定するには、`X-Watson-Metadata: customer_id` プロパティーをヘッダーに含めます。次に例を示します。

```
curl -X POST -u "apikey:3Df... ...Y7Pc9"
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id=abc'
 --data
   '{"input":{"text":"hello"}}'
  'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

`customer_id` ストリングにセミコロン (`;`) や等号 (`=`) の文字を含めることはできません。各 `customer ID` プロパティーがお客様の間で一意であることを確認する必要があります。
{: note}

複数の **customer ID** 値を渡すには、一連の `customer_id={value}` ペアをセミコロンで区切って指定します。例えば、`'X-Watson-Metadata: customer_id=abc;customer_id=xyz'` などです。

アシスタントに検索スキルを追加すると、そのアシスタントに送信されるユーザー入力は、検索照会として {{site.data.keyword.discoveryshort}} サービスに渡されます。{{site.data.keyword.conversationshort}} の統合によってお客様 ID が提供される場合は、結果として得られる /message API 要求のヘッダーにはそのお客様 ID が含まれ、その ID は {{site.data.keyword.discoveryshort}} の /query API 要求に渡されます。特定のお客様に関連付けられた照会データを削除するには、ご使用のアシスタントにリンクされた {{site.data.keyword.discoveryshort}} サービス・インスタンスに削除要求を直接送信する必要があります。詳しくは、{{site.data.keyword.discoveryshort}} の[機密保護](https://cloud.ibm.com/docs/services/discovery/information-security#gdpr-discovery)のトピックを参照してください。

### ユーザー・データの照会
{: #information-security-query-customer-id}

v1 `/logs` メソッドの `filter` パラメーターを使用して、アプリケーション・ログ内で特定のユーザー・データを検索できます。例えば、`my_best_customer` に合致する `customer_id` に固有のデータを検索するには、照会は次のようになります。

``` curl
curl -X GET -u "apikey:3Df... ...Y7Pc9"
'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/logs?version=2018-09-20&filter=customer_id::my_best_customer'
```
{: codeblock}

詳しくは、[フィルター照会のリファレンス](/docs/services/assistant?topic=assistant-filter-reference)を参照してください。

### データの削除
{: #information-security-delete-data}

特定のユーザーに関連付けられたメッセージ・ログ・データのうち、サービスによって保管された可能性のあるデータを削除するには、`DELETE /user_data` v1 API メソッドを使用します。要求とともに `customer_id` パラメーターを渡すことで、このユーザーのお客様 ID を指定します。

この削除メソッドを使用して削除できるのは、お客様 ID を関連付けて `POST /message` API エンドポイントを使用して追加されたデータのみです。他のメソッドによって追加されたデータをお客様 ID に基づいて削除することはできません。例えば、お客様の会話から追加されたエンティティーとインテントをこの方法で削除することはできません。これらのメソッドでは個人データはサポートされていません。

**重要**: `customer_id` を指定すると、その `customer_id` を持つメッセージのうち、削除要求の前に受信された*すべての* メッセージが、1 つのスキル内だけでなく {{site.data.keyword.conversationshort}} インスタンス全体にわたって削除されます。

例えば、`abc` というお客様 ID を持つユーザーに関連付けられたメッセージ・データを {{site.data.keyword.conversationshort}} インスタンスから削除するには、次の cURL コマンドを送信します。

```
curl -X DELETE -u "apikey:3Df... ...Y7Pc9"
 'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/user_data?customer_id=abc&version=2018-09-20'
```
{: codeblock}

空の JSON オブジェクト `{}` が返されます。

詳しくは、[API リファレンス](https://cloud.ibm.com/apidocs/assistant?curl=#delete-labeled-data)を参照してください。

**注:** 削除要求は複数回に分けてバッチ処理されるため、完了までに最大で 24 時間かかることがあります。
