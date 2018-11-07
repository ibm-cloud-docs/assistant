---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Definindo entidades

***Entidades*** representam uma classe de objeto ou um tipo de dados que é relevante para o propósito de um usuário. Ao reconhecer as entidades mencionadas na entrada do usuário, o serviço {{site.data.keyword.conversationshort}} pode escolher as ações específicas a serem tomadas para cumprir uma intenção.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/kAZ9m-oCKxM" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Limites de entidade
{: #entity-limits}

O número de entidades, valores de entidade e sinônimos que você pode criar depende de seu plano de serviço {{site.data.keyword.conversationshort}}:

| Plano de Serviço      | Entidades por área de trabalho | Valores de entidade por área de trabalho | Sinônimos da entidade por área de trabalho |
|-------------------|-----------------------:|----------------------------:|--------------------------------:|
| Padrão/Premium |                          1000 |                            100.000 |                              100.000 |
| Lite              |                            25 |                            100.000 |                              100.000 |

Entidades do sistema que você ativa para uso contam para o total de uso do seu plano.

## Criando entidades
{: #creating-entities}

Use a ferramenta {{site.data.keyword.conversationshort}} para criar entidades.

1.  Na ferramenta do {{site.data.keyword.conversationshort}}, abra a sua área de trabalho e, em seguida, clique na guia **Entidades**. Se **Entidades** não estiver visível, use o menu ![Menu](images/Menu_16.png) para abrir a página.

1.  Clique em **Incluir entidade**.

    Também é possível clicar em **Usar Entidades do Sistema** para selecionar desde uma lista de entidades comuns, fornecidas pelo {{site.data.keyword.IBM_notm}}, que podem ser aplicadas a qualquer caso de uso. Consulte [Ativando entidades do sistema](#enable_system_entities) para obter detalhes adicionais.

1.  No campo **Nome da entidade**, digite um nome descritivo para a entidade.

    O nome da entidade pode conter letras (em Unicode), números, sublinhados e hifens. Por exemplo:
    - `@location`
    - `@menu_item`
    - `@product`

    Não inclua o caractere `@` nos nomes de entidade ao criá-los na ferramenta {{site.data.keyword.conversationshort}}. Os nomes das entidades não podem conter espaços ou ter mais de 64 caracteres. E nomes de entidade não podem começar com a sequência `sys-`, que é reservada para entidades do sistema.
    {: tip}

1.  Selecione **Criar entidade**.

    ![Captura de tela de criação de uma entidade](images/create_entity.png)

1.  No campo **Nome do valor**, digite o texto de um valor possível para a entidade e pressione a tecla `Enter`. Um valor de entidade pode ser qualquer sequência de até 64 caracteres de comprimento.

    > **Importante:** Não inclua informações sensíveis ou pessoais em nomes de entidades ou valores. Os nomes e valores podem ser expostos em URLs em um aplicativo.

1.  Para **Correspondência difusa**, clique no botão para selecionar ligada ou desligada; a correspondência difusa está desligada por padrão. Esse recurso está disponível para os idiomas observados no tópico [Idiomas suportados](lang-support.html).
 {: #fuzzy-matching}

    É possível ligar a correspondência difusa para melhorar a capacidade do serviço para reconhecer os termos de entrada do usuário com sintaxe que é semelhante à entidade, mas sem requerer uma correspondência exata. Há três componentes para correspondência difusa: stemming, erro de ortografia e correspondência parcial:
    - *Stemming* - o recurso reconhece o formulário raiz de valores de entidade que possuem várias formas gramaticais. Por exemplo, a raiz das 'bananas' seria 'banana', enquanto a raiz de 'execução' seria 'executar'.
    - *Erro de ortografia* - o recurso é capaz de mapear a entrada do usuário para a entidade correspondente apropriada apesar da presença de erros ortográficos ou pequenas diferenças de sintaxe. Por exemplo, se você define *girafa* como um sinônimo para uma entidade animal e a entrada do usuário contém os termos *girafas* ou *girafa*, a correspondência difusa é capaz de mapear o termo para a entidade animal corretamente.
    - *Correspondência parcial* - com correspondência parcial, o recurso sugere automaticamente sinônimos baseados em subsequência presentes nas entidades definidas pelo usuário e designa uma pontuação de confiança inferior em comparação com a correspondência de entidade exata.

    **Nota** - para inglês, a correspondência difusa impede a captura de algumas palavras comuns válidas do inglês como correspondências difusas para uma determinada entidade. Esse recurso usa palavras do dicionário em inglês padrão. Também é possível definir um valor/sinônimo de entidade em inglês e a correspondência difusa corresponderá somente a seu valor/sinônimo de entidade definido. Por exemplo, a correspondência difusa pode corresponder o termo `unsure` com `insurance`; mas se você tiver `unsure` definido como um valor/sinônimo para uma entidade como `@option`, `unsure` sempre será correspondido com `@option` e não com `insurance`.

1.  Depois de inserir um nome do valor, é possível incluir quaisquer sinônimos ou definir padrões específicos para esse valor de entidade selecionando `Synonyms` ou `Patterns` no menu suspenso *Tipo*.

    ![Seletor de tipo para valor](images/value_type.png)

    > **Nota:** É possível incluir *qualquer* sinônimo ou padrão para um valor de entidade única, você não pode incluir ambos.

    - No campo **Sinônimos**, digite quaisquer sinônimos para o valor da entidade. Um sinônimo pode ser qualquer sequência de até 64 caracteres de comprimento.

      ![Captura de tela de definição de uma entidade](images/define_entity.png)

    - O campo **Padrões** permite definir padrões específicos para um valor de entidade. Um padrão **deve** ser inserido como uma expressão regular no campo.

      ![Captura de tela de definição de uma entidade padrão](images/patternents1.png)
      {: #pattern-entities}

      Como neste exemplo, para a entidade *ContactInfo*, os padrões para os valores de telefone, e-mail e website podem ser definidos como a seguir:
      - Telefone
        - `localPhone`: `(\d{3})-(\d{4})`, por exemplo, 426-4968
        - `fullUSphone`: `(\d{3})-(\d{3})-(\d{4})`, por exemplo, 800-426-4968
        - `internationalPhone`: `^(\(?\+?[0-9]*\)?)?[0-9_\- \(\)]*$`, por exemplo, +44 1962 815000
      - `email`: `\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}\b`, por exemplo, nome@ibm.com
      - `website`: `(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$`, por exemplo, https://www.ibm.com

      Geralmente ao usar as entidades padrão, será necessário armazenar o texto que corresponde ao padrão em uma variável de contexto (ou variável de ação) desde sua árvore de diálogos.

      Imagine um caso em que você está perguntando a um usuário por seu endereço de e-mail. A condição do nó de diálogo conterá uma condição semelhante a `@contactInfo:email`. Para designar o e-mail inserido pelo usuário como uma variável de contexto, a sintaxe a seguir pode ser usada para capturar a correspondência padrão dentro da seção de resposta do nó de diálogo:

      ```json
      {
          "context" : {
              "e-mail": "<? @contactInfo.literal ?>"
          }
      }
      ```
      {: screen}
      {: #capture-group}

      *Grupos de captura* - para expressões regulares, qualquer parte de um padrão dentro de um par de parênteses normais será capturada como um grupo. Por exemplo, o valor de entidade `fullUSphone` contém três grupos capturados:

        - `(\d{3})` - código de área dos EUA
        - `(\d{3})` - prefixo
        - `(\d{4})` - número da linha

      O agrupamento pode ser útil se, por exemplo, você deseja que o serviço {{site.data.keyword.conversationshort}} pergunte aos usuários seus números de telefone e, então, use somente o código de área do número fornecido em uma resposta.

      Para designar o código de área inserido pelo usuário como uma variável de contexto, a sintaxe a seguir pode ser usada para capturar a correspondência do grupo dentro da seção de resposta do nó de diálogo:

        ```json
        {
            "context" : {
                "area_code": "<? @fullUSphone.groups[1] ?>"
            }
        }
        ```
       {: screen}

      Para obter informações adicionais sobre o uso de grupos de captura no tempo de execução do diálogo, veja [Armazenando os valores de entidade padrão em variáveis de contexto](dialog-overview-context-groups.html).

      O mecanismo de correspondência padrão utilizado pelo serviço {{site.data.keyword.conversationshort}} tem algumas limitações de sintaxe, que são necessárias para evitar problemas de desempenho que podem ocorrer ao usar outros mecanismos de expressão regular.
        - Os padrões de entidade não podem conter:
          - Repetições positivas (por exemplo `x*+`)
          - Referências anteriores (por exemplo `\g1`)
          - Ramificações condicionais (por exemplo `(?(cond)true)`)
        - Quando uma entidade padrão inicia ou termina com um caractere Unicode e inclui limites de palavras, por exemplo `\bš\b`, a correspondência de padrões não corresponde ao limite de palavra corretamente. Neste exemplo, para a entrada `š zkouška`, a correspondência retorna `Group 0: 6-7 š` (`š zkou`_**`š`**_`ka`), em vez do `Group 0: 0-1 š` correto (_**`š`**_ `zkouška`).

      O mecanismo de expressão regular baseia-se vagamente no mecanismo de expressão regular Java. O serviço {{site.data.keyword.conversationshort}} produzirá um erro se você tentar fazer upload de um padrão não suportado, por meio da API ou de dentro da Tooling UI do serviço {{site.data.keyword.conversationshort}}.

1.  Clique em **Incluir valor** e repita o processo para incluir mais valores de entidade.

1.  Quando tiver concluído a inclusão de valores de entidade, selecione ![Seta de fechamento](images/close_arrow.png) para concluir a criação da entidade.

### Resultados

A entidade criada é incluída na guia **Entidades** e o sistema começa a treinar com os novos dados.

## Editando entidades

É possível clicar em qualquer entidade na lista para abri-la para edição. É possível renomear ou excluir entidades e é possível incluir, editar ou excluir valores, sinônimos ou padrões.

> **Nota**: se você muda o tipo de entidade de `synonym` para `pattern` ou vice-versa, os valores existentes são convertidos, mas podem não ser úteis no estado em que se encontram.

## Procurando entidades

Use o recurso Procurar para localizar nomes de entidade, valores e sinônimos.

1.  Selecione a guia **Entidades** na barra de navegação, em seguida, *Minhas entidades*.

    ![Visão geral da guia Entidade](images/entity_oview.png)

    **Nota**: as entidades do sistema não são pesquisáveis.

1.  Selecione o ícone Procurar: ![Ícone Procurar](images/search_icon.png)

1.  Inserir um termo de procura ou frase.

    ![Termo de procura de entidade](images/searchent_1.png)

    **Nota**: na primeira vez que você procurar, um índice será criado; você poderá ver uma mensagem para esperar enquanto os seus conteúdos estiverem sendo indexados.

### Resultados

As entidades que contêm seu termo de procura, com exemplos correspondentes, são mostradas. Selecione qualquer resultado para abri-lo para edição.

  ![Retorno de procura de entidade](images/searchent_2.png)

## Importando entidades

Se ver um grande número de entidades, você poderá achar mais fácil importá-los de um arquivo CSV (Comma-Separated Value) do que defini-los um por um na ferramenta {{site.data.keyword.conversationshort}}.

1.  Colete as entidades em um arquivo CSV ou exporte-as de uma planilha para um arquivo CSV. O formato obrigatório para cada linha no arquivo é o seguinte:

    ```
    <entity>,<value>,<synonyms>
    ```
    {: screen}

    Em que &lt;entity&gt; é o nome de uma entidade, &lt;value&gt; é um valor para a entidade e &lt;synonyms&gt; é uma lista separada por vírgula de sinônimos para esse valor.

    ```
    weekday,Monday,Mon
    weekday,Tuesday,Tue,Tues
    weekday,Wednesday,Wed
    weekday,Thursday,Thur,Thu,Thurs
    weekday,Friday,Fri
    weekday,Saturday,Sat
    weekday,Sunday,Sun
    month,January,Jan
    month,February,Feb
    month,March,Mar
    month,April,Apr
    month,May
    ```
    {: screen}

    A importação de um arquivo CSV também suporta padrões. Qualquer sequência agrupada com `/` será considerada um padrão (em oposição a um sinônimo).

    ```
    ContactInfo,localPhone,/(\d{3})-(\d{4})/
    ContactInfo,fullUSphone,/(\d{3})-(\d{3})-(\d{4})/
    ContactInfo,internationalPhone,/^(\(?\+?[0-9]*\)?)?[0-9_\- \(\)]*$/
    ContactInfo,email,/\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}\b/
    ContactInfo,website,/(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/
    ```
    {: screen}

    Salve o arquivo CSV com codificação UTF-8 e nenhuma marca de ordem de byte (BOM). O tamanho máximo do arquivo CSV é 10MB. Se seu arquivo CSV for maior, considere dividir isso em vários arquivos e importá-los separadamente.  Na ferramenta {{site.data.keyword.conversationshort}}, abra a sua área de trabalho e, em seguida, clique na guia **Entidades**.
    {: tip}

1.  Clique em ![Importar](images/importGA.png) e, em seguida, arraste um arquivo ou navegue para selecionar um arquivo de seu computador. O arquivo é validado e importado e o sistema começa a treinar com os novos dados.

### Resultados

É possível visualizar as entidades importadas na guia Entidades. Você pode precisar atualizar a página para ver as novas entidades.

## Exportando entidades
{: #export_entities}

É possível exportar várias entidades para um arquivo CSV, depois é possível importar e reutilizá-las para outro aplicativo {{site.data.keyword.conversationshort}}.

A exportação de um arquivo CSV suporta padrões. Qualquer sequência agrupada com `/` será considerada um padrão (em oposição a um sinônimo).
{: tip}

1.  Selecione as entidades desejadas e, em seguida, selecione **Exportar**.

    ![Botão Exportar entidade](images/ExportEntity.png)

## Excluindo entidades
{: #delete_entities}

É possível selecionar um número de entidades para exclusão.

**IMPORTANTE**: Ao excluir entidades, você também estará excluindo todos os valores, sinônimos ou padrões associados e esses itens não podem ser recuperados posteriormente. Todos os nós de diálogo que referenciam essas entidades ou valores devem ser atualizados manualmente para não fazer referência ao conteúdo excluído.

1.  Selecione as entidades desejadas e, em seguida, selecione **Excluir**.

    ![Botão Excluir entidade](images/DeleteEntity.png)

## Ativando entidades do sistema
{: #enable_system_entities}

O serviço {{site.data.keyword.conversationshort}} fornece um número de *entidades de sistema*, que são entidades comuns que você pode usar para qualquer aplicativo. A ativação de uma entidade do sistema torna possível preencher rapidamente sua área de trabalho com dados de treinamento que são comuns a muitos casos de uso.

Entidades do Sistema podem ser usadas para reconhecer uma ampla variedade de valores para os tipos de objetos que eles representam. Por exemplo, a entidade do sistema `@sys-number` corresponde a qualquer valor numérico, incluindo números inteiros, frações decimais ou mesmo números gravados como palavras.

As entidades do sistema são mantidas centralmente, portanto, quaisquer atualizações ficarão disponíveis automaticamente. Não é possível modificar entidades do sistema.

1.  Na guia Entidades, clique em **Entidades do sistema**.

    ![Captura de tela da guia "Entidades do sistema" ](images/system_entities_1.png)

1.  Navegue pela lista de entidades do sistema para escolher as que são úteis para seu aplicativo.
    - Para consultar informações adicionais sobre uma entidade do sistema, incluindo exemplos de entrada correspondente, clique na entidade na lista.
    - Para obter detalhes sobre as entidades do sistema disponíveis, consulte [Entidades do Sistema](system-entities.html).

1.  Clique no interruptor de duas posições próximo a uma entidade do sistema para ativá-la ou desativá-la.

### Resultados

Depois de ativar entidades do sistema, o serviço {{site.data.keyword.conversationshort}} começa a treinar novamente. Após o treinamento ser concluído, você poderá usar as entidades.
