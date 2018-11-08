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

# 升級

瞭解如何升級服務方案。
{: shortdesc}

## 升級方案
{: #plan-upgrade}

您可以使用「精簡」方案來廣泛探索 {{site.data.keyword.conversationshort}} 服務。不過，您可以執行的作業有所限制。


### 構件的限制
如需每個方案之構件限制的相關資訊，請參閱下列主題：

- [工作區](configure-workspace.html#workspace-limits)
- [對話節點](dialog-build.html#dialog-node-limits)
- [目的](intents.html#intent-limits)
- [實體](entities.html#entity-limits)
- [日誌](logs_convo.html#log-limits)

### API 呼叫限制
每個實例容許的 API 呼叫數目，取決於您的服務方案。如需詳細資料，請參閱方案說明。

如果您有「精簡」方案並達到 API 呼叫限制，但日誌顯示您進行的呼叫數低於預期，則請記住「精簡」方案只會儲存 7 天的日誌資訊。

若要升級方案，請完成下列步驟：

1.  從 {{site.data.keyword.Bluemix_notm}} 功能表中，選取**服務** > **儀表板**。
1.  選取您要升級的服務實例以將它開啟。
1.  從導覽窗格中按一下**方案**。
   您可以在這裡查看現行方案及其他可用的方案選項，並進行變更。

如需訂閱常見問題的回答，請參閱[管理計費及用量 ![外部鏈結圖示](../../icons/launch-glyph.svg "外部鏈結圖示")](/docs/billing-usage/how_charged.html){: new_window}。

您可以從下列的鏈結進一步瞭解 IBM Cloud 所管理的服務：

- [雲端服務條款](http://www.ibm.com/software/sla/sladb.nsf/sla/saas)
- [雲端服務資料安全及隱私權](http://www.ibm.com/software/sla/sladb.nsf/sla/csdsp)

## 升級工作區
{: #upgrade-workspace}

{{site.data.keyword.conversationshort}} 服務會定期新增及更新特性。其中某些變更自動套用至工作區時，需要手動更新具有大型影響的更新項目。


只有在顯示升級圖示 (![升級圖示](images/upgrade.png)) 時，才能升級工作區。

**附註**：升級工作區之後，就無法將工作區回復成舊版本。

若要升級工作區，請完成下列步驟：
1.  [複製工作區](configure-workspace.html#exporting-and-copying-workspaces)。
2.  升級複製工作區。

    升級工作區時，該工具會啟用 API 的最新版本，而且「試用」窗格會開始使用最新的特性。
3.  測試已升級的工作區。
4.  評估複製工作區以瞭解升級對應用程式的影響之後，請將升級套用至主要工作區。
5.  升級應用程式，方法是變更訊息 API 呼叫來使用最新 API 版本。
