---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-06"

keywords: import workspace, import JSON, export JSON

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

# Criando uma habilidade de diálogo
{: #skill-dialog-add}

O processamento de linguagem natural para o serviço {{site.data.keyword.conversationshort}} é definido em uma *qualificação de diálogo*, que é um contêiner para todos os artefatos que definem um fluxo de conversa.
{: shortdesc}

É possível incluir uma qualificação de diálogo em um assistente. Consulte [Limites de qualificação](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits) para obter informações sobre os limites por plano.

## Criar a qualificação de diálogo
{: #skill-dialog-add-task}

É possível criar uma qualificação do zero, usar uma qualificação de amostra fornecida pela IBM ou importar uma qualificação de um arquivo JSON.

Para incluir uma qualificação, conclua as etapas a seguir:

1.  Clique na guia **Qualificações** e, em seguida, clique em **Criar qualificação**.

1.  Clique no quadro *Qualificação de diálogo* e, em seguida, clique em **Avançar**.

1.  Execute uma das ações a seguir:

    - Para criar uma qualificação do zero, clique em **Criar qualificação**.
    - Para incluir uma qualificação de amostra fornecida com o produto como um ponto de início para sua própria qualificação ou como um exemplo a ser explorado antes de criar uma por conta própria, clique em **Usar a qualificação de amostra** e, em seguida, clique na amostra que deseja usar.

      A qualificação de amostra é incluída em sua lista de qualificações. Ele não é associado a nenhum assistente. Ignore as etapas restantes neste procedimento.

    - Para incluir uma qualificação existente nessa instância de serviço, é possível importá-la como um arquivo JSON. Clique em **Importar qualificação** e, em seguida, clique em **Escolher arquivo JSON** e selecione o arquivo JSON que você deseja importar.

      **Importante:**

      - O arquivo JSON importado deve usar codificação UTF-8, sem codificação de marca de ordem de byte (BOM).
      - O tamanho máximo de um arquivo JSON de qualificação é 10 MB. Se for necessário importar uma qualificação maior, considere importar as intenções e as entidades separadamente depois de ter importado a qualificação. (Também é possível importar qualificações maiores usando a API de REST. Para obter informações adicionais, consulte a [Referência da API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/apidocs/assistant?curl=#create-workspace){: new_window}.)
      - O JSON não pode conter guias, novas linhas ou retornos de linha.

      Especifique os dados que você deseja incluir:

        - Selecione **Tudo (Intenções, entidades e diálogo)** se quiser importar uma cópia completa da qualificação exportada, incluindo o diálogo.
        - Selecione **Intenções e entidades** se quiser usar as intenções e as entidades da qualificação exportada, mas planeja construir um novo diálogo.

      Clique em  ** Importar **.

      Se você tiver problemas para importar uma qualificação, consulte [Resolução de problemas de importação de qualificação](#skill-dialog-add-import-errors).

1.  Especifique os detalhes para a habilidade:

    - **Nome**: um nome com não mais de 100 caracteres de comprimento. Um nome é necessário.
    - **Descrição**: uma descrição opcional com não mais de 200 caracteres de comprimento.
    - **Linguagem**: a linguagem da entrada do usuário com a qual a qualificação será treinada para entender. O valor padrão é inglês.

Depois de criar a qualificação de diálogo, ela aparece como um bloco na página Qualificações. Agora é possível começar a identificar os objetivos do usuário os quais você deseja que a qualificação de diálogo trate.

- Para incluir intenções pré-construídas em sua qualificação, consulte [Usando catálogos de conteúdo](/docs/services/assistant?topic=assistant-catalog).
- Para definir suas próprias intenções, consulte [Definindo intenções](/docs/services/assistant?topic=assistant-intents).

A qualificação do diálogo não pode interagir com os clientes até que seja incluída em um assistente e o assistente seja implementado. Consulte  [ Criando um assistente ](/docs/services/assistant?topic=assistant-assistant-add).

### Resolução de problemas de importação de habilidades
{: #skill-dialog-add-import-errors}

Se você receber uma mensagem informando que a qualificação contém artefatos que excedem os limites impostos por seu plano de serviço, conclua as etapas a seguir para importar a qualificação com êxito:

1.  Compre um plano com limites maiores de artefatos.
1.  Crie uma instância de serviço no novo plano.
1.  Importe a qualificação para a nova instância de serviço.
1.  Faça edições na qualificação para que ela atenda aos requisitos de limite de artefato do plano que você deseja usar no futuro. Talvez seja necessário diminuir o número de nós de diálogo usados na árvore de diálogo, por exemplo.
1.  Exporte a qualificação editada fazendo download dela.
1.  Tente novamente importar a qualificação editada para a instância de serviço original no plano desejado.

### Incluindo a habilidade em um assistente
{: #skill-dialog-add-to-assistant}

É possível incluir uma qualificação de diálogo em um assistente. Deve-se abrir o quadro de assistente e incluir a qualificação no assistente por meio da página de configuração do assistente; não é possível escolher o assistente que usará a qualificação na página de configuração de qualificação. Uma qualificação de diálogo pode ser usada por mais de um assistente.

1.  Na guia Assistentes, clique para abrir o quadro do assistente no qual você deseja incluir a qualificação.

1.  Clique em  ** Incluir Skill de Diálogo **.

1.  Clique em  ** Incluir habilidade existente **.

    Clique na qualificação que você deseja incluir entre as qualificações disponíveis exibidas.

Ao incluir uma qualificação de diálogo daqui, você obtém a versão de desenvolvimento. Se desejar incluir a versão de uma qualificação específica, inclua-a na guia *Versões* da qualificação.

## Fazendo download de uma habilidade de diálogo
{: #skill-dialog-add-download}

É possível fazer download de uma qualificação de diálogo no formato JSON. Talvez você queira fazer download de uma qualificação caso queira usar a mesma qualificação de diálogo em uma instância diferente do serviço {{site.data.keyword.conversationshort}}, por exemplo. É possível fazer download dele de uma instância e importá-lo para outra instância como uma nova qualificação de diálogo.

Para fazer download de uma qualificação de diálogo, conclua as etapas a seguir:

1.  Localize o quadro de qualificações de diálogo na página Qualificações ou na página de configuração de um assistente que usa a qualificação.

1.  Clique no ícone ![abrir e fechar a lista de opções](images/kabob-beta.png) e, em seguida, escolha **Fazer download de JSON**.

1.  Especifique um nome para o arquivo JSON e onde salvá-lo e, em seguida, clique em **Salvar**.

Também é possível exportar uma qualificação usando a API. Inclua o parâmetro `export=true` com a solicitação. Consulte [Referência de API](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace) para obter mais detalhes.

## Compartilhando uma qualificação de diálogo com membros da equipe
{: #skill-dialog-add-invite-others}

Depois de criar a instância de serviço, é possível fornecer a outras pessoas acesso a ela. Junto, é possível definir os dados de treinamento e construir o diálogo.

Somente uma pessoa pode editar uma intenção, uma entidade ou um nó de diálogo de cada vez. Se múltiplas pessoas trabalham no mesmo item ao mesmo tempo, as mudanças feitas pela pessoa que as salva por último são as únicas mudanças aplicadas. As mudanças que são feitas durante o mesmo prazo por outra pessoa e são salvas primeiro não são retidas. Coordene as atualizações que você planeja fazer com seus membros da equipe para evitar que alguém perca o trabalho.
{: important}

Para compartilhar uma qualificação de diálogo com outras pessoas, deve-se conceder a elas acesso à instância de serviço que hospeda a qualificação. Observe que a pessoa que você convidar poderá acessar qualquer qualificação hospedada por essa instância de serviço.

1.  Anote o nome da instância atual, que é exibida na parte superior da página atual.
1.  Clique no ícone Usuário ![Usuário](images/user-icon2.png) no cabeçalho da página e selecione **Gerenciar usuários** na lista suspensa.

1.  Na área de janela de navegação, clique em **Usuários**.

    Se você forneceu anteriormente o acesso a uma instância de serviço para alguém, essa pessoa poderá já estar listada como um usuário convidado. Para mudar o nível de acesso de alguém à instância, clique no menu ao lado do nome da pessoa, escolha **Designar acesso** e, em seguida, clique em **Designar acesso aos recursos**.
    
1.  Clique em **Convidar usuários**. 

1.  Na seção *Serviços*, escolha o serviço {{site.data.keyword.conversationshort}}.

1.  Selecione ao menos uma região e uma instância de serviço para compartilhar com esse usuário.

1.  Conceda, no mínimo, as designações a seguir a esse usuário:
 
    - **Funções de acesso à plataforma**: operador
    - **Funções de acesso ao serviço**: gravador

    Para obter mais informações sobre funções, consulte [Acesso ao IAM ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/docs/iam?topic=iam-userroles).

    Para instâncias mais antigas gerenciadas pelo Cloud Foundry, deve-se expandir a seção *Acesso ao Cloud Foundry*, escolher sua organização e, em seguida, designar a pessoa à função de espaço **Desenvolvedor**.
    {: note}

1.  Clique em **Convidar usuários**.

    Se você estiver editando o acesso para um usuário existente, clique em **Designar acesso**.

Quando as pessoas que você convidar em seguida efetuarem login no {{site.data.keyword.cloud}}, sua conta será incluída em sua lista de contas. Se eles selecionarem sua conta, poderão ver sua instância de serviço e abrir e editar suas qualificações.

Com mais pessoas contribuindo para o desenvolvimento de qualificações do diálogo, podem ocorrer mudanças indesejadas, incluindo exclusões de qualificações. Considere criar cópias de backup de sua qualificação de diálogo regularmente, para que seja possível recuperar uma versão anterior, se necessário. Para criar um backup, basta [fazer download da qualificação como um arquivo JSON](#skill-dialog-add-download).
