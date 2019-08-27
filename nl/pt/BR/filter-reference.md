---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-29"

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

# Referência de consulta de filtro
{: #filter-reference}

A API de REST do serviço {{site.data.keyword.conversationshort}} oferece recursos de procura de log poderosos através de consultas de filtro. É possível usar o parâmetro `filter` da API /logs para procurar o log de qualificação para eventos que correspondem a uma consulta especificada.

O parâmetro `filter` é uma consulta em cache que limita os resultados àqueles que correspondem ao filtro especificado. É possível filtrar diversos objetos que fazem parte do modelo de resposta JSON (por exemplo, o texto de entrada do usuário, as intenções e as entidades detectadas ou a pontuação de confiança).

Para ver exemplos de consultas de filtro, consulte [Exemplos](#filter-reference-examples).

Para obter informações adicionais sobre o método /logs `GET` e seu modelo de resposta, consulte a [Referência da API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/apidocs/assistant?curl=#list-log-events-in-a-workspace){: new_window}.

## Sintaxe de consulta de filtro
{: #filter-reference-syntax}

O exemplo a seguir mostra o formulário geral de uma consulta de filtro:

| Localização           | Operador de Consulta | Termo         |
|:------------------:|:--------------:|:------------:|
|`request.input.text`|`::`            |`"IBM Watson"`|

- A _localização_ identifica o campo que você deseja filtrar (neste exemplo, `request.input.text`).
- O _operador de consulta_, que especifica o tipo de correspondência que você deseja usar (correspondência difusa ou correspondência exata).
- O _termo_ especifica a expressão ou valor que você deseja usar para avaliar o campo para correspondência. O termo pode conter texto literal e operadores, conforme descrito na [próxima seção](#filter-reference-operators).

A filtragem por intenção ou entidade requer sintaxe um pouco diferente da filtragem em outros campos. Para obter informações adicionais, consulte [Filtragem por intenção ou entidade](#filter-reference-intent-entity).

**Nota:** A sintaxe da consulta de filtro usa alguns caracteres que não são permitidos em consultas HTTP. Certifique-se de que todos os caracteres especiais, incluindo espaços e aspas, sejam codificados por URL quando enviados como parte de uma consulta HTTP. Por exemplo, o filtro `response_timestamp<2016-11-01` seria especificado como `response_timestamp%3C2016-11-01`.

## Operadores
{: #filter-reference-operators}

É possível usar os operadores a seguir em sua consulta de filtro.

| Operador | Descrição |
|:-------------------:|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `:` | Operador de consulta de correspondência difusa. Prefixe o termo de consulta com `:` se você deseja corresponder qualquer valor que contém o termo de consulta ou uma variante gramatical do termo de consulta. Correspondência difusa está disponível para texto de entrada do usuário, texto de saída de resposta e valores de entidade. |
| `::` | Operador de consulta de correspondência exata. Prefixe o termo de consulta com `::` se você deseja corresponder apenas os valores que são exatamente iguais ao termo de consulta. |
| `:!` | Operador de consulta de correspondência difusa negativa. Prefixe o termo de consulta com `:!` se você deseja corresponder apenas os valores que _não_ contêm o termo de consulta ou uma variante gramatical do termo de consulta. |
| `::!` | Operador de consulta de correspondência exata negativa. Prefixe o termo de consulta com `::!` se você deseja corresponder apenas valores que _não_ correspondem exatamente ao termo de consulta. |
| `<=`, `>=`, `>`, `<` | Operadores de comparação. Prefixe o termo de consulta com esses operadores para que correspondam com base na comparação aritmética. |
| `\` | Operador de escape. Use em consultas que incluem caracteres de controle que caso contrário seriam analisados como operadores. Por exemplo, `\! hello` corresponderia ao texto `! hello `. |
| `""` | Frase literal. Use para incluir um termo de consulta que contém espaços ou outros caracteres especiais. Nenhum caractere especial dentro das aspas é analisado pela API. |
| `~` | Correspondência aproximada. Anexe esse operador seguido por `1` ou `2` para o fim do termo de consulta para especificar o número permitido de diferenças de caractere único entre a sequência de consulta e uma correspondência no objeto de resposta. Por exemplo, `car~1` corresponderia `car`, `cat` ou `cars`, mas não corresponderia `cats`. Este operador não é válido na filtragem de `log_id` ou qualquer campo de data ou hora ou com correspondência difusa. |
| `*` | Operador curinga. Corresponde qualquer sequência de zero ou mais caracteres. Este operador não é válido na filtragem de `log_id` ou qualquer campo de data ou hora. |
| `()`, `[]` | Operadores de agrupamento. Use para incluir um agrupamento lógico de várias expressões usando operadores booleanos. |
| `|` | Operador _or_ booleano. |
| `,` | Operador _and_ booleano. |

### Filtrando por intenção ou entidade
{: #filter-reference-intent-entity}

Devido a diferenças em como as intenções e entidades são armazenadas internamente, a sintaxe para filtragem em uma entidade ou intenção específica é diferente da sintaxe usada para outros campos no JSON retornado. Para especificar um campo `intent` ou `entity` dentro de uma coleção de `intents` ou `entities`, deve-se usar o operador de correspondência `:` em vez de um ponto.

Por exemplo, esta consulta corresponde a qualquer evento registrado no qual a resposta inclui uma intenção detectada denominada `hello`:

`response.intents:intent::hello`

Observe o operador `:` no lugar de um ponto (intents:intent)

Use o mesmo padrão para corresponder em qualquer campo de uma intenção ou entidade detectada na resposta. Por exemplo, essa consulta corresponde a qualquer evento registrado em que a resposta inclui uma entidade detectada com o valor `soda`:

`response.entities:value::soda`

Da mesma forma, é possível filtrar intenções ou entidades enviadas como parte da solicitação, como neste exemplo:

`request.intents:intent::hello`

A filtragem em intenções opera em todas as intenções detectadas. Para filtrar somente a intenção detectada com a confiança mais alta, é possível usar a sintaxe abreviada `response.top_intent`. Por exemplo:

`response.top_intent::goodbye`

### Filtrando por ID de cliente
{: #filter-reference-customer-id}

Para filtrar por ID de cliente, use o local especial `customer_id`. (Para obter mais informações sobre como rotular mensagens com um ID de cliente, consulte [Segurança de informações](/docs/services/assistant?topic=assistant-information-security)).

### Filtrando por outros campos
{: #filter-reference-fields}

Para filtrar outro campo nos dados do log, especifique o local como um caminho que identifica os níveis de objetos aninhados na resposta JSON da API /logs. Use pontos (`.`) para especificar níveis sucessivos de aninhamento nos dados JSON. Por exemplo, o local `request.input.text` identifica o campo de texto de entrada do usuário, conforme mostrado no fragmento JSON a seguir:

```json
  "request": {
    "input": {
      "text": "Good morning"
    }
  }
```
<!-- {data-copy=false} -->

A filtragem não está disponível para todos os campos. É possível filtrar os campos a seguir:

- request.context.metadata.deployment
- request.input.text
- response.entities
- response.input.text
- response.intents
- response.top_intent
- meta.message.entities_count

A filtragem de outros campos não é suportada atualmente.

## Exemplos
{: #filter-reference-examples}

Os exemplos a seguir ilustram vários tipos de consultas usando esta sintaxe.

| Descrição | Consulta |
|---------|-----------|
| A data da resposta é no mês de julho de 2017. | `response_timestamp>=2017-07-01,response_timestamp<2017-08-01` |
| O registro de data e hora da resposta é anterior a `2016-11-01T04:00:00.000Z`. | `response_timestamp<2016-11-01T04:00:00.000Z` |
| A mensagem é rotulada com o ID de cliente `my_id`. | `customer_id::my_id` |
| O texto de entrada do usuário contém a palavra "order" ou uma variante gramatical (por exemplo, `orders` ou `ordering`. | `request.input.text:order` |
| Um nome de intenção na resposta corresponde exatamente `place_order`. | `response.intents:intent::place_order` |
| Um nome de entidade na resposta corresponde exatamente `beverage`.  | `response.entities:entity::beverage` |
| O texto de entrada do usuário não contém a palavra "order" ou uma variante gramatical. | `request.input.text:!order` |
| O nome da intenção detectada com a confiança mais alta não corresponde exatamente a `hello`. | `response.top_intent::!hello` |
| O texto de entrada do usuário contém a sequência `! hello`. | `request.input.text:\!hello` |
| O texto de entrada do usuário contém a sequência `IBM Watson`. | `request.input.text:"IBM Watson"` |
| O texto de entrada do usuário contém uma sequência que não possui mais de 2 diferenças de caractere único de `Watson`. | `request.input.text:Watson~2` |
| O texto de entrada do usuário contém uma sequência que consiste de `comm`, seguido por zero ou mais caracteres adicionais, seguido por `s`. | `request.input.text:comm*s` |
| O ID de implementação no contexto corresponde a `my_app`. | `request.context.metadata.deployment::my_app` |
| Uma intenção na resposta tem um valor de confiança maior que 0,8. | `response.intents:confidence>0.8` |
| Um nome de intenção na resposta corresponde exatamente a `hello` ou `goodbye`. | ` response.intents:intenção :: (hello|goodbye)` |
| Uma intenção na resposta possui o nome `hello` e um valor de confiança igual ou maior que 0,8. | `response.intents:(intent:hello,confidence>=0.8)` |
| Um nome de intenção na resposta corresponde exatamente a `order` e um nome de entidade na resposta corresponde exatamente a `beverage`. | `[response.intents:intent::order,response.entities:entity::beverage]` |
<!-- -->
