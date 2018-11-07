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

# Mit dem {{site.data.keyword.conversationshort}}-Connector in einem Kanal bereitstellen

Nachdem Sie Ihren Arbeitsbereich eingerichtet haben, können Sie ihn mit dem {{site.data.keyword.conversationshort}}-Connector schnell und einfach mit einem Kommunikationskanal wie Slack oder Facebook Messenger verbinden. Der {{site.data.keyword.conversationshort}}-Connector besteht aus einer Reihe von {{site.data.keyword.openwhisk}}-Komponenten, die die Kommunikation zwischen Ihrem {{site.data.keyword.conversationshort}}-Arbeitsbereich und einer Slack- oder Facebook-App ermöglichen, deren Eigner Sie sind.

Durch das Verbinden mit einer Kanal-App können Sie Ihren Arbeitsbereich als Chat-Bot für die Interaktion mit Benutzern von Slack oder Facebook Messenger verfügbar machen. Die Antworten von Ihrem Dialogmodul können Multimedia-Elemente und interaktive Antworten wie Bilder und per Mausklick steuerbare Schaltflächen enthalten. (Weitere Informationen zum Definieren einer Antwort mit Multimedia-Elementen finden Sie unter [Multimedia-Antworten](dialog-multimedia.html).)

![{{site.data.keyword.openwhisk_short}}-Bereitstellung - Übersichtsdiagramm](images/deploytochannel_diagram.png)

Für den {{site.data.keyword.conversationshort}}-Connector gelten einige Einschränkungen:

- Sie müssen eine Slack- oder Facebook-App erstellen oder über die Administratorberechtigung zum Ändern der zu verwendenden App verfügen.
- Aufgrund von Einschränkungen für {{site.data.keyword.openwhisk_short}} ist dieses Tool gegenwärtig nur für die {{site.data.keyword.Bluemix_notm}}-Region 'Vereinigte Staaten (Süden)' verfügbar.

## Bereitstellung in Slack mit dem {{site.data.keyword.conversationshort}}-Tool

Das {{site.data.keyword.conversationshort}}-Tool bietet eine schnelle und einfache Methode zum Bereitstellen eines Bots mit dem {{site.data.keyword.conversationshort}}-Connector. Das Tool führt Sie schrittweise durch den Prozess zum Sammeln der erforderlichen Konfigurationsinformationen für Ihre Kanal-App und die Connector-Komponenten. Anschließend werden die erforderlichen Komponenten von dem Tool automatisch in IBM Cloud bereitgestellt.

**Hinweis:** Die Schnittstelle des {{site.data.keyword.conversationshort}}-Tools unterstützt derzeit nur Slack. Für die Bereitstellung in Facebook Messenger können Sie jedoch einen automatisierten {{site.data.keyword.Bluemix_short}}-Prozess verwenden. Weitere Informationen zur Bereitstellung in Facebook Messenger enthält die Dokumentation für {{site.data.keyword.conversationshort}}-Connector im [GitHub-Repository ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/watson-developer-cloud/conversation-connector/blob/master/channels/facebook/README.md){: new_window}.

So stellen Sie einen Bot mit dem automatisierten Bereitstellungstool bereit:

1. Öffnen Sie den Arbeitsbereich, den Sie bereitstellen möchten, im {{site.data.keyword.conversationshort}}-Tool.
1. Klicken Sie auf das Menüsymbol in der linken oberen Ecke und wählen Sie die Option **Bereitstellen** aus.

   ![Menüoption für schnelle Bereitstellung](images/deploy_menu_testdeploy.png)

1. Klicken Sie unter **Mit {{site.data.keyword.openwhisk_short}}** bereitstellen auf **In Slack bereitstellen**.
1. Klicken Sie auf der Seite 'Slack' auf **Als Slack-App bereitstellen** und befolgen Sie die Anweisungen.

   ![Schaltfläche 'Als Slack-App bereitstellen'](images/deploy_deploytoslack.png)

## Manuell bereitstellen

