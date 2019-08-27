---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-02"

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

# Migration à partir de Cloud Foundry
{: #migrate}

Migrez une instance de service {{site.data.keyword.conversationshort}} pour la déplacer de son organisation et de son espace Cloud Foundry en cours vers un groupe de ressources.
{: shortdesc}

{{site.data.keyword.cloud_notm}} est passé de Cloud Foundry à l'utilisation de groupes de ressources.

Les groupes de ressources offrent les avantages suivants par rapport à Cloud Foundry :

- Contrôle d'accès plus détaillé à l'aide de la gestion IAM (IBM Cloud Identity and Access Management)
- Possibilité de connecter des instances de service aux applications et aux services de différentes régions
- Simplification de la capture des données d'utilisation par groupe

Si vous avez créé des instances de service avant novembre 2018, en fonction de l'emplacement où votre instance est hébergée, il est possible que Cloud Foundry soit utilisé à la place de groupes de ressources. Pour savoir quand chaque emplacement a commencé à utiliser IAM avec de nouvelles instances, reportez-vous à la rubrique [Centres de données](/docs/services/assistant?topic=assistant-services-information#services-information-regions).

## Migration d'une instance de service
{: #migrate-task}

Si vous souhaitez en savoir plus sur le processus de migration avant de commencer, reportez-vous à la [documentation sur la migration IBM Cloud ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/resources?topic=resources-migrate).

Seule la personne ou le groupe qui a créé l'instance ou a obtenu l'accès du rôle `Developer` à l'instance peut la faire migrer.
{: note}

Pour faire migrer votre instance de service, procédez comme suit :

1.  Déterminez le groupe de ressources vers lequel vous souhaitez déplacer l'instance de service.

    Pour obtenir des conseils, reportez-vous à [Best practices for organizing resource in a resource group ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/resources?topic=resources-bp_resourcegroups).

    Vous pouvez utiliser une instance de service pour développer, tester et déployer un assistant. Par conséquent, ne créez pas de groupes de ressources distincts pour différents types d'environnements de déploiement.
    {:tip}

    Avec un forfait Premium, vous pouvez créer plusieurs instances. Toutefois, les instances doivent toutes être créées dans le même groupe de ressources. 

1.  Dans la liste de services IBM Cloud Dashboard, cliquez sur l'icône de migration ![Migrate](images/migrate.svg) de l'instance à migrer, puis cliquez sur **Migrate** dans la fenêtre contextuelle.

    Toutes les instances de service créées dans le centre de données de Sydney avant le 7 mai 2018 ou dans le centre de données de Londres avant le 13 décembre 2018 ont été souscrites au centre de données de Dallas. Lorsque vous migrez une instance de service Cloud Foundry basée à Sydney ou à Londres, celle-ci est convertie en une ressource hébergée à Dallas.
    {: note}

1.  Cliquez sur **Continue**, puis choisissez un groupe de ressources.

    Si vous n'avez pas créé de groupe de ressources, vous pouvez en créer un maintenant. Un groupe de ressources **par défaut** est disponible. Prenez le temps de comprendre comment les groupes sont utilisés et créez-en un si nécessaire. Le groupe de ressources que vous choisissez ici *ne peut pas* être modifié ultérieurement.

1.  Cliquez sur **Migrate**.

    Un message s'affiche lorsque le processus est terminé. Si vous devez migrer d’autres instances de service, vous pouvez poursuivre la migration d’autres instances de service ou cliquer sur **Done**.

1.  **Etape unique du forfait Premium** : si l'instance de service que vous migrez a été créée dans le cadre d'un forfait Premium, vous devez informer l'équipe de service de la migration d'une instance de forfait Premium. Pour ce faire, créez un cas à partir d'[IBM Cloud Support ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/unifiedsupport/supportcenter){: new_window}.

    L'équipe de service doit suivre certaines étapes supplémentaires en votre nom. Ajoutez les informations suivantes au cas : 

    - Région dans laquelle l'instance de service est hébergée, telle que Dallas ou Francfort. 
    - Nom du groupe de ressources
    - ID du groupe de ressources

      Si vous ne connaissez pas l'ID, ouvrez l'outil d'interface de ligne de commande (CLI) IBM Cloud et entrez la commande suivante : 

      ```bash
      ibmcloud resource groups
      ```
      {: codeblock}

      La réponse affiche l'ID du groupe de ressources. Pour plus d'informations sur la commande CLI, reportez-vous à la rubrique [Utilisation de ressources et de groupes de ressources ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/docs/cli?topic=cloud-cli-ibmcloud_commands_resource#ibmcloud_commands_resource). 

      Donnez à l’équipe de support jusqu’à 24 heures à compter du moment où vous soumettez le dossier pour effectuer le travail nécessaire à la prise en charge de la migration du forfait Premium. Il n'y a aucune interruption de service pendant le processus de migration. 

      Vous devez uniquement effectuer cette étape la première fois que vous migrez une instance de service pour le forfait Premium. Lorsque vous cliquez sur *Migrate* pour les autres instances de service appartenant au même forfait, elles sont migrées vers le même groupe de ressources sans que l'intervention de l'équipe de service ne soit nécessaire.
      {: important}

L'ancienne instance de service (basée sur une organisation Cloud Foundry) que vous avez migrée continue d'être répertoriée dans la section Cloud Foundry Services du tableau de bord et s'affiche maintenant sous la forme d'un *alias* de la nouvelle version (basée sur le groupe de ressources) de l'instance.

![Illustration indiquant que l'instance de service actuelle est désormais un alias d'une instance basée sur une ressource](images/alias.png)

Pour plus d'informations sur les alias, reportez-vous à la [documentation IBM Cloud Connections ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/docs/resources/connecting_apps#what_is_alias).

Vous devez ouvrir la nouvelle version de l'instance de service basée sur un groupe de ressources pour accéder au bouton **Launch tool**. La nouvelle instance est répertoriée dans la section Services du {{site.data.keyword.Bluemix_notm}} Dashboard.

## Authentification
{: #migrate-auth-support}

Si des applications existantes utilisent l’authentification de base pour accéder au service, elles peuvent continuer à transmettre un nom d’utilisateur et un mot de passe pour s’authentifier auprès de l’instance de service après sa migration. Vous pouvez voir les données d'identification à partir de la page **Service credentials** de l'alias.

Votre nouvelle instance gère l'authentification avec IBM Cloud Identity and Access Management (IAM), un mécanisme amélioré qui utilise des clés API au lieu des données d'identification de nom d'utilisateur et de mot de passe. Vous pouvez voir les informations de clé d'API à partir de la page **Service credentials** de la nouvelle instance de service.

Pensez à mettre à jour vos applications personnalisées existantes de sorte qu'elles utilisent la nouvelle méthode d'authentification pour tirer parti de la sécurité améliorée offerte. Toute modification apportée aux compétences de dialogue dans la nouvelle instance est également reflétée dans les applications qui utilisent également des données d'identification d'authentification de base. Une fois que vous avez mis à jour toutes vos applications pour qu'elles utilisent la nouvelle approche de clé d'API, vous n'avez plus besoin de l'alias, vous pouvez le supprimer.
