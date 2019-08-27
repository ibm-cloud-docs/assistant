---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

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

# Integrazione con Intercom ![Solo piano Plus o Premium](images/plus.png)
{: #deploy-intercom}

Intercom è una piattaforma di messaggistica del cliente che aiuta a guidare la crescita aziendale tramite relazioni migliori in tutto il ciclo di vita del cliente.
{: shortdesc}

Intercom ha collaborato con IBM per aggiungere un nuovo agent al team di supporto del cliente, un Watson Assistant virtuale. Puoi integrare il tuo assistente con un'applicazione Intercom per consentire all'applicazione di passare senza interruzioni le conversazioni utente tra il tuo assistente e gli operatori. Leggi il [Post del blog Watson ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://medium.com/@blakemcgregor/contact-center-post-394dff427c8) per ulteriori informazioni sull'integrazione.

Questa integrazione è disponibile solo per gli utenti con piano Plus o Premium. Se vuoi provarlo, puoi registrarti per un piano Plus Trial gratuito. [Get Plus Trial](https://cloud.ibm.com/registration?target=%2Fdeveloper%2Fwatson%2Flaunch-tool%2Fconversation%3Fplan%3Dplus-trial&cm_mmc=OSocial_Voicestorm-_-Watson+AI_Watson+Core+-+Conversation-_-WW_WW-_-Intercom+Trial+Registration+Link&cm_mmca1=000027BD&cm_mmca2=10004432).
{: note}

Se integri l'assistente con Intercom, l'applicazione Intercom diventa l'applicazione rivolta al client per la tua capacità. Tutte le interazioni con gli utenti vengono avviate e gestite da Intercom.

Al momento non esiste un modo per passare una conversazione in corso da un canale di integrazione a un altro.

## Creazione di un agent monouso
{: #deploy-intercom-account-prereq}

È necessario che tu o qualcuno della tua organizzazione completi questi passi prerequisiti monouso prima di aggiungere l'integrazione Intercom al tuo assistente.

1.  Crea un account email funzionante per il tuo assistente.

    Ciascun assistente deve avere un indirizzo email univoco e valido prima di poter essere aggiunto a un team in Intercom.
1.  Dal tuo spazio di lavoro Intercom, aggiungi l'assistente al tuo team come un nuovo agent.

    Vai alla pagina delle impostazioni del membro del team nel tuo spazio di lavoro Intercom, invita l'assistente come un nuovo agent aggiungendo l'indirizzo email che hai creato nel passo precedente.

    Se non hai ancora uno spazio di lavoro Intercom configurato, creane uno all'indirizzo [www.intercom.com ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.intercom.com). Come minimo, hai bisogno di una sottoscrizione al prodotto *Inbox* di Intercom per poter creare uno spazio di lavoro.

1.  Dall'account email dell'assistente che hai creato in precedenza, trova l'invito da parte di Intercom. Fai clic su link nella email per unirti al team. Esegui l'accesso utilizzando l'indirizzo email funzionante dell'assistente e poi unisciti al team.

1.  **Facoltativo**: aggiorna il profilo del tuo assistente.

    Puoi modificare il nome e l'immagine del profilo del tuo assistente. Questo profilo rappresenta l'assistente nelle comunicazioni agent private all'interno del tuo spazio di lavoro e nelle interazioni pubbliche con i clienti tramite le tue applicazioni Intercom. Crea un profilo che rifletta il tuo marchio.

    Fai clic sul profilo Intercom nella barra di navigazione per accedere alle impostazioni del profilo e dello spazio di lavoro.

    ![Immagine della pagine delle impostazioni di Intercom.](images/intercom-settings.png)

## Preparazione del dialogo
{: #deploy-intercom-dialog-prereq}

Se non hai una capacità di dialogo associata al tuo assistente, creane o aggiungine una al tuo assistente in questo momento. Per ulteriori dettagli, vedi [Creazione di un dialogo](/docs/services/assistant?topic=assistant-dialog-build).

L'attivazione di una ricerca tramite una capacità di ricerca non è al momento supportata da un'integrazione Intercom.
{: note}

Completa questi passi nella tua capacità di dialogo in modo che l'assistente possa gestire le richieste utente e possa passare la conversazione a un operatore quando un cliente ne richiede uno.

1.  Aggiungi un intento alla tua capacità che possa riconoscere la richiesta di un utente di poter parlare con una persona.

    Puoi creare il tuo intento o aggiungerne uno precostruito denominato `#General_Connect_to_Agent` che viene fornito con il catalogo di contenuto **General** sviluppato da IBM.

1.  Aggiungi un nodo root al tuo dialogo che condizioni l'intento che hai creato nel passo precedente. Scegli **Connect to human agent** come tipo di risposta.

1.  Prepara ciascun ramo di dialogo per essere attivato dall'assistente dall'applicazione Intercom.

    Ogni nodo di dialogo root nel dialogo può essere elaborato dall'assistente mentre sta funzionando come un membro del team Intercom, inclusi i nodi root nelle cartelle. Specificherai l'azione che desideri venga eseguita dall'assistente per ciascun ramo di dialogo in un secondo momento, quando configurerai l'integrazione Intercom. Pertanto, sebbene tu non possa nascondere un nodo da Intercom, puoi configurare l'assistente in modo che non esegua alcuna operazione quando viene attivato il nodo.

    Riempi i seguenti campi del nodo root di ciascun ramo di dialogo:

    - **Node name**: assegna un nome al nodo. Questo nome è il modo in cui identificherai il nodo quando successivamente, configurerai le interazioni relative ad esso. Se non aggiungi un nome, dovrai scegliere il nodo in base al suo ID nodo.
    - **External node name**: aggiungi un riepilogo dello scopo del ramo di dialogo. Ad esempio, *Find a store*.

      Queste informazioni vengono mostrate agli altri agent nel team dell'assistente quando l'assistente si offre di rispondere a una query utente. Se è presente più di un nodo di dialogo che può far fronte alla query, l'assistente condivide un elenco delle opzioni di risposta con i membri del team dell'operatore per avere i loro suggerimenti su quale risposta utilizzare.

      ![Acquisizione di schermo del campo nella vista di modifica del nodo in cui aggiungi il riepilogo dello scopo del nodo.](images/disambig-node-purpose.png)

      **Non** aggiungere un nome nodo esterno al nodo root che hai creato nel Passo 2. Quando si verifica un'escalation, il tuo assistente esamina il nome del nodo esterno dell'ultimo nodo elaborato per apprendere quale obiettivo utente non è stato soddisfatto correttamente. Se includi un nome nodo esterno nel nodo con la connessione all'intento dell'operatore, impedirai al tuo assistente di comprendere l'ultimo nodo reale orientato all'obiettivo con cui l'utente ha interagito prima dell'escalation del problema.
      {: tip}

1.  Se è presente un nodo figlio nelle condizioni del ramo in una richiesta o domanda di follow-up che non vuoi che venga gestita dall'assistente, aggiungi un tipo di risposta **Connect to human agent** al nodo.

    Ad esempio, potresti voler aggiungere questo tipo di risposta ai nodi che trattano problemi sensibili che solo una persona dovrebbe gestire o che tengono traccia quando un assistente non riesce a comprendere ripetutamente un utente.

    Nel runtime, se la conversazione raggiunge questo nodo figlio, a questo punto il dialogo viene passato ad un operatore. Successivamente, quando configuri l'integrazione Intercom, puoi scegliere un operatore come un backup per ciascun ramo.

Ora il tuo dialogo è pronto per supportare il tuo assistente in Intercom.

### Considerazioni sul dialogo
{: #deploy-intercom-dialog}

Alcune risposte esaurienti che aggiungi a un dialogo vengono visualizzate in modo diverso nel riquadro "Try it out" rispetto a quando vengono visualizzate agli utenti Intercom. La tabella riportata di seguito descrive in che modo i tipi di risposta vengono gestiti da Intercom.

| Tipo di risposta | Come vengono visualizzate agli utenti Intercom  |
|---------------|---------------------------|
| **Opzione**    | Le opzioni vengono visualizzate come un elenco numerato. Nei campi del **titolo** o della **descrizione**, fornisci le istruzioni che spiegano all'utente in che modo scegliere un'opzione dall'elenco. |
| **Immagine**     | Viene eseguito il rendering del **titolo**, della **descrizione** e dell'immagine stessa. |
| **Pausa**     | A prescindere dal fatto che lo abiliti oppure no, durante la pausa non viene visualizzato un indicatore di digitazione. |

Per ulteriori informazioni sui tipi di risposta, vedi [Risposte esaurienti](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia).

## Aggiunta di un'integrazione Intercom
{: #deploy-intercom-add-intercom}

1.  Dalla scheda Assistants, fai clic per aprire il tile dell'assistente che desideri distribuire.

1.  Dalla sezione Integrations, fai clic su **Add integration**.

1.  Fai clic su **Intercom**.

    Segui le istruzioni fornite a video. Le seguenti sezioni costituiscono un supporto per l'esecuzione dei passi.

Il seguente video di 4 minuti illustra la procedura. 

<iframe class="embed-responsive-item" id="youtubeplayer" title="Configurazione rapida" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/SkbFWNScueU" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Collegamento dell'assistente a Intercom
{: #deploy-intercom-connect}

Non appena concedi a Intercom l'autorizzazione a utilizzare l'assistente, quest'ultimo diventa un membro di supporto del team Intercom.

Gli operatori possono assegnare messaggi all'assistente utilizzando le regole di assegnazione di Intercom. I messaggi possono essere assegnati all'assistente nei seguenti modi: 

- Assegnazione automatica delle conversazioni in entrata a un membro del team o alla cartella della posta in arrivo del team in base ad alcuni criteri

  Il seguente video di un minuto e mezzo illustra la procedura.

  <iframe class="embed-responsive-item" id="youtubeplayer2" title="Assegnazione automatica" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/4M9wu8NHxcY" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

- Riassegnazione manuale effettuata da un operatore nel runtime.

  Il seguente video inferiore a 3 minuti illustra la procedura. 

  <iframe class="embed-responsive-item" id="youtubeplayer3" title="Assegnazione manuale" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/jAnolyUJAIA" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Per ulteriori dettagli, vedi la [documentazione Intercom ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.intercom.com/help/support-and-retain-customers/work-as-a-team/assign-conversations-to-teammates-and-teams).

1.  Quando il tuo dialogo è pronto, fai clic su **Connect now**.
1.  Fai clic su **Access Intercom** per essere reindirizzato al sito Intercom.

     Accedi a Intercom utilizzando l'indirizzo email funzionante e la password dell'assistente, non i tuoi. Desideri stabilire la connessione a un ID funzionale che sia condiviso da più persone nella tua organizzazione.
     {: important}

1.  Fai clic su **Authorize access**.
1.  Fai clic su **Back to overview**.

Ora l'assistente è disponibile a ricevere le assegnazioni dai membri del team Intercom. Se non lo hai ancora fatto, [configura il tuo dialogo](#deploy-intercom-dialog-prereq).
{: important}

## Configurazione dell'instradamento del messaggio
{: #deploy-intercom-config-backup}

Assegna membri umani del team come backup per l'assistente nel caso in cui questo debba trasferire una conversazione in corso ad una persona. Puoi scegliere un team o un membro del team diverso in modo che sia il contatto di backup per ciascun ramo di dialogo.

Per configurare le assegnazioni di instradamento per le escalation dall'assistente a una persona, completa i seguenti passi:

1.  Dalla pagina di integrazione di Intercom, fai clic su **My Dialog Skill is ready** per confermare che hai preparato il tuo dialogo.

    Fai clic su questo pulsante solo se hai completato la procedura [Preparazione del dialogo](#deploy-intercom-dialog-prereq).
    {: important}

1.  Nella sezione *Settings*, fai clic su **Manage rules**.

    Se non hai apportato modifiche, il contatto di backup per tutti i nodi rimane non assegnato.

1.  Fai clic su **New rule**.

1.  Dall'elenco a discesa *Choose node*, scegli il nodo per il ramo di dialogo che desideri configurare.

    Ricorda, i rami sono identificati dal loro nome nodo. Se non hai specificato un nome nodo, verrà visualizzato l'ID del nodo.

1.  Scegli il team o un membro del team dell'operatore in modo sia il contatto di backup per questo ramo di dialogo. La query utente verrà inoltrata a questa persona se l'assistente non può rispondere alla query o raggiunge un nodo figlio con il tipo di risposta *Connect to human agent*, indicando che a rispondere dovrà essere solo una persona.

1.  Per definire le regole di instradamento per gli altri rami di dialogo, fai clic di nuovo su **New rule** e ripeti i passi precedenti.

    Non dimenticare di configurare un'assegnazione per i nodi root che hanno un tipo di risposta *Connect to human agent* in un nodo figlio nel loro ramo. Se non trasferisci il nodo root associato a una specifica persona o a un determinato team, un problema sensibile può essere trasferito alla cartella della posta in entrata *Unassigned*.

1.  Una volta aggiunte le regole, fai clic su **Return to overview** per uscire dalla pagina.

Il seguente video di 3 minuti illustra la procedura. 

<iframe class="embed-responsive-item" id="youtubeplayer0" title="Instradamento dell'escalation in base all'argomento" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/dTwJZOqdzII" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Concedi all'assistente l'autorizzazione per monitorare e rispondere alle query utente
{: #deploy-intercom-config-action}

Quando desideri che l'assistente inizi a monitorare una cartella della posta in arrivo Intercom e a rispondere ai messaggi da solo, attiva il monitoraggio.

Il tuo assistente controlla le domande utente mentre gli utenti sono collegati a Intercom. Quando l'assistente è sicuro di sapere la risposta a una query utente, risponde direttamente all'utente. (L'assistente è sicuro quando l'intento principale identificato dal tuo assistente ha un punteggio di affidabilità pari a 0,75 o superiore.) 

Se non desideri che l'assistente risponda a determinati tipi di query utente, puoi aggiungere regole per specificare altre azioni che devono essere eseguite dall'assistente per il ramo di dialogo. Ad esempio, potresti voler iniziare a incorporare l'assistente nel team Intercom in modo più prudente, consentendogli solo di suggerire le risposte mentre trasferisce i messaggi utente agli altri membri del team affinché rispondano. Nel corso del tempo, una volta che l'utente ha dato prova di se stesso, puoi affidargli più responsabilità.

Per configurare in che modo desideri che l'assistente gestisca specifici rami di dialogo, definisci le regole.

1.  Dalla pagina di integrazione Intercom, nella sezione *Enable your assistant to monitor an inbox*, **attiva** il monitoraggio.

1.  In *Settings*, fai clic su **Manage rules**.

    Se non definisci le regole, l'assistente viene configurato per monitorare la cartella della posta in entrata *Unassigned* e rispondere automaticamente alle domande utente a cui è sicuro di poter far fronte.

1.  Dal campo *Monitor Intercom inbox*, scegli la cartella della posta in entrata Intercom che desideri venga monitorata dall'assistente.

1.  Fai clic su **New rule** per definire un modello di interazione univoco per uno specifico ramo di dialogo.

1.  Dall'elenco a discesa *Choose node*, scegli il nodo per il ramo che desideri configurare.

    Ricorda, i rami sono identificati dal loro nome nodo. Se non hai specificato un nome nodo, verrà visualizzato l'ID del nodo.

1.  Seleziona il tipo di azione che desideri venga eseguito dall'assistente quando questo nodo di dialogo viene attivato. Le opzioni per il tipo di azione sono:

    - **Do nothing**: l'assistente non viene coinvolto nella risposta; il messaggio dell'utente rimane nella cartella della posta in entrata affinché qualcuno se ne occupi.
    - **Send to team or teammate**: l'assistente valuta l'input utente e ne determina l'obiettivo, quindi lo trasferisce al membro del team appropriato.
    - **Suggest to team or teammate**: l'assistente fornisce suggerimenti al membro del team su come rispondere condividendo le note con l'operatore tramite l'applicazione interna Intercom.

      - Se l'input utente attiva un ramo di dialogo, ossia un nodo di dialogo root con nodi figlio che rappresentano un'interazione completa, l'assistente indica che è in grado di far fronte alla richiesta e si offre di farlo.
      - Se l'input utente attiva un nodo root senza nodi figlio, l'assistente condivide semplicemente la risposta programmata dal nodo con l'operatore, ma non risponde direttamente all'utente.
      - Se l'input attiva più nodi di dialogo con affidabilità elevata, l'assistente mostra al membro del team umano un elenco delle possibili risposte e gli chiede di scegliere la risposta migliore.

      In ogni caso, l'operatore decide se consentire all'assistente di prendere il controllo.

    - **Answer**: l'assistente risponde all'utente direttamente, senza conferire con gli altri membri del team.

    Il coinvolgimento del membro del team è necessario per i nodi ai quali hai assegnato i tipi di azione *Send to team or teammate* o *Suggest to team or teammate*. Assicurati di tornare indietro e di assegnare una regola che assegni la persona corretta o il team corretto come backup per questo nodo di dialogo in particolare.

1.  Per definire le impostazioni di interazione univoche per gli altri rami di dialogo, fai clic di nuovo su **New rule** e ripeti i passi precedenti.

1.  Una volta aggiunte le regole, fai clic su **Return to overview** per uscire dalla pagina.

Man mano che il tuo dialogo cambia, probabilmente tornerai alla pagina di integrazione Intercom per apportare modifiche incrementali a queste regole.

Il seguente video di 3 minuti illustra la procedura. 

<iframe class="embed-responsive-item" id="youtubeplayer1" title="Monitoraggio posta in entrata" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/fFKjWUfIftw" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Test dell'integrazione
{: #deploy-intercom-try}

Per verificare efficacemente la tua integrazione Intercom da end-to-end, devi avere accesso a un'applicazione dell'utente finale Intercom. Hai già creato o modificato uno spazio di lavoro Intercom. Lo spazio di lavoro deve avere un client di interfaccia utente associato. Nel caso in cui non sia così, vedi [Apps in Intercom ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://www.intercom.com/help/apps-in-intercom){: new_window} per assistenza mentre ne configuri uno.

Inoltra le query utente di test tramite un'applicazione client associata al tuo spazio di lavoro Intercom per vedere in che modo i messaggi vengono gestiti da Intercom. Verifica che i messaggi a cui deve rispondere l'assistente stiano generando le risposte appropriate e che l'assistente non stia rispondendo a messaggi a cui non deve rispondere.
