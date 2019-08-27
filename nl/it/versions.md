---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-05"

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

# Creazione di versioni della capacità
{: #versions}

Le versioni sono utili per gestire il flusso di lavoro di un progetto di sviluppo della capacità di dialogo.
{: shortdesc}

Crea una versione della capacità per acquisire un'istantanea dei dati di apprendimento (intenti ed entità) e il dialogo nella capacità in alcuni punti chiave durante il processo di sviluppo. Poter salvare una capacità in corso in uno specifico punto temporale è particolarmente utile quando inizi ad ottimizzare il tuo assistente. Hai spesso bisogno di apportare una modifica e vedere il suo impatto in tempo reale prima di poter determinare se la modifica migliora o meno l'efficacia dell'assistente. In base ai tuoi risultati dallo sviluppo di un ambiente di test, puoi prendere una decisione consapevole se distribuire una data modifica a un assistente che viene distribuito in un ambiente di produzione.

Se hai un piano (Lite) gratuito, non puoi creare le versioni della capacità.
{: note}

Questo video di 2 minuti e mezzo illustra come l'utilizzo delle versioni può esserti utile.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Creazione delle versioni della capacità" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/FDolnBxvcZ8" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Per ulteriori informazioni su come le versioni possono migliorare il flusso di lavoro che utilizzi per creare un assistente, [leggi questo post di blog ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://medium.com/ibm-watson/watson-assistant-versions-announcement-d60869b1f5f){: new_window}.

## Creazione di una versione
{: #versions-create}

Puoi modificare solo una versione della capacità di dialogo alla volta. La versione in corso viene denominata *di sviluppo*.

Quando salvi una versione, anche tutte le impostazioni della capacità che hai applicato alla versione di sviluppo vengono salvate.

Per creare una versione della capacità di dialogo, completa la seguente procedura:

1.  Dall'intestazione della capacità, fai clic su **Save new version** e quindi descrivi lo stato corrente della capacità.

    L'aggiunta di una buona descrizione ti aiuterà a distinguere più versioni tra loro.

1.  Fai clic su **Salva**.

Viene presa un'istantanea della capacità corrente e salvata come una nuova versione. Rimani nella versione di sviluppo della capacità. Tutte le modifiche che apporti continuano ad essere applicate alla versione di sviluppo, non alla versione che hai salvato. Per accedere alla versione che hai salvato, passa alla pagina **Versions**.

## Distribuzione di una versione della capacità
{: #versions-deploy}

1.  Dall'intestazione della capacità, fai clic sulla scheda **Versions**.
1.  Fai clic sull'icona ![Click to view actions](images/kebab-react.png) dalla versione che vuoi distribuire e scegli **Assign to assistant**.

    Viene visualizzato un elenco di assistenti a cui puoi collegare questa versione. L'elenco è limitato agli assistenti che non hanno alcuna capacità associata loro o che sono state associate con una versione differente di questa capacità.
1.  Fai clic sulla casella di spunta di uno o più assistenti e poi su **Assign**.

Tieni traccia di quando questa versione viene distribuita a un assistente e per quanto tempo. È probabile che vorrai analizzare le conversazioni dell'utente che si verificano tra gli utenti e questa versione specifica della capacità. Puoi ottenere queste informazioni dalla pagina **Analytics**. Tuttavia, quando selezioni un'origine dati, le versioni non vengono elencate. Devi scegliere il nome dell'assistente a cui hai distribuito questa versione. Puoi successivamente filtrare i dati delle metriche per visualizzare solo le conversazioni che si sono verificate tra la data di inizio e di fine dell'intervallo di tempo durante il quale questa versione della capacità è stata distribuita all'assistente.
{: important}

## Limiti della versione della capacità
{: #versions-limits}

Il numero di versioni che puoi creare per una sola capacità dipende dal tuo piano {{site.data.keyword.conversationshort}}.

| Piano di servizio     | Versioni per capacità |
|------------------|-------------------:|
| Premium          |                 50 |
| Plus             |                 10 |
| Standard         |                 10 |
| Plus Trial       |                 10 |
| Lite             |                  0 |
{: caption="Dettagli piano di servizio" caption-side="top"}

## Scambio capacità
{: #versions-swap-skills}

Se trovi che una versione precedente della capacità ha fatto un lavoro migliore nel riconoscere e risolvere i bisogni del cliente rispetto a una versione successiva, puoi cambiare la capacità collegata all'assistente in modo da utilizzare invece la versione precedente.

Segui la stessa procedura che hai utilizzato per [distribuire una versione della capacità](#versions-deploy) per modificare la versione collegata a un assistente.

## Accesso a una versione salvata
{: #versions-view}

L'unico modo per visualizzare una versione salvata è di sovrascrivere la versione di sviluppo in corso della capacità con la versione salvata. (Ma non prima di aver salvato tutto il lavoro eseguito nella versione di sviluppo corrente.)

Non puoi modificare una versione salvata. Per ottenere lo stesso risultato, puoi utilizzare una versione salvata come base per una nuova versione in cui incorpori tutte le modifiche che vuoi apportare. Per iniziare lo sviluppo da una versione salvata, sovrascrivi la versione di sviluppo in corso della capacità con la versione salvata.

1.  Salva tutte le modifiche che hai apportato alla capacità dall'ultima volta che hai creato una versione.

    Salva una versione della capacità ora. Altrimenti, il tuo lavoro andrà perso quando segui questa procedura:
    {: important}

1.  Dalla versione che vuoi modificare, fai clic sull'icona **Skill actions** ![Skill actions](images/kebab-react.png), scegli **Revert to this version** e conferma l'azione.

    La pagina si aggiorna per ripristinare lo stato a cui era la capacità quando è stata creata la versione.

Se vuoi salvare tutte le modifiche che hai apportato a questa versione, devi salvare la capacità come una nuova versione. Non puoi applicare le modifiche a una versione già salvata.

Quando apri una capacità facendo clic sul suo tile dalla pagina dell'assistente, viene visualizzata la versione di sviluppo della capacità. Anche se hai associato una versione successiva all'assistente, quando accedi alla capacità, viene aperta la sua versione di sviluppo.
{: important}
