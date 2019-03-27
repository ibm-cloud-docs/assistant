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

# Implementazione delle risposte
{: #dialog-api-responses}

Un nodo di dialogo può rispondere agli utenti con una risposta che include testo, immagini o elementi interattivi come le opzioni su cui è possibile fare clic. Se stai creando una tua applicazione client, devi implementare la visualizzazione corretta di tutti i tipi di risposta restituiti dal tuo dialogo. (Per ulteriori informazioni sulle risposte del dialogo, vedi [Risposte](/docs/services/assistant?topic=assistant-dialog-overview#responses)).

## Formato di output della risposta
{: #dialog-api-responses-output}

Per impostazione predefinita, le risposte da un nodo di dialogo vengono specificate nell'oggetto `output.generic` nel JSON di risposta restituito dall'API /message. L'oggetto `generic` contiene un array di massimo 5 elementi di risposta destinati a qualsiasi canale. Il seguente esempio di JSON mostra una risposta che include del testo e un'immagine: 

```json
{
  "output":{
    "generic":[
      {
        "response_type": "text",
        "text": "OK, here's a picture of a dog."
      },
      {
        "response_type": "image",
        "source": "http://example.com/dog.jpg"
      }
    ],
    "text" : ["OK, here's a picture of a dog."]
  }
}
```

**Nota:** come mostra questo esempio, la risposta del testo (`OK, here's a picture of a dog.`) viene restituita anche nell'array `output.text`. Viene inclusa per una compatibilità con le versioni precedenti per le applicazioni che non supportano il formato `output.generic`.

È responsabilità della tua applicazione client gestire tutti i tipi di risposta in modo corretto. In questo caso, la tua applicazione deve visualizzare il testo specificato e l'immagine all'utente. 

## Tipi di risposta
{: #dialog-api-responses-types}

Ciascun elemento di una risposta è di uno dei tipi di risposta supportati (attualmente `image`, `option`, `pause` e `text`). Ciascun tipo di risposta viene specificato utilizzando una serie diversa di proprietà JSON, quindi le proprietà incluse per ciascuna risposta varieranno in base al tipo di risposta. Per informazioni complete sul modello di risposta dell'API /message, vedi il [Riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}.)

Questa sezione descrive i tipi di risposta disponibili e il modo in cui sono rappresentati nel JSON di risposta dell'API /message. (Se stai utilizzando l'SDK Watson, puoi utilizzare le interfacce fornite per la tua lingua per accedere agli stessi oggetti.)

**Nota:** gli esempi in questa sezione mostrano il formato dei dati JSON restituiti dall'API /message nel runtime. Tieni presente che è diverso dal formato JSON utilizzato per definire le risposte all'interno di un nodo di dialogo. Per ulteriori informazioni, vedi [Definizione delle risposte utilizzando l'editor JSON](/docs/services/assistant?topic=assistant-dialog-responses-json).

### Immagine
{: #dialog-api-responses-image}

Il tipo di risposta `image` indica all'applicazione client di visualizzare un'immagine, accompagnata, facoltativamente, da un titolo e da una descrizione:

```json
{
  "output":{
    "generic":[
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Image example",
        "description": "This is an example image"
      }
    ]
  }
}
```

La tua applicazione è responsabile del richiamo dell'immagine specificata dalla proprietà `source` e della sua visualizzazione all'utente. Se vengono forniti un `titolo` e una `descrizione` facoltativi, la tua applicazione può visualizzarli in qualsiasi modo sia appropriato (ad esempio, eseguendo il rendering del titolo sotto l'immagine e della descrizione come testo a comparsa).

### Opzione
{: #dialog-api-responses-option}

Il tipo di risposta `option` indica all'applicazione client di visualizzare un controllo dell'interfaccia utente che consenta all'utente di selezionare da un elenco di opzioni e quindi reinviare l'input al servizio {{site.data.keyword.conversationshort}} in base all'opzione selezionata: 

```json
{
  "output":{
    "generic":[
      {
        "response_type": "option",
        "title": "Available options",
        "description": "Please select one of the following options:",
        "preference": "button",
        "options": [
          {
            "label": "Option 1",
            "value": {
              "input": {
                "text": "option 1"
              }
            }
          },
          {
            "label": "Option 2",
            "value": {
              "input": {
                "text": "option 2"
              }
            }
          }
        ]
      }
    ]
  }
}
```

La tua applicazione può visualizzare le opzioni specificate utilizzando qualsiasi controllo utente adatto (ad esempio, una serie di pulsanti o un elenco a discesa). La proprietà `preference` facoltativa indica il tipo di controllo preferito che deve essere utilizzato dalla tua applicazione (`button` o `dropdown`), se supportato.

Per ciascuna opzione, la proprietà `label` specifica il testo dell'etichetta che deve comparire per l'opzione nel controllo IU. La proprietà `value` specifica l'input che deve essere reinviato al servizio {{site.data.keyword.conversationshort}} (utilizzando l'API /message) quando l'utente seleziona l'opzione corrispondente. Tieni presente che la proprietà `value` può includere non solo l'input del testo ma anche altri oggetti di input, come ad esempio gli intenti e le entità, che devono essere tutti inviati al servizio. 

### Pausa
{: #dialog-api-responses-pause}

Il tipo di risposta `pause` indica all'applicazione di attendere un determinato intervallo prima di visualizzare la risposta successiva: 

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

Questa pausa potrebbe essere richiesta dal dialogo per consentire il completamento di una richiesta oppure semplicemente per simulare la comparsa di un operatore che potrebbe applicare delle pause tra le risposte. La durata massima della pausa può essere di 10 secondi. 

Di norma, una risposta `pause` viene inviata insieme ad altre risposte. La tua applicazione deve rimanere in pausa per l'intervallo specificato dalla proprietà `time` (in millisecondi) prima di visualizzare la risposta successiva nell'array. La proprietà `typing` facoltativa richiede che l'applicazione client mostri un indicatore "user is typing", se supportato, al fine di simulare un operatore.

### Testo
{: #dialog-api-responses-text}

Il tipo di risposta `text` viene utilizzato per le risposte di testo ordinarie provenienti dal dialogo:

```json
{
  "output":{
    "generic":[
      {
        "response_type": "text",
        "text": "OK, you want to fly to Boston next Monday."
      }
    ]
  }
}
```

Tieni presente che per la compatibilità con le versioni precedenti, lo stesso testo viene incluso anche nell'array `output.text` nella risposta del dialogo (come mostrato nell'esempio all'inizio di questa pagina).
