---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-20"

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

# Fazendo backup e restaurando dados
{: #backup}

Faça backup e restaure seus dados exportando e, em seguida, importando os dados.
{: shortdesc}

É possível exportar os dados a seguir por meio de uma instância de serviço do {{site.data.keyword.conversationshort}}:

- Dados de treinamento de qualificação de diálogo (intenções e entidades)
- Diálogo de qualificação de diálogo

Não é possível exportar os dados a seguir:

- Qualificação de procura
- Assistente, incluindo quaisquer integrações configuradas

## Retendo logs
{: #backup-retain-logs}

Se você desejar armazenar logs de conversas que os usuários tiveram com seu assistente, será possível usar a API `/logs` para exportar seus dados do log. Consulte [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/apidocs/assistant#list-log-events-in-a-workspace) para obter detalhes.

Para obter o ID de área de trabalho para uma qualificação, no tile de qualificação, clique no ícone ![abrir e fechar a lista de opções](images/kabob-beta.png) e, em seguida, escolha **Visualizar detalhes da API**.
{: tip}

Os logs são armazenados por um período de tempo diferente dependendo de seu plano de serviço. Por exemplo, os planos Lite fornecem logs somente dos últimos 7 dias. Consulte  [ Limites de log ](/docs/services/assistant?topic=assistant-logs#logs-limits)  para obter mais informações.

Não é possível importar logs de uma qualificação para outra.

## Exportando uma habilidade de diálogo
{: #backup-export-skill}

Para fazer backup dos dados de qualificação de diálogo, exporte a qualificação como um arquivo JSON e armazene o arquivo JSON.

1.  Localize o quadro de qualificações de diálogo na página Qualificações ou na página de configuração de um assistente que usa a qualificação.

1.  Clique no ícone ![abrir e fechar a lista de opções](images/kabob-beta.png) e, em seguida, escolha **Fazer download de JSON**.

1.  Especifique um nome para o arquivo JSON e onde salvá-lo e, em seguida, clique em **Salvar**.

Como alternativa, é possível usar a API `/workspaces` para exportar uma qualificação de diálogo. Inclua o parâmetro `export=true` com a solicitação de área de trabalho GET. Consulte a [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace) para obter mais detalhes.

## Importando uma habilidade de diálogo
{: #backup-import-skill}

Para restabelecer uma cópia de backup de uma qualificação de diálogo que você exportou de outra instância de serviço ou ambiente, crie uma nova qualificação de diálogo importando o arquivo JSON da qualificação de diálogo exportada.

Se o serviço {{site.data.keyword.conversationshort}} mudar entre o momento em que você exportar a qualificação e importá-la, devido a atualizações funcionais que são aplicadas regularmente a instâncias em ambientes de entrega contínua hospedados em nuvem, sua qualificação importada poderá funcionar de forma diferente do que fez antes.
{: important}

1.  Clique na guia **Qualificações**.

1.  Clique em **Criar qualificação**.

1.  Se tiver a opção de escolher entre mais de um tipo de qualificação, escolha criar uma qualificação de diálogo.

1.  Clique em **Importar qualificação** e, em seguida, clique em **Escolher arquivo JSON** e selecione o arquivo JSON que você deseja importar.

    **Importante:**

    - O arquivo JSON importado deve usar codificação UTF-8, sem codificação de marca de ordem de byte (BOM).
    - O tamanho máximo de um arquivo JSON de qualificação é 10 MB. Se for necessário importar uma qualificação maior, considere importar as intenções e as entidades separadamente depois de ter importado a qualificação. (Também é possível importar qualificações maiores usando a API de REST. Para obter informações adicionais, consulte a [Referência da API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/apidocs/assistant#create-workspace){: new_window}.)
    - O arquivo JSON não pode conter guias, novas linhas ou retornos de linha.

    Selecione **Tudo (intenções, entidades e diálogo)** para importar uma cópia completa da qualificação exportada.

    Clique em  ** Importar **.

    Se você tiver problemas para importar uma qualificação, consulte [Resolução de problemas de importação de qualificação](/docs/services/assistant?topic=assistant-skill-add#skill-add-import-errors).

1.  Especifique os detalhes para a habilidade:

    - **Nome**: um nome com não mais de 100 caracteres de comprimento. Um nome é necessário.
    - **Descrição**: uma descrição opcional com não mais de 200 caracteres de comprimento.
    - **Linguagem**: a linguagem da entrada do usuário com a qual a qualificação será treinada para entender. O valor padrão é inglês.

Depois de criar a qualificação de diálogo, ela aparece como um bloco na página Qualificações.

## Reccriando seu assistente
{: #backup-recreate-assistant}

Agora é possível recriar seu assistente. Em seguida, é possível vincular sua qualificação de diálogo importada ao assistente e configurar integrações para ela.

Consulte [Criando um assistente](/docs/services/assistant?topic=assistant-assistant-add) e [Incluindo integrações](/docs/services/assistant?topic=assistant-deploy-integration-add#deploy-integration-add-task) para obter mais detalhes.

## Atualize seus aplicativos clientes
{: #backup-update-api}

Ao importar uma qualificação de diálogo que você exportou, uma nova habilidade é criada. A nova qualificação tem um novo ID de área de trabalho. Se você tem aplicativos clientes existentes que usam a API v1 para acessar essa qualificação, deve-se atualizar quaisquer referências ao ID de área de trabalho para usar o novo ID de área de trabalho em seu lugar.

Quando você recria seu assistente, um novo ID de assistente é fornecido a ele. Se você tem aplicativos clientes existentes que usam a API v2 para acessar o assistente, deve-se atualizar quaisquer referências ao ID de assistente para usar o novo ID de assistente em seu lugar.
