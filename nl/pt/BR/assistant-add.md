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

# Criando um assistente
{: #assistant-add}

Crie um assistente com as qualificações necessárias para tratar das metas de negócio de seus clientes.
{: shortdesc}

Siga estas etapas para criar um assistente:

1.  Clique na guia  ** Assistentes ** .

1.  Execute um dos seguintes procedimentos:

    - Para criar um assistente de amostra por meio do qual é possível revisar e aprender, clique em **Incluir uma amostra** e, em seguida, escolha o assistente de amostra a ser criado.

      O assistente de amostra foi incluído. É possível ignorar as etapas restantes neste procedimento.

      Uma qualificação de amostra é fornecida com o assistente de amostra e é incluída em sua lista de qualificações. Se você já criou uma qualificação de amostra do mesmo tipo, a qualificação existente será associada a esse novo assistente de amostra automaticamente.
      {: note}

    - Para criar um assistente a partir do zero, clique em **Criar novo** e, em seguida, conclua as etapas restantes neste procedimento.

1.  Especifique os detalhes para o novo assistente:
    - **Nome**: um nome com não mais de 100 caracteres de comprimento. Um nome é necessário.
    - **Descrição**: uma descrição opcional com não mais de 200 caracteres de comprimento.

    Uma página da web pública da marca IBM é criada automaticamente, a qual você e sua equipe podem usar para testar seu assistente. Se você não desejar que a página da web de visualização seja criada, desmarque a caixa de seleção **Ativar link de visualização**.

1.  Clique em **Criar**.

1.  Inclua uma qualificação no assistente clicando em **Incluir qualificação**. É possível escolher incluir uma qualificação existente ou criar uma nova.

    Ao incluir uma qualificação aqui, você obtém a versão de desenvolvimento. Se quiser incluir uma versão de qualificação específica, faça isso na guia *Histórico de versões* de qualificação, como alternativa.

    Se você criou ou recebeu o acesso de função de desenvolvedor para quaisquer áreas de trabalho que foram construídas com a versão geralmente disponível do serviço {{site.data.keyword.conversationshort}} (anteriormente Watson Conversation), você as verá listadas como qualificações de diálogo existentes.
    {: note}

    Consulte [Criando uma qualificação](/docs/services/assistant?topic=assistant-skill-add) para obter mais informações sobre como criar uma qualificação.

## Limites do Assistente
{: #assistant-add-limits}

O número de assistentes que podem ser criados em uma única instância de serviço depende de seu plano do {{site.data.keyword.conversationshort}}.

| Plano de Serviço | Assistentes por instância de serviço | Integrações por assistente  | Período de inatividade da sessão de bate-papo |
|--------------|--------------------------------:|----------------------------:|-----------------:|
| Premium      |                             100 |                         100 |       60 minutos |
| Mais         |                             100 |                         100 |       60 minutos |
| Padrão     |                             100 |                         100 |        5 minutos |
| Lite*        |                             100 |                         100 |        5 minutos |
{: caption="Detalhes do plano de serviço" caption-side="top"}

*Depois de 30 dias de inatividade, um assistente não usado em uma instância de serviço do plano Lite pode ser excluído para liberar espaço.

É possível conectar uma qualificação a seu assistente. O número de qualificações que podem ser construídas difere dependendo do plano que você tem. Consulte  [ Limites de Skill ](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)  para obter mais detalhes.

## Excluindo um assistente
{: #assistant-add-delete}

Quando você exclui um assistente, quaisquer integrações definidas para o assistente também são excluídas automaticamente. As qualificações que você incluiu no assistente não são excluídas.

Para excluir um assistente, siga estas etapas:

1.  Na guia Assistentes, localize o assistente que você deseja excluir.

1.  Clique no ícone ![abrir e fechar a lista de opções](images/kabob-beta.png) e, em seguida, escolha **Excluir**. Confirme a exclusão.

## Renomeando um assistente
{: #assistant-add-rename}

É possível mudar o nome de um assistente e sua descrição associada depois de criar o assistente.

Para renomear um assistente, siga estas etapas:

1.  Na guia Assistentes, localize o assistente que você deseja renomear.

1.  Clique no ícone ![abrir e fechar lista de opções](images/kabob-beta.png) e, em seguida, escolha **Renomear**.

1.  Edite o nome e, em seguida, clique em **Renomear** para salvar suas mudanças.

### Mudando a qualificação que está associada ao assistente
{: #assistant-add-swap-skill}

É possível incluir uma qualificação em um assistente. Se desejar mudar a qualificação usada por seu assistente, será possível trocar uma qualificação por outra.

1.  Na guia Assistentes, clique para abrir o tile para o assistente para o qual você deseja mudar a qualificação.

1.  Clique no ícone ![abrir e fechar lista de opções](images/kabob-beta.png) e, em seguida, escolha **Trocar qualificação**. Para trocar a qualificação atual por uma versão diferente da qualificação, escolha **Mudar versão de qualificação**.

1.  Escolha uma qualificação existente para usar no lugar ou [crie uma qualificação](/docs/services/assistant?topic=assistant-skill-add).

### Alternando entre instâncias de serviço
{: #assistant-add-switch-instance}

Se você tiver mais de uma instância de serviço, será possível verificar o cabeçalho da página para descobrir qual instância você está usando atualmente. Se você estiver trabalhando em uma qualificação, clique no link de trilha de navegação **Qualificações** primeiro. O banner exibe o nome da instância atual. Para alternar para uma instância de serviço diferente, clique em **mudar** e, em seguida, escolha a instância apropriada.
