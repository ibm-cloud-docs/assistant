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

# Tutorial: incluindo um nó com intervalos em um diálogo
{: #tutorial-slots}

Neste tutorial, você incluirá intervalos em um nó de diálogo para coletar múltiplas informações de um usuário em um único nó. O nó que você cria coletará as informações necessárias para fazer uma reserva de restaurante.
{: shortdesc}

## Objetivos do aprendizado
{: #tutorial-slots-objectives}

Quando terminar o tutorial, você entenderá como:

- Definir as intenções e entidades que são necessárias ao seu diálogo
- Incluir intervalos em um nó de diálogo
- Teste o nó com intervalos

### Duração
{: #tutorial-slots-duration}

Este tutorial levará aproximadamente 30 minutos para ser concluído.

### Pré-requisito
{: #tutorial-slots-prereqs}

Antes de iniciar, conclua o [Tutorial de Introdução](/docs/services/assistant?topic=assistant-getting-started). Você usará a qualificação do tutorial do {{site.data.keyword.conversationshort}} que criou e incluirá nós no diálogo simples construído como parte do exercício de introdução.

Também é possível iniciar com uma nova qualificação de diálogo, se desejar. Basta ter certeza de criar a qualificação antes de iniciar este tutorial.
{: note}

## Etapa 1: incluir intenções e exemplos
{: #tutorial-slots-add-intent}

Inclua uma intenção na guia Intenções. Uma intenção é o propósito ou objetivo expresso na entrada do usuário. Você incluirá uma intenção #reservation que reconhece a entrada do usuário que indica que o usuário deseja fazer uma reserva de restaurante.

1.  Na página **Intenções** da qualificação de tutorial, clique em **Incluir intenção**.
1.  Inclua o nome da intenção a seguir e, em seguida, clique em **Criar intenção**:

    ```json
    reservation
    ```
    {: screen}

    A intenção #reservation é incluída. Um sinal de número (`#`) é anexado ao nome da intenção para rotulá-la como uma intenção. Essa convenção de nomenclatura ajuda você e outros a reconhecer a intenção como uma intenção. Não há elocuções do usuário de exemplo associadas a ela ainda.
1.  No campo **Incluir exemplos do usuário**, digite a elocução a seguir e, em seguida, clique em **Incluir exemplo**:

    ```json
    i'd like to make a reservation
    ```
    {: screen}

1.  Inclua estes exemplos adicionais para ajudar o Watson a reconhecer a intenção `#reservation`.

    ```json
    I want to reserve a table for dinner
    Can 3 of us get a table for lunch?
    do you have openings for next Wednesday at 7?
    Is there availability for 4 on Tuesday night?
    i'd like to come in for brunch tomorrow
    can i reserve a table?
    ```
    {: screen}

1.  Clique no ícone **Fechar** ![Seta de fechamento](images/close_arrow.png) para concluir a inclusão da intenção `#reservation` e suas elocuções de exemplo.

## Etapa 2: incluir entidades
{: #tutorial-slots-add-entity}

Uma definição de entidade inclui um conjunto de *valores* de entidade que representam o vocabulário que é frequentemente usado no contexto de uma determinada intenção. Ao definir entidades, é possível ajudar o serviço a identificar referências na entrada do usuário que estão relacionadas a intenções de interesse. Nesta etapa, você ativará as entidades do sistema que podem reconhecer referências ao horário, data e números.

1.  Clique em **Entidades** para abrir a página Entidades.
1.  Ative as entidades do sistema que podem reconhecer as referências de data, horário e número na entrada do usuário. Clique na guia **Entidades do sistema** e, em seguida, ative estas entidades:

    - `@sys-time`
    - `@sys-date               `
    - `@sys-number               `

Você ativou com êxito as entidades do sistema @sys-date, @sys-time e @sys-number. Agora é possível usá-las em seu diálogo.

## Etapa 3: incluir um nó de diálogo com intervalos
{: #tutorial-slots-add-dialog-with-slots}

Um nó de diálogo representa o início de um encadeamento de diálogo entre o serviço e o usuário. Ele contém uma condição que deve ser atendida para que o nó seja processado pelo serviço. No mínimo, ele também contém uma resposta. Por exemplo, uma condição de nó pode procurar a intenção `#hello` na entrada do usuário e responder com `Hi. How can I help you?` Esse exemplo é a forma mais simples de um nó de diálogo, uma que contém uma condição única e uma resposta única. É possível definir diálogos complexos, incluindo respostas condicionais em um único nó, incluindo nós-filhos que prolongam a troca com o usuário e muito mais. (Se você deseja aprender mais sobre diálogos complexos, é possível concluir o tutorial [Construindo um diálogo complexo](/docs/services/assistant?topic=assistant-tutorial).)

O nó que você incluirá nesta etapa é aquele que contém intervalos. Os intervalos fornecem um formato estruturado por meio do qual é possível perguntar e salvar múltiplas informações de um usuário em um único nó. Eles são mais úteis quando você tem uma tarefa específica em mente e precisa de informações chave do usuário antes de poder executá-la. Veja [Reunindo informações com intervalos](/docs/services/assistant?topic=assistant-dialog-slots) para obter mais informações.

O nó que você incluir coletará as informações necessárias para fazer uma reserva em um restaurante.

1.  Clique na guia **Diálogos** para abrir a árvore de diálogo.
1.  Clique no ícone Mais ![Mais opções](images/kabob.png) no nó **#General_Greetings** e, em seguida, selecione **Incluir o nó abaixo**.
1.  Comece digitando `#reservation` no campo de condição e, em seguida, selecione-o na lista.
    Esse nó será avaliado se a entrada do usuário corresponder à intenção `#reservation`.
1.  Clique em **Customizar**, clique na alternância **Intervalos** para **Ativar** e, em seguida, clique em **Aplicar**.

    ![Mostra o diálogo de customização no qual os intervalos são ativados.](images/slots-toggle-on.png)
1.  Defina os intervalos a seguir:

    <table>
    <caption>Detalhes do intervalo</caption>
    <tr>
      <th>Verificar</th>
      <th>Salvá-lo como</th>
      <th>Se não estiver presente, peça</th>
    </tr>
    <tr>
      <td>@sys-date</td>
      <td>$date</td>
      <td>Que dia você gostaria de vir?</td>
    </tr>
    <tr>
      <td>@sys-time</td>
      <td>$time</td>
      <td>Para qual horário você deseja que a reserva seja feita?</td>
    </tr>
    </tr>
    <tr>
      <td>@sys-number</td>
      <td>$guests</td>
      <td>Quantas pessoas estarão jantando?</td>
    </tr>
    </table>

1.  Como resposta, especifique `OK. I am making you a reservation for $guests on $date at $time.`

    ![Mostra como cada intervalo e a resposta se parecem na ferramenta quando preenchidos conforme especificado.](images/slots-simple-node.png)

1.  Clique em ![Fechar](images/close.png) para fechar a visualização de edição de nó.

## Etapa 4: testar o diálogo
{: #tutorial-slots-test}

1.  Selecione o ícone ![Pergunte ao Watson](images/ask_watson.png) para abrir a área de janela de bate-papo.
1.  Digite `i want to make a reservation`.

    O assistente reconhece a intenção #reservation e ele responde com o prompt para o primeiro intervalo, `What day would you like to come in?`.

1.  Digite `Friday`.

    O assistente reconhece o valor e usa-o para preencher a variável de contexto $date para o primeiro intervalo. Em seguida, mostra o prompt para o próximo intervalo, `What time do you want the reservation to be made for?`

1.  Digite `5pm`.

    O assistente reconhece o valor e usa-o para preencher a variável de contexto $time para o segundo intervalo. Em seguida, mostra o prompt para o próximo intervalo, `How many people will be dining?`

1.  Digite `6`.

    O assistente reconhece o valor e usa-o para preencher a variável de contexto $guests para o terceiro intervalo. Agora que todos os intervalos estão preenchidos, ele mostra a resposta do nó `OK. I am making you a reservation for 6 on 2017-12-29 at 17:00:00.`

![Mostra um diálogo de teste na área de janela Experimente que é preenchido com êxito nos intervalos do nó.](images/slots-test-simple-node.png)

Funcionou! Parabéns! Você criou com êxito um nó com intervalos.

## Resumo
{: #tutorial-slots-summary}

Neste tutorial, você criou um nó com intervalos que podem capturar as informações necessárias para reservar uma mesa em um restaurante.

## Próximas etapas
{: #tutorial-slots-next-steps}

Melhore a experiência de usuários que interagem com o nó. Conclua o tutorial de continuidade, [Melhorando um nó com intervalos](/docs/services/assistant?topic=assistant-tutorial-slots-complex). Ele abrange as melhorias simples, como reformatar os valores de data (28-12-2017) e horário (17h) que são retornados pelo sistema. Ele também abrange tarefas mais complexas, como o que fazer se o usuário não fornecer o tipo de valor que seu diálogo espera para um intervalo.
