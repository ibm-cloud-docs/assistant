---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-14"

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
{: #api-dialog-responses}

Un nodo di dialogo può rispondere agli utenti con una risposta che include testo, immagini o elementi interattivi come le opzioni su cui è possibile fare clic. Se stai creando una tua applicazione client, devi implementare la visualizzazione corretta di tutti i tipi di risposta restituiti dal tuo dialogo. (Per ulteriori informazioni sulle risposte del dialogo, vedi [Risposte](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)).

## Formato di output della risposta
{: #api-dialog-responses-output}

Per impostazione predefinita, le risposte da un nodo di dialogo vengono specificate nell'oggetto `output.generic` nel JSON di risposta restituito dall'API `/message`. L'oggetto `generic` contiene un array di massimo 5 elementi di risposta destinati a qualsiasi canale. Il seguente esempio di JSON mostra una risposta che include del testo e un'immagine:

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

Come mostra questo esempio, la risposta di testo (`OK, here's a picture of a dog.`) viene restituita anche nell'array `output.text`. Viene inclusa per una compatibilità con le versioni precedenti per le applicazioni più vecchie che non supportano il formato `output.generic`.
{: note}

È responsabilità della tua applicazione client gestire tutti i tipi di risposta in modo corretto. In questo caso, la tua applicazione deve visualizzare il testo specificato e l'immagine all'utente.

## Tipi di risposta
{: #api-dialog-responses-types}

Ciascun elemento di una risposta è di uno dei tipi di risposta supportati (attualmente `image`, `option`, `pause`, `text` e `suggestion`). Ciascun tipo di risposta viene specificato utilizzando una serie diversa di proprietà JSON, quindi le proprietà incluse per ciascuna risposta varieranno in base al tipo di risposta. Per informazioni complete sul modello di risposta dell'API `/message`, vedi la [Guida di riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}.)

Questa sezione descrive i tipi di risposta disponibili e il modo in cui sono rappresentati nel JSON di risposta dell'API `/message`. (Se stai utilizzando l'SDK Watson, puoi utilizzare le interfacce fornite per la tua lingua per accedere agli stessi oggetti.)

Gli esempi in questa sezione mostrano il formato dei dati JSON restituiti dall'API `/message API` nel runtime. Tieni presente che è diverso dal formato JSON utilizzato per definire le risposte all'interno di un nodo di dialogo. Per ulteriori informazioni, vedi [Definizione delle risposte utilizzando l'editor JSON](/docs/services/assistant?topic=assistant-dialog-responses-json)).
{: note}

### Testo
{: #api-dialog-responses-text}

Il tipo di risposta `text` (testo) viene utilizzato per le risposte di testo ordinarie provenienti dal dialogo:

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

### Immagine
{: #api-dialog-responses-image}

Il tipo di risposta `image` (immagine) indica all'applicazione client di visualizzare un'immagine, accompagnata, facoltativamente, da un titolo e da una descrizione:

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

### Pausa
{: #api-dialog-responses-pause}

Il tipo di risposta `pause` (pausa) indica all'applicazione di attendere un determinato intervallo prima di visualizzare la risposta successiva:

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

### Opzione
{: #api-dialog-responses-option}

Il tipo di risposta `option` (option) indica all'applicazione client di visualizzare un controllo dell'interfaccia utente che consenta all'utente di selezionare da un elenco di opzioni e quindi reinviare l'input all'assistente in base all'opzione selezionata:

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

La tua applicazione può visualizzare le opzioni specificate utilizzando qualsiasi controllo utente adatto (ad esempio, una serie di pulsanti o un elenco a discesa). La proprietà `preference` facoltativa indica il tipo di controllo preferito che deve essere utilizzato dalla tua applicazione (`button` o `dropdown`), se supportato. Per la miglior esperienza dell'utente, ti consigliamo di presentare massimo tre opzioni come pulsanti e più di tre opzioni come un elenco a discesa. 

Per ciascuna opzione, la proprietà `label` specifica il testo dell'etichetta che deve comparire per l'opzione nel controllo IU. La proprietà `value` specifica l'input che deve essere reinviato all'assistente (utilizzando l'API `/message`) quando l'utente seleziona l'opzione corrispondente. 

Per un esempio di implementazione delle risposte `option` in un'applicazione client semplice, vedi [Esempio: implementazione delle risposte option](#api-dialog-option-example).

### Suggerimento
{: #api-dialog-responses-suggestion}

Questa funzione è disponibile solo per gli utenti con piano Plus o Premium.
{: tip}

Il tipo di risposta `suggestion` (suggerimento) viene utilizzato dalla funzione di disambiguazione per suggerire possibili corrispondenze quando non è chiaro cosa vuole fare l'utente. Una risposta `suggestion` include un array di suggerimenti (`suggestions`), ciascuno dei quali corrisponde a un possibile nodo di dialogo corrispondente:

```json
{
  "output":{
    "generic":[
      {
        "response_type": "suggestion",
        "title": "Did you mean:",
        "suggestions": [
          {
            "label": "I'd like to order a drink.",
            "value": {
              "intents": [
                {
                  "intent": "order_drink",
                  "confidence": 0.7330395221710206
                }
              ],
              "entities": [],
              "input": {
                "suggestion_id": "576aba3c-85b9-411a-8032-28af2ba95b13",
                "text": "I want to place an order"
              }
            },
  "output": {
              "text": [
                "I'll get you a drink."
              ],
              "generic": [
                {
                  "response_type": "text",
                  "text": "I'll get you a drink."
                }
              ],
              "nodes_visited_details": [
                {
                  "dialog_node": "node_1_1547675028546",
                  "title": "order drink",
                  "user_label": "I'd like to order a drink.",
                  "conditions": "#order_drink"
                }
              ]
            },
            "source_dialog_node": "root"
          },
          {
            "label": "I need a drink refill.",
            "value": {
              "intents": [
                {
                  "intent": "refill_drink",
                  "confidence": 0.2529746770858765
                }
              ],
              "entities": [],
              "input": {
                "suggestion_id": "6583b547-53ff-4e7b-97c6-4d062270abcd",
                "text": "I need a drink refill"
              }
            },
  "output": {
              "text": [
                "I'll get you a refill."
              ],
              "generic": [
                {
                  "response_type": "text",
                  "text": "I'll get you a refill."
                }
              ],
              "nodes_visited_details": [
                {
                  "dialog_node": "node_2_1547675097178",
                  "title": "refill drink",
                  "user_label": "I need a drink refill.",
                  "conditions": "#refill_drink"
                }
              ]
            },
            "source_dialog_node": "root"
          }
        ]
      }
    ],
  }
}
```

Tieni presente che la struttura di una risposta `suggestion` è molto simile a quella di una risposta `option`. Come per le opzioni, ogni suggerimento include un'etichetta (`label`) che può essere mostrata all'utente e un valore (`value`) che specifica che l'input deve essere reinviato all'assistente se l'utente sceglie il suggerimento corrispondente. Per implementare le risposte `suggestion` nella tua applicazione, puoi utilizzare lo stesso approccio che utilizzeresti per le risposte `option`.

Per ulteriori informazioni sulla funzione di disambiguazione, vedi [Disambiguazione](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).

## Esempio: implementazione delle risposte option
{: #api-dialog-option-example}

Per mostrare in che modo un'applicazione client potrebbe gestire le risposte option, che richiedono all'utente di effettuare una selezione da un elenco di scelte, possiamo ampliare l'esempio client descritto in [Creazione di un'applicazione client](/docs/services/assistant?topic=assistant-api-client). Questa è un'applicazione client semplificata che utilizza l'input e l'output standard per gestire tre intenti (invio di un messaggio iniziale, visualizzazione dell'ora corrente e uscita dall'applicazione):

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

Se vuoi provare l'esempio di codice mostrato in questo argomento, devi innanzitutto configurare lo spazio di lavoro necessario e ottenere i dettagli API di cui avrai bisogno. Per ulteriori informazioni, vedi [Creazione di un'applicazione client](/docs/services/assistant?topic=assistant-api-client).
{: note}

### Ricezione di una risposta option

La risposta `option` può essere utilizzata quando vuoi presentare all'utente un elenco finito di scelte piuttosto che interpretare l'input del linguaggio naturale. Può essere utilizzata in qualsiasi situazione in cui vuoi consentire all'utente di effettuare velocemente una selezione da una serie di opzioni inequivocabili. 

Nella nostra applicazione client semplificata, utilizzeremo questa capacità per effettuare una selezione da un elenco delle azioni supportate dall'assistente (messaggi iniziali, visualizzazione dell'ora e uscita). Oltre ai tre intenti mostrati in precedenza (`#hello`, `#time` e `#goodbye`), lo spazio di lavoro di esempio supporta un quarto intento: `#menu`, che viene soddisfatto quando l'utente chiede di vedere un elenco delle azioni disponibili. 

Quando lo spazio di lavoro riconosce l'intento `#menu`, il dialogo risponde con una risposta `option`:

```json
{
  "output":{
    "generic":[
      {
        "title": "What do you want to do?",
        "options": [
          {
            "label": "Send greeting",
            "value": {
              "input": {
                "text": "hello"
              }
            }
          },
          {
            "label": "Display the local time",
            "value": {
              "input": {
                "text": "time"
              }
            }
          },
          {
            "label": "Exit",
      "value": {
              "input": {
                "text": "goodbye"
              }
            }
          }
        ],
        "response_type": "option"
      }
    ],
    "intents": [
      {
        "intent": "menu",
        "confidence": 0.6178638458251953
      }
    ],
    "entities": []
  }
}
```

La risposta `option` contiene più opzioni da presentare all'utente. Ciascuna opzione include due oggetti, `label` e `value`. `label` è una stringa rivolta all'utente che identifica l'opzione; `value` specifica l'input del messaggio corrispondente che deve essere reinviato all'assistente se l'utente sceglie l'opzione.

La nostra applicazione client dovrà utilizzare i dati presenti in questa risposta per creare l'output che mostreremo all'utente e per inviare il messaggio appropriato all'assistente.

### Elenco delle opzioni disponibili

Il primo passo nella gestione di una risposta option consiste nel mostrare le opzioni all'utente utilizzando il testo specificato dalla proprietà `label` di ciascuna opzione. Puoi visualizzare le opzioni utilizzando qualsiasi tecnica supportata dalla tua applicazione, di norma un elenco a discesa o una serie di pulsanti su cui è possibile fare clic. (La proprietà `preference` facoltativa di una risposta option, se specificata, indica quale tipo di visualizzazione deve essere mostrata dall'applicazione, se possibile)

Il nostro esempio semplificato utilizza l'input e l'output standard, quindi non abbiamo accesso a una IU reale. Presenteremo invece semplicemente le opzioni sotto forma di elenco numerato. 

```javascript
// Option example 1: lists options.

const prompt = require('prompt-sync')();
const AssistantV2 = require('ibm-watson/assistant/v2');

// Set up Assistant service wrapper.
const service = new AssistantV2({
  iam_apikey: '{apikey}', // replace with API key
  version: '2019-02-28',
});

const assistantId = '{assistant_id}'; // replace with assistant ID
let sessionId;

// Create session.
service
  .createSession({
    assistant_id: assistantId,
  })
  .then(res => {
    sessionId = res.session_id;
    sendMessage({text: ''}); // start conversation with empty message
  })
  .catch(err => {
    console.log(err); // something went wrong
  });

// Send message to assistant.
function sendMessage(messageInput) {
  service
    .message({
      assistant_id: assistantId,
      session_id: sessionId,
      input: messageInput,
    })
    .then(res => {
      processResponse(res);
    })
    .catch(err => {
      console.log(err); // something went wrong
    });
}

// Process the response.
function processResponse(response) {
  let endConversation = false;

  // Check for client actions requested by the assistant.
  if (response.output.actions) {
    if (response.output.actions[0].type === 'client'){
      if (response.output.actions[0].name === 'display_time') {
        // User asked what time it is, so we output the local system time.
        console.log('The current time is ' + new Date().toLocaleTimeString() + '.');
      } else if (response.output.actions[0].name === 'end_conversation') {
        // User said goodbye, so we're done.
        console.log(response.output.generic[0].text);
        endConversation = true;
      }
    }
  } else {
    // Display the output from assistant, if any. Supports only a single
    // response.
    if (response.output.generic) {
    if (response.output.generic.length > 0) {
      switch (response.output.generic[0].response_type) {
          case 'text':
            // It's a text response, so we just display it.
            console.log(response.output.generic[0].text);
            break;
          case 'option':
            // It's an option response, so we'll need to show the user
            // a list of choices.
            console.log(response.output.generic[0].title);
            const options = response.output.generic[0].options;
            // List the options by label.
            for (let i = 0; i < options.length; i++) {
              console.log((i+1).toString() + '. ' + options[i].label);
            }
            break;
        }
      }
    }
  }

  // If we're not done, prompt for the next round of input.
  if (!endConversation) {
    const newMessageFromUser = prompt('>> ');
    newMessageInput = {
      message_type: 'text',
      text: newMessageFromUser
    }
    sendMessage(newMessageInput);
  } else {
    // We're done, so we delete the session.
    service
      .deleteSession({
        assistant_id: assistantId,
        session_id: sessionId,
      })
      .then(res => {
      return;
      })
      .catch(err => {
      console.log(err); // something went wrong
    });
}

}
```
{: codeblock}
{: javascript}

Guardiamo più attentamente il codice che genera l'output della risposta dall'assistente. Ora, invece di presupporre una risposta `text`, l'applicazione supporta sia il tipo di risposta `text` che `option`:

```javascript
    // Display the output from assistant, if any. Supports only a single
    // response.
    if (response.output.generic) {
    if (response.output.generic.length > 0) {
      switch (response.output.generic[0].response_type) {
          case 'text':
            // It's a text response, so we just display it.
            console.log(response.output.generic[0].text);
            break;
          case 'option':
            // It's an option response, so we'll need to show the user
            // a list of choices.
            console.log(response.output.generic[0].title);
            const options = response.output.generic[0].options;
            // List the options by label.
            for (let i = 0; i < options.length; i++) {
              console.log((i+1).toString() + '. ' + options[i].label);
            }
            break;
        }
      }
    }
```
{: codeblock}
{: javascript}

Se `response_type`=`text`, visualizzeremo solo l'output, come in precedenza. Ma se `response_type`=`option`, dovremo lavorarci ancora un po'. Innanzitutto, visualizziamo il valore della proprietà `title`, che serve come testo iniziale per introdurre l'elenco delle opzioni; successivamente, elenchiamo le opzioni utilizzando la proprietà `label` per identificare ciascuna di esse. (Un'applicazione reale mostra queste etichette in un elenco a discesa o come etichette su pulsanti su cui è possibile fare clic.) 

Puoi vedere il risultato attivando l'intento `#menu`:

```
Welcome to the Watson Assistant example!
>> what are the available actions?
What do you want to do?
1. Send greeting
2. Display the local time
3. Exit
>> 2
Sorry, I have no idea what you're talking about.
>>
```
{: screen}

Come puoi vedere, ora l'applicazione sta gestendo correttamente la risposta `option` elencando le scelte disponibili. Tuttavia, non stiamo ancora traducendo la scelta dell'utente in un input significativo. 

### Selezione di un'opzione

Oltre a `label`, ogni opzione nella risposta include anche un oggetto `value` che contiene i dati di input che devono essere reinviati all'assistente se l'utente sceglie l'opzione corrispondente. L'oggetto `value.input` equivale alla proprietà `input` dell'API `/message`, ciò significa che possiamo solo reinviare questo oggetto all'assistente così com'è. 

Per eseguire ciò nella nostra applicazione, configureremo un nuovo indicatore `promptOption` quando il client riceve una risposta `option`. Quando il valore di questo indicatore è true, sappiamo che vogliamo prendere il prossimo ciclo di input da `value.input` piuttosto che accettare l'input di testo in lingua naturale proveniente dall'utente. (Di nuovo, non abbiamo un'interfaccia utente reale, quindi richiederemo solo all'utente di selezionare un'opzione valida dall'elenco in base al numero.)

```javascript
// Option example 2: sends back selected option value.

const prompt = require('prompt-sync')();
const AssistantV2 = require('ibm-watson/assistant/v2');

// Set up Assistant service wrapper.
const service = new AssistantV2({
  iam_apikey: 'AZkSnK4b40UI5kLepHKxIKYpVcxeg0yPLbVVwGFW8kjM', // replace with API key
  version: '2019-02-28',
});

const assistantId = 'dcd5c5ad-f3a1-4345-89c5-708b0b5ff4f7'; // replace with assistant ID
let sessionId;

// Create session.
service
  .createSession({
    assistant_id: assistantId,
  })
  .then(res => {
    sessionId = res.session_id;
    sendMessage({text: ''}); // start conversation with empty message
  })
  .catch(err => {
    console.log(err); // something went wrong
  });

// Send message to assistant.
function sendMessage(messageInput) {
  service
    .message({
      assistant_id: assistantId,
      session_id: sessionId,
      input: messageInput,
    })
    .then(res => {
      processResponse(res);
    })
    .catch(err => {
      console.log(err); // something went wrong
    });
}

// Process the response.
function processResponse(response) {
  let endConversation = false;
  let promptOption = false;

  // Check for client actions requested by the assistant.
  if (response.output.actions) {
    if (response.output.actions[0].type === 'client'){
      if (response.output.actions[0].name === 'display_time') {
        // User asked what time it is, so we output the local system time.
        console.log('The current time is ' + new Date().toLocaleTimeString() + '.');
      } else if (response.output.actions[0].name === 'end_conversation') {
        // User said goodbye, so we're done.
        console.log(response.output.generic[0].text);
        endConversation = true;
      }
    }
  } else {
    // Display the output from assistant, if any. Supports only a single
    // response.
    if (response.output.generic) {
    if (response.output.generic.length > 0) {
      switch (response.output.generic[0].response_type) {
          case 'text':
            // It's a text response, so we just display it.
            console.log(response.output.generic[0].text);
            break;
          case 'option':
            // It's an option response, so we'll need to show the user
            // a list of choices.
            console.log(response.output.generic[0].title);
            const options = response.output.generic[0].options;
            // List the options by label.
            for (let i = 0; i < options.length; i++) {
              console.log((i+1).toString() + '. ' + options[i].label);
              }
            promptOption = true;
            break;
        }
      }
    }
  }

  // If we're not done, prompt for the next round of input.
  if (!endConversation) {
    let messageInput;
    if (promptOption == true) {
      // Prompt for a valid selection from the list of options.
      let choice;
      do {
        choice = prompt('? ');
          if (isNaN(choice)) {
            choice = 0;
          }
      } while (choice < 1 || choice > response.output.generic[0].options.length);
      const value = response.output.generic[0].options[choice-1].value;
      // Use message input from the selected option.
      messageInput = value.input;
    } else {
      // We're not showing options, so we just prompt for the next
      // round of input.
      const newText = prompt('>> ');
      messageInput = {
        text: newText
      }
    }
    sendMessage(messageInput); 
  } else {
    // We're done, so we delete the session.
    service
      .deleteSession({
        assistant_id: assistantId,
        session_id: sessionId,
      })
      .then(res => {
        return;
      })
      .catch(err => {
        console.log(err); // something went wrong
      });
  }
}
```
{: codeblock}
{: javascript}

Tutto quello che dobbiamo fare è utilizzare l'oggetto `value.input` proveniente dalla risposta selezionata come prossimo ciclo dell'input del messaggio piuttosto che creare un nuovo oggetto `input` utilizzando l'input del testo. Poi l'assistente risponde esattamente come se fosse stato l'utente a digitare direttamente il testo di input. 

```
Welcome to the Watson Assistant example!
>> hi
Good day to you.
>> what are the choices?
What do you want to do?
1. Send greeting
2. Display the local time
3. Exit
? 2
The current time is 1:29:14 PM.
>> bye
OK! See you later.
```
{: screen}

Ora possiamo accedere a tutte le funzioni dell'assistente effettuando richieste in lingua naturale oppure effettuando una selezione da un menu di opzioni. 

Tieni presente che lo stesso approccio viene utilizzato anche per le risposte `suggestion`. Se il tuo piano supporta la funzione di disambiguazione, puoi utilizzare una logica simile per richiedere agli utenti di effettuare la selezione da un elenco quando non è chiaro quale delle diverse opzioni possibili sia corretta. Per ulteriori informazioni sulla funzione di disambiguazione, vedi [Disambiguazione](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).
