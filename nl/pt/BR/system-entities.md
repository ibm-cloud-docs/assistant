---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-24"

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

# Detalhes da entidade do sistema

Essa seção de referência fornece informações completas sobre as entidades do sistema disponíveis. Para obter mais informações sobre entidades do sistema e como usá-las, consulte [Definindo entidades](entities.html#enable_system_entities) e procure por "Ativando entidades do sistema".
{: shortdesc}

As entidades do sistema estão disponíveis para idiomas anotados no tópico [Idiomas suportados](lang-support.html).

## Entidade @sys-currency
{: #sys-currency}

A entidade do sistema @sys-currency detecta valores monetários que são expressos em uma elocução com um símbolo monetário ou termos específicos da moeda. Um valor numérico é retornado.

### Formatos reconhecidos

- 20 centavos
- Cinco dólares
- $10

### Metadados

- `.numeric_value`: o valor numérico canônico como um número inteiro ou um duplo, em unidades base
- `.unit`: o código de moeda de unidade base (por exemplo, 'USD' ou 'EUR')

### Retorna

Para a entrada `twenty dollars` ou `$ 1,234.56`, @sys-currency retorna esses valores:

| Atributo                   | Tipo   | Retornado para `twenty dollars` | Retornado para `$ 1,234.56` |
|-----------------------------|--------|-------------------------------|-------------------------:|
| @sys-currency               | sequência | 20                            |                  1234,56 |
| @sys-currency.literal       | sequência | vinte dólares                |                $ 1.234,56 |
| @sys-currency.numeric_value | número | 20                            |                  1234,56 |
| @sys-currency.location      | matriz  | [0,14]                        |                    [0,9] |
| @sys-currency.unit          | sequência | USD*                          |                      USD |

*@sys-currency.unit sempre retorna o código de moeda ISO de 3 letras.

Para a entrada `veinte euro` ou <code>&euro;1,234.56</code>, em espanhol, @sys-currency retorna esses valores:

| Atributo                   | Tipo   | Retornado para `veinte euro` | Retornado para <code>&euro; 1,234.56</code> |
|-----------------------------|--------|-----------------------------|-------------------------:|
| @sys-currency               | sequência | 20                          |                  1234,56 |
| @sys-currency.literal       | sequência | veinte euro                 |                &euro; 1.234,56 |
| @sys-currency.numeric_value | número | 20                          |                  1234,56 |
| @sys-currency.location      | matriz  |[0,11]                       |                     [0,9]|
| @sys-currency.unit          | sequência | EUR*                        |                     EUR  |
*@sys-currency.unit sempre retorna o código de moeda ISO de 3 letras.

Você obtém resultados equivalentes para outros idiomas e moedas nacionais suportados.

### Dicas de uso de @system-currency

- Os valores de moeda também são reconhecidos como instâncias de entidades @sys-number. Se você estiver usando condições separadas para verificar os valores de moeda e os números, coloque a condição que verifica a moeda acima daquela que verifica um número.

- Se você usar a entidade @sys-currency como uma condição de nó e o usuário especificar `$0` como o valor, o valor será reconhecido como uma moeda corretamente, mas a condição será avaliada para o número zero, não a moeda zero. Como resultado, ela não retorna a resposta esperada. Para procurar valores de moeda de uma maneira que manipule zeros corretamente, use a sintaxe `entities['sys-currency']` na condição do nó.

## Entidades @sys-date e @sys-time
{: #sys-datetime}

A entidade do sistema `@sys-date` extrai menções como `Friday`, `today` ou `November 1`. O valor dessa entidade armazena a data inferida correspondente como uma sequência no formato "aaaa-MM-dd", por exemplo, "2016-11-21". O sistema aumenta elementos ausentes de uma data (como o ano para "21 de novembro") com os valores de data atuais.

**Nota:** - apenas para o idioma inglês, o comportamento do sistema padrão para a entrada de data é MM/DD/AAAA. Isso mudará para DD/MM/AAAA apenas se os primeiros dois números forem maiores que 12. O valor armazenado ainda estará no formato "aaaa-MM-dd".

A entidade do sistema `@sys-time` extrai menções como `2pm`, `at 4` ou `15:30`. O valor dessa entidade armazena a hora como uma sequência no formato "HH :mm:ss". Por exemplo, "13:00:00."

### Menções de data/hora

Menções de data e hora, como `now` ou `two hours from now` são extraídas como duas menções de entidade separadas - uma `@sys-date` e uma `@sys-time`. Essas duas menções não estão vinculadas uma com a outra, exceto que elas compartilham a mesma sequência literal que estende a menção de data/hora completa.

Menções com várias palavras, data e hora como `on Monday at 4pm` também são extraídas como duas menções @sys-date e @sys-time. Quando mencionadas juntas consecutivamente, elas também compartilham uma única sequência literal que abrange a menção de data/hora completa.

### Intervalos de data e hora

Menções de um intervalo de data, como `the weekend`, `next week` ou `from Monday to Friday` são extraídas como um par de menções de entidade `@sys-date` que mostram o início e o término do intervalo. Da mesma forma, menções de intervalos de tempo como `from 2 to 3` são extraídos como duas entidades `@sys-time`, mostrando os horários de início e de encerramento. As duas entidades no par compartilham uma sequência literal que corresponde à menção do intervalo de data ou hora integral.

### `Last` e `Next` datas e horas

Em alguns códigos de idioma, uma frase como "na segunda-feira passada" é usada para especificar a segunda-feira da semana anterior apenas. Em contraste, outros códigos de idioma usar "na segunda-feira passada" para especificar o último dia que foi uma segunda-feira, mas que pode ter sido na mesma semana ou na semana anterior.

Como um exemplo, para sexta-feira, 16 de junho, em alguns códigos de idioma "na segunda-feira passada" pode referir-se a 12 de junho ou a 5 de junho, enquanto que em outros códigos de idioma se refere apenas a 5 de junho (a semana anterior). Essa mesma lógica vale para uma frase como "na próxima segunda-feira".

O serviço {{site.data.keyword.conversationshort}} trata "última" e "próxima" datas como referência ao último ou próximo dia mais imediato referenciado, que pode estar na mesma semana ou em uma semana anterior.

Para frases de tempo como "nos últimos 3 dias" ou "nas próximas 4 horas", a lógica é equivalente. Por exemplo, no caso de "nas próximas 4 horas", isso resulta em duas entidades `@sys-time`: uma do horário atual e uma das quatro horas posteriores à hora atual.

### Fusos horários

Menções de uma data ou hora que são relativas ao horário atual são resolvidas com relação a um fuso horário escolhido. Por padrão, este é UTC (GMT). Isso significa que, por padrão, os clientes da API de REST localizados em fusos horários diferentes de UTC observarão o valor de `now` extraído de acordo com o horário UTC atual.

Opcionalmente, o cliente da API de REST pode incluir o fuso horário local como a variável de contexto `$timezone`. Essa variável de contexto deve ser enviada com cada solicitação do cliente. Por exemplo, o valor `$timezone` deve ser `America/Los_Angeles`, `EST` ou `UTC`. Para obter uma lista completa de fusos horários suportados, consulte [Fusos horários suportados](supported-timezones.html).

Quando a variável `$timezone` é fornecida, os valores das menções @sys-date e @sys-time relativas são calculadas com base no fuso horário do cliente em vez de UTC.

#### Exemplos de menções relativas aos fusos horários

- agora
- em duas horas
- hoje
- amanhã
- 2 dias a partir de agora

### Formatos reconhecidos

- 21 de novembro
- 10h30
- às 18h
- nesse fim de semana

### Retorna

Para a entrada `November 21` @sys-date retorna esses valores:

| Atributo               | Tipo   | Retornado para `November 21` |
|-------------------------|--------|---------------------------:|
| @sys-date.literal       | sequência |                21 de novembro |
| @sys-date               | sequência |                20xx-11-21 *|
| @sys-date.location      | matriz |                     [0,11]  |
| @sys-date.calendar_type | sequência |                  GREGORIANO |

- @sys-date sempre retorna a data neste formato: aaaa-MM-dd.
- \* Retorna a próxima data de correspondência. Se essa data já passou este ano, retorna a data do ano seguinte.

Para a entrada `at 6 pm` @sys-time retorna esses valores:

| Atributo               | Tipo   | Retornado para `at 6 pm` |
|-------------------------|--------|-----------------------:|
| @sys-time.literal       | sequência |                at 6 pm |
| @sys-time               | sequência |               18:00:00 |
| @sys-time.location      | matriz |                   [0,7]|
| @sys-time.calendar_type | sequência |              GREGORIANO |

- @sys-time sempre retorna o horário neste formato: HH:mm:ss.

Para obter informações sobre o processamento de valores de data e hora, consulte a referência do método [Data e hora](dialog-methods.html#date-time).
{: tip}

## Entidade @sys-location
{: #sys-location}

**BETA, para idiomas observados no tópico [Idiomas Suportados](lang-support.html)**: a entidade do sistema @sys-location extrai nomes de locais (país, estado/município, cidade, etc). da entrada dos usuários. O valor da entidade não é um valor padrão do sistema para o local.

### Formatos reconhecidos

- Boston
- EUA
- Nova Gales do Sul

Para obter informações sobre o processamento de valores de sequência, veja a referência de método de [Sequências](dialog-methods.html#strings).
{: tip}

## Entidade @sys-number
{: #sys-number}

A entidade do sistema @sys-number detecta números que são gravados usando numerais ou palavras. Seja qual for o caso, um valor numérico será retornado.

### Formatos reconhecidos

- 21
- vinte e um
- 3,13

### Metadados

- `.numeric_value` - o valor numérico canônico como um número inteiro ou um duplo

### Retorna

Para a entrada `twenty` ou `1,234.56`, @sys-number retorna esses valores:

| Atributo                   | Tipo   | Retornado para `twenty` | Retornado para `1,234.56` |
|-----------------------------|--------|-------------------|------------------------:|
| @sys-number               | sequência | 20                |                 1234,56 |
| @sys-number.literal       | sequência | vinte            |                1,234.56 |
| @sys-number.location      | matriz |  [0,6]             |                    [0,8]|
| @sys-number.numeric_value | número | 20                |                 1234,56 |

Para a entrada `veinte` ou `1.234,56`, em espanhol, @sys-number retorna esses valores:

| Atributo                   | Tipo   | Retornado para `veinte` | Retornado para `1.234,56` |
|-----------------------------|--------|-----------------------|------------------------:|
| @sys-number               | sequência | 20                    |                 1234,56 |
| @sys-number.literal       | sequência | veinte                |                1.234,56 |
| @sys-number.location      | matriz  | [0,6]                 |                   [0,8] |
| @sys-number.numeric_value | número | 20                    |                 1234,56 |

Você obtém resultados equivalentes para outros idiomas suportados.

### Dicas de uso de @system-number

- Se você usar a entidade @sys-number como uma condição de nó e o usuário especificar zero como o valor, o valor 0 será reconhecido corretamente como um número, mas a condição será avaliada como false e não poderá retornar a resposta associada corretamente. Para procurar números de uma maneira que manipule os zeros corretamente, use a sintaxe `entities['sys-number']` na condição do nó.

- Se você usar @sys-number para comparar valores numéricos em uma condição, certifique-se de incluir separadamente uma verificação para a presença de um número em si. Se nenhum número for localizado, @sys-number será avaliado como nulo, o que pode resultar em sua comparação sendo avaliada como verdadeira mesmo quando nenhum número está presente.

  Por exemplo, não use `@sys-number<4` sozinho porque se nenhum número for localizado, `@sys-number` será avaliado como nulo. Como nulo é menor que 4, a condição será avaliada como verdadeira, embora nenhum número esteja presente.

  Use `@sys-number AND @sys-number<4` em vez disso. Se nenhum número estiver presente, a primeira condição será avaliada como falsa, o que resulta adequadamente na condição inteira sendo avaliada como falsa.

Para obter informações sobre o processamento de valores numéricos, consulte a referência de método [Números](dialog-methods.html#numbers).
{: tip}

## Entidade @sys-percentage
{: #sys-percentage}

A entidade do sistema @sys-percentage detecta porcentagens expressas em uma elocução com o símbolo percentual ou escrita usando a palavra `percent`. Seja qual for o caso, um valor numérico será retornado.

### Formatos reconhecidos

- 15%
- 10 por cento

### Metadados

`.numeric_value`: o valor numérico canônico como um número inteiro ou um duplo

### Retorna

Para a entrada `1,234.56%`, @sys-percentage retorna esses valores:

| Atributo                     | Tipo   | Retornado para `1,234.56%` |
|-------------------------------|--------|-------------------------:|
| @sys-percentage               | sequência |                  1234,56 |
| @sys-percentage.literal       | sequência |                1,234.56% |
| @sys-percentage.location      | matriz  |                    [0,9] |
| @sys-percentage.numeric_value | número |                  1234,56 |

Para a entrada `1.234,56%`, em espanhol, @sys-currency retorna esses valores:

| Atributo                     | Tipo   | Retornado para `1.234,56%` |
|-------------------------------|--------|-------------------------:|
| @sys-percentage               | sequência |                  1234,56 |
| @sys-percentage.literal       | sequência |                1.234,56% |
| @sys-percentage.location      | matriz  |                    [0,9] |
| @sys-percentage.numeric_value | número |                  1234,56 |

Você obtém resultados equivalentes para outros idiomas suportados.

### Dicas de uso @system-percentage

- Os valores de porcentagem também são reconhecidos como instâncias de entidades @sys-number. Se você estiver usando condições separadas para verificar os valores de porcentagem e números, coloque a condição que verifica uma porcentagem acima daquela que verifica um número.

- Se você usar a entidade @sys-percentage como um nó de
condição e o usuário especificar `0%` como o valor, o valor será reconhecido como uma porcentagem corretamente, mas a condição será avaliada para o número zero, não a porcentagem 0%. Portanto, ela não retornará a resposta esperada. Para verificar as porcentagens de forma que manipule nenhuma porcentagem corretamente, use a sintaxe `entities['sys-percentage']` na condição de nó.

- Se você inserir um valor como `1-2%`, os valores `1%` e `2%` serão retornados como entidades do sistema. O índice será o intervalo inteiro entre 1% e 2%, e ambas as entidades terão o mesmo índice.

## Entidade @sys-person
{: #sys-person}

**BETA, para idiomas observados no tópico [Idiomas Suportados](lang-support.html)**: a entidade do sistema @sys-person extrai os nomes da entrada do usuário. Os nomes são reconhecidos individualmente, para que "Will" não seja tratado como "William" ou vice-versa. O valor da entidade não é um valor padrão do sistema para o nome.

### Formatos reconhecidos

- Will
- Jane Doe
- Vijay

Para obter informações sobre o processamento de valores de sequência, veja a referência de método de [Sequências](dialog-methods.html#strings).
{: tip}
