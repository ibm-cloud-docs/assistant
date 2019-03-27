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

# Aggiornamento
{: #upgrade}

Apprendi come aggiornare il tuo piano di servizio.
{: shortdesc}

## Aggiornamento del tuo piano
{: #upgrade-plan}

Puoi esplorare {{site.data.keyword.conversationshort}} le opzioni del piano di servizio [ ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window} per decidere quale piano è il migliore per te.

Non puoi eseguire l'upgrade di un'istanza basata su Cloud Foundry a un piano Plus. Devi migrare l'istanza, che sta utilizzando un gruppo di risorse prima che tu possa eseguirne l'upgrade. Per ulteriori dettagli, vedi [Migrazione](/docs/services/assistant?topic=assistant-migrate).
{: note}

Per aggiornare il tuo piano, completa questa procedura:

1.  Dal menu di {{site.data.keyword.Bluemix_notm}}, seleziona **Upgrade Plan**.
    Da qui, puoi visualizzare il tuo piano corrente e altre opzioni di piano disponibili e apportare modifiche.

Per le risposte alle domande più frequenti sulle sottoscrizioni, vedi [Gestione della fatturazione e dell'utilizzo![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/billing-usage?topic=billing-usage-charges){: new_window}.

Hai ancora altre domande? Contatta [IBM Sales ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.

## Upgrade di una capacità di dialogo
{: #upgrade-skill}

Il servizio {{site.data.keyword.conversationshort}} aggiunge e aggiorna regolarmente le funzioni. Mentre alcune di queste modifiche vengono applicate automaticamente alle tue capacità di dialogo, gli aggiornamenti che hanno un impatto maggiore richiedono un aggiornamento manuale al modello di machine learning che viene utilizzato dalla tua capacità.

Un aggiornamento è disponibile per la tua capacità di dialogo solo se viene visualizzata l'icona di aggiornamento (![icona di aggiornamento](images/upgrade.png)).

Per aggiornare la tua capacità di dialogo, completa la procedura riportata di seguito: 

1.  [Scarica una copia della tua capacità di dialogo](/docs/services/assistant?topic=assistant-skill-add#skill-add-download-skill) e quindi importala come una nuova capacità.
2.  Aggiorna la nuova copia della tua capacità di dialogo.

    Quando aggiorni la tua capacità di dialogo, nello strumento viene abilitata l'ultima versione dell'API e il riquadro "Try it out" inizia a utilizzare le funzioni più recenti.
3.  Verifica la capacità aggiornata. 
4.  Dopo aver valutato la capacità aggiornata per comprendere in che modo l'aggiornamento influirà sulla tua applicazione, applica l'aggiornamento alla capacità di dialogo primaria.
5.  Aggiorna la tua applicazione. A tale scopo, modifica le chiamate API del messaggio che utilizza per specificare l'ultima versione dell'API. Per i dettagli sulla versione dell'API, vedi le [note sulla release](/docs/services/assistant?topic=assistant-release-notes).
