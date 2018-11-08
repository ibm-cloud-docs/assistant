---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-16"

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

# Espressioni per l'accesso agli oggetti

Puoi scrivere le espressioni che accedono agli oggetti e alle proprietà utilizzando il linguaggio Spring Expression (SpEL). Per ulteriori informazioni, vedi [Spring Expression Language (SpEL) ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}.
{: shortdesc}

## Sintassi della valutazione

Per espandere i valori delle variabili all'interno di altre variabili oppure per richiamare i metodi sulle proprietà e gli oggetti globali, utilizza la sintassi dell'espressione `<? expression ?>`. Ad esempio:

- **Espansione di una proprietà**
    - `"output":{"text":"Your name is <? context.userName ?>"}`

- **Richiamo dei metodi sulle proprietà degli oggetti globali**
    - `"context":{"email": "<? @email.literal ?>"}`

## Sintassi abbreviata
{: #shorthand-syntax}

Apprendi come fare velocemente riferimento ai seguenti oggetti utilizzando la sintassi abbreviata SpEL:

- [Variabili di contesto](expression-language.html#shorthand-context)
- [Entità](expression-language.html#shorthand-entities)
- [Intenti](expression-language.html#shorthand-intents)

### Sintassi abbreviata per le variabili di contesto
{: #shorthand-context}

La seguente tabella mostra esempi di sintassi abbreviata che puoi utilizzare per scrivere variabili di contesto nelle espressioni di condizione.

| Sintassi abbreviata           | Sintassi completa in SpEL                     |
|----------------------------|-----------------------------------------|
| `$card_type`               | `context['card_type']`                  |
| `$(card-type)`             | `context['card-type']`                  |
| `$card_type:VISA`          | `context['card_type'] == 'VISA'`        |
| `$card_type:(MASTER CARD)` | `context['card_type'] == 'MASTER CARD'` |

Nei nomi delle variabili di contesto puoi includere caratteri speciali come trattini o punti. Tuttavia, ciò può causare problemi quando viene valutata l'espressione SpEL. Ad esempio, il trattino potrebbe essere interpretato come un segno meno. Per evitare tali problemi, fai riferimento alla variabile utilizzando la sintassi completa dell'espressione o la sintassi abbreviata `$(variable-name)` e non utilizzare i seguenti caratteri speciali nel nome:

- Parentesi ()
- Più di un apostrofo ''
- Virgolette "

### Sintassi abbreviata per le entità
{: #shorthand-entities}

La seguente tabella mostra esempi di sintassi abbreviata che puoi utilizzare per fare riferimento alle entità.

| Sintassi abbreviata    | Sintassi completa in SpEL                      |
|---------------------|------------------------------------------|
| `@year`             | `entities['year']?.value`                |
| `@year == 2016`     | `entities['year']?.value == 2016`        |
| `@year != 2016`     | `entities['year']?.value != 2016`        |
| `@city == 'Boston'` | `entities['city']?.value == 'Boston'`    |
| `@city:Boston`      | `entities['city']?.contains('Boston')`   |
| `@city:(New York)`  | `entities['city']?.contains('New York')` |

In SpEL, il punto interrogativo `(?)` impedisce di attivare un'eccezione di puntatore vuoto quando un oggetto di entità è null.

Se il valore di entità che vuoi controllare contiene un carattere `)`, non puoi utilizzare l'operatore `:` per il confronto.  Ad esempio, se vuoi controllare se l'entità città è `Dublin (Ohio)`, devi utilizzare `@city == 'Dublin (Ohio)'` invece di `@city:(Dublin (Ohio))`.

### Sintassi abbreviata per gli intenti
{: #shorthand-intents}

La seguente tabella mostra esempi di sintassi abbreviata che puoi utilizzare per fare riferimento agli intenti.

| Sintassi abbreviata        | Sintassi completa in SpEL |
|-------------------------|---------------------|
| `#help`                 | `intent == 'help'`  |
| `! #help`               | `intent != 'help'`  |
| `NOT #help`             | `intent != 'help'`  |
| `#help` o `#i_am_lost` | <code>(intent == 'help' \|\| intent == 'I_am_lost')</code> |

## Variabili globali integrate
{: #builtin-vars}

Puoi utilizzare il linguaggio dell'espressione per estrarre le informazioni sulla proprietà per le seguenti variabili globali: 

| Variabile globale      | Definizione |
|----------------------|------------|
| *context*            | Parte di oggetto JSON del messaggio di conversazione elaborato. |
| *entities[ ]*        | Elenco di entità che supporta l'accesso predefinito al primo elemento. |
| *input*              | Parte di oggetto JSON del messaggio di conversazione elaborato. |
| *intents[ ]*         | Elenco di intenti che supporta l'accesso predefinito al primo elemento. |
| *output*             | Parte di oggetto JSON del messaggio di conversazione elaborato. |

## Accesso alle entità
{: #access-entity}

L'array di entità contiene una o più entità che sono state riconosciute nell'input utente.

Mentre verifichi il tuo dialogo, puoi vedere i dettagli delle entità riconosciute nell'input utente specificando questa espressione in una risposta del nodo di dialogo:

```json
<? entities ?>
```
{: codeblock}

Per l'input utente, *Hello now*, il servizio riconosce le entità di sistema @sys-date e @sys-time, pertanto la risposta contiene questi oggetti di entità:

```json
[
{"entity":"sys-date","location":[6,9],"value":"2017-08-07",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}},
{"entity":"sys-time","location":[6,9],"value":"15:01:00",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}}
]
```
{: codeblock}

### Importanza del posizionamento delle entità nell'input

Utilizza l'espressione completa SpEL se il posizionamento delle entità nell'input è importante. La condizione `entities['city']?.contains('Boston')` restituisce true quando almeno un'entità città 'Boston' viene trovata in tutte le entità @city, indipendentemente dal posizionamento.

Ad esempio, un utente invia `"Voglio andare da Toronto a Boston."` Le entità `@city:Toronto` e `@city:Boston` vengono rilevate e sono rappresentate in queste entità:

- `entities.city[0].value = 'Toronto'`
- `entities.city[1].value = 'Boston'`

La condizione `@city.contains('Boston')` in un nodo di dialogo restituisce true anche se Boston è la seconda entità rilevata.

### Proprietà di entità

Ogni entità ha una serie di proprietà associate. Puoi accedere alle informazioni su un'entità attraverso le sue proprietà.

| Proprietà              | Definizione | Suggerimenti sull'utilizzo |
|-----------------------|------------|------------|
| *confidence*          | Una percentuale decimale che rappresenta l'affidabilità del servizio nell'entità riconosciuta. L'affidabilità di un'entità è 0 o 1, a meno che non sia stata attivata la corrispondenza fuzzy delle entità. Se la corrispondenza fuzzy è abilitata, la soglia del livello di affidabilità predefinita è 0,3. A prescindere dell'abilitazione della corrispondenza fuzzy, le entità di sistema hanno sempre un livello di affidabilità pari a 1. | Puoi utilizzare questa proprietà in una condizione in modo che restituisca false se il livello di affidabilità non è superiore a una percentuale da te specificata. |
| *location*            | Un offset di caratteri in base zero che indica dove iniziano e terminano i valori di entità rilevati nel testo di input. | Utilizza `.literal` per estrarre la parte di testo tra i valori di indice iniziali e finali memorizzati nella proprietà di posizione. |
| *value*               | Il valore di entità identificato nell'input. | Questa proprietà restituisce il valore di entità come definito nei dati di addestramento, anche se la corrispondenza è avvenuta in uno dei suoi sinonimi associati. Puoi utilizzare `.values` per acquisire più ricorrenze di un'entità che potrebbero essere presenti nell'input utente. |

### Esempi di utilizzo delle proprietà di entità
Nei seguenti esempi, lo spazio di lavoro contiene un'entità aeroporto che include il valore JFK e il sinonimo 'Aeroporto Kennedy". L'input utente è *Voglio andare all'aeroporto Kennedy*.

- Per restituire una risposta specifica se l'entità 'JFK' viene riconosciuta nell'input utente, puoi aggiungere questa espressione alla condizione di risposta:
  `entities.airport[0].value == 'JFK'`
  o
  `@airport = "JFK"`
- Per restituire il nome entità come specificato dall'utente nella risposta di dialogo, utilizza la proprietà .literal:
  `So you want to go to <?entities.airport[0].literal?>...`
  o
  `So you want to go to @airport.literal ...`

  Entrambi i formati restituiscono 'So you want to go to Kennedy Airport...' nella risposta.

- Espressioni come `@airport:(JFK)` o `@airport.contains('JFK')` fanno sempre riferimento al **valore** dell'entità (in questo esempio, `JFK`).
- Per essere più restrittivi sui termini che vengono identificati come aeroporti nell'input quando è abilitata la corrispondenza fuzzy, puoi specificare questa espressione in una condizione del nodo, ad esempio: `@airport && @airport.confidence > 0.7`. Il nodo verrà eseguito solo se il servizio è sicuro al 70% che il testo di input contiene un riferimento all'aeroporto.

In questo esempio, l'input utente è *Ci sono dei posti per cambiare la valuta in JFK, Logan e O'Hare?*

- Per acquisire più ricorrenze di un tipo di entità nell'input utente, utilizza una sintassi come questa:

    ```json
    "context":{
      "airports":"@airport.values"
    }
    ```

  Per fare successivamente riferimento all'elenco acquisito in una risposta di dialogo, aggiungi questa sintassi:
  `Hai chiesto informazioni su questi aeroporti: <? $airports.join(', ') ?>.`
  Viene visualizzato come questo:
  `Hai chiesto informazioni su questi aeroporti: JFK, Logan, O'Hare.`

## Accesso agli intenti
{: #access-intent}

L'array di intenti contiene uno o più intenti riconosciuti nell'input utente, ordinati in ordine decrescente di affidabilità. 

Ogni intento ha un'unica proprietà: la proprietà `confidence`. La proprietà di affidabilità è una percentuale decimale che rappresenta l'affidabilità del servizio nell'intento riconosciuto.

Mentre testi il tuo dialogo, puoi vedere i dettagli degli intenti riconosciuti nell'input utente specificando questa espressione in una risposta del nodo di dialogo:

```json
<? intents ?>
```
{: codeblock}

Per l'input utente, *Hello now*, il servizio trova una corrispondenza esatta con l'intento #greeting. Quindi, elenca prima i dettagli dell'oggetto di intento #greeting. La risposta include anche i primi 10 altri intenti definiti nella competenza indipendentemente dal loro punteggio di affidabilità. (In questo esempio, l'affidabilità negli altri intenti è impostata su 0 in quanto il primo intento è una corrispondenza esatta.) Vengono restituiti i primi 10 intenti in quanto il riquadro "Provalo" invia il parametro `alternate_intents:true` con la sua richiesta. Se stai utilizzando direttamente l'API e desideri vedere i primi 10 risultati, assicurati di specificare questo parametro nella tua chiamata. Se `alternate_intents` è false, che è il valore predefinito, nell'array verranno restituiti solo gli intenti con un'affidabilità superiore a 0,2.

```json
[{"intent":"greeting","confidence":1},
{"intent":"yes","confidence":0},
{"intent":"pizza-order","confidence":0}]
```
{: codeblock}

I seguenti esempi mostrano come controllare un valore di intento:

- `intents[0] == 'Help'`
- `intent == 'Help'`

`intent == 'help'` è diverso da `intents[0] == 'help'` perché `intent == 'help'` non genera un'eccezione se non viene rilevato alcun intento. Viene valutato come true solo se l'affidabilità dell'intento supera una soglia.  Se lo desideri, puoi specificare un livello di affidabilità personalizzato per una condizione, ad esempio `intents.size() > 0 && intents[0] == 'help' && intents[0].confidence > 0.1`

## Accesso all'input
{: #access-input}

L'oggetto JSON di input contiene una sola proprietà: la proprietà di testo. La proprietà di testo rappresenta il testo dell'input utente.

### Esempi di utilizzo delle proprietà di input

Il seguente esempio mostra come accedere all'input:

- Per eseguire un nodo se l'input utente è "Yes", aggiungi questa espressione alla condizione del nodo:
  `input.text == 'Yes'`

Puoi utilizzare uno qualsiasi dei [Metodi stringa](/docs/services/conversation/dialog-methods.html#strings) per valutare o manipolare il testo dell'input utente. Ad esempio:

- Per controllare se l'input utente contiene "Yes", utilizza: `input.text.contains( 'Yes' )`.
- Restituisce true se l'input utente è un numero: `input.text.matches( '[0-9]+' )`.
- Per controllare se la stringa di input contiene dieci caratteri, utilizza: `input.text.length() == 10`.
