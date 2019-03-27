---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# Processo di sviluppo
{: #dev-process}

Utilizza lo strumento {{site.data.keyword.conversationshort}} per utilizzare AI mentre crei, distribuisci e migliori in modo incrementale un assistente convenzionale.
{: shortdesc}

![Mostra il flusso dei passi di sviluppo partendo dallo sviluppo dei dati di addestramento e terminando con la distribuzione per la produzione](images/dev-process.png)

## Flusso di lavoro
{: #dev-process-workflow}

Un flusso di lavoro tipico per un progetto di assistente include i seguenti passi: 

1.  Definisci una serie ridotta delle esigenze chiave del cliente a cui desideri faccia fronte l'assistente per conto tuo, inclusi i processi aziendali che può avviare o completare per i tuoi clienti. 
1.  Crea gli intenti che rappresentano le esigenze del cliente che hai identificato nel passo precedente. Ad esempio, intenti come `About_company` o `#Place_order`.

    Utilizza la funzione dei consigli sull'esempio dell'intento per estrarre i log esistenti del call center per trovare esempi ottimali dell'intento utente.
    {: tip}

1.  Crea un dialogo che rilevi gli intenti definiti e se ne occupi, con semplici risposte o con un flusso di dialogo che raccolga innanzitutto ulteriori informazioni.
1.  Definisci le entità necessarie per comprendere più chiaramente l'intenzione dell'utente. 

    Estrai gli esempi utente dell'intento esistenti per citazioni comuni del valore di entità. L'utilizzo delle annotazioni per definire le entità acquisisce non solo il testo del valore di entità, ma il contesto in cui il valore di entità viene di norma utilizzato in una frase. 

    Per le entità basate sul dizionario, utilizza i consigli sui sinonimi per espandere le tue definizioni di entità.
    {: tip}

1.  Verifica ogni funzione che aggiungi all'assistente nel riquadro "Provalo", in modo incrementale, man mano che lavori. 
1.  Quando hai un assistente funzionante che può gestire correttamente le attività chiave, aggiungi un'integrazione che distribuisca l'assistente in un ambiente di sviluppo. Verifica l'assistente distribuito e apporta miglioramenti. 

1.  Dopo aver creato un assistente affidabile, acquisisci un'istantanea della capacità di dialogo e salvala come una versione. 

    Il salvataggio di una versione quando raggiungi una tappa fondamentale di sviluppo, ti fornisce un punto a cui tornare se le modifiche successive che apporterai alla capacità ne ridurranno l'efficacia. Vedi [Creazione di versioni della capacità](/docs/services/assistant?topic=assistant-versions).
1.  Distribuisci la versione dell'assistente in un ambiente di test e verificala. 

    Se utilizzi un widget della chat ospitato nel web, puoi condividere l'URL con altri utenti per avere supporto quando esegui il test.
1.  Utilizza le metriche della scheda Analisi per trovare le aree per il miglioramento e apportare le modifiche. 

    Se hai bisogno di verificare approcci alternativi per risolvere un problema, crea una versione per ciascuna soluzione in modo che tu possa distribuire e verificare ciascuna di esse indipendentemente e confrontare i risultati.
1.  Una volta soddisfatto delle prestazioni del tuo assistente, distribuiscine la versione migliore in un ambiente di produzione. 
1.  Monitora i log delle conversazioni che gli utenti hanno con l'assistente distribuito. 

    Puoi esaminare i log di una versione di una capacità in esecuzione nell'ambiente di produzione dalla scheda Analisi di una versione di sviluppo della capacità. Nel momento in cui trovi un errore di classificazione o altri problemi, puoi risolverli nella versione di sviluppo della capacità e poi distribuire la versione migliorata nell'ambiente di produzione dopo il test. Per ulteriori dettagli, vedi [Miglioramento tra gli assistenti](/docs/services/assistant?topic=assistant-logs#logs-deploy-id). 

Il processo di analisi dei log e di miglioramento della capacità di dialogo è continuo. Nel corso del tempo, potresti voler espandere le attività che l'assistente può gestire per te. Anche le esigenze del cliente cambiano. Man mano che emergono nuove esigenze, le metriche generate dai tuoi assistenti distribuiti possono aiutarti a identificarle e a farvi fronte nelle iterazioni successive. 
