---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-29"

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

# Mudando a configuração do tempo limite de inatividade ![Somente nos planos Plus ou Premium](images/plus.png)
{: #assistant-settings}

Quando um usuário interage com seu assistente por meio de uma das integrações integradas, a sessão de bate-papo termina após um período específico sem a interação do usuário com o assistente. É possível especificar a quantidade de tempo de espera antes de uma sessão de bate-papo ser reconfigurada mudando a configuração de tempo limite de inatividade.
{: shortdesc}

Quando uma sessão de bate-papo é reconfigurada, o diálogo perde todas as informações contextuais que foram salvas durante a troca anterior com o usuário. Por exemplo, se o diálogo solicitar o nome do usuário e, em seguida, chamá-lo por esse nome durante o restante da conversa, depois que a sessão de bate-papo terminar e uma nova for iniciada, o diálogo começará perguntando o nome do usuário novamente.

## Limites de sessão
{: #assistant-settings-session-limits}

O comprimento permitido para um tempo limite de inatividade difere de acordo com o tipo de plano da instância de serviço. A tabela a seguir lista os limites.

| Plano de Serviço | Período de inatividade padrão da sessão de bate-papo | Período de inatividade máximo da sessão de bate-papo |
|--------------|--------------------------------:|----------------------------:|
| Premium      |                      60 minutos |                    24 horas |
| Mais         |                      60 minutos |                    24 horas |
| Padrão     |                       5 minutos |                   5 minutos |
| Plus Trial   |                       5 minutos |                   5 minutos |
| Lite*        |                       5 minutos |                   5 minutos |
{: caption="Detalhes do plano de serviço" caption-side="top"}

## Mudando a configuração
{: #assistant-settings-timeout-task}

1.  Clique no menu de seu assistente e, em seguida, escolha **Configurações**.

1.  Mude o tempo especificado no campo **Limite de tempo**.

1.  Feche a página de configurações. Sua mudança será salva automaticamente.
