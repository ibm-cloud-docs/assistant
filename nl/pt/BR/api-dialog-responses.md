---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-14"

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

# Implementando Respostas
{: #api-dialog-responses}

Um nó de diálogo pode responder aos usuários com uma resposta que inclua texto, imagens ou elementos interativos, tais como opções clicáveis. Se você está construindo seu próprio aplicativo cliente, deve-se implementar a exibição correta de todos os tipos de resposta retornados por seu diálogo. (Para obter mais informações sobre respostas de diálogo, consulte [Respostas](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses)).

## Formato da saída de resposta
{: #api-dialog-responses-output}

Por padrão, as respostas de um nó de diálogo são especificadas no objeto `output.generic` do JSON de resposta retornado da API `/message`. O objeto `generic` contém uma matriz de até 5 elementos de resposta que são destinados a qualquer canal. O exemplo de JSON a seguir mostra uma resposta que inclui texto e uma imagem:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "OK, here's a picture of a dog."
      },
      {
        "response_type": "image",
        "source": "http://example.com/dog.jpg"
      }
    ],
    "text" : ["OK, here's a picture of a dog."]
  }
}
```

Como mostrado nesse exemplo, a resposta de texto (`OK, here's a picture of a dog.`) também é retornada na matriz `output.text`. Isso é incluído para a compatibilidade com versões anteriores de aplicativos mais antigos que não suportam o formato `output.generic`.
{: note}

É responsabilidade do seu aplicativo cliente manipular todos os tipos de resposta apropriadamente. Nesse caso, seu aplicativo precisaria exibir o texto e a imagem especificados para o usuário.

## Tipos de resposta
{: #api-dialog-responses-types}

Cada elemento de uma resposta é de um dos tipos de resposta suportados (atualmente `image`, `option`, `pause`, `text` e `suggestion`). Cada tipo de resposta é especificado usando um conjunto diferente de propriedades JSON, portanto, as propriedades incluídas para cada resposta variarão dependendo do tipo de resposta. Para obter informações completas sobre o modelo de resposta da API `/message`, consulte a [Referência de API ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/apidocs/assistant-v2#send-user-input-to-assistant){: new_window}.

Esta seção descreve os tipos de resposta disponíveis e como eles são representados no JSON de resposta da API `/message` (se estiver usando o SDK do Watson, será possível usar as interfaces fornecidas para seu idioma para acessar os mesmos objetos).

Os exemplos nesta seção mostram o formato dos dados JSON retornados da `/message API` no tempo de execução. Tenha em mente que isso é diferente do formato JSON usado para definir respostas dentro de um nó de diálogo. Para obter mais informações, consulte [Definindo respostas por meio do editor JSON](/docs/services/assistant?topic=assistant-dialog-responses-json).
{: note}

### Texto
{: #api-dialog-responses-text}

O tipo de resposta `text` é usado para respostas de texto ordinário por meio do diálogo:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "text",
        "text": "OK, you want to fly to Boston next Monday."
      }
    ]
  }
}
```

Observe que, para compatibilidade com versões anteriores, o mesmo texto também é incluído na matriz `output.text` na resposta do diálogo (conforme mostrado no exemplo no início desta página).

### Imagem
{: #api-dialog-responses-image}

O tipo de resposta `image` instrui o aplicativo cliente a exibir uma imagem, acompanhada opcionalmente por um título e descrição:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "image",
        "source": "http://example.com/image.jpg",
        "title": "Image example",
        "description": "This is an example image"
      }
    ]
  }
}
```

Seu aplicativo é responsável por recuperar a imagem especificada pela propriedade `source` e exibi-la para o usuário. Se o `title` e ` description` opcionais forem fornecidos, seu aplicativo poderá exibi-los de qualquer maneira que seja apropriada (por exemplo, renderizando o título abaixo da imagem e a descrição como texto de ajuda instantânea).

