---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

subcollection: assistant

---

{:curl: #curl .ph data-hd-programlang='curl'}
{:javascript: #javascript .ph data-hd-programlang='javascript'}
{:java: #java .ph data-hd-programlang='java'}
{:python: #python .ph data-hd-programlang='python'}
{:swift: data-hd-programlang='swift'}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:download: .download}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# {{site.data.keyword.conversationshort}} API の概要
{: #api-overview}

{{site.data.keyword.conversationshort}} REST API および対応する SDK を使用して、サービスと対話するアプリケーションを開発することができます。{{site.data.keyword.conversationshort}} API には v1 と v2 の 2 つのバージョンがあり、それぞれ異なる機能セットをサポートしています。使用する API は、アプリケーションに必要なメソッドの種類によって異なります。次の種類があります。

- **ランタイム・メソッド**: クライアント・アプリケーションが既存のアシスタントまたはスキルと対話する (ただし変更はできない) ことを可能にするメソッド。これらのメソッドを使用して、実動用にデプロイ可能なユーザー向けクライアント、アシスタントと他のサービス (チャット・サービスやバックエンド・システムなど) との間の通信を仲介するアプリケーション、またはテスト・アプリケーションを開発できます。

  {{site.data.keyword.conversationshort}} v2 API は、実行時にアシスタントと対話するために使用できるメソッド (`/message` など) へのアクセスを提供します。これは、新しいクライアント・アプリケーションの開発に使用する推奨の API です。v2 API について詳しくは、{{site.data.keyword.conversationshort}} [v2 API リファレンス ![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/assistant-v2){: new_window} を参照してください。

  {{site.data.keyword.conversationshort}} v1 API には、アシスタントをバイパスし、ダイアログ・スキルで使用されるワークスペースにユーザー入力を直接送信する `/message` メソッドが含まれます。v1 ランタイム API は、主に下位互換性のためにサポートされています。v1 `/message` メソッドを使用する場合、アプリは、アシスタントのオーケストレーション機能と状態管理機能を利用することはできません。詳しくは、[v1 ランタイム API の使用](/docs/services/assistant?topic=assistant-api-client#v1-api)を参照してください。 

- **オーサリング・メソッド**: {{site.data.keyword.conversationshort}} ツールを使用してグラフィカルにスキルを構築する代わりに、アプリケーションがダイアログ・スキルを作成または変更できるようにするメソッド。オーサリング・アプリケーションは、さまざまなメソッドを使用して、ダイアログ・スキルを構成するスキル、インテント、エンティティー、ダイアログ・ノードなどの成果物を作成および変更します。

  オーサリング・アプリケーションを構築するには、v1 API を使用します。詳細については、[v1 API Reference![外部リンク・アイコン](../../icons/launch-glyph.svg "外部リンク・アイコン")](https://{DomainName}/apidocs/assistant){: new_window} を参照してください。

  **注:** v1 オーサリング・メソッドは、スキルではなくワークスペースと対話します。ワークスペースは、ダイアログ・スキル内のダイアログおよびトレーニング・データ (インテントやエンティティーなど) のコンテナーです。ほとんどの目的では、API を使用する場合、ワークスペースとダイアログ・スキルは交換可能であると考えることができます。たとえば、API を使用して新しいワークスペースを作成した場合、それは {{site.data.keyword.conversationshort}} ツールで新しいダイアログ・スキルとして表示されます。

使用可能な API メソッドのリストについては、[API メソッドのサマリー](/docs/services/assistant?topic=assistant-api-methods)を参照してください。
