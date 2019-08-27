---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-01"

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

# Chamando um aplicativo cliente ![BETA](images/beta.png)
{: #dialog-actions-client}

Inclua uma ação do cliente em seu nó de diálogo que pause o diálogo para que um processo do lado do cliente possa ser executado e retorne um resultado como parte do processamento que ocorre dentro da ocorrência do diálogo.
{: shortdesc}

Uma ação do cliente define uma chamada programática em um formato padronizado que seu aplicativo cliente externo pode entender. Seu aplicativo cliente externo deve usar as informações fornecidas para executar a chamada ou função programática e, em seguida, retornar o resultado para o diálogo.

Essa ação não faz uma chamada direta por conta própria. Ela basicamente informa ao diálogo que ele deve ser pausado aqui e aguardar que um aplicativo cliente externo realize alguma ação. O programa que o aplicativo cliente executa pode ser qualquer coisa que você escolher. Certifique-se de especificar o nome da chamada, os detalhes do parâmetro e o nome da variável da mensagem de erro, de acordo com as regras de formatação JSON descritas posteriormente neste tópico.

É possível chamar um aplicativo cliente para realizar uma das ações a seguir:

- Validar informações coletadas do usuário.
- Executar cálculos ou manipulações de sequência na entrada do usuário que são muito complexos para serem manipulados pelos métodos de expressão SpEL suportados.
- Obter dados de outro aplicativo ou serviço.

Para obter informações sobre como chamar um serviço externo, como uma ação da web do {{site.data.keyword.openwhisk_short}}, consulte [Fazendo uma chamada programática por meio de um nó de diálogo](/docs/services/assistant?topic=assistant-dialog-webhooks).

O diagrama a seguir ilustra como é possível usar uma chamada do cliente para obter informações de previsão de tempo e retorná-las ao usuário.

![Mostra alguém solicitando uma previsão do tempo e o diálogo enviando uma solicitação a um aplicativo cliente que a envia ao serviço externo](images/forecast.png)

Observe que a ação do cliente chama o programa `MyWeatherFunction`, que é executado pelo aplicativo cliente. O aplicativo cliente faz a chamada para o serviço da web externo (`/weather`) para obter as informações reais de previsão. Em seguida, o aplicativo cliente retorna a resposta para o diálogo. Quando você inclui uma ação do cliente em um diálogo, um aplicativo cliente deve existir e fazer o processamento por conta própria ou transmitir as informações entre seu diálogo e quaisquer serviços externos de back-end que você deseja usar.

## Procedimento
{: #dialog-actions-client-call}

Para fazer uma chamada programática para um aplicativo cliente por meio de um nó de diálogo, conclua as etapas a seguir:

1.  No nó de diálogo do qual você deseja fazer a chamada programática, abra o editor JSON.

    - Para fazer uma chamada programática que é executada após a resposta para um nó ser avaliada, abra o editor JSON para a resposta do nó.

      ![Mostra como acessar o editor da JSON associado com uma resposta de nó padrão.](images/contextvar-json-response.png)

      Se a configuração **Múltiplas respostas** é **Ativado** para o nó, deve-se clicar no ícone **Editar resposta** ![Editar resposta](images/edit-slot.png) para que o menu **Opções** ![Resposta avançada](images/kabob.png) fique visível.

      ![Mostra como acessar o editor JSON que está associado a um nó padrão que tem múltiplas respostas condicionais que estão ativadas para ele.](images/contextvar-json-multi-response.png)

    Se desejar exibir ou processar ainda mais a resposta do mesmo diálogo, um segundo nó deverá ser incluído para isso e deverá ser acessado por meio desse nó.
    {: tip}

    - Para fazer uma chamada que possa ser usada por um intervalo individual, clique no ícone **Editar intervalo** ![Editar intervalo](images/edit-slot.png) para o intervalo e, em seguida, faça uma das coisas a seguir:

      - Para fazer uma chamada programática que é executada após a condição do intervalo ser avaliada como true, abra o editor JSON que está associado à condição do intervalo.

        ![Mostra como acessar o editor da JSON associado com uma condição do intervalo.](images/contextvar-json-slot-condition.png)

      - Para fazer uma chamada programática que é executada após o slot ser preenchido com êxito, abra o editor JSON que está associado à resposta Localizado. Para fazer isso, no menu **Opções** ![Ícone Opções](images/kabob.png) para o intervalo, clique em **Ativar respostas condicionais**. Para a resposta Localizado, clique no ícone **Editar resposta** ![Editar resposta](images/edit-slot.png). No menu **Opções** ![Ícone Opções](images/kabob.png) para a resposta Localizado, clique em **Abrir editor JSON**.

        ![Mostra como acessar o editor da JSON associado com a resposta condicional para um intervalo.](images/contextvar-json-slot-multi-response.png)

1.  Use a sintaxe a seguir para definir a chamada programática.

    ```json
    {
      "context": {
        "variable_name" : "variable_value"
      },
      "actions": [
        {
          "name":"<actionName>",
          "type":"client",
          "parameters": {
            "<parameter_name>":"<parameter_value>",
            "<parameter_name>":"<parameter_value>"
          },
          "result_variable": "<result_variable_name>"
        }
      ],
      "output": {
        "text": "response text"
      }
    }
    ```
    {: codeblock}

    A matriz `actions` especifica as chamadas programáticas a serem feitas no diálogo. Ela pode definir até cinco chamadas programáticas separadas. Especifique os pares de nome e valor a seguir na matriz JSON:

    - `<actionName>`: necessário. O nome da ação ou do serviço a ser chamado. Especifique um nome em qualquer sintaxe desejada. O objetivo é especificar um nome que seu aplicativo cliente reconheça e saiba como manipular. O nome não pode ter mais que 256 caracteres.

      Por exemplo: `calculateRate`

    - `<type>`: indica o tipo de chamada a ser feita. Opcionalmente, especifique `client`, que é o valor padrão.

      Envia uma resposta de mensagem com informações de chamada programática em um formato padronizado que seu aplicativo cliente externo entende. Seu aplicativo cliente deve usar as informações fornecidas para executar a chamada ou a função programática e retornar o resultado para o diálogo. O objeto JSON no corpo de resposta especifica o serviço ou a função a ser chamada, quaisquer parâmetros associados a serem passados com a chamada e o formato do resultado a ser enviado de volta.

    - `<action_parameters>`: quaisquer parâmetros que são esperados pelo programa externo, que são especificados como um objeto JSON. Os parâmetros são necessários somente se requeridos pelo programa externo.

    - `<result_variable_name>`: o nome a ser usado para referenciar o objeto JSON que é retornado pelo serviço ou programa externo. O resultado é incluído na seção de contexto da resposta `/message`. Em outras palavras, o resultado é armazenado como uma variável de contexto para que possa ser exibido na resposta do nó ou acessado por nós de diálogo que forem acionados mais tarde. Qualquer valor existente para a variável de contexto é sobrescrito pelo valor retornado pela ação. É possível especificar o `result_variable_name` usando a sintaxe a seguir:

      - `my_result`
      - `$my_result`

      O nome não pode ter mais que 64 caracteres. O nome de variável não pode conter os caracteres a seguir: parênteses `()`, colchetes (`[]`), uma aspa simples (`'`), aspas (`"`) ou uma barra invertida (`\`).

      Se desejar salvar o resultado na seção de entrada ou saída da resposta `/message`, inclua uma das palavras-chave de local a seguir como um prefixo para `result_variable_name`:

       - `output.`: inclui o resultado para a seção de saída da resposta /message. Por exemplo, `output.my_result`.
       - `input.`: inclui o resultado para a seção de entrada da resposta /message. Por exemplo, `input.my_result`.

      É possível especificar um prefixo de palavra-chave de local `context.` também. Por exemplo, `context.my_result`. No entanto, isso não é necessário porque o resultado é incluído no contexto por padrão.

      É possível incluir pontos no nome de variável para criar um objeto JSON aninhado. Por exemplo, é possível definir essas variáveis para capturar os resultados de duas solicitações separadas para um serviço meteorológico para as previsões de hoje e amanhã.

      - `context.weather.today`
      - `context.weather.tomorrow`

      Os resultados (valores de parâmetro `temp` e `rain`) são armazenados no contexto desta estrutura:

      ```json
      {
        "weather": {
          "today": {
            "temp": "20",
            "rain": "30"
          },
          "tomorrow": {
            "temp": "23",
            "rain": "80"
          }
        }
      }
      ```
      {: codeblock}

      Se diversas ações em uma única matriz de ação JSON incluírem o resultado da chamada programática delas na mesma variável de contexto, a ordem na qual o contexto será atualizado importará. Por tipo de ação, a ordem na qual as ações são definidas na matriz determina a ordem na qual o valor da variável de contexto é configurado. O valor da variável de contexto retornado pela última ação na matriz sobrescreve os valores calculados por quaisquer outras ações.

## Exemplo de chamada de cliente
{: #dialog-actions-client-example}

O exemplo a seguir mostra como uma chamada para um serviço meteorológico externo pode se parecer. Ele é incluído no editor JSON associado à resposta do nó. Até a resposta no nível do nó ser acionada, os intervalos coletaram e armazenaram as informações de data e local do usuário. Este exemplo pressupõe que o programa `MyWeatherFunction` será chamado e usará os parâmetros `location` e `date` para retornar um objeto JSON `{"forecast": "<value>"}`.

 ``` json
{
  "ações": [ {
      "name": "MyWeatherFunction",
      "type": "client",
      "parameters": {
        "date": "$date",
        "location": "$location"
      },
      "result_variable": "context.my_forecast"
    }
  ]
}
```
{: codeblock}

Normalmente, o serviço realiza o retorno ao cliente por meio de uma solicitação POST '/message' apenas quando a entrada do novo usuário é necessária, como após a execução de um pai e antes da execução de um de seus nós-filhos. No entanto, se você inclui uma ação do cliente em um nó, então após a avaliação, o serviço sempre é retornado ao cliente para que o resultado da chamada de ação possa ser retornado. Para evitar a espera da entrada do usuário quando não deveria, como para um nó que está configurado para ir diretamente para um nó-filho, o serviço inclui o valor a seguir no contexto da mensagem:

```json
{
    "context": {
      "skip_user_input": true
    }
  }
```
{: codeblock}

Se você deseja que o cliente execute uma ação, mas não obtenha a entrada do usuário, é possível seguir a mesma convenção e incluir a variável de contexto `skip_user_input` no nó pai para comunicar isso ao aplicativo cliente.

Seu aplicativo cliente deve sempre verificar a variável `skip_user_input` no contexto. Se presente, ele não solicita nova entrada do usuário, mas em vez disso executa a ação, inclui seu resultado na mensagem e a transmite de volta para o serviço. A nova solicitação de mensagem POST deve incluir a mensagem retornada pela resposta de mensagem POST anterior (ou seja, o contexto, entrada, intenções, entidades e, opcionalmente, a seção de saída) e, em vez do objeto JSON que define a chamada programática a ser feita, ela deve incluir o resultado que foi retornado da chamada programática.

Em um nó-filho para o qual você vai depois desse nó, inclua a resposta a ser mostrada para o usuário:

``` json {
  "output": {
    "text": {
      "values": [
        "It will be $my_forecast $date.literal in $location.literal."
      ]
    }
  }
```
{: codeblock}
