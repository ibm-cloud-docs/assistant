---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-12"

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

# Alta disponibilità e ripristino di emergenza
{: #recovery}

{{site.data.keyword.conversationfull}} è altamente disponibile con più ubicazioni {{site.data.keyword.cloud}} (ad esempio, Dallas e Washington, DC). Tuttavia, il ripristino da potenziali emergenze che influenza un'intera ubicazione richiede pianificazione e preparazione.
{: shortdesc}

Sei responsabile della comprensione della configurazione, della personalizzazione e dell'utilizzo di {{site.data.keyword.conversationshort}}. Si è anche responsabili di essere in grado di ricreare un'istanza del servizio in una nuova ubicazione e di ripristinare i dati in qualsiasi ubicazione. Vedi [Come garantisco nessun tempo di inattività? ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/overview?topic=overview-zero-downtime#zero-downtime){: new_window} per maggiori informazioni.

## Alta disponibilità (HA)
{: #recovery-ha}

{{site.data.keyword.conversationshort}} supporta l'alta disponibilità con nessun singolo punto di errore. Il servizio ottiene l'alta disponibilità automaticamente e in modo trasparente utilizzando la funzione regione multizona fornita da {{site.data.keyword.cloud_notm}}.

{{site.data.keyword.cloud_notm}} abilita più zone che non condividono un singolo punto di guasto all'interno di una singola ubicazione. Fornisce anche il bilanciamento del carico automatico tra le zone all'interno di una regione. 

## Ripristino di emergenza
{: #recovery-dr}

Il ripristino di emergenza può diventare un problema se in un'ubicazione {{site.data.keyword.cloud_notm}} si verifica un guasto significativo che include la potenziale perdita di dati. Poiché la funzione regione multizona non è disponibile in tutte le ubicazioni, devi attendere che IBM riporti un'ubicazione online se diventa indisponibile. Se i servizi di dati sottostanti vengono compromessi dall'errore, devi attendere anche che IBM ripristini questi servizi di dati.

Se si verifica un guasto catastrofico, IBM potrebbe non essere in grado di ripristinare i dati dai backup di database. In questo caso, devi ripristinare i tuoi dati per riportare l'istanza del servizio al suo stato più recente. Puoi ripristinare i dati nella stessa ubicazione o in un'ubicazione diversa. 

Il tuo piano di ripristino di emergenza include conoscere, preservare, ed essere pronti a ripristinare tutti i dati conservati su {{site.data.keyword.cloud_notm}}. Vedi [Backup e ripristino dei dati](/docs/services/assistant?topic=assistant-backup) per informazioni su come eseguire il backup delle tue istanze del servizio.
