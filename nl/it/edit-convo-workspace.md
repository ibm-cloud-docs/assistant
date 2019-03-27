---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

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

# Accesso agli spazi di lavoro
{: #edit-convo-workspace}

Se hai creato uno spazio di lavoro con una versione precedente del servizio (anche quando era noto come {{site.data.keyword.watson}} Conversation), puoi continuare a utilizzarlo.
{: shortdesc}

## Modifica dello spazio di lavoro della conversazione
{: #edit-convo-workspace-task}

Il tuo spazio di lavoro è ancora disponibile, solo che ora vi si fa riferimento come a una *capacità*. Per apportare modifiche a uno spazio di lavoro legacy, completa i seguenti passi: 

1.  Dalla home page di {{site.data.keyword.conversationshort}}, fai clic su **Skills**.

    Gli spazi di lavoro che hai creato con la versione generalmente disponibile del servizio {{site.data.keyword.conversationshort}} sono visualizzati come tile della capacità di dialogo.
1.  Fai clic sulla capacità di dialogo che rappresenta lo spazio di lavoro che desideri modificare. 
1.  Apporta le modifiche o le aggiunte che desideri ai dati di addestramento o al dialogo. 
1.  Salva le tue modifiche. 

Una volta completato l'addestramento, i tuoi aggiornamenti sono disponibili nell'applicazione personalizzata da cui stai richiamando il servizio. Per ulteriori informazioni, vedi [Panoramica sull'API Watson Assistant](/docs/services/assistant?topic=assistant-api-overview).

## Limitazioni
{: #edit-convo-workspace-cons}

Con l'ultima versione del servizio, puoi continuare ad eseguire tutte le operazioni disponibili con il servizio legacy ma con una maggiore flessibilità. Forse hai creato uno spazio di lavoro con una versione precedente del servizio e lo stai richiamando da un'applicazione esistente che non desideri sostituire? Gestisce già lo stato ed esegue funzioni utili nei singoli turni di dialogo che desideri continuare a controllare. Puoi ancora orchestrare tra le chiamate con la versione più recente dell'API /message. Il vantaggio è che non devi farlo. Nella versione più recente, puoi supportare più di un canale di integrazione alla volta con la stessa capacità di dialogo sottostante. 

Se scegli di ignorare il passo per la creazione di un assistente e per l'aggiunta della tua capacità di dialogo ad esso, rinuncerai alla semplificazione fornita dagli assistenti. Ossia, **non** potrai utilizzare una singola conversazione per interagire con i clienti tramite più canali di integrazione contemporaneamente ed espandere o passare velocemente ai nuovi canali che diventano popolari tra gli utenti. 

## Capacità e spazi di lavoro
{: #edit-convo-workspace-names}

Ciò che viene presentato negli strumenti come capacità di dialogo è effettivamente un wrapper per uno spazio di lavoro V1. Mentre al momento non ci sono metodi API per creare capacità con l'API V2, puoi continuare a utilizzare l'API V1 per creare gli spazi di lavoro. 

- Quando apri lo strumento, gli spazi di lavoro che hai creato prima del 9 novembre vengono visualizzati come capacità. Il nome della capacità viene preso dal nome dello spazio di lavoro. Tuttavia, se successivamente modifichi il nome o la descrizione della capacità, queste modifiche non avranno effetto sullo spazio di lavoro. Allo stesso modo, se utilizzi le API per modificare il nome o la descrizione dello spazio di lavoro, queste modifiche non avranno effetto sulla capacità. 
- Dallo strumento, i dettagli API della capacità ti mostrano sia l'ID capacità che l'ID spazio di lavoro. 
