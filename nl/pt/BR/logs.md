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

# Sobre o componente Improve

O componente Improve do {{site.data.keyword.conversationshort}} fornece um histórico de interações com usuários de sua área de trabalho. É possível usar esse histórico para melhorar o entendimento de sua área de trabalho de entradas dos usuários.
{: shortdesc}

Seções do painel **Melhorar**:

* [Visão geral](logs_oview.html): um resumo de interações de usuários com uma área de trabalho.
* [Conversas do usuário](logs_convo.html): uma lista de elocuções do usuário. É possível atualizar intenções e entidades ao visualizar uma elocução de usuário individual. **Nota**: uma única conversa do usuário pode consistir em múltiplas elocuções.
* [Recomendações](logs_recommend.html): formas de melhorar seu sistema. Disponível somente para usuários Premium.

## Melhorando em áreas de trabalho
{: #deploy_id}

Para entender como usar os dados de elocução para fazer melhorias em áreas de trabalho, é útil revisar as definições a seguir associadas ao serviço {{site.data.keyword.conversationshort}}:

* ***Instância***: sua implementação do {{site.data.keyword.conversationshort}}, acessível com credenciais exclusivas. Uma instância do {{site.data.keyword.conversationshort}} pode ser composta por múltiplas áreas de trabalho
* ***Área de trabalho***: uma área de trabalho é um modelo de seu conteúdo do {{site.data.keyword.conversationshort}}; muitas vezes, isso pode equiparado a um robô
* ***ID de área de trabalho***: o identificador exclusivo de uma área de trabalho
* ***ID de implementação***: os IDs de implementação são rótulos exclusivos passados com elocuções do usuário para ajudar a identificar o ambiente de implementação do qual as elocuções vêm
* ***Elocução***: uma elocução é uma única mensagem que um usuário envia para a área de trabalho

A criação de uma área de trabalho é um processo muito iterativo. Enquanto desenvolve sua área de trabalho, você usa a área de trabalho *Experimente* para verificar se sua área de trabalho reconhece as intenções e entidades corretas em entradas de teste e fazer as correções conforme necessário.

No painel **Melhorar**, é possível visualizar informações sobre interações reais com seus usuários e fazer correções semelhantes para melhorar a precisão com a qual as intenções e as entidades são reconhecidas por sua área de trabalho. É difícil saber exatamente *como* seus usuários farão perguntas ou quais elocuções aleatórias eles podem fazer, então é importante visitar frequentemente o painel **Melhorar**, a fim de melhorar suas áreas de trabalho.

Para uma instância do {{site.data.keyword.conversationshort}} que inclui múltiplas áreas de trabalho, às vezes pode ser útil usar dados de elocução de uma área de trabalho para melhorar outra área de trabalho dentro dessa mesma instância. **Nota**: se você é um usuário Premium do {{site.data.keyword.conversationshort}}, suas instâncias premium podem opcionalmente ser configuradas para permitir acesso a dados do log de áreas de trabalho em diferentes instâncias premium.

Como um exemplo, suponha que você tenha uma instância do {{site.data.keyword.conversationshort}} nomeada *HelpDesk*. Você pode ter uma área de trabalho Produção e uma área de trabalho Desenvolvimento em sua instância do HelpDesk. Ao trabalhar na área de trabalho Desenvolvimento, é possível filtrar elocuções com base no `Deployment ID` para a área de trabalho Produção, assim você está usando elocuções da área de trabalho Produção para melhorar sua área de trabalho Desenvolvimento.

![Link de origem de dados](images/data_source_1.png)

Todas as edições que você fizer dentro da área de trabalho Desenvolvimento afetarão somente a área de trabalho Desenvolvimento, mesmo se estiver usando as elocuções da área de trabalho Produção. Veja [Selecionando uma origem de dados](logs_convo.html#select-source) para obter informações adicionais.

Para especificar o ID de implementação para uma elocução enviada usando a API `/messaege`, inclua a propriedade de implementação dentro do objeto de metadados em seu [contexto ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#send_message){: new_window}, como neste exemplo:

```
"context" : {
  "username" : "jane_doe@myemail.com",
  "member_type" : "gold",
  "metadata" : {
       "deployment": "HelpDesk-Production"
  }
}
```
{: #codeblock}
