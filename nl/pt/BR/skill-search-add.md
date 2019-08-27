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

# Criando uma qualificação de procura ![Somente nos planos Plus ou Premium](images/plus.png)
{: #skill-search-add}

Um assistente usa uma *qualificação de procura* para rotear consultas complexas de clientes para o serviço {{site.data.keyword.discoveryfull}}. O {{site.data.keyword.discoveryshort}} trata a entrada do usuário como uma consulta de procura. Ele localiza informações que são relevantes para a consulta de uma origem de dados externa e as retorna para o assistente.
{: shortdesc}

Esse recurso está disponível somente para usuários do plano Plus ou Premium.
{: note}

Inclua uma qualificação de procura no seu assistente para evitar que ele tenha que dizer coisas como `I'm sorry. I can't help you with that`. Em vez disso, o assistente pode consultar documentos ou dados existentes da empresa para ver se alguma informação útil pode ser encontrada e compartilhada com o cliente.

![Mostra o resultado de uma procura na integração do link de visualização](images/search-skill-preview-link.png)

O vídeo de 4 minutos a seguir fornece uma visão geral da qualificação de procura.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Visão geral da qualificação de procura" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/ZcgGf8J2Cfw?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Para saber mais sobre como a qualificação de procura pode beneficiar sua empresa, [leia esta postagem do blog ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://medium.com/ibm-watson/adding-search-to-watson-assistant-99e4e81839e5){: new_window}.

## Como ele Funciona
{: #skill-search-add-how}

A qualificação de procura faz a procura informações de uma coleta de dados que você cria usando o serviço {{site.data.keyword.discoveryshort}}.

O {{site.data.keyword.discoveryshort}} é um serviço que efetua crawl, converte e normaliza seus dados não estruturados. O produto aplica a análise de dados e a intuição cognitiva para enriquecer seus dados, possibilitando mais facilmente a localização e a recuperação posteriores de informações significativas. Para ler mais sobre o {{site.data.keyword.discoveryshort}}, consulte a [documentação do produto ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/discovery?topic=discovery-about){: new_window}.

Normalmente, o tipo de coleta de dados incluído no {{site.data.keyword.discoveryshort}} e acessado pelo seu assistente contêm informações de propriedade da sua empresa. Essas informações protegidas por direitos autorais podem incluir perguntas mais frequentes, materiais paralelos de vendas, manuais técnicos ou artigos escritos por especialistas no assunto. Explore essa densa coleção de informações protegidas por direitos autorais para encontrar rapidamente respostas às perguntas dos clientes.

O diagrama a seguir ilustra como a entrada do usuário é processada quando uma qualificação de diálogo e uma qualificação de procura são incluídas em um assistente.

![Diagrama que mostra que algumas entradas do usuário são respondidas por diálogo e outras por procura.](images/search-skill-diagram.png)

## Antes de iniciar
{: #skill-search-add-prereqs}

Se não tiver uma instância de serviço do {{site.data.keyword.discoveryshort}}, uma instância gratuita do plano Lite será provisionada para você como parte desse processo. Se tiver uma instância de serviço existente do {{site.data.keyword.discoveryshort}}, conecte-se a ela. A criação de uma nova instância não é solicitada como parte desse processo.

Se uma instância do Discovery for criada primeiro, não inclua a origem de dados pré-enriquecida chamada *Watson Discovery News* em sua instância. Esse não é um tipo de dados que pode ser procurado no {{site.data.keyword.conversationshort}}.
{: tip}

## Criar a qualificação de procura
{: #skill-search-add-task}

1.  Clique na guia **Qualificações** e, em seguida, clique em **Criar qualificação**.

1.  Clique no quadro *Qualificação de procura* e, em seguida, clique em **Avançar**.

    Somente será possível selecionar a qualificação de procura se você for um usuário dos planos Plus ou Premium.
    {: note}

1.  Especifique os detalhes para a nova qualificação:
    - **Nome**: um nome com não mais de 100 caracteres de comprimento. Um nome é necessário.
    - **Descrição**: uma descrição opcional com não mais de 200 caracteres de comprimento.

1.  Clique em **Continuar**.

As etapas restantes diferem dependendo se você tem acesso, ou não, a uma instância de serviço existente do {{site.data.keyword.discoveryshort}} com coleções criadas. Siga o procedimento apropriado para sua situação:

- [Conectar-se a uma instância do Watson Discovery existente](#skill-search-add-connect-discovery)
- [ Criar uma instância do Watson Discovery ](#skill-search-add-create-discovery)

## Conectar-se a uma instância de serviço existente do Watson Discovery
{: #skill-search-add-connect-discovery}

1.  Escolha a instância de serviço do {{site.data.keyword.discoveryshort}} da qual você deseja extrair informações.
{: #choose-d-instance}

    Quaisquer instâncias de serviço do {{site.data.keyword.discoveryshort}} que você tenha acesso serão exibidas na lista.

    Se for exibido um aviso de que algumas de suas instâncias de serviço do {{site.data.keyword.discoveryshort}} não têm credenciais configuradas, será possível acessar diretamente pelo menos uma instância nunca antes aberta por meio do painel do {{site.data.keyword.cloud_notm}}. Deve-se acessar uma instância de serviço para que credenciais sejam criadas para ela. E as credenciais devem existir antes que o {{site.data.keyword.conversationshort}} possa estabelecer uma conexão com a instância de serviço do {{site.data.keyword.discoveryshort}} em seu nome. Se suspeitar que uma instância de serviço do {{site.data.keyword.discoveryshort}} está ausente da lista, abra-a diretamente no painel do {{site.data.keyword.cloud}} para gerar credenciais para ela.
    {: note}

1.  Indique a coleta de dados a ser usada, executando uma das coisas a seguir:
{: #pick-data-collection}

    - Escolha uma coleta de dados existente.

      É possível clicar no link *Abrir no Discovery* para revisar a configuração de uma coleta de dados antes de decidir qual usar.

      Acesse [Configurar a procura](#search-skill-add-configure).

    - Se você não tiver uma coleta ou não desejar usar nenhuma das coletas de dados que estiverem listadas, clique em **Criar uma nova coleta** para incluir uma. Siga o procedimento em [Criar uma coleta de dados](#search-skill-add-create-discovery-collection).

      O botão **Criar uma nova coleta** não será exibido se o limite tiver sido atingido para o número de coletas que você tem permissão para criar com base em seu plano de serviço do {{site.data.keyword.discoveryshort}}. Consulte [Planos de precificação do {{site.data.keyword.discoveryshort}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/discovery/discovery-about?topic=discovery-discovery-pricing-plans){: new_window} para obter detalhes de limite do plano.
      {: note}

## Criar uma instância de serviço do Watson Discovery
{: #skill-search-add-create-discovery}

1.  Para criar uma instância de serviço do {{site.data.keyword.discoveryshort}}, clique em **Criar nova coleta**.

    Se não tiver uma instância de serviço existente do {{site.data.keyword.discoveryshort}}, será criada para você uma instância gratuita do serviço {{site.data.keyword.discoveryshort}}.

    Uma instância do plano Lite do serviço é provisionada no {{site.data.keyword.Bluemix_notm}}, independentemente do tipo de plano de serviço do {{site.data.keyword.conversationshort}} que você tem.
    {: note}

1.  Revise os termos e condições para usar a instância e, em seguida, clique em **Aceitar** para continuar.

1.  [ Crie uma coleta de dados ](#skill-search-add-create-discovery-collection).

## Criar uma coleta de dados
{: #skill-search-add-create-discovery-collection}

Se tiver um plano Lite do serviço Discovery, você terá a oportunidade de fazer upgrade de seu plano. Se não quiser fazer upgrade agora, clique em **Vamos começar**.

1.  Para criar uma coleta do {{site.data.keyword.discoveryshort}}, execute uma das ações a seguir:

      - Para criar uma coleção dos dados armazenados em um tipo de origem de dados para o qual o {{site.data.keyword.discoveryshort}} fornece suporte integrado, escolha um tipo de origem de dados.

        1.  Forneça as informações necessárias para a origem de dados que você escolher e, em seguida, clique em **Conectar**.

            Consulte [Conectando-se a origens de dados ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/discovery?topic=discovery-sources){: new_window} para obter mais detalhes.
        1.  Indique a frequência com a qual você deseja que os dados da origem de dados sejam sincronizados com a coleta que você está criando no {{site.data.keyword.discoveryshort}}.
        1.  Especifique as informações que você deseja extrair da origem de dados e incluir em sua coleta do {{site.data.keyword.discoveryshort}} .

            As opções exibidas são diferentes, dependendo do tipo de origem de dados.

            - Para uma origem de dados Salesforce, você seleciona os tipos de objeto dos quais deseja extrair os documentos de origem. Você pode selecionar um [Tipo de objeto de caso ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_case.htm#!) que representa um *caso*, que é uma emissão ou um problema do cliente, por exemplo.
            - Para uma origem de dados Sharepoint, você especifica caminhos.
            - Para repositórios de arquivo, você especifica diretórios ou arquivos.
            - Para uma origem de dados de crawl da web, especifique a URL base de um website no qual deseja fazer crawl. A página da web especificada e as páginas às quais ela se conecta têm o crawl feito e um documento é criado por página da web.

            Aguarde alguns minutos até que o Watson comece a criar documentos. Assim que a origem começa a ser alimentada, o número de documentos exibidos na página de detalhes do {{site.data.keyword.discoveryshort}} aumenta. Pode ser necessário atualizar a página.
            
            Para obter ajuda na criação de origens de dados, consulte [Resolução de problemas](#skill-search-add-troubleshoot).

        1.  Clique em **Salvar e sincronizar objetos**.

            A coleta de dados é criada. Após a conclusão do processo, uma página de resumo é exibida no {{site.data.keyword.discoveryshort}}, que é hospedado em uma guia separada do navegador da web.

      - Para criar uma coleção fazendo upload de documentos, clique em **Fazer upload de documentos**.

        1.  Primeiro, defina a coleção e, em seguida, faça upload dos documentos. Forneça as informações a seguir:

            - Nome da coleta. O nome deve ser exclusivo para essa instância de serviço.
            - Idioma. Selecione o idioma dos arquivos sendo incluídos nessa coleção. Para obter informações sobre os idiomas suportados pelo {{site.data.keyword.discoveryshort}}, consulte [Suporte ao idioma ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/discovery?topic=discovery-language-support){: new_window}.

              Se estiver fazendo upload de um documento em PDF e desejar extrair dele informações sobre a parte, a natureza e a categoria, expanda a seção **Avançado** e clique em **Usar a configuração padrão de contrato com essa coleção**. Consulte [Requisitos da coleção ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/discovery?topic=discovery-element-classification#element-collection){: new_window} para obter mais detalhes.
        1.  Faça upload de documentos.

            Os tipos de arquivo suportados incluem arquivos PDF, HTML, JSON e DOC. Consulte [Incluindo conteúdo ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/discovery?topic=discovery-addcontent){: new_window} para obter mais detalhes.
            {: note}

            Nenhuma sincronização contínua de documentos transferidos por upload está disponível. Se desejar escolher mudanças feitas em um documento, faça upload de uma versão mais recente dele.

Aguarde até que a coleta seja totalmente alimentada antes de retornar ao {{site.data.keyword.conversationshort}}.

### Exemplo de criação de coleção de dados
{: #skill-search-add-json-collection-example}

Por exemplo, é possível que você tenha um arquivo JSON semelhante ao seguinte:

```bash
{
  "Title": "About",
  "Shortdesc": "IBM Watson Assistant is a cognitive bot that you can customize for your business needs, and deploy across multiple channels to bring help to your customers where and when they need it.",
  "Topics": "overview",
  "url": "https://cloud.ibm.com/docs/services/assistant?topic=assistant-index"
}
```
{: codeblock}

Se fizer upload de um arquivo JSON que contenha valores de nome repetidos, somente a primeira ocorrência do par nome/valor será indexada e retornada pela procura. Divida o arquivo em diversos arquivos JSON e faça upload do conjunto.
{: tip}

## Configurar a Procura
{: #skill-search-add-configure}

1.  Na instância do {{site.data.keyword.discoveryshort}}, clique em **Concluir a configuração no Watson Assistant**.

1.  Na página da qualificação de procura do {{site.data.keyword.conversationshort}}, clique em **Configurar**.

1.  Escolha os campos de coleção do {{site.data.keyword.discoveryshort}} dos quais deseja extrair o texto a ser incluído no resultado da procura retornado ao usuário.

    Os campos disponíveis diferem com base nos dados ingeridos.

    Cada resultado da procura pode consistir nas seções a seguir:

    - **Título**: título do resultado da procura. Use o título, o nome ou o tipo de campo semelhante da coleta como o título do resultado da procura.

      Deve-se selecionar algo para o título ou nenhuma resposta do resultado da procura é exibida nas integrações do Facebook e do Slack.
    - ** Corpo **: descrição do resultado da procura. Use um campo de abstrato, resumo ou destaque da coleta como o corpo do resultado da procura.

       Deve-se selecionar algo para o corpo ou nenhuma resposta do resultado da procura é exibida nas integrações do Facebook e do Slack.
    - **URL**: esse campo pode ser preenchido com qualquer conteúdo de rodapé que você queira incluir no final do resultado da procura.

       Por exemplo, talvez você queira incluir um link de hipertexto para o objeto de dados original na origem de dados nativa dele. A maioria das origens de dados on-line fornece URLs públicas autorreferentes para objetos no armazenamento para suportar acesso direto. Ao incluir uma URL, ela deverá ser válida e acessível. Se não for, a integração do Slack não incluirá a URL em sua resposta e a integração do Facebook não retornará nenhuma resposta.

       As integrações do Facebook e do Slack poderão exibir com sucesso a resposta do resultado da procura quando o campo da URL estiver vazio.
  
    Deve-se escolher um valor para pelo menos uma das seções do resultado da procura.
    {: important}

    Consulte [Dicas para seleção de campo de coleta](#skill-search-add-field-tips) para obter ajuda.

    Se nenhuma opção estiver disponível nos campos suspensos, aguarde mais tempo até que o {{site.data.keyword.discoveryshort}} tenha concluído a criação da coleção. Caso não seja criada após o período de espera, a coleção pode não conter nenhum documento ou ter erros de ingestão que precisem de resolução.

    Para continuar o [exemplo do arquivo JSON transferido por upload](#skill-search-add-json-collection-example), uma boa forma de mapeamento é usar os campos *Título*, *Shortdesc* e *URL*.

    ![Mostra que os campos Título, Shortdesc e URL estão selecionados e o cartão de procura de visualização está preenchido com informações desses campos](images/search-skill-configure-fields.png)

    À medida que mapeamentos de campos são incluídos, uma visualização do resultado da procura é exibida com informações dos campos correspondentes de sua coleta de dados. Essa visualização mostra o que é incluído na resposta do resultado da procura retornada aos usuários.

    Para obter ajuda com a configuração da procura, consulte [Resolução de problemas](#skill-search-add-troubleshoot).

1.  Elabore mensagens diferentes para compartilhar com os usuários com base no sucesso da procura.

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
      <td>Encontrei informações que podem ser úteis: </td>
    </tr>
    <tr>
      <td>Nenhum resultado localizado</td>
      <td>Nenhum resultado da procura foi localizado</td>
      <td>Eu procurei a minha base de conhecimento para obter informações que possam direcionar sua consulta, mas não encontrei nada útil para compartilhar.</td>
    </tr>
    <tr>
      <td>Mensagem de erro</td>
      <td>Não consegui concluir a procura por algum motivo</td>
      <td>Talvez eu tenha informações que possam ajudar a direcionar sua consulta, mas não posso procurar em minha base de conhecimento no momento.</td>
    </tr>
    </table>

1.  Clique em **Experimentar** para abrir a área de janela "Experimentar" para o teste. Insira uma mensagem de teste para ver os resultados retornados quando suas opções de configuração são aplicadas à procura. Faça ajustes conforme necessário.

1.  Clique em **Criar**.

Se desejar mudar a configuração do cartão do resultado da procura posteriormente, abra a qualificação de procura novamente e faça edições. Você não precisa salvar as mudanças à medida que as faz; elas são aplicadas automaticamente. Quando estiver satisfeito com os resultados da procura, clique em **Salvar** para concluir a configuração da qualificação de procura.

Se decidir se conectar a uma instância de serviço ou coleta de dados diferente do {{site.data.keyword.discoveryshort}}, crie uma nova qualificação de procura e configure-a para a conexão com outra instância. **Não** é possível mudar os detalhes da instância de serviço ou da coleta de dados para uma qualificação de procura depois da criação dela.
{: important}

### Dicas para seleção de campo de coleta
{: #skill-search-add-field-tips}

Os campos de coleção apropriados para extrair dados variam dependendo da origem de dados de sua coleção e de como ela foi enriquecida. Depois de escolher um tipo de coleta de dados, os valores do campo de coleção são preenchidos com os campos de origem que tenham a maior probabilidade de conter informações úteis, considerando o tipo de origem de dados da coleção. No entanto, você conhece seus dados melhor do que ninguém. É possível mudar os campos de origem para aqueles que contêm as melhores informações para atender às suas necessidades.

Para saber mais sobre a estrutura dos documentos em sua coleção, incluindo os nomes dos campos que contêm informações que podem ser extraídas, abra a coleção no {{site.data.keyword.discoveryshort}} e, em seguida, clique no ícone Visualizar esquema de dados ![Ícone Visualizar esquema de dados](images/icon-view-data-schema.png).

Os campos de origem são criados quando a coleção é criada. Para saber mais sobre os campos gerados para você, como `enriched_text.concepts.text`, consulte [Configurando seu serviço > Incluindo enriquecimentos ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/discovery?topic=discovery-configservice#adding-enrichments){: new_window}.

## Resolução de problemas
{: #skill-search-add-troubleshoot}

Revise essas informações para obter ajuda com a execução de tarefas comuns.

- **Criando uma coleta de dados de crawl da web**: informações necessárias para criar uma origem de dados de crawl da web:

    - Para um plano Lite do {{site.data.keyword.discoveryshort}}, não é possível criar mais de 1.000 documentos. 
    - Para aumentar o número de documentos disponíveis para a coleta de dados, clique em Incluir um grupo de URLs para que seja possível listar as URLs das páginas nas quais você deseja realizar crawl, mas que não estão vinculadas à URL inicial.
    - Para diminuir o número de documentos disponíveis para a coleta de dados, especifique um subdomínio da URL base. Ou, nas configurações de crawl da web, limite o número de saltos permitidos para o Watson da página original. Também é possível especificar subdomínios que serão excluídos explicitamente do crawl.
    - Se nenhum documento for listado após alguns minutos e uma atualização da página, certifique-se de que o conteúdo que deseja alimentar esteja disponível na origem da página da URL. Alguns conteúdos de página da web são gerados dinamicamente e, portanto, não podem ter crawl.

- **Configurando resultados da procura para documentos transferidos por upload**: se você estiver usando uma coleção de documentos transferidos por upload e não conseguir obter os resultados corretos da procura ou se os resultados não forem suficientemente concisos, considere o uso do *Smart Document Understanding* ao criar a coleta de dados. 

  Esse recurso permite a anotação de documentos com base na formatação do texto. Por exemplo, é possível ensinar o {{site.data.keyword.discoveryshort}} que qualquer texto em fonte negrito de tamanho 28 é um título de documento. Ao aplicar essas informações à coleção ao alimentá-la, será possível usar o campo *título* posteriormente como a origem da seção de título do resultado da procura. 
  
  Também é possível usar o Smart Document Understanding para dividir documentos grandes em segmentos mais fáceis de procurar. Para obter mais informações, consulte o tópico [Smart Document Understanding ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/discovery?topic=discovery-sdu) na documentação do {{site.data.keyword.discoveryshort}}.

- **Melhorar os resultados da procura**: se não gostar dos resultados exibidos, revise essas informações para obter ajuda.

  - Chame a qualificação de procura por meio de um nó de diálogo e especifique os detalhes do filtro. 

    Na resposta da qualificação de procura de um nó de diálogo, é possível especificar um filtro de sintaxe de consulta completo do {{site.data.keyword.discoveryshort}} para ajudar a limitar os resultados. 
    
    Por exemplo, é possível definir um filtro que filtra todos os documentos da coleta de dados que não mencionam uma intenção no título do documento ou em algum outro campo de metadados. Outra opção é definir que o filtro filtre documentos que não identificam uma entidade como uma entidade conhecida nos metadados da coleta de dados ou que não mencionam a entidade em nenhum lugar em todo o texto do documento. Para obter detalhes sobre como incluir um tipo de resposta de qualificação de procura, consulte [Incluindo respostas ricas](https://cloud.ibm.com/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia-add).

    Para obter mais dicas sobre como melhorar os resultados, leia a postagem do blog [Melhore os resultados de sua consulta de língua natural no Watson Discovery ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/blogs/improving-your-natural-language-query-results-from-watson-discovery/).

## Próximas etapas
{: #skill-search-add-next-steps}

Depois que a qualificação é criada, ela aparece como um quadro na página Qualificações.

A qualificação de procura não pode interagir com os clientes até que ela seja incluída em um assistente e o assistente seja implementado. Consulte  [ Criando assistentes ](/docs/services/assistant?topic=assistant-assistant-add).

### Incluindo a habilidade em um assistente
{: #skill-search-add-to-assistant}

É possível incluir uma qualificação em um assistente. Abra o quadro do assistente e, nele, inclua a qualificação no assistente. Não é possível escolher o assistente que usará a qualificação na página de configuração dela.

Uma qualificação de procura pode ser usada por mais de um assistente.

1.  Na guia Assistentes, clique para abrir o quadro do assistente no qual você deseja incluir a qualificação.

1.  Clique em  ** Incluir Skill de Procura **.

1.  Clique em  ** Incluir habilidade existente **.

    Clique na qualificação que você deseja incluir entre as qualificações disponíveis exibidas.

Depois de incluir uma qualificação de procura em um assistente, ela é ativada automaticamente para ele da seguinte maneira:

- Se o assistente tiver somente uma qualificação de procura, qualquer entrada do usuário enviada a um dos canais de integração do assistente a acionará.

- Se o assistente tiver uma qualificação de diálogo e uma qualificação de procura, qualquer entrada do usuário acionará primeiro a qualificação de diálogo. O diálogo lidará com entradas do usuário que tenham uma pontuação alta de confiança com relação ao fornecimento de uma resposta correta. Quaisquer consultas que normalmente acionariam o nó `anything_else` na árvore de diálogo são enviadas para a qualificação de procura.

  É possível evitar que a procura seja acionada por meio do nó `anything_else` seguindo as etapas em [Desativando a procura](#search-skill-add-disable).
  {: note}

- Se desejar que uma consulta de procura específica seja acionada para perguntas específicas, inclua um tipo de resposta de qualificação de procura no nó de diálogo apropriado. Consulte  [ Respostas ](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia)  para obter mais detalhes.

## Acionadores de procura
{: #skill-search-add-trigger}

A qualificação de procura é acionada das seguintes maneiras:

- **Nó Anything else**: procura uma origem de dados externa para obter uma resposta relevante quando nenhum dos nós de diálogo pode direcionar a consulta do usuário.

  Em vez de mostrar uma mensagem padrão, tal como `I don't know how to help you with that.`, o assistente pode dizer `Maybe this information can help:` seguido pela passagem retornada pela procura. Se uma qualificação de procura estiver vinculada ao seu assistente, sempre que o nó `anything_else` for acionado, em vez da resposta do nó ser exibida, ocorrerá uma procura. O assistente passa a entrada do usuário como a consulta para sua qualificação de procura e retorna os resultados da procura como a resposta.

  É possível evitar que a procura seja acionada por meio do nó `anything_else` seguindo as etapas em [Desativando a procura](#search-skill-add-disable).
  {: note}

- **Tipo de resposta da procura**: ao incluir um tipo de resposta de procura em um nó de diálogo, seu assistente recupera uma passagem de uma origem de dados externa e a retorna como resposta para uma pergunta específica. Esse tipo de procura ocorre somente quando o nó de diálogo individual é processado.

  Esta abordagem será útil se desejar limitar uma consulta de usuário antes de acionar uma procura. Por exemplo, a ramificação de diálogo pode coletar informações sobre o tipo de dispositivo que o cliente deseja comprar. Quando você conhece a fabricação e o modelo, é possível enviar uma palavra-chave de modelo na consulta que é enviada para a qualificação da procura e obter melhores resultados.
- **Somente qualificação de procura**: se somente uma qualificação de procura for vinculada a um assistente, sem a vinculação de nenhuma qualificação de diálogo, uma consulta de procura será enviada ao serviço {{site.data.keyword.discoveryshort}} quando qualquer entrada do usuário for recebida de um dos canais de integração do assistente.

## Testar a qualificação de procura
{: #search-skill-add-test}

Depois de configurar a procura, é possível enviar consultas de teste para ver os resultados retornados para ela pelo {{site.data.keyword.discoveryshort}} por meio da área de janela "Experimentar" da qualificação de procura.

Para testar a experiência completa que os clientes terão ao fazer perguntas que serão respondidas pelo diálogo ou que acionarão uma procura, use uma integração de canais, como o link de visualização.

Não é possível testar toda a experiência de ponta a ponta do usuário na área de janela "Experimentar" do diálogo. A qualificação de procura é configurada separadamente e conectada a um assistente. A qualificação de diálogo não sabe os detalhes da procura e, portanto, não pode exibir os resultados da procura na área de janela "Experimentar".
{: important}

Configure pelo menos um canal de integração para testar a qualificação de procura. No canal, insira as consultas que acionam a procura. Ao iniciar qualquer tipo de procura em seu diálogo, teste-o para garantir que a procura seja acionada conforme o esperado. Se não estiver usando os tipos de resposta de procura, teste se uma procura será acionada somente quando nenhum nó de diálogo existente puder lidar com a entrada do usuário. E sempre que uma procura for acionada, assegure-se de que ela retorne resultados significativos.

## Enviando mais solicitações à qualificação de procura
{: #search-skill-add-increase-flow}

Se deseja que a qualificação de diálogo responda com menos frequência e envie mais consultas para a qualificação de procura, configure o diálogo para isso.

Deve-se incluir uma qualificação de diálogo e uma qualificação de procura no assistente para que essa abordagem funcione.

Siga este procedimento para diminuir a probabilidade de o diálogo responder reconfigurando o limite do nível de confiança da configuração padrão de 0,2 para 0,5. Mudar o limite do nível de confiança para 0,5 instrui o seu assistente a não responder com uma resposta do diálogo, a menos que tenha mais de 50% de certeza de que ele seja capaz entender a intenção do usuário e resolvê-la.

1.  Na página *Diálogo* de sua qualificação de diálogo, certifique-se de que o último nó na árvore de diálogo tenha uma condição `anything_else`.

    Sempre que esse nó é processado, a qualificação de procura é acionada.

1.  Inclua uma pasta no diálogo. Posicione a pasta acima do primeiro nó de diálogo cuja ênfase deseja remover. Inclua a condição a seguir na pasta:

    `intents[0].confidence > 0.5`

    Essa condição é aplicada a todos os nós da pasta. Ela informa ao seu assistente que ele deverá processar os nós na pasta somente se tiver pelo menos 50% de certeza de que sabe a intenção do usuário.

1.  Mova para a pasta todos os nós de diálogo que não deseja que seu assistente processe com frequência.

Depois de mudar o diálogo, teste o assistente para garantir que a qualificação de procura seja acionada com a frequência desejada.

Uma abordagem alternativa é ensinar o diálogo sobre os tópicos a serem ignorados. Para isso, é possível incluir elocuções que você deseja que o assistente envie para a qualificação de procura imediatamente como elocuções de teste na área de janela "Experimentar" da qualificação de diálogo. Em seguida, é possível selecionar a opção **Marcar como irrelevante** na área de janela "Experimentar" para ensinar o diálogo a não responder a essa elocução ou a outras semelhantes. Para obter mais informações, consulte [Ensinando seu assistente sobre tópicos a serem ignorados](/docs/services/assistant?topic=assistant-logs#logs-mark-irrelevant).

## Desativando a procura
{: #search-skill-add-disable}

É possível desativar o acionamento da qualificação de procura.

É possível que você queira que isso seja temporário enquanto estiver configurando a integração. Também é possível que você queira sempre acionar uma procura para consultas específicas do usuário que possam ser identificadas no diálogo e usar um tipo de resposta de qualificação de procura para responder.

Para evitar que a qualificação de procura seja acionada, conclua as etapas a seguir:

1.  Na página **Assistentes**, clique no menu para seu assistente e, em seguida, escolha **Configurações**.
1.  Abra a página *Qualificação de procura* e, em seguida, clique para alternar a tecla de alternância para **Desativado**.
