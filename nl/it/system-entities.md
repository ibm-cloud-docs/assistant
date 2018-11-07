---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-24"

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

# Dettagli delle entità di sistema

Questa sezione di riferimento fornisce informazioni complete sulle entità di sistema disponibili. Per ulteriori informazioni sulle entità di sistema e su come utilizzarle, consulta [Definizione di entità](entities.html#enable_system_entities) e cerca "Abilitazione delle entità di sistema".
{: shortdesc}

Le entità di sistema sono disponibili per le lingue indicate nell'argomento [Lingue supportate](lang-support.html).

## Entità @sys-currency
{: #sys-currency}

L'entità di sistema @sys-currency rileva i valori monetari espressi in un'espressione con un simbolo di valuta o termini specifici della valuta. Viene restituito un valore numerico.

### Formati riconosciuti

- 20 centesimi
- Cinque dollari
- $10

### Metadati

- `.numeric_value`: il valore numerico canonico come un numero intero o un doppio, in unità di base
- `.unit`: il codice valuta dell'unità di base (ad esempio, 'USD' o 'EUR')

### Restituzioni

Per l'input `twenty dollars` o `$1,234.56`, @sys-currency restituisce questi valori:

| Attributo                   | Tipo   | Valore restituito per `twenty dollars` | Valore restituito per `$1,234.56` |
|-----------------------------|--------|-------------------------------|-------------------------:|
| @sys-currency               | stringa | 20                            |                  1234.56 |
| @sys-currency.literal       | stringa | twenty dollars                |                $1,234.56 |
| @sys-currency.numeric_value | numero | 20                            |                  1234.56 |
| @sys-currency.location      | array  | [0,14]                        |                    [0,9] |
| @sys-currency.unit          | stringa | USD*                          |                      USD |

*@sys-currency.unit restituisce sempre il codice di valuta ISO di 3 lettere.

Per l'input `veinte euro` o <code>&euro;1.234,56</code>, in spagnolo, @sys-currency restituisce questi valori:

| Attributo                   | Tipo   | Valore restituito per  `veinte euro` | Valore restituito per <code>&euro;1.234,56</code> |
|-----------------------------|--------|-----------------------------|-------------------------:|
| @sys-currency               | stringa | 20                          |                  1234.56 |
| @sys-currency.literal       | stringa | veinte euro                 |                &euro;1.234,56 |
| @sys-currency.numeric_value | numero | 20                          |                  1234.56 |
| @sys-currency.location      | array  |[0,11]                       |                     [0,9]|
| @sys-currency.unit          | stringa | EUR*                        |                     EUR  |
*@sys-currency.unit restituisce sempre il codice di valuta ISO di 3 lettere.

Ottieni risultati equivalenti per altre lingue supportate e valute nazionali.

### Suggerimenti sull'utilizzo di @system-currency

- I valori di valuta vengono riconosciuti anche come istanze delle entità @sys-number. Se utilizzi condizioni separate per controllare sia i valori di valuta che i numeri, inserisci la condizione che controlla la valuta sopra quella che controlla un numero.

- Se utilizzi l'entità @sys-currency come condizione del nodo e l'utente specifica `$0` come valore, il valore viene riconosciuto correttamente come valuta, ma la condizione viene valutata sul numero zero, non sulla valuta zero. Di conseguenza, non restituisce la risposta prevista. Per controllare i valori di valuta in modo da gestire correttamente gli zeri, nella condizione del nodo utilizza la sintassi `entities['sys-currency']`.

## Entità @sys-date e @sys-time
{: #sys-datetime}

L'entità di sistema `@sys-date` estrae citazioni come `Friday`, `today` o `November 1`. Il valore di questa entità memorizza la data desunta corrispondente come una stringa nel formato "aaaa-MM-gg" ad esempio, "2016-11-21". Il sistema aggiunge gli elementi mancanti di una data (come l'anno per "21 novembre") con i valori di data correnti.

**Nota:** - solo per la locale inglese, il comportamento di sistema predefinito per l'input di data è MM/GG/AAAA. Questo diventerà GG/MM/AAAA solo se i primi due numeri sono maggiori di 12. Il valore memorizzato sarà ancora nel formato "aaaa-MM-gg".

L'entità di sistema `@sys-time` estrae citazioni come `2pm`, `at 4` o `15:30`. Il valore di questa entità memorizza l'ora come una stringa nel formato "HH:mm:ss". Ad esempio, "13:00:00."

### Citazioni di data e ora

Le citazioni di data e ora, come `now` o `two hours from now` vengono estratte come due citazioni di entità separate, una `@sys-date` e l'altra `@sys-time`. Queste due citazioni non sono collegate tra loro, tranne per il fatto che condividono la stessa stringa letterale che comprende la citazione data-ora completa.

Le citazioni di data e ora con più parole, come `on Monday at 4pm` vengono anche estratte come due citazioni @sys-date e @sys-time. Se citate insieme consecutivamente, condividono anche una singola stringa letterale che comprende la citazione data-ora completa.

### Intervalli di date e ore

Le citazioni di un intervallo di date, come `the weekend`, `next week` o `from Monday to Friday` vengono estratte come coppia di citazioni di entità `@sys-date` che mostrano l'inizio e la fine dell'intervallo. Allo stesso modo, le citazioni di intervalli di ore, come `from 2 to 3` vengono estratte come due entità `@sys-time`, che mostrano le ore di inizio e di fine. Le due entità nella coppia condividono una stringa letterale che corrisponde alla citazione dell'intero intervallo di date o ore.

### Date e ore `Last` e `Next`

In alcune locali, una frase come "last Monday" viene utilizzata solo per specificare il lunedì della settimana precedente. Al contrario, altre locali utilizzano "last Monday" per specificare l'ultimo giorno che era un lunedì, ma che potrebbe essere stato nella stessa settimana o nella settimana precedente.

Ad esempio, per venerdì 16 giugno, in alcune locali "last Monday" potrebbe riferirsi al 12 giugno o al 5 giugno, mentre in altre locali si riferisce solo al 5 giugno (la settimana precedente). Questa stessa logica vale per una frase come "next Monday".

Il servizio {{site.data.keyword.conversationshort}} considera le date "last" e "next" come il giorno di riferimento precedente o successivo più immediato, che può essere nella stessa settimana o in una settimana precedente. 

Per frasi temporali come "for the last 3 days" o "in the next 4 hours", la logica è equivalente. Ad esempio, nel caso di "in the next 4 hours", questo comporta due entità `@sys-time`: una dell'ora corrente e una delle quattro ore più tardi rispetto all'ora corrente.

### Fusi orari

Le citazioni di una data o un'ora relative all'ora corrente vengono risolte rispetto a un fuso orario selezionato. Per impostazione predefinita, questo è UTC (GMT). Ciò significa che, per impostazione predefinita, i client API REST situati in fusi orari diversi da UTC osserveranno il valore di `now` estratto in base all'ora UTC corrente.

Facoltativamente, il client API REST può aggiungere il fuso orario locale come variabile di contesto `$timezone`. Questa variabile di contesto dovrebbe essere inviata con ogni richiesta del client. Ad esempio, il valore `$timezone` deve essere `America/Los_Angeles`, `EST` o `UTC`. Per un elenco completo di fusi orari supportati, vedi [Fusi orari supportati](supported-timezones.html).

Quando viene fornita la variabile `$timezone`, i valori delle citazioni relative @sys-date e @sys-time sono calcolati in base al fuso orario del client anziché UTC.

#### Esempi di citazioni relative ai fusi orari

- now
- in two hours
- today
- tomorrow
- 2 days from now

### Formati riconosciuti

- November 21
- 10:30
- at 6 pm
- this weekend

### Restituzioni

Per l'input `November 21` @sys-date restituisce questi valori:

| Attributo               | Tipo   | Valore restituito per `November 21` |
|-------------------------|--------|---------------------------:|
| @sys-date.literal       | stringa |                November 21 |
| @sys-date               | stringa |                20xx-11-21 *|
| @sys-date.location      | array |                     [0,11]  |
| @sys-date.calendar_type | stringa |                  GREGORIAN |

- @sys-date restituisce la data sempre in questo formato: aaaa-MM-gg.
- \* Restituisce la data corrispondente successiva. Se tale data è già passata quest'anno, restituisce la data del prossimo anno.

Per l'input `at 6 pm` @sys-time restituisce questi valori:

| Attributo               | Tipo   | Valore restituito per `at 6 pm` |
|-------------------------|--------|-----------------------:|
| @sys-time.literal       | stringa |                at 6 pm |
| @sys-time               | stringa |               18:00:00 |
| @sys-time.location      | array |                   [0,7]|
| @sys-time.calendar_type | stringa |              GREGORIAN |

- @sys-time restituisce l'ora sempre in questo formato: HH:mm:ss.

Per informazioni sull'elaborazione dei valori di data e ora, vedi il riferimento al metodo [Data e ora](dialog-methods.html#date-time).
{: tip}

## Entità @sys-location
{: #sys-location}

**BETA, per le lingue indicate nell'argomento [Lingue supportate](lang-support.html)**: l'entità di sistema @sys-location estrae i nomi dei luoghi (paese, stato/provincia, città, ecc.) dall'input dell'utente. Il valore dell'entità non è un valore standard di sistema della posizione.

### Formati riconosciuti

- Boston
- U.S.A.
- New South Wales

Per informazioni sull'elaborazione dei valori Stringa, vedi il riferimento al metodo [Stringhe](dialog-methods.html#strings).
{: tip}

## Entità @sys-number
{: #sys-number}

L'entità di sistema @sys-number rileva i numeri scritti usando valori numerali o parole. In entrambi i casi, viene restituito un valore numerico.

### Formati riconosciuti

- 21
- twenty one
- 3.13

### Metadati

- `.numeric_value` - il valore numerico canonico come un numero intero o un doppio

### Restituzioni

Per l'input `twenty` o `1,234.56`, @sys-number restituisce questi valori:

| Attributo                   | Tipo   | Valore restituito per `twenty` | Valore restituito per `1,234.56` |
|-----------------------------|--------|-------------------|------------------------:|
| @sys-number               | stringa | 20                |                 1234.56 |
| @sys-number.literal       | stringa | twenty            |                1,234.56 |
| @sys-number.location      | array |  [0,6]             |                    [0,8]|
| @sys-number.numeric_value | numero | 20                |                 1234.56 |

Per l'input `veinte` o `1.234,56`, in spagnolo, @sys-number restituisce questi valori:

| Attributo                   | Tipo   | Valore restituito per `veinte` | Valore restituito per `1.234,56` |
|-----------------------------|--------|-----------------------|------------------------:|
| @sys-number               | stringa | 20                    |                 1234.56 |
| @sys-number.literal       | stringa | veinte                |                1.234,56 |
| @sys-number.location      | array  | [0,6]                 |                   [0,8] |
| @sys-number.numeric_value | numero | 20                    |                 1234.56 |

Ottieni risultati equivalenti per le altre lingue supportate.

### Suggerimenti sull'utilizzo di @system-number

- Se utilizzi l'entità @sys-number come condizione del nodo e l'utente specifica zero come valore, il valore 0 viene riconosciuto correttamente come numero, ma la condizione viene valutata su false e non può restituire correttamente la risposta associata. Per controllare i numeri in modo da gestire correttamente gli zeri, nella condizione del nodo utilizza la sintassi `entities['sys-number']`.

- Se utilizzi @sys-number per confrontare i valori di numero in una condizione, assicurati di includere separatamente un controllo per la presenza di un numero stesso. Se non viene trovato alcun numero, @sys-number viene valutato come null, il che potrebbe comportare che il confronto venga valutato come true anche se non è presente alcun numero.

  Ad esempio, non utilizzare `@sys-number<4` da solo perché, se non viene trovato alcun numero, `@sys-number` viene valutato come null. Poiché null è inferiore a 4, la condizione viene valutata come true anche se non è presente un numero.

  Utilizza invece `@sys-number AND @sys-number<4`. Se non è presente alcun numero, la prima condizione viene valutata come false, il che porta a valutare correttamente l'intera condizione come false.

Per informazioni sull'elaborazione dei valori di numero, vedi il riferimento al metodo [Numeri](dialog-methods.html#numbers).
{: tip}

## Entità @sys-percentage
{: #sys-percentage}

L'entità di sistema @sys-percentage rileva le percentuali indicate in un'espressione con il simbolo della percentuale o scritte usando la parola `percent`. In entrambi i casi, viene restituito un valore numerico.

### Formati riconosciuti

- 15%
- 10 percent

### Metadati

`.numeric_value`: il valore numerico canonico come un numero intero o un doppio

### Restituzioni

Per l'input `1,234.56%`, @sys-percentage restituisce questi valori:

| Attributo                     | Tipo   | Valore restituito per `1,234.56%` |
|-------------------------------|--------|-------------------------:|
| @sys-percentage               | stringa |                  1234.56 |
| @sys-percentage.literal       | stringa |                1,234.56% |
| @sys-percentage.location      | array  |                    [0,9] |
| @sys-percentage.numeric_value | numero |                  1234.56 |

Per l'input `1.234,56%`, in spagnolo, @sys-currency restituisce questi valori:

| Attributo                     | Tipo   | Valore restituito per `1.234,56%` |
|-------------------------------|--------|-------------------------:|
| @sys-percentage               | stringa |                  1234.56 |
| @sys-percentage.literal       | stringa |                1.234,56% |
| @sys-percentage.location      | array  |                    [0,9] |
| @sys-percentage.numeric_value | numero |                  1234.56 |

Ottieni risultati equivalenti per le altre lingue supportate.

### Suggerimenti sull'utilizzo di @system-percentage

- I valori di percentuale vengono riconosciuti anche come istanze delle entità @sys-number. Se utilizzi condizioni separate per controllare sia i valori di percentuale che i numeri, inserisci la condizione che controlla una percentuale sopra quella che controlla un numero.

- Se utilizzi l'entità @sys-percentage come condizione del nodo e l'utente specifica `0%`  come valore, il valore viene riconosciuto correttamente come percentuale, ma la condizione viene valutata sul numero zero, non sulla percentuale 0%. Pertanto, non restituisce la risposta prevista. Per controllare le percentuali in modo da gestire correttamente le percentuali di zero, nella condizione del nodo utilizza la sintassi `entities['sys-percentage']`.

- Se immetti un valore come `1-2%`, i valori `1%` e `2%` vengono restituiti come entità di sistema. L'indice sarà l'intero intervallo tra 1% e 2%, ed entrambe le entità avranno lo stesso indice.

## Entità @sys-person
{: #sys-person}

**BETA, per le lingue indicate nell'argomento [Lingue supportate](lang-support.html)**: l'entità di sistema @sys-person estrae i nomi dall'input dell'utente. I nomi vengono riconosciuti individualmente, pertanto "Will" non è considerato come "William" o viceversa. Il valore dell'entità non è un valore standard di sistema del nome.

### Formati riconosciuti

- Will
- Jane Doe
- Vijay

Per informazioni sull'elaborazione dei valori Stringa, vedi il riferimento al metodo [Stringhe](dialog-methods.html#strings).
{: tip}
