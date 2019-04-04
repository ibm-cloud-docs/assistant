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

# 升級
{: #upgrade}

瞭解如何升級服務方案。
{: shortdesc}

## 升級方案
{: #upgrade-plan}

您可以探索 {{site.data.keyword.conversationshort}} [服務方案選項 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window}，以決定哪個方案最適合您。

您無法將 Cloud Foundry 型實例升級至「加值」方案。您必須移轉該實例，讓它使用資源群組，這樣您才能將它升級。如需詳細資料，請參閱[移轉](/docs/services/assistant?topic=assistant-migrate)。
{: note}

若要升級方案，請完成下列步驟：

1.  從 {{site.data.keyword.Bluemix_notm}} 功能表中，選取**升級方案**。您可以在這裡查看現行方案及其他可用的方案選項，並進行變更。

如需訂閱常見問題的回答，請參閱[管理計費及用量 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/billing-usage?topic=billing-usage-charges){: new_window}。

還有其他問題嗎？請與 [IBM 業務代表 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window} 聯絡。

## 升級對話技能
{: #upgrade-skill}

{{site.data.keyword.conversationshort}} 服務會定期新增及更新特性。雖然其中有一些變更會自動套用至您的對話技能，但具有重大影響的更新項目需要手動更新您的技能所使用的機器學習模型。

只有在顯示升級圖示（![升級圖示](images/upgrade.png)）時，才能升級對話技能。

若要升級對話技能，請完成下列步驟：

1.  [下載對話技能的副本](/docs/services/assistant?topic=assistant-skill-add#skill-add-download-skill)，然後將它匯入成新技能。
2.  升級對話技能的新副本。

    當您升級技能時，工具中會啟用 API 的最新版本，且「試用」窗格會開始使用最新特性。
3.  測試已升級的技能。
4.  在評估已升級的技能以瞭解升級對應用程式的影響之後，請將升級套用至您的主要對話技能。
5.  升級您的應用程式。若要這樣做，請變更它用來指定最新 API 版本的訊息 API 呼叫。如需 API 版本的詳細資料，請參閱[版本注意事項](/docs/services/assistant?topic=assistant-release-notes)。
