---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-06"

keywords: intent, intent conflicts, annotate

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

# Definindo intenções
{: #intents}

***Intenções*** são objetivos ou propósitos expressos em uma entrada do cliente, como a resposta a uma pergunta ou o processamento de um pagamento de fatura. Ao reconhecer a intenção expressa na entrada de um cliente, o serviço {{site.data.keyword.conversationshort}} pode escolher o fluxo de diálogo correto para responder a isso.
{: shortdesc}

<iframe class="embed-responsive-item" id="youtubeplayer" title="Trabalhando com intenções" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/OPdOCUPGMIQ" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Visão Geral da Criação de Intenção
{: #intents-described}

- Planeje as intents para seu aplicativo.

  Considere o que seus clientes podem desejar fazer e o que você deseja que seu aplicativo seja capaz de manipular em seu nome. Por exemplo, você pode desejar que o aplicativo ajude seus clientes a fazer uma compra. Neste caso, é possível incluir uma intenção `#buy_something` (o `#` incluído como um prefixo no nome da intenção ajuda a identificá-la claramente como uma intenção).

- Ensina o Watson sobre suas intents.

  Depois de decidir quais solicitações de negócios o aplicativo deve manipular para os clientes, deve-se ensinar o Watson sobre eles. Para cada objetivo de negócios (como `#buy_something`), deve-se fornecer pelo menos 5 exemplos de elocuções que são usadas normalmente pelos clientes para indicar seus objetivos. Por exemplo, `I want to make a purchase.`
  
  Idealmente, localize exemplos de elocução do usuário do mundo real que podem ser extraídos de processos de negócios existentes. Os exemplos do usuário devem ser customizados para seus negócios específicos. Por exemplo, se você for uma empresa de seguros, um exemplo de usuário poderá ser semelhante a `I want to buy a new XYZ insurance plan.`
  
  Os exemplos fornecidos são utilizados por seu assistente para construir um modelo de aprendizado de máquina que possa reconhecer os mesmos tipos de elocuções e elocuções semelhantes, mapeando-os para a intenção apropriada.

Inicie com algumas intenções e teste-as conforme você expande iterativamente o escopo do aplicativo.

![Somente nos planos Plus ou Premium](images/plus.png) Se já tiver transcrições de bate-papo de uma central de atendimento ou consultas de cliente coletadas de um aplicativo on-line, aproveite esses dados. Compartilhe elocuções reais do cliente com o Watson e permita que ele recomende as melhores intenções e exemplos de usuário de intenção para suas necessidades. Consulte [Obter ajuda ao definir intenções](/docs/services/assistant?topic=assistant-intent-recommendations) para obter mais detalhes.

## Criando intenções
{: #intents-create-task}

1.  Abra sua qualificação de diálogo. A qualificação é aberta para a página **Intenções**.

1.  Selecione **Criar intenção**.

1.  No campo **Nome da intenção**, digite um nome para a intenção.
    - O nome de intenção pode conter letras (em Unicode), números, sublinhados, hifens e pontos.
    - O nome não pode ser composto de `..` ou qualquer outra cadeia de somente pontos.
    - Os nomes das intenções não podem conter espaços e não devem exceder 128 caracteres. A seguir estão exemplos de nomes de intenção:
        - `#weather_conditions`
        - `#pay_bill`
        - `#escalate_to_agent`

    Um sinal de número `#` precede automaticamente o nome da intenção para ajudar a identificar o termo como uma intenção. Não é necessário incluí-lo.
    {: tip}

    Opcionalmente, inclua uma descrição da intenção no campo **Descrição**.

1.  Selecione **Criar intenção** para salvar o nome da intenção.

    ![Captura de tela que mostra uma nova definição de intenção](images/create_intent.png)

1.  Em seguida, no campo **Incluir exemplo de usuário**, digite o texto de um exemplo de usuário para a intenção. Um exemplo pode ser qualquer sequência de até 1024 caracteres de comprimento. As elocuções a seguir podem ser exemplos para a intenção `#pay_bill`:
    - `I need to pay my bill.`
    - `Pay my account balance`
    - `make a payment`

    Para saber sobre o impacto de incluir referências a entidades em seus exemplos do usuário, consulte [Como as referências de entidade são tratadas](#intents-entity-references).
    {: tip}

    Os nomes de intenção e o texto de exemplo podem ser expostos em URLs quando um aplicativo interage com o {{site.data.keyword.conversationshort}}. Não inclua informações sensíveis ou pessoais nestes artefatos.
    {: important}

1.  Clique em **Incluir exemplo** para salvar o exemplo de usuário.

1.  Repita o mesmo processo para incluir mais exemplos.

    Forneça pelo menos cinco exemplos para cada intenção.
    {: important}

    ![Somente nos planos Plus ou Premium](images/plus.png) Para obter ajuda com a criação de exemplos de usuário, consulte [Obter recomendações de exemplo de usuário de intenção](/docs/services/assistant?topic=assistant-intent-recommendations#intent-recommendations-get-example-recommendations).

1.  Ao concluir a inclusão de exemplos, clique em ![Fechar seta](images/close_arrow.png) para concluir a criação da intenção.

O sistema começa a treinar a si mesmo sobre a intenção e os exemplos do usuário que você incluiu.

## Como as referências de entidade são tratadas
{: #intents-entity-references}

Quando você inclui uma menção de entidade em um exemplo do usuário, o modelo de aprendizado de máquina usa as informações de diferentes maneiras nestes cenários:

- [Referenciando valores de entidade e sinônimos em exemplos de intenção](#intents-related-entities)
- [ menções anotadas ](#intents-annotated-mentions)
- [Referenciando diretamente um nome de entidade em um exemplo de intenção](#intents-entity-as-example)

### Referenciando valores de entidade e sinônimos em exemplos de intenção
{: #intents-related-entities}

Se você tiver definido, ou planeja definir, entidades que estão relacionadas a essa intenção, mencione os valores de entidade ou sinônimos em alguns dos exemplos. Fazer isso ajuda a estabelecer um relacionamento entre a intenção e as entidades. É um relacionamento fraco, mas ele informa o modelo.

![Captura de tela que mostra a definição de intenção](images/define_intent.png)

*Importante*:

  - Os dados de exemplo de intenção devem ser representativos e típicos de dados fornecidos por seus usuários. Os exemplos podem ser coletados de dados reais do usuário ou de pessoas que são especialistas em seu campo específico. A natureza representativa e precisa dos dados é importante.
  - Os dados de treinamento e teste (para propósitos de avaliação) devem refletir a distribuição de intenções no uso real. Geralmente, intenções mais frequentes têm relativamente mais exemplos e melhor cobertura de resposta.
  - É possível incluir pontuação no texto de exemplo, contanto que apareça naturalmente. Se acreditar que alguns usuários expressem suas intenções com exemplos que incluam pontuação e outros não, inclua ambas as versões. Geralmente, quando mais cobertura para vários padrões, melhor a resposta.

### Menas anotadas
{: #intents-annotated-mentions}

Ao definir entidades, é possível anotar menções da entidade diretamente de seus exemplos de usuário de intenção existentes. Um relacionamento que você identifica dessa maneira entre a intenção e a entidade *não* é usado pelo modelo de classificação de intenção. No entanto, quando você inclui a menção na entidade, ela também é incluída nessa entidade como um novo valor. E quando você inclui a menção em um valor de entidade existente, ela também é incluída nesse valor de entidade como um novo sinônimo. A classificação de intenção usa esses tipos de referências de dicionário em exemplos do usuário de intenção para estabelecer uma referência fraca entre uma intenção e uma entidade.

Para obter mais informações sobre entidades contextuais, consulte [Incluindo entidades contextuais](/docs/services/assistant?topic=assistant-entities#entities-create-annotation-based).

### Referenciando diretamente um nome de entidade em um exemplo de intenção
{: #intents-entity-as-example}

Esta abordagem é avançada. Ao adotá-la, use-a de forma consistente.
{: note}

É possível escolher referenciar diretamente as entidades em seus exemplos de intenção. Por exemplo, suponha que você tenha uma entidade chamada `@PhoneModelName` com os valores *Galaxy S8*, *Moto Z2*, *LG G6* e *Google Pixel 2*. Ao criar uma intenção, por exemplo, `#order_phone`, será possível fornecer dados de treinamento como a seguir:

- Posso obter um `@PhoneModelName`?
- Ajude-me pedir um `@PhoneModelName`.
- O `@PhoneModelName` está no estoque?
- Inclua um `@PhoneModelName` em minha ordem.

![Screen capture showing intent definition](images/define_intent_entity.png)

Atualmente, é possível referenciar diretamente somente as entidades de sinônimos que você define (os valores padrão são ignorados). Não é possível usar [entidades do sistema](/docs/services/assistant?topic=assistant-system-entities).

Se optar por referenciar uma entidade como um exemplo de intenção (por exemplo, `@PhoneModelName`) em *qualquer lugar* de seus dados de treinamento, ela cancelará o valor do uso de uma referência direta (por exemplo, *Galaxy S8*) em um exemplo de intenção em qualquer outro lugar. Todas as intenções usarão, então, a abordagem de entidade como um exemplo de intenção. Não é possível aplicar essa abordagem somente a uma intenção específica.
{: important}

Na prática, isso significa que, se você tiver treinado anteriormente a maioria de suas intenções com base em referências diretas (*Galaxy S8*) e agora usar referências de entidade (`@PhoneModelName`) para apenas uma intenção, a mudança afetará seu treinamento anterior. Se você escolhe usar referências `@Entity`, deve-se substituir todas as referências diretas anteriores por referências `@Entity`.

Definir uma intenção de exemplo com uma `@Entity` com 10 valores definidos para ela **não** equivale a especificar essa intenção de exemplo 10 vezes. O serviço {{site.data.keyword.conversationshort}} não dá tanto peso a essa sintaxe de intenção de exemplo.

## Testando suas intenções
{: #intents-test}

Depois de ter concluído a criação de novas intenções, é possível testar o sistema para ver se ele reconhece suas intenções como você espera.

1.  Clique no ícone ![Perguntar ao Watson](images/ask_watson.png).

1.  Na área de janela "Experimentar", insira uma pergunta ou outra sequência de texto e pressione Enter para ver qual intenção é reconhecida. Se a intenção incorreta é reconhecida, você pode melhorar seu modelo incluindo esse texto como um exemplo para a intenção correta.

    Se tiver feito mudanças recentemente em sua qualificação, uma mensagem poderá ser exibida indicando que o sistema ainda está realizando o novo treinamento. Se você vir essa mensagem, aguarde até que o treinamento seja concluído antes de testar:
    {: tip}

    ![Captura de tela mostrando mensagem de treinamento em andamento](images/training.png)

    A resposta indica qual intenção foi reconhecida de sua entrada.

    ![Captura de tela de intenções de teste](images/test_intents.png)

1.  Se o sistema não reconhecer a intenção correta, será possível corrigi-la. Para corrigir a intenção reconhecida, selecione a intenção exibida e, em seguida, selecione a intenção correta na lista. Após o envio de sua correção, o sistema inicia automaticamente um novo treinamento para incorporar os novos dados.

    ![Captura de tela da correção de uma intenção reconhecida](images/correct_intent.png)

1.  {: #intents-mark-irrelevant}Se a entrada não estiver relacionada a nenhuma intenção em seu aplicativo, isso poderá ser ensinado ao seu assistente selecionando a intenção exibida e, em seguida, clicando em **Marcar como irrelevante**.

    ![Captura de tela marcar como irrelevante](images/irrelevant.png)

    Para obter mais informações sobre essa ação, consulte [Ensinando seu assistente sobre tópicos a serem ignorados](/docs/services/assistant?topic=assistant-logs#logs-mark-irrelevant).

Se suas intenções não estão sendo corretamente reconhecidas, considere fazer os seguintes tipos de mudanças:

- Inclua o texto não reconhecido como um exemplo para a intenção correta.
- Mova exemplos existentes de uma intenção para outra.
- Considere se suas intenções são muito semelhantes e redefina-as conforme apropriado.

## Pontuação absoluta
{: #intents-absolute-scoring}

O serviço {{site.data.keyword.conversationshort}} pontua a confiança de cada intenção independentemente, não em relação a outras intenções. Essa abordagem inclui flexibilidade; múltiplas intenções podem ser detectadas em uma única entrada do usuário. Isso também significa que o sistema pode não retornar nenhuma intenção. Se a intenção superior tiver uma pontuação de confiança baixa (menor que 0,2), a intenção superior será incluída na matriz de intenções que é retornada pela API, mas quaisquer nós que condicionarem a intenção não serão acionados. Se você desejar detectar o caso quando nenhuma intenção com boa pontuação de confiança foi detectada, use a condição especial `irrelevant` em seu nó de diálogo. Consulte  [ Condições especiais ](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-special-conditions)  para obter mais informações.

Conforme as pontuações de confiança de intenção mudam, seus diálogos podem precisar de reestruturação. Por exemplo, se um nó de diálogo usar uma intenção em sua condição e a pontuação de confiança da intenção começar a cair de forma consistente abaixo de 0,2, o nó de diálogo parará de ser processado. Se a pontuação de confiança mudar, o comportamento do diálogo também poderá mudar.

## Limites de intenção
{: #intents-limits}

O número de intenções e exemplos que podem ser criados depende do tipo de plano de seu {{site.data.keyword.conversationshort}}:

| Plano     | Intenções por habilidade | Exemplos por habilidade |
|------------------|------------------:|-------------------:|
| Premium          |             2.000 |             25.000 |
| Mais             |             2.000 |             25.000 |
| Padrão         |             2.000 |             25.000 |
| Lite, Plus Trial |               100 |             25.000 |
{: caption="Detalhes do plano" caption-side="top"}

## Editando intenções
{: #intents-edit}

É possível clicar em qualquer intenção na lista para abri-la para edição. É possível fazer as seguintes mudanças:

- Renomear a intenção.
- Excluir a intenção.
- Incluir, editar ou excluir exemplos.
- Mover um exemplo para uma intenção diferente.

É possível usar a tecla tab do nome da intenção para cada exemplo, editando os exemplos se você desejar.

Para mover ou excluir um exemplo, clique na caixa de seleção associada a ela e, em seguida, clique em **Mover** ou **Excluir**.

  ![Captura de tela mostrando como mover ou excluir um exemplo](images/move_example.png)

## Procurando intenções
{: #intents-search}

Use o recurso Procurar para localizar exemplos de usuários, nomes de intenções e descrições.

1.  Na página **Intenções**, clique no ícone de Procura.

    ![Ícone de procura no cabeçalho da página Intenções](images/header-search-icon.png)

1.  Inserir um termo de procura ou frase.

    ![Termo de procura de Intenção](images/searchint_1.png)

As intenções contendo seu termo de procura, com exemplos correspondentes, são mostradas.

  ![Retorno de procura de intenção](images/searchint_2.png)

## Exportando intenções
{: #intents-export}

É possível exportar várias intenções para um arquivo CSV, depois é possível importar e reutilizá-las para outro aplicativo {{site.data.keyword.conversationshort}}.

1.  Na página **Intenções**, selecione as intenções que deseja na lista e clique em **Exportar**.

    ![Opção Exportar](images/ExportIntent.png)

## Importando intenções e exemplos
{: #intents-import}

Se tiver um grande número de intenções e exemplos, importá-los de um arquivo de valor separado por vírgula (CSV) poderá ser mais fácil do que os definir um a um. Certifique-se de remover quaisquer dados pessoais dos exemplos do usuário que você incluir no arquivo.

Como alternativa, é possível fazer upload de um arquivo com elocuções brutas do usuário (de logs de central de atendimento, por exemplo) e permitir que o Watson localize candidatos para exemplos de usuário nos dados. Consulte [Incluindo exemplos de arquivos de log](/docs/services/assistant?topic=assistant-intent-recommendations#intent-recommendations-get-example-recommendations) para obter mais informações. Esse recurso está disponível somente para os usuários do plano Plus ou Premium.

1.  Colete as intenções e exemplos em um arquivo CSV ou exporte-os de uma planilha para um arquivo CSV. O formato obrigatório para cada linha no arquivo é o seguinte:

    ```
    <example>,<intent>
    ```
    {: screen}

    em que `<example>` é o texto de um exemplo de usuário e `<intent>` é o nome da intenção com a qual você deseja que o exemplo corresponda. Por exemplo:

    ```
    Tell me the current weather conditions.,weather_conditions
    Is it raining?,weather_conditions
    What's the temperature?,weather_conditions
    Where is your nearest location?,find_location
    Do you have a store in Raleigh?,find_location
    ```
    {: screen}

    **Importante:** Salve o arquivo CSV com codificação UTF-8 e nenhuma marca de ordem de byte (BOM).

1.  Na página **Intenções**, clique no ícone *Importar* ![Ícone Importar](images/importGA.png) e, em seguida, arraste um arquivo ou navegue para selecionar um arquivo de seu computador.

    ![Opção Importar](images/ImportIntent.png)

    **Importante:** o tamanho máximo do arquivo CSV é de 10 MB. Se o seu arquivo CSV for maior, considere dividi-lo em múltiplos arquivos e importá-los separadamente.

    O arquivo é validado e importado e o sistema começa a treinar com os novos dados.

É possível visualizar as intenções importadas e os exemplos correspondentes na guia **Intenções**. Talvez seja necessário atualizar a página para ver as novas intenções e exemplos.

## Resolvendo conflitos de intenção ![Somente Plus ou Premium](images/plus.png)
{: #intents-resolve-conflicts}

Esse recurso está disponível somente para usuários do Plus ou Premium.
{: note}

O aplicativo {{site.data.keyword.conversationshort}} detecta um conflito quando dois ou mais exemplos de intenção em intenções *separadas* são tão semelhantes que o {{site.data.keyword.conversationshort}} fica confuso quanto a qual intenção usar.

Para resolver conflitos:

1.  Na página **Intenções**, revise quaisquer intenções com conflitos.

    ![Conflicts in intent list](images/ConflictIntent1.png)

    Alterne o comutador **Mostrar somente conflitos** para ver uma lista de apenas suas intenções com conflitos.
    {: tip}

    ![Conflicts only view](images/ConflictIntent2.png)

1.  Abra um conflito de intenção. Para obter o exemplo de intenção que está causando o conflito, clique em **Resolver conflito**.

    ![Conflicting intent example](images/ConflictIntent3.png)

1.  Agora, você tem a opção de mover um exemplo conflitante para outra intenção ou excluir um exemplo conflitante completamente.

    Nesse caso, os exemplos `Cancel my order` e `I want to cancel my order` aparecem na intenção `#cancel` e na intenção `#eCommerce_Cancel_Product_Order`.

    ![Conflicting intent example](images/ConflictIntent4.png)

    Exemplos do usuário adicionais são exemplos de treinamento que não estão necessariamente em conflito, mas são semelhantes aos exemplos em conflito. Eles são mostrados para fornecer contexto para ajudar a resolver o conflito.

1.  Selecione os exemplos `Cancel my order` e `I want to cancel my order` e mova-os da intenção `#cancel` para a intenção `#eCommerce_Cancel_Product_Order`:

    ![Conflicting intent example](images/ConflictIntent5.png)

1.  Ao decidir onde colocar um exemplo, procure a intenção que tenha exemplos sinônimos ou quase sinônimos.

    Mantenha cada intenção como distinta e focada em um objetivo, conforme possível. Se você tiver duas intenções com múltiplos exemplos do usuário que se sobrepõem, talvez não precise de duas intenções separadas. É possível mover ou excluir exemplos do usuário que não se sobrepõem diretamente em uma intenção e, em seguida, excluir o outro.
    {: tip}

    Selecione os outros exemplos na intenção `#cancel` e exclua-os:

    ![Conflicting intent example](images/ConflictIntent6.png)

1.  Clique no botão **Enviar** para resolver os conflitos:

    ![Conflicting intent example](images/ConflictIntent7.png)

    A opção *Reconfigurar* permite iniciar novamente com a movimentação do exemplo de conflito entre as intenções. *Cancelar* retorna você para a página de intenção.

Você resolveu um conflito e pode continuar sua revisão de outras intenções com conflitos.

Assista a este vídeo para aprender mais.

<iframe class="embed-responsive-item" id="youtubeplayer0" title="Visão geral de resolução de conflito de intenção" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/9gQtjCBxjdc?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Excluindo intenções
{: #intents-delete}

É possível selecionar várias intenções para exclusão.

**IMPORTANTE**: ao excluir intenções, todos os exemplos associados também são excluídos e não é possível recuperá-los posteriormente. Todos os nós de diálogo que fazem referência a essas intenções devem ser atualizados manualmente para que não façam referencia ao conteúdo excluído.

1.  Na página **Intenções**, selecione as intenções que deseja na lista e clique em **Excluir**.

    ![Opção Excluir](images/DeleteIntent.png)
