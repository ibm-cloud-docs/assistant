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

# Configurazione di uno spazio di lavoro {{site.data.keyword.conversationshort}}

L'elaborazione del linguaggio naturale per il servizio {{site.data.keyword.conversationshort}} avviene all'interno di uno *spazio di lavoro*, che è un contenitore di tutte le risorse che definiscono il flusso di conversazione per un'applicazione.
{: shortdesc}

Una singola istanza del servizio {{site.data.keyword.conversationshort}} può contenere più spazi di lavoro. Uno spazio di lavoro contiene i seguenti tipi di risorse:

- [**Intenti**](intents.html): un *intento* rappresenta lo scopo dell'input di un utente, come una domanda relativa alle sedi aziendali o al pagamento di una fattura. Puoi definire un intento per ogni tipo di richiesta utente che desideri sia supportata dalla tua applicazione. Nello strumento, il nome di un intento ha sempre come prefisso il carattere `#`. Per addestrare lo spazio di lavoro a riconoscere i tuoi intenti, fornisci molti esempi di input utente e indica a quali intenti sono associati.
- [**Entità**](entities.html): un'*entità* rappresenta un termine o oggetto che è rilevante per i tuoi intenti e che fornisce un contesto specifico per un intento. Ad esempio, un'entità potrebbe rappresentare una città in cui l'utente vuole trovare una sede aziendale o l'importo di pagamento di una fattura. Nello strumento, il nome di un'entità ha sempre come prefisso il carattere `@`. Per addestrare lo spazio di lavoro a riconoscere le tue entità, elenca i valori possibili per ogni entità e i sinonimi che gli utenti potrebbero immettere.
- [**Dialogo**](dialog-build.html): un *dialogo* è un flusso di conversazione ramificato che definisce come l'applicazione risponde quando riconosce gli intenti e le entità che hai definito. Utilizza il builder di dialoghi nello strumento per creare conversazioni con gli utenti, fornendo risposte basate sugli intenti e sulle entità che riconosci nel loro input.

Man mano che aggiungi le informazioni il sistema si addestra da solo, pertanto non dovrai fare nulla per iniziare l'addestramento.

