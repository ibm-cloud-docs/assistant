---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-12"

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

# Intégration à Intercom ![Forfait Plus ou Premium uniquement](images/premium.png)
{: #deploy-intercom}

Intercom est une plateforme de messagerie client qui aide à stimuler la croissance de l'entreprise grâce à de meilleures relations tout au long du cycle de vie du client.
{: shortdesc}

Intercom s'est associé à IBM pour ajouter un nouvel agent à l'équipe de support client, un assistant virtuel Watson. Vous pouvez intégrer votre assistant à une application Intercom pour lui permettre de transmettre en toute transparence les conversations utilisateur entre votre assistant et les agents humains. Pour plus d'informations sur l'intégration, reportez-vous à la rubrique [l'article Watson ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://medium.com/@blakemcgregor/contact-center-post-394dff427c8). 

Cette intégration est disponible uniquement pour les utilisateurs du forfait Plus ou Premium.
{: note}

Si vous intégrez l'assistant à Intercom, l'application Intercom devient l'application orientée client de votre compétence. Toutes les interactions avec les utilisateurs sont initiées et gérées par Intercom. 

Il n'existe actuellement aucun moyen de transmettre une conversation en cours d'un canal d'intégration à un autre.

## Création unique d'agent  
{: #deploy-intercom-account-prereq}

Vous ou une personne de votre organisation devez effectuer ces étapes préalables uniques avant d'ajouter l'intégration Intercom à votre assistant. 

1.  Créez un compte de messagerie fonctionnel pour votre assistant. 

     Chaque assistant doit avoir une adresse électronique unique valide pour qu'il puisse être ajouté à une équipe dans Intercom.
1.  Dans votre espace de travail Intercom, ajoutez l'assistant à votre équipe en tant que nouvel agent. 

    Accédez à la page des paramètres des coéquipiers dans votre espace de travail Intercom, invitez l'assistant en tant que nouvel agent en ajoutant l'email créé à l'étape précédente à la zone d'invitation. 

    Si vous n'avez pas encore configuré d'espace de travail Intercom, créez-en un sur [www.intercom.com ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.intercom.com). Pour créer un espace de travail, vous devez au minimum avoir souscrit un abonnement au produit *Inbox*.

1.  Dans le compte de messagerie de l'assistant que vous avez créé précédemment, recherchez l'invitation d'Intercom. Cliquez sur le lien dans l'e-mail pour rejoindre l'équipe. Inscrivez-vous en utilisant l'adresse électronique fonctionnelle de l'assistant, puis rejoignez l'équipe.

1.  **Facultatif** : Mettez à jour le profil de votre assistant. 

    Vous pouvez modifier le nom et la photo de profil de votre assistant. Ce profil représente l'assistant dans les communications d'agent privées au sein de votre espace de travail et dans les interactions publiques avec les clients via vos applications Intercom. Créez un profil qui reflète votre marque.

    Cliquez sur l'icône de profil Intercom dans la barre de navigation pour accéder aux paramètres de profil et d'espace de travail. 

    ![Capture d'écran de la page Intercom Setting.](images/intercom-settings.png)

## Préparation du dialogue
{: #deploy-intercom-dialog-prereq}

Procédez comme suit dans votre compétence de dialogue pour que l’assistant puisse traiter les demandes des utilisateurs et transmettre la conversation à un agent humain lorsqu'un client le demande. 

1.  Ajoutez à votre compétence une intention capable de reconnaître la demande d'un utilisateur de parler à un humain. 

    Vous pouvez créer votre propre intention ou ajouter une intention prédéfinie nommée `#General_Connect_to_Agent` fournie avec le catalogue de contenu **General** développé par IBM.

1.  Ajoutez un noeud racine à votre dialogue conditionné par l'intention créée à l'étape précédente. Choisissez **Connect to human agent** comme type de réponse.

1.  Préparez chaque branche de dialogue à déclencher par l’assistant à partir de l’application Intercom. 

    Chaque noeud racine du dialogue peut être traité par l'assistant lorsqu'il fonctionne en tant que membre d'équipe Intercom, y compris les noeuds racine des dossiers. Vous spécifierez l'action que l'assistant doit exécuter pour chaque branche de dialogue ultérieurement lorsque vous configurerez l'intégration Intercom. Par conséquent, bien que vous ne puissiez pas masquer un noeud pour Intercom, vous pouvez configurer l’assistant pour qu’il ne fasse rien lorsque le noeud est déclenché.  

    Renseignez les zones suivantes du noeud racine de chaque branche de dialogue : 

    - **Node name** : attribuez un nom au noeud. Ce nom indique comment vous identifierez le noeud lors de la configuration ultérieure de ses interactions. Si vous n’ajoutez pas de nom, vous devrez choisir le noeud en fonction de son ID de noeud. 
    - **External node name** : ajoutez un résumé de l'objectif de la branche de dialogue. Par exemple, *Trouver un magasin*.

      Cette information est montrée aux autres agents de l'équipe de l'assistant lorsque celui-ci propose de répondre à une requête de l'utilisateur. S'il existe plusieurs noeuds de dialogue pouvant traiter la requête, l'assistant partage une liste d'options de réponse avec des membres d'équipe agents humains pour obtenir leur avis sur la réponse à utiliser. 

      ![Capture d'écran de la zone dans la vue d'édition du noeud dans laquelle vous ajoutez le résumé de l'objectif du noeud.](images/disambig-node-purpose.png)

      **N'ajoutez pas** de nom de noeud externe au noeud racine que vous avez créé à l'étape 2. En cas d'escalade, le service examine le nom du noeud externe du dernier noeud traité pour savoir quel objectif utilisateur n'a pas été atteint. Si vous incluez un nom de noeud externe dans le noeud avec l'intention de connexion à un agent humain, vous empêcherez le service d'apprendre le dernier noeud réel axé sur les objectifs avec lequel l'utilisateur a interagi avant de faire remonter le problème.
      {: tip}

1.  Si un noeud enfant de la branche est conditionné par une demande ou une question de suivi que vous ne voulez pas que l'assistant traite, ajoutez un type de réponse **Connect to human agent** au noeud.

    Par exemple, vous pouvez ajouter ce type de réponse aux noeuds qui traitent de problèmes sensibles que seul un humain doit gérer ou qui suivent les échecs répétés d'un assistant à comprendre un utilisateur.  

    Au moment de l'exécution, si la conversation atteint ce noeud enfant, le dialogue est transmis à un agent humain à ce stade. Plus tard, lorsque vous configurerez l’intégration Intercom, vous pourrez choisir un agent humain comme remplaçant pour chaque branche.  

Votre dialogue est maintenant prêt à prendre en charge votre assistant dans Intercom.  

## Ajout d’une intégration Intercom
{: #deploy-intercom-add-intercom}

1.  Dans l'onglet Assistants, cliquez pour ouvrir la vignette de l'assistant que vous souhaitez déployer. 

1.  Dans la section Integrations, cliquez sur **Add Integration**.

1.  Cliquez sur **Intercom**.

    Suivez les instructions fournies à l'écran. Les sections suivantes vous aider à effectuer la procédure.

## Connexion de l’assistant à Intercom
{: #deploy-intercom-connect}

Dès que vous autorisez Intercom à utiliser l'assistant, celui-ci devient un membre viable de l'équipe Intercom. 

Les agents humains peuvent attribuer des messages à l'assistant à l'aide des règles d'attribution d'Intercom, lesquelles peuvent attribuer automatiquement des conversations entrantes à un équipier ou à une boîte de réception d'équipe en fonction de certains critères, ou par une réaffectation manuelle effectuée par un agent humain lors de l'exécution. Pour plus d'informations, reportez-vous à la [documentation Intercom ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.intercom.com/help/support-and-retain-customers/work-as-a-team/assign-conversations-to-teammates-and-teams).

1.  Lorsque le dialogue est prêt, cliquez sur **Connect now**.
1.  Cliquez sur **Access Intercom** pour être redirigé vers le site Intercom. 

     Connectez-vous à Intercom en utilisant l’adresse électronique et du mot de passe fonctionnels de l’assistant, et non des vôtres. Vous souhaitez établir la connexion à un ID fonctionnel partagé par plusieurs personnes de votre organisation. {: important}

1.  Cliquez sur **Authorize access**.
1.  Cliquez sur **Back to overview**.

L’assistant est maintenant disponible pour recevoir des affectations de la part des membres d'équipe Intercom. Si vous ne l'avez pas encore fait, [configurez votre dialogue](#deploy-intercom-dialog-prereq).
{: important}

## Configuration du routage des messages  
{: #deploy-intercom-config-backup}

Affectez des coéquipiers humains en tant que remplaçants de l’assistant au cas où celui-ci aurait besoin de transférer une conversation en cours à un humain. Vous pouvez choisir une équipe ou un coéquipier différent comme contact de secours pour chaque branche de dialogue. 

Pour configurer les affectations de routage pour les escalades de l'assistant vers un agent humain, procédez comme suit :  

1.  Dans la page d'intégration Intercom, cliquez sur **My Dialog Skill is ready** pour confirmer que vous avez préparé votre dialogue.

    Cliquez sur ce bouton uniquement si vous avez effectué la procédure [Preparing the dialog](#deploy-intercom-dialog-prereq).
    {: important}

1.  Dans la section *Settings*, cliquez sur **Manage rules**.

    Si vous n'apportez aucune modification, le contact de secours pour tous les noeuds reste non attribué.  

1.  Cliquez sur **New rule**.

1.  Dans la liste déroulante *Choose node*, choisissez le noeud de la branche de dialogue que vous souhaitez configurer.

    N'oubliez pas que les branches sont identifiées par leur nom de noeud. Si vous n'avez pas spécifié de nom de noeud, l'ID du noeud est affiché. 

1.  Choisissez l'équipe ou le coéquipier agent humain qui sera le contact de secours pour cette branche de dialogue. La requête de l'utilisateur sera remontée à cette personne si l'assistant ne peut pas répondre à la requête ou rencontre un noeud enfant avec le type de réponse *Connect to human agent* indiquant que seul un agent humain doit y répondre.

1.  Pour définir les règles de routage pour les autres branches du dialogue, cliquez à nouveau sur **New rule**, puis répétez les étapes précédentes.

    N'oubliez pas de définir une affectation pour tous les noeuds racine dont le type de réponse est *Connect to human agent* dans un noeud enfant de leur branche. Si vous ne transférez pas le noeud racine associé à une personne ou à une équipe spécifique, une question sensible peut être transférée vers la boîte de réception *Unassigned*.

1.  Après avoir ajouté les règles, cliquez sur **Return to overview** pour quitter la page.

## Octroi à l'assistant du droit de surveiller et de répondre aux requêtes des utilisateurs  
{: #deploy-intercom-config-action}

Lorsque vous souhaitez que l'assistant commence à surveiller une boîte de réception Intercom et à répondre seul aux messages, activez la surveillance. 

Votre assistant surveille les demandes des utilisateurs à mesure qu'elles sont enregistrées dans Intercom. Lorsque l'assistant a la certitude de savoir comment répondre à une requête d'un utilisateur, il répond directement à l'utilisateur. (L'assistant est confiant lorsque l'intention principale identifiée par le service a une cote de confiance égale ou supérieure à 0,75.)

Si vous ne souhaitez pas que l'assistant réponde à certains types de requêtes utilisateur, vous pouvez ajouter des règles permettant de spécifier d'autres actions que l'assistant doit exécuter par branche de dialogue. Par exemple, vous pouvez commencer à intégrer l’assistant à l’équipe Intercom de manière plus prudente, en lui permettant uniquement de suggérer des réponses à mesure qu’il transfère les messages de l’utilisateur aux autres coéquipiers afin qu'ils y répondent. Au fil du temps, une fois que l'assistant aura fait ses preuves, vous pourrez lui donner plus de responsabilités. 

Pour configurer la manière dont l'assistant doit gérer des branches de dialogue spécifiques, définissez des règles. 

1.  Dans la page d'intégration Intercom, dans la section *Enable your assistant to monitor an inbox*, activez la surveillance (**On**).

1.  Dans *Settings*, cliquez sur **Manage rules**.

    Si vous ne définissez pas de règles, l'assistant est configuré pour surveiller la boîte de réception *Unassigned* et pour répondre automatiquement aux demandes des utilisateurs qu'il est sûr de pouvoir gérer.

1.  Dans la zone *Monitor Intercom inbox*, choisissez la boîte de réception Intercom que l'assistant doit surveiller.

1.  Cliquez sur **New rule** pour définir un canevas d'interaction unique pour une branche de dialogue spécifique.

1.  Dans la liste déroulante *Choose node*, choisissez le noeud de la branche que vous souhaitez configurer.

    N'oubliez pas que les branches sont identifiées par leur nom de noeud. Si vous n'avez pas spécifié de nom de noeud, l'ID du noeud est affiché. 

1.  Choisissez le type d'action que l'assistant doit effectuer lorsque ce noeud de dialogue est déclenché. Les options de type d'action sont les suivantes :  

    - **Do nothing** : l'assistant n'est pas impliqué dans la réponse. Le message de l'utilisateur reste dans la boîte de réception afin que quelqu'un d'autre puisse y répondre. 
    - **Send to team or teammate** : l'assistant évalue l'entrée utilisateur pour déterminer son objectif, puis la transfère au coéquipier approprié.
    - **Suggest to team or teammate** : l'assistant fournit au coéquipier des suggestions sur la façon de réagir en partageant des notes avec l'agent humain via l'application interne Intercom.

      - Si l'entrée utilisateur déclenche une branche de dialogue, c'est-à-dire un noeud racine de dialogue avec des noeuds enfants représentant une interaction complète, l'assistant indique qu'il est capable de répondre à la demande et propose de le faire. 
      - Si l'entrée utilisateur déclenche un noeud racine sans enfant, l'assistant partage simplement la réponse programmée du noeud avec l'agent humain, mais ne répond pas directement à l'utilisateur.
      - Si l'entrée déclenche plusieurs noeuds de dialogue avec une cote de confiance élevée, l'assistant présente au coéquipier humain une liste des réponses possibles et lui demande de choisir la meilleure réponse. 

      Dans tous les cas, l’agent humain décide de laisser l’assistant prendre le relais. 

    - **Answer** : l’assistant répond directement à l’utilisateur, sans s’entretenir avec aucun autre coéquipier.

    L'implication de coéquipiers est obligatoire pour tous les noeuds auxquels vous attribuez les types d'action *Send to team or teammate* ou *Suggest to team or teammate*. Veillez à revenir en arrière et ajouter une règle qui assigne la personne ou l'équipe appropriée en tant que secours pour ce noeud de dialogue en particulier. 

1.  Pour définir des paramètres d'interaction uniques pour les autres branches du dialogue, cliquez à nouveau sur **New rule**, puis répétez les étapes précédentes.

1.  Après avoir ajouté les règles, cliquez sur **Return to overview** pour quitter la page.

Au fur et à mesure de l'évolution de votre dialogue, vous retournerez probablement sur la page d'intégration Intercom pour apporter des modifications incrémentielles à ces règles. 

## Test de l'intégration
{: #deploy-intercom-try}

Pour tester efficacement votre intégration Intercom de bout en bout, vous devez avoir accès à une application utilisateur Intercom. Vous avez déjà créé ou modifié un espace de travail Intercom. L'espace de travail doit posséder un client d'interface utilisateur associé. Si tel n'est pas le cas, reportez-vous à la rubrique [Apps in Intercom ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.intercom.com/help/apps-in-intercom){: new_window} pour obtenir de l'aide sur la configuration de ce client.

Soumettez les requêtes des utilisateurs de test via une application client associée à votre espace de travail Intercom pour vérifier comment les messages sont gérés par Intercom. Vérifiez que les messages destinés à recevoir une réponse par l’assistant génèrent les réponses appropriées et que l’assistant ne répond pas aux messages auxquels il n’est pas configuré pour répondre. 

## Considérations sur les dialogues 
{: #deploy-intercom-dialog}

Certaines réponses enrichies que vous ajoutez à un dialogue sont affichées différemment dans le panneau "Try it out" par rapport à la façon dont elles sont présentées aux utilisateurs Intercom. Le tableau ci-dessous décrit le mode de traitement des types de réponse par Intercom. 

| Type de réponse | Affichage pour les utilisateurs Intercom |
|---------------|---------------------------|
| **Option**    | Les options sont affichées sous forme de liste numérotée. Dans le champ **titre** ou **description**, fournissez des instructions expliquant à l'utilisateur comment choisir une option dans la liste. |
| **Image**     |Le **itre** et la **description** de l'image, et l'image même sont affichés. |
| **Pause**     | Que vous l'activiez ou non, aucun indicateur de frappe ne s'affiche pendant la pause. |

Pour plus d'informations sur les types de réponse, reportez-vous à la rubrique [Réponses enrichies](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia). 
