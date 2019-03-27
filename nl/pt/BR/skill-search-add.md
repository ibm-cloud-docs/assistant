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

# Construindo uma habilidade de procura
{: #skill-search-add}

Um assistente usa uma *qualificação de procura* para rotear consultas complexas de clientes para o serviço {{site.data.keyword.discoveryfull}}. O {{site.data.keyword.discoveryshort}} trata a entrada do usuário como uma consulta de procura. Ele localiza informações que são relevantes para a consulta de uma origem de dados externa e as retorna para o assistente.
{: shortdesc}

Esse recurso fica disponível para uso por participantes somente no programa beta. Para descobrir como solicitar acesso, consulte [Participar do programa beta](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

![Beta](images/beta.png) A IBM libera serviços, recursos e suporte ao idioma classificados como beta para sua avaliação. Esses recursos podem ficar instáveis, podem mudar frequentemente e podem ser descontinuados com breve aviso. Os recursos beta podem também não fornecer o mesmo nível de desempenho ou compatibilidade que os recursos geralmente disponíveis fornecem e não são destinados ao uso em um ambiente de produção.

É possível incluir uma qualificação de procura em um assistente. Consulte [Limites de qualificação](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits) para obter informações sobre os limites por plano.

A qualificação de procura faz a procura informações de uma coleta de dados que você cria usando o serviço {{site.data.keyword.discoveryshort}}. O {{site.data.keyword.discoveryshort}} é um serviço que efetua crawl, converte e normaliza seus dados não estruturados. O serviço aplica a análise de dados e a intuição cognitiva para enriquecer seus dados de forma que seja possível localizar e recuperar mais facilmente informações significativas dele posteriormente. Para ler mais sobre o {{site.data.keyword.discoveryshort}}, consulte a [documentação do produto ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/discovery?topic=discovery-about).

O serviço {{site.data.keyword.discoveryfull}} é acionado das maneiras a seguir:

- **Nó Anything else**: procura uma origem de dados externa para obter uma resposta relevante quando nenhum dos nós de diálogo pode direcionar a consulta do usuário. Em vez de mostrar uma mensagem padrão, tal como `I don't know how to help you with that.`, o assistente pode dizer `Maybe this information can help:` seguido pela passagem retornada pela procura. Se uma qualificação de procura estiver vinculada a seu assistente, sempre que o nó `anything_else` for acionado, em vez de exibir a resposta do nó, uma procura será executada no lugar. O assistente passa a entrada do usuário como a consulta para sua qualificação de procura e retorna os resultados da procura como a resposta.
- **Tipo de resposta de procura**: se você incluir um tipo de resposta de procura em um nó de diálogo, o serviço recuperará uma passagem de uma origem de dados externa e a retornará como a resposta para uma pergunta específica. Esse tipo de procura ocorre somente quando o nó de diálogo individual é processado. Essa abordagem será útil se você desejar limitar uma consulta do usuário antes de executar uma procura. Por exemplo, a ramificação de diálogo pode coletar informações sobre o tipo de dispositivo que o cliente deseja comprar. Quando você conhece a fabricação e o modelo, é possível enviar uma palavra-chave de modelo na consulta que é enviada para a qualificação da procura e obter melhores resultados.
- **Somente qualificação de procura**: se somente uma qualificação de procura estiver vinculada a um assistente e nenhuma qualificação de diálogo estiver vinculada ao assistente, uma consulta de procura será enviada para o serviço {{site.data.keyword.discoveryshort}} quando qualquer entrada do usuário for recebida de um dos canais de integração do assistente.

## Criando uma habilidade de procura
{: #skill-search-add-task}

Se você ainda não tiver feito isso, conclua as etapas de pré-requisito no [tutorial de introdução](/docs/services/assistant?topic=assistant-getting-started#getting-started-prerequisites) para criar uma instância de serviço do {{site.data.keyword.conversationshort}} e ativar a ferramenta {{site.data.keyword.conversationshort}}.

Você usa a ferramenta {{site.data.keyword.conversationshort}} para criar qualificações. Siga estas etapas para criar uma qualificação de procura:

1.  Clique na guia **Qualificações**.

1.  Clique em **Criar novo**.

1.  Clique em **Incluir** para criar uma *qualificação de procura*.

1.  Especifique os detalhes para a nova qualificação:
    - **Nome**: um nome com não mais de 100 caracteres de comprimento. Um nome é necessário.
    - **Descrição**: uma descrição opcional com não mais de 200 caracteres de comprimento.

1.  Clique em **Criar**.

As etapas restantes diferem dependendo se você tem acesso, ou não, a uma instância de serviço existente do {{site.data.keyword.discoveryshort}} com coleções criadas. Siga o procedimento apropriado para sua situação:

- [Conectar-se a uma instância do Watson Discovery existente](#skill-search-add-connect-discovery)
- [ Criar uma instância do Watson Discovery ](#skill-search-add-create-discovery)

## Conectar-se a uma instância de serviço existente do Watson Discovery
{: #skill-search-add-connect-discovery}

1.  Escolha a instância de serviço do {{site.data.keyword.discoveryshort}} da qual você deseja extrair informações.
{: #choose-d-instance}

    Quaisquer instâncias de serviço do {{site.data.keyword.discoveryshort}} que você tenha acesso serão exibidas na lista.

    Se aparecer um aviso de que algumas de suas instâncias de serviço do {{site.data.keyword.discoveryshort}} não têm credenciais configuradas, isso significa que você tem acesso a pelo menos uma instância que nunca foi aberta por si mesmo no painel do {{site.data.keyword.cloud_notm}} diretamente. Deve-se acessar uma instância de serviço para que credenciais sejam criadas para ela. E as credenciais devem existir antes que o {{site.data.keyword.conversationshort}} possa estabelecer uma conexão com a instância de serviço do {{site.data.keyword.discoveryshort}} em seu nome. Se você achar que uma instância de serviço do {{site.data.keyword.discoveryshort}} que deveria estar listada não está, abra a instância no painel do {{site.data.keyword.cloud_notm}} diretamente para gerar credenciais para ela.

1.  Indique a coleta de dados a ser usada, executando uma das coisas a seguir:
{: #pick-data-collection}

    - Escolha uma coleção de dados existente.

      É possível clicar no link *Abrir no Discovery* para revisar a configuração de uma coleta de dados antes de decidir qual usar.

      Acesse [Configurar a procura](#beta-search-skill-add-configure).

    - Se você não tiver uma coleta ou não desejar usar nenhuma das coletas de dados que estiverem listadas, clique em **Criar uma nova coleta** para incluir uma. Siga o procedimento em [Criar uma coleta de dados](#beta-search-skill-add-create-discovery-collection).

      O botão **Criar uma nova coleta** não será exibido se o limite tiver sido atingido para o número de coletas que você tem permissão para criar com base em seu plano de serviço do {{site.data.keyword.discoveryshort}}. Consulte [Planos de precificação do {{site.data.keyword.discoveryshort}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/discovery/discovery-about?topic=discovery-discovery-pricing-plans) para obter detalhes do limite do plano.

## Criar uma instância de serviço do Watson Discovery
{: #skill-search-add-create-discovery}

1.  Para criar uma instância de serviço do {{site.data.keyword.discoveryshort}}, clique em **Criar nova coleta**.

    Uma instância do serviço {{site.data.keyword.discoveryshort}} é criada para você e uma página de configuração é aberta para a nova instância de serviço do {{site.data.keyword.discoveryshort}}.

    Uma instância do plano Lite do serviço é provisionada no {{site.data.keyword.Bluemix_notm}}, não importa qual plano de serviço do {{site.data.keyword.conversationshort}} você use. Se desejar criar a instância de serviço do {{site.data.keyword.discoveryshort}} como parte de um plano diferente, pare aqui. Crie a instância de serviço diretamente do [Catálogo do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/catalog/services/discovery) e siga o procedimento [Conectar-se a uma instância de serviço do {{site.data.keyword.discoveryshort}}](#skill-search-add-connect-discovery).
    {: important}

1.  Revise os termos e condições para usar a instância e, em seguida, clique em **Aceitar** para continuar.

1.  [ Crie uma coleção de dados ](#skill-search-add-create-discovery-collection).

## Criar uma coleção de dados
{: #skill-search-add-create-discovery-collection}

Não tente incluir a origem de dados pré-enriquecida denominada *Watson Discovery News* em sua instância. Esse não é um tipo de dados que pode ser procurado no {{site.data.keyword.conversationshort}}.
{: important}

1.  Para criar uma coleta do {{site.data.keyword.discoveryshort}}, execute uma das ações a seguir:

      - Para criar uma coleta dos dados que estão armazenados em um tipo de origem de dados para o qual o {{site.data.keyword.discoveryshort}} fornece suporte integrado, clique em **Conectar-se à origem de dados**.

        1.  Escolha um tipo de origem de dados.
        1.  Forneça as informações necessárias para a origem de dados que você escolher e, em seguida, clique em **Conectar**.

            Consulte [Conectando-se a origens de dados ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/discovery?topic=discovery-sources) para obter mais detalhes.
        1.  Indique a frequência com a qual você deseja que os dados da origem de dados sejam sincronizados com a coleta que você está criando no {{site.data.keyword.discoveryshort}}.
        1.  Especifique as informações que você deseja extrair da origem de dados e incluir em sua coleta do {{site.data.keyword.discoveryshort}} .

            As opções exibidas são diferentes, dependendo do tipo de origem de dados.

            - Para uma origem de dados Salesforce, você seleciona os tipos de objeto dos quais deseja extrair os documentos de origem. Você pode selecionar um [Tipo de objeto de caso ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_case.htm#!) que representa um *caso*, que é uma emissão ou um problema do cliente, por exemplo.
            - Para uma origem de dados Sharepoint, você especifica caminhos.
            - Para repositórios de arquivo, você especifica diretórios ou arquivos.

        1.  Clique em  ** Salvar e sincronizar dados **.

            A coleta de dados é criada. Após a conclusão do processo, uma página de resumo é exibida na ferramenta {{site.data.keyword.discoveryshort}} em uma guia do navegador da web separada.
        1.  Clique em **Configurar sua qualificação no {{site.data.keyword.conversationshort}}** para retornar para a ferramenta {{site.data.keyword.conversationshort}}.

      - Para criar uma coleta fazendo upload de documentos, clique em **Fazer upload de seus próprios dados**.

        1.  Primeiro, você define a coleta e, em seguida, faz upload dos documentos. Forneça as informações a seguir:

            - Nome da coleta. O nome deve ser exclusivo para essa instância de serviço.
            - Configuração. É possível escolher usar um modelo de configuração padrão ou uma configuração salva. Consulte [Configurando seu serviço ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/discovery?topic=discovery-configservice) para obter mais informações sobre configurações.
            - Idioma. Selecione o idioma dos arquivos que serão incluídos nessa coleta. Consulte [Suporte ao idioma ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/discovery?topic=discovery-language-support) para obter informações sobre os idiomas suportados pelo {{site.data.keyword.discoveryshort}}.
        1.  Faça upload de documentos.

            Os tipos de arquivo suportados incluem arquivos PDF, HTML, JSON e DOC. Consulte [Incluindo conteúdo ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/discovery?topic=discovery-addcontent) para obter mais detalhes.
            {: note}

            Nenhuma sincronização contínua de documentos transferidos por upload está disponível. Se você desejar assimilar as mudanças feitas em um documento, faça upload de uma versão mais recente do documento.

Aguarde até que a coleta seja totalmente alimentada antes de retornar ao {{site.data.keyword.conversationshort}}.

## Configurar a Procura
{: #skill-search-add-configure}

1.  Na página de qualificação de procura do {{site.data.keyword.conversationshort}}, clique em **Configurar**.

1.  Rascunhe mensagens diferentes para compartilhar com os usuários, dependendo do sucesso da procura.

    <table>
    <caption>Procurar mensagens de resultado</caption>
    <tr>
      <th>Nome do campo</th>
      <th>Cenário</th>
      <th>Exemplo de mensagem</th>
    </tr>
    <tr>
      <td>Mensagem</td>
      <td>Os resultados da procura são retornados</td>
      <td>Eu localizei estas informações que podem ser úteis: </td>
    </tr>
    <tr>
      <td>Nenhum resultado localizado</td>
      <td>Nenhum resultado da procura foi localizado</td>
      <td>Eu procurei a minha base de conhecimento para obter informações que possam direcionar sua consulta, mas não encontrei nada útil para compartilhar.</td>
    </tr>
    <tr>
      <td>Mensagem de erro</td>
      <td>O serviço não foi capaz de executar a procura por alguma razão</td>
      <td>Talvez eu tenha informações que possam ajudar a direcionar sua consulta, mas não posso procurar em minha base de conhecimento no momento.</td>
    </tr>
    </table>

1.  Escolha os campos de coleta do {{site.data.keyword.discoveryshort}} dos quais você deseja extrair texto.

    Os campos que estão disponíveis diferem com base nos dados que você alimentou e na configuração usada para alimentá-los.

    Cada resultado da procura pode consistir nestas partes de informações:

    - **Título**: título do resultado da procura. Use o título, o nome ou o tipo de campo semelhante da coleta como o título do resultado da procura.

      Algo diferente de `None` deve ser selecionado para que as integrações do Facebook e do Slack exibam a resposta.
    - ** Corpo **: descrição do resultado da procura. Use um campo de abstrato, resumo ou destaque da coleta como o corpo do resultado da procura.

      Algo diferente de `None` deve ser selecionado para que as integrações do Facebook e do Slack exibam a resposta.
    - **URL**: um link de hipertexto para o objeto de dados original em sua origem de dados nativa. A maioria das origens de dados on-line fornece URLs públicas autorreferentes para objetos no armazenamento para suportar acesso direto.

      A URL resultante deve ser válida e atingível para que a integração do Slack inclua a URL na resposta e para que a integração do Facebook exiba a resposta de qualquer maneira. `None` é uma seleção aceitável para as integrações do Facebook e do Slack.

    Consulte [Dicas para seleção de campo de coleta](#skill-search-add-field-tips) para obter ajuda.
  
    Deve-se escolher um valor diferente de `None` para pelo menos uma das opções.

    Se nenhuma opção estiver disponível nos campos suspensos, poderá ser necessário dar ao {{site.data.keyword.discoveryshort}} mais tempo para concluir a criação da coleta. Caso contrário, sua coleta poderá não conter nenhum documento ou poderá ter erros de ingestão que precisam ser direcionados primeiro.

1.  Na área de janela de visualização, insira uma mensagem de teste para ver os resultados que são retornados quando as opções de configuração são aplicadas à procura. Faça ajustes conforme necessário.

1.  Clique em **Criar**.

Se você desejar mudar a configuração posteriormente, abra a qualificação de procura novamente e faça edições. Você não precisa salvar as mudanças à medida que as faz; elas são aplicadas automaticamente. Quando estiver satisfeito com os resultados da procura, clique em **Salvar** para concluir a configuração da qualificação de procura.

## Próximas etapas
{: #skill-search-add-next-steps}

Depois que a qualificação é criada, ela aparece como um quadro na página Qualificações.

A qualificação de procura não pode interagir com os clientes até que ela seja incluída em um assistente e o assistente seja implementado. Consulte  [ Criando assistentes ](/docs/services/assistant?topic=assistant-assistant-add).

Quando você vincular uma qualificação de diálogo e uma qualificação de procura a um assistente, a qualificação de procura será acionada automaticamente se a entrada do usuário for processada pela qualificação de diálogo e não puder ser direcionada por nenhum de seus nós de diálogo. Em vez de responder com uma resposta genérica do nó `anything_else`, uma procura que usa a entrada do usuário como sua sequência de consultas é iniciada.

Se desejar, será possível definir uma consulta de procura específica para ser chamada em resposta a uma condição do nó específica. Para fazer isso, inclua um tipo de resposta de procura no nó de diálogo. Consulte  [ Respostas ](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)  para obter mais detalhes.

Se você iniciar qualquer tipo de procura em sua qualificação de diálogo, teste o diálogo para assegurar que a procura esteja sendo acionada conforme o esperado. Por exemplo, se você não estiver usando tipos de resposta de procura, teste se uma procura será acionada somente quando nenhum nó de diálogo existente puder direcionar a entrada do usuário. E sempre que uma procura for acionada, assegure-se de que ela retorne resultados significativos.

### Dicas para seleção de campo de coleta
{: #skill-search-add-field-tips}

Os campos de coleta apropriados para extrair dados variam dependendo da origem de dados de sua coleta e da configuração que você usou para alimentar a origem de dados. Para saber mais sobre a estrutura dos documentos em sua coleta, incluindo os nomes de campos que contêm informações que você pode desejar extrair, abra a coleta na ferramenta {{site.data.keyword.discoveryshort}} e, em seguida, clique em **Visualizar esquema de dados**.

A tabela a seguir fornece campos de coleta que você pode tentar como introdução. Essas sugestões supõem que você usou o modelo de configuração padrão ao criar a coleta.

| Tipo de origem de dados   | Título | Corpo | Url |
|--------------------|-------|------|-----|
| Documentos PDF transferidos por upload | enriched_text.concepts.text | text | Nenhuma |
| Caixa              | Nome | description | listing_url |

Os campos de coleta são criados quando a coleta é criada. Para saber mais sobre os campos que são gerados quando você usa o modelo de configuração padrão para ingestão, como `enriched_text.concepts.text`, consulte [Configurando seu serviço > Incluindo enriquecimentos ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/discovery?topic=discovery-configservice#adding-enrichments).

### Incluindo a habilidade em um assistente
{: #skill-search-add-to-assistant}

É possível incluir uma qualificação em um assistente. Deve-se abrir o quadro de assistente e incluir a qualificação no assistente por meio da página de configuração do assistente; não é possível escolher o assistente que usará a qualificação na página de configuração de qualificação.

Uma qualificação de procura pode ser usada por mais de um assistente.

1.  Na guia Assistentes, clique para abrir o quadro do assistente no qual você deseja incluir a qualificação.

1.  Clique em  ** Incluir Skill de Procura **.

1.  Clique em  ** Incluir habilidade existente **.

    Clique na qualificação que você deseja incluir entre as qualificações disponíveis exibidas.

Configure pelo menos um canal de integração de teste. Não é possível testar a qualificação de procura na área de janela Experimente. Teste a qualificação em um canal de integração, inserindo consultas que acionam a procura. Assegure-se de que a procura esteja sendo acionada adequadamente e que esteja retornando resultados relevantes.

A integração de link compartilhável não funciona atualmente para assistentes com uma qualificação de procura.
{: important}
