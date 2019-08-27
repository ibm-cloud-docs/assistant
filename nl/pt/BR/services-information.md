---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

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

# Informações sobre serviços do IBM Cloud
{: #services-information}

O assistente é um robô totalmente hospedado que é gerenciado pelo {{site.data.keyword.cloud}}, o que significa que você não precisa se preocupar em configurar ou manter a infraestrutura para suportá-lo.
{: shortdesc}

## Informações do plano de serviço
{: #services-information-plans}

Explore as [opções de plano de serviço ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window} do {{site.data.keyword.conversationshort}}.

Antes de criar uma instância de serviço, decida como você deseja organizar os recursos em sua conta do {{site.data.keyword.cloud_notm}}. Se você não definir seu próprio grupo de recursos, o grupo de recursos **padrão** será usado e *não será possível* mudá-lo posteriormente. Consulte [Melhores práticas para organizar recursos em um grupo de recursos ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/docs/resources/bestpractice_rgs#bp_resourcegroups){: new_window} para obter mais detalhes. Todos os usuários devem ter uma função de acesso de plataforma de Operador. (As funções de acesso ao serviço não são alavancadas pelo {{site.data.keyword.conversationshort}}.)

Para descobrir o plano de serviço ao qual sua instância atual pertence, conclua estas etapas:

1.  Anote o nome da instância que você está utilizando atualmente (é possível localizar e mudar a instância nas páginas principais Qualificações ou Assistentes).
1.  Acesse a página [Lista de recursos do IBM Cloud ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/resources).
1.  Expanda a seção **Serviços**, localize o nome da instância anotado anteriormente e clique nele para ver as informações do plano associado.

### Limites de plano por tipo de artefato
{: #services-information-limits}

As informações sobre os limites de artefato por plano estão disponíveis nos tópicos que descrevem como criar os artefatos, portanto, será possível consultar os limites quando precisar conhecê-los. Aqui estão os links para os tópicos:

- [Assistentes](/docs/services/assistant?topic=assistant-assistant-add#assistant-add-limits)
- [Nós de diálogo](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-node-limits)
- [Entidades ](/docs/services/assistant?topic=assistant-entities#entities-limits)
- [Tempo limite de inatividade](/docs/services/assistant?topic=assistant-assistant-settings#assistant-settings-session-limits)
- [Intents](/docs/services/assistant?topic=assistant-intents#intents-limits)
- [Integrações](/docs/services/assistant?topic=assistant-deploy-integration-add#deploy-integration-add-limits)
- [Logs](/docs/services/assistant?topic=assistant-logs#logs-limits)
- [Habilidades](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)
- [ Versões ](/docs/services/assistant?topic=assistant-versions#versions-limits)

### Limites da chamada API
{: #services-information-api-limits}

O número de chamadas API permitidas por instância depende de seu plano de serviço. Consulte a descrição do plano para obter detalhes.

Se você tiver um plano Lite e atingir o limite de sua chamada API, mas os registros mostrarem que você fez menos chamadas do que o esperado, lembre-se de que o plano Lite armazena informações de log por apenas 7 dias.

Se você desejar fazer upgrade de um plano para outro, veja [Fazendo upgrade](/docs/services/assistant?topic=assistant-upgrade).

### Recursos dos planos Plus e Premium ![Somente nos planos Plus ou Premium](images/plus.png)
{: #services-information-premium}

Os recursos a seguir estão disponíveis apenas para os usuários dos planos Plus ou Premium.

- [ Desambiguação ](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)
- [ Resolução de conflitos de Intenção ](/docs/services/assistant?topic=assistant-intents#intents-resolve-conflicts)
- [Recomendações de intenção e recomendações de exemplo de usuário da intenção](/docs/services/assistant?topic=assistant-intent-recommendations)
- [ Integração de Intercom ](/docs/services/assistant?topic=assistant-deploy-intercom)
- [ Habilidade de procura ](/docs/services/assistant?topic=assistant-skill-search-add)

### Planos Baseados em Usuário
{: #services-information-user-based-plans}

Ao contrário de planos baseados em API, que medem o uso pelo número de chamadas da API feitas durante um intervalo de tempo especificado, o novo plano Plus e o plano Premium atualizado usam o faturamento baseado em usuário. Eles medem o uso pelo número de usuários exclusivos que interagiram com o assistente durante um intervalo de tempo especificado.

O {{site.data.keyword.conversationshort}} verifica as informações a seguir, nessa ordem, em solicitações de API para propósitos de faturamento:

  1.  **user_id**: uma propriedade definida na API que é enviada no objeto de contexto de uma chamada da API /message. O uso dessa propriedade é a melhor maneira de assegurar que você atribua com precisão as chamadas da API /message a usuários exclusivos. Para obter mais informações sobre a propriedade de ID do usuário, consulte a documentação de referência da API:
  
    - ` context.global.system.user_id `:  [ v2 API ](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant)
    - ` context.metadata.user_id `:  [ v1 API ](https://cloud.ibm.com/apidocs/assistant#get-response-to-user-input)

  1.  **session_id**: uma propriedade definida na API v2 que identifica uma única conversa entre um usuário e o assistente. Um ID de sessão é fornecido em chamadas da API /message que são geradas pelas integrações integradas. A sessão termina quando um usuário fecha a janela de bate-papo ou depois que o bate-papo fica inativo por 60 minutos.

  1.  **conversation_id**: uma propriedade definida na API v1 que é armazenada no objeto de contexto de uma chamada da API /message. Essa propriedade pode ser usada para identificar múltiplas chamadas da API /message que estão associadas a uma única troca de conversação com um usuário. No entanto, o mesmo ID será usado somente se você retiver explicitamente o ID e passá-lo de volta com cada solicitação feita como parte da mesma conversa. Caso contrário, um novo ID será gerado para cada nova chamada da API /message.

Para obter todos os benefícios dos novos planos de serviços baseados no usuário, projete quaisquer aplicativos customizados usados para implementar seu assistente para capturar um ID de usuário ou de sessão exclusivo e transmitir as informações para o {{site.data.keyword.conversationshort}}.

## Autenticando chamadas API
{: #services-information-authenticate-api-calls}

O mecanismo de autenticação usado por sua instância de serviço afeta como deve-se fornecer credenciais ao fazer uma chamada da API.

1.  Obtenha as credenciais de serviço.

    - Localize e clique na instância de serviço na [Lista de recursos do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com){: new_window}.

    - Clique para abrir sua instância de serviço, clique em **Credenciais de serviço** e, em seguida, clique em **Visualizar credenciais**.

      **Credenciais do Cloud Foundry**

      ![Mostra a página de credenciais de serviço para instâncias hospedadas pelo Cloud Foundry.](images/cf-cred-ui.png)

      ** Credenciais do IAM **

      ![Mostra a página de credenciais de serviço para instâncias hospedadas pelo IAM.](images/iam-creds.png)

1.  Use essas credenciais em sua chamada da API.

    ** Chamada API do Cloud Foundry **

    Forneça suas credenciais de nome de usuário e senha.

    ```curl
    curl -X GET \
    --user {username}:{password} \
    'https://gateway.watson.net/assistant/api/v1/workspaces?version=2018-09-20 '
    ```
    {: codeblock}

     ** Chamada API do IAM **

    - A URL base deve incluir o local. Use a sintaxe `gateway-<location>.watsonplatform.net` para especificar o local de criação da instância de serviço. Os códigos de local são listados na tabela *Locais do data center*.
    - Forneça o tipo apropriado de token no cabeçalho. É possível passar um token de acesso ou uma chave de API.

      - Tokens suportam solicitações autenticadas sem a integração de credenciais de serviço em cada chamada. O exemplo a seguir mostra um token de acesso sendo usado.

        ```curl
        curl -X GET \
        'https://gateway-syd.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        --header 'Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs'
        ```
        {: codeblock}

      - As chaves API usam autenticação básica. O exemplo a seguir mostra um apikey sendo usado.

        ```curl
        curl -X GET -u " apikey:3Df ... ...Y7Pc9" \
        'https://gateway-us-east.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20 ' \
        ```
        {: codeblock}

        Ao usar qualquer um dos SDKs do Watson, é possível passar a chave de API e deixar que o SDK gerencie o ciclo de vida dos tokens.
        {: note}

        Os recursos do IAM não podem ser gerenciados com a Interface da Linha de Comandos (CLI) do Cloud Foundry. Por exemplo, os comandos da CLI do Cloud Foundry (iniciando com `cf`) que criam ou gerenciam instâncias de serviço não funcionam com instâncias hospedadas em locais usando o IAM. Em vez disso, deve-se usar a CLI do {{site.data.keyword.cloud_notm}} e seus comandos associados. Consulte [Trabalhando com recursos e grupos de recursos ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource) para obter mais detalhes.

        Consulte [Autenticando com tokens do IAM ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/watson?topic=watson-iam){: new_window} para obter mais informações.

    Para obter exemplos, consulte [Autenticação ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/assistant-v2#authentication){: new_window} para seu idioma na referência de API.

### Data centers
{: #services-information-regions}

O {{site.data.keyword.cloud_notm}} tem uma rede de data centers globais que fornecem benefícios de desempenho para seus serviços de nuvem. Consulte [Data centers globais do {{site.data.keyword.cloud_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/cloud/data-centers/){: new_window} para obter mais detalhes.

O {{site.data.keyword.cloud_notm}} mudou do gerenciamento de acesso de usuário com o Cloud Foundry para o uso da autenticação do Identity and Access Management (IAM) baseada em token. O IAM foi apresentado em diferentes locais em diferentes momentos. É possível migrar uma instância de serviço para movê-la de sua organização e espaço atuais do Cloud Foundry para um grupo de recursos. Consulte  [ Migrando ](/docs/services/watson?topic=watson-migrate)  para obter mais detalhes.

É possível criar instâncias de serviço do {{site.data.keyword.conversationshort}} que estão hospedadas nos locais de data center a seguir:

| Localização    | Código do local | Tipo de autenticação | Data de adoção do IAM | Notas |
|-------------|---------------|---------------------|-------------------|-------|
| Dallas      | us-south      | IAM                 | 30 de outubro de 2018 | N/A |
| Frankfurt   | eu-de         | IAM                 | 30 de outubro de 2018 | N/A |
| Sydney      | au-syd        | IAM                 | 7 de maio de 2018 | As instâncias criadas antes de 7 de maio foram organizadas em Dallas |
| Tóquio       | jp-tok        | IAM                 | 8 de novembro de 2018 | N/A |
| Londres      | eu-gb, lon    | IAM                 | 13 de dezembro de 2018 | As instâncias que foram criadas na região do Reino Unido antes de 13 de dezembro foram organizadas para a região Sul dos EUA |
| Washington DC  | nós-leste    | IAM                 | 14 de junho de 2018 | N/A |
{: caption="Locais do data center" caption-side="top"}

Para obter informações sobre os data centers nos quais outros serviços do {{site.data.keyword.cloud_notm}} estão hospedados, consulte [Serviços por região ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/docs/resources/services_region#services_region){: new_window}.

## Termos e segurança
{: #services-information-terms}

Para saber mais sobre os termos de serviço e a segurança de dados, leia as informações a seguir:

- [Termos de serviço ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/home?OpenDocument){: new_window}
- [Segurança e privacidade de dados ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/software/sla/sladb.nsf/sla/csdsp?OpenDocument){: new_window}
- [Segurança de Informações](/docs/services/assistant?topic=assistant-information-security)

Consulte [Visão geral da plataforma ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/overview?topic=overview-whatis-platform){: new_window} para obter mais informações sobre o {{site.data.keyword.cloud_notm}}.

## Ainda tem dúvidas? 
{: #services-information-sales}

Entre em contato com o [IBM Sales ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.
