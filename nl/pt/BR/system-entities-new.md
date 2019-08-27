---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-17"

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

# Novas entidades do sistema ![Beta](images/beta.png)
{: #beta-system-entities}

Permita que as novas entidades do sistema aproveitem os aprimoramentos feitos nas entidades do sistema baseadas em números que foram fornecidas pela IBM.

Atualmente, essa configuração pode ser ativada somente para qualificações de diálogo escritas em inglês ou alemão.
{: note}

As novas entidades do sistema podem reconhecer mais menções diferenciadas na entrada do usuário. Por exemplo, `@sys-date` pode calcular a data de um feriado nacional, como `Thanksgiving`, quando ele é mencionado pelo nome. Além disso, `@sys-date` pode reconhecer quando um ano é especificado como parte de uma data mencionada na entrada do usuário. Para seu assistente, as melhorias também facilitam a distinção entre as muitas entidades do sistema baseadas em números. Por exemplo, uma menção de data, como `May 10`, reconhecida como `@sys-date`, não é identificada também como uma menção `@sys-number`.

As entidades do sistema `@sys-location` e `@sys-person` não foram mudadas.
{: note}

## Ativando as novas entidades do sistema
{: #beta-system-entities-enable}

Para ativar as novas entidades do sistema, conclua as etapas a seguir:

1.  Na página Qualificações, abra sua qualificação.
1.  Clique na guia **Opções**.
1.  Clique em **Entidades do sistema** e, em seguida, escolha **Experimentar o beta**.
1.  Clique na guia **Entidades** e, em seguida, clique em **Entidades do sistema**.
1.  Ative as entidades do sistema que deseja usar em seu diálogo.

Teste as novas entidades do sistema incluindo uma ou mais delas nas condições do nó de diálogo ou na condição das respostas condicionais de um nó de diálogo. Em seguida, na área de janela "Experimentar", envie elocuções de usuário que acionem os nós incluídos.

Se decidir que prefere o comportamento da versão anterior das entidades do sistema, será possível parar de usar a versão beta a qualquer momento. Volte à página **Entidades do sistema** da guia **Opções** e escolha **Manter existente**. Aguarde até que o Watson realize o novo treinamento.

Se decidir continuar usando a versão beta, revise os nós de diálogo existentes que condicionam ou retornam valores de entidade do sistema para determinar se mudanças são necessárias. Por exemplo, algumas menções foram classificadas anteriormente como mais de um tipo de entidade do sistema. Agora, se uma moeda for mencionada, por exemplo, ela será classificada somente como `@sys-currency`. É possível simplificar a lógica usada anteriormente para solucionar o comportamento ou pode ser necessário revisar a lógica incluída que depende do comportamento anterior.

### Novas propriedades de entidades do sistema
{: #beta-system-entities-props}

Para melhorar as entidades do sistema, novas propriedades foram incluídas nos objetos de entidade das entidades do sistema baseadas em números.

A tabela a seguir resume as novas propriedades incluídas. Para ver as propriedades associadas às entidades atuais do sistema, consulte [Detalhes da entidade do sistema](/docs/services/assistant?topic=assistant-system-entities).

Tabela 1. Novas propriedades de entidades do sistema

| Entidade do sistema | Propriedade | Descrição |
|---------------|----------|-------------|
| `@sys-date` | `alternatives` | Quando a data não é claramente indicada, captura outras datas além da que foi salva como o valor `@sys-date` que podem ter sido ditas pelo usuário. Por exemplo, se a data de hoje for 10 de março de 2019 e o usuário digitar `on March 1`, a data de 1º de março do próximo ano será salva como `@sys-date` (`2020-03-01`). No entanto, o usuário talvez quisesse dizer 1º de março de 2019. O sistema também salva a data desse ano (`2019-03-01`) como um valor de data alternativo. Os valores alternativos são salvos como um objeto que contém um valor e uma confiança para cada data alternativa e são armazenados em uma matriz JSON. Para obter um valor alternativo, use a sintaxe: `@sys-date.alternative`. Na maioria dos casos, a data futura é salva como o valor `@sys-date` e a data anterior é salva como a alternativa. Isso ocorre para os dias da semana especificados sem uma preposição, como `on`, ou um adjetivo, como `this` ou `next`. Por exemplo, `Are you open Monday?`. No entanto, se um usuário especificar um dia da semana como parte de uma frase, como `on Monday` ou `next Monday`, a data será considerada futura e nenhuma data alternativa será criada para ela. Por exemplo, se hoje for terça-feira e um usuário inserir `this Tuesday` ou `next Tuesday`, `@sys-date` será configurado para a data de terça-feira da próxima semana e datas alternativas não serão criadas. |
| `@sys-date` | `datetime_link` | Se presente, indica que a entrada do usuário menciona uma data e hora juntas, o que implica que a data e hora estão relacionadas entre si. Os valores de índice de início e de encerramento da sequência que inclui a data e hora são salvos como parte do nome do link. Por exemplo, se a entrada for `Are you open today at 5?`, `today` será reconhecido como uma referência de data no local `[13,18]` e `at 5` será reconhecido como uma referência de horário no local `[19,23]`. Um valor de local de `[13,23]`, que abrange as menções de data e hora, é anexado ao datetime_link resultante. Ele é denominado `datetime_link_13_23` e é incluído na saída das entidades do sistema `@sys-date` e `@sys-time` que participam do relacionamento. |
| `@sys-date` | `festival` | Reconhece nomes de feriados específicos de códigos de idioma e pode retornar a data dos próximos feriados, como `Thanksgiving`, `Christmas` e `Halloween`. Use todas as letras minúsculas se precisar especificar um nome de feriado em uma condição, por exemplo (`@sys-date.festival == 'thanksgiving'`). |
| `@sys-date` | `granularity` | Reconhece menções de intervalos de tempo, como `next year` (=`year`). As opções são `day`, `weekend`, `week`, `fortnight`, `month`, `quarter` e `year`. |
| `@sys-date` | `interpretation` | O objeto que é retornado por seu assistente que contém novos campos que aumentam a precisão do classificador de entidade do sistema. É possível omitir `interpretation` da expressão que você usa para se referir a seus campos. Por exemplo, é possível acessar o valor do campo `festival` no objeto de interpretação usando a sintaxe abreviada `<? @sys-date.festival ?>`. |
| `@sys-date` | `range_link` | Se presente, indicará que a entrada do usuário contém uma sintaxe que sugere que um intervalo de datas seja especificado. Por exemplo, a entrada pode ser `I will travel from March 15 to March 22`. Cada uma das duas entidades do sistema `@sys-date` detectadas tem uma propriedade `range_link` em sua saída. Informações adicionais são fornecidas, incluindo a função de cada `@sys-date` no relacionamento do intervalo. Por exemplo, a data de início tem um tipo de função `date_from` e a data de encerramento tem um tipo de função `date_to`. Para verificar o valor de uma função, é possível usar a sintaxe `@sys-date.role?.type == 'date_from'`. Se a entrada do usuário sugerir um intervalo, mas somente uma data for especificada, a propriedade `range_link` não será retornada, mas um tipo de função será retornado. Por exemplo, se o usuário perguntar `Have you been open since yesterday`, a data de ontem será reconhecida como a menção `@sys-date` e uma função do tipo `date_from` será retornada para ela. Um `range_modificfier` que identifica a palavra na entrada que aciona a identificação de um intervalo também é retornado. Neste exemplo, o modificador é `since`. |
| `@sys-date` | `relative_year`, `relative_month`, `relative_week`, `relative_weekend`, `relative_day` | Reconhece e captura menções de valores de data relativos na entrada do usuário, como `two weeks ago` (`relative_week = -2`), `this weekend (relative_weekend = 0)` ou `today` (`relative_day = 0`). |
| `@sys-date` | `specific_year`, `specific_quarter`, `specific_month`, `specific_day`, `specific_day_of_week` | Reconhece e captura menções de valores de data específicos na entrada do usuário, como `2020` (`specific_year = 2020`), `Q2` (`specific_quarter = 2`), `March`  (`specific_month = 3`), `Monday` (`specific_day_of_week = monday`) ou `3rd` (`specific_day = 3`). |
| `@sys-number` | `interpretation` | O objeto que é retornado por seu assistente que contém novos campos que aumentam a precisão do classificador de entidade do sistema. É possível omitir `interpretation` da expressão que você usa para se referir a seus campos. Por exemplo, é possível acessar o valor do campo `range_link` no objeto de interpretação usando a sintaxe abreviada `<? @sys-date.range_link ?>`. |
| `@sys-number` | `range_link` | Se presente, indicará que a entrada do usuário contém uma sintaxe que sugere que um intervalo de número seja especificado. Por exemplo, a entrada pode ser `I'm interested in 5 to 7.`. Cada uma das duas entidades do sistema `@sys-number` detectadas tem uma propriedade `range_link` em sua saída. Informações adicionais são fornecidas, incluindo a função de cada `@sys-number` no relacionamento do intervalo. Por exemplo, o número inicial tem um tipo de função `number_from` e o número final tem um tipo de função `number_to`. Para verificar o valor de uma função, é possível usar a sintaxe `@sys-number.role?.type == 'number_from'`. |
| `@sys-time` | `alternatives` | Quando o horário não é claramente indicado, captura horários além do que foi salvo como o valor `@sys-time` que podem ter sido ditos pelo usuário. Por exemplo, quando um usuário diz `The meeting is at 5`, o sistema calcula o horário como 5:00:00 ou 5 AM e o salva como `@sys-time`. No entanto, o usuário pode ter dito 17:00:00 ou 5 PM. O sistema também salva o horário posterior como um valor de horário alternativo. Se o usuário inserir `The meeting is at 5pm`, `@sys-time` será configurado como 17:00:00 e nenhum valor alternativo será criado, pois não haverá confusão entre AM e PM. Os valores alternativos são salvos como um objeto que contém um valor e uma confiança para cada horário alternativo e são armazenados em uma matriz JSON. Para obter um valor alternativo, use a sintaxe: `@sys-time.alternative`. |
| `sys-time` | `datetime_link` | Se presente, indica que a entrada do usuário menciona uma data e hora juntas, o que implica que a data e hora estão relacionadas entre si. Os valores de índice de início e de encerramento da sequência que inclui a data e hora são salvos como parte do nome do link. Por exemplo, se a entrada for `Are you open today at 5?`, `today` será reconhecido como uma referência de data no local `[13,18]` e `at 5` será reconhecido como uma referência de horário no local `[19,23]`. Um valor de local de `[13,23]`, que abrange as menções de data e hora, é anexado ao datetime_link resultante. Ele é denominado `datetime_link_13_23` e é incluído na saída das entidades do sistema `@sys-date` e `@sys-time` que participam do relacionamento. |
| `@sys-time` | `granularity` | Reconhece as menções de prazo, como `now` (=`instant`), `3 o'clock`, `noon` (=`hour`) ou `17:00:00` (=`second`). As opções são `hour`, `minute`, `second` e `instant`. |
| `@sys-time` | `interpretation` | O objeto que é retornado por seu assistente que contém novos campos que aumentam a precisão do classificador de entidade do sistema. É possível omitir `interpretation` da expressão que você usa para se referir a seus campos. Por exemplo, é possível acessar o valor do campo `granularity` no objeto de interpretação usando a sintaxe abreviada `<? @sys-time.granularity ?>`. |
| `@sys-time` | `part_of_day` | Reconhece termos que representam o horário do dia, como `morning`, `afternoon`, `evening`, `night` ou `now`. Também configura horários arbitrários, como `9:00:00` para manhã, `15:00:00` para tarde, `18:00:00` para início da noite e `22:00:00` para fim da noite. |
| `@sys-time` | `range_link` | Se presente, indicará que a entrada do usuário contém uma sintaxe que sugere que um prazo seja especificado. Por exemplo, a entrada pode ser `Are you open from 9AM to 11AM`. Cada uma das duas entidades do sistema `@sys-time` detectadas tem uma propriedade `range_link` em sua saída. Informações adicionais são fornecidas, incluindo a função de cada `@sys-time` no relacionamento do intervalo. Por exemplo, o horário de início tem um tipo de função `time_from` e o horário de encerramento tem um tipo de função `time_to`. Para verificar o valor de uma função, é possível usar a sintaxe `@sys-time.role?.type == 'time_from'`. Se a entrada do usuário sugerir um intervalo, mas somente um horário for especificado, a propriedade `range_link` não será retornada, mas um tipo de função será retornado. Por exemplo, se o usuário perguntar `Are you open until 9PM`, 9PM será reconhecido como a menção `@sys-time` e uma função do tipo `time_to` será retornada para ela. Um `range_modificfier` que identifica a palavra na entrada que aciona a identificação de um intervalo também é retornado. Neste exemplo, o modificador é `until`. |
| `@sys-time` | `relative_hour`, `relative_minute`, `relative_second` | Reconhece menções relativas de horário, como `5 hours ago` (`relative_hour = -5`), `in two minutes` (`relative_minute = 2`) ou `in a second` (`relative_second = 1`). |
| `@sys-time` | `specific_hour`, `specific_minute`, `specific_second` | Reconhece menções específicas de horário, como `at 5 o'clock` (`specific_hour = 5`), `at 2:30` (`specific_minute = 30`) ou `23:30:22` (`specific_second = 22`). |
{: caption="Novas propriedades de entidades do sistema" caption-side="top"}

Os valores da tabela a seguir não são propriedades do objeto de entidade. Eles são valores auxiliares que podem ser usados no produto para extrair rapidamente detalhes de data ou hora das novas entidades do sistema.

Tabela 2. Funções auxiliares de entidades do sistema

| Entidade do sistema | Valor auxiliar | Descrição |
|---------------|----------|-------------|
| `@sys-date` | `day` | Retorna o dia especificado na data como um valor numérico. Por exemplo, se a data for `5 December 2019`, ele retornará `5`. |
| `@sys-date` | `day_of_week` | Avalia a data para determinar qual é o dia da semana. Armazena o dia da semana em minúsculas. Por exemplo, `Sunday` é armazenado como `sunday`. Use uma letra inicial minúscula para verificar o nome de um dia da semana em uma condição. Por exemplo: `@sys-date.day_of_week == 'sunday'` |
| `@sys-date` | `month` | Retorna o mês especificado na data como um valor numérico. Por exemplo, se a data for `5 December 2019`, ele retornará `12`. |
| `@sys-date` | `year` | Retorna o ano especificado na data como um valor numérico. Por exemplo, se a data for `5 December 2019`, ele retornará `2019`. |
| `@sys-time` | `hour` | Retorna a hora especificada no horário como um valor numérico. Por exemplo, se o horário for `5:30:10 PM`, ele retornará `17` para representar 5 PM. |
| `@sys-time` | `minute` | Retorna o valor do minuto especificado no horário. `30`. Se nenhum minuto for especificado, esse auxiliar retornará `0`.|
| `@sys-time` | `second` | Retorna o segundo valor especificado no horário. `10`. Se nenhum segundo valor for especificado, esse auxiliar retornará `0`. |
{: caption="Funções auxiliares de entidades do sistema" caption-side="top"}

### Exemplos de uso
{: #beta-system-entities-examples}

É possível usar os tipos de expressões a seguir para aproveitar as novas propriedades nas entidades do sistema aprimoradas. A tabela mostra que os resultados são retornados quando um usuário menciona a data `4 July 2019` e o horário `3:30 PM`. É possível usar essas expressões em respostas de texto do diálogo sem envolvê-las na sintaxe `<? ?>`.

Tabela 3. Usando as novas entidades do sistema para obter detalhes de data e hora

| Sintaxe de expressão SpEL | Resultado |
|------------------------|---------|
| `@sys-date.year` | `2019` |
| `@sys-date.month` | `7` |
| `@sys-date.day` | `4` |
| `@sys-date.day_of_week` | `thursday` |
| `@sys-time.hour` | `15` |
| `@sys-time.minute` | `30` |
| `@sys-time.second` | `0` |
{: caption="Usar as novas entidades do sistema para obter detalhes de data e hora" caption-side="top"}

Em um nó que condiciona a intenção `#Customer_Care_Store_Hours`, é possível incluir respostas condicionais que usam novas propriedades de entidades do sistema para fornecer respostas ligeiramente diferentes sobre os horários da loja, dependendo do que for solicitado pelo usuário. 

Provavelmente, você não desejará usar todas essas respostas condicionais em um diálogo real. Elas são descritas aqui meramente para ilustrar as possibilidades.
{: tip}

Tabela 4. Usando novas entidades do sistema em respostas condicionais

| Sintaxe de condição da resposta condicional | Descrição | Texto de resposta de exemplo |
|--------------------------------|-------------|----------|
| `@sys-date.festival == 'thanksgiving'` | Verifica se o cliente está perguntando sobre horários particularmente no dia de Ação de Graças. | Oferecemos refeições gratuitas para aqueles que precisam no dia de Ação de Graças. |
| `@sys-date.festival` | Verifica se o cliente está perguntando sobre horários em outro feriado específico. | Estaremos fechados no Natal, no dia 4 de julho e no Dia do Presidente. Em outros feriados, abrimos das 10h às 17h. |
| `@sys-time.part_of_day == 'night'` | Verifica se o usuário incluiu algum termo que mencione o fim da noite como o horário do dia na entrada. | Não ficamos abertos até tarde; fechamos às 21h na maioria dos dias. |
| `@sys-date.datetime_link && input.text.contains('today') && now().sameOrAfter(@sys-time)` | Verifica se a entrada contém uma frase, como `today at 8`. Também verifica se o `@sys-time` detectado é anterior ao horário atual do dia. É possível incluir uma variável de contexto na resposta condicional que cria uma variável `$new_time` com o valor `<? @sys-time.plusHours(12) ?>`. É possível usar esta abordagem para criar uma variável de data que capture o horário mais provável dito por um usuário que, ao meio-dia, pergunta `Are you open today at 8`. O sistema pressupõe que o usuário disse 8AM em vez de 8PM. | Você quis dizer hoje, às $new_time, certo? |
| `@sys-time.range_link && entities[0].role?.type == 'time_from' && entities[1].role?.type == 'time_to'` | Verifica se a entrada contém um intervalo de tempo. A condição também garante que a primeira entidade do sistema `@sys-time` listada na matriz de entidades seja o horário de início e que a segunda seja o horário de encerramento antes de incluí-las na resposta. | Você quer saber se estamos abertos entre `<? entities[0] ?>` e `<? entities[1] ?>`, certo? |
| `@sys-time && entities.size() < 2 && entities[0].role?.type == 'time_to'` | Verifica se a entrada contém uma menção `@sys-time` com o tipo de função `time_to`. Se a entrada do usuário corresponder a essa condição, ela pressuporá que ele especificou o horário de encerramento de um intervalo de tempo aberto. Por exemplo, a resposta seria acionada pela entrada `Are you open until 9pm?`. Essa entrada é correspondida porque é identificada somente uma vez em uma amplitude de tempo e a menção tem uma função `time_to`. | Você quis dizer de agora (`<? now().reformatDateTime('h:mm a') ?>`) até as `<? @sys-time.reformatDateTime('h:mm a') ?>`? |
| `@sys-date.day_of_week == 'sunday'` | Verifica se uma data específica sobre a qual o usuário está perguntando se enquadra em um domingo. | Estamos fechados aos domingos. |
| `@sys-date.specific_day_of_week == 'monday' && @sys-date.alternative` | Verifica se o usuário mencionou o dia da semana `Monday` em sua consulta. Por exemplo, `Are you open Monday`. A condição também verifica se alguma data alternativa foi armazenada. Datas alternativas são criadas quando seu assistente não tem certeza de qual segunda-feira o usuário quis dizer, por isso, ele também armazena as datas de segundas-feiras alternativas. Quando um usuário especifica um dia da semana, seu assistente pressupõe que ele quis dizer a ocorrência futura do dia (a próxima segunda-feira). É possível incluir uma resposta que confirme a data pretendida usando o valor alternativo detectado. Neste caso, a data alternativa é a data da segunda-feira anterior. | Você quis dizer `@sys-date` ou `@sys-date.alternative`? |
| `true` | Responde a quaisquer outras solicitações de informações sobre o horário da loja. | Estamos abertos das 9h às 21h de segunda a sábado. |
{: caption="Usar as novas entidades do sistema em respostas condicionais" caption-side="top"}
