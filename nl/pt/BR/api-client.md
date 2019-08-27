---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-08"

subcollection: assistant


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
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# Construindo um aplicativo cliente
{: #api-client}

Então, você tem uma qualificação de diálogo de trabalho e um assistente que a usa. Agora, você deseja desenvolver um aplicativo cliente customizado que interagirá com seus usuários e se comunicará com o serviço {{site.data.keyword.conversationfull}}.
{: shortdesc}

## Configurando o serviço {{site.data.keyword.conversationshort}}
{: #api-client-setup}

O aplicativo de exemplo que criaremos nesta seção implementa várias funções de um assistente pessoal cognitivo. O código do aplicativo se conectará a um assistente do {{site.data.keyword.conversationshort}}, no qual o processamento cognitivo (como a detecção de intenções do usuário) ocorre.

Antes de continuar com este exemplo, você precisa configurar o assistente necessário:

1.  Faça download da habilidade de diálogo  <a target="_blank" href="https://watson-developer-cloud.github.io/doc-tutorial-downloads/assistant/assistant-simple-example.json" download="assistant-simple-example.json"> Arquivo JSON </a>.
1.  [Importe a qualificação](/docs/services/assistant?topic=assistant-skill-dialog-add#skill-dialog-add-task) para uma instância do serviço {{site.data.keyword.conversationshort}}.
1.  [Crie um assistente](/docs/services/assistant?topic=assistant-assistant-add) e conecte a qualificação importada.

## Obtendo informações de serviço
{: #api-client-get-info}

Para acessar as APIs de REST do serviço {{site.data.keyword.conversationshort}}, seu aplicativo precisa ser capaz de autenticar com o {{site.data.keyword.Bluemix}} e conectar-se ao assistente correto. Será necessário copiar as credenciais de serviço e o ID do assistente e colá-los em seu código do aplicativo.

Para acessar as credenciais de serviço e o ID do assistente por meio da ferramenta {{site.data.keyword.conversationshort}}, acesse a guia **Assistentes** e clique no menu ![Menu](images/kebab-react.png) para o assistente ao qual você deseja se conectar. Selecione **Visualizar detalhes da API** para ver os detalhes para o assistente, incluindo o ID do assistente e a chave de API.

Também é possível acessar as credenciais de serviço de seu painel do {{site.data.keyword.Bluemix_short}}.

## Comunicando-se com o serviço {{site.data.keyword.conversationshort}}
{: #api-client-communicate}

Interagir com o serviço do {{site.data.keyword.conversationshort}} é simples. Vamos dar uma olhada em um exemplo que se conecta ao serviço, envia uma única mensagem e imprime a saída no console:

```javascript
// Example 1: sets up service wrapper, sends initial message, and
// receives response.

const AssistantV2 = require('ibm-watson/assistant/v2');

// Configurar o wrapper de serviço do Assistente.
const service = new AssistantV2({
  iam_apikey: '{apikey}', // replace with API key version: '2019-02-28', });

const assistantId = '{assistant_id}'; // replace with assistant ID let sessionId;

// Create session.
service .createSession({ assistant_id: assistantId, })
  .then (res = >{
    sessionId = res.session_id; sendMessage({ message_type: 'text', text: '' // start conversation with empty message }); })
  .catch(err => {
    console.log(err); // something went wrong });

// Send message to assistant.
function sendMessage(messageInput) {
  service .message({ assistant_id: assistantId, session_id: sessionId, input: messageInput })
    .then (res = >{
      processResponse(res); })
    .catch(err => {
      console.log(err); // something went wrong }); }

// Processe a resposta.
function processResponse(response) {
  // Display the output from assistant, if any. Supports only a single // text response.
  if (response.output.generic) {
    if (response.output.generic.length > 0) {
      if (response.output.generic[0].response_type === 'text') {
        console.log (response.output.generic [ 0 ] .text); }
    }
  }


// We're done, so we close the session.
service .deleteSession({ assistant_id: assistantId, session_id: sessionId, })
  .catch(err => {
    console.log(err); // something went wrong }); }
```
{: codeblock}
{: javascript}

```python
# Example 1: sets up service wrapper, sends initial message, and
# receives response.

import ibm_watson

# Set up Assistant service.
service = ibm_watson.AssistantV2( iam_apikey = '{apikey}', # replace with API key version = '2019-02-28'
)

assistant_id = '{assistant_id}' # replace with assistant ID

# Criar sessão.
session_id = service.create_session( assistant_id = assistant_id ).get_result()['session_id']

# Start conversation with empty message.
response = service.message(
    assistant_id,
    session_id
).get_result()

# Imprima a saída do diálogo, se houver. Supports only a single # text response.
if response['output']['generic']:
    if response['output']['generic'][0]['response_type'] == 'text':
        print(response['output']['generic'][0]['text'])

# We're done, so we delete the session.
service.delete_session( assistant_id = assistant_id, session_id = session_id )
```
{: codeblock}
{: python}

```java
/*
 * Example 1: sets up service wrapper, sends initial message, and
 * receives response.
 */

import com.ibm.watson.assistant.v2.Assistant;
import com.ibm.watson.assistant.v2.model.CreateSessionOptions;
import com.ibm.watson.assistant.v2.model.DeleteSessionOptions;
import com.ibm.watson.assistant.v2.model.MessageOptions;
import com.ibm.watson.assistant.v2.model.MessageResponse;
import com.ibm.watson.assistant.v2.model.SessionResponse;
import com.ibm.cloud.sdk.core.service.security.IamOptions;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main (String [ ] args) {

    // Suppress log messages in stdout.
    LogManager.getLogManager ().reset ();

    // Set up Assistant service.
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build(); Assistant service = new Assistant("2019-02-28", iamOptions); String assistantId = "{assistant_id}"; // replace with assistant ID

    // Create session.
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build(); SessionResponse session = service.createSession(createSessionOptions).execute().getResult(); String sessionId = session.getSessionId();

    // Start conversation with empty message.
    MessageOptions messageOptions = new MessageOptions.Builder(assistantId,
                                                        sessionId).build();
    MessageResponse response = service.message(messageOptions).execute().getResult();

    // Print the output from dialog, if any. Assume uma única resposta de texto.
    System.out.println(response.getOutput().getGeneric().get(0).getText());

    // We're done, so we delete the session.
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build(); service.deleteSession(deleteSessionOptions).execute(); }
}
```
{: codeblock}
{: java}

A primeira etapa é criar um wrapper para o serviço do {{site.data.keyword.conversationshort}}.

O wrapper é um objeto que você usará para enviar entrada para o serviço e para receber a saída dele. Ao criar o wrapper de serviço, especifique as credenciais de autenticação da chave de serviço, bem como a versão da API do {{site.data.keyword.conversationshort}} que você está usando.

Neste exemplo do Node.js, o wrapper é uma instância de `AssistantV2`, armazenada na variável `service`. Os SDKs do Watson para outros idiomas fornecem mecanismos equivalentes para instanciar um wrapper de serviço.
{: javascript}

Neste exemplo do Python, o wrapper é uma instância de `watson_developer_cloud.AssistantV2`, armazenada na variável `service`. Os SDKs do Watson para outros idiomas fornecem mecanismos equivalentes para instanciar um wrapper de serviço.
{: python}

Neste exemplo do Java, o wrapper é uma instância de `Assistant`, armazenada na variável `service`. Os SDKs do Watson para outros idiomas fornecem mecanismos equivalentes para instanciar um wrapper de serviço.
{: java}

Após a criação do wrapper do serviço, nós o usamos para criar uma sessão e enviar uma mensagem para o assistente. Neste exemplo, a mensagem está vazia; nós só queremos acionar o nó conversation_start no diálogo, então não precisamos de nenhum texto de entrada. Em seguida, imprimimos o texto de resposta no console e, finalmente, excluímos a sessão.

Use o comando `node <filename.js>` para executar o aplicativo de exemplo.
{: javascript}

Use o comando `python3 <filename.py>` para executar o aplicativo de exemplo.
{: python}

Cole o código de exemplo em um arquivo denominado `AssistantSimpleExample.java`. Em seguida, é possível compilar e executá-lo.
{: java}

**Nota:** certifique-se de ter instalado o SDK do Watson para Node.js usando `npm install ibm-watson`.
{: javascript}

**Nota:** certifique-se de ter instalado o SDK do Watson para Python usando `pip install --upgrade ibm-watson` ou `easy_install --upgrade ibm-watson`.
{: python}

**Nota:** certifique-se de que tenha instalado o [Watson SDK for Java ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/watson-developer-cloud/java-sdk/blob/master/README.md){: new_window}.
{: java}

Supondo que tudo funcione conforme o esperado, o assistente retorna a saída do diálogo, que é, então, impressa no console:

```
Bem-vindo ao exemplo do Watson Assistant!
```
{: screen}

Essa saída nos informa que nos comunicamos com sucesso com o serviço do {{site.data.keyword.conversationshort}} e recebemos a mensagem de boas-vindas especificada pelo nó conversation_start no diálogo. Agora podemos incluir uma interface com o usuário, tornando possível processar a entrada do usuário.

## Processando a entrada do usuário para detectar intenções
{: #api-client-process-input}

Para ser capaz de processar a entrada do usuário, é necessário incluir uma interface com o usuário em nosso aplicativo cliente. Para este exemplo, vamos manter as coisas simples e usar a entrada e a saída padrão.
<span class="ph style-scope doc-content" data-hd-programlang="javascript">Podemos usar o módulo prompt-sync do Node.js para fazer isso. (É possível instalar o prompt-sync usando `npm install prompt-sync`.)</span>
<span class="ph style-scope doc-content" data-hd-programlang="python">Podemos usar a função `input` do Python 3 para fazer isso.</span>
<span class="ph style-scope doc-content" data-hd-programlang="java">Podemos usar a função `Console.readLine()` do Java para fazer isso.</span>

```javascript
// Example 2: adds user input and detects intents.

const prompt = require('prompt-sync')(); const AssistantV2 = require('ibm-watson/assistant/v2');

// Configurar o wrapper de serviço do Assistente.
const service = new AssistantV2({
  iam_apikey: '{apikey}', // replace with API key version: '2019-02-28', });

const assistantId = '{assistant_id}'; // replace with assistant ID let sessionId;

// Create session.
service .createSession({ assistant_id: assistantId, })
  .then (res = >{
    sessionId = res.session_id;
    sendMessage({
      message_type: 'text',
      text: ''
    }); // start conversation with empty message
  })
  .catch(err => {
    console.log(err); // something went wrong });

// Send message to assistant.
function sendMessage(messageInput) {
  service .message({ assistant_id: assistantId, session_id: sessionId, input: messageInput })
    .then (res = >{
      processResponse(res); })
    .catch(err => {
      console.log(err); // something went wrong }); }

// Processe a resposta.
function processResponse(response) {

  // If an intent was detected, log it out to the console.
  if (response.output.intents.length > 0) {
    console.log('Detected intent: #' + response.output.intents[0].intent);
  }

  // Display the output from assistant, if any. Supports only a single // text response.
  if (response.output.generic) {
    if (response.output.generic.length > 0) {
      if (response.output.generic[0].response_type === 'text') {
        console.log (response.output.generic [ 0 ] .text); }
    }
  }

  // Prompt for the next round of input.
  const newMessageFromUser = prompt('>> ');
  if (newMessageFromUser === 'quit') {
    service .deleteSession({ assistant_id: assistantId, session_id: sessionId, })
      .catch(err => {
        console.log(err); // something went wrong
      });
    return;
  }
  newMessageInput = {
    message_type: 'text',
    text: newMessageFromUser
  }
  sendMessage(newMessageInput);
}
```
{: codeblock}
{: javascript}

```python
# Example 2: adds user input and detects intents.

import ibm_watson

# Set up Assistant service.
service = ibm_watson.AssistantV2( iam_apikey = '{apikey}', # replace with API key version = '2019-02-28'
)

assistant_id = '{assistant_id}' # replace with assistant ID

# Criar sessão.
session_id = service.create_session( assistant_id = assistant_id ).get_result()['session_id']

# Initialize with empty value to start the conversation.
message_input = {
    'message_type:': 'text',
    'text': ''
    }

# Main input/output loop
while message_input['text'] != 'quit':

    # Send message to assistant.
    response = service.message( assistant_id, session_id, input = message_input ).get_result()

    # If an intent was detected, print it to the console.
    if response['output']['intents']:
        print('Detected intent: #' + response['output']['intents'][0]['intent'])

    # Imprima a saída do diálogo, se houver. Supports only a single # text response.
    if response['output']['generic']:
        if response['output']['generic'][0]['response_type'] == 'text':
            print(response['output']['generic'][0]['text'])

    # Prompt for next round of input.
    user_input = input('>> ')
    message_input = {
        'text': user_input
    }

# We're done, so we delete the session.
service.delete_session( assistant_id = assistant_id, session_id = session_id )
```
{: codeblock }
{: python }

```java
/*
 * Example 2: adds user input and detects intents.
 */

import com.ibm.watson.assistant.v2.Assistant;
import com.ibm.watson.assistant.v2.model.CreateSessionOptions;
import com.ibm.watson.assistant.v2.model.DeleteSessionOptions;
import com.ibm.watson.assistant.v2.model.DialogRuntimeResponseGeneric;
import com.ibm.watson.assistant.v2.model.MessageInput;
import com.ibm.watson.assistant.v2.model.MessageOptions;
import com.ibm.watson.assistant.v2.model.MessageResponse;
import com.ibm.watson.assistant.v2.model.RuntimeIntent;
import com.ibm.watson.assistant.v2.model.SessionResponse;
import com.ibm.cloud.sdk.core.service.security.IamOptions;
import java.util.List;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main (String [ ] args) {

    // Suppress log messages in stdout.
    LogManager.getLogManager ().reset ();

    // Set up Assistant service.
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build(); Assistant service = new Assistant("2019-02-28", iamOptions); String assistantId = "{assistant_id}"; // replace with assistant ID

    // Create session.
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build(); SessionResponse session = service.createSession(createSessionOptions).execute().getResult(); String sessionId = session.getSessionId();

    // Initialize with empty value to start the conversation.
    Sequência inputText = "";

    // Main input/output loop do {
      // Send message to assistant.
      MessageInput input = new MessageInput.Builder().text(inputText).build(); MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId)
                                                  .input (entrada)
                                                  .build(); MessageResponse response = service.message(messageOptions).execute().getResult();

      // If an intent was detected, print it to the console.
      List<RuntimeIntent> responseIntents = response.getOutput().getIntents();
      if(responseIntents.size() > 0) {
        System.out.println("Detected intent: #" + responseIntents.get(0).getIntent());
      }

      // Print the output from dialog, if any. Assume uma única resposta de texto.
      List<DialogRuntimeResponseGeneric> responseGeneric = response.getOutput().getGeneric(); if(responseGeneric.size() > 0) {
        System.out.println(response.getOutput().getGeneric().get(0).getText()); }

      // Prompt for next round of input.
      System.out.print(">> ");
      inputText = System.console().readLine();
    } while(!inputText.equals("quit"));

    // We're done, so we delete the session.
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build(); service.deleteSession(deleteSessionOptions).execute(); }
}
```
{: codeblock }
{: java }

Esta versão do aplicativo inicia da mesma maneira que antes: enviando uma mensagem vazia para o assistente para iniciar a conversa.

A função `processResponse()` exibe agora qualquer intenção detectada pela qualificação de diálogo, juntamente com o texto de saída. Em seguida, ele solicita a próxima rodada de entrada do usuário.
{: javascript }

Em seguida, ela exibe qualquer intenção detectada pela qualificação de diálogo, juntamente com o texto de saída. Em seguida, ele solicita a próxima rodada de entrada do usuário.
{: python }

Em seguida, exibe qualquer intenção detectada pelo diálogo junto com o texto de saída e, então, solicita a próxima rodada de entrada do usuário.
{: java}

Ainda não implementamos uma maneira de língua natural para terminar a conversa, portanto, em vez disso, estamos usando o comando literal `quit` para indicar que o programa deve excluir a sessão e sair.

```
Bem-vindo ao exemplo do Watson Assistant! > > hello
Detected intent: #hello
Good day to you.
>> what time is it?
Detected intent: #time
>> goodbye
Detected intent: #goodbye
OK! Vejo você mais tarde. > > quit
```
{: screen}

Agora estamos fazendo progresso! O serviço do {{site.data.keyword.conversationshort}} está reconhecendo corretamente nossas intenções e o diálogo está retornando o texto de saída correto (quando fornecido) para cada intenção.

No entanto, nada mais está acontecendo. Quando perguntamos o horário, ficamos sem resposta; e quando dizemos adeus, a conversa não termina. Isso ocorre porque essas intenções requerem que ações adicionais sejam executadas pelo aplicativo.

## Implementando ações do aplicativo
{: #api-client-implement-actions}

Além do texto de saída a ser exibido para o usuário, nosso diálogo do {{site.data.keyword.conversationshort}} usa a matriz `actions` no JSON de resposta para sinalizar quando o aplicativo precisa executar uma ação, com base nas intenções detectadas. Quando o diálogo determina que o aplicativo cliente precisa fazer algo, ele retorna um objeto de ação com um `type` de `client`. O `name` da ação indica a ação específica, `display_time` ou `end_conversation`. (Propriedades adicionais da ação podem especificar parâmetros, credenciais e outras informações relacionadas à ação, mas, para nosso exemplo, não precisamos de nada, exceto o nome da ação.)

Sabemos que nosso diálogo nunca solicitará mais de uma ação por vez, portanto, nosso cliente precisa somente verificar a existência de uma única ação `client` na matriz `actions`. Se localizar uma, ele poderá, então, realizar a ação especificada. (Essa versão também remove a exibição de intenções detectadas, agora que temos certeza de que elas estão sendo identificadas corretamente.)

```javascript
// Exemplo 3: implemente ações do aplicativo.

const prompt = require('prompt-sync')(); const AssistantV2 = require('ibm-watson/assistant/v2');

// Configurar o wrapper de serviço do Assistente.
const service = new AssistantV2({
  iam_apikey: '{apikey}', // replace with API key version: '2019-02-28', });

const assistantId = '{assistant_id}'; // replace with assistant ID let sessionId;

// Create session.
service .createSession({ assistant_id: assistantId, })
  .then (res = >{
    sessionId = res.session_id; sendMessage({ message_type: 'text', text: '' // start conversation with empty message }); })
  .catch(err => {
    console.log(err); // something went wrong });

// Send message to assistant.
function sendMessage(messageInput) {
  service .message({ assistant_id: assistantId, session_id: sessionId, input: messageInput })
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
    // Display the output from assistant, if any. Assume uma única resposta de texto.
    if (response.output.generic) {
      if (response.output.generic.length > 0) {
        if (response.output.generic[0].response_type === 'text') {
          console.log (response.output.generic [ 0 ] .text); }
      }
    }
  }

  // If we're not done, prompt for the next round of input.
  if (!endConversation) {
    const newMessageFromUser = prompt('>> '); newMessageInput = {
      message_type: 'text', text: newMessageFromUser } sendMessage(newMessageInput); } else {
    service .deleteSession({ assistant_id: assistantId, session_id: sessionId, })
      .then (res = >{
        return; })
      .catch(err => {
        console.log(err); // something went wrong }); }
}
```
{: codeblock}
{: javascript}

```python
# Exemplo 3: Implementa ações do aplicativo.

import ibm_watson
import time

# Set up Assistant service.
service = ibm_watson.AssistantV2( iam_apikey = '{apikey}', # replace with API key version = '2019-02-28'
)

assistant_id = '{assistant_id}' # replace with assistant ID

# Criar sessão.
session_id = service.create_session( assistant_id = assistant_id ).get_result()['session_id']

# Initialize with empty values to start the conversation.
message_input = {'text': ''}
current_action = ''

# Main input/output loop
while current_action != 'end_conversation':
    # Clear any action flag set by the previous response.
    current_action = ''

    # Send message to assistant.
    response = service.message( assistant_id, session_id, input = message_input ).get_result()

    # Imprima a saída do diálogo, se houver. Supports only a single # text response.
    if response['output']['generic']:
        if response['output']['generic'][0]['response_type'] == 'text':
            print(response['output']['generic'][0]['text'])

    # Check for client actions requested by the assistant.
    if 'actions' in response['output']:
        if response['output']['actions'][0]['type'] == 'client':
            current_action = response['output']['actions'][0]['name']

    # User asked what time it is, so we output the local system time.
    if current_action == 'display_time':
        print('The current time is ' + time.strftime('%I:%M:%S %p') + '.')
    # If we're not done, prompt for next round of input.
    if current_action != 'end_conversation':
    user_input = input('>> ')
        message_input = {
            'text': user_input
    }

# We're done, so we delete the session.
service.delete_session( assistant_id = assistant_id, session_id = session_id )
```
{: codeblock}
{: python}

```java
/*
 * Example 3: implements app actions.
 */

import com.ibm.watson.assistant.v2.Assistant;
import com.ibm.watson.assistant.v2.model.CreateSessionOptions;
import com.ibm.watson.assistant.v2.model.DeleteSessionOptions;
import com.ibm.watson.assistant.v2.model.DialogNodeAction;
import com.ibm.watson.assistant.v2.model.DialogRuntimeResponseGeneric;
import com.ibm.watson.assistant.v2.model.MessageInput;
import com.ibm.watson.assistant.v2.model.MessageOptions;
import com.ibm.watson.assistant.v2.model.MessageResponse;
import com.ibm.watson.assistant.v2.model.RuntimeIntent;
import com.ibm.watson.assistant.v2.model.SessionResponse;
import com.ibm.cloud.sdk.core.service.security.IamOptions;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;
import java.util.List;
import java.util.logging.LogManager;

public class AssistantSimpleExample {
  public static void main (String [ ] args) {

    // Suppress log messages in stdout.
    LogManager.getLogManager ().reset ();

    // Set up Assistant service.
    IamOptions iamOptions = new IamOptions.Builder().apiKey("{apikey}").build(); Assistant service = new Assistant("2019-02-28", iamOptions); String assistantId = "{assistant_id}"; // replace with assistant ID

    // Create session.
    CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build(); SessionResponse session = service.createSession(createSessionOptions).execute().getResult(); String sessionId = session.getSessionId();

    // Initialize with empty values to start the conversation.
    String inputText = "";
    String currentAction;

    // Main input/output loop do {
      // Clear any action flag set by the previous response.
      currentAction = "";

      // Send message to assistant.
      MessageInput input = new MessageInput.Builder().text(inputText).build(); MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId)
                                                  .input (entrada)
                                                  .build(); MessageResponse response = service.message(messageOptions).execute().getResult();

      // Print the output from dialog, if any. Assume uma única resposta de texto.
      List<DialogRuntimeResponseGeneric> responseGeneric = response.getOutput().getGeneric(); if(responseGeneric.size() > 0) {
        System.out.println(response.getOutput().getGeneric().get(0).getText()); }

      // Check for any actions requested by the assistant.
      List<DialogNodeAction> responseActions = response.getOutput().getActions();
      if(responseActions != null) {
        if (responseActions.get (0) .getActionType ().equals ("client")) {
          currentAction = responseActions.get(0).getName();
        }
      }

      // User asked what time it is, so we output the local system time.
      if (currentAction.equals ("display_time")) {
        DateTimeFormatter fmt = DateTimeFormatter.ofPattern("h:mm:ss a");
        LocalTime time = LocalTime.now();
        System.out.println("The current time is " + time.format(fmt) + ".");
      }

      // If we're not done, prompt for next round of input.
      if (!currentAction.equals ("end_UNK")) {
        System.out.print(">> ");
        inputText = System.console().readLine();
      }

    } while (!currentAction.equals ("end_UNK"));

    // We're done, so we delete the session.
    DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId).build(); service.deleteSession(deleteSessionOptions).execute(); }
}
```
{: codeblock}
{: java}

O app verifica agora a matriz `actions` na resposta para ver se uma ação com `type`=`client` está presente. Se sim, ele verifica o valor `name` da ação e realiza a ação apropriada (exibindo o tempo do sistema local ou configurando uma sinalização interna que indica que a conversa acabou).

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

Claro, um aplicativo real usará uma interface com o usuário mais sofisticada, como uma janela de bate-papo da web. E ele implementará ações mais complexas, possivelmente integrando com um banco de dados de clientes ou outros sistemas de negócios. Ele também precisaria enviar dados adicionais para o assistente, como um ID do usuário para identificar cada usuário exclusivo. Mas os princípios básicos de como o aplicativo interage com o serviço do {{site.data.keyword.conversationshort}} continuarão os mesmos.

Para obter alguns exemplos mais complexos, consulte [Aplicativos de amostra](/docs/services/assistant?topic=assistant-sample-apps).

## Usando a API v1
{: #api-client-v1-api}

O uso da API v2 é a maneira recomendada de construir um aplicativo cliente de tempo de execução que se comunica com o serviço {{site.data.keyword.conversationshort}}. No entanto, alguns aplicativos mais antigos ainda podem estar usando a API v1, que inclui um método de tempo de execução semelhante para enviar mensagens para a área de trabalho em uma qualificação de diálogo.

Observe que, se seu app usar a API v1, ele se comunicará diretamente com a área de trabalho, ignorando os recursos de orquestração e de gerenciamento de estado do assistente. Isso significa que seu aplicativo é responsável por gerenciar as informações de estado usando o contexto. Seu aplicativo deve manter o contexto salvando o contexto recebido com cada resposta e enviando-o de volta para o serviço com cada nova solicitação de mensagem. (O método `/message` v1 sempre retorna o contexto com cada resposta.)

Para obter mais informações sobre o método `/message` v1 e o contexto, consulte a [Referência de API v1 ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/apidocs/assistant#get-response-to-user-input){: new_window}.
