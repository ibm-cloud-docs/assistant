---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-20"

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

# Note sulla release

## Controllo versione dell'API del servizio
{: shortdesc}

Le richieste API richiedono un parametro di versione che utilizza una data nel formato `version=YYYY-MM-DD`. Ogni volta che modifichiamo l'API in modo incompatibile con le versioni precedenti, rilasciamo una nuova versione secondaria dell'API.

Invia il parametro di versione con ogni richiesta API. Il servizio utilizza la versione dell'API per la data da te specificata o la versione più recente prima di tale data. Non impostare come predefinita la data corrente. Specifica invece una data che corrisponda a una versione compatibile con la tua applicazione e non modificarla finché l'applicazione non è pronta per una versione successiva.

La versione corrente è `2018-02-16`.

Il riquadro "Provalo" negli strumenti {{site.data.keyword.conversationshort}} utilizza l'ultima versione di API.

## Funzioni beta

IBM rilascia servizi, funzioni e supporto linguistico classificati come beta per la tua valutazione. Queste funzioni potrebbero essere instabili, cambiare frequentemente e potrebbero essere sospese con breve preavviso. Inoltre, le funzioni beta potrebbero non fornire lo stesso livello di prestazioni o di compatibilità fornito generalmente dalle funzioni e non sono progettate per essere utilizzate in un ambiente di produzione. Le funzioni beta sono supportate solo su [developerWorks Answers ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/answers/topics/watson-conversation/){: new_window}.

