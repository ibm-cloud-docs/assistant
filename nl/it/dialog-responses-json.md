---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-11"

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

# Definizione delle risposte utilizzando l'editor JSON
{: #dialog-responses-json}

In alcune situazioni, potresti dover definire le risposte utilizzando l'editor JSON. (Per ulteriori informazioni sulle risposte del dialogo, vedi [Risposte](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)). La modifica del JSON di risposta ti consente di accedere direttamente ai dati che verranno restituiti al canale di comunicazione o all'applicazione personalizzata.

## Formato JSON generico
{: #dialog-responses-json-generic}

Il formato JSON generico per le risposte viene utilizzato per specificare le risposte destinate a qualsiasi canale. Questo formato può soddisfare diversi tipi di risposta supportati dalle integrazioni Slack e Facebook e può anche essere implementato da un'applicazione client personalizzata. (Si tratta del formato utilizzato per impostazione predefinita dalle risposte del dialogo definite utilizzando lo strumento {{site.data.keyword.conversationshort}}.)

Per informazioni su come aprire l'editor JSON per una risposta del nodo di dialogo dallo strumento, vedi [Variabili di contesto nell'editor JSON](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-var-json).

Per specificare una risposta interattiva nel formato JSON generico, inserisci gli oggetti JSON appropriati nel campo `output.generic` della risposta del nodo di dialogo. Il seguente esempio mostra come puoi inviare una risposta contenente più tipi di risposta (testo, un'immagine e opzioni su cui è possibile fare clic):

```json
{
  "output":{
    "generic":[
      {
        "response_type": "text",
        "values": [
          {
            "text": "Here are your nearest stores."
          }
        ]
      },
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Example image",
        "description": "Some description for the image."
      },
      {
        "response_type": "option",
        "title": "Click on one of the following",
        "options": [
          {
            "label": "Location 1",
            "value": {
              "input": {
                "text": "Location 1"
              }
            }
          },
          {
            "label": "Location 2",
            "value": {
              "input": {
                "text": "Location 2"
              }
            }
          },
          {
            "label": "Location 3",
            "value": {
              "input": {
                "text": "Location 3"
              }
            }
          }
        ]
      }
    ]
  }
}
```

Per ulteriori informazioni su come specificare ogni tipo di risposta supportato utilizzando gli oggetti JSON, vedi [Tipi di risposta](#dialog-responses-json-response-types).

Se stai utilizzando il connettore {{site.data.keyword.conversationshort}}, la risposta viene convertita nel runtime nel formato previsto dal canale (Slack o Facebook Messenger). Se la risposta contiene più tipi di elementi multimediali o di allegati, la risposta generica viene convertita in una serie di payload di messaggi separati secondo necessità. Il connettore invia quindi ciascun payload di messaggi al canale in un messaggio separato.

**Nota:** quando una risposta viene suddivisa in più messaggi, il connettore {{site.data.keyword.conversationshort}} invia questi messaggi al canale in sequenza. È responsabilità del canale consegnare questi messaggi all'utente finale; tale operazione può essere influenzata da problemi di rete o del server.

Se stai creando una tua applicazione client, la tua applicazione deve implementare ogni tipo di risposta come appropriato. Per ulteriori informazioni, vedi [Implementazione delle risposte](/docs/services/assistant?topic=assistant-api-dialog-responses).

## Formato JSON nativo
{: #dialog-responses-json-native}

Oltre al formato JSON generico, il JSON del nodo di dialogo supporta anche risposte specifiche del canale scritte utilizzando i formati Slack e Facebook Messenger nativi. Questi formati sono supportati anche dal connettore {{site.data.keyword.conversationshort}}. Potresti voler utilizzare i formati JSON nativi se sai che il tuo spazio di lavoro verrà integrato solo con un tipo di canale e devi specificare un tipo di risposta che attualmente non è supportato dal formato JSON generico.

Puoi specificare il JSON nativo per Slack o Facebook utilizzando il campo appropriato nella risposta del nodo di dialogo:

- `output.slack`: inserisci qualsiasi JSON desideri venga incluso nel campo `attachment` della risposta Slack. Per ulteriori informazioni sul formato JSON Slack, vedi la [documentazione ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://api.slack.com/docs/message-attachments){: new_window} Slack.

- `output.facebook`: inserisci qualsiasi JSON che vuoi includere nel campo `message.attachment.payload` della risposta Facebook. Per ulteriori informazioni sul formato JSON Facebook, vedi la [documentazione ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developers.facebook.com/docs/messenger-platform/send-messages/templates){: new_window} Facebook.

## Tipi di risposta
{: #dialog-responses-json-response-types}

Il formato JSON generico supporta i seguenti tipi di risposta.

### Immagine
{: #dialog-responses-json-image}

Visualizza un'immagine specificata da un URL.

#### Campi
{: #{: #dialog-responses-json-image-fields}

| Nome          | Tipo   | Descrizione                        | Obbligatorio? |
|---------------|--------|------------------------------------|-----------|
| response_type | enum   | `image`                            | S         |
| source        | stringa | L'URL dell'immagine. Il formato dell'immagine specificata deve essere .jpg, .gif o .png. | S |
| title         | stringa | Il titolo da mostrare prima dell'immagine.| N         |
| description   | stringa | Il testo della descrizione che accompagna l'immagine. | N |

#### Esempio
{: #dialog-responses-json-image-example}

Questo esempio visualizza un'immagine con un titolo e un testo descrittivo.

```json
{
  "output":{
    "generic":[
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Example image",
        "description": "An example image returned as part of a multimedia response."
      }
    ]
  }
}
```

### Opzione
{: #dialog-responses-json-option}

Visualizza una serie di pulsanti o un elenco a discesa che gli utenti possono utilizzare per scegliere un'opzione. Il valore specificato viene quindi inviato allo spazio di lavoro come input utente.

#### Campi
{: #dialog-responses-json-option-fields}

| Nome          | Tipo   | Descrizione                         | Obbligatorio? |
|---------------|--------|-------------------------------------|-----------|
| response_type | enum   | `option`                            | S         |
| title         | stringa | Il titolo da mostrare prima delle opzioni. | S       |
| description   | stringa | Il testo della descrizione che accompagna le opzioni. | N |
| preference    | enum   | Il tipo preferito di controllo da visualizzare, se supportato dal canale (`dropdown` o `button`). Il connettore {{site.data.keyword.conversationshort}} al momento supporta solo `button`.| N |
| options       | elenco   | Un elenco delle coppie chiave/valore che specifica le opzioni disponibili per l'utente. | S |
| options[].label | stringa | L'etichetta rivolta all'utente per l'opzione. | S     |
| options[].value | oggetto | Un oggetto che definisce la risposta che verrà inviata al servizio {{site.data.keyword.conversationshort}} se l'utente seleziona l'opzione. | S |
| options[].value.input | oggetto | Un oggetto di input che include il testo di input corrispondente all'opzione. | N |
| options[].value.input.text | stringa | Il testo che verrà inviato al tuo assistente per l'opzione. | N |

#### Esempio
{: #dialog-responses-json-option-example}

Questo esempio visualizza due opzioni:

- L'Opzione 1 (etichettata `Buy something`) invia un messaggio stringa semplice (`Place order`), che viene inviato allo spazio di lavoro come testo di input.
- L'Opzione 2 (etichettata `Exit`) invia un messaggio complesso che include sia il testo di input che un array degli intenti. La risposta può includere qualsiasi campo che costituisca una parte valida di un messaggio {{site.data.keyword.conversationshort}}. (Per ulteriori informazioni sulla struttura dell'input del messaggio, vedi la [Guida di riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}.)

```json
{
  "output":{
    "generic":[
      {
        "response_type": "option",
        "title": "Choose from the following options:",
        "preference": "button",
        "options": [
          {
            "label": "Buy something",
            "value": {
              "input": {
                "text": "Place order"
              }
            }
          },
          {
            "label": "Exit",
      "value": {
              "input": {
                "text": "Exit"
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

### Pausa
{: #dialog-responses-json-pause}

Sospendi prima di inviare il messaggio successivo al canale e, facoltativamente, invia un evento "user is typing" (per i canali che lo supportano).

#### Campi
{: #dialog-responses-json-pause-fields}

| Nome          | Tipo   | Descrizione        | Obbligatorio? |
|---------------|--------|--------------------|-----------|
| response_type | enum   | `pause`            | S         |
| time          | int    | La durata della pausa, in millisecondi. | S |
| typing        | booleano | Se viene inviato l'evento "user is typing" durante la pausa. Ignorato se il canale non supporta questo evento. | N |

#### Esempio
{: #dialog-responses-json-pause-example}

Questo esempio invia l'evento "user is typing" durante una pausa di 5 secondi.

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

### Testo
{: #dialog-responses-json-text}

Visualizza il testo. Per aggiungere varietà, puoi specificare più risposte di testo alternative. Se specifichi più risposte, puoi scegliere di ruotare l'elenco in sequenza, scegliere una risposta in modo casuale oppure generare l'output di tutte le risposte specificate.

#### Campi
{: #dialog-responses-json-text-fields}

| Nome          | Tipo   | Descrizione        | Obbligatorio? |
|---------------|--------|--------------------|-----------|
| response_type | enum   | `text`             | S         |
| values        | elenco   | Un elenco di uno o più oggetti che definiscono la risposta di testo. | S |
| values.[_n_].text   | stringa | Il testo di una risposta. Può includere caratteri di nuova riga (`\n`) e l'inserimento di tag Markdown, se supportato dal canale. (Qualsiasi formattazione non supportata dal canale viene ignorata.) | N |
| selection_policy | stringa | Come viene selezionata una risposta dall'elenco se viene specificata più di una risposta. I valori possibili sono `sequential`, `random` e `multiline`. | N |
| delimiter     | stringa | Il delimitatore nell'output come separatore tra le risposte. Utilizzato solo quando `selection_policy`=`multiline`. Il delimitatore predefinito è la nuova riga (`\n`). | N |

#### Esempio
{: #dialog-responses-json-text-example}

Questo esempio visualizza il messaggio di benvenuto all'utente.

```json
{
  "output":{
    "generic":[
      {
        "response_type": "text",
        "values": [
          { "text": "Hello." },
          { "text": "Hi there." }
        ],
        "selection_policy": "random"
      }
    ]
  }
}  
```
