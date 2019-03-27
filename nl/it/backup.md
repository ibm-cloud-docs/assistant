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

# Backup e ripristino dei dati
{: #backup}

Esegui il backup e il ripristino dei tuoi dati esportando e poi importando i dati.
{: shortdesc}

Puoi esportare i seguenti dati da un'istanza del servizio {{site.data.keyword.conversationshort}}:

- Dati di apprendimento della capacità di dialogo (intente ed entità)
- Dialogo della capacità di dialogo

Non puoi esportare i seguenti dati:

<!--- Search skill -->
- Assistente, incluse le integrazioni configurate

## Conservazione dei log
{: #backup-retain-logs}

Se desideri memorizzare i log delle conversazioni che gli utenti hanno avuto con il tuo assistente, puoi utilizzare l'API `/logs` per esportare i tuoi dati di log. Per i dettagli, vedi [Riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/apidocs/assistant#list-log-events-in-a-workspace).

Per richiamare l'ID spazio di lavoro per una capacità, dal tile della capacità, fai clic sull'icona ![apri e chiudi elenco delle opzioni](images/kabob-beta.png) e poi scegli **View API Details**.
{: tip}

I log vengono memorizzati per un lasso di tempo diverso, a seconda del tuo piano di servizio. Ad esempio, i piani Lite forniscono log solo degli ultimi 7 giorni. Per ulteriori informazioni, vedi [Limiti dei log](/docs/services/assistant?topic=assistant-logs#logs-limits).

Non puoi importare i log da una capacità in un'altra. 

## Esportazione di una capacità di dialogo
{: #backup-export-skill}

Per eseguire il backup dei dati della capacità di dialogo, esporta la capacità come un file JSON e memorizza il file JSON. 

1.  Trova il tile della capacità di dialogo nella pagina Skills o nella pagina di configurazione di un assistente che utilizza la capacità. 

1.  Fai clic sull'icona ![apri e chiudi elenco delle opzioni](images/kabob-beta.png) e poi scegli **Download JSON**. 

1.  Specifica un nome per il file JSON e l'ubicazione in cui salvarlo e poi fai clic su **Save**.

In alternativa, puoi utilizzare l'API `/workspaces` per esportare una capacità di dialogo. Includi il parametro `export=true` con la richiesta di spazio di lavoro GET. Per ulteriori dettagli, vedi il [Riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace). 

## Importazione di una capacità di dialogo
{: #backup-import-skill}

Per reinstallare una copia di backup di una capacità di dialogo che hai esportato da un'altra istanza del servizio o da un altro ambiente, crea una nuova capacità di dialogo importando il file JSON della capacità di dialogo che hai esportato. 

Se il servizio {{site.data.keyword.conversationshort}} cambia tra il momento in cui hai esportato la capacità e quello in cui vuoi importarla, a causa di aggiornamenti che vengono applicati regolarmente alle istanze in ambienti di fornitura continua ospitati nel cloud, la tua capacità importata potrebbe funzionare in modo diverso rispetto a prima.
{: important}

1.  Fai clic sulla scheda **Skills**.

1.  Fai clic su **Create new**.

1.  Fai clic su **Import skill** e poi fai clic su **Choose JSON File** e seleziona il file JSON che desideri importare.

    **Importante:**

    - Il file JSON importato deve utilizzare la codifica UTF-8, senza codifica BOM (byte order mark).
    - La dimensione massima per un file JSON della capacità è 10MB. Se devi importare una capacità più grande, ti consigliamo di importare gli intenti e le entità separatamente dopo aver importato la capacità. (Puoi anche importare capacità più grandi utilizzando l'API REST. Per ulteriori informazioni, vedi il [Riferimento API![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/apidocs/assistant#create-workspace){: new_window}.)
    - Il file JSON non può contenere caratteri di tabulazione, nuova riga o ritorno a capo. 

    Seleziona **Everything (Intents, Entities, and Dialog)** per importare una copia completa della capacità esportata. 

    Fai clic su **Import**.

    Se riscontri dei problemi nell'importazione di una capacità, vedi [Risoluzione dei problemi di importazione della capacità](/docs/services/assistant?topic=assistant-skill-add#skill-add-import-errors).

1.  Specifica i dettagli per la capacità:

    - **Name**: un nome lungo massimo 100 caratteri. Un nome è obbligatorio.
    - **Description**: una descrizione facoltativa lunga massimo 200 caratteri.
    - **Language**: la lingua dell'input utente che la capacità imparerà a comprendere. Il valore predefinito è Inglese. 

Una volta creata la capacità, comparirà come un tile nella pagina Skills.

## Ricreazione del tuo assistente
{: #backup-recreate-assistant}

Ora puoi ricreare il tuo assistente. Poi puoi collegare la tua capacità di dialogo importata all'assistente e configurare le integrazioni relative ad essa. 

Per ulteriori dettagli, vedi [Creazione di un assistente](/docs/services/assistant?topic=assistant-assistant-add) e [Aggiunta delle integrazioni](/docs/services/assistant?topic=assistant-deploy-integration-add#deploy-integration-add-task).

## Aggiorna le tue applicazioni client
{: #backup-update-api}

Quando importi una capacità di dialogo che hai esportato, viene creata una nuova capacità. La nuova capacità ha un nuovo ID spazio di lavoro. Se hai applicazioni client esistenti che utilizzano l'API v1 per accedere a questa capacità, devi aggiornare tutti i riferimenti dell'ID spazio di lavoro per poter utilizzare invece il nuovo ID spazio di lavoro.

Quando ricrei il tuo assistente, gli viene fornito un nuovo ID assistente. Se hai applicazioni client esistenti che utilizzano l'API v2 per accedere all'assistente, devi aggiornare tutti i riferimenti ID assistente per poter utilizzare invece il nuovo ID assistente. 
