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

# Fazendo upgrade

Saiba como fazer upgrade de seu plano de serviço.
{: shortdesc}

## Atualizando seu plano
{: #plan-upgrade}

É possível explorar o serviço {{site.data.keyword.conversationshort}} amplamente com o plano Lite. No entanto, há limites para o que pode ser feito.

### Limites por artefato
Para obter informações sobre limites de artefatos por plano, consulte esses tópicos:

- [Áreas de Trabalho](configure-workspace.html#workspace-limits)
- [Nós de diálogo](dialog-build.html#dialog-node-limits)
- [Intenções](intents.html#intent-limits)
- [Entidades](entities.html#entity-limits)
- [Logs](logs_convo.html#log-limits)

### Limites da chamada API
O número de chamadas API permitidas por instância depende de seu plano de serviço. Consulte a descrição do plano para obter detalhes.

Se você tiver um plano Lite e atingir o limite de sua chamada API, mas os registros mostrarem que você fez menos chamadas do que o esperado, lembre-se de que o plano Lite armazena informações de log por apenas 7 dias.

Para atualizar seu plano, conclua essas etapas:

1.  No menu do {{site.data.keyword.Bluemix_notm}}, selecione **Serviços** > **Painel**.
1.  Selecione a instância de serviço que você deseja atualizar para abri-la.
1.  Clique em **Planejar** na área de janela de navegação.
   Aqui é possível ver seu plano atual e outras opções de plano disponíveis e fazer mudanças.

Para obter respostas às perguntas comuns sobre assinaturas, consulte [Gerenciando faturamento e uso ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/billing-usage/how_charged.html){: new_window}.

É possível aprender mais sobre serviços hospedados pelo IBM Cloud nos links a seguir:

- [Termos dos serviços de nuvem](http://www.ibm.com/software/sla/sladb.nsf/sla/saas)
- [Segurança e privacidade de dados de Serviços de nuvem](http://www.ibm.com/software/sla/sladb.nsf/sla/csdsp)

## Atualizando sua área de trabalho
{: #upgrade-workspace}

O serviço {{site.data.keyword.conversationshort}} inclui e atualiza recursos regularmente. Embora algumas dessas mudanças sejam aplicadas automaticamente para sua área de trabalho, as atualizações que têm um grande impacto requerem uma atualização manual.

Um upgrade está disponível para sua área de trabalho somente se o ícone de upgrade (![ícone de upgrade](images/upgrade.png)) é exibido.

**Nota**: depois de fazer upgrade de uma área de trabalho, não é possível reverter sua área de trabalho para uma versão anterior.

Para fazer upgrade de sua área de trabalho, conclua as etapas a seguir:
1.  [Duplique sua área de trabalho](configure-workspace.html#exporting-and-copying-workspaces).
2.  Atualize a área de trabalho duplicada

    Quando você faz upgrade de sua área de trabalho, a versão mais recente da API é ativada na ferramenta e a área de janela "Experimente" começa a usar os recursos mais recentes.
3.  Testar a área de trabalho atualizada.
4.  Depois de avaliar a área de trabalho duplicada para entender como o upgrade impactará seu aplicativo, aplique o upgrade à sua área de trabalho primária.
5.  Faça upgrade de seu aplicativo mudando a chamada API de mensagem para usar a versão mais recente da API.
