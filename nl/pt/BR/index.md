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

# Sobre
{: #index}

O {{site.data.keyword.conversationfull}} é um robô cognitivo que você pode customizar para suas necessidades de negócios e implementar em múltiplos canais para trazer ajuda para seus clientes onde e quando eles precisarem.
{: shortdesc}

## Como ele Funciona
{: #index-how-it-works}

Este diagrama mostra a arquitetura geral:

![Flow diagram of the service](images/arch-overview.png)

- Os usuários interagem com o assistente por meio de um ou mais destes pontos de **integração**:

  - Um robô de bate-papo que você publica diretamente em uma plataforma de sistema de mensagens de mídia social existente, como Slack ou Facebook Messenger.
  - Uma interface com o usuário de robô de bate-papo simples que é hospedada pelo IBM Cloud.
  - Aplicativo customizado que você desenvolve, como um app móvel ou um robô com uma interface de voz.

- O **assistente** recebe a entrada do usuário e a roteia para a qualificação de diálogo.

- O diálogo **qualificação** interpreta a entrada do usuário mais adiante e, em seguida, direciona o fluxo da conversa e reúne quaisquer informações que ele precisa para responder ou executar uma transação em nome do usuário.

## Implementação
{: #index-mplementation}

Aqui está como você implementará seu assistente:

- ** Criar uma habilidade de diálogo **. Use a ferramenta gráfica intuitiva para definir os dados de treinamento e o diálogo para a conversa entre seu assistente e seus clientes.

  Os dados de treinamento consistem nos seguintes artefatos:

  - **Intenções**: objetivos que você prevê que seus usuários terão quando eles interagem com o serviço. Defina uma intenção para cada objetivo que pode ser identificado em uma entrada do usuário. Por exemplo, você pode definir uma intenção chamada *store_hours* que responde perguntas sobre horários da loja. Para cada intenção, você inclui elocuções de amostra que refletem a entrada que os clientes podem usar para perguntar as informações de que eles precisam, como `What time do you open?`

    Ou use os **catálogos de conteúdo** pré-construídos fornecidos pela IBM para começar a usar os dados que direcionam os objetivos comuns do cliente.

  - **Diálogo**: use a ferramenta de diálogo para construir um fluxo de diálogo que incorpora suas intenções. O fluxo de diálogo é representado graficamente na ferramenta como uma árvore. É possível incluir uma ramificação para processar cada uma das intenções que você deseja que o serviço manipule.

  - **Entidades**: uma entidade representa um termo ou objeto que fornece contexto para uma intenção. Por exemplo, uma entidade pode ser um nome de cidade que ajuda seu diálogo a distinguir para qual loja o usuário quer saber os horários da loja. Depois de incluir entidades, atualize seu diálogo para usá-las. Inclua nós de diálogo que manipulam as muitas permutações possíveis de uma solicitação com base nas entidades localizadas na entrada do usuário.

    À medida que você inclui dados de treinamento, um classificador de língua natural é incluído automaticamente na qualificação e é treinado para entender os tipos de solicitações que você indicou que o serviço deve atender e responder.

- ** Crie um assistente **.

- **Inclua a qualificação de diálogo em seu assistente.**

- **Integre seu assistente.** Crie uma integração de canal para implementar o assistente configurado diretamente em uma mídia social ou canal do sistema de mensagens.

  O seu assistente implementado é hospedado pelo {{site.data.keyword.cloud_notm}}, a plataforma de computação em nuvem da IBM. (Veja [Visão geral da plataforma ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/docs/overview/ibm-cloud#overview) para obter mais informações.)

Leia mais sobre essas etapas de implementação seguindo estes links:

- [ Visão Geral da Criação de Intenção ](/docs/services/assistant?topic=assistant-intents#intents-described)
- [Visão geral do diálogo](/docs/services/assistant?topic=assistant-dialog-overview)
- [ Visão Geral da Criação de Entidade ](/docs/services/assistant?topic=assistant-entities#entities-described)
- [Visão geral do assistente](/docs/services/assistant?topic=assistant-assistant-add)
- [Incluindo integrações](/docs/services/assistant?topic=assistant-deploy-integration-add)

## Onde estão minhas áreas de trabalho?
{: #index-existing-customers}

Se você criou uma *área de trabalho* em uma versão anterior do serviço, não se preocupe; ainda é possível chegar a ela. As áreas de trabalho agora são chamadas de  * qualificações *. Para chegar à sua área de trabalho, clique na guia **Qualificações**.

## Suporte ao navegador
{: #index-browser-support}

A ferramenta de serviço do {{site.data.keyword.conversationshort}} requer o mesmo nível de software do navegador que é necessário para o {{site.data.keyword.Bluemix_notm}}. Consulte o tópico [Pré-requisitos ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/docs/overview/prereqs#browsers){: new_window} do {{site.data.keyword.Bluemix_notm}} para obter detalhes.

## Suporte ao idioma
{: #index-lang-support}

O suporte ao idioma por recurso é detalhado no tópico [Idiomas suportados](/docs/services/assistant?topic=assistant-language-support).

## Termos e avisos
{: #index-notices}

Consulte [Termos e avisos do IBM Cloud ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/overview/terms-of-use?topic=overview-terms) para obter informações sobre os termos de serviço.

## Próximas etapas
{: #index-next-steps}

- [Introdução](/docs/services/assistant?topic=assistant-getting-started) ao serviço.
- Visualize a lista de [recursos do desenvolvedor ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/watson/developer-resources/){: new_window}.

Ainda tem dúvidas? Entre em contato com o [IBM Sales ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.
