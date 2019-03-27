---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-11"

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

# Correzione dell'input utente
{: #beta-spell-check}

Questa funzione è disponibile per essere utilizzata solo dai partecipanti al programma beta. Per capire come richiedere l'accesso, vedi [Partecipa al programma beta](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

![Beta](images/beta.png) IBM rilascia servizi, funzioni e supporto linguistico classificati come beta per la tua valutazione. Queste funzioni potrebbero essere instabili, cambiare frequentemente e potrebbero essere sospese con breve preavviso. Inoltre, le funzioni beta potrebbero non fornire lo stesso livello di prestazioni o di compatibilità fornito generalmente dalle funzioni e non sono progettate per essere utilizzate in un ambiente di produzione.

Abilita la funzione di *controllo ortografico* per correggere gli errori di ortografia commessi dagli utenti nelle espressioni che inoltrano come input utente. Quando il controllo ortografico è abilitato, le parole scritte in modo errato vengono corrette automaticamente. E sono le parole corrette che vengono utilizzate per valutare l'input. Quando viene fornito un input più preciso, il servizio può riconoscere più frequentemente le citazioni di entità e comprendere l'intento dell'utente. 

Attualmente, questa impostazione può essere abilitata solo per le capacità di dialogo in lingua Inglese.
{: note}

Con il controllo ortografico abilitato, l'input utente viene corretto nel seguente modo:

- Input originale: `letme applt for a memberdhip`
- Input corretto: `let me apply for a membership`

Quando il servizio valuta se correggere l'ortografia di una parola, non si basa su un semplice processo di ricerca nel dizionario. Utilizza invece una combinazione di elaborazione del linguaggio naturale e di modelli probabilistici per valutare se un termine è, in effetti, scritto in modo errato e deve essere corretto. 

## Abilitazione del controllo ortografico
{: #beta-spell-check-enable}

Per abilitare la funzione di controllo ortografico, completa i seguenti passi: 

1.  Dalla pagina Skills, trova la tua capacità. 
1.  Fai clic sull'icona ![apri e chiudi elenco delle opzioni](images/kabob-beta.png) e quindi scegli **Settings**.
1.  Dalla pagina delle impostazioni *Spell Check*, attiva **Spell check auto-correction**.

### Test della correzione ortografica
{: #beta-spell-check-test}

1.  Dal pannello "Provalo", immetti un'espressione che includa alcune parole scritte in modo errato. 

    Se le parole nel tuo input sono scritte in modo errato, verranno corrette automaticamente e verrà visualizzata un'icona ![correzione automatica](images/auto-correct.png). L'espressione corretta è sottolineata.
1.  Posiziona il mouse sull'espressione sottolineata per vedere l'originale. 

Se sono presenti termini scritti in modo errato di cui hai previsto la correzione da parte del servizio, ma che non sono stati corretti, riesamina le regole utilizzate dal servizio per stabilire se correggere una parola per vedere se la parola rientra nella categoria delle parole che non verranno cambiate intenzionalmente dal servizio. 

Per evitare un'ipercorrezione, il servizio non corregge l'ortografia dei seguenti tipi di input:

- Parole in maiuscolo
- Emoji
- Entità di posizione, ad esempio stati e indirizzi
- Numeri e unità di misura o di tempo
- Nomi propri, ad esempio nomi comuni o nomi di società
- Testo tra virgolette
- Parole che contengono caratteri speciali, ad esempio trattini (-), asterischi (*) ed e commerciale (&)
- Parole che *appartengono* a questa capacità, ossia parole che hanno un significato implicito perché sono presenti nei valori di entità, nei sinonimi di entità o negli esempi utente dell'intento. 

Se la parola che non viene corretta non è ovviamente uno di questi tipi di input, vale la pena controllare se l'entità ha una corrispondenza fuzzy per tale parola. 

#### Come si correla la correzione ortografica alla corrispondenza fuzzy?
{: #beta-spell-check-vs-fuzzy-matching}

La corrispondenza fuzzy aiuta il servizio a riconoscere le citazioni di entità basate sul dizionario nell'input utente. Utilizza un approccio di ricerca nel dizionario per mettere in corrispondenza una parola dell'input utente con un valore o un sinonimo di entità esistente nei dati di addestramento della capacità. Ad esempio, se l'utente immette `books` e i tuoi dati di addestramento contengono il sinonimo di entità `book`, la corrispondenza fuzzy riconosce che questi due termini indicano la stessa cosa. 

Quando abiliti sia il controllo ortografico che la corrispondenza fuzzy, quest'ultima viene eseguita prima che venga attivato il primo. Se trova un termine che può essere messo in corrispondenza con un sinonimo o un valore di entità del dizionario esistente, lo aggiunge all'elenco delle parole che *appartengono* alla capacità e quindi non deve essere corretto. Allo stesso modo, se un utente immette una frase come `I want to buy a boook`, la corrispondenza fuzzy riconosce che il termine `boook` indica la stessa cosa del tuo sinonimo di entità `book` e lo aggiunge all'elenco delle parole protette. Di conseguenza, il servizio *non* corregge l'ortografia di `boook`.

#### Modalità di funzionamento
{: #beta-spell-check-how-it-works}

Di norma, l'input utente viene salvato così com'è nel campo `text` dell'oggetto `input` del messaggio. Se e soltanto se l'input utente viene corretto in qualche modo, verrà creato un nuovo campo nell'oggetto `input`, denominato `original_text`. Questo campo memorizza l'input originale dell'utente che include tutti i termini scritti in modo errato contenuti in esso. E il testo corretto viene aggiunto al campo `input.text`.
