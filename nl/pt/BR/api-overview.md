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

# Visão geral da API do {{site.data.keyword.conversationshort}}
{: #api-overview}

É possível usar as APIs de REST do {{site.data.keyword.conversationshort}} e os SDKs correspondentes para desenvolver aplicativos que interagem com o serviço. Há duas versões da API do {{site.data.keyword.conversationshort}}, cada uma suportando um conjunto diferente de funções: v1 e v2. A API que deve ser usada depende do tipo de métodos que seu aplicativo requer:

- **Métodos de tempo de execução**: métodos que permitem que um aplicativo cliente interaja com (mas não modifique) um assistente ou qualificação existente. É possível usar esses métodos para desenvolver um cliente voltado para o usuário que possa ser implementado para uso de produção, um aplicativo que faça a intermediação da comunicação entre um assistente e outro serviço (como um serviço de bate-papo ou um sistema back-end) ou um aplicativo de teste.

  A API v2 do {{site.data.keyword.conversationshort}} fornece acesso a métodos que podem ser usados para interagir com um assistente no tempo de execução (como `/message`). Essa é a API preferencial a ser usada para desenvolver novos aplicativos clientes. Para obter detalhes sobre a API v2, consulte a [Referência de API v2 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/assistant-v2){: new_window} do {{site.data.keyword.conversationshort}}.

  A API v1 do {{site.data.keyword.conversationshort}} inclui um método `/message` que envia a entrada do usuário diretamente para a área de trabalho usada por uma qualificação de diálogo, efetuando bypass do assistente. A API de tempo de execução v1 é suportada principalmente para propósitos de compatibilidade com versões anteriores. Se você usar o método `/message` v1, seu app não poderá aproveitar os recursos de orquestração e de gerenciamento de estado de um assistente. Para obter mais informações, consulte [Usando a API de tempo de execução v1](/docs/services/assistant?topic=assistant-api-client#v1-api).

- **Métodos de autoria **: métodos que permitem que um aplicativo crie ou modifique as qualificações de diálogo, como uma alternativa à construção gráfica de uma qualificação usando a ferramenta {{site.data.keyword.conversationshort}}. Um aplicativo de autoria usa vários métodos para criar e modificar qualificações, intenções, entidades, nós de diálogo e outros artefatos que compõem uma qualificação de diálogo.

  Para construir um aplicativo de autoria, use a API v1. Para obter mais informações, consulte a [Referência de API v1 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/assistant){: new_window}.

  **Nota:** os métodos de autoria v1 interagem com áreas de trabalho em vez de qualificações. Uma área de trabalho é um contêiner para os dados de diálogo e de treinamento (como intenções e entidades) dentro de uma qualificação de diálogo. Para a maioria dos propósitos, ao usar a API, é possível pensar em áreas de trabalho e qualificações de diálogo como intercambiáveis; por exemplo, se você criar uma nova área de trabalho usando a API, ela aparecerá como uma nova qualificação de diálogo na ferramenta {{site.data.keyword.conversationshort}}.

Para obter uma lista dos métodos de API disponíveis, consulte [Resumo de métodos de API](/docs/services/assistant?topic=assistant-api-methods).
