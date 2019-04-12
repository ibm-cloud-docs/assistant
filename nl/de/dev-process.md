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

# Entwicklungsprozess
{: #dev-process}

Verwenden Sie das {{site.data.keyword.conversationshort}}-Tool, um beim Entwickeln, Bereitstellen und Optimieren eines Dialogmodulassistenten von den Vorteilen der künstlichen Intelligenz zu profitieren.
{: shortdesc}

![Zeigt den Ablauf der Entwicklungsschritte vom Erstellen der Trainingsdaten bis zum Bereitstellen in der Produktionsumgebung](images/dev-process.png)

## Workflow
{: #dev-process-workflow}

Der typische Workflow für ein Projekt zum Erstellen eines Assistenten umfasst die folgenden Schritte:

1.  Definieren Sie eine kleine Gruppe grundlegender Kundenanforderungen, die der Assistent für Sie abdecken soll, sowie Geschäftsprozesse, die der Assistent für Ihre Kunden einleiten oder abschließen kann. 
1.  Erstellen Sie Absichten, die die Kundenanforderungen abdecken, die Sie im vorherigen Schritt angegeben haben. Beispiele für solche Absichten sind `About_company` (Informationen zum Unternehmen) oder `#Place_order` (Bestellung aufgeben).

    Verwenden Sie die Funktion zum Vorschlagen von Beispielen für Absichten, um Ihre vorhandenen Call-Center-Protokolle nach geeigneten Benutzerbeispielen für Absichten zu durchsuchen.{: tip}

1.  Erstellen Sie ein Dialogmodul, das die definierten Absichten erkennt und adressiert, entweder durch einfache Antworten oder durch einen Dialogablauf, in dem zunächst weitere Informationen gesammelt werden.
1.  Definieren Sie die erforderlichen Entitäten, um die Zielsetzung des Benutzers deutlicher zu erkennen.

    Durchsuchen Sie vorhandene Beispiele für Benutzerabsichten nach Erwähnungen gemeinsamer Entitätswerte. Durch Anmerkungen zum Definieren von Entitäten kann nicht nur der Text des Entitätswerts erfasst werden, sondern auch der typische Kontext, in dem der Entitätswert in einem Satz verwendet wird.

    Bei Entitäten, die auf Wörterverzeichnissen basieren, können Sie Ihre Entitätsdefinitionen durch Synonymempfehlungen erweitern.
    {: tip}

1.  Testen Sie alle Funktionen, die Sie zum Assistenten hinzufügen, nacheinander im Fensterbereich 'Ausprobieren'.
1.  Wenn ein funktionsfähiger Assistent vorliegt, der grundlegende Tasks erfolgreich ausführen kann, fügen Sie eine Integration hinzu, die den Assistenten in einer Entwicklungsumgebung bereitstellt. Testen Sie den bereitgestellten Assistenten und optimieren Sie ihn.

1.  Nachdem Sie einen effektiven Assistenten erstellt haben, erstellen Sie einen Snapshot des Dialogskills und speichern Sie diesen als Version. 

    Auf eine Version, die bei Erreichen eines Meilensteins im Entwicklungsprozess gespeichert wurde, können Sie später zurückgreifen, falls sich nachfolgende Änderungen, die Sie an dem Skill vornehmen, nachteilig auf die Effektivität auswirken. Weitere Informationen enthält der Abschnitt [Skillversionen erstellen](/docs/services/assistant?topic=assistant-versions).
1.  Stellen Sie die Version des Assistenten in einer Testumgebung bereit und testen Sie sie.

    Wenn Sie das webbasierte Chat-Widget verwenden, können Sie die URL mit anderen Benutzern teilen, um Hilfe beim Testen zu erhalten.
1.  Verwenden Sie Metriken aus der Registerkarte 'Analyse', um festzustellen, welche Bereiche verbessert werden können, und entsprechende Anpassungen vorzunehmen.

    Wenn Sie alternative Verfahren zur Behebung eines Problems testen möchten, erstellen Sie für jede Lösung eine Version, damit die einzelnen Lösungen unabhängig voneinander bereitgestellt und getestet werden können.
1.  Wenn die Leistung des Assistenten Ihren Erwartungen entspricht, stellen Sie die beste Version des Assistenten in einer Produktionsumgebung bereit. 
1.  Überwachen Sie die Protokolle der Dialoge zwischen Benutzern und dem bereitgestellten Assistenten. 

    Die Protokolle für eine Skillversion, die in der Produktionsumgebung ausgeführt wird, können Sie in der Registerkarte 'Analyse' einer Entwicklungsversion des Skills aufrufen. Fehlklassifizierungen und andere Fehler können Sie in der Entwicklungsversion des Skills korrigieren und anschließend die verbesserte Version in der Produktionsumgebung bereitstellen. Weitere Details enthält der Abschnitt [Verbesserungen für mehrere Assistenten](/docs/services/assistant?topic=assistant-logs#logs-deploy-id). 

Das Analysieren der Protokolle und das Verbessern der Dialogskills ist ein fortlaufender Prozess. Im Laufe der Zeit können Sie die Tasks erweitern, die der Assistent für Sie übernehmen kann. Auch die Kundenanforderungen können sich ändern. Wenn neue Anforderungen entstehen, können sie mithilfe der von Ihren bereitgestellten Assistenten erfassten Metriken identifiziert und in nachfolgenden Entwicklungsschritten adressiert werden.
