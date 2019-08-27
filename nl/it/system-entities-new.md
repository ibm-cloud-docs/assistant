---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-17"

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
{:gif: data-image-type='gif'}

# Nuove entità di sistema ![Beta](images/beta.png)
{: #beta-system-entities}

Abilita le nuove entità di sistema per avvalerti dei miglioramenti apportati alle entità di sistema basate sui numeri fornite da IBM.

Attualmente, questa impostazione può essere abilitata solo per le capacità di dialogo scritte in inglese o tedesco.
{: note}

Le nuove entità di sistema possono riconoscere citazione con più sfumature nell'input utente. Ad esempio, `@sys-date` può calcolare la data di una festività nazionale, come `Thanksgiving`, quando viene citata per nome. E `@sys-date` può riconoscere quando un anno viene specificato come parte di una data citata nell'input dell'utente. I miglioramenti rendono inoltre più facile per il tuo assistente distinguere tra le molte entità di sistema basate sui numeri. Ad esempio, una citazione di data, come `May 10`, riconosciuta come `@sys-date` non viene identificata anche come una citazione `@sys-number`.

Le entità di sistema `@sys-location` e `@sys-person` non sono state modificate.
{: note}

## Abilitazione delle nuove entità di sistema
{: #beta-system-entities-enable}

Per abilitare le nuove entità di sistema, completa i seguenti passi: 

1.  Dalla pagina Skills, apri la tua capacità. 
1.  Fai clic sulla scheda **Options**.
1.  Fai clic su **System Entities** e scegli **Try beta**.
1.  Fai clic sulla scheda **Entities** e poi su **System entities**.
1.  Attiva l'entità di sistema che vuoi utilizzare nel tuo dialogo.

Verifica le nuove entità di sistema aggiungendone una o più alle condizioni del nodo di dialogo o nella condizione delle risposte condizionali di un nodo di dialogo. Successivamente, dal riquadro "Try it out", invia le espressioni dell'utente che attivano i nodi che hai aggiunto.

Se decidi che preferisci il comportamento della versione precedente delle entità di sistema, puoi smettere di utilizzare la versione beta in qualsiasi momento. Ritorna alla pagina **System entities** sulla scheda **Options** e scegli **Keep existing**. Dai a Watson il tempo necessario a recuperarle.

Se decidi di continuare ad utilizzare la versione beta, controlla tutti i nodi di dialogo esistenti che generano delle condizioni o restituiscono i valori delle entità di sistema per determinare se devi apportare delle modifiche. Ad esempio, alcune citazioni sono state precedentemente classificare come più di un tipo di entità di sistema. Ora ad esempio, se viene citata una valuta, viene classificata solo come `@sys-currency`. Potresti essere in grado di semplificare la logica utilizzata prima della soluzione temporanea del comportamento. Oppure potresti aver bisogno di rivedere la logica aggiunta su cui si basa il precedente comportamento.

### Nuove proprietà delle entità di sistema
{: #beta-system-entities-props}

Per migliorare le entità di sistema, sono state aggiunte delle nuove proprietà agli oggetti di entità delle entità di sistema basate sui numeri.

La seguente tabella riepiloga le nuove proprietà che sono state aggiunte. Per vedere le proprietà associate alle entità di sistema correnti, vedi [Dettagli delle entità di sistema](/docs/services/assistant?topic=assistant-system-entities).

Tabella 1. Nuove proprietà delle entità di sistema

| Entità di sistema | Proprietà | Descrizione |
|---------------|----------|-------------|
| `@sys-date` | `alternatives` | Acquisisce le date diverse da quella salvata come valore `@sys-date` che l'utente potrebbe aver avuto intenzione di dire quando la data non è chiaramente indicata. Ad esempio, se la data di oggi è 10 March 2019 e l'utente immette `on March 1`, la data del primo marzo dell'anno successivo viene salvata come `@sys-date` (`2020-03-01`). Tuttavia, l'utente potrebbe aver voluto dire il primo marzo del 2019. Il sistema salva anche la data di quest'anno (`2019-03-01`) come un valore di data alternativo. I valori alternativi vengono salvati come un oggetto che contiene un valore e l'affidabilità di ogni data alternativa e viene archiviato in un array JSON. Per richiamare un valore alternativo, utilizza la sintassi: `@sys-date.alternative`. Nella maggior parte dei casi, la data futura viene salvata come valore `@sys-date` e la data passata come alternativa. Questo è vero per i giorni della settimana quando ne viene specificato uno senza preposizione ad esempio `on` o un aggettivo come `this` o `next`. Ad esempio, `Are you open Monday?`. Tuttavia, se un utente specifica un giorno della settimana come parte di una frase come `on Monday` o `next Monday`, si assume che la data sia una data futura e non viene creata alcuna data alternativa per essa. Ad esempio, se oggi è martedì e un utente immette, `this Tuesday` o `next Tuesday`, `@sys-date` viene impostata sulla data del martedì della settimana successiva e non vengono create delle date alternative. |
| `@sys-date` | `datetime_link` | Se presente, indica che l'input dell'utente menziona insieme una data e ora, la qual cosa implica che la data e l'ora siano correlate tra loro. I valori di indice di inizio e fine della stringa che include la data e l'ora vengono salvati come parte del nome del link. Ad esempio, se l'input è `Are you open today at 5?`, `today` viene riconosciuto come un riferimento di data alla posizione `[13,18]` e `at 5` viene riconosciuto come un riferimento di tempo alla posizione `[19,23]`. Un valore di posizione di `[13,23]`, che si estende ad entrambe le citazioni di data e ora, viene accodato al datetime_link risultante. Viene denominato `datetime_link_13_23` e viene incluso nell'output delle entità di sistema `@sys-date` e `@sys-time` che partecipano alla relazione. |
| `@sys-date` | `festival` | Riconosce i nomi delle festività specifiche nella locale e può restituire la data di una festività imminente, ad esempio `Thanksgiving`, `Christmas` e `Halloween`. Utilizza tutte lettere minuscole se devi specificare un nome di festività in una condizione, ad esempio (`@sys-date.festival == 'thanksgiving'`). |
| `@sys-date` | `granularity` | Riconosce le citazioni di intervalli di tempo, ad esempio `next year` (=`year`). Le opzioni sono `day`, `weekend`, `week`, `fortnight`, `month`, `quarter` e `year`. |
| `@sys-date` | `interpretation` | L'oggetto restituito dal tuo assistente che contiene dei nuovi campi che aumentano la precisione del classificatore di entità di sistema. Puoi omettere `interpretation` dall'espressione che utilizzi per fare riferimento ai relativi campi. Ad esempio, puoi accedere al valore del campo `festival` nell'oggetto interpretation utilizzando la sintassi abbreviata `<? @sys-date.festival ?>`. |
| `@sys-date` | `range_link` | Se presente, indica che l'input dell'utente contiene della sintassi che suggerisce che è stato specificato un intervallo di date. Ad esempio, l'input potrebbe essere `I will travel from March 15 to March 22`. Ognuna delle due entità di sistema `@sys-date` che viene rilevata ha una proprietà `range_link` nel suo output. Vengono fornite ulteriori informazioni, incluso il ruolo che ciascuna `@sys-date` ricopre nella relazione di intervallo. Ad esempio, la data di inizio ha un tipo di ruolo di `date_from` mentre la data di fine `date_to`. Per controllare un valore di ruolo, puoi utilizzare la sintassi `@sys-date.role?.type == 'date_from'`. Se l'input utente implica un intervallo, ma viene specificata solo una data, la proprietà `range_link` non viene restituita, ma viene restituito un tipo di ruolo. Ad esempio, se l'utente chiede, `Have you been open since yesterday`. La data yesterday viene riconosciuta come la citazione `@sys-date` e viene restituita per essa un tipo di ruolo `date_from`. Viene restituito anche un `range_modifier` che identifica la parola nell'input che attiva l'identificazione di un intervallo. In questo esempio, il modificatore è `since`. |
| `@sys-date` | `relative_year`, `relative_month`, `relative_week`, `relative_weekend`, `relative_day` | Riconosce e acquisisce le citazioni di valori di data relativi nell'input dell'utente, ad esempio `two weeks ago` (`relative_week = -2`) o `this weekend (relative_weekend = 0)` o `today` (`relative_day = 0`). |
| `@sys-date` | `specific_year`, `specific_quarter`, `specific_month`, `specific_day`, `specific_day_of_week` | Riconosce e acquisisce le citazioni di valori di data specifici nell'input dell'utente, ad esempio `2020` (`specific_year = 2020`), `Q2` (`specific_quarter = 2`), `March`  (`specific_month = 3`), `Monday` (`specific_day_of_week = monday`) o `3rd` (`specific_day = 3`). |
| `@sys-number` | `interpretation` | L'oggetto restituito dal tuo assistente che contiene dei nuovi campi che aumentano la precisione del classificatore di entità di sistema. Puoi omettere `interpretation` dall'espressione che utilizzi per fare riferimento ai relativi campi. Ad esempio, puoi accedere al valore del campo `range_link` nell'oggetto interpretation utilizzando la sintassi abbreviata `<? @sys-date.range_link ?>`. |
| `@sys-number` | `range_link` | Se presente, indica che l'input dell'utente contiene della sintassi che suggerisce che è stato specificato un intervallo di numeri. Ad esempio, l'input potrebbe essere `I'm interested in 5 to 7.`. Ognuna delle due entità di sistema `@sys-number` che viene rilevata ha una proprietà `range_link` nel loro output. Vengono fornite ulteriori informazioni, incluso il ruolo che ciascuna `@sys-number` ricopre nella relazione di intervallo. Ad esempio, il numero iniziale ha un tipo di ruolo di `number_from` mentre il numero finale `number_to`. Per controllare un valore di ruolo, puoi utilizzare la sintassi `@sys-number.role?.type == 'number_from'`. |
| `@sys-time` | `alternatives` | Acquisisce le ore diverse da quella salvata come valore `@sys-time` che l'utente potrebbe aver avuto intenzione di dire quando l'ora non è chiaramente indicata. Ad esempio, quando un utente dice `The meeting is at 5`, il sistema calcola l'ora come 5:00:00 o 5 AM e la salva come `@sys-time`. Tuttavia, l'utente potrebbe aver voluto dire 17:00:00 o 5 PM. Il sistema salva anche la data successiva come un valore di ora alternativo. Se l'utente immette `The meeting is at 5pm`, `@sys-time` viene impostato su 17:00:00 e non viene creato alcun valore alternativo perché non c'è confusione tra AM e PM. I valori alternativi vengono salvati come un oggetto che contiene un valore e l'affidabilità di ogni ora alternativa e viene archiviato in un array JSON. Per richiamare un valore alternativo, utilizza la sintassi: `@sys-time.alternative`. |
| `sys-time` | `datetime_link` | Se presente, indica che l'input dell'utente menziona insieme una data e ora, la qual cosa implica che la data e l'ora siano correlate tra loro. I valori di indice di inizio e fine della stringa che include la data e l'ora vengono salvati come parte del nome del link. Ad esempio, se l'input è `Are you open today at 5?`, `today` viene riconosciuto come un riferimento di data alla posizione `[13,18]` e `at 5` viene riconosciuto come un riferimento di tempo alla posizione `[19,23]`. Un valore di posizione di `[13,23]`, che si estende ad entrambe le citazioni di data e ora, viene accodato al datetime_link risultante. Viene denominato `datetime_link_13_23` e viene incluso nell'output delle entità di sistema `@sys-date` e `@sys-time` che partecipano alla relazione. |
| `@sys-time` | `granularity` | Riconosce le citazioni di intervalli di tempo, ad esempio `now` (=`instant`) o `3 o'clock` o `noon` (=`hour`) o `17:00:00` (=`second`). Le opzioni sono `hour`, `minute`, `second` e `instant`. |
| `@sys-time` | `interpretation` | L'oggetto restituito dal tuo assistente che contiene dei nuovi campi che aumentano la precisione del classificatore di entità di sistema. Puoi omettere `interpretation` dall'espressione che utilizzi per fare riferimento ai relativi campi. Ad esempio, puoi accedere al valore del campo `granularity` nell'oggetto interpretation utilizzando la sintassi abbreviata `<? @sys-time.granularity ?>`. |
| `@sys-time` | `part_of_day` | Riconosce i termini che rappresentano l'ora del giorno, ad esempio `morning`, `afternoon`, `evening`, `night` o `now`. Inoltre imposta le ore arbitrarie, ad esempio `9:00:00` per morning, `15:00:00` per afternoon, `18:00:00` per evening e `22:00:00` per night. |
| `@sys-time` | `range_link` | Se presente, indica che l'input dell'utente contiene della sintassi che suggerisce che è stato specificato un intervallo di ore. Ad esempio, l'input potrebbe essere `Are you open from 9AM to 11AM`. Ognuna delle due entità di sistema `@sys-time` che viene rilevata ha una proprietà `range_link` nel suo output. Vengono fornite ulteriori informazioni, incluso il ruolo che ciascuna `@sys-time` ricopre nella relazione di intervallo. Ad esempio, l'ora di inizio ha un tipo di ruolo di `time_from` mentre l'ora di fine `time_to`. Per controllare un valore di ruolo, puoi utilizzare la sintassi `@sys-time.role?.type == 'time_from'`. Se l'input utente implica un intervallo, ma viene specificata solo un'ora, la proprietà `range_link` non viene restituita, ma viene restituito un tipo di ruolo. Ad esempio, se l'utente chiede, `Are you open until 9PM`, 9PM viene riconosciuta come la citazione `@sys-time` e viene restituita per essa un tipo di ruolo `time_to`. Viene restituito anche un `range_modifier` che identifica la parola nell'input che attiva l'identificazione di un intervallo. In questo esempio, il modificatore è `until`. |
| `@sys-time` | `relative_hour`, `relative_minute`, `relative_second` | Riconosce le citazioni relative di ora, ad esempio `5 hours ago` (`relative_hour = -5`),`in two minutes`(`relative_minute = 2`) o `in a second` (`relative_second = 1`). |
| `@sys-time` | `specific_hour`, `specific_minute`, `specific_second` | Riconosce le citazioni specifiche di ora, ad esempio `at 5 o'clock` (`specific_hour = 5`), `at 2:30`(`specific_minute = 30`) o `23:30:22` (`specific_second = 22`). |
{: caption="Nuove proprietà delle entità di sistema" caption-side="top"}

I valori nella seguente tabella non sono proprietà dell'oggetto di entità. Sono valori helper che puoi utilizzare nel prodotto per estrarre velocemente i dettagli di data o ora dalle nuove entità di sistema.

Tabella 2. Funzioni helper delle entità di sistema

| Entità di sistema | Valore helper | Descrizione |
|---------------|----------|-------------|
| `@sys-date` | `day` | Restituisce il giorno specificato nella data come un valore numerico. Ad esempio, se la data è `5 December 2019`, restituisce `5`. |
| `@sys-date` | `day_of_week` | Valuta la data per determinare il giorno della settimana. Archivia il giorno della settimana in minuscolo. Ad esempio, `Sunday` viene archiviato come `sunday`. Utilizza una lettera iniziale minuscola per controllare un nome di giorno della settimana in una condizione. Ad esempio: `@sys-date.day_of_week == 'sunday'` |
| `@sys-date` | `month` | Restituisce il mese specificato nella data come un valore numerico. Ad esempio, se la data è `5 December 2019`, restituisce `12`. |
| `@sys-date` | `year` | Restituisce l'anno specificato nella data come un valore numerico. Ad esempio, se la data è `5 December 2019`, restituisce `2019`. |
| `@sys-time` | `hour` | Restituisce l'ora specificata nel tempo come un valore numerico. Ad esempio, se il tempo è `5:30:10 PM`, restituisce `17` per rappresentare 5 PM. |
| `@sys-time` | `minute` | Restituisce il valore di minuto che viene specificato nell'ora. `30`. Se non sono specificati dei minuti, questo helper restituisce `0`.|
| `@sys-time` | `second` | Restituisce il valore di secondo che viene specificato nell'ora. `10`. Se non sono specificati dei secondi, questo helper restituisce `0`.|
{: caption="Funzioni helper delle entità di sistema" caption-side="top"}

### Esempi di utilizzo
{: #beta-system-entities-examples}

Puoi utilizzare i seguenti tipi di espressioni per avvalerti delle nuove proprietà nelle entità di sistema migliorate. La tabella mostra i risultati restituiti quando un utente menziona la data `4 July 2019` e l'ora `3:30 PM`. Puoi utilizzare queste espressioni nelle risposte di testo del dialogo senza inserirle nella sintassi `<? ?>`.

Tabella 3. Utilizzo delle nuove entità di sistema per ottenere i dettagli di data e ora

| Sintassi espressione SpEL |Risultato|
|------------------------|---------|
| `@sys-date.year` | `2019` |
| `@sys-date.month` | `7` |
| `@sys-date.day` | `4` |
| `@sys-date.day_of_week` | `thursday` |
| `@sys-time.hour` | `15` |
| `@sys-time.minute` | `30` |
| `@sys-time.second` | `0` |
{: caption="Utilizza le nuove entità di sistema per ottenere i dettagli di data e ora" caption-side="top"}

In un nodo che genera condizioni sull'intento `#Customer_Care_Store_Hours`, puoi aggiungere ulteriori risposte condizionali che utilizzano le nuove proprietà dell'entità di sistema per fornire risposte leggermente diverse sugli orari del negozio a seconda di cosa chiede l'utente. 

Probabilmente non desideri utilizzare tutte queste risposte condizionali in un dialogo reale; sono qui descritte semplicemente per illustrare le varie possibilità.
{: tip}

Tabella 4. Utilizzo delle nuove entità di sistema nelle risposte condizionali.

| Sintassi condizione risposta condizionale | Descrizione | Testo risposta di esempio |
|--------------------------------|-------------|----------|
| `@sys-date.festival == 'thanksgiving'` | Controlla se il cliente sta chiedendo informazioni sugli orari del particolare giorno Thanksgiving. | We give out free meals to those in need on Thanksgiving day. |
| `@sys-date.festival` | Controlla se il cliente sta chiedendo informazioni sugli orari di un'altra festività specifica. | We are closed on Christmas Day, July 4th, and President's Day. On other holidays, we are open from 10AM to 5PM. |
| `@sys-time.part_of_day == 'night'` | Controlla se l'utente include un qualsiasi termine che menziona night come ora del giorno nell'input. | We are not open late; we close at 9PM most days. |
| `@sys-date.datetime_link && input.text.contains('today') && now().sameOrAfter(@sys-time)` | Controlla se l'input contiene una frase come ad esempio `today at 8`. Controlla anche se il valore per `@sys-time` rilevato è precedente all'ora del giorno attuale. Puoi aggiungere una variabile di contesto alla risposta condizionale che crea una variabile `$new_time` con il valore `<? @sys-time.plusHours(12) ?>`. Puoi utilizzare questo approccio per creare una variabile di data che acquisisce l'ora più probabile intesa da un utente che, a mezzogiorno chiede, `Are you open today at 8`. Il sistema assume che l'utente intende 8AM invece di 8PM. | You mean today at $new_time, right? |
| `@sys-time.range_link && entities[0].role?.type == 'time_from' && entities[1].role?.type == 'time_to'` | Controlla se l'input contiene un intervallo di ore. La condizione si assicura inoltre che la prima entità di sistema `@sys-time` elencata nell'array di entità sia la data di inizio e la seconda sia la data di fine prima di includerle nella risposta. | You want to know if we're open between `<? entities[0] ?>` and `<? entities[1] ?>`, is that right? |
| `@sys-time && entities.size() < 2 && entities[0].role?.type == 'time_to'` | Controlla se l'input contiene una citazione `@sys-time` che ha un tipo di ruolo `time_to`. Se l'input utente corrisponde a questa condizione, suggerisce che l'utente ha specificato l'ora di fine di un intervallo di tempo aperto. Ad esempio, la risposta sarà attivata dall'input, `Are you open until 9pm?`. Questo input corrisponde perché identifica solo un'ora in una suddivisione di tempo e la citazione ha un ruolo `time_to`. | Do you mean from now (`<? now().reformatDateTime('h:mm a') ?>`) until `<? @sys-time.reformatDateTime('h:mm a') ?>`? |
| `@sys-date.day_of_week == 'sunday'` | Controlla se una data specifica richiesta dall'utente cade di domenica. | We are closed on Sundays. |
| `@sys-date.specific_day_of_week == 'monday' && @sys-date.alternative` | Controlla se l'utente ha menzionato il giorno della settimana `Monday` nella sua domanda. Ad esempio, `Are you open Monday`. La condizione controlla anche se sono state archiviate delle date alternative. Le date alternative vengono create quando il tuo assistente non è completamente sicuro di quale lunedì l'utente stia parlando, per cui archivia anche le date dei lunedì alternativi. Quando un utente specifica un giorno della settimana, il tuo assistente assume che l'utente intenda la ricorrenza futura del giorno (il lunedì successivo). Puoi aggiungere una risposta che ricontrolli la data intesa, tramite il valore alternativo rilevato. In questo caso, la data alternativa è la data del lunedì precedente. | Do you mean `@sys-date` or `@sys-date.alternative`? |
| `true` | Risponde a tutte le altre richieste per informazioni sugli orari del negozio. | We're open from 9AM to 9PM Monday through Saturday. |
{: caption="Utilizza le nuove entità di sistema nelle risposte condizionali." caption-side="top"}
