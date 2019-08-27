---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-06"

keywords: system entity, sys-number, sys-date, sys-time

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

# Detalhes da entidade do sistema
{: #system-entities}

Saiba mais sobre as entidades do sistema fornecidas pela IBM prontas para utilização. Estas entidades de utilitário integradas ajudam seu assistente a reconhecer termos e referências comumente usados por clientes em conversas, como números e datas.
{: shortdesc}

As entidades do sistema estão disponíveis para idiomas anotados no tópico [Idiomas suportados](/docs/services/assistant?topic=assistant-language-support).

Se sua qualificação de diálogo estiver em inglês ou alemão, será possível experimentar as entidades do sistema atualizadas. Para obter mais detalhes, consulte [Novas entidades do sistema](/docs/services/assistant?topic=assistant-beta-system-entities).

Para obter mais informações sobre como usá-las, consulte [Criando entidades](/docs/services/assistant?topic=assistant-entities#entities-enable-system-entities).

## Entidade @sys-currency
{: #system-entities-sys-currency}

A entidade do sistema @sys-currency detecta valores monetários que são expressos em uma elocução com um símbolo monetário ou termos específicos da moeda. Um valor numérico é retornado.

### Formatos reconhecidos
{: #system-entities-sys-currency-formats}

- 20 centavos
- Cinco dólares
- $10

### Metadados
{: #system-entities-sys-currency-metadata}

- `.numeric_value`: o valor numérico canônico como um número inteiro ou um duplo, em unidades base
- `.unit`: o código de moeda de unidade base (por exemplo, 'USD' ou 'EUR')

### Retorna
{: #system-entities-sys-currency-returns}

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
{: #system-entities-currencty-usage-tips}

- Os valores de moeda também são reconhecidos como instâncias de entidades @sys-number. Se você estiver usando condições separadas para verificar os valores de moeda e os números, coloque a condição que verifica a moeda acima daquela que verifica um número.

  Essa solução alternativa não será necessária se você estiver usando as entidades do sistema revisadas. Para obter mais detalhes, consulte [Novas entidades do sistema](/docs/services/assistant?topic=assistant-beta-system-entities).
  {: note}

- Se você usar a entidade @sys-currency como uma condição de nó e o usuário especificar `$0` como o valor, o valor será reconhecido como uma moeda corretamente, mas a condição será avaliada para o número zero, não a moeda zero. Como resultado, `null` é avaliado como false na condição e o nó não é processado. Em vez disso, para verificar valores de moeda de uma forma que manipule zeros corretamente, use a expressão `@sys-currency >=0` na condição do nó.

## Entidades @sys-date e @sys-time
{: #system-entities-sys-date-time}

A entidade do sistema `@sys-date` extrai menções como `Friday`, `today` ou `November 1`. O valor dessa entidade armazena a data inferida correspondente como uma sequência no formato "aaaa-MM-dd", por exemplo, "2016-11-21". O sistema aumenta elementos ausentes de uma data (como o ano para "21 de novembro") com os valores de data atuais.

**Nota:** - apenas para o idioma inglês, o comportamento do sistema padrão para a entrada de data é MM/DD/AAAA. Isso mudará para DD/MM/AAAA apenas se os primeiros dois números forem maiores que 12. O valor armazenado ainda estará no formato "aaaa-MM-dd".

A entidade do sistema `@sys-time` extrai menções como `2pm`, `at 4` ou `15:30`. O valor dessa entidade armazena a hora como uma sequência no formato "HH :mm:ss". Por exemplo, "13:00:00."

### Menções de data/hora
{: #system-entities-date-time-mentions}

Menções de data e hora, como `now` ou `two hours from now` são extraídas como duas menções de entidade separadas - uma `@sys-date` e uma `@sys-time`. Essas duas menções não estão vinculadas uma com a outra, exceto que elas compartilham a mesma sequência literal que estende a menção de data/hora completa.

Menções com várias palavras, data e hora como `on Monday at 4pm` também são extraídas como duas menções @sys-date e @sys-time. Quando mencionadas juntas consecutivamente, elas também compartilham uma única sequência literal que abrange a menção de data/hora completa.

### Intervalos de data e hora
{: #system-entities-date-time-ranges}

Menções de um intervalo de data, como `the weekend`, `next week` ou `from Monday to Friday` são extraídas como um par de menções de entidade `@sys-date` que mostram o início e o término do intervalo. Da mesma forma, menções de intervalos de tempo como `from 2 to 3` são extraídos como duas entidades `@sys-time`, mostrando os horários de início e de encerramento. As duas entidades no par compartilham uma sequência literal que corresponde à menção do intervalo de data ou hora integral.

### `Last` e `Next` datas e horas
{: #system-entities-last-next}

Em alguns códigos de idioma, uma frase como "na segunda-feira passada" é usada para especificar a segunda-feira da semana anterior apenas. Em contraste, outros códigos de idioma usar "na segunda-feira passada" para especificar o último dia que foi uma segunda-feira, mas que pode ter sido na mesma semana ou na semana anterior.

Como um exemplo, para sexta-feira, 16 de junho, em alguns códigos de idioma "na segunda-feira passada" pode referir-se a 12 de junho ou a 5 de junho, enquanto que em outros códigos de idioma se refere apenas a 5 de junho (a semana anterior). Essa mesma lógica vale para uma frase como "na próxima segunda-feira".

O serviço {{site.data.keyword.conversationshort}} trata "última" e "próxima" datas como referência ao último ou próximo dia mais imediato referenciado, que pode estar na mesma semana ou em uma semana anterior.

Para frases de tempo como "nos últimos 3 dias" ou "nas próximas 4 horas", a lógica é equivalente. Por exemplo, no caso de "nas próximas 4 horas", isso resulta em duas entidades `@sys-time`: uma do horário atual e uma das quatro horas posteriores à hora atual.

### Fusos horários
{: #system-entities-time-zones}

Menções de uma data ou hora que são relativas ao horário atual são resolvidas com relação a um fuso horário escolhido. Por padrão, este é UTC (GMT). Isso significa que, por padrão, os clientes da API de REST localizados em fusos horários diferentes de UTC observarão o valor de `now` extraído de acordo com o horário UTC atual.

Opcionalmente, o cliente da API de REST pode incluir o fuso horário local como a variável de contexto `$timezone`. Essa variável de contexto deve ser enviada com cada solicitação do cliente. Por exemplo, o valor `$timezone` deve ser `America/Los_Angeles`, `EST` ou `UTC`. Para obter uma lista completa de fusos horários suportados, consulte [Fusos horários suportados](/docs/services/assistant?topic=assistant-time-zones).

Quando a variável `$timezone` é fornecida, os valores das menções @sys-date e @sys-time relativas são calculadas com base no fuso horário do cliente em vez de UTC.

#### Exemplos de menções relativas aos fusos horários
{: #system-entities-time-zone-examples}

- agora
- em duas horas
- hoje
- amanhã
- 2 dias a partir de agora

### Formatos reconhecidos
{: #system-entities-sys-date-time-formats}

- 21 de novembro
- 10h30
- às 18h
- nesse fim de semana

### Retorna
{: #system-entities-sys-date-time-returns}

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

Para obter informações sobre o processamento de valores de data e hora, consulte a [Referência do método de data e hora](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-date-time).
{: tip}

## Entidade @sys-location
{: #system-entities-sys-location}

**BETA, para idiomas anotados no tópico [Idiomas suportados](/docs/services/assistant?topic=assistant-language-support)**: a entidade do sistema @sys-location extrai nomes de locais (país, estado/município, cidade, município, etc.) da entrada dos usuários. O valor da entidade não é um valor padrão do sistema para o local.

### Formatos reconhecidos
{: #system-entities-sys-location-formats}

- Boston
- EUA
- Nova Gales do Sul

Para obter informações sobre o processamento de valores de Sequência, consulte a [Referência do método de sequências](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings).
{: tip}

## Entidade @sys-number
{: #system-entities-sys-number}

A entidade do sistema @sys-number detecta números que são gravados usando numerais ou palavras. Seja qual for o caso, um valor numérico será retornado.

### Formatos reconhecidos
{: #system-entities-sys-number-formats}

- 21
- vinte e um
- 3,13

### Metadados
{: #system-entities-sys-number-metadata}

- `.numeric_value` - o valor numérico canônico como um número inteiro ou um duplo

### Retorna
{: #system-entities-sys-number-returns}

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
{: #system-entities-sys-number-usage-tips}

- Se você usar a entidade @sys-number como uma condição de nó e o usuário especificar zero como o valor, esse valor será reconhecido corretamente como um número. No entanto, 0 é interpretado como um valor `null` para a condição, o que faz com que o nó não seja processado. Em vez disso, para verificar os números de uma forma que manipule zeros corretamente, use a expressão `@sys-number >= 0` na condição do nó.

- Se você usar @sys-number para comparar valores numéricos em uma condição, certifique-se de incluir separadamente uma verificação para a presença de um número em si. Se nenhum número for localizado, @sys-number será avaliado como nulo, o que pode resultar em sua comparação sendo avaliada como verdadeira mesmo quando nenhum número está presente.

  Por exemplo, não use somente `@sys-number<4` porque, se nenhum número for localizado, `@sys-number` será avaliado como nulo. Como nulo é menor que 4, a condição será avaliada como verdadeira, embora nenhum número esteja presente.

  Em vez disso, use `@sys-number AND @sys-number<4`. Se nenhum número estiver presente, a primeira condição será avaliada como falsa, o que resulta adequadamente na condição inteira sendo avaliada como falsa.

Para obter informações sobre os valores de número de processamento, consulte a [Referência do método de números](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-numbers).
{: tip}

## Entidade @sys-percentage
{: #system-entities-sys-percentage}

A entidade do sistema @sys-percentage detecta porcentagens expressas em uma elocução com o símbolo percentual ou escrita usando a palavra `percent`. Seja qual for o caso, um valor numérico será retornado.

### Formatos reconhecidos
{: #system-entities-sys-percentage-formats}

- 15%
- 10 por cento

### Metadados
{: #system-entities-sys-percentage-metadata}

`.numeric_value`: o valor numérico canônico como um número inteiro ou um duplo

### Retorna
{: #system-entities-sys-percentage-returns}

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
{: #system-entities-sys-percentage-usage-tips}

- Os valores de porcentagem também são reconhecidos como instâncias de entidades @sys-number. Se você estiver usando condições separadas para verificar os valores de porcentagem e números, coloque a condição que verifica uma porcentagem acima daquela que verifica um número.

  Essa solução alternativa não será necessária se você estiver usando as entidades do sistema revisadas. Para obter mais detalhes, consulte [Novas entidades do sistema](/docs/services/assistant?topic=assistant-beta-system-entities).
  {: note}

- Se você usar a entidade @sys-percentage como uma condição do nó e o usuário especificar `0%` como o valor, esse valor será reconhecido corretamente como uma porcentagem, mas a condição será avaliada para o número zero e não para a porcentagem de 0%. Como resultado, `null` será avaliado como false na condição e o nó não será processado. Em vez disso, para verificar porcentagens de uma forma que manipule corretamente as porcentagens de zero, use a expressão `@sys-percentage >= 0` na condição do nó.

- Se você inserir um valor como `1-2%`, os valores `1%` e `2%` serão retornados como entidades do sistema. O índice será o intervalo inteiro entre 1% e 2%, e ambas as entidades terão o mesmo índice.

## Entidade @sys-person
{: #system-entities-sys-person}

**BETA, para idiomas anotados no tópico [Idiomas suportados](/docs/services/assistant?topic=assistant-language-support)**: a entidade do sistema @sys-person extrai nomes da entrada do usuário. Os nomes são reconhecidos individualmente, para que "Joe" não seja tratado como "Joseph" ou vice-versa. O valor da entidade não é um valor padrão do sistema para o nome.

### Formatos reconhecidos
{: #system-entities-sys-person-formats}

- Ronald.
- Jane Doe
- Vijay

Para obter informações sobre o processamento de valores de Sequência, consulte a [Referência do método de sequências](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings).
{: tip}
