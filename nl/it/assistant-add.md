---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-26"

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

Per ulteriori informazioni su cos'è innanzitutto un assistente, vedi [Assistenti](/docs/services/assistant?topic=assistant-assistants).

Segui questi passi per creare un assistente:

1.  Fai clic su **Create assistant**.

1.  Aggiungi i dettagli relativi al nuovo assistente:

    - **Name**: un nome lungo massimo 100 caratteri. Un nome è obbligatorio.
    - **Description**: una descrizione facoltativa lunga massimo 200 caratteri.

    Viene creata automaticamente una pagina web pubblica personalizzata IBM e il tuo team può utilizzarla per eseguire test sul tuo assistente. Se non desideri che la pagina web di anteprima venga creata, deseleziona la casella di spunta **Enable Preview Link**.

1.  Fai clic su **Create assistant**.

1.  Aggiungi una capacità all'assistente scegliendo uno dei seguenti tipi di capacità da aggiungere. 

    **Nota**: puoi scegliere di aggiungere una capacità esistente o di crearne una nuova. 

    - **Add Dialog Skill**: utilizza le tecnologie di machine learning e di elaborazione del linguaggio naturale di Watson per comprendere le domande e le richieste utente e per rispondervi con risposte create da te. 

      Quando aggiungi una capacità di dialogo da qui, otterrai la versione di sviluppo. Se vuoi aggiungere una versione di capacità di dialogo specifica, aggiungila invece dalla scheda *Versions* della capacità.

    - **Add Search Skill** ![Solo piano Plus o Premium](images/plus.png): per una determinata query utente, utilizza il servizio {{site.data.keyword.discoveryfull}} per richiamare le informazioni da un'origine dati da te identificata e condivide le informazioni pertinenti che trova come risposta all'utente. 

      Questa opzione è visibile solo se sei un utente del piano Plus o Premium.
      {: note}

    Vedi [Creazione di una capacità](/docs/services/assistant?topic=assistant-skill-add).

## Limiti dell'assistente
{: #assistant-add-limits}

Il numero di assistenti che puoi creare dipende dal tuo tipo di piano {{site.data.keyword.conversationshort}}.

| Piano | Assistenti per istanza del servizio |
|--------------|--------------------------------:|
| Premium      |                             100 |
| Plus         |                             100 |
| Standard     |                             100 |
| Plus Trial   |                               5 |
| Lite*        |                             100 |
{: caption="Dettagli del piano" caption-side="top"}

*Dopo 30 giorni di inattività, un assistente non utilizzato in un'istanza del servizio del piano Lite potrebbe essere eliminato per liberare spazio.

Per ulteriori informazioni sull'argomento, vedi [Modifica dell'impostazione del timeout di inattività](/docs/services/assistant?topic=assistant-assistant-settings).

Puoi collegare una capacità di ciascun tipo al tuo assistente. Il numero di capacità che puoi creare dipende dal tuo piano. Per ulteriori dettagli, vedi [Limiti della capacità](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits).

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

Puoi aggiungere una capacità di ciascun tipo di capacità ad un assistente. Se desideri modificare una capacità utilizzata dal tuo assistente, puoi scambiare una capacità con un'altra. 

1.  Dalla scheda Assistants, apri l'assistente. 

1.  Fai clic sull'icona ![apri e chiudi elenco di opzioni](images/kabob-beta.png) per la capacità che desideri scambiare e poi scegli **Swap skill**.

    Per scambiare la capacità di dialogo corrente con una versione diversa della capacità, scegli **Change skill version**.

1.  Scegli invece una capacità esistente da utilizzare oppure [crea una capacità](/docs/services/assistant?topic=assistant-skill-add).

### Passaggio da un'istanza del servizio all'altra
{: #assistant-add-switch-instance}

Se hai più di un'istanza del servizio, puoi controllare l'intestazione della pagina per capire quale istanza stai attualmente utilizzando. Se stai lavorando in una capacità, fai innanzitutto clic sul link di navigazione **Skills**. Il banner visualizza il nome dell'istanza corrente. Per passare a un'istanza del servizio diversa, fai clic su **Change** e poi scegli l'istanza appropriata. 
