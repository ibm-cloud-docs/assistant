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

# Creazione di una capacità 
{: #skill-add}

L'elaborazione del linguaggio naturale per il servizio {{site.data.keyword.conversationshort}} viene definita in una *capacità di dialogo* che costituisce un contenitore per tutte le risorse che definiscono un flusso di conversazione.
{: shortdesc}

Puoi aggiungere una capacità a un assistente. 

Puoi creare una capacità di dialogo da zero, utilizzare una capacità di esempio fornita da IBM oppure importare una capacità da un file JSON. Per aggiungere una capacità, completa la seguente procedura: 

1.  Se non hai creato un'istanza del servizio {{site.data.keyword.conversationshort}}, esegui prima questa procedura una tantum. Altrimenti, ignora questo passo.

    1.  Vai alla pagina [{{site.data.keyword.conversationshort}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/catalog/services/watson-assistant) nel catalogo {{site.data.keyword.cloud_notm}}.

    1.  Registrati per un account {{site.data.keyword.cloud_notm}} o accedi.

        L'istanza del servizio sarà creata nel gruppo di risorse **predefinito** se non ne scegli un altro e *non può* essere modificato successivamente. Il predefinito (*Default*) di norma è sufficiente. Ma se desideri ulteriori informazioni sui gruppi di servizio, vedi la [documentazione {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/docs/resources?topic=resources-bp_resourcegroups#bp_resourcegroups){: new_window}.

        Puoi utilizzare una sola istanza del servizio per lo sviluppo, il test e la distribuzione di un assistente, per cui non creare gruppi di risorse distinti per diversi tipi di ambienti di distribuzione.
        {:tip}

        Con un piano Premium, puoi creare più istanze. Le istanze devono essere tutte create nello stesso gruppo di risorse.

    1.  Fai clic su **Create**.

    1.  Fai clic su **Launch tool**. Se ti viene richiesto di accedere allo strumento, fornisci le tue credenziali {{site.data.keyword.cloud_notm}}.

1.  Fai clic sulla scheda **Skills**. 

    Se hai creato o ti è stato concesso il ruolo di sviluppatore per accedere agli spazi di lavoro che sono stati creati con la versione generalmente disponibile del servizio {{site.data.keyword.conversationshort}} (in precedenza noto come Watson Conversation), li vedrai qui elencati come capacità di dialogo.
    {: note}

1.  Fai clic su **Create new**.

1.  Effettua una delle operazioni riportate di seguito:

    - Per creare una capacità da zero, fai clic su **Create skill**.
    - Per aggiungere una capacità di esempio fornita con il servizio come punto di partenza per la tua capacità o come un esempio da esplorare prima di crearne una tua, fai clic su **Use sample skill** e poi fai clic sull'esempio che desideri utilizzare. 

      La capacità di esempio viene aggiunta al tuo elenco di capacità. Non viene associata a nessun assistente. Ignora i passi rimanenti presenti in questa procedura. 

    - Per aggiungere una capacità esistente a questa istanza del servizio, puoi importarla come un file JSON. Fai clic su **Import skill** e poi fai clic su **Choose JSON File** e seleziona il file JSON che desideri importare. 

      **Importante:**

      - Il file JSON importato deve utilizzare la codifica UTF-8, senza codifica BOM (byte order mark). 
      - La dimensione massima per un file JSON della capacità è 10MB. Se devi importare una capacità più grande, ti consigliamo di importare gli intenti e le entità separatamente dopo aver importato la capacità. (Puoi anche importare capacità più grandi utilizzando l'API REST. Per ulteriori informazioni, vedi il [Riferimento API![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/apidocs/assistant?curl=#create-workspace){: new_window}.)
      - Il JSON non può contenere caratteri di tabulazione, nuova riga o ritorno a capo.

      Specifica i dati da includere:

        - Seleziona **Everything (Intents, Entities, and Dialog)** se desideri importare una copia completa della capacità esportata, incluso il dialogo. 
        - Seleziona **Intents and Entities** se desideri utilizzare gli intenti e le entità provenienti dalla capacità esportata, ma intendi creare un nuovo dialogo. 

      Fai clic su **Import**.

      Se hai problemi nell'importazione di una capacità, vedi [Risoluzione dei problemi di importazione della capacità](#skill-add-import-errors).

1.  Specifica i dettagli per la capacità: 

    - **Name**: un nome lungo massimo 100 caratteri. Un nome è obbligatorio.
    - **Description**: una descrizione facoltativa lunga massimo 200 caratteri. 
    - **Language**: la lingua dell'input utente che la capacità imparerà a comprendere. Il valore predefinito è Inglese.

Una volta creata la capacità, comparirà come un tile nella pagina Skills. Ora, puoi iniziare ad identificare gli obiettivi utente che desideri vengano soddisfatti dalla capacità di dialogo. 

- Per aggiungere intenti precostruiti alla tua capacità, vedi [Utilizzo dei cataloghi di contenuto](/docs/services/assistant?topic=assistant-catalog). 
- Per definire i tuoi intenti, vedi [Definizione di intenti](/docs/services/assistant?topic=assistant-intents).

La capacità di dialogo non può interagire con i clienti fino a quando non viene aggiunta a un assistente e quest'ultimo viene distribuito. Vedi [Creazione di un assistente](/docs/services/assistant?topic=assistant-assistant-add).

### Risoluzione dei problemi di importazione della capacità 
{: #skill-add-import-errors}

Se ricevi un messaggio indicante che la capacità contiene risorse che superano i limiti imposti dal tuo piano di servizio, completa i seguenti passi per importare correttamente la capacità: 

1.  Acquista un piano con limiti di risorse più elevati. 
1.  Crea un'istanza del servizio nel nuovo piano. 
1.  Importa la capacità nella nuova istanza del servizio. 
1.  Apporta le modifiche alla capacità in modo che soddisfi i requisiti dei limiti della risorsa per il piano che desideri utilizzare per andare avanti. Potresti dover ridurre il numero di nodi di dialogo utilizzati nella struttura ad albero del dialogo, ad esempio. 
1.  Esporta la capacità modificata scaricandola. 
1.  Riprova ad importare la capacità modificare nell'istanza del servizio originale nel piano che desideri. 

### Aggiunta della capacità a un assistente 
{: #skill-add-to-assistant}

Puoi aggiungere una capacità a un assistente. Devi aprire il tile dell'assistente e aggiungere la capacità all'assistente dalla pagina di configurazione dell'assistente; non puoi scegliere l'assistente che utilizzerà la capacità dalla pagina di configurazione della capacità. Una capacità di dialogo può essere utilizzata da più di un assistente. 

1.  Dalla scheda Assistants, fai clic per aprire il tile per l'assistente a cui desideri aggiungere la capacità. 

1.  Fai clic su **Add Dialog Skill**.

1.  Fai clic su **Add existing skill**.

    Fai clic sulla capacità che desideri aggiungere dalle capacità disponibili visualizzate. 

    Quando aggiungi una capacità da qui, otterrai la versione di sviluppo. Se desideri aggiungere una specifica versione di capacità, aggiungila invece dalla scheda *Version History* della capacità. 

## Limiti della capacità 
{: #skill-add-limits}

Il numero di capacità che puoi creare in una singola istanza del servizio dipende dal tuo piano di servizio {{site.data.keyword.conversationshort}}. Ogni capacità di dialogo di esempio disponibile nella tua istanza del servizio non viene conteggiata nel tuo limite a meno che la modifichi o duplichi.

| Piano di servizio     | Capacità per istanza del servizio |
|------------------|----------------------------:|
| Premium          |                          50 |
| Plus             |                          50 |
| Standard         |                          20 |
| Lite*            |                           5 |
{: caption="Dettagli piano di servizio" caption-side="top"}

*Dopo 30 giorni di inattività, una capacità non utilizzata in un'istanza del servizio del piano Lite potrebbe essere eliminata per liberare spazio.

## Scaricamento di una capacità di dialogo 
{: #skill-add-download}

Puoi scaricare una capacità di dialogo in formato JSON. Potresti voler scaricare una capacità se desideri utilizzare la stessa capacità di dialogo in un'istanza diversa del servizio {{site.data.keyword.conversationshort}}, ad esempio. Puoi scaricarla da un'istanza e importarla in un'altra istanza come una nuova capacità di dialogo. 

Per scaricare una capacità di dialogo, completa i seguenti passi: 

1.  Trova il tile della capacità di dialogo nella pagina Skills o nella pagina di configurazione di un assistente che utilizza la capacità. 

1.  Fai clic sull'icona ![apri e chiudi elenco delle opzioni](images/kabob-beta.png) e poi scegli **Download JSON**. 

1.  Specifica un nome per il file JSON e l'ubicazione in cui salvarlo e poi fai clic su **Save**. 

Puoi esportare una capacità utilizzando anche l'API. Includi il parametro `export=true` alla richiesta. Per ulteriori dettagli, vedi il [riferimento API](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace). 

## Eliminazione di una capacità 
{: #skill-add-delete}

Puoi eliminare tutte le capacità a cui puoi accedere, a meno che non siano utilizzate da un assistente. Se sono in utilizzo, devi rimuoverle dall'assistente che le sta utilizzando prima di poterle eliminare.

Assicurati di controllare se qualcun altro potrebbe stare utilizzando la capacità prima di eliminarla.
{: tip}

Per eliminare una capacità, completa la seguente procedura: 

1.  Scopri se la capacità sta venendo utilizzata da uno degli assistenti. Dalla scheda Skills, trova il tile per la capacità che desideri eliminare. Il campo **Assistants** elenca gli assistenti che al momento utilizzano la capacità.

1.  Se la capacità che desideri eliminare è associata a un assistente, rimuovila dall'assistente completando la seguente procedura: 

    - Esegui un controllo con il proprietario dell'assistente che sta utilizzando la capacità prima di rimuoverla da esso.
    - Apri la scheda Assistants e fai quindi clic per aprire il tile dell'assistente.
    - Trova il tile per la capacità che desideri eliminare. Fai clic sull'icona ![apri e chiudi elenco delle opzioni](images/kabob-beta.png) e poi scegli **Remove**. 
    - Ripeti i precedenti passi per tutti gli altri assistenti che utilizzano la capacità.
    - Ritorna alla scheda Skills e trova il tile per la capacità che desideri eliminare.

1.  Fai clic sull'icona ![apri e chiudi elenco delle opzioni](images/kabob-beta.png) e poi scegli **Delete**. Conferma l'eliminazione. 

## Condivisione di una capacità di dialogo con i membri del team 
{: #skill-add-invite-others}

Dopo aver creato l'istanza del servizio, puoi concedere ad altre persone l'accesso ad essa. Contemporaneamente, puoi definire i dati di addestramento e creare il dialogo.

**Importante**: solo una persona alla volta può modificare un intento, un'entità o un nodo di dialogo. Se più persone lavorano contemporaneamente sullo stesso elemento, le modifiche apportate dall'ultima persona che ha effettuato il salvataggio saranno le uniche modifiche che verranno applicate. Le modifiche apportate nello stesso arco di tempo da un'altra persona e salvate prima dell'ultimo salvataggio non verranno conservate. Coordina gli aggiornamenti che intendi apportare con i membri del tuo team in modo che nessuno perda il proprio lavoro.

Per condividere una capacità di dialogo con altre persone, devi fornire loro l'accesso all'istanza del servizio che ospita la capacità. Tieni presente che la persona che inviti sarà in grado di accedere a qualsiasi capacità ospitata da questa istanza del servizio. 

1.  Prendi nota del nome istanza corrente visualizzato all'inizio della pagina corrente. 
1.  Fai clic sull'icona Utente ![Utente](images/user-icon2.png) nell'intestazione della pagina e seleziona **Manage Users** dall'elenco a discesa. 

1.  Fai clic su **Invite users** e quindi immetti gli indirizzi email delle persone del tuo team a cui desideri concedere l'accesso.

    Se hai concesso a qualcuno l'accesso a un'istanza del servizio quando veniva gestita da Cloud Foundry, la persona potrebbe essere già elencata come un utente invitato. Fai clic sul nome della persona per aprire le impostazioni di gestione dell'accesso per l'utente. Fai clic su **Assign access** e quindi scegli **Assign access to resources**.

1.  Nella sezione *Services*, come minimo, esegui le seguenti selezioni: 

    - **Services**: {{site.data.keyword.conversationshort}}
    - **Assign platform access roles**: Operator

    Per ulteriori informazioni sui ruoli di gestione della piattaforma, vedi [Accesso IAM ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/iam?topic=iam-userroles). (I ruoli di accesso al servizio non vengono utilizzati da {{site.data.keyword.conversationshort}} per impostazione predefinita.) 

    Per le istanze più vecchie che vengono gestite da Cloud Foundry, devi espandere la sezione *Cloud Foundry access*, scegliere la tua organizzazione e poi assegnare la persona al ruolo spazio **Sviluppatore**.
    {: note}

1.  Fai clic su **Invite users**.

    Se stai modificando l'accesso per un utente esistente, fai clic su **Assign access**. 

Quando le persone che hai invitato eseguono poi l'accesso a {{site.data.keyword.cloud_notm}}, il tuo account verrà incluso nei loro elenchi di account. Se selezionano il tuo account, possono vedere la tua istanza del servizio e aprire e modificare le tue capacità. 

Quando più persone contribuiscono allo sviluppo della capacità di dialogo, possono verificarsi modifiche involontarie, incluse le eliminazioni delle capacità. Prendi in considerazione di creare regolarmente copie di backup della tua capacità di dialogo in modo che, se necessario, tu possa eseguire il ripristino ad una versione precedente. Per creare un backup, [scarica semplicemente la capacità come un file JSON](#skill-add-download).
