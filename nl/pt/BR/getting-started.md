---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-16"

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
{:download: .download}

# Tutorial de introdução
{: #gettingstarted}

Neste tutorial curto, introduzimos a ferramenta do {{site.data.keyword.conversationshort}} e passamos pelo processo de criação de sua primeira conversa.
{: shortdesc}

## Antes de iniciar
{: #prerequisites}

Será necessária uma instância de serviço para iniciar.

<!-- Remove the text marked `download` after there's no g-s tab in the catalog dashboard -->

Você criou sua instância de serviço. Clique em **Gerenciar** e, em seguida,
**Ativar ferramenta**. Acesse a Etapa 2.
{: download tip}

Se você criar um projeto com o serviço do {{site.data.keyword.conversationshort}}, estará pronto com esses pré-requisitos. Acesse a Etapa 1.

1.  Acesse a página {{site.data.keyword.watson}} Developer Console [Services ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.{DomainName}/developer/watson/services){: new_window}.
1.  Selecione {{site.data.keyword.conversationshort}}, clique em **Incluir serviços** e inscreva-se para uma conta do {{site.data.keyword.Bluemix_notm}} grátis ou efetue login.
1.  Mude o nome do projeto para `conversation-tutorial` e, em seguida, clique em **Criar projeto**.

<!-- Remove this text after dedicated instances have the developer console: begin -->

Se você usa o {{site.data.keyword.Bluemix_dedicated_notm}}, crie sua instância de serviço na página [{{site.data.keyword.conversationshort}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.{DomainName}/catalog/services/conversation/){: new_window} no Catálogo.

<!-- Remove this text after dedicated instances have the developer console: end -->

## Etapa 1: Ativar a ferramenta
{: #launch-tool}

Depois de criar um projeto que inclui o serviço do {{site.data.keyword.conversationshort}}, você acessará a página de detalhes do projeto. Ative a ferramenta {{site.data.keyword.conversationshort}} desde aqui.

Clique em **ferramenta Ativar** para {{site.data.keyword.conversationshort}} em **Serviços**.

<!-- To do: Add screenshot for developer console -->

Se você for solicitado a efetuar login na ferramenta, forneça suas credenciais do {{site.data.keyword.Bluemix_notm}}.

Se você não estiver em uma página de detalhes do projeto para o serviço do {{site.data.keyword.conversationshort}}, acesse a página {{site.data.keyword.watson}} Developer Console [Projetos ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://console.{DomainName}/developer/watson/projects) e selecione o projeto.
{: tip}

<!-- Remove this text after dedicated instances have the developer console: begin -->

{{site.data.keyword.Bluemix_dedicated_notm}}: selecione sua instância de serviço no Painel para ativar o conjunto de ferramentas.

<!-- Remove this text after dedicated instances have the Developer Console: end -->

## Etapa 2: Criar uma área de trabalho
{: #create-workspace}

Sua primeira etapa na ferramenta {{site.data.keyword.conversationshort}} é criar uma área de trabalho.

Uma [*área de trabalho*](configure-workspace.html) é um contêiner para os artefatos que define o fluxo de conversa.

1.  Na ferramenta {{site.data.keyword.conversationshort}}, clique em **Criar**.
1.  Forneça à sua área de trabalho o nome `{{site.data.keyword.conversationshort}} tutorial`. Se o diálogo que você planeja construir usará uma linguagem diferente do inglês, escolha a linguagem apropriada na lista. Clique em **Criar**. Você entrará na guia **Intenções** de sua nova área de trabalho.

![Mostra uma animação de um usuário criando uma área de trabalho do tutorial do {{site.data.keyword.conversationshort}}.](images/gs-create-workspace-animated.gif)

## Etapa 3: Criar intenções
{: #create-intents}

Uma [intenção](intents.html) representa o propósito de uma entrada do usuário. É possível pensar em intenções como as ações que seus usuários podem querer executar com seu aplicativo.

Para este exemplo, vamos manter as coisas simples e definir apenas duas intenções: uma para dizer olá e uma para dizer adeus.

1.  Certifique-se de estar na guia Intenções. (Você já deve estar lá se você acabou de criar a área de trabalho.)
1.  Clique em **Incluir intenção**.
1.  Nomeie a intenção `hello` e, em seguida, clique em **Criar intenção**.
1.  Digite `hello` no campo **Incluir exemplo do usuário** e, em seguida, pressione **Enter**.

   *Exemplos* informam ao serviço {{site.data.keyword.conversationshort}} quais tipos de entrada do usuário você deseja corresponder à intenção. Quanto mais exemplos você fornecer, mais preciso o serviço pode ser em reconhecer as intenções dos usuários.
1.  Inclua mais quatro exemplos:
    - `good morning`
    - `greetings`
    - `hi`
    - `howdy`

1.  Clique no ícone **Fechar** ![Seta de fechamento](images/close_arrow.png) para concluir a criação da intenção #hello.
1.  Crie outra intenção chamada #goodbye com esses cinco exemplos:
    - `bye`
    - `farewell`
    - `goodbye`
    - `I'm done`
    - `see you later`

Você criou duas intenções, #hello e #goodbye e forneceu a entrada do usuário de exemplo para treinar o {{site.data.keyword.watson}} para reconhecer essas intenções na entrada de seus usuários.

![Mostra a página Intenções listando as intenções #goodbye e #hello](images/gs-add-intents-result.png)

## Etapa 4: incluir intenções de um catálogo
{: #add-catalog}

Inclua os dados de treinamento que foram construídos pela IBM em sua área de trabalho, incluindo intenções de um catálogo. Em particular, você fornecerá a seu assistente acesso ao catálogo `Informações de negócios` para que o diálogo possa direcionar solicitações do usuário para informações de contato da empresa.

1.  Na ferramenta {{site.data.keyword.conversationshort}}, clique na guia **Catálogo**.
1.  Localize **Informações de negócios** na lista e, em seguida, clique em **Incluir no robô**.
1.  Abra a guia **Intenções** para revisar as intenções e as elocuções de exemplo associadas que foram incluídas em seus dados de treinamento. É possível reconhecê-las porque cada nome de intenção inicia com o prefixo `#Business_Information_`. Você incluirá a intenção `#Business_Information_Contact_Us` em seu diálogo em uma etapa posterior.

Você complementou com sucesso seus dados de treinamento com conteúdo pré-construído fornecido pela IBM.

## Etapa 5: construir um diálogo
{: #build-dialog}

Um [diálogo](dialog-build.html) define o fluxo de sua conversa na forma de uma árvore lógica. Cada nó da árvore tem uma condição que o aciona, com base na entrada do usuário.

Vamos criar um diálogo simples que manipula nossas intenções #hello e #goodbye, cada uma com um nó único.

### Incluindo um nó inicial

1.  Na ferramenta {{site.data.keyword.conversationshort}}, clique na guia **Diálogo**.
1.  Clique em **Criar**. Você verá dois nós:
    - **Bem-vindo**: contém uma saudação que é exibida para os usuários quando eles se relacionam com o robô inicialmente.
    - **Qualquer outra coisa**: contém frases que são usadas para responder aos usuários quando a entrada deles não é reconhecida.

    ![Mostra a árvore de diálogo com os nós Bem-vindo e Qualquer outra coisa](images/gs-add-dialog-node-animated-cover.png)
1.  Clique no nó **Bem-vindo** para abri-lo na visualização de edição.
1.  Substitua a resposta padrão pelo texto `Welcome to the {{site.data.keyword.conversationshort}} tutorial!`.

    ![Mostra o nó Bem-vindo aberto na visualização de edição](images/gs-edit-welcome-node.png)
1.  Clique em ![Fechar](images/close.png) para fechar a visualização de edição.

Você criou um nó diálogo que é acionado pela condição `welcome`, que é uma condição especial que indica que o usuário iniciou uma nova conversa. Seu nó especifica que quando uma nova conversa inicia, o sistema deve responder com a mensagem de boas-vindas.

### Testando o nó inicial

É possível testar seu diálogo a qualquer momento para verificar o diálogo. Vamos testá-lo agora.

- Clique no ícone ![Pergunte ao Watson](images/ask_watson.png) para abrir a área de janela "Experimente". Você deverá ver sua mensagem de boas-vindas.

    ![Testando o nó de diálogo](images/gs-tryitout-welcome-node.png)

### Incluindo nós para manipular intenções

Agora vamos incluir nós para manipular nossas intenções entre o nó `Welcome` e o nó `Anything else`.

1.  Clique no ícone Mais ![Mais opções](images/kabob.png) no nó **Bem-vindo** e, em seguida, selecione **Incluir nó abaixo**.
1.  Digite `#hello` no campo **Insira uma condição** desse nó. Em seguida, selecione a opção **#hello**.
1.  Inclua a resposta, `Good day to you.`
1.  Clique em ![Fechar](images/close.png) para fechar a visualização de edição.

   ![Mostra uma animação de um usuário incluindo um nó hello para o diálogo.](images/gs-add-dialog-node-animated.gif)
1.  Clique no ícone Mais ![Mais opções](images/kabob.png) nesse nó e, em seguida, selecione **Incluir nó abaixo** para criar um nó de mesmo nível. No nó de mesmo nível, especifique `#Business_Information_Contact_Us` como a condição.
1.  Inclua o texto a seguir como a resposta.

    `Call us at 800-426-4968 or give us your feedback at https://www.ibm.com/scripts/contact/contact/us/en.`
1.  Clique no ícone Mais ![Mais opções](images/kabob.png) nesse nó e, em seguida, selecione **Incluir nó abaixo** para criar outro nó peer. No nó de mesmo nível, especifique `#goodbye` como a condição, e `OK. See you later!` como a resposta.

### Testando o reconhecimento de intenção

Você construiu um diálogo simples para reconhecer e responder a ambas as entradas, olá e adeus. Vamos ver o quão bem ele funciona.

1.  Clique no ícone ![Pergunte ao Watson](images/ask_watson.png) para abrir a área de janela "Experimente". Há aquela mensagem de boas-vindas tranquilizadora.
1.  Na parte inferior da área de janela, digite `Hello` e pressione Enter. A saída indica que a intenção #hello foi reconhecida e a resposta apropriada (`Good day to you.`) aparece.
1.  Tente a seguinte entrada:
    - `bye`
    - `howdy`
    - `see ya`
    - `good morning`
    - `sayonara`

   ![Mostra uma animação do usuário testando o diálogo na área de janela Experimente.](images/gs-test-dialog-animated.gif)
1.  Insira `Who can I call if I have questions?` e pressione Enter. A saída indica que a intenção `#Business_Information_Contact_Us` foi reconhecida e a resposta que você incluiu para ela é exibida.

O {{site.data.keyword.watson}} pode reconhecer suas intenções mesmo quando sua entrada não corresponde exatamente aos exemplos incluídos. O diálogo utiliza intenções para identificar o propósito da entrada do usuário independentemente do texto exato usado e então responde da maneira que você especificar.

### Resultado da construção de um diálogo

É isso. Você criou uma conversa simples com duas intenções e um diálogo para reconhecê-las.

## Etapa 6: revisar a área de trabalho de amostra
{: #review-sample-workspace}

Abra a área de trabalho de amostra para ver as intenções semelhantes àquelas que você acabou de criar e muitas mais, e ver como elas são usadas em um diálogo complexo.

1.  Volte para a página Áreas de Trabalho.
   É possível clicar no botão ![Botão voltar para as áreas de trabalho](images/workspaces-button.png) no menu de navegação.
1.  No quadro da área de trabalho **Painel do carro - Amostra**, clique no botão **Editar amostra**.

    ![Mostra o quadro amostra de painel do carro na página Áreas de trabalho](images/gs-workspace-car-sample.png)

## Próximas etapas
{: #next-steps}

Este tutorial é construído em torno de um exemplo simples. Para um aplicativo real, você precisará definir algumas intenções mais interessantes, algumas entidades e um diálogo mais complexo.

- Tente o [tutorial](tutorial.html) avançado para incluir entidades e esclarecer o propósito de um usuário.
- [Implemente](deploy.html) sua área de trabalho conectando-a a uma interface com o usuário de front-end, mídia social ou um canal de mensagens.
- Veja os [aplicativos de amostra](sample-applications.html).
