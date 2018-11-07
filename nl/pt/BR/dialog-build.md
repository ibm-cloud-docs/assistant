---

copyright:
  years: 2015, 2018
lastupdated: "2018-03-19"

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
{:table: .aria-labeledby="caption"}

# Criando um diálogo
{: #dialog-build}

Use a ferramenta {{site.data.keyword.conversationshort}} para criar seu diálogo.
{: shortdesc}

## Limites do nó de diálogo
{: #dialog-node-limits}

O número de nós de diálogo que podem ser criados depende de seu plano de serviço.

| Plano de Serviço     | Nós de diálogo por área de trabalho |
|------------------|---------------------------:|
| Standard/Premium |                    100.000 |
| Lite             |                     25.000 |
{: caption="Detalhes do plano de serviço" caption-side="top"}

Limite de profundidade da árvore: o serviço suporta 2.000 descendentes de nó do diálogo; o conjunto de ferramentas é melhor executado com 20 ou menos.

## Procedimento
{: #dialog-procedure}

Para criar um diálogo, conclua as etapas a seguir:

1.  Abra a página **Construir** na barra de navegação, clique na guia **Diálogo** e, em seguida, clique em **Criar**.

    Quando você abre o construtor de diálogo pela primeira vez, os nós a seguir são criados para você:
    - **Welcome**: o primeiro nó. Ele contém uma saudação que é exibida para seus usuários quando eles se relacionam pela primeira vez com o serviço. É possível editar a saudação.
    - **Anything else**: O nó final. Ele contém frases que são usadas para responder aos usuários quando suas entradas não são reconhecidas. É possível substituir as respostas que são fornecidas ou incluir mais respostas com um significado semelhante para incluir variedade à conversa. Também é possível escolher se deseja que o serviço retorne cada resposta que está definida por vez ou as retorne em ordem aleatória.
1.  Para incluir mais nós na árvore de diálogo, clique no ícone **Mais** ![Ícone Mais](images/kabob.png) no nó **Bem-vindo** e, em seguida, selecione **Incluir nó abaixo**.
1.  Insira uma condição que, quando atendida, aciona o serviço para processar o nó.

    À medida que você começa a definir uma condição, uma caixa será exibida mostrando suas opções. É possível inserir um dos caracteres a seguir e, em seguida, escolher um valor na lista de opções que é exibida.

    <table>
    <caption>Sintaxe do construtor de condição</caption>
    <tr>
      <th>Caractere</th>
      <th>Lista os valores definidos para esses tipos de artefato</th>
    </tr>
    <tr>
      <td>`#`</td>
      <td>intenções</td>
    </tr>
    <tr>
      <td>`@`</td>
      <td>entidades</td>
    </tr>
    <tr>
      <td>`@{entity-name}:`</td>
      <td>Valores de {entity-name}</td>
    </tr>
    <tr>
      <td>`$`</td>
      <td>Variáveis de contexto que você definiu ou referenciou em outro lugar no diálogo</td>
    </tr>
    </table>

    É possível criar uma nova intenção, entidade, valor de entidade ou variável de contexto definindo uma nova condição que a use. Se você criar um artefato dessa maneira, certifique-se de voltar e concluir quaisquer outras etapas que sejam necessárias para que o artefato seja criado completamente, tal como definir elocuções de amostra para uma intenção.

    Para definir um nó que é acionado com base em mais de uma condição, insira uma condição e, em seguida, clique no ícone de sinal de mais (+) próximo a ela. Se você deseja aplicar um operador `OR` nas várias condições em vez de `AND`, clique no `and` que é exibido entre os campos para mudar o tipo de operador. As operações AND são executadas antes das operações OR, mas você pode mudar a ordem usando parênteses. Por exemplo:
    `$isMember:true AND ($memberlevel:silver OR $memberlevel:gold)`

    A condição que você define deve ser menor que 500 caracteres de comprimento.

    Para obter mais informações sobre como testar valores em condições, consulte [Condições](dialog-overview.html#conditions).
1.  **Opcional**: se você deseja coletar várias partes de informações do usuário nesse nó, clique em **Customizar** e ative **Intervalos**. Veja [Reunindo informações com intervalos](dialog-slots.html) para obter mais detalhes.
1.  Insira uma resposta.
    - Inclua o texto que você deseja que o serviço exiba para o usuário como uma resposta.
    - Se você deseja definir respostas diferentes com base em determinadas condições, clique em **Customizar** e ative **Múltiplas respostas**.
    - Para obter informações sobre respostas condicionais ou como incluir uma variedade de respostas, veja [Respostas](dialog-overview.html##responses).

1.  Especifique o que fazer após o nó atual ser processado. É possível escolher entre as opções a seguir:

    - **Aguardar entrada do usuário**: o serviço pausa até que uma nova entrada seja fornecida pelo usuário.
    - **Ignorar entrada do usuário**: o serviço vai diretamente para o primeiro nó-filho. Essa opção estará disponível somente se o nó atual tiver pelo menos um nó-filho.
    - **Ir para**: o serviço continua o diálogo processando o nó que você especifica. É possível escolher se o serviço deve avaliar a condição do nó de destino ou ir diretamente para a resposta do nó de destino. Veja [Configurando a ação Ir para](dialog-overview.html#jump-to-config) para obter mais detalhes.

1.  **Opcional**: nomeie o nó.

    O nome do nó de diálogo pode conter letras (em Unicode), números, espaços, sublinhados, hifens e pontos.

    A nomeação do nó torna mais fácil para você lembrar de seu propósito e localizar o nó quando ele é minimizado. Se você não fornecer um nome, a condição de nó será usada como o nome.

1.  Para incluir mais nós, selecione um nó na árvore e, em seguida, clique no ícone **Mais** ![Ícone Mais](images/kabob.png).
    - Para criar um nó de mesmo nível que será verificado em seguida se a condição para o nó existente não for atendida, selecione **Incluir nó abaixo**.
    - Para criar um nó de mesmo nível que será verificado antes de a condição para o nó existente ser verificada, selecione **Incluir nó acima**.
    - Para criar um nó-filho para o nó selecionado, selecione **Incluir nó-filho**. Um nó-filho é processado após seu nó pai.
    - Para copiar o nó atual, selecione **Duplicar**.

    Para obter mais informações sobre a ordem na qual os nós de diálogo são processados, consulte [Visão Geral do Diálogo](dialog-overview.html#dialog-flow).
1.  Teste o diálogo à medida que você o constrói.
   Consulte [Testando seu diálogo](#test) para obter informações adicionais.

## Testando seu diálogo
{: #test}

À medida que você faz mudanças em seu diálogo, é possível testá-lo a qualquer momento para ver como ele reage à entrada.

1.  Na guia Diálogo, clique no ícone ![Pergunte ao Watson](images/ask_watson.png).
1.  Na área de janela de bate-papo, digite algum texto e, em seguida, pressione Enter.

    Certifique-se de que o sistema tenha concluído o treinamento sobre suas mudanças mais recentes antes de iniciar a testar o diálogo. Se o sistema ainda estiver treinando, uma mensagem será exibida na área de janela *Experimente*:
    {: tip}

    ![Captura de tela da mensagem de treinamento](images/training.png)
1.  Verifique a resposta para ver se o diálogo interpretou corretamente sua entrada e escolheu a resposta apropriada.

    A janela de bate-papo indica quais intenções e entidades foram reconhecidas na entrada:

    ![Captura de tela da saída de diálogo de teste](images/test_dialog_output.png)
1.  Se você deseja saber qual nó na árvore de diálogo acionou uma resposta, clique no ícone **Local** ![Local](images/location.png) próximo a ele. Se você ainda não estiver na guia Diálogo, abra-a.

    É dado o foco ao nó de origem e a rota que o serviço atravessou na árvore para chegar até ele é destacada. Ela permanece destacada até que você execute outra ação, como inserir uma nova entrada de teste.
1.  Para verificar ou configurar o valor de uma variável de contexto, clique no link **Gerenciar contexto**.

    Quaisquer variáveis de contexto que você definiu no diálogo serão exibidas.

    Além disso, uma variável de contexto `$timezone` é listada. A interface com o usuário da área de janela *Experimente* obtém informações de código de idioma do usuário do navegador da web e as usa para configurar a variável de contexto `$timezone`. Esta variável de contexto torna mais fácil lidar com referências de horário em trocas de diálogo de teste. Considere fazer algo semelhante em seu aplicativo de usuário. Se não especificado, a Hora de Greenwich (GMT) é usada.

    É possível incluir uma variável e configurar seu valor para ver como o diálogo responde na próxima rodada do diálogo de teste. Esse recurso é útil se, por exemplo, o diálogo foi configurado para mostrar diferentes respostas com base em um valor de variável de contexto que é fornecido pelo usuário.

    1.  Para incluir uma variável de contexto, especifique o nome da variável e pressione **Enter**.
    1.  Para definir um valor padrão para a variável de contexto, localize a variável de contexto incluída na lista e, em seguida, especifique um valor para ela.

    Veja [Variáveis de contexto](dialog-runtime.html#context) para obter mais detalhes.

1.  Continue interagindo com o diálogo para ver como a conversa flui através dele.
    - Para localizar e reenviar uma elocução de teste, é possível pressionar a tecla Para Cima para percorrer suas entradas recentes.
    - Para remover elocuções de teste anteriores da área de janela de bate-papo e recomeçar, clique no link **Limpar**. Não apenas as elocuções e respostas de teste são removidas, mas essa ação também limpa os valores de quaisquer variáveis de contexto que foram configuradas como resultado de suas interações com o diálogo. Os valores de variáveis de contexto que você configurar ou mudar explicitamente não serão limpos.

### O que fazer em seguida

Se você determinar que intenções ou entidades erradas estão sendo reconhecidas, poderá precisar modificar suas definições de intenção ou entidade.

Se as intenções e entidades corretas estão sendo reconhecidas, mas os nós errados estão sendo acionados em seu diálogo, certifique-se de que suas condições estejam escritas corretamente.

## Copiando um nó de diálogo
{: #copy-node}

É possível duplicar um nó para criar uma cópia exata dele como um nó de mesmo nível diretamente abaixo dele na árvore de diálogo. O nó copiado em si recebe o mesmo nome que o nó original, mas com `- copy`*`n`* anexado a ele, em que *`n`* é um número que inicia com 1. Se você duplica o mesmo nó mais de uma vez, o *`n`* no nome é incrementado por um para cada cópia para ajudá-lo a distinguir uma cópia da outra. Se o nó não tem nome, ele recebe o nome `copy`*`n`*.

Ao duplicar um nó que tem nós-filhos, os nós-filhos são duplicados também. Os nós-filhos copiados têm exatamente os mesmos nomes que os nós-filhos originais. A única maneira de distinguir um nó-filho copiado de um nó filho original é a referência `copy` no nome do nó pai.

1.  No nó que você deseja copiar, clique no ícone **Mais** ![Ícone Mais](images/kabob.png) e, em seguida, selecione **Duplicar**.
1.  Considere renomear os nós copiados ou editar suas condições para torná-los distintos.

## Movendo um nó de diálogo
{: #move-node}

Cada nó que você cria pode ser movido para outro lugar na árvore de diálogo.

Você pode desejar mover um nó criado anteriormente para outra área do fluxo para mudar a conversa. É possível mover nós para que se tornem irmãos ou de mesmo nível em outra ramificação.

1.  No nó que você deseja mover, clique no ícone **Mais** ![ícone Mais](images/kabob.png) e, em seguida, selecione **Mover**.
1.  Selecione um nó de destino que esteja localizado na árvore perto de onde você deseja mover este nó. Escolha se deseja colocar esse nó antes ou após o nó de destino ou torná-lo um filho do nó de destino.

## Organizando o diálogo com pastas
{: #folders}

É possível agrupar os nós de diálogo incluindo-os em uma pasta. Há muitos motivos para agrupar os nós, incluindo:

- Para manter os nós que direcionam um assunto semelhante juntos para torná-los mais fáceis de serem localizados. Por exemplo, você pode agrupar os nós que direcionam perguntas sobre contas do usuário em uma pasta *Conta do usuário* e os nós que manipulam consultas relacionadas a pagamento em uma pasta *Pagamento*.
- Para agrupar um conjunto de nós que você deseja que o diálogo processe somente se uma determinada condição for atendida. Use uma condição, como `$isPlatinumMember`, por exemplo, para agrupar os nós que oferecem serviços extras que deverão ser processados somente se o usuário atual estiver autorização para receber os serviços extras.
- Para ocultar os nós do tempo de execução enquanto você trabalha neles. É possível incluir os nós em uma pasta com uma condição `false` para evitar que eles sejam processados.
- Para aplicar as mesmas definições de configuração de digressão em um nó para múltiplos nós raiz de uma vez. Veja [Digressões](dialog-runtime.html#digressions) para obter mais informações.

Estas características da pasta afetam como os nós em uma pasta são processados:

- Condição: se especificada, o serviço primeiro avalia a condição da pasta para determinar se deve processar os nós dentro dela.
- Customizações: todas as definições de configuração que você aplica à pasta são herdadas pelos nós na pasta. Se você muda as configurações de digressão da pasta, por exemplo, as mudanças são herdadas por todos os nós na pasta.
- Hierarquia em árvore: os nós em uma pasta são tratados como nós raiz ou filho com base em se a pasta está incluída na árvore de diálogo no nível raiz ou filho. Quaisquer nós de nível raiz que você inclui em uma pasta de nível raiz continuam funcionando como nós raiz; eles não se tornam os nós-filhos da pasta, por exemplo. No entanto, se você move os nós de nível raiz para uma pasta que é uma filha de outro nó, os nós raiz tornam-se filhos desse outro nó.

As pastas não têm impacto sobre a ordem na qual os nós são avaliados. Os nós continuam a ser processados do primeiro ao último. Conforme o serviço desce a árvore, quando ele encontra uma pasta, se a condição da pasta é true, ele processa imediatamente o primeiro nó na pasta e continua descendo a árvore na ordem. Se uma pasta não tem uma condição da pasta, ela é transparente para o serviço.

### Incluindo uma pasta
{: #folders-add}

Para incluir uma pasta em uma árvore de diálogo, conclua as etapas a seguir:

1.  Na visualização em árvore da guia **Diálogo**, clique em **Incluir pasta**.

    A pasta é incluída no final da árvore de diálogo, um pouco antes do nó **Qualquer outra coisa**. A menos que um nó existente na árvore esteja selecionado, nesse caso, ele é incluído abaixo do nó selecionado.

    Se você deseja incluir a pasta em outro lugar na árvore, no nó acima do ponto em que você deseja incluí-la, clique no ícone **Mais** ![Ícone Mais](images/kabob.png) e, em seguida, selecione **Incluir pasta**.

    É possível incluir uma pasta abaixo de um nó-filho em uma ramificação de diálogo existente. Para fazer isso, clique em **Mais** ![Ícone Mais](images/kabob.png) no nó filho e, em seguida, selecione **Incluir pasta**.

    A pasta é aberta na visualização de edição.

1.  **Opcional**: nomeie a pasta.

1.  **Opcional**: defina uma condição para a pasta.

    Se você não especifica uma condição, `true` é usado, significando que os nós na pasta são sempre processados.

1.  Inclua nós de diálogo na pasta.

    - Para incluir nós de diálogo existentes na pasta, deve-se movê-los para a pasta um por vez.

      No nó que você deseja mover, clique no ícone **Mais** ![Ícone Mais](images/kabob.png), selecione **Mover** e, em seguida, clique na pasta. Selecione **Para a pasta** como o destino para o qual mover.

      Conforme você move os nós, eles são incluídos no início da árvore dentro da pasta. Portanto, se você deseja reter a ordem de um conjunto de nós de diálogo raiz consecutivos, por exemplo, mova-os iniciando com o último nó primeiro.
      {: tip}

    - Para incluir um novo nó de diálogo na pasta, clique no ícone **Mais** ![Ícone Mais](images/kabob.png) na pasta e, em seguida, selecione **Incluir nó na pasta**.

      O nó de diálogo é incluído no final da árvore de diálogo dentro da pasta.

### Excluindo uma pasta
{: #folders-delete}

É possível excluir uma pasta sozinha ou a pasta e todos os seus nós de diálogo.

Para excluir uma pasta, conclua as etapas a seguir:

1.  Na visualização em árvore da guia **Diálogo**, localize a pasta que você deseja excluir.

1.  Clique no ícone **Mais** ![Ícone Mais](images/kabob.png) na pasta e, em seguida, selecione **Excluir**.

1.  Execute um dos seguintes procedimentos:

    - Para excluir somente a pasta e manter os nós de diálogo que estão na pasta, desmarque a caixa de seleção **Excluir os nós dentro da pasta** e, em seguida, clique em **Sim, excluir isso**.
    - Para excluir a pasta e todos os seus nós de diálogo, clique em **Sim, excluir isso**.

Se você excluiu somente a pasta, os nós que estavam na pasta são exibidos na árvore de diálogo no ponto em que a pasta ficava.

## Localizando um nó de diálogo pelo ID do nó
{: #get-node-id}

Talvez você deseje localizar o nó de diálogo que está associado a um ID de nó conhecido por qualquer um dos motivos a seguir:

- Você está revisando logs e o log refere-se a uma seção do diálogo pelo ID do nó.
- Você deseja mapear os IDs de nó listados na propriedade `nodes_visited` da saída de mensagem da API para nós que você pode ver em sua árvore de diálogo.
- Uma mensagem de erro de tempo de execução do diálogo informa você sobre um erro de sintaxe e usa um ID de nó para identificar o nó que você precisa corrigir.

Para descobrir um nó com base em seu ID do nó, conclua as etapas a seguir:

1.  Na guia Diálogo do conjunto de ferramentas, selecione qualquer nó em sua árvore de diálogo.
1.  Feche a visualização de edição se ela estiver aberta para o nó atual.
1.  No campo de local do navegador da web, deve ser exibida uma URL que possa a sintaxe a seguir:

    `    https://watson-conversation.ng.bluemix.net/space/instance-id/workspaces/workspace-id/build/dialog#node=node-id
    `

1.  Edite a URL substituindo o valor de `node-id` atual pelo ID do nó que você deseja localizar e, em seguida, envie a nova URL.
1.  Se necessário, destaque a URL editada novamente e reenvie-a.

O conjunto de ferramentas é atualizado e muda o foco para o nó de diálogo com o ID do nó que você especificou. Se o ID do nó é para um intervalo, um manipulador de intervalo ou uma resposta condicional, o nó no qual o intervalo ou resposta condicional é definido recebe foco e o modal correspondente é exibido.

**Nota**: caso o nó ainda não possa ser localizado, será possível exportar a área de trabalho e usar um editor JSON para procurar o arquivo JSON da área de trabalho.
