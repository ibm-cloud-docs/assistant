---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-28"

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

# Avvio del dialogo
{: #dialog-start}

Non puoi utilizzare il nodo di benvenuto integrato per avviare un dialogo allo stesso modo per tutte le integrazioni. Utilizza invece questa soluzione temporanea.
{: shortdesc}

La risposta che definisci per il nodo di benvenuto nel dialogo viene visualizzata per iniziare una conversazione dal riquadro "Try it out" e dal widget della chat nell'integrazione Preview Link. Tuttavia, non viene visualizzata da molte delle altre integrazioni del canale perché i nodi con la condizione speciale `welcome` vengono ignorati nei flussi di dialogo avviati dagli utenti. E, di norma, gli assistenti distribuiti attendono che gli utenti inizino una conversazione con loro, non il contrario.

A differenza della condizione speciale `welcome`, la condizione speciale `conversation_start` viene sempre attivata quando si avvia un dialogo. Puoi utilizzare una combinazione di nodi con queste due condizioni speciali (`welcome` e `conversation_start`) per gestire l'avvio del tuo dialogo in modo congruente.

Completa i seguenti passi per gestire l'avvio del dialogo:

1.  Aggiungi un nodo di dialogo sopra il nodo di benvenuto che viene aggiunto automaticamente all'inizio della struttura ad albero di dialogo quando crei il dialogo.

1.  Imposta la condizione nodo per questo nodo appena aggiunto su `conversation_start`, la condizione speciale descritta in precedenza.

1.  Definisci le variabili di contesto che desideri impostare con i valori predefiniti per il dialogo nel nodo `conversation_start`.

1.  Non definire una risposta di testo per questo nodo.

1.  Configura questo nodo perché passi al nodo `Welcome` direttamente sotto di esso nella struttura ad albero di dialogo e scegli **If bot recognizes (condition)**.

![Acquisizione schermo della struttura ad albero di dialogo con un nodo conversation_start che passa al nodo di benvenuto sotto di esso.](images/dialog-start.png)

Questa progettazione genera un dialogo che funziona in questo modo:

- Indipendentemente dal tipo di integrazione, il nodo `conversation_start` viene elaborato, ciò significa che le variabili di contesto che definisci in esso vengono inizializzate.
- Nelle integrazioni in cui gli assistenti avviano il flusso di dialogo, il nodo `Welcome` viene attivato e viene visualizzata la sua risposta di testo.
- Nelle integrazioni in cui l'utente avvia il flusso di dialogo, viene valutato innanzitutto l'input dell'utente e poi viene elaborato dal nodo che può fornire la risposta migliore.
