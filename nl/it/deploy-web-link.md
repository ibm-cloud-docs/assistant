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

# Integrazione con un widget della chat ospitato nel web
{: #deploy-web-link}

Se non disabiliti il link di anteprima, l'assistente è immediatamente disponibile per il test da una pagina web.
{: shortdesc}

L'assistente viene implementato automaticamente come un widget della chat incorporato in una pagina web personalizzata IBM semplice. Puoi verificare la capacità di dialogo che hai aggiunto all'assistente immettendo del testo nel widget della chat. Puoi anche condividere l'URL della pagina con altri per ottenere supporto nel verificare e ottenere il feedback sull'assistente.

A differenza di quando esegui il test utilizzando il riquadro "Try it out", le chiamate API risultanti dalle tue interazioni con l'assistente ospitato dall'URL Preview Link incorreranno in addebiti. 

## Utilizzo dell'integrazione Preview Link per verificare il tuo assistente
{: #deploy-web-link-try}

Per verificare l'assistente da un widget della chat ospitato nel web, completa i seguenti passi:

1.  Dalla scheda Assistants, fai clic per aprire il tile dell'assistente che desideri verificare.

1.  Dalla sezione *Integrations*, fai clic sul tile **Preview Link**.

    Se non hai abilitato Preview Link quando hai creato l'assistente, fai clic su **Add integration** e poi fai clic sul tile dell'integrazione **Preview Link**.

1.  **Facoltativo**: modifica il nome e la descrizione della pagina web di anteprima.

1.  Fai clic sul link URL visualizzato per aprire la pagina di test.

    Viene aperta una scheda del browser web separata che contiene l'implementazione di un widget della chat del tuo assistente.

    Se la tua istanza del servizio è stata creata in un data center nel Regno Unito prima del 13 dicembre 2018 oppure a Sydney prima del 7 maggio 2018, devi modificare l'URL di Preview Link. L'URL include un codice ubicazione per il data center in cui è ospitata l'istanza. Poiché le istanze a Londra e a Sydney sono state diffuse a Dallas prima delle date specificate, devi sostituire il riferimento `eu-gb` o `au-syd` nell'URL con `us-south` per eseguire correttamente il rendering della pagina web.
    {: important}

1.  Inoltra le espressioni di test per vedere in che modo risponde l'assistente.

    Non vengono restituite risposte fino a quando non crei una capacità di dialogo e la aggiungi all'assistente.

    Il flusso di dialogo per la sessione corrente viene riavviato dopo 60 minuti di inattività (5 minuti per i piani Lite e Standard). Ciò significa che se un utente smette di interagire con l'assistente, dopo 60 (o 5) minuti, i valori delle variabili di contesto impostati durante la conversazione precedente vengono impostati su null o riportati ai loro valori predefiniti.

1.  Dopo il test, puoi chiudere la scheda del browser per uscire dalla pagina web pubblica.

1.  Fai clic su **Save Changes** per salvare le modifiche che hai apportato all'integrazione Preview Link e chiudere la pagina oppure fai clic sulla **X** per chiudere la pagina senza salvare. 

## Considerazioni sul dialogo
{: #deploy-web-link-dialog}

Le risposte esaurienti che aggiungi a un catalogo vengono visualizzate nel widget della chat ospitato nel web come previsto, con le seguenti eccezioni:

- **Connect to human agent**: questo tipo di risposta viene ignorato.

- **Pause**: questo tipo di risposta sospende l'attività dell'assistente nel widget della chat. Tuttavia, l'attività non riprende dopo la pausa fino a quando non viene attivata un'altra risposta. Ogni volta che includi un tipo di risposta `pause`, aggiungi un altro tipo di risposta diverso, ad esempio `text`, dopo di esso.

Per ulteriori informazioni sui tipi di risposta, vedi [Risposte esaurienti](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia).

## Aggiunta di un'integrazione Preview Link
{: #deploy-web-link-add-more}

Se hai accidentalmente eliminato l'integrazione Preview Link creata automaticamente per te oppure desideri creare un'altra pagina web pubblica separata, puoi aggiungere un'integrazione Preview Link.

1.  Dalla scheda Assistant, fai clic per aprire il tile per l'assistente che desideri distribuire.

1.  Vai alla sezione **Integrations** e poi fai clic su **Add integration**.

1.  Fai clic sul tile di integrazione **Preview Link**.

1.  Facoltativamente, modifica il nome e la descrizione dell'integrazione Preview Link e poi fai clic su **Create**.

    Viene aggiunto un URL alla pagina. Fai clic su di esso per aprire la pagina web di anteprima.
