---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

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

# Tarefas avançadas
{: #logs-resources}

Aprenda sobre APIs e outras ferramentas que podem ser usadas para acessar e analisar dados do log.
{: shortdesc}

## API
{: #logs-resources-api}

É possível usar a API `/logs` para listar eventos das transcrições de conversas que ocorreram entre seus usuários e seu assistente. Para obter a documentação de referência detalhada da API, consulte [Listar eventos de log ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/apidocs/assistant#list-log-events-in-a-workspace).

O número de dias que os logs são armazenados difere pelo tipo de plano de serviço. Consulte  [ Limites de log ](/docs/services/assistant?topic=assistant-logs#logs-limits)  para obter detalhes.

Para um script Python que pode ser executado para exportar logs e convertê-los em formato CSV, faça download do arquivo `export_logs.py` por meio do repositório [GitHub do Watson Assistant ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://github.com/watson-developer-cloud/community/blob/master/watson-assistant/export_logs.py).

## Terminologia Relacionada a Logs
{: #logs-resources-terminology}

Primeiro, revise as definições de termos que estão associados aos logs do {{site.data.keyword.conversationshort}}:

- ***Assistente***: um aplicativo - às vezes referido como um 'robô de bate-papo' - que implementa seu conteúdo do {{site.data.keyword.conversationshort}}.
- ***Conversa***: um conjunto de mensagens consistindo nas mensagens que um usuário individual envia para seu assistente e as mensagens que seu assistente envia de volta.
- ***ID de conversa***: identificador exclusivo que é incluído em chamadas de mensagem individual para vincular as trocas de mensagens relacionadas. Os desenvolvedores de aplicativo que usam a versão V1 da API de serviço incluem esse valor para as chamadas de mensagem em uma conversa, incluindo o ID nos metadados do objeto de contexto.
- ***ID de cliente***: um ID exclusivo que poderá ser usado para rotular os dados do cliente de forma que eles possam ser subsequentemente excluídos se o cliente solicitar a remoção de seus dados.
- ***ID de implementação***: um rótulo exclusivo que os desenvolvedores de aplicativo que usam a versão V1 da API de serviço passam com cada mensagem do usuário para ajudar a identificar o ambiente de implementação que produziu a mensagem.
- ***Instância***: sua implementação do {{site.data.keyword.conversationshort}}, acessível com credenciais exclusivas. Uma instância do  {{site.data.keyword.conversationshort}}  pode conter múltiplos assistentes.
- ***Mensagem***: uma mensagem é uma única elocução que um usuário envia para o assistente.
- ***ID de qualificação***: o identificador exclusivo de uma qualificação.
- ***Usuário***: um usuário é qualquer um que interage com seu assistente; geralmente esses são seus clientes.
- ***ID do usuário***: um rótulo exclusivo que é usado para controlar o nível de uso de serviço de um usuário específico.
- ***ID de área de trabalho***: o identificador exclusivo de uma área de trabalho. Embora quaisquer áreas de trabalho criadas antes de 9 de novembro sejam mostradas como qualificações na ferramenta, uma qualificação e uma área de trabalho não são a mesma coisa. Uma qualificação é efetivamente um wrapper para uma área de trabalho V1.

**Importante**: a propriedade **ID do usuário** *não* é equivalente à propriedade **ID do cliente**, embora ambas possam ser passadas para o serviço. O campo **ID do usuário** é usado para rastrear níveis de uso para propósitos de faturamento, enquanto o campo **ID do cliente** é usado para suportar a rotulagem e a exclusão subsequente de mensagens que estão associadas a usuários finais. O ID do cliente é usado de forma consistente em todos os serviços do Watson e é especificado no cabeçalho `X-Watson-Metadata`. O ID do usuário é usado exclusivamente pelo serviço {{site.data.keyword.conversationshort}} e é passado no objeto de contexto de cada chamada da API /message.

## Ativando Métricas do Usuário
{: #logs-resources-user-id}

As métricas do usuário permitem que você veja, por exemplo, o número de usuários exclusivos que se engajaram com seu assistente ou o número médio de conversas por usuário durante um determinado intervalo de tempo na [página Visão geral](/docs/services/assistant?topic=assistant-logs-overview). As métricas do usuário são ativadas usando um parâmetro `User ID` exclusivo.

Para especificar o `User ID` para uma mensagem enviada usando a API `/message`, inclua a propriedade `user_id` dentro do objeto de metadados em seu [contexto ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}, como neste exemplo:

```
"context" : {
  "metadata" : {
       "user_id": "{"
  }
}
```
{: codeblock}

## Associando dados da mensagem a um usuário para exclusão
{: #logs-resources-customer_id}

Pode chegar um momento em que você desejará remover completamente um conjunto de dados de seu usuário de uma instância do {{site.data.keyword.conversationshort}}. Quando o recurso de exclusão for usado, as métricas de Visão geral não refletirão mais essas mensagens excluídas; por exemplo, elas terão menos Totais de conversas.

### Antes de iniciar
{: #logs-resources-delete-customer-id-prereqs}

Para excluir mensagens para um ou mais indivíduos, é necessário primeiro associar uma mensagem a um **ID de cliente** exclusivo para cada indivíduo. Para especificar o **ID de cliente** para qualquer mensagem enviada usando a API `/message`, inclua a propriedade `X-Watson-Metadata: customer_id` em seu cabeçalho. É possível passar múltiplas entradas de **ID de cliente** com pares `field=value` separados por ponto e vírgula, usando `customer_id`, como no exemplo a seguir:

```
curl -X POST -u " apikey:3Df ... ...Y7Pc9 "
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id={first-customer-ID};customer_id={second-customer-ID}'
 --data '{"input":{"text":"hello"}}' 'https:// gateway-us-south.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

A sequência `customer_id` não pode incluir os caracteres ponto e vírgula (`;`) ou sinal de igual (`=`). Você é responsável por assegurar que cada parâmetro `Customr ID` seja exclusivo entre seus clientes.
{: note}

Para excluir mensagens usando valores `customer_id`, consulte o tópico [Segurança de informações](/docs/services/assistant?topic=assistant-information-security#information-security-gdpr-wa).

## Blocos de notas do Jupyter
{: #logs-resources-jupyter-notebooks}

A IBM criou blocos de notas do Jupyter que podem ser usados para analisar os dados do log com mais detalhes. Um bloco de notas do Jupyter é um ambiente baseado na web para computação interativa. É possível executar pequenas partes de código que processam seus dados e é possível visualizar imediatamente os resultados de seu cálculo.

Há um conjunto de blocos de notas que podem ser usados com as ferramentas Python padrão e um conjunto projetado para uso otimizado com o {{site.data.keyword.DSX_full}}. O {{site.data.keyword.DSX_short}} é um produto que fornece um ambiente no qual é possível selecionar e escolher as ferramentas que você precisa para analisar e visualizar dados, para limpar e modelar dados, para alimentar dados de fluxo ou para criar, treinar e implementar modelos de aprendizado de máquina. Consulte a [documentação do produto ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/welcome-main.html){:new_window} para obter mais detalhes.

Os seguintes notebooks estão disponíveis:

- **Medida**: reúne métricas que focam a cobertura (com que frequência o assistente está suficientemente confiante para responder aos usuários) e a efetividade (quando o assistente responde, se as respostas estão satisfazendo as necessidades do usuário).

- **Efetividade**: executa uma análise mais profunda de seus logs para ajudá-lo a entender as etapas que podem ser executadas para melhorar seu assistente.

O [Guia de melhores práticas de melhoria contínua do Watson Assistant ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/common/ssi/cgi-bin/ssialias?htmlfid=54022554USEN&) descreve como obter o máximo dos blocos de notas.

### Usando os blocos de notas com o  {{site.data.keyword.DSX}}
{: #logs-resources-notebooks-studio}

Se você escolher usar os blocos de notas que são projetados para uso com o {{site.data.keyword.DSX}}, em um alto nível, as etapas serão:

1.  Crie uma conta do {{site.data.keyword.DSX}}, [crie um projeto ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/projects.html?context=analytics){:new_window} e inclua uma conta do Cloud Object Storage nele.
1.  Na comunidade do {{site.data.keyword.DSX}}, obtenha o [bloco de notas Medir o desempenho do Watson Assistant ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")]( https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f635e568).
1   Siga as instruções passo a passo fornecidas com o bloco de notas para analisar um subconjunto de trocas de diálogo nos logs.

    Os insights são visualizados de maneiras que tornam mais fácil entender a cobertura e a efetividade do assistente.
1.  Exporte um conjunto de amostra das visualizações subjacentes dos logs de conversas ineficazes e, em seguida, analise e anote-as.

    Por exemplo, indique se uma resposta está correta. Se estiver correta, marque se ela é útil. Se uma resposta estiver incorreta, identifique a causa raiz, por exemplo, a intenção ou entidade errada foi detectada ou o nó de diálogo errado foi acionado. Depois de identificar a causa raiz, indique qual teria sido a opção correta.
1.  Alimente a planilha anotada para o [bloco de notas Analisar a efetividade do Watson Assistant](https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f636921c).

Esse processo ajuda você a entender as etapas que podem ser executadas para melhorar seu assistente. Não há nenhuma maneira de aplicar automaticamente o que você aprende de volta em sua instância de serviço. Mantenha o controle de quaisquer mudanças feitas para melhorar o sistema, assim será possível aplicá-las subsequentemente aos dados de treinamento de sua qualificação de diálogo diretamente.

### Usando os blocos de notas com ferramentas Python padrão
{: #logs-resources-notebooks-python}

Se você escolher usar as ferramentas Python padrão para executar os blocos de notas, será possível obter os blocos de notas do [Repositório IBM GitHub](https://github.com/watson-developer-cloud/assistant-improve-recommendations-notebook/tree/master/notebook). Certifique-se de executá-los na ordem a seguir:

1.  Measure Notebook.ipynb
1.  Efetividade Notebook.ipynb
