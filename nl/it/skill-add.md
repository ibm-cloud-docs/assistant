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

# Creazione di una capacità
{: #skill-add}

Personalizza il tuo assistente aggiungendo ad esso le capacità di cui necessita per soddisfare gli obiettivi dei tuoi clienti.
{: shortdesc}

Puoi creare i seguenti tipi di capacità:

- **Capacità di dialogo**: utilizza le tecnologie di machine learning e di elaborazione del linguaggio naturale di Watson per comprendere le domande e le richieste utente e per rispondervi con risposte create da te.

- **Capacità di ricerca** ![Solo piano Plus o Premium](images/plus.png): per una determinata query utente, utilizza il servizio {{site.data.keyword.discoveryfull}} per ricercare un'origine dati del tuo contenuto self-service e restituire una risposta.

  Solo gli utenti dei piani Plus o Premium possono creare questo tipo di capacità.
  
  Normalmente, crei per prima cosa una capacità di ogni tipo. Successivamente, quando crei un dialogo per la capacità di dialogo, decidi quando avviare la capacità di ricerca. Per alcune domande o richieste, una risposta codificata o derivata in modo programmatico (definita nella capacità di dialogo) è sufficiente. Per altre, potresti voler fornire una risposta più consistente restituendo un passaggio completo di informazioni correlate (estratto da un'origine dati esterna utilizzando la capacità di ricerca).

## Crea la capacità
{: #skill-add-task}

Puoi aggiungere una capacità di ogni tipo all'assistente.

Per creare una capacità, completa la seguente procedura:

1.  Fai clic sulla scheda **Skills** e poi fai clic su **Create skill**.

1.  Scegli il tipo di capacità da creare e poi fai clic su **Next**.

    Segui le rimanenti fasi nella procedura appropriata per completare il processo di creazione della capacità.

      - [Capacità di dialogo](/docs/services/assistant?topic=assistant-skill-dialog-add)
      - [Capacità di ricerca](/docs/services/assistant?topic=assistant-skill-search-add)

## Limiti della capacità
{: #skill-add-limits}

Il numero di capacità che puoi creare dipende dal tuo tipo di piano {{site.data.keyword.conversationshort}}. Ogni capacità di dialogo di esempio disponibile per il tuo utilizzo non viene conteggiata nel tuo limite finché non la utilizzi. Una versione della capacità non viene contata come una capacità.

|Piano| Capacità per istanza del servizio |
|------------------|----------------------------:|
| Premium          |                          50 |
| Plus             |                          50 |
| Standard         |                          20 |
| Lite`*`, Plus Trial |                        5 |
{: caption="Dettagli del piano" caption-side="top"}

`*` Dopo 30 giorni di inattività, una capacità non utilizzata in un'istanza del servizio del piano Lite potrebbe essere eliminata per liberare spazio.

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

1.  Fai clic sull'icona ![apri e chiudi elenco delle opzioni](images/kabob-beta.png) e quindi scegli **Delete**. Conferma l'eliminazione.
