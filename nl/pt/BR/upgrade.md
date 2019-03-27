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

# Fazendo upgrade
{: #upgrade}

Saiba como fazer upgrade de seu plano de serviço.
{: shortdesc}

## Atualizando seu plano
{: #upgrade-plan}

É possível explorar as [opções de plano de serviço ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window} do {{site.data.keyword.conversationshort}} para decidir qual plano é melhor para você.

Não é possível fazer upgrade de uma instância baseada no Cloud Foundry para um plano Plus. Deve-se migrar a instância, de forma que ela esteja usando um grupo de recursos antes que seja possível fazer upgrade dela. Consulte  [ Migrando ](/docs/services/assistant?topic=assistant-migrate)  para obter mais detalhes.
{: note}

Para atualizar seu plano, conclua essas etapas:

1.  No menu  {{site.data.keyword.Bluemix_notm}} , selecione  ** Fazer upgrade do plano **.
    Aqui é possível ver seu plano atual e outras opções de plano disponíveis e fazer mudanças.

Para obter respostas às perguntas comuns sobre assinaturas, consulte [Gerenciando faturamento e uso ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/billing-usage?topic=billing-usage-charges){: new_window}.

Ainda tem dúvidas? Entre em contato com o [IBM Sales ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.

## Fazendo upgrade de uma habilidade de diálogo
{: #upgrade-skill}

O serviço {{site.data.keyword.conversationshort}} inclui e atualiza recursos regularmente. Embora algumas dessas mudanças sejam aplicadas automaticamente às suas qualificações de diálogo, as atualizações que têm um grande impacto requerem uma atualização manual para o modelo de aprendizado de máquina que é usado por sua qualificação.

Um upgrade estará disponível para sua qualificação de diálogo somente se o ícone de upgrade (![ícone de upgrade](images/upgrade.png)) for exibido.

Para fazer upgrade de sua qualificação de diálogo, conclua as etapas a seguir:

1.  [Faça download de uma cópia de sua qualificação de diálogo](/docs/services/assistant?topic=assistant-skill-add#skill-add-download-skill) e, em seguida, importe-a como uma nova qualificação.
2.  Faça upgrade da nova cópia de sua qualificação de diálogo.

    Quando você faz upgrade de sua qualificação, a versão mais recente da API é ativada na ferramenta e a área de janela "Experimente" começa a usar os recursos mais recentes.
3.  Teste a habilidade atualizada.
4.  Depois de avaliar a qualificação submetida a upgrade para entender como o upgrade afetará seu aplicativo, aplique o upgrade à sua qualificação de diálogo primário.
5.  Faça upgrade de seu aplicativo. Para fazer isso, mude as chamadas da API da mensagem que ele usa para especificar a versão da API mais recente. Para obter detalhes da versão da API, consulte as [notas sobre a liberação](/docs/services/assistant?topic=assistant-release-notes).
