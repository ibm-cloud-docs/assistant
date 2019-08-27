---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-01"

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

# Richiamo di un'applicazione client ![BETA](images/beta.png)
{: #dialog-actions-client}

Aggiungi un'azione client al tuo nodo di dialogo che sospenda il dialogo in modo che un processo lato client possa eseguire e restituire un risultato come parte dell'elaborazione che si verifica all'interno di un turno di dialogo.
{: shortdesc}

Un'azione client definisce una chiamata programmatica in un formato standard che possa essere compreso dalla tua applicazione client esterna. La tua applicazione client esterna deve utilizzare le informazioni fornite per eseguire la funzione o la chiamata programmatica e poi restituire il risultato al dialogo. 

Questa azione non effettua una chiamata diretta da sola. Fondamentalmente indica al dialogo di fermarsi e attendere che un'applicazione client esterna faccia qualcosa. Il programma che l'applicazione client esegue può essere qualsiasi operazione scelta da te. Assicurati di specificare i dettagli del nome della chiamata e dei parametri e il nome della variabile del messaggio di errore, in base alle regole di formattazione JSON che vengono descritte successivamente in questo argomento.

Puoi richiamare un'applicazione per eseguire i seguenti tipi di operazione: 

- Convalidare le informazioni che hai raccolto dall'utente.
- Eseguire calcoli e modifiche di stringhe nell'input utente troppo complessi da gestire per i metodi di espressione SpEL supportati.
- Richiamare i dati da un'altra applicazione o da un altro servizio. 

Per informazioni su come richiamare un servizio esterno, ad esempio un'azione web {{site.data.keyword.openwhisk_short}}, vedi [Esecuzione di una chiamata programmatica da un nodo di dialogo](/docs/services/assistant?topic=assistant-dialog-webhooks).

Il seguente diagramma mostra come utilizzare una chiamata client per acquisire le informazioni sulle previsioni meteorologiche e restituirle all'utente. 

![Mostra una persona che chiede una previsione meteorologica, il dialogo che invia la richiesta ad una applicazione client che la invia al servizio esterno](images/forecast.png)

Tieni presente che l'azione client richiama il programma `MyWeatherFunction` che è un programma che viene eseguito dall'applicazione client. L'applicazione client effettua la chiamata al servizio web esterno (`/weather`) per ottenere le informazioni sulle previsioni reali. L'applicazione client restituisce poi la risposta al dialogo. Quando aggiungi un'azione client a un dialogo, deve esistere un'applicazione client che esegue l'elaborazione da sola o che passa le informazioni tra il tuo dialogo e i servizi di backend esterni che vuoi utilizzare. 

## Procedura
{: #dialog-actions-client-call}

Per eseguire una chiamata programmatica a un'applicazione client da un nodo di dialogo, completa la seguente procedura: 

1.  Nel nodo di dialogo da cui desideri eseguire la chiamata programmatica, apri l'editor JSON.

    - Per eseguire una chiamata programmatica che viene eseguita dopo la valutazione della risposta per un nodo, aprire l'editor JSON relativo alla risposta del nodo.

      ![Mostra come accedere all'editor JSON associato ad una risposta del nodo standard.](images/contextvar-json-response.png)

      Se l'impostazione **Risposte multiple** è **Attivo** per il nodo, devi fare clic sull'icona **Modifica risposta** ![Modifica risposta](images/edit-slot.png) affinché il menu **Opzioni** ![Risposta avanzata](images/kabob.png) sia visibile.

      ![Mostra come accedere all'editor JSON associato a un nodo standard che ha più risposte condizionali abilitate per esso.](images/contextvar-json-multi-response.png)

    Se desideri visualizzare o elaborare ulteriormente la risposta proveniente dallo stesso turno di dialogo, devi aggiungere un secondo nodo che esegua tale operazione e passare ad esso da questo nodo.
    {: tip}

    - Per eseguire una chiamata che possa essere utilizzata da un singolo slot, fai clic sull'icona **Modifica slot** ![Icona Modifica slot](images/edit-slot.png) relativa allo slot ed esegui una delle operazioni riportate di seguito:

      - Per eseguire una chiamata programmatica che viene eseguita una volta valutata come true la condizione dello slot, apri l'editor JSON associato alla condizione dello slot.

        ![Mostra come accedere all'editor JSON associato ad una condizione slot.](images/contextvar-json-slot-condition.png)

      - Per eseguire una chiamata programmatica che viene eseguita una volta riempito correttamente lo slot, apri l'editor JSON associato alla risposta Trovato. Per eseguire tale operazione, dal menu **Opzioni** ![Icona Opzioni](images/kabob.png) dello slot, fai clic su **Abilita risposte condizionali**. Per la risposta Trovato, fai clic sull'icona **Modifica risposta** ![Modifica risposta](images/edit-slot.png). Dal menu **Opzioni** ![Icona Opzioni](images/kabob.png) per la risposta Trovato, fai clic su **Apri editor JSON**.

        ![Mostra come accedere all'editor JSON associato alla risposta condizionale per uno slot.](images/contextvar-json-slot-multi-response.png)

1.  Utilizzare la seguente sintassi per definire la chiamata programmatica.

    ```json
    {
      "context": {
        "variable_name" : "variable_value"
      },
      "actions": [
        {
          "name":"<actionName>",
          "type":"client",
          "parameters": {
            "<parameter_name>":"<parameter_value>",
            "<parameter_name>":"<parameter_value>"
          },
          "result_variable": "<result_variable_name>"
        }
      ],
      "output": {
        "text": "response text"
      }
    }
    ```
    {: codeblock}

    L'array `actions` specifica le chiamate programmatiche da eseguire dal dialogo. Può definire fino a cinque chiamate programmatiche separate. Specifica le seguenti coppie nome e valore nell'array JSON:

    - `<actionName>`: obbligatorio. Il nome dell'azione o del servizio da richiamare. Specifica un nome nella sintassi che preferisci. L'obiettivo è quello di specificare un nome che l'applicazione client riconoscerà e saprà come gestire. Il nome non può essere più lungo di 256 caratteri.

      Ad esempio: `calculateRate`

    - `<type>`: indica il tipo di chiamata da eseguire. Facoltativamente, specifica `client`, che è il valore predefinito. 

      Invia un messaggio di risposta con le informazioni sulla chiamata programmatica in un formato standard comprensibile dalla tua applicazione client esterna. La tua applicazione client deve utilizzare le informazioni fornite per eseguire la funzione o la chiamata programmatica e restituire il risultato al dialogo. L'oggetto JSON nel corpo della risposta specifica il servizio o la funzione da richiamare, i parametri associati da passare con la chiamata e il formato del risultato da restituire.

    - `<action_parameters>`: i parametri previsti dal programma esterno, specificati come un oggetto JSON. I parametri sono necessari solo se il programma esterno li richiede.

    - `<result_variable_name>`: il nome da utilizzare per fare riferimento all'oggetto JSON restituito dal programma o dal servizio esterno. Il risultato viene aggiunto alla sezione di contesto della risposta `/message`. In altri termini, il risultato viene memorizzato come una variabile di contesto in modo che possa essere visualizzato nella risposta del nodo o che sia possibile accedervi dai nodi di dialogo attivati in un secondo momento. Qualsiasi valore esistente per la variabile di contesto viene sovrascritto dal valore restituito dall'azione. Puoi specificare `result_variable_name` utilizzando la seguente sintassi:

      - `my_result`
      - `$my_result`

      Il nome non può essere più lungo di 64 caratteri. Il nome della variabile non può contenere i seguenti caratteri: parentesi `()`, parentesi quadre (`[]`), apice (`'`), virgolette (`"`) o una barra rovesciata (`\`).

      Se desideri salvare il risultato della sezione di output o di input della risposta `/message`, puoi aggiungere una delle seguenti parole chiave di ubicazione come prefisso a `result_variable_name`:

       - `output.`: aggiunge il risultato alla sezione di output della risposta /message. Ad esempio, `output.my_result`.
       - `input.`: aggiunge il risultato alla sezione di input della risposta /message. Ad esempio, `input.my_result`.

      Puoi specificare anche un prefisso della parola chiave di ubicazione `context.`. Ad esempio, `context.my_result`. Tuttavia, puoi anche non farlo in quanto il risultato viene aggiunto al contesto per impostazione predefinita.

      Puoi includere i punti nel nome della variabile per creare un oggetto JSON nidificato. Ad esempio, puoi definire queste variabili per acquisire i risultati da due richieste separate ad un servizio meteo per le previsioni per oggi e domani:

      - `context.weather.today`
      - `context.weather.tomorrow`

      I risultati (i valori parametro `temp` e `rain`) vengono memorizzati nel contesto in questa struttura:

      ```json
      {
        "weather": {
          "today": {
            "temp": "20",
            "rain": "30"
          },
          "tomorrow": {
            "temp": "23",
            "rain": "80"
          }
        }
      }
      ```
      {: codeblock}

      Se più azioni in un singolo array di azioni JSON aggiungono il risultato della loro chiamata programmatica alla stessa variabile di contesto, l'ordine in cui il contesto viene aggiornato è importante. Per il tipo di azione, l'ordine in cui le azioni sono definite nell'array determina l'ordine in cui viene impostato il valore della variabile di contesto. Il valore della variabile di contesto restituito dall'ultima azione nell'array sovrascrive il valore calcolato dalle altre azioni.

## Esempio di chiamata client
{: #dialog-actions-client-example}

Il seguente esempio mostra come potrebbe essere una chiamata ad un servizio meteo esterno. Viene aggiunto all'editor JSON associato alla risposta del nodo. Al termine dell'attivazione della risposta a livello di nodo, gli slot hanno raccolto e memorizzato le informazioni sulla data e sull'ubicazione provenienti dall'utente. Questo esempio presuppone che il programma che verrà richiamato sia denominato `MyWeatherFunction` e che utilizzi i parametri `location` e `date` e restituisca un oggetto JSON, `{"forecast": "<value>"}`.

 ``` json
{
  "actions": [
    {
      "name": "MyWeatherFunction",
      "type": "client",
      "parameters": {
        "date": "$date",
        "location": "$location"
      },
      "result_variable": "context.my_forecast"
    }
  ]
}
```
{: codeblock}

Di norma, il servizio restituisce solo al client da una richiesta POST `/message` quando è necessario un nuovo input utente, ad esempio dopo l'esecuzione di un padre e prima dell'esecuzione di uno dei suoi nodi figlio. Tuttavia, se aggiungi un'azione client ad un nodo, dopo la valutazione, il servizio restituirà sempre al client in modo che il risultato della chiamata dell'azione possa essere restituito. Per impedire l'attesa dell'input utente nel caso in cui non sia previsto, ad esempio per un nodo che è configurato per passare direttamente ad un nodo figlio, il servizio aggiunge il seguente valore al contesto del messaggio:

```json
  {
    "context": {
      "skip_user_input": true
    }
  }
```
{: codeblock}

Se desideri che il client esegua un'azione, ma non acquisisca l'input utente, puoi seguire la stessa convenzione e aggiungere la variabile di contesto `skip_user_input` al nodo padre per comunicarlo all'applicazione client.

La tua applicazione client deve sempre controllare la variabile `skip_user_input` nel contesto. Se presente, l'applicazione sa di non dover richiedere nuovo input all'utente, ma di dover eseguire l'azione, aggiungerne i risultati al messaggio e ripassarlo al servizio. La nuova richiesta di messaggio POST deve includere il messaggio restituito dalla risposta al messaggio POST precedente (ossia il contesto, l'input, gli intenti, le entità e facoltativamente la sezione di output) e, invece dell'oggetto JSON che definisce la chiamata programmatica da eseguire, deve includere il risultato restituito dalla chiamata programmatica.

In un nodo figlio a cui vuoi passare dopo questo nodo, aggiungi la risposta da mostrare all'utente:

``` json
{
  "output":{
    "text": {
      "values": [
        "It will be $my_forecast $date.literal in $location.literal."
      ]
    }
  }
```
{: codeblock}
