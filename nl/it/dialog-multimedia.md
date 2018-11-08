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

# Risposte multimediali

Se il tuo spazio di lavoro è integrato con Slack o Facebook Messenger utilizzando il [connettore {{site.data.keyword.conversationshort}}](conversation-connector.html), puoi specificare risposte del nodo di dialogo che includono elementi multimediali o interattivi come ad esempio i pulsanti su cui è possibile fare clic. Per specificare una risposta interattiva, inserisci un blocco di dati JSON nell'output di un nodo di dialogo.

**Nota:** se desideri utilizzare messaggi interattivi con Slack, assicurati di aver abilitato il supporto dei messaggi interattivi. Per ulteriori informazioni, vedi il [README ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/watson-developer-cloud/conversation-connector/blob/master/channels/slack/README.md#interactive-messages){: new_window} di distribuzione Slack.

## Formato JSON generico

Puoi specificare le risposte interattive utilizzando un formato JSON generico che supporta le distribuzioni Slack o Facebook Messenger. Per specificare una risposta interattiva nel formato JSON generico, inserisci JSON nel campo `output.generic` della risposta del nodo di dialogo. Il seguente esempio mostra come puoi inviare una risposta contenente più tipi di risposta (testo, un'immagine e opzioni su cui è possibile fare clic):

```json
{
  "output":{
    "generic":[
      {
        "response_type": "text",
        "text": "Here are your nearest stores."
      },
      {
        "response_type": "image",
        "source": "http://...",
        "title": "Image title",
        "description": "Some description for the image"
      },
      {
        "response_type": "option",
        "title": "Click on one of the following",
        "options": [
          {
            "label": "Location 1",
            "value:" "Location 1"
          },
          {
            "label": "Location 2",
            "value:" "Location 2"
          },
          {
            "label": "Location 3",
            "value:" "Location 3"
          }
        ]
      }
    ]
  }
}
```

Per ulteriori informazioni sui tipi di risposta supportati e su come specificarli, vedi [Tipi di risposta](#response-types).

Nel runtime, il connettore {{site.data.keyword.conversationshort}} converte questa risposta nel formato previsto dal canale (Slack o Facebook Messenger). Se la risposta contiene più tipi di elementi multimediali o di allegati, la risposta generica viene convertita in una serie di payload di messaggi separati. Il connettore invia quindi ciascun payload di messaggi al canale in un messaggio separato. 

**Nota:** quando una risposta viene suddivisa in più messaggi, il connettore {{site.data.keyword.conversationshort}} invia questi messaggi al canale in sequenza. È responsabilità del canale consegnare questi messaggi all'utente finale; tale operazione può essere influenzata da problemi di rete o del server. 

## Formato JSON nativo

Oltre al formato JSON generico, il connettore {{site.data.keyword.conversationshort}} supporta anche risposte specifiche del canale scritte utilizzando i formati Slack e Facebook Messenger nativi. Potresti voler utilizzare i formati JSON nativi se devi specificare un tipo di risposta che attualmente non è supportato dal formato JSON generico.

Puoi specificare il JSON nativo per Slack o Facebook utilizzando il campo appropriato nella risposta del nodo di dialogo:

- `output.slack`: inserisci qualsiasi JSON che vuoi includere nel campo `attachment` della risposta Slack. Per ulteriori informazioni sul formato JSON Slack, vedi la [documentazione ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://api.slack.com/docs/message-attachments){: new_window} Slack.

- `output.facebook`: inserisci qualsiasi JSON che vuoi includere nel campo `message.attachment.payload` della risposta Facebook. Per ulteriori informazioni sul formato JSON Facebook, vedi la [documentazione ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: new_window} Facebook.

## Tipi di risposta

Il formato JSON generico supporta i seguenti tipi di risposta.

### Immagine

Visualizza un'immagine specificata da un URL.

#### Campi

| Nome          | Tipo   | Descrizione                        | Obbligatorio? |
|---------------|--------|------------------------------------|-----------|
| response_type | enum   | `image`                            | S         |
| source        | stringa | L'URL dell'immagine. Il formato dell'immagine specificata deve essere .jpg, .gif o .png. | S |
| title         | stringa | Il titolo da mostrare prima dell'immagine.| N         |
| description   | stringa | Il testo della descrizione che accompagna l'immagine. | N |

#### Esempio

Questo esempio visualizza un'immagine con un titolo e un testo descrittivo. 

```json
{
  "response_type": "image",
  "source": "http://example.com/image.jpg",
  "title": "Example image",
  "description": "An example image returned as part of a multimedia response."
}
```

### Opzione

Visualizza una serie di pulsanti su cui gli utenti possono fare clic per scegliere un'opzione. Il valore specificato viene quindi inviato allo spazio di lavoro come input utente. 

#### Campi

| Nome          | Tipo   | Descrizione                         | Obbligatorio? |
|---------------|--------|-------------------------------------|-----------|
| response_type | enum   | `option`                            | S         |
| title         | stringa | Il titolo da mostrare prima delle opzioni. | S         |
| description   | stringa | Il testo della descrizione che accompagna le opzioni. | N |
| options       | elenco   | Un elenco delle coppie chiave/valore che specifica le opzioni disponibili per l'utente. | S |
| options[].label | stringa | L'etichetta rivolta all'utente per l'opzione. | S     |
| options[].value | stringa<br/>oggetto | Il valore che verrà inviato al servizio {{site.data.keyword.conversationshort}} se l'utente seleziona l'opzione. Può essere una stringa (per risposte semplici) o un oggetto contenente più campi. Vedi di seguito per gli esempi. | S |

#### Esempio

Questo esempio visualizza due opzioni:

- L'Opzione 1 (denominata 'Buy something') invia un messaggio stringa semplice (`option_1`), che viene inviato allo spazio di lavoro utilizzando il campo `input.text` del messaggio.
- L'Opzione 2 (denominata 'Exit') invia un messaggio complesso che include sia testo di input che un array di intenti. La risposta può includere qualsiasi campo che costituisca una parte valida di un messaggio {{site.data.keyword.conversationshort}}. (Per ulteriori informazioni sulla struttura dell'input del messaggio, vedi [Riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/watson/developercloud/conversation/api/v1/?curl#send_message){: new_window}.)

```json
{
  "response_type": "option",
  "title": "Choose from the following options:",
  "options": [
    {
      "label": "Buy something",
      "value": "Place order"
    },
    {
      "label": "Exit",
      "value": {
        "input": {
          "text": "Exit"
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

### Testo

Visualizza il testo.

#### Campi

| Nome          | Tipo   | Descrizione        | Obbligatorio? |
|---------------|--------|--------------------|-----------|
| response_type | enum   | `text`             | S         |
| text          | stringa | Il testo da visualizzare. Può includere caratteri di nuova riga (`\n`) e l'inserimento di tag Markdown, se supportato dal canale. (Qualsiasi formattazione non supportata dal canale viene ignorata.) | S |

#### Esempio

Questo esempio visualizza un messaggio di testo all'utente. 

```json
{
  "response_type": "text",
  "text": "This is a text response."
}
```
