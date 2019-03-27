---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# Creazione di entità
{: #entities}

Le ***entità*** rappresentano le informazioni nell'input utente rilevanti per lo scopo dell'utente. 

Se gli intenti rappresentano i verbi (l'azione che un utente desidera eseguire), le entità rappresentano i nomi (l'oggetto o il contesto di tale di tale azione). Ad esempio, quando l'*intento* è quello di ottenere una previsione meteorologica, sono necessarie le *entità* per la posizione pertinente e la data prima che l'applicazione possa restituire una previsione accurata.

Riconoscere le entità nell'input utente ti aiuta a creare risposte più utili e più mirate. Ad esempio, potresti avere un intento `#buy_something`. Quando un utente effettua una richiesta che attiva l'intento `#buy_something`, la risposta dell'assistente dovrebbe riflettere una comprensione di cosa sia il *something* che il cliente desidera acquistare. Puoi aggiungere un'entità `@product` e poi utilizzarla per estrarre informazioni dall'input utente relative al prodotto a cui il cliente è interessato. (Il simbolo `@` aggiunto come prefisso al nome entità aiuta a identificarla chiaramente che è un'entità.)

Infine, puoi aggiungere più risposte alla tua struttura ad albero di dialogo con un testo che differisce a seconda del valore `@product` che viene rilevato nella richiesta dell'utente. 

