---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-28"

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

# Descreção de diálogo
{: #dialog-depiction}

![Uma árvore de diálogo de amostra com conteúdo de exemplo](images/dialog-depiction-full.png)

Esse diagrama mostra uma representação de uma árvore de diálogo construída com o editor de diálogo da interface gráfica com o usuário. Ele contém dois nós de diálogo raiz. Uma árvore de diálogo típica provavelmente teria muitos outros nós, mas essa representação fornece um vislumbre de como um subconjunto de nós pode se parecer.

- O primeiro nó raiz condiciona um valor de intenção. Ele tem dois nós-filhos, sendo que cada um condiciona um valor de entidade.  O segundo nó-filho define duas respostas. A primeira resposta será retornada para o usuário se o valor da variável de contexto corresponder ao valor especificado na condição. Caso contrário, a segunda resposta será retornada.

  Esse tipo padrão de nó é útil para capturar perguntas sobre um determinado tópico e, em seguida, na resposta raiz, fazer uma pergunta de acompanhamento que é direcionada pelos nós-filhos. Por exemplo, ele pode reconhecer uma pergunta do usuário sobre descontos e fazer uma pergunta de acompanhamento sobre se o usuário é um membro de quaisquer associações com as quais a empresa tem acordos de desconto especial. E os nós-filhos fornecem respostas diferentes com base na resposta do usuário para a pergunta sobre participação de associação.

- O segundo nó raiz é um nó com intervalos. Ele também condiciona um valor de intenção. Ele define um conjunto de intervalos, um para cada parte de informações que você deseja coletar do usuário. Cada intervalo solicita uma pergunta para extrair a resposta do usuário. Ele procura um valor de entidade específico na resposta do usuário para o prompt, que ele então salva em uma variável de contexto de intervalo.

  Esse tipo de nó é útil para coletar detalhes que podem ser necessários para executar uma transação em nome do usuário. Por exemplo, se a intenção do usuário é reservar um voo, os intervalos podem coletar as informações de local de origem e de destino, as datas de viagem e assim por diante.
