---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

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

# Habilidades
{: #skills}

Uma qualificação de diálogo contém os dados de treinamento e a lógica que permite que um assistente ajude seus clientes.
{: shortdesc}

Uma qualificação de diálogo contém os tipos de artefatos a seguir:

- [**Intenções**](/docs/services/assistant?topic=assistant-intents): uma *intenção* representa o propósito de uma entrada do usuário, como uma pergunta sobre os locais de negócios ou um pagamento de fatura. Você define uma intenção para cada tipo de solicitação do usuário que você quer que seu aplicativo suporte. Na ferramenta, o nome de uma intenção é sempre prefixado com o caractere `#`. Para treinar a qualificação de diálogo para reconhecer suas intenções, você fornece muitos exemplos de entrada do usuário e indica para quais intenções eles são mapeados.

  Um *catálogo de conteúdo* é fornecido contendo as intenções comuns pré-construídas que podem ser incluídas em seu aplicativo em vez de construir a sua própria. Por exemplo, a maioria dos aplicativos requer uma intenção de saudação que inicia um diálogo com o usuário. É possível incluir o catálogo de conteúdo **Geral** para incluir uma intenção que cumprimenta o usuário e faz outras coisas úteis, como terminar a conversa.

- [**Diálogo**](/docs/services/assistant?topic=assistant-dialog-build): Um *diálogo* é um fluxo de conversa de ramificação que define como seu aplicativo responde quando ele reconhece as intenções e entidades definidas. Use o editor de diálogo na ferramenta para criar conversas com usuários, fornecendo respostas com base nas intenções e entidades que você reconhece em sua entrada.

  ![Diagrama de uma implementação básica que usa somente intenção e diálogo.](images/basic-impl.png)

Para ativar sua qualificação de diálogo para manipular perguntas mais diferenciadas, defina entidades e referencie-as por meio de seu diálogo.

- [**Entidades**](/docs/services/assistant?topic=assistant-entities); Uma *entidade* representa um termo ou objeto que é relevante para suas intenções e que fornece um contexto específico para uma intenção. Por exemplo, uma entidade pode representar uma cidade na qual o usuário deseja localizar um local de negócios ou a quantia de um pagamento de fatura. Na ferramenta, o nome de uma entidade é sempre prefixado com o caractere `@`.

  É possível treinar a qualificação para reconhecer suas entidades, fornecendo valores e sinônimos de termo de entidade, padrões de entidade, ou identificando o contexto no qual uma entidade é geralmente usada em uma sentença. Para refinar seu diálogo, volte e inclua nós que verificam menções de entidade na entrada do usuário, além de intenções.

![Diagrama de uma implementação mais complexa que usa intenção, entidade e diálogo.](images/complex-impl.png)

À medida que você inclui informações, a qualificação usa esses dados exclusivos para construir um modelo de aprendizado de máquina que possa reconhecer essas entradas do usuário e semelhantes. Cada vez que você inclui ou muda os dados de treinamento, o processo de treinamento é acionado para assegurar que o modelo subjacente permaneça atualizado conforme suas necessidades do cliente e os tópicos que eles desejam discutir mudem.

Para ajudar a criar uma qualificação de diálogo, consulte [Criando uma qualificação](/docs/services/assistant?topic=assistant-skill-add).
