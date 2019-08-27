---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-26"

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

![Qualificações](images/skill-icon.png)  Um assistente roteia as consultas do cliente para uma qualificação, que, em seguida, fornece a resposta apropriada. As qualificações de diálogo retornam respostas criadas por você para perguntas comuns, enquanto as qualificações de procura procuram e retornam passagens do conteúdo de autoatendimento existente para responder a consultas mais complexas.

## Qualificação de diálogo
{: #assistants-dialog-skill}

Uma qualificação de diálogo pode entender e lidar com perguntas ou solicitações com as quais seus clientes geralmente precisam de ajuda. Você fornece informações dos assuntos ou tarefas sobre os quais seus usuários perguntam e de como eles fazem essas perguntas e o produto constrói dinamicamente um modelo de aprendizado de máquina customizado para entender as mesmas solicitações e solicitações semelhantes do usuário.

| Árvore de diálogo | Interface Gráfica com o Usuário |
|-------------|-------------------------:|
| É possível usar o editor de diálogo gráfico para criar um script variado que pode ser lido por seu assistente ao interagir com seus clientes. O resultado é um diálogo que simula uma conversa real. O diálogo se concentra nos objetivos comuns do cliente que você o ensina a reconhecer e fornece respostas úteis. | ![Uma árvore de diálogo de amostra com conteúdo de exemplo](images/dialog-depiction.png) |

A qualificação de diálogo em si é definida no texto, mas é possível integrá-la aos serviços Watson Speech to Text ae ao Watson Text to Speech que permitem que os usuários interajam com seu assistente verbalmente.

![Dados de treinamento prontos para utilização](images/oob.png) se você desejar iniciar rapidamente, inclua dados de treinamento pré-construídos em sua qualificação de diálogo para que seu assistente possa começar a ajudar seus clientes com os fundamentos básicos.

## Procurar qualificação ![Somente plano Plus ou Premium](images/plus.png)
{: #assistants-search-skill}

A qualificação de procura está disponível apenas para usuários do Plus ou do Premium.
{: note}

Uma qualificação de procura utiliza informações de bases de conhecimento corporativas existentes ou outras coleções de conteúdo criadas por especialistas no assunto para lidar com consultas dos clientes não antecipadas ou com mais nuances.

![IBM Cloud](images/cloud.png) O assistente é um robô totalmente hospedado que é gerenciado pelo {{site.data.keyword.Bluemix_notm}}, o que significa que você não precisa se preocupar em configurar ou manter a infraestrutura para suportá-lo.

| Integrações       | Canais  |
|--------------------|:----------|
| É possível implementar o assistente por meio de múltiplas interfaces, incluindo canais do sistema de mensagens existentes, como Slack e Facebook Messenger, em apenas algumas etapas. Ou, se você desejar projetar um aplicativo customizado que o incorpore, será possível fazer chamadas diretas para as APIs subjacentes para fazer isso. | ![Métodos de integração incluindo a integração do Slack, do Facebook Messenger, de um aplicativo da web ou do agente humano](images/integrations.png) |

Consulte [Criando assistentes](/docs/services/assistant?topic=assistant-assistant-add) para começar.
