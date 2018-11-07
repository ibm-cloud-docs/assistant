---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-09"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Distribuzione a un canale con il connettore {{site.data.keyword.conversationshort}}

Una volta sviluppato il tuo spazio di lavoro, puoi utilizzare il connettore {{site.data.keyword.conversationshort}} per connetterlo velocemente ad un canale di comunicazione come Slack o Facebook Messenger. Il connettore {{site.data.keyword.conversationshort}} è una serie di componenti {{site.data.keyword.openwhisk}} che mediano la comunicazione tra il tuo spazio di lavoro {{site.data.keyword.conversationshort}} e una app Slack o Facebook di cui disponi, memorizzando i dati della sessione in un database Cloudant.

Connettendo il tuo spazio di lavoro ad una app del canale, puoi renderlo disponibile come una chatbot con cui possono interagire gli utenti Slack o Facebook Messenger. Le risposte dal dialogo possono includere risposte interattive e multimediali come immagini e pulsanti su cui è possibile fare clic. (Per maggiori informazioni sulla definizione di una risposta multimediale, vedi [Risposte multimediali](dialog-multimedia.html).)

![{{site.data.keyword.openwhisk_short}} Diagramma di panoramica della distribuzione](images/deploytochannel_diagram.png)

Il connettore {{site.data.keyword.conversationshort}} presenta alcune limitazioni:

- Devi creare una app Slack o Facebook oppure devi disporre delle autorizzazioni amministrative per modificare la app che desideri utilizzare. 
- A causa delle limitazioni di {{site.data.keyword.openwhisk_short}}, attualmente questo strumento è disponibile solo per la regione Stati Uniti Sud di {{site.data.keyword.Bluemix_notm}}.

## Distribuzione a Slack utilizzando lo strumento {{site.data.keyword.conversationshort}} 

Lo strumento {{site.data.keyword.conversationshort}} offre un modo veloce per distribuire un bot utilizzando il connettore {{site.data.keyword.conversationshort}}. Lo strumento ti guida nel processo di raccolta delle informazioni necessarie per configurare la app del canale e i componenti del connettore e poi distribuisce automaticamente i componenti richiesti in IBM Cloud.

**Nota:** attualmente l'interfaccia dello strumento {{site.data.keyword.conversationshort}} supporta solo Slack, ma puoi utilizzare un processo {{site.data.keyword.Bluemix_short}} automatizzato per la distribuzione relativa a Facebook Messenger. Per ulteriori informazioni sulla distribuzione relativa a Facebook Messenger, consulta la documentazione nel [repository GitHub ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/watson-developer-cloud/conversation-connector/blob/master/channels/facebook/README.md){: new_window} del connettore {{site.data.keyword.conversationshort}}.

Per distribuire un bot utilizzando lo strumento di distribuzione automatizzata:

1. Nello strumento {{site.data.keyword.conversationshort}}, apri lo spazio di lavoro che desideri distribuire.
1. Fai clic sull'icona di menu nell'angolo superiore sinistro e seleziona **Distribuisci**.

   ![Opzioni del menu rapido di distribuzione](images/deploy_menu_testdeploy.png)

1. Sotto **Distribuisci con {{site.data.keyword.openwhisk_short}}**, fai clic su **Distribuisci a Slack**.
1. Sulla pagina Slack, fai clic su **Distribuisci alla app Slack** e segui le istruzioni. 

   ![Pulsante Distribuisci alla app Slack](images/deploy_deploytoslack.png)

## Distribuzione manuale

Invece di utilizzare lo strumento automatizzato per distribuire il tuo spazio di lavoro come un bot, puoi configurare e distribuire manualmente i componenti richiesti in IBM Cloud. Esistono diverse situazioni in cui potresti voler eseguire tale operazione: 

- **Ridistribuzione parziale**. Potresti voler ridistribuire alcuni componenti di una distribuzione esistente al fine di modificare la tua configurazione, risolvere problemi o applicare una correzione proveniente da una versione più recente.
- **Ampliamento del tuo bot con nuove funzionalità**. Puoi modificare i componenti forniti per aggiungere nuove funzioni, ad esempio puoi preelaborare l'input utente prima di inviarlo allo spazio di lavoro {{site.data.keyword.conversationshort}}.
- **Distribuzione a un nuovo canale**. Se desideri distribuire un bot ad un canale diverso da Slack o Facebook Messenger, puoi seguire il modello dei componenti esistenti per sviluppare i tuoi componenti.

I componenti del connettore {{site.data.keyword.conversationshort}} sono ospitati in un repository GitHub pubblico che puoi scaricare o clonare. Per ulteriori informazioni, consulta la documentazione fornita nel [repository ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/watson-developer-cloud/conversation-connector){: new_window}.

## Chat con il bot

Dopo aver completato il processo di distribuzione, puoi utilizzare il nome utente bot per interagire con il tuo spazio di lavoro {{site.data.keyword.conversationshort}} proprio come faresti con qualsiasi altro bot Slack o Facebook Messenger.

Tieni presente che il connettore {{site.data.keyword.conversationshort}} conserva separatamente lo stato per ciascun utente che sta interagendo con il bot. (Nel caso di Slack, un singolo utente potrebbe avere più conversazioni univoche con il bot, una attraverso messaggi diretti e una in ciascun canale.) Ciò significa che le variabili che memorizzi nel contesto del dialogo vengono conservate a tempo indeterminato a meno che tu non le cancelli. 

Se hai bisogno di ripristinare la conversazione a uno stato iniziale noto, puoi farlo all'interno del dialogo. Assicurati che il tuo dialogo abbia un nodo che viene eseguito alla fine della conversazione o in qualsiasi altro momento in cui devi ricominciare da capo. Aggiorna il JSON per questo nodo per ripristinare tutte le variabili di contesto ai valori iniziali appropriati. (Se necessario, puoi utilizzare le azioni **Passa a** o uno speciale intento "termina conversazione" per eseguire questo nodo.)

Ad esempio, se il tuo spazio di lavoro utilizza una variabile di contesto denominata `drink_order` per memorizzare la selezione di bevande di un utente, puoi utilizzare il metodo `context.remove` per eliminare questa variabile al termine della conversazione:

```json
"context": {
   "reset_drink_order": "<?context.remove('drink_order')?>"
 }
```
{: codeblock}

Per ulteriori informazioni sulla modifica dei valori delle variabili di contesto, vedi [Aggiornamento di un valore di variabile di contesto](dialog-runtime.html#context-update).

Puoi anche cancellare il contesto, o apportare altre modifiche al comportamento del bot, modificando le azioni {{site.data.keyword.openwhisk_short}} distribuite. Per ulteriori informazioni, consulta la documentazione fornita nel [repository GitHub ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://github.com/watson-developer-cloud/conversation-connector){: new_window}.
