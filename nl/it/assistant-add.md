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

# Creazione di un assistente
{: #assistant-add}

Crea un assistente con le capacità necessarie per far fronte agli obiettivi aziendali dei tuoi clienti.
{: shortdesc}

Segui questi passi per creare un assistente:

1.  Fai clic sulla scheda **Assistants**.

1.  Effettua una delle operazioni riportate di seguito:

    - Per creare un assistente di esempio che tu possa esaminare e da cui tu possa acquisire informazioni, fai clic su **Add a sample** e scegli quindi l'assistente di esempio da creare. 

      L'assistente di esempio viene aggiunto. Puoi ignorare i passi rimanenti presenti in questa procedura. 

      Con l'assistente di esempio viene fornita una capacità di esempio e viene aggiunta al tuo elenco delle capacità. Se hai già creato una capacità di esempio dello stesso tipo, la capacità esistente viene associata automaticamente a questo nuovo assistente di esempio.
      {: note}

    - Per creare un assistente da zero, fai clic su **Create new** e poi completa i passi rimanenti di questa procedura. 

1.  Specifica i dettagli per il nuovo assistente:
    - **Name**: un nome lungo massimo 100 caratteri. Un nome è obbligatorio.
    - **Description**: una descrizione facoltativa lunga massimo 200 caratteri. 

    Viene creata automaticamente una pagina web pubblica con marchio IBM e il tuo team può utilizzarla per eseguire test sul tuo assistente. Se non desideri che la pagina web di anteprima venga creata, deseleziona la casella di spunta **Enable Preview Link**.

1.  Fai clic su **Create**.

1.  Aggiungi una capacità all'assistente facendo clic su **Add skill**. Puoi scegliere di aggiungere una capacità esistente o di crearne una nuova. 

    Quando aggiungi una capacità da qui, otterrai la versione di sviluppo. Se desideri aggiungere una specifica versione di capacità, aggiungila invece dalla scheda *Version History* della capacità. 

    Se hai creato o ti è stato fornito l'accesso del ruolo di sviluppatore agli spazi di lavoro che sono stati creati con la versione generalmente disponibile del servizio {{site.data.keyword.conversationshort}} (in precedenza noto come Watson Conversation), li vedrai elencati come capacità di dialogo esistenti.
    {: note}

    Consulta [Creazione di una capacità](/docs/services/assistant?topic=assistant-skill-add) per ulteriori informazioni su come creare una capacità. 

## Limiti dell'assistente
{: #assistant-add-limits}

Il numero di assistenti che puoi creare in una singola istanza del servizio dipende dal tuo piano {{site.data.keyword.conversationshort}}.

| Piano di servizio | Assistenti per istanza del servizio | Integrazioni per assistente | Periodo di inattività sessione di chat |
|--------------|--------------------------------:|----------------------------:|-----------------:|
| Premium      |                             100 |                         100 |       60 minuti  |
| Plus         |                             100 |                         100 |       60 minuti  |
| Standard     |                             100 |                         100 |        5 minuti  |
| Lite*        |                             100 |                         100 |        5 minuti  |
{: caption="Dettagli piano di servizio" caption-side="top"}

*Dopo 30 giorni di inattività, un assistente non utilizzato in un'istanza del servizio del piano Lite potrebbe essere eliminato per liberare spazio. 

Puoi collegare una capacità al tuo assistente. Il numero di capacità che puoi creare dipende dal tuo piano. Per ulteriori dettagli, vedi [Limiti della capacità](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits).

## Eliminazione di un assistente
{: #assistant-add-delete}

Quando elimini un assistente, anche le integrazioni che hai definito per l'assistente vengono eliminate automaticamente. Le capacità che hai aggiunto all'assistente non vengono eliminate. 

Per eliminare un assistente, segui questi passi:

1.  Dalla scheda Assistants, trova l'assistente che desideri eliminare. 

1.  Fai clic sull'icona ![apri e chiudi elenco delle opzioni](images/kabob-beta.png) e quindi scegli **Delete**. Conferma l'eliminazione. 

## Ridenominazione di un assistente
{: #assistant-add-rename}

Puoi modificare il nome di un assistente e la descrizione associata dopo averlo creato. 

Per ridenominare un assistente, segui questi passi:

1.  Dalla scheda Assistants, trova l'assistente che desideri ridenominare.

1.  Fai clic sull'icona ![apri e chiudi elenco delle opzioni](images/kabob-beta.png) e quindi scegli **Rename**.

1.  Modifica il nome e poi fai clic su **Rename** per salvare le tue modifiche. 

### Modifica della capacità associata all'assistente
{: #assistant-add-swap-skill}

Puoi aggiungere una capacità a un assistente. Se desideri modificare la capacità utilizzata dal tuo assistente, puoi scambiare una capacità con un'altra. 

1.  Dalla scheda Assistants, fai clic per aprire il tile dell'assistente per il quale desideri modificare la capacità. 

1.  Fai clic sull'icona ![apri e chiudi elenco delle opzioni](images/kabob-beta.png) e poi scegli **Swap skill**. Per scambiare la capacità corrente con una versione diversa della capacità, scegli **Change skill version**.

1.  Scegli invece una capacità esistente da utilizzare oppure [crea una capacità](/docs/services/assistant?topic=assistant-skill-add).

### Passaggio da un'istanza del servizio all'altra
{: #assistant-add-switch-instance}

Se hai più di un'istanza del servizio, puoi controllare l'intestazione della pagina per capire quale istanza stai attualmente utilizzando. Se stai lavorando in una capacità, fai innanzitutto clic sul link di navigazione **Skills**. Il banner visualizza il nome dell'istanza corrente. Per passare a un'istanza del servizio diversa, fai clic su **change** e poi scegli l'istanza appropriata.
