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

# Construindo uma habilidade de diálogo
{: #beta-skill-dialog-add}

O processamento de linguagem natural para o serviço {{site.data.keyword.conversationshort}} é definido em uma *qualificação de diálogo*, que é um contêiner para todos os artefatos que definem um fluxo de conversa.
{: shortdesc}

Esse recurso fica disponível para uso por participantes somente no programa beta. Para descobrir como solicitar acesso, consulte [Participar do programa beta](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

![Beta](images/beta.png) A IBM libera serviços, recursos e suporte ao idioma classificados como beta para sua avaliação. Esses recursos podem ficar instáveis, podem mudar frequentemente e podem ser descontinuados com breve aviso. Os recursos beta podem também não fornecer o mesmo nível de desempenho ou compatibilidade que os recursos geralmente disponíveis fornecem e não são destinados ao uso em um ambiente de produção. 

É possível incluir uma qualificação de diálogo em um assistente. Consulte [Limites de qualificação](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits) para obter informações sobre os limites por plano.

## Criando uma habilidade de diálogo
{: #beta-skill-dialog-add-task}

É possível criar uma qualificação do zero, usar uma qualificação de amostra fornecida pela IBM ou importar uma qualificação de um arquivo JSON.

Se você ainda não tiver feito isso, conclua as etapas de pré-requisito no [tutorial de introdução](/docs/services/assistant?topic=assistant-getting-started#getting-started-prerequisites) para criar uma instância de serviço do {{site.data.keyword.conversationshort}} e ativar a ferramenta.

Você usa a ferramenta {{site.data.keyword.conversationshort}} para construir qualificações. Siga estas etapas para criar uma qualificação de diálogo:

1.  Clique na guia **Qualificações**.

    Se você criou ou recebeu acesso à função de desenvolvedor a quaisquer áreas de trabalho construídas com a versão geralmente disponível do serviço {{site.data.keyword.conversationshort}} (antigo Watson Conversation), você as verá listadas aqui como qualificações de diálogo.
    {: note}

1.  Clique em **Criar novo**.

1.  Clique em **Incluir** para criar uma *qualificação de diálogo*.

1.  Execute um dos seguintes procedimentos:

    - Para criar uma qualificação do zero, clique em **Criar qualificação**.
    - Para incluir uma qualificação de amostra fornecida com o serviço como um ponto de início para sua própria qualificação ou como um exemplo a ser explorado antes de você mesmo criar um, clique em **Usar qualificação de amostra** e, em seguida, clique na amostra que deseja usar.

      A qualificação de amostra é incluída em sua lista de qualificações. Ele não é associado a nenhum assistente. Ignore as etapas restantes neste procedimento.

    - Para incluir uma qualificação existente nessa instância de serviço, é possível importá-la como um arquivo JSON. Clique em **Importar qualificação** e, em seguida, clique em **Escolher arquivo JSON** e selecione o arquivo JSON que você deseja importar.

      **Importante:**

      - O arquivo JSON importado deve usar codificação UTF-8.
      - O tamanho máximo de um arquivo JSON de qualificação é 10 MB. Se for necessário importar uma qualificação maior, considere importar as intenções e as entidades separadamente depois de ter importado a qualificação. (Também é possível importar qualificações maiores usando a API de REST. Para obter informações adicionais, consulte a [Referência da API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#create_workspace){: new_window}.)
      - O JSON não pode conter guias, novas linhas ou retornos de linha.

      Especifique os dados que você deseja incluir:

        - Selecione **Tudo (Intenções, entidades e diálogo)** se quiser importar uma cópia completa da qualificação exportada, incluindo o diálogo.
        - Selecione **Intenções e entidades** se quiser usar as intenções e as entidades da qualificação exportada, mas planeja construir um novo diálogo.

      Clique em  ** Importar **.

      Se você tiver problemas para importar uma qualificação, consulte [Resolução de problemas de importação de qualificação](#beta-skill-dialog-add-import-errors).

1.  Especifique os detalhes para a habilidade:

    - **Nome**: um nome com não mais de 100 caracteres de comprimento. Um nome é necessário.
    - **Descrição**: uma descrição opcional com não mais de 200 caracteres de comprimento.
    - **Linguagem**: a linguagem da entrada do usuário com a qual a qualificação será treinada para entender. O valor padrão é inglês.

Depois que a qualificação é criada, ela aparece como um quadro na página Qualificações. Agora é possível começar a identificar os objetivos do usuário os quais você deseja que a qualificação de diálogo trate.

- Para incluir intenções pré-construídas em sua qualificação, consulte [Usando catálogos de conteúdo](/docs/services/assistant?topic=assistant-catalog).
- Para definir suas próprias intenções, consulte [Intenções](/docs/services/assistant?topic=assistant-intents).

A qualificação do diálogo não pode interagir com os clientes até que seja incluída em um assistente e o assistente seja implementado. Consulte  [ Criando assistentes ](/docs/services/assistant?topic=assistant-assistant-add).

### Resolução de problemas de importação de habilidades
{: #beta-skill-dialog-add-import-errors}

Se você receber uma mensagem informando que a qualificação contém artefatos que excedem os limites impostos por seu plano de serviço, conclua as etapas a seguir para importar a qualificação com êxito:

1.  Compre um plano com limites maiores de artefatos.
1.  Crie uma instância de serviço no novo plano.
1.  Importe a qualificação para a nova instância de serviço.
1.  Faça edições na qualificação para que ela atenda aos requisitos de limite de artefato do plano que você deseja usar no futuro. Talvez seja necessário diminuir o número de nós de diálogo usados na árvore de diálogo, por exemplo.
1.  Exporte a qualificação editada fazendo download dela.
1.  Tente novamente importar a qualificação editada para a instância de serviço original no plano desejado.

### Incluindo a habilidade em um assistente
{: #beta-skill-dialog-add-to-assistant}

É possível incluir uma qualificação em um assistente. Deve-se abrir o quadro de assistente e incluir a qualificação no assistente por meio da página de configuração do assistente; não é possível escolher o assistente que usará a qualificação na página de configuração de qualificação. Uma qualificação de diálogo pode ser usada por mais de um assistente.

1.  Na guia Assistentes, clique para abrir o quadro do assistente no qual você deseja incluir a qualificação.

1.  Clique em  ** Incluir Skill de Diálogo **.

1.  Clique em  ** Incluir habilidade existente **.

    Clique na qualificação que você deseja incluir entre as qualificações disponíveis exibidas.

    Se você estiver incluindo uma qualificação de diálogo existente e criou ou recebeu acesso de função de desenvolvedor para quaisquer áreas de trabalho que foram construídas com a versão geralmente disponível do serviço {{site.data.keyword.conversationshort}} (anteriormente o Watson Conversation), você as verá listadas aqui como qualificações de diálogo.
    {: note}

## Fazendo download de uma habilidade de diálogo
{: #beta-skill-dialog-add-download}

É possível fazer download de uma qualificação de diálogo no formato JSON. Talvez você queira fazer download de uma qualificação caso queira usar a mesma qualificação de diálogo em uma instância diferente do serviço {{site.data.keyword.conversationshort}}, por exemplo. É possível fazer download dele de uma instância e importá-lo para outra instância como uma nova qualificação de diálogo.

Para fazer download de uma qualificação de diálogo, conclua as etapas a seguir:

1.  Localize o quadro de qualificações de diálogo na página Qualificações ou na página de configuração de um assistente que usa a qualificação.

1.  Clique no ícone ![abrir e fechar a lista de opções](images/kabob-beta.png) e, em seguida, escolha **Fazer download de JSON**.

1.  Especifique um nome para o arquivo JSON e onde salvá-lo e, em seguida, clique em **Salvar**.

Também é possível exportar uma qualificação usando a API. Inclua o parâmetro `export=true` com a solicitação. Consulte [Referência de API](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace) para obter mais detalhes.

## Compartilhando uma qualificação de diálogo com membros da equipe
{: #beta-skill-dialog-add-invite-others}

Depois de criar a instância de serviço, é possível fornecer a outras pessoas acesso a ela. Junto, é possível definir os dados de treinamento e construir o diálogo.

Somente uma pessoa pode editar uma intenção, uma entidade ou um nó de diálogo de cada vez. Se múltiplas pessoas trabalham no mesmo item ao mesmo tempo, as mudanças feitas pela pessoa que as salva por último são as únicas mudanças aplicadas. As mudanças que são feitas durante o mesmo prazo por outra pessoa e são salvas primeiro não são retidas. Coordene as atualizações que você planeja fazer com seus membros da equipe para evitar que alguém perca o trabalho.
{: important}

Para compartilhar uma qualificação de diálogo com outras pessoas, deve-se conceder a elas acesso à instância de serviço que hospeda a qualificação. Observe que a pessoa que você convidar poderá acessar qualquer qualificação hospedada por essa instância de serviço.

1.  Anote o nome da instância atual, que é exibida na parte superior da página atual.
1.  Clique no ícone Usuário ![Usuário](images/user-icon2.png) no cabeçalho da página e selecione **Gerenciar usuários** na lista suspensa.

1.  Clique em **Convidar usuários** e, em seguida, insira os endereços de e-mail das pessoas em sua equipe para quem você deseja fornecer acesso.

    Se você forneceu a alguém acesso a uma instância de serviço no Cloud Foundry, a pessoa poderá ser listada como um usuário já convidado. Clique no nome da pessoa para abrir as configurações de gerenciamento de acesso do usuário. Clique em **Designar acesso** e, em seguida, escolha **Designar acesso aos recursos**.
1.  Na seção *Serviços*, faça as seleções a seguir, no mínimo:

    - ** Serviços **:  {{site.data.keyword.conversationshort}}
    - ** Designar funções de acesso da plataforma **: Operador

    Para obter mais informações sobre funções de gerenciamento da plataforma, consulte [Acesso ao IAM ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/iam?topic=iam-userroles). (As funções de acesso ao serviço não são utilizadas pelo {{site.data.keyword.conversationshort}} por padrão.)

    Para instâncias mais antigas gerenciadas pelo Cloud Foundry, deve-se expandir a seção *Acesso ao Cloud Foundry*, escolher sua organização e, em seguida, designar a pessoa à função de espaço **Desenvolvedor**.
    {: note}

1.  Clique em **Convidar usuários**.

    Se você estiver editando o acesso para um usuário existente, clique em **Designar acesso**.

Quando as pessoas que você convidar em seguida efetuarem login no {{site.data.keyword.cloud_notm}}, sua conta será incluída em sua lista de contas. Se eles selecionarem sua conta, poderão ver sua instância de serviço e abrir e editar suas qualificações.

Com mais pessoas contribuindo para o desenvolvimento de qualificações do diálogo, podem ocorrer mudanças indesejadas, incluindo exclusões de qualificações. Considere criar cópias de backup de sua qualificação de diálogo regularmente, para que seja possível recuperar uma versão anterior, se necessário. Para criar um backup, basta [fazer download da qualificação como um arquivo JSON](#beta-skill-dialog-add-download).
