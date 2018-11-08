---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-26"

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

# Sobre

Com o serviço {{site.data.keyword.conversationfull}}, você pode construir uma solução que compreende a entrada de língua natural e usa aprendizado de máquina para responder aos clientes de uma maneira que simula uma conversa entre humanos.
{: shortdesc}

## Como ele Funciona

Este diagrama mostra a arquitetura geral de uma solução completa:![Fluxograma do serviço](images/conversation_arch_overview.png)

- Os usuários interagem com seu aplicativo por meio da **interface** com o usuário que você implementa. Por exemplo, uma janela de bate-papo simples ou um aplicativo móvel ou mesmo um robô com uma interface de voz.

- O **aplicativo** envia a entrada do usuário para o serviço {{site.data.keyword.conversationshort}}.
    - O aplicativo se conecta a uma *área de trabalho*, que é um contêiner para seu fluxo de diálogo e dados de treinamento.
    - O serviço interpreta a entrada do usuário, direciona o fluxo da conversa e reúne informações que ele precisa.
    - É possível conectar serviços adicionais do {{site.data.keyword.watson}} para analisar a entrada do usuário, como o {{site.data.keyword.toneanalyzershort}} ou o {{site.data.keyword.speechtotextshort}}.

- O aplicativo pode interagir com seus **sistemas backend** com base na intenção do usuário e informações adicionais. Por exemplo, responder uma pergunta, abrir chamados, atualizar informações de conta ou fazer pedidos. Não há limites para o que pode ser feito.

## Implementação

Aqui está como você implementará sua conversa:

- **Configurar uma área de trabalho.** Com o ambiente gráfico fácil de usar, configure os dados de treinamento e o diálogo para sua conversa.

    Os dados de treinamento consistem nos seguintes artefatos:
    - **Intenções**: objetivos que você prevê que seus usuários terão quando eles interagem com o serviço. Defina uma intenção para cada objetivo que pode ser identificado em uma entrada do usuário. Por exemplo, você pode definir uma intenção chamada *store_hours* que responde perguntas sobre horários da loja. Para cada intenção, você inclui elocuções de amostra que refletem a entrada que os clientes podem usar para perguntar as informações de que eles precisam, como `What time do you open?`
    - **Entidades**: uma entidade representa um termo ou objeto que fornece contexto para uma intenção. Por exemplo, uma entidade pode ser um nome de cidade que ajuda seu diálogo a distinguir para qual loja o usuário quer saber os horários da loja.

      Conforme você inclui dados de treinamento, um classificador de língua natural é automaticamente incluído na área de trabalho e é treinado para entender os tipos de solicitação que você indicou que o serviço deve atender e responder.

    Use a ferramenta de diálogo para construir um fluxo de diálogo que incorpora suas intenções e entidades. O fluxo de diálogo é representado graficamente na ferramenta como uma árvore. É possível incluir uma ramificação para processar cada uma das intenções que você deseja que o serviço manipule. É possível então incluir nós de ramificação que tratam muitas permutações possíveis de um pedido com base em outros fatores, como as entidades localizadas na entrada do usuário ou informações que são passadas para o serviço de seu aplicativo ou outro serviço externo.

- **Implementar sua área de trabalho.** Implemente a área de trabalho configurada para os usuários conectando-se a uma interface com o usuário de front-end, mídia social ou um canal de mensagens. Sua instância implementada do serviço {{site.data.keyword.conversationshort}} é hospedada pelo {{site.data.keyword.cloud_notm}}, a plataforma de computação em nuvem da IBM. (Veja [Visão geral da plataforma ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/docs/overview/ibm-cloud.html#overview) para obter mais informações.)

Leia mais sobre essas etapas de implementação seguindo estes links:

- [Planejando suas intenções e entidades](intents-entities.html#planning-your-entities)
- [Visão geral do diálogo](dialog-overview.html)
- [Visão Geral da Implementação](deploy.html)

## Suporte ao navegador

O conjunto de ferramentas do serviço {{site.data.keyword.conversationshort}} requer o mesmo nível de software do navegador que é requerido pelo {{site.data.keyword.Bluemix_notm}}. Veja o tópico {{site.data.keyword.Bluemix_notm}} [Pré-requisitos do ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/docs/overview/prereqs.html#browsers){: new_window} para obter detalhes.

## Suporte ao idioma

O suporte ao idioma por recurso é detalhado no tópico [Idiomas suportados](lang-support.html).

## Próximas etapas

- [Introdução](getting-started.html) com o serviço
- Tente algumas [demos](sample-applications.html).
- Visualize a lista de [SDKs ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/watson/developercloud/developer-tools.html){: new_window}.

Ainda tem dúvidas? Entre em contato com o [IBM Sales ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.
