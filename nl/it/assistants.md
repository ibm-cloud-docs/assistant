---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-26"

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

# Assistenti
{: #assistants}

Un assistente è un bot cognitivo che puoi personalizzare per le tue esigenze aziendali e distribuire attraverso più canali per fornire supporto ai tuoi clienti dove e quando ne hanno bisogno.
{: shortdesc}

![Capacità](images/skill-icon.png)  Un assistente instrada le query del tuo cliente a una capacità che poi fornisce la risposta appropriata. Le capacità di dialogo restituiscono le risposte create da te per rispondere alle domande comuni, mentre le capacità di ricerca ricercano e restituiscono i passaggi dal contenuto self-service esistente per rispondere a domande più complesse. 

## Capacità di dialogo
{: #assistants-dialog-skill}

Una capacità di dialogo può comprendere e far fronte alle domande o alle richieste per le quali, di norma, i tuoi clienti hanno bisogno di supporto. Devi fornire le informazioni sugli argomenti o sulle attività che i tuoi clienti richiederanno e il modo in cui le richiederanno e il prodotto creerà dinamicamente un modello di machine learning personalizzato per comprendere le richieste uguali e simili dell'utente. 

| Struttura ad albero di dialogo | GUI |
|-------------|-------------------------:|
| Puoi utilizzare l'editor del dialogo grafico per creare uno script di tipi da cui il tuo assistente deve leggere quando interagisce con i tuoi clienti. Il risultato è un dialogo che simula una conversazione reale. Il dialogo sfrutta gli obiettivi comuni del cliente che gli hai insegnato a riconoscere e fornisce risposte utili. | ![Una struttura ad albero di dialogo di esempio con contenuto di esempio](images/dialog-depiction.png) |

La capacità di dialogo, di per sé, viene definita nel testo, ma puoi integrarla con i servizi Watson Speech to Text e Watson Text to Speech che consentono agli utenti di interagire verbalmente con il tuo assistente.

![Dati di apprendimento pronti all'uso](images/oob.png)  Se desideri iniziare rapidamente, aggiungi i dati di apprendimento precostruiti alla tua capacità di dialogo in modo che il tuo assistente possa iniziare ad aiutare i tuoi clienti con le nozioni di base.

## Capacità di ricerca ![Solo piano Plus o Premium](images/plus.png)
{: #assistants-search-skill}

La capacità di ricerca è disponibile solo per gli utenti Plus o Premium.
{: note}

Una capacità di ricerca utilizza le informazioni da knowledge base aziendali esistenti o da altre raccolte di contenuto create dagli esperti in materia per far fronte a domande del cliente non previste o con più sfumature. 

![IBM Cloud](images/cloud.png)  L'assistente è un bot ospitato completo gestito da {{site.data.keyword.Bluemix_notm}}, ciò significa che non devi preoccuparti della configurazione o della manutenzione dell'infrastruttura per supportarlo.

| Integrazioni       | Canali  |
|--------------------|:----------|
| Puoi distribuire l'assistente attraverso più interfacce, inclusi i canali di messaggistica esistenti, come Slack e Facebook Messenger, in pochi passi. Oppure, se desideri progettare un'applicazione personalizzata che lo incorpora, puoi eseguire chiamate dirette alle API sottostanti per raggiungere tale scopo. | ![Metodi di integrazione inclusi Slack, Facebook Messenger, un'applicazione web o un'integrazione dell'operatore](images/integrations.png) |

Per iniziare, vedi [Creazione di assistenti](/docs/services/assistant?topic=assistant-assistant-add).
