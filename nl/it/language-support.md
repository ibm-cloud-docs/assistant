---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-20"

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

# Lingue supportate
{: #language-support}

Il servizio {{site.data.keyword.conversationshort}} supporta le lingue elencate. Le singole funzioni sono supportate in misura maggiore o minore per ciascuna lingua.
{: shortdesc}

Nelle seguenti tabelle, il livello di supporto per lingua e funzione è indicato da questi codici:

- **GA** - La funzione è generalmente disponibile e supportata per questa lingua. Nota che le funzioni possono continuare ad essere aggiornate anche dopo che sono generalmente disponibili.
- **Beta** - La funzione è supportata solo come release Beta ed è ancora in fase di test prima di essere resa generalmente disponibile in questa lingua.
- **ND** - Indica che una funzione non è disponibile in questa lingua.

La prima tabella mostra il livello di supporto per tutte le funzioni, con l'eccezione di quelle relative agli intenti e alle entità, che vengono mostrate nella seconda e terza tabella.

**Tabella 1. Dettagli del supporto della funzione**

| Lingua | **Definizione di [intenti](/docs/services/assistant?topic=assistant-intents)**, **[entità](/docs/services/assistant?topic=assistant-entities)** e **[dialogo](/docs/services/assistant?topic=assistant-dialog-build)** | **Ricerca** |
|:---:|:---:|:---:|
| **Inglese (en)**                   | GA | GA |
| **Arabo (ar)**                    | GA | ND |
| **Cinese (semplificato) (zh-cn)**   | GA | Beta |
| **Cinese (tradizionale) (zh-tw)**  | Beta | Beta |
| **Ceco (cs)**                     | GA | Beta |
| **Olandese (nl)**                     | GA | Beta |
| **Francese (fr)**                    | GA | Beta |
| **Tedesco (de)**                    | GA | Beta |
| **Italiano (it)**                   | GA | Beta |
| **Giapponese (ja)**                  | GA | Beta |
| **Coreano (ko)**                    | GA | Beta |
| **Portoghese (brasiliano) (pt-br)** | GA | Beta |
| **Spagnolo (es)**                   | GA | Beta |
{: caption="Dettagli del supporto della funzione" caption-side="top"}

**Tabella 2a. Dettagli del supporto della funzione dell'intento**

| Lingua | **[Punteggio assoluto](/docs/services/assistant?topic=assistant-intents#intents-absolute-scoring)** e **[Contrassegna come irrilevante](/docs/services/assistant?topic=assistant-logs#logs-mark-irrelevant)** | **[Catalogo contenuto](/docs/services/assistant?topic=assistant-catalog)** |
|:---:|:---:|:---:|
| **Inglese (en)**                   | GA | GA |
| **Arabo (ar)**                    | Beta | GA |
| **Cinese (semplificato) (zh-cn)**   | GA | ND |
| **Cinese (tradizionale) (zh-tw)**  | Beta | ND |
| **Ceco (cs)**                     | GA | ND |
| **Olandese (nl)**                     | GA | ND |
| **Francese (fr)**                    | GA | GA |
| **Tedesco (de)**                    | GA | GA |
| **Italiano (it)**                   | GA | GA |
| **Giapponese (ja)**                  | GA | GA |
| **Coreano (ko)**                    | GA | ND |
| **Portoghese (brasiliano) (pt-br)** | GA | GA |
| **Spagnolo (es)**                   | GA | GA |
{: caption="Dettagli del supporto della funzione dell'intento" caption-side="top"}

**Tabella 2b. Dettagli del supporto della funzione dell'intento - continua**

| Lingua | **[Consigli sugli esempi utente](/docs/services/assistant?topic=assistant-intent-recommendations#intent-recommendations-get-example-recommendations)** | **[Consigli sull'intento](/docs/services/assistant?topic=assistant-intent-recommendations#intent-recommendations-get-intent-recommendations)** |
|:---:|:---:|
| **Inglese (en)**                   | GA | GA |
| **Arabo (ar)**                    | ND | ND |
| **Cinese (semplificato) (zh-cn)**   | ND | ND |
| **Cinese (tradizionale) (zh-tw)**  | ND | ND |
| **Ceco (cs)**                     | ND | ND |
| **Olandese (nl)**                     | ND | ND |
| **Francese (fr)**                    | ND | ND |
| **Tedesco (de)**                    | ND | ND |
| **Italiano (it)**                   | ND | ND |
| **Giapponese (ja)**                  | GA | ND |
| **Coreano (ko)**                    | ND | ND |
| **Portoghese (brasiliano) (pt-br)** | ND | ND |
| **Spagnolo (es)**                   | ND | ND |
{: caption="Dettagli del supporto della funzione dell'intento - continua " caption-side="top"}

**Tabella 3. Dettagli del supporto della funzione dell'entità**

| Lingua | **[Corrispondenza fuzzy di entità](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching)** | **[Entità contestuali](/docs/services/assistant?topic=assistant-entities#entities-create-annotation-based)** | **[Consigli sui sinonimi](/docs/services/assistant?topic=assistant-entities#entities-synonyms)** |
|:---:|:---:|:---:|:---:|
| **Inglese (en)**                   | GA | Beta | GA |
| **Arabo (ar)**                    | GA (Solo errore di ortografia) | ND | ND |
| **Cinese (semplificato) (zh-cn)**   | ND | ND | ND |
| **Cinese (tradizionale) (zh-tw)**  | ND | ND | ND |
| **Ceco (cs)**                     | GA (Solo errore di ortografia) | ND | ND |
| **Olandese (nl)**                     | GA (Solo errore di ortografia) | ND | ND |
| **Francese (fr)**                    | GA (Solo errore di ortografia) | ND | GA |
| **Tedesco (de)**                    | GA (Solo errore di ortografia) | ND | ND |
| **Italiano (it)**                   | GA (Solo errore di ortografia) | ND | ND |
| **Giapponese (ja)**                  | GA (Solo errore di ortografia) | ND | GA |
| **Coreano (ko)**                    | GA (Solo errore di ortografia) | ND | ND |
| **Portoghese (brasiliano) (pt-br)** | GA (Solo errore di ortografia) | ND | ND |
| **Spagnolo (es)**                   | GA (Solo errore di ortografia) | ND | GA |
{: caption="Dettagli del supporto della funzione dell'entità" caption-side="top"}

**Tabella 4. Dettagli del supporto della funzione dell'entità di sistema**

| Lingua | **Entità di sistema ([numero](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-number), [valuta](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-currency), [percentuale](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-percentage), [data, ora](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-date-time))** | **[Nuove entità di sistema](/docs/services/assistant?topic=assistant-beta-system-entities)** |
|:---|:---:|:---:|
| **Inglese (en)**                   | GA, Beta ([posizione](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-location), [persona](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-person)) | Beta |
| **Arabo (ar)**                    | Beta | ND |
| **Cinese (semplificato) (zh-cn)**   | GA | ND |
| **Cinese (tradizionale) (zh-tw)**  | Beta | ND |
| **Ceco (cs)**                     | GA | ND |
| **Olandese (nl)**                     | GA | ND |
| **Francese (fr)**                    | GA | ND |
| **Tedesco (de)**                    | GA | Beta |
| **Italiano (it)**                   | GA | ND |
| **Giapponese (ja)**                  | GA | ND |
| **Coreano (ko)**                    | GA | ND |
| **Portoghese (brasiliano) (pt-br)** | GA | ND |
| **Spagnolo (es)**                   | GA | ND |
{: caption="Dettagli del supporto della funzione dell'entità di sistema " caption-side="top"}

**Nota:** il servizio {{site.data.keyword.conversationshort}} supporta più lingue come indicato, ma l'interfaccia dello strumento stessa (descrizioni, etichette, ecc.) è in inglese. Tutte le lingue supportate possono essere inserite e addestrate attraverso l'interfaccia inglese.

**Conformità GB18030**: GB18030 è uno standard cinese che specifica una code page estesa da utilizzare nel mercato cinese. Questo standard di code page è importante il settore software in quanto China National Information Technology Standardization Technical Committee ha ordinato che qualsiasi applicazione software rilasciata per il mercato cinese dopo il 1° settembre 2001, deve essere abilitata per GB18030. Il servizio {{site.data.keyword.conversationshort}} supporta questa codifica e viene certificato come conforme a GB18030.

## Modifica di una lingua della capacità
{: #language-support-change-language}

Una volta creata una capacità, la sua lingua non può essere modificata. Se è necessario modificare la lingua supportata di una capacità, l'utente deve scaricare la capacità. Quindi, deve modificare il file JSON risultante in un editor di testo, cercando una proprietà JSON chiamata `language`.

La proprietà `language` deve essere impostata sulla lingua originale della capacità; ad esempio, inglese sarà `en`. Modifica il valore di questa proprietà, cambiandolo nella lingua desiderata (`fr` per francese, `de` per tedesco, ecc). Salva le modifiche nel file JSON e importa il file modificato nella tua istanza del servizio {{site.data.keyword.conversationshort}}.

## Configurazione di lingue bidirezionali
{: #language-support-configure-bi-directional}

Per le lingue bidirezionali, come l'arabo, puoi modificare le preferenze della capacità di conseguenza. Dal tuo tile della capacità, seleziona il menu a discesa *Azioni* e seleziona **Preferenze bidi** (questa opzione è disponibile solo per le capacità impostate su una lingua bidirezionale):

![Preferenze bidi](images/bidi_prefs.png)

Scegli tra le seguenti opzioni per la tua capacità:

- **Direzione GUI**: specifica la direzione di layout degli elementi, come pulsanti o menu, nell'interfaccia utente grafica. Scegli `LTR` (da sinistra a destra) o `RTL` (da destra a sinistra). Se non specificato, lo strumento segue l'impostazione della direzione della GUI del browser Web.
- **Direzione testo**: specifica la direzione del testo immesso. Scegli `LTR` (da sinistra a destra) o `RTL` (da destra a sinistra) oppure seleziona `Auto` che sceglierà automaticamente la direzione del testo in base alle impostazioni del tuo sistema. L'opzione `None` visualizzerà il testo da sinistra a destra.
- **Forma numerica**: specifica quale forma di numeri utilizzare quando si presentano cifre regolari. Scegli tra `Nominal`, `Arabic-Indic` o `Arabic-European`. L'opzione `None` visualizzerà i numeri occidentali.
- **Tipo di calendario**: specifica come scegli di filtrare le date nella IU della capacità. Scegli `Islamic-Civil`, `Islamic-Tabular`, `Islamic-Umm al-Qura` o `Gregorian`.

  Questa impostazione non si applica al riquadro "Try it out".
  {: note}

![Opzioni bidi](images/bidi_opts.png)

Al termine delle selezioni, fai clic su **Aggiorna** per salvare e ritornare al tile della capacità.

## Utilizzo di caratteri accentati
{: #language-support-accents}

In un contesto di conversazione, gli utenti possono utilizzare o meno accenti durante l'interazione con il servizio {{site.data.keyword.conversationshort}}. Di conseguenza, sia le versioni accentate che quelle non accentate delle parole possono essere trattate allo stesso modo per il rilevamento di intenti e il riconoscimento di entità.

Tuttavia per alcune lingue, come lo spagnolo, alcuni accenti possono modificare il significato dell'entità. Pertanto, per il rilevamento di entità, sebbene l'entità originale possa implicitamente avere un accento, il tuo assistente può anche abbinare la versione non accentata della stessa entità, ma con un punteggio di affidabilità leggermente inferiore.

Ad esempio, per la parola "barrió", che ha un accento e corrisponde al passato del verbo "barrer" (spazzare), il tuo assistente può abbinare anche la parola "barrio" (quartiere), ma con una confidenza leggermente inferiore.

Il sistema fornirà punteggi di affidabilità più alti nelle entità con corrispondenze esatte. Ad esempio, `barrio` non verrà rilevato se `barrió` è nel set di addestramento e `barrió` non verrà rilevato se `barrio` è nel set di addestramento.

Dovresti addestrare il sistema con i caratteri e gli accenti appropriati. Ad esempio, se prevedi `barrió` come risposta, non devi inserire `barrió` nel set di addestramento.

Sebbene non sia un segno di accento, lo stesso vale per le parole che utilizzano, ad esempio, la lettera spagnola `ñ` rispetto alla lettera `n`, ad esempio "uña" rispetto a "una". In questo caso, la lettera `ñ` non è semplicemente una `n` con un accento; è una lettera unica e specifica per lo spagnolo.

Puoi abilitare la corrispondenza fuzzy se ritieni che i tuoi clienti non utilizzeranno gli accenti appropriati o faranno errori di ortografia (incluso, ad esempio, mettere una `n` al posto di una `ñ`) o puoi includerli esplicitamente negli esempi di addestramento.
