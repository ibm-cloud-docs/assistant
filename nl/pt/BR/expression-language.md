---

copyright:
  years: 2015, 2019
lastupdated: "2019-06-04"

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

# Expressões para acessar objetos
{: #expression-language}

É possível escrever expressões que acessam objetos e propriedades de objetos usando a linguagem Spring Expression (SpEL). Para obter mais informações, veja [Spring Expression Language (SpEL) ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}.
{: shortdesc}

## Sintaxe de avaliação
{: #expression-language-long-syntax}

Para expandir valores de variáveis dentro de outras variáveis ou chamar métodos em propriedades e objetos globais, use a sintaxe de expressão `<? expression ?>`. Por exemplo:

- **Expandindo uma propriedade**
    - `"output":{"text":"Your name is <? context.userName ?>"}`

- **Chamando métodos em propriedades de objetos globais**
    - `"context":{"email": "<? @email.literal ?>"}`

## Sintaxe de abreviação
{: #expression-language-shorthand-syntax}

Saiba como referenciar rapidamente os objetos a seguir usando a sintaxe abreviada de SpEL:

- [Variáveis de contexto](#expression-language-shorthand-context)
- [Entidades ](#expression-language-shorthand-entities)
- [Intents](#expression-language-shorthand-intents)

### Sintaxe abreviada para variáveis de contexto
{: #expression-language-shorthand-context}

A tabela a seguir mostra exemplos da sintaxe abreviada que você pode utilizar para gravar variáveis de contexto em expressões de condição.

| Sintaxe de abreviação           | Sintaxe completa em SpEL                     |
|----------------------------|-----------------------------------------|
| `$card_type`               | `context['card_type']`                  |
| `$(card-type)`             | `context['card-type']`                  |
| `$card_type:VISA`          | `context['card_type'] == 'VISA'`        |
| `$card_type:(MASTER CARD)` | `context['card_type'] == 'MASTER CARD'` |

É possível incluir caracteres especiais como hifens ou pontos em nomes de variável de contexto. No entanto, fazer isso pode levar a problemas quando a expressão SpEL é avaliada. O hífen poderia ser interpretado como um sinal de menos, por exemplo. Para evitar tais problemas, referencie a variável usando a sintaxe de expressão completa ou a sintaxe abreviada `$(variable-name)` e não use os caracteres especiais a seguir no nome:

- Parênteses ()
- Mais de um apóstrofo ''
- Aspas "

### Sintaxe abreviada para entidades
{: #expression-language-shorthand-entities}

A tabela a seguir mostra exemplos de sintaxe abreviada que podem ser usados ao se referir a entidades.

| Sintaxe de abreviação    | Sintaxe completa em SpEL                      |
|---------------------|------------------------------------------|
| `@year`             | `entities['year']?.value`                |
| `@year == 2016`     | `entities['year']?.value == 2016`        |
| `@year != 2016`     | `entities['year']?.value != 2016`        |
| `@city == 'Boston'` | `entities['city']?.value == 'Boston'`    |
| `@city:Boston`      | `entities['city']?.contains('Boston')`   |
| `@city:(New York)`  | `entities['city']?.contains('New York')` |

Em SpEL, o ponto de interrogação `(?)` evita que uma exceção de ponteiro nulo seja acionada quando um objeto de entidade for nulo.

Se o valor da entidade que você deseja verificar contiver um caractere `)`, não será possível usar o operador `:` para comparação.  Por exemplo, se desejar verificar se a entidade city é `Dublin (Ohio)`, você deverá usar `@city == 'Dublin (Ohio)'`
em vez de `@city:(Dublin (Ohio))`.

### Sintaxe abreviada para intenções
{: #expression-language-shorthand-intents}

A tabela a seguir mostra exemplos de sintaxe abreviada que você pode usar ao se referir a intenções.

<table>
  <caption>Sintaxe de Intents Shorthand</caption>
  <tr>
    <th>Sintaxe de abreviação</th>
    <th>Sintaxe integral em SpEL</th>
  </tr>
  <tr>
    <td>`#help`</td>
    <td>`intent == 'help'`</td>
  </tr>
  <tr>
    <td>`! #help`</td>
    <td>`intent != 'help'`</td>
  </tr>
  <tr>
    <td>`NOT #help`</td>
    <td>`intent != 'help'`</td>
  </tr>
  <tr>
    <td>`#help` or `#i_am_lost`</td>
    <td>`(intent == 'help' || intent == 'I_am_lost')`</td>
  </tr>
</table>

## Variáveis globais integradas
{: #expression-language-builtin-vars}

É possível usar a linguagem de expressão para extrair informações de propriedade para as variáveis globais a seguir:

| Variável global      | Definição |
|----------------------|------------|
| *context*            | Parte de objeto JSON da mensagem de conversa processada. |
| *entities[ ]*        | Lista de entidades que suportam acesso padrão ao primeiro elemento. |
| *input*              | Parte de objeto JSON da mensagem de conversa processada. |
| *intents[ ]*         | Lista de intenções que suportam acesso padrão ao primeiro elemento. |
| *output*             | Parte de objeto JSON da mensagem de conversa processada. |

## Acessando entidades
{: #expression-language-access-entity}

A matriz de entidades contém uma ou mais entidades que foram reconhecidas na entrada do usuário.

Ao testar seu diálogo, é possível ver detalhes das entidades que são reconhecidas na entrada do usuário especificando essa expressão em uma resposta do nó de diálogo:

```json
<? entities ?>
```
{: codeblock}

Para a entrada do usuário, *Hello now*, seu assistente reconhece as entidades do sistema @sys-date e @sys-time, portanto, a resposta contém esses objetos de entidade:

```json
[
{"entity":"sys-date","location":[6,9],"value":"2017-08-07",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}},
{"entity":"sys-time","location":[6,9],"value":"15:01:00",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}}
]
```
{: codeblock}

### Quando o posicionamento de entidades na entrada importa
{: #expression-language-placement-matters}

Quando você usar a expressão abreviada `@city.contains('Boston')` em uma condição, o nó de diálogo retornará true **somente se** `Boston` for a primeira entidade detectada na entrada do usuário. Use essa sintaxe somente se o posicionamento de entidades na entrada importar e você desejar verificar somente a primeira menção.

Use a expressão SpEL integral se você desejar que a condição retorne true sempre que o termo for mencionado na entrada do usuário, independentemente da ordem na qual as entidades são mencionadas. A condição `entities['city']?.contains('Boston')` retorna verdadeiro quando pelo menos uma entidade city 'Boston' é encontrada em todas as entidades @city, independentemente do posicionamento.

Por exemplo, um usuário envia `"I want to go from Toronto to Boston."`. As entidades `@city:Toronto` e `@city:Boston` são detectadas e são representadas na matriz que é retornada, conforme a seguir:

- `entities.city[0].value = 'Toronto'`
- `entities.city[1].value = 'Boston'`

A ordem das entidades na matriz que é retornada corresponde à ordem na qual elas são mencionadas na entrada do usuário.
{: note}

### Propriedades da entidade
{: #expression-language-entity-props}

Cada entidade possui um conjunto de propriedades associadas a ela. É possível acessar informações sobre uma entidade através de suas propriedades.

| Propriedade              | Definição | Dicas de Uso |
|-----------------------|------------|------------|
| *confiança*          | Uma porcentagem decimal que representa a confiança de seu assistente na entidade reconhecida. A confiança de uma entidade é 0 ou 1, a menos que você tenha ativado a correspondência difusa de entidades. Quando a correspondência difusa está ativada, o limite de nível de confiança padrão é 0,3. Independentemente de a correspondência difusa estar ativada ou não, as entidades do sistema sempre terão um nível de confiança 1,0. | Será possível usar essa propriedade em uma condição para que ela retorne falso se o nível de confiança não for maior que um percentual especificado. |
| *localização*            | Um deslocamento de caractere baseado em zero que indica onde os valores de entidade detectados começam e terminam no texto de entrada. | Use `.literal` para extrair o período de texto entre os valores de índice iniciais e finais que estão armazenados na propriedade localização. |
| *valor*               | O valor da entidade identificado na entrada. | Essa propriedade retorna o valor da entidade conforme definido nos dados de treinamento, mesmo se a correspondência foi feita contra um de seus sinônimos associados. É possível usar `.values` para capturar várias ocorrências de uma entidade que podem estar presentes na entrada do usuário. |

### Exemplos de uso da propriedade da entidade
{: #expression-language-entity-props-example}

Nos exemplos a seguir, a qualificação contém uma entidade de aeroporto que inclui um valor de JFK e o sinônimo "Kennedy Airport". A entrada do usuário é *Eu quero ir para o aeroporto Kennedy*.

- Para retornar uma resposta específica se a entidade 'JFK' for reconhecida na entrada do usuário, você poderia incluir essa expressão para a condição de resposta: `entities.airport[0].value == 'JFK'` ou `@airport = "JFK"`
- Para retornar o nome da entidade como especificado pelo usuário na resposta do diálogo, use a propriedade .literal:
  `So you want to go to <?entities.airport[0].literal?>...`
  ou
  `So you want to go to @airport.literal ...`

  Ambos os formatos são avaliados para `So you want to go to Kennedy Airport...' na resposta.

- Expressões como `@airport:(JFK)` ou `@airport.contains('JFK')` sempre referem-se ao **valor** da entidade (`JFK` neste exemplo).
- Para ser mais restritivo sobre quais termos são identificados como aeroportos na entrada quando a correspondência difusa estiver ativada, é possível especificar essa expressão em uma condição de nó, por exemplo: `@airport && @airport.confidence > 0.7`. O nó será executado apenas se o seu assistente estiver 70% confiante de que o texto de entrada contém uma referência de aeroporto.

Nesse exemplo, a entrada do usuário é *Há lugares para troca de moeda no JFK, Logan e O'Hare?*

- Para capturar diversas ocorrências de um tipo de entidade na entrada do usuário, use uma sintaxe como esta:

    ```json
    "context":{
      "airports":"@airport.values"
    }
    ```

  Para consultar posteriormente a lista capturada em uma resposta de diálogo, use esta sintaxe:
  `You asked about these airports: <? $airports.join(', ') ?>.`
  Ela é exibida assim:
`You asked about these airports: JFK, Logan, O'Hare.`

## Acessando intenções
{: #expression-language-intent}

A matriz de intenções contém uma ou mais intenções que foram reconhecidas na entrada do usuário, classificadas em ordem decrescente de confiança.

Cada intenção tem somente uma propriedade: a propriedade `confidence`. A propriedade de confiança é uma porcentagem decimal que representa a confiança de seu assistente na intenção reconhecida.

Ao testar seu diálogo, é possível ver detalhes das intenções que são reconhecidas na entrada do usuário, especificando essa expressão em uma resposta do nó diálogo:

```json
<? intents ?>
```
{: codeblock}

Para a entrada do usuário, *Hello now*, seu assistente localiza uma correspondência exata com a intenção #greeting. Portanto, ele lista os detalhes do objeto de intenção #greeting primeiro. A resposta também inclui as outras 10 principais intenções que estão definidas na qualificação, independentemente da sua pontuação de confiança. (Neste exemplo, sua confiança nas outras intenções é configurada para 0 porque a primeira intenção é uma correspondência exata.) As 10 principais intenções são retornadas porque a área de janela "Experimente" envia o parâmetro `alternate_intents:true` com sua solicitação. Se você está usando a API diretamente e deseja ver os 10 resultados principais, certifique-se de especificar esse parâmetro em sua chamada. Se `alternate_intents` é false, que é o valor padrão, somente intenções com uma confiança acima de 0,2 são retornadas na matriz.

```json
[{"intent":"greeting","confidence":1},
{"intent":"yes","confidence":0},
{"intent":"pizza-order","confidence":0}]
```
{: codeblock}

Os exemplos a seguir mostram como verificar um valor de intenção:

- `intents[0] == 'Help'`
- `intent == 'Help'`

`intent == 'help` difere de `intents[0] == 'help'` porque `intent == 'help'` não lança uma exceção se nenhuma intenção for detectada. Ela é avaliada como verdadeira somente se a confiança na intenção exceder um limite.  Se você quiser, será possível especificar um nível de confiança customizado para uma condição, por exemplo, `intents.size() > 0 && intents[0] == 'help' && intents[0].confidence > 0.1`

## Acessando a entrada
{: #expression-language-intent-props}

O objeto JSON de entrada contém uma única propriedade: a propriedade de texto. A propriedade de texto representa o texto da entrada do usuário.

### Exemplos de uso da propriedade de entrada
{: #expression-language-intent-props-example}

O exemplo a seguir mostra como acessar a entrada:

- Para executar um nó se a entrada do usuário for "Sim", inclua essa expressão para a condição nó:
`input.text == 'Yes'`

É possível usar qualquer [Método de sequência](/docs/services/assistant/dialog-methods#dialog-methods-strings) para avaliar ou manipular texto da entrada do usuário. Por exemplo:

- Para verificar se a entrada do usuário contém "Yes", use: `input.text.contains( 'Yes' )`.
- Retorna verdadeiro se a entrada do usuário for um número: `input.text.matches( '[0-9]+' )`.
- Para verificar se a cadeia de entrada contém dez caracteres, use: `input.text.length() == 10`.
