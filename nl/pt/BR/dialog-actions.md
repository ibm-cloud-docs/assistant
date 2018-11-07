---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-15"

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

# Fazendo chamadas programáticas de um nó de diálogo ![BETA](images/beta.png)
{: #dialog-actions}

Defina as ações que podem fazer chamadas programáticas para aplicativos ou serviços externos e obtenha de volta um resultado como parte do processamento que ocorre dentro de uma rodada de diálogo.
{: shortdesc}

É possível usar um serviço externo para validar as informações que você coletou do usuário ou executar cálculos ou manipulações de sequência na entrada que são muito complexos para serem manipulados usando expressões e métodos SpEL suportados. Ou é possível interagir com um serviço da web externo para obter informações, como um serviço de tráfego aéreo para verificar o horário de chegada esperado de um voo ou um serviço meteorológico para obter uma previsão. É possível até mesmo interagir com um aplicativo externo, como um site de reserva de restaurante, para concluir uma transação simples em nome do usuário.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/y0A6X-KNoB8?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Ao definir a chamada programática, você escolhe um dos tipos a seguir:

- **client**: define uma chamada programática em um formato padronizado que seu aplicativo cliente externo pode usar para executar a chamada ou função programática e retornar o resultado para o diálogo.
- **server**: chama uma ação do {{site.data.keyword.openwhisk_short}} diretamente e retorna o resultado para o diálogo.

    Atualmente, é possível chamar uma ação do {{site.data.keyword.openwhisk_short}} de instâncias do {{site.data.keyword.conversationshort}} que são hospedadas nas regiões de sul dos EUA ou Alemanha.

    **Nota**: a instância do {{site.data.keyword.openwhisk_short}} que usada é aquela hospedada no mesmo local (sul dos EUA ou Alemanha). Portanto, não defina uma ação em uma instância do {{site.data.keyword.openwhisk_short}} que esteja hospedada na Alemanha se você planeja acessá-la de uma instância de serviço do {{site.data.keyword.conversationshort}} hospedada no sul dos EUA, por exemplo.

    **Importante**: somente use esse método para fazer uma chamada para uma ação do {{site.data.keyword.openwhisk_short}} que você sabe que pode retornar em **menos de 5 segundos**. A solicitação para o {{site.data.keyword.openwhisk_short}} atingirá o tempo limite se uma chamada de serviço individual levar mais tempo que isso. E se seu diálogo fizer mais de uma chamada para um serviço externo, o período de tempo total permitido para as chamadas serem concluídas será de 7 segundos. Se as três primeiras chamadas forem concluídas em 2 segundos cada e a quarta levar mais de 1 segundo, a quarta chamada será interrompida e a mensagem de erro para a chamada indicará que a chamada não foi concluída. Para serviços menos eficientes que você precisa chamar, gerencie a chamada por meio do seu aplicativo cliente e transmita as informações para o diálogo como uma etapa separada.

## Procedimento
{: #call-action}

Para fazer uma chamada programática de um nó de diálogo, conclua as etapas a seguir:

1.  No nó de diálogo do qual você deseja fazer a chamada programática, abra o editor JSON.

    - Para fazer uma chamada programática que é executada após a resposta para um nó ser avaliada, abra o editor JSON para a resposta do nó.

      ![Mostra como acessar o editor da JSON associado com uma resposta de nó padrão.](images/contextvar-json-response.png)

      Se a configuração **Múltiplas respostas** é **Ativado** para o nó, deve-se clicar no ícone **Editar resposta** ![Editar resposta](images/edit-slot.png) antes de o menu **Opções** ![Resposta avançada](images/kabob.png) ficar visível.

      ![Mostra como acessar o editor da JSON associado com um nó padrão que tem múltiplas respostas condicionais ativadas para ele.](images/contextvar-json-multi-response.png)

    Se deseja exibir ou processar ainda mais a resposta do serviço externo na mesma rodada de diálogo, deve-se incluir um segundo nó que faça isso e ir para ele por meio desse nó.
    {: tip}

    - Para fazer uma chamada que possa ser usada por um intervalo individual, clique no ícone **Editar intervalo** ![Editar intervalo](images/edit-slot.png) para o intervalo e, em seguida, faça uma das coisas a seguir:

      - Para fazer uma chamada programática executada após a condição do intervalo ser avaliada como true, abra o editor JSON que está associado à condição do intervalo.

        ![Mostra como acessar o editor da JSON associado com uma condição do intervalo.](images/contextvar-json-slot-condition.png)

      - Para fazer uma chamada programática executada após o intervalo ser preenchido com êxito, abra o editor JSON que está associado com a resposta Localizado. Para fazer isso, no menu **Opções** ![Ícone Opções](images/kabob.png) para o intervalo, clique em **Ativar respostas condicionais**. Para a resposta Localizado, clique no ícone **Editar resposta** ![Editar resposta](images/edit-slot.png). No menu **Opções** ![Ícone Opções](images/kabob.png) para a resposta Localizado, clique em **Abrir editor JSON**.

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
          "type":"client | server",
          "parameters": {
            "<parameter_name>":"<parameter_value>",
            "<parameter_name>":"<parameter_value>"
          },
          "result_variable": "<result_variable_name>",
          "credentials": "<reference_to_credentials>"
        }
      ],
      "output": {
        "text": "response text"
      }
    }
    ```
    {: codeblock}

    A matriz `actions` especifica as chamadas programáticas a serem feitas no diálogo. É possível definir até 5 chamadas programáticas separadas. Especifique os pares de nome e valor a seguir na matriz JSON:

    - `<actionName>`: necessário. O nome da ação ou do serviço a ser chamado. O nome não pode ter mais de 64 caracteres.

       - Para tipos de ação do cliente, especifique um nome em qualquer sintaxe que você desejar. O objetivo é especificar um nome que seu aplicativo cliente reconheça e saiba como manipular.

          Por exemplo: `calculateRate`

       - Para os tipos de ação do {{site.data.keyword.openwhisk_short}} (servidor), use esta sintaxe para fornecer o nome completo da ação: `/<namespace>/[<package-name>]/<action name>`

         - Se a ação fizer parte de um pacote, as informações do `<package-name>` serão necessárias; caso contrário, não.
         - Se você estiver chamando uma sequência de ações, especifique o `<sequence name>` no lugar do `<action name>`.
         - O namespace para uma ação definida pelo usuário geralmente tem a sintaxe: `<myIBMCloudOrganizationID>_<myIBMCloudSpace>`. Por exemplo: `/jdoeorg_prod10/search flights`
         - As ações que são fornecidas com o {{site.data.keyword.openwhisk_short}} frequentemente têm o namespace: `whisk.system`, mas é sempre necessário verificar o namespace para ter certeza. Por exemplo: `/whisk.system/weather/forecast`

    - `<type>`: indica o tipo de chamada a ser feita. Escolha entre os tipos a seguir:

      - **client**: envia uma resposta de mensagem com informações de chamada programática em um formato padronizado que seu aplicativo cliente externo pode usar para executar a chamada ou função e obter um resultado em nome do diálogo. O objeto JSON no corpo de resposta especifica o serviço ou a função a ser chamada, quaisquer parâmetros associados para passar com a chamada e como o resultado deve ser enviado de volta.

      - **server**: chama uma ação do {{site.data.keyword.openwhisk_short}} (uma ou mais) diretamente. Deve-se definir a ação em si separadamente usando o {{site.data.keyword.openwhisk}}. Para obter mais informações, veja [Criando uma ação do {{site.data.keyword.openwhisk_short}}](dialog-actions.html#create-action) abaixo.

      A especificação do tipo é opcional. O valor padrão é `client`.

    - `<action_parameters>`: quaisquer parâmetros que são esperados pelo programa externo, especificado como um objeto JSON. Os parâmetros são necessários somente se requeridos pelo programa externo.

    - `<result_variable_name>`: o nome a ser usado para referenciar o objeto JSON que é retornado pelo serviço ou programa externo. O resultado é incluído na seção de contexto da resposta /message. Em outras palavras, o resultado é armazenado como uma variável de contexto para que possa ser exibido na resposta do nó ou acessado por nós de diálogo que forem acionados mais tarde. Qualquer valor existente para a variável de contexto é sobrescrito pelo valor retornado pela ação. É possível especificar o `result_variable_name` usando a sintaxe a seguir:

      - `my_result`
      - `$my_result`

      O nome não pode ter mais que 64 caracteres. O nome de variável não pode conter os caracteres a seguir: parênteses `()`, colchetes (`[]`), uma aspa simples (`'`), uma aspa (`"`) ou uma barra invertida (`\`).

      Se você deseja salvar o resultado para a seção de saída ou entrada da resposta /message, é possível pré-anexar uma das palavras-chave de local a seguir ao `result_variable_name`:

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

      Se múltiplas ações em uma única matriz de ação JSON incluem o resultado de sua chamada programática para a mesma variável de contexto, a ordem na qual o contexto é atualizado importa:

      1. Se você tem uma combinação de ações do servidor e cliente na matriz, o serviço processa as ações do tipo do servidor primeiro. Como resultado, o valor que é calculado para a variável de contexto pela última ação do tipo de cliente na matriz sobrescreve o valor calculado para ela por quaisquer ações do tipo de servidor.
      1. Por tipo de ação, a ordem na qual as ações são definidas na matriz determina a ordem na qual o valor da variável de contexto é configurado. O valor da variável de contexto retornado pela última ação na matriz sobrescreve os valores calculados por quaisquer outras ações.

    - `<reference_to_credentials>`: o nome do objeto no qual as credenciais do {{site.data.keyword.openwhisk_short}} são armazenadas. Necessário somente para ações do servidor. Essas credenciais são usadas para acessar a instância do {{site.data.keyword.openwhisk_short}} na qual a ação é executada. Elas não são suas credenciais do {{site.data.keyword.Bluemix_notm}}.

      Para descobrir as credenciais, conclua as etapas a seguir:
      1.  Acesse a chave API do [{{site.data.keyword.openwhisk_short}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/openwhisk/learn/api-key){: new_window}.

          - Se você ainda não tiver criado uma conta, faça isso.
          - Se você não estiver com login efetuado, efetue login.

      1.  Clique no ícone **Mostrar chave de aut.** ![Mostrar chave de aut.](images/show-auth-icon.png) para mostrar as credenciais. O segmento antes de dois-pontos (:) é seu ID do usuário. O segmento após os dois-pontos é sua senha.

      **Atenção**: quaisquer encargos incorridos quando a ação é executada são cobrados da pessoa que possui essas credenciais.

      Para proteger as credenciais, não as armazene na área de trabalho do {{site.data.keyword.conversationshort}}. Em vez disso, passe-as do aplicativo cliente como parte do contexto. É possível evitar que as informações sejam armazenadas em logs do Watson aninhando sua variável de contexto dentro da seção $private do contexto da mensagem. Por exemplo: `$private.my_credentials`.

      O objeto de credenciais que você define deve conter os parâmetros `user` e `password`.

      ```json
      {
        "user":"5tj3b41j-bf3j-5d92-24g9-4a7769ab12af",
        "password":"y65gqSTSRzqE..."
      }
      ```
      {: codeblock}

      Ao testar o diálogo, é possível configurar temporariamente a variável de contexto `$private.my_credentials` com seus valores reais de nome do usuário e senha do {{site.data.keyword.openwhisk_short}} clicando em **Gerenciar contexto** na área de janela "Experimente" no conjunto de ferramentas.

      ![Mostra como a variável de contexto $private.my_credentials é definida pela interface de gerenciamento de contexto de Experimente](images/testing-creds.png)

## Criando uma ação do {{site.data.keyword.openwhisk_short}}
{: #create-action}

Se você escolhe definir uma chamada programática de tipo de servidor, então antes de poder chamá-la em um diálogo, deve-se criar a ação no {{site.data.keyword.openwhisk}}. Se você está definindo uma chamada programática de tipo de cliente, ignore esse procedimento.

**Nota:** a execução de uma ação do {{site.data.keyword.openwhisk_short}} pode incorrer em um custo. Para obter mais informações, veja [Precificação ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/openwhisk/learn/pricing){: new_window}. O {{site.data.keyword.openwhisk_short}} não distingue entre chamadas que são feitas da área de janela "Experimente" durante o teste e chamadas que são feitas de um aplicativo em produção.

Para criar uma ação do {{site.data.keyword.openwhisk_short}}, conclua as etapas a seguir:

1.  Acesse o [editor do {{site.data.keyword.openwhisk_short}} on-line ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.ng.bluemix.net/openwhisk/create){: new_window}, no qual é possível gravar o código diretamente em seu navegador.

    Há também uma [interface da linha de comandos ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/openwhisk/learn/cli){: new_window} que é possível instalar, que permite definir uma ação usando o código escrito localmente.

1.  Crie uma ação do {{site.data.keyword.openwhisk_short}} usando uma das linguagens de programação suportadas. Veja a documentação do [{{site.data.keyword.openwhisk_short}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.ng.bluemix.net/docs/openwhisk/openwhisk_actions.html){: new_window} para obter detalhes.

    Tenha em mente as dicas a seguir:

    - Consulte o [exemplo de uma ação do {{site.data.keyword.openwhisk_short}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.ng.bluemix.net/docs/openwhisk/openwhisk_actions.html#openwhisk_apicall_action){: new_window} para ver como chamar um serviço externo.
    - Para fazer uma chamada para um serviço do Watson, use o [Watson Developer Cloud SDK ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/watson-developer-cloud){: new_window} para a linguagem que você deseja usar.
    - Certifique-se de que sua ação do {{site.data.keyword.openwhisk_short}} aceite quaisquer parâmetros de entrada como um objeto JSON e retorne qualquer saída como um objeto JSON.
    - Se você estiver usando Node.js para escrever sua ação do {{site.data.keyword.openwhisk_short}}, certifique-se de usar `Promise` para processamento assíncrono. Certifique-se também de retornar o resultado final da função `main`.

    Como alternativa, é possível criar uma sequência de ações.
    {: tip}

## Manipulando erros

Se a ação do {{site.data.keyword.openwhisk_short}} encontra um erro, a mensagem de erro é retornada ao diálogo e armazenada como uma propriedade da variável de resposta denominada `cloud_functions_call_error`. O erro pode ocorrer se sua ação do {{site.data.keyword.openwhisk_short}} não pode obter uma resposta de um serviço externo ou se a ação do Cloud Function falha, por exemplo. Se as credenciais do Cloud Function não são fornecidas ou estão incorretas, um erro é retornado. Essa variável de contexto é usada somente para ações do servidor; em seu aplicativo cliente, considere criar um objeto semelhante que capture informações de erro e as retorna ao diálogo como uma variável de contexto.

É possível condicionar a resposta do nó de diálogo para primeiro verificar erros. Por exemplo, é possível assegurar que a resposta que referencia um resultado da ação do {{site.data.keyword.openwhisk_short}} seja mostrada somente se nenhum erro foi encontrado, incluindo esta expressão na condição de resposta:

```json
  $forecast_result.cloud_functions_call_error == null
```
{: codeblock}

Para uma chamada programática de tipo de cliente, é possível passar informações sobre o processamento de erro definindo uma variável de contexto, como `action_error`. É possível passá-las de volta para o serviço como parte da variável de resultado. Será possível então exibir uma resposta somente se nenhum erro tiver sido encontrado, definindo uma condição de resposta como esta:

```json
  $forecast_result.action_error == null
```
{: codeblock}

## Exemplo de chamada de cliente
{: #action-client-example}

O exemplo a seguir mostra como uma chamada para um serviço meteorológico externo pode se parecer. Ele é incluído no editor JSON associado à resposta do nó. Até a resposta no nível do nó ser acionada, os intervalos coletaram e armazenaram as informações de data e local do usuário. Este exemplo assume que o serviço que será chamado tem um terminal nomeado `/weather` e que toma os parâmetros `location` e `date` e retorna um objeto JSON, `{"forecast": "<value>"}`.

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

Normalmente, o serviço somente é retornado ao cliente de uma solicitação POST /message quando a nova entrada do usuário é necessária, como depois de executar um pai e antes de executar um dos seus nós-filhos. No entanto, se você inclui uma ação do cliente em um nó, então após a avaliação, o serviço sempre é retornado ao cliente para que o resultado da chamada de ação possa ser retornado. Para evitar a espera da entrada do usuário quando não deveria, como para um nó que está configurado para ir diretamente para um nó-filho, o serviço inclui o valor a seguir no contexto da mensagem:

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

O diagrama a seguir ilustra como é possível usar uma chamada do cliente para obter informações de previsão meteorológica e retorná-la ao usuário.

![Mostra alguém solicitando uma previsão meteorológica e o diálogo enviando a solicitação a um aplicativo do cliente, que então a envia ao serviço externo](images/forecast.png)

## Exemplo de chamada do servidor
{: #action-server-example}

O exemplo a seguir mostra como uma chamada a uma ação do {{site.data.keyword.openwhisk_short}} pode se parecer. Este exemplo mostra como usar a ação `echo` do {{site.data.keyword.openwhisk_short}} que está definida no [pacote de Utilitários ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/docs/openwhisk/openwhisk_actions.html#openwhisk_create_action_sequence){: new_window} fornecido com o serviço. A ação toma uma sequência de texto e a retorna.

 ``` json
     {
  "ações": [ {
      "name": "/whisk.system/utils/echo",
      "type":"server",
      "parameters": {
        "message": "<?input.text?>"
      },
      "result_variable": "context.my_input_returned",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

A saída da ação do {{site.data.keyword.openwhisk_short}}, que é armazenada na variável `context.my_input_returned`, agora pode ser acessada por nós de diálogo subsequentes.

 ``` json
     {
  "output": {
    "text": {
      "values": [
        "Your input was: $my_input_returned."
      ]
    }
  }
}
```
{: codeblock}

O diagrama a seguir ilustra como chamar uma ação do {{site.data.keyword.openwhisk_short}} com um exemplo simples que chama o serviço de eco integrado do {{site.data.keyword.openwhisk_short}}. Ele pede ao usuário a entrada e a passa para o serviço de eco. O serviço de eco retorna o mesmo texto de volta, que é exibido para o usuário.

![Mostra alguém inserindo texto e o diálogo enviando a solicitação para o serviço {{site.data.keyword.openwhisk_short}}, retornando o texto para o diálogo.](images/echo-via-cf.png)

### Exemplo de ação de eco

Para ver uma área de trabalho com um diálogo já configurado para chamar a ação de Eco integrada do {{site.data.keyword.openwhisk_short}}, conclua as etapas a seguir:

1.  Faça download do arquivo [CloudFunctionsEcho.json ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/watson-developer-cloud/community/raw/master/conversation/cloud-functions-echo.json){: new_window}.
1.  Importe o arquivo JSON como uma nova área de trabalho.
1.  Revise o diálogo para ver como a chamada para a ação de Eco é especificada.
1.  Na área de janela "Experimente", clique em **Gerenciar contexto** e, em seguida, configure (temporariamente) as variáveis de contexto para seu nome do usuário e senha do {{site.data.keyword.openwhisk_short}}.

    ```json
    $private.my_credentials
    {
      "user":"<your-CF-instance-username>",
      "password":"<your-CF-instance-password>"
    }
    ```
    {: codeblock}

1.  Teste o diálogo inserindo uma entrada.

    O serviço usará a ação de Eco do {{site.data.keyword.openwhisk_short}} para repetir o que você inserir.

## Exemplo de chamada do servidor avançado
{: #advanced-action-server-example}

É possível chamar múltiplas ações de dentro de um único fluxo de diálogo. Na verdade, é possível chamar até cinco ações dentro de um objeto JSON `actions` em um único nó de diálogo. No entanto, quaisquer ações de tipo de servidor definidas em uma matriz JSON `actions` são todas processadas em paralelo. Portanto, não é possível chamar uma ação de tipo de servidor e passar o resultado disso para uma segunda ação de tipo de servidor no mesmo bloco `actions`. A melhor maneira de chamar ações do servidor em uma ordem específica é usar uma sequência do {{site.data.keyword.openwhisk_short}}. No tempo de execução, essa abordagem é mais rápida porque o diálogo somente tem que fazer uma chamada externa para concluir múltiplas ações. Para usar uma sequência, apenas referencie o nome da sequência em vez de um nome de ação na definição de bloco `actions`. Como alternativa, é possível chamar a primeira ação de tipo de servidor de um nó e ir para um nó-filho que chame a próxima ação de tipo de servidor.

Se você define uma matriz `actions` que tem uma combinação de ações de tipo de cliente e tipo de servidor, quando o nó de diálogo é executado, as ações do cliente não são enviadas para o cliente até que todas as ações de servidor tenham sido processadas.
{: tip}

Os exemplos a seguir mostram como uma chamada para uma ação do {{site.data.keyword.openwhisk_short}} pode se parecer.

Este exemplo mostra como usar uma ação do {{site.data.keyword.openwhisk_short}} para chamar um serviço externo que toma um nome de cidade e retorna as coordenadas de latitude e longitude do local fornecido.

``` json {
  "ações": [ {
      "name": "/jdoeorg_prod/get coordinates",
      "type":"server",
      "parameters": {
        "location": "$location"
      },
      "result_variable": "context.my_coordinates",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

A variável de contexto `$my_coordinates` salva os dois valores que são retornados pelo serviço `get coordinates` como este:

```json
{
 "lat":"42.3611",
 "long":"-71.0571"
}
```
{: codeblock}

Este exemplo mostra como usar a ação `forecast` do {{site.data.keyword.openwhisk_short}} que está definida no [pacote Weather ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.bluemix.net/docs/openwhisk/openwhisk_weather.html#openwhisk_catalog_weather){: new_window} fornecido com o serviço {{site.data.keyword.openwhisk_short}}. A ação espera as coordenadas de latitude e longitude e um período. Ela retorna um objeto JSON com informações de previsão para o local especificado durante o período especificado. As coordenadas, que são retornadas pela ação anterior, são especificadas como `$my_coordinates.lat` e `$my_coordinates.long`.

 ``` json {
  "ações": [ {
      "name": "/whisk.system/weather/forecast",
      "type":"server",
      "parameters": {
        "latitude": "$my_coordinates.lat",
        "longitude": "$my_coordinates.long",
        "timePeriod": "$period",
        "username": "$private.my_weather_service.username",
        "password": "$private.my_weather_service.password"
      },
      "result_variable": "context.forecasts",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

**Nota**: um nome do usuário e uma senha são listados como parâmetros. Eles estão presentes somente porque essa ação específica os requer; juntos eles definem as credenciais requeridas pelo serviço Weather externo que a ação fornecida chama no backend. Eles são diferentes das credenciais de conta do IBM Cloud Function. Execute também as etapas para manter essas credenciais privadas.

A saída da ação do {{site.data.keyword.openwhisk_short}}, que é armazenada na variável `context.forecasts`, agora pode ser acessada pelos nós de diálogo subsequentes.

 ``` json {
  "output": {
    "text": {
      "values": [
        "For the next $period in $location, you can expect $forecasts."
      ]
    }
  }
}
```
{: codeblock}

O diagrama a seguir ilustra uma interação complexa. Um usuário pede uma previsão meteorológica. Os intervalos no nó de diálogo solicitam ao usuário as informações de local e de período. Para chamar o serviço de previsão do {{site.data.keyword.openwhisk_short}}, deve-se fornecer coordenadas geográficas. Portanto, o diálogo primeiro obtém detalhes de latitude e longitude para o local fornecido pelo usuário enviando uma solicitação ao serviço {{site.data.keyword.openwhisk_short}}, que a passa para um serviço externo que retorna as informações de coordenadas. Em um nó subsequente, o diálogo envia as informações de coordenadas recém-obtidas junto com as informações do período obtidas do usuário para o serviço de previsão integrado do {{site.data.keyword.openwhisk_short}} para obter os detalhes da previsão. O diálogo então responde a pergunta do usuário mostrando o resultado de previsão que foi fornecido pelo serviço de previsão do {{site.data.keyword.openwhisk_short}}.

![Mostra alguém solicitando uma previsão meteorológica e o diálogo enviando a solicitação ao serviço Cloud Function para passá-la ao serviço externo.](images/forecast-via-cf.png)
