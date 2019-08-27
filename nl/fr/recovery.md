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

# Haute disponibilité et reprise après incident
{: #recovery}

{{site.data.keyword.conversationfull}} est hautement disponible dans plusieurs emplacements {{site.data.keyword.cloud}} (par exemple, Dallas et Washington, DC). Cependant, la reprise après des incidents potentiels qui affectent un site entier nécessite une planification et une préparation.
{: shortdesc}

Vous êtes responsable de la compréhension de votre configuration, de la personnalisation et de l’utilisation de {{site.data.keyword.conversationshort}}. Vous êtes également tenu d'être prêt à recréer une instance du service sur un nouveau site et à restaurer vos données sur n'importe quel site. Voir [Comment garantir une disponibilité permanente ? ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/overview?topic=overview-zero-downtime#zero-downtime){: new_window} pour plus d'informations.

## Haute disponibilité
{: #recovery-ha}

{{site.data.keyword.conversationshort}} prend en charge la haute disponibilité sans aucun point de défaillance unique. Le service atteint automatiquement et de manière transparente la haute disponibilité en utilisant la fonctionnalité de région à zones multiples fournie par {{site.data.keyword.cloud_notm}}.

{{site.data.keyword.cloud_notm}} active plusieurs zones qui ne partagent pas un seul point de défaillance au sein d'un même emplacement. Il fournit également un équilibrage automatique de la charge entre les zones d'une région. 

## Reprise après incident
{: #recovery-dr}

La reprise après incident peut devenir un problème si un site {{site.data.keyword.cloud_notm}} est confronté à un échec important incluant la perte potentielle de données. La fonctionnalité de région à zones multiples n'étant pas disponible sur plusieurs sites, vous devez attendre qu'IBM remette un site en ligne s'il devient indisponible. Si les services de données sous-jacents sont compromis par l'échec, vous devez également attendre qu'IBM restaure ces services de données.

En cas de défaillance grave, IBM pourrait ne pas être en mesure de récupérer les données à partir des sauvegardes de la base de données. Dans ce cas, vous devez restaurer vos données pour rétablir votre instance de service à son état le plus récent. Vous pouvez restaurer les données sur le même site ou sur un site différent. 

Votre plan de reprise après incident comprend la connaissance, la conservation et la préparation à la restauration de toutes les données gérées sur {{site.data.keyword.cloud_notm}}. Pour plus d'informations sur la sauvegarde de vos instances de service, reportez-vous à la rubrique [Sauvegarde et restauration des données](/docs/services/assistant?topic=assistant-backup).
