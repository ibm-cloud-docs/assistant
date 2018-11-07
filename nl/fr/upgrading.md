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

# Mise à niveau

Découvrez comment mettre à niveau votre plan de service.
{: shortdesc}

## Mise à niveau de votre plan
{: #plan-upgrade}

Vous pouvez explorer longuement le service {{site.data.keyword.conversationshort}} à l'aide du plan Lite. Toutefois, les actions que vous pouvez exécuter sont limitées.

### Nombre limite d'artefacts
Pour plus d'informations sur le nombre limite d'artefacts par plan, reportez-vous aux rubriques suivantes :

- [Espaces de travail](configure-workspace.html#workspace-limits)
- [Noeuds de dialogue](dialog-build.html#dialog-node-limits)
- [Intentions](intents.html#intent-limits)
- [Entités](entities.html#entity-limits)
- [Journaux](logs_convo.html#log-limits)

### Nombre limite d'appels d'API
Le nombre d'appels d'API autorisés par instance dépend de votre plan de service. Pour plus d'informations, reportez-vous à la description de votre plan.

Si vous disposez d'un plan Lite et atteignez le nombre limite d'appels d'API mais les journaux indiquent que vous avez passé moins d'appels que prévu, gardez à l'esprit que le plan Lite stocke des informations de journal pendant 7 jours uniquement.

Pour mettre votre plan à niveau, procédez comme suit :

1.  Dans le menu {{site.data.keyword.Bluemix_notm}}, sélectionnez **Services** > **Tableau de bord**.
1.  Sélectionnez l'instance de service que vous souhaitez mettre à niveau afin de l'ouvrir.
1.  Cliquez sur **Plan** dans le panneau de navigation.
   Votre plan en cours, ainsi que d'autres options de plan disponibles, sont affichés et vous pouvez effectuer des modifications.

Pour visualiser les réponses aux questions les plus courantes sur les abonnements, reportez-vous à la rubrique sur la [gestion de la facturation et de l'utilisation ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/billing-usage/how_charged.html){: new_window}.

Pour en savoir plus sur les services hébergés par IBM Cloud, cliquez sur les liens suivants :

- [Cloud Services terms](http://www.ibm.com/software/sla/sladb.nsf/sla/saas)
- [Cloud Services data security and privacy](http://www.ibm.com/software/sla/sladb.nsf/sla/csdsp)

## Mise à niveau de votre espace de travail
{: #upgrade-workspace}

Le service {{site.data.keyword.conversationshort}} ajoute et met à jour des fonctions régulièrement. Si certaines modifications sont appliquées automatiquement à votre espace de travail, celles qui ont un impact majeur nécessitent d'être appliquées manuellement.

Une mise à niveau est disponible pour votre espace de travail uniquement si l'icône Upgrade (![icône Upgrade](images/upgrade.png)) est affichée. 

**Remarque** : une fois que vous avez mis à niveau un espace de travail, vous ne pouvez plus rétablir la version précédente de cet espace de travail. 

Pour mettre votre espace de travail à niveau, procédez comme suit :
1.  [Dupliquez votre espace de travail](configure-workspace.html#exporting-and-copying-workspaces).
2.  Mettez à niveau l'espace de travail dupliqué.

    Lorsque vous mettez à niveau votre espace de travail, la dernière version de l'API est activée dans l'outil et le panneau "Try it out" commence à utiliser les fonctions les plus récentes.
3.  Testez l'espace de travail mis à niveau. 
4.  Après avoir évalué l'espace de travail en double pour comprendre de quelle manière la mise à niveau impactera votre application, appliquez la mise à niveau à votre espace de travail principal. 
5.  Mettez à niveau votre application en modifiant l'appel d'API de message de telle manière que la dernière version d'API soit utilisée. 
