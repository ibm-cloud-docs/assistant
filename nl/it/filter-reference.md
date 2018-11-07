---

copyright:
  years: 2015, 2017
lastupdated: "2017-09-18"

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

# Riferimento alla query di filtro

L'API REST del servizio {{site.data.keyword.conversationshort}} offre potenti funzioni di ricerca dei log tramite le query di filtro. Puoi utilizzare il parametro `filter` dell'API /logs per ricercare nel log dello spazio di lavoro gli eventi che corrispondono a una query specificata.

Il parametro `filter` è una query di cache che limita i risultati a quelli corrispondenti al filtro specificato. Puoi applicare un filtro su qualsiasi oggetto che fa parte del modello di risposta JSON (ad esempio, il testo dell'input utente, gli intenti e le entità rilevati o il punteggio di affidabilità).

Per visualizzare degli esempi dei vari tipi di query di filtro, vedi [Esempi](#examples).

Per ulteriori informazioni sul metodo  `GET` /logs e sul suo modello di risposta, vedi il [Riferimento API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#get_logs){: new_window}.

## Sintassi della query di filtro

Il seguente esempio mostra il formato generale di una query di filtro:

| Posizione           | Operatore di query | Termine         |
|:------------------:|:--------------:|:------------:|
|`request.input.text`|`::`            |`"IBM Watson"`|

- La _posizione_ identifica il campo in cui applicare il filtro (in questo esempio, `request.input.text`).
- L'_operatore di query_ specifica il tipo di corrispondenza che vuoi utilizzare (corrispondenza fuzzy o corrispondenza esatta).
- Il _termine_ specifica l'espressione o il valore che vuoi utilizzare per valutare la corrispondenza nel campo. Il termine può contenere testo e operatori letterali, come descritto nella [sezione successiva](#operators).

Il filtro per intento o entità richiede una sintassi leggermente diversa dal filtro sugli altri campi. Per ulteriori informazioni, vedi [Filtro per intento o entità](#intent_entity_filter).

**Nota:** la sintassi delle query di filtro utilizza alcuni caratteri che non sono ammessi nelle query HTTP. Assicurati che tutti i caratteri speciali, inclusi spazi e virgolette, siano codificati tramite URL quando inviati come parte di una query HTTP. Ad esempio, il filtro `response_timestamp<2016-11-01` viene specificato come `response_timestamp%3C2016-11-01`.

## Operatori
<!-- {#operators} -->

Nella query di filtro puoi utilizzare i seguenti operatori.

| Operatore | Descrizione |
|:-------------------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `:` | Operatore di query di corrispondenza fuzzy. Aggiungi il prefisso `:` al termine di query se vuoi trovare una corrispondenza con qualsiasi valore che contenga il termine di query o una variante grammaticale del termine di query. La corrispondenza fuzzy è disponibile per il testo dell'input utente, il testo di output della risposta e i valori di entità. |
| `::` | Operatore di query di corrispondenza esatta. Aggiungi il prefisso `::` se vuoi trovare una corrispondenza solo con i valori esattamente uguali al termine di query. |
| `:!` | Operatore di query di corrispondenza fuzzy negativa. Aggiungi il prefisso `:!` al termine di query se vuoi trovare una corrispondenza solo con i valori che _non_ contengono il termine di query o una variabile grammaticale del termine di query. |
| `::!` | Operatore di query di corrispondenza esatta negativa. Aggiungi il prefisso `::!` al termine di query se vuoi trovare una corrispondenza solo con i valori che _non_ corrispondono esattamente al termine di query. |
| `<=`, `>=`, `>`, `<` | Operatori di confronto. Aggiungi questi operatori come prefisso per il termine di query per trovare una corrispondenza in base al confronto aritmetico. |
| `\` | Operatore di escape. Da utilizzare nelle query che includono caratteri di controllo che altrimenti verrebbero analizzati come operatori. Ad esempio, `\!hello` potrebbe corrispondere al testo `!hello`. |
| `""` | Frase letterale. Da utilizzare per racchiudere un termine di query che contiene spazi o altri caratteri speciali. Nessun carattere speciale all'interno delle virgolette viene analizzato dall'API. |
| `~` | Corrispondenza approssimativa. Aggiungi questo operatore seguito da `1` o `2` alla fine del termine di query per specificare il numero consentito di differenze a carattere singolo tra la stringa di query e una corrispondenza nell'oggetto di risposta. Ad esempio, `car~1` può corrispondere a `car`, `cat` o `cars`, ma non può corrispondere a `cats`. Questo operatore non è valido quando si utilizza il filtro su `log_id` o su qualsiasi campo di data e ora oppure con la corrispondenza fuzzy. |
| `*` | Operatore carattere jolly. Corrisponde a qualsiasi sequenza di zero o più caratteri. Questo operatore non è valido quando si utilizza il filtro su `log_id` o su qualsiasi campo di data e ora. |
| `()`, `[]` | Operatori di raggruppamento. Da utilizzare per racchiudere un raggruppamento logico di più espressioni utilizzando operatori booleani. |
| \| | Operatore booleano _or_. |
| `,` | Operatore booleano _and_. |

### Filtro per intento o entità
<!-- {#intent_entity_filter} -->

A causa delle differenze nel modo in cui gli intenti e le entità vengono memorizzati internamente, la sintassi per il filtro su un determinato intento o entità è diversa dalla sintassi utilizzata per gli altri campi nel JSON restituito. Per specificare un campo `intent` o `entity` in una raccolta di `intents` o `entities`, devi utilizzare l'operatore di corrispondenza `:` al posto di un punto.

Ad esempio, questa query corrisponde a qualsiasi evento registrato in cui la risposta include un intento rilevato denominato `hello`:

`response.intents:intent::hello`

Nota l'operatore `:` al posto di un punto (intents:intent)

Utilizza lo stesso modello per la corrispondenza su qualsiasi campo di un intento o entità rilevato nella risposta. Ad esempio, questa query corrisponde a qualsiasi evento registrato in cui la risposta include un'entità rilevata con il valore `soda`:

`response.entities:value::soda`

Allo stesso modo, puoi filtrare su intenti o entità inviati come parte della richiesta, come in questo esempio:

`request.intents:intent::hello`

**Nota**: il filtro sugli intenti agisce su tutti gli intenti rilevati. Per filtrare solo sull'intento rilevato con l'affidabilità più alta, puoi utilizzare la sintassi abbreviata `response.top_intent`, come in questo esempio:

`response.top_intent::goodbye`

### Filtro per altri campi

Per filtrare su qualsiasi altro campo nei dati di log, specifica la posizione come percorso che identifica i livelli degli oggetti nidificati nella risposta JSON dall'API /logs. Utilizza i punti (`.`) per specificare i livelli successivi di nidificazione nei dati JSON. Ad esempio, la posizione `request.input.text` identifica il campo di testo dell'input utente come mostrato nel seguente frammento JSON:

```json
  "request": {
    "input": {
      "text": "Good morning"
    }
  }
```
<!-- {data-copy=false} -->

## Esempi

I seguenti esempi illustrano vari tipi di query che utilizzano questa sintassi.

| Descrizione | Query |
|---------|-----------|
| La data della risposta è nel mese di luglio 2017. | `response_timestamp>=2017-07-01,response_timestamp<2017-08-01` |
| La data/ora della risposta è precedente a `2016-11-01T04:00:00.000Z`. | `response_timestamp<2016-11-01T04:00:00.000Z` |
| Il testo dell'input utente contiene la parola "order" o una variante grammaticale (ad esempio, `orders` o `ordering`. | `request.input.text:order` |
| Un nome intento nella risposta corrisponde esattamente a `place_order`. | `response.intents:intent::place_order` |
| Un nome entità nella risposta corrisponde esattamente a `beverage`.  | `response.entities:entity::beverage` |
| Il testo dell'input utente non contiene la parola "order" o una variante grammaticale. | `request.input.text:!order` |
| Il nome dell'intento rilevato con l'affidabilità più alta non corrisponde esattamente a `hello`. | `response.top_intent::!hello` |
| Il testo dell'input utente contiene la stringa `!hello`. | `request.input.text:\!hello` |
| Il testo dell'input utente contiene la stringa `IBM Watson`. | `request.input.text:"IBM Watson"` |
| Il testo dell'input utente contiene una stringa che non ha più di 2 differenze a carattere singolo rispetto a `Watson`. | `request.input.text:Watson~2` |
| Il testo dell'input utente contiene una stringa composta da `comm`, seguito da zero o più caratteri aggiuntivi, seguiti da `s`. | `request.input.text:comm*s` |
| L'ID distribuzione nel contesto corrisponde a `my_app`. | `request.context.metadata.deployment::my_app` |
| Un'entità nella risposta ha un valore di affidabilità superiore a 0,8. | `response.intents:confidence>0.8` |
| Un nome intento nella risposta corrisponde esattamente a `hello` o a `goodbye`. | <code>response.intents:intent::(hello\|goodbye)</code> |
| Un intento nella risposta ha il nome `hello` e un valore di affidabilità uguale o superiore a 0,8. | `response.intents:(intent:hello,confidence>=0.8)` |
| Un nome intento nella risposta corrisponde esattamente a `order` e un nome entità nella risposta corrisponde esattamente a `beverage`. | `[response.intents:intent::order,response.entities:entity::beverage]` |
<!-- -->
