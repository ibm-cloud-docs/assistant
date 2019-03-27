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

# Acessando áreas de trabalho
{: #edit-convo-workspace}

Se você criou uma área de trabalho com uma versão anterior do serviço (mesmo quando ele era conhecido como {{site.data.keyword.watson}} Conversation), é possível continuar a usar a área de trabalho.
{: shortdesc}

## Mudando a área de trabalho de
{: #edit-convo-workspace-task}

Sua área de trabalho ainda está disponível; ela é apenas referida como uma *qualificação* agora. Para fazer mudanças em uma área de trabalho anterior, conclua as etapas a seguir:

1.  Na página inicial do  {{site.data.keyword.conversationshort}} , clique em  ** Skills **.

    Quaisquer áreas de trabalho que você criou com a versão geralmente disponível do serviço {{site.data.keyword.conversationshort}} são exibidas como tiles de qualificação de diálogo.
1.  Clique na qualificação de diálogo que representa a área de trabalho que você deseja editar.
1.  Faça quaisquer mudanças ou inclusões desejadas nos dados de treinamento ou no diálogo.
1.  Salve suas mudanças.

Quando o treinamento for concluído, suas atualizações estarão disponíveis para o aplicativo customizado por meio do qual você está chamando o serviço. Consulte [Visão geral da API do Watson Assistant](/docs/services/assistant?topic=assistant-api-overview) para obter mais informações.

## Limitações
{: #edit-convo-workspace-cons}

Com a versão mais recente do serviço, é possível continuar a fazer tudo o que poderia ser feito com o serviço anterior, mas com mais flexibilidade. Talvez você tenha criado uma área de trabalho com uma versão anterior do serviço e esteja chamando-a de um aplicativo existente que não deseja substituir? Ele já gerencia o estado e executa funções úteis em turnos de diálogo individuais que você deseja continuar controlando. Ainda é possível orquestrar entre as chamadas com a versão mais recente da API /message. A vantagem é que você não tem que fazer isso. Na versão mais recente, é possível suportar mais de um canal de integração de cada vez com a mesma qualificação de diálogo subjacente.

Se escolher ignorar a etapa de criação de um assistente e de inclusão de sua qualificação de diálogo nele, você perderá a simplicidade que os assistentes fornecem. Ou seja, **não é possível** usar uma única conversa para interagir com os clientes por meio de múltiplos canais de integração de uma vez e rapidamente expandir ou alternar para novos canais que se tornam populares com os usuários.

## Habilidades e áreas de trabalho
{: #edit-convo-workspace-names}

O que é apresentado no conjunto de ferramentas como uma qualificação de diálogo é efetivamente um wrapper para uma área de trabalho V1. Embora não haja atualmente nenhum método de API para qualificações de autoria com a API V2, é possível continuar a usar a API V1 para as áreas de trabalho de autoria.

- Quando você abre a ferramenta, quaisquer áreas de trabalho criadas antes de 9 de novembro são mostradas como qualificações. O nome da qualificação é obtido do nome da área de trabalho. No entanto, se você mudar subsequentemente o nome ou a descrição da qualificação, essas mudanças não afetarão a área de trabalho. Da mesma forma, se você usar as APIs para editar o nome ou a descrição da área de trabalho, essas mudanças não afetarão a qualificação.
- Na ferramenta, os detalhes da API para a qualificação mostram o ID de qualificação e o ID de área de trabalho.
