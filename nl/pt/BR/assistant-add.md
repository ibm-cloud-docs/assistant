---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-26"

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

Antes de tudo, para compreender melhor o que é um assistente, consulte [Assistentes](/docs/services/assistant?topic=assistant-assistants).

Siga estas etapas para criar um assistente:

1.  Clique em **Criar assistente**.

1.  Inclua detalhes sobre o novo assistente:

    - **Nome**: um nome com não mais de 100 caracteres de comprimento. Um nome é necessário.
    - **Descrição**: uma descrição opcional com não mais de 200 caracteres de comprimento.

    Uma página da web pública da marca IBM é criada automaticamente, a qual você e sua equipe podem usar para testar seu assistente. Se você não desejar que a página da web de visualização seja criada, desmarque a caixa de seleção **Ativar link de visualização**.

1.  Clique em **Criar assistente**.

1.  Inclua uma qualificação no assistente escolhendo um dos tipos de qualificação a seguir.

    **Nota**: é possível optar por incluir uma qualificação existente ou criar uma nova.

    - **Incluir qualificação de diálogo**: usa tecnologias de processamento de língua natural e aprendizado de máquina do Watson para entender perguntas e solicitações do usuário e respondê-las da maneira elaborada por você.

      Ao incluir uma qualificação de diálogo daqui, você obtém a versão de desenvolvimento. Para incluir a versão de uma qualificação de diálogo específica, use a página *Versões* da qualificação.

    - **Incluir qualificação de procura** ![Somente nos planos Plus ou Premium](images/plus.png): para uma determinada consulta do usuário, usa o serviço {{site.data.keyword.discoveryfull}} para recuperar informações de uma origem de dados identificada e compartilha quaisquer informações relevantes localizadas como resposta ao usuário.

      Essa opção ficará visível somente se você for um usuário dos planos Plus ou Premium.
      {: note}

    Consulte [Criando uma qualificação](/docs/services/assistant?topic=assistant-skill-add).

## Limites do Assistente
{: #assistant-add-limits}

O número de assistentes que podem ser criados depende do tipo de plano do seu {{site.data.keyword.conversationshort}}.

| Plano | Assistentes por instância de serviço |
|--------------|--------------------------------:|
| Premium      |                             100 |
| Mais         |                             100 |
| Padrão     |                             100 |
| Plus Trial   |                               5 |
| Lite*        |                             100 |
{: caption="Detalhes do plano" caption-side="top"}

*Depois de 30 dias de inatividade, um assistente não usado em uma instância de serviço do plano Lite pode ser excluído para liberar espaço.

Consulte [Mudando a configuração de tempo limite de inatividade](/docs/services/assistant?topic=assistant-assistant-settings) para obter mais informações sobre o assunto.

É possível conectar uma qualificação de cada tipo ao seu assistente. O número de qualificações que podem ser construídas difere dependendo do plano que você tem. Consulte  [ Limites de Skill ](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)  para obter mais detalhes.

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

É possível incluir uma qualificação de cada tipo de qualificação em um assistente. Para mudar uma qualificação que seu assistente está usando, troque uma qualificação por outra.

1.  Na guia Assistentes, abra o assistente.

1.  Clique no ícone ![abrir e fechar a lista de opções](images/kabob-beta.png) para a qualificação que deseja trocar e, em seguida, escolha **Trocar qualificação**.

    Para trocar a qualificação de diálogo atual por uma versão diferente, escolha **Mudar versão da qualificação**.

1.  Escolha uma qualificação existente para usar no lugar ou [crie uma qualificação](/docs/services/assistant?topic=assistant-skill-add).

### Alternando entre instâncias de serviço
{: #assistant-add-switch-instance}

Se você tiver mais de uma instância de serviço, será possível verificar o cabeçalho da página para descobrir qual instância você está usando atualmente. Se você estiver trabalhando em uma qualificação, clique no link de trilha de navegação **Qualificações** primeiro. O banner exibe o nome da instância atual. Para alternar para uma instância de serviço diferente, clique em **Mudar** e, em seguida, escolha a instância apropriada.
