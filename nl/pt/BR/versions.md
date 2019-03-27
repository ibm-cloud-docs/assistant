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

# Criando versões de qualificação
{: #versions}

As versões ajudam a gerenciar o fluxo de trabalho de um projeto de desenvolvimento de qualificação de diálogo.
{: shortdesc}

Crie uma versão de qualificação para fazer uma captura instantânea dos dados de treinamento (intenções e entidades) e do diálogo na qualificação em pontos chave durante o processo de desenvolvimento. Ser capaz de salvar uma qualificação em andamento em um ponto específico no tempo é especialmente útil quando você começa a ajustar seu assistente. Frequentemente, é necessário fazer uma mudança e ver o impacto da mudança em tempo real antes de poder determinar se a mudança melhora ou diminui a efetividade do assistente ou não. Com base em suas descobertas de uma implementação de ambiente de teste, é possível tomar uma decisão informada sobre se uma determinada mudança deve ser implementada em um assistente que é implementado em um ambiente de produção.

Se você tiver um plano grátis (Lite), não será possível criar versões de qualificação.
{: note}

Esse vídeo de 2 1/2 minutos descreve como o uso de versões pode ajudá-lo.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Criando versões de qualificação" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/FDolnBxvcZ8" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Criando uma versão
{: #versions-create}

É possível usar a ferramenta {{site.data.keyword.conversationshort}} para editar somente uma versão de qualificação de diálogo de cada vez. A versão em andamento é chamada de versão de *desenvolvimento*.

Quando você salva uma versão, quaisquer configurações de qualificação aplicadas à versão de desenvolvimento também são salvas.

Para criar uma versão de qualificação de diálogo, siga estas etapas:

1.  No cabeçalho da qualificação, clique em **Salvar nova versão** e, em seguida, descreva o estado atual da qualificação.

    A inclusão de uma boa descrição o ajudará a distinguir múltiplas versões uma da outra posteriormente.

1.  Clique em **Salvar**.

Uma captura instantânea é obtida da qualificação atual e salva como uma nova versão. Você permanece na versão de desenvolvimento da qualificação. Quaisquer mudanças feitas continuam a ser aplicadas à versão de desenvolvimento, não à versão que você salvou. Para acessar a versão salva, acesse a página **Histórico de versão**.

## Implementando uma Versão de Habilidade
{: #versions-deploy}

1.  No cabeçalho da qualificação, clique na guia **Histórico de versão**.
1.  Clique no ícone ![Clique para visualizar ações](images/kebab-react.png) da versão que você deseja implementar e, em seguida, escolha **Designar ao assistente**.

    Uma lista de assistentes para os quais é possível vincular essa versão é exibida. A lista é limitada a esses assistentes que não têm nenhuma qualificação associada a eles ou que estão associados a uma versão diferente dessa qualificação.
1.  Clique na caixa de seleção de um ou mais dos assistentes e, em seguida, clique em **Designar**.

Mantenha o controle de quando essa versão é implementada em um assistente e por quanto tempo. É provável que você desejará analisar as conversas do usuário que ocorrem entre os usuários e essa versão específica da qualificação. É possível obter essas informações na página **Analítica**. No entanto, quando você seleciona uma origem de dados, as versões não são listadas. Deve-se escolher o nome do assistente no qual você implementou essa versão. Em seguida, é possível filtrar os dados de métricas para mostrar somente aquelas conversas que ocorreram entre as datas de início e de encerramento do prazo durante o qual essa versão de qualificação foi implementada no assistente.
{: important}

## Limites de Versão de Skill
{: #skill-version-limits}

O número de versões que podem ser criadas para uma única qualificação depende de seu plano do {{site.data.keyword.conversationshort}}.

| Plano de Serviço     | Versões por habilidade |
|------------------|-------------------:|
| Premium          |                 50 |
| Mais             |                 10 |
| Padrão         |                 10 |
| Lite             |                  0 |
{: caption="Detalhes do plano de serviço" caption-side="top"}

## Trocando qualificações
{: #versions-swap-skills}

Se você descobrir que uma versão anterior da qualificação fez uma tarefa melhor de reconhecer e direcionar as necessidades do cliente do que uma versão mais recente, será possível trocar a qualificação que está vinculada ao assistente para usar a versão anterior.

Siga as mesmas etapas que você usa para [implementar uma versão de qualificação](#versions-deploy) para mudar a versão que está vinculada a um assistente.

## Acessando uma versão salva
{: #versions-view}

A única maneira de visualizar uma versão salva é sobrescrever a versão de desenvolvimento em andamento da qualificação com a versão salva. (Mas não antes de você ter salvo qualquer trabalho que tenha feito na versão de desenvolvimento atual.)

Não é possível editar uma versão salva. Para alcançar o mesmo objetivo, é possível usar uma versão salva como a base para uma nova versão na qual você incorpora quaisquer mudanças que deseja fazer. Para iniciar o trabalho de desenvolvimento de uma versão salva, sobrescreva a versão de desenvolvimento em andamento da qualificação com a versão salva.

1.  Salve todas as mudanças feitas na qualificação desde a última vez que você criou uma versão.

    Salve uma versão da qualificação agora. Caso contrário, seu trabalho será perdido quando você seguir estas etapas.
    {: important}

1.  Na versão que você deseja editar, clique no ícone **Ações de qualificação** ![Ações de qualificação](images/kebab-react.png) e, em seguida, escolha **Copiar para desenvolvimento** e confirme a ação.

    A página é atualizada para reverter para o estado em que a qualificação estava quando a versão foi criada.

Se você deseja salvar quaisquer mudanças feitas nessa versão, deve-se salvar a qualificação como uma nova versão. Não é possível aplicar mudanças a uma versão já salva.

Quando você abre uma qualificação clicando no tile de qualificações na página do assistente, a versão de desenvolvimento da qualificação é exibida. Mesmo se você associou uma versão mais recente ao assistente, quando acessar a qualificação, sua versão de desenvolvimento será aberta.
{: important}
