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

# Mise à niveau
{: #upgrade}

Découvrez comment mettre à niveau votre forfait de service.
{: shortdesc}

## Mise à niveau de votre forfait
{: #upgrade-plan}

Vous pouvez explorer les [options de forfait de service ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window} {{site.data.keyword.conversationshort}} pour décider quel forfait vous convient le mieux.

Vous ne pouvez pas mettre à niveau une instance Cloud Foundry vers un forfait Plus. Vous devez migrer l'instance pour qu'elle utilise un groupe de ressources avant de pouvoir la mettre à niveau.  Pour plus d'informations, reportez-vous à la rubrique [Migrations](/docs/services/assistant?topic=assistant-migrate).
{: note}

Pour mettre votre forfait à niveau, procédez comme suit :

1.  Dans le menu {{site.data.keyword.Bluemix_notm}}, sélectionnez **Upgrade Plan**.
    Votre forfait en cours, ainsi que d'autres options de forfait disponibles, sont affichés et vous pouvez effectuer des modifications.

Pour visualiser les réponses aux questions les plus courantes sur les abonnements, reportez-vous à la rubrique sur la [gestion de la facturation et de l'utilisation ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/billing-usage?topic=billing-usage-charges){: new_window}.

D'autres questions ? Contactez [IBM Sales ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.

## Mise à niveau d'une compétence de dialogue 
{: #upgrade-skill}

Le service {{site.data.keyword.conversationshort}} ajoute et met à jour des fonctions régulièrement. Si certaines modifications sont appliquées automatiquement à vos compétences de dialogue, celles qui ont un impact majeur nécessitent d'être appliquées manuellement au modèle d’apprentissage automatique utilisé par vos compétences.

Une mise à niveau est disponible pour votre compétence de dialogue uniquement si l'icône Upgrade (![icône Upgrade](images/upgrade.png)) est affichée. 

Pour mettre à niveau une compétence de dialogue, procédez comme suit : 

1.  [Téléchargez une copie de votre compétence de dialogue](/docs/services/assistant?topic=assistant-skill-add#skill-add-download-skill), puis importez-la en tant que nouvelle compétence.
2.  Mettez à niveau la nouvelle copie de votre compétence de dialogue. 

    Lorsque vous mettez à niveau votre compétence, la dernière version de l'API est activée dans l'outil et le panneau "Try it out" commence à utiliser les dernières fonctionnalités.
3.  Testez la compétence mise à niveau. 
4.  Après avoir évalué la compétence mise à niveau pour comprendre de quelle manière la mise à niveau impactera votre application, appliquez la mise à niveau à votre compétence de dialogue principale. 
5.  Mettez à niveau votre application. Pour ce faire, modifiez les appels de l'API de message utilisés par votre applications pour spécifier la dernière version de l'API. Pour plus d'informations sur la version de l'API, reportez-vous aux [Notes sur l'édition](/docs/services/assistant?topic=assistant-release-notes).
