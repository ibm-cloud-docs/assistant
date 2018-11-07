---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-09"

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

# Implementando em um canal com o conector do {{site.data.keyword.conversationshort}}

Depois de ter desenvolvido sua área de trabalho, é possível usar o conector do {{site.data.keyword.conversationshort}} para conectá-la rapidamente a um canal de comunicação, como Slack ou Facebook Messenger. O conector do {{site.data.keyword.conversationshort}} é um conjunto de componentes do {{site.data.keyword.openwhisk}} que mediam a comunicação entre a área de trabalho do {{site.data.keyword.conversationshort}} e um aplicativo Slack ou Facebook que você possui, armazenando dados de sessão em um banco de dados Cloudant.

Conectando sua área de trabalho a um app de canal, é possível torná-la disponível como um robô de bate-papo com o qual os usuários do Slack ou Facebook Messenger podem interagir. As respostas de seu diálogo podem incluir multimídia e respostas interativas, como imagens e botões clicáveis. (Para obter mais informações sobre como definir uma resposta de multimídia, veja [Respostas de multimídia](dialog-multimedia.html).)

![{{site.data.keyword.openwhisk_short}} diagrama de visão geral de implementação](images/deploytochannel_diagram.png)

O conector do {{site.data.keyword.conversationshort}} tem algumas limitações:

- Deve-se criar um app Slack ou Facebook ou ter permissões administrativas para modificar o app que você deseja usar.
- Devido a restrições do {{site.data.keyword.openwhisk_short}}, esta ferramenta está disponível atualmente apenas para a Região Sul dos EUA {{site.data.keyword.Bluemix_notm}}.

## Implementando no Slack usando a ferramenta {{site.data.keyword.conversationshort}}

A ferramenta {{site.data.keyword.conversationshort}} fornece uma maneira rápida de implementar um robô usando o conector do {{site.data.keyword.conversationshort}}. A ferramenta conduz você pelo processo de reunião das informações necessárias para configurar seu app de canal e os componentes do conector e, em seguida, implementa automaticamente os componentes necessários no IBM Cloud.

**Nota:** a interface da ferramenta {{site.data.keyword.conversationshort}} suporta atualmente somente o Slack, mas é possível usar um processo automatizado do {{site.data.keyword.Bluemix_short}} para implementar no Facebook Messenger. Para obter mais informações sobre como implementar no Facebook Messenger, veja a documentação no [repositório GitHub ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/watson-developer-cloud/conversation-connector/blob/master/channels/facebook/README.md){: new_window} do conector do {{site.data.keyword.conversationshort}}.

Para implementar um robô usando a ferramenta de implementação automatizada:

1. Na ferramenta {{site.data.keyword.conversationshort}}, abra a área de trabalho que você deseja implementar.
1. Clique no ícone de menu no canto superior esquerdo e, em seguida, selecione **Implementar**.

   ![Opção de menu implementação rápida](images/deploy_menu_testdeploy.png)

1. Em **Implementar com o {{site.data.keyword.openwhisk_short}}**, clique em **Implementar no Slack**.
1. Na página Slack, clique em **Implementar no app Slack** e siga as instruções.

   ![Botão Implementar no app Slack](images/deploy_deploytoslack.png)

## Implementando manualmente

Em vez de usar a ferramenta automatizada para implementar sua área de trabalho como um robô, é possível configurar e implementar manualmente os componentes necessários no IBM Cloud. Existem várias situações nas quais você pode desejar fazer isso:

- **Reimplementação parcial**. Você pode desejar reimplementar alguns componentes de uma implementação existente para mudar sua configuração, corrigir os problemas ou aplicar uma correção de uma versão mais recente.
- **Estendendo seu robô com novos recursos**. É possível modificar os componentes fornecidos para incluir novas funções, como pré-processar a entrada do usuário antes de enviá-la para a área de trabalho do {{site.data.keyword.conversationshort}}.
- **Implementando em um novo canal**. Se você deseja implementar um robô em um canal diferente do Slack ou Facebook Messenger, é possível seguir o padrão dos componentes existentes para desenvolver seus próprios componentes.

Os componentes do conector do {{site.data.keyword.conversationshort}} são hospedados em um repositório GitHub público, do qual é possível fazer download ou clonar. Para obter mais informações, veja a documentação fornecida no [repositório ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/watson-developer-cloud/conversation-connector){: new_window}.

## Conversando com o robô

Depois de concluir o processo de implementação, é possível usar o nome do usuário do robô para interagir com sua área de trabalho do {{site.data.keyword.conversationshort}}, assim como você faria com qualquer outro robô do Slack ou Facebook Messenger.

Tenha em mente que o conector do {{site.data.keyword.conversationshort}} preserva o estado separadamente para cada usuário que está interagindo com o robô. (No caso de Slack, um único usuário pode ter múltiplas conversas exclusivas com o robô, uma por meio de mensagens diretas e uma em cada canal.) Isso significa que quaisquer variáveis armazenadas no contexto de diálogo são mantidas indefinidamente, a menos que sejam limpas.

Se você precisa ser capaz de reconfigurar a conversa para um estado inicial conhecido, é possível fazer isso em seu diálogo. Certifique-se de que o diálogo possui um nó que é executado no final da conversa ou a qualquer outro momento necessário para recomeçar. Atualize o JSON para este nó para reconfigurar todas as variáveis de contexto para os valores iniciais apropriados. (Se necessário, é possível usar as ações **Ir para** ou uma intenção especial "terminar conversa" para executar esse nó.)

Por exemplo, se sua área de trabalho usa uma variável de contexto chamada `drink_order` para armazenar uma seleção de bebidas de um usuário, é possível utilizar o método `context.remove` para excluir esta variável quando a conversa termina:

```json
"context": {
   "reset_drink_order": "<?context.remove('drink_order')?>"
 }
```
{: codeblock}

Para obter informações adicionais sobre modificação de valores de variáveis de contexto, consulte [Atualizando um valor de variável de contexto](dialog-runtime.html#context-update).

Também é possível limpar o contexto ou fazer outras mudanças no comportamento do robô, editando as ações implementadas do {{site.data.keyword.openwhisk_short}}. Para obter mais informações, veja a documentação fornecida no [repositório GitHub ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/watson-developer-cloud/conversation-connector){: new_window}.
