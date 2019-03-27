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

# Creazione di una capacità di dialogo
{: #beta-skill-dialog-add}

L'elaborazione del linguaggio naturale per il servizio {{site.data.keyword.conversationshort}} viene definita in una *capacità di dialogo* che costituisce un contenitore per tutte le risorse che definiscono un flusso di conversazione.
{: shortdesc}

Questa funzione è disponibile per essere utilizzata solo dai partecipanti al programma beta. Per capire come richiedere l'accesso, vedi [Partecipa al programma beta](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

![Beta](images/beta.png) IBM rilascia servizi, funzioni e supporto linguistico classificati come beta per la tua valutazione. Queste funzioni potrebbero essere instabili, cambiare frequentemente e potrebbero essere sospese con breve preavviso. Inoltre, le funzioni beta potrebbero non fornire lo stesso livello di prestazioni o di compatibilità fornito generalmente dalle funzioni e non sono progettate per essere utilizzate in un ambiente di produzione. 

Puoi aggiungere una capacità di dialogo a un assistente. Vedi [Limiti di capacità](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits) per informazioni sui limiti per piano. 

## Creazione di una capacità di dialogo
{: #beta-skill-dialog-add-task}

Puoi creare una capacità di dialogo da zero, utilizzare una capacità di esempio fornita da IBM oppure importare una capacità da un file JSON. 

Se ancora non hai fatto nessuna di queste operazioni, completa i passi prerequisiti presenti nell'[esercitazione introduttiva](/docs/services/assistant?topic=assistant-getting-started#getting-started-prerequisites) per creare un'istanza del servizio {{site.data.keyword.conversationshort}} e avviare lo strumento. 

Utilizza lo strumento {{site.data.keyword.conversationshort}} per creare le capacità. Segui questi passi per creare una capacità di dialogo: 

1.  Fai clic sulla scheda **Skills**.

    Se hai creato o ti è stato fornito l'accesso del ruolo di sviluppatore agli spazi di lavoro che sono stati creati con la versione generalmente disponibile del servizio {{site.data.keyword.conversationshort}} (in precedenza noto come Watson Conversation), li vedrai elencati qui come capacità di dialogo.
    {: note}

1.  Fai clic su **Create new**.

1.  Fai clic su **Add** per creare una *capacità di dialogo*.

1.  Effettua una delle operazioni riportate di seguito:

    - Per creare una capacità da zero, fai clic su **Create skill**.
    - Per aggiungere una capacità di esempio fornita con il servizio come punto di partenza per la tua capacità o come esempio da esplorare prima che tu ne crei una tua, fai clic su **Use sample skill** e poi fai clic sull'esempio che desideri utilizzare.

      La capacità di esempio viene aggiunta al tuo elenco di capacità. Non viene associata a nessun assistente. Ignora i passi rimanenti presenti in questa procedura. 

    - Per aggiungere una capacità esistente a questa istanza del servizio, puoi importarla come un file JSON. Fai clic su **Import skill** e poi fai clic su **Choose JSON File** e seleziona il file JSON che desideri importare.

      **Importante:**

      - Il file JSON importato deve utilizzare la codifica UTF-8.
      - La dimensione massima per un file JSON della capacità è 10MB. Se devi importare una capacità più grande, ti consigliamo di importare gli intenti e le entità separatamente dopo aver importato la capacità. (Puoi anche importare capacità più grandi utilizzando l'API REST. Per ulteriori informazioni, vedi il [Riferimento API![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#create_workspace){: new_window}.)
      - Il JSON non può contenere caratteri di tabulazione, nuova riga o ritorno a capo.

      Specifica i dati da includere:

        - Seleziona **Everything (Intents, Entities, and Dialog)** se desideri importare una copia completa della capacità esportata, incluso il dialogo.
        - Seleziona **Intents and Entities** se desideri utilizzare gli intenti e le entità provenienti dalla capacità esportata, ma intendi creare un nuovo dialogo. 

      Fai clic su **Import**.

      Se riscontri dei problemi nell'importazione di una capacità, vedi [Risoluzione dei problemi di importazione della capacità](#beta-skill-dialog-add-import-errors).

1.  Specifica i dettagli per la capacità:

    - **Name**: un nome lungo massimo 100 caratteri. Un nome è obbligatorio.
    - **Description**: una descrizione facoltativa lunga massimo 200 caratteri.
    - **Language**: la lingua dell'input utente che la capacità imparerà a comprendere. Il valore predefinito è Inglese.

Una volta creata la capacità, comparirà come un tile nella pagina Skills. Ora, puoi iniziare ad identificare gli obiettivi utente che desideri vengano soddisfatti dalla capacità di dialogo. 

- Per aggiungere intenti precostruiti alla tua capacità, vedi [Utilizzo dei cataloghi di contenuto](/docs/services/assistant?topic=assistant-catalog).
- Per definire i tuoi intenti, vedi [Intenti](/docs/services/assistant?topic=assistant-intents).

La capacità di dialogo non può interagire con i clienti fino a quando non viene aggiunta a un assistente e quest'ultimo viene distribuito. Vedi [Creazione di assistenti](/docs/services/assistant?topic=assistant-assistant-add).

### Risoluzione dei problemi di importazione della capacità
{: #beta-skill-dialog-add-import-errors}

Se ricevi un messaggio indicante che la capacità contiene risorse che superano i limiti imposti dal tuo piano di servizio, completa i seguenti passi per importare correttamente la capacità: 

1.  Acquista un piano con limiti di risorse più elevati. 
1.  Crea un'istanza del servizio nel nuovo piano. 
1.  Importa la capacità nella nuova istanza del servizio. 
1.  Apporta le modifiche alla capacità in modo che soddisfi i requisiti dei limiti della risorsa per il piano che desideri utilizzare andando avanti. Potresti dover ridurre il numero di nodi di dialogo utilizzati nella struttura ad albero del dialogo, ad esempio. 
1.  Esporta la capacità modificata scaricandola. 
1.  Riprova ad importare la capacità modificata nell'istanza del servizio originale nel piano che desideri. 

### Aggiunta della capacità a un assistente
{: #beta-skill-dialog-add-to-assistant}

Puoi aggiungere una capacità a un assistente. Devi aprire il tile dell'assistente e aggiungere la capacità all'assistente dalla pagina di configurazione dell'assistente; non puoi scegliere l'assistente che utilizzerà la capacità dalla pagina di configurazione della capacità. Una capacità di dialogo può essere utilizzata da più di un assistente. 

1.  Dalla scheda Assistants, fai clic per aprire il tile per l'assistente a cui desideri aggiungere la capacità. 

1.  Fai clic su **Add Dialog Skill**.

1.  Fai clic su **Add existing skill**.

    Fai clic sulla capacità che desideri aggiungere dalle capacità disponibili visualizzate. 

    Se stai aggiungendo una capacità di dialogo esistente e hai creato o ti è stato fornito l'accesso del ruolo di sviluppatore agli spazi di lavoro che sono stati creati con la versione generalmente disponibile del servizio {{site.data.keyword.conversationshort}} (in precedenza noto come Watson Conversation), li vedrai elencati qui come capacità di dialogo.
    {: note}

## Scaricamento di una capacità di dialogo
{: #beta-skill-dialog-add-download}

Puoi scaricare una capacità di dialogo in formato JSON. Potresti voler scaricare una capacità se desideri utilizzare la stessa capacità di dialogo in un'istanza diversa del servizio {{site.data.keyword.conversationshort}}, ad esempio. Puoi scaricarla da un'istanza e importarla in un'altra istanza come una nuova capacità di dialogo. 

Per scaricare una capacità di dialogo, completa i seguenti passi: 

1.  Trova il tile della capacità di dialogo nella pagina Skills o nella pagina di configurazione di un assistente che utilizza la capacità.

1.  Fai clic sull'icona ![apri e chiudi elenco delle opzioni](images/kabob-beta.png) e poi scegli **Download JSON**.

1.  Specifica un nome per il file JSON e l'ubicazione in cui salvarlo e poi fai clic su **Save**.

Puoi esportare una capacità utilizzando anche l'API. Includi il parametro `export=true` alla richiesta. Per ulteriori dettagli, vedi il [riferimento API](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace). 

## Condivisione di una capacità di dialogo con i membri del team
{: #beta-skill-dialog-add-invite-others}

Dopo aver creato l'istanza del servizio, puoi concedere ad altre persone l'accesso ad essa. Contemporaneamente, puoi definire i dati di addestramento e creare il dialogo.

Solo una persona alla volta può modificare un intento, un'entità o un nodo di dialogo. Se più persone lavorano contemporaneamente sullo stesso elemento, le modifiche apportate dall'ultima persona che ha effettuato il salvataggio saranno le uniche modifiche che verranno applicate. Le modifiche apportate nello stesso arco di tempo da un'altra persona e salvate prima dell'ultimo salvataggio non verranno conservate. Coordina gli aggiornamenti che intendi apportare con i membri del tuo team in modo che nessuno perda il proprio lavoro.
{: important}

Per condividere una capacità di dialogo con altre persone, devi fornire loro l'accesso all'istanza del servizio che ospita la capacità. Tieni presente che la persona che inviti sarà in grado di accedere a qualsiasi capacità ospitata da questa istanza del servizio. 

1.  Prendi nota del nome istanza corrente visualizzato all'inizio della pagina corrente. 
1.  Fai clic sull'icona Utente ![Utente](images/user-icon2.png) nell'intestazione della pagina e seleziona **Gestisci utenti** dall'elenco a discesa.

1.  Fai clic su **Invita utenti** e quindi immetti gli indirizzi email delle persone del tuo team a cui desideri concedere l'accesso.

    Se concedi a qualcuno l'accesso a un'istanza del servizio in Cloud Foundry, la persona potrebbe essere già elencata come utente invitato. Fai clic sul nome della persona per aprire le impostazioni di gestione dell'accesso per l'utente. Fai clic su **Assegna accesso** e poi scegli **Assegna l'accesso alle risorse**.
1.  Nella sezione *Servizi*, esegui almeno le seguenti selezioni:

    - **Servizi**: {{site.data.keyword.conversationshort}}
    - **Assegna i ruoli di accesso alla piattaforma**: Operatore

    Per ulteriori informazioni sui ruoli di gestione della piattaforma, vedi [Accesso IAM ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/iam?topic=iam-userroles). (I ruoli di accesso di servizio non vengono utilizzati da {{site.data.keyword.conversationshort}} per impostazione predefinita.)

    Per le istanze più vecchie che vengono gestite da Cloud Foundry, devi espandere la sezione *Accesso Cloud Foundry*, scegliere la tua organizzazione e poi assegnare la persona al ruolo spazio **Sviluppatore**.
    {: note}

1.  Fai clic su **Invita utenti**.

    Se stai modificando l'accesso per un utente esistente, fai clic su **Assegna accesso**.

Quando le persone che hai invitato eseguono poi l'accesso a {{site.data.keyword.cloud_notm}}, il tuo account verrà incluso nei loro elenchi di account. Se selezionano il tuo account, possono vedere la tua istanza del servizio e aprire e modificare le tue capacità. 

Quando più persone contribuiscono allo sviluppo della capacità di dialogo, possono verificarsi modifiche involontarie, incluse le eliminazioni delle capacità. Prendi in considerazione di creare regolarmente copie di backup della tua capacità di dialogo in modo che, se necessario, tu possa eseguire il ripristino ad una versione precedente. Per creare un backup, devi semplicemente [scaricare la capacità come un file JSON](#beta-skill-dialog-add-download).
