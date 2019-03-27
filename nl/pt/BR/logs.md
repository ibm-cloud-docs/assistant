---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-28"

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

# Saiba de conversas
{: #logs}

Para abrir uma lista de mensagens entre usuários e o assistente que usa essa qualificação de diálogo, selecione **Conversas do usuário** na barra de navegação.
{: shortdesc}

Quando você abre a página **Conversas do usuário**, a visualização padrão lista resultados para o último dia, com os resultados mais recentes primeiro. A intenção principal (#intent) e quaisquer valores de entidade (@entity) reconhecidos usados em uma mensagem, além do texto da mensagem, estão disponíveis. Para as intenções que não são reconhecidas, o valor mostrado é *Irrelevante*. Se uma entidade não é reconhecida ou não foi fornecida, o valor mostrado é *Nenhuma entidade localizada*. ![Página padrão de logs](images/logs_page1.png)

É importante observar que a página **Conversas do usuário** exibe o número total de *mensagens* entre usuários e seu aplicativo. Uma mensagem é uma única elocução que o usuário envia para o aplicativo. Cada conversa pode ser composta por múltiplas mensagens. Assim, o número de resultados nesta página **Conversas do usuário** é diferente do número de conversas mostrado na página [Visão Geral](/docs/services/assistant?topic=assistant-logs-overview).

## Limites de log
{: #logs-limits}

O comprimento de tempo durante o qual as mensagens são retidas depende de seu plano de serviço do {{site.data.keyword.conversationshort}}:

  Plano de Serviço                         | Retenção de mensagem do bate-papo
  ------------------------------------ | ------------------------------------
  Premium                              | Últimos 90 dias
  Mais                                 | Últimos 30 dias
  Padrão                             | Últimos 30 dias
  Lite                                 | Últimos 7 dias

## Filtrando mensagens
{: #logs-filter-messages}

É possível filtrar mensagens por *Procurar instruções do usuário*, *Intenções*, *Entidades* e *Últimos* n *dias*:

*Procurar instruções do usuário* - digite uma palavra na barra de procura. Isso procura as entradas dos usuários, mas não as respostas de seu aplicativo.

*Intenções* - selecione o menu suspenso e digite uma intenção no campo de entrada ou escolha na lista preenchida. É possível selecionar mais de uma intenção, que filtra os resultados usando qualquer uma das intenções selecionadas, incluindo *Irrelevante*.

![Menu suspenso intenções](images/intents_filter.png)

*Entidades* - selecione o menu suspenso e digite um nome de entidade no campo de entrada ou escolha na lista preenchida. É possível selecionar mais de uma entidade, que filtra os resultados por qualquer uma das entidades selecionadas. Se você filtrar por intenção *e* entidade, seus resultados incluirão as mensagens que possuem ambos os valores. Também é possível filtrar por resultados com *Nenhuma entidade encontrada*.

![Menu suspenso entidades](images/entities_filter.png)

As mensagens podem levar algum tempo para serem atualizadas. Permita pelo menos 30 minutos após a interação de um usuário com seu aplicativo antes de tentar filtrar por esse conteúdo.

## Visualizando uma mensagem individual
{: #logs-see-message}

É possível expandir cada entrada de mensagem para ver o que o usuário disse na conversa toda e como seu aplicativo respondeu. Para fazer isso, selecione **Abrir conversa**. Você é levado automaticamente para a mensagem selecionada dentro dessa conversa.

O horário mostrado na parte superior de cada conversa é localizado para refletir o fuso horário de seu navegador. Isso poderá diferir do registro de data e hora mostrado se você revisar o mesmo log de conversa por meio de uma chamada de API; as chamadas de log de API são sempre mostradas em UTC.

![Painel abrir conversa](images/open_convo.png)

Em seguida, é possível escolher mostrar as classificações para a mensagem selecionada.

![Painel abrir conversa com classificações](images/open_convo_classes.png)

## Melhorar entre os assistentes
{: #logs-deploy-id}

A criação de uma qualificação de diálogo é um processo iterativo. Ao desenvolver sua qualificação, use a área de janela *Experimente* para verificar se o serviço reconhece as intenções e entidades corretas em entradas de teste e para fazer correções, conforme necessário.

Na página Conversas do usuário, é possível analisar interações reais entre o assistente que você usou para implementar a qualificação e seus usuários. Com base nessas interações, é possível fazer correções para melhorar a precisão com a qual as intenções e entidades são reconhecidas por sua qualificação de diálogo. É difícil saber exatamente *como* seus usuários farão perguntas ou quais mensagens aleatórias eles poderão enviar, portanto, é importante analisar frequentemente conversas reais para melhorar suas qualificações de diálogo.

Para uma instância do {{site.data.keyword.conversationshort}} que inclui múltiplos assistentes, às vezes pode ser útil usar os dados da mensagem da qualificação de diálogo de um assistente para melhorar a qualificação de diálogo usada por outro assistente dentro dessa mesma instância.

![Somente plano Premium](images/premium0.png) Se você for um usuário Premium do {{site.data.keyword.conversationshort}}, suas instâncias poderão ser configuradas opcionalmente para permitir acesso a dados do log de assistentes em suas diferentes instâncias premium.

Como um exemplo, suponha que você tenha uma instância do {{site.data.keyword.conversationshort}} nomeada *HelpDesk*. É possível ter um assistente de Produção e um assistente de Desenvolvimento em sua instância do HelpDesk. Ao trabalhar na qualificação de diálogo para o assistente de Desenvolvimento, é possível usar logs das mensagens do assistente de Produção para melhorar a qualificação de diálogo do assistente de Desenvolvimento.

Qualquer edição feita, então, dentro da qualificação de diálogo para o assistente de Desenvolvimento afetará somente a qualificação de diálogo do assistente de Desenvolvimento, mesmo que você esteja usando dados de mensagens enviadas para o assistente de Produção.

Da mesma forma, se você criar múltiplas versões de uma qualificação, talvez deseje usar dados da mensagem de uma versão para melhorar os dados de treinamento de outra versão.

### Escolhendo uma origem de dados
{: #logs-pick-data-source}

O termo *origem de dados* refere-se aos logs compilados de conversas entre clientes e o aplicativo assistente ou customizado pelo qual uma qualificação de diálogo foi implementada.

Quando você abre a guia *Analítica*, as métricas que foram geradas por interações com o usuário são mostradas com a qualificação de diálogo atual. Nenhuma métrica será mostrada se a qualificação atual não tiver sido implementada e usada por clientes.

Para preencher as métricas com dados da mensagem de uma qualificação de diálogo ou versão de qualificação que foi incluída em um assistente ou aplicativo customizado diferente, um que tenha interagido com os clientes, conclua estas etapas:

1.  Clique no campo **Origem de dados** para ver uma lista de assistentes com dados do log que você pode desejar usar.

    A lista inclui assistentes que foram implementados e para os quais você tem acesso. Consulte [*Mostrar IDs de implementação* explicados](#logs-deployment-id-explained) para obter mais informações sobre essa opção.

1.  Escolha uma origem de dados.

As informações estatísticas para a origem de dados selecionada são exibidas.

Observe que a lista não inclui versões de qualificação. Para obter dados que estão associados a uma versão de qualificação específica, deve-se saber o prazo durante o qual uma versão de qualificação específica foi usada por um assistente implementado. É possível selecionar o assistente como a origem de dados e, em seguida, filtrar os dados de métricas pelas datas apropriadas.

### *Mostrar IDs de implementação* explicado
{: #logs-deployment-id-explained}

Os aplicativos que usam a versão V1 da API devem especificar um ID de implementação em cada mensagem enviada usando a API `/message`. Esse ID identifica o app implementado do qual a chamada foi feita. A página Analítica pode usar esse ID de implementação para recuperar e exibir logs que estão associados a um aplicativo em tempo real específico.

Para assistentes ou apps customizados que usam a versão V2 da API, o serviço inclui automaticamente um ID do sistema e um ID de qualificação com cada chamada /message, para que seja possível escolher uma origem de dados pelo nome do assistente em vez de usar um ID de implementação.

Para incluir o ID de implementação, os usuários da API V1 incluem a propriedade de implementação dentro dos metadados do [contexto ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}, como neste exemplo:

```json
"context" : {
  "metadata" : {
       "deployment": "HelpDesk-Production"
  }
}
```
{: codeblock}

## Fazendo melhorias de dados de treinamento
{: #logs-fix-data}

Use insights de conversas do usuário real para corrigir o modelo associado à sua qualificação de diálogo.

Se você usar dados de outra origem de dados, quaisquer melhorias feitas no modelo serão aplicadas somente à qualificação de diálogo atual. O campo **Origem de dados** mostra a origem das mensagens que você está usando para melhorar essa qualificação de diálogo e a parte superior da página mostra a qualificação de diálogo à qual você está aplicando mudanças.

### Corrigindo uma intenção
{: #logs-correct-intent}

1.  Para corrigir uma intenção, selecione o ícone editar ![Editar](images/edit_icon.png) ao lado da #intent escolhida.
1.  Na lista fornecida, selecione a intenção correta para esta entrada.
    - Comece digitando no campo de entrada e a lista de intenções é filtrada.
    - Também é possível escolher **Marcar como irrelevante** neste menu. (Para obter mais informações, veja [Marcar como irrelevante](/docs/services/assistant?topic=assistant-intents#intents-mark-irrelevant).) Ou, é possível escolher **Não treinar na intenção**, o que não salva essa mensagem como um exemplo para treinamento.

    ![Selecionar intenção](images/select_intent.png)
1.  Selecione **Salvar**.

    ![Salvar intenção](images/save_intent.png)

    O serviço {{site.data.keyword.conversationshort}} suporta a inclusão de entrada do usuário como um exemplo para uma intenção *no estado em que se encontra*. Se você está usando referências @entity como exemplos em seus dados de treinamento de intenção e uma mensagem do usuário a ser salva contém um valor de entidade ou sinônimo de seus dados de treinamento, deve-se editar a mensagem posteriormente. Depois de salvá-la, edite a mensagem na página Intenções para substituir a entidade que ela referencia. Para obter mais informações, veja [Referenciando diretamente uma @Entity como um exemplo de intenção](/docs/services/assistant?topic=assistant-intents#intents-entity-as-example).
    {: tip}

### Incluindo um valor de entidade ou sinônimo
{: #logs-add-entity}

1.  Para incluir um valor de entidade ou sinônimo, selecione o ícone editar ![Editar](images/edit_icon.png) ao lado da @entity escolhida.
1.  Selecione **Incluir entidade**.

    ![Incluir entidade](images/add_entity.png)
1.  Agora, selecione uma palavra ou frase na entrada do usuário sublinhada.

    ![Selecionar entidade](images/select_entity.png)
1.  Escolha uma entidade à qual a frase destacada será incluída como um valor.
    - Comece digitando no campo de entrada e a lista de entidades e valores é filtrada.
    - Para incluir a frase destacada como um sinônimo para um valor existente, escolha o `@entity:value` na lista suspensa.

    ![Incluir palavra de entidade](images/add_entity_word.png)
1.  Selecione **Salvar**.

    ![Salvar entidade](images/add_entity_save.png)
