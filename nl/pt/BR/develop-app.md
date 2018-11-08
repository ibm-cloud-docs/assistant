---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-29"

---

{:curl: #curl .ph data-hd-programlang='curl'}
{:javascript: #javascript .ph data-hd-programlang='javascript'}
{:java: #java .ph data-hd-programlang='java'}
{:python: #python .ph data-hd-programlang='python'}
{:swift: data-hd-programlang='swift'}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:download: .download}
{:tip: .tip}

# Construindo um aplicativo cliente

Então você tem um diálogo de trabalho. Agora você deseja desenvolver o aplicativo que irá interagir com seus usuários e se comunicar com o serviço do {{site.data.keyword.conversationfull}}.
{: shortdesc}

É possível visualizar este tutorial para o Node.js (Javascript) ou Python clicando no seletor de linguagem na parte superior direita. Para obter detalhes sobre todas as linguagens suportadas, consulte os {{site.data.keyword.watson}} [SDKs ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/services/watson/getting-started-sdks.html#sdks){: new_window}.
{: tip }

## Configurando o serviço {{site.data.keyword.conversationshort}}

O aplicativo de exemplo que criaremos nesta seção implementa várias funções de um assistente pessoal cognitivo. O código do aplicativo irá se conectar a uma área de trabalho do {{site.data.keyword.conversationshort}}, em que o processamento cognitivo (como a detecção de intenções de usuário) ocorre.

Antes de continuar com este exemplo, é necessário configurar a área de trabalho do {{site.data.keyword.conversationshort}} necessária:

1.  Faça download do <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/conversation/conversation-simple-example.json" download="conversation-simple-example.json">arquivo JSON</a> da área de trabalho.
1.  [Importe a área de trabalho](/docs/services/conversation/configure-workspace.html#creating-workspaces) para uma instância do serviço do {{site.data.keyword.conversationshort}}.

## Obtendo informações de serviço

Para acessar as APIs REST do serviço do {{site.data.keyword.conversationshort}}, seu aplicativo precisa ser capaz de autenticar-se no {{site.data.keyword.Bluemix}} e de se conectar à área de trabalho correta do {{site.data.keyword.conversationshort}}. Será necessário copiar as credenciais de serviço e o ID da área de trabalho e colá-los no código do aplicativo.

Para acessar as credenciais de serviço e o ID da área de trabalho de sua área de trabalho, selecione o menu ![Menu](images/Menu_16.png), escolha **Implementar** e, em seguida, acesse a guia **Credenciais**.

Também é possível acessar as credenciais de serviço de seu painel do {{site.data.keyword.Bluemix_short}}.

## Comunicando-se com o serviço {{site.data.keyword.conversationshort}}

Interagir com o serviço do {{site.data.keyword.conversationshort}} é simples. Vamos dar uma olhada em um exemplo que se conecta ao serviço, envia uma única mensagem e imprime a saída no console:

```javascript
// Example 1: sets up service wrapper, sends initial message, and
// receives response.

var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service wrapper.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with service username password: 'PASSWORD', // replace with service password version_date: '2017-05-26'
});

var workspace_id = 'WORKSPACE_ID'; // replace with workspace ID

// Start conversation with empty message.
conversation.message({ workspace_id: workspace_id }, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }
  
  // Display the output from dialog, if any.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }
}
```
{: codeblock}
{: javascript}

```python
# Example 1: sets up service wrapper, sends initial message, and
# receives response.

import watson_developer_cloud

# Set up Conversation service.
conversation = watson_developer_cloud.ConversationV1( username = 'USERNAME', # substituir pelo nome do usuário da senha de chave = 'PASSWORD', # substituir pela senha da versão de chave de serviço = '2017-05-26'
)
workspace_id = 'WORKSPACE_ID' # replace with workspace ID

# Start conversation with empty message.
response = conversation.message( workspace_id = workspace_id, input = {
    'text': ''
  }
)

# Print the output from dialog, if any.
if response['output']['text']:
  print(response['output']['text'][0])
```
{: codeblock}
{: python}

A primeira etapa é criar um wrapper para o serviço do {{site.data.keyword.conversationshort}}.

O wrapper é um objeto que você usará para enviar entrada para o serviço e para receber a saída dele. Ao criar o wrapper de serviço, especifique as credenciais de autenticação da chave de serviço, bem como a versão da API do {{site.data.keyword.conversationshort}} que você está usando.

Neste exemplo de Node.js, o wrapper é uma instância do `ConversationV1`, armazenado na variável `conversation`. Os SDKs do Watson para outros idiomas fornecem mecanismos equivalentes para instanciar um wrapper de serviço.
{: javascript}

Neste exemplo Python, o wrapper é uma instância do `watson_developer_cloud.ConversationV1`, armazenado na variável `conversation`. Os SDKs do Watson para outros idiomas fornecem mecanismos equivalentes para instanciar um wrapper de serviço.
{: python}

Depois de criar o wrapper de serviço, nós o usamos para enviar uma mensagem para o serviço do {{site.data.keyword.conversationshort}}. Neste exemplo, a mensagem está vazia; nós só queremos acionar o nó conversation_start no diálogo, então não precisamos de nenhum texto de entrada.

Use o comando `node <filename.js>` para executar o aplicativo de exemplo.
{: javascript}

Use o comando `python <filename.py>` para executar o aplicativo de exemplo.
{: python}

**Nota:** certifique-se de que tenha instalado o Watson SDK for Node.js usando `npm install watson-developer-cloud`.
{: javascript}

**Nota:** certifique-se de que você tenha instalado o Watson SDK for Python usando `pip install --upgrade watson-developer-cloud` ou `easy_install --upgrade watson-developer-cloud`.
{: python}

Supondo que tudo funciona conforme o esperado, o serviço {{site.data.keyword.conversationshort}} retorna a saída do diálogo, que é então impressa no console:

```
Welcome to the {{site.data.keyword.conversationshort}} example!
```
{: screen}

Essa saída nos informa que nos comunicamos com sucesso com o serviço do {{site.data.keyword.conversationshort}} e recebemos a mensagem de boas-vindas especificada pelo nó conversation_start no diálogo. Agora podemos incluir uma interface com o usuário, tornando possível processar a entrada do usuário.

## Processando a entrada do usuário para detectar intenções

Para podermos processar a entrada do usuário, precisamos incluir uma interface com o usuário em nosso aplicativo. Para este exemplo, vamos manter as coisas simples e usar a entrada e a saída padrão.
<span class="ph style-scope doc-content" data-hd-programlang="javascript">Podemos usar o módulo prompt-sync do Node.js para fazer isso. (É possível instalar o prompt-sync usando `npm install prompt-sync`.)</span>
<span class="ph style-scope doc-content" data-hd-programlang="python">Podemos usar a função `input` do Python para fazer isso.</span>

```javascript
// Example 2: adds user input and detects intents.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service wrapper.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with service username
  password: 'PASWORD', // replace with service password
  version_date: '2017-05-26'
});

var workspace_id = 'WORKSPACE_ID'; // replace with workspace ID

// Start conversation with empty message.
conversation.message({ workspace_id: workspace_id }, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // If an intent was detected, log it out to the console.
  if (response.intents.length > 0) {
    console.log('Detected intent: #' + response.intents[0].intent);
  }

  // Display the output from dialog, if any.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }

  // Prompt for the next round of input.
  var newMessageFromUser = prompt('>> ');
  conversation.message({
    workspace_id: workspace_id,
    input: { text: newMessageFromUser }
    }, processResponse)
}
```
{: codeblock}
{: javascript}

```python
# Example 2: adds user input and detects intents.

import watson_developer_cloud

# Set up Conversation service.
conversation = watson_developer_cloud.ConversationV1( username = 'USERNAME', # substituir pelo nome do usuário da senha de chave = 'PASSWORD', # substituir pela senha da versão de chave de serviço = '2017-05-26'
)
workspace_id = 'WORKSPACE_ID' # replace with workspace ID

# Initialize with empty value to start the conversation.
user_input = ''

# Principal loop de entrada/saída enquanto True:

  # Send message to Conversation service.
  response = conversation.message( workspace_id = workspace_id, input = {
      'text': user_input
    }
  )

  # If an intent was detected, print it to the console.
  if response['intents']:
    print('Detected intent: #' + response['intents'][0]['intent'])

  # Print the output from dialog, if any.
  if response['output']['text']:
    print(response['output']['text'][0])

  # Prompt for next round of input.
  user_input = input('>> ')
```
{: codeblock }
{: python }

Esta versão do aplicativo começa da mesma maneira que antes: enviando uma mensagem vazia para o serviço do {{site.data.keyword.conversationshort}} para iniciar a conversa.

A função `processResponse ()` agora exibe qualquer intenção detectada pelo diálogo junto com o texto de saída e, então, solicita a próxima rodada de entrada do usuário.
{: javascript }

Em seguida, exibe qualquer intenção detectada pelo diálogo junto com o texto de saída e, então, solicita a próxima rodada de entrada do usuário. (Estamos usando um loop `while True` no momento, já que ainda não implementamos uma maneira de terminar a conversa.)
{: python }

Mas algo ainda não está certo:

```
Welcome to the {{site.data.keyword.conversationshort}} example!
>> hello
Detected intent: #hello
Welcome to the {{site.data.keyword.conversationshort}} example!
>> what time is it?
Detected intent: #time
Welcome to the {{site.data.keyword.conversationshort}} example!
>> goodbye
Detected intent: #goodbye
Welcome to the {{site.data.keyword.conversationshort}} example!
>>
```
{: screen}

O serviço {{site.data.keyword.conversationshort}} está detectando as intenções corretas e com isso cada rodada da conversa retorna a mensagem de boas-vindas do nó conversation_start (`Welcome to the {{site.data.keyword.conversationshort}} example!`).

Isso está acontecendo porque o serviço do {{site.data.keyword.conversationshort}} está sem estado; é responsabilidade do aplicativo manter as informações de estado. Como nós ainda não estamos fazendo nada para manter o estado, o serviço do {{site.data.keyword.conversationshort}} vê cada rodada de entrada do usuário como a primeira rodada de uma nova conversa, acionando o nó conversation_start.

## Mantendo o estado

As informações de estado para sua conversa são mantidas usando o *contexto*. O contexto é um objeto JSON que é transmitido e retornado entre seu aplicativo e o serviço do {{site.data.keyword.conversationshort}}. É responsabilidade de seu aplicativo manter o contexto de uma rodada de conversa para a próxima.

O contexto inclui um identificador exclusivo para cada conversa com um usuário, bem como um contador que é incrementado com cada rodada da conversa. A versão anterior do exemplo não preservava o contexto, o que significa que cada rodada de entrada parecia ser o início de uma nova conversa. Podemos consertar isso salvando o contexto e enviando-o de volta ao serviço do {{site.data.keyword.conversationshort}} toda vez.

Além de manter nossa posição na conversa, o contexto também pode ser usado para armazenar quaisquer outros dados que você deseje transmitir e receber entre seu aplicativo e o serviço do {{site.data.keyword.conversationshort}}. Isso pode incluir dados persistentes que você deseja manter durante a conversa (como nome ou o número da conta de um cliente) ou quaisquer outros dados que você deseje rastrear (como o status atual de configurações de opção).

```javascript
// Example 3: maintains state.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service wrapper.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with service username password: 'PASSWORD', // replace with service password version_date: '2017-05-26'
});

var workspace_id = 'WORKSPACE_ID'; // replace with workspace ID

// Start conversation with empty message.
conversation.message({ workspace_id: workspace_id }, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  // If an intent was detected, log it out to the console.
  if (response.intents.length > 0) {
    console.log('Detected intent: #' + response.intents[0].intent);
  }
  
  // Display the output from dialog, if any.
  if (response.output.text.length != 0) {
      console.log(response.output.text[0]);
  }

  // Prompt for the next round of input.
    var newMessageFromUser = prompt('>> ');
    // Send back the context to maintain state.
    conversation.message({
      workspace_id: workspace_id,
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
}
```
{: codeblock}
{: javascript }

```python
# Example 3: maintains state.

import watson_developer_cloud

# Set up Conversation service.
conversation = watson_developer_cloud.ConversationV1( username = 'USERNAME', # substituir pelo nome do usuário da senha de chave = 'PASSWORD', # substituir pela senha da versão de chave de serviço = '2017-05-26'
)
workspace_id = 'WORKSPACE_ID' # replace with workspace ID

# Initialize with empty value to start the conversation.
user_input = ''
context = {}

# Principal loop de entrada/saída enquanto True:

  # Send message to Conversation service.
  response = conversation.message( workspace_id = workspace_id, input = {
      'text': user_input }, context = context )

  # If an intent was detected, print it to the console.
  if response['intents']:
    print('Detected intent: #' + response['intents'][0]['intent'])

  # Imprima a saída do diálogo, se houver.
  if response['output']['text']:
    print(response['output']['text'][0])

  # Atualize o contexto armazenado com o recebido mais recentemente do diálogo.
  context = response['context']

  # Prompt for next round of input.
  user_input = input('>> ')
```
{: codeblock }
{: python }

A única mudança do exemplo anterior é que, com cada rodada da conversa, agora enviamos de volta o objeto `response.context` que recebemos na rodada anterior:
{: javascript }

```javascript
    conversation.message({
      input: { text: newMessageFromUser },
      context : response.context,
    }, processResponse)
```
{: codeblock}
{: javascript }

A única mudança do exemplo anterior é que agora estamos armazenando o contexto recebido do diálogo em uma variável chamada `context` e estamos enviando-a de volta com a próxima rodada de entrada do usuário:
{: python }

```python
  response = conversation.message( workspace_id = workspace_id, input = {
      'text': user_input }, context = context )
```
{: codeblock }
{: python }

Isso assegura que o contexto seja mantido de uma vez para a outra, para que o serviço do {{site.data.keyword.conversationshort}} não pense mais que cada vez é a primeira:

```
>> hello
Detected intent: #hello
Good day to you.
>> what time is it?
Detected intent: #time
>> goodbye
Detected intent: #goodbye
OK! See you later.
>>
```
{: screen}

Agora estamos fazendo progresso! O serviço do {{site.data.keyword.conversationshort}} está reconhecendo corretamente nossas intenções e o diálogo está retornando o texto de saída correto (quando fornecido) para cada intenção.

No entanto, nada mais está acontecendo. Quando perguntamos o horário, ficamos sem resposta; e quando dizemos adeus, a conversa não termina. Isso ocorre porque essas intenções requerem que ações adicionais sejam executadas pelo aplicativo.

## Implementando ações do aplicativo

Além do texto de saída a ser exibido para o usuário, o nosso diálogo usa o objeto de `output` no JSON de resposta para sinalizar quando o aplicativo precisa realizar uma ação, com base nas intenções detectadas.

Essas sinalizações de ação são enviadas usando a propriedade `action`, que o nosso diálogo define como parte do JSON de resposta. Quando o diálogo determina que o aplicativo precisa fazer algo, ele configura o valor de `action` com o valor apropriado, `display_time` ou `end_conversation`.

Lembre-se de que `output` é apenas um objeto JSON e você pode incluir nele qualquer conteúdo válido que desejar. Para um aplicativo mais complexo, você pode usar uma matriz com várias sinalizações de ação.

Mas, em nosso exemplo, estamos usando um par de chave/valor simples que suporta uma única sinalização de ação. Nosso código do aplicativo precisa verificar o valor da propriedade `action` na resposta e, em seguida, executar qualquer ação especificada. (Essa versão também remove a exibição de intenções detectadas, agora que temos certeza de que elas estão sendo identificadas corretamente.)

```javascript
// Example 4: implements app actions.

var prompt = require('prompt-sync')();
var ConversationV1 = require('watson-developer-cloud/conversation/v1');

// Set up Conversation service wrapper.
var conversation = new ConversationV1({
  username: 'USERNAME', // replace with service username password: 'PASSWORD', // replace with service password version_date: '2017-05-26'
});

var workspace_id = 'WORKSPACE_ID'; // replace with workspace ID

// Start conversation with empty message.
conversation.message({ workspace_id: workspace_id }, processResponse);

// Process the conversation response.
function processResponse(err, response) {
  if (err) {
    console.error(err); // something went wrong
    return;
  }

  var endConversation = false;
  
  // Check for action flags.
  if (response.output.action === 'display_time') {
    // User asked what time it is, so we output the local system time.
    console.log('The current time is ' + new Date().toLocaleTimeString());
  } else if (response.output.action === 'end_conversation') {
    // User said goodbye, so we're done.
    console.log(response.output.text[0]);
    endConversation = true;
  } else {
    // Display the output from dialog, if any.
    if (response.output.text.length != 0) {
        console.log(response.output.text[0]);
    }
  }

  // If we're not done, prompt for the next round of input.
  if (!endConversation) {
    var newMessageFromUser = prompt('>> ');
    conversation.message({
      workspace_id: workspace_id,
      input: { text: newMessageFromUser },
      // Send back the context to maintain state.
      context : response.context,
    }, processResponse)
  }
}
```
{: codeblock}
{: javascript}

```python
# Example 4: implements app actions.

import watson_developer_cloud
import time

# Set up Conversation service.
conversation = watson_developer_cloud.ConversationV1( username = 'USERNAME', # substituir pelo nome do usuário da senha de chave = 'PASSWORD', # substituir pela senha da versão de chave de serviço = '2017-05-26'
)
workspace_id = 'WORKSPACE_ID' # replace with workspace ID

# Initialize with empty value to start the conversation.
user_input = ''
context = {}
current_action = ''

# Main input/output loop
while current_action != 'end_conversation':

  # Send message to Conversation service.
  response = conversation.message( workspace_id = workspace_id, input = {
      'text': user_input }, context = context )

  # Imprima a saída do diálogo, se houver.
  if response['output']['text']:
    print(response['output']['text'][0])

  # Atualize o contexto armazenado com o recebido mais recentemente do diálogo.
  context = response['context']
  # Check for action flags sent by the dialog.
  if 'action' in response['output']:
    current_action = response['output']['action']
  # User asked what time it is, so we output the local system time.
  if current_action == 'display_time':
    print('The current time is ' + time.strftime('%I:%M:%S %p'))
  # If we're not done, prompt for next round of input.
  if current_action != 'end_conversation':
    user_input = input('>> ')
```
{: codeblock}
{: python}

A função processResponse() agora verifica o valor da propriedade `action` do objeto `output` recebido do serviço do {{site.data.keyword.conversationshort}}. Se o valor for `display_time` ou `end_conversation`, o aplicativo realizará a ação apropriada.
{: javascript}

O app agora verifica o valor da propriedade `action` do objeto `output` recebido do serviço {{site.data.keyword.conversationshort}}. Se o valor for `display_time`, o aplicativo realizará a ação apropriada. Se o valor for `end_conversation`, o app saberá não solicitar mais entrada do usuário e o loop `while` terminará.
{: python}

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

Êxito! O aplicativo agora usa o serviço {{site.data.keyword.conversationshort}} para identificar as intenções na entrada da língua natural, exibe as respostas apropriadas e implementa as ações do cliente solicitadas.

Claro, um aplicativo real usará uma interface com o usuário mais sofisticada, como uma janela de bate-papo da web. E ele implementará ações mais complexas, possivelmente integrando com um banco de dados de clientes ou outros sistemas de negócios. Mas os princípios básicos de como o aplicativo interage com o serviço do {{site.data.keyword.conversationshort}} continuarão os mesmos.

Para obter alguns exemplos mais complexos, dê uma olhada nos aplicativos de Amostra na área de janela de navegação.