### Pausar
{: #api-dialog-responses-pause}

O tipo de resposta `pause` instrui o aplicativo a aguardar por um intervalo especificado antes de exibir a próxima resposta:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "pause",
        "time": 500,
        "typing": false
      }
    ]
  }
}
```

Essa pausa pode ser solicitada pelo diálogo para permitir tempo para que uma solicitação seja concluída ou simplesmente para imitar a aparência de um agente humano que possa pausar entre as respostas. A pausa pode ser de qualquer duração até 10 segundos.

Uma resposta `pause` é geralmente enviada em combinação com outras respostas. Seu aplicativo deve pausar pelo intervalo especificado pela propriedade `time` (em milissegundos) antes de exibir a próxima resposta na matriz. A propriedade `typing` opcional solicita que o aplicativo cliente mostre um indicador "user is typing", se suportado, para simular um agente humano.

### Opção
{: #api-dialog-responses-option}

O tipo de resposta `option` instrui o aplicativo cliente a exibir um controle da interface com o usuário que permite que o usuário realize uma seleção em uma lista de opções e, em seguida, envie a entrada de volta para o assistente com base na opção selecionada:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "option",
        "title": "Available options",
        "description": "Please select one of the following options:",
        "preference": "button",
        "options": [
          {
            "label": "Option 1",
            "value": {
              "input": {
                "text": "option 1"
              }
            }
          },
          {
            "label": "Option 2",
            "value": {
              "input": {
                "text": "option 2"
              }
            }
          }
        ]
      }
    ]
  }
}
```

Seu app pode exibir as opções especificadas usando qualquer controle apropriado da interface com o usuário (por exemplo, um conjunto de botões ou uma lista suspensa). A propriedade `preference` opcional indica o tipo preferencial de controle que seu app deve usar (`button` ou `dropdown`), se suportado. Para uma experiência de usuário melhor, uma boa prática é apresentar três opções ou menos como botões e mais de três opções como uma lista suspensa.

Para cada opção, a propriedade `label` especifica o texto do rótulo que deve aparecer para a opção no controle de IU. A propriedade `value` especifica a entrada que deve ser enviada de volta ao assistente (usando a API `/message`) quando o usuário seleciona a opção correspondente.

