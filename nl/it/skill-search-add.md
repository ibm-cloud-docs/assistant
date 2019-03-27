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

# Creazione di una capacità di ricerca 
{: #skill-search-add}

Un assistente utilizza una *capacità di ricerca* per instradare le domande del cliente complesse al servizio {{site.data.keyword.discoveryfull}}. {{site.data.keyword.discoveryshort}} tratta l'input utente come una query di ricerca. Trova le informazioni pertinenti per la query da un'origine dati esterna e le restituisce all'assistente.
{: shortdesc}

Questa funzione è disponibile solo per l'utilizzo da parte dei partecipanti del programma beta. Per scoprire come richiedere l'accesso, vedi [Partecipa al programma beta](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

![Beta](images/beta.png) IBM rilascia servizi, funzioni e supporto linguistico classificati come beta per la tua valutazione. Queste funzioni potrebbero essere instabili, cambiare frequentemente e potrebbero essere sospese con breve preavviso. Inoltre, le funzioni beta potrebbero non fornire lo stesso livello di prestazioni o di compatibilità fornito generalmente dalle funzioni e non sono progettate per essere utilizzate in un ambiente di produzione.

Puoi aggiungere una capacità di ricerca a un assistente. Vedi [Limiti della capacità](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits) per informazioni sui limiti per piano.

La capacità di ricerca cerca le informazioni da una raccolta di dati che crei utilizzando il servizio {{site.data.keyword.discoveryshort}}. {{site.data.keyword.discoveryshort}} è un servizio che indicizza, converte e normalizza i tuoi dati non strutturati. Il servizio applica l'analisi dei dati e l'intuizione cognitiva per arricchire i tuoi dati in modo che sia più facile per te trovare e richiamare informazioni importanti da essi in un secondo momento. Per informazioni su {{site.data.keyword.discoveryshort}}, vedi la [documentazione del prodotto ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/discovery?topic=discovery-about).

Il servizio {{site.data.keyword.discoveryfull}} viene attivato nei seguenti modi:

- **Nodi Altro**: effettua una ricerca in un'origine dati esterna per una domanda pertinente quando nessuno dei nodi di dialogo può far fronte alla query dell'utente. Invece di mostrare un messaggio standard, come ad esempio `I don't know how to help you with that.` l'assistente può rispondere, `Maybe this information can help:` seguito dal passaggio restituito dalla ricerca. Se una capacità di ricerca viene collegata al tuo assistente, ogni volta che viene attivato il nodo `anything_else`, invece di visualizzare la risposta del nodo, viene effettuata una ricerca. L'assistente passa l'input utente come ad esempio la query alla tua capacità di ricerca e restituisce i risultati della ricerca come risposta.
- **Tipo di risposta della ricerca**: se aggiungi un tipo di risposta della ricerca a un nodo di dialogo, il servizio richiama un passaggio da un'origine dati esterna e lo restituisce come risposta a una determinata domanda. Questo tipo di ricerca si verifica solo quando viene elaborato il singolo nodo di dialogo. Questo approccio è utile se desideri restringere una query utente prima di eseguire una ricerca. Ad esempio, il ramo di dialogo potrebbe raccogliere le informazioni sul tipo di dispositivo che il cliente vuole comprare. Quando conosci la marca e il modello, puoi inviare una parola chiave model nella query inoltrata alla capacità di ricerca e ottenere dei risultati migliori.
- **Solo capacità di ricerca**: se viene collegata solo una capacità di ricerca a un assistente e nessuna capacità di dialogo è collegata all'assistente, viene inoltrata una query di ricerca al servizio {{site.data.keyword.discoveryshort}} quando viene ricevuto un input utente da uno dei canali di integrazione dell'assistente.

## Creazione di una capacità di ricerca 
{: #skill-search-add-task}

Se non lo hai fatto, completa i passi preliminari nell'[esercitazione introduttiva](/docs/services/assistant?topic=assistant-getting-started#getting-started-prerequisites) per creare un'istanza del servizio {{site.data.keyword.conversationshort}} e avviare lo strumento {{site.data.keyword.conversationshort}}.

Utilizza lo strumento {{site.data.keyword.conversationshort}} per creare le capacità. Per creare una capacità di ricerca, segui questa procedura:

1.  Fai clic sulla scheda **Skills**. 

1.  Fai clic su **Create new**.

1.  Fai clic su **Add** per creare una *capacità di ricerca*.

1.  Specifica i dettagli per la nuova capacità:
    - **Name**: un nome lungo massimo 100 caratteri. Un nome è obbligatorio.
    - **Description**: una descrizione facoltativa lunga massimo 200 caratteri. 

1.  Fai clic su **Create**.

I passi rimanenti differiscono a seconda se hai accesso a un'istanza del servizio {{site.data.keyword.discoveryshort}} esistente con delle raccolte create oppure no. Segui la procedura appropriata per la tua situazione:

- [Connessione a un'istanza Watson Discovery esistente](#skill-search-add-connect-discovery)
- [Crea un'istanza Watson Discovery](#skill-search-add-create-discovery)

## Connessione a un'istanza del servizio Watson Discovery esistente
{: #skill-search-add-connect-discovery}

1.  Scegli l'istanza del servizio {{site.data.keyword.discoveryshort}} da cui vuoi estrarre le informazioni.
{: #choose-d-instance}

    Ogni istanza del servizio {{site.data.keyword.discoveryshort}} a cui hai accesso viene visualizzata nell'elenco.

    Se vedi un'avvertenza che alcune delle tue istanze del servizio {{site.data.keyword.discoveryshort}} non hanno le credenziali configurate, significa che hai accesso ad almeno un'istanza che non hai mai aperto direttamente dal dashboard {{site.data.keyword.cloud_notm}}. Devi accedere a un'istanza del servizio di cui sono state create le credenziali. E le credenziali devono esistere prima che {{site.data.keyword.conversationshort}} possa stabilire una connessione all'istanza del servizio {{site.data.keyword.discoveryshort}} al tuo posto. Se pensi che un'istanza del servizio {{site.data.keyword.discoveryshort}} dovrebbe essere elencata ma non lo è, apri l'istanza direttamente dal dashboard {{site.data.keyword.cloud_notm}} per generarne le credenziali.

1.  Indica la raccolta di dati da utilizzare, effettuando una delle seguenti azioni:
{: #pick-data-collection}

    - Scegli una raccolta dati esistente.

      Puoi far clic sul link *Open in Discovery* per controllare la configurazione di una raccolta di dati prima di decidere quale utilizzare.

      Vai a [Configura la ricerca](#beta-search-skill-add-configure).

    - Se non hai una raccolta o non vuoi utilizzarne una di quelle elencate, fai clic su **Create a new collection** per aggiungerne una. Segui la procedura in [Crea una raccolta di dati](#beta-search-skill-add-create-discovery-collection).

      Il pulsante **Create a new collection** non viene visualizzato se hai raggiunto il limite di numero di raccolte che ti è consentito creare in base al tuo piano di servizio {{site.data.keyword.discoveryshort}}. Vedi [Piani dei prezzi {{site.data.keyword.discoveryshort}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/discovery/discovery-about?topic=discovery-discovery-pricing-plans) per i dettagli sul limite del piano.

## Crea un'istanza del servizio Watson Discovery
{: #skill-search-add-create-discovery}

1.  Per creare un'istanza del servizio {{site.data.keyword.discoveryshort}}, fai clic su **Create new collection**.

    Un'istanza del servizio {{site.data.keyword.discoveryshort}} viene creata per te e si apre una pagina di configurazione per la nuova istanza del servizio {{site.data.keyword.discoveryshort}}.

    Viene eseguito il provisioning di un'istanza del servizio del piano Lite in {{site.data.keyword.Bluemix_notm}}, indipendentemente dal piano di servizio {{site.data.keyword.conversationshort}} che utilizzi. Se vuoi creare l'istanza del servizio {{site.data.keyword.discoveryshort}} che fa parte di un piano diverso, fermati qui. Crea l'istanza del servizio direttamente dal [Catalogo {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/catalog/services/discovery) e segui invece la procedura [Connessione a un'istanza del servizio {{site.data.keyword.discoveryshort}} esistente](#skill-search-add-connect-discovery).
    {: important}

1.  Controlla i termini e le condizioni di utilizzo dell'istanza e quindi fai clic su **Accept** per continuare.

1.  [Crea una raccolta di dati](#skill-search-add-create-discovery-collection).

## Crea una raccolta di dati
{: #skill-search-add-create-discovery-collection}

Non tentare di aggiungere un'origine dati pre-arricchita denominata *Watson Discovery News* alla tua istanza. Non è un tipo di dati che è possibile ricercare da {{site.data.keyword.conversationshort}}.
{: important}

1.  Per creare una raccolta {{site.data.keyword.discoveryshort}}, effettua una delle seguenti azioni:

      - Per creare una raccolta da dati archiviati in un tipo di origine dati per cui {{site.data.keyword.discoveryshort}} fornisce il supporto integrato, fai clic su **Connect to data source**.

        1.  Seleziona un tipo di origine dati.
        1.  Fornisci le informazioni richieste per l'origine dati che scegli e quindi fai clic su **Connect**.

            Per ulteriori dettagli, vedi [Connessione alle origini dati ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/discovery?topic=discovery-sources).
        1.  Indica la frequenza con cui vuoi che i dati dall'origine dati vengano sincronizzati con la raccolta che stai creando in {{site.data.keyword.discoveryshort}}.
        1.  Specifica le informazioni che vuoi estrarre dall'origine dati e includile nella tua raccolta {{site.data.keyword.discoveryshort}}.

            Le opzioni che vengono visualizzate differiscono a seconda del tipo di origine dati.

            - Per un'origine dati Salesforce, seleziona i tipi di oggetto che vuoi estrarre dai documenti di origine. Puoi selezionare un [Tipo di oggetto caso ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_case.htm#!) che rappresenta ad esempio un *caso*, che è un problema o un errore del cliente.
            - Per un'origine dati Sharepoint, specifica i percorsi.
            - Per i repository di file, specifica le directory o i file.

        1.  Fai clic su **Save and sync data**.

            Viene creata la raccolta di dati. Dopo il completamento del processo, viene visualizzata una pagina di riepilogo nello strumento {{site.data.keyword.discoveryshort}} in una scheda del browser web separata.
        1.  Fai clic su **Configure your skill in {{site.data.keyword.conversationshort}}** per ritornare allo strumento {{site.data.keyword.conversationshort}}.

      - Per creare una raccolta caricando dei documenti, fai clic su **Upload your own data**.

        1.  Prima definisci la raccolta e poi carica i documenti. Fornisci le seguenti informazioni:

            - Nome della raccolta. Il nome deve essere univoco per questa istanza del servizio.
            - Configurazione. Puoi scegliere di utilizzare un template di configurazione predefinito o una configurazione salvata. Vedi [Configurazione del tuo servizio ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/discovery?topic=discovery-configservice) per ulteriori informazioni sulle configurazioni.
            - Lingua. Seleziona la lingua dei file che aggiungerai a questa raccolta. Vedi [Supporto linguistico ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/discovery?topic=discovery-language-support) per informazioni sulle lingue supportate da {{site.data.keyword.discoveryshort}}.
        1.  Carica i documenti.

            I tipi di file supportati includono file PDF, HTML, JSON e DOC. Per ulteriori dettagli, vedi [Aggiunta di contenuto ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/discovery?topic=discovery-addcontent).
            {: note}

            Non è disponibile una sincronizzazione continua dei documenti caricati. Se vuoi selezionare le modifiche apportate a un documento, carica una versione successiva del documento.

Attendi che la raccolta sia stata completamente inserita prima di tornare a {{site.data.keyword.conversationshort}}.

## Configura la ricerca
{: #skill-search-add-configure}

1.  Dalla pagina della capacità di ricerca {{site.data.keyword.conversationshort}}, fai clic su **Configure**.

1.  Prepara messaggi diversi da condividere con gli utenti a seconda se la ricerca è stata eseguita correttamente.

    <table>
    <caption>Messaggi del risultato della ricerca</caption>
    <tr>
      <th>Nome campo</th>
      <th>Scenario</th>
      <th>Messaggio di esempio </th>
    </tr>
    <tr>
      <td>Messaggio</td>
      <td>Vengono restituiti i risultati della ricerca</td>
      <td>I found this information which might be helpful: </td>
    </tr>
    <tr>
      <td>Nessun risultato trovato</td>
      <td>Non è stato trovato alcun risultato</td>
      <td>I searched my knowledge base for information that might address your query, but did not find anything useful to share.</td>
    </tr>
    <tr>
      <td>Messaggio di errore</td>
      <td>Il servizio non è stato in grado di eseguire la ricerca per qualche motivo</td>
      <td>I might have information that could help address your query, but am unable to search my knowledge base at the moment.</td>
    </tr>
    </table>

1.  Scegli i campi della raccolta {{site.data.keyword.discoveryshort}} da cui vuoi estrarre il testo.

    I campi disponibili differiscono in base ai dati che hai inserito e alla configurazione che hai utilizzato per inserirli.

    Ogni risultato della ricerca può essere formato da queste informazioni:

    - **Titolo**: il titolo del risultato della ricerca. Utilizza il titolo, il nome o un tipo di campo simile dalla raccolta come titolo del risultato della ricerca.

      Deve essere selezionato qualcosa di diverso da `None` per le integrazioni Facebook e Slack per non visualizzare affatto la risposta.
    - **Corpo**: la descrizione del risultato della ricerca. Utilizza un campo astratto, di riepilogo o di evidenziazione dalla raccolta come corpo del risultato della ricerca.

      Deve essere selezionato qualcosa di diverso da `None` in modo che le integrazioni Facebook e Slack per non visualizzino affatto la risposta.
    - **Url**: un collegamento ipertestuale all'oggetto dati originale nella sua origine dati nativa. La maggior parte delle origini dati fornite forniscono URL pubblici autoreferenziali per gli oggetti nell'archivio per supportare l'accesso diretto.

      L'URL risultante deve essere valido e ricercabile in modo che l'integrazione Slack includa l'URL nella risposta e l'integrazione Facebook non visualizzi affatto la risposta. `None` è una selezione accettabile per le integrazioni Facebook e Slack.

    Per assistenza, vedi [Suggerimenti per la selezione del campo di raccolta](#skill-search-add-field-tips).
  
    Devi scegliere un valore diverso da `None` per almeno una delle opzioni.

    Se non è disponibile alcuna opzione dai campi a discesa, potresti dover dare più tempo a {{site.data.keyword.discoveryshort}} per terminare la creazione della raccolta. Altrimenti, la tua raccolta potrebbe non contenere alcun documento o contenere degli errori di inserimento che devono essere prima risolti.

1.  Nel pannello di anteprima, immetti un messaggio di testo per vedere i risultati che vengono restituiti quando le tue scelte di configurazione vengono applicate alla ricerca. Apporta le modifiche secondo necessità.

1.  Fai clic su **Crea**.

Se vuoi modificare la configurazione in un secondo momento, apri nuovamente la capacità di ricerca e apporta le modifiche. Non devi salvare le modifiche mentre le apporti; vengono applicate immediatamente. Quando sei soddisfatto dei risultati della ricerca, fai clic su **Save** per terminare la configurazione della capacità di ricerca.

## Passi successivi
{: #skill-search-add-next-steps}

Una volta creata la capacità, comparirà come un tile nella pagina Skills. 

La capacità di ricerca non può interagire con i clienti fino a quando non viene aggiunta a un assistente e quest'ultimo viene distribuito. Vedi [Creazione di assistenti](/docs/services/assistant?topic=assistant-assistant-add).

Quando colleghi una capacità di dialogo e di ricerca a un assistente, la capacità di ricerca viene attivata automaticamente se l'input utente viene elaborato dalla capacità di dialogo e non può essere indirizzato da alcuno dei suoi nodi di dialogo. Invece di rispondere con una risposta generica dal nodo `anything_else`, viene avviata una ricerca che utilizza l'input utente come propria stringa di query.

Se lo desideri, puoi definire una query di ricerca specifica da richiamare in risposta a una determinata condizione del nodo. A tale scopo, aggiungi un tipo di risposta della ricerca al nodo di dialogo. Per ulteriori dettagli, vedi [Risposte](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia).

Se avvii un qualsiasi tipo di ricerca dalla tua capacità di ricerca, verifica il dialogo per assicurarti che la ricerca sia stata attivata come previsto. Ad esempio, se non stai utilizzando i tipi di risposta della ricerca, verifica che una ricerca venga attivata solo quando nessun nodo di dialogo esistente può risolvere l'input utente. E ogni volta che viene attivata una ricerca, assicurati che restituisca dei risultati significativi.

### Suggerimenti per la selezione del campo di raccolta 
{: #skill-search-add-field-tips}

I campi della raccolta appropriati per estrarre i dati variano a seconda dell'origine dati e della configurazione della tua raccolta che hai utilizzato per inserire l'origine dati. Per ulteriori informazioni sulla struttura dei documenti nella tua raccolta, includi i nomi dei campi che contengono le informazioni che potresti voler estrarre, apri la raccolta nello strumento {{site.data.keyword.discoveryshort}} e fai quindi clic su **View data schema**.

La seguente tabella fornisce i campi della raccolta che puoi provare quando inizi. Questi suggerimenti assumono che hai utilizzato il template di configurazione predefinito durante la creazione della raccolta.

| Tipo di origine dati | Titolo | Corpo | Url |
|--------------------|-------|------|-----|
| Documenti PDF caricati | enriched_text.concepts.text | testo | Nessuno |
| Box                | nome | description | listing_url |

I campi della raccolta vengono creati quando viene creata la raccolta. Per ulteriori informazioni sui campi che vengono generati quando utilizzi il template di configurazione predefinito per l'inserimento, come ad esempio `enriched_text.concepts.text`, vedi [Configurazione del tuo servizio > Aggiunta degli arricchimenti ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/discovery?topic=discovery-configservice#adding-enrichments).

### Aggiunta della capacità a un assistente 
{: #skill-search-add-to-assistant}

Puoi aggiungere una capacità a un assistente. Devi aprire il tile dell'assistente e aggiungere la capacità all'assistente dalla pagina di configurazione dell'assistente; non puoi scegliere l'assistente che utilizzerà la capacità dalla pagina di configurazione della capacità. 

Una capacità di ricerca può essere utilizzata da più di un assistente. 

1.  Dalla scheda Assistants, fai clic per aprire il tile per l'assistente a cui desideri aggiungere la capacità. 

1.  Fai clic su **Add Search Skill**.

1.  Fai clic su **Add existing skill**.

    Fai clic sulla capacità che desideri aggiungere dalle capacità disponibili visualizzate. 

Configura almeno un canale di integrazione di test. Non puoi verificare la capacità di ricerca dal riquadro Try it out. Verifica la capacità da un canale di integrazione immettendo delle query che attivano la ricerca. Assicurati che la ricerca sia stata attivata correttamente e stia restituendo dei risultati pertinenti.

L'integrazione del link condivisibile non funziona al momento per gli assistenti con una capacità di ricerca.
{: important}
