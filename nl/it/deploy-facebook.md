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
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Integrazione con Facebook Messenger
{: #deploy-facebook}

Facebook Messenger è un'applicazione di messaggistica mobile che aiuta le aziende e i clienti a comunicare direttamente tra loro.
{: shortdesc}

Una volta configurata una capacità di dialogo e averla aggiunta a un assistente, puoi integrare l'assistente con Facebook Messenger.

Al momento non ci sono meccanismi per identificare gli utenti che interagiscono con l'assistente tramite Facebook Messenger, ciò significa che non esiste un modo per identificare o eliminare i dati associati a uno specifico utente. Non utilizzare questo metodo di integrazione per le distribuzioni che devono essere conformi a GDPR. Per ulteriori dettagli, vedi [Sicurezza delle informazioni](/docs/services/assistant?topic=assistant-information-security).
{: important}

1.  Dalla scheda Assistants, fai clic per aprire il tile dell'assistente che desideri distribuire. 

1.  Dalla sezione Integrations, fai clic su **Add Integration**.

1.  Fai clic sul pulsante **Select Integration** per *Facebook Messenger*.

1.  Segui le istruzioni fornite a video per completare il processo di integrazione. 

Se desideri seguire un altro utente che esegue i passi di distribuzione, guarda questo video.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Guida dettagliata dei passi di distribuzione di Facebook" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/8o-FFU5sYNM?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Durata: 8 minuti

## Considerazioni sul dialogo
{: #deploy-facebook-dialog}

Le risposte esaurienti che aggiungi a un catalogo vengono visualizzate in un'applicazione Facebook come previsto, con le seguenti eccezioni: 

- **Connect to human agent**: questo tipo di risposta viene ignorato.

- **Image**: questo tipo di risposta incorpora un'immagine nella risposta. Non vengono visualizzati un titolo e una descrizione prima dell'immagine, indipendentemente dal fatto che tu li abbia specificati o meno. 

- **Option**: questo tipo di risposta mostra un elenco delle opzioni tra cui può scegliere l'utente. 

  - Un titolo è **obbligatorio** e viene visualizzato prima dell'elenco di opzioni. 
  - Non viene visualizzata una descrizione, indipendentemente dal fatto che tu l'abbia specificata o meno. 
  - Una volta che un utente fa clic su uno dei pulsanti, le scelte del pulsante scompaiono e vengono sostituite dall'input utente generato dalle scelte dell'utente. Se l'assistente o l'utente immettono nuovo input, l'input generato dal pulsante scompare. Quindi, se includi più tipi di risposta in una singola risposta, posiziona il tipo di risposta dell'opzione per ultimo. Diversamente, il contenuto delle risposte successive, come ad esempio il testo di un tipo di risposta di testo, verrà sostituito dal testo generato dal pulsante. 

- **Pause**: questo tipo di risposta sospende l'attività dell'assistente nel Messenger. Tuttavia, l'attività non riprende dopo la pausa a meno non venga attivato un altro tipo di risposta dopo di essa. Ogni volta che includi questo tipo di risposta, aggiungi un altro tipo di risposta diverso, ad esempio una risposta di testo, e posizionalo dopo questo. 

Per ulteriori informazioni sui tipi di risposta, vedi [Risposte esaurienti](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia).

## Chat con l'assistente
{: #deploy-facebook-try}

Per avviare una chat con l'assistente, completa i seguenti passi: 

1.  Apri Facebook Messenger.
1.  Immetti il nome della pagina che hai creato in precedenza. 
1.  Una volta visualizza la pagina, fai clic su di essa e inizia a parlare con l'assistente. 

Il nodo di benvenuto del tuo dialogo non viene elaborato dell'integrazione Facebook Messenger. Il messaggio di benvenuto non viene visualizzato nella chat di Facebook così come appare nel riquadro "Provalo" all'interno dello strumento o nella pagina web di integrazione Preview Link. Non viene attivato da qui perché i nodi con la condizione speciale `welcome` vengono ignorati nei flussi di dialogo che vengono avviati dagli utenti. Facebook Messenger attende che l'utente inizi la conversazione. Se hai bisogno di impostare valori predefiniti per le variabili di contesto all'avvio della tua conversazione, non impostarli nel nodo di benvenuto. Per ulteriori informazioni, vedi [Avvio del dialogo](/docs/services/assistant?topic=assistant-dialog-start).
{: note}

Il flusso di dialogo per la sessione corrente viene riavviato dopo 60 minuti di inattività (5 minuti per i piani Lite e Standard). Ciò significa che se un utente smette di interagire con l'assistente, dopo 60 (o 5) minuti, i valori delle variabili di contesto impostati durante la conversazione precedente vengono impostati su null o riportati ai loro valori predefiniti. 
