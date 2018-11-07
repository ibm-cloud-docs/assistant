---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-05"

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

# Métodos de linguagem de expressão

É possível processar os valores extraídos de elocuções do usuário que você deseja referenciar em uma variável de contexto, condição ou em outro lugar na resposta.
{: shortdesc}

## Sintaxe de avaliação

Para expandir os valores de variáveis dentro de outras variáveis ou aplicar métodos para texto de saída ou variáveis de contexto, use a sintaxe de expressão `<? expression ?>`. Por exemplo:

- **Incrementando uma propriedade numérica**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **Chamando um método em um objeto**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

As seções a seguir descrevem os métodos que podem ser usados para processar valores. Eles são organizados por tipo de dados:

- [Matrizes](dialog-methods.html#arrays)
- [Data e Hora](dialog-methods.html#date-time)
- [Números](dialog-methods.html#numbers)
- [Objetos](dialog-methods.html#objects)
- [Sequências](dialog-methods.html#strings)

## Matrizes
{: #arrays}

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

### JSONArray.contains(object value)

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

### JSONArray.get(integer)

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
    "text": {
      "values": [
        "The first item in the array is <?$nested.array.get(0)?>"
      ],
      "selection_policy": "sequential"
    }
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
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
  }
}
```
{: codeblock}

Resultado: `"ham is a great choice!"` ou `"onion is a great choice!"` ou `"olives is a great choice!"`

**Nota:** O texto de saída resultante é escolhido aleatoriamente.

### JSONArray.join(string delimiter)

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
    "text": "This is the array: <? $toppings_array.join(';') ?>"
  }
}
```
{: codeblock}

Resultado:

```json
This is the array: onion;olives;ham;
```
{: codeblock}

### JSONArray.remove(integer)

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

### JSONArray.set(integer index, object value)

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
{: #com.google.gson.JsonArray}

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
{: #date-time}

Vários métodos estão disponíveis para trabalhar com data e hora.

Para obter informações sobre como reconhecer e extrair as informações de data e hora da entrada do usuário, veja [Entidades @sys-date e @sys-time](system-entities.html#sys-datetime).

### .after(String date/time)

- Determina se o valor de data/hora é após o argumento de data/hora.
- Análogo a `.before()`.

### .before(String date or time)

- Por exemplo:
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')
- Determina se o valor de data/hora é anterior ao argumento de data/hora.
- Se comparar itens diferentes, como `time vs. date`, `date vs. time` e `time vs. date and time`, o método retornará falso e uma exceção será impressa no log JSON de resposta `output.log_messages`.
    - Por exemplo, `@sys-date.before(@sys-time)`.
- Se comparar `date and time vs. time` o método ignorará a data e comparará apenas os horários.

### now()

- Função estática.
- Retorna uma sequência com a data e hora atuais no formato `yyyy-MM-dd HH:mm:ss`.
- Os outros métodos de data/hora podem ser chamados em valores de data/hora que são retornados por essa função e ela pode ser transmitida como seus argumentos.
- Se a variável de contexto `$timezone` estiver configurada, essa função retornará datas e horas no fuso horário do cliente.

Exemplo de um nó de diálogo com `now()` usado no campo de saída:

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "text": "<? now() ?>"
   }
}
```
{: codeblock}

Exemplo de `now()` em condições do nó (para decidir se ainda é de manhã):

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
    "text": "Good morning!"
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

### Suporte ao java.util.Date
{: #java.util.Date}

Além dos métodos integrados, é possível usar os métodos padrão da classe `java.util.Date`.

#### Cálculos de data

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
{: #numbers}

Esses métodos ajudam a obter e formatar valores de número.

Para obter informações sobre entidades do sistema que podem reconhecer e extrair números da entrada do usuário, veja [@sys-number entity](system-entities.html#sys-number).

Se desejar que o serviço reconheça formatos numéricos específicos na entrada do usuário, como referências de números de ordem, pense em criar uma entidade padrão para capturá-los. Veja [Criando entidades](entities.html#creating-entities) para obter mais detalhes.

### toDouble()

  Converte o objeto ou campo no tipo de número Duplo. Será possível chamar esse método em qualquer objeto ou campo. Se a conversão falhar, *null* será retornado.

### toInt()

  Converte o objeto ou campo no tipo de número Inteiro. Será possível chamar esse método em qualquer objeto ou campo. Se a conversão falhar, *null* será retornado.

### toLong()

  Converte o objeto ou campo no tipo de número Longo. Será possível chamar esse método em qualquer objeto ou campo. Se a conversão falhar, *null* será retornado.

  Se você especifica um tipo de número Longo em uma expressão SpEL, deve-se anexar um `L` ao número para identificá-lo como tal. Por exemplo, `5000000000L`. Essa sintaxe é necessária para quaisquer números que não se ajustam ao tipo de Número inteiro de 32 bits. Por exemplo, números que são maiores que 2^31 (2.147.483.648) ou menores que -2^31 (-2.147.483.648) são considerados tipos de número Longo. Os tipos de número Longo têm um valor mínimo de -2^63 e um valor máximo de 2^63-1.

### Suporte a números do Java
{: #java.lang.Number}

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
    "text": {
      "values": [
        "The bigger number is $bigger_number."
      ],
      "selection_policy": "sequential"
    }
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
    "text": {
      "values": [
        "The smaller number is $smaller_number."
      ],
      "selection_policy": "sequential"
    }
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
    "text": {
      "values": [
        "Your number $base to the second power is $power_of_two."
      ],
      "selection_policy": "sequential"
    }
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
    "text": {
      "values": [
        "Here's a random number between 0 and @sys-number.literal: $answer."
      ],
      "selection_policy": "sequential"
    }
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
{: #objects}

### JSONObject.has(string)

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

### JSONObject.remove(string)

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
{: #com.google.gson.JsonObject}

Além dos métodos integrados, é possível usar métodos padrão da classe `com.google.gson.JsonObject`.

## Sequências
{: #strings}

Existem métodos que ajudam a trabalhar com texto.

Para obter informações sobre como reconhecer e extrair determinados tipos de Sequências, como nomes de pessoas e locais, da entrada do usuário, veja [Entidades do sistema](system-entities.html).

**Nota:** para métodos que envolvem expressões regulares, veja [Referência da Sintaxe RE2 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/google/re2/wiki/Syntax){: new_window} para obter detalhes sobre a sintaxe a ser usada ao especificar a expressão regular.

### String.append(object)

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

### String.contains(string)

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

### String.endsWith(string)

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

### String.find(string regexp)

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

### String.matches(string regexp)

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

### String.startsWith(string)

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

### String.substring(int beginIndex, int endIndex)

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

É possível aplicar o método de Sequência Java padrão `format()` ao texto. Veja [Referência java.util.formatter ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter.html#syntax){: new_window} para obter informações sobre a sintaxe a ser usada para especificar os detalhes de formato.

Por exemplo, a expressão a seguir toma três números inteiros decimais (1, 1 e 2) e os inclui em uma sentença.

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d equals %d', 1, 1, 2) ?>"
}
```
{: codeblock}

Resultado: `1 + 1 equals 2`.

## Conversão indireta do tipo de dado

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
