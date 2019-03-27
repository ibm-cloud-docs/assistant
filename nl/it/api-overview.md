---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

subcollection: assistant

---

{:curl: #curl .ph data-hd-programlang='curl'}
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
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# Panoramica sull'API {{site.data.keyword.conversationshort}}
{: #api-overview}

Puoi utilizzare le API REST {{site.data.keyword.conversationshort}}, e gli SDK corrispondenti, per sviluppare le applicazioni che interagiscono con il servizio. Esistono due versioni dell'API {{site.data.keyword.conversationshort}}, ognuna delle quali supporta una diversa serie di funzioni: v1 e v2. L'API che devi utilizzare dipende dal tipo di metodi di cui necessita la tua applicazione: 

- **Metodi di runtime**: metodi che consentono a un'applicazione client di interagire con una competenza o un assistente esistente (ma non di modificarla/o). Puoi utilizzare questi metodi per sviluppare un client rivolto all'utente che può essere distribuito per un utilizzo di produzione, un'applicazione che agisce da broker per la comunicazione tra un assistente e un altro servizio (ad esempio un servizio chat o un servizio di backend) o un'applicazione di test.

  L'API v2 {{site.data.keyword.conversationshort}} fornisce l'accesso ai metodi che puoi utilizzare per interagire con un assistente nel runtime (ad esempio `/message`). Si tratta dell'API preferita da utilizzare per sviluppare le nuove applicazioni client. Per i dettagli sull'API v2, vedi il {{site.data.keyword.conversationshort}} [Riferimento API v2 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/assistant-v2){: new_window}.

  L'API v1 {{site.data.keyword.conversationshort}} include un metodo `/message` che invia l'input utente direttamente allo spazio di lavoro utilizzato da una capacità di dialogo, escludendo l'assistente. L'API runtime v1 è supportata principalmente a scopo di compatibilità con le versioni precedenti. Se utilizzi il metodo v1 `/message`, la tua applicazione non potrà trarre vantaggio dalle capacità di orchestrazione e gestione dello stato di un assistente. Per ulteriori informazioni, vedi [Utilizzo dell'API runtime v1](/docs/services/assistant?topic=assistant-api-client#v1-api).

- **Metodi di creazione**: metodi che consentono ad un'applicazione di creare o modificare le capacità di dialogo, come alternativa alla creazione grafica di una capacità utilizzando lo strumento {{site.data.keyword.conversationshort}}. Un'applicazione di creazione utilizza diversi metodi per creare e modificare capacità, intenti, entità, nodi di dialogo e altre risorse che compongono una capacità di dialogo. 

  Per creare un'applicazione di creazione, utilizza l'API v1. Per ulteriori informazioni, vedi il [Riferimento API v1 ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](https://{DomainName}/apidocs/assistant){: new_window}.

  **Nota:** i metodi di creazione v1 interagiscono con gli spazi di lavoro piuttosto che con le capacità. Uno spazio di lavoro è un contenitore per il dialogo e i dati di addestramento (ad esempio intenti e entità) all'interno di una capacità di dialogo. Per la maggior parte degli scopi, quando utilizzi l'API, puoi considerare gli spazi di lavoro e le capacità di dialogo come interscambiabili; ad esempio, se crei un nuovo spazio di lavoro utilizzando l'API, comparirà come una nuova capacità di dialogo nello strumento {{site.data.keyword.conversationshort}}.

Per un elenco dei metodi API disponibili, vedi [Riepilogo dei metodi API](/docs/services/assistant?topic=assistant-api-methods).
