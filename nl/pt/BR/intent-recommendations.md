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

# Obter ajuda para definir intenções ![Somente nos planos Plus ou Premium](images/plus.png)
{: #intent-recommendations}

Se você tiver dados existentes de transcrição do bate-papo do suporte ao cliente corporativo, permita que o Watson analise os dados para entender as necessidades do cliente em que sua equipe de suporte gasta a maior parte do tempo direcionando. Em seguida, o Watson poderá recomendar intenções e exemplos de usuários que poderão ser usados para treinar seu assistente para que ele possa reconhecer as mesmas solicitações e solicitações semelhantes no futuro.
{: shortdesc}

Esse recurso está disponível para usuários dos planos Plus ou Premium.
{: note}

As necessidades do cliente são representadas no {{site.data.keyword.conversationshort}} como *intenções*. Se você ainda não tiver definido as intenções, será possível iniciar mais rápido solicitando ajuda ao Watson. Faça upload de arquivos com elocuções do cliente de transcrições da central de atendimento para que o serviço {{site.data.keyword.conversationshort}} os analise. Com base nos insights descobertos, o Watson recomenda um conjunto básico de intenções que devem ser construídas para cobrir as necessidades mais comuns de seus clientes.

Como os assuntos que seus clientes desejam discutir mudam, é possível usar o recurso de recomendações de exemplo de usuário de intenção para ajudar a manter suas intenções atualizadas e relevantes ao longo do tempo.

O vídeo a seguir fornece uma visão geral de 3,5 minutos de recomendações de intenções e um exemplo de usuário de intenção.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Visão geral de recomendações de intenção" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/64h59KqDY98?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Para saber mais sobre como as recomendações de intenção podem ajudá-lo a construir um robô útil mais rápido, [leia esta postagem do blog ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://medium.com/ibm-watson/let-watson-train-your-virtual-assistant-57bd53b12bc3){: new_window}.

Explore seus dados existentes para executar uma das ações a seguir:

- [ Obter recomendações de intenção ](#intent-recommendations-get-intent-recommendations)
- [ Obter recomendações de exemplo do usuário de intenção ](#intent-recommendations-get-example-recommendations)

Consulte [Idiomas suportados](/docs/services/assistant?topic=assistant-language-support) para obter informações sobre o suporte ao idioma desse recurso.

## Criando um arquivo de origem de exemplo de usuário
{: #intent-recommendations-log-files-add}

Para que o Watson possa analisar seus dados, eles devem ser fornecidos no formato correto. Crie um arquivo de valor separado por vírgula (CSV) com uma elocução do cliente por linha. Teoricamente, as elocuções são frases curtas extraídas de transcrições da central de atendimento que contêm perguntas e solicitações de clientes do mundo real. Cada arquivo de origem de exemplo de usuário pode ter um tamanho máximo de 20 MB.

Siga estas diretrizes adicionais:

  - Remova quaisquer dados sensíveis das elocuções incluídas no arquivo.

    Dados sensíveis incluem quaisquer informações relacionadas a uma pessoa natural identificável, como nomes, endereços de e-mail e IDs de clientes, além de dados regulados, como informação protegida de saúde.
  - Não inclua elocuções que excedam 1024 caracteres de comprimento. As utterâncias mais longas são truncadas.
  - O arquivo deve conter pelo menos 100 elocuções; um arquivo com 500 ou mais elocuções fornecerá melhores resultados.
  - Se uma elocução contiver uma vírgula, coloque-a entre aspas.
  - O CSV deve incluir somente uma coluna.
  - Remova tudo que não for uma elocução gerada pelo cliente, incluindo quaisquer respostas ou observações do agente humano.

  Por exemplo:

  ```
  O que acontece com a minha cobertura se eu trocar meu carro?
  Eu gostaria de comprar uma casa.
  Como incluo um dependente em meu plano?
  "primeiro, eu quero saber se já estou registrado."
  ```

Todos os arquivos dos quais você faz upload são compartilhados em todas as qualificações na instância de serviço atual. As elocuções de todos os arquivos disponíveis são extraídas quando você pede recomendações de intenção e recomendações de exemplo do usuário de intenção.

## Obter recomendações de intenção do Watson
{: #intent-recommendations-get-intent-recommendations}

Permita que o Watson analise as transcrições de bate-papo de sua central de atendimento e recomende algumas intenções iniciais para você começar. Se você já criou algumas intenções, permita que o Watson analise seus logs e compare as descobertas feitas com suas intenções existentes para identificar lacunas em seus dados de treinamento e sugerir novas intenções que possam preenchê-las.

Para usar esse recurso, faça upload de um arquivo com elocuções do cliente. O Watson avaliará essas elocuções e identificará áreas de problemas comuns que os clientes mencionam com frequência. Em seguida, o {{site.data.keyword.conversationshort}} exibirá um conjunto de intenções discretos que capturam as necessidades mais frequentes do cliente. É possível revisar cada intenção recomendada e seus exemplos do usuário correspondentes para escolher os que você deseja incluir em seus dados de treinamento.

## Obtendo recomendações de intenção
{: #intent-recommendations-get-intent-recommendations-task}

Antes de iniciar, crie um arquivo CSV com seus dados. Consulte [Criando um arquivo de origem de exemplo de usuário](#intent-recommendations-log-files-add).

Para obter recomendações de intenção, conclua as etapas a seguir:

1.  Abra sua qualificação de diálogo. A qualificação é aberta para a página **Intenções**.

1.  Clique em  ** Obter recomendações **.

1.  **Somente primeira vez**: clique em **Incluir arquivo** e, em seguida, clique em **Escolher um arquivo** para procurar o arquivo CSV que você criou anteriormente e selecione-o.

    Aguarde até que o Watson tenha analisado seus dados e agrupado as elocuções.

1.  Revise as intenções recomendadas pelo Watson.

    As recomendações de intenção são ordenadas por:

    1.  Intenções com as elocuções que ocorrem com mais frequência. A frequência das elocuções associadas a essas intenções sugere que elas identificam os pontos de dificuldade mais comuns do cliente.
    1.  A diferença dessas intenções para aquelas já incluídas em sua qualificação. Sua exclusividade sugere que elas podem atender às necessidades do cliente que não estão sendo atendidas atualmente.
    1.  O grau de semelhança entre os exemplos de usuário na intenção, o que indica a intensidade da intenção.

    É possível passar o mouse sobre uma intenção para ver alguns exemplos de suas elocuções associadas. Se você desejar ver uma lista de todos os agrupamentos de intenção, clique em **Mostrar todas as recomendações**.

1.  Clique em uma intenção para ver o conjunto integral de exemplos do usuário que estão associados a ela e para tomar uma das ações a seguir:

    Cancele a seleção de quaisquer elocuções que você não deseja incluir como exemplos do usuário.

    - Para incluir a intenção recomendada com as elocuções selecionadas como exemplos do usuário, clique em **Criar intenção**.
    - Para incluir as elocuções selecionadas da intenção recomendada para uma de suas intenções existentes como exemplos do usuário, clique em **Incluir em intenção existente**, escolha a intenção e, em seguida, clique em **Incluir**.

As intenções e os exemplos do usuário de intenção que você inclui dessa maneira contam para seus totais de intenção e de exemplo do usuário de intenção para os quais há limites por plano. Consulte [Limites de intenção](/docs/services/assistant?topic=assistant-intents#intents-limits) para obter mais detalhes.

Como os assuntos que seus clientes desejam discutir mudam, é possível usar o recurso de recomendações de exemplo do usuário de intenção para ajudar a manter suas intenções atualizadas e relevantes ao longo do tempo.

## Obter recomendações de exemplo de usuário de
{: #intent-recommendations-get-example-recommendations}

Os dados fornecidos ao Watson para a localização de exemplos de usuário de intenção não precisam associar os exemplos a intenções. Basta fornecer as elocuções brutas do cliente e permitir que o Watson faça o trabalho de escolher aquelas que são apropriadas para a intenção atual. Para cada intenção, o Watson analisa os mesmos dados para localizar exemplos de usuário apropriados para essa intenção.

Antes de iniciar, crie um arquivo CSV com seus dados. Consulte [Criando um arquivo de origem de exemplo de usuário](#intent-recommendations-log-files-add).

1.  Siga as etapas em [Criando intenções](/docs/services/assistant?topic=assistant-intents#intents-creating-intents-task) para criar uma intenção.

1.  Inclua pelo menos 5 exemplos de usuário que ilustrem a gama completa de elocuções típicas que você antecipa que os clientes possam dizer para acionar essa intenção.

    Esses exemplos de usuário de valor inicial ensinam o Watson sobre os tipos de elocuções a serem procuradas nos arquivos transferidos por upload.

1.  Clique em **Mostrar recomendações**.

    Os arquivos de origem de exemplo são compartilhados entre as qualificações em uma instância de serviço. Se os seus colegas de trabalho tiverem qualificações na mesma instância e fizerem upload de arquivos, seus arquivos serão incluídos em sua coleção compartilhada de *Arquivos de origem de exemplos do usuário*.

1.  **Apenas primeira vez**: clique em **Fazer upload de arquivos** e, em seguida, clique em **Escolher um arquivo** para procurar o arquivo CSV criado anteriormente e selecioná-lo.

    Tudo bem se o arquivo transferido por upload contiver elocuções para todos os tipos de intenções. O Watson sabe em qual intenção você está trabalhando e localiza exemplos adequados a serem recomendados para essa intenção específica.

    Depois que o arquivo for transferido por upload e processado pelo Watson, as elocuções recomendadas serão exibidas. Se nenhuma recomendação for feita, o arquivo não conterá exemplos adequados para essa intenção.

1.  Se o arquivo não puder fornecer recomendações úteis para nenhuma de suas intenções, será possível tentar um conjunto diferente de elocuções fazendo upload de outro arquivo. Consulte [Gerenciando arquivos transferidos por upload](#intent-recommendations-manage-files) para obter mais detalhes.

1.  Depois que o Watson mostrar recomendações, selecione as elocuções que deseja incluir como exemplos de usuário para essa intenção e, em seguida, clique em **Incluir**. Ou cliquem em **Próximo conjunto** para revisar mais elocuções.
1.  Se quiser você mesmo procurar exemplos de usuário no conteúdo do arquivo CSV, clique na guia **Logs de procura**, insira uma palavra-chave na qual basear a procura e, em seguida, pressione **Enter**.

    Siga estas diretrizes de sintaxe da consulta de procura:

    - Os operadores booleanos (como `AND` e `OR`) são suportados.
    - Inclua texto entre aspas para procurar uma correspondência de texto exata ("thisstringmustbepresent").
    - É possível usar expressões regulares, como `*ly` para localizar todos os termos que terminam com `ly`.
    - Os caracteres a seguir são usados como operadores de expressão regular:

      `+ - = && || > < ! ( ) { } [ ] ^ " ~ * ? : \ /`

      Caso queira incluir alguém em um termo de procura sem que ele seja processado como operador, deve-se prefixá-lo com uma barra invertida (`\`).

Os exemplos de usuário incluídos dessa maneira são considerados para os totais de exemplos de usuário com intenção para os quais há limites por plano. Consulte [Limites de intenção](/docs/services/assistant?topic=assistant-intents#intents-limits) para obter mais detalhes.

## Gerenciando arquivos transferidos por upload
{: #intent-recommendations-manage-files}

Depois de fazer upload de pelo menos um arquivo, é possível abrir a coleção *Arquivos de origem de exemplo do usuário* para gerenciar seus arquivos. Os arquivos transferidos por upload são incluídos em uma coleção de arquivos compartilhada na instância de serviço atual do {{site.data.keyword.conversationshort}}.

1.  Para acessar a coleção, clique para abrir uma intenção, clique em **Mostrar recomendações** e, em seguida, clique em **Visualizar arquivos** na barra lateral.

    Se você não tiver nenhuma intenção incluída ainda, na página principal Intenções, expanda a seção *Recomendações de intenção*, clique em **Obter recomendações**, clique em **Mostrar todas as recomendações** e, em seguida, clique em **Visualizar arquivos**.

1.  É possível executar as ações a seguir:

    - Para incluir um arquivo, clique em **Incluir arquivos** e, em seguida, procure um arquivo e selecione-o.
    - Para excluir um arquivo, deve-se remover todos os arquivos que foram transferidos por upload de qualquer qualificação que esteja associada a essa instância de serviço; não é possível excluir apenas um arquivo. Primeiramente, certifique-se de que ninguém mais esteja usando os arquivos e, em seguida, clique em **Excluir todos** para excluir todos os arquivos transferidos por upload.

1.  Feche a página *Arquivos de origem de exemplo do usuário*.
