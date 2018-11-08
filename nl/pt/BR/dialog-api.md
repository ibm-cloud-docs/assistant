---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-26"

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

# Modificando um diálogo utilizando a API

A API de REST do {{site.data.keyword.conversationshort}} suporta a modificação de seu diálogo de maneira programática, sem o uso da ferramenta {{site.data.keyword.conversationshort}}. É possível usar a API /dialog_nodes para criar, excluir ou modificar nós de diálogo.

Lembre-se de que o diálogo é uma árvore de nós interconectados e que deve estar em conformidade com certas regras para ser válido. Isso significa que qualquer mudança feita em um nó de diálogo pode ter efeitos em cascata em outros nós ou na estrutura do diálogo. Antes de usar a API /dialog_nodes para modificar o diálogo, certifique-se de entender como suas mudanças afetarão o resto do diálogo. É possível fazer uma cópia de backup do diálogo atual exportando a área de trabalho na qual ele reside. Veja [Exportando e copiando áreas de trabalho](configure-workspace.html#exporting-and-copying-workspaces) para obter detalhes.

Um diálogo válido sempre satisfaz os critérios a seguir:

- Cada nó do diálogo possui um ID exclusivo (a propriedade `dialog_node`).
- Um nó-filho está ciente de seu nó pai (a propriedade `parent`). No entanto, um nó pai não está ciente de seus filhos.
- Um nó está ciente de seu irmão anterior imediato, se houver (a propriedade `previous_sibling`). Isso significa que todos os irmãos que compartilham o mesmo pai formam uma lista vinculada, com cada nó apontando para o nó anterior.
- Somente um filho de um pai fornecido pode ser o primeiro irmão (significando que seu `previous_sibling` é nulo).
- Um nó não pode apontar para um irmão anterior que seja um filho de um pai diferente.
- Dois nós não podem apontar para o mesmo irmão anterior.
- Um nó pode especificar outro nó que deve ser executado em seguida (a propriedade `next_step`).
- Um nó não pode ser seu próprio pai ou seu próprio irmão.
- Um nó deve ter uma propriedade de tipo que contenha um dos valores a seguir. Se nenhuma propriedade de tipo for especificada, o tipo será `standard`.

  - `event_handler`: um manipulador que está definido para um nó de quadro ou um nó de intervalo individual.

    No conjunto de ferramentas, é possível definir um manipulador de nó de quadro clicando no link **Gerenciar manipuladores** de um nó com intervalos. (A interface com o usuário do conjunto de ferramentas não expõe o manipulador de eventos de nível de intervalo, mas é possível definir um por meio da API.)

  - `frame`: um nó com um ou mais nós-filhos do tipo `slot`. Quaisquer nós-filhos de intervalo necessários devem ser preenchidos antes que o serviço possa sair do nó de quadro.

    O tipo de nó de quadro é representado como um nó com intervalos no conjunto de ferramentas. O nó que contém os intervalos é representado como um nó de type=`frame`. Ele é o nó pai para cada intervalo, que é representado como um nó filho do tipo `slot`.

  - `response_condition`: uma resposta condicional.

    No conjunto de ferramentas, é possível incluir uma ou mais respostas condicionais em um nó. Cada resposta condicional que você define é representada no JSON subjacente como um nó individual de type=`response_condition`.

  - `slot`: um nó-filho de um nó do tipo `frame`.

    Esse tipo de nó é representado no conjunto de ferramentas como sendo um dos múltiplos intervalos incluídos em um único nó. Esse único nó é representado no JSON como um nó pai do tipo `frame`.

  - `standard`: um nó de diálogo típico. Este é o tipo padrão.

- Para os nós do tipo `slot` que têm o mesmo nó pai, a ordem do irmão (especificada pela propriedade `previous_sibling`) reflete a ordem na qual os intervalos serão processados.
- Um nó do tipo `slot` deve ter um nó pai do tipo `frame`.
- Um nó do tipo `frame` deve ter pelo menos um nó-filho do tipo `slot`.
- Um nó do tipo `response_condition` deve ter um nó pai do tipo `standard` ou `frame`.
- Os nós do tipo `response_condition` e `event_handler` não podem ter filhos.
- Um nó do tipo `event_handler` também deve ter uma propriedade `event_name` que contém um dos valores a seguir para identificar o tipo de evento do nó:

  - `filled`: define o que fazer se o usuário fornece um valor que atenda a condição especificada no campo *Verificar* de um intervalo e o intervalo é preenchido. Um manipulador com esse nome estará presente somente se uma condição de Localizado estiver definida para o intervalo.
  - `focus`: define a pergunta a ser mostrada para solicitar ao usuário para fornecer as informações necessárias pelo intervalo. Um manipulador com esse nome estará presente somente se o intervalo for necessário.
  - `generic`: define uma condição para observar que pode direcionar questões não relacionadas que os usuários podem perguntar ao preencher um intervalo ou nó com intervalos.
  - `input`: atualiza o contexto da mensagem para incluir uma variável de contexto com o valor que é coletado do usuário para preencher o intervalo. Um manipulador com esse nome deve estar presente para cada intervalo no nó de quadro.
  - `nomatch`: define o que fazer se a resposta do usuário para o prompt de intervalo não contém um valor válido. Um manipulador com esse nome está presente somente se uma condição de Não localizado está definida para o intervalo.

  O diagrama a seguir ilustra onde na interface com o usuário do conjunto de ferramentas você define o código que é acionado para cada evento nomeado.

  ![Local da UI em que o código acionado por manipuladores de eventos nomeados é criado](images/api-event-handlers.png)

- Um nó do tipo `event_handler` com um event_name de `generic` pode ter um pai do tipo `slot` ou `frame`.
- Um nó do tipo `event_handler` com um event_name de `focus`, `input`, `filled` ou `nomatch` deve ter um pai do tipo `slot`.
- Se mais de um event_handler com o mesmo event_name estiver associado ao mesmo nó pai, a ordem dos irmãos refletirá a ordem na qual os manipuladores de eventos serão executados.
- Para os nós `event_handler` com o mesmo nó pai de intervalo, a ordem de execução é a mesma independentemente do posicionamento das definições de nó. Os eventos são acionados nesta ordem por event_name:

  1. focus
  1. input
  1. filled
  1. generic*
  1. nomatch

  *Se um `event_handler` com o event_name `generic` for definido para esse intervalo ou para o quadro pai, ele será executado entre os nós event_handler filled e nomatch.

Os exemplos a seguir mostram como várias modificações podem causar mudanças em cascata.

## Criando um nó
{: #create-node}

Considere a árvore de diálogo simples a seguir:

![Diálogo de exemplo](images/dialog_api_1.png)

Podemos criar um novo nó ao fazer uma solicitação POST para /dialog_nodes com o corpo a seguir:

```json
{
  "Dialog_node": "node_8"
}
```

O diálogo agora é semelhante a este:

![Diálogo de exemplo 2](images/dialog_api_2.png)

Como o **node_8** foi criado sem especificar um valor para `parent` ou `previous_sibling`, ele agora é o primeiro nó no diálogo. Observe que além de criar o **node_8**, o serviço também modificou **node_1** para que sua propriedade `previous_sibling` aponte para o novo nó.

É possível criar um nó em outro lugar no diálogo especificando o pai e irmão anteriores:

```json
{
  "dialog_node": "node_9",
  "parent": "node_2",
  "previous_sibling": "node_5"
}
```

Os valores especificados para `parent` e `previous_node` devem ser válidos:

- Ambos os valores devem referir-se a nós existentes.
- O pai especificado deve ser o mesmo que o pai do irmão anterior (ou `null` se o irmão anterior não tiver pai).
- O pai não pode ser um nó do tipo `response_condition` ou `event_handler`.

O diálogo resultante é semelhante a este:

![Diálogo de exemplo 3](images/dialog_api_3.png)

Além de criar o **node_9**, o serviço atualiza automaticamente a propriedade `previous_sibling` de *node_6* para que aponte para o novo nó.

## Movendo um nó para um pai diferente
{: #change-parent}

Vamos mover **node_5** para um pai diferente usando o método POST /dialog_nodes/node_5 com o corpo a seguir:

```json
{
  "parent": "node_1"
}
```

O valor especificado para `parent` deve ser válido:
- Ele deve referir-se a um nó existente.
- Ele não deve referir-se ao nó que está sendo modificado (um nó não pode ser seu próprio pai).
- Ele não deve referir-se a um descendente do nó que está sendo modificado.
- Ele não deve referir-se a um nó do tipo `response_condition` ou `event_handler`.

Isso resulta na estrutura mudada a seguir:

![Diálogo de exemplo 4](images/dialog_api_4.png)

Muitas coisas aconteceram aqui:
- Quando **node_5** mudou para seu novo pai, **node_7** foi com ele (porque o valor de `parent` para **node_7** não mudou). Quando você move um nó, todos os descendentes desse nó permanecem com ele.
- Como nós não especificamos um valor `previous_sibling` para **node_5**, agora ele é o primeiro irmão sob **node_1**.
- A propriedade `previous_sibling` de **node_4** foi atualizada para `node_5`.
- A propriedade `previous_sibling` de **node_9** foi atualizada para `null`, porque agora ela é o primeiro irmão sob **node_2**.

## Resequenciando irmãos
{: #change-sibling}

Agora vamos tornar **node_5** o segundo irmão em vez de o primeiro. Podemos fazer isso usando o método POST /dialog_nodes/node_5 com o corpo a seguir:

```json
{
  "previous_sibling": "node_4"
}
```

Quando você modifica `previous_sibling`, o novo valor deve ser válido:
- Ele deve referir-se a um nó existente
- Ele não deve referir-se ao nó que está sendo modificado (um nó não pode ser seu próprio irmão)
- Ele deve referir-se a um filho do mesmo pai (todos os irmãos devem ter o mesmo pai)

A estrutura muda conforme a seguir:

![Diálogo de exemplo 5](images/dialog_api_5.png)

Observe que, mais uma vez, **node_7** permanece com seu pai. Além disso, **node_4** é modificado para que seu `previous_sibling` seja `null`, porque agora ele é o primeiro irmão.

## Excluindo um nó
{: #delete-node}

Agora vamos excluir **node_1**, usando o método DELETE /dialog_nodes/node_1.

O resultado é este:

![Diálogo de exemplo 6](images/dialog_api_6.png)

Observe que **node_1**, **node_4**, **node_5** e **node_7** foram todos excluídos. Quando você exclui um nó, todos os descendentes desse nó são excluídos também. Portanto, se excluir um nó raiz, você estará de fato excluindo uma ramificação inteira da árvore de diálogo. Quaisquer outras referências ao nó excluído (como referências `next_step`) serão mudadas para `null`.

Além disso, **node_2** é atualizado para apontar para **node_8** como seu novo irmão anterior.

## Renomeando um nó
{: #rename-node}

Finalmente, vamos renomear **node_2** usando o método POST /dialog_nodes/node_2 com o corpo a seguir:

```json
{
  "dialog_node": "node_X"
}
```

![Diálogo de exemplo 7](images/dialog_api_7.png)

A estrutura do diálogo não mudou, mas mais uma vez múltiplos nós foram modificados para refletir o nome mudado:

- As propriedades `parent` de **node_9** e **node_6**
- A propriedade `previous_sibling` de **node_3**

Quaisquer outras referências ao nó excluído (como referências `next_step`) também são mudadas.
