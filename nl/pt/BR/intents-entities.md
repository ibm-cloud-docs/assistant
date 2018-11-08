---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-09"

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

# Planejando suas intenções e entidades
{: #planning-your-entities}

Para planejar as *intenções* para seu aplicativo, você precisa considerar o que seus clientes podem querer fazer e o que você quer que seu aplicativo seja capaz de manipular. A escolha da intenção correta para uma entrada de usuário é o primeiro passo para fornecer uma resposta útil. As intenções identificadas para seu aplicativo determinarão os fluxos de diálogo que precisarão ser criados; elas também podem determinar com quais sistemas backend seu aplicativo precisa se integrar para concluir as solicitações dos clientes (como bancos de dados do cliente ou sistemas de processamento de pagamento).

Uma *entidade* representa um termo ou objeto na entrada do usuário que fornece esclarecimento ou contexto específico para uma intenção específica. Se intenções representam verbos (algo que um usuário deseja fazer), as entidades representam substantivos (como o objeto de, ou o contexto para, uma ação). Entidades possibilitam que uma única intenção represente várias ações específicas. Uma entidade define uma classe de objetos, com valores específicos representando objetos possíveis na classe.

Basicamente, se você quiser capturar uma solicitação ou executar uma ação, use uma intenção. Se você deseja capturar informações que podem afetar como você responde a um pedido ou ação, use uma entidade.

Como exemplo, suponha que queira criar um aplicativo de painel de carro inteligente que permita que os usuários ativem ou desativem acessórios:

1.  Reúna tantas perguntas, comandos ou outras entradas de clientes reais quanto possível. O uso de entrada de usuários reais dá uma imagem melhor da entrada esperada do que ter especialistas criando listas de possíveis elocuções. Lembre-se de que os clientes podem expressar o mesmo tipo de solicitação de várias maneiras diferentes. Por exemplo, todas as elocuções de exemplo a seguir representam solicitações para desligar algo:

    - `Headlights off`
    - `Turn the radio off`
    - `Stop the air conditioner`

1.  Quando tiver uma lista de exemplos, classifique-os em categorias com base nos recursos que deseja que seu aplicativo suporte; essas categorias representam as intenções que você definirá, enquanto os exemplos ajudarão seu aplicativo a identificar essas intenções na nova entrada.

    Não faça suas intenções muito semelhantes. Intenções semelhantes podem ser difíceis para o serviço {{site.data.keyword.conversationshort}} distinguir. Se você achar que tem várias intenções com o mesmo significado, considere se pode combiná-las em uma única intenção e, em seguida, use as entidades para fornecer várias respostas possíveis para essa intenção.
    {: tip}

    Como todos os exemplos anteriores representam a mesma intenção (desligar algo), você pode chamar essa categoria de `#turn_off`.
    Lembre-se de que a entrada de um cliente não precisa ser uma correspondência exata para qualquer um dos exemplos; as intenções são reconhecidas usando processamento de linguagem natural.
    {: tip}

1.  Você poderia, então, usar uma entidade para representar o que o usuário deseja desligar. Para fazer isso, você pode criar uma entidade chamada `@accessory` e fornecer-lhe os valores possíveis a seguir:

    - `headlights`
    - `radio`
    - `air conditioner`

    Para cada valor, também é possível especificar sinônimos. Por exemplo, você pode especificar `AC` e `cooling` como sinônimos para `air conditioner`; todas as três sequências são tratadas como o mesmo valor. Listar várias maneiras como os usuários podem referir-se ao mesmo valor ajuda a melhorar a precisão de seu aplicativo.
1.  Quando a entrada do usuário é recebida, a conversa reconhece as intenções e entidades. Seu fluxo de diálogo pode, então, usá-las para fornecer a melhor resposta. Você deve criar uma entidade apenas para algo que importa em termos de mudar a forma como o aplicativo responde a uma intenção.
    Além das entidades definidas por você, o serviço fornece um conjunto de entidades do sistema que já estão definidas e disponíveis para seu uso. Para obter mais informações, consulte [Ativando entidades do sistema](entities.html#enable_system_entities).
    {: tip}

1.  Continue refinando suas intenções, entidades e exemplos conforme necessário. Não pense em seu conjunto de intenções e entidades como produtos acabados. É provável que você, ao [construir seus diálogos](dialog-build.html), identifique intenções e entidades adicionais que precisem ser incluídas. Também é possível continuar reunindo entrada de novos clientes e usá-las para incluir novos exemplos; esse processo iterativo melhora a capacidade de seu aplicativo para reconhecer intenções e entidades com precisão.
