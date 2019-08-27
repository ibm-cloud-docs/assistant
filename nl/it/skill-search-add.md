---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-02"

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

# Creazione di una capacità di ricerca ![Solo piano Plus o Premium](images/plus.png)
{: #skill-search-add}

Un assistente utilizza una *capacità di ricerca* per instradare le domande del cliente complesse al servizio {{site.data.keyword.discoveryfull}}. {{site.data.keyword.discoveryshort}} tratta l'input utente come una query di ricerca. Trova le informazioni pertinenti per la query da un'origine dati esterna e le restituisce all'assistente.
{: shortdesc}

Questa funzione è disponibile solo per gli utenti del piano Plus o Premium.
{: note}

Aggiungi una capacità di ricerca al tuo assistente per evitare che dica cose come, `I'm sorry. I can't help you with that`. Invece, l'assistente può interrogare i dati o i documenti aziendali esistenti per vedere se è possibile trovare e condividere delle informazioni utili con il cliente.

![Mostra un risultato della ricerca nell'integrazione Preview link](images/search-skill-preview-link.png)

Il seguente video di 4 minuti fornisce una panoramica della capacità di ricerca. 

<iframe class="embed-responsive-item" id="youtubeplayer" title="Panoramica della capacità di ricerca" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/ZcgGf8J2Cfw?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Per ulteriori informazioni su come la capacità di ricerca può giovare al tuo business, [leggi questo post di blog ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://medium.com/ibm-watson/adding-search-to-watson-assistant-99e4e81839e5){: new_window}.

## Modalità di funzionamento
{: #skill-search-add-how}

La capacità di ricerca cerca le informazioni da una raccolta di dati che crei utilizzando il servizio {{site.data.keyword.discoveryshort}}.

{{site.data.keyword.discoveryshort}} è un servizio che indicizza, converte e normalizza i tuoi dati non strutturati. Il prodotto applica l'analisi dei dati e l'intuizione cognitiva per arricchire i tuoi dati in modo che sia più facile per te trovare e richiamare informazioni importanti da essi in un secondo momento. Per informazioni su {{site.data.keyword.discoveryshort}}, vedi la [documentazione del prodotto ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/discovery?topic=discovery-about){: new_window}.

Normalmente, il tipo di raccolta dati che aggiungi a {{site.data.keyword.discoveryshort}} e a cui accede il tuo assistente, contiene informazioni di proprietà della tua azienda. Queste informazioni proprietarie possono includere FAQ, materiale pubblicitario, manuali tecnici o documenti scritti da esperti in materia. Estrae questa densa raccolta di informazioni proprietarie per trovare velocemente delle risposte alle domande del cliente.

Il seguente diagramma illustra come viene elaborato l'input utente quando vengono aggiunte una capacità di dialogo e di ricerca a un assistente. 

![Diagramma che mostra come viene data risposta all'input utente dal dialogo e altre domande a cui viene data risposta dalla ricerca.](images/search-skill-diagram.png)

## Prima di iniziare
{: #skill-search-add-prereqs}

Se non hai un'istanza del servizio {{site.data.keyword.discoveryshort}}, ti viene fornita un'istanza del piano Lite gratuita come parte di questo processo. Se hai un'istanza del servizio {{site.data.keyword.discoveryshort}} esistente, connettiti ad essa; non ti viene richiesto di creare una nuova istanza come parte di questo processo.

Se crei prima un'istanza Discovery, non aggiungere l'origine dati pre-arricchita denominata *Watson Discovery News* alla tua istanza. Non è un tipo di dati che è possibile ricercare da {{site.data.keyword.conversationshort}}.
{: tip}

## Crea la capacità di ricerca
{: #skill-search-add-task}

1.  Fai clic sulla scheda **Skills** e poi fai clic su **Create skill**.

1.  Fai clic sul tile *Search skill* e poi su **Next**.

    Puoi selezionare solo la capacità di ricerca se sei un utente del piano Plus o Premium.
    {: note}

1.  Specifica i dettagli per la nuova capacità:
    - **Name**: un nome lungo massimo 100 caratteri. Un nome è obbligatorio.
    - **Description**: una descrizione facoltativa lunga massimo 200 caratteri.

1.  Fai clic su **Continue**. 

I passi rimanenti differiscono a seconda se hai accesso a un'istanza del servizio {{site.data.keyword.discoveryshort}} esistente con delle raccolte create oppure no. Segui la procedura appropriata per la tua situazione:

- [Connessione a un'istanza Watson Discovery esistente](#skill-search-add-connect-discovery)
- [Crea un'istanza Watson Discovery](#skill-search-add-create-discovery)

## Connessione a un'istanza del servizio Watson Discovery esistente
{: #skill-search-add-connect-discovery}

1.  Scegli l'istanza del servizio {{site.data.keyword.discoveryshort}} da cui vuoi estrarre le informazioni.
{: #choose-d-instance}

    Ogni istanza del servizio {{site.data.keyword.discoveryshort}} a cui hai accesso viene visualizzata nell'elenco.

    Se vedi un'avvertenza che alcune delle tue istanze del servizio {{site.data.keyword.discoveryshort}} non hanno le credenziali configurate, significa che puoi accedere ad almeno un'istanza che non hai mai aperto direttamente dal dashboard {{site.data.keyword.cloud_notm}}. Devi accedere a un'istanza del servizio di cui sono state create le credenziali. E le credenziali devono esistere prima che {{site.data.keyword.conversationshort}} possa stabilire una connessione all'istanza del servizio {{site.data.keyword.discoveryshort}} al tuo posto. Se pensi che un'istanza del servizio {{site.data.keyword.discoveryshort}} non è presente nell'elenco, apri l'istanza direttamente dal dashboard {{site.data.keyword.cloud}} per generarne le credenziali.
    {: note}

1.  Indica la raccolta di dati da utilizzare, effettuando una delle seguenti azioni:
{: #pick-data-collection}

    - Scegli una raccolta dati esistente.

      Puoi far clic sul link *Open in Discovery* per controllare la configurazione di una raccolta di dati prima di decidere quale utilizzare.

      Vai a [Configura la ricerca](#search-skill-add-configure).

    - Se non hai una raccolta o non vuoi utilizzarne una di quelle elencate, fai clic su **Create a new collection** per aggiungerne una. Segui la procedura in [Crea una raccolta di dati](#search-skill-add-create-discovery-collection).

      Il pulsante **Create a new collection** non viene visualizzato se hai raggiunto il limite di numero di raccolte che ti è consentito creare in base al tuo piano di servizio {{site.data.keyword.discoveryshort}}. Vedi [Piani dei prezzi {{site.data.keyword.discoveryshort}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/discovery/discovery-about?topic=discovery-discovery-pricing-plans){: new_window} per i dettagli sul limite del piano.
      {: note}

## Crea un'istanza del servizio Watson Discovery
{: #skill-search-add-create-discovery}

1.  Per creare un'istanza del servizio {{site.data.keyword.discoveryshort}}, fai clic su **Create new collection**.

    Se non hai un'istanza del servizio {{site.data.keyword.discoveryshort}} esistente, ti viene creata un'istanza gratuita del servizio {{site.data.keyword.discoveryshort}}.

    Viene eseguito il provisioning di un'istanza del servizio del piano Lite in {{site.data.keyword.Bluemix_notm}}, indipendentemente dal tipo di piano di servizio {{site.data.keyword.conversationshort}} che hai.
    {: note}

1.  Controlla i termini e le condizioni di utilizzo dell'istanza e quindi fai clic su **Accept** per continuare.

1.  [Crea una raccolta di dati](#skill-search-add-create-discovery-collection).

## Crea una raccolta di dati
{: #skill-search-add-create-discovery-collection}

Se hai un piano Lite del servizio Discovery, ti viene data l'opportunità di eseguire l'upgrade del tuo piano. Se non vuoi eseguire l'upgrade in questo momento, fai clic su **Let's get started**.

1.  Per creare una raccolta {{site.data.keyword.discoveryshort}}, effettua una delle seguenti azioni:

      - Per creare una raccolta da dati archiviati in un tipo di origine dati per cui {{site.data.keyword.discoveryshort}} fornisce il supporto integrato, seleziona un tipo di origine dati.

        1.  Fornisci le informazioni richieste per l'origine dati che scegli e quindi fai clic su **Connect**.

            Per ulteriori dettagli, vedi [Connessione alle origini dati ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/discovery?topic=discovery-sources){: new_window}.
        1.  Indica la frequenza con cui vuoi che i dati dall'origine dati vengano sincronizzati con la raccolta che stai creando in {{site.data.keyword.discoveryshort}}.
        1.  Specifica le informazioni che vuoi estrarre dall'origine dati e includile nella tua raccolta {{site.data.keyword.discoveryshort}}.

            Le opzioni che vengono visualizzate differiscono a seconda del tipo di origine dati.

            - Per un'origine dati Salesforce, seleziona i tipi di oggetto che vuoi estrarre dai documenti di origine. Puoi selezionare un [Tipo di oggetto caso ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_case.htm#!) che rappresenta ad esempio un *caso*, che è un problema o un errore del cliente.
            - Per un'origine dati Sharepoint, specifica i percorsi.
            - Per i repository di file, specifica le directory o i file.
            - Per un'origine dati di ricerca per l'indicizzazione, specifica l'URL di base di un sito web di cui vuoi eseguire la ricerca per l'indicizzazione. Della pagina web che specifichi e di tutte le pagine a cui è collegata viene eseguita la ricerca per l'indicizzazione e viene creato un documento per la pagina web.

            Dai a Watson alcuni minuti per consentirgli di iniziare a creare i documenti. Non appena l'origine inizia ad essere inserita, il numero di documenti visualizzati nella pagina dei dettagli {{site.data.keyword.discoveryshort}} aumenta. Potresti dover aggiornare la pagina. 
            
            Per ottenere aiuto con la creazione delle origini dati, vedi [Risoluzione dei problemi](#skill-search-add-troubleshoot).

        1.  Fai clic su **Save and sync objects**.

            Viene creata la raccolta di dati. Dopo il completamento del processo, viene visualizzata una pagina di riepilogo in {{site.data.keyword.discoveryshort}}, la quale è ospitata in una scheda del browser web separata.

      - Per creare una raccolta caricando dei documenti, fai clic su **Upload documents**.

        1.  Prima definisci la raccolta e poi carica i documenti. Fornisci le seguenti informazioni:

            - Nome della raccolta. Il nome deve essere univoco per questa istanza del servizio.
            - Lingua. Seleziona la lingua dei file che aggiungi a questa raccolta. Per informazioni sulle lingue supportate da {{site.data.keyword.discoveryshort}}, vedi [Supporto linguistico ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/discovery?topic=discovery-language-support){: new_window}.

              Se stai caricando un documento PDF e vuoi estrarre le informazioni su parte, natura e categoria da esso, espandi la sezione **Advanced** e fai clic su **Use the Default Contract Configuration with this collection**. Per ulteriori dettagli, vedi [Requisiti della raccolta ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/discovery?topic=discovery-element-classification#element-collection){: new_window}.
        1.  Carica i documenti.

            I tipi di file supportati includono file PDF, HTML, JSON e DOC. Per ulteriori dettagli, vedi [Aggiunta di contenuto ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/discovery?topic=discovery-addcontent){: new_window}.
            {: note}

            Non è disponibile una sincronizzazione continua dei documenti caricati. Se vuoi selezionare le modifiche apportate a un documento, carica una versione successiva del documento.

Attendi che la raccolta sia stata completamente inserita prima di tornare a {{site.data.keyword.conversationshort}}.

### Esempio di creazione di raccolta dati
{: #skill-search-add-json-collection-example}

Ad esempio, potresti avere un file JSON simile a questo:

```bash
{
  "Title": "About",
  "Shortdesc": "IBM Watson Assistant is a cognitive bot that you can customize for your business needs, and deploy across multiple channels to bring help to your customers where and when they need it.",
  "Topics": "overview",
  "url": "https://cloud.ibm.com/docs/services/assistant?topic=assistant-index"
}
```
{: codeblock}

Se carichi un file JSON che contiene valori di nomi ripetuti, solo la prima ricorrenza della coppia di nome e valore viene indicizzata e restituita dalla ricerca. Suddividi il file in più file JSON e carica la serie.
{: tip}

## Configura la ricerca
{: #skill-search-add-configure}

1.  Dall'istanza di {{site.data.keyword.discoveryshort}}, fai clic su **Finish setup in Watson Assistant**.

1.  Sulla pagina della capacità di ricerca {{site.data.keyword.conversationshort}}, fai clic su **Configure**.

1.  Scegli i campi della raccolta {{site.data.keyword.discoveryshort}} da cui vuoi estrarre il testo da includere nel risultato della ricerca restituito all'utente.

    I campi disponibili differiscono in base ai dati che hai inserito.

    Ogni risultato della ricerca può essere formato dalle seguenti sezioni:

    - **Titolo**: il titolo del risultato della ricerca. Utilizza il titolo, il nome o un tipo di campo simile dalla raccolta come titolo del risultato della ricerca.

      Devi selezionare qualcosa per il titolo o non viene visualizzata alcuna risposta di risultato della ricerca nelle integrazioni Facebook e Slack.
    - **Corpo**: la descrizione del risultato della ricerca. Utilizza un campo astratto, di riepilogo o di evidenziazione dalla raccolta come corpo del risultato della ricerca.

       Devi selezionare qualcosa per il corpo o non viene visualizzata alcuna risposta di risultato della ricerca nelle integrazioni Facebook e Slack.
    - **URL**: questo campo può essere popolato con qualsiasi contenuto di piè di pagina che vuoi includere alla fine del risultato della ricerca.

       Ad esempio, potresti voler includere un collegamento ipertestuale all'oggetto dati originale nella sua origine dati nativa. La maggior parte delle origini dati fornite forniscono URL pubblici autoreferenziali per gli oggetti nell'archivio per supportare l'accesso diretto. Se aggiungi un URL, deve essere valido e raggiungibile. Se non lo fai, l'integrazione Slack non includerà l'URL nelle sue risposte e l'integrazione Facebook non restituirà alcuna risposta.

       Le integrazioni Facebook e Slack possono correttamente visualizzare la risposta del risultato della ricerca quando il campo URL è vuoto.
  
    Devi scegliere un valore per almeno una delle sezioni del risultato della ricerca.
    {: important}

    Per assistenza, vedi [Suggerimenti per la selezione del campo di raccolta](#skill-search-add-field-tips).

    Se non è disponibile alcuna opzione dai campi a discesa, dai più tempo a {{site.data.keyword.discoveryshort}} per terminare la creazione della raccolta. Dopo l'attesa, se la raccolta non viene creata, potrebbe non contenere alcun documento o contenere degli errori di inserimento che devono essere prima risolti.

    Per continuare con l'[esempio del file JSON caricato](#skill-search-add-json-collection-example), una buona associazione è di utilizzare i campi *Title*, *Shortdesc* e *url*.

    ![Mostra come i campi Title, Shortdesc e url sono selezionati e come la scheda di ricerca di anteprima viene popolata con le informazioni da questi campi](images/search-skill-configure-fields.png)

    Quando aggiungi le associazioni del capo, viene visualizzata un'anteprima del risultato della ricerca con le informazioni dai campi corrispondenti della tua raccolta dati. Questa anteprima mostra cosa viene incluso nella risposta del risultato della ricerca restituita agli utenti.

    Per ottenere aiuto con la configurazione della ricerca, vedi [Risoluzione dei problemi](#skill-search-add-troubleshoot).

1.  Prepara messaggi diversi da condividere con gli utenti a seconda se la ricerca è stata eseguita correttamente.

    <table>
    <caption>Messaggi del risultato della ricerca</caption>
    <tr>
      <th>Nome campo</th>
      <th>Scenario</th>
      <th>Messaggio di esempio</th>
    </tr>
    <tr>
      <td>Messaggio</td>
      <td>Vengono restituiti i risultati della ricerca</td>
      <td>I found this information that might be helpful: </td>
    </tr>
    <tr>
      <td>Nessun risultato trovato</td>
      <td>Non è stato trovato alcun risultato</td>
      <td>I searched my knowledge base for information that might address your query, but did not find anything useful to share.</td>
    </tr>
    <tr>
      <td>Messaggio di errore</td>
      <td>Impossibile completare la ricerca per qualche motivo</td>
      <td>I might have information that could help address your query, but am unable to search my knowledge base at the moment.</td>
    </tr>
    </table>

1.  Fai clic su **Try it** per aprire il riquadro "Try it out" per il test. Immetti un messaggio di testo per vedere i risultati che vengono restituiti quando le tue scelte di configurazione vengono applicate alla ricerca. Apporta le modifiche secondo necessità.

1.  Fai clic su **Create**.

Se vuoi modificare la configurazione della scheda del risultato della ricerca in un secondo momento, apri nuovamente la capacità di ricerca e apporta le modifiche. Non devi salvare le modifiche mentre le apporti; vengono applicate immediatamente. Quando sei soddisfatto dei risultati della ricerca, fai clic su **Save** per terminare la configurazione della capacità di ricerca.

Se decidi di volerti connettere a una raccolta dati o a un'istanza del servizio {{site.data.keyword.discoveryshort}} differente, crea una nuova capacità di ricerca e configurala per connettersi all'altra istanza. **Non puoi** modificare i dettagli dell'istanza del servizio o della raccolta dati di una capacità di ricerca dopo averla creata.
{: important}

### Suggerimenti per la selezione del campo di raccolta
{: #skill-search-add-field-tips}

I campi della raccolta appropriati da cui estrarre i dati variano a seconda dell'origine dati della tua raccolta e in base a come è stata arricchita. Dopo aver scelto un tipo di raccolta dati, i valori dei campi della raccolta vengono prepopolati con i campi dell'origine che vengono considerati quelli che più probabilmente contengono delle informazioni utili per il tipo di origine dati della raccolta fornita. Tuttavia, conosci i tuoi dati meglio di chiunque altro. Puoi modificare i campi dell'origine con quelli che contengono le informazioni migliori per soddisfare le tue esigenze.

Per ulteriori informazioni sulla struttura dei documenti nella tua raccolta, includi i nomi dei campi che contengono le informazioni che potresti voler estrarre, apri la raccolta in {{site.data.keyword.discoveryshort}} e fai quindi clic sull'icona View data schema![icona View data schema](images/icon-view-data-schema.png).

I campi dell'origine vengono creati quando viene creata la raccolta. Per ulteriori informazioni sui campi che vengono generati per tuo conto, ad esempio `enriched_text.concepts.text`, vedi [Configurazione del tuo servizio > Aggiunta degli arricchimenti ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/discovery?topic=discovery-configservice#adding-enrichments){: new_window}.

## Risoluzione dei problemi
{: #skill-search-add-troubleshoot}

Controlla queste informazioni per assistenza nell'esecuzione di attività comuni.

- **Creazione di una raccolta dati di ricerca per l'indicizzazione web**: cose da sapere quando crei una origine dati di ricerca per l'indicizzazione web:

    - Per un piano Lite di {{site.data.keyword.discoveryshort}}, non puoi creare più di 1.000 documenti. 
    - Per aumentare il numero di documenti disponibili per la raccolta dati, fai clic per aggiungere un gruppo di URL in cui puoi elencare gli URL per le pagine che vuoi ricercare per l'indicizzazione ma che non sono collegate all'URL seed iniziale.
    - Per diminuire il numero di documenti disponibili per la raccolta dati, specifica un dominio secondario dell'URL di base. Oppure, nelle impostazioni della ricerca per l'indicizzazione web, limita il numero di hop che Watson può effettuare dalla pagina originale. Puoi specificare dei domini secondari da escludere esplicitamente anche dalla ricerca per l'indicizzazione.
    - Se non viene elencato alcun documento entro pochi minuti e un aggiornamento della pagina, assicurati che il contenuto che vuoi inserire sia disponibile dall'origine della pagina dell'URL. Alcuni contenuti delle pagine web vengono generati dinamicamente e pertanto non ne può essere eseguita la ricerca per l'indicizzazione.

- **Configurazione dei risultati della ricerca per i documenti caricati**: se stai utilizzando una raccolta di documenti caricati e non puoi ottenere i risultati della ricerca corretti o i risultati non sono abbastanza concisi, prendi in considerazione l'utilizzo di *Smart Document Understanding* quando crei la raccolta dati. 

  Questa funzione ti consente di annotare i documenti in base alla formattazione del testo. Ad esempio, puoi istruire {{site.data.keyword.discoveryshort}} che tutto il testo con un font in grassetto di dimensione 28 è un titolo di documento. Se applichi queste informazioni alla raccolta quando la inserisci, puoi successivamente utilizzare il campo *title* come origine per la sezione del titolo del tuo risultato della ricerca. 
  
  Puoi anche utilizzare Smart Document Understanding per suddividere dei grandi documenti in segmenti in cui è più semplice eseguire la ricerca. Per ulteriori informazioni, vedi l'argomento [Smart Document Understanding ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/services/discovery?topic=discovery-sdu) nella documentazione {{site.data.keyword.discoveryshort}}.

- **Miglioramento dei risultati della ricerca**: se non ti piacciono i risultati che stai visualizzando, controlla queste informazioni per assistenza.

  - Richiama la capacità di ricerca da un nodo di dialogo e specifica i dettagli del filtro. 

    Da una risposta della capacità di ricerca del nodo di dialogo, puoi specificare un filtro di sintassi di query completo {{site.data.keyword.discoveryshort}} per restringere i risultati. 
    
    Ad esempio, puoi definire un filtro che esclude tutti i documenti nella raccolta dati che non menzionano un intento nel titolo del documento o alcuni altri campi di metadati. Oppure il filtro può escludere i documenti che non identificano un'entità come nota nei metadati della raccolta dati o che non menzionano l'entità da nessuna parte in tutto il testo del documento. Per i dettagli su come aggiungere un tipo di risposta della capacità di ricerca, vedi [Aggiunta di risposte esaurienti](https://cloud.ibm.com/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia-add).

    Per ulteriori suggerimenti sul miglioramento dei risultati, leggi il post di blog [Improve your natural language query results from Watson Discovery ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/blogs/improving-your-natural-language-query-results-from-watson-discovery/).

## Passi successivi
{: #skill-search-add-next-steps}

Una volta creata la capacità, comparirà come un tile nella pagina Skills.

La capacità di ricerca non può interagire con i clienti fino a quando non viene aggiunta a un assistente e quest'ultimo viene distribuito. Vedi [Creazione di assistenti](/docs/services/assistant?topic=assistant-assistant-add).

### Aggiunta della capacità a un assistente
{: #skill-search-add-to-assistant}

Puoi aggiungere una capacità a un assistente. Apri il tile dell'assistente e aggiungi la capacità all'assistente da qui. Non puoi scegliere l'assistente che utilizzerà la capacità dall'interno della pagina di configurazione della capacità. 

Una capacità di ricerca può essere utilizzata da più di un assistente.

1.  Dalla scheda Assistants, fai clic per aprire il tile per l'assistente a cui desideri aggiungere la capacità.

1.  Fai clic su **Add Search Skill**.

1.  Fai clic su **Add existing skill**.

    Fai clic sulla capacità che desideri aggiungere dalle capacità disponibili visualizzate.

Se aggiungi una capacità di ricerca a un assistente, viene automaticamente abilitata per l'assistente nel seguente modo:

- Se l'assistente ha solo una capacità di ricerca, tutto l'input utente inviato a uno dei canali di integrazione dell'assistente attiva la capacità di ricerca.

- Se l'assistente ha sia una capacità di dialogo che di ricerca, tutto l'input utente attiva prima la capacità di dialogo. Il dialogo risponde a tutto l'input utente con cui ha affidabilità elevata e a cui può rispondere correttamente. Tutte le domande che normalmente attivano il nodo `anything_else` nella struttura ad albero di dialogo vengono inoltrate invece alla capacità di ricerca.

  Puoi evitare che la ricerca venga attivata dal nodo `anything_else` seguendo la procedura in [Disabilitazione della ricerca](#search-skill-add-disable).
  {: note}

- Se vuoi che un query di ricerca specifica venga attivata per domande specifiche, aggiungi un tipo di risposta della capacità di ricerca al nodo di dialogo appropriato. Per ulteriori dettagli, vedi [Risposte](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia).

## Trigger della ricerca
{: #skill-search-add-trigger}

La capacità di ricerca viene attivata nei seguenti modi:

- **Nodi Altro**: effettua una ricerca in un'origine dati esterna per una domanda pertinente quando nessuno dei nodi di dialogo può far fronte alla query dell'utente.

  Invece di mostrare un messaggio standard, come ad esempio `I don't know how to help you with that.` l'assistente può rispondere, `Maybe this information can help:` seguito dal passaggio restituito dalla ricerca. Se una capacità di ricerca viene collegata al tuo assistente, ogni volta che viene attivato il nodo `anything_else`, invece di visualizzare la risposta del nodo, viene effettuata una ricerca. L'assistente passa l'input utente come ad esempio la query alla tua capacità di ricerca e restituisce i risultati della ricerca come risposta.

  Puoi evitare che la ricerca venga attivata dal nodo `anything_else` seguendo la procedura in [Disabilitazione della ricerca](#search-skill-add-disable).
  {: note}

- **Tipo di risposta della ricerca**: se aggiungi un tipo di risposta della ricerca a un nodo di dialogo, il tuo assistente richiama un passaggio da un'origine dati esterna e lo restituisce come risposta a una determinata domanda. Questo tipo di ricerca si verifica solo quando viene elaborato il singolo nodo di dialogo.

  Questo approccio è utile se desideri restringere una query utente prima di attivare una ricerca. Ad esempio, il ramo di dialogo potrebbe raccogliere le informazioni sul tipo di dispositivo che il cliente vuole comprare. Quando conosci la marca e il modello, puoi inviare una parola chiave model nella query inoltrata alla capacità di ricerca e ottenere dei risultati migliori.
- **Solo capacità di ricerca**: se viene collegata solo una capacità di ricerca a un assistente e nessuna capacità di dialogo è collegata all'assistente, viene inoltrata una query di ricerca al servizio {{site.data.keyword.discoveryshort}} quando viene ricevuto un input utente da uno dei canali di integrazione dell'assistente.

## Verifica la capacità di ricerca
{: #search-skill-add-test}

Dopo aver configurato la ricerca, puoi inviare delle domande di verifica per vedere i risultati della ricerca che vengono restituiti da {{site.data.keyword.discoveryshort}} utilizzando il riquadro "Try it out" della capacità di ricerca.

Per verificare l'esperienza completa dei clienti quando pongono delle domande a cui risponde il dialogo o che attivano una ricerca, utilizza un'integrazione del canale, ad esempio un Preview link.

Non puoi verificare l'esperienza utente end-to-end completa dal riquadro "Try it out" del dialogo. La capacità di ricerca viene configurata separatamente e collegata a un assistente. La capacità di dialogo non ha modo di conoscere i dettagli della ricerca e pertanto non può mostrare i risultati della ricerca nel proprio riquadro "Try it out".
{: important}

Configura almeno un canale di integrazione per verificare la capacità di ricerca. Nel canale, immetti le query che attivano la ricerca. Se avvii un qualsiasi tipo di ricerca dal tuo dialogo, verifica il dialogo per assicurarti che la ricerca sia stata attivata come previsto. Se non stai utilizzando i tipi di risposta della ricerca, verifica che una ricerca venga attivata solo quando nessun nodo di dialogo esistente può soddisfare l'input utente. E ogni volta che viene attivata una ricerca, assicurati che restituisca dei risultati significativi.

## Invio di ulteriori richieste alla capacità di ricerca
{: #search-skill-add-increase-flow}

Se vuoi che la capacità di dialogo risponda meno spesso e che invece invii più query alla capacità di ricerca, puoi configurare il dialogo in modo da farlo.

Devi aggiungere una capacità di dialogo e una capacità di ricerca al tuo assistente in modo che questo approccio funzioni.

Segui questa procedura per rendere meno probabile che il dialogo risponda reimpostando la soglia del livello di affidabilità dal valore predefinito di 0.2 con 0.5. La modifica della soglia del livello di affidabilità con 0.5 indica al tuo assistente di non rispondere con una domanda dal dialogo a meno che non sia sicuro a più del 50% che il dialogo possa comprendere l'intento dell'utente e possa rispondergli.

1.  Dalla pagina *Dialog* della tua capacità di dialogo, assicurati che l'ultimo nodo nella struttura ad albero di dialogo abbia una condizione `anything_else`.

    Se questo nodo viene elaborato. La capacità di ricerca viene attivata.

1.  Aggiungi una cartella al dialogo. Posiziona la cartella sopra il primo nodo di dialogo che vuoi de-enfatizzare. Aggiungi la seguente condizione alla cartella:

    `intents[0].confidence > 0.5`

    Questa condizione viene applicata a tutti i nodi nella cartella. La condizione indica al tuo assistente di elaborare i nodi solo se è sicuro almeno al 50% di conoscere l'intento dell'utente.

1.  Sposta tutti i nodi di dialogo che non vuoi il tuo assistente elabori spesso nella cartella.

Dopo aver modificato il dialogo, verifica l'assistente per assicurarti che la capacità di ricerca venga attivata quanto spesso tu voglia lo sia.

Un approccio alternativo è di istruire il dialogo sugli argomenti da ignorare. Per farlo, puoi aggiungere delle espressioni che vuoi l'assistente invii immediatamente alla capacità di ricerca come espressioni di test nel riquadro "Try it out" della capacità di dialogo. Puoi quindi selezionare l'opzione **Mark as irrevlant** nel riquadro "Try it out" per indicare al dialogo di non rispondere a queste espressioni o ad altre espressioni simili. Per ulteriori informazioni, vedi [Istruisci il tuo assistente sugli argomenti da ignorare](/docs/services/assistant?topic=assistant-logs#logs-mark-irrelevant).

## Disabilitazione della ricerca
{: #search-skill-add-disable}

Puoi disabilitare la capacità di ricerca in modo che non venga attivata.

Potresti volerlo fare temporaneamente, mentre stai configurando l'integrazione. Oppure potresti voler attivare solo una ricerca per domande dell'utente specifiche che puoi identificare all'interno del dialogo e utilizzare un tipo di risposta della capacità di ricerca per rispondere.

Per evitare che la capacità di ricerca venga attivata, completa la seguente procedura:

1.  Dalla pagina **Assistants**, fai clic sul menu del tuo assistente e scegli **Settings**.
1.  Apri la pagina *Search Skill* e fai quindi clic per spostare l'interruttore su **Disabled**.
