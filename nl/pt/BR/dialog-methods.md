---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# Métodos de linguagem de expressão
{: #dialog-methods}

É possível processar os valores extraídos de elocuções do usuário que você deseja referenciar em uma variável de contexto, condição ou em outro lugar na resposta.
{: shortdesc}

## Sintaxe de avaliação
{: #dialog-methods-evaluation-syntax}

Para expandir os valores de variáveis dentro de outras variáveis ou aplicar métodos para texto de saída ou variáveis de contexto, use a sintaxe de expressão `<? expression ?>`. Por exemplo:

- **Incrementando uma propriedade numérica**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **Chamando um método em um objeto**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

As seções a seguir descrevem os métodos que podem ser usados para processar valores. Eles são organizados por tipo de dados:

- [Matrizes](#dialog-methods-arrays)
- [Data e Hora](#dialog-methods-date-time)
- [Números](#dialog-methods-numbers)
- [Objetos](#dialog-methods-objects)
- [Sequências](#dialog-methods-strings)

## Matrizes
{: #dialog-methods-arrays}

Não é possível usar esses métodos para procurar um valor em uma matriz em uma condição de nó ou condição de resposta dentro do mesmo nó no qual você configura os valores de matriz.

### JSONArray.append(object)

Esse método anexa um novo valor no JSONArray e retorna o JSONArray modificado.

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

### JSONArray.clear ()

Esse método limpa todos os valores da matriz e retorna nulo.

Use a expressão a seguir na saída para definir um campo que limpa uma matriz que você salvou em uma variável de contexto ($toppings_array) de seus valores.

```json
{
  "output": {
    "array_eraser": " <? $toppings_array.clear ()? > "
  }
}
```
{: codeblock}

Se você referenciar subsequentemente a variável de contexto $toppings_array, ela retornará somente '[]'.

### JSONArray.contains (Object value)

Esse método retorna true se o JSONArray de entrada contém o valor de entrada.

Para esse Contexto de tempo de execução de diálogo que foi configurado por um nó ou aplicativo externo prévio:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Nó de diálogo ou condição de resposta:

```json
$toppings_array.contains('ham')
```
{: codeblock}

Resultado: `True` porque a matriz contém o elemento ham.

### JSONArray.containsIntent (String intent_name, Double min_score, [ Integer top_n ])
{: #dialog-methods-array-containsIntent}

Esse método retornará `true` se o JSONArray `intents` contiver particularmente a intenção especificada e essa intenção tiver uma pontuação de confiança que seja igual ou maior que a pontuação mínima especificada. Opcionalmente, é possível especificar um número para indicar que a intenção deve ser incluída dentro desse número de elementos principais na matriz.

Retornará `false` se a intenção especificada não estiver na matriz, não tiver uma pontuação de confiança que seja igual ou maior que a pontuação de confiança mínima ou a intenção for menor na matriz do que o local de índice especificado.

O serviço gera automaticamente uma matriz `intents` que lista as intenções que o serviço detecta na entrada sempre que a entrada do usuário é enviada. A matriz lista todas as intenções que são detectadas pelo serviço em ordem de maior confiança primeiro.

É possível usar esse método em uma condição do nó para não somente verificar a presença de uma intenção, mas para configurar um limite de pontuação de confiança que deve ser atendido antes que o nó possa ser processado e sua resposta retornada.

Por exemplo, use a expressão a seguir em uma condição do nó quando você desejar acionar o nó de diálogo somente quando as condições a seguir forem atendidas:

- A intenção  ` #General_Ending `  está presente.
- A pontuação de confiança da intenção `#General_Ending` é superior a 80%.
- A intenção `#General_Ending` é uma das 2 principais intenções na matriz de intenções.

```bash
intents.containsIntent (" General_Ending", 0.8, 2)
```
{: codeblock}

### JSONArray.filter (temp, "temp.property operator comparison_value")
{: #dialog-methods-array-filter}

Filtra uma matriz comparando cada valor do elemento de matriz com um valor especificado. Esse método é semelhante a uma [projeção de coleção](#collection-projection). Uma projeção de coleção retorna uma matriz filtrada com base em um nome em um par nome-valor de elemento de matriz. O método de filtro retorna uma matriz filtrada com base em um valor em um par nome-valor de elemento da matriz.

A expressão de filtro consiste nos valores a seguir:

- `temp`: nome de uma variável que é usada temporariamente conforme cada elemento de matriz é avaliado. Por exemplo, `city`.
- `property`: propriedade do elemento que você deseja comparar com o `comparison_value`. Especifique a propriedade como uma propriedade da variável provisória que você nomeia no primeiro parâmetro. Use a sintaxe:  ` temp.property `. Por exemplo, se `latitude` for um nome de elemento válido para um par nome-valor na matriz, especifique a propriedade como `city.latitude`.
- `operator`: operador a ser usado para comparar o valor da propriedade com o `comparison_value`.

    Os operadores suportados são:

    <table>
    <caption>Operadores de filtro suportados</caption>
    <tr>
      <th>Operador</th>
      <th>Descrição</th>
    </tr>
    <tr>
      <td>`==`</td>
      <td>É igual a</td>
    </tr>
    <tr>
      <td>`>`</td>
      <td>É maior que</td>
    </tr>
    <tr>
      <td>`<`</td>
      <td>É menor do que</td>
    </tr>
    <tr>
      <td>`>=`</td>
      <td>É maior ou igual a</td>
    </tr>
    <tr>
      <td>`<=`</td>
      <td>É menor do que ou igual a</td>
    </tr>
    <tr>
      <td>`!=`</td>
      <td>Não é igual a</td>
    </tr>
    </table>

- `comparison_value`: valor que você deseja comparar com cada valor da propriedade do elemento de matriz. Para especificar um valor que possa mudar dependendo da entrada do usuário, use uma variável de contexto ou entidade como o valor. Se você especificar um valor que possa variar, inclua a lógica para garantir que o valor `comparison_value` seja válido no tempo de avaliação ou um erro ocorrerá.

#### Exemplo de filtro 1

Por exemplo, é possível usar o método de filtro para avaliar uma matriz que contém um conjunto de nomes de cidades e seus números de população para retornar uma matriz menor que contém somente cidades com uma população acima de 5 milhões.

A variável de contexto `$cities` a seguir contém uma matriz de objetos. Cada objeto contém uma propriedade `name` e `population`.

```json
[
   {
      "name":"Tokyo", "population":9273000
   },
   {
      "name":"Rome",
      "population":2868104
   },
   {
      "name":"Beijing", "population":20693000
   },
   {
      "name":"Paris",
      "population":2241346
   }
]
```
{: codeblock}

No exemplo a seguir, o nome arbitrário da variável provisória é `city`. A expressão SpEL filtra a matriz `$cities` para incluir apenas as cidades com uma população superior a 5 milhões:

```bash
$cities.filter ("city", "city.demográfica > 5000000")
```
{: codeblock}

A expressão retorna a matriz filtrada a seguir:

```json
[
   {
      "name":"Tokyo", "population":9273000
   },
   {
      "name":"Beijing", "population":20693000
   }
]
```
{: codeblock}

É possível usar uma projeção de coleção para criar uma nova matriz que inclui somente os nomes de cidade da matriz retornada pelo método de filtro. Em seguida, é possível usar o método `join` para exibir os dois valores do elemento de nome da matriz como uma Sequência e separar os valores com uma vírgula e um espaço.

```bash
The cities with more than 5 million people include <?  T (String) .join ("," ,($cities.filter ("city", "city.populacionais > 5000000")).! [name])?>.
```
{: codeblock}

A resposta resultante é: `The cities with more than 5 million people include Tokyo, Beijing.`

#### Filtrar exemplo 2

O poder do método de filtro é que você não precisa codificar permanentemente o valor `comparison_value`. Neste exemplo, o valor codificado permanentemente de 5000000 é substituído por uma variável de contexto.

Neste exemplo, a variável de contexto `$population_min` contém o número `5000000`. O nome arbitrário da variável provisória é `city`. A expressão SpEL filtra a matriz `$cities` para incluir apenas as cidades com uma população superior a 5 milhões:

```bash
$cities.filter ("city", "city.populacionais > $population_min")
```
{: codeblock}

A expressão retorna a matriz filtrada a seguir:

```json
[
   {
      "name":"Tokyo", "population":9273000
   },
   {
      "name":"Beijing", "population":20693000
   }
]
```
{: codeblock}

Ao comparar valores de número, certifique-se de configurar a variável de contexto envolvida na comparação com um valor válido antes que o método de filtro seja acionado. Observe que `null` poderá ser um valor válido se o elemento de matriz com o qual você estiver comparando puder contê-lo. Por exemplo, se o par de nome e valor de população para Tóquio for `"population":null` e a expressão de comparação for `"city.population == $population_min"`, `null` será um valor válido para a variável de contexto `$population_min`.
{: tip}

É possível usar uma expressão de resposta do nó de diálogo como esta:

```bash
The cities with more than $population_min people include <?  T (String) .join ("," ,($cities.filter ("city", "city.populacionais > $population_min")).! [name])?>.
```
{: codeblock}

A resposta resultante é: `The cities with more than 5000000 people include Tokyo, Beijing.`

#### Filtrar exemplo 3

Neste exemplo, um nome de entidade é usado como o `comparison_value`. A entrada do usuário é `What is the population of Tokyo?` O nome arbitrário da variável provisória é `y`. Você criou uma entidade denominada `@city` que reconhece nomes de cidades, incluindo `Tokyo`.

```bash
$cities.filter ("y", "y.name == @city")
```

A expressão retorna a matriz a seguir:

```json
[
   {
      "name":"Tokyo", "population":9273000
   }
]
```
{: codeblock}

É possível usar um projeto de coleção para obter uma matriz com somente o elemento de população da matriz original e, em seguida, usar o método `get` para retornar o valor do elemento de população.

```bash
A população de @city é: <? ($cities.filter ("y", "y.name == @city").! [população ]) .get (0)? >.
```
{: codeblock}

A expressão retorna: `The population of Tokyo is 9273000.`

### JSONArray.get (Integer)

Esse método retorna o índice de entrada do JSONArray.

Para esse Contexto de tempo de execução de diálogo que foi configurado por um nó ou aplicativo externo prévio:

```json
{
  "context": {
    "name": "John",
    "nested": {
      "array": [ "one", "two" ]
    }
  }
}
```
{: codeblock}

Nó de diálogo ou condição de resposta:

```json
$nested.array.get(0).getAsString().contains('one')
```
{: codeblock}

Resultado:
`True` porque a matriz aninhada contém `one` como um valor.

Resposta:

```json
"output": {
  "generic":[
      {
      "values": [ {
        "text" : "The first item in the array is <?$nested.array.get(0)?>"
        }
      ], "response_type": "text", "selection_policy": "sequential" }
  ]
  }
```
{: codeblock}

### JSONArray.getRandomItem()

Esse método retorna um item aleatório do JSONArray de entrada.

Para esse Contexto de tempo de execução de diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Saída do nó de diálogo:

```json
{
  "output": {
  "generic":[
      {
      "values": [ {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
        }
      ], "response_type": "text", "selection_policy": "sequential" }
  ]
  }
}
```
{: codeblock}

Resultado: `"ham is a great choice!"` ou `"onion is a great choice!"` ou `"olives is a great choice!"`

**Nota:** O texto de saída resultante é escolhido aleatoriamente.

### JSONArray.indexOf (value)
{: #dialog-methods-array-indexOf}

Esse método retorna o número de índice do elemento na matriz que corresponde ao valor especificado como um parâmetro ou `-1` se o valor não é localizado na matriz. O valor pode ser uma Sequência (`"School"`), Número inteiro (`8`) ou Duplo (`9.1`). O valor deve ser uma correspondência exata e faz distinção entre maiúsculas e minúsculas.

Por exemplo, as variáveis de contexto a seguir contêm matrizes:

```json
{
  "context": {
    "array1": ["Mary","Lamb","School"],
    "array2": [8,9,10],
    "array3": [8.1,9.1,10.1]
  }
}
```

As expressões a seguir podem ser usadas para determinar o índice de matriz em que o valor é especificado:

```bash
<? $array1.indexOf("Mary") ?> returns `0`
<? $array2.indexOf(9) ?> returns ` 1 `
<? $array3.indexOf(10.1) ?> retorna ` 2 `
```

Esse método pode ser útil para obter o índice de um elemento em uma matriz de intenções, por exemplo. É possível aplicar o método `indexOf` à matriz de intenções gerados cada vez que a entrada do usuário é avaliada para determinar o número de índice da matriz de uma intenção específica.

```bash
intents.indexOf ("General_Greetings")
```
{: codeblock}

Se você desejar saber a pontuação de confiança para uma intenção específica, será possível passar a expressão acima como o valor *`index`* para uma expressão com a sintaxe `intents[`*`index`*`].confidence`. Por exemplo:

```bash
intents [ intents.indexOf ("General_Greetings") ] .confiança
```
{: codeblock}

### JSONArray.join (delimitador de sequência)

Esse método reúne todos os valores nessa matriz para uma sequência. Os valores são convertidos em sequência e delimitados pelo delimitador de entrada.

Para esse Contexto de tempo de execução de diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Saída do nó de diálogo:

```json
{
  "output": {
  "generic":[
      {
      "values": [ {
    "text": "This is the array: <? $toppings_array.join(';') ?>"
        }
      ], "response_type": "text", "selection_policy": "sequential" }
  ]
  }
}
```
{: codeblock}

Resultado:

```json
This is the array: onion;olives;ham;
```
{: codeblock}

Se você definir uma variável que armazena múltiplos valores em uma matriz JSON, será possível retornar um subconjunto de valores da matriz e, em seguida, usar o método join() para formatá-los adequadamente.

#### Projeção de coleta
{: #dialog-methods-collection-projection}

Uma expressão SpEL `collection projection` extrai uma subcoleção de uma matriz que contém objetos. A sintaxe para uma projeção de coleção é `array_that_contains_value_sets.![value_of_interest ] `.

Por exemplo, a variável de contexto a seguir define uma matriz JSON que armazena informações de voo. Há dois pontos de dados por voo, o horário e o código de voo.

```json
"flights_localizado": [ {
    "time": "10:00",
    "flight_code": "OK123"
  },
  {
    "time": "12:30",
    "flight_code": "LH421"
  },
  {
    "time": "16:15",
    "flight_code": "TS4156"
  }
]
```
{: codeblock}

Para retornar somente os códigos de voo, é possível criar uma expressão de projeção de coleção usando a sintaxe a seguir:

```
<? $flights_found.![flight_code] ?>
```

Essa expressão retorna uma matriz dos valores `flight_code` como `["OK123","LH421","TS4156"]`. Consulte a [Documentação de projeção de Coleção SpEL](https://docs.spring.io/spring/docs/3.0.x/reference/expressions.html) para obter mais detalhes.

Se você aplicar o método `join()` aos valores na matriz retornada, os códigos de voo serão exibidos em uma lista separada por vírgulas. Por exemplo, é possível usar a sintaxe a seguir em uma resposta:

```
The flights that fit your criteria are:
  <? T (String) .join (",", $flights_localizado.![flight_code ])? >.
```
{: codeblock}

Resultado: `The flights that match your criteria are: OK123,LH421,TS4156.`

### JSONArray.joinToArray (modelo)
{: #dialog-methods-joinToArray}

Esse método aplica o formato que você define em um modelo para a matriz e retorna uma matriz que é formatada de acordo com suas especificações. Esse método é útil para aplicar formatação a valores de matriz que você deseja retornar em uma resposta de diálogo, por exemplo.

O modelo pode ser especificado como uma Sequência, Objeto JSON ou Matriz JSON. Para referenciar valores da matriz que você está editando no modelo, siga estas convenções de sintaxe:

- `%`: representa o início ou o término de um elemento ou propriedade do elemento que você deseja retornar da matriz que está sendo editada.
- `e`: representa temporariamente o elemento de matriz ao qual você deseja aplicar a formatação. Esse nome de variável provisória não pode ser mudado de `e`.

Por exemplo, você tem uma variável de contexto que contém uma matriz com uma lista de detalhes de voo para três voos.

```json
"flights": [
      {
        "flight": "DL1040",
        "origin": "JFK",
        "carrier": "Alitalia",
        "duration": 485,
        "destination": "FCO",
        "arrival_date": "2019-02-03",
        "arrival_time": "07:00",
        "departure_date": "2019-02-02",
        "departure_time": "16:45"
      },
      {
        "flight": "DL1710",
        "origin": "JFK",
        "carrier": "Delta",
        "duration": 379,
        "destination": "LAX",
        "arrival_date": "2019-02-02",
        "arrival_time": "10:19",
        "departure_date": "2019-02-02",
        "departure_time": "07:00"
      },
      {
        "flight": "DL4379",
        "origin": "BOS",
        "carrier": "Virgin Atlantic",
        "duration": 385,
        "destination": "LHR",
        "arrival_date": "2019-02-03",
        "arrival_time": "09:05",
        "departure_date": "2019-02-02",
        "departure_time": "21:40"
      }
    ]
```
{: codeblock}

Você deseja retornar apenas a lista de códigos de voo. Para extrair somente o valor do elemento `flight` de cada matriz e retorná-lo em uma lista, é possível usar a expressão a seguir:

```
Os voos disponíveis são <? $flights.joinToArray ("%e.flight%"). ?>
```
{: codeblock}

A resposta do nó de diálogo é `The available flights are ["DL1040","DL1710","DL4379"].`

Para exibir a matriz como texto, use o método `join` na expressão como esta:

```
Os voos disponíveis são <? $flights.joinToArray ("%e.flight%") .join (","). ?>
```
{: codeblock}

A resposta é `The available flights are DL1040, DL1710, DL4379.`

#### Modelo Complex
{: #dialog-methods-complex-template}

Para criar um modelo mais complexo, em vez de especificar os detalhes do modelo no parâmetro de método diretamente, é possível criar uma variável de contexto.

Essa variável de contexto de modelo contém um subconjunto dos elementos de matriz e inclui rótulos na frente deles, portanto, as informações serão exibidas em uma lista legível na resposta:

```json
"template": "<br/>Número do voo: %e.flight% <br/> Airline: %e.carrier% <br/> Data de saída: %e.partiture_date% <br/> Tempo de saída: %e.departure_time% <br/> Hora de chegada: %e.arrival_time% <br/>"
```
{: codeblock}

A tag HTML `<br/>` para uma quebra de linha *não* é renderizada por alguns dos canais de integração, incluindo Facebook e Slack.
{: note}

Use essa expressão na resposta do nó de diálogo para aplicar o modelo definido em `$template` à matriz em `$flights`.

```
The flight info is <? $flights.joinToArray($template).join(" ") ?>
```
{: codeblock}

A resposta é semelhante a esta:

```
The flight info is
Flight number: DL1040
Airline: Alitalia
Departure date: 2019-02-02
Departure time: 16:45
Arrival time: 07:00

Flight number: DL1710
Airline: Delta
Departure date: 2019-02-02
Departure time: 07:00
Arrival time: 10:19

Flight number: DL4379
Airline: Virgin Atlantic
Departure date: 2019-02-02
Departure time: 21:40
Arrival time: 09:05
```
{: screen}

A vantagem de usar esse método é que não importa a frequência com que os valores na matriz mudam ou se o número de elementos na matriz aumenta. Contanto que cada elemento de matriz contenha pelo menos o subconjunto de propriedades que são referenciadas pelo modelo, a expressão funcionará.

#### Exemplo de modelo de objeto JSON
{: #dialog-methods-object-template}

Neste exemplo, a variável de contexto de modelo é definida como um objeto JSON que extrai o número de voo e as datas de chegada e de partida e os horários de cada um dos elementos de voo especificados na matriz na variável de contexto `$flights`. É possível usar essa abordagem para aplicar formatação padrão a detalhes de voo para voos que são gerenciados por duas transportadoras diferentes e que formatam informações de voo de forma diferente em seus serviços da web, por exemplo.

```json
"template": {
      "partida": "Vôo %e.flight%departs on %e.departure_date% at %e.departure_time %.",
      "arrival": "Flight %e.flight% arrives on %e.arrival_date% at %e.arrival_time%."
    }
```
{: codeblock}

Você pode desejar projetar seu aplicativo cliente customizado para ler os objetos da matriz retornada e formatar os valores adequadamente para a resposta de seu robô de bate-papo. Sua resposta do nó de diálogo pode retornar o objeto de detalhes de chegada de voo como uma matriz usando esta expressão:

```
<? $flights.joinToArray($template) ?>
```
{: screen}

Esta é a resposta do nó de diálogo:

```json
[
  {
    "arrival":"Flight DL1040 arrives on 2019-02-03 at 07:00.",
    "departure":"Flight DL1040 departs on 2019-02-02 at 16:45."
    },
  {
    "arrival":"Flight DL1710 arrives on 2019-02-02 at 10:19.",
    "departure":"Flight DL1710 departs on 2019-02-02 at 07:00."
    },
  {
    "arrival":"Flight DL4379 arrives on 2019-02-03 at 09:05.",
    "departure":"Flight DL4379 departs on 2019-02-02 at 21:40."
    }
  ]
  ```

Observe que a ordem dos elementos `arrival` e `departure` é trocada na resposta. O serviço geralmente reordena os elementos em um objeto JSON. Se você desejar que os elementos sejam retornados em uma ordem específica, defina o modelo usando um valor de Matriz JSON ou Sequência.

### JSONArray.remove (Integer)

Esse método remove o elemento na posição de índice do JSONArray e retorna o JSONArray atualizado.

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

### JSONArray.removeValue(object)

Esse método remove a primeira ocorrência do valor do JSONArray e retorna o JSONArray atualizado.

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

### JSONArray.set(Integer index, Object value)

Esse método configura o índice de entrada do JSONArray para o valor de entrada e retorna o JSONArray modificado.

Para esse Contexto de tempo de execução de diálogo:

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Saída do nó de diálogo:

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.set(1,'ketchup')?>"
  }
}
```
{: codeblock}

Resultado:

```json
{
  "context": {
    "toppings_array": ["onion", "ketchup", "ham"]
  }
}
```
{: codeblock}

### JSONArray.size()

Esse método retorna o tamanho do JSONArray como um número inteiro.

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
    "toppings_array_size": "<? $toppings_array.size() ?>"
  }
}
```
{: codeblock}

Resultado:

```json
{
  "context": {
    "toppings_array_size": 2
  }
}
```
{: codeblock}

### JSONArray split(String regexp)

Esse método divide a sequência de entrada usando a expressão regular de entrada. O resultado é um JSONArray de sequências.

Para essa entrada:

```
"bananas;apples;pears"
```
{: codeblock}

Essa sintaxe:

```json
{
  "context": {
    "array": "<?input.text.split(";")?>
  }
}
```
{: codeblock}

Resultados nessa saída:

```json
{
  "context": {
    "array": [ "bananas", "apples", "pears" ]
  }
}
```
{: codeblock}

### Suporte ao com.google.gson.JsonArray
{: #dialog-methods-com.google.gson.JsonArray}

Além dos métodos integrados, é possível usar os métodos padrão da classe `com.google.gson.JsonArray`.

#### Nova matriz

new JsonArray().append('value')

Para definir uma nova matriz que será preenchida com valores fornecidos pelos usuários, é possível instanciar uma matriz. Deve-se também incluir um valor do item temporário na matriz quando instanciá-la. É possível usar a sintaxe a seguir para fazer isso:

```json
{
  "context":{
    "answer": "<? output.answer?:new JsonArray().append('temp_value') ?>"
  }
```
{: codeblock}

## Data e Hora
{: #dialog-methods-date-time}

Vários métodos estão disponíveis para trabalhar com data e hora.

Para obter informações sobre como reconhecer e extrair as informações de data e hora da entrada do usuário, veja [Entidades @sys-date e @sys-time](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-date-time).

### .after (Data ou hora da sequência)

Determina se o valor de data/hora é após o argumento de data/hora.

### .before(String date or time)
Determina se o valor de data/hora é anterior ao argumento de data/hora.

Por exemplo:
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')

- Se comparar itens diferentes, como `time vs. date`, `date vs. time` e `time vs. date and time`, o método retornará falso e uma exceção será impressa no log JSON de resposta `output.log_messages`.

  Por exemplo, `@sys-date.before(@sys-time)`.
- Se comparar `date and time vs. time` o método ignorará a data e comparará apenas os horários.

### now()

Retorna uma sequência com a data e hora atuais no formato `yyyy-MM-dd HH:mm:ss`.

- Função estática.
- Os outros métodos de data/hora podem ser chamados em valores de data/hora que são retornados por essa função e ela pode ser transmitida como seus argumentos.
- Se a variável de contexto `$timezone` estiver configurada, essa função retornará datas e horas no fuso horário do cliente.

Exemplo de um nó de diálogo com `now()` usado no campo de saída:

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "generic":[
      {
        "values": [ {
          "text": "<? now() ?>"
          }
        ], "response_type": "text", "selection_policy": "sequential" }
    ]
  }
}
```
{: codeblock}

Exemplo de `now()` em condições do nó (para decidir se ainda é de manhã):

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
      "generic":[
      {
        "values": [ {
          "text": "Good morning!"
          }
        ], "response_type": "text", "selection_policy": "sequential" }
      ]
  }
}
```
{: codeblock}

### .reformatDateTime(String format)

Formata sequências de data e hora no formato desejado para a saída de usuário.

Retorna uma sequência formatada de acordo com o formato especificado:

- `MM/dd/yyyy` para 12/31/2016
- `h a` para 10pm

Para retornar o dia da semana:

- `E` para terça-feira
- `u` para o índice do dia (1 = Segunda..,, 7 = Domingo)

Por exemplo, essa definição de variável de contexto cria uma variável $time que salva o valor 17:30:00 como *17:30 PM*.

```json
{
  "context": {
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

O formato segue as regras do Java [SimpleDateFormat ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html){: new_window}.

**Nota**: ao tentar formatar somente o horário, a data é tratada como `1970-01-01`.

### .sameMoment(String date/time)

- Determina se o valor de data/hora é o mesmo que o argumento de data/hora.

### .sameOrAfter(String date/time)

- Determina se o valor de data/hora é posterior ou igual ao argumento de data/hora.
- Análogo a `.after()`.

### .sameOrBefore(String date/time)

- Determina se o valor de data/hora é anterior ou igual ao argumento de data/hora.

### hoje ()

Retorna uma sequência com a data atual no formato `yyyy-MM-dd`.

- Função estática.
- Os outros métodos de data podem ser chamados em valores de data que são retornados por essa função e podem ser passados como seus argumentos.
- Se a variável de contexto `$timezone` estiver configurada, essa função retornará datas no fuso horário do cliente. Caso contrário, o fuso horário `GMT` será usado.

Exemplo de um nó de diálogo com `today()` usado no campo de saída:

```json
{
  "condições": "#what_day_is_it", "output": {
    "generic":[
      {
        "values": [ {
          "text": "Today's date is <? today() ?>."
          }
        ], "response_type": "text", "selection_policy": "sequential" }
    ]
  }
}
```
{: codeblock}

Resultado:  ` Hoje a data é 2018-03-09. `

## Cálculos de data e hora
{: #dialog-methods-calculations}

Use os métodos a seguir para calcular uma data.

| Método                  | Descrição |
|-------------------------|-------------|
| `<date>.minusDays(n)`   | Retorna a data do dia n número de dias antes da data especificada. |
| `<date>.minusMonths(n)` | Retorna a data do dia n número de meses antes da data especificada. |
| `<date>.minusYears(n)`  | Retorna a data do dia n número de anos antes da data especificada. |
| `<date>.plusDays(n)`   | Retorna a data do dia n número de dias após a data especificada. |
| `<date>.plusMonths(n)` | Retorna a data do dia n número de meses após a data especificada. |
| `<date>.plusYears(n)`  | Retorna a data do dia n número de anos após a data especificada. |

em que `<date>` é especificado no formato `yyyy-MM-dd` ou `yyyy-MM-dd HH:mm:ss`.

Para obter a data de amanhã, especifique a expressão a seguir:

```json
{
  "output": {
    "generic":[
      {
        "values": [ {
          "text": " Tomorrow's date is <? today().plusDays(1) ?>."
          }
        ], "response_type": "text", "selection_policy": "sequential" }
    ]
  }
}
```
{: codeblock}

Resultado se hoje for 9 de março de 2018: `Tomorrow's date is 2018-03-10.`

Para obter a data para o dia de uma semana a partir de hoje, especifique a expressão a seguir:

```json
{
  "output": {
    "generic":[
      {
        "values": [ {
          "text": " A data da próxima semana é <? @sys-date.plusDays(7) ?>."
          }
        ], "response_type": "text", "selection_policy": "sequential" }
    ]
  }
}
```
{: codeblock}

Resultado se a data capturada pela entidade @sys-date for a data de hoje, dia 9 de março de 2018: `Next week's date is 2018-03-16.`

Para obter a data do mês passado, especifique a expressão a seguir:

```json
{
  "output": {
    "generic":[
      {
        "values": [ {
          "text": " No mês passado a data foi <? today().minusMonths(1) ?>."
          }
        ], "response_type": "text", "selection_policy": "sequential" }
    ]
  }
}
```
{: codeblock}

Resultado se hoje for 9 de março de 2018: `Last month the date was 2018-02-9.`

Use os métodos a seguir para calcular o horário.

| Método                  | Descrição |
|-------------------------|-------------|
| `<time>.minusHours(n)`   | Retorna o horário n horas antes do horário especificado. |
| `<time>.minusMinutes(n)` | Retorna o horário n minutos antes do horário especificado. |
| `<time>.minusSeconds(n)`  | Retorna o horário n segundos antes do horário especificado. |
| `<time>.plusHours(n)`   | Retorna o horário n horas após o horário especificado. |
| `<time>.plusMinutes(n)` | Retorna o horário n minutos após o horário especificado. |
| `<time>.plusSeconds(n)`  | Retorna o horário n segundos após o horário especificado. |

em que `<time>` é especificado no formato `HH:mm:ss`.

Para obter o horário daqui a uma hora, especifique a expressão a seguir:

```json
{
  "output": {
    "generic":[
      {
        "values": [ {
          "text": " Uma hora a partir de agora é <? now().plusHours(1) ?>."
          }
        ], "response_type": "text", "selection_policy": "sequential" }
    ]
  }
}
```
{: codeblock}

Resultado se for 8h: `One hour from now is 09:00:00.`

Para obter o horário 30 minutos atrás, especifique a expressão a seguir:

```json
{
  "output": {
    "generic":[
      {
        "values": [ {
          "text": "A half hour before @sys-time is <? @sys-time.minusMinutes(30) ?>."
          }
        ], "response_type": "text", "selection_policy": "sequential" }
    ]
  }
}
```
{: codeblock}

Resultado se o horário capturado pela entidade @sys-time for 8h: `A half hour before 08:00:00 is 07:30:00.`

Para reformatação do horário que é retornado, é possível usar a expressão a seguir:

```json
{
  "output": {
    "generic":[
      {
        "values": [ {
          "text": " 6 horas atrás era <? now().minusHours(6).reformatDateTime('h:mm a') ?>."
          }
        ], "response_type": "text", "selection_policy": "sequential" }
    ]
  }
}
```
{: codeblock}

Resultado se for 14h19: `6 hours ago was 8:19 AM.`

### Trabalhando com períodos de tempo
{: #dialog-methods-time-spans}

Para mostrar uma resposta com base em se a data de hoje cai dentro de um determinado prazo, é possível usar uma combinação de métodos relacionados a horário. Por exemplo, se você executar uma oferta especial durante a temporada de férias todos os anos, será possível verificar se a data de hoje cai entre 25 de novembro e 24 de dezembro do presente ano. Primeiro, defina as datas de interesse como variáveis de contexto.

Nas expressões de variável de contexto de data de início e encerramento a seguir, a data está sendo construída concatenando o valor do ano atual derivado dinamicamente com os valores de mês e de dia codificados permanentemente.

```json
"context": {
   "end_date": " <? now ().reformatDateTime ('Y') + '-12-24'? > ", "start_date": " <? now ().reformatDateTime ('Y') + '-11-25'? > "
 }
```

Na condição de resposta, será possível indicar que você deseja mostrar a resposta somente se a data atual cair entre as datas de início e encerramento definidas como variáveis de contexto.

`now().after($start_date) && now().before($end_date)`

### Suporte ao java.util.Date
{: #dialog-methods-java.util.Date}

Além dos métodos integrados, é possível usar os métodos padrão da classe `java.util.Date`.

Para obter a data do dia que cai uma semana a partir de hoje, é possível usar a sintaxe a seguir.

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() + (7 * (24*60*60*1000L))) ?>"
  }
}
```
{: codeblock}

Essa expressão obtém primeiro a data atual em milissegundos (desde 1 de janeiro de 1970, 0h GMT). Ela também calcula o número de milissegundos em 7 dias. (O `(24*60*60*1000L)` representa um dia em milissegundos.) Em seguida, ele inclui 7 dias na data atual. O resultado é a data integral do dia que cai uma semana a partir de hoje. Por exemplo, `Fri Jan 26 16:30:37 UTC 2018`. Observe que o horário está no fuso horário UTC. É possível sempre mudar o 7 para uma variável (`$number_of_days`, por exemplo) que você pode passar. Apenas certifique-se de que seu valor seja configurado antes dessa expressão ser avaliada.

Se você deseja ser capaz de comparar subsequentemente a data com outra data que é gerada pelo serviço, deve-se reformatar a data. As entidades do sistema (`@sys-date`) e outros métodos integrados (`now()`) convertem as datas para o formato `yyyy-MM-dd`.

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() + (7 * (24*60*60*1000L))).format('yyyy-MM-dd') ?>"
  }
}
```
{: codeblock}

Depois de reformatar a data, o resultado será `2018-01-26`. Agora, é possível usar uma expressão como `@sys-date.after($week_from_today)` em uma condição de resposta para comparar uma data especificada na entrada do usuário com a data salva na variável de contexto.

A expressão a seguir calcula o horário daqui a 3 horas.

```json
{
  "context": {
    "future_time": "<? new Date(new Date().getTime() + (3 * (60*60*1000L)) -
      (5 * (60*60*1000L))).format('h:mm a') ?>"
  }
}
```
{: codeblock}

O valor `(60*60*1000L)` representa uma hora em milissegundos. Essa expressão inclui 3 horas no horário atual. Em seguida, recalcula o horário de um fuso horário UTC para o fuso horário EST subtraindo 5 horas dele. Também reformata os valores de data para incluir horas e minutos AM ou PM.

## Números
{: #dialog-methods-numbers}

Esses métodos ajudam a obter e formatar valores de número.

Para obter informações sobre entidades do sistema que podem reconhecer e extrair números da entrada do usuário, veja [@sys-number entity](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-number).

Se desejar que o serviço reconheça formatos numéricos específicos na entrada do usuário, como referências de números de ordem, pense em criar uma entidade padrão para capturá-los. Veja [Criando entidades](/docs/services/assistant?topic=assistant-entities) para obter mais detalhes.

Se você desejar mudar o posicionamento decimal para um número, para reformatar um número como um valor de moeda, por exemplo, consulte o [Método String format()](#dialog-methods-java.lang.String).

### toDouble()

  Converte o objeto ou campo no tipo de número Duplo. Será possível chamar esse método em qualquer objeto ou campo. Se a conversão falhar, *null* será retornado.

### toInt()

  Converte o objeto ou campo no tipo de número Inteiro. Será possível chamar esse método em qualquer objeto ou campo. Se a conversão falhar, *null* será retornado.

### toLong()

  Converte o objeto ou campo no tipo de número Longo. Será possível chamar esse método em qualquer objeto ou campo. Se a conversão falhar, *null* será retornado.

  Se você especifica um tipo de número Longo em uma expressão SpEL, deve-se anexar um `L` ao número para identificá-lo como tal. Por exemplo, `5000000000L`. Essa sintaxe é necessária para quaisquer números que não se ajustam ao tipo de Número inteiro de 32 bits. Por exemplo, números que são maiores que 2^31 (2.147.483.648) ou menores que -2^31 (-2.147.483.648) são considerados tipos de número Longo. Os tipos de número Longo têm um valor mínimo de -2^63 e um valor máximo de 2^63-1.

### Suporte a números do Java
{: #dialog-methods-java.lang.Number}

### java.lang.Math()

Executa operações numéricas básicas.

É possível usar os métodos de Classe, incluindo estes:

- max()

```json
{
  "context": {
    "bigger_number": "<? T(Math).max($number1,$number2) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [ {
          "text": "The bigger number is $bigger_number."
          }
        ], "response_type": "text", "selection_policy": "sequential" }
    ]
  }
}
```
{: codeblock}

- min()

```json
{
  "context": {
    "smaller_number": "<? T(Math).min($number1,$number2) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [ {
          "text": "The smaller number is $smaller_number."
          }
        ], "response_type": "text", "selection_policy": "sequential" }
    ]
  }
}
```
{: codeblock}

- pow()

```json
{
  "context": {
    "power_of_two": "<? T(Math).pow($base.toDouble(),2.toDouble()) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [ {
          "text": "Your number $base to the second power is $power_of_two."
          }
        ], "response_type": "text", "selection_policy": "sequential" }
    ]
  }
}
```
{: codeblock}

Veja a [documentação de referência do java.lang.Math](https://docs.oracle.com/javase/7/docs/api/java/lang/Math.html) para obter informações sobre outros métodos.

### java.util.Random()

Retorna um número aleatório. É possível usar uma das opções de sintaxe a seguir:

- Para retornar um valor booleano aleatório (true ou false), use `<?new Random().nextBoolean()?>`.
- Para retornar um número duplo aleatório entre 0 (incluído) e 1 (excluído), use `<?new Random().nextDouble()?>`
- Para retornar um número inteiro aleatório entre 0 (incluído) e um número que você especificar, use `<?new Random().nextInt(n)?>` em que n é o topo do intervalo de números que deseja + 1.
  Por exemplo, se você deseja retornar um número aleatório entre 0 e 10, especifique `<?new Random().nextInt(11)?>`.
- Para retornar um número inteiro aleatório do intervalo de valores de número inteiro completo (-2147483648 a 2147483648), use `<?new Random().nextInt()?>`.

Por exemplo, você pode criar um nó de diálogo que é acionado pela intenção #random_number. A primeira condição de resposta pode ser semelhante a esta:

```json
Condition = @sys-number
{
  "context": {
    "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [ {
          "text": "Here's a random number between 0 and @sys-number.literal: $answer."
          }
        ], "response_type": "text", "selection_policy": "sequential" }
    ]
  }
}
```
{: codeblock}

Veja a [documentação de referência do java.util.Random](https://docs.oracle.com/javase/7/docs/api/java/util/Random.html) para obter informações sobre outros métodos.

Também é possível usar métodos padrão das classes a seguir:

- `java.lang.Byte`
- `java.lang.Integer`
- `java.lang.Long`
- `java.lang.Double`
- `java.lang.Short`
- `java.lang.Float`

## Objetos
{: #dialog-methods-objects}

### JSONObject.clear ()

Esse método limpa todos os valores do objeto JSON e retorna nulo.

Por exemplo, você deseja limpar os valores atuais da variável de contexto $user.

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

Use a expressão a seguir na saída para definir um campo que limpa o objeto de seus valores.

```json
{
  "output": {
    "object_eraser": " <? $user.clear ()? > "
  }
}
```
{: codeblock}

Se você referenciar subsequentemente a variável de contexto $user, ela retornará somente `{}`.

É possível usar o método `clear()` nos objetos JSON `context` ou `output` no corpo da chamada `/message` da API .

#### Limpando contexto
{: #dialog-methods-clearing-context}

Quando você usa o método `clear()` para limpar o objeto `context`, ele limpa **todas** as variáveis, exceto estas:

 - ` context.conversation_id `
 - ` context.timezone `
 - ` context.system `

** Aviso **: todos os valores de variáveis de contexto significam:

  - Todos os valores padrão que foram configurados para variáveis em nós que foram acionados durante a sessão atual.
  - Quaisquer atualizações feitas nos valores padrão com informações fornecidas pelo usuário ou serviços externos durante a sessão atual.

Para usar o método, é possível especificá-lo em uma expressão em uma variável que você define no objeto de saída. Por exemplo:

```json
{
  "output": {
    "generic":[
      {
        "values": [ {
          "text": "Response for this node."
          }
        ], "response_type": "text", "selection_policy": "sequential" }
    ],
    "context_eraser": "<? context.clear ()? > "
  }
}

```

#### Limpando a saída
{: #dialog-methods-clearing-output}

Quando você o método `clear ()` para limpar o objeto `output`, ele limpa todas as variáveis, exceto aquela usada para limpar o objeto de saída e quaisquer respostas de texto definidas no nó atual. Ele também não limpa estas variáveis:

- `output.nodes_visited`
- ` output.nodes_visited_details `

Para usar o método, é possível especificá-lo em uma expressão em uma variável que você define no objeto de saída. Por exemplo:

```json
{
  "output": {
    "generic":[
      {
        "values": [ {
          "text": "Have a great day!"
          }
        ], "response_type": "text", "selection_policy": "sequential" }
    ],
    "output_eraser": "<? output.clear ()? > "
  }
}
```

Se um nó anterior na árvore definir uma resposta de texto de `I'm happy to help.` e, em seguida, ir para um nó com o objeto de saída JSON definido acima, somente `Have a great day.` será exibido como a resposta. A saída `I'm happy to help.` não é exibida, porque ela é limpa e substituída pela resposta de texto do nó que está chamando o método `clear()`.

### JSONObject.tem (Sequência)

Esse método retorna true se o JSONObject complexo possui uma propriedade do nome de entrada.

Para esse Contexto de tempo de execução de diálogo:

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

Saída do nó de diálogo:

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

Resultado: a condição é true porque o objeto de usuário contém a propriedade `first_name`.

### JSONObject.remove (Sequência)

Esse método remove uma propriedade do nome da entrada `JSONObject`. O `JSONElement` que é retornado por esse método é o `JSONElement` que está sendo removido.

Para esse Contexto de tempo de execução de diálogo:

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

Saída do nó de diálogo:

```json
{
  "context": {
    "attribute_removed": "<? $user.remove('first_name') ?>"
  }
}
```
{: codeblock}

Resultado:

```json
{
  "context": {
    "user": {
      "last_name": "Snow"
    },
    "attribute_removed": {
      "first_name": "John"
    }
  }
}
```
{: codeblock}

### Suporte ao com.google.gson.JsonObject
{: #dialog-methods-com.google.gson.JsonObject}

Além dos métodos integrados, é possível usar métodos padrão da classe `com.google.gson.JsonObject`.

## Sequências
{: #dialog-methods-strings}

Existem métodos que ajudam a trabalhar com texto.

Para obter informações sobre como reconhecer e extrair determinados tipos de Sequências, como nomes de pessoas e locais, da entrada do usuário, veja [Entidades do sistema](/docs/services/assistant?topic=assistant-system-entities).

**Nota:** para métodos que envolvem expressões regulares, veja [Referência da Sintaxe RE2 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/google/re2/wiki/Syntax){: new_window} para obter detalhes sobre a sintaxe a ser usada ao especificar a expressão regular.

### String.append (Object)

Esse método anexa um objeto de entrada à sequência como uma sequência e retorna uma sequência modificada.

Para esse Contexto de tempo de execução de diálogo:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

Essa sintaxe:

```json
{
  "context": {
    "my_text": "<? $my_text.append(' More text.') ?>"
  }
}
```
{: codeblock}

Resultados nessa saída:

```json
{
  "context": {
    "my_text": "This is a text. More text."
  }
}
```
{: codeblock}

### String.contains (String)

Esse método retorna true se a sequência contém a subsequência de entrada.

Entrada: "Yes, I'd like to go."

Essa sintaxe:

```json
{
  "conditions": "input.text.contains('Yes')"
}
```
{: codeblock}

Resultados: a condição é `true`.

### String.endsWith (String)

Esse método retorna true se a sequência termina com a subsequência de entrada.

Para essa entrada:

```
"What is your name?".
```
{: codeblock}

Essa sintaxe:

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{: codeblock}

Resultados: a condição é `true`.

### String.extract(String regexp, Integer groupIndex)

Esse método retorna uma sequência extraída pelo índice de grupo especificado da expressão regular de entrada.

Para essa entrada:

```
"Hello 123456".
```
{: codeblock}

Essa sintaxe:

```json
{
  "context": {
    "number_extract": "<? input.text.extract('[\\d]+',0) ?>"
  }
}
```
{: codeblock}

  **Importante:** para processar `\\d` como a expressão regular, você precisa escapar as barras invertidas incluindo outro `\\`: `\\\\d`

Resultado:

```json
{
  "context": {
    "number_extract": "123456"
  }
}
```
{: codeblock}

### String.localizar (String regexp)

Esse método retorna true se qualquer segmento da sequência corresponde à expressão regular de entrada.  É possível chamar esse método em um elemento JSONArray ou JSONObject, e ele converterá a matriz ou o objeto em uma sequência antes de fazer a comparação.

Para essa entrada:

```
"Hello 123456".
```
{: codeblock}

Essa sintaxe:

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

Resultado: a condição é true porque a parte numérica do texto de entrada corresponde à expressão regular `^[^\d]*[\d]{6}[^\d]*$`.

### String.isEmpty()

Esse método retorna true se a sequência é uma sequência vazia, mas não nula.

Para esse Contexto de tempo de execução de diálogo:

```json
{
  "context": {
    "my_text_variable": ""
  }
}
```
{: codeblock}

Essa sintaxe:

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{: codeblock}

Resultados: a condição é `true`.

### String.length()

Esse método retorna o comprimento de caracteres da sequência.

Para essa entrada:

```
"Hello"
```
{: codeblock}

Essa sintaxe:

```json
{
  "context": {
    "input_length": "<? input.text.length() ?>"
  }
}
```
{: codeblock}

Resultados nessa saída:

```json
{
  "context": {
    "input_length": 5
  }
}
```
{: codeblock}

### String.matches (String regexp)

Esse método retorna true se a sequência corresponde à expressão regular de entrada.

Para essa entrada:

```
"Hello".
```
{: codeblock}

Essa sintaxe:

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{: codeblock}

Resultado: a condição é true porque o texto de entrada corresponde à expressão regular `\^Hello\$`.

### String.startsWith(String)

Esse método retorna true se a sequência inicia com a subsequência de entrada.

Para essa entrada:

```
"What is your name?".
```
{: codeblock}

Essa sintaxe:

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{: codeblock}

Resultados: a condição é `true`.

### String.substring( Integer beginIndex, Integer endIndex)

Esse método obtém uma subsequência com o caractere em `beginIndex` e o último caractere configurado como índice antes de `endIndex`.
O caractere endIndex não está incluído.

Para esse Contexto de tempo de execução de diálogo:

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

Essa sintaxe:

```json
{
  "context": {
    "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{: codeblock}

Resultados nessa saída:

```json
{
  "context": {
    "my_text": "is a text."
  }
}
```
{: codeblock}

### String.toLowerCase()

Esse método retorna a Sequência original convertida em letras minúsculas.

Para essa entrada:

```
"This is A DOG!"
```
{: codeblock}

Essa sintaxe:

```json
{
  "context": {
    "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{: codeblock}

Resultados nessa saída:

```json
{
  "context": {
    "input_upper_case": "this is a dog!"
  }
}
```
{: codeblock}

### String.toUpperCase()

Esse método retorna a Sequência original convertida em letras maiúsculas.

Para essa entrada:

```
"hi there".
```
{: codeblock}

Essa sintaxe:

```json
{
  "context": {
    "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{: codeblock}

Resultados nessa saída:

```json
{
  "context": {
    "input_upper_case": "HI THERE"
  }
}
```
{: codeblock}

### String.trim()

Esse método reduz quaisquer espaços no início e no final da sequência e retorna a sequência modificada.

Para esse Contexto de tempo de execução de diálogo:

```json
{
  "context": {
    "my_text": "   something is here    "
  }
}
```
{: codeblock}

Essa sintaxe:

```json
{
  "context": {
    "my_text": "<? $my_text.trim() ?>"
  }
}
```
{: codeblock}

Resultados nessa saída:

```json
{
  "context": {
    "my_text": "something is here"
  }
}
```
{: codeblock}

### Suporte ao java.lang.String
{: #java.lang.String}

Além dos métodos integrados, é possível usar métodos padrão da classe `java.lang.String`.

#### java.lang.String.format()

É possível aplicar o método de Sequência Java padrão `format()` ao texto. Veja [Referência java.util.formatter ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter#syntax){: new_window} para obter informações sobre a sintaxe a ser usada para especificar os detalhes de formato.

Por exemplo, a expressão a seguir toma três números inteiros decimais (1, 1 e 2) e os inclui em uma sentença.

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d equals %d', 1, 1, 2) ?>"
}
```
{: codeblock}

Resultado: `1 + 1 equals 2`.

Para mudar o posicionamento decimal para um número, use a sintaxe a seguir:

```
{
  <? T(String).format('%.2f',<number to format>) ?>
}
```
{: codeblock}

Por exemplo, se a variável $number que precisa ser formatada em dólares dos EUA for `4.5`, uma resposta, como `Your total is $<? T(String).format('%.2f',$number) ?>` retornará `Your total is $4.50.`

## Conversão indireta do tipo de dado
{: #dialog-methods-indirect-type-conversion}

Ao incluir uma expressão em texto, como parte de uma resposta do nó, por exemplo, o valor é renderizado como uma Sequência. Se você deseja que a expressão seja renderizada em seu tipo de dados original, não a demarque com texto.

Por exemplo, é possível incluir essa expressão em uma resposta do nó de diálogo para retornar as entidades que são reconhecidas na entrada do usuário no formato de Sequência:

```json
  The entities are <? entities ?>.
```
{: codeblock}

Se o usuário especifica *Hello now* como a entrada, as entidades @sys-date e @sys-time são acionadas pela referência `now`. O objeto de entidades é uma matriz, mas como a expressão está incluída em texto, as entidades são retornadas em formato de Sequência, como este:

```json
  The entities are 2018-02-02, 14:34:56.
```
{: codeblock}

Se você não inclui texto na resposta, uma matriz é retornada no lugar. Por exemplo, se a resposta é especificada somente como uma expressão, não circundada por texto.

```
  <? entities ?>
```
{: codeblock}

As informações de entidade são retornadas em seu tipo de dados nativos, como uma matriz.

```json
[
  {
    "entity":"sys-date","location":[6,9],"value":"2018-02-02","confidence":1,"metadata":{"calendar_type":"GREGORIAN","timezone":"America/New_York"}
  },
  {
    "entity":"sys-time","location":[6,9],"value":"14:33:22","confidence":1,"metadata":{"calendar_type":"GREGORIAN","timezone":"America/New_York"}
  }
  ]
```
{: codeblock}

Como outro exemplo, a variável de contexto $array a seguir é uma matriz, mas a variável de contexto $string_array é uma sequência.

```json
{
  "context": {
    "array": [
      "one",
      "two"
    ],
    "array_in_string": "this is my array: $array"
  }
}
```
{: codeblock}

Se você verificar os valores dessas variáveis de contexto na área de janela Experimente, verá seus valores especificados conforme a seguir:

**$array** : `["one","two"]`

**$array_in_string** : `"this is my array: [\"one\",\"two\"]"`

É possível executar métodos de matriz subsequentemente na variável $array, tal como `<? $array.removeValue('two') ?>`, mas não a variável $array_in_string.
