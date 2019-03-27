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

# Assistentes
{: #assistants}

Um assistente é um robô cognitivo que pode ser customizado para suas necessidades de negócios e implementado em múltiplos canais para trazer ajuda para seus clientes onde e quando eles precisam.
{: shortdesc}

![Qualificações](images/skill-icon.png) Você customiza o assistente incluindo-o nas qualificações necessárias para satisfazer os objetivos de seus clientes.

Inclua uma qualificação de diálogo que possa entender e tratar perguntas ou solicitações para as quais seus clientes normalmente precisam de ajuda. Você fornece informações sobre os assuntos ou tarefas que seus usuários perguntam e a maneira como eles perguntam e o serviço constrói dinamicamente um modelo de aprendizado de máquina que é customizado para entender as mesmas solicitações do usuário e semelhantes.

| Árvore de diálogo | Interface Gráfica com o Usuário |
|-------------|-------------------------:|
| É possível usar ferramentas gráficas para criar um diálogo para seu assistente ler ao interagir com seus usuários, um diálogo que simula uma conversa real. O diálogo se concentra nos objetivos comuns do cliente que você o ensina a reconhecer e fornece respostas úteis. | ![Uma árvore de diálogo de amostra com conteúdo de exemplo](images/dialog-depiction.png) |

A qualificação de diálogo em si é definida no texto, mas é possível integrá-la aos serviços Watson Speech to Text ae ao Watson Text to Speech que permitem que os usuários interajam com seu assistente verbalmente.

![Dados de treinamento prontos para utilização](images/oob.png) se você desejar iniciar rapidamente, inclua dados de treinamento pré-construídos em sua qualificação de diálogo para que seu assistente possa começar a ajudar seus clientes com os fundamentos básicos.

![IBM Cloud](images/cloud.png) O assistente é um robô totalmente hospedado que é gerenciado pelo {{site.data.keyword.cloud_notm}}, o que significa que você não precisa se preocupar em configurar ou manter a infraestrutura para suportá-lo.

| Integrações       | Canais  |
|--------------------|:----------|
| É possível implementar o assistente por meio de múltiplas interfaces, incluindo canais do sistema de mensagens existentes, como Slack e Facebook Messenger, em apenas algumas etapas. Ou, se você desejar projetar um aplicativo customizado que o incorpore, será possível fazer chamadas diretas para as APIs subjacentes para fazer isso. | ![Métodos de integração incluindo a integração do Slack, do Facebook Messenger, de um aplicativo da web ou do agente humano](images/integrations.png) |

Consulte [Criando um assistente](/docs/services/assistant?topic=assistant-assistant-add) para iniciar.
