---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-16"

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
{:tip: .tip}

# Visão geral da API do {{site.data.keyword.conversationshort}}
{: #api-overview}

É possível usar as APIs de REST do {{site.data.keyword.conversationshort}} e os SDKs correspondentes para desenvolver aplicativos que interagem com o serviço.

## Aplicativos clientes

Para construir um assistente virtual ou outro aplicativo cliente que se comunique com um assistente no tempo de execução, use a nova API v2. Usando essa API, é possível desenvolver um cliente voltado para o usuário que pode ser implementado para uso de produção, um aplicativo que intermedeia a comunicação entre um assistente e outro serviço (como um serviço de bate-papo ou um sistema de back-end) ou um aplicativo de teste.

Usando a API de tempo de execução da v2 para se comunicar com seu assistente, seu aplicativo pode aproveitar os recursos a seguir:

- **Gerenciamento de estado automático.** A API de tempo de execução da v2 gerencia cada sessão com um usuário final, armazenando e mantendo todos os dados de contexto necessários para que seu assistente tenha uma conversa completa.

- **Facilidade de implementação usando assistentes.** Além de suportar clientes customizados, um assistente pode ser facilmente implementado em canais de sistema de mensagens populares, como o Slack e o Facebook Messenger.

- **Versionamento.** Com o versionamento da qualificação de diálogo, é possível salvar uma captura instantânea de sua qualificação e vincular seu assistente a essa versão específica. Em seguida, é possível continuar atualizando sua versão de desenvolvimento sem afetar o assistente de produção.

- **Recursos de procura.** A API de tempo de execução da v2 pode ser usada para receber respostas de qualificações de diálogo e de procura. Quando uma consulta que sua qualificação de diálogo não pode responder é enviada, o assistente pode usar uma qualificação de procura para localizar a melhor resposta nas origens de dados configuradas (as qualificações de procura são um recurso beta disponível apenas para usuários dos planos Plus ou Premium).

Para obter detalhes sobre a API v2, consulte a [Referência de API v2 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/assistant-v2){: new_window} do {{site.data.keyword.conversationshort}}.

**Nota**: a API do {{site.data.keyword.conversationshort}} v1 ainda suporta o método legado `/message` que envia a entrada do usuário diretamente para a área de trabalho usada por uma qualificação de diálogo. A API de tempo de execução v1 é suportada principalmente para propósitos de compatibilidade com versões anteriores. Ao usar o método `/message` da v1, deve-se implementar o próprio gerenciamento de estado e não será possível aproveitar o versionamento ou qualquer um dos outros recursos de um assistente.

## Aplicativos de criação

A API da v1 fornece métodos que permitem que um aplicativo crie ou modifique qualificações de diálogo, como uma alternativa para construir uma qualificação graficamente por meio da interface com o usuário do {{site.data.keyword.conversationshort}}. Um aplicativo de criação usa a API para criar e modificar qualificações, intenções, entidades, nós de diálogo e outros artefatos que constituem uma qualificação de diálogo. Para obter mais informações, consulte a [Referência da API da v1 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/assistant){: new_window}.

  **Nota:** os métodos de criação da v1 criam e modificam áreas de trabalho em vez de qualificações. Uma área de trabalho é um contêiner para os dados de diálogo e de treinamento (como intenções e entidades) dentro de uma qualificação de diálogo. Se você criar uma nova área de trabalho usando a API, ela aparecerá como uma nova qualificação de diálogo na interface com o usuário do {{site.data.keyword.conversationshort}}.

Para obter uma lista dos métodos de API disponíveis, consulte [Resumo de métodos de API](/docs/services/assistant?topic=assistant-api-methods).
