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

# Multimedia-Antworten

Wenn Ihr Arbeitsbereich über den [{{site.data.keyword.conversationshort}}-Connector](conversation-connector.html) in Slack oder Facebook Messenger integriert ist, können Sie Antworten für das Dialogmodul angeben, die Multimedia-Elemente oder interaktive Elemente wie per Mausklick steuerbare Schaltflächen einbeziehen. Um eine interaktive Antwort anzugeben, fügen Sie einen Block mit JSON-Daten in die Ausgabe eines Dialogmodulknotens ein.

**Hinweis:** Wenn Sie interaktive Nachrichten mit Slack verwenden möchten, stellen Sie sicher, dass die Unterstützung für interaktive Nachrichten aktiviert ist. Weitere Informationen dazu finden Sie in der [README-Datei der Slack-Bereitstellung ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://github.com/watson-developer-cloud/conversation-connector/blob/master/channels/slack/README.md#interactive-messages){: new_window}.

## Generisches JSON-Format

Interaktive Antworten können in einem generischen JSON-Format angegeben werden, das entweder Slack- oder Facebook Messenger-Bereitstellungen unterstützt. Um eine interaktive Antwort im generischen JSON-Format anzugeben, fügen Sie den JSON-Code in das Feld `output.generic` in der Antwort des Dialogmodulknotens ein. Das folgende Beispiel zeigt, wie Sie eine Antwort senden können, die mehrere Antworttypen (Text, ein Bild und per Mausklick steuerbare Optionen) enthält):

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "Dies sind die Filialen in Ihrer Nähe."
      },
      {
        "response_type": "image",
        "source": "http://...",
        "title": "Titel des Bilds",
        "description": "Eine Beschreibung des Bilds"
      },
      {
        "response_type": "option",
        "title": "Klicken Sie auf eine der folgenden Optionen",
        "options": [
          {
            "label": "Standort 1",
            "value:" "Standort 1"
          },
          {
            "label": "Standort 2",
            "value:" "Standort 2"
          },
          {
            "label": "Standort 3",
            "value:" "Standort 3"
          }
        ]
      }
    ]
  }
}
```

Weitere Informationen zu den unterstützten Antworttypen und zum Angeben dieser Typen finden Sie in [Antworttypen](#response-types).

Während der Laufzeit wandelt der {{site.data.keyword.conversationshort}}-Connector diese Antwort in das vom Kanal (Slack oder Facebook Messenger) erwartete Format um. Wenn die Antwort mehrere Medientypen oder Anhänge enthält, wird die generische Antwort in eine Serie separater Nachrichtennutzdaten umgewandelt. Anschließend sendet der Connector die einzelnen Nachrichtennutzdaten in einer separaten Nachricht an den Kanal.

**Hinweis:** Wenn eine Antwort in mehrere Nachrichten aufgeteilt wird, sendet der {{site.data.keyword.conversationshort}}-Connector die Nachrichten nacheinander an den Kanal. Der Kanal ist dafür zuständig, diese Nachrichten an den Endbenutzer zu übermitteln (dies kann durch Netz- oder Serverprobleme beeinträchtigt werden).

## Natives JSON-Format

Neben dem generischen JSON-Format unterstützt der {{site.data.keyword.conversationshort}}-Connector auch kanalspezifische Antworten, die in den nativen Formaten für Slack und Facebook Messenger erstellt werden. Möglicherweise möchten Sie die nativen JSON-Formate verwenden, wenn Sie einen Antworttyp angeben müssen, der vom generischen JSON-Format derzeit nicht unterstützt wird.

Sie können das native JSON-Format für Slack oder Facebook über das entsprechende Feld in der Antwort des Dialogmodulknotens angeben.

- `output.slack`: Fügen Sie den JSON-Code, den Sie einbeziehen möchten, in das Feld `attachment` der Slack-Antwort ein. Weitere Informationen zum JSON-Format für Slack finden Sie in der [Slack-Dokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://api.slack.com/docs/message-attachments){: new_window}.

- `output.facebook`: Fügen Sie den JSON-Code, den Sie einbeziehen möchten, in das Feld `message.attachment.payload` der Facebook-Antwort ein. Weitere Informationen zum JSON-Format für Facebook finden Sie in der [-Facebook-Dokumentation ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: new_window}.

## Antworttypen

Die folgenden Antworttypen werden vom generischen JSON-Format unterstützt.

### Bild

Zeigt ein Bild an, das durch eine URL angegeben wird.

#### Felder

| Name          | Typ   | Beschreibung                        | Erforderlich? |
|---------------|--------|------------------------------------|-----------|
| response_type | enum   | `Bild`                            | Ja         |
| source        | string | Die URL für das Bild. Das angegebene Bild muss im Dateifomat .jpg, .gif oder .png vorliegen. | Ja |
| title         | string | Der Titel, der über dem Bild angezeigt werden soll.| Nein         |
| description   | string | Der Text für die zugehörige Beschreibung des Bilds. | Nein |

#### Beispiel

In diesem Beispiel wird ein Bild mit Titel und beschreibendem Text angezeigt.

```json
{
  "response_type": "image",
  "source": "http://example.com/image.jpg",
  "title": "Beispielbild",
  "description": "Ein Beispielbild, das als Teil einer Multimedia-Antwort zurückgegeben wird."
}
```

### Option

Zeigt eine Reihe von Schaltflächen an, die Benutzer anklicken können, um eine Option auszuwählen. Der angegebene Wert wird als Benutzereingabe an den Arbeitsbereich gesendet.

#### Felder

| Name          | Typ   | Beschreibung                         | Erforderlich? |
|---------------|--------|-------------------------------------|-----------|
| response_type | enum   | `option`                            | Ja         |
| title         | string | Der Titel, der über den Optionen angezeigt werden soll. | Ja       |
| description   | string | Der Text für die zugehörige Beschreibung der Optionen. | Nein |
| options       | list   | Eine Liste der Schlüssel/Wert-Paare für die Optionen, die der Benutzer auswählen kann. | Ja |
| options[].label | string | Die für den Benutzer sichtbare Bezeichnung der Option. | Ja     |
| options[].value | string<br/>object | Dieser Wert wird an den {{site.data.keyword.conversationshort}}-Service gesendet, wenn der Benutzer die Option auswählt. Dies kann eine Zeichenfolge (für einfache Antworten) sein oder ein Objekt, das mehrere Felder enthält. Beispiele siehe unten. | Ja |

#### Beispiel

Dieses Beispiel zeigt zwei Optionen an:

- Option 1 (Bezeichnung 'Einkaufen') sendet eine einfache Textnachricht (`option_1`), die über das Feld `input.text` der Nachricht an den Arbeitsbereich gesendet wird.
- Option 2 (mit der Bezeichnung 'Beenden') sendet eine komplexe Textnachricht, die den Eingabetext und ein Array mit Absichten enthält. Die Antwort kann jedes Feld enthalten, das ein gültiger Bestandteil für eine {{site.data.keyword.conversationshort}}-Nachricht ist. Weitere Informationen zur Struktur der Nachrichteneingabe finden Sie in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://www.ibm.com/watson/developercloud/conversation/api/v1/?curl#send_message){: new_window}.

```json
{
  "response_type": "option",
  "title": "Wählen Sie eine der folgenden Optionen aus:",
  "options": [
    {
      "label": "Einkaufen",
      "value": "Bestellung aufgeben"
    },
    {
      "label": "Beenden",
      "value": {
        "input": {
          "text": "Beenden"
        },
        "intent": [
          {
            "intent": "exit_app",
            "confidence": 1.0
          }
        ]
      }
    }
  ]
}
```

### Text

Zeigt Text an.

#### Felder

| Name          | Typ   | Beschreibung        | Erforderlich? |
|---------------|--------|--------------------|-----------|
| response_type | enum   | `text`             | Ja         |
| text          | string | Der Text, der angezeigt werden soll. Die Zeichenfolge kann Zeilenvorschubzeichen (`\n`) und Markdown-Tags enthalten, wenn dies vom Kanal unterstützt wird. (Alle vom Kanal nicht unterstützten Formatierungen werden ignoriert.)| Ja |

#### Beispiel

Dieses Beispiel zeigt eine Textnachricht für den Benutzer an.

```json
{
  "response_type": "text",
  "text": "Dies ist ein Antworttext."
}
```
