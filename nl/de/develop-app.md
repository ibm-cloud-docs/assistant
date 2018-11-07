---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-29"

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
{:tip: .tip}

# Clientanwendung erstellen

Sie haben bereits ein funktionsfähiges Dialogmodul erstellt. Jetzt soll die Anwendung entwickelt werden, die mit Ihren Benutzern interagiert und mit dem Service '{{site.data.keyword.conversationfull}}' kommuniziert.
{: shortdesc}

Sie können dieses Lernprogramm entweder für Node.js (Javascript) oder für Python anzeigen, indem Sie auf die Sprachauswahl in der rechten oberen Ecke klicken. Details zu allen unterstützten Sprachen finden Sie unter {{site.data.keyword.watson}} [SDKs ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](/docs/services/watson/getting-started-sdks.html#sdks){: new_window}.
{: tip }

## Service '{{site.data.keyword.conversationshort}}' einrichten

Die Beispielanwendung, die in diesem Abschnitt erstellt wird, implementiert verschiedene Funktionen eines kognitiven persönlichen Assistenten. Der Anwendungscode stellt eine Verbindung zu einem {{site.data.keyword.conversationshort}}-Arbeitsbereich her, in dem die kognitive Verarbeitung (z. B. die Erkennung der Benutzerabsichten) stattfindet.

Bevor Sie mit diesem Beispiel fortfahren, müssen Sie den erforderlichen {{site.data.keyword.conversationshort}}-Arbeitsbereich einrichten:

1.  Laden Sie die <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/conversation/conversation-simple-example.json" download="conversation-simple-example.json">JSON-Datei</a> für den Arbeitsbereich herunter.
1.  [Importieren Sie den Arbeitsbereich](/docs/services/conversation/configure-workspace.html#creating-workspaces) in eine Instanz des Service '{{site.data.keyword.conversationshort}}'.

## Serviceinformationen abrufen

Damit Ihre Anwendung auf die REST-APIs des Service '{{site.data.keyword.conversationshort}}' zugreifen kann, muss sie sich bei {{site.data.keyword.Bluemix}} authentifizieren und eine Verbindung zum richtigen {{site.data.keyword.conversationshort}}-Arbeitsbereich herstellen können. Sie müssen die Berechtigungsnachweise für den Service und die Arbeitsbereichs-ID kopieren und sie in Ihren Anwendungscode einfügen.

Um auf die Berechtigungsnachweise für den Service und die Arbeitsbereichs-ID im Arbeitsbereich zuzugreifen, wählen Sie das Menü ![Menü](images/Menu_16.png) aus, wählen Sie **Bereitstellen** aus und wechseln Sie dann auf die Registerkarte **Berechtigungsnachweise**.

Auf die Berechtigungsnachweise für den Service können Sie auch über das {{site.data.keyword.Bluemix_short}}-Dashboard zugreifen.

## Mit dem Service '{{site.data.keyword.conversationshort}}' kommunizieren

Die Interaktion mit dem Service '{{site.data.keyword.conversationshort}}' ist ganz leicht. Das folgende Beispiel stellt eine Verbindung zum Service her, sendet eine einfache Nachricht und protokolliert die Ausgabe in der Konsole:

```javascript
// Beispiel 1: Service-Wrapper einrichten, Startnachricht senden und
// Antwort empfangen

var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Service-Wrapper für Conversation einrichten
var conversation = new ConversationV1({
  username: 'USERNAME', // Durch den Servicebenutzernamen ersetzen
  password: 'PASSWORD', // Durch das Servicekennwort ersetzen
  version_date: '2017-05-26'
});

var workspace_id = 'WORKSPACE_ID'; // Durch Arbeitsbereichs-ID ersetzen

// Dialog mit leerer Nachricht starten
conversation.message({
  workspace_id: workspace_id
  }, processResponse);

// Dialogantwort verarbeiten
function processResponse(err, response) {
  if (err) {
    console.error(err); // Bei Auftreten eines Fehlers
    return;
  }

  // Ausgabe des Dialogs anzeigen, sofern vorhanden
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }
}
```
{: codeblock}
{: javascript}

```python
# Beispiel 1: Service-Wrapper einrichten, Startnachricht senden und
# Antwort empfangen

import watson_developer_cloud

# Service 'Conversation' einrichten
conversation = watson_developer_cloud.ConversationV1(
  username = 'USERNAME', # Durch Benutzernamen aus Serviceschlüssel ersetzen
  password = 'PASSWORD', # Durch Kennwort aus Serviceschlüssel ersetzen
  version = '2017-05-26'
)
workspace_id = 'WORKSPACE_ID' # Durch Arbeitsbereichs-ID ersetzen

# Dialog mit leerer Nachricht starten
response = conversation.message(
  workspace_id = workspace_id,
  input = {
    'text': ''
  }
)

# Ausgabe des Dialogs anzeigen, sofern vorhanden
if response['output']['text']:
  print(response['output']['text'][0])
```
{: codeblock}
{: python}

Im ersten Schritt wird ein Wrapper für den Service '{{site.data.keyword.conversationshort}}' erstellt.

Der Wrapper ist ein Objekt, das Sie verwenden, um Eingabe an den Service zu senden und Ausgabe vom Service zu empfangen. Beim Erstellen des Service-Wrappers geben Sie die Authentifizierungsnachweise aus dem Serviceschlüssel sowie die Version der verwendeten {{site.data.keyword.conversationshort}}-API an.

In diesem Node.js-Beispiel ist der Wrapper eine Instanz von `ConversationV1`, gespeichert in der Variablen `conversation`. Die Watson-SDKs für andere Sprachen bieten funktional entsprechende Mechanismen für die Instanziierung eines Service-Wrappers.
{: javascript}

In diesem Python-Beispiel ist der Wrapper eine Instanz von `watson_developer_cloud.ConversationV1`, gespeichert in der Variablen `conversation`. Die Watson-SDKs für andere Sprachen bieten funktional entsprechende Mechanismen für die Instanziierung eines Service-Wrappers.
{: python}

Nachdem Sie den Service-Wrapper erstellt haben, verwenden Sie ihn, um eine Nachricht an den Service '{{site.data.keyword.conversationshort}}' zu senden. In diesem Beispiel ist die Nachricht leer, denn es soll lediglich der Knoten 'conversation_start' im Dialogmodul ausgelöst werden, weshalb kein Eingabetext benötigt wird.

Verwenden Sie den Befehl `node <filename.js>`, um die Beispielanwendung auszuführen.
{: javascript}

Verwenden Sie die Syntax `python <filename.py>`, um die Beispielanwendung auszuführen.
{: python}

**Hinweis:** Stellen Sie sicher, dass Sie das Watson-SDK für Node.js mit `npm install watson-developer-cloud` installiert haben.
{: javascript}

**Hinweis:** Stellen Sie sicher, dass Sie das Watson-SDK für Python mit `pip install --upgrade watson-developer-cloud` oder `easy_install --upgrade watson-developer-cloud` installiert haben.
{: python}

Wann alles wie erwartet funktioniert, gibt der Service '{{site.data.keyword.conversationshort}}' die Ausgabe aus dem Dialogmodul zurück, die anschließend in der Konsole ausgegeben wird:

```
Welcome to the {{site.data.keyword.conversationshort}} example!
```
{: screen}

Diese Ausgabe bedeutet, dass Sie erfolgreich mit dem Service '{{site.data.keyword.conversationshort}}' kommuniziert und die Willkommensnachricht empfangen haben, die durch den Knoten 'conversation_start' im Dialogmodul angegeben wird. Nun können Sie eine Benutzerschnittstelle hinzufügen und so die Verarbeitung von Benutzereingaben ermöglichen.

## Benutzereingabe zur Erkennung von Absichten verarbeiten

Damit eine Benutzereingabe verarbeitet werden kann, müssen Sie Ihrer Anwendung eine Benutzerschnittstelle hinzufügen. Das Beispiel ist auch in dieser Hinsicht einfach gehalten; es werden Standardeingabe und -ausgabe verwendet.
<span class="ph style-scope doc-content" data-hd-programlang="javascript">Hierzu kann das Node.js-Modul 'prompt-sync' verwendet werden. (Sie können das Modul 'prompt-sync' mit dem Befehl `npm install prompt-sync` installieren.)</span>
<span class="ph style-scope doc-content" data-hd-programlang="python">Hierzu kann die Python-Funktion `input` verwendet werden.</span>

```javascript
// Beispiel 2: Benutzereingabe hinzufügen und Absichten erkennen

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Service-Wrapper für Conversation einrichten
var conversation = new ConversationV1({
  username: 'USERNAME', // Durch den Servicebenutzernamen ersetzen
  password: 'PASWORD', // Durch das Servicekennwort ersetzen
  version_date: '2017-05-26'
});

var workspace_id = 'WORKSPACE_ID'; // Durch Arbeitsbereichs-ID ersetzen

// Dialog mit leerer Nachricht starten
conversation.message({
  workspace_id: workspace_id
  }, processResponse);

// Dialogantwort verarbeiten
function processResponse(err, response) {
  if (err) {
    console.error(err); // Bei Auftreten eines Fehlers
    return;
  }

  // Bei Erkennung einer Absicht diese in der Konsole protokollieren
  if (response.intents.length > 0) {
    console.log('Detected intent: #' + response.intents[0].intent);
  }

  // Ausgabe des Dialogs, sofern vorhanden, anzeigen
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }

  // Nächste Eingaberunde anfordern
  var newMessageFromUser = prompt('>> ');
  conversation.message({
    workspace_id: workspace_id,
    input: { text: newMessageFromUser }
    }, processResponse)
}
```
{: codeblock}
{: javascript}

```python
# Beispiel 2: Benutzereingabe hinzufügen und Absichten erknnen

import watson_developer_cloud

# Service 'Conversation' einrichten
conversation = watson_developer_cloud.ConversationV1(
  username = 'USERNAME', # Durch Benutzernamen aus Serviceschlüssel ersetzen
  password = 'PASSWORD', # Durch Kennwort aus Serviceschlüssel ersetzen
  version = '2017-05-26'
)
workspace_id = 'WORKSPACE_ID' # Durch Arbeitsbereichs-ID ersetzen

# Mit leerem Wert initialisieren, um den Dialog zu starten
user_input = ''

# Hauptschleife für Ein-/Ausgabe
while True:

  # Nachricht an Service 'Conversation' senden
  response = conversation.message(
    workspace_id = workspace_id,
    input = {
      'text': user_input
    }
  )

  # Bei Erkennung einer Absicht diese in der Konsole ausgeben
  if response['intents']:
    print('Erkannte Absicht: #' + response['intents'][0]['intent'])

  # Ausgabe des Dialogs anzeigen, sofern vorhanden
  if response['output']['text']:
    print(response['output']['text'][0])

  # Nächste Eingaberunde anfordern
  user_input = input('>> ')
```
{: codeblock }
{: python }

Diese Version der Anwendung beginnt wie die vorherige mit dem Senden einer leeren Nachricht an den Service '{{site.data.keyword.conversationshort}}', um den Dialog zu starten.

Die Funktion `processResponse()` zeigt jetzt die vom Dialogmodul erkannten Absichten zusammen mit dem Ausgabetext an und fordert anschließend die nächste Runde der Benutzereingabe an.
{: javascript }

Sie zeigt jetzt die vom Dialogmodul erkannten Absichten zusammen mit dem Ausgabetext an und fordert anschließend die nächste Runde der Benutzereingabe an. (Momentan wird eine Schleife `while True` verwendet, da noch keine Methode zum Beenden des Dialogs implementiert wurde.)
{: python }

Aber etwas stimmt immer noch nicht:

```
Welcome to the {{site.data.keyword.conversationshort}} example!
>> hello
Detected intent: #hello
Welcome to the {{site.data.keyword.conversationshort}} example!
>> what time is it?
Detected intent: #time
Welcome to the {{site.data.keyword.conversationshort}} example!
>> goodbye
Detected intent: #goodbye
Welcome to the {{site.data.keyword.conversationshort}} example!
>>
```
{: screen}

Der Service '{{site.data.keyword.conversationshort}}' erkennt die korrekten Absichten, aber in jeder Runde des Dialogs wird weiterhin die Willkommensnachricht aus dem Knoten 'conversation_start' (`Welcome to the {{site.data.keyword.conversationshort}} example!`) zurückgegeben.

Dies liegt daran, dass der Service '{{site.data.keyword.conversationshort}}' statusunabhängig ist; für die Verwaltung von Statusinformationen ist die Anwendung zuständig. Da noch keine Aktion zum Verwalten von Statuswerten definiert wurde, betrachtet der Service '{{site.data.keyword.conversationshort}}' jede Runde mit Benutzereingabe als erste Runde eines neuen Dialogs und löst somit den Knoten 'conversation_start' aus.

## Status verwalten

Zur Verwaltung von Statusinformationen für Ihren Dialog wird der *Kontext* verwendet. Der Kontext is ein JSON-Objekt, das zwischen Ihrer Anwendung und dem Service '{{site.data.keyword.conversationshort}}' hin und her übergeben wird. Für die Verwaltung des Kontextes zwischen einer Runde des Dialogs und der nächsten Runde ist die Anwendung zuständig.

Der Kontext enthält für jeden Dialog mit einem Benutzer eine eindeutige ID sowie einen Zähler, der bei jeder Dialogrunde erhöht wird. In der vorherigen Fassung des Beispiels wurde der Kontext nicht beibehalten, was bedeutet, dass jede Eingaberunde als Beginn eines neuen Dialogs betrachtet wurde. Dies kann korrigiert werden, indem der Kontext jedes Mal gespeichert und zurück an den Service '{{site.data.keyword.conversationshort}}' gesendet wird.

Der Kontext kann aber nicht nur verwalten, an welcher Stelle sich der Dialog gerade befindet, sondern auch zum Speichern von anderen Daten verwendet werden, die zwischen der Anwendung und dem Service '{{site.data.keyword.conversationshort}}' hin und zurück übergeben werden sollen. Hierzu können persistente Daten gehören, die während des gesamten Dialogs erhalten bleiben sollen (z. B. der Name oder die Kontonummer eines Kunden), oder andere Daten, die Sie protokollieren wollen (z. B. der aktuelle Status der Optionseinstellungen).

```javascript
// Beispiel 3: Status verwalten

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Service-Wrapper für Conversation einrichten
var conversation = new ConversationV1({
  username: 'USERNAME', // Durch den Servicebenutzernamen ersetzen
  password: 'PASSWORD', // Durch das Servicekennwort ersetzen
  version_date: '2017-05-26'
});

var workspace_id = 'WORKSPACE_ID'; // Durch Arbeitsbereichs-ID ersetzen

// Dialog mit leerer Nachricht starten
conversation.message({
  workspace_id: workspace_id
  }, processResponse);

// Dialogantwort verarbeiten
function processResponse(err, response) {
  if (err) {
    console.error(err); // Bei Auftreten eines Fehlers
    return;
  }

  // Bei Erkennung einer Absicht diese in der Konsole protokollieren
  if (response.intents.length > 0) {
    console.log('Erkannte Absicht: #' + response.intents[0].intent);
  }

  // Ausgabe des Dialogs anzeigen, sofern vorhanden
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }

  // Nächste Eingaberunde anfordern
    var newMessageFromUser = prompt('>> ');
    // Kontext zurück an Statusverwaltung senden
    conversation.message({
      workspace_id: workspace_id,
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
}
```
{: codeblock}
{: javascript }

```python
# Beispiel 3: Status verwalten

import watson_developer_cloud

# Service 'Conversation' einrichten
conversation = watson_developer_cloud.ConversationV1(
  username = 'USERNAME', # Durch Benutzernamen aus Serviceschlüssel ersetzen
  password = 'PASSWORD', # Durch Kennwort aus Serviceschlüssel ersetzen
  version = '2017-05-26'
)
workspace_id = 'WORKSPACE_ID' # Durch Arbeitsbereichs-ID ersetzen

# Mit leerem Wert initialisieren, um den Dialog zu starten
user_input = ''
context = {}

# Hauptschleife für Ein-/Ausgabe
while True:

  # Nachricht an Service 'Conversation' senden
  response = conversation.message(
    workspace_id = workspace_id,
    input = {
      'text': user_input
    },
    context = context
  )

  # Bei Erkennung einer Absicht diese in der Konsole ausgeben
  if response['intents']:
    print('Erkannte Absicht: #' + response['intents'][0]['intent'])

  # Ausgabe des Dialogs anzeigen, sofern vorhanden
  if response['output']['text']:
    print(response['output']['text'][0])

  # Gespeicherten Kontext durch den zuletzt vom Dialogmodul empfangenen Kontext ersetzen
  context = response['context']

  # Nächste Eingaberunde anfordern
  user_input = input('>> ')
```
{: codeblock }
{: python }

Die einzige Änderung zum vorherigen Beispiel besteht darin, dass in jeder Dialogrunde jetzt das Objekt `response.context` zurückgesendet wird, das in der vorherigen Runde empfangen wurde.
{: javascript }

```javascript
    conversation.message({
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
```
{: codeblock}
{: javascript }

Die einzige Änderung im Vergleich zum vorherigen Beispiel besteht darin, dass nun der vom Dialogmodul übermittelte Kontext in einer Variable namens `context` Kontext gespeichert und mit der nächsten Runde der Benutzereingabe zurückgesendet wird:
{: python }

```python
  response = conversation.message(
    workspace_id = workspace_id,
    input = {
      'text': user_input
    },
    context = context
  )
```
{: codeblock }
{: python }

Dies stellt sicher, dass der Kontext von einer Runde zur nächsten verwaltet wird, weshalb der Service '{{site.data.keyword.conversationshort}}' nicht mehr bei jeder Runde davon ausgeht, dass es sich um die erste Runde handelt:

```
>> hello
Detected intent: #hello
Good day to you.
>> what time is it?
Detected intent: #time
>> goodbye
Detected intent: #goodbye
OK! See you later.
>>
```
{: screen}

Jetzt sind Sie einen Schritt weiter! Der Service '{{site.data.keyword.conversationshort}}' erkennt korrekt die Absichten und das Dialogmodul gibt für jede Absicht den richtigen Ausgabetext zurück (sofern verfügbar).

Mehr findet jedoch nicht statt. Bei der Frage nach der Uhrzeit wird keine Antwort ausgegeben und wenn sich der Benutzer verabschiedet, wird der Dialog nicht beendet. Dies liegt daran, dass für solche Absichten zusätzliche Aktionen durch die App ausgeführt werden müssen.

## Appaktionen implementieren

Neben dem Ausgabetext, der für den Benutzer angezeigt werden soll, verwendet dieses Dialogmodul das Objekt `output` in der JSON-Antwort, um zu signalisieren, wann die Anwendung aufgrund der erkannten Absichten eine Aktion ausführen muss.

Diese Aktionsflags werden mithilfe der Eigenschaft `action` gesendet, die im Dialogmodul als Teil der JSON-Antwort definiert ist. Sobald das Dialogmodul feststellt, dass die Anwendung eine Aktion ausführen muss, legt es für `action` den entsprechenden Wert fest, entweder `display_time` (= Zeit anzeigen) oder `end_conversation` (= Dialog beenden).

Denken Sie immer daran, dass `output` einfach nur ein JSON-Objekt ist und Sie jeden gewünschten gültigen Inhalt zu diesem Objekt hinzufügen können. Für eine komplexere Anwendung könnten Sie ein Array mit mehreren Aktionsflags verwenden.

In unserem Beispiel wird jedoch ein einfaches Schlüssel/Wert-Paar verwendet, das ein einzelnes Aktionsflag unterstützt. Der Anwendungscode muss den Wert der Eigenschaft `action` in der Antwort überprüfen und anschließend jede angegebene Aktion ausführen. (Diese Version entfernt auch das Anzeigen der erkannten Absichten, da jetzt sicher ist, dass sie korrekt erkannt werden.)

```javascript
// Beispiel 4: Appaktionen implementieren

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Service-Wrapper für Conversation einrichten
var conversation = new ConversationV1({
  username: 'USERNAME', // Durch den Servicebenutzernamen ersetzen
  password: 'PASSWORD', // Durch das Servicekennwort ersetzen
  version_date: '2017-05-26'
});

var workspace_id = 'WORKSPACE_ID'; // Durch Arbeitsbereichs-ID ersetzen

// Dialog mit leerer Nachricht starten
conversation.message({
  workspace_id: workspace_id
  }, processResponse);

// Dialogantwort verarbeiten
function processResponse(err, response) {
  if (err) {
    console.error(err); // Bei Auftreten eines Fehlers
    return;
  }

  var endConversation = false;
  
  // Auf Aktionsflags überprüfen
  if (response.output.action === 'display_time') {
    // Lokale Systemzeit ausgeben, da der Benutzer nach der Uhrzeit gefragt hat
    console.log('The current time is ' + new Date().toLocaleTimeString());
  } else if (response.output.action === 'end_conversation') {
    // Dialog beenden, da sich der Benutzer verabschiedet hat
    console.log(response.output.text[0]);
    endConversation = true;
  } else {
    // Ausgabe des Dialogs, sofern vorhanden, anzeigen
    if (response.output.text.length != 0) {
        console.log(response.output.text[0]);
    }
  }

  // Nächste Eingaberunde anfordern, falls der Dialog nicht beendet ist
  if (!endConversation) {
    var newMessageFromUser = prompt('>> ');
    conversation.message({
      workspace_id: workspace_id,
      input: { text: newMessageFromUser },
      // Kontext zur Statusverwaltung zurücksenden.
      context : response.context,
    }, processResponse)
  }
}
```
{: codeblock}
{: javascript}

```python
# Beispiel 4: Appaktionen implementieren

import watson_developer_cloud
import time

# Service 'Conversation' einrichten
conversation = watson_developer_cloud.ConversationV1(
  username = 'USERNAME', # Durch Benutzernamen aus Serviceschlüssel ersetzen
  password = 'PASSWORD', # Durch Kennwort aus Serviceschlüssel ersetzen
  version = '2017-05-26'
)
workspace_id = 'WORKSPACE_ID' # Durch Arbeitsbereichs-ID ersetzen

# Mit leerem Wert initialisieren, um den Dialog zu starten
user_input = ''
context = {}
current_action = ''

# Hauptschleife für Ein-/Ausgabe
while current_action != 'end_conversation':

  # Nachricht an Service 'Conversation' senden
  response = conversation.message(
    workspace_id = workspace_id,
    input = {
      'text': user_input
    },
    context = context
  )

  # Ausgabe des Dialogs anzeigen, sofern vorhanden
  if response['output']['text']:
    print(response['output']['text'][0])

  # Gespeicherten Kontext durch den zuletzt vom Dialogmodul empfangenen Kontext ersetzen
  context = response['context']
  # Auf Aktionsflags prüfen, die vom Dialogmodul gesendet wurden
  if 'action' in response['output']:
    current_action = response['output']['action']
  # Lokale Systemzeit ausgeben, da der Benutzer nach der Uhrzeit gefragt hat
  if current_action == 'display_time':
    print('Die aktuelle ist ' + time.strftime('%I:%M:%S %p'))
  # Nächste Eingaberunde anfordern, falls der Dialog nicht beendet ist
  if current_action != 'end_conversation':
    user_input = input('>> ')
```
{: codeblock}
{: python}

Die Funktion 'processResponse()' überprüft nun den Wert der Eigenschaft `action` für das Objekt `output`, das vom Service '{{site.data.keyword.conversationshort}}' empfangen wurde. Falls der Wert entweder `display_time` oder `end_conversation` lautet, führt die Anwendung die entsprechende Aktion aus.
{: javascript}

Die App überprüft nun den Wert der Eigenschaft `action` für das Objekt `output`, das vom Service '{{site.data.keyword.conversationshort}}' empfangen wurde. Falls der Wert `display_time` lautet, führt die Anwendung die entsprechende Aktion aus. Wenn der wert `end_conversation` lautet, fragt die App nicht nach weiteren Benutzereingaben und die Schleife `while` endet.
{: python}

```
Welcome to the {{site.data.keyword.conversationshort}} example!
>> hello
Good day to you.
>> what time is it?
The current time is 12:40:42 PM.
>> goodbye
OK! See you later.
```
{: screen}

Gratulation! Die Anwendung verwendet jetzt den Service '{{site.data.keyword.conversationshort}}', um die Absichten aus der Eingabe in natürlicher Sprache zu erkennen, zeigt die entsprechenden Antworten an und implementiert die angeforderten Clientaktionen.

Eine reale Anwendung würde natürlich eine ausgereiftere Schnittstelle wie beispielsweise ein Fenster für einen Web-Chat verwenden. Auch die implementierten Aktionen wären komplexer und möglicherweise mit der Integration bei einer Kundendatenbank oder einem anderen Unternehmenssystem verbunden. Aber die Grundprinzipien für die Interaktion der Anwendung mit dem Service '{{site.data.keyword.conversationshort}}' bleiben dieselben.

Einige komplexere Beispiele finden Sie in Form der Beispielapps im Navigationsbereich.
