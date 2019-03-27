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

# Migrando
{: #migrate}

Migre uma instância de serviço do {{site.data.keyword.conversationshort}} para movê-la de sua organização e do espaço do Cloud Foundry atuais para um grupo de recursos.
{: shortdesc}

O {{site.data.keyword.cloud_notm}} foi movido do uso do Cloud Foundry para o uso de grupos de recursos.

Os grupos de recursos oferecem esses benefícios sobre o Cloud Foundry:

- Controle de acesso de granularidade mais baixa usando o IBM Cloud Identity and Access Management (IAM)
- Capacidade de conectar instâncias de serviço a apps e serviços em diferentes regiões
- Simplifica a captura de dados de uso por grupo

Se você criou instâncias de serviço antes de novembro de 2018, dependendo do local no qual sua instância está hospedada, ela pode estar usando o Cloud Foundry em vez de grupos de recursos. Consulte [Data centers](/docs/services/assistant?topic=assistant-services-information#services-information-regions) para obter informações sobre quando cada local foi iniciado para usar o IAM com novas instâncias.

## Migrando uma instância de serviço
{: #migrate-task}

Se desejar saber mais sobre o processo de migração antes de iniciar, consulte a [documentação de migração do IBM Cloud ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/resources?topic=resources-migrate).

Somente a pessoa ou o grupo que criou a instância ou recebeu o acesso de função `Developer` para a instância pode migrá-la.
{: note}

Para migrar sua instância de serviço, conclua estas etapas:

1.  Determine para qual grupo de recursos você deseja mover a instância de serviço primeiro.

    Consulte [Melhores práticas para organizar o recurso em um grupo de recursos ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/resources?topic=resources-bp_resourcegroups) para obter dicas.

1.  Na lista de serviços do IBM Cloud Dashboard, clique no ícone de migração ![Migrar](images/migrate.svg) para a instância que você deseja migrar e, em seguida, clique em **Migrar** no pop-up.

    Todas as instâncias de serviço que foram criadas no data center de Sydney antes de 7 de maio de 2018 ou no data center de Londres antes de 13 de dezembro de 2018 foram organizadas para o data center de Dallas. Ao migrar uma instância de serviço do Cloud Foundry baseada em Sydney ou em Londres, ela é convertida em um recurso que está hospedado em Dallas.
    {: note}

1.  Clique em **Continuar** e, em seguida, escolha um grupo de recursos.

    Se você não tiver criado um grupo de recursos, será possível criar um agora. Um grupo de recursos  ** padrão **  está disponível. Reserve um tempo para entender como os grupos são usados e crie um, se necessário. O grupo de recursos que você escolher aqui *não poderá* ser mudado posteriormente.

1.  Clique em  ** Migrar **.

    Uma mensagem será exibida quando o processo estiver pronto. Se você tiver outras instâncias de serviço para migrar, será possível continuar a migração de outras instâncias de serviço ou clicar em **Pronto**.

A instância de serviço antiga (baseada na organização do Cloud Foundry) que você migrou continua a ser listada na seção Serviços do Cloud Foundry do Painel e agora é mostrada como um *alias* da nova versão (baseada em grupo de recursos) da instância.

![Mostra que a instância de serviço atual é agora um alias de uma instância de recurso baseada em recurso](images/alias.png)

Para obter mais informações sobre aliases, consulte a [Documentação do IBM Cloud Connections ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/docs/resources/connecting_apps#what_is_alias).

Deve-se abrir a nova versão baseada em grupo de recursos da instância de serviço para acessar o botão **Ativar ferramenta**. A nova instância é listada na seção Serviços do Painel do {{site.data.keyword.Bluemix_notm}}.

## Autenticação
{: #migrate-auth-support}

Se houver aplicativos existentes que usam autenticação básica para acessar o serviço, eles poderão continuar a passar um nome do usuário e uma senha para autenticação na instância de serviço após a migração. É possível ver as informações de credenciais na página **Credenciais de serviço** do alias.

Sua nova instância gerencia a autenticação com o IBM Cloud Identity and Access Management (IAM), que é um mecanismo aprimorado que usa chaves de API em vez de credenciais de nome do usuário e senha. É possível ver as informações da chave de API na página **Credenciais de serviço** da nova instância de serviço.

Considere atualizar seus aplicativos customizados existentes de forma que eles usem o novo método de autenticação para aproveitar a segurança melhorada que ele oferece. Quaisquer mudanças feitas nas qualificações de diálogo na nova instância são refletidas também em aplicativos que usam credenciais de autenticação básica. Depois de atualizar todos os apps para usar a nova abordagem de chave de API, você não precisará do alias e poderá excluí-lo.
