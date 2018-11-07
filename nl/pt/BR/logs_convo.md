---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-13"

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

# Trabalhando com conversas
{: #logs_convo}

Para abrir uma lista de interações entre usuários e suas áreas de trabalho, selecione **Conversas do usuário** na barra de navegação. Se **Conversas do usuário** não estiver visível, use o menu ![Menu](images/Menu_16.png) para abrir a página.
{: shortdesc}

Quando você abre a página **Conversas do usuário**, a visualização padrão lista resultados para o último dia, com os resultados mais recentes primeiro. A intenção principal (#intent) e quaisquer valores de entidade (@entity) reconhecidos usados em uma mensagem, além do texto da mensagem, estão disponíveis. Para as intenções que não são reconhecidas, o valor mostrado é *Irrelevante*. Se uma entidade não é reconhecida ou não foi fornecida, o valor mostrado é *Nenhuma entidade localizada*. ![Página padrão de logs](images/logs_page1.png)

É importante observar que a página **Conversas do usuário** exibe o número total de *elocuções* entre os usuários e sua área de trabalho. Uma elocução é uma única mensagem que o usuário envia para a área de trabalho. Cada conversa pode ser composta por múltiplas elocuções. Assim, o número de resultados nesta página **Conversas do usuário** é diferente do número de conversas mostrado na página [Visão Geral](logs_oview.html).

## Limites de log
{: #log-limits}

O comprimento de tempo durante o qual as mensagens são retidas depende de seu plano de serviço do {{site.data.keyword.conversationshort}}:

  Plano de Serviço                         | Retenção de mensagem do bate-papo
  ------------------------------------ | ------------------------------------
  Premium                              | Últimos 90 dias
  Padrão                             | Últimos 30 dias
  Lite                                 | Últimos 7 dias

## Selecionando uma origem de dados
{: #select-source}

Por padrão, a página **Conversas do usuário** mostra os dados de elocução para a área de trabalho atual. No entanto, às vezes pode ser útil melhorar uma área de trabalho com elocuções que foram enviadas para outras áreas de trabalho dentro de sua instância. Por exemplo, você pode ter múltiplas versões de áreas de trabalho de produção e áreas de trabalho de desenvolvimento; é possível usar os mesmos dados de elocução para melhorar qualquer uma dessas áreas de trabalho.

Ao alternar para outra origem de dados, o serviço {{site.data.keyword.conversationshort}} verifica elocuções para um elemento chamado `Deployment ID`. Os IDs de implementação são identificadores exclusivos na API do serviço {{site.data.keyword.conversationshort}} que você inclui em suas chamadas API de mensagem. Para obter informações sobre como incluir IDs de implementação em chamadas por mensagem, veja [Melhorando entre áreas de trabalho](logs.html#deploy_id).

Para preencher a seção Melhorar usando elocuções com um ID de implementação especificado:

1.  Selecione **Origem de dados:**
    ![Link de origem de dados](images/data_source_1.png)
1.  Selecione uma implementação
    ![Link de origem de dados](images/data_source_2.png)
1.  Clique em **Dados da visualização**

A origem de dados selecionada agora é exibida.

**Nota:** embora a **Origem de dados:** agora mostre a origem das elocuções que você está usando para melhorar essa área de trabalho, a parte superior da página ainda mostra a área de trabalho à qual as mudanças estão sendo aplicadas.

Neste exemplo, a página Melhorar é preenchida com as elocuções que tiveram o ID de implementação `HelpDesk-Production` incluído em suas chamadas API de mensagem, mas se a elocução *entrada de teste* for incluída na intenção **#No** clicando em **Salvar**, *entrada de teste* será incluído como um exemplo de `#No` na área de trabalho `HelpDesk-Development`.
![Link de origem de dados](images/data_source_3.png)

## Filtrando elocuções

É possível filtrar elocuções por *Procurar instruções do usuário*, *Intenções*, *Entidades* e *Últimos* n *dias*:

*Procurar instruções do usuário* - digite uma palavra na barra de procura. Isso procura as entradas dos usuários, mas não as respostas de sua área de trabalho.

*Intenções* - selecione o menu suspenso e digite uma intenção no campo de entrada ou escolha na lista preenchida. É possível selecionar mais de uma intenção, que filtra os resultados usando qualquer uma das intenções selecionadas, incluindo *Irrelevante*.

![Menu suspenso intenções](images/intents_filter.png)

*Entidades* - selecione o menu suspenso e digite um nome de entidade no campo de entrada ou escolha na lista preenchida. É possível selecionar mais de uma entidade, que filtra os resultados por qualquer uma das entidades selecionadas. Se você filtrar por intenção *e* entidade, seus resultados incluirão as mensagens que possuem ambos os valores. Também é possível filtrar por resultados com *Nenhuma entidade encontrada*.

![Menu suspenso entidades](images/entities_filter.png)

Elocuções podem levar algum tempo para atualizar. Permita pelo menos 30 minutos após a interação de um usuário com sua área de trabalho antes de tentar filtrar esse conteúdo.

## Visualizando uma elocução individual
É possível expandir cada entrada de elocução para ver o que o usuário disse na conversa toda e como sua área de trabalho respondeu. Para fazer isso, selecione **Abrir conversa**. Você é levado automaticamente para a elocução selecionada nessa conversa.

![Painel abrir conversa](images/open_convo.png)

É possível então optar por mostrar a classificação(ões) para a elocução que você selecionou.

![Painel abrir conversa com classificações](images/open_convo_classes.png)

## Corrigindo uma intenção

1.  Para corrigir uma intenção, selecione o ícone editar ![Editar](images/edit_icon.png) ao lado da #intent escolhida.
1.  Na lista fornecida, selecione a intenção correta para esta entrada.
    - Comece digitando no campo de entrada e a lista de intenções é filtrada.
    - Também é possível escolher **Marcar como irrelevante** neste menu. (Para obter mais informações, veja [Marcar como irrelevante](intents.html#mark-irrelevant).) Ou é possível escolher **Não treinar com a intenção**, que não salva essa elocução como um exemplo para treinamento.

    ![Selecionar intenção](images/select_intent.png)
1.  Selecione **Salvar**.

    ![Salvar intenção](images/save_intent.png)

    **Nota**: o serviço {{site.data.keyword.conversationshort}} suporta a inclusão de entrada do usuário como um exemplo para uma intenção *no estado em que se encontra*. Se referências @entity estão sendo usadas como exemplos em seus dados de treinamento de intenção e uma elocução do usuário que você deseja salvar contém um valor de entidade ou sinônimo de seus dados de treinamento, deve-se editar a elocução mais tarde. Depois de salvá-la, edite a elocução na página Intenções para substituir a entidade que ela referencia. Para obter mais informações, veja [Referenciando diretamente uma @Entity como um exemplo de intenção](intents.html#entity-as-example).

## Incluindo um valor de entidade ou sinônimo

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
