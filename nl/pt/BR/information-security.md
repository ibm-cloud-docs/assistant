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

# Segurança de Informações
{: #information-security}

A IBM está comprometida em fornecer aos nossos clientes e parceiros soluções inovadoras de privacidade de dados, segurança e governança.
{: shortdesc}

**Aviso:**
os clientes são responsáveis por assegurar a sua própria conformidade com várias leis e regulamentações, incluindo o Regulamento Geral sobre a Proteção de Dados (GDPR) da União Europeia. Os clientes são os únicos responsáveis por obter aconselhamento de consultoria jurídica competente quanto à identificação e interpretação de quaisquer leis e regulamentos relevantes que possam afetar os negócios dos clientes e quaisquer ações que os clientes possam precisar tomar para cumprir tais leis e regulamentações.

Os produtos, serviços e outros recursos descritos aqui não são adequados para todas as situações do cliente e podem ter disponibilidade restrita. A IBM não fornece aviso jurídico, contábil ou de auditoria nem representa ou garante que os seus serviços ou produtos assegurarão que os clientes estejam em conformidade com qualquer lei ou regulamentação.

Se você precisar solicitar suporte do GDPR para os recursos do {{site.data.keyword.cloud}} {{site.data.keyword.watson}} que forem criados

- Na União Europeia, consulte [Solicitando suporte para recursos do IBM Cloud Watson criados na União Europeia ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/docs/services/watson/getting-started-gdpr-sar#request-EU){: new_window}.
- Fora da União Europeia, consulte [Solicitando suporte para recursos fora da União Europeia ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/docs/services/watson/getting-started-gdpr-sar#request-non-EU){: new_window}.

## Regulamento Geral sobre a Proteção de Dados (GDPR) da União Europeia
{: #information-security-gdpr}

A IBM está comprometida em fornecer aos nossos clientes e parceiros soluções inovadoras de privacidade de dados, segurança e controle para auxiliá-los em sua jornada para a conformidade com GDPR.

Saiba mais sobre a própria jornada de prontidão GDPR da IBM e nossas capacidades e ofertas de GDPR para suportar sua jornada de conformidade [aqui ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](../../icons/launch-glyph.svg "Ícone de link externo")](http://www.ibm.com/gdpr){: new_window}.

## Rotulando e excluindo dados no  {{site.data.keyword.conversationshort}}
{: #information-security-gdpr-wa}

Não inclua dados pessoais nos dados de treinamento (entidades e intenções, incluindo exemplos do usuário) que você cria. Em particular, certifique-se de remover quaisquer informações pessoalmente identificáveis de arquivos que contenham elocuções reais do usuário das quais você faz upload para extrair recomendações de exemplo do usuário.

**Nota:** os recursos experimentais e beta não são destinados ao uso com um ambiente de produção e, portanto, não é garantido que funcionem como esperado ao rotular e excluir dados. Os recursos experimentais e beta não devem ser usados ao implementar uma solução que requer a rotulagem e a exclusão de dados.

Se for necessário remover os dados da mensagem de um cliente de uma instância do {{site.data.keyword.conversationshort}}, será possível fazer isso com base no ID de cliente do cliente, desde que você associe a mensagem a um ID de cliente quando a mensagem for enviada para o serviço.

**Nota:** os recursos de Link de visualização e de integração automática do Facebook não suportam a rotulagem e, portanto, a exclusão de dados com base no ID de cliente. Esses recursos não devem ser usados em uma solução que requeira a capacidade de excluir com base no ID de cliente.

### Antes de iniciar
{: #information-security-delete-user-data-prereqs}

Para ser capaz de excluir dados da mensagem associados a um usuário específico, deve-se primeiro associar todas as mensagens a um **ID de cliente** exclusivo para cada usuário. Para especificar o **ID de cliente** para quaisquer mensagens enviadas usando a API `/message`, inclua a propriedade `X-Watson-Metadata: customer_id` em seu cabeçalho. Por exemplo:

```
curl -X POST -u " apikey:3Df ... ...Y7Pc9 "
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id=abc'
 --data
   '{"input":{"text":"hello"}}'
  'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

A sequência `customer_id` não pode incluir os caracteres ponto e vírgula (`;`) ou sinal de igual (`=`). Você é responsável por assegurar que cada propriedade `customer ID` seja exclusiva em seus clientes.
{: note}

É possível passar múltiplos valores de **ID de cliente** com pares `customer_id={value}` separados por ponto e vírgula. Por exemplo:  ` 'X-Watson-Metadata: customer_id = abc; customer_id = xyz' `

Se você incluir uma qualificação de procura em um assistente, a entrada do usuário que é enviada para o assistente será passada para o serviço {{site.data.keyword.discoveryshort}} como uma consulta de procura. Se a integração do {{site.data.keyword.conversationshort}} fornecer um ID de cliente, a solicitação da API /message resultante incluirá o ID de cliente no cabeçalho e o ID passará pela solicitação da API /query do {{site.data.keyword.discoveryshort}}. Para excluir quaisquer dados de consulta associados a um cliente específico, deve-se enviar uma solicitação de exclusão diretamente para a instância de serviço do {{site.data.keyword.discoveryshort}} que está vinculada a seu assistente. Consulte o tópico [segurança de informações](https://cloud.ibm.com/docs/services/discovery/information-security#gdpr-discovery) do {{site.data.keyword.discoveryshort}} para obter detalhes.

### Consultando dados do usuário
{: #information-security-query-customer-id}

Use o parâmetro `filter` do método `/logs ` v1 para procurar um log do aplicativo para dados específicos do usuário. Por exemplo, para procurar dados específicos para um `customer_id` que corresponde a `my_best_customer`, a consulta pode ser:

``` curl
curl -X GET -u " apikey:3Df ... ...Y7Pc9"
'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/logs?version=2018-09-20&filter=customer_id::my_best_customer'
```
{: codeblock}

Consulte a [Referência de consulta de filtro ](/docs/services/assistant?topic=assistant-filter-reference) para obter detalhes adicionais.

### Excluindo dados
{: #information-security-delete-data}

Para excluir quaisquer dados do log de mensagens associados a um usuário específico que o serviço pode ter armazenado, use o método da API v1 `DELETE /user_data`. Especifique o ID de cliente do usuário passando um parâmetro `customer_id` com a solicitação.

Somente os dados que foram incluídos usando o terminal da API `POST /message` com um ID de cliente associado podem ser excluídos usando esse método de exclusão. Os dados que foram incluídos por outros métodos não podem ser excluídos com base no ID de cliente. Por exemplo, as entidades e as intenções que foram incluídas de conversas do cliente não podem ser excluídas dessa maneira. Os Dados pessoais não são suportados para esses métodos.

**IMPORTANTE**: especificar um `customer_id` excluirá *todas* as mensagens com esse `customer_id` que foram recebidas antes da solicitação de exclusão, em toda a sua instância do {{site.data.keyword.conversationshort}}, não apenas dentro de uma qualificação.

Como um exemplo, para excluir quaisquer dados de mensagem associados a um usuário que tem o ID de cliente `abc` de sua instância do {{site.data.keyword.conversationshort}}, envie o comando cURL a seguir:

```
curl -X DELETE -u " apikey:3Df ... ...Y7Pc9"
 'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/user_data?customer_id=abc&version=2018-09-20'
```
{: codeblock}

Um objeto JSON vazio `{}` é retornado.

Para obter mais informações, consulte a [Referência de API](https://cloud.ibm.com/apidocs/assistant?curl=#delete-labeled-data).

**Nota:** as solicitações de exclusão são processadas em lotes e podem levar até 24 horas para serem concluídas.
