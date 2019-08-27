---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-06"

keywords: context, context variable, digression, disambiguation, autocorrection, spelling correction, spell check, confidence 

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
{:gif: data-image-type='gif'}

# Como o diálogo é processado
{: #dialog-runtime}

Entenda como seu diálogo é processado quando uma pessoa interage com sua instância do serviço {{site.data.keyword.conversationshort}} implementado no tempo de execução.
{: shortdesc}

## Anatomia de uma chamada de diálogo
{: #dialog-runtime-message-anatomy}

Cada elocução do usuário é passada para o diálogo como uma chamada de API /message. Isso inclui elocuções que os usuários fazem em resposta a prompts do diálogo que fazem perguntas a eles para obter mais informações. Alguns planos de assinatura incluem um número configurado de chamadas API, assim isso ajuda a entender o que constitui uma chamada. Uma única chamada API /message é equivalente a uma única rodada de diálogo, que consiste em uma entrada do usuário e uma resposta correspondente do diálogo.

O corpo da solicitação e resposta de chamada API /message inclui os objetos a seguir:

- `context`: contém as variáveis que devem ser persistidas. Para passar informações de uma chamada para a próxima, o desenvolvedor de aplicativos deve passar o contexto de resposta da chamada API anterior com cada chamada API subsequente. Por exemplo, o diálogo pode coletar o nome do usuário e, então, referir-se ao usuário pelo nome em nós subsequentes. O exemplo a seguir mostra como o objeto de contexto é representado no editor JSON de diálogo:

  ```json
  {
    "context" : {
      "user_name" : "<? @sys-person.literal ?>"
    }
  ```
  {: codeblock}

  Veja [Retendo informações em rodadas de diálogo](#dialog-runtime-context) para obter mais informações.

- `input`: a sequência de texto que foi enviada pelo usuário. A sequência de texto pode conter até 2.048 caracteres. O exemplo a seguir mostra como o objeto de entrada é representado no editor JSON de diálogo:

  ```json
  {
    "input" : {
      "text" : "Where's your nearest store?"
    }
  ```
  {: codeblock}

- `output`: a resposta de diálogo para retornar ao usuário. O exemplo a seguir mostra como o objeto de saída é representado no editor JSON de diálogo:

  ```json
  {
  "output": {
    "generic":[
      {
        "values": [
          {
            "text": "This is my response text."
          }
        ], "response_type": "text", "selection_policy": "sequential" }
    ]
  }
  }
  ```
  {: codeblock}

Na resposta /message da API resultante, a resposta de texto é formatada conforme a seguir:

```json
{
   "text": "This is my response text.",
   "response_type": "text"
}
```

O formato JSON de objeto `output` a seguir é suportado para a compatibilidade com versões anteriores. Quaisquer áreas de trabalho que especificarem uma resposta de texto usando esse formato continuarão a funcionar adequadamente. Com a introdução de tipos de resposta rica, a estrutura `output.text` foi aumentada com a estrutura `output.generic` para facilitar o suporte a outros tipos de respostas, além do texto. Use o novo formato quando você criar novos nós para dar a si mesmo mais flexibilidade, porque será possível mudar posteriormente o tipo de resposta, se necessário.
{: note}

  ```json
  {
  "output": {
    "text": {
      "values": [
        "This is my response text."
      ]
    }
  }
  ```
  {: codeblock}

Há tipos de resposta diferentes de uma resposta de texto que podem ser definidos. Consulte  [ Respostas ](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)  para obter mais detalhes.

É possível aprender mais sobre a chamada de API /message na [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/assistant-v2){: new_window}.

### Retendo informações em rodadas de diálogo
{: #dialog-runtime-context}

O diálogo em uma qualificação de diálogo é stateless, o que significa que ele não retém informações de uma interação com o usuário para a seguinte. Quando você inclui uma qualificação de diálogo em um assistente e implementa-a, o assistente salva o contexto de uma chamada de mensagem e, então, envia-a novamente na próxima solicitação durante a sessão atual. A sessão atual dura pelo tempo que um usuário interage com o assistente e, em seguida, até 60 minutos de inatividade para planos Plus ou Premium (5 minutos para os planos Lite ou Padrão). Se você não incluir a qualificação de diálogo em um assistente, será sua responsabilidade como desenvolvedor de aplicativos customizados manter quaisquer informações contínuas que o aplicativo precisar. O aplicativo deve procurar e armazenar o objeto de contexto na resposta da API da mensagem e passá-lo no objeto de contexto com a próxima solicitação de API /message que for feita como parte do fluxo de conversa.

Uma maneira de reter as informações em si mesmo é armazenar o objeto de contexto inteiro na memória no aplicativo cliente, em um navegador da web, por exemplo. Conforme um aplicativo se torna mais complexo ou se ele precisa passar e armazenar informações pessoalmente identificáveis, é possível armazenar e recuperar as informações de um banco de dados. Obviamente, a abordagem mais simples é aquela que evita que você tenha que armazenar o contexto. Para implementar essa abordagem, inclua a qualificação do diálogo em um assistente e permita que o assistente mantenha o controle do contexto para você.

O aplicativo pode passar informações para o diálogo e o diálogo pode atualizar essas informações e passá-las de volta para o aplicativo ou para um nó subsequente. O diálogo faz isso usando *variáveis de contexto*.

## Variáveis de contexto
{: #dialog-runtime-context-variables}

Uma variável de contexto é aquela que você define em um nó. É possível especificar um valor padrão para ela. Outros nós, lógica de aplicativo ou entrada do usuário podem configurar ou mudar subsequentemente o valor da variável de contexto.

É possível condicionar com relação aos valores das variáveis de contexto referenciando uma variável de contexto de uma condição de nó de diálogo para determinar se deve executar um nó. Também é possível referenciar uma variável de contexto de condições de resposta do nó de diálogo para mostrar diferentes respostas, dependendo de um valor fornecido por um serviço externo ou pelo usuário.

Saiba mais:

- [Transmitindo o contexto do aplicativo](#dialog-runtime-context-from-app)
- [Transmitindo o contexto de nó para nó](#dialog-runtime-context-node-to-node)
- [Definindo uma variável de contexto](#dialog-runtime-context-var-define)
- [Tarefas comuns de variável de contexto](#dialog-runtime-context-common-tasks)
- [Excluindo uma variável de contexto](#dialog-runtime-context-delete)
- [ Atualizando uma variável de contexto ](#dialog-runtime-context-update)
- [ Como as variáveis de contexto são processadas ](#dialog-runtime-context-processing)
- [Ordem de operação](#dialog-runtime-context-order-of-ops)
- [Incluindo variáveis de contexto em um nó com intervalos](#dialog-runtime-context-var-slots)

### Transmitindo o contexto do aplicativo
{: #dialog-runtime-context-from-app}

Transmita informações do aplicativo para o diálogo configurando uma variável de contexto e transmitindo a variável de contexto para o diálogo.

Por exemplo, seu aplicativo pode configurar uma variável de contexto $time_of_day e transmiti-la para o diálogo, que pode usar as informações para customizar a saudação exibida para o usuário.

![Mostra um nó Welcome que usa condições de resposta para verificar o valor da variável de contexto $time_of_day que é transmitida para o diálogo desde o aplicativo.](images/set-context.png)

Neste exemplo, o diálogo sabe que o aplicativo configura a variável com um destes valores: *manhã*, *tarde* ou *noite*. Ele pode verificar cada valor e, dependendo de qual valor estiver presente, retornar a saudação apropriada. Se a variável não for transmitida ou possuir um valor que não corresponde a um dos valores esperados, uma saudação mais genérica será exibida para o usuário.

### Transmitindo o contexto de nó para nó
{: #dialog-runtime-context-node-to-node}

O diálogo também pode incluir variáveis de contexto para transmitir informações de um nó para outro ou para atualizar os valores de variáveis de contexto. Conforme o diálogo pede e obtém informações do usuário, ele pode rastrear as informações e fazer referência a elas mais tarde na conversa.

Por exemplo, em um nó você pode perguntar o nome dos usuários e, em um nó posterior, tratá-los pelo nome.

![Mostra um nó de introduções que pergunta o nome do usuário e o armazena como uma variável de contexto. O próximo nó refere-se ao usuário por nome usando a variável de contexto $username.](images/set-context-username.png)

Neste exemplo, a entidade do sistema @sys-person será usada para extrair o nome do usuário da entrada, se o usuário fornecer um. No editor JSON, a variável de contexto username é definida e configurada com o valor @sys-person. Em um nó subsequente, a variável de contexto $username é incluída na resposta para tratar o usuário pelo nome.

### Definindo uma variável de contexto
{: #dialog-runtime-context-var-define}

Defina uma variável de contexto incluindo o nome da variável no campo **Variável** e incluindo um valor padrão para ela no campo **Valor** na visualização de edição do nó.

1.  Clique para abrir o nó de diálogo no qual você deseja incluir uma variável de contexto.

1.  Clique no ícone **Opções** ![Resposta avançada](images/kabob.png) que está associado à resposta do nó e, em seguida, clique em **Abrir o editor de contexto**.

      ![Mostra como acessar o editor da JSON associado com uma resposta de nó padrão.](images/contextvar-json-response.png)

      Caso a configuração de **Múltiplas respostas** esteja **Ativa** para o nó, deve-se primeiramente clicar no ícone **Editar resposta** ![Editar resposta](images/edit-slot.png) da resposta à qual você deseja associar a variável de contexto.

      ![Mostra como acessar o editor da JSON associado com um nó padrão que tem múltiplas respostas condicionais ativadas para ele.](images/contextvar-json-multi-response.png)

1.  Inclua o nome da variável e o par de valores nos campos **Variável** e **Valor**.

    - O `name` pode conter quaisquer caracteres alfabéticos em maiúsculas e minúsculas, caracteres numéricos (0-9) e sublinhados.

      É possível incluir outros caracteres, como pontos e hifens, no nome. No entanto, em caso positivo, deve-se especificar a sintaxe abreviada `$(variable-name)` sempre que a variável for referenciada subsequentemente. Consulte [Expressões para acessar objetos](/docs/services/assistant?topic=assistant-expression-language#expression-language-shorthand-context) para obter mais detalhes.
      {:tip}

    - O `value` pode ser qualquer tipo JSON suportado, tal como uma variável de sequência simples, um número, uma matriz JSON ou um objeto JSON.

A tabela a seguir mostra alguns exemplos de como definir pares de nome e valor para diferentes tipos de valores:

| Variável       | Valor                         | Tipo de Valor |
|:---------------|-------------------------------|------------|
| dessert        | "Bolo"                        | Sequência     |
| age            | 18                            | Número     |
| toppings_array | ["onions","olives"]            | Matriz JSON |
| full_name      | {"first":"John","last":"Doe"} | Objeto JSON |

Para referir-se subsequentemente a essas variáveis de contexto, use a sintaxe `$name`, em que *name* é o nome da variável de contexto que você definiu.

Por exemplo, você pode especificar a expressão a seguir como a resposta de diálogo:

`The customer, $age-year-old <? $full_name.first ?>, wants a pizza with <? $toppings_array.join(' and ') ?>, and then $dessert.`

A saída resultante é exibida conforme a seguir:

`The customer, 18-year-old John, wants a pizza with onions and olives, and then cake.`

É possível usar o editor JSON para definir variáveis de contexto também. Você poderá preferir usar o editor JSON se desejar incluir uma expressão complexa como o valor da variável. Consulte [Variáveis de contexto no editor JSON](#dialog-runtime-context-var-json) para obter mais detalhes.

### Tarefas comuns de variável de contexto
{: #dialog-runtime-context-common-tasks}

Para armazenar a sequência inteira que foi fornecida pelo usuário como entrada, use `input.text`:

| Variável | Valor            |
|----------|------------------|
| repeat   | `<?input.text?>` |

Por exemplo, a entrada do usuário é `I want to order a device.` Se a resposta do nó for `You said: $repeat`, a resposta será exibida como `You said: I want to order a device.`

Para armazenar o valor de uma entidade em uma variável de contexto, use esta sintaxe:

| Variável | Valor            |
|----------|------------------|
| place    | `@place`         |

Por exemplo, a entrada do usuário é `I want to go to Paris.` Se sua entidade `@place` reconhecer `Paris`, seu assistente salvará `Paris` na variável de contexto `$place`.

Para armazenar o valor de uma sequência que você extrai da entrada do usuário, é possível incluir uma expressão SpEL que usa o método `extract` para aplicar uma expressão regular à entrada do usuário. A expressão a seguir extrai um número da entrada do usuário e o salva na variável de contexto `$number`.

| Variável | Valor                               |
|----------|-------------------------------------|
| número   | `<?input.text.extract('[\d]+',0)?>` |

Para armazenar o valor de uma entidade padrão, anexe .literal ao nome da entidade. O uso desta sintaxe assegura que a extensão exata de texto da entrada do usuário que correspondeu ao padrão especificado seja armazenada na variável.

| Variável | Valor                  |
|----------|------------------------|
| email    | `<? @email.literal ?>` |

Por exemplo, a entrada do usuário é `Contact me at joe@example.com.` Sua entidade denominada `@email` reconhece o formato de e-mail `name@domain.com`. Configurando a variável de contexto para armazenar `@email.literal`, você indica que deseja armazenar a parte da entrada que correspondeu ao padrão. Se você omitir a propriedade `.literal` da expressão de valor, o nome do valor da entidade especificado para o padrão será retornado no lugar do segmento de entrada do usuário que correspondeu ao padrão.

Muitos desses exemplos de valor usam métodos para capturar diferentes partes da entrada do usuário. Para obter mais informações sobre os métodos disponíveis para você usar, consulte [Métodos de linguagem de expressão](/docs/services/assistant?topic=assistant-dialog-methods).

### Excluindo uma variável de contexto
{: #dialog-runtime-context-delete}

Para excluir uma variável de contexto, configure a variável para nulo.

| Variável   | Valor            |
|------------|------------------|
| order_form | `null`           |

Como alternativa, é possível excluir a variável de contexto na sua lógica de aplicativo. Para obter informações sobre como remover a variável inteiramente, consulte [Excluindo uma variável de contexto em JSON](#dialog-runtime-context-delete-json).

### Atualizando um valor da variável de contexto
{: #dialog-runtime-context-update}

Para atualizar o valor de uma variável de contexto, defina uma variável de contexto com o mesmo nome que a variável de contexto anterior, mas, desta vez, especifique um valor diferente para ela.

Quando mais de um nó configura o valor da mesma variável de contexto, o valor para a variável de contexto pode mudar durante o curso de uma conversa com um usuário. O valor que é aplicado em um determinado momento depende de qual nó está sendo acionado pelo usuário no curso da conversa. O valor especificado para a variável de contexto no último nó que é processado sobrescreve quaisquer valores que foram configurados para a variável por nós que foram processados anteriormente.

Para obter informações sobre como atualizar o valor de uma variável de contexto quando o valor for um objeto JSON ou um tipo de dados da matriz JSON, consulte [Atualizando um valor da variável de contexto em JSON](#dialog-runtime-context-update-json)

### Como as variáveis de contexto são processadas
{: #dialog-runtime-context-processing}

Onde você define a variável de contexto importa. A variável de contexto não é criada e configurada para o valor especificado para ela até que seu assistente processe a parte do nó de diálogo na qual você definiu a variável de contexto. Na maioria dos casos, você define a variável de contexto como parte da resposta do nó. Ao fazer isso, a variável de contexto é criada e recebe o valor especificado quando seu assistente retorna a resposta do nó.

Para um nó com respostas condicionais, a variável de contexto é criada e configurada quando a condição para uma resposta específica é atendida e essa resposta é processada. Por exemplo, se você definir uma variável de contexto para a resposta condicional nº 1 e o seu assistente processar somente a resposta condicional nº 2, a variável de contexto definida para a resposta condicional nº 1 não será criada e configurada.

Para obter informações sobre o local de inclusão das variáveis de contexto que deseja que seu assistente crie e configure, à medida que um usuário interage com um nó com slots, consulte [Incluindo variáveis de contexto em um nó com slots](#dialog-runtime-context-var-slots).

### Ordem de operação
{: #dialog-runtime-context-order-of-ops}

Ao definir que diversas variáveis sejam processadas juntas, a ordem da definição não determina a ordem de avaliação dessas variáveis pelo seu assistente. Seu assistente avalia as variáveis em ordem aleatória. Não configure um valor na primeira variável de contexto na lista e espere ser capaz de usá-lo na segunda variável na lista, porque não há garantia de que a primeira variável de contexto será executada antes da segunda. Por exemplo, não use duas variáveis de contexto para implementar a lógica que verifica se a entrada do usuário contém a palavra `Yes` nela.

| Variável        | Valor            |
|-----------------|------------------|
| user_input      | <? input.text ?> |
| contains_yes    | <? $user_input.contains('Yes') ?> |

Em vez disso, use uma expressão um pouco mais complexa para evitar precisar depender do valor da primeira variável em sua lista (user_input) que está sendo avaliada antes que a segunda variável (contains_yes) seja avaliada.

| Variável      | Valor            |
|---------------|------------------|
| contains_yes  | <? input.text.contains('Yes') ?> |

### Incluindo variáveis de contexto em um nó com intervalos
{: #dialog-runtime-context-var-slots}

Para obter mais informações sobre intervalos, consulte [Reunindo informações com intervalos](/docs/services/assistant?topic=assistant-dialog-slots).

1.  Abra o nó com intervalos na visualização de edição.

    - Para incluir uma variável de contexto que é processada após uma condição de resposta para um intervalo ser atendida, execute as etapas a seguir:

      1.  Clique no ícone **Editar slot** ![Editar resposta](images/edit-slot.png).
      1.  Clique no ícone **Opções** ![Resposta avançada](images/kabob.png) e, em seguida, selecione **Ativar respostas condicionais**.
      1.  Clique no ícone **Editar resposta** ![Editar resposta](images/edit-slot.png) ao lado da resposta com a qual você deseja associar a variável de contexto.
      1.  Clique no ícone **Opções** ![Resposta avançada](images/kabob.png) na seção de resposta e, em seguida, clique em **Abrir editor de contexto**.
      1.  Inclua o nome da variável e o par de valores nos campos **Variável** e **Valor**.

      ![Mostra como acessar o editor da JSON associado com a resposta condicional para um intervalo.](images/contextvar-json-slot-multi-response.png)

    - Para incluir uma variável de contexto que é configurada ou atualizada depois que uma condição de intervalo é atendida, conclua as etapas a seguir:

      1.  Clique no ícone **Editar slot** ![Editar resposta](images/edit-slot.png).
      1.  No menu **Opções** ![Resposta avançada](images/kabob.png) no cabeçalho de visualização *Configurar intervalo*, clique em **Abrir editor JSON**.
      1.  Inclua o nome da variável e o par de valores no formato JSON.

          ```json
          {
            "time_of_day": "morning"
          }
          ```
          {: codeblock}

      Atualmente, não há como usar o editor de contexto para definir as variáveis de contexto que são configuradas durante essa fase de avaliação do nó de diálogo. Deve-se usar o editor JSON no lugar. Para obter mais informações sobre como usar o editor JSON, consulte [Variáveis de contexto no editor JSON](#dialog-runtime-context-var-json).
      {: note}

      ![Mostra como acessar o editor da JSON associado com uma condição do intervalo.](images/contextvar-json-slot-condition.png)

## Variáveis de contexto no editor JSON
{: #dialog-runtime-context-var-json}

Também é possível definir uma variável de contexto no editor JSON. Você poderá desejar usar o editor JSON se estiver definindo uma variável de contexto complexa e desejar ser capaz de ver a expressão SpEL integral à medida que incluir ou mudá-la.

O par de nome e valor deve atender a estes requisitos:

- O `name` pode conter quaisquer caracteres alfabéticos em maiúsculas e minúsculas, caracteres numéricos (0-9) e sublinhados.

  É possível incluir outros caracteres, como pontos e hifens, no nome. No entanto, em caso positivo, deve-se especificar a sintaxe abreviada `$(variable-name)` sempre que a variável for referenciada subsequentemente. Consulte [Expressões para acessar objetos](/docs/services/assistant?topic=assistant-expression-language#expression-lanaguage-shorthand-context) para obter mais detalhes.
  {:tip}

- O `value` pode ser qualquer tipo JSON suportado, tal como uma variável de sequência simples, um número, uma matriz JSON ou um objeto JSON.

A amostra JSON a seguir define valores para as variáveis de contexto de sequência $dessert, matriz $toppings_array, número $age e objeto $full_name:

```json
{
  "context": {
    "dessert": "cake",
    "toppings_array": [
      "onions",
      "olives"
    ],
    "age": 18,
    "full_name": {
      "first": "Jane",
      "last": "Doe"
    }
  },
  "output":{}
}
```
{: codeblock}

Para definir uma variável de contexto no formato JSON, conclua as etapas a seguir:

1.  Clique para abrir o nó de diálogo no qual você deseja incluir a variável de contexto.

    Quaisquer valores de variáveis de contexto existentes que estão definidos para esse nó são exibidos em um conjunto de campos **Variável** e **Valor** correspondentes. Se você não deseja que eles sejam exibidos na visualização de edição do nó, deve-se fechar o editor de contexto. É possível fechar o editor por meio do mesmo menu que é usado para abrir o editor JSON; as etapas a seguir descrevem como acessar o menu.
    {: note}

1.  Clique no ícone **Opções** ![Resposta avançada](images/kabob.png) que está associado à resposta e, em seguida, clique em **Abrir editor JSON**.

    ![Mostra como acessar o editor da JSON associado com uma resposta de nó padrão.](images/contextvar-json-response.png)

    Caso a configuração de **Múltiplas respostas** esteja **Ativa** para o nó, deve-se primeiramente clicar no ícone **Editar resposta** ![Editar resposta](images/edit-slot.png) da resposta à qual você deseja associar a variável de contexto.

    ![Mostra como acessar o editor da JSON associado com um nó padrão que tem múltiplas respostas condicionais ativadas para ele.](images/contextvar-json-multi-response.png)

1.  Inclua um bloco `"context":{}` se um não estiver presente.

    ```json
    {
      "context":{},
      "output":{}
    }
    ```
    {: codeblock}

1.  No bloco de contexto, inclua um par `"name"` e `"value"` para cada variável de contexto que você deseja definir.

    ```json
    {
      "context":{
        "name": "value"
    },
      "output": {}
    }
    ```
    {: codeblock}

    Neste exemplo, uma variável nomeada `new_variable` é incluída em um bloco de contexto que já contém uma variável.

    ```json
    {
      "context":{
        "existing_variable": "value",
        "new_variable":"value"
      }
    }
    ```
    {: codeblock}

    Para referenciar a variável de contexto subsequentemente, use a sintaxe `$name` em que *name* é o nome da variável de contexto que você definiu. Por exemplo, `$new_variable`.

Saiba mais:

- [ Excluindo uma variável de contexto em JSON ](#dialog-runtime-context-delete-json)
- [Atualizando um valor da variável de contexto em JSON](#dialog-runtime-context-update-json)
- [Configurando uma variável de contexto igual à outra](#dialog-runtime-var-equals-var)

### Excluindo uma variável de contexto no JSON
{: #dialog-runtime-context-delete-json}

Para excluir uma variável de contexto, configure a variável para nulo.

```json
{
  "context": {
    "order_form": null
  }
}
```
{: codeblock}

Se você deseja remover todo rastreio da variável de contexto, é possível usar o método JSONObject.remove(string) para excluí-lo do objeto de contexto. No entanto, deve-se usar uma variável para executar a remoção. Defina a nova variável na saída de mensagem para que ela não seja salva além da chamada atual.

```json
{
  "output": {
    "text" : {},
    "deleted_variable" : "<? context.remove('order_form') ?>"
  }
}
```
{: codeblock}

Como alternativa, é possível excluir a variável de contexto na sua lógica de aplicativo.

### Atualizando um valor da variável de contexto em JSON
{: #dialog-runtime-context-update-json}

Em geral, se um nó configura o valor de uma variável de contexto que já está configurado, o valor anterior é sobrescrito pelo novo valor.

#### Atualizando um objeto JSON complexo

Os valores anteriores são sobrescritos para todos os tipos de JSON, exceto um objeto JSON. Se a variável de contexto é um tipo complexo tal como o objeto JSON, um procedimento de mesclagem JSON é usado para atualizar a variável. A operação de mesclagem inclui quaisquer propriedades recém-definidas e sobrescreve quaisquer propriedades existentes do objeto.

Neste exemplo, uma variável de contexto name é definida como um objeto complexo.

```json
{
  "context": {
    "complex_object": {
      "user_firstname" : "Paul",
      "user_lastname" : "Pan",
      "has_card" : false
    }
  }
}
```
{: codeblock}

Um nó de diálogo atualiza o objeto JSON da variável de contexto com os valores a seguir:

```json
{
  "complex_object": {
    "user_firstname": "Peter",
    "has_card": true
  }
}
```
{: codeblock}

O resultado é este contexto:

```json
{
  "complex_object": {
    "user_firstname": "Peter",
    "user_lastname": "Pan",
    "has_card": true
  }
}
```
{: codeblock}

Veja [Métodos de linguagem de expressão](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-objects) para obter mais informações sobre os métodos que podem ser executados em objetos.

#### Atualizando matrizes

Se seus dados de contexto de diálogo contiverem uma matriz de valores, você poderá atualizar a matriz anexando valores, removendo um valor ou substituindo todos os valores.

Escolha uma dessas ações para atualizar a matriz. Em cada caso, vemos a matriz antes da ação, a ação e a matriz após a ação ter sido aplicada.

- **Anexar**: para incluir valores no final de uma matriz, use o método `append`.

    Para esse Contexto de tempo de execução de diálogo:

    ```json
    {
      "context": {
        "toppings_array": ["onion", "olives"]
      }
    }
    ```
    {: codeblock}

    Faça essa atualização:

    ```json
    {
      "context": {
        "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
      }
    }
    ```
    {: codeblock}

    Resultado:

    ```json
    {
      "context": {
        "toppings_array": ["onion", "olives", "ketchup", "tomatoes"]
      }
    }
    ```
    {: codeblock}

- **Remover**: Para remover um elemento, use o método `remove` e especifique seu valor ou posição na matriz.

    - **Remover por valor** remove um elemento de uma matriz por seu valor.

        Para esse Contexto de tempo de execução de diálogo:

        ```json
        {
          "context": {
            "toppings_array": ["onion", "olives"]
          }
        }
        ```
        {: codeblock}

        Faça essa atualização:

        ```json
        {
          "context": {
            "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
          }
        }
        ```
        {: codeblock}

        Resultado:

        ```json
        {
          "context": {
            "toppings_array": ["olives"]
          }
        }
        ```
        {: codeblock}

    - **Remover por posição**: removendo um elemento de uma matriz por sua posição de índice:

        Para esse Contexto de tempo de execução de diálogo:

        ```json
        {
          "context": {
            "toppings_array": ["onion", "olives"]
          }
        }
        ```
        {: codeblock}

        Faça essa atualização:

        ```json
        {
          "context": {
            "toppings_array": "<? $toppings_array.remove(0) ?>"
          }
        }
        ```
        {: codeblock}

        Resultado:

        ```json
        {
          "context": {
            "toppings_array": ["olives"]
          }
        }
        ```
        {: codeblock}

- **Sobrescrever**: para sobrescrever os valores em uma matriz, simplesmente configure a matriz com os novos valores:

    Para esse Contexto de tempo de execução de diálogo:

        ```json
        {
          "context": {
            "toppings_array": ["onion", "olives"]
          }
        }
        ```
        {: codeblock}

    Faça essa atualização:

        ```json
        {
          "context": {
            "toppings_array": ["ketchup", "tomatoes"]
          }
        }
        ```
        {: codeblock}

    Resultado:

        ```json
        {
          "context": {
            "toppings_array": ["ketchup", "tomatoes"]
          }
        }
        ```
        {: codeblock}

Veja [Métodos de linguagem de expressão](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-arrays) para obter mais informações sobre os métodos que podem ser executados em matrizes.

### Configurando uma variável de contexto igual à outra
{: #dialog-runtime-var-equals-var}

Ao configurar uma variável de contexto igual à outra variável de contexto, você define um ponteiro de uma para a outra. Se o valor de uma das variáveis mudar subsequentemente, o valor da outra variável também mudará.

Por exemplo, se você especificar uma variável de contexto como a seguir, quando o valor de `$var1` ou `$var2` mudar subsequentemente, o valor das outras mudará também.

| Variável  | Valor  |
|-----------|--------|
| var2      | var1   |

Não configure uma variável igual à outra para capturar um valor de momento. Ao lidar com matrizes, por exemplo, se você desejar capturar um valor de matriz armazenado em uma variável de contexto em um determinado ponto no diálogo para salvá-lo para uso posterior, será possível criar uma nova variável com base no valor atual da variável.

Por exemplo, para criar uma cópia dos valores de uma matriz em um determinado ponto do fluxo de diálogo, inclua uma nova matriz que seja preenchida com os valores para a matriz existente. Para fazer isso, é possível usar a sintaxe a seguir:

```json
{
"context": {
   "var2": "<? output.var2?:new JsonArray().append($var1) ?>"
 }
 }
 ```
{: codeblock}

## Digressões
{: #dialog-runtime-digressions}

Uma digressão ocorre quando um usuário está no meio de um fluxo de diálogo projetado para endereçar um objetivo e alterna abruptamente os tópicos para iniciar um fluxo de diálogo projetado para endereçar um objetivo diferente. O diálogo sempre suportou a capacidade do usuário de mudar assuntos. Se nenhum dos nós na ramificação de diálogo que está sendo processada corresponde ao objetivo da entrada mais recente do usuário, a conversa volta para a árvore para verificar as condições do nó raiz para uma correspondência apropriada. As configurações de digressão que estão disponíveis por nó fornecem a capacidade de customizar esse comportamento ainda mais.

Com as configurações de digressão, é possível permitir que a conversa retorne para o fluxo de diálogo que foi interrompido quando a digressão ocorreu. Por exemplo, o usuário pode estar pedindo um novo telefone, mas alterna os tópicos para perguntar sobre tablets. Seu diálogo pode responder à pergunta sobre tablets e depois levar o usuário de volta para onde ele parou no processo de pedir um telefone. Permitir que digressões ocorram e retornem fornece a seus usuários mais controle sobre o fluxo da conversa no tempo de execução. Eles podem mudar tópicos, seguir um fluxo de diálogo sobre o tópico não relacionado até seu término e, em seguida, retornar para onde estavam antes. O resultado é um fluxo de diálogo que simula mais estritamente uma conversa entre humanos.

![Mostra alguém que está fornecendo detalhes sobre uma reserva de jantar perguntar a respeito de opções vegetarianas, obter uma resposta e então retornar para fornecer detalhes de reserva.](images/digression.gif){: gif}

A imagem animada usa um protótipo da interface com o usuário da árvore de diálogo para ilustrar o conceito de uma digressão. Ela mostra como um usuário interage com os nós de diálogo que são configurados para permitir digressões que retornam ao fluxo de diálogo que estava em andamento. O usuário começa a fornecer as informações necessárias para fazer uma reserva de jantar. No meio de preenchimento de intervalos no nó #reservation, o usuário faz uma pergunta sobre as opções de menu vegetariano. O diálogo responde à nova pergunta do usuário localizando um nó que a endereça entre os nós raiz (um nó que condiciona na intenção #cuisine). Em seguida, retorna à conversa que estava em andamento mostrando o prompt para o próximo intervalo vazio do nó de diálogo original.

Assista a este vídeo para aprender mais.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Visão geral de digressões" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/I3K7mQ46K3o?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

- [Antes de iniciar](#dialog-runtime-digression-prereqs)
- [Customizando digressões](#dialog-runtime-enable-digressions)
- [ Dicas de uso de Digressão ](#dialog-runtime-digress-tips)
- [Desativando digressões em um nó raiz](#dialog-runtime-disable-digressions)
- [ Tutorial de Digressão ](#dialog-runtime-digression-tutorial)
- [Considerações de design](#dialog-runtime-digression-design-considerations)

### Antes de iniciar
{: #dialog-runtime-digression-prereqs}

Ao testar seu diálogo geral, decida quando e onde faz sentido permitir que digressões e devoluções de digressões ocorram. Os controles de digressão a seguir são aplicados aos nós automaticamente. Tome a ação somente se desejar mudar esse comportamento padrão.

- Cada nó raiz em seu diálogo está configurado para permitir que as digressões os destinem por padrão. Os nós-filhos não podem ser o destino de uma digressão.
- Os nós com intervalos são configurados para evitar digressões fora. Todos os outros nós são configurados para permitir digressões fora. No entanto, a conversa não pode digressionar fora de um nó sob as circunstâncias a seguir:

  - Se algum dos nós-filhos do nó atual contém a condição `anything_else` ou `true`

    Essas condições são especiais por serem sempre avaliadas como true. Por causa de seu comportamento conhecido, elas são frequentemente usadas em diálogos para forçar um nó pai a avaliar um nó-filho específico sucessivamente. Para evitar a quebra da lógica do fluxo de diálogo existente, a digressão não é permitida neste caso. Antes de poder ativar digressões fora de, por exemplo, um nó, deve-se mudar a condição do nó-filho para outra coisa.

  - Se o nó estiver configurado para ir para outro nó ou ignorar a entrada do usuário após ela ser processada

    A seção de etapa final de um nó especifica o que deve acontecer após o nó ser processado. Quando o diálogo é configurado para ir diretamente para outro nó, é frequentemente para assegurar que uma sequência específica seja seguida. E quando o nó é configurado para ignorar entrada do usuário, é equivalente a forçar o diálogo para processar o primeiro nó-filho após o nó atual sucessivamente. Para evitar a quebra da lógica do fluxo de diálogo existente, as digressões não são permitidas em qualquer um desses casos. Antes de poder ativar as digressões fora desse nó, deve-se mudar o que é especificado na seção de etapa final.

### Customizando digressões
{: #dialog-runtime-enable-digressions}

Você não define o início e o término de uma digressão. O usuário está inteiramente no controle do fluxo de digressão no tempo de execução. Você somente especifica como cada nó deve ou não deve participar em uma digressão liderada pelo usuário. Para cada nó, você configura se:

- uma digressão pode ser iniciada no nó e deixar o nó
- uma digressão iniciada em outro lugar pode destinar e inserir o nó
- uma digressão que é iniciada em outro lugar e insere o nó deve retornar para o fluxo de diálogo interrompido após o fluxo de diálogo atual ser concluído

Para mudar o comportamento de digressão para um nó individual, conclua as etapas a seguir:

1.  Clique no nó para abrir sua visualização de edição.

1.  Clique em **Customizar** e, em seguida, clique na guia **Digressões**.

    As opções de configuração diferem dependendo se o nó que você está editando é um nó raiz, um nó-filho, um nó com filhos ou um nó com intervalos.

    **Digressions fora desse nó**

    Se as circunstâncias listadas anteriormente não se aplicam, é possível fazer as opções a seguir:

    - **Todos os tipos de nós**: escolha se deseja permitir a digressão do nó atual pelos usuários antes de atingirem o término da ramificação do diálogo atual.

    - **Todos os nós que têm filhos**: escolha se você deseja que a conversa volte para o nó atual após uma digressão se a resposta do nó atual já foi exibida e seus nós-filhos são incidentais com o objetivo do nó. Configure a alternância *Permitir retorno de digressões acionadas após a resposta deste nó* para **Não** para evitar que o diálogo retorne para o nó atual e continue a processar sua ramificação.

      Por exemplo, se o usuário pergunta `Deseja vender cupcakes?` e a resposta `Oferecemos cupcakes em uma variedade de sabores e tamanhos` é exibida antes de o usuário mudar os assuntos, você talvez não deseje que o diálogo retorne para onde parou. Especialmente, se os nós-filhos direcionam somente perguntas de acompanhamento possíveis do usuário e podem ser ignorados com segurança.

      No entanto, se o nó depende de seus nós-filhos para direcionar a questão, você pode desejar forçar a conversa para retornar e continuar processando os nós na ramificação atual. Por exemplo, a resposta inicial pode ser `We offer cupcakes in all shapes and sizes. Which menu do you want to see: gluten-free, dairy-free, or regular?` Se o usuário muda os assuntos neste momento, você pode desejar que o diálogo seja retornado para que o usuário possa escolher um tipo de menu e obter as informações desejadas.

    - **Nós com intervalos**: escolha se você deseja permitir aos usuários digressionar fora do nó antes de todos os intervalos serem preenchidos. Configure a alternância *Permitir digressões fora durante o preenchimento de intervalo* para **Sim** para ativar digressões fora.

      Se ativado, quando a conversa retorna da digressão, o prompt para o próximo intervalo vazio é exibido para encorajar o usuário a continuar fornecendo informações. Se desativado, quaisquer entradas enviadas pelo usuário que não contêm um valor que possa preencher um intervalo são ignoradas. No entanto, é possível direcionar questões não solicitadas que você prevê que os usuários podem perguntar enquanto interagem com o nó definindo manipuladores de intervalos. Consulte [Incluindo intervalos](/docs/services/assistant?topic=assistant-dialog-slots#dialog-slots-add) para obter mais informações.

      A imagem a seguir mostra como as digressões fora do nó #reservation com intervalos (mostrados na ilustração anterior) são configuradas.

      ![Mostra as configurações de digressões fora de um nó com intervalos.](images/digress-away-slots-full.png)

    - **Nós com intervalos**: escolha se o usuário será permitido somente digressionar fora se ele retornar para o nó atual marcando a caixa de seleção **Somente digressionar de intervalos para os nós que permitem devoluções**.

      Quando selecionado, conforme o diálogo procura um nó para responder à pergunta não relacionada do usuário, ele ignora quaisquer nós raiz que não estão configurados para retornar após a digressão. Marque essa caixa de seleção se você deseja evitar que os usuários sejam capazes de deixar permanentemente o nó antes de terem concluído o preenchimento dos intervalos necessários.

    **Digressões neste nó**

    É possível fazer as opções a seguir sobre como as digressões em um nó se comportam:

    - Evitar que os usuários possam digressionar no nó. Veja [Desativando digressões em um nó raiz](#dialog-runtime-disable-digressions) para obter mais detalhes.

    - Quando as digressões no nó são ativadas, escolha se o diálogo deve voltar para o fluxo de diálogo do qual ele é digressionado. Quando selecionado, após a ramificação do nó atual terminar de ser processada, o fluxo de diálogo volta para o nó interrompido. Para fazer o diálogo retornar depois disso, selecione **Retornar após digressão**.

    A imagem a seguir mostra como as digressões no nó #cuisine (mostrado na ilustração anterior) são configuradas.

    ![Mostra as configurações de digressões fora de um nó com intervalos.](images/digress-into-cuisine-full.png)

1.  Clique em **Aplicar**.

1.  Use a área de janela "Experimente" para testar o comportamento de digressão.

    Novamente, não é possível definir o início e o término de uma digressão. O usuário controla onde e quando as digressões acontecem. É possível aplicar somente configurações que determinam como um único nó participa de uma. Como as digressões são imprevisíveis, é difícil saber como suas decisões de configuração impactarão a conversa geral. Para realmente ver o impacto das opções feitas, deve-se testar o diálogo.

Os nós #reservation e #cuisine representam duas ramificações de diálogo que podem participar de uma única digressão direcionada ao usuário. As configurações de digressão definidas para cada nó individual são o que torna esse tipo de digressão possível no tempo de execução.

![Mostra dois diálogos, um que configura as digressões fora do nó de intervalos de reserva e um que configura a digressão no nó cuisine.](images/digression-settings.png)

### Dicas de uso de Digressão
{: #dialog-runtime-digress-tips}

Esta seção descreve soluções para situações que você pode encontrar ao usar digressões.

- **Mensagem de retorno customizada**: para quaisquer nós em que você ativar retornos de digressões fora, considere incluir texto que permita que os usuários saibam que estão retornando para o ponto em que eles pararam em um fluxo de diálogo anterior. Em sua resposta de texto, use uma sintaxe especial que permita incluir duas versões da resposta.

  Se você não tomar uma ação, a mesma resposta de texto será exibida uma segunda vez para permitir que os usuários saibam que eles retornaram para o nó do qual eles digressionaram. É possível tornar mais claro para os usuários que eles retornaram ao encadeamento de conversa original, especificando uma mensagem exclusiva para ser exibida quando eles retornarem.

  Por exemplo, se a resposta de texto original para o nó é `What's the order number?`, você pode desejar exibir uma mensagem como `Now let's get back to where we left off. What is the order number?` quando os usuários retornam para o nó.

  Para fazer isso, use a sintaxe a seguir para especificar a resposta de texto do nó:

  `<? (returning_from_digression)? "post-digression message" : "first-time message" ?>`

  Por exemplo:

  ```bash
  <? (returning_from_digression)? "Now, let's get back to where we left off.
  What is the order number?" : "What's the order number?" ?>
  ```
  {: codeblock}

  Não é possível incluir expressões SpEL ou sintaxe abreviada nas respostas de texto que você inclui. De fato, não é possível usar a sintaxe abreviada. Em vez disso, deve-se construir a mensagem concatenando as sequências de texto e a sintaxe de expressão SpEL integral para formar a resposta integral.
  {: note}
  
  Por exemplo, use a sintaxe a seguir para incluir uma variável de contexto em uma resposta de texto que você normalmente especificaria como `What can I do for you, $username?`:

  ```bash
  <? (returning_from_digression)? "Where were we, " +
  context["username"] + "? Oh right, I was asking what can I do
  for you today." : "What can I do for you today, " +
  context["username"] + "?" ?>
  ```

  Para obter detalhes da sintaxe de expressão SpEL integral, consulte [Expressão para acessar objetos](/docs/services/assistant?topic=assistant-expression-language#expression-language-shorthand-syntax).

- **Evitando retornos**: em alguns casos, você pode desejar evitar um retorno para o fluxo de conversa interrompido com base em uma opção que o usuário faz no fluxo de diálogo atual. É possível usar a sintaxe especial para evitar um retorno de um nó específico.

  Por exemplo, você pode ter um nó que condicione `#General_Connect_To_Agent` ou uma intenção semelhante. Quando acionado, se desejar obter a confirmação do usuário antes de transferi-los para um serviço externo, você poderá incluir uma resposta como `Do you want me to transfer you to an agent now?` Em seguida, será possível incluir dois nós-filhos que condicionam `#yes` e `#no`, respectivamente.
  
  A melhor maneira de gerenciar digressões para esse tipo de ramificação é configurar o nó raiz para permitir retornos de digressão. No entanto, no nó `#yes`, inclua a expressão SpEL `<? clearDialogStack() ?>` na resposta. Por exemplo:
  
    ```bash
  OK. Vou transferir você agora. <? clearDialogStack() ?>
  ```
  {: codeblock}

  Essa expressão SpEL evita que o retorno de digressão ocorra nesse nó. Quando uma confirmação for solicitada, se o usuário disser sim, a resposta adequada será exibida e o fluxo de diálogo que foi interrompido não será continuado. Se o usuário disser não, ele será retornado para o fluxo que foi interrompido.

### Desativando as digressões em um nó raiz
{: #dialog-runtime-disable-digressions}

Quando um fluxo digressiona em um nó raiz, ele segue o curso do diálogo que está configurado para esse nó. Em seguida, ele pode processar uma série de nós-filhos antes de atingir o final da ramificação de nós e, então, se configurado para fazer isso, volta para o fluxo de diálogo que foi interrompido. Por meio do teste de diálogo, você pode achar que um nó raiz é acionado com muita frequência, ou em horários inesperados, ou que seu diálogo é muito complexo e leva o usuário muito longe do curso para ser um bom candidato a uma digressão provisória. Se você determina que prefere não permitir que usuários digressionem nele, é possível configurar o nó raiz para não permitir digressões dentro dele.

Para desativar completamente as digressões em um nó raiz, conclua as etapas a seguir:

1.  Clique para abrir o nó raiz que você deseja editar.
1.  Clique em **Customizar** e, em seguida, clique na guia **Digressões**.
1.  Configure a alternância *Permitir digressões neste nó* como **Desativado**.
1.  Clique em ** Apply**.

Se você decide que deseja evitar digressões em vários nós raiz, mas não deseja editar cada um individualmente, é possível incluir os nós em uma pasta. Na página *Customizar* da pasta, é possível configurar a opção *Permitir digressões neste nó* como **Off** para aplicar a configuração a todos os nós de uma vez. Consulte [Organizando o diálogo com pastas](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-folders) para obter mais informações.

### Tutorial de digressão
{: #dialog-runtime-digression-tutorial}

Siga o [tutorial](/docs/services/assistant?topic=assistant-tutorial-digressions) para importar uma área de trabalho que tenha um conjunto de nós já definidos. É possível percorrer alguns exercícios que ilustram como as digressões funcionam.

### Considerações de design
{: #dialog-runtime-digression-design-considerations}

- **Evite a proliferação de nó de fallback**: muitos designers de diálogo incluem um nó com uma condição `true` ou `anything_else` no final de cada ramificação de diálogo como uma maneira de evitar que os usuários fiquem presos na ramificação. Esse design retorna uma mensagem genérica se a entrada do usuário não corresponde nada que você previu e incluiu um nó de diálogo específico para direcionar. No entanto, os usuários não podem digressionar fora de fluxos de diálogo que usam essa abordagem.

  Avalie as ramificações que usam essa abordagem para determinar se seria melhor permitir digressões fora da ramificação. Se a entrada do usuário não corresponde nada que você previu, ela pode localizar uma correspondência com relação a um fluxo de diálogo totalmente diferente em sua árvore. Em vez de responder com uma mensagem genérica, é possível colocar efetivamente o resto do diálogo para trabalhar para tentar direcionar a entrada do usuário. E o nó "Anything else" do nível raiz sempre pode responder à entrada que nenhum dos outros nós raiz pode direcionar.

- **Reconsidere saltos para um nó de fechamento**: muitos diálogos são projetados para fazer uma pergunta de fechamento padrão, como: `Did I answer your question today?` Os usuários não podem digressionar de nós que estão configurados para ir para outro nó. Então, se você configura todos os nós de ramificação final para ir para um nó de fechamento comum, as digressões não podem ocorrer. Considere rastrear a satisfação do usuário por meio de métricas ou algum outro meio.

- **Teste as cadeias de digressão possíveis**: se um usuário digressionar do nó atual para outro nó que permita digressões, o usuário poderá potencialmente digressionar desse outro nó e repetir esse padrão uma ou mais vezes novamente. Se o nó inicial na cadeia de digressão estiver configurado para retornar após a digressão, o usuário eventualmente será trazido de volta para o nó de diálogo atual. Na verdade, quaisquer nós subsequentes na cadeia que estiverem configurados para não retornar serão excluídos de serem considerados como destinos de digressão. Teste os cenários que digressionam múltiplas vezes para determinar se os nós individuais funcionam conforme o esperado.

- **Lembre-se de que o nó atual tem prioridade**: lembre-se de que os nós fora do fluxo atual somente serão considerados como destinos de digressão se o fluxo atual não puder direcionar a entrada do usuário. É ainda mais importante em um nó com intervalos que permite digressões fora, em particular, deixar claro aos usuários quais informações são necessárias deles e incluir instruções de confirmação que são exibidas após o usuário fornecer um valor.

  Qualquer intervalo pode ser preenchido durante o processo de preenchimento de intervalo. Portanto, um intervalo pode capturar entrada do usuário inesperadamente. Por exemplo, talvez você tenha um nó com intervalos que coleta as informações necessárias para fazer uma reserva de jantar. Um dos intervalos coleta informações de data. Ao fornecer os detalhes da reserva, o usuário pode perguntar `What's the weather meant to be tomorrow`. Você pode ter um nó raiz que condicione #forecast que pode responder ao usuário. No entanto, como a entrada do usuário inclui a palavra `tomorrow` e o nó de reserva com slots está sendo processado, seu assistente pressupõe que o usuário está fornecendo ou atualizando a data da reserva. *O nó atual sempre tem prioridade.* Se você define uma instrução de confirmação clara, como `Ok, setting the reservation date to tomorrow`, é mais provável que o usuário perceba que houve um erro de comunicação e o corrija.

  Por outro lado, enquanto preenche os intervalos, se o usuário fornece um valor que não é esperado por nenhum dos intervalos, há uma chance que ele corresponderá com relação a um nó raiz completamente não relacionado para o qual o usuário nunca pretendeu digressionar.

  Certifique-se de fazer vários testes enquanto você configura o comportamento de digressão.

- **Quando usar digressões em vez de manipuladores de intervalo**: para perguntas gerais que os usuários podem perguntar a qualquer momento, use um nó raiz que permita digressões nele, processe a entrada e, em seguida, volte para o fluxo que estava em andamento. Para os nós com intervalos, tente prever os tipos de perguntas relacionadas que os usuários podem desejar fazer ao preencher os intervalos e direcione-os incluindo manipuladores no nó.

  Por exemplo, se o nó com intervalos coleta as informações necessárias para preencher uma solicitação de seguro, você pode desejar incluir manipuladores que direcionam perguntas comuns sobre o seguro. No entanto, para perguntas sobre como obter ajuda, ou seus locais de lojas ou o histórico de sua empresa, use um nó de nível raiz.

## Corrigindo a entrada do usuário
{: #dialog-runtime-spell-check}

Ative o recurso *Autocorrection* para corrigir erros de ortografia cometidos pelos usuários nas elocuções enviadas como entrada do usuário. Quando a autocorreção está ativada, as palavras incorretas são automaticamente corrigidas. E são as palavras corrigidas que são usadas para avaliar a entrada. Quando recebe uma entrada mais precisa, seu assistente pode reconhecer com mais frequência as menções de entidade e entender a intenção do usuário.

É possível ativar essa configuração apenas para as qualificações de diálogo em inglês. Ele é ativado automaticamente para novas qualificações de diálogo no idioma inglês.
{: note}

Com a Autocorreção ativada, a entrada do usuário é corrigida da seguinte maneira:

- Entrada original: `letme applt for a memberdhip`
- Entrada corrigida: `let me apply for a membership`

Quando seu assistente avalia a necessidade de correção da ortografia de uma palavra, ele não confia em um simples processo de consulta de dicionário. Em vez disso, ele usa uma combinação de Processamento de linguagem natural e modelos probabilísticos para avaliar se um termo está, de fato, com erro de ortografia e deve ser corrigido.

### Ativando a autocorreção
{: #dialog-runtime-spell-check-enable}

Para ativar o recurso de autocorreção, conclua as etapas a seguir:

1.  Na página Qualificações, abra sua qualificação.
1.  Clique na guia **Opções**.
1.  Ligue a **Autocorreção**.

### Testando a autocorreção
{: #dialog-runtime-spell-check-test}

1.  Na área de janela "Experimente", envie uma elocução que inclua algumas palavras com erro de ortografia.

    Se as palavras da sua entrada estiverem incorretas, elas serão corrigidas automaticamente e um ícone ![auto-correct](images/auto-correct.png) será exibido. A elocução corrigida é sublinhada.
1.  Passe o mouse sobre a elocução sublinhada para ver o texto original.

Se houver termos com erros de ortografia que você esperava que seu assistente corrigisse, mas ele não os corrigiu, revise as regras usadas por ele para decidir se uma palavra deve ser corrigida para ver se ela se encaixa na categoria de palavras que ele intencionalmente não muda.

Para evitar sobrecorreção, seu assistente não corrige a ortografia dos tipos de entrada a seguir:

- Palavras em maiúsculas
- Emojis
- Entidades de localização, como estados e endereços
- Números e unidades de medida ou tempo
- Nomes próprios, como primeiros nomes comuns ou nomes de empresas
- Texto entre aspas
- Palavras contendo caracteres especiais, como hifens (-), asteriscos (*), e comercial (&) ou sinais (@), incluindo aqueles usados em endereços de e-mail ou URLs.
- Palavras que *pertencem* a essa qualificação, ou seja, palavras que têm significado implícito porque ocorrem em valores de entidades, sinônimos de entidades ou exemplos de usuários de intenção.

  As menções de uma entidade contextual podem ser corrigidas inadvertidamente. Isso porque termos que funcionam como menções de entidades contextuais são fluidos e não podem ser predeterminados e evitados pela função do verificador ortográfico da maneira como uma lista de termos baseados em dicionário pode ser. Se, após o teste, você descobrir que as menções estão sendo corrigidas incorretamente para uma determinada entidade contextual, considere o uso de uma entidade baseada em dicionário.
  {: note}

Se a palavra não corrigida não for obviamente um desses tipos de entrada, poderá ser útil verificar se a entidade tem uma correspondência difusa ativada para ela.

#### Como a autocorreção de ortografia está relacionada à correspondência difusa?
{: #dialog-runtime-spell-check-vs-fuzzy-matching}

A correspondência difusa ajuda o seu assistente a reconhecer as menções da entidade baseada em dicionário na entrada do usuário. Ela usa uma abordagem de consulta de dicionário para corresponder a uma palavra da entrada do usuário a um valor ou sinônimo de entidade existente nos dados de treinamento da qualificação. Por exemplo, se o usuário inserir `boook` e seus dados de treinamento contiverem uma entidade `@reading_material` com um valor `book`, a correspondência difusa reconhecerá que os dois termos (`book` e `book`) significam a mesma coisa.

Quando você ativa a autocorreção e a correspondência difusa, a função de correspondência difusa é executada antes do acionamento da autocorreção. Se ela encontrar um termo que possa corresponder a um valor ou sinônimo de entidade do dicionário existente, ela o incluirá na lista de palavras que *pertencem* à qualificação e não o corrige.
Por exemplo, se um usuário inserir uma sentença como `I wnt to buy a boook`, a correspondência difusa reconhecerá que o termo `boook` significa a mesma coisa que o valor `book` de sua entidade e o incluirá na lista de palavras protegidas. Seu assistente corrige a entrada para `I want to buy a boook`. Observe que ele corrige `wnt`, mas *não* corrige a ortografia de `boook`. Ao ver esse tipo de resultado durante o teste de seu diálogo, é possível que você pense que seu assistente está se comportando incorretamente. No entanto, ele não está. Graças à correspondência difusa, ele identifica corretamente `boook` como uma menção da entidade `@reading_material`. E graças à autocorreção que revisa o termo para `want`, seu assistente é capaz de mapear a entrada para sua intenção `#buy_something`. Cada recurso faz sua parte para ajudar o assistente a entender o significado da entrada do usuário.

#### Como funciona a autocorreção
{: #dialog-runtime-spell-check-how-it-works}

Normalmente, a entrada do usuário é salva no estado em que se encontra no campo `text` do objeto `input` da mensagem. Se, e somente se a entrada do usuário for corrigida de alguma forma, um novo campo será criado no objeto `input` com o nome `original_text`. Esse campo armazena a entrada original do usuário que inclui quaisquer palavras com erro de ortografia nele. E o texto corrigido é incluído no campo `input.text`.

## Desambiguação ![Somente nos planos Plus ou Premium](images/plus.png)
{: #dialog-runtime-disambiguation}

Esse recurso está disponível somente para usuários do Plus ou Premium.
{: note}

Ao ativar a desambiguação, você instrui seu assistente a solicitar ajuda aos usuários quando descobrir que mais de um nó de diálogo pode responder à entrada deles. Em vez de adivinhar qual nó processar, seu assistente compartilha uma lista das opções do nó superior com o usuário e solicita que o usuário selecione a correta.

![Mostra uma conversa de amostra entre um usuário e o assistente, em que o assistente solicita esclarecimentos do usuário.](images/disambig-demo.png)

Se ativado, a desambiguação não será acionada, a menos que as condições a seguir sejam atendidas:

- A pontuação de confiança de uma ou mais das intenções de preparação detectadas na entrada do usuário é maior que 55% da pontuação de confiança da intenção superior.
- A pontuação de confiança da intenção superior está acima de 0,2.

Mesmo quando essas condições são atendidas, a desambiguação não ocorre a menos que dois ou mais nós independentes em seu diálogo atendam aos critérios a seguir:

- A condição do nó inclui uma das intenções que acionaram a desambiguação. Ou a condição do nó é avaliada de outra forma como true. Por exemplo, se o nó verifica um tipo de entidade e a entidade é mencionada na entrada do usuário, ela é elegível.
- Há texto no campo *nome do nó externo* do nó.

Saiba mais

- [Exemplo de desambiguação](#dialog-runtime-disambig-example)
- [Ativando a desambiguação](#dialog-runtime-disambig-enable)
- [Escolhendo nós](#dialog-runtime-choose-nodes)
- [Manipulando nenhum dos acima](#dialog-runtime-handle-none)
- [Testando a desambiguação](#dialog-runtime-disambig-test)

### Exemplo de desambiguação
{: #dialog-runtime-disambig-example}

Por exemplo, você tem um diálogo que tem dois nós com condições de intenção que direcionam solicitações de cancelamento. As condições são:

- eCommerce_Cancel_Product_Order
- Customer_Care_Cancel_Account

Se a entrada do usuário for `i must cancel it today`, as intenções a seguir poderão ser detectadas na entrada:

`[`
`{"intent":"Customer_Care_Cancel_Account","confidence":0.6618281841278076},`
`{"intent":"eCommerce_Cancel_Product_Order","confidence":0.4330700159072876},`
`{"intent":"Customer_Care_Appointments","confidence":0.2902342438697815},`
`{"intent":"Customer_Care_Store_Hours","confidence":0.2550420880317688},`
`...]`

Seu assistente está `0.6618281841278076` (66%) confiante de que o objetivo do usuário corresponde à intenção `#Customer_Care_Cancel_Account`. Se qualquer outra intenção tiver uma pontuação de confiança maior que 55% de 66%, ela se ajustará aos critérios para ser uma candidata à desambiguação.

`0.66 x 0.55 = 0.36`

As intenções com uma pontuação maior que 0,36 são elegíveis.

Em nosso exemplo, a intenção `#eCommerce_Cancel_Product_Order` está acima do limite, com uma pontuação de confiança de `0.4330700159072876`.

Quando a entrada do usuário for `i must cancel it today`, ambos os nós de diálogo serão considerados candidatos viáveis para responder. Para determinar qual nó de diálogo deve ser processado, o assistente solicita que o usuário selecione um. E para ajudar o usuário a escolher entre eles, o assistente fornece um resumo curto do que cada nó faz. O texto de resumo que ele exibe é extraído diretamente das informações de *nome do nó externo* que foram especificadas para cada nó.

![O serviço solicita que o usuário selecione dentre uma lista de opções de diálogo, incluindo Cancelar uma conta, Cancelar uma ordem de produto e Nenhuma das acima.](images/disambig-tryitout.png)

Observe que seu assistente reconhece o termo `today` na entrada do usuário como uma data, ou seja, uma menção da entidade `@sys-date`. Se a árvore de diálogo contiver um nó que condicione a entidade `@sys-date`, ela também será incluída na lista de opções de desambiguação. Essa imagem mostra-a incluída na lista como a opção *Capturar informações de data*.

![O serviço solicita que o usuário escolha dentre uma lista de opções de diálogo, incluindo informações de data de captura.](images/disambig-tryitout-date.png)

O vídeo a seguir fornece uma visão geral da desambiguação.

<iframe class="embed-responsive-item" id="youtubeplayer0" title="Visão geral da desambiguação" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/VVyklAXlmbA?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

### Ativando a desambiguação
{: #dialog-runtime-disambig-enable}

Para ativar a desambiguação, conclua as etapas a seguir:

1.  Abra a aba **Opções** para a qualificação de diálogo na qual deseja ativar a desambiguação.

    Se seu aplicativo estiver hospedado em Dallas, para ativar a desambiguação, clique em **Configurações** na página **Diálogo**.
    {: note}

1.  Na seção *Desambiguação*, mude a tecla de alternância para **Ligado**.
1.  No campo **Mensagem de desambiguação**, inclua um texto a ser mostrado antes da lista de opções do nó de diálogo. Por exemplo, *O que você deseja fazer?*
1.  No campo **Mais alguma coisa**, inclua texto a ser exibido como uma opção adicional que os usuários poderão escolher se nenhuma das outras opções do nó de diálogo refletir o que eles desejam fazer. Por exemplo, *None of the acima *.

    Mantenha a mensagem curta, assim ela é exibida em sequencial com as outras opções. A mensagem deve ter menos que 512 caracteres. Para obter informações sobre o que seu assistente faz quando um usuário escolhe essa opção, consulte [Não lidando com nenhuma das opções acima](#dialog-runtime-handle-none).

1.  Se quiser limitar o número de opções de desambiguação que podem ser exibidas a um usuário, no campo **Número máximo de sugestões**, especifique um número entre 2 e 5.

    Suas mudanças são salvas automaticamente.

1.  Agora, clique na aba **Diálogo**. Revise seu diálogo para decidir em quais nós de diálogo deseja que o assistente solicite ajuda.

    - É possível selecionar nós em qualquer nível da hierarquia em árvore.
    - É possível selecionar nós que condicionam intenções, entidades, condições especiais, variáveis de contexto ou qualquer combinação desses valores.

    Consulte [Escolhendo nós](#dialog-runtime-choose-nodes) para obter dicas.

    Para cada nó que deseja disponibilizar na lista de opções de desambiguação, conclua as etapas a seguir:

    1.  Clique para abrir o nó na visualização de edição.
    1.  No campo *nome do nó externo*, descreva a tarefa do usuário que esse nó de diálogo foi projetado para manipular. Por exemplo, *Cancelar uma conta *.

        ![Mostra onde incluir as informações de nome do nó externo na visualização de edição do nó.](images/disambig-node-purpose.png)

### Escolhendo nós
{: #dialog-runtime-choose-nodes}

Escolha nós que sirvam como a raiz de uma ramificação distinta do diálogo para serem opções de desambiguação. Eles podem incluir nós que são filhos de outros nós. É importante que o nó condicione algum valor ou valor distinto que o distinga de todo o resto.

O {{site.data.keyword.conversationshort}} pode reconhecer conflitos de intenção, que ocorrem quando duas ou mais intenções têm exemplos de usuários que se sobrepõem. [Resolva tais conflitos](/docs/services/assistant?topic=assistant-intents#intents-resolve-conflicts) primeiro para garantir que as intenções em si sejam as únicas possíveis, o que ajuda seu assistente a obter melhores pontuações de confiança.
{: note}

Tenha em mente:

- Para nós que condicionam intenções, se seu assistente estiver confiante de que a condição de intenção do nó corresponde à intenção do usuário, o nó será incluído como uma opção de desambiguação.
- Para nós com condições booleanas (condições que são avaliadas como true ou false), o nó será incluído como uma opção de desambiguação se a condição for avaliada como true. Por exemplo, quando o nó condicionar um tipo de entidade, se a entidade for mencionada na entrada que aciona a desambiguação, o nó será incluído.
- A ordem de nós na hierarquia em árvore afeta a desambiguação.

  - Isso afeta se a desambiguação é acionada
  
    Consulte o [cenário](#dialog-runtime-disambig-example) que foi usado anteriormente para introduzir a desambiguação, por exemplo. Se o nó que condiciona `@sys-date` foi colocado mais alto na árvore de diálogo do que os nós que condicionam as intenções `#Customer_Care_Cancel_Account` e `#eCommerce_Cancel_Product_Order`, a desambiguação nunca será acionada quando um usuário inserir `i must cancel it today`. Isso acontece porque seu assistente consideraria a menção de data (`today`) mais importante do que as referências de intenção, devido ao posicionamento dos nós correspondentes na árvore.

  - Isso afeta quais nós são incluídos na lista de opções de desambiguação
  
    Às vezes, um nó não é listado como uma opção de desambiguação conforme o esperado. Isso poderá acontecer se um valor da condição também for referenciado por um nó que não é elegível para inclusão na lista de desambiguação por alguma razão. Por exemplo, uma menção de entidade pode acionar um nó que está situado antes na árvore de diálogo, mas não está ativado para a desambiguação. Se a mesma entidade for a única condição para um nó que *é* ativado para desambiguação, mas estiver situada mais abaixo na árvore, ela não será incluída como uma opção de desambiguação porque seu assistente nunca a alcançará. Ele realizou a correspondência com o nó anterior e ocorreu a omissão, portanto, seu assistente não processará o nó posterior.

Para cada nó que você inscreve para a desambiguação, teste os cenários nos quais você espera que o nó seja incluído na lista de opções de desambiguação. O teste fornece a você uma chance de fazer ajustes na ordem de nós ou outros fatores que podem afetar o quão bem a desambiguação funciona no tempo de execução.

### Não lidando com nenhuma das opções acima
{: #dialog-runtime-handle-none}

Quando um usuário clica na opção *Nenhuma das opções acima*, seu assistente retira as intenções reconhecidas na entrada do usuário da mensagem e as envia novamente. Essa ação geralmente aciona o nó Anything else em sua árvore de diálogo.

Para customizar a resposta retornada nessa situação, é possível incluir um nó raiz com uma condição que verifica uma entrada do usuário sem intenções reconhecidas (as intenções são removidas, lembre-se) e contém uma propriedade `suggestion_id`. Uma propriedade `suggestion_id` é incluída pelo seu assistente quando a desambiguação é acionada.
{: tip}

Inclua um nó raiz com a condição a seguir:

```json
intents.size()==0 && input.suggestion_id
```
{: codeblock}

Essa condição é atendida somente por entrada que acionou um conjunto de opções de desambiguação das quais o usuário indicou que nenhuma corresponde ao seu objetivo.

Inclua uma resposta que permita que os usuários saibam que você entende que nenhuma das opções que foi sugerida atende às suas necessidades e tome a ação apropriada.

Novamente, o posicionamento de nós na árvore importa. Se um nó que condiciona um tipo de entidade que é mencionado na entrada do usuário estiver mais alto na árvore do que esse nó, sua resposta será exibida no lugar.

### Testando a desambiguação
{: #dialog-runtime-disambig-test}

Para testar a desambiguação, conclua as etapas a seguir:

1.  Na área de janela "Experimente", insira uma elocução de teste que você acha que seja uma boa candidata para a desambiguação, o que significa que dois ou mais de seus nós de diálogo estão configurados para direcionar elocuções como ela.

1.  Se a resposta não contiver uma lista de opções de nó de diálogo para você escolher, conforme o esperado, primeiro verifique se informações de resumo foram incluídas no campo de nome do nó externo para cada um dos nós.

1.  Se a desambiguação ainda não for acionada, poderá ser que as pontuações de confiança para os nós não são tão próximas em valor quanto você pensou.

    - Para obter uma lista das intenções que atendem ao limite de desambiguação de uma determinada entrada do usuário, é possível usar a expressão SpEL a seguir na resposta de texto de um nó.

      ```json
      As intenções a seguir atendem ao limite de desambiguação: <? intents.filter("x", "x.confidence > intents[0].confidence * 0.55") ?>
      ```
      {: codeblock}

      Essa expressão utiliza a pontuação de confiança da primeira intenção na matriz de intenções reconhecidas na entrada do usuário. Em seguida, multiplica a pontuação de confiança da intenção superior por 0,55 para obter o limite de confiança que deve ser atendido por outras intenções na matriz para que a desambiguação ocorra. Por fim, filtra a matriz de intenções para incluir apenas as intenções com pontuações de confiança acima do limite.

      A matriz retornada por essa expressão fornece uma ideia de quais e quantas intenções seriam incluídas como opções de desambiguação se cada intenção fosse usada em uma condição de nó de diálogo de um nó ativado para a desambiguação. Inclua a expressão em um nó que você sabe que será acionado por sua entrada de teste.

      Para obter mais detalhes sobre o método `JSONArray.filter` usado na expressão, consulte [Métodos de linguagem de expressão](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-array-filter).

    - Para ver as pontuações de confiança de todas as intenções detectadas na entrada do usuário, inclua temporariamente `<? intents ?>` no final da resposta de nó para um nó que você sabe que será acionado.

      Essa expressão SpEL mostra as intenções que foram detectadas na entrada do usuário como uma matriz. A matriz inclui o nome da intenção e o nível de confiança que seu assistente tem de que ela reflete o objetivo pretendido pelo usuário.

    - Para ver quais entidades foram detectadas, se houver alguma, na entrada do usuário, é possível substituir temporariamente a resposta atual por uma única resposta de texto que contém a expressão SpEL `<? entities ?>`.

      Essa expressão SpEL mostra as entidades que foram detectadas na entrada do usuário como uma matriz. A matriz inclui o nome da entidade, o local da menção da entidade na sequência da entrada do usuário, a sequência da menção da entidade e o nível de confiança que seu assistente tem de que o termo é uma menção do tipo de entidade especificado.

    - Para ver detalhes de todos os artefatos ao mesmo tempo, incluindo outras propriedades, como o valor de uma determinada variável de contexto no momento da chamada, é possível inspecionar toda a resposta da API. Consulte [Visualizando detalhes da chamada da API](/docs/services/assistant?topic=assistant-dialog-tips#dialog-tips-inspect-api).

1.  Remova temporariamente a descrição incluída no campo *nome do nó externo* para pelo menos um dos nós que você prevê que serão listados como uma opção de desambiguação.

1.  Insira a elocução de teste na área de janela "Experimente" novamente.

    Se você incluiu a expressão `<? intents ?>` na resposta, o texto retornado incluirá uma lista das intenções reconhecidas pelo seu assistente na elocução de teste e a pontuação de confiança para cada uma delas.

    ![O serviço retorna uma matriz de intenções, incluindo Customer_Care_Cancel_Account e eCommerce_Cancel_Product_Order.](images/disambig-show-intents.png)

Após concluir o teste, remova quaisquer expressões SpEL conectadas às respostas do nó ou inclua de volta quaisquer respostas originais que foram substituídas por expressões e preencha novamente quaisquer campos *nome do nó externo* dos quais você removeu o texto.
