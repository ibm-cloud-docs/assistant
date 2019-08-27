---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-12"

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

# Alta disponibilidade e recuperação de desastre
{: #recovery}

O {{site.data.keyword.conversationfull}} é altamente disponível em vários locais do {{site.data.keyword.cloud}} (por exemplo, Dallas e Washington, DC). No entanto, a recuperação de possíveis desastres que afetam todo um local requer planejamento e preparação.
{: shortdesc}

Você é responsável por entender sua configuração, customização e uso do {{site.data.keyword.conversationshort}}. Você também é responsável por estar pronto para recriar uma instância do serviço em uma nova localização e por restaurar seus dados em qualquer localização. Consulte [Como assegurar tempo de inatividade zero? ![Ícone de link externo](../../icons/launch-glyph.svg "Ícone de link externo")](/docs/overview?topic=overview-zero-downtime#zero-downtime){: new_window} para obter mais informações.

## Alta disponibilidade
{: #recovery-ha}

O {{site.data.keyword.conversationshort}} suporta alta disponibilidade sem nenhum ponto único de falha. O serviço alcança a alta disponibilidade de forma automática e transparente, usando o recurso de região multizona fornecido pelo {{site.data.keyword.cloud_notm}}.

O{{site.data.keyword.cloud_notm}}permite diversas zonas que não compartilham um único ponto de falha em um único local. Ele também fornece balanceamento automático de carga
entre as zonas dentro de uma região.

## Recuperação de desastre
{: #recovery-dr}

A recuperação de desastre pode se tornar um problema se um local do {{site.data.keyword.cloud_notm}} passar por uma falha significativa que inclua a possível perda de dados. Como o recurso de região multizona não está disponível nos locais, deve-se aguardar até que a IBM coloque um local novamente on-line no caso de ele se tornar indisponível. Caso serviços de dados subjacentes tenham sido comprometidos pela falha, deve-se também aguardar que a IBM os restaure.

No caso de uma falha catastrófica, a IBM pode não conseguir recuperar dados de backups de banco de dados. Nesse caso, é necessário restaurar seus dados para retornar a sua instância de serviço para seu estado mais recente. É possível restaurar os dados para o mesmo local ou para um local diferente.

Seu plano de recuperação de desastre inclui conhecer, preservar e estar preparado para restaurar todos os dados que são mantidos no {{site.data.keyword.cloud_notm}}. Consulte [Fazendo backup e restaurando dados](/docs/services/assistant?topic=assistant-backup) para obter informações sobre como fazer backup de suas instâncias de serviço.
