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

# Antworten bereitstellen
{: #dialog-api-responses}

Ein Dialogmodulknoten kann Antworten an die Benutzer zurückgeben, die Text, Bilder oder interaktive Elemente (z. B. per Mausklick steuerbare Optionen) enthalten. Wenn Sie eine eigene Clientanwendung erstellen, müssen Sie die korrekte Darstellung aller Antworttypen implementieren, die von Ihrem Dialogmodul zurückgegeben werden. (Weitere Informationen zu Dialogmodulantworten enthält der Abschnitt [Antworten](/docs/services/assistant?topic=assistant-dialog-overview#responses).)

## Ausgabeformat der Antwort
{: #dialog-api-responses-output}

Antworten von einem Dialogmodulknoten werden standardmäßig im Objekt `output.generic` der JSON-Antwort angegeben, die von der API '/message' zurückgegeben wird. Das Objekt `generic` enthält ein Array mit bis zu 5 Antwortelementen, die für einen beliebigen Kanal bestimmt sind. Das folgende JSON-Beispiel zeigt eine Antwort, die Text und ein Bild enthält. 

```json
{
  "output":{
    "generic":[
      {
        "response_type": "text",
        "text": "OK, hier ist ein Bild von einem Hund."
      },
      {
        "response_type": "image",
        "source": "http://example.com/dog.jpg"
      }
    ],
    "text" : ["OK, hier ist ein Bild von einem Hund."]
  }
}
```

**Hinweis:** Wie dieses Beispiel zeigt, wird der Antworttext (`OK, hier ist ein Bild von einem Hund.`) auch im Array `output.text` zurückgegeben. Dies geschieht aus Gründen der Abwärtskompatibilität für Anwendungen, die das Format `output.generic` nicht unterstützen.

Ihre Clientanwendung ist dafür verantwortlich, alle Antworttypen ordnungsgemäß zu verarbeiten. Im vorliegenden Beispiel muss Ihre Anwendung den angegebenen Text und das angegebene Bild dem Benutzer anzeigen.

## Antworttypen
{: #dialog-api-responses-types}

Jedes Element einer Antwort weist einen der unterstützten Antworttypen auf  (derzeit sind dies `image`, `option`, `pause` und `text`). Jeder Antworttyp wird durch eine andere Kombination von JSON-Eigenschaften angegeben. Dies bedeutet: die Eigenschaften für jede Antwort variieren je nach Antworttyp. Ausführliche Informationen zum Antwortmodell der API '/message' finden Sie in der [API-Referenz ![Symbol für externen Link](../../icons/launch-glyph.svg "Symbol für externen Link")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}.

In diesem Abschnitt werden die verfügbaren Antworttypen und ihre Darstellung in der JSON-Antwort der API '/message' beschrieben. (Wenn Sie das Watson-SDK verwenden, können Sie die für Ihre Sprache bereitgestellten Schnittstellen verwenden, um auf dieselben Objekte zuzugreifen. 

**Hinweis:** Die Beispiele in diesem Abschnitt veranschaulichen das Format der JSON-Daten, die von der API '/message' währen der Laufzeit zurückgegeben werden. Beachten Sie, dass sich dieses Format von dem JSON-Format zum Definieren von Antworten in einem Dialogmodulknoten unterscheidet. Weitere Informationen enthält der Abschnitt [Antworten mit dem JSON-Editor definieren](/docs/services/assistant?topic=assistant-dialog-responses-json).

### Bild
{: #dialog-api-responses-image}

Der Antworttyp `image` weist die Clientanwendung an, ein Bild (optional mit Titel und Beschreibung) anzuzeigen: 

```json
{
  "output":{
    "generic":[
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Bildbeispiel",
        "description": "Dies ist ein Bildbeispiel"
      }
    ]
  }
}
```

Ihre Anwendung ist dafür verantwortlich, das in der Eigenschaft `source` angegebene Bild abzurufen und dem Benutzer anzuzeigen. Wenn die optionalen Angaben `title` und `description` vorhanden sind, können Sie von Ihren Anwendung auf jede beliebige, geeignete Art angezeigt werden (z. B. kann der Titel unterhalb des Bilds und die Beschreibung als Kurzinfotext dargestellt werden).

### Option
{: #dialog-api-responses-option}

Der Antworttyp `option` weist die Clientanwendung an, ein Steuerelement der Benutzerschnittstelle anzuzeigen, das dem Benutzer ermöglicht, aus einer Liste mit Optionen auszuwählen und die Eingabe anschließend gemäß der ausgewählten Option an den {{site.data.keyword.conversationshort}}-Service zurückzugeben.

```json
{
  "output":{
    "generic":[
      {
        "response_type": "option",
        "title": "Verfügbare Optionen",
        "description": "Wählen Sie bitte eine der folgenden Optionen aus:",
        "preference": "button",
        "options": [
          {
            "label": "Option 1",
            "value": {
              "input": {
                "text": "Option 1"
              }
            }
          },
          {
            "label": "Option 2",
            "value": {
              "input": {
                "text": "Option 2"
              }
            }
          }
        ]
      }
    ]
  }
}
```

In Ihrer App können die angegebenen Optionen durch ein beliebiges Steuerelement der Benutzerschnittstelle (z. B. durch eine Reihe von Schaltflächen oder eine Dropdown-Liste) dargestellt werden. Die optionale Eigenschaft `preference` gibt den bevorzugten Steuerelementtyp (`button` oder `dropdown`) an, der von Ihrer App verwendet werden soll (falls unterstützt). 

Die Eigenschaft `label` gibt für jede Option die Beschriftung an, die im Steuerelement der Benutzerschnittstelle angezeigt werden soll. Die Eigenschaft `value` gibt die Eingabe an, die an den {{site.data.keyword.conversationshort}}-Service zurückgegeben werden soll (mithilfe der API '/message'), wenn der Benutzer die betreffende Option auswählt. Beachten Sie, dass die Eigenschaft `value` nicht nur Text, sondern auch andere Eingabeobjekte wie Absichten und Entitäten enthalten kann, die ebenfalls an den Service gesendet werden sollen.

### Pause
{: #dialog-api-responses-pause}

Der Antworttyp `pause` weist die Anwendung an, eine angegebene Zeit lang zu warten, bevor die nächste Antwort angezeigt wird.

```json
{
  "output":{
    "generic":[
      {
        "response_type": "pause",
        "time": 500,
        "typing": false
      }
    ]
  }
}
```

Diese Pause (Wartezeit) kann vom Dialogmodul angefordert werden, damit eine Anfrage abgeschlossen werden kann oder um das Verhalten eines Servicemitarbeiters nachzuahmen, der nicht immer sofort antwortet. Für die Pause kann eine beliebige Dauer bis zu 10 Sekunden festgelegt werden. 

Der Antworttyp `pause` wird normalerweise in Kombination mit anderen Antworten gesendet. Ihre Anwendung wird für den in der Eigenschaft `time` (in Millisekunden) angegebenen Zeitraum angehalten, bevor die nächste Antwort in dem Array angezeigt wird. Die optionale Eigenschaft `typing` gibt an, dass die Clientanwendung einen Hinweis "Benutzer schreibt" anzeigen soll (sofern dies unterstützt wird), um das Verhalten eines Servicemitarbeiters nachzuahmen. 

### Text
{: #dialog-api-responses-text}

Der Antworttyp `text` wird für normale Textantworten des Dialogmoduls verwendet. 

```json
{
  "output":{
    "generic":[
      {
        "response_type": "text",
        "text": "In Ordnung, Sie möchten nächsten Montag nach Boston fliegen."
      }
    ]
  }
}
```

Beachten Sie Folgendes: Aus Gründen der Abwärtskompatibilität wird der gleiche Text auch in das Array `output.text` der Dialogmodulantwort eingefügt (wie in dem Beispiel am Anfang dieser Seite gezeigt).
