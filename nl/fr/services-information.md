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

# Informations sur les services IBM Cloud 
{: #services-information}

L’assistant est un bot entièrement hébergé géré par {{site.data.keyword.cloud_notm}}, ce qui signifie que vous n'avez pas à vous soucier de la configuration ou de la maintenance d'une infrastructure pour le prendre en charge.
{: shortdesc}

## Informations sur le forfait de service 
{: #services-information-plans}

Explorez les [options de forfait de service ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window} {{site.data.keyword.conversationshort}}.

Avant de créer une instance de service, décidez comment vous souhaitez organiser les ressources de votre compte {{site.data.keyword.cloud_notm}}. Si vous ne définissez pas votre propre groupe de ressources, le groupe de ressources **par défaut** est utilisé et vous *ne pourrez pas* le modifier ultérieurement. Pour plus d'informations, reportez-vous à [Meilleures pratiques pour l'organisation des ressources dans un groupe de ressources ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/docs/resources/bestpractice_rgs#bp_resourcegroups){: new_window}. Tous les utilisateurs doivent disposer du rôle Operator d'accès à la plateforme. (Les rôles d'accès au service ne sont pas exploités par {{site.data.keyword.conversationshort}}.)

### Limites de forfait par type d'artefact  
{: #services-information-limits}

Des informations sur les limites d'artefact par forfait sont disponibles dans les rubriques décrivant comment créer les artefacts. Vous pouvez ainsi vous référer aux limites lorsque vous devez les connaître. Voici des liens vers ces rubriques :

- [Assistants](/docs/services/assistant?topic=assistant-assistant-add#assistant-add-limits)
- [Noeuds de dialogue](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-node-limits)
- [Entités](/docs/services/assistant?topic=assistant-entities#entities-limits)
- [Intentions](/docs/services/assistant?topic=assistant-intents#intents-limits)
- [Intégrations](/docs/services/assistant?topic=assistant-deploy-integration-add#deploy-integration-add-limits)
- [Journaux](/docs/services/assistant?topic=assistant-logs#logs-limits)
- [Compétences](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)
- [Versions](/docs/services/assistant?topic=assistant-versions#versions-limits)

### Nombre limite d'appels d'API
{: #services-information-api-limits}

Le nombre d'appels d'API autorisés par instance dépend de votre forfait de service. Pour plus d'informations, reportez-vous à la description de votre forfait.

Si vous disposez d'un forfait Lite et atteignez le nombre limite d'appels d'API alors que les journaux indiquent que vous avez passé moins d'appels que prévu, gardez à l'esprit que le forfait Lite stocke des informations de journal pendant 7 jours uniquement.

Si vous souhaitez effectuer une mise à niveau d'un forfait à un autre, reportez-vous à la rubrique [Mise à niveau](/docs/services/assistant?topic=assistant-upgrade).

### Fonctionnalités des forfaits Plus et Premium 
{: #services-information-premium}

 Les fonctionnalités suivantes ne sont disponibles que pour les utilisateurs des forfaits Premium. 

- [Désambiguïsation](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)
- [Résolution des conflits d'intentions](/docs/services/assistant?topic=assistant-intents#intents-resolve-conflicts)
- [Recommandations sur les exemples utilisateur d'intention](/docs/services/assistant?topic=assistant-intent-recommendations)
- [Intégration Intercom ](/docs/services/assistant?topic=assistant-deploy-intercom)

### Forfaits basés sur l'utilisateur 
{: #services-information-user-based-plans}

Contrairement aux forfaits basés sur des API, qui mesurent l'utilisation par le nombre d'appels d'API effectués au cours d'une période donnée, le nouveau forfait Plus et le forfait Premium mis à jour utilisent une facturation basée sur l'utilisateur. Ils mesurent l’utilisation en fonction du nombre d’utilisateurs uniques ayant interagi avec l’assistant pendant une période donnée. 

Le service recherche les informations suivantes issues des demandes d'API de cette commande à des fins de facturation : 

  1.  **user_id** : propriété définie dans l'API qui est envoyée dans l'objet contextuel d'un appel d'API /message. L'utilisation de cette propriété est le meilleur moyen de vous assurer que vous attribuez correctement des appels d'API /message à des utilisateurs uniques. Pour plus d'informations sur la propriété user ID, reportez-vous à la documentation de référence de l'API :
  
    - `context.global.system.user_id` : [API v2](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant)
    - `context.metadata.user_id` : [API v1](https://cloud.ibm.com/apidocs/assistant#get-response-to-user-input)

  1.  **session_id** : propriété définie dans l'API v2 qui identifie une seule conversation entre un utilisateur et l'assistant. Un identifiant de session est fourni dans les appels d'API /message générés par les intégrations incorporées. La session se termine lorsqu'un utilisateur ferme la fenêtre de discussion ou après 60 minutes d'inactivité. 

  1.  **conversation_id** : propriété définie dans l'API v1 et stockée dans l'objet contexte d'un appel d'API /message. Cette propriété peut être utilisée pour identifier plusieurs appels d'API /message associés à un seul échange conversationnel avec un utilisateur. Toutefois, le même identifiant n'est utilisé que si vous conservez explicitement l'identifiant et le renvoyez avec chaque demande formulée dans le cadre de la même conversation.  Faute de quoi, un nouvel ID est généré pour chaque nouvel appel d'API /message. 

Pour tirer le meilleur parti des nouveaux forfaits de service basés sur les utilisateurs, concevez les applications personnalisées que vous utilisez pour déployer votre assistant afin de capturer un ID utilisateur ou un ID de session unique et de transmettre les informations au service. 

## Authentification des appels d'API 
{: #services-information-authenticate-api-calls}

Le mécanisme d'authentification utilisé par votre instance de service a une incidence sur la manière dont vous devez fournir les données d'identification lors d'un appel d'API. 

1.  Obtention des données d'identification du service. 

    - Recherchez et cliquez sur l'instance de service dans [{{site.data.keyword.Bluemix_notm}}  Liste de ressources ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/resources){: new_window}.

    - Cliquez pour ouvrir votre instance de service, cliquez sur **Données d'identification du service**, puis cliquez sur **Afficher les données d'identification**.

      **Données d'identification de Cloud Foundry**

      ![Illustration de la page de données d'identification du service pour les instances hébergées par Cloud Foundry.](images/cf-cred-ui.png)

      **Données d'identification IAM**

      ![Illustration de la page de données d'identification de service pour les instances hébergées par IAM.](images/iam-creds.png)

1.  Utilisez ces données d'identification dans votre appel d'API. 

    **Appel de l'API Cloud Foundry**

    Indiquez votre nom d'utilisateur et votre mot de passe.

    ```curl
    curl -X GET \
    --user {username}:{password} \
    'https://gateway.watson.net/assistant/api/v1/workspaces?version=2018-09-20'
    ```
    {: codeblock}

     **Appel de l'API IAM**

    - L'URL de base doit inclure l'emplacement. Utilisez la syntaxe `gateway-<location>.watsonplatform.net` pour spécifier l'emplacement dans lequel vous avez créé l'instance de service. Les codes d'emplacement sont répertoriés dans le tableau *Emplacements de centre de données*.
    - Indiquez le type de jeton approprié dans l'en-tête. Vous pouvez transmettre un jeton bearer ou une clé d'API. 

      - Les jetons prennent en charge les demandes authentifiées sans intégrer de données d'identification de service dans chaque appel. L'exemple suivant montre l'utilisation d'un jeton bearer.

        ```curl
        curl -X GET \
        'https://gateway-syd.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        --header 'Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs'
        ```
        {: codeblock}

      - Les clés d'API utilisent l'authentification de base. L'exemple suivant montre l'utilisation d'une clé apikey.

        ```curl
        curl -X GET -u "apikey:3Df... ...Y7Pc9" \
        'https://gateway-us-east.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        ```
        {: codeblock}

        Lorsque vous utilisez un kit SDK Watson, vous pouvez transmettre la clé d'API et laisser le kit SDK gérer le cycle de vie des jetons.
        {: note}

        Les ressources IAM ne peuvent pas être gérées avec l'interface de ligne de commande (CLI) Cloud Foundry. Par exemple, les commandes CLI de Cloud Foundry (commençant par `cf`) qui créent ou gèrent des instances de service ne fonctionnent pas avec des instances hébergées dans des emplacements utilisant IAM. Au lieu de cela, vous devez utiliser la CLI {{site.data.keyword.cloud_notm}} et ses commandes associées. Pour plus d'informations, reportez-vous à la rubrique [Utilisation de ressources et de groupes de ressources ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource). 

        Pour plus d'informations, reportez-vous à la rubrique [Authentification avec des jetons IAM ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/watson?topic=watson-iam){: new_window}. 

    Pour consulter des exemples, reportez-vous à la rubrique [Authentification ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/assistant-v2#authentication){: new_window} pour votre langue dans la référence d'API.

### Centres de données
{: #services-information-regions}

{{site.data.keyword.cloud_notm}} dispose d'un réseau de centres de données mondiaux qui offrent des avantages en termes de performances à ses services de cloud. Pour plus d'informations, reportez-vous à la rubrique [Centres de données mondiaux {{site.data.keyword.cloud_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/data-centers/){: new_window}. 

{{site.data.keyword.cloud_notm}} est passé de la gestion de l'accès des utilisateurs avec Cloud Foundry à l'utilisation de l'authentification IAM (Identity and Access Management) basée sur des jetons. IAM a été déployé à différents endroits et à différents moments. Vous pouvez migrer une instance de service pour la déplacer de son organisation et de son espace Cloud Foundry actuels vers un groupe de ressources. Pour plus d'informations, reportez-vous à la rubrique [Migrations](/docs/services/assistant?topic=assistant-migrate).

Vous pouvez créer des instances de service {{site.data.keyword.conversationshort}} hébergées dans les emplacements de centre de données suivants : 

| Emplacement    | Code d'emplacement | Type d'authentification | Date d'adoption IAM | Remarques |
|-------------|---------------|---------------------|-------------------|-------|
| Dallas      | us-south      | IAM                 | 30 octobre 2018 | N/A |
| Francfort   | eu-de         | IAM                 | 30 octobre 2018 | N/A |
| Sydney      | au-syd        | IAM                 | 7 mai 2018| Les instances créées avant le 7 mai ont été syndiquées à Dallas |
| Tokyo       | jp-tok        | IAM                 | 8 novembre 2018 | N/A |
| Londres     | eu-gb, lon    | IAM                 | 13 décembre 2018 |Les instances créées au Royaume-Uni avant le 13 décembre ont été syndiquées dans la région sud des Etats-Unis. |
| Washington DC  | us-east    | IAM                 | 14 juin 2018| N/A |
{: caption="Emplacement des centres de données " caption-side="top"}

Pour plus d'informations sur les centres de données hébergeant d'autres services {{site.data.keyword.cloud_notm}}, reportez-vous à la rubrique [Services par région![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/docs/resources/services_region#services_region){: new_window}.

## Conditions et sécurité
{: #services-information-terms}

Pour en savoir plus sur les conditions générales du service et de la sécurité des données, lisez les informations suivantes : 

- [Conditions des services ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/home?OpenDocument){: new_window}
- [Sécurité et confidentialité des données ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/software/sla/sladb.nsf/sla/csdsp?OpenDocument){: new_window}
- [Sécurité des informations](/docs/services/assistant?topic=assistant-information-security)

Pour plus d'informations sur {{site.data.keyword.cloud_notm}}, reportez-vous à [Présentation de la plateforme ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/overview?topic=overview-whatis-platform){: new_window}.

## D'autres questions ? 
{: #services-information-sales}

Contactez [IBM Sales ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.