Alternativ zum Bereitstellen Ihres Arbeitsbereichs als Bot mit dem automatisierten Tool können Sie die erforderlichen Komponenten auch manuell konfigurieren und in IBM Cloud bereitstellen. Dies kann beispielsweise in den folgenden Situationen sinnvoll sein:

- **Teilweise erneute Bereitstellung**. Angenommen, Sie möchten einige Komponenten einer bestehenden Bereitstellung erneut bereitstellen, um die Konfiguration zu ändern, Probleme zu beheben oder eine Programmkorrektur aus einer neuen Version anzuwenden.
- **Bot durch neue Funktionen erweitern**. Sie können die bereitgestellten Komponenten ändern, um neue Funktionen hinzuzufügen (z. B. Vorverarbeitung der Benutzereingaben vor dem Senden zum {{site.data.keyword.conversationshort}}-Arbeitsbereich).
- **Neuen Kanal bereitstellen**. Wenn Sie einen Bot über einen anderen Kommunikationskanal als Slack oder Facebook Messenger bereitstellen möchten, können Sie die vorhandenen Komponenten als Vorlage verwenden, um eigene Komponenten zu entwickeln.

Die Komponenten des {{site.data.keyword.conversationshort}}-Connectors werden in einem öffentlichen GitHub-Repository gehostet, das Sie herunterladen oder klonen können. Weitere Informationen dazu finden Sie in der Dokumentation für das [-Repository ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/watson-developer-cloud/conversation-connector){: new_window}.

## Mit dem Bot chatten

Nachdem Sie den Bereitstellungsprozess abgeschlossen haben, können Sie unter dem Bot-Benutzernamen mit Ihrem {{site.data.keyword.conversationshort}}-Arbeitsbereich interagieren wie mit jedem anderen Slack- oder Facebook Messenger-Bot.

Dabei ist zu beachten, dass der {{site.data.keyword.conversationshort}}-Connector den Status für jeden Benutzer, der mit dem Bot interagiert, separat verwaltet. (Am Beispiel von Slack bedeutet dies, dass ein einzelner Benutzer mehrere separate Dialoge mit dem Bot führen kann - einen über Direktnachrichten und einen über jeden Kanal). Mit anderen Worten: Alle Variablen, die Sie im Dialogkontext speichern, bleiben unbegrenzt erhalten. bis Sie von Ihnen gelöscht werden.

Falls Sie in der Lage sein müssen, den Dialog auf einen bekannten Anfangsstatus zurückzusetzen, können Sie innerhalb des Dialogmoduls dafür sorgen. Achten Sie darauf, dass Ihr Dialogmodul einen Knoten enthält, der am Ende des Dialogs bzw. zu einem anderen Zeitpunkt ausgeführt wird, an dem Sie von vorne beginnen wollen. Aktualisieren Sie die JSON-Daten für diesen Knoten so, dass alle Kontextvariablen auf die entsprechenden Anfangswerte zurückgesetzt werden. (Bei Bedarf können Sie diesen Knoten mit Aktionen **Springen zu** oder einer speziellen Absicht 'dialog_beenden' ausführen.)

Beispiel: Ihr Arbeitsbereich verwendet eine Kontextvariable namens `drink_order`, um die Getränkeauswahl eines Benutzers zu speichern. Mit der Methode `context.remove` können Sie diese Variable löschen, wenn der Dialog beendet wird:

```json
"context": {
   "reset_drink_order": "<?context.remove('drink_order')?>"
 }
```
{: codeblock}

Weitere Informationen zum Ändern von Werten für Kontextvariablen finden Sie unter [Wert einer Kontextvariablen aktualisieren](dialog-runtime.html#context-update).

Um den Kontext zu löschen oder andere Änderungen am Verhalten des Bots vorzunehmen, können Sie die bereitgestellten {{site.data.keyword.openwhisk_short}}-Aktionen bearbeiten. Weitere Informationen enthält die bereitgestellte Dokumentation im [GitHub-Repository ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/watson-developer-cloud/conversation-connector){: new_window}.
