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

# Integrationen hinzufügen
{: #deploy-integration-add}

Um Ihren Skill bereitzustellen, fügen Sie den Skill zu einem Assistenten hinzu und fügen Sie anschließend Integrationen zu dem Assistenten hinzu, die Ihren Bot in den Kanälen publizieren, über die Ihre Kunden Hilfe anfordern.
{: shortdesc}

## Integration hinzufügen
{: #deploy-integration-add-task}

So fügen Sie Integrationen zu Ihrem Assistenten hinzu: 

1.  Klicken Sie auf die Registerkarte **Assistenten**.

1.  Klicken Sie auf die entsprechende Kachel, um den Assistenten zu öffnen, den Sie bereitstellen möchten.

1.  Rufen Sie den Abschnitt 'Integrationen' auf. 

    **Was ist die Vorschaulink-Integration?** Wenn Sie einen Assistenten erstellen, wird automatisch eine Testwebsite für Sie bereitgestellt (sofern Sie dies nicht inaktivieren). Diese Website verfügt über eine einfache Chat-Widget-Schnittstelle, die Sie zum Interagieren mit Ihrem Assistenten für Testzwecke verwenden können. Sie können die URL dieser Site mit IBM Schutzmarke auch mit Ihren Teammitgliedern gemeinsam nutzen. 

1.  Klicken Sie auf **Integration hinzufügen**.

1.  Klicken Sie auf die Kachel für den Kanal, in den Sie den Assistenten integrieren möchten. Zu den Optionen gehören die folgenden: 

    - [Angepasste Anwendung](/docs/services/assistant?topic=assistant-deploy-custom-app)
    - [Facebook Messenger](/docs/services/assistant?topic=assistant-deploy-facebook)
    - [Intercom](/docs/services/assistant?topic=assistant-deploy-intercom)  ![Nur Plus- oder Premium-Plan](images/premium.png)
    - [Vorschaulink](/docs/services/assistant?topic=assistant-deploy-web-link)
    - [Slack](/docs/services/assistant?topic=assistant-deploy-slack)
    - [Voice Agent (Telefonie)  ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/catalog/services/voice-agent-with-watson){: new_window}

      Öffnet die Übersichtsseite des **Voice Agent with Watson**-Service in {{site.data.keyword.cloud_notm}}.
    - [WordPress-Plug-in ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://wordpress.org/plugins/conversation-watson/){: new_window}

      Öffnet die Übersichtsseite für das IBM Watson Assistant-Plug-in auf der WordPress-Site.

1.  Befolgen Sie die angezeigten Anweisungen, um den Integrationsprozess abzuschließen. 

Testen Sie nach der Integration den Assistenten im Zielkanal, um sicherzustellen, dass der Assistent erwartungsgemäß funktioniert.

Tipps zum konsistenten Starten des Dialogmoduls in allen Integrationstypen enthält der Abschnitt [Dialogmodul starten](/docs/services/assistant?topic=assistant-dialog-start).

## Begrenzungen der Integration
{: #deploy-integration-add-limits}

Wie viele Integrationen Sie in einer einzelnen Serviceinstanz erstellen können, richtet sich nach Ihrem {{site.data.keyword.conversationshort}}-Plan.

| Serviceplan     | Integrationen pro Assistent |
|------------------|---------------------------:|
| Premium          |                        100 |
| Plus             |                        100 |
| Standard         |                        100 |
| Lite             |                        100 |
{: caption="Details des Serviceplans" caption-side="top"}