## Limiti di spazi di lavoro
{: #workspace-limits}

Il numero di spazi di lavoro che puoi creare in una singola istanza del servizio dipende dal tuo piano di servizio {{site.data.keyword.conversationshort}}. Lo spazio di lavoro di esempio non viene conteggiato nel limite di spazi a meno che non modifichi o duplichi l'esempio:

| Piano di servizio     | Spazi di lavoro per istanza del servizio |
|------------------|--------------------------------:|
| Standard/Premium |                              20 |
| Lite*            |                               5 |

*Dopo 30 giorni di inattività, uno spazio di lavoro Lite non utilizzato potrebbe essere eliminato per liberare spazio.

## Creazione di spazi di lavoro
{: #creating-workspaces}

Puoi creare uno spazio di lavoro da zero, utilizzare quello di esempio fornito o importarne uno da un file JSON. Puoi anche duplicare uno spazio di lavoro esistente all'interno della stessa istanza del servizio.

Utilizza lo strumento {{site.data.keyword.conversationshort}} per creare gli spazi di lavoro. Per creare uno spazio di lavoro, segui questa procedura:

1.  Se la pagina "Dettagli del servizio" non è già aperta, fai clic sulla tua istanza del servizio {{site.data.keyword.conversationshort}} nella console. (Quando crei un'istanza del servizio, viene visualizzata la pagina "Dettagli del servizio.)

1.  Fai clic su **Avvia strumento**.

1.  Effettua una delle operazioni riportate di seguito:
    - Per creare uno spazio di lavoro da zero, fai clic su **Crea**.
    - Per utilizzare l'esempio come punto di partenza per il tuo spazio di lavoro, modifica lo spazio di lavoro di esempio. Se vuoi utilizzarlo per più spazi di lavoro, dovrai invece duplicarlo.
    - Per importare uno spazio di lavoro da un file JSON, fai clic sull'icona ![Importa spazio di lavoro](images/workspace_import.png) e seleziona il file JSON da cui vuoi importarlo.

        **Importante:**

        - Il file JSON importato deve utilizzare la codifica UTF-8.
        - La dimensione massima per un file JSON dello spazio di lavoro è di 10 MB. Se devi importare uno spazio di lavoro più grande, si consiglia di importare gli intenti e le entità separatamente dopo aver importato lo spazio di lavoro. (Puoi anche importare spazi di lavoro più grandi utilizzando l'API REST. Per ulteriori informazioni, vedi il [Riferimento API![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#create_workspace){: new_window}.)
        - Il JSON non può contenere caratteri di tabulazione, nuova riga o ritorno a capo.

        Specifica i dati da includere:

        - Seleziona **Tutto (intenti, entità e dialogo)** se vuoi importare una copia completa dello spazio di lavoro esportato, incluso il dialogo.
        - Seleziona **Intenti ed entità** se vuoi utilizzare gli intenti e le entità dello spazio di lavoro esportato, ma intendi creare un nuovo dialogo.

1.  Specifica i dettagli per il nuovo spazio di lavoro:
    - **Nome**: un nome che non superi i 64 caratteri. Questo valore è obbligatorio.
    - **Descrizione**: una descrizione che non superi i 128 caratteri.
    - **Lingua**: la lingua dell'input utente che lo spazio di lavoro imparerà a comprendere.

Dopo aver creato lo spazio di lavoro, viene visualizzato sotto forma di tile nella pagina Spazi di lavoro.

## Esportazione e copia degli spazi di lavoro
{: #exporting-and-copying-workspaces}

Puoi utilizzare la pagina Spazi di lavoro per esportare e copiare spazi di lavoro in un nuovo spazio di lavoro.

L'esportazione di uno spazio di lavoro salva una copia di tutti i suoi dati in un file JSON. Utilizza questa opzione se vuoi importare lo spazio di lavoro in una istanza del servizio diversa o salvare una copia dei dati dello spazio di lavoro non in linea. Quando importi uno spazio di lavoro, puoi scegliere di importare solo gli intenti e le entità, il che può essere utile se vuoi creare un nuovo dialogo utilizzando gli stessi dati di addestramento.

La copia di uno spazio di lavoro crea una copia completa dello spazio di lavoro all'interno della stessa istanza del servizio. Utilizza questa opzione se vuoi adattare uno spazio di lavoro esistente conservando la versione originale.

- Per esportare uno spazio di lavoro esistente in un file JSON, fai clic sull'icona di menu nel tile dello spazio di lavoro e seleziona quindi **Scarica come JSON**.
- Per creare una copia di uno spazio di lavoro esistente all'interno della stessa istanza del servizio, fai clic sull'icona di menu nel tile dello spazio di lavoro e seleziona quindi **Duplica**.

    Specifica il nome, la descrizione e la lingua per il nuovo spazio di lavoro. Nella copia vengono mantenuti tutti i dati (che includono intenti, entità e dialogo).

## Condivisione dello spazio di lavoro con i membri del team
{: #invite-others}

Dopo aver creato l'istanza del servizio, puoi concedere ad altre persone l'accesso ad essa. Contemporaneamente, puoi definire i dati di addestramento e creare il dialogo. 

**Importante**: solo una persona alla volta può modificare un intento, un'entità o un nodo di dialogo. Se più persone lavorano contemporaneamente sullo stesso elemento, le modifiche apportate dall'ultima persona che ha effettuato il salvataggio saranno le uniche modifiche che verranno applicate. Le modifiche apportate nello stesso arco di tempo da un'altra persona e salvate prima dell'ultimo salvataggio non verranno conservate. Coordina gli aggiornamenti che intendi apportare con i membri del tuo team in modo che nessuno perda il proprio lavoro. 

Per condividere uno spazio di lavoro con altre persone, devi fornire loro l'accesso degli sviluppatori all'istanza del servizio che ospita lo spazio di lavoro. Se l'istanza condivisa ospita altri spazi di lavoro che non vuoi che vengano modificati da altri, comunicalo esplicitamente alle persone invitate.

1.  Vai alla pagina {{site.data.keyword.watson}} Developer Console [Projects ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.{DomainName}/developer/watson/projects) ed esegui l'accesso. Fai clic su **Gestisci > Account > Utenti** dal menu.
1.  Fai clic su **Invita utenti** e quindi immetti gli indirizzi email delle persone del tuo team a cui desideri concedere l'accesso.
1.  Nella sezione *Accesso Cloud Foundry*, scegli la tua organizzazione dell'elenco *Organizzazione*.

    Il campo *Ruoli organizzazione* viene compilato automaticamente con *Revisore*. Puoi mantenere il valore predefinito presente nel campo.
1.  Facoltativamente, limita l'accesso che stai concedendo agli spazi di lavoro in una singola regione e in un singolo spazio. 
1.  Nel campo *Ruoli spazio*, scegli **Sviluppatore**.

    Per ulteriori informazioni sui ruoli, vedi [Ruoli Cloud Foundry ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://console.bluemix.net/docs/iam/cfaccess.html#cfroles).
1.  Fai clic su **Invita utenti**.

Quando le persone che hai invitato eseguono poi l'accesso a {{site.data.keyword.cloud_notm}}, la tua organizzazione verrà inclusa nei loro elenchi di account.

Quando più persone contribuiscono allo sviluppo dello spazio di lavoro, possono verificarsi modifiche involontarie, incluse eliminazioni dello spazio di lavoro. Prendi in considerazione di creare regolarmente copie di backup del tuo spazio di lavoro in modo che, se necessario, tu possa eseguire il ripristino ad una versione precedente. Per creare un backup, esporta semplicemente lo spazio di lavoro come un file JSON.
