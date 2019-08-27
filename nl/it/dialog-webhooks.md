---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-09"

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

# Esecuzione di una chiamata programmatica da un nodo di dialogo
{: #dialog-webhooks}

Per eseguire una chiamata programmatica, definisci un webhook che invia un callout della richiesta POST a un'applicazione esterna che esegue una funzione programmatica. Poi puoi richiamare il webhook da uno o più nodi di dialogo. 

Un webhook è un meccanismo che ti consente di eseguire il callout a un programma esterno basato su cosa sta accadendo nel tuo programma. Quando viene utilizzato in una capacità di dialogo, un webhook viene attivato quando l'assistente elabora un nodo che ha un webhook abilitato. Il webhook raccoglie i dati che specifichi o che raccogli dall'utente durante la conversazione e che salvi nelle variabili di contesto. Invia i dati come parte di una richiesta POST HTTP all'URL che specifichi come parte della tua definizione di webhook. L'URL che riceve il webhook è il listener. Esegue un'azione predefinita utilizzando le informazioni che gli passi come specificato nella definizione di webhook e può, facoltativamente, restituire una risposta.

Guarda questo video per ulteriori informazioni.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Demo webhook" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/j8TBqD2rx2o?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Puoi utilizzare un webhook per eseguire i seguenti tipi di operazione: 

- Convalidare le informazioni che hai raccolto dall'utente.
- Interagire con un servizio web esterno per acquisire informazioni. Ad esempio, potresti controllare l'orario di arrivo previsto per un volo da un servizio di traffico aereo oppure ottenere una previsione da un servizio meteorologico.
- Inviare le richieste a un'applicazione esterna, ad esempio un sito di prenotazione al ristorante, per completare una semplice transazione al posto dell'utente.
- Attivare una notifica SMS.
- Attivare un'azione web {{site.data.keyword.openwhisk}}.

Non puoi utilizzare un webhook per richiamare un'azione {{site.data.keyword.openwhisk_short}} che utilizza l'autenticazione IAM (Identity and Access Management) basata su token.
{: note}

Per informazioni su come richiamare un'applicazione client, vedi [Richiamo di un'applicazione client da un nodo di dialogo](/docs/services/assistant?topic=assistant-dialog-actions-client).

## Definizione del webhook
{: #dialog-webhooks-create}

Puoi definire un URL webhook per una capacità di dialogo e poi richiamare il webhook da uno o più nodi di dialogo. 

La chiamata programmatica al servizio esterno deve soddisfare questi requisiti:

- La chiamata deve essere una richiesta POST HTTP.
- La richiesta e la risposta devono essere nel formato JSON. Ad esempio: `Content-Type: application/json`.
- La chiamata deve essere restituita entro **massimo 5 secondi**.

  Per i servizi meno efficienti che devi richiamare, puoi gestire la chiamata tramite un'applicazione client e passare le informazioni al dialogo come passo separato. Per ulteriori informazioni, vedi [Richiamo di un'applicazione client da un nodo di dialogo](/docs/services/assistant?topic=assistant-dialog-actions-client).
  {: tip}

Per aggiungere i dettagli webhook, completa i seguenti passi:

1.  Dalla capacità in cui vuoi aggiungere il webhook, fai clic sulla scheda **Options**.

1.  Fai clic su **Webhooks**.

1.  Nel campo **URL**, aggiungi l'URL per l'applicazione esterna a cui vuoi inviare i callout della richiesta POST HTTP. 

    Ad esempio, per richiamare il servizio Language Translator, specifica l'URL per la tua istanza del servizio. 

    ```bash
    https://gateway.watsonplatform.net/language-translator/api/v3/translate?version=2018-05-01
    ```
    {: codeblock}

    Se l'applicazione esterna che richiami restituisce una risposta, deve essere in grado di reinviare una risposta in formato JSON. Per il servizio Language Translator, ad esempio, devi specificare il formato in cui vuoi che venga restituito il risultato. Puoi eseguire questa operazione passando un'intestazione al servizio. 

1.  Nella sezione Headers, aggiungi le intestazioni che vuoi passare al servizio una alla volta facendo clic su **Add header**.

    Ad esempio, aggiungi un'intestazione che indichi che vuoi che il valore risultante venga restituito in formato JSON.

    <table>
    <caption>Esempio di intestazione</caption>
      <tr>
      <th>Nome intestazione</th>
      <th>Valore intestazione</th>
      </tr>
      <tr>
      <td>Content-Type</td>
      <td>application/json</td>
      </tr>
    </table>
    
