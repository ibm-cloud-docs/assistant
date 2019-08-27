---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-12"

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

# Esercitazione: Comprensione delle digressioni
{: #tutorial-digressions}

In questa esercitazione, vedrai di persona come funzionano le digressioni.
{: shortdesc}

## Obiettivi di apprendimento
{: #tutorial-digressions-objectives}

Al termine dell'esercitazione, imparerai come:

- le digressioni vengono progettata per l'utilizzo
- le impostazioni della digressione influenzano il flusso del dialogo
- verificare le impostazioni della digressione per un dialogo

### Durata
{: #tutorial-digressions-duration}

Il completamento di questa esercitazione richiede circa 20 minuti.

### Prerequisito
{: #tutorial-digressions-prereqs}

Se non hai un'istanza {{site.data.keyword.conversationshort}}, completa la procedura **Prima di cominciare** dell'[Esercitazione introduttiva](/docs/services/assistant?topic=assistant-getting-started#getting-started-prerequisites) per crearne una.

## Passo 1: importa la capacità di dialogo Digressions showcase
{: #tutorial-digressions-import-json}

Per prima cosa dovrai importare la capacità di dialogo *Digression showcase* nella tua istanza {{site.data.keyword.conversationshort}}.

1.  Scarica il file [digression-showcase.json](https://github.com/watson-developer-cloud/community/raw/master/watson-assistant/digression-showcase.json).
1.  Nella tua istanza {{site.data.keyword.conversationshort}}, fai clic sull'icona ![Import](images/workspace_import.png).
1.  Fai clic su **Choose a file** e quindi seleziona il file **digression-showcase.json** che hai scaricato precedentemente.
1.  Fai clic su **Import** per terminare l'importazione della capacità di dialogo.

## Passo 2: generazione della digressione temporanea dal dialogo
{: #tutorial-digressions-temporarily-digress-away}

Le digressioni consentono agli utenti di separarsi da un ramo del dialogo per poter modificare temporaneamente l'argomento prima di ritornare al flusso di dialogo originale. In questo passo, inizierai prenotando un ristorante, poi genererai una digressione per chiedere gli orari di apertura del ristorante. Dopo aver fornito le informazioni sugli orari di apertura, il tuo assistente ritornerà al flusso di dialogo della prenotazione del ristorante.

1.  Fai clic su **Dialog** per passare dalla pagina con gli intenti a una vista della struttura ad albero del dialogo.

1.  Fai clic sull'icona ![Try it](images/ask_watson.png) per aprire il riquadro "Try it out".
1.  Immetti `Book me a restaurant` nel campo di testo.

    Il tuo assistente risponde con una richiesta del giorno in cui si vuole prenotare, `When do you want to go?`

1.  Fai clic sull'icona **Location** ![Location](images/location.png) accanto alla risposta per evidenziare il nodo che ha attivato la risposta, il nodo **Restaurant booking**, nella struttura ad albero del dialogo.

    ![Mostra il nodo Restaurant booking evidenziato e il dialogo in corso nel riquadro Try it out.](images/tut-dig-location.png)
1.  Immetti `Tomorrow`.

    Il tuo assistente risponde con una richiesta dell'ora in cui prenotare, `What time do you want to go?`

1.  Non sai quando il ristorante chiude, per cui chiedi, `What time do you close?`

    Il bot genera una digressione dal nodo Restaurant booking per elaborare il nodo **Restaurant opening hours**. Risponde con, `The restaurant is open from 8:00 AM to 10:00 PM.` Il tuo assistente quindi ritorna al nodo Restaurant booking e ti chiede nuovamente l'ora di prenotazione.

    ![Mostra la digressione che si è verificata nel riquadro Try it out.](images/tut-dig-digression.png)
1.  **Facoltativo**: per completare il flusso di dialogo, immetti `8pm` per l'ora di prenotazione e `2` per il numero di clienti.

Congratulazioni. Hai correttamente generato una digressione e sei ritornato a un flusso di dialogo.

## Passo 3: disabilitazione delle digressioni dello slot
{: #tutorial-digressions-disable-slot}

In questo passo, modificherai le impostazioni della digressione per il nodo Restaurant booking per impedire agli utenti di generare delle digressioni da esso e vedrai come la modifica alle impostazioni influisce sul flusso di dialogo.

1.  Vediamo le impostazioni della digressione correnti per il nodo **Restaurant booking**. Fai clic sul nodo per aprirlo nella vista di modifica.

1.  Fai clic su **Personalizza** e quindi fai clic sulla scheda **Digressioni**.

    ![Mostra le impostazioni della digressione per il nodo Restaurant booking.](images/tut-dig-resto-settings.png)

1.  Modifica l'interruttore **Allow digressions away** da on a **off** e quindi fai clic su **Apply**.

1.  Fai clic su ![Chiudi](images/close.png) per chiudere la vista di modifica del nodo.

1.  Fai clic su **Clear** nel riquadro "Try it out" per reimpostare il dialogo.

1.  Immetti `Book me a restaurant`.

    Il tuo assistente risponde con una richiesta del giorno in cui si vuole prenotare, `When do you want to go?`

1.  Immetti `Tomorrow`.

    Il tuo assistente risponde con una richiesta dell'ora in cui prenotare, `What time do you want to go?`

1.  Chiedi, `What time do you close?`

    Il tuo assistente riconosce che la domanda attiva l'intento #restaurant_opening_hours, ma la ignora e visualizza invece nuovamente la richiesta associata allo slot @sys-time.

Hai correttamente impedito all'utente di generare una digressione dal processo di prenotazione del ristorante.

## Passo 4: digressione a un nodo senza ritorno
{: #tutorial-digressions-digress-without-return}

Puoi configurare un nodo di dialogo per non ritornare al nodo da cui il tuo assistente ha generato la digressione in modo che il nodo corrente venga elaborato. A tal fine, modificherai le impostazioni della digressione per il nodo Restaurant hours. Nel passo 2, hai visto che dopo la generazione della digressione dal nodo Restaurant booking per passare al nodo Restaurant opening hours, il tuo assistente era ritornato al nodo Restaurant booking per continuare con il processo di prenotazione. In questo esercizio, dopo aver modificato l'impostazione, genererai la digressione dal dialogo **Job opportunities** per richiedere informazioni sugli orari di apertura del ristorante e vedrai che il tuo assistente non ritorna nel punto in cui aveva lasciato.

1.  Fai clic per aprire il nodo **Restaurant opening hours**.

1.  Fai clic su **Personalizza** e quindi fai clic sulla scheda **Digressioni**.

1.  Espandi la sezione **Digressions can come into this node** e deseleziona la casella di spunta **Return after digression**. Fai clic su **Apply** e quindi su ![Close](images/close.png) per chiudere la vista di modifica del nodo.

1.  Fai clic su **Clear** nel riquadro "Try it out" per reimpostare il dialogo.

1.  Per interagire con il nodo di dialogo **Job opportunities**, immetti `I'm looking for a job`.

    Il tuo assistente risponde dicendo, `We are always looking for talented people to add to our team. What type of job are you interested in?`

1.  Invece di rispondere a questa domanda, chiedi al bot una domanda non collegata. Immetti `What time do you open?`

    Il tuo assistente genera una digressione da nodo Job opportunities al nodo Restaurant opening hours per rispondere alla tua domanda. Il tuo assistente risponde con `The restaurant is open from 8:00 AM to 10:00 PM.`

    A differenza del test precedente, questa volta il dialogo non riprende da dove era stato interrotto nel nodo **Job opportunities**. Il tuo assistente non ritorna al dialogo che era in corso perché hai modificato l'impostazione sul nodo **Restaurant opening hours** per non ritornare.

    ![Mostra una conversazione che non ritorna dopo una digressione.](images/tut-dig-noreturn.png)

Congratulazioni. Hai correttamente generato una digressione da un dialogo senza ritorno.

## Riepilogo
{: #tutorial-digressions-summary}

In questa esercitazione, hai sperimentato come funzionano le digressioni e visto come le impostazioni di un singolo nodo di dialogo possono influenzare il comportamento delle digressioni.

## Passi successivi
{: #tutorial-digressions-next-steps}

Per assistenza su come si configurano le digressioni per il tuo dialogo, vedi [Digressioni](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions).
