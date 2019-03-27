---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-11"

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

# Iniciando o diálogo
{: #dialog-start}

Não é possível usar o nó de boas-vindas integrado para iniciar um diálogo da mesma maneira para todas as integrações. Use essa solução alternativa no lugar.
{: shortdesc}

A resposta que você define para o nó de boas-vindas no diálogo é exibida para iniciar uma conversa na área de janela "Experimente" dentro da ferramenta e por meio do widget de bate-papo na integração do Link de visualização. No entanto, ela não é exibida em muitas das outras integrações de canal porque os nós com a condição especial `welcome` são ignorados em fluxos de diálogo que são iniciados pelos usuários. E os assistentes implementados geralmente esperam que os usuários iniciem conversas com eles, não o contrário.

Diferentemente da condição especial `welcome`, a condição especial `conversation_start` é sempre acionada no início de um diálogo. É possível usar uma combinação de nós com estas duas condições especiais (`welcome` e `conversation_start`) para gerenciar o início de seu diálogo de uma maneira consistente.

Conclua as etapas a seguir para gerenciar o início do diálogo:

1.  Inclua um nó de diálogo acima do nó de Boas-vindas que é incluído automaticamente na parte superior da árvore de diálogos quando você cria o diálogo.

1.  Configure a condição do nó para esse nó recém-incluído como `conversation_start`, que é uma condição especial, conforme descrito anteriormente.

1.  Defina quaisquer variáveis de contexto que você deseja configurar com valores padrão para o diálogo no nó `conversation_start`.

1.  Não defina uma resposta de texto para esse nó.

1.  Configure esse nó para ir para o nó `Welcome` diretamente abaixo dele na árvore de diálogo e escolha **Se o robô reconhecer (condição)**.

![Captura de tela da árvore de diálogo com um nó conversation_start indo para um nó de boas-vindas abaixo dele.](images/dialog-start.png)

Esse design resulta em um diálogo que funciona como este:

- Qualquer que seja o tipo de integração, o nó `conversation_start` é processado, o que significa que quaisquer variáveis de contexto definidas nele serão inicializadas.
- Em integrações em que o assistente inicia o fluxo de diálogo, o nó `Welcome` é acionado e sua resposta de texto é exibida.
- Em integrações em que o usuário inicia o fluxo de diálogo, a primeira entrada do usuário é avaliada e, em seguida, processada pelo nó que pode fornecer a melhor resposta.
