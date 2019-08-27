---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-29"

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

# Modifica dell'impostazione del timeout di inattività ![Solo piano Plus o Premium](images/plus.png)
{: #assistant-settings}

Quando un utente interagisce con il tuo assistente tramite una delle integrazioni integrate, la sessione di chat termina una volta trascorso un periodo di tempo specifico durante il quale l'utente non interagisce con l'assistente. Puoi specificare la quantità di tempo di attesa prima che una sessione di chat venga reimpostata modificando l'impostazione del timeout di inattività.
{: shortdesc}

Quando una sessione di chat viene reimpostata, il dialogo perde le informazioni contestuali salvate durante il precedente scambio con l'utente. Ad esempio, se il dialogo chiede il nome dell'utente e poi lo chiama utilizzando tale nome per il resto della conversazione, una volta terminata la sessione di chat e iniziata una nuova sessione, il dialogo inizierà chiedendo di nuovo il nome dell'utente.

## Limiti di sessione
{: #assistant-settings-session-limits}

La lunghezza consentita per un timeout di inattività differisce a seconda del tipo di piano dell'istanza del servizio. La seguente tabella elenca i limiti. 

| Piano di servizio | Periodo di inattività predefinito sessione di chat | Periodo di inattività massimo sessione di chat |
|--------------|--------------------------------:|----------------------------:|
| Premium      |                      60 minuti |                    24 ore  |
| Plus         |                      60 minuti |                    24 ore  |
| Standard     |                       5 minuti |                   5 minuti |
| Plus Trial   |                       5 minuti |                   5 minuti |
| Lite*        |                       5 minuti |                   5 minuti |
{: caption="Dettagli piano di servizio" caption-side="top"}

## Modifica dell'impostazione
{: #assistant-settings-timeout-task}

1.  Fai clic sul menu relativo al tuo assistente e poi scegli **Settings**.

1.  Modifica il tempo specificato nel campo **Timeout limit**.

1.  Chiudi la pagina delle impostazioni. Le tue modifiche vengono salvate automaticamente. 
