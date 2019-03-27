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

# Obter ajuda do Watson
{: #intent-recommendations}

Se você tiver transcrições de log de bate-papo, de um centro de suporte ao cliente, por exemplo, que contenham elocuções do usuário do mundo real, permita que o serviço extraia seus dados existentes para candidatos de exemplo do usuário de intenção.

## Fazer upload de seus arquivos
{: #intent-recommendations-log-files-add}

Antes de iniciar, crie um arquivo para fornecer ao serviço. O arquivo deve ser um arquivo de valor separado por vírgula (CSV) com uma elocução de usuário por linha. Teoricamente, as elocuções são frases curtas extraídas de transcrições da central de atendimento que contêm perguntas e solicitações de clientes do mundo real. Cada arquivo de elocução do usuário pode ter um tamanho máximo de 20 MB.

Siga estas diretrizes adicionais:

  - Remova quaisquer dados sensíveis das elocuções incluídas no arquivo.

    Dados sensíveis incluem quaisquer informações relacionadas a uma pessoa natural identificável, como nomes, endereços de e-mail e IDs de clientes, além de dados regulados, como informação protegida de saúde.
  - Não inclua elocuções do usuário que excedam 1.024 caracteres de comprimento. As utterâncias mais longas são truncadas.
  - O arquivo deve conter pelo menos 100 elocuções; um arquivo com 500 ou mais elocuções fornecerá melhores resultados.
  - Se uma elocução contiver uma vírgula, coloque-a entre aspas.
  - O CSV deve incluir somente uma coluna.
  - Remova tudo que não seja uma elocução gerada pelo usuário, incluindo notas ou respostas de agentes humanos.

  Por exemplo:

  ```
  O que acontece com a minha cobertura se eu trocar meu carro?
  Eu gostaria de comprar uma casa.
  Como incluo um dependente em meu plano?
  "primeiro, eu quero saber se já estou registrado."
  ```

Todos os arquivos dos quais você faz upload são compartilhados em todas as qualificações na instância de serviço atual. As elocuções de todos os arquivos disponíveis são extraídas quando você solicita recomendações de exemplo do usuário de intenção.

## Obter recomendações de exemplo de usuário de
{: #intent-recommendations-get-example-recommendations}

O vídeo a seguir fornece uma visão geral de 2 minutos sobre como obter recomendações de exemplo do usuário de intenção.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Recomendações de exemplo do usuário de intenção" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/L3FI8KeZfsc?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Os arquivos transferidos por upload não precisam associar os exemplos a intenções. Basta fornecer as elocuções cruas do usuário e deixar o serviço fazer o trabalho de escolher aquelas apropriadas para a intenção atual. Para cada intenção, o serviço analisa os mesmos dados para localizar exemplos de usuário apropriados para essa intenção.

1.  Siga as etapas em [Criando intenções](/docs/services/assistant?topic=assistant-intents#intents-creating-intents-task) para criar uma intenção.

1.  Inclua pelo menos 5 exemplos de usuário que ilustrem o intervalo completo de elocuções típicas que você espera que os usuários digam para acionar essa intenção.

    Esses exemplos de usuário iniciais ensinam o serviço sobre os tipos de elocuções a serem procuradas nos arquivos dos quais você faz upload.

1.  Clique em **Mostrar recomendações**.

    Os arquivos de origem de exemplo são compartilhados entre as qualificações em uma instância de serviço. Se os seus colegas de trabalho tiverem qualificações na mesma instância e fizerem upload de arquivos, seus arquivos serão incluídos em sua coleção compartilhada de *Arquivos de origem de exemplos do usuário*.

1.  **Apenas primeira vez**: clique em **Fazer upload de arquivos** e, em seguida, clique em **Escolher um arquivo** para procurar o arquivo CSV criado anteriormente e selecioná-lo.

    Tudo bem se o arquivo transferido por upload contiver elocuções para todos os tipos de intenções. O serviço sabe em que intenção você está trabalhando e localiza exemplos adequados para recomendar para essa intenção específica.

    Depois que o arquivo é transferido por upload e processado pelo serviço, as elocuções recomendadas são exibidas. Se nenhuma recomendação for feita, o arquivo não conterá exemplos adequados para essa intenção.

1.  Se o arquivo não puder fornecer recomendações úteis para nenhuma de suas intenções, será possível tentar um conjunto diferente de elocuções fazendo upload de outro arquivo. Consulte [Gerenciando arquivos transferidos por upload](#intent-recommendations-manage-files) para obter mais detalhes.

1.  Depois que o serviço mostrar recomendações, selecione as elocuções que você deseja incluir como exemplos de usuário para essa intenção e, em seguida, clique em **Incluir**. Ou cliquem em **Próximo conjunto** para revisar mais elocuções.
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

1.  É possível executar as ações a seguir:

    - Para incluir um arquivo, clique em **Incluir arquivos** e, em seguida, procure um arquivo e selecione-o.
    - Para excluir um arquivo, deve-se remover todos os arquivos que foram transferidos por upload de qualquer qualificação que esteja associada a essa instância de serviço; não é possível excluir apenas um arquivo. Primeiramente, certifique-se de que ninguém mais esteja usando os arquivos e, em seguida, clique em **Excluir todos** para excluir todos os arquivos transferidos por upload.

1.  Feche a página *Arquivos de origem de exemplo do usuário*.