Para obter um exemplo da implementação de respostas `option` em um aplicativo cliente simples, consulte [Exemplo: implementando respostas de opção](#api-dialog-option-example).

### Sugestão
{: #api-dialog-responses-suggestion}

Esse recurso está disponível somente para usuários do plano Plus ou Premium.
{: tip}

O tipo de resposta `suggestion` é usado pelo recurso de desambiguação para sugerir correspondências possíveis quando não estiver claro o que o usuário deseja fazer. Uma resposta `suggestion` inclui uma matriz de `suggestions`, cada uma com um nó de diálogo possivelmente correspondente:

```json
{
  "output": {
    "generic":[
      {
        "response_type": "suggestion",
        "title": "Did you mean:",
        "suggestions": [
          {
            "label": "I'd like to order a drink.", "value": {
              "intents": [ {
                  "intent": "order_drink",
                  "confidence": 0.7330395221710206
                }
              ], "entities": [], "input": {
                "suggestion_id": "576aba3c-85b9-411a-8032-28af2ba95b13",
                "text": "I want to place an order"
              }
            },
  "output": {
              "text": [
                "I'll get you a drink."
              ], "generic": [ {
                  "response_type": "text",
                  "text": "I'll get you a drink."
                }
              ], "nodes_visited_details": [ {
                  "dialog_node": "node_1_1547675028546",
                  "title": "order drink",
                  "user_label": "I'd like to order a drink.",
                  "conditions": "#order_drink"
                }
              ]
            },
            "source_dialog_node": "root"
          },
          {
            "label": "I need a drink refill.", "value": {
              "intents": [ {
                  "intent": "refill_drink",
                  "confidence": 0.2529746770858765
                }
              ], "entities": [], "input": {
                "suggestion_id": "6583b547-53ff-4e7b-97c6-4d062270abcd",
                "text": "I need a drink refill"
              }
            },
  "output": {
              "text": [
                "I'll get you a refill."
              ], "generic": [ {
                  "response_type": "text",
                  "text": "I'll get you a refill."
                }
              ], "nodes_visited_details": [ {
                  "dialog_node": "node_2_1547675097178",
                  "title": "refill drink",
                  "user_label": "I need a drink refill.",
                  "conditions": "#refill_drink"
                }
              ]
            },
            "source_dialog_node": "root"
          }
        ]
      }
    ],
  }
}
```

Observe que a estrutura de uma resposta `suggestion` é muito semelhante à estrutura de uma resposta `option`. Assim como ocorre com as opções, cada sugestão inclui um `label` que pode ser exibido ao usuário e um `value` que especifica a entrada que deve ser enviada de volta ao assistente caso o usuário escolha a sugestão correspondente. Para implementar respostas `suggestion` em seu aplicativo, é possível usar a mesma abordagem usada para respostas `option`.

Para obter mais informações sobre o recurso de desambiguação, consulte [Desambiguação](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).

## Exemplo: implementando respostas de opção
{: #api-dialog-option-example}

Para mostrar como um aplicativo cliente pode lidar com respostas de opção, que solicitam que o usuário realize uma seleção em uma lista de opções, podemos estender o exemplo de cliente descrito em [Construindo um aplicativo cliente](/docs/services/assistant?topic=assistant-api-client). Este é um aplicativo cliente simplificado, que usa a entrada e a saída padrão para lidar com três intenções (envio de uma saudação, exibição do horário atual e saída do aplicativo):

```
Welcome to the {{site.data.keyword.conversationshort}} example!
>> hello
Good day to you.
>> what time is it?
The current time is 12:40:42 PM.
>> goodbye
OK! See you later.
```
{: screen}

Para experimentar o código de exemplo mostrado neste tópico, é necessário primeiro configurar a área de trabalho necessária e obter os detalhes da API necessários. Para obter mais informações, consulte [Construindo um aplicativo cliente](/docs/services/assistant?topic=assistant-api-client).
{: note}

### Recebendo uma resposta de opção

A resposta `option` pode ser usada para apresentar ao usuário uma lista finita de opções, em vez de interpretar a entrada de língua natural. Isso pode ser usado em qualquer situação na qual você queira permitir que o usuário realize uma rápida seleção em um conjunto de opções não ambíguas.

Em nosso aplicativo cliente simplificado, usaremos esse recurso para fazer uma seleção em uma lista de ações suportadas pelo assistente (saudações, exibição do horário e saída). Além das três intenções anteriormente mostradas (`#hello`, `#time` e `#goodbye`), a área de trabalho de exemplo suporta uma quarta intenção, `#menu`, que é correspondida quando o usuário solicita a exibição de uma lista de ações disponíveis.

Quando a área de trabalho reconhece a intenção `#menu`, o diálogo responde com uma resposta `option`:

```json
{
  "output": {
    "generic":[
      {
        "title": "What do you want to do?",
        "options": [
          {
            "label": "Send greeting",
            "value": {
              "input": {
                "text": "hello"
              }
            }
          },
          {
            "label": "Display the local time",
            "value": {
              "input": {
                "text": "time"
              }
            }
          },
          {
            "label": "Exit",
      "value": {
              "input": {
                "text": "goodbye"
              }
            }
          }
        ],
        "response_type": "option"
      }
    ],
    "intents": [
      {
        "intent": "menu",
        "confidence": 0.6178638458251953
      }
    ],
    "entities": []
  }
}
```

A resposta `option` contém diversas opções a serem apresentadas ao usuário. Cada uma delas inclui dois objetos, `label` e `value`. O objeto `label` é uma sequência voltada ao usuário que identifica a opção e o objeto `value` especifica a entrada de mensagem correspondente que deve ser enviada de volta ao assistente caso o usuário escolha a opção.

Nosso aplicativo cliente precisará usar os dados nessa resposta para construir a saída que mostramos ao usuário e enviar a mensagem apropriada ao assistente.

### Listando opções disponíveis

A primeira etapa ao lidar com uma resposta de opção é exibir as opções ao usuário, usando o texto especificado pela propriedade `label` de cada opção. É possível exibir as opções por meio de qualquer técnica suportada por seu aplicativo, geralmente uma lista suspensa ou um conjunto de botões clicáveis (a propriedade opcional `preference` de uma resposta de opção, se especificada, indicará qual tipo de exibição o aplicativo usará, se possível).

Nosso exemplo simplificado usa a entrada e a saída padrão, portanto, não temos acesso a uma IU real. Em vez disso, apresentaremos as opções apenas como uma lista numerada.

```javascript
// Option example 1: lists options.

const prompt = require('prompt-sync')(); const AssistantV2 = require('ibm-watson/assistant/v2');

// Configurar o wrapper de serviço do Assistente.
const service = new AssistantV2({
  iam_apikey: '{apikey}', // replace with API key version: '2019-02-28', });

const assistantId = '{assistant_id}'; // replace with assistant ID let sessionId;

// Create session.
service .createSession({ assistant_id: assistantId, })
  .then (res = >{
    sessionId = res.session_id; sendMessage({text: ''}); // start conversation with empty message })
  .catch(err => {
    console.log(err); // something went wrong });

// Send message to assistant.
function sendMessage(messageInput) {
  service .message({ assistant_id: assistantId, session_id: sessionId, input: messageInput, })
    .then (res = >{
      processResponse(res); })
    .catch(err => {
      console.log(err); // something went wrong }); }

// Processe a resposta.
function processResponse(response) {

  let endConversation = false;

  // Check for client actions requested by the assistant.
  if (response.output.actions) {
    if (response.output.actions[0].type === 'client'){
      if (response.output.actions[0].name === 'display_time') {
        // User asked what time it is, so we output the local system time.
        console.log('The current time is ' + new Date().toLocaleTimeString() + '.');
      } else if (response.output.actions[0].name === 'end_conversation') {
        // User said goodbye, so we're done.
        console.log(response.output.generic[0].text);
        endConversation = true;
      }
    }
  } else {
    // Display the output from assistant, if any. Supports only a single // response.
    if (response.output.generic) {
      if (response.output.generic.length > 0) {
        switch (response.output.generic[0].response_type) {
          case 'text': // It's a text response, so we just display it.
            console.log(response.output.generic[0].text); break; case 'option': // It's an option response, so we'll need to show the user // a list of choices.
            console.log(response.output.generic[0].title); const options = response.output.generic[0].options; // List the options by label.
            for (let i = 0; i < options.length; i++) {
              console.log((i+1).toString() + '. ' + options[i].label); } break; }
      }
    }
  }

  // If we're not done, prompt for the next round of input.
  if (!endConversation) {
    const newMessageFromUser = prompt('>> '); newMessageInput = {
      message_type: 'text', text: newMessageFromUser } sendMessage(newMessageInput); } else {
    // We're done, so we delete the session.
    service .deleteSession({ assistant_id: assistantId, session_id: sessionId, })
      .then (res = >{
        return; })
      .catch(err => {
        console.log(err); // something went wrong }); }
}
```
{: codeblock}
{: javascript}

A seguir, vejamos detalhes do código que cria a resposta do assistente. Agora, em vez de presumir uma resposta `text`, o aplicativo suporta os tipos de resposta `text` e `option`:

```javascript
    // Display the output from assistant, if any. Supports only a single // response.
    if (response.output.generic) {
      if (response.output.generic.length > 0) {
        switch (response.output.generic[0].response_type) {
          case 'text': // It's a text response, so we just display it.
            console.log(response.output.generic[0].text); break; case 'option': // It's an option response, so we'll need to show the user // a list of choices.
            console.log(response.output.generic[0].title); const options = response.output.generic[0].options; // List the options by label.
            for (let i = 0; i < options.length; i++) {
              console.log((i+1).toString() + '. ' + options[i].label); } break; }
      }
    }
```
{: codeblock}
{: javascript}

Se `response_type`=`text`, a saída será exibida normalmente, como antes. No entanto, se `response_type`=`option`, um pouco mais de trabalho será necessário. Primeiro, exibiremos o valor da propriedade `title`, que atua como texto introdutório para a lista de opções. Em seguida, listaremos as opções por meio do valor da propriedade `label` para identificar cada uma delas (um aplicativo real mostraria esses rótulos em uma lista suspensa ou como rótulos de botões clicáveis).

É possível ver o resultado acionando a intenção `#menu`:

```
Bem-vindo ao exemplo do Watson Assistant! > > what are the available actions?
O que você deseja fazer?
1. Enviar saudação 2. Exibir o horário local 3. Exit
>> 2
Sorry, I have no idea what you're talking about.
>>
```
{: screen}

Como é possível notar, o aplicativo agora está lidando corretamente com a resposta `option`, listando as opções disponíveis. No entanto, ainda não estamos traduzindo a opção do usuário em uma entrada significativa.

### Selecionando uma opção

Além de `label`, cada opção na resposta também inclui um objeto `value`, que contém os dados de entrada que devem ser enviados de volta ao assistente caso o usuário escolha a opção correspondente. O objeto `value.input` é equivalente à propriedade `input` da API `/message`, o que significa que basta enviá-lo de volta ao assistente no estado em que ele se encontra.

Para fazer isso em nosso aplicativo, configuramos um novo sinalizador `promptOption` quando o cliente recebe uma resposta `option`. Quando esse sinalizador é true, queremos realizar a próxima rodada de entradas de `value.input`, em vez de aceitar a entrada de texto em língua natural do usuário (novamente, não temos uma interface real com o usuário, portanto, vamos apenas solicitar que ele selecione uma opção válida na lista numerada).

```javascript
// Option example 2: sends back selected option value.

const prompt = require('prompt-sync')(); const AssistantV2 = require('ibm-watson/assistant/v2');

// Configurar o wrapper de serviço do Assistente.
const service = new AssistantV2({
  iam_apikey: 'AZkSnK4b40UI5kLepHKxIKYpVcxeg0yPLbVVwGFW8kjM', // replace with API key
  version: '2019-02-28',
});

const assistantId = 'dcd5c5ad-f3a1-4345-89c5-708b0b5ff4f7'; // replace with assistant ID
let sessionId;

// Create session.
service .createSession({ assistant_id: assistantId, })
  .then (res = >{
    sessionId = res.session_id; sendMessage({text: ''}); // start conversation with empty message })
  .catch(err => {
    console.log(err); // something went wrong });

// Send message to assistant.
function sendMessage(messageInput) {
  service .message({ assistant_id: assistantId, session_id: sessionId, input: messageInput, })
    .then (res = >{
      processResponse(res); })
    .catch(err => {
      console.log(err); // something went wrong }); }

// Processe a resposta.
function processResponse(response) {

  let endConversation = false;
  let promptOption = false;

  // Check for client actions requested by the assistant.
  if (response.output.actions) {
    if (response.output.actions[0].type === 'client'){
      if (response.output.actions[0].name === 'display_time') {
        // User asked what time it is, so we output the local system time.
        console.log('The current time is ' + new Date().toLocaleTimeString() + '.');
      } else if (response.output.actions[0].name === 'end_conversation') {
        // User said goodbye, so we're done.
        console.log(response.output.generic[0].text);
        endConversation = true;
      }
    }
  } else {
    // Display the output from assistant, if any. Supports only a single // response.
    if (response.output.generic) {
      if (response.output.generic.length > 0) {
        switch (response.output.generic[0].response_type) {
          case 'text': // It's a text response, so we just display it.
            console.log(response.output.generic[0].text); break; case 'option': // It's an option response, so we'll need to show the user // a list of choices.
              console.log(response.output.generic[0].title); const options = response.output.generic[0].options; // List the options by label.
              for (let i = 0; i < options.length; i++) {
                console.log((i+1).toString() + '. ' + options[i].label);
              }
            promptOption = true;
            break;
        }
      }
    }
  }

  // If we're not done, prompt for the next round of input.
  if (!endConversation) {
    let messageInput;
    if (promptOption == true) {
      // Prompt for a valid selection from the list of options.
      let choice;
      do {
        choice = prompt('? ');
          if (isNaN(choice)) {
            choice = 0;
          }
      } while (choice < 1 || choice > response.output.generic[0].options.length);
      const value = response.output.generic[0].options[choice-1].value;
      // Use message input from the selected option.
      messageInput = value.input;
    } else {
      // We're not showing options, so we just prompt for the next
      // round of input.
      const newText = prompt('>> ');
      messageInput = {
        text: newText
      }
    }
    sendMessage(messageInput); 
  } else {
    // We're done, so we delete the session.
    service .deleteSession({ assistant_id: assistantId, session_id: sessionId, })
      .then (res = >{
        return; })
      .catch(err => {
        console.log(err); // something went wrong }); }
}
```
{: codeblock}
{: javascript}

Tudo o que temos de fazer é usar o objeto `value.input` da resposta selecionada como a próxima rodada de entradas da mensagem, em vez de construir um novo objeto `input` usando a entrada de texto. Em seguida, o assistente responde exatamente o que responderia se o usuário tivesse digitado o texto de entrada diretamente.

```
Bem-vindo ao exemplo do Watson Assistant! > > hi
Good day to you.
>> what are the choices?
O que você deseja fazer?
1. Enviar saudação 2. Exibir o horário local 3. Exit
? 2
The current time is 1:29:14 PM.
>> bye
OK! See you later.
```
{: screen}

Agora, podemos acessar todas as funções do assistente, fazendo solicitações de língua natural ou seleções de um menu de opções.

Observe que a mesma abordagem também é usada para respostas `suggestion`. Se seu plano suportar o recurso de desambiguação, será possível usar uma lógica semelhante para solicitar que os usuários façam seleções em uma lista quando não estiver claro qual das diversas opções possíveis está correta. Para obter mais informações sobre o recurso de desambiguação, consulte [Desambiguação](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).