1.  Se il servizio esterno richiede che passi le credenziali di autenticazione di base con la richiesta, forniscile. Fai clic su **Add authorization**, aggiungi le tue credenziali ai campi **User name** e **Password** e poi fai clic su **Save**. 

    Il prodotto crea una stringa ASCII con codifica base-64 dalle credenziali e genera un'intestazione che la aggiunge alla pagina per te.  

    <table>
    <caption>Esempio di intestazione</caption>
      <tr>
      <th>Nome intestazione</th>
      <th>Valore intestazione</th>
      </tr>
      <tr>
      <td>Authorization</td>
      <td>Basic `<encoded-api-key>`</td>
      </tr>
    </table>
    
    Per scopi di test, puoi inoltrare una chiave API nell'autenticazione di base. Fai clic su **Add authorization** e poi aggiungi `apikey` al campo **User name** e incolla il valore della chiave API nel campo **Password**. Fai clic su **Salva**.
    {: tip}

I dettagli del tuo webhook vengono salvati automaticamente. 

## Aggiunta di un callout webhook a un nodo di dialogo
{: #dialog-webhooks-dialog-node-callout}

Per utilizzare un webhook da un nodo di dialogo, devi abilitare i webhook sul nodo e poi aggiungere i dettagli per il callout.

1.  Fai clic sulla scheda **Dialog**.

1.  Trova un nodo di dialogo in cui vuoi aggiungere un callout. Il callout al webhook si verificherà ogni volta che questo nodo viene attivato durante una conversazione con un utente. 

    Ad esempio, potresti voler inviare un callout al webhook dal nodo `#General_Greetings`.

1.  Fai clic per aprire il nodo di dialogo e poi fai clic su **Customize**.

1.  Scorri alla sezione *Webhooks* e imposta l'interruttore su **On** e poi fai clic su **Apply**.

    Se non è stata già abilitata, l'impostazione *Multiple conditional responses* viene attivata automaticamente e non puoi disabilitarla. Questa impostazione viene abilitata per supportare l'aggiunta di diverse risposte a seconda della riuscita o meno della chiamata Webhook. Se avevi già una risposta specificata per il nodo, diventa la prima risposta condizionale.
    {: note}

1.  Aggiungi i dati che vuoi passare all'applicazione esterna come coppie chiave e valore nella sezione *Parameters*.

    Ad esempio, se richiami il servizio Language Translator, devi fornire i valori per i seguenti parametri: 

    <table>
    <caption>Esempio di parametro</caption>
      <tr>
        <th>Chiave</th>
        <th>Valore</th>
        <th>Descrizione</th>
      </tr>
      <tr>
        <td>model_id</td>
        <td>"en-es"</td>
        <th>Identifica le lingue di input e di output. In questo esempio, la richiesta è per il testo in inglese (en) da tradurre in spagnolo (es).</th>
      </tr>
      <tr>
        <td>testo</td>
        <td>"How are you?"</td>
        <th>Questo parametro contiene la stringa di testo che vuoi che venga tradotta dal servizio. Puoi codificare nel programma questo valore, passare una variabile di contesto, ad esempio $saved_text, oppure passare l'input utente direttamente al servizio, specificando `<? input.text ?>` come questo valore. </th>
      </tr>
    </table>

    In casi di utilizzo più complessi, potresti raccogliere le informazioni durante una conversazione con un utente sui suoi piani di viaggio, ad esempio. Puoi raccogliere le informazioni sulle date e sulla destinazione e salvarle in variabili di contesto che puoi passare a un'applicazione esterna come parametri. 

    <table>
    <caption>Esempio di parametri di viaggio</caption>
      <tr>
        <th>Chiave</th>
        <th>Valore</th>
      </tr>
      <tr>
        <td>depart_date</td>
        <td>$departure</td>
      </tr>
    <tr>
        <td>arrive_date</td>
        <td>$arrival</td>
      </tr>
    <tr>
        <td>origin</td>
        <td>$origin</td>
      </tr>
    <tr>
        <td>destination</td>
        <td>$destination</td>
      </tr>
    </table>

1.  Qualsiasi risposta effettuata dal callout viene salvata nella variabile di ritorno. Puoi ridenominare la variabile che viene aggiunta automaticamente al campo **Return variable** per te. Se il callout genera un errore, questa variabile viene impostata su `null`.

    Il nome variabile generato ha la sintassi `webhook_result_n`, dove il valore `_n` accodato viene incrementato ogni volta che aggiungi un callout webhook a un nodo di dialogo. Questa convenzione di denominazione garantisce che i nomi delle variabili di contesto siano univoci nella capacità di dialogo. Se modifichi il nome, assicurati di utilizzare un nome univoco. 

1.  Nella sezione delle risposte condizionali, vengono salvate automaticamente due risposte, una risposta da mostrare quando il callout webhook ha esito positivo e viene reinviata una variabile di ritorno. E una risposta da mostrare quando il callout ha esito negativo. Puoi modificare queste risposte e aggiungere altre risposte condizionali al nodo. 

    Se il callout restituisce una risposta e conosci il formato della risposta JSON, puoi modificare la risposta del nodo di dialogo per includere solo la sezione della risposta che vuoi condividere con gli utenti.  
    
    Ad esempio, il servizio Language Translator restituisce un oggetto come questo:
    
    ```json
       {
       "translations":[
          {"translation":"¿Cómo estás?"}
       ],
       "word_count":3,
       "character_count":12
       }
    ```
    {: codeblock}
    
    Utilizza un'espressione SpEL che estrae solo il valore di testo tradotto. 

    <table>
    <caption>Esempio di risposte condizionali</caption>
      <tr>
        <th>Condizione</th>
        <th>Risposta</th>
      </tr>
      <tr>
        <td>$webhook_result_1</td>
        <td>Your words in Spanish: <? $webhook_result_1.translations[0].translation ?>.</td>
      </tr>
      <tr>
        <td>anything_else</td>
        <td>The call to the external application failed. Please try again later.</td>
      </tr>
    </table>

    Se utilizzi il formato consigliato per la risposta e viene restituita la risposta di traduzione sopra citata, la risposta dell'assistente all'utente sarebbe: `Your words in Spanish: ¿Cómo estás?`

1.  Una volta terminato, fai clic sulla X per chiudere il nodo. Le tue modifiche vengono salvate automaticamente. 

## Verifica dei webhook
{: #dialog-webhooks-test}

Quando aggiungi un callout webhook per la prima volta, può essere utile vedere esattamente cosa viene restituito nella risposta dall'applicazione esterna, i dati e il loro formato. Per eseguire tale operazione, aggiungi questa espressione come risposta di testo per la risposta condizionale riuscita del callout: `$webhook_result_n` dove `n` è il numero appropriato per il webhook che stai verificando.

Questa risposta restituisce il corpo completo della variabile di ritorno, quindi puoi vedere cosa sta reinviando il callout e decidere cosa condividere con gli utenti. Puoi quindi utilizzare i metodi documentati in [Metodi del linguaggio delle espressioni](/docs/services/assistant?topic=assistant-dialog-methods) per estrarre solo le informazioni che ti interessano dalla risposta.

Verifica se determinati input utente possono generare errori nel callout e crea modi per gestire tali situazioni. Gli errori generati dall'applicazione esterna vengono archiviati in `output.webhook_error.<result_variable>`. Puoi utilizzare una risposta condizionale come questa mentre stai verificando l'acquisizione di questo tipo di errori: 

| Condizione | Risposta |
|-----------|----------|
| output.webhook_error | The callout generated this error: <? output.webhook_error.webhook_result_1 ?>. |

Ad esempio, è possibile che tu non stia autenticando correttamente la richiesta (401) oppure che tu stia tentando di passare un parametro con un nome già utilizzato dall'applicazione esterna. Verifica il webhook per rilevare e correggere questi tipi di errori prima di distribuire il webhook.

## Rimozione di un webhook
{: #dialog-webhooks-delete}

Se decidi di non voler effettuare una chiamata webhook da un nodo di dialogo, apri la pagina *Customize* del nodo e poi imposta i Webhook su **Off**.

La sezione *Parameters* e il campo **Return variable** vengono rimossi dall'editor del nodo di dialogo. Tuttavia, le risposte condizionali aggiunte per te o che hai aggiunto da solo rimangono. 

La sezione *Multiple conditioned responses* è di nuovo modificabile. Puoi scegliere di disattivare la funzione. Se lo fai, solo la prima risposta condizionale viene salvata come unica risposta di testo del nodo. 

Per modificare il servizio esterno che hai richiamato dai nodi di dialogo, modifica i dettagli webhook definiti nella pagina Webhooks della scheda **Options**. Se il nuovo servizio prevede che gli vengano passati parametri diversi, assicurati di aggiornare i nodi di dialogo che lo richiamano. 

## Richiamo di IBM Cloud Functions
{: #dialog-webhooks-cf}

Scrivi l'URL webhook e fornisci intestazioni in modo diverso in base al fatto che tu stia richiamando un'azione standard o un'azione web. 

### Richiamo di un'azione web. 
{: #dialog-webhooks-cf-web-action}

I seguenti suggerimenti ti aiutano a richiamare un'azione web {{site.data.keyword.openwhisk_short}} dal tuo dialogo. 

1.  Apri la pagina **Options** per la capacità e poi fai clic su **Webhooks**.

1.  Nel campo **URL**, aggiungi l'URL per l'applicazione esterna a cui vuoi inviare i callout della richiesta POST HTTP. 

    Ad esempio, per richiamare un'azione web {{site.data.keyword.openwhisk_short}}, specifica l'URL per l'azione web pubblica. Ad esempio:

    ```bash
    https://us-south.functions.cloud.ibm.com/api/v1/web/my_org_dev/default/Hello%20World.json
    ```
    {: codeblock}

    Se l'applicazione esterna che richiami restituisce una risposta, deve essere in grado di reinviare una risposta in formato JSON. 

    Nota che l'URL della richiesta in questo esempio termina con `.json`. Specificando questa estensione, usufruisci di una funzione delle azioni web che ti consente di specificare il tipo di contenuto desiderato della risposta. Specificando questo tipo di estensione ti assicuri che, se le azioni web possono restituire risposte in più di un formato, verrà restituita una risposta JSON. Per ulteriori dettagli, vedi [Funzioni aggiuntive](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_extra){: new_window}.
    {: tip}

1.  Non hai bisogno di aggiungere intestazioni. 

    Le azioni web {{site.data.keyword.openwhisk_short}} non devono essere autenticate, quindi non hai bisogno di definire un'intestazione Authorization.

    I dettagli del tuo webhook vengono salvati automaticamente. 

1.  Fai clic sulla scheda **Dialog**.

1.  Fai clic per aprire il nodo di dialogo da cui vuoi richiamare l'azione web e poi fai clic su **Customize**.

1.  Scorri alla sezione *Webhooks* e imposta l'interruttore su **On** e poi fai clic su **Apply**.

1.  Aggiungi i dati che vuoi passare all'applicazione esterna come coppie chiave e valore nella sezione *Parameters*.

    Ad esempio, se richiami l'azione web Hello World {{site.data.keyword.openwhisk_short}}, potresti voler aggiungere le seguenti informazioni da passare nel parametro del messaggio accettato da tale applicazione: 

    <table>
    <caption>Esempio di parametro</caption>
      <tr>
        <th>Chiave</th>
        <th>Valore</th>
      </tr>
      <tr>
        <td>message</td>
        <td>"hello"</td>
      </tr>
    </table>

    Quando richiami un'azione web {{site.data.keyword.openwhisk_short}}, non puoi passare i parametri con la stessa chiave dei parametri definiti come parte dell'azione web. Per ulteriori dettagli, vedi [Parametri protetti](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_protect){: new_window}.
    {: note}

1.  Puoi modificare la risposta del nodo di dialogo per includere solo la sezione della risposta che vuoi mostrare agli utenti.  

    L'azione web Hello World {{site.data.keyword.openwhisk_short}} include una coppia nome e valore del messaggio nella sua risposta, insieme ad altre informazioni. Per mostrare solo il contenuto del messaggio, puoi utilizzare la sintassi `<return-variable>.message` per estrarre la sezione del messaggio solo dall'oggetto di risposta. 

    <table>
    <caption>Esempio di risposte condizionali</caption>
      <tr>
        <th>Condizione</th>
        <th>Risposta</th>
      </tr>
      <tr>
        <td>$webhook_result_1</td>
        <td>The application returned "$webhook_result_1.message".</td>
      </tr>
      <tr>
        <td>anything_else</td>
        <td>The call to the external application failed. Please try again later.</td>
      </tr>
    </table>

1.  Una volta terminato, fai clic sulla X per chiudere il nodo. Le tue modifiche vengono salvate automaticamente. 

### Richiamo di un'azione standard
{: #dialog-webhooks-cf-action}

Puoi effettuare una chiamata a un'azione gestita da Cloud Foundry, ma non a un'azione che utilizza l'autenticazione IAM (Identity and Access Management) basata su token. 

Le azioni {{site.data.keyword.openwhisk_short}} supportano sia le chiamate sincrone o asincrone. Tuttavia, non puoi effettuare una chiamata asincrona a un'azione {{site.data.keyword.openwhisk_short}} con un webhook. Quando invii una richiesta asincrona, viene restituito solo un ID di attivazione. 

Per effettuare una chiamata sincrona a un'azione {{site.data.keyword.openwhisk_short}} gestita da Cloud Foundry, completa i seguenti passi:

1.  Dalla capacità in cui vuoi aggiungere il webhook, fai clic sulla scheda **Options**.

1.  Fai clic su **Webhooks**.

1.  Nel campo **URL**, specifica l'URL per l'azione. 

    Accoda un parametro `?blocking=true` all'URL dell'azione per forzare l'esecuzione di una chiamata sincrona. 

    Questa sintassi invia una richiesta sincrona:

    ```bash
    https://us-south.functions.cloud.ibm.com/api/v1/namespaces/my_org_dev/actions/Hello%20World?blocking=true
    ```
    {: codeblock}

    Quando richiami un'azione, devi fornire la chiave API associata all'azione con un'intestazione, ciò viene descritto nel prossimo passo. 

1.  Fornisci i dettagli di autorizzazione con la richiesta.

    Per richiamare un'azione {{site.data.keyword.openwhisk_short}} gestita da Cloud Foundry, completa i seguenti passi:

    1.  Ottieni la chiave API. Dalla pagina {{site.data.keyword.openwhisk_short}} Endpoint, trova la sezione CURL e fai clic sull'icona a forma di occhio per mostrare i dettagli della chiave. Copia la chiave `user ID:password` elencata dopo il parametro `-u` nel comando curl.
    
    1.  Fai clic su **Add authorization**, aggiungi le tue credenziali ai campi **User name** e **Password** e poi fai clic su **Save**. 

    Le credenziali sono codificate e un'intestazione viene generata e aggiunta alla pagina per te.  
    
    ![Mostra il campo URL e la sezione Headers della pagina Options.](images/webhook-to-cfaction.png)

    <!-- - If you are calling a {{site.data.keyword.openwhisk_short}} action that is managed by IBM Cloud Identity and Access Management (IAM) instead of CLoud Foundry, then you must provide an IAM bearer token to authenticate the request. 
    
      1. Follow the instructions in [Passing an IBM Cloud IAM token to authenticate with a service's API](/docs/iam?topic=iam-iamapikeysforservices#token_auth). 
      
      1. To pass the IAM bearer token, use a header like this:
    
         <table>
         <caption>Header example</caption>
           <tr>
             <th>Header name</th>
             <th>Header value</th>
           </tr>
           <tr>
             <td>Authorization</td>
             <td>Bearer `<IAM token>`</td>
           </tr>
         </table>

        For more information about how to manage IAM tokens, see this [IBM Developer article](https://developer.ibm.com/tutorials/accessing-iam-based-services-from-ibm-cloud-functions/){: external}.
    -->
    I dettagli del tuo webhook vengono salvati automaticamente. 

1.  Fai clic sulla scheda **Dialog**.

1.  Fai clic per aprire il nodo di dialogo da cui vuoi richiamare l'azione web e poi fai clic su **Customize**.

1.  Scorri alla sezione *Webhooks* e imposta l'interruttore su **On** e poi fai clic su **Apply**.

1.  Aggiungi i dati che vuoi passare all'applicazione esterna come coppie chiave e valore nella sezione *Parameters*.

    Quando richiami un'azione {{site.data.keyword.openwhisk_short}}, *puoi* passare i parametri con la stessa chiave dei parametri definiti come parte dell'azione. I parametri non sono riservati come quelli per le azioni web. 

1.  Puoi modificare la risposta del nodo di dialogo per includere solo la sezione della risposta che vuoi mostrare agli utenti.  

    Ad esempio, nella sezione delle risposte condizionali, puoi utilizzare un'espressione con la sintassi `$webhook_result.response.result.message` per estrarre solo il messaggio restituito. 

    <table>
    <caption>Esempio di risposte condizionali</caption>
      <tr>
        <th>Condizione</th>
        <th>Risposta</th>
      </tr>
      <tr>
        <td>$webhook_result_1</td>
        <td>The application returned "$webhook_result.response.result.message".</td>
      </tr>
      <tr>
        <td>anything_else</td>
        <td>The call to the external application failed. Please try again later.</td>
      </tr>
    </table>

1.  Una volta terminato, fai clic sulla X per chiudere il nodo. Le tue modifiche vengono salvate automaticamente. 
