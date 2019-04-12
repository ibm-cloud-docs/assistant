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

# Informationen zu IBM Cloud-Services
{: #services-information}

Der Assistent ist ein vollständig per Hosting bereitgestellter Bot, der von {{site.data.keyword.cloud_notm}} verwaltet wird, d. h. Sie müssen sich nicht um das Einrichten oder Verwalten der zugehörigen Infrastruktur kümmern.
{: shortdesc}

## Informationen zum Serviceplan
{: #services-information-plans}

Erkunden Sie die [Serviceplanoptionen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window} für {{site.data.keyword.conversationshort}}.

Legen Sie vor dem Erstellen einer Serviceinstanz fest, wie die Ressourcen in Ihrem {{site.data.keyword.cloud_notm}}-Konto zusammegefasst werden sollen. Wenn Sie keine eigene Ressourcengruppe definieren, wird die Ressourcengruppe **Standard** verwendet. Dies kann später *nicht* geändert werden. Weitere Details finden Sie unter [Bewährte Verfahren zum Organisieren von Ressourcen in Ressourcengruppen![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/docs/resources/bestpractice_rgs#bp_resourcegroups){: new_window}. Alle Benutzer müssen über die Rolle 'Operator' (Bediener) für den Plattformzugriff verfügen. (Servicezugriffsrollen werden von {{site.data.keyword.conversationshort}} nicht genutzt.)

### Plangrenzwerte nach Artefakttyp
{: #services-information-limits}

Informationen zu den Artefaktgrenzwerten der einzelnen Pläne finden Sie in den Abschnitten über die Vorgehensweise beim Erstellen der Artefakte. Dort können Sie die verschiedenen Grenzwerte abrufen. Die folgenden Links führen zu den betreffenden Abschnitten: 

- [Assistenten](/docs/services/assistant?topic=assistant-assistant-add#assistant-add-limits)
- [Dialogmodulknoten](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-node-limits)
- [Entitäten](/docs/services/assistant?topic=assistant-entities#entities-limits)
- [Absichten](/docs/services/assistant?topic=assistant-intents#intents-limits)
- [Integrationen](/docs/services/assistant?topic=assistant-deploy-integration-add#deploy-integration-add-limits)
- [Protokolle](/docs/services/assistant?topic=assistant-logs#logs-limits)
- [Skills](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)
- [Versionen](/docs/services/assistant?topic=assistant-versions#versions-limits)

### Begrenzungen für API-Aufrufe
{: #services-information-api-limits}

Die Anzahl der zulässigen API-Aufrufe pro Instanz richtet sich nach Ihrem Serviceplan. Details können Sie der Beschreibung Ihres Plans entnehmen.

Falls Sie einen Lite-Plan verwenden und den Grenzwert für die API-Aufrufe erreichen, die Protokolle jedoch angeben, dass Sie weniger Aufrufe als erwartet durchgeführt haben, müssen Sie bedenken, dass der Lite-Plan Informationen nur für 7 Tage speichert.

Informationen zu Upgrades von einem Plan auf einen anderen enthält der Abschnitt [Upgrade durchführen](/docs/services/assistant?topic=assistant-upgrade).

### Funktionen im Plus- und im Premium-Plan
{: #services-information-premium}

Die folgenden Funktionen sind nur für Benutzer der Premium-Pläne verfügbar. 

- [Vereindeutigen](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)
- [Absichtskonflikte lösen](/docs/services/assistant?topic=assistant-intents#intents-resolve-conflicts)
- [Empfehlung von Benutzerbeispielen für Absichten](/docs/services/assistant?topic=assistant-intent-recommendations)
- [Intercom-Integration](/docs/services/assistant?topic=assistant-deploy-intercom)

### Benutzerbasierte Pläne
{: #services-information-user-based-plans}

Im Unterschied zu API-basierten Plänen, die die Nutzung an der Anzahl der API-Aufrufe in einem bestimmten Zeitraum messen, wird im neuen Plus-Plan und im aktualisierten Premium-Plan die benutzerbasierte Abrechnung verwendet. Dabei wird die Nutzung an der Anzahl der eindeutigen Benutzer gemessen, die in einem bestimmten Zeitraum mit dem Assistenten interagiert haben. 

Der Service sucht für Abrechnungszwecke in den API-Anforderungen nach den folgenden Angaben in der angegebenen Reihenfolge:

  1.  **user_id**: Eine in der API definierte Eigenschaft, die im Kontextobjekt eines API-Aufrufs '/message' übergeben wird. Die Verwendung dieser Eigenschaft ist die beste Methode, um sicherzustellen, dass API-Aufrufe des Typs '/message' ordnungsgemäß eindeutigen Benutzern zugeordnet werden. Weitere Informationen zur Eigenschaft 'user_id' (Benutzer-ID) finden Sie in der API-Referenzdokumentation:
  
    - `context.global.system.user_id`: [API der Version 2](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant)
    - `context.metadata.user_id`: [API der Version 1](https://cloud.ibm.com/apidocs/assistant#get-response-to-user-input)

  1.  **session_id**: Eine in der API der Version 2 definierte Eigenschaft, die einen einzelnen Dialog zwischen einem Benutzer und dem Assistenten identifiziert. In API-Aufrufen des Typs '/message', die von den integrierten Integrationen generiert werden, wird eine Sitzungs-ID angegeben. Die Sitzung wird beendet, wenn ein Benutzer das Chatfenster schließt oder wenn der Chat 60 Minuten inaktiv ist.

  1.  **conversation_id**: Eine in der API der Version 1 definierte Eigenschaft, die im Kontextobjekt eines API-Aufrufs '/message' gespeichert wird. Mithilfe dieser Eigenschaft können mehrere API-Aufrufe des Typs '/message' identifiziert werden, die einem einzelnen Dialog mit einem Benutzer zugeordnet sind. Es wird jedoch nur dieselbe ID verwendet, wenn die ID explizit aufbewahrt und mit jeder Anforderung zurückgegeben wird, die als Teil desselben Dialogs übergeben wird. Andernfalls wird für jeden API-Aufruf '/message' eine neue ID generiert. 

Um den größtmöglichen Nutzen aus den neuen benutzerbasierten Serviceplänen zu ziehen, konfigurieren Sie alle angepassten Anwendungen, die Sie zum Bereitstellen Ihres Assistenten verwenden so, dass eine eindeutige Benutzer-ID oder Sitzungs-ID erfasst und an den Service übergeben wird.

## API-Aufrufe authentifizieren
{: #services-information-authenticate-api-calls}

Der von Ihrer Serviceinstanz verwendete Authentifizierungsmechanismus wirkt sich darauf aus, wie Berechtigungsnachweise beim Absetzen eines API-Aufrufs angegeben werden müssen. 

1.  Rufen Sie die Serviceberechtigungsnachweise ab.

    - Lokalisieren Sie die Serviceinstanz in der [{{site.data.keyword.Bluemix_notm}}-Ressourcenliste ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/resources){: new_window} und klicken Sie darauf.

    - Klicken Sie auf Ihre Serviceinstanz, um sie zu öffnen, klicken Sie auf **Serviceberechtigungsnachweise** und anschließend auf **Berechtigungsnachweise anzeigen**.

      **Cloud Foundry-Berechtigungsnachweise**

      ![Zeigt die Seite mit Serviceberechtigungsnachweisen für Instanzen, die in Cloud Foundry gehostet werden.](images/cf-cred-ui.png)

      **IAM-Berechtigungsnachweise**

      ![Zeigt die Seite mit Serviceberechtigungsnachweisen für Instanzen an, die in IAM gehostet werden.](images/iam-creds.png)

1.  Verwenden Sie diese Berechtigungsnachweise in Ihrem API-Aufruf.

    **API-Aufruf für Cloud Foundry**

    Geben Sie Ihren Benutzernamen und das zugehörige Kennwort als Berechtigungsnachweis an.

    ```curl
    curl -X GET \
    --user {benutzername}:{kennwort} \
    'https://gateway.watson.net/assistant/api/v1/workspaces?version=2018-09-20'
    ```
    {: codeblock}

     **API-Aufruf für IAM**

    - In der Basis-URL muss der Standort enthalten sein. Verwenden Sie die Syntax `gateway-<location>.watsonplatform.net`, um den Standort anzugeben, an dem Sie die Serviceinstanz erstellt haben. Die Standortcodes sind in der Tabelle *Standorte der Rechenzentren* aufgelistet.
    - Geben Sie im Header den geeigneten Tokentyp an. Sie können entweder ein Trägertoken oder einen API-Schlüssel übergeben. 

      - Token unterstützen authentifizierte Anforderungen ohne eingebettete Serviceberechtigungsnachweise in jedem Aufruf. Das folgende Beispiel veranschaulicht die Verwendung eines Trägertokens.

        ```curl
        curl -X GET \
        'https://gateway-syd.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        --header 'Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs'
        ```
        {: codeblock}

      - Für API-Schlüssel wird die Basisauthentifizierung verwendet. Das folgende Beispiel veranschaulicht die Verwendung eines API-Schlüssels.

        ```curl
        curl -X GET -u "apikey:3Df... ...Y7Pc9" \
        'https://gateway-us-east.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        ```
        {: codeblock}

        Wenn Sie ein Watson-SDK verwenden, können Sie den API-Schlüssel übergeben und den Lebenszyklus der Token vom SDK verwalten lassen.
        {: note}

        IAM-Ressourcen können nicht mit der Cloud Foundry-Befehlszeilenschnittstelle (Command Line Interface, CLI) verwaltet werden. Beispielsweise können Befehle der Cloud Foundry-CLI (Befehle, die mit `cf` beginnen) zum Erstellen oder Verwalten von Serviceinstanzen nicht für Instanzen verwendet werden, die an Standorten mit IAM gehostet werden. Stattdessen müssen Sie die {{site.data.keyword.cloud_notm}}-CLI und die zugehörigen Befehle verwenden. Weitere Details enthält der Abschnitt [Mit Ressourcen und Ressourcengruppen arbeiten![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource).

        Weitere Informationen enthält der Abschnitt [Mit IAM-Tokens authentifizieren ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/watson?topic=watson-iam){: new_window}.

    Beispiele enthält der Abschnitt über die [Authentifizierung ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/assistant-v2#authentication){: new_window} für Ihre Sprache in der API-Referenz.

### Rechenzentren
{: #services-information-regions}

{{site.data.keyword.cloud_notm}} verfügt über ein globales Netz von Rechenzentren, die Leistungsvorteile für zugehörige Cloud Services bieten. Weitere Details finden Sie unter [{{site.data.keyword.cloud_notm}}-Rechenzentren weltweit ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud/data-centers/){: new_window}.

In {{site.data.keyword.cloud_notm}} wird der Benutzerzugriff anstelle von Cloud Foundry jetzt mit der tokenbasierten Authentifizierung von IAM (Identity and Access Management verwaltet. IAM wurde an den einzelnen Standorten an verschiedenen Zeitpunkten implementiert. Durch Migration können Sie eine Serviceinstanz aus der aktuellen Cloud Foundry-Organisation und dem zugehörigen Bereich in eine Ressourcengruppe verschieben. Weitere Details enthält der Abschnitt [Migration](/docs/services/assistant?topic=assistant-migrate).

Sie können {{site.data.keyword.conversationshort}}-Serviceinstanzen erstellen, die an den folgenden Standorten der Rechenzentren gehostet werden: 

| Standort    | Standortcode | Authentifizierungstyp | Datum der IAM-Einführung | Hinweise |
|-------------|---------------|---------------------|-------------------|-------|
| Dallas      | us-south      | IAM                 | 30. Oktober 2018 | Keine |
| Frankfurt   | eu-de         | IAM                 | 30. Oktober 2018 | Keine |
| Sydney      | au-syd        | IAM                 | 7. Mai 2018 | Vor dem 7. Mai erstellte Instanzen wurden nach Dallas syndiziert. |
| Tokyo       | jp-tok        | IAM                 | 8. November 2018 | Keine |
| London      | eu-gb, lon    | IAM                 | 13. Dezember 2018 | Vor dem 13 Dezember im Vereinigten Königreich erstellt Instanzen wurden in die Region 'US South' syndiziert. |
| Washington DC  | us-east    | IAM                 | 14. Juni 2018 | Keine |
{: caption="Standorte der Rechenzentren" caption-side="top"}

Informationen zu den Rechenzentren, in denen andere {{site.data.keyword.cloud_notm}}-Services gehostet werden, finden Sie unter [Services nach Region ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/docs/resources/services_region#services_region){: new_window}.

## Nutzungsbedingungen und Sicherheit
{: #services-information-terms}

Weitere Informationen zu Nutzungsbedingungen und Datensicherheit für Services finden Sie den folgenden Stellen: 

- [Service terms ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/home?OpenDocument){: new_window}
- [Data security and privacy ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/software/sla/sladb.nsf/sla/csdsp?OpenDocument){: new_window}
- [Informationssicherheit](/docs/services/assistant?topic=assistant-information-security)

Weitere Informationen zu {{site.data.keyword.cloud_notm}} enthält der Abschnitt [Übersicht über die Plattform![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/overview?topic=overview-whatis-platform){: new_window}. 

## Sie haben noch Fragen? 
{: #services-information-sales}

Wenden Sie sich an [IBM Sales ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.
