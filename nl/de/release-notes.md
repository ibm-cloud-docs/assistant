---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# Releaseinformationen
{: #release-notes}

## Versionierung der Service-API
{: #release-notes-api-version}

API-Anforderungen erfordern einen Parameter 'version', der das Datum im Format `version=JJJJ-MM-TT` annimmt. Bei jeder abwärtskompatiblen Änderung der API wird eine neue untergeordnete Version der API freigegeben.

Senden Sie den Parameter 'version' in jeder API-Anforderung. Der Service verwendet die API-Version des von Ihnen angegebenen Datums oder die letzte Version vor diesem Datum. Verwenden Sie nicht standardmäßig das aktuelle Datum. Geben Sie stattdessen ein Datum an, das mit einer Version übereinstimmt, die mit Ihrer App kompatibel ist, und ändern Sie es erst dann, wenn Ihre App für eine neuere Version bereit ist.

- Die aktuelle Version 1 ist `2019-02-28`.
- Die aktuelle Version 2 ist `2019-02-28`.
- In der Anzeige 'Ausprobieren' der {{site.data.keyword.conversationshort}}-Tools wird die Version `2018-07-10` verwendet.

## Features als Betaversion
{: #release-notes-beta}

IBM gibt Services, Features und Sprachunterstützung für Sie zum Bewerten frei, die als Betaversion klassifiziert sind. Diese Services können unter Umständen instabil sein, häufig geändert werden und nach Ankündigung kurzfristig eingestellt werden. Außerdem bieten Beta-Features möglicherweise nicht dieselben Leistungswerte oder dieselbe Kompatibilität wie allgemein verfügbare Features und sind nicht für die Verwendung in einer Produktionsumgebung bestimmt. Betaversionsfeatures werden nur in [IBM Developer Answers ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developer.ibm.com/answers/topics/watson-assistant/){: new_window} unterstützt.

## Aktualisierte Modelle
{: #release-notes-updated-models}

Die {{site.data.keyword.conversationshort}}-Algorithmen können aufgrund von Rückmeldungen, wissenschaftlichen Verbesserungen und weiteren Faktoren regelmäßig optimiert und aktualisiert werden, um die Leistung kontinuierlich zu verbessern. Aktualisierungen des Modells werden in den vorliegenden Releaseinformationen mitgeteilt.

Vorhandene Modelle, die Sie trainiert haben, sind nicht unmittelbar betroffen. Abgelaufene Modelle werden jedoch 60 Tage nach Beginn der Verfügbarkeit eines neuen Modells auf das aktuelle Modell aktualisiert, wenn Sie dies bis dahin nicht ausgeführt haben.

**Hinweis:** Diese Aktualisierungsaussage gilt nur für Sprachen und Features mit dem Status 'GA' (= allgemein verfügbar).

Die folgenden neuen Features und Änderungen am Service sind verfügbar. Detaillierte Informationen zum potenziellen Nutzen der neuesten Features für Ihr Unternehmen enthält unser [Blog ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://medium.com/ibm-watson/assistant/home).

## 1. März 2019
{: #1March2019}

- **Vereinfachte Navigation**: Die Seitenleiste für Navigation mit separaten Registerkarten *Erstellen*, *Verbessern* und *Bereitstellen* wurde entfernt. Alle Tools, die Sie zum Erstellen eines Dialogskills benötigen, sind jetzt auf der Hauptseite für Skills verfügbar. 

- **Seite 'Verbessern' heißt jetzt 'Analyse'**: Die Metriken für Informationszwecke, die der Service aus Dialogen zwischen Ihren Benutzern und Ihrem virtuellen Assistenten generiert, wurden aus der Registerkarte *Verbessern* der Seitenleiste auf die Hauptseite für Skills mit dem Namen **Analyse** verschoben.

## 28. Februar 2019
{: #28February2019}

- **Neue API-Version**: Die aktuelle API-Version ist jetzt `2019-02-28`. Mit dieser Version wurden die folgenden Änderungen eingeführt: 

    - Die Reihenfolge der Auswertung von Bedingungen in Knoten mit Slots wurde geändert. Bisher wurde bei einem Knoten mit Slots, für den abgehende Abschweifungen zulässig waren, der Stammknoten `anything_else` ausgelöst, bevor die Bedingungen 'Nicht gefunden' auf Slotebene ausgewertet werden konnten. Die Reihenfolge der Operationen wurde angepasst, um dieses Verhalten zu ändern. Wenn ein Benutzer von einem Knoten mit Slots abschweift, werden jetzt alle Stammknoten außer dem Knoten `anything_else` verarbeitet. Danach werden die Bedingungen 'Nicht gefunden' auf Slotebene ausgewertet. Erst zum Abschluss wird der Knoten `anything_else` der Stammebene verarbeitet. Eine ausführliche Erläuterung der vollständigen Verarbeitungsreihenfolge für einen Knoten mit Slots enthält der Abschnitt [Tipps für die Verwendung von Slots](/docs/services/assistant?topic=assistant-dialog-slots#dialog-slots-node-level-handler).

    - Mit einem Nummernzeichen (#) beginnende Zeichenfolgen in den Objekten `context` und `output` einer Nachricht werden nicht mehr als Absichtsreferenzen behandelt.
  
      Bislang wurden diese Zeichenfolgen automatisch als Absichten behandelt. Wenn Sie beispielsweise eine Kontextvariable wie `"color":"#FFFFFF"` angegeben haben, wurde der hexadezimale Farbcode (#FFFFFF) als eine Absicht behandelt. Dabei wurde vom Service geprüft, ob eine Absicht mit dem Namen '#FFFFFF' in der Benutzereingabe enthalten war. Wenn dies nicht zutraf, wurde '#FFFFFF' durch `false` ersetzt. Diese Ersetzung findet nicht mehr statt. 
  
      Ebenso musste einem in der Textzeichenfolge einer Knotenantwort enthaltenes Nummernzeichen (#) ein Backslash (`\`) als Escapezeichen vorangestellt werden. Beispiel: `Wir sind die \#1 im Verkauf von Hummerbrötchen in Maine.` Das Symbol `#` in einer Textantwort muss nicht mehr mit einem Escapezeichen versehen werden.

      Diese Änderung gilt nicht bei Knotenbedingungen oder Bedingungen für bedingte Antworten. Mit einem Nummernzeichen (#) beginnende Zeichenfolgen in Bedingungen werden weiterhin als Absichtsreferenzen behandelt. Außerdem können Sie mithilfe der SpEL-Ausdruckssyntax erzwingen, dass das System eine Zeichenfolge in den Objekten `context` oder `output` einer Nachricht als Absicht behandelt. Beispiel: Geben Sie die Absicht als `<? #intent-name ?>` an.

## 25. Februar 2019
{: #25February2019}

**Erweiterung für Slack-Integration**: Sie können jetzt in einem Slack-Kanal den Typ des Ereignisses auswählen, das Ihren Assistenten auslöst. Bisher erfolgte bei einem Assistenten mit Slack-Integration die Interaktion des Assistenten mit Benutzern über einen direkten Nachrichtenkanal. Jetzt können Sie den Assistenten so konfigurieren, dass er Erwähnungen überwacht und antwortet, wenn er in anderen Kanälen erwähnt wird. Sie können wahlweise einen oder beide Ereignistypen als Methode verwenden, über die Ihr Assistent mit Benutzern interagiert. 

## 11. Februar 2019
{: #11February2019}

**Integration mit Intercom**: IBM hat mit Intercom, einer führenden Messagingplattform für Kundenservice, einen Partner gefunden, um das Team durch einen neuen Agenten (eine virtuelle Watson Assistant-Instanz) zu ergänzen. Sie können in Ihren Assistenten eine Intercom-Anwendung integrieren, um in Ihrer App die nahtlose Übertragung von Benutzerdialogen zwischen Ihrem Assistenten und Servicemitarbeitern zu ermöglichen. Diese Integration ist nur für Benutzer des Plus- und des Premium-Plans verfügbar. Weitere Details enthält der Abschnitt [In Intercom integrieren](/docs/services/assistant?topic=assistant-deploy-intercom).

## 8. Februar 2019
{: #8February2019}

- **Skillversionen erstellen**: Sie können jetzt einen Snapshot mit Absichten, Entitäten, Dialogmodul und Konfigurationseinstellungen für einen Skill an entscheidenden Stellen im Entwicklungsprozess aufzeichnen. Mit Versionen können Sie ohne Risiko kreativ werden. Sie können neue Designkonzepte in einer Testumgebung bereitstellen und auswerten, bevor Sie Updates auf eine Produktionsbereitstellung Ihres Assistenten anwenden. Weitere Details enthält der Abschnitt [Skillversionen erstellen](/docs/services/assistant?topic=assistant-versions).

- **Arabischer Inhaltskatalog**: Benutzer von Skills in arabischer Sprache können jetzt vordefinierte Absichten zu ihren Dialogmodulen hinzufügen. Weitere Informationen enthält der Abschnitt [Inhaltskataloge verwenden](/docs/services/assistant?topic=assistant-catalog).

## 17. Januar 2019
{: #17January2019}

- **Sprachunterstützung für Tschechisch allgemein verfügbar**: Die Sprachunterstützung für Tschechisch ist jetzt allgemein verfügbar und nicht länger als Betafunktion klassifiziert. Weitere Informationen enthält der Abschnitt [Unterstützte Sprachen](/docs/services/assistant?topic=assistant-language-support).

- **Verbesserte Sprachunterstützung**: Die Komponenten für Sprachverarbeitung wurden aktualisiert, um die folgenden Features zu verbessern: 

  - Systementitäten in Deutsch und Koreanisch
  - Zerlegung der Absichtsklassifizierung in Tokens für Arabisch, Niederländisch, Französisch, Italienisch, Japanisch, Portugiesisch und Spanisch

## 4. Januar 2019
{: #4January2019}

- **IBM Cloud Functions an den Standorten Washington DC und London**: Sie können jetzt programmgesteuerte Aufrufe an IBM Cloud Functions über das Dialogmodul eines Assistenten in einer Serviceinstanz ausführen, die in den Rechenzentren London und Washington DC gehostet werden. Weitere Informationen enthält der Abschnitt [Programmgesteuerte Aufrufe über einen Dialogmodulknoten absetzen](/docs/services/assistant?topic=assistant-dialog-actions).

- **Neue Methoden zum Arbeiten mit Arrays**: Die folgenden Methoden für SpEL-Ausdrücke sind verfügbar und erleichtern das Arbeiten mit Array-Werten in Ihrem Dialogmodul:

  - **JSONArray.filter**: Filtert ein Array durch Vergleichen jedes Werts in dem Array mit einen Wert, der je nach Benutzereingabe variieren kann.
  - **JSONArray.includesIntent**: Prüft, ob ein Array `intents` eine bestimmte Absicht enthält.
  - **JSONArray.indexOf**: Ruft die Indexnummer eines bestimmten Werts in einem Array ab.
  - **JSONArray.joinToArray**: Formatiert Werte, die aus einem Array zurückgegeben werden.

   Weitere Details enthält der Abschnitt [Dokumentation der Array-Methoden](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-arrays).

## 13. Dezember 2018
{: #13December2018}

- **Rechenzentrum London**: Sie können jetzt {{site.data.keyword.conversationshort}}-Serviceinstanzen erstellen, die im Rechenzentrum London ohne Syndikation gehostet werden. Weitere Details enthält der Abschnitt [Rechenzentren](/docs/services/assistant?topic=assistant-services-information#services-information-regions).

- **Geänderte Begrenzungen für Dialogmodulknoten**: Die Begrenzung für Dialogmodulknoten wurde für neue Instanzen des Standard-Plans vorübergehend von 100.000 auf 500 geändert. Diese geänderte Begrenzung wurde später rückgängig gemacht. Wenn Sie innerhalb des Zeitrahmens der geänderten Begrenzung eine Instanz des Standard-Plans erstellt haben, hatte sich dies möglicherweise auf Ihre Dialogmodule ausgewirkt. Die Begrenzung galt für Skills, die zwischen dem 10. Dezember und dem 12. Dezember 2018 erstellt wurden. Die geringeren Grenzwerte werden im Januar aus allen betroffenen Instanzen entfernt. Wenn der geringere Grenzwert schon früher aufgehoben werden muss, öffnen Sie ein Support-Ticket.

## 1. Dezember 2018
{: #1December2018}

   Führen Sie eine der folgenden Aktionen aus, um die Anzahl der Dialogmodulknoten in einem Dialogskill festzustellen:

   - Verwenden Sie das Tool, sofern es noch keinem Assistenten zugeordnet ist, um den Dialogskill einem Assistenten zuzuordnen, und zeigen Sie anschließend die Kachel für den Skill auf der Hauptseite des Assistenten an. Im Abschnitt *Trainierte Daten* wird die Anzahl der Dialogmodulknoten angegeben. 

   - Senden Sie eine GET-Anforderung an den API-Endpunkt '/dialog_nodes' und fügen Sie den Parameter `include_count=true` ein. Beispiel:

     ```curl
     curl -u "apikey:{apikey}" "https://gateway.watsonplatform.net/assistant/api/v1/workspaces/{workspace_id}/dialog_nodes?version=2018-09-20&include_count=true"
     ```

     In der Antwort ist die Anzahl der Dialogmodulknoten im Attribut `total` des Objekts `pagination` enthalten.

     Informationen zum Bearbeiten von Skills, die Sie weiter verwenden möchten, enthält der Abschnitt [Fehlerbehebung beim Importieren von Skills](/docs/services/assistant?topic=assistant-skill-add#skill-add-import-errors).

## 27. November 2018
{: #27November2018}

- **Der Plus-Plan ist als neuer Serviceplan verfügbar**: Der neue Plan bietet Features der Premium-Kategorie zu einem reduzierten Preistarif. Im Unterschied zu vorherigen Plänen wird der Plus-Plan auf Benutzerbasis abgerechnet. Dabei wird die Nutzung an der Anzahl der eindeutigen Benutzer gemessen, die in einem bestimmten Zeitraum mit Ihrem Assistenten interagieren. Um den Plan optimal zu nutzen, gestalten Sie beim Erstellen einer eigenen Clientanwendung Ihre App so, dass sie eine eindeutige ID für jeden Benutzer definiert und die Benutzer-ID mit jedem Aufruf der API '/message' übergibt. Bei den Standardintegrationen wird die Sitzungs-ID verwendet, um Benutzerinteraktionen mit dem Assistenten zu identifizieren. Weitere Informationen enthält der Abschnitt [Benutzerbasierte Pläne](/docs/services/assistant?topic=assistant-services-information#services-information-user-based-plans).

  <table>
  <caption>Begrenzungen des Plus-Plans</caption>
    <tr>
      <th>Artefakt</th>
      <th>Grenzwert</th>
    </tr>
    <tr>
      <td>Assistenten</td>
      <td>100</td>
    </tr>
    <tr>
       <td>Kontextbasierte Entitäten</td>
       <td>20</td>
    </tr>
    <tr>
       <td>Annotation für kontextbasierte Entitäten</td>
       <td>2.000</td>
    </tr>
    <tr>
       <td>Dialogmodulknoten</td>
       <td>100.000</td>
    </tr>
    <tr>
       <td>Entitäten</td>
       <td>1.000</td>
    </tr>
    <tr>
       <td>Entitätssynonyme</td>
       <td>100.000</td>
    </tr>
    <tr>
       <td>Entitätswerte</td>
       <td>100.000</td>
    </tr>
    <tr>
       <td>Absichten</td>
       <td>2.000</td>
    </tr>
    <tr>
       <td>Benutzerbeispiele für Absichten</td>
       <td>25.000</td>
    </tr>
    <tr>
       <td>Integrationen</td>
       <td>100</td>
    </tr>
    <tr>
       <td>Protokolle</td>
       <td>30 Tage</td>
    </tr>
    <tr>
       <td>Skills</td>
       <td>50</td>
    </tr>
  </table>

- **Benutzerbasierter Premium-Plan**: Die Abrechnung des Premium-Plans basiert jetzt auf der Anzahl der aktiven eindeutigen Benutzer. Wenn Sie diesen Plan verwenden, gestalten Sie alle von Ihnen erstellten, angepassten Anwendungen so, dass die Benutzer, die Aufrufe der API '/message' generieren, ordnungsgemäß identifiziert werden. Weitere Informationen enthält der Abschnitt [Benutzerbasierte Pläne](services-information#user-based-plans).

  Diese Änderung hat keine Auswirkungen auf bereits vorhandene Serviceinstanzen des Premium-Plans; sie verwenden weiterhin die API-basierten Abrechnungsmethoden. Nur für bereits vorhandene Benutzer des Premium-Plans wird der API-basierte Plan als Planoption *Premium (API)* aufgelistet.
  {: note}

  Weitere Informationen zu allen verfügbaren Serviceplänen finden Sie unter {{site.data.keyword.conversationshort}}-[Serviceplanoptionen ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window}.

- **Empfehlungen für Benutzerbeispiele für Absichten ![Nur Plus-Plan oder Premium-Plan](images/premium.png)**: Sie können eine Datei mit unformatierten Benutzereingaben hochladen (z. B. Benutzeranfragen aus einem Call-Center-Protokoll), die vom Service analysiert und nach potenziellen Benutzerbeispielen für Absichten durchsucht werden können. Weitere Informationen enthält der Abschnitt [Beispiele aus Protokolldateien hinzufügen](intent-recommendations#intent-recommendations-get-example-recommendations).

## 20. November 2018
{: #20November2018}

**Empfehlungen wurden eingestellt**: Der Abschnitt 'Empfehlungen' auf der Registerkarte 'Verbessern' wurde entfernt. Die Empfehlungen wurden als Betafeature nur für Benutzer des Premium-Plans zur Verfügung gestellt. Dabei wurden Aktionen empfohlen, mit denen die Benutzer ihre Trainingsdaten verbessern konnten. Empfehlungen werden nicht mehr an einer zentralen Stelle konsolidiert, sondern in den Bereichen des Tools bereitgestellt, in denen Änderungen an den Trainingsdaten vorgenommen werden können. Beispielsweise können Sie jetzt beim Hinzufügen von Synonymen für Entitäten eine Liste mit synonymen Begriffen aufrufen, die von dem Service empfohlen werden. Wenn Sie nach anderen Verfahren zum Analysieren Ihrer Benutzerdialogprotokolle suchen, ziehen Sie die Verwendung von Jupyter-Notizbüchern in Betracht. Weitere Details enthält der Abschnitt [Erweiterte Tasks](/docs/services/assistant?topic=assistant-logs-resources).

## 9. November 2018
{: #9November2018}

- **Grundlegende Überarbeitung der Benutzerschnittstelle**: Der {{site.data.keyword.conversationshort}}-Service hat jetzt ein neues Erscheindungsbild und zusätzliche Features.

  Die vorliegende Version des Tools wurde in den vergangenen Monaten von Teilnehmern des Betaprogramms bewertet.

  - **Skills**: Was Sie aus den Vorgängerversionen als *Arbeitsbereich* kennen, wird jetzt als *Skill* bezeichnet. Ein *Dialogskill* ist ein Container für Trainingsdaten und Artefakte zur Verarbeitung natürlicher Sprache, die Ihren Assistenten in die Lage versetzen, Fragen von Benutzern zu verstehen und zu beantworten. 

    **Wo sind meine Arbeitsbereiche?** Alle Arbeitsbereiche, die Sie bereits erstellt haben, werden jetzt in Ihrer Serviceinstanz als Skills aufgelistet. Klicken Sie auf die Registerkarte **Skills**, um sie anzuzeigen. Weitere Informationen enthält der Abschnitt [Skills](/docs/services/assistant?topic=assistant-skills).

  - **Assistenten**: Sie können Ihren Skill jetzt mit nur zwei Schritten veröffentlichen. Fügen Sie den Skill zu einem Assistenten hinzu und richten Sie mindestens eine Integration ein, in der Ihr Skill bereitgestellt werden soll. Der Assistent fügt eine übergeordnete Funktionsebene über Ihrem Skill hinzu, die es dem Service ermöglicht, den Informationsaustausch für Sie zu koordinieren und zu verwalten. Weitere Informationen enthält der Abschnitt [Assistenten](/docs/services/assistant?topic=assistant-assistants).

  - **Standardintegrationen**: Anstatt Ihren Arbeitsbereich über die Registerkarte **Bereitstellen** zu implementieren, fügen Sie Ihren Dialogskill zu einem Assistenten hinzu und fügen in dem Assistenten Integrationen hinzu, die den Skill für Ihre Benutzer verfügbar machen. Sie müssen weder eine angepasste Front-End-Anwendung erstellen noch den Dialogstatus von einem Aufruf bis zum nächsten verwalten. Sie können dies jedoch nach wie vor tun, falls gewünscht. Weitere Informationen enthält der Abschnitt [Integrationen hinzufügen](/docs/services/assistant?topic=assistant-deploy-integration-add).

  - **Neue Hauptversion der API**: Die Version 2 der API ist jetzt verfügbar. Diese Version bietet Zugriff auf Methoden, die Sie zum Interagieren mit einem Assistenten während der Laufzeit verwenden können. Kein mühsames Übergeben des Kontexts in jedem API-Aufruf mehr. Der Sitzungsstatus wird als Teil der Assistentenebene für Sie verwaltet.
  
    Was in den Tools als Dialogskill dargestellt wird, ist im Grunde eine Oberfläche für einen Arbeitsbereich aus Version 1. Die API der Version 2 bietet derzeit keine API-Methoden für das Authoring von Skills und Assistenten. Für das Authoring von Arbeitsbereichen können Sie jedoch weiterhin die API der Version 1 verwenden. Weitere Details enthält der Abschnitt [API im Überblick](/docs/services/assistant?topic=assistant-api-overview).
    {: note}

  - **Datenquellen wechseln**: Es ist jetzt leichter möglich, das Modell in einem Skill mithilfe von Benutzerdialogprotokollen aus einem anderen Skill zu verbessern. Sie sind nicht mehr auf Bereitstellungs-IDs angewiesen, sondern können einfach den Namen des Assistenten auswählen, indem ein Skill hinzugefügt und bereitgestellt wurde, um die zugehörigen Daten zu nutzen. Weitere Informationen enthält der Abschnitt [Verbesserungen für mehrere Assistenten](/docs/services/assistant?topic=assistant-logs#logs-deploy-id).

  Das nachfolgende Video bietet in zwei Minuten einen Überblick über das aktualisierte {{site.data.keyword.conversationshort}}-Tool.

  <iframe class="embed-responsive-item" id="youtubeplayer" title="Produktübersicht" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/OkW7gnHrw30?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

  - **Vorschaulinks für Instanzen in London**: Wenn Ihre Serviceinstanz in London gehostet wird, müssen Sie die Vorschaulink-URL bearbeiten. Die URL enthält einen Code für die Region, in der die Instanz gehostet wird. Da Instanzen in London nach Dallas syndiziert wurden, müssen Sie die Angabe `eu-gb` in der URL durch `us-south` ersetzen, damit die Webseite korrekt dargestellt wird. 

## 8. November 2018
{: #8November2018}

- **Rechenzentrum in Japan**: Sie können jetzt {{site.data.keyword.conversationshort}}-Serviceinstanzen erstellen, die im Rechenzentrum Tokyo gehostet werden. Weitere Details enthält der Abschnitt [Rechenzentren](/docs/services/assistant?topic=assistant-services-information#services-information-regions).

## 30. Oktober 2018
{: #30October2018}

- **Neuer API-Authentifizierungsprozess**: Im {{site.data.keyword.conversationshort}}-Service wurde von Cloud Foundry jetzt in den folgenden Regionen auf die tokenbasierte Authentifizierung mit Identity and Access Management (IAM) umgestellt:

  - Dallas (us-south)
  - Frankfurt (eu-de)

  Bei neuen Serviceinstanzen wird IAM für die Authentifizierung verwendet. Sie können entweder ein Trägertoken oder einen API-Schlüssel übergeben. Token unterstützen authentifizierte Anforderungen ohne eingebettete Serviceberechtigungsnachweise in jedem Aufruf. Für API-Schlüssel wird die Basisauthentifizierung verwendet. 

  Alle bereits vorhandenen Serviceinstanzen verwenden weiterhin Serviceberechtigungsnachweise (`{benutzername}:{kennwort}`) für die Authentifizierung.

  Weitere Informationen enthält der Abschnitt [API-Aufrufe authentifizieren](/docs/services/assistant?topic=assistant-services-information#services-information-authenticate-api-calls).

## 25. Oktober 2018
{: #25October2018}

- **Empfehlungen für Entitätssynonyme in zusätzlichen Sprachen verfügbar**: Unterstützung für Synonymempfehlungen wurde in den Sprachen Französisch, Japanisch und Spanisch hinzugefügt.

## 26. September 2018
{: #26September2018}

- **{{site.data.keyword.conversationfull}} ist in {{site.data.keyword.icpfull}}** verfügbar. Weitere Informationen finden Sie in der [{{site.data.keyword.icpfull}}-Dokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.3/featured_applications/watson_assistant.html).

## 21. September 2018
{: #21September2018}

- **Neue API-Version**: Die aktuelle API-Version ist jetzt `2018-09-20`. In dieser Version wird das Attribut `errors[].path` des von der API zurückgegebenen Fehlerobjekts als [JSON-Zeiger ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://tools.ietf.org/html/rfc6901) dargestellt und nicht in der Punktschreibweise.
- **Unterstützung für Webaktionen**: {{site.data.keyword.openwhisk_short}}-Webaktionen können jetzt über einen Dialogmodulknoten aufgerufen werden. Weitere Details enthält der Abschnitt [Programmgesteuerte Aufrufe über einen Dialogmodulknoten absetzen](/docs/services/assistant?topic=assistant-dialog-actions).

## 15. August 2018
{: #15August2018}

- **Verbesserte Unterstützung für unscharfe Suche**: Die unscharfe Suche wird für Entitäten in englischer Sprache vollständig unterstützt und das Feature für Rechtschreibfehler ist in vielen weiteren Sprachen kein reines Betafeature mehr. Details finden Sie im Abschnitt [Unterstützte Sprachen](/docs/services/assistant?topic=assistant-language-support).

## 6. August 2018
{: #6August2018}

- **Absichtskonflikte lösen![Nur Plus- oder Premium-Plan](images/premium.png)**: Das Tool unterstützt jetzt auch das Lösen von Konflikten, wenn zwei oder mehr Beispiele in verschiedenen Absichten Ähnlichkeiten aufweisen. Nicht eindeutige Benutzerbeispiele können die Trainingsdaten beeinträchtigen und es dem Service erschweren, Benutzereingaben während der Laufzeit der geeigneten Absicht zuzuordnen. Details enthält der Abschnitt [Absichtskonflikte lösen](/docs/services/assistant?topic=assistant-intents#intents-resolve-conflicts).

- **Vereindeutigung** ![Nur Plus- oder Premium-Plan](images/premium.png): Aktivieren Sie die Vereindeutigung, damit Ihr Assistent den Benutzer um Hilfe bitten kann, wenn eine Entscheidung zwischen zwei oder mehr infrage kommenden Dialogmodulknoten zu treffen ist, die für eine Antwort verarbeitet werden können. Weitere Details enthält der Abschnitt [Vereindeutigung](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).

- **Korrektur für Sprungaktionen**: Ein Fehler im Tool 'Dialogmodule' wurde behoben. Dieser Fehler hatte Sie daran gehindert, eine Sprungaktion zu konfigurieren, deren Ziel eine Knotenantwort mit der Sonderbedingung `anything_else` ist.

- **Nachricht für Abschweifungsrückkehr**: Sie können jetzt Text angeben, der dem Benutzer angezeigt wird, wenn nach einer Abschweifung wieder der vorherige Knoten aufgerufen wird. Zu diesem Zeitpunkt hat der Benutzer bereits die Eingabeaufforderung für den Knoten gesehen. Sie können die Nachricht ändern, um den Benutzern mitzuteilen, dass wieder der Knoten aufgerufen wird, von dem die Abschweifung ausging. Geben Sie zum Beispiel eine Antwort wie `Wo waren wir vorher? Ja genau...` an. Weitere Details enthält der Abschnitt [Abschweifungen](dialog-runtime#digressions).

## 12. Juli 2018
{: #12July2018}

- **Erweiterte Antworttypen**: Sie können jetzt erweiterte Antworten in Ihrem Dialogmodul hinzufügen, die neben Textzeichenfolgen zusätzliche Elementen wie Bilder oder Schaltflächen enthalten. Weitere Informationen enthält der Abschnitt [Erweiterte Antworten](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia).

- **Kontextbasierte Entitäten (Beta)**: Kontextbasierte Entitäten können Sie durch Beschriften von Äußerungen des Entitätstyps definieren, die in Benutzerbeispielen für Absichten vorkommen. Mithilfe dieser Entitätstypen kann der Service nicht nur relevante Begriffe lernen, sondern auch den Kontext, in dem diese Begriffe typischerweise in Benutzeräußerungen vorkommen. Auf diese Weise kann der Service bislang unbekannte Entitätserwähnungen allein anhand ihrer Referenzierung in Benutzereingaben erkennen. Wenn Sie beispielsweise beim Annotieren des Benutzerbeispiels 'Ich suche einen Flug nach Boston' das Wort 'Boston' als Entität '@destination' kennzeichnen, kann der Service das Wort 'Chicago' in der Benutzereingabe 'Ich suche einen Flug nach Chicago' als Entität '@destination' erkennen. Dieses Feature ist derzeit nur für die Sprache Englisch verfügbar. Weitere Informationen enthält der Abschnitt [Kontextbasierte Entitäten hinzufügen](/docs/services/assistant?topic=assistant-entities#entities-create-annotation-based).

  Wenn Sie das Tools in einem Internet Explorer-Web-Browser aufrufen, können weder Entitätserwähnungen in Benutzerbeispielen für Absichten beschriftet noch Texte für Benutzerbeispiele bearbeitet werden.
  {: note}

- **Entitätsempfehlungen**: Der Service kann jetzt Synonyme für Ihre Entitätswerte empfehlen. Die Empfehlungsfunktion findet zugehörige Synonyme basierend auf der kontextbezogenen Ähnlichkeit durch Extrahierung aus einem großen Datenbestand, der umfangreiche Quellen mit geschriebenem Text enthält. Dabei kommen Verarbeitungsverfahren für natürliche Sprache zum Einsatz, um Wörter zu finden, die Ähnlichkeit mit den vorhandenen Synonymen in Ihrem Entitätswert haben. Weitere Informationen enthält der Abschnitt [Synonyme](/docs/services/assistant?topic=assistant-entities#entities-synonyms).

- **Neue API-Version**: Die aktuelle API-Version ist jetzt `2018-07-10`. Mit dieser Version werden die folgenden Änderungen eingeführt:

  - Der Inhalt des Objekts `output` im API-Aufruf '/message' wurde von einem JSON-Objekt `text` in ein Array `generic` geändert, das mehrere erweiterte Antworttypen unterstützt, einschließlich `Bild`, `Option`, `Pause` und `Text`.
  - Unterstützung für kontextbasierte Entitäten wurde hinzugefügt. 

- **Datumsfilter für Übersichtsseite**: Mit den neuen Datumsfiltern können Sie den Zeitraum für die angezeigten Daten auswählen. Die Filter wirken sich auf alle auf der Seite angezeigten Daten aus, d. h. nicht nur auf die im Diagramm dargestellte Anzahl der Dialoge, sondern auch auf die angezeigten Statistikdaten für das Diagramm und auf die Listen der häufigsten Absichten und Entitäten. Weitere Informationen enthält der Abschnitt [Steuerelemente](logs-overview#controls).

- **Erweiterter Grenzwert für Muster**: Wenn Sie im Feld **Muster** [spezifische Muster für einen Entitätswert definieren](/docs/services/assistant?topic=assistant-entities#entities-patterns), ist das Muster (regulärer Ausdruck) jetzt auf 512 Zeichen begrenzt.

## 2. Juli 2018
{: #2July2018}

- **Sprungaktionen von bedingten Antworten aus **: Sie können jetzt eine bedingte Antwort so konfigurieren, dass eine Sprungaktion direkt zu einem anderen Knoten ausgeführt wird. Weitere Details enthält der Abschnitt [Bedingte Antworten](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multiple).

## 21. Juni 2018
{: #21June2018}

- **Erweiterte Sprachunterstützung für Systementitäten**: Sprachunterstützung für Niederländisch und vereinfachtes Chinesisch ist jetzt allgemein verfügbar. Für Niederländisch wird auch die unscharfe Suche nach Rechtschreibfehlern unterstützt. Die Sprachunterstützung für traditionelles Chinesisch schließt die Verfügbarkeit von [Systementitäten](/docs/services/assistant?topic=assistant-system-entities) als Beataversion mit ein. Details enthält der Abschnitt [Unterstützte Sprachen](/docs/services/assistant?topic=assistant-language-support).

## 14. Juni 2018
{: #14June2018}

- **Rechenzentrum in Washington DC eröffnet**: Sie können jetzt {{site.data.keyword.conversationshort}}-Serviceinstanzen erstellen, die im Rechenzentrum in Washington DC gehostet werden. Weitere Details enthält der Abschnitt [Rechenzentren](/docs/services/assistant?topic=assistant-services-information#services-information-regions).

- **Neuer API-Authentifizierungsprozess**: Im {{site.data.keyword.conversationshort}}-Service wird ein neuer API-Authentifizierungsprozess für Serviceinstanzen eingeführt, die in den folgenden Regionen gehostet werden: 

  - Washington DC (us-east) ab 14. Juni 2018
  - Sydney, Australien (au-syd) ab 7. Mai 2018

  {{site.data.keyword.cloud_notm}} wird auf die tokenbasierte Authentifizierung mit Identity and Access Management (IAM) umgestellt.

  Bei neuen Serviceinstanzen in den oben aufgelisteten Serviceinstanzen wird IAM für die Authentifizierung verwendet. Sie können entweder ein Trägertoken oder einen API-Schlüssel übergeben. Token unterstützen authentifizierte Anforderungen ohne eingebettete Serviceberechtigungsnachweise in jedem Aufruf. Für API-Schlüssel wird die Basisauthentifizierung verwendet. 

  Für alle neuen und vorhandenen Serviceinstanzen in anderen Regionen werden weiterhin Serviceberechtigungsnachweise (`{benutzername}:{kennwort}`) für die Authentifizierung verwendet.

  Wenn Sie ein Watson-SDK verwenden, können Sie den API-Schlüssel übergeben und den Lebenszyklus der Token vom SDK verwalten lassen. Weitere Informationen und Beispiele finden Sie unter [Authentifizierung ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")]https://{DomainName}/apidocs/assistant-v2#authentication){: new_window} in der API-Referenz.

  Wenn Sie nicht genau wissen, welcher Authentifizierungstyp verwendet werden sollte, öffnen Sie die Serviceinstanz durch Klicken in der [{{site.data.keyword.Bluemix_notm}}-Ressourcenliste ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/resources){: new_window}.

## 25. Mai 2018
{: #25May2018}

- **Neuer Beispielarbeitsbereich**: Der bereitgestellte Beispielarbeitsbereich, der zum Durchsuchen oder als Ausgangspunkt für einen eigenen Arbeitsbereich verwendet werden kann, wurde geändert. Der Beispielarbeitsbereich **Car Dashboard** wurde durch einen Beispielarbeitsbereich **Customer Service** ersetzt. Das neue Beispiel veranschaulicht die Verwendung von Absichten aus Inhaltskatalogen und weiterer neuer Features beim Erstellen von Bots. Der Arbeitsbereich kann häufig gestellte Fragen (z. B. Fragen zu Öffnungszeiten und Geschäftsstellen) beantworten und veranschaulicht die Verwendung eines Knotens mit Slots bei der Terminplanung für Geschäftsstellen.

- **HTML-Darstellung in der Anzeige 'Ausprobieren' hinzugefügt**: In der Anzeige 'Ausprobieren' kann jetzt die HTML-Formatierung aus dem Antworttext dargestellt werden. Bislang wurde beim Testen im Fenster 'Ausprobieren' der HTML-Quellcode angezeigt, wenn ein Hypertext-Link als HTML-Tag 'anchor' in eine Textantwort eingefügt wurde. Das sah ungefähr so aus: 

  `Kontaktieren Sie uns unter <a href="https://www.ibm.com">ibm.com</a>.`

  Jetzt wird der Hypertext-Link wie auf einer Webseite dargestellt. Die Darstellung sieht jetzt so aus:

  `Kontaktieren Sie uns unter ` [ibm.com](https://www.ibm.com){: new_window}.

    Beachten Sie, dass in Ihren Antworten der geeignete Syntaxtyp für die Clientanwendung verwendet werden muss, in der Sie den Dialog bereitstellen möchten. Verwenden Sie die HTML-Syntax nur, wenn sie von Ihrer Clientanwendung korrekt interpretiert werden kann. Für andere Integrationskanäle sind möglicherweise andere Formate erforderlich. 

- **Geänderte Bereitstellung**: Die Option **In Slack testen** wurde entfernt.

## 11. Mai 2018
{: #11May2018}

- **Informationssicherheit**: Die Dokumentation enthält neue Angaben zum Datenschutz. Weitere Informationen enthält der Abschnitt [Informationssicherheit](/docs/services/assistant?topic=assistant-information-security).

## 7. Mai 2018
{: #7May2018}

- **Rechenzentrum in Sydney (Australien) eröffnet**: Sie können jetzt {{site.data.keyword.conversationshort}}-Serviceinstanzen erstellen, die im Rechenzentrum in Sydney (Australien) gehostet werden. Weitere Details finden Sie unter [IBM Cloud-Rechenzentren weltweit ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link"](https://www.ibm.com/cloud/data-centers/).

## 4. April 2018
{: #4April2018}

- **Dialogmodule durchsuchen**: Sie können jetzt [Dialogmodulknoten durchsuchen](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-search), um ein bestimmtes Wort oder einen bestimmten Ausdruck zu finden.

## 15. März 2018
{: #15March2018}

- **Einführung von {{site.data.keyword.conversationfull}}**: {{site.data.keyword.ibmwatson}} Conversation wurde umbenannt. Das Produkt heißt jetzt {{site.data.keyword.conversationfull}}. Der geänderte Name macht deutlich, dass der Service erweitert wurde und jetzt vordefinierte Inhalte und Tools bietet, die es Ihnen leichter machen, die von Ihnen erstellten virtuellen Assistenten für die gemeinsame Nutzung freizugeben. Weitere Details finden Sie in [diesem Blogbeitrag ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/blogs/watson/2018/03/the-future-of-watson-conversation-watson-assistant/).

- **Neue REST-APIs und SDKs für Watson Assistant verfügbar**: Die neuen APIs bieten dieselbe Funktionalität wie die bestehenden Conversation-APIs, die weiter unterstützt werden. Weitere Informationen zu den Watson Assistant-APIs enthält die [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/assistant){: new_window}.

- **Erweiterungen für Dialogmodule**: Die folgenden Features wurden zum Tool für Dialogmodule hinzugefügt: 

  - Einfache Felder für Variablennamen und -werte sind jetzt verfügbar und können zum Hinzufügen von Kontextvariablen oder zum Ändern von Kontextvariablenwerten verwendet werden. Der JSON-Editor muss nur geöffnet werden, wenn dies ausdrücklich gewünscht wird. Weitere Details enthält der Abschnitt [Kontextvariable definieren](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-var-define).
  - Verwalten Sie Dialogmodule mithilfe von Ordnern, um verwandte Dialogmodulknoten in Gruppen zusammenzufassen. Weitere Details enthält der Abschnitt [Dialogmodul in Ordnern zusammenfassen](dialog-build#folders).
  - Unterstützung zum Anpassen der Mitwirkung einzelner Dialogmodulknoten an benutzergesteuerten abgehenden Abschweifungen vom vorgesehenen Dialogablauf. Weitere Details enthält der Abschnitt [Abschweifungen](dialog-runtime#digressions).

- **Absichten und Entitäten suchen**: Eine neu hinzugefügte Suchfunktion ermöglicht das [Durchsuchen von Absichten](intents#searching-intents) nach Benutzerbeispielen, Absichtsnamen oder Beschreibungen sowie das [Durchsuchen von Entitäten ](/docs/services/assistant?topic=assistant-entities#entities-search) nach Entitätswerten und Synonymen.

- **Inhaltskataloge**: Die neuen [Inhaltskataloge](/docs/services/assistant?topic=assistant-catalog#catalog-add) enthalten eine einzelne Kategorie mit vordefinierten, häufig vorkommenden Absichten und Entitäten, die Sie zu Ihrer Anwendung hinzufügen können. Beispielsweise ist für die meisten Anwendungen ein allgemeiner Absichtstyp für Begrüßungen (#greeting-type) erforderlich, um einen Dialog mit einem Benutzer einzuleiten. Sie können diese Absicht aus dem Inhaltskatalog hinzufügen, anstatt sie völlig neu zu erstellen. 

- **Erweiterte Benutzermetriken**: Die Komponente 'Verbessern' wurde durch zusätzliche Benutzermetriken und Protokollierungsstatistiken erweitert. Beispielsweise enthält die Seite 'Übersicht' mehrere neue und detaillierte Diagramme, die Interaktionen zwischen Benutzern und Ihrer Anwendung zusammenfassen sowie den Umfang des Datenverkehrs in einem bestimmten Zeitraum und die Absichten und Entitäten, die am häufigsten in Benutzerdialogen erkannt wurden. 

## 12. März 2018
{: #12March2018}

- **Neue Methoden für Datum und Uhrzeit**: Es wurden Methoden hinzugefügt, die das Ausführen von Datumsberechnungen innerhalb des Dialogmoduls erleichtern. Weitere Details enthält der Abschnitt [Datum und Uhrzeit berechnen](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-date-and-time-calculations).

## 16. Februar 2018
{: #16February2018}

- **Tracing für Dialogmodulknoten**: Beim Testen eines Dialogmoduls in der Anzeige 'Ausprobieren' wird neben jeder Antwort ein Positionssymbol angezeigt. Durch Klicken auf dieses Symbol können Sie den Pfad hervorheben, den der Service in der Baumstruktur des Dialogmoduls durchlaufen hat, um zu der Antwort zu gelangen. Details finden Sie unter [Dialogmodul erstellen](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-test).

- **Neue API-Version**: Die aktuelle API-Version ist jetzt `2018-02-16`. Mit dieser Version werden die folgenden Änderungen eingeführt:

  - In den meisten GET-Anforderungen wird jetzt ein neuer Parameter `include_audit` unterstützt. Dieser optionale boolesche Parameter gibt an, ob die Antwort die Prüfeigenschaften (Zeitmarken `created` und `updated`) enthalten soll. Der Standardwert ist `false`. (Wenn Sie eine API-Version vor `2018-02-16` verwenden, ist `true` der Standardwert.) Weitere Informationen enthält die [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/assistant){: new_window}.

  - Antworten von API-Aufrufen, die die neue API-Version verwenden, enthalten nur Eigenschaften mit Werten ungleich `null`.

  - Die Eigenschaften `output.nodes_visited` und `output.nodes_visited_details` in Nachrichtenantworten schließen jetzt auch Knoten der folgenden Typen ein, die bislang ausgelassen wurden:

    - Knoten mit `type`=`response_condition`
    - Knoten mit `type`=`event_handler` und `event_name`=`input`

## 9. Februar 2018
{: #9February2018}

- **Niederländische Systementitäten (Betaversion)**: Die Sprachunterstützung für Niederländisch wurde erweitert und enthält jetzt die Verfügbarkeit von [Systementitäten](/docs/services/assistant?topic=assistant-system-entities) als Betaversion. Details finden Sie im Abschnitt [Unterstützte Sprachen](/docs/services/assistant?topic=assistant-language-support).

## 29. Januar 2018
{: #29January2018}

- Die REST-API von {{site.data.keyword.conversationshort}} unterstützt jetzt die folgenden neuen Anforderungsparameter:
  - `append` - Verwenden Sie diesen Parameter beim Aktualisieren eines Arbeitsbereiches, um anzugeben, ob die vorhandenen Daten durch die neuen Arbeitsbereichsdaten ergänzt oder ersetzt werden sollen. Weitere Informationen finden Sie unter [Arbeitsbereich aktualisieren![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/apidocs/assistant?curl=#update-workspace){: new_window}.
  - `nodes_visited_details` - Verwenden Sie diesen Parameter beim Senden einer Nachricht, um anzugeben, ob die Antwort zusätzliche Diagnoseinformationen zu den Knoten enthalten soll, die beim Verarbeiten der Nachricht besucht wurden. Weitere Informationen finden Sie unter [Nachricht senden![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}.

## 23. Januar 2018
{: #23January2018}

- **Liste der Arbeitsbereiche kann nicht abgerufen werden**: Wenn beim Arbeiten mit dem Tool diese oder eine ähnlich lautende Fehlernachricht angezeigt wird, kann dies bedeuten, dass Ihre Sitzung abgelaufen ist. Melden Sie sich ab, indem Sie die Funktion **Abmelden** über das Symbol **Benutzerinformationen** ![Menü des Symbols 'Benutzerinformationen'](images/user-icon.png) auswählen, und melden Sie sich danach wieder an.

## 8. Dezember 2017
{: #8December2017}

- **Zugriff auf Protokolldaten in mehreren Instanzen (nur Benutzer der Kategorie 'Premium')**: Wenn Sie ein Benutzer der Premiumversion von {{site.data.keyword.conversationshort}} sind, können Ihre Premiuminstanzen optional so konfiguriert werden, dass der Zugriff auf die Protokolldaten über Arbeitsbereiche Ihrer verschiedenen Premiuminstanzen möglich ist.

- **Knoten kopieren**: Sie können jetzt einen Knoten duplizieren, um eine Kopie des Knotens und der zugehörigen untergeordneten Elemente zu erstellen. Diese Funktion ist hilfreich, wenn Sie einen Knoten mit nützlicher Logik erstellt haben, die Sie an anderer Stelle in Ihrem Dialogmodul wiederverwenden möchten. Weitere Informationen finden Sie unter [Dialogmodulknoten kopieren](dialog-build#copy-node).

- **Gruppen in Musterentitäten erfassen**: Sie können Gruppen in dem regulären Ausdrucksmuster identifizieren, das Sie für eine Entität definieren. Das Identifizieren von Gruppen ist hilfreich, wenn Sie später auf Teilbereiche des Musters verweisen möchten. Angenommen, Ihre Entität verfügt über ein Muster für einen regulären Ausdruck zum Erfassen von US-Telefonnummern. Wenn Sie das Vorwahlsegment des Musters für Telefonnummern als eine Gruppe identifizieren, können Sie später auf diese Gruppe verweisen, um allein auf das Vorwahlsegment einer Telefonnummer zuzugreifen. Weitere Informationen finden Sie unter [Entitäten definieren](/docs/services/assistant?topic=assistant-entities#entities-creating-task).

## 6. Dezember 2017
{: #6December2017}

- **{{site.data.keyword.openwhisk}}-Integration (Betaversion)**: Rufen Sie {{site.data.keyword.openwhisk}}-Aktionen (früher als IBM OpenWhisk bezeichnet) direkt aus einem Dialogmodulknoten auf. Diese Funktion ermöglicht beispielsweise das Aufrufen einer Aktion zum Abrufen von Wetterinformationen über einen Dialogmodulknoten und das Verwenden der zurückgegebenen Informationen als Bedingung in der Antwort des Dialogmoduls. Gegenwärtig können Sie eine Aktion über eine {{site.data.keyword.openwhisk_short}}-Instanz, die in der Region 'Vereinigte Statten (Süden)' gehostet wird, über {{site.data.keyword.conversationshort}} -Instanzen aufrufen, die in der Region 'Vereinigte Staaten (Süden)' gehostet werden. Weitere Details enthält der Abschnitt [Programmgesteuerte Aufrufe über einen Dialogmodulknoten absetzen](/doc/services/assistant?topic=assistant-dialog-actions).

## 5. Dezember 2017
{: #5December2017}

- **Überarbeitete Benutzerschnittstelle für Absichten und Entitäten**: Die Registerkarten `Absichten` und `Entitäten` wurden neu gestaltet und ermöglichen jetzt einen einfacheren und effizienteren Workflow beim Erstellen und Bearbeiten von Entitäten und Absichten. Weitere Informationen zum Arbeiten mit diesen Registerkarten finden Sie unter [Absichten definieren](intents#creating-intents) und [Entitäten definieren](/docs/services/assistant?topic=assistant-entities#entities-creating-task).

## 30. November 2017
{: #30November2017}

- **Unterstützung für ostarabische Zahlen**: Für Systementitäten in arabischer Sprache werden jetzt ostarabische Zahlen unterstützt.

## 29. November 2017
{: #29November2017}

- **Erkennung der Benutzereingaben durch mehrere Arbeitsbereiche verbessern**: Sie können jetzt einen Arbeitsbereich durch Äußerungen verbessern, die an andere Arbeitsbereiche in Ihrer Instanz gesendet wurden. Beispiel: Wenn Sie über mehrere Versionen von Produktions- und Entwicklungsarbeitsbereichen verfügen, können Sie dieselben Äußerungsdaten verwenden, um beliebige dieser Arbeitsbereiche zu verbessern. Weitere Informationen enthält der Abschnitt [Verbesserungen für mehrere Arbeitsbereiche](/docs/services/assistant?topic=assistant-logs#logs-deploy-id).

## 20. November 2017
{: #20November2017}

- **Konformität mit GB18030**: GB18030 ist eine chinesische Norm, die eine erweiterte Codepage für die Verwendung im chinesischen Markt spezifiziert. Diese normierte Codepage ist für die Softwarebranche von großer Bedeutung, da vom China National Information Technology Standardization Technical Committee verfügt wurde, dass jede Softwareanwendung, die nach dem 1. September 2001 im chinesischen Markt freigegeben wird, für GB18030 aktiviert sein muss. Der Service '{{site.data.keyword.conversationshort}}' unterstützt diese Codierung und ist für die Einhaltung von GB18030 zertifiziert.

## 9. November 2017
{: #9November2017}

- **Direkte Referenzierung von Entitäten in Beispielen für Absichten**: Sie können jetzt eine direkte Referenz auf eine Entität in einem Beispiel für Absichten angeben. Diese Entitätsreferenz und alle zugehörigen Werte oder Synonyme werden vom Klassifikationsmerkmal für den Service '{{site.data.keyword.conversationshort}}' zum Trainieren der Absicht verwendet. Weitere Informationen finden Sie unter [*Entität als Beispiel*](/docs/services/assistant?topic=assistant-intents#intents-entity-as-example) im Abschnitt [Absichten](/docs/services/assistant?topic=assistant-intents).

  Gegenwärtig können Sie nur von Ihnen definierte geschlossene Entitäten direkt referenzieren. Sie können keine [Musterentitäten](/docs/services/assistant?topic=assistant-entities#entities-pattern) oder [Systementitäten](/docs/services/assistant?topic=assistant-system-entities) direkt referenzieren.
  {: note}

## 8. November 2017
{: #8November2017}

- **{{site.data.keyword.conversationshort}}-Connector**: >Mit dem neuen {{site.data.keyword.conversationshort}}-Connector-Tool können Sie Ihren Arbeitsbereich mit einer eigenen Slack- oder Facebook Messenger-App verbinden und als Chat-Bot für die Interaktion mit Benutzern von Slack oder Facebook Messenger bereitstellen. Dieses Tool ist nur für die {{site.data.keyword.Bluemix_notm}}-Region 'Vereinigte Staaten (Süden)' verfügbar.

## 3. November 2017
{: #3November2017}

- **Aktualisierungen für Dialogmodule**: Die folgenden Aktualisierungen erleichtern Ihnen das Erstellen eines Dialogmoduls. (Details finden Sie unter [Dialogmodul erstellen](/docs/services/assistant?topic=assistant-dialog-build).)

    - Sie können eine Bedingung für einen Slot hinzufügen, damit der Slot nur erforderlich ist, wenn bestimmte Bedingungen erfüllt sind. Beispiel: Ein Slot, der den Namen eines Ehegatten abfragt, soll nur dann als erforderlich eingestuft werden, wenn ein vorheriger (erforderlicher) Slot, der den Familienstand abfragt, darauf hindeutet, dass der Benutzer verheiratet ist.

    - Sie können jetzt **Benutzereingabe überspringen** als nächsten Schritt für einen Knoten auswählen. Wenn diese Option ausgewählt wird, springt der Service nach der Verarbeitung des aktuellen Knotens direkt zum ersten untergeordneten Knoten des aktuellen Knotens. Diese Option entspricht weitgehend der vorhandenen Option *Springen zu* für nächste Schritte, sie bietet jedoch mehr Flexibilität. Sie müssen nicht genau angeben, zu welchem Knoten gesprungen werden soll. Der Service springt während der Laufzeit stets zum ersten zugehörigen untergeordneten Knoten, selbst wenn nach dem Definieren des Verhaltens für nächste Schritte die Reihenfolge der untergeordneten Knoten geändert wurde oder neue Knoten hinzugefügt wurden.

    - Sie können bedingte Antworten für Slots hinzufügen. Bei den Antworten für 'Gefunden' und für 'Nicht gefunden' können Sie anpassen, wie der Service reagiert, wenn bestimmte Bedingungen erfüllt sind. Diese Funktion ermöglicht das Erkennen und Korrigieren potenzieller Missverständnisse, bevor der vom Benutzer bereitgestellte Wert in der Kontextvariablen für den Slot gespeichert wird. Beispiel: Wenn der Slot das Alter des Benutzers speichert und die Variable `@sys-number` im Feld *Überprüfen auf* verwendet, um diese Information zu erfassen, können Sie eine Bedingung hinzufügen, die Zahlen größer als 100 findet und eine Antwort wie die folgende zurückgibt: *Bitte geben Sie eine gültiges Altersangabe in Jahren an.* Weitere Details finden Sie unter [Bedingungen zu Antworten für 'Gefunden' und 'Nicht gefunden' hinzufügen](/docs/services/assistant?topic=assistant-dialog-slots#dialog-slots-handler-next-steps).

    - Die Schnittstelle zum Hinzufügen von Antworten zu einem Knoten wurde überarbeitet um das Auflisten der einzelnen Bedingungen und der zugehörigen Antworten zu vereinfachen. Um bedingte Antworten auf Knotenebene hinzuzufügen, klicken Sie auf **Anpassen** und aktivieren Sie anschließend die Option **Mehrere Antworten**.

     Das Umschaltsteuerelement **Mehrere Antworten** aktiviert bzw. inaktiviert die Funktion nur für die Antwort auf Knotenebene. Es steuert nicht die Möglichkeit zum Hinzufügen bedingter Antworten für einen Slot. Die Einstellung für mehrere Antworten in Slots wird separat gesteuert.
     {: note}

    - Damit die Seite zum Bearbeiten eines Slots übersichtlich bleibt, stehen jetzt Menüoptionen für folgende Aufgaben zur Auswahl: a) eine Bedingung hinzufügen, die erfüllt sein muss, damit der Slot verarbeitet wird und b) bedingte Antworten für die Bedingungen für 'Gefunden' und 'Nicht gefunden' in einem Slot hinzufügen. Wenn Sie diese Zusatzfunktion nicht ausdrücklich hinzufügen, werden die Felder für Slotbedingung und für mehrere Antworten ausgeblendet, um die Seite übersichtlicher und benutzerfreundlicher zu gestalten.

## 25. Oktober 2017
{: #25October2017}

- **Aktualisierungen für vereinfachtes Chinesisch**: Die Sprachunterstützung für vereinfachtes Chinesisch wurde verbessert. Dies beinhaltet Verbesserungen für die Absichtsklassifizierung mithilfe von Worteinbettungen auf Zeichenebene und die Verfügbarkeit von Systementitäten. Die Lernmodelle des Service '{{site.data.keyword.conversationshort}}' wurden im Rahmen dieser Verbesserung ebenfalls aktualisiert und bei einem erneuten Training des Modells werden alle Änderungen angewendet. Weitere Informationen finden Sie unter [Aktualisierte Modelle](#release-notes-updated-models).
- **Aktualisierungen für Spanisch:** Die Absichtsklassifizierung für Spanisch wurde für sehr große Datensätze verbessert.

## 11. Oktober 2017
{: #11October2017}

- **Aktualisierungen für Koreanisch**: Die Sprachunterstützung für Koreanisch wurde verbessert. Die Lernmodelle des Service '{{site.data.keyword.conversationshort}}' wurden im Rahmen dieser Verbesserung ebenfalls aktualisiert und bei einem erneuten Training des Modells werden alle Änderungen angewendet. Weitere Informationen finden Sie unter [Aktualisierte Modelle](#release-notes-updated-models).

## 3. Oktober 2017
{: #3October2017}

- **Musterdefinierte Entitäten (Betaversion):** Sie können nun unter Verwendung von regulären Ausdrücken bestimmte Muster für eine Entität definieren. Dies kann für die Ermittlung von Entitäten von Nutzen sein, die ein definiertes Muster befolgen, z. B. Artikel- bzw. Teilenummern, Telefonnummern oder E-Mail-Adressen. Weitere Details enthält der Abschnitt [Musterdefinierte Entitäten](/docs/services/assistant?topic=assistant-entities#entities-pattern).
    - Sie können entweder Synonyme oder Muster für einen einzelnen Entitätswert hinzufügen, jedoch nicht beides.
    - Für jeden Entitätswert kann es höchstens 5 Muster geben.
    - Jedes Muster (also jeder reguläre Ausdruck) ist auf 128 Zeichen begrenzt.
    - Beim Importieren oder Exportieren über eine CSV-Datei werden Muster gegenwärtig nicht unterstützt.
    - Die REST-API unterstützt keinen direkten Zugriff auf Muster. Sie können Muster jedoch mit dem Endpunkt `/values` abrufen oder ändern.

- **Nach Wörterverzeichnis gefilterte unscharfe Suche (nur bei Englisch):** Für Englisch gibt es jetzt eine verbesserte Version der unscharfen Suche für Entitäten. Diese Verbesserung verhindert die Erfassung einiger gängiger und gültiger englischer Wörter als grobe Übereinstimmungen für eine bestimmte Entität. Bei der unscharfen Suche wird beispielsweise der Entitätswert `like` nicht als Übereinstimmung für die gültigen englischen Wörter `hike` oder `bike` gewertet, jedoch weiterhin beispielsweise für `lkie` oder `oike`.

## 27. September 2017
{: #27September2017}

- **Aktualisierungen beim Bedingungsbuilder:** Das Steuerelement, das angezeigt wird, um Sie beim Definieren einer Bedingung in einem Dialogmodulknoten zu unterstützen, wurde aktualisiert. Zu den Erweiterungen zählen die Unterstützung der Auflistung von verfügbaren Kontextvariablennamen, nachdem Sie das Zeichen $ eingegeben haben, um eine Kontextvariable hinzuzufügen.

## 31. August 2017
{: #31August2017}

- **Verbesserungen des Abschnittsrollbacks**: Die Metrik für die gemittelte Dialogzeit und entsprechende Filter wurden vorübergehend von der Seite 'Übersicht' des Abschnitts 'Verbessern' entfernt. Dies verhindert, dass die Berechnung bestimmter Metriken die Metrik für die gemittelte Dialogzeit verursacht und im Diagramm für Dialoge über einen bestimmten Zeitraum hinweg ungenaue Informationen angezeigt werden. IBM bedauert das Entfernen von Funktionalität aus dem Tool, setzt hierdurch jedoch seine Zusage um, die Bereitstellung von präzisen Informationen für die Benutzer sicherzustellen.
- **Namen von Dialogmodulknoten**: Einem Dialogmodulknoten kann nun ein beliebiger Name zugeordnet werden und der Name muss nicht eindeutig sein. Außerdem kann der Knotenname nachfolgend geändert werden, ohne dass hierdurch die interne Referenzierung des Knotens beeinflusst wird. Der Name, den Sie angeben, wird als Attribut 'title' des Knotens in der JSON-Datei für den Arbeitsbereich gespeichert; das System referenziert den Knoten mithilfe einer eindeutigen ID, die im Attribut 'name' gespeichert wird.

## 23. August 2017
{: #23August2017}

- **Aktualisierungen für Koreanisch, Japanisch und Italienisch**: Die Sprachunterstützung für Koreanisch, Japanisch und Italienisch wurde verbessert. Die Lernmodelle des Service '{{site.data.keyword.conversationshort}}' wurden im Rahmen dieser Verbesserung ebenfalls aktualisiert und bei einem erneuten Training des Modells werden alle Änderungen angewendet. Weitere Informationen finden Sie unter [Aktualisierte Modelle](#release-notes-updated-models).

## 10. August 2017
{: #10August2017}

- **Akzentnormalisierung**: In einer dialogorientierten Einstellung können Benutzer bei der Interaktion mit dem Service '{{site.data.keyword.conversationshort}}' Akzente verwenden oder nicht. Insofern wurde eine Aktualisierung des Algorithmus vorgenommen, damit Fassungen von Wörtern mit Akzent und ohne Akzent bei der Absichts- und Entitätserkennung identisch behandelt werden.

  Bei einigen Sprachen (z. B. Spanisch) können manche Akzente jedoch die Bedeutung der Entität ändern. Obwohl die Originalentität implizit einen Akzent besitzt, kann der Service daher bei der Entitätserkennung auch eine Übereinstimmung mit der Version derselben Entität ohne Akzent erzielen, allerdings mit einer etwas geringeren Konfidenzbewertung.

  Beispiel: Für das Wort `barrió` mit Akzent, das der Vergangenheitsform des Verbs `barrer` (fegen) entspricht, kann der Service auch eine Übereinstimmung mit dem Wort `barrio` (Nachbarschaft) ermitteln, jedoch mit einer etwas geringeren Konfidenzbewertung.

  Das System stellt die höchsten Konfidenzbewertungen in Entitäten mit exakten Übereinstimmungen bereit. Beispielsweise wird `barrio` nicht erkannt, wenn `barrió` im Trainingsset angegeben ist, und `barrió` wird nicht erkannt, wenn `barrio` im Trainingsset enthalten ist.

  Sie müssen dafür sorgen, dass das System mit den richtigen Zeichen und Akzenten trainiert wird. Falls Sie beispielsweise `barrió` als Antwort erwarten, müssen Sie `barrió` in das Trainingsset aufnehmen.

  Obwohl es sich streng genommen bei der Tilde nicht um ein Akzentzeichen handelt, gilt dasselbe für Wörter, die z. B. den spanischen Buchstaben `ñ` bzw. den Buchstaben `n` verwenden (z. B. `uña` und `una`). In diesem Fall ist der Buchstabe `ñ` nicht einfach ein `n` mit einem Akzent, sondern ein eindeutiger besonderer Buchstabe des Spanischen.

  Sie können die unscharfe Suche aktivieren, wenn Sie vermuten, dass Ihre Kunden möglicherweise nicht die richtigen Akzente verwenden oder Wörter falsch schreiben (beispielsweise `n` statt `ñ` verwenden). Sie können die Varianten aber auch explizit in die Trainingsbeispiele aufnehmen.

  **Hinweis:** Die Akzentnormalisierung ist für Portugiesisch, Spanisch, Französisch und Tschechisch aktiviert.

- **Flag zum Ablehnen von Arbeitsbereichen**: Die REST-API von {{site.data.keyword.conversationshort}} unterstützt jetzt ein Flag für die Ablehnung von Arbeitsbereichen. Dieses Flag gibt an, dass Trainingsdaten für den Arbeitsbereich wie Absichten und Entitäten nicht für allgemeine Serviceverbesserungen durch IBM verwendet werden sollen. Weitere Informationen enthält die [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/apidocs/assistant?curl=#data-collection){: new_window}.

## 7. August 2017
{: #7August2017}

- **Interpretation von Datumsangaben mit `nächste/r/s` und `letzte/r/s`**: Der Service '{{site.data.keyword.conversationshort}}' behandelt Datumsangaben mit `letzte/r/s` und `nächste/r/s` so, als ob sich diese auf den unmittelbar letzten oder nächsten Tag beziehen, der in derselben, aber auch in einer vorherigen Woche liegen kann. Weitere Informationen enthält der Abschnitt [Systementitäten](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-datetime).

## 3. August 2017
{: #3August2017}

- **Unscharfe Suche für zusätzliche Sprachen (Betaversion)**: Die unscharfe Suche für Entitäten ist jetzt für weitere Sprachen verfügbar (siehe [Unterstützte Sprachen](/docs/services/assistant?topic=assistant-language-support)).
- **Suche mit teilweiser Übereinstimmung (Betaversion, nur Englisch)**: Bei der unscharfen Suche werden jetzt automatisch auf Teilzeichenfolgen basierende Synonyme vorgeschlagen, die in benutzerdefinierten Entitäten vorhanden sind; und im Vergleich zur exakten Übereinstimmung für die Entität wird eine geringere Konfidenzbewertung zugeordnet. Details finden Sie unter [Unscharfe Suche](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching).

## 28. Juli 2017
{: #28July2017}

- Wenn Sie für das Tool Vorgaben für bidirektionale Sprachen festlegen, können Sie nun die Richtung der grafischen Benutzerschnittstelle angeben.
- Das Toolfarbschema wurde aktualisiert und ist nun mit anderen Watson-Services und -Produkten konsistent.

## 19. Juli 2017
{: #19July2017}

- Die REST-API von {{site.data.keyword.conversationshort}} unterstützt jetzt den Zugriff auf Dialogmodulknoten. Weitere Informationen enthält die [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/apidocs/assistant?curl=#list-dialog-nodes){: new_window}.

## 14. Juli 2017
{: #14July2017}

- Die Slotfunktionalität von Dialogmodulen wurde erweitert. Beispielsweise wurde eine Eigenschaft *slot_in_focus* hinzugefügt, damit Sie eine Bedingung definieren können, die nur auf einen einzigen Slot angewendet wird. Details können Sie dem Abschnitt [Informationen mit Slots erfassen](/docs/services/assistant?topic=assistant-dialog-slots) entnehmen.

## 12. Juli 2017
{: #12July2017}

- **Unterstützung für Tschechisch**: Die Unterstützung der tschechischen Sprache wurde eingeführt. Weitere Details enthält der Abschnitt [Unterstützte Sprachen](/docs/services/assistant?topic=assistant-language-support).

## 11. Juli 2017
{: #11July2017}

- **In Slack testen**: Mit dem neuen Tool **In Slack testen** können Sie Ihren Arbeitsbereich jetzt ohne großen Aufwand zu Testzwecken als Slack-Bot-Benutzer bereitstellen. Dieses Tool ist nur für die {{site.data.keyword.Bluemix_notm}}-Region 'Vereinigte Staaten (Süden)' verfügbar.
- **Aktualisierungen für Arabisch**: Die Unterstützung der arabischen Sprache wurde erweitert und beinhaltet jetzt die absolute Bewertung pro Absicht sowie die Möglichkeit, Absichten als irrelevant zu markieren (weitere Details enthält der Abschnitt [Unterstützte Sprachen](/docs/services/assistant?topic=assistant-language-support)). Die Lernmodelle des Service '{{site.data.keyword.conversationshort}}' wurden im Rahmen dieser Verbesserung ebenfalls aktualisiert und bei einem erneuten Training des Modells werden alle Änderungen angewendet. Weitere Informationen finden Sie unter [Aktualisierte Modelle](#release-notes-updated-models).

## 23. Juni 2017
{: #23June2017}

- **Aktualisierungen für Koreanisch**: Die Unterstützung der koreanischen Sprache wurde erweitert. Weitere Details enthält der Abschnitt [Unterstützte Sprachen](/docs/services/assistant?topic=assistant-language-support). Die Lernmodelle des Service '{{site.data.keyword.conversationshort}}' wurden im Rahmen dieser Verbesserung ebenfalls aktualisiert und bei einem erneuten Training des Modells werden alle Änderungen angewendet. Weitere Informationen finden Sie unter [Aktualisierte Modelle](#release-notes-updated-models).

## 22. Juni 2017
{: #22June2017}

- **Einführung von Slots**: Die Erfassung mehrerer Einzelinformationen von einem Benutzer in einem einzigen Knoten wird jetzt durch das Hinzufügen von Slots vereinfacht. Zuvor mussten mehrere Dialogmodulknoten erstellt werden, um alle möglichen Kombinationen der Möglichkeiten zu berücksichtigen, die Benutzer bei der Bereitstellung von Informationen haben. Mithilfe von Slots können Sie einen einzigen Knoten konfigurieren, der alle vom Benutzer bereitgestellten Informationen speichert und alle benötigten Details anfordert, die vom Benutzer nicht angegeben wurden. Weitere Details enthält der Abschnitt [Informationen mit Slots erfassen](/docs/services/assistant?topic=assistant-dialog-slots).
- **Vereinfachte Baumstruktur von Dialogmodulen:** Die Baumstruktur von Dialogmodulen wurde überarbeitet, um den Bedienungskomfort zu verbessern. Die Baumstrukturansicht ist jetzt kompakter und Sie können einfacher erkennen, an welcher Stelle Sie sich in der Baumstruktur befinden. Außerdem werden die Links zwischen Knoten jetzt so dargestellt, dass Sie die Beziehungen zwischen den Knoten leichter verstehen können.

## 21. Juni 2017
{: #21June2017}

- **Unterstützung für Arabisch**: Die Unterstützung für Arabisch ist jetzt allgemein verfügbar. Details enthält der Abschnitt [Bidirektionale Sprachen konfigurieren](/docs/services/assistant?topic=assistant-language-support#language-support-configuring-bi-directional).
- **Aktualisierungen für Sprachen**: Die Algorithmen des Service '{{site.data.keyword.conversationshort}}' wurden aktualisiert, um die gesamte Sprachunterstützung zu verbessern. Einzelheiten enthält der Abschnitt [Unterstützte Sprachen](/docs/services/assistant?topic=assistant-language-support).

## 16. Juni 2017
{: #16June2017}

- **Empfehlungen (Betaversion, nur für Premium-Benutzer)**: Die Anzeige 'Verbessern' enthält jetzt auch eine Seite **Empfehlungen**, auf der Verfahren vorgeschlagen werden, mit denen Sie Ihr System verbessern können. Hierzu werden die Dialoge von Benutzern mit Ihrem Chat-Bot analysiert und die aktuellen Trainingsdaten sowie die gegenwärtige Antwortsicherheit Ihres Systems berücksichtigt.

## 14. Juni 2017
{: #14June2017}

- **Unscharfe Suche für zusätzliche Sprachen (Betaversion)**: Die unscharfe Suche für Entitäten ist jetzt für weitere Sprachen verfügbar (siehe [Unterstützte Sprachen](/docs/services/assistant?topic=assistant-language-support)). Durch eine Aktivierung der unscharfen Suche für eine jeweilige Entität können Sie die Fähigkeit des Service verbessern, Benutzereingabeterms mit einer Syntax zu erkennen, die Ähnlichkeit mit der Entität hat, ohne dass eine exakte Übereinstimmung erforderlich ist. Das Feature kann die Benutzereingabe der entsprechenden Entität trotz einer falschen Schreibweise oder leichten syntaktischen Abweichungen zuordnen. Wenn Sie beispielsweise 'Giraffe' als Synonym einer Entität für ein Tier definieren und die Benutzereingabe den Begriff 'Giraffen' oder Girafe' enthält, kann die unscharfe Suche den Begriff korrekt zur Entität für das Tier zuordnen. Details finden Sie unter [Unscharfe Suche](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching).

## 13. Juni 2017
{: #13June2017}

- **Benutzerdialoge**: Die Anzeige 'Verbessern' enthält jetzt eine Seite **Benutzerdialoge** mit einer Liste der Benutzerinteraktionen mit Ihrem Chat-Bot, die nach Schlüsselwort, Absicht, Entität oder Anzahl von Tagen gefiltert werden kann. Sie können einzelne Dialoge öffnen, um Absichten zu korrigieren oder um Entitätswerte bzw. Synonyme hinzuzufügen.
- **Änderung für reguläre Ausdrücke**: Die regulären Ausdrücke, die von SpEL-Funktionen unterstützt werden (z. B. find, matches, extract, replaceFirst, replaceAll und split) wurden geändert. Gruppen von Konstrukten mit regulären Ausdrücken sind nicht mehr zulässig, hierzu zählen Konstrukte mit 'look-ahead', 'look-behind', possessiver Wiederholung und Rückverweisen. Diese Änderung war erforderlich, um ein Sicherheitsrisiko in der ursprünglichen Bibliothek für reguläre Ausdrücke zu verhindern.

## 12. Juni 2017
{: #12June2017}

- Die maximale Anzahl von Arbeitsbereichen, die Sie mit dem Plantyp **Lite** (zuvor 'Free' genannt) erstellen können, wurde von 3 auf 5 erhöht.
- Sie können künftig einem Dialogmodulknoten einen beliebigen Namen zuordnen, der nicht eindeutig sein muss. Außerdem kann der Knotenname nachfolgend geändert werden, ohne dass hierdurch die interne Referenzierung des Knotens beeinflusst wird. Der von Ihnen angegebene Name wird als Aliasname behandelt; das System verwendet eine eigene interne ID, um den Knoten zu referenzieren.
- Die Sprache eines Arbeitsbereichs kann künftig nicht mehr nach seiner Erstellung durch eine Bearbeitung der Arbeitsbereichsdetails geändert werden. Falls Sie die Sprache ändern müssen, können Sie den Arbeitsbereich als JSON-Datei exportieren, die Eigenschaft 'language' aktualisieren und dann die JSON-Datei als neuen Arbeitsbereich importieren.

## 6. Juni 2017
{: #6June2017}

- **Weitere Informationen**: Es gibt eine neue Seite mit *Informationen zu {{site.data.keyword.conversationfull}}*, die einführende Informationen und Links zur Servicedokumentation sowie zu anderen nützlichen Ressourcen bietet. Zum Öffnen der Seite klicken Sie auf das Symbol ![i für Informationen](images/info.png) in der Seitenkopfzeile.
- **Massenexport und -löschung**: Sie können jetzt gleichzeitig eine Reihe von Absichten oder Entitäten in eine CSV-Datei exportieren, um sie anschließend zu importieren und für eine andere {{site.data.keyword.conversationshort}}-Anwendung wiederzuverwenden. Sie können ebenfalls gleichzeitig eine Reihe von Entitäten oder Absichten für eine Massenlöschung auswählen.
- **Aktualisierungen für Koreanisch**: Die Tokenizer für Koreanisch wurden aktualisiert, um die Unterstützung von inoffizieller Sprache abzudecken. IBM arbeitet fortlaufend an Verbesserungen bei der Erkennung und Klassifizierung von Entitäten.
- **Emoji-Unterstützung**: Emojis, die zu Beispielen für Absichten oder als Entitätswerte hinzugefügt wurden, werden jetzt korrekt klassifiziert bzw. extrahiert.

  Nur Emojis, die in Ihren Trainingsdaten enthalten sind, werden korrekt und konsistent erkannt; Emojis mit abweichenden Farbtönen oder anderen Variationen werden von der Emoji-Unterstützung möglicherweise nicht korrekt klassifiziert.
  {: note}

- **Normalformenreduktion für Entitäten (Betaversion, nur Englisch)**: Die Betaversion für Features für die unscharfe Suche legt die Stammform des Entitätswerts zu Grunde, um Entitäten zu erkennen und abzugleichen. Dieses Feature erkennt beispielsweise, dass 'bananas' Ähnlichkeit mit 'banana' hat bzw. 'run' mit 'running', da diese Wörter jeweils eine gemeinsame Stammform besitzen. Weitere Informationen finden Sie unter [Unscharfe Suche](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching).
- **Verarbeitungsfortschritt beim Arbeitsbereichsimport**: Wenn Sie einen Arbeitsbereich aus einer JSON-Datei importieren, wird sofort eine Kachel für den Arbeitsbereich angezeigt, auf der Sie Informationen zum Verarbeitungsfortschritt des Imports erhalten.
- **Verringerte Trainingsdauer**: Mehrere Modelle werden jetzt parallel trainiert, was die Trainingsdauer für umfangreiche Arbeitsbereiche deutlich reduziert.

## 26. Mai 2017
{: #26May2017}

- Die aktuelle API-Version ist jetzt `2017-05-26`. Mit dieser Version werden die folgenden Änderungen eingeführt:
    - Das Schema von Objekten des Typs 'ErrorResponse' wurde geändert. Diese Änderung betrifft alle Endpunkte und Methoden. Weitere Informationen enthält die [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/assistant){: new_window}.
    - Das interne Schema für die Darstellung von Dialogmodulknoten in exportierten JSON-Dateien für Arbeitsbereiche wurde geändert. Falls Sie die API-Version `2017-05-26` verwenden, um einen Arbeitsbereich zu importieren, der mit einer früheren Version exportiert wurde, werden einige Knoten möglicherweise nicht korrekt importiert. Ein Arbeitsbereich sollte daher nach Möglichkeit immer mit derselben Version importiert werden, die auch für seinen Export verwendet wurde.

## 25. Mai 2017
{: #25May2017}

- Kontextvariablen können nun im Fenster 'Ausprobieren' verwaltet werden. Klicken Sie auf den Link **Kontext verwalten**, um ein neues Fenster zu öffnen, in dem Sie die Werte von Kontextvariablen direkt beim Testen des Dialogmoduls festlegen und überprüfen können. Weitere Informationen enthält der Abschnitt [Dialogmodul testen](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-test).

## 16. Mai 2017
{: #16May2017}

- Beim Öffnen des Tools ist jetzt ein Beispielarbeitsbereich **Car Dashboard** verfügbar. Sie können das Beispiel als Ausgangspunkt für Ihren eigenen Arbeitsbereich verwenden, indem Sie den Arbeitsbereich bearbeiten. Falls Sie das Beispiel für mehrere Arbeitsbereiche verwenden wollen, kopieren Sie es stattdessen. Der Beispielarbeitsbereich wird nur dann bei der Zählung für die Begrenzung der Arbeitsbereiche berücksichtigt, wenn Sie ihn verwenden.
- Die Navigation im Tool ist jetzt einfacher. Die Navigationsmenüoptionen sind jetzt am seitlichen und nicht mehr am oberen Rand der Hauptseite verfügbar. Am Anfang der Seite machen Breadcrumb-Links kenntlich, an welcher Stelle Sie sich befinden. Auf der Seite 'Arbeitsbereiche' können Sie jetzt zwischen Serviceinstanzen wechseln. Wenn Sie im Navigationsmenü auf **Zurück zu Arbeitsbereichen** klicken, können Sie diese Seite direkt aufrufen. Falls mehrere Serviceinstanzen vorhanden sind, wird der Name der aktuellen Instanz angezeigt. Sie können auf den Link **Ändern** neben der Instanz klicken, um eine andere Instanz auszuwählen.
- Wenn Sie ein Dialogmodul erstellen, werden zwei Knoten jetzt automatisch für Sie hinzugefügt: 1. Knoten **Welcome** am Anfang der Baumstruktur des Dialogmoduls, der die Begrüßung enthält, die für den Benutzer angezeigt werden soll. 2. Knoten **Anything else** am Ende der Baumstruktur für das Dialogmodul, der alle Benutzeranfragen erfasst, die von keinem anderen Knoten im Dialogmodul abgefangen werden, und diese Anfragen beantwortet. Weitere Informationen enthält der Abschnitt [Dialogmodul erstellen](/docs/services/assistant?topic=assistant-dialog-build).
- Wenn Sie ein Dialogmodul im Fenster 'Ausprobieren' testen, können Sie nun eine kürzlich verwendete verbale Testäußerung suchen und erneut übergeben, indem Sie die Aufwärtstaste drücken und die vorherigen Eingaben durchblättern.
- Für fünf Systementitäten (`@sys-date`, `@sys-time`, `@sys-currency`, `@sys-number`, `@sys-percentage`) ist jetzt die experimentelle Sprachunterstützung für Koreanisch verfügbar. Für einige der numerischen Entitäten liegen bekannte Probleme vor und die Eingabe inoffizieller Sprache wird nur begrenzt unterstützt.
- Auf der Registerkarte 'Verbessern' gibt es jetzt eine Seite 'Übersicht'. Die Seite bietet eine Zusammenfassung der Interaktionen mit Ihrem Bot. Sie können den Umfang des Datenverkehrs in einem bestimmten Zeitraum ebenso anzeigen wie die Absichten und Entitäten, die am häufigsten in Benutzerdialogen erkannt wurden. Weitere Informationen finden Sie unter [Seite 'Übersicht'](/docs/services/assistant?topic=assistant-logs-overview).

## 27. April 2017
{: #27April2017}

- Die folgenden Systementitäten sind jetzt als Betaversionsfeatures ausschließlich in Englisch verfügbar:
    - sys-location: Erkennt die Referenzierung von Standorten wie Gemeinden, Städten und Ländern in verbalen Äußerungen der Benutzer.
    - sys-person: Erkennt die Referenzierung von Vor- und Nachnamen für Personen in verbalen Äußerungen der Benutzer.

    Weitere Angaben enthalten die [Referenzinformationen zu Systementitäten](/docs/services/assistant?topic=assistant-system-entities).
- Die unscharfe Suche nach Entitäten ist ein Betaversionsfeature, das jetzt in Englisch verfügbar ist. Durch eine Aktivierung der unscharfen Suche für eine jeweilige Entität können Sie die Fähigkeit des Service verbessern, Benutzereingabeterms mit einer Syntax zu erkennen, die Ähnlichkeit mit der Entität hat, ohne dass eine exakte Übereinstimmung erforderlich ist. Das Feature kann die Benutzereingabe der entsprechenden Entität trotz einer falschen Schreibweise oder leichten syntaktischen Abweichungen zuordnen. Wenn Sie beispielsweise **giraffe** als Synonym einer Entität für ein Tier definieren und die Benutzereingabe den Begriff *giraffes* oder *girafe* enthält, kann die unscharfe Suche den Begriff korrekt zur Entität für das Tier zuordnen. Weitere Informationen enthält der Abschnitt [Entitäten definieren](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching). Suchen Sie dort nach `unscharfe Suche`, um Details zu erfahren.

## 18. April 2017
{: #18April2017}

- Die REST-API von {{site.data.keyword.conversationshort}} unterstützt jetzt die folgenden Ressourcen:
    - Entitäten
    - Entitätswerte
    - Synonyme für Entitätswerte
    - Protokolle

        Weitere Informationen enthält die [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/assistant){: new_window}.- Beim Verhalten der Methode `POST` für '/messages' wurde die Behandlung von Entitäten und Absichten geändert, die als Teil der Nachrichteneingabe angegeben sind:
    - Falls Sie Absichten in der Eingabe angeben, verwendet der Service die angegebenen Absichten, jedoch die Verarbeitung natürlicher Sprache, um Entitäten in der Benutzereingabe zu erkennen.
    - Falls Sie Entitäten in der Eingabe angeben, verwendet der Service die angegebenen Entitäten, jedoch die Verarbeitung natürlicher Sprache, um Absichten in der Benutzereingabe zu erkennen.

        Für Nachrichten, die sowohl Absichten als auch Entitäten angeben oder die weder Absichten noch Entitäten angeben, wurde das Verhalten nicht geändert.
- Die Option, eine Benutzereingabe als irrelevant zu markieren, ist jetzt für alle unterstützten Sprachen verfügbar. Dieses Feature liegt als Betaversion vor.
- Die neue Registerkarte 'Berechtigungsnachweise' bietet eine zentrale Stelle für alle Informationen, die Sie benötigen, um Ihre Anwendung mit einem Arbeitsbereich zu verbinden (z. B. die Serviceberechtigungsnachweise und die Arbeitsbereichs-ID), sowie weitere Optionen für die Bereitstellung. Klicken Sie zum Zugreifen auf die Registerkarte 'Berechtigungsnachweise' für Ihren Arbeitsbereich auf das Symbol ![Menü](images/Menu_16.png) und wählen Sie **Berechtigungsnachweise** aus.

## 9. März 2017
{: #9March2017}

Die REST-API von {{site.data.keyword.conversationshort}} unterstützt jetzt die folgenden Ressourcen:

- Arbeitsbereiche
- Absichten
- Beispiele
- Gegenbeispiele

Weitere Informationen enthält die [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/assistant){: new_window}.

## 7. März 2017
{: #7March2017}

- Die Verwendung von `.` oder `..` als Name einer Absicht verursacht Probleme und wird nicht mehr unterstützt.

    Eine Absicht mit diesem Namen kann weder umbenannt noch gelöscht werden; zur Änderung des Namens müssen Sie Ihre Absichten in eine Datei importieren, die Absicht in der Datei umbenennen und die aktualisierte Datei in Ihren Arbeitsbereich importieren.

    Zahlungspflichtige Kunden können Unterstützung für eine Datenbankänderung anfordern.

## 1. März 2017
{: #1March2017}

- Systementitäten sind jetzt in Deutsch verfügbar.

## 22. Februar 2017
{: #22February2017}

- Nachrichten sind künftig auf 2048 Zeichen begrenzt.

## 3. Februar 2017
{: #3February2017}

- Die Bewertung von Absichten wurde geändert und die Möglichkeit eingeführt, Eingaben als irrelevant für die Anwendung zu markieren. Details enthält der Abschnitt [Absichten definieren](/docs/services/assistant?topic=assistant-intents). Suchen Sie dort nach `Als irrelevant markieren`.

- In diesem Release wurde eine bedeutende Änderung für den Arbeitsbereich eingeführt. Um von dieser Änderung zu profitieren, müssen Sie ein manuelles Upgrade für Ihren Arbeitsbereich durchführen.

- Die Verarbeitung von Aktionen **Springen zu** wurde geändert, um Schleifen zu verhindern, die unter bestimmten Bedingungen auftreten können. Wenn Sie zuvor zur Bedingung eines Knotens gesprungen sind und weder dieser Knoten noch einer seiner Peerknoten eine Bedingung enthielten, die mit 'true' ausgewertet wurde, führte das System die Aktion 'Springen zu' zum Stammknoten aus und suchte nach einem Knoten, dessen Bedingung mit der Eingabe übereinstimmte. In einigen Situationen entstand durch diese Verarbeitung eine Schleife, die eine Fortsetzung des Dialogmoduls verhinderte.

  Wenn beim neuen Prozess weder der Zielknoten noch einer seiner Peerknoten mit 'true' ausgewertet werden, wird die Dialogmodulrunde beendet. Falls Sie das alte Verarbeitungsmodell wiederherstellen möchten, fügen Sie einen finalen Peerknoten mit einer Bedingung `true` hinzu. Verwenden Sie in der Antwort eine Aktion **Springen zu**, deren Ziel die Bedingung des ersten Knotens auf der Stammebene der Baumstruktur Ihres Dialogmoduls ist.

## 11. Januar 2017
{: #11January2017}

- In diesem Release können Sie jetzt Knotentitel im Dialogmodul anpassen.

## 22. Dezember 2016
{: #22December2016}

- In diesem Release wird in Dialogmodulknoten nun ein neuer Abschnitt für den `Knotentitel` angezeigt. Die Anpassung eines `Knotentitels` ist nicht möglich. Bei einem komprimierten Knoten wird als `Knotentitel` die `Knotenbedingung` des Dialogmodulknotens angezeigt. Falls es keine `Knotenbedingung` gibt, wird 'Nicht benannter Knoten' als Titel angezeigt.

## 19. Dezember 2016
{: #19December2016}

Die Verwendung des Dialogmoduls ist jetzt durch verschiedene Änderungen einfacher und intuitiver: 

- Durch eine größere Bearbeitungsansicht können Sie leichter alle Details eines Knotens einsehen, wenn Sie ihn bearbeiten.
- Ein Knoten kann mehrere Antworten enthalten, die jeweils durch eine eigene Bedingung ausgelöst werden. Weitere Informationen finden Sie unter [Mehrere Antworten](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses).

## 5. Dezember 2016
{: #5December2016}

- Neue Sprachen werden unterstützt (sämtlich im Modus 'Experimentell'): Deutsch, traditionelles Chinesisch, vereinfachtes Chinesisch und Niederländisch.
- Mit '@sys-date' und '@sys-time' sind zwei neue Systementitäten verfügbar. Details finden Sie unter [Systementitäten](/docs/services/assistant?topic=assistant-system-entities).

## 21. Oktober 2016
{: #21October2016}

- Der Service '{{site.data.keyword.conversationshort}}' bietet jetzt mit Systementitäten allgemeine Entitäten, die Sie in beliebigen Anwendungsfällen verwenden können. Details enthält der Abschnitt [Entitäten definieren](/docs/services/assistant?topic=assistant-entities). Suchen Sie dort nach `Systementitäten aktivieren`.
- Sie können jetzt auf der Seite 'Verbessern' ein Protokoll der Dialoge mit Benutzern anzeigen. Anhand dieses Protokolls können Sie das Verhalten des Bots nachvollziehen. Details enthält der Abschnitt [Skill verbessern](/docs/services/assistant?topic=assistant-logs-intro).
- Sie können jetzt Entitäten aus einer CSV-Datei importieren, was bei einer großen Anzahl von Entitäten hilfreich ist. Details enthält der Abschnitt [Entitäten definieren](/docs/services/assistant?topic=assistant-entities). Suchen Sie dort nach `Entitäten importieren`.

## 20. September 2016
{: #20September2016}

**Neue Version:** 2016-09-20

Um die Änderungen in einer neuen Version nutzen zu können, ändern Sie den Wert des Parameters `version` in das neue Datum. Falls Sie nicht für eine Aktualisierung auf diese Version bereit sind, ändern Sie das Versionsdatum nicht.

- Version **2016-09-20**: `dialog_stack` wurde aus einem Array von Zeichenfolgen in ein Array von Objekten geändert.

## 29. August 2016
{: #29August2016}

- Sie können Dialogmodulknoten von einer Verzweigung zu einer anderen Verzweigung als gleichgeordnete Elemente oder Peerknoten verschieben. Details enthält der Abschnitt [Dialogmodulknoten verschieben](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-move-node).
- Sie können das Fenster mit dem JSON-Editor erweitern.
- Sie können Chatprotokolle für die Dialoge Ihres Bots anzeigen, um sein Verhalten besser nachzuvollziehen. Sie können nach Absichten, Entitäten, Datum und Uhrzeit filtern. Details enthält der Abschnitt [Skill verbessern](/docs/services/assistant?topic=assistant-logs-intro).

## 11. Juli 2016
{: #21July2016}

- Dieses allgemein verfügbare Release ermöglicht Ihnen die Arbeit mit Entitäten und Dialogmodulen und somit die Erstellung eines voll funktionsfähigen Bots.

## 18. Mai 2016
{: #18May2016}

- Dieses experimentelle Release von {{site.data.keyword.conversationshort}} führt die Benutzerschnittstelle ein und ermöglicht Ihnen die Arbeit mit Arbeitsbereichen, Absichten und Beispielen.
