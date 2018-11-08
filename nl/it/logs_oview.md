---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-25"

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

# Pagina Panoramica

La pagina Panoramica del pannello **Migliora** fornisce un riepilogo delle conversazioni tra gli utenti e il tuo spazio di lavoro. Puoi visualizzare la quantità di traffico per un determinato periodo di tempo, nonché gli intenti e le entità che sono stati riconosciuti più spesso nelle conversazioni degli utenti.
{: shortdesc}

Le statistiche visualizzate nella pagina Panoramica coprono un periodo di tempo più lungo rispetto al periodo in cui vengono conservati i log delle conversazioni utente. Queste statistiche rappresentano il traffico esterno - utenti o chiamate API - che ha interagito con il tuo spazio di lavoro; non includono le interazioni dal riquadro *Provalo* nello strumento.

Puoi utilizzare la pagina Panoramica per rispondere a domande come:

* In quali mesi c'è stato il numero maggiore e minore di conversazioni l'anno scorso?
* Qual è stato il numero medio di conversazioni settimanali durante il primo trimestre?
* Quali intenti sono apparsi più spesso la scorsa settimana?
* Quali valori di entità sono stati riconosciuti più volte durante il mese di settembre?

Per aprire la pagina Panoramica, seleziona **Panoramica** nella barra di navigazione. Se **Panoramica** non è visibile, utilizza il menu ![Menu](images/Menu_16.png) per aprire la pagina.

  ![Pagina Panoramica](images/oview.png)

La parte superiore della pagina include i seguenti controlli:

* *Aggiorna dati* - Ti consente di aggiornare immediatamente le statistiche della pagina Panoramica. La pagina Panoramica mostra quando sono stati aggiornati l'ultima volta i dati visualizzati. Puoi selezionare **Aggiorna dati** se pensi che potrebbero essere disponibili dati più recenti.
* Controllo del periodo di tempo - Utilizza questo controllo per scegliere il periodo per il quale vengono visualizzati i dati.  Questo controllo riguarda tutti i dati visualizzati nella pagina: non solo il numero di conversazioni visualizzate nel grafico, ma anche le statistiche visualizzate insieme al grafico e gli elenchi di intenti ed entità principali.

  ![Controllo del periodo di tempo](images/oview-time.png)

Puoi scegliere se visualizzare i dati per un singolo giorno, una settimana, un mese, un trimestre o un anno.  In ogni caso, i punti dati sul grafico si adattano ad un periodo di misurazione appropriato.  Ad esempio, quando si visualizza un grafico per un giorno, i dati vengono presentati in valori orari, ma quando si visualizza un grafico per una settimana, i dati vengono visualizzati per giorno.  Una settimana va sempre da domenica a sabato.  Non puoi creare periodi di tempo personalizzati, ad esempio una settimana che va dal giovedì al mercoledì successivo o un mese che inizia in un giorno diverso dal primo.

## Tutte le conversazioni

Un grafico visualizza il numero totale di conversazioni per l'intervallo di date selezionato.

**Nota**: per "conversazione" si intende qualsiasi interazione con lo spazio di lavoro, quindi se ci sono conversazioni in cui il servizio ha detto `Ciao, come posso aiutarla?`, e l'utente chiude il browser senza risposta, tale conversazione verrà inclusa nel numero totale di conversazioni.

Puoi selezionare **Visualizza log** per aprire la pagina [Conversazioni utente](logs_convo.html), con l'intervallo di date filtrato per corrispondere al periodo di tempo selezionato per la pagina Panoramica. La pagina [Conversazioni utente](logs_convo.html) visualizza il numero totale di *espressioni*. Un'espressione è un singolo messaggio che l'utente invia allo spazio di lavoro. Ogni conversazione può essere composta da più espressioni. Pertanto, il numero di risultati nella pagina [Conversazioni utente](logs_convo.html) è diverso dal numero di conversazioni mostrato nella pagina Panoramica.

**Nota**: a seconda del tuo piano e dell'intervallo di date che hai selezionato, potresti non visualizzare alcun dato. Ad esempio, il {{site.data.keyword.conversationshort}} [piano di servizio Standard](logs_convo.html#log-limits) conserva le conversazioni solo per 30 giorni; se scegli un intervallo di date superiore a 30 giorni, non vedrai alcun dato. 

Durante la visualizzazione del grafico, puoi fare clic su un singolo punto dati per visualizzare il valore numerico, come mostrato qui:

![Singolo punto dati](images/oview-point.png)

Sotto il grafico, vengono mostrate le statistiche relative ai dati visualizzati:

* *Numero totale di conversazioni* - Il numero totale di conversazioni avvenute durante questo periodo di tempo
* *Numero massimo di conversazioni* - Il numero massimo di conversazioni per un singolo punto dati entro il periodo di tempo
* *Scarsa comprensione* - Il numero di singole conversazioni con scarsa comprensione. Queste espressioni non sono classificate da un intento e non contengono alcuna entità nota. Queste possono essere utili per identificare potenziali problemi di dialogo.

## Intenti principali ed entità principali

Puoi anche visualizzare gli intenti e le entità che sono stati riconosciuti più spesso durante il periodo di tempo specificato. Per impostazione predefinita, vengono visualizzati i primi tre di ciascuno, ma puoi modificare il valore in un numero più grande, ad esempio 5 o 10.

* *Intenti principali* - Gli intenti sono mostrati in un elenco semplice.  Oltre a vedere il numero di volte in cui è stato riconosciuto un intento, puoi utilizzare il link **Visualizza log** per aprire la pagina Conversazioni utente con l'intervallo di date filtrato per corrispondere ai dati che stai visualizzando e l'intento filtrato per corrispondere all'intento selezionato.

* Le *Entità principali* sono mostrate in un grafico a barre. Per ogni entità puoi selezionare la barra per vedere il numero rappresentato dalla barra.

  ![Fumetto dati di entità](images/oview-entity.png)

  Seleziona **Mostra valori principali** per visualizzare un elenco dei valori più comuni identificati per questa entità durante il periodo di tempo. Seleziona **Visualizza log** per aprire la pagina [Conversazioni utente](logs_convo.html) con l'intervallo di date filtrato per corrispondere ai dati che stai visualizzando e l'entità filtrata per corrispondere all'entità selezionata.
