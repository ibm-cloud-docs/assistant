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

# Suchskill erstellen
{: #skill-search-add}

Ein Assistent verwendet einen *Suchskill*, um komplexe Kundenanfragen an den {{site.data.keyword.discoveryfull}}-Service weiterzuleiten. {{site.data.keyword.discoveryshort}} behandelt die Benutzereingabe als eine Suchabfrage. Der Service findet relevante Informationen für die Abfrage in einer externen Datenquelle und gibt sie an den Assistenten zurück.
{: shortdesc}

Dieses Feature ist nur für Benutzer verfügbar, die am Betaprogramm teilnehmen. Informationen zum Anfordern des Zugriffs enthält der Abschnitt [Am Betaprogramm teilnehmen](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

![Beta](images/beta.png) IBM gibt Services, Features und Sprachunterstützung für Sie zum Bewerten frei, die als Betaversion klassifiziert sind. Diese Services können unter Umständen instabil sein, häufig geändert werden und nach Ankündigung kurzfristig eingestellt werden. Außerdem bieten Beta-Features möglicherweise nicht dieselben Leistungswerte oder dieselbe Kompatibilität wie allgemein verfügbare Features und sind nicht für die Verwendung in einer Produktionsumgebung bestimmt.

Sie können einen Suchskill in einem Assistenten hinzufügen. Informationen zu Begrenzungen in bestimmten Plänen enthält der Abschnitt [Begrenzungen für Skills](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits).

Der Suchskill sucht Informationen in einer Datensammlung, die Sie mit dem {{site.data.keyword.discoveryshort}}-Service erstellen. {{site.data.keyword.discoveryshort}} ist ein Service zum Durchsuchen, Umwandeln und Normalisieren Ihrer unstrukturierten Daten. Der Service nutzt Datenanalyse und kognitive Prozesse zum Aufbereiten Ihrer Daten, damit Sie später ohne großen Aufwand aussagefähige Daten daraus gewinnen können. Weitere Informationen zu {{site.data.keyword.discoveryshort}} finden Sie in der [Produktdokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/discovery?topic=discovery-about).

Der {{site.data.keyword.discoveryfull}}-Service kann mit diesen Methoden ausgelöst werden: 

- **Knoten 'Anything else'**: Durchsucht eine externe Datenquelle nach einer relevanten Antwort, wenn keiner der Dialogknoten die Benutzeranfrage beantworten kann. Anstelle einer Standardnachricht wie `Dabei kann ich Ihnen nicht weiterhelfen.` kann der Assistent die Nachricht `Vielleicht hilft Ihnen das weiter:` und im Anschluss die vom Suchvorgang zurückgegebene Textpassage ausgeben. Wenn ein Suchskill mit Ihrem Assistenten verknüpft ist, wird bei jedem Auslösen des Knotens `anything_else` statt dem Anzeigen der Knotenantwort ein Suchvorgang ausgeführt. Der Assistent leitet die Benutzereingabe als Anfrage an Ihren Suchskill weiter und gibt als Antwort die Suchergebnisse zurück. 
- **Suchantworttyp**: Wenn Sie einen Suchantworttyp zu einem Dialogmodulknoten hinzufügen, ruft der Service eine Textpassage aus einer externen Datenquelle ab und gibt die Passage als Antwort auf eine bestimmte Frage zurück. Dieser Suchtyp kommt nur zum Einsatz, wenn der entsprechende Dialogmodulknoten verarbeitet wird. Dieser Ansatz ist hilfreich, wenn Sie eine Benutzeranfrage eingrenzen müssen, bevor ein Suchvorgang ausgeführt wird. Angenommen, die Dialogmodulverzweigung erfasst Informationen zu dem Gerätetyp, den der Kunde kaufen möchte. Wenn Sie die Marke und das Modell des Geräts kennen, können Sie mit der Anfrage ein Modellschlüsselwort an den Suchskill übergeben, um bessere Ergebnisse zu erzielen. 
- **Nur Suchskill**: Wenn nur ein Suchskill mit einem Assistenten verknüpft ist und kein Dialogskill, wird eine Suchabfrage an den {{site.data.keyword.discoveryshort}}-Service übergeben, sobald eine Benutzereingabe von einem der Integrationskanäle des Assistenten empfangen wird. 

## Suchskill erstellen
{: #skill-search-add-task}

Falls noch nicht geschehen, führen Sie die vorausgesetzten Schritte aus dem [Lernprogramm 'Einführung'](/docs/services/assistant?topic=assistant-getting-started#getting-started-prerequisites) aus, um eine {{site.data.keyword.conversationshort}}-Serviceinstanz zu erstellen und starten Sie das {{site.data.keyword.conversationshort}}-Tool.

Zum Erstellen von Skills verwenden Sie das {{site.data.keyword.conversationshort}}-Tool. So erstellen Sie einen Suchskill:

1.  Klicken Sie auf die Registerkarte **Skills**.

1.  Klicken Sie auf **Neu erstellen**.

1.  Klicken Sie auf **Hinzufügen**, um einen *Suchskill* zu erstellen.

1.  Geben Sie die Details für den neuen Skill an:
    - **Name**: Der Name darf nicht länger als 100 Zeichen sein.. Diese Angabe ist erforderlich. 
    - **Beschreibung**: Die Beschreibung ist optional und darf nicht länger als 200 Zeichen sein. 

1.  Klicken Sie auf **Erstellen**.

Die weiteren Schritte variieren je nachdem, ob Sie auf eine vorhandene {{site.data.keyword.discoveryshort}}-Serviceinstanz mit erstellten Datensammlungen zugreifen können oder nicht. Führen Sie das entsprechende Verfahren für Ihre Situation aus: 

- [Verbindung zu einer vorhandenen Watson Discovery-Instanz herstellen](#skill-search-add-connect-discovery)
- [Watson Discovery-Instanz erstellen](#skill-search-add-create-discovery)

## Verbindung zu einer vorhandenen Watson Discovery-Serviceinstanz herstellen
{: #skill-search-add-connect-discovery}

1.  Wählen Sie die {{site.data.keyword.discoveryshort}}-Serviceinstanz aus, aus der Sie Informationen extrahieren möchten.
{: #choose-d-instance}

    Alle {{site.data.keyword.discoveryshort}}-Serviceinstanzen, auf die Sie zugreifen können, werden in der Liste angezeigt. 

    Wenn eine Warnung darauf hinweist, dass für manche Ihrer {{site.data.keyword.discoveryshort}}-Serviceinstanzen keine Berechtigungsnachweise festgelegt wurden, dann bedeutet dies, dass Sie auf mindestens eine Instanz zugreifen können, die Sie bisher noch nicht direkt über das {{site.data.keyword.cloud_notm}}-Dashboard geöffnet haben. Erst wenn Sie eine Serviceinstanz aufrufen, werden Berechtigungsnachweise für die betreffende Instanz erstellt. Nur wenn Berechtigungsnachweise vorhanden sind, kann {{site.data.keyword.conversationshort}} eine Verbindung zu der {{site.data.keyword.discoveryshort}}-Serviceinstanz für Sie herstellen. Wenn Sie der Ansicht sind, dass eine {{site.data.keyword.discoveryshort}}-Serviceinstanz nicht aufgelistet wird, die eigentlich aufgelistet werde müsste, öffnen Sie die Instanz direkt über das {{site.data.keyword.cloud_notm}}-Dashboard, damit die zugehörigen Berechtigungsnachweise generiert werden. 

1.  Führen Sie eine der folgenden Aktionen aus, um anzugeben, welche Datensammlung verwendet werden soll:
{: #pick-data-collection}

    - Wählen Sie eine vorhandene Datensammlung aus.

      Sie können auf den Link *In Discovery öffnen * klicken, um die Konfiguration einer Datensammlung zu überprüfen, bevor Sie festlegen, welche Datensammlung verwendet werden soll. 

      Fahren Sie mit [Suche konfigurieren](#beta-search-skill-add-configure) fort.

    - Wenn Sie nicht über eine Datensammlung verfügen und keine der aufgelisteten Datensammlungen verwenden möchten, klicken Sie auf **Neue Datensammlung erstellen**, um eine neue Datensammlung hinzuzufügen. Führen Sie die in [Datensammlung erstellen](#beta-search-skill-add-create-discovery-collection) beschriebenen Schritte aus.

      Die Schaltfläche **Neue Datensammlung erstellen** wird nicht angezeigt, wenn der in Ihrem {{site.data.keyword.discoveryshort}}-Serviceplan festgelegte Grenzwert für zulässige Datensammlungen erreicht ist. Details zum Grenzwert für den Preistarif enthält der Abschnitt [Preisstrukturpläne für {{site.data.keyword.discoveryshort}} ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/discovery/discovery-about?topic=discovery-discovery-pricing-plans).

## Watson Discovery-Serviceinstanz erstellen
{: #skill-search-add-create-discovery}

1.  Klicken Sie zum Erstellen einer {{site.data.keyword.discoveryshort}}-Serviceinstanz auf **Neue Datensammlung erstellen**.

    Eine Instanz des {{site.data.keyword.discoveryshort}}-Service wird erstellt und eine Konfigurationsseite für die neue {{site.data.keyword.discoveryshort}}-Serviceinstanz wird geöffnet. 

    Eine Instanz des Lite-Plans für den Service wird in {{site.data.keyword.Bluemix_notm}} bereitgestellt (unabhängig von dem verwendeten {{site.data.keyword.conversationshort}}-Serviceplan). Wenn die {{site.data.keyword.discoveryshort}}-Serviceinstanz im Rahmen eines anderen Plans erstellt werden soll, brechen Sie den Vorgang an dieser Stelle ab. Erstellen Sie die Serviceinstanz in diesem Fall direkt über den [{{site.data.keyword.Bluemix_notm}}-Katalog ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/catalog/services/discovery) mit der Vorgehensweise [Verbindung zu vorhandener {{site.data.keyword.discoveryshort}}-Serviceinstanz herstellen](#skill-search-add-connect-discovery).
    {: important}

1.  Überprüfen Sie die Nutzungsbedingungen für die Instanz und klicken Sie anschließend auf **Akzeptieren**, um fortzufahren.

1.  [Erstellen Sie eine Datensammlung](#skill-search-add-create-discovery-collection).

## Datensammlung erstellen
{: #skill-search-add-create-discovery-collection}

Versuchen Sie nicht, die vorab aufbereitete Datenquelle mit dem Namen *Watson Discovery News* zu Ihrer Instanz hinzuzufügen. Dieser Datentyp kann nicht mit {{site.data.keyword.conversationshort}} durchsucht werden.
{: important}

1.  Verwenden Sie eine der folgenden Vorgehensweisen, um eine {{site.data.keyword.discoveryshort}}-Datensammlung zu erstellen: 

      - Um eine Datensammlung aus den Daten zu erstellen, die in einem Datenquellentyp gespeichert sind, der von {{site.data.keyword.discoveryshort}} unterstützt wird, klicken Sie auf **Mit Datenquelle verbinden**.

        1.  Wählen Sie einen Datenquellentyp aus. 
        1.  Geben Sie die erforderlichen Informationen für die von Ihnen ausgewählte Datenquelle an und klicken Sie anschließend auf **Verbinden**.

            Weitere Details enthält der Abschnitt [Mit Datenquellen verbinden ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/discovery?topic=discovery-sources).
        1.  Geben Sie an, wie häufig Daten aus der Datenquelle mit der Datensammlung synchronisiert werden sollen, die Sie in {{site.data.keyword.discoveryshort}} erstellen.
        1.  Geben Sie an, welche Informationen aus der Datenquelle extrahiert und in Ihre {{site.data.keyword.discoveryshort}}-Datensammlung einbezogen werden sollen.

            Die angezeigten Optionen variieren je nach Datenquellentyp.

            - Für eine Salesforce-Datenquelle wählen Sie die Objekttypen aus, die aus den Quellendokumenten extrahiert werden sollen. Sie können beispielsweise einen [Objekttyp 'Fall'![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_case.htm#!) für einen *Fall* auswählen, der ein Kundenproblem repräsentiert.
            - Für eine Sharepoint-Datenquelle geben Sie Pfade an.
            - Für Dateirepositorys geben Sie Verzeichnisse oder Dateien an.

        1.  Klicken Sie auf **Daten speichern und synchronisieren**.

            Die Datensammlung wird erstellt. Nach der Erstellung wird im {{site.data.keyword.discoveryshort}}-Tool eine Übersichtsseite in einer separaten Browserregisterkarte angezeigt. 
        1.  Klicken Sie auf **Skill in {{site.data.keyword.conversationshort}} konfigurieren**, um zum {{site.data.keyword.conversationshort}}-Tool zurückzukehren.

      - Um eine Datensammlung durch Hochladen von Dokumenten zu erstellen, klicken Sie auf **Eigene Daten hochladen**.

        1.  Definieren Sie zuerst die Datensammlung und laden Sie anschließend die Dokumente hoch. Geben Sie die folgenden Informationen an: 

            - Name der Datensammlung. Der Name muss für diese Serviceinstanz eindeutig sein. 
            - Konfiguration. Sie können eine Standardkonfigurationsvorlage verwenden oder eine gespeicherte Konfiguration. Weitere Informationen zu Konfigurationen enthält der Abschnitt [Service konfigurieren ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/discovery?topic=discovery-configservice).
            - Sprache. Wählen Sie die Sprache für die Dateien aus, die Sie zu dieser Datensammlung hinzufügen möchten. Informationen zu den von {{site.data.keyword.discoveryshort}} unterstützten Sprachen enthält der Abschnitt [Sprachunterstützung ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/discovery?topic=discovery-language-support).
        1.  Laden Sie Dokumente hoch.

            Zu den unterstützten Dateitypen gehören PDF, HTML, JSON und DOC. Weitere Details enthält der Abschnitt [Inhalte hinzufügen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/discovery?topic=discovery-addcontent).
            {: note}

            Die fortlaufende Synchronisierung der hochgeladenen Dokumente wird nicht unterstützt. Wenn Sie Dokumentänderungen übernehmen möchten, laden Sie aktuellen Versionen der betreffenden Dokumente hoch.

Warten Sie, bis die Datensammlung vollständig eingepflegt wurde, bevor Sie zu {{site.data.keyword.conversationshort}} zurückkehren.

## Suchvorgang konfigurieren
{: #skill-search-add-configure}

1.  Klicken Sie in der {{site.data.keyword.conversationshort}}-Seite für Suchskills auf **Konfigurieren**.

1.  Entwerfen Sie verschiedene Nachrichten, die je nach Sucherfolg den Benutzern angezeigt werden sollen. 

    <table>
    <caption>Nachrichten für Suchergebnisse</caption>
    <tr>
      <th>Feldname</th>
      <th>Szenario</th>
      <th>Beispielnachricht</th>
    </tr>
    <tr>
      <td>Nachricht</td>
      <td>Es werden Suchergebnisse zurückgegeben.</td>
      <td>Diese Suchergebnisse könnten Ihnen weiterhelfen: </td>
    </tr>
    <tr>
      <td>Keine Ergebnisse gefunden</td>
      <td>Es werden keine Suchergebnisse gefunden.</td>
      <td>Beim Durchsuchen der Wissensbasis wurden keine hilfreichen Informationen zu Ihrer Anfrage gefunden.</td>
    </tr>
    <tr>
      <td>Fehlernachricht</td>
      <td>Der Service konnte den Suchvorgang nicht ausführen.</td>
      <td>Möglicherweise sind hilfreiche Informationen zu Ihrer Anfrage vorhanden, aber die Wissensbasis kann momentan nicht durchsucht werden.</td>
    </tr>
    </table>

1.  Wählen Sie die Felder der {{site.data.keyword.discoveryshort}}-Datensammlung aus, aus denen Text extrahiert werden soll. 

    Die verfügbaren Felder variieren entsprechend den eingepflegten Daten und der beim Einpflegen der Daten verwendeten Konfiguration. 

    Das Suchergebnis kann die folgenden Einzelinformationen enthalten: 

    - **Titel**: Der Titel für das Suchergebnis. Verwenden Sie das Titelfeld, das Namensfeld oder einen ähnlichen Feldtyp aus der Datensammlung als Titel für das Suchergebnis. 

      Die Option `Keine` darf in Facebook- und Slack-Integrationen nicht ausgewählt sein, damit Antworten angezeigt werden können. 
    - **Hauptteil**: Eine Beschreibung des Suchergebnisses. Verwenden Sie ein Kurzdarstellungs-, Zusammenfassungs- oder Hervorhebungsfeld aus der Datensammlung als Hauptteiltext für das Suchergebnis. 

      Die Option `Keine` darf in Facebook- und Slack-Integrationen nicht ausgewählt sein, damit Antworten angezeigt werden können. 
    - **URL**: Ein Hypertext-Link zu dem ursprünglichen Datenobjekt in der nativen Datenquelle. Die meisten Onlinedatenquellen stellen auf sich selbst verweisende, öffentliche URLs für Objekte im Datenspeicher bereit, um direkten Zugriff zu ermöglichen. 

      Die resultierende URL muss gültig und erreichbar sein, damit sie von der Slack-Integration in die Antwort eingefügt werden kann bzw. damit die Antwort von der Facebook-Integration angezeigt werden kann. Die Option `Keine` ist für Facebook- und Slack-Integrationen zulässig.

    Hilfetext finden Sie im Abschnitt [Tipps zum Auswählen von Datensammlungsfeldern](#skill-search-add-field-tips).
  
    Für mindestens eine der Optionen müssen Sie einen anderen Wert als `Keine` auswählen.

    Wenn in den Dropdown-Feldern keine Optionen zur Verfügung stehen, benötigt {{site.data.keyword.discoveryshort}} möglicherweise etwas mehr Zeit, bis die Datensammlung vollständig erstellt ist. Andernfalls enthält Ihre Datensammlung möglicherweise keine Dokumente oder beim Einpflegen treten Fehler auf, die zunächst behoben werden müssen. 

1.  Geben Sie im Vorschaubereich eine Testnachricht ein, um zu prüfen, welche Ergebnisse angezeigt werden, wenn Ihre Konfigurationsoptionen auf den Suchvorgang angewendet werden. Nehmen Sie bei Bedarf Anpassungen vor.

1.  Klicken Sie auf **Erstellen**.

Wenn Sie die Konfiguration zu einem späteren Zeitpunkt ändern möchten, öffnen und bearbeiten Sie den Suchskill. Sie müssen die Änderungen beim Bearbeiten nicht speichern, da sie automatisch angewendet werden. Wenn die Suchergebnisse Ihren Erwartungen entsprechen, klicken Sie auf **Speichern**, um das Konfigurieren des Suchskills abzuschließen.

## Nächste Schritte
{: #skill-search-add-next-steps}

Der erstellte Skill wird als Kachel auf der Seite 'Skills' angezeigt. 

Der Suchskill kann erst mit Kunden interagieren, nachdem er zu einem Assistenten hinzugefügt und der Assistent bereitgestellt wurde. Weitere Informationen enthält der Abschnitt [Assistenten erstellen](/docs/services/assistant?topic=assistant-assistant-add).

Wenn Sie sowohl einen Dialogskill als auch einen Suchskill mit einem Assistenten verknüpfen, wird der Suchskill automatisch ausgelöst, sobald der Dialogskill Benutzereingaben verarbeitet. Der Suchskill kann nicht von den Dialogmodulknoten des Assistenten adressiert werden. Anstelle einer Antwort in Form einer generischen Antwort vom Knoten `anything_else` wird ein Suchvorgang eingeleitet, der die Benutzereingabe als Abfragezeichenfolge verwendet.

Falls gewünscht, können Sie eine spezielle Suchabfrage definieren, die als Antwort auf eine bestimmte Knotenbedingung aufgerufen werden soll. Fügen Sie zu diesem Zweck einen Suchantworttyp zum Dialogmodulknoten hinzu. Weitere Details enthält der Abschnitt [Antworten](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia).

Wenn Sie über Ihren Dialogskill einen beliebigen Suchtyp einleiten, testen Sie das Dialogmodul, um sicherzustellen, dass der Suchvorgang wie erwartet ausgelöst wird. Wenn Sie zum Beispiel keine Suchantworttypen verwenden, sollten Sie testen, dass ein Suchvorgang nur ausgelöst wird, wenn keiner der vorhandenen Dialogmodulknoten die Benutzereingabe verarbeiten kann. Stellen Sie außerdem sicher, dass jeder ausgelöste Suchvorgang aussagefähige Ergebnisse liefert. 

### Tipps zum Auswählen von Datensammlungsfeldern
{: #skill-search-add-field-tips}

Die geeigneten Datensammlungsfelder, aus denen Daten extrahiert werden sollten, variieren je nach Datenquelle und Konfiguration zum Einpflegen der Datenquelle in Ihre Datensammlung. Weitere Informationen zur Struktur der Dokumente in Ihrer Datensammlung (einschließlich der Namen der Felder mit Informationen, die möglicherweise extrahiert werden sollen) erhalten Sie, indem Sie das {{site.data.keyword.discoveryshort}}-Tool öffnen und auf **Datenschema anzeigen** klicken.

In der folgenden Tabelle sind Datensammlungsfelder aufgeführt, die Sie als Einstieg verwenden können. Diese Vorschläge setzen voraus, dass beim Erstellen der Datensammlung die Standardkonfigurationsvorlage verwendet wurde. 

| Datenquellentyp    | Titel | Hauptteil | URL |
|--------------------|-------|------|-----|
| Hochgeladene PDF-Dokumente | enriched_text.concepts.text | text | Keine |
| Box                | Name | description | listing_url |

Die Datensammlungsfelder werden beim Erstellen der Datensammlung erstellt. Weitere Informationen zu Feldern, die generiert werden, wenn die Standardkonfigurationsvorlage zum Einpflegen verwendet wird (z. B. `enriched_text.concepts.text`) enthält der Abschnitt [Service konfigurieren > Aufbereitungen hinzufügen![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/discovery?topic=discovery-configservice#adding-enrichments).

### Skill zu einem Assistenten hinzufügen
{: #skill-search-add-to-assistant}

Sie können einen Skill in einem Assistenten hinzufügen. Dazu müssen Sie die Kachel des Assistenten öffnen und den Skill auf der Konfigurationsseite des Assistenten zum Assistenten hinzufügen. Auf der Konfigurationsseite für Skills können Sie nicht auswählen, von welchem Assistenten der Skill verwendet werden soll. 

Ein Suchskill kann von mehreren Assistenten verwendet werden. 

1.  Klicken Sie in der Registerkarte 'Assistenten' auf die entsprechende Kachel, um den Assistenten zu öffnen, dem Sie den Skill hinzufügen möchten.

1.  Klicken Sie auf **Suchskill hinzufügen**.

1.  Klicken Sie auf **Vorhandenen Skill hinzufügen**.

    Klicken Sie in den angezeigten verfügbaren Skills auf den Skill, den Sie hinzufügen möchten. 

Konfigurieren Sie mindestens einen Testintegrationskanal. Der Suchskill kann nicht auf der Seite 'Ausprobieren' getestet werden. Testen Sie den Skill in einem Integrationskanal, indem Sie Abfragen eingeben, die den Suchvorgang auslösen. Stellen Sie sicher, dass der Suchvorgang korrekt ausgelöst wird und relevante Ergebnisse liefert. 

Die Integration für gemeinsam nutzbare Links ist für Assistenten mit einem Suchskill derzeit nicht verfügbar.
{: important}
