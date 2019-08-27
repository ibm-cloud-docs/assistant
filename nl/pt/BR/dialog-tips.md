---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-28"

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

# Dicas de construção de diálogo
{: #dialog-tips}

Saiba como abordar a construção de um diálogo e obter algumas dicas sobre a conclusão de etapas mais complexas.
{: shortdesc}

Revise essas dicas de designers de diálogo experientes.

## Planejando o diálogo geral
{: #dialog-tips-plan}

- Planeje o design do diálogo que deseja construir antes de incluir um único nó de diálogo. Faça um rascunho no papel, se necessário.
- Sempre que possível, baseie suas decisões de design em dados de evidências e comportamentos do mundo real. Não inclua nós para manipular uma situação que alguém *acha* que possa ocorrer.
- Evite copiar processos de negócios como está. Eles são raramente conversacionais.
- Se as pessoas já usam um processo, examine como elas abordam-no. As pessoas geralmente otimizam o processo de uma perspectiva de conversação.
- Decida sobre o tom, a personalidade e o posicionamento de seu assistente. Reflita consistentemente essas opções no diálogo que você criar.
- Nunca represente erroneamente o assistente como sendo um humano. Se os usuários acreditarem que o assistente é uma pessoa e, então, descobrirem que não é, provavelmente desconfiarão dele.
- Nem tudo tem que ser uma conversa. Às vezes, um formulário da web funciona melhor.

## Incluindo nós
{: #dialog-tips-nodes}

- Inclua um nome do nó que descreva o propósito do nó.

  Sabe-se o que o nó faz neste momento, mas daqui a meses você pode não saber. O seu eu futuro e quaisquer membros da equipe agradecerão a inclusão de um nome do nó descritivo. E o nome do nó é exibido no log, o que pode ajudar a depurar uma conversa posteriormente.
- Para reunir as informações necessárias para executar uma tarefa, tente usar um nó com intervalos em vez de vários nós separados para extrair informações dos usuários. Consulte  [ Reunindo informações com slots ](/docs/services/assistant?topic=assistant-dialog-slots).
- Para um fluxo do processo complexo, diga aos usuários sobre qualquer informação que eles precisarão fornecer no início do processo.
- Entenda como seu assistente percorre a árvore de diálogo e o impacto que as pastas, ramificações, saltos e digressões têm na rota. Consulte  [ Fluxo de diálogo ](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-flow).
- Não inclua salto-tos em todos os lugares. Eles aumentam a complexidade do fluxo de diálogo e tornam mais difícil depurar o diálogo posteriormente.
- Para ir para um nó na mesma ramificação que o nó atual, use *Ignorar entrada do usuário* em vez de *Ir para*.

  Essa opção evita que você tenha que editar as configurações do nó atual ao remover ou reordenar os nós-filhos que estão sendo saltados. Consulte  [ Definindo o que fazer em seguida ](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-jump-to).
- Antes de ativar as digressões fora de um nó, teste os cenários de usuário mais comuns. E certifique-se de que os prováveis nós digressionados-para estejam configurados para retornar. Consulte  [ Digressões ](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions).

## Incluindo Respostas
{: #dialog-tips-responses}

- Mantenha as respostas curtas e úteis.
- Reflita a intenção do usuário na resposta.

  Fazer isso assegura aos usuários que eles estão sendo entendidos pelo robô ou, se não estiverem, fornece aos usuários uma chance de corrigir um equívoco imediatamente.
- Somente inclua links para sites externos em respostas se a resposta depender de dados que mudam frequentemente.
- Evite a utilização de botões de sobreutilização. Encorajar os usuários a selecionar opções predefinidas de um conjunto de botões é menos parecido a uma conversa real e diminui sua capacidade de aprender o que os usuários realmente desejam fazer. Quando você deixa os usuários reais perguntarem coisas em suas próprias palavras, é possível usar a entrada para treinar o sistema e derivar intenções melhores.
- Evite usar vários nós quando um nó é suficiente. Por exemplo, inclua múltiplas respostas condicionais em um único nó para retornar diferentes respostas, dependendo dos detalhes fornecidos pelo usuário. Consulte  [ Respostas condicionais ](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multiple).
- Palavra de suas respostas cuidadosamente. É possível mudar como alguém reage ao seu sistema com base apenas em como você formula uma resposta. A mudança de uma linha de texto pode evitar que você tenha que gravar múltiplas linhas de código para implementar uma solução programática complexa.
- Faça backup de sua habilidade frequentemente. Consulte  [ Fazendo download de uma habilidade ](/docs/services/assistant?topic=assistant-skill-add#skill-add-download).

## Dicas para capturar informações da entrada do usuário
{: #dialog-tips-user-input}

Pode ser difícil saber a sintaxe a ser usada em seu nó de diálogo para capturar com precisão as informações que você deseja localizar na entrada do usuário. Aqui estão algumas abordagens que podem ser usadas para direcionar os objetivos comuns.

- **Retornando a entrada do usuário**: é possível capturar o texto exato proferido pelo usuário e retorná-lo em sua resposta. Use a expressão SpEL a seguir em uma resposta para repetir o texto que o usuário especificou de volta na resposta:

  `You said: <? input.text ?>.`

  Se a autocorreção estiver ligada e você desejar retornar a entrada original do usuário antes da correção, será possível usar `<? input.original_text ?>`. No entanto, certifique-se de usar uma condição de resposta que verifique se o campo `original_text` existe primeiro.
  {: note}

- **Determinando o número de palavras na entrada do usuário**: é possível executar qualquer um dos métodos de Sequência suportados no objeto input.text. Por exemplo, é possível descobrir quantas palavras há em uma elocução do usuário usando a expressão SpEL a seguir:

  ` input.text.split ('') .size () `

  Consulte [Métodos de linguagem de expressão para Sequência](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings) para aprender sobre mais métodos que podem ser usados.

- **Lidando com múltiplas intenções**: um usuário insere a entrada que expressa um desejo de concluir duas tarefas separadas. `I want to open a savings account and apply for a credit card.` Como o diálogo reconhece e direciona ambas? Consulte a entrada [Perguntas compostas](https://sodoherty.ai/2017/02/06/compound-questions/){: new_window} do blog de Simon O'Doherty para estratégias que podem ser tentadas. (Simon é um desenvolvedor na equipe do {{site.data.keyword.conversationshort}}.)

- **Lidando com intenções ambíguas**: um usuário insere uma entrada que expressa um desejo ambíguo o suficiente para que seu assistente localize dois ou mais nós com intenções que poderiam possivelmente atendê-lo. Como o diálogo sabe qual ramificação de diálogo seguir? Se você ativar a desambiguação, ela poderá mostrar aos usuários suas opções e solicitar ao usuário para selecionar a correta. Consulte  [ Desambiguação ](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)  para obter mais detalhes.

- **Manipulando múltiplas entidades na entrada**: se você deseja avaliar somente o valor da primeira instância detectada de um tipo de entidade, é possível usar a sintaxe `@entity == 'specific-value'` em vez do formato `@entity:(specific-value)`.

  Por exemplo, quando você usa `@appliance == 'air conditioner'`, você está avaliando apenas o valor da primeira entidade `@appliance` detectada. Mas, o uso de `@appliance:(air conditioner)` é expandido para `entity['appliance'].contains('air conditioner')`, que corresponderá sempre que houver pelo menos uma entidade `@appliance` com o valor 'air conditioner' detectado na entrada do usuário.

## Dicas de uso da condição
{: #dialog-tips-condition-usage}

- **Verificando valores com caracteres especiais**: se você deseja verificar se uma entidade ou variável de contexto contém um valor e o valor inclui um caractere especial, como um apóstrofo ('), deve-se circundar o valor que deseja verificar com parênteses. Por exemplo, para verificar se uma entidade ou variável de contexto contém o nome `O'Reilly`, deve-se circundar o nome com parênteses.

  `@person:(O'Reilly)` e `$person:(O'Reilly)`

  Seu assistente converte as referências abreviadas nestas expressões SpEL completas:

  `entities['person']?.contains('O''Reilly')` e `context['person'] == 'O''Reilly'`

  O SpEL usa um segundo apóstrofo para escapar o apóstrofo simples no nome.
  {: note}

- **Verificando múltiplos valores **: se você desejar verificar mais de um valor, será possível criar uma condição que use operadores OR (`||`) para listar múltiplos valores na condição. Por exemplo, para definir uma condição que seja true se a variável de contexto `$state` contiver as abreviações para Massachusetts, Maine ou New Hampshire, será possível usar esta expressão:

  ` $state :MA | | $state :ME | | $state: NH `

- **Verificando valores de número**: ao comparar números, primeiro certifique-se de que a entidade ou a variável que você está verificando tenha um valor. Se a entidade ou a variável não tiver um valor numérico, ela será tratada como tendo um valor nulo (0) em uma comparação numérica.

  Por exemplo, você deseja verificar se um valor de dólar que um usuário especificou na entrada do usuário é menor que 100. Se você usar a condição `@price < 100` e a entidade `@price` for nula, a condição será avaliada como `true` (porque 0 é menor que 100), mesmo que o preço nunca tenha sido configurado. Para evitar esse tipo de resultado impreciso, use uma condição como `@price AND @price < 100`. Se `@price` não tem nenhum valor, essa condição retorna corretamente false.

- **Verificando intenções com um padrão de nome de intenção específico**: é possível usar uma condição que procura intenções que correspondem a um padrão. Por exemplo, para localizar quaisquer intenções detectadas com nomes de intenção que iniciam com 'User_', é possível usar uma sintaxe como esta na condição:

  `intents[0].intent.startsWith("User_")`

  No entanto, ao fazer isso, todas as intenções detectadas são consideradas, mesmo aquelas com uma confiança inferior a 0,2. Verifique também se intenções que são consideradas irrelevantes pelo Watson com base em sua pontuação de confiança não são retornadas. Para fazer isso, mude a condição conforme a seguir:

  `!irrelevant && intents[0].intent.startsWith("User_")`

- **Como a correspondência difusa afeta o reconhecimento de entidade**: se você usa uma entidade como a condição e a correspondência difusa está ativada, `@entity_name` é avaliado como true somente se a confiança da correspondência é maior que 30%. Ou seja, somente se `@entity_name.confidence > .3`.

## Armazenando e reconhecendo grupos de padrões de entidade na entrada
{: #dialog-tips-get-pattern-groups}

Para armazenar o valor de uma entidade padrão em uma variável de contexto, anexe .literal ao nome da entidade. O uso desta sintaxe assegura que a extensão exata de texto da entrada do usuário que correspondeu ao padrão especificado seja armazenada na variável.

| Variável   | Valor               |
|------------|---------------------|
| email      | <? @email.literal ?> |

Para armazenar o texto de um único grupo em uma entidade padrão com grupos definidos, especifique o número da matriz do grupo que você deseja armazenar. Por exemplo, suponha que o padrão de entidade seja definido como a seguir para a entidade @phone_number. (Lembre-se, os parênteses denotam grupos padrão):

`\b((958)|(555))-(\d{3})-(\d{4})\b`

Para armazenar somente o código de área do número do telefone especificado na entrada do usuário, é possível usar a sintaxe a seguir:

| Variável       | Valor                         |
|----------------|-------------------------------|
| area_code      | <? @phone_number.groups[1] ?> |

Os grupos são delimitados pela expressão regular que é usada para definir o padrão de grupo. Por exemplo, se a entrada do usuário que corresponde ao padrão definido na entidade `@phone_number` é: `958-234-3456`, os grupos a seguir são criados:

| Número do grupo | Valor do mecanismo Regex  | Valor do diálogo   | Explicação |
|--------------|---------------------|----------------|-------------|
| groups[0]    | `958-234-3456`      | `958-234-3456` | O primeiro grupo é sempre a sequência de correspondência total. |
| groups[1]    | `((958)`l`(555))`   | `958`          | A sequência que corresponde ao regex para o primeiro grupo definido, que é `((958)`l`(555))`. |
| groups[2]    | `(958)`             | `958`          | Correspondência com relação ao grupo incluído como o primeiro operando na expressão OR `((958)`l`(555))` |
| groups[3]    | `(555)`             | `null`         | Nenhuma correspondência com relação ao grupo que é incluído como o segundo operando na expressão OR `((958)`l`(555))` |
| groups[4]    | `(\d{3})`           | `234`          | Sequência que corresponde à expressão regular que está definida para o grupo. |
| groups[5]    | `(\d{4})`           | `3456`         | Sequência que corresponde à expressão regular que está definida para o grupo. |
{: caption="Detalhes do grupo" caption-side="top"}

Para ajudá-lo a decifrar qual número de grupo usar para capturar a seção de entrada em que você está interessado, é possível extrair informações sobre todos os grupos de uma vez. Use a sintaxe a seguir para criar uma variável de contexto que retorna uma matriz de todas as correspondências de entidade padrão agrupadas:

| Variável                 | Valor                      |
|--------------------------|----------------------------|
| array_of_matched_groups  | <? @phone_number.groups ?> |

Use a área de janela "Experimente" para inserir alguns valores de número de telefone de teste. Para a entrada `958-123-2345`, essa expressão configura `$array_of_matched_groups` para `["958-123-2345","958","958",null,"123","2345"]`.

É possível então contar cada valor na matriz iniciando com 0 para obter o número do grupo para ele.

| Valor do elemento de matriz | Número do elemento de matriz |
|---------------------|----------------------|
| "958-123-2345"      | 0 |
| "958"               | 1 |
| "958"               | 2 |
| null                | 3 |
| "123"               | 4 |
| "2345"              | 5 |
{: caption="Elementos de matriz" caption-side="top"}

Pelo resultado, é possível determinar que, para capturar os últimos quatro dígitos do número do telefone, você precisa do grupo nº 5, por exemplo.

Para retornar a estrutura JSONArray que é criada para representar a entidade padrão agrupada, use a sintaxe a seguir:

| Variável             | Valor                           |
|----------------------|---------------------------------|
| json_matched_groups  | <? @phone_number.groups_json ?> |

Essa expressão configura `$json_matched_groups` para a matriz JSON a seguir:

```json
[
  {"group": "group_0","location": [0, 12]},
  {"group": "group_1","location": [0, 3]},
  {"group": "group_2","location": [0, 3]},
  {"group": "group_3"},
  {"group": "group_4","location": [4, 7]},
  {"group": "group_5","location": [8, 12]}
]
```
{: codeblock}

`location` é uma propriedade de uma entidade que usa um deslocamento de caractere baseado em zero para indicar onde o valor de entidade detectado inicia e termina no texto de entrada.
{: note}

Se você espera que dois números de telefone sejam fornecidos na entrada, é possível verificar dois números de telefone. Se presentes, use a sintaxe a seguir para capturar o código de área do segundo número, por exemplo.

| Variável         | Valor                                       |
|------------------|---------------------------------------------|
| second_areacode  | <? entities['phone_number'][1].groups[1] ?> |

Se a entrada é `I want to change my phone number from 958-234-3456 to 555-456-5678`, `$second_areacode` é igual a `555`.

## Visualizando Detalhes da Chamada API
{: #dialog-tips-inspect-api}

Ao testar seu diálogo com a área de janela "Experimente", você talvez deseje saber como se parecem as chamadas da API subjacentes que estão sendo retornadas do serviço. É possível usar as ferramentas do desenvolvedor fornecidas por seu navegador da web para inspecioná-las.

No Chrome, por exemplo, abra as ferramentas do Desenvolvedor. Clique na ferramenta Rede. A seção Nome lista múltiplas chamadas da API. Clique na chamada de mensagem associada à sua elocução de teste e, em seguida, clique na coluna Resposta para ver o corpo de resposta da API. Isso lista as intenções e entidades que foram reconhecidas na entrada do usuário com suas pontuações de confiança e os valores de variáveis de contexto no momento da chamada. Para visualizar o corpo de resposta no formato estruturado, clique na coluna Visualizar.

![Mostra como visualizar os detalhes da chamada da API usando as ferramentas do desenvolvedor do navegador da web Chrome.](images/api-browser-dev.png)
