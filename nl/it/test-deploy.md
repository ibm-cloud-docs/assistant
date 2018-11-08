---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-24"

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

# Esecuzione di test in Slack

Puoi utilizzare lo strumento di distribuzione di test per integrare il tuo spazio di lavoro {{site.data.keyword.conversationshort}} in un team Slack come utente bot. Utilizza questo metodo se desideri testare rapidamente utilizzando un bot Slack come interfaccia utente del tuo spazio di lavoro.

Lo strumento di distribuzione di test utilizza il servizio {{site.data.keyword.openwhisk}} per distribuire un'applicazione Slack precostruita al tuo team come utente bot. Questa applicazione gestisce la comunicazione con i tuoi spazi di lavoro {{site.data.keyword.conversationshort}}.

![Diagramma di panoramica della distribuzione di test](images/testdeploy_diagram.png)

Nota che lo strumento di distribuzione di test presenta alcune limitazioni:

- Non puoi utilizzare questo strumento per pubblicare un'applicazione che venga usata da altri team.
- Se utilizzi questo metodo per distribuire più di uno spazio di lavoro allo stesso team, tutti gli spazi di lavoro risponderanno al nome utente `@ibmwatson_bot`. Si consiglia di utilizzare questo strumento per distribuire un solo spazio di lavoro alla volta a ciascun team Slack.
- Devi avere l'autorizzazione per installare le applicazioni nel tuo team Slack. Se non sei sicuro di avere questa autorizzazione, consulta il tuo amministratore Slack.
- L'applicazione Slack precostruita è solo a scopo di test e potrebbe non essere disponibile in qualsiasi momento.
- A causa delle limitazioni di {{site.data.keyword.openwhisk_short}}, attualmente questo strumento è disponibile solo per la regione Stati Uniti Sud di {{site.data.keyword.Bluemix_notm}}.

Per installare la tua applicazione come utente bot:

1. Nello strumento {{site.data.keyword.conversationshort}}, apri lo spazio di lavoro che vuoi testare in Slack.
1. Fai clic sull'icona di menu nell'angolo superiore sinistro e seleziona **Distribuisci**. Viene visualizzata la pagina Opzioni di distribuzione.

   ![Opzioni del menu rapido di distribuzione](images/deploy_menu_testdeploy.png)

1. In **Distribuisci con {{site.data.keyword.openwhisk_short}}**, fai clic su **Test in Slack** e segui le istruzioni. 

   ![Pulsante Crea test Slack](images/testdeploy_testinslack.png)

## Chat con il bot

Dopo aver completato il processo di distribuzione, puoi utilizzare il nome utente `@ibmwatson_bot` per interagire con il tuo spazio di lavoro {{site.data.keyword.conversationshort}}, proprio come faresti con qualsiasi altro bot Slack.

Tieni presente che il bot distribuito nel tuo team mantiene lo stato per ogni utente all'interno di un canale particolare. Ciò significa che tutte le variabili che memorizzi nel contesto del dialogo vengono mantenute indefinitamente, a meno che il dialogo non le cancelli.

Se hai bisogno di ripristinare la conversazione a uno stato iniziale noto, devi farlo all'interno del dialogo. Assicurati che il tuo dialogo abbia un nodo che viene eseguito alla fine della conversazione o in qualsiasi altro momento in cui devi ricominciare da capo. Aggiorna il JSON per questo nodo per ripristinare tutte le variabili di contesto ai valori iniziali appropriati. (Se necessario, puoi utilizzare le azioni **Passa a** o uno speciale intento "termina conversazione" per eseguire questo nodo.)

Ad esempio, se il tuo spazio di lavoro utilizza una variabile di contesto denominata `drink_order` per memorizzare la selezione di bevande di un utente, puoi utilizzare il metodo `context.remove` per eliminare questa variabile al termine della conversazione:

```json
"context": {
   "reset_drink_order": "<?context.remove('drink_order')?>"
 }
```
{: codeblock}

Per ulteriori informazioni sulla modifica dei valori delle variabili di contesto, vedi [Aggiornamento di un valore di variabile di contesto](dialog-overview.html#updating-a-context-variable-value).

**Nota:** quando hai finito di testare il tuo spazio di lavoro, puoi eliminare la distribuzione di test tornando allo strumento di distribuzione di test e facendo clic su **Elimina test**. Ricorda anche che devi rimuovere separatamente l'autorizzazione dell'applicazione bot dal tuo team Slack.
