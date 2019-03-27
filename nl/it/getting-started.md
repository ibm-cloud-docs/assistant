---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-01"

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

# Esercitazione introduttiva
{: #getting-started}

In questa breve esercitazione, viene presentato lo strumento {{site.data.keyword.conversationshort}} e viene descritto il processo per creare il tuo primo assistente.
{: shortdesc}

## Prima di iniziare
{: #getting-started-prerequisites}
{: hide-dashboard}

Hai bisogno di un'istanza del servizio per iniziare.
{: hide-dashboard}

1.  {: hide-dashboard} Vai alla pagina [{{site.data.keyword.conversationshort}} ![Icona link esterno](../../icons/launch-glyph.svg ">Icona link esterno")](https://cloud.ibm.com/catalog/services/watson-assistant) nel catalogo {{site.data.keyword.cloud_notm}}.

    L'istanza del servizio verrà creata nel gruppo di risorse **predefinito** se non ne scegli un altro e *non può* essere modificato successivamente. Questo gruppo è sufficiente per provare ad eseguire il servizio. 

    Se stai creando un'istanza per un utilizzo più robusto, acquisisci ulteriori informazioni sui [gruppi di risorse ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/docs/resources/bestpractice_rgs#bp_resourcegroups){: new_window}.
1.  {: hide-dashboard} Registrati per un account {{site.data.keyword.cloud_notm}} gratuito oppure esegui l'accesso. 
1.  {: hide-dashboard} Fai clic su **Create**.

## Passo 1: apri lo strumento
{: #getting-started-launch-tool}

Dopo aver creato un'istanza del servizio {{site.data.keyword.conversationshort}}, vieni portato alla pagina **Manage** del dashboard del servizio.
{: hide-dashboard}

1.  Fai clic su **Launch tool**. Se ti viene richiesto di accedere allo strumento, fornisci le tue credenziali {{site.data.keyword.cloud_notm}}. 

<!-- Remove this text after dedicated instances have the developer console: begin -->

{{site.data.keyword.Bluemix_dedicated_notm}}: seleziona la tua istanza di servizio dal dashboard per avviare lo strumento.

<!-- Remove this text after dedicated instances have the Developer Console: end -->

## Passo 2: crea una capacità di dialogo
{: #getting-started-add-skill}

Il primo passo da compiere nello strumento  {{site.data.keyword.conversationshort}} è creare una capacità. 

Una *capacità di dialogo* è un contenitore per le risorse che definiscono il flusso di una conversazione che il tuo assistente può avere con i tuoi clienti. 

1.  Dalla home page dello strumento {{site.data.keyword.conversationshort}}, fai clic su **Create a Skill**.

    ![Mostra il pulsante Add skill dalla home page](images/gs-new-skill.png)

1.  Fai clic su **Create new**.

    ![Mostra il pulsante Create new dalla pagina Skills](images/gs-click-create-new.png)

1.  Assegna il nome `Conversational skill tutorial` alla tua capacità.
1.  **Facoltativo**. Se il dialogo che intendi creare utilizzerà una lingua diversa dall'inglese, scegli la lingua corretta dall'elenco.
1.  Fai clic su **Create**.

    ![Completa la creazione della capacità](images/gs-add-skill-done.png)

Vieni portato alla pagina Intents dello strumento.

## Passo 3: aggiungi gli intenti da un catalogo di contenuto
{: #getting-started-add-catalog}

Aggiungi i dati di addestramento che sono stati creati da IBM alla tua capacità aggiungendo gli intenti da un catalogo di contenuto. In particolare, fornirai al tuo assistente l'accesso al catalogo di contenuto **General** in modo tale che il tuo dialogo possa salutare gli utenti e terminare le conversazioni con loro. 

1.  Nello strumento {{site.data.keyword.conversationshort}}, fai clic sulla scheda **Content Catalog**.
1.  Trova **General** nell'elenco e poi fai clic su **Add to skill**.

    ![Mostra il catalogo di contenuto ed evidenzia il pulsante Add per il catalogo General.](images/gs-add-general-catalog.png)
1.  Apri la scheda **Intents** per esaminare gli intenti e le espressioni di esempio associate che sono stati aggiunti ai tuoi dati di addestramento. Puoi riconoscerli perché ciascun nome intento inizia con il prefisso `#General_`. Aggiungerai gli intenti `#General_Greetings` e `#General_Ending` al tuo dialogo nel passo successivo. 

    ![Mostra gli intenti visualizzati nella scheda Intents dopo aver aggiunto il catalogo General.](images/gs-general-added.png)

Hai iniziato a creare correttamente i tuoi dati di addestramento aggiungendo il contenuto precostruito da {{site.data.keyword.IBM_notm}}.

## Passo 4: crea un dialogo
{: #getting-started-build-dialog}

Un [dialogo](/docs/services/assistant?topic=assistant-dialog-overview) definisce il flusso della tua conversazione sotto forma di una struttura ad albero logica. Mette in corrispondenza gli intenti (cosa dicono gli utenti) con le risposte (cosa risponde il bot). Ogni nodo della struttura ad albero ha una condizione che lo attiva, in base all'input utente.

Creiamo un semplice dialogo che gestisce gli intenti greeting ed ending, ognuno con un singolo nodo. 

### Aggiunta di un nodo iniziale

1.  Nello strumento {{site.data.keyword.conversationshort}}, fai clic sulla scheda **Dialog**.
1.  Fai clic su **Create**. Vedrai due nodi:
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

- Fai clic sull'icona ![Provalo](images/ask_watson.png) per aprire il riquadro "Provalo". Dovresti vedere il tuo messaggio di benvenuto.

### Aggiunta di nodi per gestire gli intenti

Adesso aggiungiamo i nodi tra il nodo `Welcome` e il nodo `Anything else` che gestiscono i nostri intenti. 

1.  Fai clic sull'icona More ![Altre opzioni](images/kabob.png) sul nodo **Welcome** e seleziona quindi **Add node below**.
1.  Immetti `#General_Greetings` nel campo **Enter a condition** di questo nodo. Quindi, seleziona l'opzione **`#General_Greetings`**.
1.  Aggiungi la risposta, `Good day to you!`
1.  Fai clic su ![Chiudi](images/close.png) per chiudere la vista di modifica.

   ![Un nodo welcome generale è stato aggiunto al dialogo.](images/gs-add-greeting-node.png)

1.  Fai clic sull'icona More ![Altre opzioni](images/kabob.png) su questo nodo e seleziona quindi **Add node below** per creare un nodo peer. Nel nodo peer, specifica `#General_Ending` come condizione e `OK. See you later.
` come risposta.

   ![Aggiunta di un nodo ending al dialogo.](images/gs-add-ending-node.png)

1.  Fai clic su ![Chiudi](images/close.png) per chiudere la vista di modifica.

   ![Mostra che anche un nodo ending generale è stato aggiunto al dialogo.](images/gs-ending-added.png)

### Test del riconoscimento degli intenti

Hai creato un semplice dialogo per riconoscere e rispondere agli input greeting ed ending. Vediamo come funziona.

1.  Fai clic sull'icona ![Provalo](images/ask_watson.png) per aprire il riquadro "Provalo". Viene visualizzato il messaggio di benvenuto.
1.  Nella parte inferiore del riquadro, immetti `Hello` e premi Invio. L'output indica che l'intento #hello è stato riconosciuto e viene visualizzata la risposta appropriata (`Good day to you.`).
1.  Prova il seguente input:
    - `bye`
    - `howdy`
    - `see ya`
    - `good morning`
    - `sayonara`

![Test del dialogo nel riquadro Provalo](images/gs-try-it.gif){: gif}

{{site.data.keyword.watson}} può riconoscere i tuoi intenti anche quando il tuo input non corrisponde esattamente agli esempi che hai incluso. Il dialogo utilizza gli intenti per identificare lo scopo dell'input utente indipendentemente dall'esatta formulazione utilizzata e quindi risponde nel modo che hai specificato.

### Risultato della creazione di un dialogo

Questo è tutto. Hai creato una semplice conversazione con due intenti e un dialogo per riconoscerli.

## Passo 5: crea un assistente
{: #getting-started-create-assistant}

Un [*assistente*](/docs/services/assistant?topic=assistant-assistants) è un bot cognitivo a cui aggiungi una capacità per consentirgli di interagire con i tuoi clienti in modo utile. 

1.  Fai clic sulla scheda **Assistants**.
1.  Fai clic su **Create new**.

    ![Pulsante Create new nella scheda Assistant](images/gs-create-assistant.png)
1.  Assegna all'assistente il nome `Watson Assistant tutorial`.
1.  Nel campo Description, immetti `This is a sample assistant that I am creating to help me learn.`
1.  Fai clic su **Create**.

    ![Completamento della creazione del nuovo assistente](images/gs-create-assistant-done0.png)

## Passo 6: aggiungi la tua capacità al tuo assistente
{: #getting-started-add-skill-to-assistant}

Aggiungi la capacità di dialogo che hai generato all'assistente che hai creato. 

1.  Dalla pagina del nuovo assistente, fai clic su **Add skill**.

    Se hai creato o ti è stato fornito l'accesso del ruolo di sviluppatore agli spazi di lavoro che sono stati creati con la versione generalmente disponibile del servizio {{site.data.keyword.conversationshort}}, li vedrai elencati nella pagina Skills come capacità di conversazione.
    {: tip}

    ![Mostra il pulsante Add skill dalla pagina Assistant](images/gs-add-skill.png)
1.  Scegli di aggiungere la capacità che hai creato in precedenza al tuo assistente. 

## Passo 7: integra l'assistente
{: #getting-started-integrate-assistant}

Ora che hai un assistente che può partecipare a uno scambio colloquiale semplice, pubblicalo su una pagina web pubblica in cui puoi provarlo. Il servizio fornisce un'integrazione integrata detta Preview Link. Quando crei questo tipo di integrazione, il tuo assistente viene creato in un widget della chat ospitato da una pagina web con marchio IBM. Puoi aprire la pagina web e conversare con il tuo assistente per provarlo. 

1.  Fai clic sulla scheda **Assistants**, trova l'assistente `Watson Assistant tutorial` che hai creato e aprilo.
1.  Dall'area *Integrations*, fai clic su **Add integration**.
1.  Trova **Preview Link** e fai clic su **Select integration**.

1.  Fai clic sull'URL visualizzato nella pagina. 

    La pagina si apre in una nuova scheda.
1.  Dì `hello` al tuo assistente e guardalo rispondere. Puoi condividere l'URL con altri utenti che potrebbero voler provare il tuo assistente. 

## Passi successivi
{: #getting-started-next-steps}

Questa esercitazione si basa su un esempio semplice. Per un'applicazione reale, devi definire intenti più interessanti, alcune entità e un dialogo più complesso che utilizzi entrambi. Quando hai una versione che ti soddisfa del tuo assistente, puoi integrarlo con i canali utilizzati dai tuoi clienti, ad esempio Slack. Man mano che il traffico tra l'assistente e i tuoi clienti aumenta, puoi utilizzare gli strumenti forniti nella scheda **Analytics** per analizzare le conversazioni reali e identificare le aree per il miglioramento. 

- Completa le esercitazioni successive che creano dialoghi più avanzati:
    - Aggiungi nodi standard con l'esercitazione [Creazione di un dialogo complesso](/docs/services/assistant?topic=assistant-tutorial).
    - Acquisisci ulteriori informazioni sugli slot con l'esercitazione [Aggiunta di un nodo con slot](/docs/services/assistant?topic=assistant-tutorial-slots).
- Controlla altre [applicazioni di esempio](/docs/services/assistant?topic=assistant-sample-apps) per farti delle idee.
