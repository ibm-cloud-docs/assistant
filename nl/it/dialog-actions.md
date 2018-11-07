---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-15"

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

# Esecuzione di chiamate programmatiche da un nodo di dialogo ![BETA](images/beta.png)
{: #dialog-actions}

Definisci le azioni che possono eseguire chiamate programmatiche ad applicazioni esterne o a servizi esterni e che restituiscono un risultato come parte dell'elaborazione eseguita all'interno di un turno di dialogo.
{: shortdesc}

Puoi utilizzare un servizio esterno per convalidare le informazioni raccolte dall'utente oppure puoi eseguire calcoli o manipolazioni di stringhe nell'input che si rivelano troppo complessi per essere gestiti utilizzando i metodi e le espressioni SpEL supportati. Oppure puoi interagire con un servizio web esterno per ottenere le informazioni, ad esempio un servizio di traffico aereo per controllare l'orario di arrivo previsto di un volo oppure un servizio meteo per avere una previsione. Puoi anche interagire con un'applicazione esterna, ad esempio un sito di prenotazione al ristorante, per completare una semplice transazione al posto dell'utente.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/y0A6X-KNoB8?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Quando definisci una chiamata programmatica, scegli uno dei seguenti tipi:

- **client**: definisce una chiamata programmatica in un formato standard utilizzabile dalla tua applicazione client esterna per eseguire la funzione o la chiamata programmatica e per restituire il risultato nel dialogo.
- **server**: richiama direttamente un'azione {{site.data.keyword.openwhisk_short}} e restituisce il risultato nel dialogo.

    Attualmente, puoi richiamare un'azione {{site.data.keyword.openwhisk_short}} dalle istanze {{site.data.keyword.conversationshort}} ospitate nelle regioni Stati Uniti del Sud o Germania.

    **Nota**: l'istanza {{site.data.keyword.openwhisk_short}} che viene utilizzata è quella ospitata nella stessa ubicazione (Stati Uniti del Sud o Germania). Quindi, non definire un'azione in un'istanza {{site.data.keyword.openwhisk_short}} ospitata in Germania se intendi accedere ad essa da un'istanza del servizio {{site.data.keyword.conversationshort}} ospitata, ad esempio, negli Stati Uniti del Sud.

    **Importante**: utilizza questo metodo unicamente per eseguire una chiamata ad un'azione {{site.data.keyword.openwhisk_short}} che sai per certo verrà restituita in **meno di 5 secondi**. La richiesta a {{site.data.keyword.openwhisk_short}} andrà in timeout se una singola chiamata di servizio impiegherà più tempo di quello consentito. E se il tuo dialogo esegue più di una chiamata ad un servizio esterno, la quantità di tempo totale consentita per il completamento delle chiamata è 7 secondi. Se il tempo di completamento delle prime tre chiamate è di 2 secondi ciascuno e la quarta impiega più di un secondo, la quarta chiamata verrà arrestata e il messaggio di errore relativo alla chiamata indicherà che non è stata completata. Per i servizi meno efficienti che devi richiamare, gestisci la chiamata tramite la tua applicazione client e passa le informazioni al dialogo come passo separato.

## Procedura
{: #call-action}

Per eseguire una chiamata programmatica da un nodo di dialogo, completa la seguente procedura:

1.  Nel nodo di dialogo da cui desideri eseguire la chiamata programmatica, apri l'editor JSON.

    - Per eseguire una chiamata programmatica che viene eseguita dopo la valutazione della risposta per un nodo, aprire l'editor JSON relativo alla risposta del nodo.

      ![Mostra come accedere all'editor JSON associato ad una risposta del nodo standard.](images/contextvar-json-response.png)

      Se l'impostazione **Risposte multiple** è **Attivo** per il nodo, devi fare clic sull'icona **Modifica risposta** ![Modifica risposta](images/edit-slot.png) prima che il menu **Opzioni**  ![Risposta avanzata](images/kabob.png) diventi visibile.

      ![Mostra come accedere all'editor JSON associato ad un nodo standard per cui sono abilitate più risposte condizionali.](images/contextvar-json-multi-response.png)

    Se desideri visualizzare o elaborare ulteriormente la risposta proveniente dal servizio esterno all'intero dello stesso turno di dialogo, devi aggiungere un secondo nodo che esegua tale operazione e passare ad esso da questo nodo.
    {: tip}

    - Per eseguire una chiamata che possa essere utilizzata da un singolo slot, fai clic sull'icona **Modifica slot** ![Icona Modifica slot](images/edit-slot.png) relativa allo slot ed esegui una delle operazioni riportate di seguito:

      - Per eseguire una chiamata programmatica che viene eseguita una volta valutata come true la condizione dello slot, apri l'editor JSON associato alla condizione dello slot.

        ![Mostra come accedere all'editor JSON associato ad una condizione dello slot.](images/contextvar-json-slot-condition.png)

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
          "type":"client | server",
          "parameters": {
            "<parameter_name>":"<parameter_value>",
            "<parameter_name>":"<parameter_value>"
          },
          "result_variable": "<result_variable_name>",
          "credentials": "<reference_to_credentials>"
        }
      ],
      "output": {
        "text": "response text"
      }
    }
    ```
    {: codeblock}

    L'array `actions` specifica le chiamate programmatiche da eseguire dal dialogo. Può definire fino a 5 chiamate programmatiche separate. Specifica le seguenti coppie nome e valore nell'array JSON:

    - `<actionName>`: obbligatorio. Il nome dell'azione o del servizio da richiamare. Il nome non può essere più lungo di 64 caratteri.

       - Per i tipi di azione client, specifica un nome nella sintassi che preferisci. L'obiettivo è quello di specificare un nome che l'applicazione client riconoscerà e saprà come gestire.

          Ad esempio: `calculateRate`

       - Per i tipi di azione {{site.data.keyword.openwhisk_short}} (server), utilizza questa sintassi per fornire il nome completo dell'azione: `/<namespace>/[<package-name>]/<action name>`

         - Se l'azione fa parte di un pacchetto, le informazioni `<package-name>` sono necessarie; in caso contrario non lo sono.
         - Se stai richiamando una sequenza di azioni, specifica `<sequence name>` al posto di `<action name>`.
         - Di norma, lo spazio dei nomi relativo ad un'azione definita dall'utente presenta la seguente sintassi: `<myIBMCloudOrganizationID>_<myIBMCloudSpace>`. Ad esempio: `/jdoeorg_prod10/search flights`
         - Le azioni fornite con {{site.data.keyword.openwhisk_short}} spesso presentano lo spazio dei nomi: `whisk.system`, ma devi sempre verificare lo spazio dei nomi per esserne sicuro. Ad esempio: `/whisk.system/weather/forecast`

    - `<type>`: indica il tipo di chiamata da eseguire. Scegli dai seguenti tipi:

      - **client**: invia una risposta al messaggio con informazioni sulla chiamata programmatica in un formato standard utilizzabile dall'applicazione client esterna per eseguire la chiamata o la funzione e per ottenere un risultato per conto del dialogo. L'oggetto JSON nel corpo della risposta specifica il servizio o la funzione da richiamare, i parametri associati da passare con la chiamata e la modalità di restituzione del risultato.

      - **server**: richiama direttamente un'azione {{site.data.keyword.openwhisk_short}} (una o più azioni). Devi definire l'azione stessa separatamente utilizzando {{site.data.keyword.openwhisk}}. Per ulteriori informazioni, vedi la sezione [Creazione di un'azione {{site.data.keyword.openwhisk_short}}](dialog-actions.html#create-action) riportata di seguito.

      La specifica del tipo è facoltativa. Il valore predefinito è `client`.

    - `<action_parameters>`: i parametri previsti dal programma esterno, specificati come un oggetto JSON. I parametri sono necessari solo se il programma esterno li richiede.

    - `<result_variable_name>`: il nome da utilizzare per fare riferimento all'oggetto JSON restituito dal programma o dal servizio esterno. Il risultato viene aggiunto alla sezione di contesto della risposta /message. In altri termini, il risultato viene memorizzato come una variabile di contesto in modo che possa essere visualizzato nella risposta del nodo o che sia possibile accedervi dai nodi di dialogo attivati in un secondo momento. Qualsiasi valore esistente per la variabile di contesto viene sovrascritto dal valore restituito dall'azione. Puoi specificare `result_variable_name` utilizzando la seguente sintassi:

      - `my_result`
      - `$my_result`

      Il nome non può essere più lungo di 64 caratteri. Il nome della variabile non può contenere i seguenti caratteri: parentesi `()`, parentesi quadre (`[]`), apice (`'`), virgolette (`"`) o una barra rovesciata (`\`).

      Se desideri salvare il risultato della sezione di output o di input della risposta /message, puoi anteporre una delle seguenti parole chiave di ubicazione a `result_variable_name`:

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

      Se più azioni in un singolo array di azioni JSON aggiungono il risultato della loro chiamata programmatica alla stessa variabile di contesto, l'ordine in cui il contesto viene aggiornato è importante:

      1. Se hai una combinazione di azioni server e client nell'array, il servizio elabora prima le azioni di tipo server. Di conseguenza, il valore calcolato per la variabile di contesto dall'ultima azione di tipo client nell'array sovrascrive il valore calcolato per essa da qualsiasi azione di tipo server.
      1. Per il tipo di azione, l'ordine in cui le azioni sono definite nell'array determina l'ordine in cui viene impostato il valore della variabile di contesto. Il valore della variabile di contesto restituito dall'ultima azione nell'array sovrascrive il valore calcolato dalle altre azioni. 

    - `<reference_to_credentials>`: il nome dell'oggetto in cui sono memorizzate le credenziali {{site.data.keyword.openwhisk_short}}. Obbligatorio solo per le azioni server. Queste credenziali vengono utilizzate per accedere all'istanza {{site.data.keyword.openwhisk_short}} su cui viene eseguita l'azione. Queste non sono le tue credenziali {{site.data.keyword.Bluemix_notm}}.

      Per rilevare le credenziali, completare la procedura riportata di seguito: 
      1.  Vai alla pagina [{{site.data.keyword.openwhisk_short}} Chiave API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/openwhisk/learn/api-key){: new_window}.

          - Se ancora non hai creato un account, fallo.
          - Se non sei collegato, collegati.

      1.  Fai clic sull'icona **Mostra chiave di autenticazione** ![Mostra chiave di autenticazione](images/show-auth-icon.png) per mostrare le credenziali. Il segmento prima dei due punti (:) è il tuo ID utente. Il segmento dopo i due punti è la tua password.

      **Attenzione**: gli addebiti sostenuti all'esecuzione dell'azione vengono addebitati alla persona che possiede queste credenziali. 

      Per proteggere le credenziali, non memorizzarle nello spazio di lavoro {{site.data.keyword.conversationshort}}. Piuttosto, passale dall'applicazione client come parte del contesto. Puoi impedire la memorizzazione delle informazioni nei log Watson nidificando la tua variabile di contesto all'interno della sezione $private del contesto del messaggio. Ad esempio: `$private.my_credentials`.

      L'oggetto credentials che definisci deve contenere i parametri `user` e `password`.

      ```json
      {
        "user":"5tj3b41j-bf3j-5d92-24g9-4a7769ab12af",
        "password":"y65gqSTSRzqE..."
      }
      ```
      {: codeblock}

      Durante il test del dialogo, puoi impostare temporaneamente la variabile di contesto `$private.my_credentials` con i tuoi valori username e password {{site.data.keyword.openwhisk_short}} reali facendo clic su **Gestisci contesto** dal riquadro "Provalo" negli strumenti.

      ![Mostra come viene definita la variabile di contesto $private.my_credentials nell'interfaccia di gestione contesti Provalo](images/testing-creds.png)

## Creazione di un'azione {{site.data.keyword.openwhisk_short}}
{: #create-action}

Se scegli di definire una chiamata programmatica di tipo server, prima che tu possa richiamarla da un dialogo, devi creare l'azione in {{site.data.keyword.openwhisk}}. Se stai definendo una chiamata programmatica di tipo client, ignora questa procedura. 

**Nota:** l'esecuzione di un'azione {{site.data.keyword.openwhisk_short}} potrebbe comportare un costo. Per ulteriori informazioni, vedi [Prezzi ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/openwhisk/learn/pricing){: new_window}. {{site.data.keyword.openwhisk_short}} non distingue le chiamate effettuate dal riquadro "Provalo" durante il test dalle chiamate effettuate da un'applicazione in produzione.

Per creare un'azione {{site.data.keyword.openwhisk_short}}, completa la seguente procedura: 

1.  Vai all'[editor {{site.data.keyword.openwhisk_short}} online ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.ng.bluemix.net/openwhisk/create){: new_window}, in cui puoi scrivere il tuo codice direttamente nel browser.

    Esiste anche un'[interfaccia riga di comando ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/openwhisk/learn/cli){: new_window} che puoi installare e che ti consente di definire un'azione utilizzando il codice che hai scritto localmente.

1.  Crea un'azione {{site.data.keyword.openwhisk_short}} utilizzando uno dei seguenti linguaggi di programmazione supportati. Consulta la [documentazione {{site.data.keyword.openwhisk_short}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.ng.bluemix.net/docs/openwhisk/openwhisk_actions.html){: new_window} per dettagli. 

    Tieni presente i seguenti suggerimenti:

    - Fai riferimento all'[esempio di un'azione {{site.data.keyword.openwhisk_short}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.ng.bluemix.net/docs/openwhisk/openwhisk_actions.html#openwhisk_apicall_action){: new_window} per vedere come richiamare un servizio esterno. 
    - Per eseguire una chiamata ad un servizio Watson, utilizza [Watson Developer Cloud SDK ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/watson-developer-cloud){: new_window} per la lingua che desideri utilizzare. 
    - Assicurati che la tua azione {{site.data.keyword.openwhisk_short}} accetti i parametri di input come un oggetto JSON e restituisca l'output come un oggetto JSON.
    - Se stai utilizzando Node.js per scrivere la tua azione {{site.data.keyword.openwhisk_short}}, assicurati di utilizzare `Promise` per l'elaborazione asincrona. Assicurati anche di restituire il risultato finale della funzione `main`.

    In alternativa, puoi creare una sequenza di azioni.
    {: tip}

## Gestione degli errori

Se l'azione {{site.data.keyword.openwhisk_short}} rileva un errore, il messaggio di errori viene restituito al dialogo e viene memorizzato come una proprietà della variabile di risposta denominata `cloud_functions_call_error`. L'errore potrebbe verificarsi, ad esempio, se la tua azione {{site.data.keyword.openwhisk_short}} non può ricevere una risposta da un servizio esterno oppure se l'azione Cloud Function ha esito negativo. Se le credenziali di Cloud Function non vengono fornite oppure non sono corrette, viene restituito un errore. Questa variabile di contesto viene utilizzata solo per le azioni server; nella tua applicazione client, considera la creazione di un oggetto simile che acquisisca le informazioni sull'errore e che le restituisca al dialogo come una variabile di contesto. 

Puoi condizionare la risposta del nodo di dialogo al primo controllo degli errori. Ad esempio, puoi assicurarti che la risposta che fa riferimento al risultato di un'azione {{site.data.keyword.openwhisk_short}} venga visualizzata solo se non sono stati rilevati errori durante l'aggiunta di questa espressione alla condizione di risposta:

```json
  $forecast_result.cloud_functions_call_error == null
```
{: codeblock}

Per una chiamata programmatica di tipo client, puoi passare le informazioni relative all'elaborazione dell'errore definendo una variabile di contesto, ad esempio, `action_error`. Puoi ripassarle al servizio come parte della variabile del risultato. Quindi, puoi visualizzare una risposta solo se non sono stati rilevati errori definendo una condizione di risposta come questa: 

```json
  $forecast_result.action_error == null
```
{: codeblock}

## Esempio di chiamata client
{: #action-client-example}

Il seguente esempio mostra come potrebbe essere una chiamata ad un servizio meteo esterno. Viene aggiunto all'editor JSON associato alla risposta del nodo. Al termine dell'attivazione della risposta a livello di nodo, gli slot hanno raccolto e memorizzato le informazioni sulla data e sull'ubicazione provenienti dall'utente. Questo esempio presuppone che il servizio che verrà richiamato disponga di un endpoint denominato `/weather` e che utilizzi i parametri `location` e `date` e restituisca un oggetto JSON, `{"forecast": "<value>"}`.

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

Di norma, il servizio restituisce al client solo da una richiesta /message POST quando è necessario un nuovo input utente, ad esempio dopo l'esecuzione di un padre e prima di eseguire uno dei suoi nodi figlio. Tuttavia, se aggiungi un'azione client ad un nodo, dopo la valutazione, il servizio restituirà sempre al client in modo che il risultato della chiamata dell'azione possa essere restituito. Per impedire l'attesa dell'input utente nel caso in cui non sia previsto, ad esempio per un nodo che è configurato per passare direttamente ad un nodo figlio, il servizio aggiunge il seguente valore al contesto del messaggio:

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

Il seguente diagramma mostra come utilizzare una chiamata client per acquisire le informazioni sulle previsioni meteorologiche e restituirle all'utente.

![Mostra una persona che chiede una previsione meteorologica e il dialogo che invia la richiesta ad una app client che la invia al servizio esterno](images/forecast.png)

## Server call example
{: #action-server-example}

Il seguente esempio mostra come potrebbe essere una chiamata ad un'azione {{site.data.keyword.openwhisk_short}}. Questo esempio mostra come utilizzare l'azione `echo` {{site.data.keyword.openwhisk_short}} definita nel [pacchetto Utilities ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/docs/openwhisk/openwhisk_actions.html#openwhisk_create_action_sequence){: new_window} fornito con il servizio. L'azione acquisisce una stringa di testo e la restituisce.

 ``` json
{
  "actions": [
    {
      "name": "/whisk.system/utils/echo",
      "type":"server",
      "parameters": {
        "message": "<?input.text?>"
      },
      "result_variable": "context.my_input_returned",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

Ora i nodi di dialogo successivi possono accedere all'output dell'azione {{site.data.keyword.openwhisk_short}}, memorizzato nella variabile `context.my_input_returned`.

 ``` json
 {
  "output":{
    "text": {
      "values": [
        "Your input was: $my_input_returned."
      ]
    }
  }
}
```
{: codeblock}

Il seguente diagramma mostra come richiamare un'azione {{site.data.keyword.openwhisk_short}} con un semplice esempio che richiama il servizio echo {{site.data.keyword.openwhisk_short}} integrato. Chiede l'input all'utente e lo passa al servizio echo. Il servizio echo restituisce lo stesso testo che poi viene visualizzato all'utente.

![Mostra un utente che inserisce del testo e il dialogo che invia la richiesta al servizio {{site.data.keyword.openwhisk_short}}, quindi mostra la restituzione del testo al dialogo.](images/echo-via-cf.png)

### Echo action example

Per vedere uno spazio di lavoro con un dialogo già impostato per richiamare l'azione Echo {{site.data.keyword.openwhisk_short}} integrata, completa la procedura riportata di seguito:

1.  Scarica il file [CloudFunctionsEcho.json ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/watson-developer-cloud/community/raw/master/conversation/cloud-functions-echo.json){: new_window}.
1.  Importa il file JSON come un nuovo spazio di lavoro.
1.  Riesamina il dialogo per vedere in che modo viene specificata la chiamata all'azione Echo.
1.  Dal riquadro "Provalo", fai clic su **Gestisci contesto** e quindi imposta (temporaneamente) le variabili di contesto utilizzando i tuoi valori di username e password {{site.data.keyword.openwhisk_short}}.

    ```json
    $private.my_credentials
    {
      "user":"<your-CF-instance-username>",
      "password":"<your-CF-instance-password>"
    }
    ```
    {: codeblock}

1.  Verifica il dialogo inserendo del testo.

    Il servizio utilizzerà l'azione Echo {{site.data.keyword.openwhisk_short}} per ripresentarti il testo che hai immesso.

## Advanced server call example
{: #advanced-action-server-example}

Puoi richiamare più azioni da un singolo flusso di dialogo. Infatti, puoi richiamare fino a cinque azioni all'interno di un oggetto JSON `actions` in un singolo nodo di dialogo. Tuttavia, le azioni di tipo server definite in un array JSON `actions` vengono elaborate tutte in parallelo. Quindi, non puoi richiamare un'azione di tipo server e passare il risultato da essa ad una seconda azione di tipo server nello stesso blocco `actions`. Il modo migliore per richiamare le azioni server in uno specifico ordine consiste nell'utilizzare una sequenza {{site.data.keyword.openwhisk_short}}. Nel runtime, questo approccio è più veloce in quanto il dialogo deve solo eseguire una chiamata esterna per completare più azioni. Per utilizzare una sequenza, fai semplicemente riferimento al nome sequenza invece che ad un nome azione nella definizione del blocco `actions`. In alternativa, puoi richiamare la prima azione di tipo server da un nodo e passare ad un nodo figlio che richiama l'azione di tipo server successiva.

Se definisci un array `actions` che contiene una combinazione di azioni di tipo client e di tipo server, quando il nodo di dialogo verrà eseguito, le azioni client non verranno inviate al client fino al completamento dell'elaborazione di tutte le azioni di tipo server.
{: tip}

Il seguente esempio mostra come potrebbe essere una chiamata ad un'azione {{site.data.keyword.openwhisk_short}}.

Questo esempio mostra come utilizzare un'azione {{site.data.keyword.openwhisk_short}} per richiamare un servizio esterno che acquisisce un nome città e restituisce le coordinate di latitudine e di longitudine della posizione fornita.

``` json
{
  "actions": [
    {
      "name": "/jdoeorg_prod/get coordinates",
      "type":"server",
      "parameters": {
        "location": "$location"
      },
      "result_variable": "context.my_coordinates",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

La variabile di contesto `$my_coordinates` salva i due valori restituiti dal servizio `get coordinates` in questo modo:

```json
  {
 "lat":"42.3611",
 "long":"-71.0571"
}
```
{: codeblock}

Questo esempio mostra come utilizzare l'azione `forecast` {{site.data.keyword.openwhisk_short}} definita nel [pacchetto Weather ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/docs/openwhisk/openwhisk_weather.html#openwhisk_catalog_weather){: new_window} fornito con il servizio {{site.data.keyword.openwhisk_short}}. L'azione prevede le coordinate di latitudine e di longitudine ed un periodo di tempo. Restituisce un oggetto JSON con le informazioni sulle previsioni per la posizione specificata nel periodo di tempo indicato. Le coordinate, restituite dall'azione precedente, vengono specificate come `$my_coordinates.lat` e `$my_coordinates.long`.

 ``` json
{
  "actions": [
    {
      "name": "/whisk.system/weather/forecast",
      "type":"server",
      "parameters": {
        "latitude": "$my_coordinates.lat",
        "longitude": "$my_coordinates.long",
        "timePeriod": "$period",
        "username": "$private.my_weather_service.username",
        "password": "$private.my_weather_service.password"
      },
      "result_variable": "context.forecasts",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

**Nota**: I valori di username e password vengono elencati come parametri. Sono presenti solamente perché questa particolare azione li richiede; insieme definiscono le credenziali richieste dal servizio meteo esterno che l'azione fornita richiama nel backend. Sono diverse dalle credenziali dell'account IBM Cloud Function. Intraprendi le azioni necessarie per proteggere anche queste credenziali.

Ora i nodi di dialogo successivi possono accedere all'output dell'azione {{site.data.keyword.openwhisk_short}}, memorizzata nella variabile `context.forecasts`.

 ``` json
{
  "output":{
    "text": {
      "values": [
        "For the next $period in $location, you can expect $forecasts."
      ]
    }
  }
}
```
{: codeblock}

Il seguente diagramma mostra un'interazione complessa. Un utente chiede una previsione meteorologica. Gli slot nel nodo di dialogo richiedono all'utente le informazioni sulla posizione e sul periodo di tempo. Per richiamare il servizio previsioni {{site.data.keyword.openwhisk_short}}, devi fornire le coordinate geografiche. Pertanto, il dialogo acquisisce innanzitutto i dettagli di latitudine e longitudine per la posizione fornita dall'utente inviando una richiesta al servizio {{site.data.keyword.openwhisk_short}} che la passa ad un servizio esterno che restituisce le informazioni sulle coordinate. In un nodo successivo, il dialogo invia le informazioni sulle coordinate appena ottenute insieme a quelle sul periodo di tempo acquisite dall'utente al servizio previsioni {{site.data.keyword.openwhisk_short}} integrato per acquisire i dettagli delle previsioni. Il dialogo quindi risponde alla domanda dell'utente mostrando il risultato delle previsioni fornito dal servizio previsioni {{site.data.keyword.openwhisk_short}}.

![Mostra una persona che chiede una previsione meteorologica e il dialogo che invia la richiesta al servizio Cloud Function per passarla al servizio esterno.](images/forecast-via-cf.png)
