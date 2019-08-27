---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-09"

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

# Fazendo uma chamada programática de um nó de diálogo
{: #dialog-webhooks}

Para fazer uma chamada programática, defina um webhook que envie um callout de solicitação POST para um aplicativo externo que execute uma função programática. Em seguida, será possível chamar o webhook de um ou mais nós de diálogo.

Um webhook é um mecanismo que permite chamar um programa externo com base em algo que está acontecendo em seu programa. Quando usado em uma qualificação de diálogo, um webhook é acionado quando o assistente processa um nó que tem um webhook ativado. O webhook coleta dados especificados ou coletados do usuário durante a conversa e os salva em variáveis de contexto. Ele os envia como parte de uma solicitação de HTTP POST para a URL especificada como parte de sua definição de webhook. A URL que recebe o webhook é o listener. Ela executa uma ação predefinida usando as informações transmitidas a ela, conforme especificado na definição de webhook, e pode retornar uma resposta, opcionalmente.

Assista a este vídeo para aprender mais.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Demo de webhooks" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/j8TBqD2rx2o?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

É possível usar um webhook para fazer os seguintes tipos de coisas:

- Validar informações coletadas do usuário.
- Interagir com um serviço da web externo para obter informações. Por exemplo, você pode verificar o horário de chegada esperado de um voo por meio de um serviço de tráfego aéreo ou obter uma previsão de um serviço de meteorologia.
- Enviar solicitações para um aplicativo externo, como um site de reserva de restaurante, para concluir uma transação simples em nome do usuário.
- Acionar uma notificação de SMS.
- Acionar uma ação da web do {{site.data.keyword.openwhisk}}.

Não é possível usar um webhook para chamar uma ação do {{site.data.keyword.openwhisk_short}} que usa a autenticação baseada em token do Identity and Access Management (IAM).
{: note}

Para obter informações sobre como chamar um aplicativo cliente, consulte [Chamando um aplicativo cliente de um nó de diálogo](/docs/services/assistant?topic=assistant-dialog-actions-client).

## Definindo o webhook
{: #dialog-webhooks-create}

É possível definir uma URL de webhook para uma qualificação de diálogo e, em seguida, chamar o webhook de um ou mais nós de diálogo.

A chamada programática para o serviço externo deve atender a esses requisitos:

- A chamada deve ser uma solicitação de HTTP POST.
- A solicitação e a resposta devem estar no formato JSON. Por exemplo: `Content-Type: application/json`.
- A chamada deve ser retornada em **5 segundos ou menos**.

  Ao chamar serviços menos eficientes, é possível gerenciar a chamada por meio de um aplicativo cliente e transmitir as informações para o diálogo como uma etapa separada. Para obter mais informações, consulte [Chamando um aplicativo cliente de um nó de diálogo](/docs/services/assistant?topic=assistant-dialog-actions-client).
  {: tip}

Para incluir detalhes do webhook, conclua as etapas a seguir:

1.  Na qualificação em que você deseja incluir o webhook, clique na guia **Opções**.

1.  Clique em **Webhooks**.

1.  No campo **URL**, inclua a URL para o aplicativo externo para o qual você deseja enviar callouts de solicitação de POST HTTP.

    Por exemplo, para chamar o serviço Language Translator, especifique a URL de sua instância de serviço.

    ```bash
    https://gateway.watsonplatform.net/language-translator/api/v3/translate?version=2018-05-01
    ```
    {: codeblock}

    Se o aplicativo externo que você chamar retornar uma resposta, ele deverá ser capaz de enviar de volta uma resposta no formato JSON. Para o serviço Language Translator, por exemplo, deve-se especificar o formato desejado para o retorno do resultado. Isso pode ser feito transmitindo um cabeçalho para o serviço.

1.  Na seção Cabeçalhos, inclua quaisquer cabeçalhos que deseja transmitir ao serviço, um por vez, clicando em **Incluir cabeçalho**.

    Por exemplo, inclua um cabeçalho que indique que você deseja que o valor resultante seja retornado no formato JSON.

    <table>
    <caption>Exemplo de cabeçalho</caption>
      <tr>
      <th>Nome do cabeçalho</th>
      <th>Valor do cabeçalho</th>
      </tr>
      <tr>
      <td>Conteúdo-tipo</td>
      <td>application/json</td>
      </tr>
    </table>
    
1.  Se o serviço externo requerer, transmita as credenciais de autenticação básica com a solicitação. Clique em **Incluir autorização**, inclua suas credenciais nos campos **Nome de usuário** e **Senha** e, em seguida, clique em **Salvar**. 

    O produto cria uma sequência ASCII codificada em base-64 das credenciais e gera um cabeçalho que é incluído na página para você. 

    <table>
    <caption>Exemplo de cabeçalho</caption>
      <tr>
      <th>Nome do cabeçalho</th>
      <th>Valor do cabeçalho</th>
      </tr>
      <tr>
      <td>Autorização</td>
      <td>`<encoded-api-key>` básica</td>
      </tr>
    </table>
    
    Para propósitos de teste, é possível enviar uma chave de API sobre a autenticação básica. Clique em **Incluir autorização** e, em seguida, inclua `apikey` no campo **Nome de usuário** e cole o valor da chave da API no campo **Senha**. Clique em **Salvar**.
    {: tip}

Os detalhes de seu webhook são salvos automaticamente.

## Incluindo um callout de webhook em um nó de diálogo
{: #dialog-webhooks-dialog-node-callout}

Para usar um webhook por meio de um nó de diálogo, deve-se ativar webhooks no nó e, em seguida, incluir detalhes para o callout.

1.  Clique na guia  ** Diálogo ** .

1.  Localize o nó de diálogo no qual deseja incluir um callout. O callout para o webhook ocorrerá sempre que esse nó for acionado durante uma conversa com um usuário.

    Por exemplo, é possível que você queira enviar um callout ao webhook por meio do nó `#General_Greetings`.

1.  Clique para abrir o nó de diálogo e, em seguida, clique em **Customizar**.

1.  Role para baixo até a seção *Webhooks* e faça a alternância para **Ligado** e, em seguida, clique em **Aplicar**.

    Se ainda não a ativou, a configuração *Diversas respostas condicionais* será ativada automaticamente e não será possível desativá-la. Essa configuração é ativada para suportar a inclusão de respostas diferentes, dependendo do sucesso ou da falha da chamada de webhook. Se já tiver uma resposta especificada para o nó, ela se tornará a primeira resposta condicional.
    {: note}

1.  Inclua quaisquer dados que você deseja passar para o aplicativo externo como pares de chave e valor na seção *Parâmetros*.

    Por exemplo, ao chamar o serviço Language Translator, deve-se fornecer valores para os parâmetros a seguir:

    <table>
    <caption>Exemplo de parâmetro</caption>
      <tr>
        <th>Chave</th>
        <th>Valor</th>
        <th>Descrição</th>
      </tr>
      <tr>
        <td>model_id</td>
        <td>"en-es"</td>
        <th>Identifica os idiomas de entrada e saída. Neste exemplo, a solicitação é para a tradução de um texto em inglês (en) para o espanhol (es).</th>
      </tr>
      <tr>
        <td>text</td>
        <td>"How are you?"</td>
        <th>Esse parâmetro contém a sequência de texto que você deseja que o serviço traduza. É possível codificar permanentemente esse valor, transmitir uma variável de contexto, como $saved_text, ou transmitir a entrada do usuário ao serviço diretamente, especificando `<? input.text ?>` como esse valor.</th>
      </tr>
    </table>

    Em casos de uso mais complexos, é possível coletar informações durante uma conversa com um usuário sobre os planos de viagem dele, por exemplo. É possível coletar informações de datas e destino e salvá-las em variáveis de contexto que podem ser transmitidas a um aplicativo externo como parâmetros.

    <table>
    <caption>Exemplo de parâmetros de viagem</caption>
      <tr>
        <th>Chave</th>
        <th>Valor</th>
      </tr>
      <tr>
        <td>depart_date</td>
        <td>$departure</td>
      </tr>
    <tr>
        <td>arrive_date</td>
        <td>$arrival</td>
      </tr>
    <tr>
        <td>origin</td>
        <td>$origin</td>
      </tr>
    <tr>
        <td>destination</td>
        <td>$destination</td>
      </tr>
    </table>

1.  Qualquer resposta do callout é salva na variável de retorno. É possível renomear a variável que é automaticamente incluída no campo **Variável de retorno** para você. Se o callout resultar em um erro, essa variável será configurada como `null`.

    O nome da variável gerado tem a sintaxe `webhook_result_n`, em que o `_n` anexado é incrementado cada vez que você inclui um callout de webhook em um nó de diálogo. Essa convenção de nomenclatura garante que os nomes das variáveis de contexto sejam exclusivos na qualificação de diálogo. Ao mudar o nome, certifique-se de usar um nome exclusivo.

1.  Na seção de respostas condicionais, duas condições de resposta serão incluídas automaticamente, uma resposta a ser mostrada quando o callout de webhook for bem-sucedido e uma variável de retorno for enviada de volta e uma resposta a ser mostrada quando o callout falhar. É possível editar essas respostas e incluir mais respostas condicionais no nó.

    Se o callout retornar uma resposta e você souber o formato da resposta JSON, será possível editar a resposta do nó de diálogo para incluir apenas a seção que você deseja compartilhar com os usuários. 
    
    Por exemplo, o serviço Language Translator retorna um objeto semelhante ao seguinte:
    
    ```json
       {
       "translations":[
          {"translation":"¿Cómo estás?"}
       ],
       "word_count":3,
       "character_count":12
       }
    ```
    {: codeblock}
    
    Use uma expressão SpEL que extraia apenas o valor de texto traduzido.

    <table>
    <caption>Exemplo de respostas condicionais</caption>
      <tr>
        <th>Condição</th>
        <th>Resposta</th>
      </tr>
      <tr>
        <td>$webhook_result_1</td>
        <td>Suas palavras em espanhol: <? $webhook_result_1.translations[0].translation ?>.</td>
      </tr>
      <tr>
        <td>anything_else</td>
        <td>A chamada para o aplicativo externo falhou. Tente novamente mais tarde.</td>
      </tr>
    </table>

    Se você usar o formato recomendado para a resposta e a resposta de tradução mostrada anteriormente for retornada, a resposta do assistente para o usuário será: `Your words in Spanish: ¿Cómo estás?`

1.  Quando tiver concluído, clique no X para fechar o nó. Suas mudanças são salvas automaticamente.

## Testando webhooks
{: #dialog-webhooks-test}

Ao incluir pela primeira vez um callout de webhook, pode ser útil ver exatamente o que é retornado na resposta do aplicativo externo, os dados e seu formato. Para isso, inclua esta expressão como a resposta de texto para a resposta condicional de callout bem-sucedida: `$webhook_result_n` em que `n` é o número adequado para o webhook sendo testado.

Esta resposta retorna o corpo completo da variável de retorno, para que seja possível ver o que o callout está enviando de volta e decidir o que compartilhar com o usuário. Em seguida, é possível usar os métodos documentados em [Métodos de linguagem de expressão](/docs/services/assistant?topic=assistant-dialog-methods) para extrair apenas as informações importantes da resposta.

Teste se determinadas entradas do usuário podem gerar erros no callout e crie maneiras de lidar com essas situações. Os erros gerados pelo aplicativo externo são armazenados em `output.webhook_error.<result_variable>`. É possível usar uma resposta condicional como essa ao testar a captura de tais erros:

| Condição | Resposta |
|-----------|----------|
| output.webhook_error | O callout gerou este erro: <? output.webhook_error.webhook_result_1 ?>. |

Por exemplo, é possível que você não esteja autenticando a solicitação corretamente (401) ou esteja tentando transmitir um parâmetro com um nome que já está sendo usado pelo aplicativo externo. Teste o webhook para descobrir e corrigir esses tipos de erros antes de implementá-lo.

## Removendo um webhook
{: #dialog-webhooks-delete}

Se decidir não fazer uma chamada de webhook por meio de um nó de diálogo, abra a página *Customizar* do nó e, em seguida, escolha **Desligar** os webhooks.

A seção *Parâmetros* e o campo **Variável de retorno** foram removidos do editor do nó de diálogo. No entanto, quaisquer respostas condicionais incluídas para ou por você serão mantidas.

A seção *Diversas respostas condicionadas* é editável novamente. É possível optar por desligar o recurso. Se fizer isso, somente a primeira resposta condicional será salva como a única resposta de texto do nó.

Para mudar o serviço externo chamado por meio dos nós de diálogo, edite os detalhes de webhook definidos na página Webhooks da guia **Opções**. Se o novo serviço espera que parâmetros diferentes sejam transmitidos a ele, certifique-se de atualizar nós de diálogo que o chamem.

## Chamando o IBM Cloud Functions
{: #dialog-webhooks-cf}

Você grava a URL do webhook e fornece cabeçalhos de forma diferente ao chamar uma ação padrão ou uma ação da web.

### Chamando uma ação da web
{: #dialog-webhooks-cf-web-action}

As dicas a seguir o ajudarão a chamar uma ação da web do {{site.data.keyword.openwhisk_short}} em seu diálogo. 

1.  Abra a página **Opções** para a qualificação e, em seguida, clique em **Webhooks**.

1.  No campo **URL**, inclua a URL para o aplicativo externo para o qual você deseja enviar callouts de solicitação de POST HTTP.

    Por exemplo, para chamar uma ação da web do {{site.data.keyword.openwhisk_short}}, especifique a URL para a ação da web pública. Por exemplo:

    ```bash
    https://us-south.functions.cloud.ibm.com/api/v1/web/my_org_dev/default/Hello%20World.json
    ```
    {: codeblock}

    Se o aplicativo externo que você chamar retornar uma resposta, ele deverá ser capaz de enviar de volta uma resposta no formato JSON.

    Observe que a URL de solicitação nesse exemplo termina em `.json`. Ao especificar essa extensão, você aproveita um recurso de ações da web que permite especificar o tipo de conteúdo de resposta desejado. Especificar esse tipo de extensão garante que, se as ações da web puderem retornar respostas em mais de um formato, uma resposta JSON seja retornada. Consulte [Recursos adicionais](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_extra){: new_window} para obter mais detalhes.
    {: tip}

1.  Não é necessário incluir cabeçalhos.

    As ações da web do {{site.data.keyword.openwhisk_short}} não precisam ser autenticadas, portanto, não é necessário definir um cabeçalho de autorização.

    Os detalhes de seu webhook são salvos automaticamente.

1.  Clique na guia  ** Diálogo ** .

1.  Clique para abrir o nó de diálogo por meio do qual você deseja chamar a ação da web e, em seguida, clique em **Customizar**.

1.  Role para baixo até a seção *Webhooks* e faça a alternância para **Ligado** e, em seguida, clique em **Aplicar**.

1.  Inclua quaisquer dados que você deseja passar para o aplicativo externo como pares de chave e valor na seção *Parâmetros*.

    Por exemplo, ao chamar a ação da web Hello World do {{site.data.keyword.openwhisk_short}}, é possível que você queira incluir as informações a seguir a serem transmitidas ao parâmetro de mensagem aceito por esse aplicativo:

    <table>
    <caption>Exemplo de parâmetro</caption>
      <tr>
        <th>Chave</th>
        <th>Valor</th>
      </tr>
      <tr>
        <td>mensagem</td>
        <td>"hello"</td>
      </tr>
    </table>

    Ao chamar uma ação da web do {{site.data.keyword.openwhisk_short}}, não é possível transmitir parâmetros com a mesma chave dos parâmetros definidos como parte da ação da web. Consulte [Parâmetros protegidos](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_protect){: new_window} para obter mais detalhes.
    {: note}

1.  É possível editar a resposta do nó de diálogo para incluir somente a seção da resposta que você deseja exibir para os usuários. 

    A ação da web Hello World do {{site.data.keyword.openwhisk_short}} inclui um nome de mensagem e um par de valores na resposta, além de outras informações. Para mostrar apenas o conteúdo da mensagem, é possível usar a sintaxe `<return-variable>.message` para extrair a seção de mensagem apenas do objeto de resposta.

    <table>
    <caption>Exemplo de respostas condicionais</caption>
      <tr>
        <th>Condição</th>
        <th>Resposta</th>
      </tr>
      <tr>
        <td>$webhook_result_1</td>
        <td>O aplicativo retornou "$webhook_result_1.message".</td>
      </tr>
      <tr>
        <td>anything_else</td>
        <td>A chamada para o aplicativo externo falhou. Tente novamente mais tarde.</td>
      </tr>
    </table>

1.  Quando tiver concluído, clique no X para fechar o nó. Suas mudanças são salvas automaticamente.

### Chamando uma ação padrão
{: #dialog-webhooks-cf-action}

É possível fazer uma chamada para uma ação gerenciada pelo Cloud Foundry, mas não para uma ação que usa a autenticação baseada em token do Identity and Access Management (IAM).

As ações do {{site.data.keyword.openwhisk_short}} suportam chamadas síncronas ou assíncronas. No entanto, não é possível fazer uma chamada assíncrona para uma ação do {{site.data.keyword.openwhisk_short}} com um webhook. Ao enviar uma solicitação assíncrona, apenas um ID de ativação é retornado.

Para fazer uma chamada síncrona para uma ação do {{site.data.keyword.openwhisk_short}} gerenciada pelo Cloud Foundry, conclua as etapas a seguir:

1.  Na qualificação em que você deseja incluir o webhook, clique na guia **Opções**.

1.  Clique em **Webhooks**.

1.  No campo **URL**, especifique a URL da ação. 

    Conecte um parâmetro `?blocking=true` à URL da ação para forçar a execução de uma chamada síncrona.

    Esta sintaxe envia uma solicitação síncrona:

    ```bash
    https://us-south.functions.cloud.ibm.com/api/v1/namespaces/my_org_dev/actions/Hello%20World?blocking=true
    ```
    {: codeblock}

    Ao chamar uma ação, deve-se fornecer a chave de API associada a ela como um cabeçalho, o que é descrito na próxima etapa.

1.  Forneça detalhes de autorização com a solicitação.

    Para chamar uma ação do {{site.data.keyword.openwhisk_short}} gerenciada pelo Cloud Foundry, conclua as etapas a seguir:

    1.  Obtenha a chave de API. Na página Terminal do {{site.data.keyword.openwhisk_short}}, localize a seção CURL e clique no ícone de olho para mostrar os detalhes da chave. Copie a chave `user ID:password` listada após o parâmetro `-u` no comando curl.
    
    1.  Clique em **Incluir autorização**, inclua suas credenciais nos campos **Nome de usuário** e **Senha** e, em seguida, clique em **Salvar**. 

    As credenciais são codificadas e um cabeçalho é gerado e incluído na página para você. 
    
    ![Mostra o campo URL e a seção Cabeçalhos da página Opções.](images/webhook-to-cfaction.png)

    <!-- - If you are calling a {{site.data.keyword.openwhisk_short}} action that is managed by IBM Cloud Identity and Access Management (IAM) instead of CLoud Foundry, then you must provide an IAM bearer token to authenticate the request. 
    
      1. Follow the instructions in [Passing an IBM Cloud IAM token to authenticate with a service's API](/docs/iam?topic=iam-iamapikeysforservices#token_auth). 
      
      1. To pass the IAM bearer token, use a header like this:
    
         <table>
         <caption>Header example</caption>
           <tr>
             <th>Header name</th>
             <th>Header value</th>
           </tr>
           <tr>
             <td>Authorization</td>
             <td>Bearer `<IAM token>`</td>
           </tr>
         </table>

        For more information about how to manage IAM tokens, see this [IBM Developer article](https://developer.ibm.com/tutorials/accessing-iam-based-services-from-ibm-cloud-functions/){: external}.
    -->
    Os detalhes de seu webhook são salvos automaticamente.

1.  Clique na guia  ** Diálogo ** .

1.  Clique para abrir o nó de diálogo por meio do qual você deseja chamar a ação da web e, em seguida, clique em **Customizar**.

1.  Role para baixo até a seção *Webhooks* e faça a alternância para **Ligado** e, em seguida, clique em **Aplicar**.

1.  Inclua quaisquer dados que você deseja passar para o aplicativo externo como pares de chave e valor na seção *Parâmetros*.

    Ao chamar uma ação do {{site.data.keyword.openwhisk_short}}, *é* possível transmitir parâmetros com a mesma chave dos parâmetros definidos como parte da ação. Os parâmetros não são reservados, como para ações da web.

1.  É possível editar a resposta do nó de diálogo para incluir somente a seção da resposta que você deseja exibir para os usuários. 

    Por exemplo, na seção de respostas condicionais, é possível usar uma expressão com a sintaxe `$webhook_result.response.result.message` somente para extrair a mensagem retornada.

    <table>
    <caption>Exemplo de respostas condicionais</caption>
      <tr>
        <th>Condição</th>
        <th>Resposta</th>
      </tr>
      <tr>
        <td>$webhook_result_1</td>
        <td>O aplicativo retornou "$webhook_result.response.result.message".</td>
      </tr>
      <tr>
        <td>anything_else</td>
        <td>A chamada para o aplicativo externo falhou. Tente novamente mais tarde.</td>
      </tr>
    </table>

1.  Quando tiver concluído, clique no X para fechar o nó. Suas mudanças são salvas automaticamente.
