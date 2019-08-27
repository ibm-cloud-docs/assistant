---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-02"

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

Uma qualificação é um contêiner para a inteligência artificial que permite que um assistente ajude seus clientes.
{: shortdesc}

Um assistente direciona as solicitações para o caminho ideal de resolução de um problema do cliente. Inclua qualificações para que seu assistente possa fornecer uma resposta direta a uma pergunta comum ou fazer referência a resultados de procura mais generalizados para algo mais complexo.

## Tipos de qualificação
{: #skills-types}

É possível incluir o tipo de qualificação a seguir em seu assistente:

- **[Qualificação de diálogo](#skills-dialog-skill)**: entende perguntas ou solicitações típicas de usuários e as responde ou preenche seguindo um diálogo roteirizado por você.

![Somente nos planos Plus ou Premium](images/plus.png) Se você for um usuário do plano Plus ou Premium, também será possível criar esse tipo de qualificação:

- **[Qualificação de procura](#skills-search-skill)**: responde à pergunta de um usuário procurando informações relevantes em uma origem de dados externa, extraindo a passagem e retornando-a como a resposta do assistente.

### Qualificação de diálogo
{: #skills-dialog-skill}

Uma qualificação de diálogo contém os dados de treinamento e a lógica que permite que um assistente ajude seus clientes. Ela contém os tipos de artefatos a seguir:

- [**Intenções**](/docs/services/assistant?topic=assistant-intents): uma *intenção* representa o propósito de uma entrada do usuário, como uma pergunta sobre os locais de negócios ou um pagamento de fatura. Você define uma intenção para cada tipo de solicitação do usuário que você quer que seu aplicativo suporte. O nome de uma intenção é sempre prefixado com o caractere `#`. Para treinar a qualificação de diálogo para reconhecer suas intenções, você fornece muitos exemplos de entrada do usuário e indica para quais intenções eles são mapeados.

  Um *catálogo de conteúdo* é fornecido contendo as intenções comuns pré-construídas que podem ser incluídas em seu aplicativo em vez de construir a sua própria. Por exemplo, a maioria dos aplicativos requer uma intenção de saudação que inicia um diálogo com o usuário. É possível incluir o catálogo de conteúdo **Geral** para incluir uma intenção que cumprimenta o usuário e faz outras coisas úteis, como terminar a conversa.

- [**Diálogo**](/docs/services/assistant?topic=assistant-dialog-build): Um *diálogo* é um fluxo de conversa de ramificação que define como seu aplicativo responde quando ele reconhece as intenções e entidades definidas. Você utiliza o editor de diálogo para criar conversas com os usuários, fornecendo respostas com base nas intenções e nas entidades reconhecidas na entrada deles.

  ![Diagrama de uma implementação básica que usa somente intenção e diálogo.](images/basic-impl.png)

Para ativar sua qualificação de diálogo para manipular perguntas mais diferenciadas, defina entidades e referencie-as por meio de seu diálogo.

- [**Entidades**](/docs/services/assistant?topic=assistant-entities); Uma *entidade* representa um termo ou objeto que é relevante para suas intenções e que fornece um contexto específico para uma intenção. Por exemplo, uma entidade pode representar uma cidade na qual o usuário deseja localizar um local de negócios ou a quantia de um pagamento de fatura. O nome de uma entidade é sempre prefixado com o caractere `@`.

  É possível treinar a qualificação para reconhecer suas entidades, fornecendo valores e sinônimos de termo de entidade, padrões de entidade, ou identificando o contexto no qual uma entidade é geralmente usada em uma sentença. Para refinar seu diálogo, volte e inclua nós que verificam menções de entidade na entrada do usuário, além de intenções.

![Diagrama de uma implementação mais complexa que usa intenção, entidade e diálogo.](images/complex-impl.png)

À medida que você inclui informações, a qualificação usa esses dados exclusivos para construir um modelo de aprendizado de máquina que possa reconhecer essas entradas do usuário e semelhantes. Cada vez que você inclui ou muda os dados de treinamento, o processo de treinamento é acionado para assegurar que o modelo subjacente permaneça atualizado conforme suas necessidades do cliente e os tópicos que eles desejam discutir mudem.

Para ajudar a criar uma qualificação de diálogo, consulte [Criando uma qualificação de diálogo](/docs/services/assistant?topic=assistant-skill-dialog-add).

### Procurar qualificação ![Somente plano Plus ou Premium](images/plus.png)
{: #skills-search-skill}

Quando o Watson Assistant não tem uma solução explícita para um problema, ele roteia a pergunta do usuário para uma qualificação de procura para localizar uma resposta em suas origens diferentes de conteúdo de autoatendimento. A qualificação de procura interage com o serviço {{site.data.keyword.discoveryfull}} para extrair essas informações de uma coleta de dados configurada.

Se já usar o serviço {{site.data.keyword.discoveryshort}}, será possível explorar suas coleções de dados existentes em busca de materiais de origem que possam ser compartilhados com os clientes para lidar com as perguntas deles.

No entanto, não é necessário ter uma instância de serviço do {{site.data.keyword.discoveryshort}}. Se optar por criar uma qualificação de procura, uma instância grátis do {{site.data.keyword.discoveryshort}} será provisionada para você. Em seguida, será possível criar uma coleção de uma origem de dados e configurar sua qualificação de procura para realizar a procura nessa coleção em busca de respostas para consultas do cliente.

O diagrama a seguir ilustra como a entrada do usuário é processada quando uma qualificação de diálogo e uma qualificação de procura são incluídas em um assistente. Quaisquer perguntas que o diálogo não seja projetado para responder são enviadas para a qualificação de procura, que localiza uma resposta relevante em uma coleta de dados do {{site.data.keyword.discoveryshort}}.

![Diagrama de como uma pergunta é roteada para a qualificação de procura.](images/search-skill-diagram.png)

Para obter ajuda ao criar uma qualificação de procura, consulte [Criando uma qualificação de procura](/docs/services/assistant?topic=assistant-skill-search-add).
