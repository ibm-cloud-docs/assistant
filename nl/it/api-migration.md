---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-30"

subcollection: assistant


---

{:curl: #curl .ph data-hd-programlang='curl'}
{:external: target="_blank" .external}
{:javascript: #javascript .ph data-hd-programlang='javascript'}
{:java: #java .ph data-hd-programlang='java'}
{:python: #python .ph data-hd-programlang='python'}
{:swift: data-hd-programlang='swift'}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:download: .download}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# Migrazione all'API v2
{: #api-migration}

L'API runtime v2 di Assistant, che supporta l'uso degli assistenti e delle capacità, è stata introdotta nel novembre 2018. Questa API offre vantaggi significativi rispetto all'API runtime v1, inclusi la gestione automatica dello stato, la facilità di distribuzione, il controllo della versione della capacità e la disponibilità di nuove funzioni come la capacità di ricerca. 

L'API v2 è disponibile per tutti gli utenti, indipendentemente dal piano di servizio e senza costi aggiuntivi. 

Al momento, l'API v2 supporta solo l'interazione di runtime con un assistente esistente. Le applicazioni di creazione che creano o modificano gli spazi di lavoro devono continuare a utilizzare l'API v1.
{: note}

## Panoramica

Con l'API v2, la tua applicazione client comunica con un assistente invece che direttamente con uno spazio di lavoro. Un assistente è un nuovo livello di orchestrazione che offre diverse nuove funzionalità, inclusi la gestione automatica dello stato, il controllo della versione della capacità, una distribuzione più semplice e (per i piani Plus e Premium) le capacità di ricerca. Il tuo spazio di lavoro esistente (ora indicato come una _capacità di dialogo_) continua a funzionare come prima, ma le nuove funzionalità vengono fornite dal nuovo livello di assistente.

Tutte le comunicazioni con un assistente si svolgono all'interno del contesto di una _sessione_, che mantiene lo stato della conversazione per tutta la durata della conversazione. I dati sullo stato, incluse le variabili di contesto definite dal tuo dialogo o dalla tua applicazione client, vengono archiviati automaticamente da {{site.data.keyword.conversationshort}}, senza alcuna azione richiesta da parte della tua applicazione. 

I dati sullo stato vengono conservati fino a quando non elimini esplicitamente la sessione o fino a quando non si verifica il timeout a causa dell'inattività. 

Se hai un'applicazione esistente che utilizza l'API v1 per inviare l'input utente direttamente a uno spazio di lavoro, la migrazione della tua applicazione per utilizzare l'API v2 è un processo semplice.

## Configura un assistente

L'API runtime v2 invia i messaggi a un assistente che instrada i messaggi alla tua capacità di dialogo (in precedenza spazio di lavoro). Per configurare un assistente, utilizza l'interfaccia utente {{site.data.keyword.conversationshort}}:

1. Fai clic sulla scheda **Skills**. Verifica che il tuo spazio di lavoro venga visualizzato come una capacità disponibile. (Tutti gli spazi di lavoro esistenti per la tua istanza del servizio vengono convertiti automaticamente in capacità nell'interfaccia utente {{site.data.keyword.conversationshort}}. Questa conversione non apporta alcuna modifica allo spazio di lavoro sottostante.)

1. Fai clic sulla scheda **Assistants**. Fai clic su **Create assistant** per creare un nuovo assistente. Quando ti viene richiesto di aggiungere le capacità, fai clic su **Add dialog skill** e seleziona la capacità di dialogo che corrisponde al tuo spazio di lavoro. 

  Per ulteriori informazioni sulla creazione degli assistenti, vedi [Creazione di un assistente](https://cloud.ibm.com/docs/services/assistant?topic=assistant-assistant-add).

1. Una volta creato il tuo nuovo assistente, fai clic sul menu ![Menu](images/kebab-react.png) e poi seleziona **Settings**.

1. Nella pagina **Assistant Settings**, trova l'ID assistente. La tua applicazione utilizzerà questo ID (invece di un ID spazio di lavoro) per comunicare con l'assistente. Le credenziali del servizio sono le stesse sia per l'API v1 che per quella v2. 

  Al momento, non esiste supporto API per richiamare un ID assistente. Per trovare l'ID assistente, devi utilizzare l'interfaccia utente {{site.data.keyword.conversationshort}}.
  {: note}

## Richiama l'API runtime v2

Dopo aver creato un assistente, puoi aggiornare la tua applicazione client in modo che utilizzi l'API runtime v2 invece dell'API runtime v1. 

1. Prima di inviare il primo messaggio in una conversazione, utilizza il metodo v2 [**Create a session**](https://cloud.ibm.com/apidocs/assistant-v2#create-a-session){: external} per creare una sessione. Salva l'ID sessione restituito:

  ```javascript
  service
  .createSession({
    assistant_id: assistantId,
  })
  .then(res => {
    sessionId = res.session_id;
  })
  ```
  {: codeblock}
  {: javascript}

  ```python
  session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']
  ```
  {: codeblock }
  {: python }

  ```java
CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute().getResult();
    String sessionId = session.getSessionId();

    ```
  {: codeblock}
  {: java}

1. Utilizza il metodo v2 [**Send user input to assistant**](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant){: external} per inviare l'input utente all'assistente. Invece di specificare l'ID spazio di lavoro come facevi con l'API v1, specifica l'ID assistente e l'ID sessione: 

  ```javascript
  service
    .message({
      assistant_id: assistantId,
      session_id: sessionId,
      input: messageInput
    })
  ```
  {: codeblock}
  {: javascript}

  ```python
  response = service.message(
      assistant_id,
      session_id,
      input = message_input
  ).get_result()
  ```
  {: codeblock}
  {: python}

  ```java
  MessageInput input = new MessageInput.Builder().text(inputText).build();
      MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId)
    .input(input)
    .build();
  MessageResponse response = service.message(messageOptions)
    .execute()
    .getResult();
  ```
  {: codeblock}
  {: java}

  La struttura di base del messaggio rimane invariata; in particolare, l'input utente viene ancora inviato come `input.text`.

1. Una volta terminata una conversazione, utilizza il metodo v2 [**Delete session**](https://cloud.ibm.com/apidocs/assistant-v2#delete-session){: external} per eliminare la sessione. 

  ```javascript
  service
    .deleteSession({
      assistant_id: assistantId,
      session_id: sessionId,
    })
  ```
  {: codeblock}
  {: javascript}

  ```python
  service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
  ```
  {: codeblock}
  {: python}

  ```java
  DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId
    .build();
  service.deleteSession(deleteSessionOptions).execute();
  ```
  {: codeblock}
  {: java}

   Se non elimini esplicitamente la sessione, verrà eliminata automaticamente dopo l'intervallo di timeout configurato. (La durata del timeout varia a seconda del tuo piano; per ulteriori informazioni, vedi [Limiti di sessione](/docs/services/assistant?topic=assistant-assistant-settings#assistant-settings-session-limits).)

Per vedere gli esempi delle API v2 nel contesto di un'applicazione client semplice, vedi [Creazione di un'applicazione client](/docs/services/assistant?topic=assistant-api-client).

## Gestisci il formato di risposta v2

La tua applicazione potrebbe dover essere aggiornata per gestire il formato di risposta runtime v2 a seconda delle parti della risposta a cui la tua applicazione deve accedere: 

- L'output per tutti i tipi di risposta (ad esempio `text` e `option`) viene ancora restituito nell'oggetto `output.generic`. Il codice applicativo per gestire queste risposte dovrebbe funzionare senza modifiche. 

- Ora le entità e gli intenti rilevati vengono restituiti come parte dell'oggetto `output` piuttosto che nella root del JSON di risposta. 

- Ora il contesto della conversazione viene organizzato in due oggetti: 

  - Il **contesto globale** contiene dati di contesto a livello di sistema condivisi da tutte le capacità utilizzate dall'assistente. 

  - Il **contesto della capacità** contiene le variabili di contesto definite dall'utente utilizzate dalla tua capacità di dialogo. 

  Tuttavia, tieni presente che tali dati di stato, incluso il contesto della conversazione, vengono ora conservati dall'assistente, quindi la tua applicazione potrebbe non avere più bisogno di accedere al contesto (vedi [Lascia che l'assistente conservi lo stato](#api-migration-state)).

Fai riferimento alla [Guida di riferimento API ](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant){: external} v2 per la documentazione completa del formato della risposta v2.

## Lascia che l'assistente conservi lo stato
{: #api-migration-state}

Per la maggior parte delle applicazioni, ora puoi rimuovere il codice incluso a scopo di conservazione dello stato. Non è più necessario salvare il contesto e reinviarlo a {{site.data.keyword.conversationshort}} a ogni turno della conversazione. Il contesto viene conservato automaticamente da {{site.data.keyword.conversationshort}} e puoi accedervi dal tuo dialogo come prima. 

Tieni presente che con l'API v2, per impostazione predefinita, il contesto non viene incluso nelle risposte all'applicazione client. Tuttavia, il tuo codice può ancora accedere alle variabili di contesto se necessario: 

- Puoi ancora inviare un oggetto `context` come parte dell'input del messaggio. Tutte le variabili di contesto incluse vengono archiviate come parte del contesto conservato da {{site.data.keyword.conversationshort}}. (Se la variabile di contesto che invii esiste già nel contesto, il nuovo valore sovrascrive quello archiviato in precedenza.)

  Assicurati che l'oggetto context che invii sia conforme al formato v2. Tutte le variabili di contesto definite dall'utente inviate dalla tua applicazione devono fare parte del contesto della capacità; di norma, l'unica variabile di contesto globale che potresti dover impostare è `system.user_id` che viene  utilizzata dai piani Plus e Premium a scopo di fatturazione. 

- Puoi ancora richiamare le variabili di contesto dal contesto globale o della capacità. Affinché l'oggetto `context` venga incluso nelle risposte del messaggio, utilizza la proprietà **return_context** nelle opzioni di input del messaggio. Per ulteriori informazioni, vedi [Accesso ai dati di contesto](/docs/services/assistant?topic=assistant-api-client-get-context).
