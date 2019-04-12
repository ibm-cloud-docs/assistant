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

# Antworten mit dem JSON-Editor definieren
{: #dialog-responses-json}

In manchen Situationen kann es erforderlich sind, Antworten mit dem JSON-Editor zu definieren. (Weitere Informationen zu Dialogmodulantworten enthält der Abschnitt [Antworten](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses).) Beim Bearbeiten des JSON-Codes für Antworten können Sie direkt auf die Daten zugreifen, die an den Kommunikationskanal oder die angepasste Anwendung zurückgegeben werden. 

## Generisches JSON-Format
{: #dialog-responses-json-generic}

Das generische JSON-Format für Antworten wird verwendet, um Antworten anzugeben, die für einen beliebigen Kanal bestimmt sind. Dieses Format kann verschiedene Antworttypen aufnehmen, die von Slack- und Facebook-Integrationen unterstützt werden. Außerdem kann dieses Format von einer angepassten Clientanwendung implementiert werden. (Dieses Format ist das Standardformat für Dialogmodulantworten, die mit dem {{site.data.keyword.conversationshort}}-Tool definiert werden.)

Informationen zum Öffnen des JSON-Editors für eine Dialogmodulknotenantwort über das Tool enthält der Abschnitt [Kontextvariablen im JSON-Editor](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-var-json).

Um eine interaktive Antwort im generischen JSON-Format anzugeben, fügen Sie die entsprechenden JSON-Objekte in das Feld `output.generic` in der Antwort des Dialogmodulknotens ein. Das folgende Beispiel zeigt, wie Sie eine Antwort senden können, die mehrere Antworttypen (Text, ein Bild und per Mausklick steuerbare Optionen) enthält):

```json
{
  "output":{
    "generic":[
      {
        "response_type": "text",
        "values": [
          {
            "text": "Dies sind die Filialen in Ihrer Nähe."
          }
        ]
      },
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Beispielbild",
        "description": "Eine Beschreibung für das Bild."
      },
      {
        "response_type": "option",
        "title": "Klicken Sie auf eine der folgenden Optionen",
        "options": [
          {
            "label": "Standort 1",
            "value": {
              "input": {
                "text": "Standort 1"
              }
            }
          },
          {
            "label": "Standort 2",
            "value": {
              "input": {
                "text": "Standort 2"
              }
            }
          },
          {
            "label": "Standort 3",
            "value": {
              "input": {
                "text": "Standort 3"
              }
            }
          }
        ]
      }
    ]
  }
}
```

Weitere Informationen zum Angeben der einzelnen unterstützten Antworttypen mithilfe von JSON-Objekten enthält der Abschnitt [Antworttypen](#dialog-responses-json-response-types).

Wenn Sie den {{site.data.keyword.conversationshort}}-Connector verwenden, wird die Antwort während der Laufzeit in das vom Kanal (Slack oder Facebook Messenger) erwartet Format umgewandelt. Wenn die Antwort mehrere Medientypen oder Anhänge enthält, wird die generische Antwort nach Bedarf in eine Reihe separater Nachrichtennutzdaten umgewandelt. Anschließend sendet der Connector die einzelnen Nachrichtennutzdaten in einer separaten Nachricht an den Kanal.

**Hinweis:** Wenn eine Antwort in mehrere Nachrichten aufgeteilt wird, sendet der {{site.data.keyword.conversationshort}}-Connector die Nachrichten nacheinander an den Kanal. Der Kanal ist dafür zuständig, diese Nachrichten an den Endbenutzer zu übermitteln (dies kann durch Netz- oder Serverprobleme beeinträchtigt werden).

Wenn Sie eine eigene Clientanwendung erstellen, muss Ihre App jeden Antworttyp entsprechend implementieren. Weitere Informationen enthält der Abschnitt [Antworten implementieren](/docs/services/assistant?topic=assistant-api-dialog-responses).

## Natives JSON-Format
{: #dialog-responses-json-native}

Neben dem generischen JSON-Format unterstützt der JSON-Dialogmodulknoten auch kanalspezifische Antworten, die in den nativen Formaten für Slack und Facebook Messenger erstellt werden. Diese Formate werden auch vom {{site.data.keyword.conversationshort}}-Connector unterstützt. Die Verwendung der nativen JSON-Formate kann hilfreich sein, wenn Ihr Arbeitsbereich nur mit einem Kanaltyp integriert werden soll und Sie einen Antworttyp angeben müssen, der vom generischen JSON-Format derzeit nicht unterstützt wird.

Sie können das native JSON-Format für Slack oder Facebook über das entsprechende Feld in der Antwort des Dialogmodulknotens angeben.

- `output.slack`: Fügen Sie den JSON-Code, den Sie einbeziehen möchten, in das Feld `attachment` der Slack-Antwort ein. Weitere Informationen zum JSON-Format für Slack finden Sie in der [Slack-Dokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://api.slack.com/docs/message-attachments){: new_window}.

- `output.facebook`: Fügen Sie den JSON-Code, den Sie einbeziehen möchten, in das Feld `message.attachment.payload` der Facebook-Antwort ein. Weitere Informationen zum JSON-Format für Facebook finden Sie in der [-Facebook-Dokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: new_window}.

## Antworttypen
{: #dialog-responses-json-response-types}

Die folgenden Antworttypen werden vom generischen JSON-Format unterstützt.

### Bild
{: #dialog-responses-json-image}

Zeigt ein Bild an, das durch eine URL angegeben wird.

#### Felder
{: #{: #dialog-responses-json-image-fields}

| Name          | Typ   | Beschreibung                        | Erforderlich? |
|---------------|--------|------------------------------------|-----------|
| response_type | enum   | `Bild`                            | Ja         |
| source        | string | Die URL für das Bild. Das angegebene Bild muss im Dateifomat .jpg, .gif oder .png vorliegen. | Ja |
| title         | string | Der Titel, der über dem Bild angezeigt werden soll.| Nein         |
| description   | string | Der Text für die zugehörige Beschreibung des Bilds. | Nein |

#### Beispiel
{: #dialog-responses-json-image-example}

In diesem Beispiel wird ein Bild mit Titel und beschreibendem Text angezeigt.

```json
{
  "output":{
    "generic":[
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Beispielbild",
        "description": "Ein Beispielbild, das als Teil einer Multimedia-Antwort zurückgegeben wird."
      }
    ]
  }
}
```

### Option
{: #dialog-responses-json-option}

Zeigt eine Reihe von Schaltflächen oder eine Dropdown-Liste an, über die Benutzer eine Option auswählen können. Der angegebene Wert wird als Benutzereingabe an den Arbeitsbereich gesendet.

#### Felder
{: #dialog-responses-json-option-fields}

| Name          | Typ   | Beschreibung                         | Erforderlich? |
|---------------|--------|-------------------------------------|-----------|
| response_type | enum   | `option`                            | Ja         |
| title         | string | Der Titel, der über den Optionen angezeigt werden soll. | Ja       |
| description   | string | Der Text für die zugehörige Beschreibung der Optionen. | Nein |
| preference    | enum   | Der bevorzugte Steuerelementtyp, der angezeigt werden soll, wenn er von dem Kanal unterstützt wird (`dropdown` oder `button`). Der {{site.data.keyword.conversationshort}}-Connector unterstützt derzeit nur `button`.| Nein |
| options       | list   | Eine Liste der Schlüssel/Wert-Paare für die Optionen, die der Benutzer auswählen kann. | Ja |
| options[].label | string | Die für den Benutzer sichtbare Bezeichnung der Option. | Ja     |
| options[].value | object | Dieses Objekt definiert die Antwort, die an den {{site.data.keyword.conversationshort}}-Service gesendet wird, wenn der Benutzer die Option auswählt. | Ja |
| options[].value.input | object | Dieses Eingabeobjekt enthält den entsprechenden Eingabetext für die Option. | Nein |
| options[].value.input.text | string | Enthält den Text, der für die Option an den Service gesendet wird. | Nein |

#### Beispiel
{: #dialog-responses-json-option-example}

Dieses Beispiel zeigt zwei Optionen an:

- Option 1 (beschriftet mit `Einkaufen`) sendet eine einfache Textnachricht (`Bestellung aufgeben`), die als Eingabetext an den Arbeitsbereich gesendet wird.
- Option 2 (beschriftet mit `Beenden`) sendet eine komplexe Nachricht, die Eingabetext und ein Array mit Absichten enthält. Die Antwort kann jedes Feld enthalten, das ein gültiger Bestandteil für eine {{site.data.keyword.conversationshort}}-Nachricht ist. Weitere Informationen zur Struktur der Nachrichteneingabe finden Sie in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}.

```json
{
  "output":{
    "generic":[
      {
        "response_type": "option",
        "title": "Wähllen Sie eine der folgenden Optionen aus:",
        "preference": "button",
        "options": [
          {
            "label": "Einkaufen",
            "value": {
              "input": {
                "text": "Bestellung aufgeben"
              }
            }
          },
          {
            "label": "Beenden",
      "value": {
              "input": {
                "text": "Beenden"
              },
              "intents": [
                {
                  "intent": "exit_app",
            "confidence": 1.0
          }
              ]
            }
          }
        ]
      }
    ]
  }
}
```

### Pause
{: #dialog-responses-json-pause}

Unterbricht die Verarbeitung, bevor die nächste Nachricht an den Kanal gesendet wird, und sendet optional ein Ereignis 'Benutzer schreibt' (sofern dies vom Kanal unterstützt wird).

#### Felder
{: #dialog-responses-json-pause-fields}

| Name          | Typ   | Beschreibung        | Erforderlich? |
|---------------|--------|--------------------|-----------|
| response_type | enum   | `pause`            | Ja         |
| time          | int    | Die Dauer der Unterbrechung in Millisekunden. | Ja |
| typing        | boolean | Gibt an, ob während der Unterbrechung das Ereignis 'Benutzer schreibt' gesendet werden soll. Wird ignoriert, wenn dieses Ereignis vom Kanal nicht unterstützt wird. | Nein |

#### Beispiel
{: #dialog-responses-json-pause-example}

In diesem Beispiel wird das Ereignis 'Benutzer schreibt' gesendet und die Unterbrechung dauert 5 Sekunden.

```json
{
  "output":{
    "generic":[
      {
        "response_type": "pause",
        "time": 5000,
        "typing": true
      }
    ]
  }
}
```

### Text
{: #dialog-responses-json-text}

Zeigt Text an. Um den Dialog interessanter zu gestalten, können Sie mehrere alternative Textantworten angeben. Wenn Sie mehrere Antworten angeben, besteht die Möglichkeit, die Antworten in der Liste einzeln nacheinander anzuwenden, eine beliebige Antwort nach dem Zufallsprinzip auszuwählen oder alle angegebenen Antworten auszugeben.

#### Felder
{: #dialog-responses-json-text-fields}

| Name          | Typ   | Beschreibung        | Erforderlich? |
|---------------|--------|--------------------|-----------|
| response_type | enum   | `text`             | Ja         |
| values        | list   | Eine Liste mit mindestens einem Objekt, das die Textantwort(en) definiert. | Ja |
| values.[_n_].text   | string | Der Text für eine Antwort. Die Zeichenfolge kann Zeilenvorschubzeichen (`\n`) und Markdown-Tags enthalten, wenn dies vom Kanal unterstützt wird. (Alle vom Kanal nicht unterstützten Formatierungen werden ignoriert.) | Nein |
| selection_policy | string | Gibt an, wie eine Antwort in der Liste ausgewählt wird, wenn mehr als eine Antwort angegeben ist. Mögliche Werte sind `sequential`, `random` und `multiline`. | Nein |
| delimiter     | string | Das Trennzeichen, das als Begrenzer zwischen einzelnen Antworten ausgegeben werden soll. Wird nur verwendet, wenn `selection_policy`=`multiline` angegeben ist. Der Standardbegrenzer ist das Zeilenvorschubzeichen (`\n`). | Nein |

#### Beispiel
{: #dialog-responses-json-text-example}

Dieses Beispiel zeigt eine Begrüßungsnachricht für den Benutzer an.

```json
{
  "output":{
    "generic":[
      {
        "response_type": "text",
        "values": [
          { "text": "Hallo." },
          { "text": "Guten Tag." }
        ],
        "selection_policy": "random"
      }
    ]
  }
}  
```
