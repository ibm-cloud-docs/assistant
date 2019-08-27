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

# Integrando com o Facebook Messenger
{: #deploy-facebook}

O Facebook Messenger é um aplicativo de sistema de mensagens móvel que ajuda empresas e clientes a se comunicarem diretamente entre si.
{: shortdesc}

Depois de configurar uma qualificação de diálogo e incluí-la em um assistente, é possível integrar o assistente ao Facebook Messenger.

Não há atualmente nenhum mecanismo para identificar usuários que interagem com o assistente por meio do Facebook Messenger, o que significa que não há nenhuma maneira de identificar ou excluir dados associados a um usuário específico. Não use esse método de integração para implementações que devem ser compatíveis com GDPR. Consulte  [ Segurança de informações ](/docs/services/assistant?topic=assistant-information-security)  para obter mais detalhes.
{: important}

1.  Na guia Assistentes, clique para abrir o quadro do assistente que você deseja implementar.

1.  Na seção Integrações, clique em **Incluir integração**.

1.  Clique em **Facebook Messenger**.

1.  Siga as instruções fornecidas na tela para concluir o processo de integração.

Se quiser acompanhar enquanto outra pessoa percorre as etapas de implementação, assista a este vídeo.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Passagem das etapas de implementação do Facebook" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/8o-FFU5sYNM?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Duração: 8 minutos

## Considerações sobre Diálogo
{: #deploy-facebook-dialog}

As respostas ricas que você inclui em um diálogo são exibidas em um app do Facebook conforme o esperado, com as exceções a seguir:

- **Conectar-se ao agente humano**: esse tipo de resposta é ignorado.

- **Imagem**: esse tipo de resposta integra uma imagem à resposta. Um título e uma descrição não são exibidos antes da imagem, independentemente de você especificá-los ou não.

- **Opção**: esse tipo de resposta mostra uma lista de opções das quais o usuário pode escolher.

  - Um título é **necessário** e é exibido antes da lista de opções.
  - Uma descrição não é exibida, independentemente de você especificá-la ou não.
  - Depois que um usuário clica em um dos botões, as opções de botão desaparecem e são substituídas pela entrada do usuário que é gerada pela opção do usuário. Se o assistente ou o usuário inserir uma nova entrada, a entrada gerada pelo botão desaparecerá. Portanto, se você incluir múltiplos tipos de resposta em uma única resposta, posicione o tipo de resposta da opção por último. Caso contrário, o conteúdo de respostas subsequentes, como texto de um tipo de resposta de texto, substituirá o texto gerado pelo botão.

- **Pausar**: esse tipo de resposta pausa a atividade do assistente no Messenger. No entanto, a atividade não continua após a pausa, a menos que outro tipo de resposta seja acionado depois dela. Sempre que você incluir esse tipo de resposta, inclua outro tipo de resposta diferente, como uma resposta de texto, e posicione-o depois desse.

Consulte [Respostas avançadas](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia) para obter mais informações sobre os tipos de resposta.

## Conversando com o assistente
{: #deploy-facebook-try}

Para iniciar um bate-papo com o assistente, conclua as etapas a seguir:

1.  Abra o Facebook Messenger.
1.  Digite o nome da página que você criou anteriormente.
1.  Depois que a página aparecer, clique nela e, em seguida, inicie o bate-papo com o assistente.

O nó Bem-vindo de seu diálogo não é processado pela integração do Facebook Messenger. A mensagem de boas-vindas não é exibida no bate-papo do Facebook, somente na área de janela "Experimentar" ou na página da web de integração Link de visualização. Ele não é acionado daqui porque os nós com a condição especial `welcome` são ignorados em fluxos de diálogo iniciados por usuários. O Facebook Messenger espera que o usuário inicie a conversa. Se for necessário configurar valores padrão para variáveis de contexto no início de sua conversa, não os configure no nó de boas-vindas. Consulte [Iniciando o diálogo](/docs/services/assistant?topic=assistant-dialog-start) para obter mais informações.
{: note}

O fluxo de diálogo da sessão atual é reiniciado após 60 minutos de inatividade (5 minutos para os planos Lite e Standard). Isso significa que, se um usuário parar de interagir com o assistente, após 60 (ou 5) minutos, todos os valores da variável de contexto configurados durante a conversa anterior serão configurados como nulos ou de volta para seus valores padrão.
