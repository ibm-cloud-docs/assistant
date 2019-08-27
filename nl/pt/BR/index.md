---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-06"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
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

# Sobre
{: #index}

Use o {{site.data.keyword.conversationfull}} para construir o assistente de sua própria marca em qualquer dispositivo, aplicativo ou canal. Seu assistente se conecta aos recursos de engajamento do cliente que você já usa para entregar uma experiência de resolução de problemas unificada e envolvente para seus clientes.
{: shortdesc}

## Como ele Funciona
{: #index-how-it-works}

Este diagrama ilustra como o produto funciona:

![Flow diagram of the service](images/simple-overview.png)

- Os usuários interagem com o assistente por meio de um ou mais destes pontos de **integração**:

  - Um assistente virtual que você publica diretamente em uma plataforma de sistema de mensagens de mídia social existente, como o Slack ou o Facebook Messenger.
  - Um aplicativo customizado que você desenvolve, como um aplicativo móvel ou um robô com uma interface de voz.

- O **assistente** recebe a entrada do usuário e a roteia para a qualificação de diálogo.

- A **qualificação de diálogo** interpreta a entrada do usuário mais a fundo e, em seguida, direciona o fluxo da conversa. O diálogo reúne quaisquer informações necessárias para responder ou executar uma transação no nome do usuário.

- Todas as perguntas que não podem ser respondidas pela qualificação de diálogo são enviadas à **qualificação de procura**, que localiza respostas relevantes procurando as bases de conhecimento da empresa configuradas para esse propósito.

## Implementação
{: #index-implementation}

Este diagrama mostra a implementação com mais detalhes:

![Flow diagram of the service](images/arch-overview-search.png)

Veja a seguir como implementar seu assistente:

- ** Crie um assistente **.

- **Inclua uma qualificação em seu assistente**.

  Dependendo de seu plano de serviço, é possível incluir os tipos de qualificação a seguir:

  - **Incluir uma qualificação de diálogo**.  
  
    Use o produto gráfico intuitivo para definir os dados de treinamento e o diálogo para a conversa entre seu assistente e seus clientes. Os dados de treinamento consistem nos seguintes artefatos:

    - **Intenções**: objetivos que você espera que seus usuários tenham quando interagirem com seu assistente. Defina uma intenção para cada objetivo que pode ser identificado em uma entrada do usuário. Por exemplo, é possível definir uma intenção denominada *store_hours* que responde às perguntas sobre os horários da loja. Para cada intenção, você inclui elocuções de amostra que refletem a entrada que os clientes podem usar para perguntar as informações de que eles precisam, como `What time do you open?`

      Ou use os **catálogos de conteúdo** pré-construídos que são fornecidos pela IBM para começar a usar dados que lidem com os objetivos comuns do cliente.

    - **Diálogo**: use o editor de diálogo para construir um fluxo de diálogo que incorpore suas intenções. O fluxo de diálogo é representado graficamente como uma árvore. É possível incluir uma ramificação para processar cada uma das intenções que você deseja que seja manipulada por seu assistente.

    - **Entidades**: uma entidade representa um termo ou objeto que fornece contexto para uma intenção. Por exemplo, uma entidade pode ser um nome de cidade que ajuda seu diálogo a distinguir para qual loja o usuário quer saber os horários da loja. Depois de incluir entidades, atualize seu diálogo para usá-las. Inclua nós de diálogo que manipulem as muitas possíveis permutações de uma solicitação com base nas entidades localizadas na entrada do usuário.

    À medida que você inclui dados de treinamento, um classificador de língua natural é incluído automaticamente na qualificação. O modelo do classificador é treinado para entender os tipos de solicitações que você ensina seu assistente a atender e responder.

  - **Inclua uma qualificação de procura**. ![Somente nos planos Plus ou Premium](images/plus.png)

    Aproveite as coleções de dados criadas no {{site.data.keyword.discoveryfull}} para fornecer respostas às perguntas do cliente. Quando um cliente faz uma pergunta que o diálogo não foi projetado para responder, seu assistente pode procurar informações relevantes nas origens de dados configuradas, extrair as informações e retorná-las como resposta.

- **Integre seu assistente.** Inclua uma integração de canal integrada para implementar o assistente configurado diretamente em uma mídia social ou canal de sistema de mensagens. Ou construa seu próprio aplicativo cliente como a interface com o usuário para o assistente.

  O seu assistente implementado é hospedado pelo {{site.data.keyword.cloud}}, a plataforma de computação em nuvem da IBM (para obter mais informações, consulte [Visão geral da plataforma](/docs/overview/ibm-cloud#overview){: external}).

Leia mais sobre essas etapas de implementação seguindo estes links:

- [Visão geral do assistente](/docs/services/assistant?topic=assistant-assistants)
- [Visão geral da qualificação de procura](/docs/services/assistant?topic=assistant-skill-add-search)
- [ Visão Geral da Criação de Intenção ](/docs/services/assistant?topic=assistant-intents#intents-described)
- [Visão geral do diálogo](/docs/services/assistant?topic=assistant-dialog-overview)
- [ Visão Geral da Criação de Entidade ](/docs/services/assistant?topic=assistant-entities#entities-described)
- [Incluindo integrações](/docs/services/assistant?topic=assistant-deploy-integration-add)

## Suporte ao navegador
{: #index-browser-support}

O {{site.data.keyword.conversationshort}} requer o mesmo nível de software do navegador que o {{site.data.keyword.Bluemix_notm}}. Para obter mais informações, consulte {{site.data.keyword.Bluemix_notm}} [Pré-requisitos](/docs/overview?topic=overview-prereqs-platform#browsers-platform){: external}.

## Suporte ao idioma
{: #index-lang-support}

O suporte ao idioma por recurso é detalhado no tópico [Idiomas suportados](/docs/services/assistant?topic=assistant-language-support).

## Termos e avisos
{: #index-notices}

Consulte [Termos e avisos do IBM Cloud](/docs/overview/terms-of-use?topic=overview-terms){: external} para obter informações sobre os termos do serviço.

O suporte ao Health Insurance Portability and Accountability Act (HIPAA) dos EUA está disponível para os planos Premium que estão hospedados no local de Washington, DC criado em ou após 1º de abril de 2019. Para obter mais informações, consulte [Ativando as configurações suportadas da UE e do HIPAA](/docs/account?topic=account-eu-hipaa-supported#eu-hipaa-supported){: external}.

## Próximas etapas
{: #index-next-steps}

- [Introdução](/docs/services/assistant?topic=assistant-getting-started) ao produto.
- Veja a lista de [recursos do desenvolvedor](https://www.ibm.com/watson/developer-resources/){: external}.

Possui perguntas? Entre em contato com o departamento de [Vendas IBM](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: external}.