## Modelli aggiornati
{: #updated-models}

Gli algoritmi di {{site.data.keyword.conversationshort}} possono essere periodicamente perfezionati e aggiornati sulla base di feedback, miglioramenti scientifici e fattori aggiuntivi, al fine di migliorare continuamente le prestazioni. Gli aggiornamenti al modello verranno comunicati in queste note sulla release.

I modelli esistenti che hai addestrato non saranno immediatamente interessati, ma i modelli scaduti verranno aggiornati al modello corrente, se non lo hai già fatto, dopo 60 giorni dalla disponibilità di un nuovo modello.

> **Nota:** questa istruzione di aggiornamento si applica solo alle lingue e alle funzioni genericamente disponibili (GA).

## Modifiche
{: #change-log}

Sono disponibili le seguenti nuove funzioni e modifiche al servizio.

### 16 febbraio 2018
{: #16February2018}

- **Traccia nodo di dialogo**: quando utilizzi il riquadro "Provalo" per verificare un dialogo, viene visualizzata un'icona di posizione accanto a ciascuna risposta. Puoi fare clic sull'icona per evidenziare il percorso utilizzato dal servizio nella struttura ad albero di dialogo per arrivare alla risposta. Per dettagli, vedi [Creazione di un dialogo](dialog-build.html#test).

- **Nuova versione API**: ora la versione API corrente è `2018-02-16`. Questa versione introduce le seguenti modifiche:

  - Ora, nella maggior parte delle richieste GET è supportato il nuovo parametro `include_audit`. Si tratta di un parametro booleano facoltativo che specifica se la risposta deve includere le proprietà di controllo (i valori di data/ora di `creazione` e `aggiornamento`). Il valore predefinito è `false`. (Se stai utilizzando una versione API precedente a `2018-02-16`, il valore predefinito è `true`.) Per ulteriori informazioni, vedi il [Riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.

  - Le risposte dalle chiamate API che utilizzano la nuova versione includono solo le proprietà con valori non-`null`.

  - Le proprietà `output.nodes_visited` e `output.nodes_visited_details` delle risposte al messaggio ora includono i nodi nei seguenti tipi, precedentemente omessi:

    - Nodi con `type`=`response_condition`
    - Nodi con `type`=`event_handler` e `event_name`=`input`

### 9 febbraio 2018
{: #9February2018}

- **Entità di sistema in olandese (Beta)**: il supporto della lingua olandese è stato migliorato per includere la disponibilità delle [entità di sistema](system-entities.html) nella release beta. Per i dettagli, vedi [Lingue supportate](lang-support.html).

### 29 gennaio 2018
{: #29January2018}

- L'API REST {{site.data.keyword.conversationshort}} ora supporta i nuovi parametri di richiesta:
  - Utilizza il parametro `append` quando aggiorni uno spazio di lavoro per indicare se i nuovi dati dello spazio di lavoro devono essere aggiunti ai dati esistenti invece di sostituirli. Per ulteriori informazioni, vedi [Aggiorna spazio di lavoro ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#update_workspace){: new_window}.
  - Utilizza il parametro `nodes_visited_details` quando invii un messaggio per indicare se la risposta deve includere informazioni di diagnostica aggiuntive sui nodi che sono stati visitati durante l'elaborazione del messaggio. Per ulteriori informazioni, vedi [Invia messaggio ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#send_message){: new_window}.

### 23 gennaio 2018
{: #23January2018}

- **Impossibile richiamare l'elenco degli spazi di lavoro**: se visualizzi questo messaggio o messaggi di errore simili quando ti trovi negli strumenti, potrebbe significare che la tua sessione è scaduta. Disconnettiti selezionando **Disconnetti** dall'icona **Informazioni utente** ![Menu icona Informazioni utente](images/user-icon.png) e quindi accedi nuovamente. 

### 8 dicembre 2017
{: #8December2017}

- **Accesso ai dati di log tra istanze (solo utenti Premium)**: se sei un utente {{site.data.keyword.conversationshort}} Premium, le tue istanze premium possono essere facoltativamente configurate per consentire l'accesso ai dati di log dagli spazi di lavoro tra diverse istanze premium. 

- **Copia nodi**: ora puoi duplicare un nodo per creare una copia sua e dei suoi figli. Questa funzione è utile se crei un nodo con una logica utile che desideri riutilizzare altrove nel dialogo. Per ulteriori informazioni, vedi [Copia di un nodo di dialogo](dialog-build.html#copy-node). 

- **Gruppi di acquisizione in entità modello**: puoi identificare i gruppi nel modello di espressione regolare che definisci per un'entità. L'identificazione dei gruppi è utile se desideri poter fare riferimento ad una sottosezione del modello in un secondo momento. Ad esempio, la tua entità potrebbe avere un modello regex che acquisisce numeri di telefono statunitensi. Se identifichi il segmento del prefisso del modello di numero come un gruppo, puoi successivamente fare riferimento a tale gruppo per accedere solo al segmento del prefisso di un numero di telefono. Per ulteriori informazioni, vedi [Definizione di entità](entities.html#creating-entities). 

### 6 dicembre 2017
{: #6December2017}

- **Integrazione {{site.data.keyword.openwhisk}} (Beta)**: chiamata delle azioni {{site.data.keyword.openwhisk}} (in precedenza IBM OpenWhisk) direttamente da un nodo di dialogo. Questa funzione ti consente, ad esempio, di richiamare un'azione per recuperare le informazioni meteo da un nodo di dialogo e quindi creare una condizione sulle informazioni restituite nella risposta di dialogo. Attualmente, puoi richiamare un'azione da un'istanza {{site.data.keyword.openwhisk_short}} che è ospitata nella regione Stati Uniti del Sud dalle istanze {{site.data.keyword.conversationshort}} ospitate nella regione Stati Uniti del Sud. Per ulteriori dettagli, vedi [Esecuzione di chiamate programmatiche da un nodo di dialogo](dialog-actions.html).

### 5 dicembre 2017
{: #5December2017}

- **IU riprogettata per Intenti ed Entità**: le schede `Intenti` ed `Entità` sono state riprogettate per fornire un flusso di lavoro più semplice e più efficiente quando crei e modifichi entità e intenti. Vedi [Definizione di intenti](intents.html#defining-intents) e [Definizione di entità](entities.html#defining-entities) per informazioni su come utilizzare queste schede. 

### 30 novembre 2017
{: #30November2017}

- **Supporto numeri arabi orientali**: i numeri arabi orientali sono ora supportati nelle entità di sistema in arabo. 

### 29 novembre 2017
{: #29November2017}

- **Miglioramento della comprensione dell'input utente tra spazi di lavoro**: ora puoi migliorare uno spazio di lavoro con espressioni che sono state inviate agli altri spazi di lavoro all'interno della tua istanza. Ad esempio, potresti avere più versioni degli spazi di lavoro di produzione e di quelli di sviluppo; puoi utilizzare gli stessi dati di espressione per migliorare uno qualsiasi di questi spazi di lavoro. Per ulteriori informazioni, vedi [Miglioramento tra spazi di lavoro](logs.html#deploy_id) e [Selezione di un'origine dati](logs_convo.html#select-source). 

### 20 novembre 2017
{: #20November2017}

- **Conformità GB18030**: GB18030 è uno standard cinese che specifica una code page estesa da utilizzare nel mercato cinese. Questo standard di code page è importante il settore software in quanto China National Information Technology Standardization Technical Committee ha ordinato che qualsiasi applicazione software rilasciata per il mercato cinese dopo il 1° settembre 2001, deve essere abilitata per GB18030. Il servizio {{site.data.keyword.conversationshort}} supporta questa codifica e viene certificato come conforme a GB18030. 

### 9 novembre 2017
{: #9November2017}

- **Gli esempi di intento possono fare direttamente riferimento alle entità**: ora puoi specificare un riferimento di identità direttamente in un esempio di intento. Tale riferimento di entità, insieme a tutti i relativi valori o sinonimi, viene utilizzato dal classificatore del servizio {{site.data.keyword.conversationshort}} per addestrare l'intento. Per ulteriori informazioni, vedi [*Entità come esempio*](intents.html#entity-as-example) nell'argomento [Intenti](intents.html).

  **Nota**: al momento, puoi fare direttamente riferimento solo alle entità chiuse che hai definito. Non puoi fare riferimento direttamente alle [entità modello](entities.html#pattern-entities) o alle [entità di sistema](system-entities.html).

### 8 novembre 2017
{: #8November2017}

- **Connettore {{site.data.keyword.conversationshort}}**: puoi utilizzare il nuovo strumento connettore {{site.data.keyword.conversationshort}} per connettere il tuo spazio di lavoro ad una app Slack o Facebook Messenger di cui disponi, rendendolo disponibile come una chatbot con cui possono interagire gli utenti Slack o Facebook Messenger. Questo strumento è disponibile solo per la regione Stati Uniti Sud di {{site.data.keyword.Bluemix_notm}}. Per ulteriori informazioni, vedi [Distribuzione a un canale con il connettore {{site.data.keyword.conversationshort}}](conversation-connector.html).

### 3 novembre 2017
{: #3November2017}

- **Aggiornamenti al dialogo**: i seguenti aggiornamenti semplificano la creazione di un dialogo. (Per dettagli, vedi [Creazione di un dialogo](dialog-build.html).)

    - Puoi aggiungere una condizione ad uno slot per renderlo obbligatorio solo se vengono soddisfatte determinate condizioni. Ad esempio, puoi creare uno slot che richieda il nome di una sposa obbligatorio solo se uno slot precedente (obbligatorio) che richiede lo stato civile indica che l'utente è sposato. 

    - Ora puoi scegliere **Ignora input utente** come passo successivo per un nodo. Quando scegli questa opzione, dopo aver elaborato il nodo corrente, il servizio passa direttamente al primo nodo figlio del nodo corrente. Questa opzione è simile all'opzione del passo successivo *Passa a* esistente, ad eccezione del fatto che consente una maggiore flessibilità. Non devi specificare il nodo esatto a cui passare. Nel runtime, il servizio passa sempre ad un qualunque nodo che sia il primo nodo figlio, anche se i nodi figlio sono stati riordinati oppure sono stati aggiunti nuovi nodi dopo aver definito il comportamento del passo successivo. 

    - Puoi aggiungere risposte condizionali per gli slot. Per le risposte Trovato e Non trovato, puoi personalizzare come risponde il servizio a seconda del fatto che vengano soddisfatte determinate condizioni. Questa funzione ti consente di controllare la presenza di possibili errori di interpretazione e di correggerli prima di salvare il valore fornito dall'utente nella variabile di contesto dello slot. Ad esempio, se lo slot salva l'età dell'utente e utilizza `@sys-number` nel campo *Controlla* per acquisirla, puoi aggiungere una condizione che controlli i numeri superiori a 100 e risponda con una frase simile a *Specifica un'età valida in anni.* Per ulteriori dettagli, vedi [Aggiunta di condizioni alle risposte Trovato e Non trovato](dialog-slots.html#slot-handler-next-steps).

    - L'interfaccia che utilizzi per aggiungere le risposte condizionali a un nodo è stata riprogettata per rendere più semplice elencare ciascuna condizione e la relativa risposta. Per aggiungere risposte condizionali a livello di nodo, fai clic su **Personalizza** e quindi abilita l'opzione **Risposte multiple**.

     **Nota**: l'interruttore **Risposte multiple** attiva o disattiva la funzione solo per la risposta a livello di nodo. Non controlla la capacità di definire risposte condizionali per uno slot. L'impostazione per le risposte multiple dello slot viene controllata separatamente. 

    - Per conservare la pagina in cui hai modificato uno slot semplice, devi ora selezionare le opzioni di menu per a.) aggiungere una condizione che deve essere soddisfatta affinché lo slot venga elaborato e b.) aggiungere risposte condizionali per le condizioni Trovato e Non trovato di uno slot. A meno che non scegli di aggiungere questa funzionalità supplementare, i campi della condizione slot e delle risposte multiple non vengono visualizzati, ciò consentirà di fare spazio nella pagina e di renderla più semplice da utilizzare. 

### 25 ottobre 2017
{: #25October2017}

- **Aggiornamenti per il cinese semplificato**: è stato migliorato il supporto linguistico per il cinese semplificato. Ciò include i miglioramenti della classificazione degli intenti utilizzando il word embedding a livello di carattere e la disponibilità delle entità di sistema. Nota che i modelli di apprendimento del servizio {{site.data.keyword.conversationshort}} potrebbero essere stati aggiornati come parte di questo miglioramento e quando addestri nuovamente il tuo modello verranno applicate tutte le modifiche; per ulteriori informazioni, vedi [Modelli aggiornati](release-notes.html#updated-models).
- **Aggiornamenti per lo spagnolo** - Sono stati apportati miglioramenti alla classificazione degli intenti in spagnolo, per dataset di grandi dimensioni.

### 11 ottobre 2017
{: #11October2017}

- **Aggiornamenti per il coreano**: è stato migliorato il supporto linguistico per il coreano. Nota che i modelli di apprendimento del servizio {{site.data.keyword.conversationshort}} potrebbero essere stati aggiornati come parte di questo miglioramento e quando addestri nuovamente il tuo modello verranno applicate tutte le modifiche; per ulteriori informazioni, vedi [Modelli aggiornati](release-notes.html#updated-models).

### 3 ottobre 2017
{: #3October2017}

- **Entità definite dal modello**: puoi ora definire specifici modelli per un'entità, utilizzando espressioni regolari. Ciò può aiutarti a identificare le entità che seguono un modello definito, ad esempio SKU o numeri parte, numeri di telefono o indirizzi e-mail. Per ulteriori dettagli, vedi [Entità definite dal modello](entities.html#pattern-entities).
    - Per un singolo valore di entità puoi aggiungere i sinonimi o i modelli, non puoi aggiungere entrambi.
    - Per ogni valore di entità, ci possono essere un massimo di 5 modelli.
    - Ogni modello (espressione regolare) è limitato a 128 caratteri.
    - L'importazione o l'esportazione tramite un file CSV attualmente non supporta i modelli.
    - L'API REST non supporta l'accesso diretto ai modelli, ma puoi richiamare o modificare i modelli utilizzando l'endpoint `/values`.

- **Corrispondenza fuzzy filtrata per dizionario (solo inglese)**: è ora disponibile una versione migliorata della corrispondenza fuzzy per le entità, solo per l'inglese. Questo miglioramento impedisce l'acquisizione di alcune parole comuni valide in inglese come corrispondenze fuzzy per una determinata entità. Ad esempio, la corrispondenza fuzzy non abbinerà il valore di entità `like` ad `hike` o `bike`, che sono parole inglesi valide, ma continuerà ad abbinare esempi come `lkie` o `oike`.

### 27 settembre 2017
{: #27September2017}

- **Aggiornamenti al builder di condizioni**: è stato aggiornato il controllo che viene visualizzato per aiutarti a definire una condizione in un nodo di dialogo. I miglioramenti includono il supporto per elencare i nomi delle variabili di contesto disponibili dopo aver immesso $ per iniziare ad aggiungere una variabile di contesto.

### 31 agosto 2017
{: #31August2017}

- **Rollback della sezione Migliora**: la metrica del tempo di conversazione mediana e i filtri corrispondenti verranno temporaneamente rimossi dalla pagina Panoramica della sezione Migliora. Questa rimozione impedirà al calcolo di alcune metriche di mostrare informazioni imprecise per la metrica del tempo di conversazione mediana e il grafico delle conversazioni nel tempo. IBM si rammarica di aver rimosso la funzionalità dalla strumento, ma si impegna a garantire che agli utenti vengano comunicate informazioni accurate.
- **Nomi di nodo del dialogo**: adesso puoi assegnare qualsiasi nome a un nodo di dialogo: non è necessario che sia univoco. Inoltre, puoi modificare successivamente il nome del nodo senza influire sul modo in cui viene fatto riferimento al nodo internamente. Il nome che specifichi viene salvato come attributo titolo del nodo nel file JSON dello spazio di lavoro e il sistema utilizza un ID univoco che è memorizzato nell'attributo nome per fare riferimento al nodo.

### 23 agosto 2017
{: #23August2017}

- **Aggiornamenti per coreano, giapponese e italiano**: è stato migliorato il supporto linguistico per il coreano, il giapponese e l'italiano. Nota che i modelli di apprendimento del servizio {{site.data.keyword.conversationshort}} potrebbero essere stati aggiornati come parte di questo miglioramento e quando addestri nuovamente il tuo modello verranno applicate tutte le modifiche; per ulteriori informazioni, vedi [Modelli aggiornati](release-notes.html#updated-models).

### 10 agosto 2017
{: #10August2017}

- **Normalizzazione degli accenti**: in un contesto di conversazione, gli utenti possono utilizzare o meno accenti durante l'interazione con il servizio {{site.data.keyword.conversationshort}}. Di conseguenza, è stato effettuato un aggiornamento all'algoritmo in modo che le versioni accentate e non accentate delle parole siano trattate allo stesso modo per il rilevamento degli intenti e il riconoscimento delle entità.

  Tuttavia per alcune lingue, come lo spagnolo, alcuni accenti possono modificare il significato dell'entità. Pertanto, per il rilevamento di entità, sebbene l'entità originale possa implicitamente avere un accento, il servizio può anche abbinare la versione non accentata della stessa entità, ma con un punteggio di affidabilità leggermente inferiore.

  Ad esempio, per la parola `barrió`, che ha un accento e corrisponde al passato del verbo `barrer` (spazzare), il servizio può abbinare anche la parola `barrio` (quartiere), ma con una affidabilità leggermente inferiore. 

  Il sistema fornirà punteggi di affidabilità più alti nelle entità con corrispondenze esatte. Ad esempio, `barrio` non verrà rilevato se `barrió` è nel set di addestramento e `barrió` non verrà rilevato se `barrio` è nel set di addestramento.

  Dovresti addestrare il sistema con i caratteri e gli accenti appropriati. Ad esempio, se prevedi `barrió` come risposta, non devi inserire `barrió` nel set di addestramento.

  Sebbene non sia un segno di accento, lo stesso vale per le parole che utilizzano, ad esempio, la lettera spagnola `ñ` rispetto alla lettera `n`, ad esempio `uña` vs. `una`. In questo caso, la lettera `ñ` non è semplicemente una `n` con un accento; è una lettera unica e specifica per lo spagnolo.

  Puoi abilitare la corrispondenza fuzzy se ritieni che i tuoi clienti non utilizzeranno gli accenti appropriati o faranno errori di ortografia (incluso, ad esempio, mettere una `n` al posto di una `ñ`) o puoi includerli esplicitamente negli esempi di addestramento.

  **Nota:** la normalizzazione degli accenti è abilitata per portoghese, spagnolo, francese e ceco.

- **Indicatore di rifiuto dello spazio di lavoro**: l'API REST {{site.data.keyword.conversationshort}} adesso supporta un indicatore di rifiuto per gli spazi di lavoro. Questo indicatore indica che i dati di addestramento dello spazio di lavoro come gli intenti e le entità non devono essere utilizzati da IBM per i miglioramenti del servizio generale. Per ulteriori informazioni, vedi il [Riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#data-collection){: new_window}

### 7 agosto 2017
{: #7August2017}

- **Interpretazione delle date `Next` e `last`**: il servizio {{site.data.keyword.conversationshort}} considera `last` e `next` come il giorno di riferimento precedente o successivo più immediato, che può essere nella stessa settimana o in una settimana precedente. Per ulteriori informazioni, vedi l'argomento [Entità di sistema](system-entities.html#sys-datetime).

### 3 agosto 2017
{: #3August2017}

- **Corrispondenza fuzzy per altre lingue (Beta)**: la corrispondenza fuzzy per le entità è ora disponibile per altre lingue, come indicato nell'argomento [Lingue supportate](lang-support.html). 
- **Corrispondenza parziale (Beta - Solo inglese)**: la corrispondenza fuzzy ora suggerirà automaticamente dei sinonimi basati sulla sottostringa presenti nelle entità definite dall'utente e assegnerà un punteggio di affidabilità inferiore rispetto alla corrispondenza esatta dell'entità. Per i dettagli, vedi [Corrispondenza fuzzy](entities.html#fuzzy-matching).

### 28 luglio 2017
{: #28July2017}

- Quando imposti le preferenze bidirezionali per gli strumenti, puoi ora specificare la direzione dell'interfaccia utente grafica.
- Lo schema di colori degli strumenti è stato aggiornato per essere coerente con altri servizi e prodotti Watson.

### 19 luglio 2017
{: #19July2017}

- L'API REST {{site.data.keyword.conversationshort}} supporta ora l'accesso ai nodi di dialogo. Per ulteriori informazioni, vedi il [Riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#dialog_nodes){: new_window}.

### 14 luglio 2017
{: #14July2017}

- La funzionalità degli slot dei dialoghi è stata migliorata. Ad esempio, è stata aggiunta una proprietà *slot_in_focus* che puoi utilizzare per definire una condizione applicabile solo a un singolo slot. Per i dettagli, vedi [Raccolta di informazioni con gli slot](/docs/services/conversation/dialog-slots.html).

### 12 luglio 2017
{: #12July2017}

- **Supporto per il ceco**: è stato introdotto il supporto per la lingua ceca; per ulteriori dettagli, vedi l'argomento [Lingue supportate](lang-support.html).

### 11 luglio 2017
{: #11July2017}

- **Test in Slack**: puoi utilizzare il nuovo strumento **Test in Slack** per distribuire rapidamente il tuo spazio di lavoro come utente del bot Slack a scopo di test. Questo strumento è disponibile solo per la regione Stati Uniti Sud di {{site.data.keyword.Bluemix_notm}}. Per ulteriori informazioni, vedi [Esecuzione di test in Slack](test-deploy.html).
- **Aggiornamenti per l'arabo**: il supporto per la lingua araba è stato migliorato per includere il punteggio assoluto per ogni intento e la capacità di contrassegnare gli intenti come irrilevanti; per ulteriori dettagli, vedi l'argomento [Lingue supportate](lang-support.html). Nota che i modelli di apprendimento del servizio {{site.data.keyword.conversationshort}} potrebbero essere stati aggiornati come parte di questo miglioramento e quando addestri nuovamente il tuo modello verranno applicate tutte le modifiche; per ulteriori informazioni, vedi [Modelli aggiornati](release-notes.html#updated-models).

### 23 giugno 2017
{: #23June2017}

- **Aggiornamenti per il coreano**: il supporto per la lingua coreana è stato migliorato; per ulteriori dettagli, vedi l'argomento [Lingue supportate](lang-support.html). Nota che i modelli di apprendimento del servizio {{site.data.keyword.conversationshort}} potrebbero essere stati aggiornati come parte di questo miglioramento e quando addestri nuovamente il tuo modello verranno applicate tutte le modifiche; per ulteriori informazioni, vedi [Modelli aggiornati](release-notes.html#updated-models).

### 22 giugno 2017
{: #22June2017}

- **Introduzione di slot**: adesso è più facile raccogliere più informazioni da un utente in un singolo nodo aggiungendo degli slot. In precedenza, dovevi creare diversi nodi di dialogo per coprire tutte le possibili combinazioni di modi in cui gli utenti potevano fornire le informazioni. Con gli slot, puoi configurare un singolo nodo che salva tutte le informazioni fornite dall'utente e richiede i dettagli necessari che non sono stati forniti. Per ulteriori dettagli, vedi [Raccolta di informazioni con gli slot](dialog-slots.html).
- **Struttura ad albero di dialogo semplificata** - La struttura ad albero di dialogo è stata riprogettata per migliorarne l'usabilità. La vista ad albero è più compatta quindi è più facile vedere dove ti trovi al suo interno. Inoltre, i link tra i nodi sono rappresentati in un modo che facilita la comprensione delle relazioni tra i nodi.

### 21 giugno 2017
{: #21June2017}

- **Supporto per l'arabo**: il supporto linguistico per l'arabo è ora generalmente disponibile. Per i dettagli, vedi [Configurazione di lingue bidirezionali](lang-support.html#configuring-bi-directional).
- **Aggiornamenti delle lingue**: gli algoritmi del servizio {{site.data.keyword.conversationshort}} sono stati aggiornati per migliorare l'intero supporto linguistico. Per i dettagli, vedi l'argomento [Lingue supportate](lang-support.html).

### 16 giugno 2017
{: #16June2017}

- **Suggerimenti (Beta - Solo utenti Premium)**: il pannello Migliora include anche una pagina di **Suggerimenti** che consiglia modi per migliorare il sistema analizzando le conversazioni che gli utenti hanno con la tua chatbot e tenendo conto dei dati di addestramento correnti del tuo sistema e della certezza della risposta. 

### 14 giungo 2017
{: #14June2017}

- **Corrispondenza fuzzy per altre lingue (Beta)**: la corrispondenza fuzzy per le entità è ora disponibile per altre lingue, come indicato nell'argomento [Lingue supportate](lang-support.html). Puoi attivare la corrispondenza fuzzy per entità per migliorare la capacità del servizio di riconoscere i termini nell'input utente con una sintassi simile all'entità, senza richiedere una corrispondenza esatta. La funzione è in grado di associare l'input utente all'entità corrispondente adeguata nonostante la presenza di errori di ortografia o lievi differenze sintattiche. Ad esempio, se definisci giraffa come sinonimo di un'entità animale e l'input utente contiene i termini giraffe o girafa, la corrispondenza fuzzy è in grado di associare correttamente il termine all'entità animale. Per i dettagli, vedi [Corrispondenza fuzzy](entities.html#fuzzy-matching).

### 13 giungo 2017
{: #13June2017}

- **Conversazioni utente**: il pannello Migliora include ora una pagina **Conversazioni utente**, che fornisce un elenco di interazioni dell'utente con la tua chatbot che può essere filtrato per parola chiave, intento, entità o numero di giorni. Puoi aprire singole conversazioni per correggere intenti o aggiungere valori di entità o sinonimi.
- **Modifica delle espressioni regolari**: Le espressioni regolari supportate dalle funzioni SpEL come find, matches, extract, replaceFirst, replaceAll e split sono state modificate. Non è più consentito un gruppo di costrutti di espressioni regolari, inclusi i costrutti look-ahead, look-behind, possessive repetition e backreference. Questa modifica è stata necessaria per evitare un rischio relativo alla sicurezza nella libreria di espressioni regolari originale.

### 12 giugno 2017
{: #12June2017}

- Il numero massimo di spazi di lavoro che puoi creare con il piano **Lite** (in precedenza denominato piano Gratuito) è cambiato da 3 a 5.
- Adesso puoi assegnare qualsiasi nome a un nodo di dialogo: non è necessario che sia univoco. Inoltre, puoi modificare successivamente il nome del nodo senza influire sul modo in cui viene fatto riferimento al nodo internamente. Il nome specificato viene trattato come un alias e il sistema utilizza il suo proprio identificativo interno per fare riferimento al nodo.
- Non puoi più modificare la lingua di uno spazio di lavoro dopo averlo creato modificandone i dettagli. Se devi modificare la lingua, puoi esportare lo spazio di lavoro come file JSON, aggiornare la proprietà lingua e quindi importare il file JSON come nuovo spazio di lavoro.

### 6 giugno 2017
{: #6June2017}

- **Impara**: è disponibile una nuova pagina *Informazioni su {{site.data.keyword.watson}} {{site.data.keyword.conversationshort}}* che fornisce informazioni preliminari e link alla documentazione del servizio e ad altre risorse utili. Per aprire la pagina, fai clic sull'icona ![i per informazioni](images/info.png) nell'intestazione della pagina.
- **Esportazione ed eliminazione in massa**: puoi ora esportare contemporaneamente una serie di intenti o entità in un file CSV, in modo da poterli importare e riutilizzare in un'altra applicazione {{site.data.keyword.conversationshort}}. Puoi anche selezionare contemporaneamente una serie di entità o intenti per l'eliminazione in massa.
- **Aggiornamenti per il coreano**: i tokenizer per il coreano sono stati aggiornati per far fronte al supporto linguistico informale. IBM continua a lavorare sui miglioramenti del riconoscimento e della classificazione delle entità.
- **Supporto per le emoji**: le emoji aggiunte agli esempi di intenti o come valori di entità, saranno ora classificate/estratte correttamente. **Nota**: solo le emoji incluse nei dati di addestramento saranno identificate in modo corretto e coerente; il supporto per le emoji potrebbe non classificare correttamente emoji simili con tonalità di colore diverse o altre varianti.
- **Stemming di entità (Beta - Solo inglese)**: la funzione beta di corrispondenza fuzzy riconosce le entità e le confronta in base alla forma della radice del valore di entità. Ad esempio, questa funzione riconosce correttamente 'bananas' come simile a 'banana' e 'run' come simile a 'running' in quanto condividono una forma di radice comune. Per ulteriori informazioni, vedi [Corrispondenza fuzzy](entities.html#fuzzy-matching).
- **Avanzamento dell'importazione dello spazio di lavoro**: quando importi uno spazio di lavoro da un file JSON, viene visualizzato immediatamente un tile per lo spazio di lavoro, in cui vengono mostrate le informazioni sull'avanzamento dell'importazione.
- **Tempo di addestramento ridotto**: adesso, più modelli vengono addestrati in parallelo, il che riduce notevolmente il tempo di addestramento per spazi di lavoro di grandi dimensioni.

### 26 maggio 2017
{: #26May2017}

- La versione API corrente è ora `2017-05-26`. Questa versione introduce le seguenti modifiche:
    - Lo schema degli oggetti ErrorResponse è cambiato. Questa modifica influisce su tutti gli endpoint e i metodi. Per ulteriori informazioni, vedi il [Riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.
    - Lo schema interno utilizzato per rappresentare i nodi di dialogo nel JSON dello spazio di lavoro esportato è cambiato. Se utilizzi l'API `2017-05-26` per importare uno spazio di lavoro che era stato esportato utilizzando una versione precedente, alcuni nodi di dialogo potrebbero non essere importati correttamente. Per ottenere risultati ottimali, importa uno spazio di lavoro utilizzando sempre la stessa versione utilizzata per esportarlo.

### 25 maggio 2017
{: #25May2017}

- Puoi ora gestire le variabili di contesto nel riquadro "Provalo". Fai clic sul link **Gestisci contesto** per aprire un nuovo riquadro in cui puoi impostare e controllare i valori delle variabili di contesto durante il test del dialogo. Per ulteriori informazioni, vedi [Test del dialogo](dialog-test.html).

### 16 maggio 2017
{: #16May2017}

- Quando apri lo strumento, è ora disponibile uno spazio di lavoro di esempio **Dashboard automobile**. Per utilizzare l'esempio come punto di partenza per il tuo spazio di lavoro, modifica lo spazio di lavoro. Se vuoi utilizzarlo per più spazi di lavoro, dovrai invece duplicarlo. Lo spazio di lavoro di esempio non viene conteggiato nel totale di spazi di lavoro della sottoscrizione a meno che non lo utilizzi.
- Ora è più semplice navigare nello strumento. Le opzioni del menu di navigazione sono disponibili dal lato della pagina principale anziché dalla parte superiore. Nella parte superiore della pagina, vengono visualizzati i link di navigazione che mostrano dove ti trovi. Puoi ora passare tra le istanze del servizio dalla pagina Spazi di lavoro. Per arrivarci rapidamente, fai clic su **Torna a spazi di lavoro** dal menu di navigazione. Se hai più istanze del servizio, viene visualizzato il nome dell'istanza corrente. Puoi fare clic sul link **Cambia** accanto ad esso per scegliere un'altra istanza.
- Quando crei un dialogo, vengono ora aggiunti automaticamente due nodi: 1) un nodo **Benvenuto** nella parte superiore della struttura ad albero di dialogo che contiene il messaggio iniziale da visualizzare all'utente e 2) un nodo **Altro** nella parte inferiore della struttura ad albero che cattura qualsiasi richiesta utente non riconosciuta dagli altri nodi nel dialogo e risponde a esse. Per ulteriori dettagli, vedi [Creazione di un dialogo](dialog-create.html).
- Quando testi un dialogo nel riquadro "Provalo", puoi ora trovare e inviare nuovamente un'istruzione di test recente premendo il tasto Su per scorrere i tuoi input precedenti.
- È ora disponibile il supporto sperimentale per la lingua coreana per 5 entità di sistema (`@sys-date`, `@sys-time`, `@sys-currency`, `@sys-number`, `@sys-percentage`). Esistono problemi noti per alcune entità numeriche e un supporto limitato per l'input in lingua informale.
- Dalla scheda Migliora è disponibile una pagina Panoramica. La pagina fornisce un riepilogo delle interazioni con il tuo bot. Puoi visualizzare la quantità di traffico per un determinato periodo di tempo, nonché gli intenti e le entità che sono stati riconosciuti più spesso nelle conversazioni degli utenti. Per ulteriori informazioni, vedi [Utilizzo della pagina Panoramica](logs_oview.html).

### 27 aprile 2017
{: #27April2017}

- Le seguenti entità di sistema sono ora disponibili come funzioni beta solo in lingua inglese:
    - sys-location: riconosce i riferimenti a località, come città e paesi, nelle espressioni dell'utente.
    - sys-person: riconosce i riferimenti ai nomi delle persone, nomi e cognomi, nelle espressioni dell'utente.

    Per ulteriori informazioni, vedi [Riferimento alle entità di sistema](system-entities.html).
- La corrispondenza fuzzy per le entità è una funzione beta ora disponibile in inglese. Puoi attivare la corrispondenza fuzzy per entità per migliorare la capacità del servizio di riconoscere i termini nell'input utente con una sintassi simile all'entità, senza richiedere una corrispondenza esatta. La funzione è in grado di associare l'input utente all'entità corrispondente adeguata nonostante la presenza di errori di ortografia o lievi differenze sintattiche. Ad esempio, se definisci **giraffe** come sinonimo di un'entità animale e l'input utente contiene i termini *giraffes* o *girafe*, la corrispondenza fuzzy è in grado di associare correttamente il termine all'entità animale. Per i dettagli, vedi [Definizione di entità](entities.html#fuzzy-matching) e cerca `Corrispondenza fuzzy`.

### 18 aprile 2017
{: #18April2017}

- L'API REST {{site.data.keyword.conversationshort}} supporta ora l'accesso alle seguenti risorse:
    - entità
    - valori di entità
    - sinonimi dei valori di entità
    - log

    Per ulteriori informazioni, vedi il [Riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.
- Il comportamento del metodo `POST` /messages ha modificato la gestione delle entità e degli intenti specificati come parte dell'input del messaggio:
    - Se specifichi gli intenti sull'input, il servizio utilizza gli intenti specificati, ma utilizza l'elaborazione del linguaggio naturale per rilevare le entità nell'input utente.
    - Se specifichi le entità sull'input, il servizio utilizza le entità specificate, ma utilizza l'elaborazione del linguaggio naturale per rilevare gli intenti nell'input utente.

        Il comportamento non è cambiato per i messaggi che specificano sia intenti che entità o per i messaggi che non specificano nessuno dei due.
- L'opzione per contrassegnare l'input dell'utente come irrilevante è ora disponibile per tutte le lingue supportate. Questa è una funzione beta.
- Una nuova scheda Credenziali fornisce un singolo luogo in cui puoi trovare tutte le informazioni necessarie per connettere la tua applicazione a uno spazio di lavoro (come le credenziali del servizio e l'ID spazio di lavoro), nonché altre opzioni di distribuzione. Per accedere alla scheda Credenziali per il tuo spazio di lavoro, fai clic sull'icona ![Menu](images/Menu_16.png) e seleziona **Credenziali**.

### 9 marzo 2017
{: #9March2017}

L'API REST {{site.data.keyword.conversationshort}} supporta ora l'accesso alle seguenti risorse:

- spazi di lavoro
- intenti
- esempi
- esempi contatori

Per ulteriori informazioni, vedi il [Riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.

### 7 marzo 2017
{: #7March2017}

- L'utilizzo di `.` o di `..` come nome intento causa dei problemi e non è più supportato.

    Non puoi ridenominare o eliminare un intento con questo nome; per modificare il nome, esporta i tuoi intenti in un file, ridenomina l'intento nel file e importa il file aggiornato nel tuo spazio di lavoro.

    I clienti paganti possono contattare il supporto per una modifica del database.

### 1 marzo 2017
{: #1March2017}

- Le entità di sistema ora sono abilitate in tedesco.

### 22 febbraio 2017
{: #22February2017}

- I messaggi sono ora limitati a 2.048 caratteri.

### 3 febbraio 2017
{: #3February2017}

- Abbiamo modificato come viene calcolato il punteggio degli intenti e abbiamo aggiunto la possibilità di contrassegnare l'input come irrilevante per la tua applicazione. Per i dettagli, vedi [Definizione di intenti](intents.html) e cerca `Contrassegna come irrilevante`.

- In questa release è stato introdotto un cambiamento importante allo spazio di lavoro. Per trarre vantaggio da queste modifiche, devi aggiornare manualmente il tuo spazio di lavoro. Per eseguire tale operazione, completa la procedura riportata di seguito: 

  1.  [Duplica il tuo spazio di lavoro](configure-workspace.html#exporting-and-copying-workspaces).
  1.  Aggiorna lo spazio di lavoro duplicato facendo clic sull'icona di aggiornamento (![icona aggiornamento](images/upgrade.png)).
  1.  Verifica lo spazio di lavoro aggiornato. 
  1.  Una volta eseguita la verifica e ottenuto il risultato previsto, applica l'aggiornamento alla tua applicazione modificando la chiamata API per utilizzare **2017-02-03** o successiva.

- L'elaborazione delle azioni **Passa a** è stata modificata per evitare loop che possono verificarsi in determinate condizioni. In precedenza, se passavi alla condizione di un nodo e né tale nodo né uno dei suoi nodi peer aveva una condizione valutata come true, il sistema sarebbe passato al nodo di livello root e avrebbe cercato un nodo la cui condizione soddisfasse l'input. In alcune situazioni, questa elaborazione creava un loop che impediva l'avanzamento del dialogo.

  Nel nuovo processo, se né il nodo di destinazione né i suoi peer vengono valutati come true, il turno di dialogo viene terminato. Per reimplementare il vecchio modello, aggiungi un nodo peer finale con una condizione `true`. Nella risposta, utilizza un'azione **Passa a** da destinare alla condizione del primo nodo al livello root della tua struttura ad albero di dialogo.

### 11 gennaio 2017
{: #11January2017}

- In questa release, puoi personalizzare i titoli dei nodi nel dialogo.

### 22 dicembre 2016
{: #22December2016}

- In questa release, i nodi di dialogo visualizzano una nuova sezione per il `titolo del nodo`. La possibilità di personalizzare il `titolo del nodo` non è disponibile. Quando compresso, il `titolo del nodo` visualizza la `condizione di nodo` del nodo di dialogo. Se non c'è una `condizione di nodo`, come titolo viene visualizzato "Nodo senza titolo".

### 19 dicembre 2016
{: #19December2016}

Diverse modifiche rendono il builder di dialoghi più semplice e intuitivo:

- Una vista di modifica più ampia rende più semplice visualizzare tutti i dettagli di un nodo mentre ci lavori.
- Un nodo può contenere più risposte, ognuna attivata da una condizione separata. Per ulteriori informazioni, vedi [Risposte multiple](dialog-overview.html#responses).

### 5 dicembre 2016
{: #5December2016}

- Sono supportate nuove lingue, tutte in modalità Sperimentale: tedesco, cinese tradizionale, cinese semplificato e olandese.
- Sono disponibili due nuove entità di sistema: @sys-date e @sys-time. Per i dettagli, vedi [Entità di sistema](system-entities.html).

### 21 ottobre 2016
{: #21October2016}

- Il servizio {{site.data.keyword.conversationshort}} fornisce ora entità di sistema, che sono entità comuni che possono essere utilizzate in qualsiasi caso di utilizzo. Per i dettagli, vedi [Definizione di entità](entities.html) e cerca `Abilitazione delle entità di sistema`.
- Puoi ora visualizzare una cronologia delle conversazioni con gli utenti nella pagina Migliora. Puoi utilizzarla per comprendere il comportamento del tuo bot. Per i dettagli, vedi [Miglioramento della comprensione](logs.html).
- Puoi ora importare le entità da un file CSV (comma-separated value), che ti aiuta quando hai un gran numero di entità. Per i dettagli, vedi [Definizione di entità](entities.html) e cerca `Importazione di entità`.

### 20 settembre 2016
{: #20September2016}

**Nuova versione**: 2016-09-20

Per usufruire delle modifiche in una nuova versione, cambia il valore del parametro `version` con la nuova data. Se non sei pronto per l'aggiornamento a questa versione, non modificare la data della versione.

- versione **2016-09-20**: `dialog_stack` modificato da un array di stringhe a un array di oggetti JSON.

### 29 agosto 2016
{: #29August2016}

- Puoi spostare i nodi di dialogo da un ramo all'altro, come nodi di pari livello o peer. Per i dettagli, vedi [Spostamento di un nodo di dialogo](dialog-build.html#move-node).
- Puoi espandere la finestra dell'editor JSON.
- Puoi visualizzare i log di chat delle conversazioni del tuo bot per aiutarti a comprendere il suo comportamento. Puoi filtrare per intenti, entità, data e ora. Per i dettagli, vedi [Miglioramento della comprensione](logs.html)

### 11 luglio 2016
{: #21July2016}

- Questa release di Disponibilità generale ti consente di lavorare con entità e dialoghi per creare un bot pienamente funzionante.

### 18 maggio 2016
{: #18May2016}

- Questa release Sperimentale di {{site.data.keyword.conversationshort}} introduce l'interfaccia utente e ti consente di lavorare con spazi di lavoro, intenti ed esempi.
