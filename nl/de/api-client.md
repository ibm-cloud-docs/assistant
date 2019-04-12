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
{:tip: .tip}

# Clientanwendung erstellen
{: #api-client}

Sie verfügen jetzt über einen funktionsfähigen Dialogskill und über einen Assistenten, der diesen Skill verwendet. Nun soll eine  angepasste Clientanwendung entwickelt werden, die mit Ihren Benutzern interagiert und mit dem {{site.data.keyword.conversationfull}}-Service kommuniziert.
{: shortdesc}

## Service '{{site.data.keyword.conversationshort}}' einrichten
{: #api-client-setup}

Die Beispielanwendung, die in diesem Abschnitt erstellt wird, implementiert verschiedene Funktionen eines kognitiven persönlichen Assistenten. Der Anwendungscode soll eine Verbindung zu einem {{site.data.keyword.conversationshort}}-Assistenten herstellen, in dem die kognitive Verarbeitung (z. B. die Erkennung der Benutzerabsichten) stattfindet. 

Bevor Sie mit diesem Beispiel fortfahren, müssen Sie den erforderlichen Assistenten einrichten. Gehen Sie dabei wie folgt vor: 

1.  Laden Sie die <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/assistant/assistant-simple-example.json" download="assistant-simple-example.json">JSON-Datei</a> herunter.
1.  [Importieren Sie den Skill](/docs/services/assistant?topic=assistant-skill-add#creating-skills) in eine Instanz des {{site.data.keyword.conversationshort}}-Service.
1.  [Erstellen Sie einen Assistenten](/docs/services/assistant?topic=assistant-assistant-add#creating-assistants) und verbinden Sie den von Ihnen importierten Skill.

## Serviceinformationen abrufen
{: #api-client-get-info}

Damit Ihre Anwendung auf die REST-APIs des {{site.data.keyword.conversationshort}}-Service zugreifen kann, muss sie sich bei {{site.data.keyword.Bluemix}} authentifizieren und eine Verbindung zum richtigen Assistenten herstellen können. Sie müssen die Berechtigungsnachweise für den Service und die ID des Assistenten kopieren und in Ihren Anwendungscode einfügen.

Um auf die Serviceberechtigungsnachweise und die Assistenten-ID zuzugreifen, rufen Sie im {{site.data.keyword.conversationshort}}-Tool die Registerkarte **Assistenten** auf und klicken Sie auf das ![Menü](images/kabob-grey.png) für den Assistenten, zu dem Sie eine Verbindung herstellen möchten. Wählen Sie **API-Details anzeigen** aus, um die Details für den Assistenten (einschließlich Assistenten-ID und API-Schlüssel) anzuzeigen.

Auf die Berechtigungsnachweise für den Service können Sie auch über das {{site.data.keyword.Bluemix_short}}-Dashboard zugreifen.

## Mit dem Service '{{site.data.keyword.conversationshort}}' kommunizieren
{: #api-client-communicate}

Die Interaktion mit dem Service '{{site.data.keyword.conversationshort}}' ist ganz einfach. Das folgende Beispiel stellt eine Verbindung zum Service her, sendet eine einfache Nachricht und protokolliert die Ausgabe in der Konsole:

```javascript
// Beispiel 1: Service-Wrapper einrichten, Startnachricht senden und
// Antwort empfangen

var AssistantV2 = require('watson-developer-cloud/assistant/v2');

// Service-Wrappper für Assistent einrichten
var service = new AssistantV2({
  iam_apikey: '{apikey}', // Durch API-Schlüssel ersetzen
  version: '2018-09-20'
});

var assistantId = '{assistant_id}'; // Durch Assistenten-ID ersetzen
var sessionId;

// Sitzung erstellen.
service.createSession({
  assistant_id: assistantId
}, function(err, result) {
  if (err) {
    console.error(err); // Bei Auftreten eines Fehlers
    return;
  }
  sessionId = result.session_id;
  sendMessage(); // Dialog mit leerer Nachricht starten
});

// Nachricht an Assistent senden
function sendMessage() {
  service.message({
    assistant_id: assistantId,
    session_id: sessionId
  }, processResponse);
}

// Antwort verarbeiten
function processResponse(err, response) {
  if (err) {
    console.error(err); // Bei Auftreten eines Fehlers
    return;
  }

  // Ausgabe des Assistenten anzeigen, falls vorhanden. Setzt einzelne Textantwort voraus.
  if (response.output.generic.length != 0) {
      console.log(response.output.generic[0].text);
  }

  // Abschließend die Sitzung beenden
  service.deleteSession({
    assistant_id: assistantId,
    session_id: sessionId
  }, function(err, result) {
    if (err) {
      console.error(err); // Bei Auftreten eines Fehlers
    }
  });
}
```
{: codeblock}
{: javascript}

```python
# Beispiel 1: Service-Wrapper einrichten, Startnachricht senden und
# Antwort empfangen

import watson_developer_cloud

# Service 'Assistant' einrichten
service = watson_developer_cloud.AssistantV2(
    iam_apikey = '{apikey}', # Durch API-Schlüssel ersetzen
    version = '2018-09-20'
)

assistant_id = '{assistant_id}' # Durch Assistenten-ID ersetzen

# Sitzung erstellen
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

# Dialog mit leerer Nachricht starten
response = service.message(
    assistant_id,
    session_id
).get_result()

# Ausgabe des Dialogs anzeigen, sofern vorhanden. Setzt einzelne Textantwort voraus.
if response['output']['generic']:
    print(response['output']['generic'][0]['text'])

# Zum Abschluss die Sitzung löschen
service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
```
{: codeblock}
{: python}

```java
/*
 * Beispiel 1: Service-Wrapper einrichten, Startnachricht senden und
 * Antwort empfangen
 */

import com.ibm.watson.developer_cloud.assistant.v2.Assistant;
import com.ibm.watson.developer_cloud.assistant.v2.model.CreateSessionOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.DeleteSessionOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageResponse;
import com.ibm.watson.developer_cloud.assistant.v2.model.SessionResponse;
import com.ibm.watson.developer_cloud.service.security.IamOptions;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Protokollnachrichten in stdout unterdrücken
    LogManager.getLogManager().reset();

    // Service 'Assistant' einrichten
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build();
    Assistant service = new Assistant("2018-09-20", iamOptions);
    String assistantId = "{assistant_id}"; // Durch Assistenten-ID ersetzen

    // Sitzung erstellen
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute();
    String sessionId = session.getSessionId();

    // Dialog mit leerer Nachricht starten
    MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId).build();
    MessageResponse response = service.message(messageOptions).execute();

    // Ausgabe des Dialogs anzeigen, falls vorhanden. Setzt einzelne Textantwort voraus.
System.out.println(response.getOutput().getGeneric().get(0).getText());

    // Zum Abschluss die Sitzung löschen
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build();
    service.deleteSession(deleteSessionOptions).execute();
  }
}
```
{: codeblock}
{: java}

Im ersten Schritt wird ein Wrapper für den Service '{{site.data.keyword.conversationshort}}' erstellt.

Der Wrapper ist ein Objekt, das Sie verwenden, um Eingaben an den Service zu senden und Ausgaben vom Service zu empfangen. Beim Erstellen des Service-Wrappers geben Sie die Authentifizierungsnachweise aus dem Serviceschlüssel sowie die Version der verwendeten {{site.data.keyword.conversationshort}}-API an.

In diesem Node.js-Beispiel ist der Wrapper eine Instanz von `AssistantV2`, gespeichert in der Variablen `service`. Die Watson-SDKs für andere Sprachen bieten funktional entsprechende Mechanismen für die Instanziierung eines Service-Wrappers.
{: javascript}

In diesem Python-Beispiel ist der Wrapper eine Instanz von `watson_developer_cloud.AssistantV2`, gespeichert in der Variablen `service`. Die Watson-SDKs für andere Sprachen bieten funktional entsprechende Mechanismen für die Instanziierung eines Service-Wrappers.
{: python}

In diesem Java-Beispiel ist der Wrapper eine Instanz von `Assistant`, gespeichert in der Variablen `service`. Die Watson-SDKs für andere Sprachen bieten funktional entsprechende Mechanismen für die Instanziierung eines Service-Wrappers.
{: java}

Nach dem Erstellen wird der Service-Wrapper verwendet, um eine Sitzung zu erstellen und eine Nachricht an den Assistenten zu senden. In diesem Beispiel ist die Nachricht leer, denn es soll lediglich der Knoten 'conversation_start' im Dialogmodul ausgelöst werden, weshalb kein Eingabetext benötigt wird. Danach wird der Antworttext in der Konsole ausgegeben und abschließend die Sitzung gelöscht.

Verwenden Sie den Befehl `node <filename.js>`, um die Beispielanwendung auszuführen.
{: javascript}

Verwenden Sie den Befehl `python3 <filename.py>`, um die Beispielanwendung auszuführen.
{: python}

Fügen Sie den Beispielcode in eine Datei mit dem Namen `AssistantSimpleExample.java` ein. Anschließend können Sie die Datei kompilieren und ausführen.
{: java}

**Hinweis:** Stellen Sie sicher, dass Sie das Watson-SDK für Node.js mit `npm install watson-developer-cloud` installiert haben.
{: javascript}

**Hinweis:** Stellen Sie sicher, dass Sie das Watson-SDK für Python mit `pip install --upgrade watson-developer-cloud` oder `easy_install --upgrade watson-developer-cloud` installiert haben.
{: python}

**Hinweis:** Stellen Sie sicher, dass Sie das [Watson-SDK for Java ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/watson-developer-cloud/java-sdk/blob/develop/README.md){: new_window} installiert haben.
{: java}

Wenn alles wie erwartet funktioniert, gibt der Assistent die Ausgabe des Dialogs zurück, die anschließend in der Konsole ausgegeben wird: 

```
Welcome to the Watson Assistant example!
```
{: screen}

Diese Ausgabe bedeutet, dass Sie erfolgreich mit dem Service '{{site.data.keyword.conversationshort}}' kommuniziert und die Willkommensnachricht empfangen haben, die durch den Knoten 'conversation_start' im Dialogmodul angegeben wird. Nun können Sie eine Benutzerschnittstelle hinzufügen und so die Verarbeitung von Benutzereingaben ermöglichen.

## Benutzereingabe zur Erkennung von Absichten verarbeiten
{: #api-client-process-input}

Damit Benutzereingaben verarbeitet werden können, müssen Sie in Ihrer Anwendung eine Benutzerschnittstelle hinzufügen. Das Beispiel ist auch in dieser Hinsicht einfach gehalten; es werden Standardeingabe und -ausgabe verwendet.
<span class="ph style-scope doc-content" data-hd-programlang="javascript">Hierzu kann das Node.js-Modul 'prompt-sync' verwendet werden. (Sie können das Modul 'prompt-sync' mit dem Befehl `npm install prompt-sync` installieren.)</span>
<span class="ph style-scope doc-content" data-hd-programlang="python">Hierzu kann die Python 3-Funktion `input` verwendet werden.</span>
<span class="ph style-scope doc-content" data-hd-programlang="java">Hierzu kann die Java-Funktion `Console.readLine()` verwendet werden.</span>

```javascript
// Beispiel 2: Benutzereingabe hinzufügen und Absichten erkennen

var prompt = require('prompt-sync')();
var AssistantV2 = require('watson-developer-cloud/assistant/v2');

// Service-Wrapper für Assistent einrichten
var service = new AssistantV2({
  iam_apikey: '{apikey}', // Durch API-Schlüssel ersetzen
  version: '2018-09-20'
});

var assistantId = '{assistant_id}'; // Durch Assistenten-ID ersetzen
var sessionId;

// Sitzung erstellen
service.createSession({
  assistant_id: assistantId
}, function(err, result) {
  if (err) {
    console.error(err); // Bei Auftreten eines Fehlers
    return;
  }
  sessionId = result.session_id;
  sendMessage(); // Dialog mit leerer Nachricht starten
});

// Nachricht an Assistent senden
function sendMessage(messageText) {
  service.message({
    assistant_id: assistantId,
    session_id: sessionId,
    input: {
      message_type: 'text',
      text: messageText
    }
  }, processResponse);
}

// Antwort verarbeiten
function processResponse(err, response) {
  if (err) {
    console.error(err); // Bei Auftreten eines Fehlers
    return;
  }

  // Bei Erkennung einer Absicht diese in der Konsole protokollieren
  if (response.output.intents.length > 0) {
    console.log('Detected intent: #' + response.output.intents[0].intent);
  }

  // Ausgabe des Assistenten anzeigen, falls vorhanden. Setzt einzelne Textantwort voraus.
  if (response.output.generic.length != 0) {
    console.log(response.output.generic[0].text);
  }

  // Nächste Eingaberunde anfordern
  var newMessageFromUser = prompt('>> ');
    if (newMessageFromUser === 'quit') {
      service.deleteSession({
        assistant_id: assistantId,
        session_id: sessionId
      }, function(err, result) {
        if (err) {
          console.error(err); // Bei Auftreten eines Fehlers
        }
      });
      return;
    }
  sendMessage(newMessageFromUser);
}
```
{: codeblock}
{: javascript}

```python
# Beispiel 2: Benutzereingabe hinzufügen und Absichten erknnen

import watson_developer_cloud

# Service 'Assistant' einrichten
service = watson_developer_cloud.AssistantV2(
    iam_apikey = '{apikey}', # Durch API-Schlüssel ersetzen
    version = '2018-09-20'
)

assistant_id = '{assistant_id}' # Durch Assistenten-ID ersetzen

# Sitzung erstellen
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

# Mit leerem Wert initialisieren, um den Dialog zu starten
user_input = ''

# Hauptschleife für Ein-/Ausgabe
while user_input != 'quit':

    # Nachricht an Assistent senden
    response = service.message(
        assistant_id,
        session_id,
        input = {
            'text': user_input
    }
    ).get_result()

    # Bei Erkennung einer Absicht diese in der Konsole ausgeben
    if response['output']['intents']:
        print('Detected intent: #' + response['output']['intents'][0]['intent'])

    # Ausgabe des Dialogs anzeigen, sofern vorhanden. Setzt einzelne Textantwort voraus.
if response['output']['generic']:
        print(response['output']['generic'][0]['text'])

    # Nächste Eingaberunde anfordern
    user_input = input('>> ')

# Zum Abschluss die Sitzung löschen
service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
```
{: codeblock }
{: python }

```java
/*
 * Beispiel 2: Benutzereingabe hinzufügen und Absichten erkennen
 */

import com.ibm.watson.developer_cloud.assistant.v2.Assistant;
import com.ibm.watson.developer_cloud.assistant.v2.model.CreateSessionOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.DeleteSessionOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.DialogRuntimeResponseGeneric;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageInput;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageResponse;
import com.ibm.watson.developer_cloud.assistant.v2.model.RuntimeIntent;
import com.ibm.watson.developer_cloud.assistant.v2.model.SessionResponse;
import com.ibm.watson.developer_cloud.service.security.IamOptions;
import java.util.List;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Protokollnachrichten in stdout unterdrücken
    LogManager.getLogManager().reset();

    // Service 'Assistant' einrichten
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build();
    Assistant service = new Assistant("2018-09-20", iamOptions);
    String assistantId = "{assistant_id}"; // Durch Assistenten-ID ersetzen

    // Sitzung erstellen
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute();
    String sessionId = session.getSessionId();

    // Mit leerem Wert initialisieren, um den Dialog zu starten
    String inputText = "";

    // Hauptschleife für Ein-/Ausgabe
    do {
      // Nachricht an Assistent senden
      MessageInput input = new MessageInput.Builder().text(inputText).build();
      MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId)
                                                  .input(input)
                                                  .build();
      MessageResponse response = service.message(messageOptions).execute();

      // Bei Erkennung einer Absicht diese in der Konsole ausgeben
      List<RuntimeIntent> responseIntents = response.getOutput().getIntents();
      if(responseIntents.size() > 0) {
        System.out.println("Detected intent: #" + responseIntents.get(0).getIntent());
      }

      // Ausgabe des Dialogs anzeigen, falls vorhanden. Setzt einzelne Textantwort voraus.
      List<DialogRuntimeResponseGeneric> responseGeneric = response.getOutput().getGeneric();
      if(responseGeneric.size() > 0) {
        System.out.println(response.getOutput().getGeneric().get(0).getText());
      }

      // Nächste Eingaberunde anfordern
      System.out.print(">> ");
      inputText = System.console().readLine();
    } while(!inputText.equals("quit"));

    // Zum Abschluss die Sitzung löschen
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build();
    service.deleteSession(deleteSessionOptions).execute();
  }
}
```
{: codeblock }
{: java }

Diese Version der Anwendung beginnt wie die vorherige mit dem Senden einer leeren Nachricht an den Assistenten, um den Dialog zu starten.

Die Funktion `processResponse()` zeigt jetzt die vom Dialogskill erkannten Absichten zusammen mit dem Ausgabetext an. Anschließend wird die nächste Runde der Benutzereingabe angefordert.
{: javascript }

Danach wird die vom Dialogskill erkannte Absicht zusammen mit dem Ausgabetext angezeigt. Anschließend wird die nächste Runde der Benutzereingabe angefordert.
{: python }

Sie zeigt jetzt die vom Dialogmodul erkannten Absichten zusammen mit dem Ausgabetext an und fordert anschließend die nächste Runde der Benutzereingabe an.
{: java}

Da noch kein Prozess mit natürlicher Sprache zum Beenden des Dialogs implementiert ist, geben wir stattdessen mit dem Befehl `quit` an, dass die Sitzung gelöscht und das Programm beendet werden soll.

```
Welcome to the Watson Assistant example!
>> hello
Detected intent: #hello
Good day to you.
>> what time is it?
Detected intent: #time
>> goodbye
Detected intent: #goodbye
OK! See you later.
>> quit
```
{: screen}

Jetzt sind Sie einen Schritt weiter! Der Service '{{site.data.keyword.conversationshort}}' erkennt korrekt die Absichten und das Dialogmodul gibt für jede Absicht den richtigen Ausgabetext zurück (sofern verfügbar).

Mehr findet jedoch nicht statt. Bei der Frage nach der Uhrzeit wird keine Antwort ausgegeben und wenn sich der Benutzer verabschiedet, wird der Dialog nicht beendet. Dies liegt daran, dass für solche Absichten zusätzliche Aktionen durch die App ausgeführt werden müssen.

## Appaktionen implementieren
{: #api-client-implement-actions}

Neben dem Ausgabetext, der für den Benutzer angezeigt werden soll, verwendet dieses {{site.data.keyword.conversationshort}}-Dialogmodul das Array `actions` in der JSON-Antwort, um zu signalisieren, wann die Anwendung aufgrund der erkannten Absichten eine Aktion ausführen muss. Sobald das Dialogmodul feststellt, dass die Clientanwendung eine Aktion ausführen muss, gibt es ein Aktionsobjekt mit `client` als Wert für `type` zurück. Der Name (`name`) der Aktion gibt die bestimmte Aktion an (entweder `display_time` oder `end_conversation`). (In zusätzlichen Eigenschaften können Parameter, Berechtigungsnachweise und andere Informationen für die Aktion angegeben werden. Für das vorliegende Beispiel genügt der Aktionsname.)

Da unser Dialogmodul nicht mehr als eine Aktion auf einmal anfordert, muss die Clientanwendung nur nach einer Aktion `client` im Array `actions` suchen. Wird ein solches Vorkommen gefunden, kann anschließend die angegebene Aktion ausgeführt werden. (Diese Version entfernt auch das Anzeigen der erkannten Absichten, da jetzt sicher ist, dass sie korrekt erkannt werden.)

```javascript
// Beispiel 3: Appaktionen implementieren

var prompt = require('prompt-sync')();
var AssistantV2 = require('watson-developer-cloud/assistant/v2');

// Service 'Assistant' einrichten
var service = new AssistantV2({
  iam_apikey: '{apikey}', // Durch API-Schlüssel ersetzen
  version: '2018-09-20'
});

var assistantId = '{assistant_id}'; // Durch Assistenten-ID ersetzen
var sessionId;

// Sitzung erstellen
service.createSession({
  assistant_id: assistantId
}, function(err, result) {
  if (err) {
    console.error(err); // Bei Auftreten eines Fehlers
    return;
  }
  sessionId = result.session_id;
  sendMessage(''); // Dialog mit leerer Nachricht starten
});

// Nachricht an Assistent senden
function sendMessage(messageText) {
  service.message({
    assistant_id: assistantId,
    session_id: sessionId,
    input: {
      message_type: 'text',
      text: messageText
    }
  }, processResponse);
}

// Antwort verarbeiten
function processResponse(err, response) {
  if (err) {
    console.error(err); // Bei Auftreten eines Fehlers
    return;
  }

  var endConversation = false;

  // Vom Assistenten angeforderte Clientaktionen suchen
  if (response.output.actions) {
    if (response.output.actions[0].type === 'client'){
      if (response.output.actions[0].name === 'display_time') {
        // Lokale Systemzeit ausgeben, da der Benutzer nach der Uhrzeit gefragt hat
        console.log('The current time is ' + new Date().toLocaleTimeString() + '.');
      } else if (response.output.actions[0].name === 'end_conversation') {
        // Dialog beenden, da sich der Benutzer verabschiedet hat
        console.log(response.output.generic[0].text);
        endConversation = true;
      }
    }
  } else {
    // Ausgabe des Assistenten anzeigen, falls vorhanden. Setzt einzelne Textantwort voraus.
    if (response.output.generic.length != 0) {
      console.log(response.output.generic[0].text);
    }
  }

  // Nächste Eingaberunde anfordern, falls der Dialog nicht beendet ist
  if (!endConversation) {
    var newMessageFromUser = prompt('>> ');
    sendMessage(newMessageFromUser);
  } else {
    service.deleteSession({
      assistant_id: assistantId,
      session_id: sessionId
    }, function(err, result) {
      if (err) {
        console.error(err); // Bei Auftreten eines Fehlers
      }
    });
    return;
  }
}
```
{: codeblock}
{: javascript}

```python
# Beispiel 3: Appaktionen implementieren

import watson_developer_cloud
import time

# Service 'Assistant' einrichten
service = watson_developer_cloud.AssistantV2(
    iam_apikey = '{apikey}', # Durch API-Schlüssel ersetzen
    version = '2018-09-20'
)

assistant_id = '{assistant_id}' # Durch Assistenten-ID ersetzen

# Sitzung erstellen
session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']

# Mit leeren Werten initialisieren, um den Dialog zu starten
user_input = ''
current_action = ''

# Hauptschleife für Ein-/Ausgabe
while current_action != 'end_conversation':
    # Alle von vorheriger Antwort gesetzten Aktionsflags löschen
    current_action = ''

    # Nachricht an Assistent senden
    response = service.message(
        assistant_id,
        session_id,
        input = {
            'text': user_input
    }
    ).get_result()

    # Ausgabe des Dialogs anzeigen, sofern vorhanden. Setzt einzelne Textantwort voraus.
    if response['output']['generic']:
        print(response['output']['generic'][0]['text'])

    # Vom Assistenten angeforderte Clientaktionen suchen
    if 'actions' in response['output']:
        if response['output']['actions'][0]['type'] == 'client':
            current_action = response['output']['actions'][0]['name']

    # Lokale Systemzeit ausgeben, da der Benutzer nach der Uhrzeit gefragt hat
    if current_action == 'display_time':
        print('The current time is ' + time.strftime('%I:%M:%S %p') + '.')
    # Nächste Eingaberunde anfordern, falls der Dialog nicht beendet ist
    if current_action != 'end_conversation':
    user_input = input('>> ')

# Zum Abschluss die Sitzung löschen
service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
```
{: codeblock}
{: python}

```java
/*
 * Beispiel 3: Appaktionen implementieren
 */

import com.ibm.watson.developer_cloud.assistant.v2.Assistant;
import com.ibm.watson.developer_cloud.assistant.v2.model.CreateSessionOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.DeleteSessionOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.DialogNodeAction;
import com.ibm.watson.developer_cloud.assistant.v2.model.DialogRuntimeResponseGeneric;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageInput;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageOptions;
import com.ibm.watson.developer_cloud.assistant.v2.model.MessageResponse;
import com.ibm.watson.developer_cloud.assistant.v2.model.RuntimeIntent;
import com.ibm.watson.developer_cloud.assistant.v2.model.SessionResponse;
import com.ibm.watson.developer_cloud.service.security.IamOptions;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;
import java.util.List;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main(String[] args) {

    // Protokollnachrichten in stdout unterdrücken
    LogManager.getLogManager().reset();

    // Service 'Assistant' einrichten
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build();
    Assistant service = new Assistant("2018-09-20", iamOptions);
    String assistantId = "{assistant_id}"; // Durch Assistenten-ID ersetzen

    // Sitzung erstellen
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute();
    String sessionId = session.getSessionId();

    // Mit leerem Wert initialisieren, um den Dialog zu starten.
    String inputText = "";
    String currentAction;

    // Hauptschleife für Ein-/Ausgabe
    do {
      // Alle von vorheriger Antwort gesetzten Aktionsflags löschen
      currentAction = "";

      // Nachricht an Assistent senden
      MessageInput input = new MessageInput.Builder().text(inputText).build();
      MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId)
                                                  .input(input)
                                                  .build();
      MessageResponse response = service.message(messageOptions).execute();

      // Ausgabe des Dialogs anzeigen, falls vorhanden. Setzt einzelne Textantwort voraus.
      List<DialogRuntimeResponseGeneric> responseGeneric = response.getOutput().getGeneric();
      if(responseGeneric.size() > 0) {
        System.out.println(response.getOutput().getGeneric().get(0).getText());
      }

      // Vom Assistenten angeforderte Aktionen suchen.
      List<DialogNodeAction> responseActions = response.getOutput().getActions();
      if(responseActions != null) {
        if(responseActions.get(0).getActionType().equals("client")) {
          currentAction = responseActions.get(0).getName();
        }
      }

      // Lokale Systemzeit ausgeben, da der Benutzer nach der Uhrzeit gefragt hat
      if(currentAction.equals("display_time")) {
        DateTimeFormatter fmt = DateTimeFormatter.ofPattern("h:mm:ss a");
        LocalTime time = LocalTime.now();
        System.out.println("The current time is " + time.format(fmt) + ".");
      }

      // Nächste Eingaberunde anfordern, falls der Dialog nicht beendet ist
      if(!currentAction.equals("end_conversation")) {
        System.out.print(">> ");
        inputText = System.console().readLine();
      }

    } while(!currentAction.equals("end_conversation"));

    // Zum Abschluss die Sitzung löschen
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build();
    service.deleteSession(deleteSessionOptions).execute();
  }
}
```
{: codeblock}
{: java}

Die App sucht im Array `actions` der Antwort nach vorhandenen Aktionen mit `type`=`client`. Wenn solche Aktionen vorhanden sind, wird der Wert `name` der Aktion überprüft und die zugehörige Aktion (lokale Systemzeit ausgeben oder internes Flag für Dialogende setzen) ausgeführt.

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

Eine reale Anwendung würde natürlich eine ausgereiftere Schnittstelle wie beispielsweise ein Fenster für einen Web-Chat verwenden. Auch die implementierten Aktionen wären komplexer und möglicherweise mit der Integration bei einer Kundendatenbank oder einem anderen Unternehmenssystem verbunden. Außerdem müssten zusätzliche Daten an den Assistenten gesendet werden (z. B. eine Benutzer-ID, um jeden Benutzer eindeutig zu identifizieren). Aber die Grundprinzipien für die Interaktion der Anwendung mit dem Service '{{site.data.keyword.conversationshort}}' bleiben dieselben.

Einige komplexere Beispiele finden Sie unter '[Sample apps](/docs/services/assistant?topic=assistant-sample-apps'.

## Auf Kontext zugreifen
{: #api-client-get-context}

Der *Kontext* ist ein Objekt mit Variablen, die während eines Dialogs bestehen bleiben und die vom Dialogmodul und der Clientanwendung gemeinsam genutzt werden können. Wenn Ihre Anwendung die API der Version 2 verwendet, wird der Kontext vom Assistenten für jede Sitzung automatisch verwaltet. Sowohl das Dialogmodul als auch die Clientanwendung kann Kontextvariablen lesen und schreiben. Der Kontext wird nicht standardmäßig an die Clientanwendung zurückgegeben. Sie können jedoch optional anfordern, dass der Kontext in der Antwort an jede Anforderung `/message` angegeben wird.

**Wichtig:** Eine Nutzungsmöglichkeit für den Kontext ist das Angeben einer eindeutigen Benutzer-ID für jeden Endbenutzer, der mit dem Assistenten interagiert. Bei benutzerbasierten Plänen wird diese ID für Abrechnungszwecke verwendet. (Weitere Information enthält der Abschnitt [Benutzerbasierte Pläne](/docs/services/assistant?topic=assistant-services-information#user-based-plans).)

Es gibt zwei Arten von Kontext: 

- **Globaler Kontext**: Kontextvariablen, die alle von einem Assistenten verwendeten Skills gemeinsam nutzen (einschließlich interner Systemvariablen, die zum Verwalten des Dialogablaufs verwendet werden).

- **Skillbezogener Kontext**: Spezifische Kontextvariablen für einen bestimmten Skill, (einschließlich aller von Ihrer Anwendung benötigten benutzerdefinierten Variablen). Momentan wird nur ein einziger Skill mit dem Namen `main skill` unterstützt.

Das folgende Beispiel zeigt eine Anforderung `/message` mit globalen und mit skillbezogenen Kontextvariablen. Außerdem wird in dem Beispiel durch die Eigenschaft `options.return_context` angefordert, dass der Kontext in der Antwort zurückgegeben werden soll. 

```javascript
service.message({
  assistant_id: '{assistant_id}',
  session_id: '{session_id}',
  input: {
    'message_type': 'text',
    'text': 'Hello',
    'options': {
      'return_context': true
    }
  },
  context: {
    'global': {
      'system': {
        'user_id': 'my_user_id'
      }
    },
    'skills': {
      'main skill': {
        'user_defined': {
          'account_number': '123456'
        }
      }
    }
  }
}, function(err, response) {
  if (err)
    console.log('error:', err);
  else
    console.log(JSON.stringify(response, null, 2));
});
```
{: codeblock}
{: javascript}

```python
response=service.message(
    assistant_id='{assistant_id}',
    session_id='{session_id}',
    input={
        'message_type': 'text',
        'text': 'Hello',
        'options': {
            'return_context': True
        }
    },
    context={
        'global': {
            'system': {
                'user_id': 'my_user_id'
            }
        },
        'skills': {
            'main skill': {
                'user_defined': {
                    'account_number': '123456'
                }
            }
        }
    }
).get_result()

print(json.dumps(response, indent=2))
```
{: codeblock}
{: python}

```java
    MessageInputOptions inputOptions = new MessageInputOptions();
    inputOptions.setReturnContext(true);

    MessageInput input = new MessageInput.Builder()
      .messageType("text")
      .text("Hello")
      .options(inputOptions)
      .build();

    // Globalen Kontext mit Benutzer-ID erstellen
    MessageContextGlobalSystem system = new MessageContextGlobalSystem();
    system.setUserId("my_user_id");
    MessageContextGlobal globalContext = new MessageContextGlobal();
    globalContext.setSystem(system);

    // Benutzerdefinierte Kontexvariablen in skillbezogenem Kontext für Hauptskill erstellen
    Map<String, String> userDefinedContext = new HashMap<>();
    userDefinedContext.put("account_num","123456");
    Map<String, Map> mainSkillContext = new HashMap<>();
    mainSkillContext.put("user_defined", userDefinedContext);
    MessageContextSkills skillsContext = new MessageContextSkills();
    skillsContext.put("main skill", mainSkillContext);

    MessageContext context = new MessageContext();
    context.setGlobal(globalContext);
    context.setSkills(skillsContext);

    MessageOptions options = new MessageOptions.Builder("{assistant_id}", "{session_id}")
      .input(input)
      .context(context)
      .build();

    MessageResponse response = service.message(options).execute();

    System.out.println(response);
```
{: codeblock}
{: java}

In dieser Beispielanforderung gibt die Anwendung einen Wert für `user_id` als Teil des globalen Kontexts an. Außerdem wird eine benutzerdefinierte Kontextvariable (`account_number`) als Teil des skillbezogenen Kontexts festgelegt. Diese Kontextvariable kann von Dialogmodulknoten als `$account_number` aufgerufen werden. (Weitere Informationen zur Verwendung des Kontexts in Ihrem Dialogmodul enthält der Abschnitt [Informationen zur Verarbeitung des Dialogmoduls](/docs/services/assistant?topic=assistant-dialog-runtime).)

Für eine benutzerdefinierte Kontextvariable können Sie jeden beliebigen Variablennamen angeben. Falls die angegebene Variable bereits vorhanden ist, wird sie durch den neuen Wert überschrieben, andernfalls wird eine neue Variable zum Kontext hinzugefügt. 

Die Ausgabe dieser Anforderung enthält neben den üblichen Ausgabedaten den Kontext und zeigt, dass die angegebenen Werte hinzugefügt wurden.

```json
{
  "output":{
    "generic":[
      {
        "response_type": "text",
        "text": "Welcome to the Watson Assistant example!"
      }
    ],
    "intents": [
      {
        "intent": "hello",
        "confidence": 1
      }
    ],
    "entities": []
  },
"context": {
    "global": {
      "system": {
        "turn_count": 1,
        "user_id": "my_user_id"
      }
    },
    "skills": {
      "main skill": {
        "user_defined": {
          "account_number": "123456"
        }
      }
    }
  }
}
```

Ausführliche Informationen zum Zugreifen auf Kontextvariablen mithilfe der API finden Sie unter [Version 2 - API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}.

## API der Version 2 verwenden
{: #api-client-v1-api}

Die API der Version 2 ist die empfohlene Methode zum Erstellen einer Laufzeitclientanwendung, die mit dem {{site.data.keyword.conversationshort}}-Service kommuniziert. Einige ältere Anwendungen verwenden möglicherweise noch die API der Version 1, die eine ähnliche Laufzeitmethode zum Senden von Nachrichten an den Arbeitsbereich innerhalb eines Dialogskills enthält. 

Beachten Sie Folgendes: Wenn Ihre App die API der Version 1 verwendet, kommuniziert sie direkt mit dem Arbeitsbereich, d. h. unter Umgehung der Orchestrierungs- und Statusmanagementfunktionen des Assistenten. Dies bedeutet, dass Ihre Anwendung für die Verwaltung von Statusinformationen unter Verwendung des Kontexts verantwortlich ist. Die Anwendung muss den Kontext verwalten, indem der mit jeder Antwort empfangene Kontext gespeichert und mit jeder neuen Nachrichtenanforderung an den Service zurückgesendet wird. (Die Methode `/message` der Version 1 gibt den Kontext mit jeder Antwort zurück.)

Weitere Informationen zur Methode `/message` der Version 1 und zum Kontext finden Sie in der [Version 2 - API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/apidocs/assistant#get-response-to-user-input){: new_window}.
