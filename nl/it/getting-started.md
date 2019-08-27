---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-06"

keywords: assistant, omnichannel, virtual agent, virtual assistant, chatbot, conversation, watson assistant, watson conversation

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
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
{:hide-dashboard: .hide-dashboard}
{:download: .download}
{:gif: data-image-type='gif'}

# Introduzione a {{site.data.keyword.conversationshort}}
{: #getting-started}

In questa breve esercitazione, viene presentato {{site.data.keyword.conversationfull}} e viene descritto il processo per creare il tuo primo assistente.
{: shortdesc}

## Prima di iniziare
{: #getting-started-prerequisites}
{: hide-dashboard}

Hai bisogno di un'istanza del servizio per iniziare.
{: hide-dashboard}

1.  {: hide-dashboard} Vai alla pagina [{{site.data.keyword.conversationshort}} ![Icona link esterno](../../icons/launch-glyph.svg ">Icona link esterno")](https://cloud.ibm.com/catalog/services/watson-assistant) nel catalogo {{site.data.keyword.cloud}}.

    L'istanza del servizio verrà creata nel gruppo di risorse **predefinito** se non ne scegli un altro e *non può* essere modificato successivamente. Questo gruppo è sufficiente per provare il prodotto. 

    Se stai creando un'istanza per un utilizzo più robusto, acquisisci ulteriori informazioni sui [gruppi di risorse ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/docs/resources/bestpractice_rgs#bp_resourcegroups){: new_window}.
1.  {: hide-dashboard} Registrati per un account {{site.data.keyword.cloud_notm}} gratuito oppure esegui l'accesso.
1.  {: hide-dashboard} Fai clic su **Create**.

## Passo 1: apri Watson Assistant
{: #getting-started-launch-tool}

Dopo aver creato un'istanza del servizio {{site.data.keyword.conversationshort}}, vieni portato alla pagina **Manage** del dashboard {{site.data.keyword.conversationshort}}.
{: hide-dashboard}

1.  Fai clic su **Launch {{site.data.keyword.conversationshort}}**. Se ti viene richiesto di eseguire l'accesso, fornisci le tue credenziali {{site.data.keyword.cloud_notm}}. 

<!-- Remove this text after dedicated instances have the developer console: begin -->

{{site.data.keyword.Bluemix_dedicated_notm}}: seleziona la tua istanza del servizio dal Dashboard per avviare il prodotto.

<!-- Remove this text after dedicated instances have the Developer Console: end -->

Se sei un nuovo utente, verrà creato automaticamente per te un assistente denominato *My first assistant*. Passa al passo successivo. 

![Mostra che un assistente è stato aggiunto automaticamente alla pagina Assistants](images/gs-ass-created-for-me.png)

Se disponibile nella tua ubicazione, inizia un tour che puoi utilizzare per acquisire informazioni sul prodotto. Segui il tour; si sovrappone a questi passi dell'esercitazione, per cui puoi riprenderla una volta terminato il tour.
  {: tip}

Un [*assistente*](/docs/services/assistant?topic=assistant-assistants) è un bot cognitivo a cui aggiungi una capacità per consentirgli di interagire con i tuoi clienti in modo utile.

Se un assistente non viene creato automaticamente, il tuo primo passo consiste nel creare un assistente. 

## Passo 2: crea un assistente
{: #getting-started-create-assistant}

1.  Fai clic su **Create assistant**.

    ![Pulsante Create assistant sulla pagina Assistants.](images/gs-create-assistant.png)
1.  Assegna all'assistente il nome `My first assistant`.
1.  Fai clic su **Create assistant**.

    ![Completamento della creazione del nuovo assistente](images/gs-create-assistant-done.png)

## Passo 3: crea una capacità di dialogo
{: #getting-started-add-skill}

Una *capacità di dialogo* è un contenitore per le risorse che definiscono il flusso di una conversazione che il tuo assistente può avere con i tuoi clienti.

1.  Se l'assistente è stato creato per te, fai clic sul tile *My first assistant* per aprire l'assistente. 

1.  Fai clic su **Add dialog skill**.

    ![Mostra il pulsante Add skill dalla home page](images/gs-new-skill.png)

1.  Assegna il nome `Conversational skill tutorial` alla tua capacità.
1.  **Facoltativo**. Se il dialogo che intendi creare utilizzerà una lingua diversa dall'inglese, scegli la lingua corretta dall'elenco.

    ![Completa la creazione della capacità](images/gs-add-skill-done.png)

1.  Fai clic su **Create dialog skill**.

    ![Completa la creazione della capacità](images/gs-skill-added.png)

1.  Fai clic per aprire la capacità che hai appena creato. 

Vieni portato alla pagina Intents.

## Passo 4: aggiungi gli intenti da un catalogo di contenuto
{: #getting-started-add-catalog}

Aggiungi i dati di addestramento che sono stati creati da IBM alla tua capacità aggiungendo gli intenti da un catalogo di contenuto. In particolare, fornirai al tuo assistente l'accesso al catalogo di contenuto **General** in modo tale che il tuo dialogo possa salutare gli utenti e terminare le conversazioni con loro.

1.  Fai clic sulla scheda **Content Catalog**.
1.  Trova **General** nell'elenco e poi fai clic su **Add to skill**.

    ![Mostra il catalogo di contenuto ed evidenzia il pulsante Add per il catalogo General.](images/gs-add-general-catalog.png)
1.  Apri la scheda **Intents** per esaminare gli intenti e le espressioni di esempio associate che sono stati aggiunti ai tuoi dati di addestramento. Puoi riconoscerli perché ciascun nome intento inizia con il prefisso `#General_`. Aggiungerai gli intenti `#General_Greetings` e `#General_Ending` al tuo dialogo nel passo successivo.

    ![Mostra gli intenti visualizzati nella scheda Intents dopo aver aggiunto il catalogo General.](images/gs-general-added.png)

Hai iniziato a creare correttamente i tuoi dati di addestramento aggiungendo il contenuto precostruito da {{site.data.keyword.IBM_notm}}.

## Passo 5: crea un dialogo
{: #getting-started-build-dialog}

Un [dialogo](/docs/services/assistant?topic=assistant-dialog-overview) definisce il flusso della tua conversazione sotto forma di una struttura ad albero logica. Mette in corrispondenza gli intenti (cosa dicono gli utenti) con le risposte (cosa risponde il bot). Ogni nodo della struttura ad albero ha una condizione che lo attiva, in base all'input utente.

Creiamo un semplice dialogo che gestisce gli intenti greeting ed ending, ognuno con un singolo nodo.

### Aggiunta di un nodo iniziale

1.  Fai clic sulla scheda **Dialog**.
1.  Fai clic su **Create dialog**. Vedrai due nodi:
    - **Welcome**: contiene un messaggio iniziale che viene visualizzato agli utenti la prima volta che si collegano all'assistente.
    - **Anything else**: contiene frasi che vengono utilizzate per rispondere agli utenti quando il loro input non viene riconosciuto.

    ![Un nuovo dialogo con due nodi integrati](images/gs-new-dialog.png)
1.  Fai clic sul nodo **Welcome** per aprirlo nella vista di modifica.
1.  Sostituisci la risposta predefinita con il testo, `Welcome to the Watson Assistant tutorial!`.

    ![Modifica della risposta del nodo welcome](images/gs-edit-welcome.png)
1.  Fai clic su ![Chiudi](images/close.png) per chiudere la vista di modifica.

Hai creato un nodo di dialogo che viene attivato dalla condizione `welcome`. (`welcome` è una condizione speciale che funziona come un intento, ma non inizia con un simbolo `#`.) Viene attivato quando inizia una nuova conversazione. Il tuo nodo specifica che quando inizia una nuova conversazione, il sistema deve rispondere con il messaggio di benvenuto che hai aggiunto alla sezione di risposta di questo primo nodo.

### Test del nodo iniziale

Puoi testare il tuo dialogo in qualsiasi momento per verificare come funziona. Testiamolo adesso.

- Fai clic sull'icona ![Try it](images/ask_watson.png) per aprire il riquadro "Try it out". Dovresti vedere il tuo messaggio di benvenuto.

### Aggiunta di nodi per gestire gli intenti

Adesso aggiungiamo i nodi tra il nodo `Welcome` e il nodo `Anything else` che gestiscono i nostri intenti.

1.  Fai clic sull'icona More ![Altre opzioni](images/kabob.png) sul nodo **Welcome** e seleziona quindi **Add node below**.
1.  Nel campo **If assistant recognizes** di questo nodo, inizia a digitare `#General_Greetings`. Quindi, seleziona l'opzione **`#General_Greetings`**.
1.  Aggiungi il testo della risposta, `Good day to you!`
1.  Fai clic su ![Chiudi](images/close.png) per chiudere la vista di modifica.

   ![Un nodo welcome generale è stato aggiunto al dialogo.](images/gs-add-greeting-node.png)

1.  Fai clic sull'icona More ![Altre opzioni](images/kabob.png) su questo nodo e seleziona quindi **Add node below** per creare un nodo peer. Nel nodo peer, specifica `#General_Ending` nel campo **If assistant recognizes** e `OK. See you later.
` come testo della risposta.

   ![Aggiunta di un nodo ending al dialogo.](images/gs-add-ending-node.png)

1.  Fai clic su ![Chiudi](images/close.png) per chiudere la vista di modifica.

### Test del riconoscimento degli intenti

Hai creato un semplice dialogo per riconoscere e rispondere agli input greeting ed ending. Vediamo come funziona.

1.  Fai clic sull'icona ![Try it](images/ask_watson.png) per aprire il riquadro "Try it out". Viene visualizzato il messaggio di benvenuto.
1.  Nella parte inferiore del riquadro, immetti `Hello` e premi Invio. L'output indica che l'intento `#General_Greetings` è stato riconosciuto e viene visualizzata la risposta appropriata (`Good day to you.`).
1.  Prova il seguente input:
    - `bye`
    - `howdy`
    - `see ya`
    - `good morning`
    - `sayonara`

![Test del dialogo nel riquadro Try it out](images/gs-try-it.gif){: gif}

{{site.data.keyword.watson}} può riconoscere i tuoi intenti anche quando il tuo input non corrisponde esattamente agli esempi che hai incluso. Il dialogo utilizza gli intenti per identificare lo scopo dell'input utente indipendentemente dall'esatta formulazione utilizzata e quindi risponde nel modo che hai specificato.

### Risultato della creazione di un dialogo

Questo è tutto. Hai creato una semplice conversazione con due intenti e un dialogo per riconoscerli.

## Passo 6: integra l'assistente
{: #getting-started-integrate-assistant}

Ora che hai un assistente che può partecipare a uno scambio colloquiale semplice, provalo.

1.  Fai clic sulla scheda **Assistants**, trova l'assistente *My first assistant* e aprilo.
1.  Effettua una delle seguenti operazioni per provare il tuo assistente con un'integrazione Preview Link. 

    L'integrazione Preview Link crea il tuo assistente in un widget della chat ospitato da una pagina web personalizzata IBM. Puoi aprire la pagina web e conversare con il tuo assistente per provarlo.

    - Se l'assistente è stato creato per te, devi aggiungere un'integrazione Preview Link. Dall'area *Integrations*, fai clic su **Add integration** e poi fai clic su **Preview Link**. Fai clic su **Create**.

    - Se hai creato tu stesso l'assistente, fai clic sul tile dell'integrazione Preview Link per aprirlo.  
    
      Quando crei tu stesso un assistente, un'integrazione Preview Link viene creata automaticamente per te. 

1.  Fai clic sull'URL visualizzato nella pagina.

    La pagina web di test si apre in una nuova scheda.
1.  Immetti `hello` nel campo di testo e guarda il tuo assistente rispondere.  

    ![Il widget nell'integrazione Preview Link che mostra un singolo scambio di dialogo.](images/gs-test-from-preview-link.png)

    Puoi condividere l'URL con altri utenti che potrebbero voler provare il tuo assistente.

1.  Una volta eseguito il test, chiudi la pagina web. Fai clic sulla **X** per chiudere la pagina di integrazione Preview Link.

## Passi successivi
{: #getting-started-next-steps}

Questa esercitazione si basa su un esempio semplice. Per un'applicazione reale, devi definire intenti più interessanti, alcune entità e un dialogo più complesso che utilizzi entrambi. Quando hai una versione che ti soddisfa del tuo assistente, puoi integrarlo con i canali utilizzati dai tuoi clienti, ad esempio Slack. Man mano che il traffico tra l'assistente e i tuoi clienti aumenta, puoi utilizzare gli strumenti forniti nella scheda **Analytics** per analizzare le conversazioni reali e identificare le aree per il miglioramento.

- Completa le esercitazioni successive che creano dialoghi più avanzati:
    - Aggiungi nodi standard con l'esercitazione [Creazione di un dialogo complesso](/docs/services/assistant?topic=assistant-tutorial).
    - Acquisisci ulteriori informazioni sugli slot con l'esercitazione [Aggiunta di un nodo con slot](/docs/services/assistant?topic=assistant-tutorial-slots).
- Controlla altre [applicazioni di esempio](/docs/services/assistant?topic=assistant-sample-apps) per farti delle idee.
