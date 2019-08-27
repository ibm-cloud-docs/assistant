---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-30"

subcollection: assistant


---

{:curl: #curl .ph data-hd-programlang='curl'}
{:external: target="_blank" .external}
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

# Migrando para a API da v2
{: #api-migration}

A API de tempo de execução do Assistant v2, que suporta o uso de assistentes e qualificações, foi introduzida em novembro de 2018. Ela oferece vantagens significativas quando comparada à API de tempo de execução da v1, incluindo o gerenciamento de estado automático, a facilidade de implementação, o versionamento de qualificação e a disponibilidade de novos recursos, como a qualificação de procura.

A API da v2 está disponível para todos os usuários, independentemente do plano de serviços, sem custo adicional.

Atualmente, a API da v2 suporta apenas a interação de tempo de execução com um assistente existente. Os aplicativos de criação que criam ou modificam áreas de trabalho devem continuar usando a API da v1.
{: note}

## Visão geral

Com a API da v2, seu aplicativo cliente se comunica com um assistente, em vez de diretamente com uma área de trabalho. Um assistente é uma nova camada de orquestração que oferece diversos novos recursos, incluindo o gerenciamento de estado automático, o versionamento de qualificações, a facilidade de implementação e as qualificações de procura (para planos Plus e Premium). Sua área de trabalho existente (agora chamada _qualificação de diálogo_) continua funcionando como antes, mas os novos recursos são fornecidos pela nova camada do assistente.

Toda a comunicação com um assistente ocorre dentro do contexto de uma _sessão_, que mantém o estado da conversa durante toda a duração. Os dados de estado, incluindo quaisquer variáveis de contexto definidas por seu diálogo ou aplicativo cliente, são automaticamente armazenadas pelo {{site.data.keyword.conversationshort}}, sem a necessidade de ações de seu aplicativo.

Os dados de estado são mantidos até que a sessão seja explicitamente excluída ou atinja o tempo limite devido à inatividade.

Se tiver um aplicativo existente usando a API da v1 para enviar entradas do usuário diretamente para uma área de trabalho, a migração dele para o uso da API da v2 será um processo direto.

## Configurar um assistente

A API de tempo de execução da v2 envia mensagens para um assistente, que as roteia para sua qualificação de diálogo (anteriormente chamada de área de trabalho). Para configurar um assistente, use a interface com o usuário do {{site.data.keyword.conversationshort}}:

1. Clique na guia **Qualificações**. Verifique se sua área de trabalho é mostrada como uma qualificação disponível (todas as áreas de trabalho existentes para sua instância de serviço são convertidas automaticamente em qualificações na interface com o usuário do {{site.data.keyword.conversationshort}}. Essa conversão não muda a área de trabalho subjacente).

1. Clique na guia  ** Assistentes ** . Clique em **Criar assistente** para criar um novo assistente. Quando a inclusão de qualificações for solicitada, clique em **Incluir qualificação de diálogo** e selecione a qualificação de diálogo que corresponde à sua área de trabalho.

  Para obter mais informações sobre como criar assistentes, consulte [Criando um assistente](https://cloud.ibm.com/docs/services/assistant?topic=assistant-assistant-add).

1. Depois da criação de seu novo assistente, clique no menu ![Menu](images/kebab-react.png) e, em seguida, selecione **Configurações**.

1. Na página **Configurações do assistente**, localize o ID do assistente. Seu aplicativo usará esse ID (em vez de um ID de área de trabalho) para se comunicar com o assistente. As credenciais de serviço serão iguais para as APIs da v1 e da v2.

  Atualmente, não há suporte de API para recuperar um ID de assistente. Para localizar o ID do assistente, deve-se usar a interface com o usuário do {{site.data.keyword.conversationshort}}.
  {: note}

## Chamar a API de tempo de execução da v2

Depois de criar um assistente, é possível atualizar seu aplicativo cliente para usar a API de tempo de execução da v2, em vez da API de tempo de execução da v1.

1. Antes de enviar a primeira mensagem em uma conversa, utilize o método [**Criar uma sessão**](https://cloud.ibm.com/apidocs/assistant-v2#create-a-session){: external} da v2 para criar uma sessão. Salve o ID de sessão retornado:

  ```javascript
  service .createSession({ assistant_id: assistantId, })
  .then (res = >{
    sessionId = res.session_id;
  })
  ```
  {: codeblock}
  {: javascript}

  ```python
  session_id = service.create_session( assistant_id = assistant_id ).get_result()['session_id']
  ```
  {: codeblock }
  {: python }

  ```java
  CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build(); SessionResponse session = service.createSession(createSessionOptions).execute().getResult(); String sessionId = session.getSessionId();
  ```
  {: codeblock}
  {: java}

1. Use o método [**Enviar entrada do usuário ao assistente**](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant){: external} da v2 para enviar a entrada do usuário ao assistente. Em vez de especificar o ID da área de trabalho, como ocorria na API da v1, você especifica o ID do assistente e da sessão:

  ```javascript
  service .message({ assistant_id: assistantId, session_id: sessionId, input: messageInput })
  ```
  {: codeblock}
  {: javascript}

  ```python
  response = service.message( assistant_id, session_id, input = message_input ).get_result()
  ```
  {: codeblock}
  {: python}

  ```java
  MessageInput input = new MessageInput.Builder().text(inputText).build(); MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId)
    .input (entrada)
    .build(); MessageResponse response = service.message(messageOptions).execute().getResult();
  ```
  {: codeblock}
  {: java}

  A estrutura de mensagem básica não foi mudada, especialmente com relação à entrada do usuário, que ainda é enviada como `input.text`.

1. Depois da conclusão de uma conversa, use o método [**Excluir sessão**](https://cloud.ibm.com/apidocs/assistant-v2#delete-session){: external} da v2 para excluir a sessão.

  ```javascript
  service .deleteSession({ assistant_id: assistantId, session_id: sessionId, })
  ```
  {: codeblock}
  {: javascript}

  ```python
  service.delete_session( assistant_id = assistant_id, session_id = session_id )
  ```
  {: codeblock}
  {: python}

  ```java
  DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId
    .build();
  service.deleteSession(deleteSessionOptions).execute();
  ```
  {: codeblock}
  {: java}

   Se a sessão não for excluída explicitamente, isso ocorrerá de forma automática após o intervalo de tempo limite configurado (a duração do tempo limite dependerá de seu plano. Para obter mais informações, consulte [Limites de sessão](/docs/services/assistant?topic=assistant-assistant-settings#assistant-settings-session-limits)).

Para ver exemplos das APIs da v2 no contexto de um aplicativo cliente simples, consulte [Construindo um aplicativo cliente](/docs/services/assistant?topic=assistant-api-client).

## Lidar com o formato de resposta da v2

Seu aplicativo pode precisar ser atualizado para lidar com o formato de resposta do tempo de execução da v2, dependendo de quais partes da resposta ele precisar acessar:

- As saídas para todos os tipos de resposta (como `text` e `option`) ainda são retornadas no objeto `output.generic`. O código do aplicativo para lidar com essas respostas deve funcionar sem modificação.

- Agora, entidades e intenções detectadas são retornadas como parte do objeto `output`, em vez de na raiz do JSON de resposta.

- Agora, o contexto de conversa está organizado em dois objetos:

  - O **contexto global** contém os dados de contexto do nível do sistema compartilhados por todas as qualificações usadas pelo assistente.

  - O **contexto de qualificação** contém quaisquer variáveis de contexto definidas pelo usuário que são usadas por sua qualificação de diálogo.

  No entanto, tenha em mente que os dados de estado, incluindo o contexto de conversa, agora são mantidos pelo assistente, portanto, seu aplicativo pode nunca precisar acessar o contexto (consulte [Permitir que o assistente mantenha o estado](#api-migration-state)).

Consulte a [Referência de API ](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant){: external} da v2 para obter a documentação completa do formato de resposta da v2.

## Permitir que o assistente mantenha o estado
{: #api-migration-state}

Para a maioria dos aplicativos, agora é possível remover qualquer código incluído para o propósito de manutenção do estado. Não é mais necessário salvar o contexto e enviá-lo de volta para o {{site.data.keyword.conversationshort}} com cada parte da conversa. O contexto é mantido automaticamente pelo {{site.data.keyword.conversationshort}} e pode ser acessado por seu diálogo como antes.

Observe que, por padrão, o contexto não é incluído nas respostas para o aplicativo cliente com a API da v2. No entanto, seu código ainda pode acessar variáveis de contexto, se necessário:

- Ainda é possível enviar um objeto `context` como parte da entrada da mensagem. Todas as variáveis de contexto incluídas são armazenadas como parte do contexto mantido pelo {{site.data.keyword.conversationshort}} (se a variável de contexto enviada já existir no contexto, o novo valor sobrescreverá o valor armazenado anteriormente).

  Certifique-se de que o objeto de contexto enviado esteja em conformidade com o formato da v2. Todas as variáveis de contexto definidas pelo usuário que são enviadas por seu aplicativo devem fazer parte do contexto de qualificação. Geralmente, a única variável de contexto global que pode precisar de configuração é a `system.user_id`, que é usada pelos planos Plus e Premium para propósitos de faturamento.

- Ainda é possível recuperar as variáveis de contexto do contexto de qualificação ou global. Para ter o objeto `context` incluído com respostas de mensagem, use a propriedade **return_context** nas opções de entrada de mensagem. Para obter mais informações, consulte [Acessando dados de contexto](/docs/services/assistant?topic=assistant-api-client-get-context).
