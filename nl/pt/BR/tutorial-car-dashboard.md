---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# Tutorial: construindo um diálogo de painel de carro
{: #tutorial-car-dashboard}

Neste tutorial, você usará o serviço {{site.data.keyword.conversationshort}} para criar um diálogo que ajuda os usuários a interagir com um painel de carro inteligente.
{: shortdesc}

## Objetivos do aprendizado
{: #car-dashboard-objectives}

Quando terminar o tutorial, você entenderá como:

- Definir entidades
- Planejar um diálogo
- Usar condições de resposta e nó em um diálogo

### Duração
{: #tutorial-car-dashboard-duration}

Este tutorial levará aproximadamente de 2 a 3 horas para ser concluído.

### Pré-requisito
{: #tutorial-car-dashboard-prereqs}

Antes de iniciar, conclua o [Tutorial de Introdução](/docs/services/assistant?topic=assistant-getting-started).

Você usará a qualificação do tutorial do {{site.data.keyword.conversationshort}} que criou e incluirá nós no diálogo simples construído como parte do exercício de introdução.

## Etapa 1: incluir intenções e exemplos
{: #tutorial-car-dashboard-add-intents}

Inclua uma intenção na guia Intenções. Uma intenção é o propósito ou objetivo expresso em entrada do usuário.

1.  Na página **Intenções** da qualificação de tutorial do {{site.data.keyword.conversationshort}}, clique em **Incluir intenção**.
1.  Inclua o nome da intenção a seguir e, em seguida, clique em **Criar intenção**:

    ```
    turn_on
    ```
    {: codeblock}

    Um `#` é anexado ao nome da intenção que você especificar. A intenção `#turn_on` indica que o usuário deseja ligar um dispositivo como o rádio, limpadores de para-brisa ou faróis.
1.  No campo **Incluir exemplo do usuário**, digite a elocução a seguir e, em seguida, clique em **Incluir exemplo**:

    ```
    I need lights
    ```
    {: codeblock}

1.  Inclua mais esses 5 exemplos para ajudar o Watson a reconhecer a intenção `#turn_on`.

    ```
    Play some tunes
    Turn on the radio
    turn on
    Air on please
    Crank up the AC
    Turn on the headlights
    ```
    {: codeblock}

1.  Clique no ícone **Fechar** ![Seta de fechamento](images/close_arrow.png) para concluir a inclusão da intenção `#turn_on`.

Agora você tem três intenções, a intenção `#turn_on` que acabou de ser incluída e as intenções `#hello` e `#goodbye` que foram incluídas no *Tutorial de introdução* que foi concluído como uma etapa de pré-requisito. Cada intenção tem um conjunto de elocuções de exemplo que ajudam a treinar o Watson para reconhecer as intenções na entrada do usuário.

## Etapa 2: incluir entidades
{: #tutorial-car-dashboard-add-entities}

Uma definição de entidade inclui um conjunto de *valores* de entidade que podem ser usados para acionar respostas diferentes. Cada valor de entidade pode ter vários *sinônimos*, que definem diferentes maneiras com que o mesmo valor pode ser especificado na entrada do usuário.

Crie entidades que podem ocorrer na entrada do usuário que tem a intenção #turn_on para representar o que o usuário deseja ligar.

1.  Clique na guia **Entidades** para abrir a página Entidades.
1.  Clique em **Incluir entidade**.
1.  Inclua o nome da entidade a seguir e, em seguida, pressione Enter:

    ```
    appliance
    ```
    {: codeblock}

    Um `@` é anexado ao nome da entidade que você especificar. A entidade `@appliance` representa um dispositivo no carro que um usuário pode querer ligar.
1.  Inclua o valor a seguir no campo **Nome do valor**:

    ```
    radio
    ```
    {: codeblock}

    O valor representa um dispositivo específico que os usuários podem desejar ativar.
1.  Inclua outras maneiras para especificar a entidade de dispositivo de rádio no campo **Sinônimos**. Pressione **Tab** para dar o foco no campo e, em seguida, insira os sinônimos a seguir. Pressione **Enter** após cada sinônimo.

    ```
    music
    tunes
    ```
    {: codeblock}

1.  Clique em **Incluir valor** para concluir a definição do valor `radio` para a entidade `@appliance`.
1.  Inclua outros tipos de dispositivos.

    - Valor: `headlights`. Sinônimo: `lights`.
    - Valor: `air conditioning`. Sinônimos: `air` e `AC`.

1.  Clique na alternância para que a correspondência difusa seja **Ativada** para a entidade `@appliance`.
    Essa configuração ajuda o serviço a reconhecer referências a entidades na entrada do usuário mesmo quando a entidade é especificada de uma maneira que não corresponde exatamente à sintaxe usada aqui.
1.  Clique no ícone **Fechar** ![Seta de fechamento](images/close_arrow.png) para concluir a inclusão da entidade `@appliance`.
1.  Repita as Etapas 2-8 para criar a entidade `@genre` com a correspondência difusa ativa e estes valores e sinônimos:

    - Valor: `classical`. Sinônimo: `symphonic`.
    - Valor: `rhythm and blues` Sinônimo: `r&b`.
    - Valor: `rock`. Sinônimo: `rock & roll`, `rock and roll` e `pop`.

Você definiu duas entidades: `@appliance` (representando um dispositivo o assistente pode ativar) e `@genre` (representando um gênero de música que o usuário pode escolher ouvir).

Quando a entrada do usuário é recebida, o serviço {{site.data.keyword.conversationshort}} identifica as intenções e entidades. Agora é possível definir um diálogo que usa intenções e entidades para escolher a resposta correta.

## Etapa 3: criar um diálogo complexo
{: #tutorial-car-dashboard-complex-dialog}

Nesse diálogo complexo, você criará ramificações de diálogo que manipularão a intenção #turn_on que você definiu previamente.

### Incluir um nó raiz para #turn_on
{: #tutorial-car-dashboard-add-turn-on}

Crie uma ramificação de diálogo para responder à intenção #turn_on. Inicie criando o nó raiz:

1.  Clique no ícone Mais ![Mais opções](images/kabob.png) no nó **#hello** e, em seguida, selecione **Incluir nó abaixo**.
1.  Comece digitando `#turn_on` no campo de condição e, em seguida, selecione-o na lista.
    Essa condição é acionada por qualquer entrada que corresponda à intenção #turn_on.
1.  Não insira uma resposta nesse nó. Clique em ![Fechar](images/close.png) para fechar a visualização de edição de nó.

### Cenários
{: #tutorial-car-dashboard-scenarios}

O diálogo precisa determinar qual dispositivo o usuário deseja ligar. Para lidar com isso, crie várias respostas com base em condições adicionais.

Há três cenários possíveis, com base nas intenções e entidades que você definiu:

**Cenário 1**: o usuário deseja ligar a música e, nesse caso, o assistente deve solicitar o gênero.

**Cenário 2**: o usuário deseja ligar qualquer outro dispositivo válido, caso em que o assistente ecoa o nome do dispositivo solicitado em uma mensagem que indica que ele está sendo ativado.

**Cenário 3**: o usuário não especifica um nome de dispositivo reconhecível; nesse caso, o assistente deve solicitar esclarecimento.

Inclua nós que verifiquem essas condições do cenário nesse pedido para que o diálogo avalie a condição mais específica primeiro.

### Direcionamento do cenário 1
{: #tutorial-car-dashboard-address-scenario1}

Inclua nós que direcionem o cenário 1, que significa que o usuário deseja ligar a música. Em resposta, o assistente deve solicitar o gênero musical.

#### Inclua um nó-filho que verifica se o tipo de dispositivo é música
{: #tutorial-car-dashboard-add-music-check}

1.  Clique no ícone Mais ![Mais opções](images/kabob.png) no nó **#turn_on** e selecione **Incluir nó-filho**.
1.  No campo de condição, insira `@appliance:radio`.
    Essa condição é verdadeira se o valor da entidade @appliance é `radio` ou um de seus sinônimos, conforme definido na guia Entidades.
1.  No campo de resposta, insira `What kind of music would you like to hear?`
1.  Nomeie o nó `Music`.
1.  Clique em ![Fechar](images/close.png) para fechar a visualização de edição de nó.

#### Inclua um salto do nó #turn_on para o nó Music
{: #tutorial-car-dashboard-add-jump-to-music}

Salte diretamente do nó `#turn on` para o nó `Music` sem pedir qualquer entrada adicional do usuário. Para fazer isso, é possível usar uma ação **Ir para**.

1.  Clique no ícone Mais ![Mais opções](images/kabob.png) no nó **#turn_on** e selecione **Ir para**.
1.  Selecione o nó-filho **Music** e, em seguida, selecione **Se o robô reconhecer (condição)** para indicar que você deseja processar a condição do nó Music.

![Ir para anterior](images/tut-dialog-jumpto.png)

Observe que você tinha que criar o nó de destino (o nó para o qual você deseja saltar) antes de ter incluído a ação **Ir para** ação.

Depois de criar o relacionamento Ir para, você verá uma nova entrada na árvore:

![Ir para próximo](images/tut-dialog-jump2.png)

#### Inclua um nó-filho que verifica o gênero musical
{: #tutorial-car-dashboard-check-genre}

Agora inclua um nó para processar o tipo de música que o usuário solicita.

1.  Clique no ícone Mais ![Mais opções](images/kabob.png) no nó **Music** e selecione **Incluir nó-filho**.
    Esse nó-filho é avaliado somente depois que o usuário responde à pergunta sobre o tipo de música que deseja ouvir. Como precisamos de uma entrada do usuário antes desse nó, não há necessidade de usar uma ação **Ir para**.
1.  Inclua `@genre` no campo de condição.  Essa condição é verdadeira sempre que um valor válido para a entidade @genre é detectado.
1.  Insira `OK! Playing @genre.` como a resposta. Essa resposta reitera o valor gênero que o usuário fornece.

#### Inclua um nó que manipule tipos de gênero não reconhecidos em respostas do usuário
{: #tutorial-car-dashboard-catch-genre}

Inclua um nó para responder quando o usuário não especificar um valor reconhecido para @genre.

1.  Clique no ícone Mais ![Mais opções](images/kabob.png) no nó *@genre* e selecione **Incluir nó abaixo** para criar um nó de mesmo nível.
1.  Insira `true` no campo de condição.
    A condição verdadeira é uma condição especial. Ela especifica que se o fluxo de diálogo atinge esse nó, ele deve sempre ser avaliado como verdadeiro. (Se o usuário especificar um valor @genre válido, esse nó nunca será atingido.)
1.  Insira `I'm sorry, I don't understand. I can play classical, rhythm and blues, or rock music.` como a resposta.

Isso cuida de todos os casos em que o usuário pede para ligar a música.

#### Teste o diálogo para música
{: #tutorial-car-dashboard-test-music}

1.  Selecione o ícone ![Pergunte ao Watson](images/ask_watson.png) para abrir a área de janela de bate-papo.
1.  Digite `Play music`.
    O assistente reconhece a intenção #turn_on e a entidade @appliance:music e ele responde solicitando um gênero musical.

1.  Digite um valor @genre válido (por exemplo, `rock`).
    O assistente reconhece a entidade @genre e responde apropriadamente.

    ![Mostra uma solicitação bem-sucedida para reproduzir música](images/tut-test-music.png)

1.  Digite `Play music` novamente, mas desta vez especifique uma resposta inválida para o gênero. O assistente responde que ele não entende.

### Direcionamento do cenário 2
{: #tutorial-car-dashboard-address-scenario2}

Vamos incluir nós direcionem o cenário 2, que significa que o usuário deseja ligar outro dispositivo válido. Nesse caso, o assistente ecoa o nome do dispositivo solicitado em uma mensagem que indica que ele está sendo ativado.

#### Inclua um nó-filho que verifica qualquer dispositivo
{: #tutorial-car-dashboard-check-applicance}

Inclua um nó que é acionado quando qualquer outro valor válido para @appliance é fornecido pelo usuário.
Para os outros valores de @appliance, o assistente não precisa solicitar mais nenhuma entrada. Ele apenas retorna uma resposta positiva.

1.  Clique no ícone Mais ![Mais opções](images/kabob.png) no nó **Music** e, em seguida, selecione **Incluir nó abaixo** para criar um nó de mesmo nível que é avaliado após a condição @appliance:music ser avaliada.
1.  Insira `@appliance` como a condição do nó.
    Essa condição é acionada se a entrada do usuário incluir qualquer valor reconhecido para a entidade @appliance além da música.
1.  Insira `OK! Turning on the @appliance.` como a resposta.
    Essa resposta reitera o valor do dispositivo que o usuário forneceu.

#### Teste o diálogo com outros dispositivos
{: #tutorial-car-dashboard-test-other-appliances}

1.  Selecione o ícone ![Pergunte ao Watson](images/ask_watson.png) para abrir a área de janela de bate-papo.
1.  Digite `lights on`.

    O assistente reconhece a intenção #turn_on e a entidade @appliance:headlights e ele responde com `OK, turning on the headlights`.

    ![Mostra uma solicitação bem-sucedida para ligar as luzes](images/tut-test-lights.png)

1.  Digite `turn on the air`.

    O assistente reconhece a intenção #turn_on e a entidade @appliance:(air conditioning) e responde com `OK, turning on the air conditioning.`

1.  Tente variações em todos os comandos suportados com base nas elocuções de exemplo e sinônimos de entidade definidos.

### Direcionamento do cenário 3
{: #tutorial-car-dashboard-address-scenario3}

Agora inclua um nó de mesmo nível que é acionado se o usuário não especificar um tipo de dispositivo válido.

1.  Clique no ícone Mais ![Mais opções](images/kabob.png) no nó **@appliance** e, em seguida, selecione **Incluir nó abaixo** para criar um nó de mesmo nível que é avaliado após a condição @appliance ser avaliada.
1.  Insira `true` no campo de condição.
    (Se o usuário especificar um valor @appliance válido, esse nó nunca será atingido.)
1.  Insira `I'm sorry, I'm not sure I understood you. I can turn on music, headlights, or air conditioning.` como a resposta.

#### Teste mais
{: #tutorial-car-dashboard-test-more}

1.  Tente mais variações de elocução para testar o diálogo.

    Se o assistente falha em reconhecer a intenção correta, é possível treiná-lo de novo diretamente na janela de bate-papo. Selecione a seta ao lado da intenção incorreta e escolha a correta na lista.

    ![Mostra a escolha de uma intenção diferente e a reeducação](images/tut-change-intent.gif){: gif}

Opcionalmente, é possível revisar a qualificação **Atendimento ao cliente - Amostra** para ver esse mesmo caso de uso detalhado ainda mais com um diálogo mais longo e funcionalidade adicional.

1.  Clique no botão **Voltar para qualificações** ![Mostra o botão Voltar para qualificações no menu](images/workspaces-button.png) no menu de navegação.

1.  Clique em  ** Incluir uma amostra **.

    A qualificação de amostra é incluída em sua lista de qualificações. Ele não é associado a nenhum assistente.

## Próximas etapas
{: #tutorial-car-dashboard-deploy}

Agora que você construiu e testou sua qualificação de diálogo, é possível compartilhá-la com os clientes. Implemente sua qualificação conectando-a primeiramente a um assistente e, em seguida, implementando o assistente. Há várias maneiras de fazer isso. Consulte  [ Incluindo Integrações ](/docs/services/assistant?topic=assistant-deploy-integration-add)  para obter mais detalhes.

É possível acessar o código-fonte para um aplicativo de amostra de painel de carro integral por meio do [GitHub ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/watson-developer-cloud/car-dashboard).
