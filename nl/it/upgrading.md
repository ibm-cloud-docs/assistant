---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-09"

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

# Aggiornamento

Apprendi come aggiornare il tuo piano di servizio.
{: shortdesc}

## Aggiornamento del tuo piano
{: #plan-upgrade}

Con il piano Lite, puoi esplorare ampiamente il servizio {{site.data.keyword.conversationshort}}. Tuttavia, ci sono dei limiti a ciò che puoi fare.

### Limiti per risorsa
Per informazioni sui limiti di risorse per ciascun piano, vedi questi argomenti:

- [Spazi di lavoro](configure-workspace.html#workspace-limits)
- [Nodi del dialogo](dialog-build.html#dialog-node-limits)
- [Intenti](intents.html#intent-limits)
- [Entità](entities.html#entity-limits)
- [Log](logs_convo.html#log-limits)

### Limiti di chiamate API
Il numero di chiamate API consentite per ogni istanza dipende dal tuo piano di servizio. Per i dettagli, vedi la descrizione del tuo piano.

Se hai un piano Lite e raggiungi il tuo limite di chiamate API, ma i log mostrano che hai fatto meno chiamate del previsto, ricorda che il piano Lite memorizza le informazioni di log solo per 7 giorni.

Per aggiornare il tuo piano, completa questa procedura:

1.  Dal menu {{site.data.keyword.Bluemix_notm}}, seleziona **Servizi** > **Dashboard**.
1.  Seleziona l'istanza del servizio da aggiornare per aprirla.
1.  Fai clic su **Piano** dal riquadro di navigazione.
   Da qui, puoi visualizzare il tuo piano corrente e altre opzioni di piano disponibili e apportare modifiche.

Per le risposte alle domande più frequenti sulle sottoscrizioni, vedi [Gestione della fatturazione e dell'utilizzo![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/billing-usage/how_charged.html){: new_window}.

Puoi reperire ulteriori informazioni sui servizi ospitati da IBM Cloud ai seguenti link:

- [Termini dei servizi Cloud](http://www.ibm.com/software/sla/sladb.nsf/sla/saas)
- [Privacy e sicurezza dei dati dei servizi Cloud](http://www.ibm.com/software/sla/sladb.nsf/sla/csdsp)

## Aggiornamento del tuo spazio di lavoro
{: #upgrade-workspace}

Il servizio {{site.data.keyword.conversationshort}} aggiunge e aggiorna regolarmente le funzioni. Mentre alcune di queste modifiche vengono applicate automaticamente allo spazio di lavoro, le modifiche che hanno un impatto maggiore richiedono un aggiornamento manuale.

Un aggiornamento è disponibile per il tuo spazio di lavoro solo se viene visualizzata l'icona di aggiornamento (![icona di aggiornamento](images/upgrade.png)).

**Nota**: dopo aver aggiornato uno spazio di lavoro, non puoi ripristinarlo a una versione precedente. 

Per aggiornare il tuo spazio di lavoro, completa la procedura riportata di seguito: 
1.  [Duplica il tuo spazio di lavoro](configure-workspace.html#exporting-and-copying-workspaces).
2.  Aggiorna lo spazio di lavoro duplicato.

    Quando aggiorni il tuo spazio di lavoro, nello strumento viene abilitata l'ultima versione dell'API e il riquadro "Provalo" inizia a utilizzare le funzioni più recenti.
3.  Verifica lo spazio di lavoro aggiornato. 
4.  Dopo aver valutato lo spazio di lavoro duplicato per comprendere in che modo l'aggiornamento influirà sulla tua applicazione, applica l'aggiornamento allo spazio di lavoro primario.
5.  Aggiorna la tua applicazione modificando la chiamata API message per utilizzare la versione API più recente. 
