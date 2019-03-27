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
{:gif: data-image-type='gif'}

# Suggerimenti per la creazione del dialogo
{: #dialog-tips}

Apprendi come approcciarti alla creazione di un dialogo e come ottenere alcuni suggerimenti per completare passi più complessi.
{: shortdesc}

Esamina questi suggerimenti di designer del dialogo esperti. 

## Pianificazione dell'intero dialogo
{: #dialog-tips-plan}

- Pianifica la progettazione del dialogo che desideri creare prima di aggiungere un singolo nodo di dialogo nello strumento. Abbozzalo su carta se necessario. 
- Quando possibile, basa le tue decisioni di progettazione su dati provenienti da prove e comportamenti reali. Non aggiungere nodi per gestire una situazione che qualcuno *pensa* possa verificarsi. 
- Evita di copiare processi aziendali così come sono. Raramente sono discorsivi. 
- Se delle persone utilizzano già un processo, esamina il modo in cui vi si approcciano. Di norma, le persone ottimizzano il processo da una prospettiva colloquiale. 
- Decidi il tono, la personalità e il posizionamento del tuo assistente. Queste scelte devono riflettersi coerentemente nel dialogo che hai creato. 
- Non far apparire mai l'assistente come una persona. Se gli utenti credono che l'assistente è una persona e poi si rendono conto che non lo è, è probabile che non si fidino. 
- Non tutto può essere considerato una conversazione. A volte un formato web funziona meglio. 

## Aggiunta di nodi
{: #dialog-tips-nodes}

- Aggiungi un nome nodo che descrive lo scopo del nodo. 

  Al momento sai quali operazioni vengono svolte dal tuo nodo, ma tra alcuni mesi potresti non saperlo. Il tuo io futuro e i membri del tuo team ti ringrazieranno per avere aggiunto un nome nodo descrittivo. E il nome nodo viene visualizzato nel log, ciò può esserti utile per eseguire il debug di una conversazione in un secondo momento. 
- Per raccogliere le informazioni necessarie per eseguire un'attività, prova a utilizzare un nodo con slot invece di un bel po' di nodi separati per carpire le informazioni agli utenti. Vedi [Raccolta di informazioni con gli slot](/docs/services/assistant?topic=assistant-dialog-slots).
- Per un flusso del processo complesso, indica agli utenti quali informazioni devono fornire all'inizio del processo. 
- Comprendi in che modo il servizio passa attraverso la struttura ad albero di dialogo e l'impatto che cartelle, rami, passaggi e digressioni hanno sulla rotta. Vedi [Flusso di dialogo](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-flow).
- Non aggiungere passaggi ovunque. Aumentano la complessità del flusso di dialogo e rendono più difficile eseguire il debug del dialogo in un secondo momento. 
- Per passare a un nodo nello stesso ramo del nodo corrente, utilizza *Ignora input utente* invece di *Passa a*.

  Questa scelta ti evita di dover modificare le impostazioni del nodo corrente quando rimuovi o riordini i nodi figlio a cui passare. Vedi [Definizione delle operazioni successive](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-jump-to).
- Prima di consentire la generazione di digressioni da un nodo, verifica gli scenari utente più comuni. E assicurati che presumibilmente i nodi da cui generare la digressione siano configurati per il ritorno. Vedi [Digressioni](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions).

## Aggiunta di risposte
{: #dialog-tips-responses}

- Utilizza risposte brevi e utili. 
- Nella risposta, rifletti l'intento dell'utente. 

  In questo modo, assicuri agli utenti che il bot li sta comprendendo oppure, se non lo sta facendo, dai loro la possibilità di risolvere subito l'incomprensione. 
- Includi solo link a siti esterni nelle risposte se la risposta dipende da dati che cambiano di frequente. 
- Evita di utilizzare eccessivamente i pulsanti. Incoraggiare gli utenti a scegliere le opzioni predefinite da una serie di pulsanti rende meno reale la conversazione e riduce la tua capacità di capire cosa vogliono veramente fare gli utenti. Quando consenti agli utenti di porre domande con parole loro, puoi utilizzare l'input per addestrare il sistema e ricavare intenti migliori. 
- Evita di utilizzare un bel po' di nodi quando ne basta uno. Ad esempio, aggiungi più risposte condizionali a un singolo nodo per restituire risposte diverse a seconda dei dettagli forniti dall'utente. Vedi [Risposte condizionali](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multiple).
- Formula attentamente le tue risposte. Puoi cambiare il modo in cui una persona reagisce al tuo sistema semplicemente in base a come formuli una risposta. La modifica di una riga del testo può evitarti di dover scrivere più righe di codice per implementare una soluzione programmatica complessa. 
- Esegui frequentemente il backup della tua capacità. Vedi [Scaricamento di una capacità](/docs/services/assistant?topic=assistant-skill-add#skill-add-download).

## Suggerimenti per l'acquisizione delle informazioni dall'input utente
{: #dialog-tips-user-input}

Può essere difficile sapere quale sintassi utilizzare nel tuo nodo di dialogo per acquisire con precisione le informazioni che desideri trovare nell'input utente. Di seguito troverai alcuni approcci da utilizzare per occuparti di obiettivi comuni. 

- **Restituzione dell'input dell'utente**: puoi acquisire il testo esatto espresso dall'utente e restituirlo nella tua risposta. Utilizza la seguente espressione SpEL in una risposta per ripetere il testo che l'utente ha specificato nella risposta:

  `You said: <? input.text ?>.`

- **Determinazione del numero di parole nell'input utente**: puoi eseguire uno qualsiasi dei metodi Stringa supportati sull'oggetto input.text. Ad esempio, puoi scoprire quante parole ci sono in un'espressione utente utilizzando la seguente espressione SpEL:

  `input.text.split(' ').size()`

  Vedi [Metodi del linguaggio delle espressioni per Stringa](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings) per informazioni su altri metodi che puoi utilizzare. 

- **Gestione di più intenti**: un utente immette un input in cui desidera completare due attività separate. `I want to open a savings account and apply for a credit card.` In che modo il dialogo riconosce e fa fronte a entrambe? Vedi la voce [Compound questions](https://sodoherty.ai/2017/02/06/compound-questions/){: new_window} nel blog di Simon O'Doherty per le strategie che puoi provare a utilizzare. (Simon è uno sviluppatore del team {{site.data.keyword.conversationshort}}.)

- **Gestione degli intenti ambigui**: un utente immette un input che esprime una richiesta sufficientemente ambigua da consentire al servizio di trovare due o più nodi con intenti che potrebbero potenzialmente occuparsene. In che modo il dialogo stabilisce quale ramo di dialogo seguire? Se abiliti la disambiguazione, questa può mostrare agli utenti le loro opzioni e chiedere loro di selezionare quella corretta. Per ulteriori dettagli, vedi [Disambiguazione](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).

- **Gestione di più entità nell'input**: se vuoi valutare solo il valore della prima istanza rilevata di un tipo di entità, puoi utilizzare la sintassi  `@entity == 'specific-value'` invece del formato `@entity:(specific-value)`.

  Ad esempio, quando utilizzi `@appliance == 'air conditioner'`, valuti solo il valore della prima entità `@appliance` rilevata. Ma, l'utilizzo di `@appliance:(air conditioner)` viene esteso in `entity['appliance'].contains('air conditioner')`, che corrisponde ogni volta che viene rilevata almeno un'entità `@appliance` di valore 'air conditioner' nell'input utente.

## Suggerimenti sull'utilizzo delle condizioni
{: #dialog-tips-condition-usage}

- **Controllo dei valori con caratteri speciali**: se desideri controllare se un'entità o una variabile di contesto contiene un valore e se il valore include un carattere speciale, ad esempio un apostrofo ('), devi racchiudere tra parentesi il valore che desideri controllare. Ad esempio, per controllare se un'entità o una variabile di contesto contiene il nome `O'Reilly`, devi racchiudere il nome tra parentesi.

  `@person:(O'Reilly)` e `$person:(O'Reilly)`

  Il servizio converte questi riferimenti abbreviati in queste espressioni SpEL complete:

  `entities['person']?.contains('O''Reilly')` e `context['person'] == 'O''Reilly'`

  SpEL utilizza un secondo apostrofo per eseguire l'escape del singolo apostrofo nel nome.
  {: note}

- **Controllo di più valori**: se desideri controllare più di un valore, puoi creare una condizione che utilizza gli operatori OR (`||`) per elencare più valori nella condizione. Ad esempio, per definire una condizione che viene valutata come true se la variabile di contesto `$state` contiene le abbreviazioni per Massachusetts, Maine o New Hampshire, puoi utilizzare questa espressione:

  `$state:MA || $state:ME || $state:NH`

- **Controllo di valori numerici**: quando confronti i numeri, assicurati innanzitutto che l'entità o la variabile che stai controllando contenga un valore. Se l'entità o la variabile non contiene un valore numerico, viene trattata come se avesse un valore null (0) in un confronto numerico. 

  Ad esempio, desideri controllare se un valore del dollaro specificato dall'utente nell'input utente è inferiore a 100. Se utilizzi la condizione `@price < 100` e l'entità `@price` è null, la condizione viene valutata come `true` perché 0 è inferiore a 100, anche se il prezzo non è stato mai impostato. Per impedire questo tipo di risultato impreciso, utilizza una condizione come `@price AND @price < 100`. Se `@price` non ha alcun valore, questa condizione restituirà correttamente false.

- **Controllo degli intenti con uno specifico modello di nome intento**: puoi utilizzare una condizione che ricerca gli intenti che corrispondono ad un modello. Ad esempio, per trovare tutti gli intenti rilevati con nomi intento che iniziano con 'User_', nella condizione puoi utilizzare una sintassi come quella riportata di seguito:

  `intents[0].intent.startsWith("User_")`

  Tuttavia, quando esegui questa operazione, vengono considerati tutti gli intenti rilevati, anche quelli con una affidabilità inferiore a 0,2. Inoltre, tieni presente che gli intenti ritenuti irrilevanti da Watson in base al loro punteggio di affidabilità non vengono restituiti. Affinché questo avvenga, modifica la condizione come segue:

  `!irrelevant && intents[0].intent.startsWith("User_")`

- **In che modo la corrispondenza fuzzy influisce sul riconoscimento dell'entità**: se utilizzi un'entità come condizione ed è abilitata la corrispondenza fuzzy, `@entity_name` viene valutato su true solo se l'affidabilità della corrispondenza è superiore al 30%.  Ossia, solo se `@entity_name.confidence > .3`.

## Memorizzazione e riconoscimento dei gruppi di entità modello nell'input
{: #dialog-tips-get-pattern-groups}

Per memorizzare il valore di un'entità modello in una variabile di contesto, aggiungi .literal al nome dell'entità. L'utilizzo di questa sintassi assicura che nella variabile venga memorizzata l'esatta parte di testo dell'input utente corrispondente al modello specificato.

| Variabile   | Valore               |
|------------|---------------------|
| email      | <? @email.literal ?> |

Per memorizzare il testo da un singolo gruppo in un'entità modello con gruppi definiti, specifica il numero di array del gruppo che desideri memorizzare. Ad esempio, presupponi che il modello di entità sia definito come segue per l'entità @phone_number. (Ricorda, le parentesi indicano i gruppi di modelli):

`\b((958)|(555))-(\d{3})-(\d{4})\b`

Per memorizzare solo il prefisso dal numero di telefono specificato nell'input utente, puoi utilizzare la seguente sintassi:

| Variabile       | Valore                         |
|----------------|-------------------------------|
| area_code      | <? @phone_number.groups[1] ?> |

I gruppi sono delimitati dall'espressione regolare utilizzata per definire il modello di gruppo. Ad esempio, se l'input utente che corrisponde al modello definito nell'entità `@phone_number` è: `958-234-3456`, verranno creati i seguenti gruppi:

| Numero gruppo | Valore motore Regex  | Valore dialogo   | Spiegazione |
|--------------|---------------------|----------------|-------------|
| groups[0]    | `958-234-3456`      | `958-234-3456` | Il primo gruppo è sempre una stringa di corrispondenza completa. |
| groups[1]    | `((958)`l`(555))`   | `958`          | La stringa che corrisponde a regex per il primo gruppo definito, che è `((958)`l`(555))`. |
| groups[2]    | `(958)`             | `958`          | La corrispondenza nel gruppo incluso come primo operando nell'espressione OR `((958)`l`(555))` |
| groups[3]    | `(555)`             | `null`         | Nessuna corrispondenza nel gruppo incluso come secondo operando nell'espressione OR `((958)`l`(555))` |
| groups[4]    | `(\d{3})`           | `234`          | La stringa che corrisponde all'espressione regolare definita per il gruppo. |
| groups[5]    | `(\d{4})`           | `3456`         | La stringa che corrisponde all'espressione regolare definita per il gruppo. |
{: caption="Dettagli del gruppo" caption-side="top"}

Per aiutarti nel decifrare quale numero di gruppo utilizzare per acquisire la sezione dell'input a cui sei interessato, puoi estrarre contemporaneamente le informazioni relative a tutti i gruppi. Utilizza la seguente sintassi per creare una variabile di contesto che restituisca un array di tutte le corrispondenze di entità modello raggruppate:

| Variabile                 | Valore                      |
|--------------------------|----------------------------|
| array_of_matched_groups  | <? @phone_number.groups ?> |

Utilizza il riquadro "Provalo" per immettere alcuni valori di numero di telefono di test. Per l'input `958-123-2345`, questa espressione imposta `$array_of_matched_groups` su `["958-123-2345","958","958",null,"123","2345"]`.

Puoi quindi conteggiare ciascun valore nell'array partendo da 0 per acquisire il numero di gruppo relativo ad esso.

| Valore elemento array | Numero elemento array |
|---------------------|----------------------|
| "958-123-2345"      | 0 |
| "958"               | 1 |
| "958"               | 2 |
| null                | 3 |
| "123"               | 4 |
| "2345"              | 5 |
{: caption="Elementi dell'array" caption-side="top"}

Dal risultato, puoi stabilire che, per acquisire le ultime quattro cifre del numero di telefono, hai bisogno, ad esempio, del gruppo #5. 

Per ritornare alla struttura JSONArray creata per rappresentare l'entità di modello raggruppata, utilizza la seguente sintassi:

| Variabile             | Valore                           |
|----------------------|---------------------------------|
| json_matched_groups  | <? @phone_number.groups_json ?> |

Questa espressione imposta `$json_matched_groups` sul seguente array JSON:

```json
[
  {"group": "group_0","location": [0, 12]},
  {"group": "group_1","location": [0, 3]},
  {"group": "group_2","location": [0, 3]},
  {"group": "group_3"},
  {"group": "group_4","location": [4, 7]},
  {"group": "group_5","location": [8, 12]}
]
```
{: codeblock}

`location` è una proprietà di un'entità che utilizza un offset di caratteri in base zero per indicare dove inizia e termina il valore di entità rilevato nel testo di input.
{: note}

Se prevedi che nell'input verranno forniti due numeri di telefono, puoi controllare due numeri di telefono. Se presenti, utilizza la seguente sintassi per acquisire, ad esempio, il prefisso del secondo numero.

| Variabile         | Valore                                       |
|------------------|---------------------------------------------|
| second_areacode  | <? entities['phone_number'][1].groups[1] ?> |

Se l'input è `I want to change my phone number from 958-234-3456 to 555-456-5678`, `$second_areacode` sarà uguale a `555`.

## Visualizzazione dei dettagli della chiamata API
{: #dialog-tips-inspect-api}

Mentre verifichi il tuo dialogo con il riquadro "Provalo", potresti voler sapere qual è l'aspetto delle chiamate API sottostanti che vengono restituite dal servizio. Puoi utilizzare gli strumenti per gli sviluppatori forniti dal tuo browser web per controllarle. 

Da Chrome, ad esempio, apri Strumenti per sviluppatori. Fai clic sullo strumento Network. La sezione Name elenca più chiamate API. Fai clic sulla chiamata message associata alla tua espressione di test e poi fai clic sulla colonna Response per vedere il corpo della risposta API. Elenca gli intenti e le entità che sono stati riconosciuti nell'input utente con i loro punteggi di affidabilità e i valori delle variabili di contesto al momento della chiamata. Per visualizzare il corpo della risposta nel formato strutturato, fai clic sulla colonna Preview.

![Mostra come visualizzare i dettagli della chiamata API utilizzando Strumenti per sviluppatori del browser web Chrome.](images/api-browser-dev.png)