<iframe class="embed-responsive-item" id="youtubeplayer" title="Utilizzo delle entità" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/o-uhdw6bIyI" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Panoramica sulla valutazione dell'entità
{: #entities-described}

Il servizio rileva le entità nell'input utente utilizzando uno dei seguenti metodi di valutazione: 

### Metodo basato sul dizionario
{: #entities-dictionary-overview}

Il servizio ricerca i termini nell'input utente che corrispondono a valori, sinonimi o modelli che definisci per l'entità. 

- **Entità sinonimo**: definisci una categoria di termini come un'entità (`color`) e poi uno o più valori in tale categoria (`blue`). Per ciascun valore specifichi un gruppo di sinonimi (`aqua`, `navy`). Puoi anche selezionare sinonimi da aggiungere dai suggerimenti che ti ha proposto il servizio. 

    Nel runtime, il servizio riconosce i termini nell'input utente che corrispondono esattamente ai valori o ai sinonimi che hai definito per l'entità come citazioni di tale entità.
- **Entità modello**: definisci una categoria di termini come un'entità (`contact_info`) e poi uno o più valori in tale categoria (`email`). Per ciascun valore, specifichi un'espressione regolare che definisce il modello testuale delle citazioni di tale tipo di valore. Per un valore di entità `email`, potresti voler specificare un'espressione regolare che definisce un modello `text@text.com`.

    Nel runtime, il servizio ricerca i modelli che corrispondono alla tua espressione regolare nell'input utente e identifica le corrispondenze come citazioni di tale entità.
- **Entità di sistema**: le entità sinonimo vengono precostruite automaticamente da IBM. Coprono le categorie più utilizzate come ad esempio numeri, date e orari. Abilita semplicemente un'entità di sistema per iniziare a utilizzarla. 

### Metodo basato sul contesto
{: #entities-annotations-overview}

Quando definisci un'entità contestuale, viene eseguito l'addestramento di un modello sia sul *termine annotato* che sul *contesto* in cui il termine viene utilizzato nella frase che hai annotato. Questo nuovo modello di entità contestuale consente al servizio di calcolare un punteggio di affidabilità che identifica la probabilità che ha una parola o una frase di essere un'istanza di un'entità, in base a come viene utilizzata nell'input utente. 

- **Entità contestuale**: innanzitutto, definisci una categoria dei termini come un'entità (`product`). Successivamente, vai alla pagina *Intenti* ed estrai gli esempi utente dell'intento esistenti per trovare le citazioni dell'entità ed etichettale come tali. Ad esempio, potresti andare all'intento `#buy_something` e trovare un esempio utente che contiene `I want to buy a Coach bag`. Puoi etichettare `Coach bag` come una citazione dell'entità `@product`.

    A scopo di addestramento, il termine che hai annotato, `Coach bag`, viene aggiunto come un valore dell'entità `@product`.

    Nel runtime, il servizio valuta i termini solo in base al contesto in cui vengono utilizzati nella frase. Se la struttura di una richiesta utente che cita il termine corrisponde alla struttura di un esempio utente dell'intento in cui è etichettata una citazione, il servizio interpreta il termine come una citazione di tale tipo di entità. Ad esempio, l'input utente potrebbe includere l'espressione `I want to buy a Gucci bag`. A causa della somiglianza della struttura di questa frase con l'esempio utente che hai annotato (`I want to buy a Coach bag`), il servizio riconosce `Gucci bag` come una citazione di entità `@product`.

    Quando il modello di entità contestuale viene utilizzato per un'entità, il servizio *non* ricerca le corrispondenze esatte del testo o del modello per l'entità nell'input utente, ma si concentra invece sul contesto della frase in cui viene citata l'entità. 

    Se scegli di definire i valori di entità utilizzando le annotazioni, aggiungi almeno 10 annotazioni per entità per fornire al modello di entità contestuale dati sufficienti per essere affidabile. 

## Creazione di entità
{: #entities-creating-task}

Utilizza lo strumento {{site.data.keyword.conversationshort}} per creare le entità.

1.  Nello strumento {{site.data.keyword.conversationshort}}, apri la capacità di dialogo e poi fai clic sulla scheda **Entità**. Se **Entità** non è visibile, utilizza il menu ![Menu](images/Menu_16.png) per aprire la pagina.

1.  Fai clic su **Aggiungi entità**.

    Puoi anche fare clic su **Utilizza entità di sistema** per effettuare una selezione da un elenco di entità comuni, fornite da {{site.data.keyword.IBM_notm}}, che possono essere applicate a qualsiasi caso di utilizzo. Per ulteriori dettagli, vedi [Abilitazione delle entità di sistema](#entities-enable-system-entities).

1.  Nel campo **Nome entità**, immetti un nome descrittivo per l'entità.

    Il nome dell'entità può contenere lettere (in Unicode), numeri, caratteri di sottolineatura e trattini. Ad esempio:
    - `@location`
    - `@menu_item`
    - `@product`

    Non includere spazi nel nome. Il nome non può essere più lungo di 64 caratteri. Non iniziare il nome con la stringa `sys-` poiché è riservata per le entità di sistema. 

    Lo strumento include automaticamente il carattere @ nel nome entità, quindi non devi aggiungerne uno.
    {: tip}

1.  Fai clic su **Crea entità**.

    ![Schermata della creazione di un'entità](images/create_entity.png)

1.  Per questa entità, scegli se vuoi che il servizio utilizzi un approccio basato sul dizionario o uno basato sul contesto per trovare citazioni relative ad essa e segui la procedura appropriata.

    **Per ciascuna entità che crei, scegli solo un tipo di entità da utilizzare.** Appena aggiungi un'annotazione per un'entità, il modello contestuale viene inizializzato e diventa l'approccio principale per analizzare l'input utente per trovare le citazioni di tale entità. Il contesto in cui viene utilizzata la citazione nell'input utente ha la precedenza su qualsiasi corrispondenza esatta che potrebbe essere presente. Vedi [Panoramica sulla valutazione dell'entità](#entities-described) per ulteriori informazioni su come viene valutato ciascun tipo. 

    - [Entità basate sul dizionario](#entities-create-dictionary-based)
    - [Entità basate sul contesto](#entities-create-annotation-based)

## Aggiunta di entità basate sul dizionario
{: #entities-create-dictionary-based}

Le entità basate sul dizionario sono quelle per cui definisci termini, sinonimi o modelli specifici. Nel runtime, il servizio trova le citazioni di entità solo quando un termine nell'input utente corrisponde esattamente (o si avvicina di più, se è abilitata la corrispondenza fuzzy) al valore o a uno dei suoi sinonimi. 

1.  Nel campo **Nome valore**, immetti il testo di un possibile valore per l'entità e premi il tasto `Invio`. Un valore di entità può essere qualsiasi stringa composta da un massimo di 64 caratteri.

    **Importante:** non includere informazioni sensibili o personali nei nomi o valori di entità. I nomi e i valori possono essere esposti negli URL in un'applicazione.

1.  Se desideri che il servizio riconosca i termini con la sintassi simile al valore di entità e ai sinonimi che hai specificato, ma senza aver bisogno di una corrispondenza esatta, fai clic sull'interruttore **Corrispondenza fuzzy** per attivarlo. 

    Questa funzione è disponibile per le lingue indicate nell'argomento [Lingue supportate](/docs/services/assistant?topic=assistant-language-support).

    **Corrispondenza fuzzy**
    {: #entities-fuzzy-matching}

    Le corrispondenze fuzzy hanno questi componenti:

    - *Stemming* - La funzione riconosce la forma della radice dei valori di entità che hanno varie forme grammaticali. Ad esempio, la radice di 'bananas' è 'banana', mentre la radice di 'running' è 'run'.
    - *Errore di ortografia* - La funzione è in grado di associare l'input utente all'entità corrispondente adeguata nonostante la presenza di errori di ortografia o lievi differenze sintattiche. Ad esempio, se definisci *giraffe* come sinonimo di un'entità animale e l'input utente contiene i termini *giraffes* o *girafe*, la corrispondenza fuzzy è in grado di associare correttamente il termine all'entità animale.
    - *Corrispondenza parziale* - Con la corrispondenza parziale, la funzione suggerisce automaticamente dei sinonimi basati sulla sottostringa presenti nelle entità definite dall'utente e assegna un punteggio di affidabilità inferiore rispetto alla corrispondenza esatta dell'entità.

    Per l'inglese, la corrispondenza fuzzy impedisce l'acquisizione di alcune parole comuni valide in inglese come corrispondenze fuzzy per una determinata entità. Questa funzione utilizza solo parole del dizionario in inglese standard. Puoi anche definire un valore/sinonimo di entità in inglese e la corrispondenza fuzzy metterà in corrispondenza solo il tuo valore/sinonimo di entità definito. Ad esempio, la corrispondenza fuzzy può mettere in corrispondenza il termine `unsure` con `insurance`; ma se hai definito `unsure` come un valore/sinonimo per un'entità come `@option`, `unsure` verrà sempre messo in corrispondenza con `@option` e non con `insurance`.
    {: note}

    L'impostazione della tua corrispondenza fuzzy non influisce sui consigli sui sinonimi. Anche se la corrispondenza fuzzy è abilitata, i sinonimi vengono suggeriti solo per il valore esatto che specifichi, non per il valore o le sottili varianti del valore. 

1.  Una volta immesso un nome valore, puoi aggiungere dei sinonimi o definire modelli specifici per tale valore selezionando `Sinonimi` o `Modelli` dal menu a discesa *Tipo*.

    ![Selettore dei tipi per Valore](images/value_type.png)

    **Nota:** puoi aggiungere sinonimi *oppure* modelli per un singolo valore di entità, non entrambi. 

    ***Sinonimi***
    {: #entities-synonyms}

    - Nel campo **Sinonimi**, immetti qualsiasi sinonimo per il valore di entità. Un sinonimo può essere qualsiasi stringa composta da un massimo di 64 caratteri.

      ![Schermata della definizione di un'entità](images/define_entity.png)

      Il servizio {{site.data.keyword.conversationshort}} può anche consigliare sinonimi per i tuoi valori di entità. La funzione di suggerimento trova i sinonimi correlati in base alla somiglianza contestuale estratta da un gran numero di informazioni esistenti, incluse grandi origini di testo scritto e utilizza le tecniche di elaborazione del linguaggio naturale per identificare delle parole simili ai sinonimi esistenti nel tuo valore di entità. 

    - Fai clic su **Show recommendations**.

    - Il servizio {{site.data.keyword.conversationshort}} proporrà alcuni consigli per i sinonimi. I termini vengono visualizzati in minuscolo, ma il servizio riconosce le citazioni dei sinonimi se vengono specificate in minuscolo o in maiuscolo. 

      Più sono coerenti i tuoi sinonimi del valore di entità, più saranno pertinenti e precisi i tuoi suggerimenti. Ad esempio, se hai diverse parole focalizzate su un tema, otterrai suggerimenti migliori rispetto a se avessi una o due parole casuali.
      {: tip}

      ![Schermata 2 consiglio sul sinonimo](images/synonym_2.png)

    - Seleziona i sinonimi che desideri includere e poi fai clic su **Aggiungi selezionati**.

      Devi fare clic sul pulsante **Aggiungi selezionati** per i sinonimi che hai selezionato per l'aggiunta. Se ti sposti sulla serie successiva senza aver prima fatto clic su questo pulsante, le tue selezioni verranno perse. 

      ![Schermata 3 consiglio sul sinonimo](images/synonym_3.png)

    - Il servizio {{site.data.keyword.conversationshort}} aggiunge tali sinonimi alla tua entità e suggerisce altri sinonimi. 

      Se non ricevi ulteriori consigli di sinonimi, è possibile che la tua entità sia già ampiamente definita o che contenga parti che la funzione di suggerimento non è in grado di ampliare in questo momento.
      {: tip}

      Se scegli di non selezionare un sinonimo consigliato, il sistema lo tratterà come un termine a cui non sei interessato e modificherà la prossima serie di consigli che vedrai quando premi `Aggiungi selezionato` o `Serie successiva`. Questa deduzione persiste solo mentre scegli i sinonimi; le informazioni sui sinonimi ignorati non vengono utilizzate dal servizio per nessuno degli altri scopi.
      {: note}

      ![Schermata 4 consiglio sul sinonimo](images/synonym_4.png)

      Continua ad aggiungere sinonimi come desideri. Dopo aver finito di accettare i consigli, fai clic sulla **X** per chiudere. 

    ***Modelli***
    {: #entities-patterns}

    - Il campo **Modelli** consente di definire specifici modelli per un valore di entità. Un modello **deve** essere immesso nel campo come espressione regolare.

      - Per ogni valore di entità, ci possono essere un massimo di 5 modelli.
      - Ogni modello (espressione regolare) è limitato a 512 caratteri.

      ![Schermata della definizione di un'entità modello](images/patternents1.png)
      {: #entities-pattern-entities}

      Come in questo esempio, per l'entità *ContactInfo*, i modelli per i valori di telefono, email e sito web possono essere definiti come segue:
      - Telefono
        - `localPhone`: `(\d{3})-(\d{4})`, ad esempio 426-4968
        - `fullUSphone`: `(\d{3})-(\d{3})-(\d{4})`, ad esempio 800-426-4968
        - `internationalPhone`: `^(\(?\+?[0-9]*\)?)?[0-9_\- \(\)]*$`, ad esempio +44 1962 815000
      - `email`: `\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}\b`, ad esempio name@ibm.com
      - `website`: `(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$`, ad esempio https://www.ibm.com

      Spesso quando si utilizzano entità modello, sarà necessario memorizzare il testo che corrisponde al modello in una variabile di contesto (o variabile di azione), dall'interno della tua struttura ad albero di dialogo. Per ulteriori informazioni, vedi [Definizione di una variabile di contesto](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-var-define).

      Immagina un caso in cui chiedi a un utente il suo indirizzo email. La condizione del nodo di dialogo conterrà una condizione simile a `@contactInfo:email`. Per assegnare l'email immessa dall'utente come variabile di contesto, è possibile utilizzare la seguente sintassi per acquisire la corrispondenza di modello all'interno della sezione di risposta del nodo di dialogo:

      <table>
      <caption>Salvataggio di un modello</caption>
        <tr>
          <th>Variabile</th>
          <th>Valore</th>
        </tr>
        <tr>
          <td>email</td>
          <td>`<? @contactInfo.literal ?>`</td>
        </tr>
      </table>

      ***Gruppi di acquisizione***
      {: #entities-capture-group}

      Per le espressioni regolari, qualsiasi parte di un modello all'interno di una coppia di parentesi normali verrà acquisito come un gruppo. Ad esempio, l'entità `@ContactInfo` ha un valore di modello denominato `fullUSphone` che contiene tre gruppi acquisiti:

      - `(\d{3})` - Prefisso US
      - `(\d{3})` - Prefisso
      - `(\d{4})` - Numero

      Il raggruppamento può essere utile se, ad esempio, desideri che il servizio {{site.data.keyword.conversationshort}} richieda agli utenti il loro numero di telefono e poi utilizzi solo il prefisso del numero specificato nella risposta.

      Per assegnare il prefisso immesso dall'utente come variabile di contesto, è possibile utilizzare la seguente sintassi per acquisire la corrispondenza di gruppo all'interno della sezione di risposta del nodo di dialogo:

      <table>
      <caption>Salvataggio di un gruppo di acquisizione</caption>
        <tr>
          <th>Variabile</th>
          <th>Valore</th>
        </tr>
        <tr>
          <td>area_code</td>
          <td>`<? @ContactInfo.groups[1] ?>`</td>
        </tr>
      </table>

      Per ulteriori informazioni sull'utilizzo dei gruppi di acquisizione nel tuo dialogo, vedi [Memorizzazione e riconoscimento dei gruppi di entità modello nell'input](/docs/services/assistant?topic=assistant-dialog-tips#dialog-tips-get-pattern-groups).

      Il motore di corrispondenze di modello utilizzato dal servizio {{site.data.keyword.conversationshort}} ha alcune limitazioni di sintassi, che sono necessarie per evitare problemi di prestazione che possono verificarsi quando si utilizzano altri motori di espressioni regolari.

      - I modelli di entità non possono contenere:
        - Ripetizioni positive (ad esempio `x*+`)
        - Riferimenti posteriori (ad esempio `\g1`)
        - Rami condizionali (ad esempio `(?(cond)true)`)
      - Quando un'entità di modello inizia o termina con un carattere Unicode, e include limiti di parole, ad esempio `\bš\b`, il modello non corrisponde correttamente ai limiti di parole. In questo esempio, per l'input `š zkouška`, la corrispondenza restituisce `Group 0: 6-7 š` (`š zkou`_**`š`**_`ka`), invece del corretto `Group 0: 0-1 š` (_**`š`**_ `zkouška`).

      Il motore di espressioni regolari si basa approssimativamente sul motore di espressioni regolari Java. Il servizio {{site.data.keyword.conversationshort}} produrrà un errore se tenti di caricare un modello non supportato, tramite l'API o dall'IU degli strumenti del servizio {{site.data.keyword.conversationshort}}.

1.  Fai clic su **Aggiungi valore** e ripeti il processo per aggiungere più valori di entità.

1.  Quando hai finito di aggiungere i valori di entità, fai clic su ![Freccia di chiusura](images/close_arrow.png) per completare la creazione dell'entità. 

L'entità creata viene aggiunta alla scheda **Entità** e il sistema comincia ad addestrarsi sui nuovi dati.

## Aggiunta di entità contestuali
{: #entities-create-annotation-based}

Le entità basate sul contesto sono quelle per cui annoti le ricorrenze dell'entità nelle frasi di esempio per istruire il servizio sul contesto in cui l'entità viene utilizzata normalmente. 

Al fine di addestrare un modello di entità contestuale, puoi trarre vantaggio dai tuoi esempi di intento che forniscono frasi già disponibili da annotare.

L'utilizzo di esempi utente dell'intento per definire le entità contestuali non influisce sulla classificazione di tale intento. Tuttavia, le citazioni di entità che etichetti vengono aggiunte anche a tale entità come sinonimi. E la classificazione degli intenti utilizza le citazioni di sinonimi negli esempi utente dell'intento per stabilire un riferimento debole tra un intento e un'entità.
{: note}

1.  Nello strumento {{site.data.keyword.conversationshort}}, apri la tua capacità e poi fai clic sulla scheda **Intenti**. Se **Intents** non è visibile, utilizza il menu ![Menu](images/Menu_16.png) per aprire la pagina.

1.  Fai clic su un intento per aprirlo. 

    Per questo esempio, l'intento `#place_order` definisce la funzione per eseguire ordini per un rivenditore online.

    ![Seleziona l'intento #place_order](images/oe-intent.png)

1.  Esamina gli esempi dell'intento per citazioni di entità potenziali. Evidenzia una citazione di entità potenziale dagli esempi di intento. 

    In questo esempio, `computer` è la citazione di entità. 

    ![Esamina gli esempi dell'intento](images/oe-intent-review.png)

    L'icona di modifica ![Icona di modifica](images/oe-intent-edit.png) viene utilizzata per modificare un esempio utente dell'intento; non è correlata all'aggiunta di annotazioni.
    {: tip}

1.  Viene aperta una casella di ricerca che puoi utilizzare per ricercare l'entità di cui la parola o la frase evidenziata costituisce una citazione. 

    ![Stato iniziale della casella di ricerca](images/oe-intent-search1.png)

    In questo esempio, la ricerca di `prod` visualizza le corrispondenze per l'entità `@product`.

    ![Casella di ricerca con il parametro di ricerca prod](images/oe-intent-search2.png)

    Se l'entità ha valori di entità esistenti, vengono visualizzati solo a scopo informativo. Stai aggiungendo l'annotazione all'entità, non a qualsiasi valore di entità specifico. 

    Se desideri insegnare al modello che la citazione è un sinonimo di un valore di entità esistente, puoi associarla a uno specifico valore di entità.
    {: important}

    Per associare la citazione a uno specifico valore di entità, segui questi passi: 

    1.  Immetti il nome entità completo e il valore nel campo di ricerca. Ad esempio, immetti `@product:IT`.
    1.  Quando nel menu a discesa viene visualizzato il valore di entità, selezionalo. 

1.  Seleziona l'entità a cui desideri aggiungere l'annotazione. 

    In questo esempio, `computer` viene aggiunto come un'annotazione per l'entità `@product`.

    Crea *almeno* 10 annotazioni per ciascuna entità contestuale; sono consigliate più annotazioni per l'uso di produzione.
    {: important}

1.  Se nessuna delle entità è appropriata, puoi creare una nuova entità scegliendo **@(create new entity)**.

1.  Ripeti questo processo per ciascuna citazione di entità che desideri annotare. 

    Assicurati di annotare ogni citazione di un tipo di entità presente negli esempi utente che modifichi. Per ulteriori dettagli, vedi [Importanza di quello che non annoti](#entities-counter-examples).
    {: important}

1.  Ora, fai clic sull'annotazione che hai appena creato. Si apre una casella che dice `Go to: <entity-name>`. Facendo clic su tale link, vai direttamente all'entità. 

    ![Verifica del valore computer per l'entità product](images/oe-verify-value.png)

    L'annotazione viene aggiunta all'entità associata ad essa e il sistema inizia ad addestrarsi sui nuovi dati. 

    Il termine annotato viene aggiunto all'entità come un nuovo valore di dizionario. Se hai associato il termine annotato a un valore di entità esistente, il termine viene aggiunto come sinonimo di tale valore di entità invece che come valore di entità indipendente.
    {: important}

1.  Per vedere tutte le citazioni che hai annotato per una particolare entità, dalla pagina di configurazione dell'entità, fai clic sulla scheda **Annotazione**.

    ![Selettore della vista dell'annotazione evidenziato](images/oe-annotate2.png)

    Le entità contestuali comprendono i valori che non hai definito esplicitamente. Il sistema fa delle previsioni sui valori di entità aggiuntivi in base a come hai annotato gli esempi utente e utilizza tali valori per addestrare altre entità. Gli esempi utente simili vengono aggiunti alla vista *Annotazione*, quindi puoi vedere in che modo questa opzione influisce sull'addestramento.
    {: note}

    Se non desideri che le tue entità contestuali utilizzino questa comprensione ampliata dei valori di entità, seleziona tutti gli esempi utente presenti nella vista *Annotazione* relativi a tale entità e poi fai clic su **Elimina**.

Il seguente video dimostra come annotare le citazioni di entità.

<iframe class="embed-responsive-item" id="youtubeplayer0" title="Annotazione di citazioni di entità" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/3WjzJpLsnhQ" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Per affrontare un'esercitazione che ti mostra come definire le entità contestuali prima di aggiungerne di tue, vai a [Tutorial: Defining contextual entities ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/cloud/garage/demo/try-watson-assistant-contextual-entities){: new_window}.

### Importanza di quello che non annoti
{: #entities-counter-examples}

Se hai un esempio di intento con un'annotazione e un'altra parola presente in tale esempio corrisponde al valore o a un sinonimo della stessa entità, ma il valore *non* è stato annotato, tale omissione ha un impatto. Il modello impara anche dal contesto del termine che non hai annotato. Di conseguenza, se etichetti un termine come una citazione di un'entità in un esempio utente, assicurati di etichettare anche le altre citazioni applicabili. 

1.  L'intento `#Customer_Care_Appointments` include due esempi di intento con la parola `visit`.

    ![Esempi intento visit](images/oe-counter-1.png)

1.  Nel primo esempio, desideri annotare la parola `visit` come un valore di entità dell'entità `@meeting`. Ciò rende `visit` equivalente ad altri valori di entità `@meeting` come ad esempio `appointment`, come in "I'd like to make an appointment" o "I'd like to schedule a visit".

    ![Entità @meeting](images/oe-counter-2.png)

1.  Per il secondo esempio, la parola `visit` viene utilizzata in un contesto diverso da un meeting (riunione). In questo caso, puoi selezionare la parola `appointment` dall'esempio di intento e annotarlo come un valore di entità dell'entità `@meeting`. Il modello impara dal fatto che la parola `visit` nello stesso esempio non è annotata. 

    ![Visit non selezionato](images/oe-counter-3.png)

## Abilitazione delle entità di sistema
{: #entities-enable-system-entities}

Il servizio {{site.data.keyword.conversationshort}} fornisce una serie di *entità di sistema*, che sono delle entità comuni che puoi utilizzare per qualsiasi applicazione. L'abilitazione di un'entità di sistema consente di popolare rapidamente la tua capacità con i dati di addestramento comuni a molti casi di utilizzo. 

Le entità di sistema possono essere utilizzate per riconoscere un'ampia gamma di valori per i tipi di oggetti che rappresentano. Ad esempio, l'entità di sistema `@sys-number` corrisponde a qualsiasi valore numerico, compresi numeri interi, frazioni decimali o anche numeri scritti come parole.

Le entità di sistema vengono mantenute centralmente, per cui gli aggiornamenti sono disponibili automaticamente. Non puoi modificare le entità di sistema.

1.  Nella scheda entità, fai clic su **Entità di sistema**.

    ![Schermata della scheda "Entità di sistema"](images/system_entities_1.png)

1.  Scorri l'elenco delle entità di sistema per scegliere quelle che sono utili per la tua applicazione.
    - Per visualizzare ulteriori informazioni su un'entità di sistema, inclusi esempi di input corrispondente, fai clic sull'entità nell'elenco.
    - Per i dettagli sulle entità di sistema disponibili, vedi [Entità di sistema](/docs/services/assistant?topic=assistant-system-entities).

1.  Fai clic sull'interruttore di attivazione accanto all'entità di sistema per abilitarla o disabilitarla.

Dopo aver abilitato le entità di sistema, il servizio {{site.data.keyword.conversationshort}} inizia di nuovo l'addestramento. Al termine dell'addestramento, puoi utilizzare le entità.

## Limiti di entità
{: #entities-limits}

Il numero di entità, valori di entità e sinonimi che puoi creare dipende dal tuo piano di servizio {{site.data.keyword.conversationshort}}:

| Piano di servizio | Entità per capacità | Valori di entità per capacità | Sinonimi di entità per capacità |
|-------------------|-------------------:|------------------------:|--------------------------:|
| Premium           |               1000 |                 100.000 |                   100.000 |
| Plus              |               1000 |                 100.000 |                   100.000 |
| Standard          |               1000 |                 100.000 |                   100.000 |
| Lite              |                 25 |                 100.000 |                   100.000 |
{: caption="Dettagli piano di servizio" caption-side="top"}

Le entità di sistema che abiliti per l'uso vengono conteggiate nei totali di utilizzo del piano.

| Piano di servizio | Entità contestuali e annotazioni |
|--------------|------------------------------------:|
| Premium      |        30 entità contestuali con 3000 annotazioni |
| Plus         |        20 entità contestuali con 2000 annotazioni |
| Standard     |        20 entità contestuali con 2000 annotazioni |
| Lite         |        10 entità contestuali con 1000 annotazioni |
{: caption="Dettagli del piano di servizio - continua" caption-side="top"}

## Modifica delle entità
{: #entities-edit}

Puoi fare clic su qualsiasi entità nell'elenco per aprirla per la modifica. Puoi rinominare o eliminare le entità e puoi aggiungere, modificare o eliminare valori, sinonimi o modelli.

Se modifichi il tipo di entità da `synonym` a `pattern` o viceversa, i valori esistenti verranno convertiti ma potrebbero non essere utili così come sono.
{: note}

## Ricerca delle entità
{: #entities-search}

Utilizza la funzione Ricerca per trovare i nomi, i valori e i sinonimi di entità. 

1.  Dalla pagina **Entità**, fai clic sull'icona Ricerca. 

    ![Icona Ricerca nell'intestazione della pagina Intenti](images/header-search-icon.png)

    Le entità di sistema non sono ricercabili.
    {: note}

1.  Immetti una frase o un termine di ricerca.

    ![Termine di ricerca entità](images/searchent_1.png)

Vengono visualizzate le entità che contengono il tuo termine di ricerca e gli esempi corrispondenti.

  ![Restituzione ricerca entità](images/searchent_2.png)

## Esportazione di entità
{: #entities-export}

Puoi esportare una serie di entità in un file CSV in modo da poterle importare e riutilizzare in un'altra applicazione {{site.data.keyword.conversationshort}}.

- Le informazioni sul modello sono incluse nell'esportazione CSV. Qualsiasi stringa contenuta tra `/` verrà considerata come un modello (invece di un sinonimo).
- Le annotazioni associate alle entità contestuali non vengono esportate. Devi esportare l'intera capacità di dialogo per acquisire sia il valore di entità che le annotazioni associate. 

1.  Seleziona le entità che desideri, quindi fai clic su **Esporta**.

    ![Pulsante Esporta entità](images/ExportEntity.png)

## Importazione di entità
{: #entities-import}

Se hai un numero elevato di entità, è più facile importarle da un file CSV (comma-separated value) anziché definirle una ad una nello strumento {{site.data.keyword.conversationshort}}.

Le annotazioni di entità non sono incluse nell'importazione di un file CSV dell'entità. Devi importare l'intera capacità di dialogo per conservare le annotazioni associate per un'entità contestuale in tale capacità. Se esporti e importi solo le entità, le entità contestuali che hai esportato vengono trattate come entità basate sul dizionario una volta che le hai importate.
{: note}

1.  Raccogli le entità in un file CSV o esportale da un foglio di calcolo in un file CSV. Il formato richiesto per ogni riga nel file è il seguente:

    ```
    <entity>,<value>,<synonyms>
    ```
    {: screen}

    dove &lt;entity&gt; è il nome di un'entità, &lt;value&gt; è il valore dell'entità e &lt;synonyms&gt; è un elenco di sinonimi separati da virgole per questo valore.

    ```
    weekday,Monday,Mon
    weekday,Tuesday,Tue,Tues
    weekday,Wednesday,Wed
    weekday,Thursday,Thur,Thu,Thurs
    weekday,Friday,Fri
    weekday,Saturday,Sat
    weekday,Sunday,Sun
    month,January,Jan
    month,February,Feb
    month,March,Mar
    month,April,Apr
    month,May
    ```
    {: screen}

    L'importazione di un file CSV supporta anche i modelli. Qualsiasi stringa contenuta tra `/` verrà considerata come un modello (invece di un sinonimo).

    ```
    ContactInfo,localPhone,/(\d{3})-(\d{4})/
    ContactInfo,fullUSphone,/(\d{3})-(\d{3})-(\d{4})/
    ContactInfo,internationalPhone,/^(\(?\+?[0-9]*\)?)?[0-9_\- \(\)]*$/
    ContactInfo,email,/\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}\b/
    ContactInfo,website,/(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/
    ```
    {: screen}

    Salva il file CSV con la codifica UTF-8 e nessun contrassegno di ordine di byte (BOM). La dimensione massima del file CSV è 10 MB. Se il tuo file CSV è più grande, potresti suddividerlo in più file e importarli separatamente.  Nello strumento {{site.data.keyword.conversationshort}}, apri la capacità di dialogo e poi fai clic sulla scheda **Entità**.
    {: tip}

1.  Fai clic su ![Importa](images/importGA.png) e trascina un file oppure seleziona un file dal tuo computer. Il file viene convalidato e importato e il sistema comincia ad addestrarsi sui nuovi dati.

Puoi visualizzare le entità importate sulla scheda Entità. Per vedere le nuove entità potresti dover aggiornare la pagina.

## Eliminazione di entità
{: #entities-delete}

Puoi selezionare una serie di entità da eliminare.

**IMPORTANTE**: eliminando le entità elimini anche i valori, i sinonimi e i modelli associati e questi elementi non potranno più essere recuperati. Tutti i nodi di dialogo che fanno riferimento a queste entità o a questi valori devono essere aggiornati manualmente per non fare più riferimento al contenuto eliminato.

1.  Seleziona le entità che desideri eliminare, quindi fai clic su **Elimina**.

    ![Pulsante Elimina entità](images/DeleteEntity.png)
