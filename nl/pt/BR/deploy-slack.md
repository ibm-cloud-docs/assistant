---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-29"

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

# Integrando com o Slack
{: #deploy-slack}

O Slack é um aplicativo de sistema de mensagens baseado em nuvem que ajuda as pessoas a colaborarem entre si.
{: shortdesc}

Depois de configurar uma qualificação de diálogo e incluí-la em um assistente, será possível integrar o assistente ao Slack.

Quando integrado, dependendo dos eventos configurados para o suporte do assistente, ele poderá responder a perguntas feitas em mensagens diretas ou em canais nos quais é mencionado diretamente. 

## Incluindo a integração do Slack
{: #deploy-slack-task}

1.  Na guia Assistentes, clique para abrir o quadro do assistente que você deseja implementar.

1.  Na seção Integrações, clique em **Incluir integração**.

1.  Clique em **Slack**.

1.  Siga as instruções fornecidas na tela para concluir o processo de integração.

    Escolha um ou mais dos eventos de assinatura a seguir para suportar:

    - `message.im`: o assistente responde quando alguém inicia uma mensagem direta com ele.
    - `app_mentions`: o assistente responde quando alguém o menciona pelo nome em um canal.

Se quiser acompanhar enquanto outra pessoa percorre as etapas de implementação, assista a este vídeo.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Passagem das etapas de implementação do Slack" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/RBGBPJ8h4HQ?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Duração: 9,5 minutos

## Considerações sobre Diálogo
{: #deploy-slack-dialog}

As respostas ricas que você inclui em um diálogo são exibidas em um canal Slack conforme esperado, com as exceções a seguir:

- **Conectar-se ao agente humano**: esse tipo de resposta é ignorado.

- ** Imagem **: um título de imagem é necessário. Se você não fornecer um título, a imagem não será exibida.

- **Opção**: esse tipo de resposta mostra uma lista de opções das quais o usuário pode escolher.

  - Um título é **necessário** e é exibido antes da lista de opções.
  - Uma descrição não é exibida, independentemente de você especificar uma ou não.
  - Depois que um usuário clica em um dos botões, as opções do botão desaparecem e são substituídas pela entrada do usuário gerada pela opção do usuário. Se você incluir múltiplos tipos de resposta em uma única resposta, posicione o tipo de resposta da opção por último. Caso contrário, a saída conterá uma combinação de respostas e entradas do usuário que podem confundir o usuário.

- **Pausar**: esse tipo de resposta pausa a atividade do assistente no canal Slack. No entanto, nenhum indicador visível será mostrado para indicar que o assistente está pausado, independentemente de você escolher mostrar a digitação ou não.

Consulte [Respostas avançadas](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia) para obter mais informações sobre os tipos de resposta.

## Conversando com o assistente
{: #deploy-slack-try}

Para iniciar um bate-papo com o assistente, conclua as etapas a seguir:

1.  Abra o Slack e acesse a área de trabalho associada ao seu app.
1.  Clique no aplicativo criado na seção Apps.
1.  Chat com o assistente.

O nó Bem-vindo de seu diálogo não é processado pela integração do Slack. A mensagem de boas-vindas não é exibida no canal do Slack da mesma forma como na área de janela "Experimentar" ou na página da web de integração do Link de visualização. Ele não é acionado daqui porque os nós com a condição especial `welcome` são ignorados em fluxos de diálogo iniciados por usuários. O Slack espera que o usuário inicie a conversa. Se for necessário configurar valores padrão para variáveis de contexto no início de sua conversa, não os configure no nó de boas-vindas. Consulte [Iniciando o diálogo](/docs/services/assistant?topic=assistant-dialog-start) para obter mais informações.
{: note}

O fluxo de diálogo da sessão atual é reiniciado após 60 minutos de inatividade (5 minutos para os planos Lite e Standard). Isso significa que, se um usuário parar de interagir com o assistente, após 60 (ou 5) minutos, todos os valores da variável de contexto configurados durante a conversa anterior serão configurados como nulos ou de volta para seus valores padrão.
