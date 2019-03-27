---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-01"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
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
{:hide-dashboard: .hide-dashboard}
{:download: .download}
{:gif: data-image-type='gif'}

# Tutorial de introdução
{: #getting-started}

Neste curto tutorial, introduzimos a ferramenta {{site.data.keyword.conversationshort}} e passaremos pelo processo de criação do seu primeiro assistente.
{: shortdesc}

## Antes de iniciar
{: #getting-started-prerequisites}
{: hide-dashboard}

É necessária uma instância de serviço para iniciar.
{: hide-dashboard}

1.  {: hide-dashboard}Acesse a página [{{site.data.keyword.conversationshort}} ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/catalog/services/watson-assistant) no catálogo do {{site.data.keyword.cloud_notm}}.

    A instância do serviço será criada no grupo de recursos **default** se você não escolher uma diferente e *não poderá* ser mudada posteriormente. Esse grupo é suficiente para os propósitos de experimentar o serviço.

    Se você estiver criando uma instância para uso mais robusto, saiba mais sobre os [grupos de recursos ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://{DomainName}/docs/resources/bestpractice_rgs#bp_resourcegroups){: new_window}.
1.  {: hide-dashboard} Inscreva-se para obter uma conta gratuita do {{site.data.keyword.cloud_notm}} ou efetue login.
1.  {: hide-dashboard} Clique em **Criar**.

## Etapa 1: abrir a ferramenta
{: #getting-started-launch-tool}

Depois de criar uma instância de serviço do {{site.data.keyword.conversationshort}}, você chegará à página **Gerenciar** do painel de serviço.
{: hide-dashboard}

1.  Clique em **Ativar ferramenta**. Se for solicitado para que efetue login na ferramenta, forneça suas credenciais do {{site.data.keyword.cloud_notm}}.

<!-- Remove this text after dedicated instances have the developer console: begin -->

{{site.data.keyword.Bluemix_dedicated_notm}}: selecione sua instância de serviço no Painel para ativar a ferramenta.

<!-- Remove this text after dedicated instances have the Developer Console: end -->

## Etapa 2: criar uma habilidade de diálogo
{: #getting-started-add-skill}

Sua primeira etapa na ferramenta {{site.data.keyword.conversationshort}} é criar uma qualificação.

Uma *qualificação de diálogo* é um contêiner para os artefatos que definem o fluxo de uma conversa que seu assistente pode ter com seus clientes.

1.  Na página inicial da ferramenta {{site.data.keyword.conversationshort}}, clique em **Criar uma qualificação**.

    ![Mostra o botão Incluir qualificação na página inicial](images/gs-new-skill.png)

1.  Clique em **Criar novo**.

    ![Mostra o botão Criar novo na página Qualificações](images/gs-click-create-new.png)

1.  Forneça à sua qualificação o nome `Conversational skill tutorial`.
1.  ** Opcional **. Se o diálogo que você planeja construir usará uma linguagem diferente do inglês, escolha a linguagem apropriada na lista.
1.  Clique em **Criar**.

    ![Finish creating the skill](images/gs-add-skill-done.png)

Você chegará à página Intenções da ferramenta.

## Etapa 3: incluir intenções de um catálogo de conteúdo
{: #getting-started-add-catalog}

Inclua dados de treinamento que foram construídos pela IBM para sua qualificação, incluindo intenções de um catálogo de conteúdo. Em particular, você fornecerá ao seu assistente acesso ao catálogo de conteúdo **Geral** para que seu diálogo possa saudar os usuários e terminar conversas com eles.

1.  Na ferramenta {{site.data.keyword.conversationshort}}, clique na guia **Catálogo de conteúdo**.
1.  Localize **Geral** na lista e, em seguida, clique em **Incluir na qualificação**.

    ![Mostra o Catálogo de conteúdo e destaca o botão Incluir na qualificação do catálogo Geral.](images/gs-add-general-catalog.png)
1.  Abra a guia **Intenções** para revisar as intenções e as elocuções de exemplo associadas que foram incluídas em seus dados de treinamento. É possível reconhecê-las porque cada nome de intenção inicia com o prefixo `#General_`. Você incluirá as intenções `#General_Greetings` e `#General_Ending` em seu diálogo na próxima etapa.

    ![Mostra as intenções que são exibidas na guia Intenções após o catálogo Geral ser incluído.](images/gs-general-added.png)

Você iniciou com êxito a construção de seus dados de treinamento incluindo o conteúdo pré-construído da {{site.data.keyword.IBM_notm}}.

## Etapa 4: Construir um diálogo
{: #getting-started-build-dialog}

Um [diálogo](/docs/services/assistant?topic=assistant-dialog-overview) define o fluxo de sua conversa na forma de uma árvore lógica. Ele corresponde intenções (o que os usuários dizem) a respostas (o que o robô diz de volta). Cada nó da árvore tem uma condição que o aciona, com base na entrada do usuário.

Nós criaremos um diálogo simples que manipula as intenções de saudação e encerramento, cada uma com um único nó.

### Incluindo um nó inicial

1.  Na ferramenta {{site.data.keyword.conversationshort}}, clique na guia **Diálogo**.
1.  Clique em **Criar**. Você vê dois nós:
    - **Bem-vindo**: contém uma saudação que é exibida para os usuários quando eles se engajam pela primeira vez com o assistente.
    - **Qualquer outra coisa**: contém frases que são usadas para responder aos usuários quando a entrada deles não é reconhecida.

    ![Um novo diálogo com dois nós integrados](images/gs-new-dialog.png)
1.  Clique no nó **Bem-vindo** para abri-lo na visualização de edição.
1.  Substitua a resposta padrão pelo texto `Welcome to the Watson Assistant tutorial!`.

    ![Editando a resposta do nó de boas-vindas](images/gs-edit-welcome.png)
1.  Clique em ![Fechar](images/close.png) para fechar a visualização de edição.

Você criou um nó de diálogo que é acionado pela condição `welcome`. (`welcome` é uma condição especial que funciona como uma intenção, mas não inicia com um `#`.) Ela é acionada quando uma nova conversa se inicia. Seu nó especifica que quando uma nova conversa se inicia, o sistema deve responder com a mensagem de boas-vindas que você inclui na seção de resposta desse primeiro nó.

### Testando o nó inicial

É possível testar seu diálogo a qualquer momento para verificar o diálogo. Vamos testá-lo agora.

- Clique no ícone ![Tente](images/ask_watson.png) para abrir a área de janela "Experimente". Você deverá ver sua mensagem de boas-vindas.

### Incluindo nós para manipular intenções

Agora, vamos incluir nós entre o nó `Welcome` e o nó `Anything else` que manipula nossas intenções.

1.  Clique no ícone Mais ![Mais opções](images/kabob.png) no nó **Bem-vindo** e, em seguida, selecione **Incluir nó abaixo**.
1.  Digite `#General_Greetings` no campo **Inserir uma condição** desse nó. Em seguida, selecione a opção  ** ` #General_Greetings ` ** .
1.  Inclua a resposta `Good day to you!`
1.  Clique em ![Fechar](images/close.png) para fechar a visualização de edição.

   ![Um nó de saudação geral foi incluído no diálogo.](images/gs-add-greeting-node.png)

1.  Clique no ícone Mais ![Mais opções](images/kabob.png) nesse nó e, em seguida, selecione **Incluir nó abaixo** para criar um nó de mesmo nível. No nó de mesmo nível, especifique `#General_Ending` como a condição e `OK. See you later.` como a resposta.

   ![Incluindo um nó de encerramento no diálogo.](images/gs-add-ending-node.png)

1.  Clique em ![Fechar](images/close.png) para fechar a visualização de edição.

   ![Mostra que um nó de encerramento geral também foi incluído no diálogo.](images/gs-ending-added.png)

### Testando o reconhecimento de intenção

Você construiu um diálogo simples para reconhecer e responder às entradas de saudação e de encerramento. Vamos ver o quão bem ele funciona.

1.  Clique no ícone ![Tente](images/ask_watson.png) para abrir a área de janela "Experimente". Há aquela mensagem de boas-vindas tranquilizadora.
1.  Na parte inferior da área de janela, digite `Hello` e pressione Enter. A saída indica que a intenção #hello foi reconhecida e a resposta apropriada (`Good day to you.`) aparece.
1.  Tente a seguinte entrada:
    - `bye`
    - `howdy`
    - `see ya`
    - `good morning`
    - `sayonara`

![Testando o diálogo na área de janela Experimente](images/gs-try-it.gif){: gif}

O {{site.data.keyword.watson}} pode reconhecer suas intenções mesmo quando sua entrada não corresponde exatamente aos exemplos incluídos. O diálogo utiliza intenções para identificar o propósito da entrada do usuário independentemente do texto exato usado e então responde da maneira que você especificar.

### Resultado da construção de um diálogo

É isso. Você criou uma conversa simples com duas intenções e um diálogo para reconhecê-las.

## Etapa 5: Criar um Assistente
{: #getting-started-create-assistant}

Um [*assistente*](/docs/services/assistant?topic=assistant-assistants) é um robô cognitivo no qual você inclui uma qualificação que permite que ele interaja com seus clientes de maneiras úteis.

1.  Clique na guia  ** Assistentes ** .
1.  Clique em **Criar novo**.

    ![Botão Criar novo na guia Assistente](images/gs-create-assistant.png)
1.  Nomeie o assistente do  ` Assistente do Watson Assistant `.
1.  No campo Descrição, insira `This is a sample assistant that I am creating to help me learn.`
1.  Clique em **Criar**.

    ![Finish creating the new assistant](images/gs-create-assistant-done0.png)

## Etapa 6: incluir sua qualificação no assistente
{: #getting-started-add-skill-to-assistant}

Inclua a qualificação de diálogo que você construir no assistente criado.

1.  Na página de novo assistente, clique em **Incluir qualificação**.

    Se você criou ou recebeu o acesso de função de desenvolvedor a quaisquer áreas de trabalho que foram construídas com a versão geralmente disponível do serviço do {{site.data.keyword.conversationshort}}, elas serão listadas na página Qualificações como qualificações de conversação.
    {: tip}

    ![Mostra o botão Incluir qualificação da página Assistente](images/gs-add-skill.png)
1.  Escolha incluir a qualificação que você criou anteriormente para o assistente.

## Etapa 7: Integrar o assistente
{: #getting-started-integrate-assistant}

Agora que você tem um assistente que pode participar de uma troca de conversação simples, publique-o em uma página da web pública na qual possa testá-lo. O serviço fornece uma integração integrada que é chamada de Link de visualização. Ao criar esse tipo de integração, isso constrói seu assistente em um widget de bate-papo que é hospedado por uma página da web com a marca da IBM. É possível abrir a página da web e bater papo com seu assistente para testá-lo.

1.  Clique na guia **Assistentes**, localize o assistente `Watson Assistant tutorial` criado e abra-o.
1.  Na área *Integrações*, clique em **Incluir integração**.
1.  Localize **Link de visualização** e clique em **Selecionar integração**.

1.  Clique na URL exibida na página.

    A página é aberta em uma nova guia.
1.  Diga `hello` para seu assistente e observe-o responder. É possível compartilhar a URL com outras pessoas que podem desejar experimentar seu assistente.

## Próximas etapas
{: #getting-started-next-steps}

Este tutorial é construído em torno de um exemplo simples. Para um aplicativo real, é necessário definir algumas intenções mais interessantes, algumas entidades e um diálogo mais complexo que use ambas. Quando você tem uma versão polida do assistente, é possível integrá-la a canais que seus clientes usam, como o Slack. À medida que o tráfego aumenta entre o assistente e seus clientes, é possível usar as ferramentas que são fornecidas na guia **Analítica** para analisar conversas reais e identificar áreas para melhoria.

- Conclua os tutoriais de continuidade que constroem diálogos mais avançados:
    - Inclua nós padrão com o tutorial [Construindo um diálogo complexo](/docs/services/assistant?topic=assistant-tutorial).
    - Saiba sobre os intervalos com o tutorial [Incluindo um nó com intervalos](/docs/services/assistant?topic=assistant-tutorial-slots).
- Verifique mais [aplicativos de amostra](/docs/services/assistant?topic=assistant-sample-apps) para obter ideias.
