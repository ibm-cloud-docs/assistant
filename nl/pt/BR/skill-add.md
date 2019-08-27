---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-02"

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

# Criando uma Habilidade
{: #skill-add}

Customize seu assistente incluindo nele as qualificações necessárias para atender aos objetivos de seus clientes.
{: shortdesc}

É possível criar o tipo de qualificação a seguir:

- **Qualificação de diálogo**: usa o processamento de língua natural do Watson e as tecnologias de aprendizado de máquina para entender as perguntas e solicitações do usuário e respondê-las com respostas criadas por você.

- **Qualificação de procura** ![Somente nos planos Plus ou Premium](images/plus.png): para uma determinada consulta do usuário, usa o serviço {{site.data.keyword.discoveryfull}} para realizar a procura em uma origem de dados de seu conteúdo de autoatendimento e retornar uma resposta.

  Apenas usuários dos planos Plus ou Premium podem criar esse tipo de qualificação.
  
  Normalmente, você cria uma qualificação de cada tipo primeiro. Em seguida, à medida que constrói um diálogo para a qualificação de diálogo, você decide quando iniciar a qualificação de procura. Para algumas perguntas ou solicitações, uma resposta codificada permanentemente ou programaticamente derivada (que é definida na qualificação de diálogo) é suficiente. Para outras, é possível fornecer uma resposta mais robusta, retornando uma passagem completa de informações relacionadas (que são extraídas de uma origem de dados externa usando a qualificação de procura).

## Criar a qualificação
{: #skill-add-task}

É possível incluir uma qualificação de cada tipo de qualificação em um assistente.

Para criar uma qualificação, conclua a etapa a seguir:

1.  Clique na guia **Qualificações** e, em seguida, clique em **Criar qualificação**.

1.  Escolha o tipo de qualificação a ser criado e, em seguida, clique em **Avançar**.

    Siga as etapas restantes no procedimento apropriado para concluir o processo de criação da qualificação.

      - [Qualificação de diálogo](/docs/services/assistant?topic=assistant-skill-dialog-add)
      - [Qualificação de procura](/docs/services/assistant?topic=assistant-skill-search-add)

## Limites de Skill
{: #skill-add-limits}

O número de qualificações que podem ser criadas depende de seu tipo de plano do {{site.data.keyword.conversationshort}}. Todas as qualificações de diálogo de amostra disponíveis para uso não são contabilizadas em seu limite, a menos que sejam usadas. A versão de uma qualificação não conta como uma qualificação.

| Plano     | Habilidades por instância de serviço |
|------------------|----------------------------:|
| Premium          |                          50 |
| Mais             |                          50 |
| Padrão         |                          20 |
| Lite`*`, Plus Trial |                        5 |
{: caption="Detalhes do plano" caption-side="top"}

`*` Após 30 dias de inatividade, uma qualificação não utilizada em uma instância de serviço do plano Lite poderá ser excluída para liberar espaço.

## Excluindo uma qualificação
{: #skill-add-delete}

É possível excluir qualquer qualificação que possa ser acessada, a menos que ela esteja sendo usada por um assistente. Se ela está em uso, deve-se removê-la do assistente que está usando-a antes de poder excluí-la.

Certifique-se de verificar com qualquer pessoa que possa estar usando a qualificação antes de excluí-la.
{: tip}

Para excluir uma qualificação, conclua as etapas a seguir:

1.  Descubra se a qualificação está sendo usada por algum assistente. Na guia Qualificações, localize o tile para a qualificação que você deseja excluir. O campo **Assistentes** lista os assistentes que usam atualmente a qualificação.

1.  Se a qualificação que você deseja excluir estiver associada a um assistente, remova-a do assistente concluindo as etapas a seguir:

    - Verifique com o proprietário do assistente que está usando a qualificação antes de remover a qualificação dele.
    - Abra a guia Assistentes e, em seguida, clique para abrir o tile de assistente.
    - Localize o tile para a qualificação que você deseja excluir. Clique no ícone ![abrir e fechar a lista de opções](images/kabob-beta.png) e, em seguida, escolha **Remover**.
    - Repita as etapas anteriores para quaisquer outros assistentes que usem a qualificação.
    - Retorne para a guia Qualificações e localize o tile para a qualificação que você deseja excluir.

1.  Clique no ícone ![abrir e fechar a lista de opções](images/kabob-beta.png) e, em seguida, escolha **Excluir**. Confirme a exclusão.
