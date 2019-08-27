---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-29"

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

# Integrazione con Slack
{: #deploy-slack}

Slack è un'applicazione di messaggistica basata su cloud che aiuta le persone a collaborare tra loro.
{: shortdesc}

Una volta configurata una capacità di dialogo e averla aggiunta a un assistente, puoi integrare l'assistente con Slack.

Una volta integrato, a seconda degli eventi che hai configurato per essere supportati dall'assistente, il tuo assistente può rispondere alle domande poste in messaggi diretti o in canali in cui l'assistente viene menzionato direttamente.  

## Aggiunta dell'integrazione Slack
{: #deploy-slack-task}

1.  Dalla scheda Assistants, fai clic per aprire il tile dell'assistente che desideri distribuire.

1.  Dalla sezione Integrations, fai clic su **Add integration**.

1.  Fai clic su **Slack**.

1.  Segui le istruzioni fornite a video per completare il processo di integrazione.

    Scegli uno o più dei seguenti eventi di sottoscrizione da supportare: 

    - `message.im`: l'assistente risponde quando un utente inizia un messaggio diretto con l'assistente. 
    - `app_mentions`: l'assistente risponde quando un utente cita l'assistente per nome in un canale. 

Se desideri seguire un altro utente che esegue i passi di distribuzione, guarda questo video.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Guida dettagliata dei passi di distribuzione di Slack" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/RBGBPJ8h4HQ?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Durata: 9.5 minuti

## Considerazioni sul dialogo
{: #deploy-slack-dialog}

Le risposte esaurienti che aggiungi a un catalogo vengono visualizzate in un canale Slack come previsto, con le seguenti eccezioni:

- **Connect to human agent**: questo tipo di risposta viene ignorato.

- **Image**: un titolo dell'immagine è obbligatorio. Se non fornisci un titolo, l'immagine non viene visualizzata.

- **Option**: questo tipo di risposta mostra un elenco delle opzioni tra cui può scegliere l'utente.

  - Un titolo è **obbligatorio** e viene visualizzato prima dell'elenco di opzioni.
  - Non viene visualizzata una descrizione, indipendentemente dal fatto che tu l'abbia specificata o meno.
  - Una volta che un utente fa clic su uno dei pulsanti, le scelte del pulsante scompaiono e vengono sostituite dall'input utente generato dalle scelte dell'utente. Se includi più tipi di risposta in una singola risposta, posiziona il tipo di risposta dell'opzione per ultimo. Diversamente, l'output conterrà una combinazione di risposte e di input utente che potrebbe confondere l'utente.

- **Pause**: questo tipo di risposta sospende l'attività dell'assistente nel canale Slack. Tuttavia, non vengono mostrati indicatori visibili per indicare che l'assistente è in pausa, indipendentemente dal fatto che tu abbia scelto o meno di mostrare l'indicatore di digitazione.

Per ulteriori informazioni sui tipi di risposta, vedi [Risposte esaurienti](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia).

## Chat con l'assistente
{: #deploy-slack-try}

Per avviare una chat con l'assistente, completa i seguenti passi:

1.  Apri Slack e vai allo spazio di lavoro associato alla tua applicazione.
1.  Fai clic sull'applicazione che hai creato dalla sezione Apps.
1.  Avvia una chat con l'assistente.

Il nodo di benvenuto del tuo dialogo non viene elaborato dell'integrazione Slack. Il messaggio di benvenuto non viene visualizzato nel canale Slack così come appare nel riquadro "Try it out" o nella pagina web di integrazione Preview Link. Non viene attivato da qui perché i nodi con la condizione speciale `welcome` vengono ignorati nei flussi di dialogo che vengono avviati dagli utenti. Slack attende che l'utente inizi la conversazione. Se hai bisogno di impostare valori predefiniti per le variabili di contesto all'avvio della tua conversazione, non impostarli nel nodo di benvenuto. Per ulteriori informazioni, vedi [Avvio del dialogo](/docs/services/assistant?topic=assistant-dialog-start).
{: note}

Il flusso di dialogo per la sessione corrente viene riavviato dopo 60 minuti di inattività (5 minuti per i piani Lite e Standard). Ciò significa che se un utente smette di interagire con l'assistente, dopo 60 (o 5) minuti, i valori delle variabili di contesto impostati durante la conversazione precedente vengono impostati su null o riportati ai loro valori predefiniti.
