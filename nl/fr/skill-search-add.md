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

# Génération d'une compétence de recherche 
{: #skill-search-add}

Un assistant utilise une *compétence de recherche* pour acheminer des demandes complexes de clients vers le service {{site.data.keyword.discoveryfull}}. {{site.data.keyword.discoveryshort}} traite l'entrée saisie comme une requête de recherche. Il trouve des informations pertinentes pour la requête à partir d'une source de données externe et les renvoie à l'assistant.
{: shortdesc}

Cette fonctionnalité est uniquement disponible pour les participants du programme bêta. Pour savoir comment demander un accès, reportez-vous à la rubrique [Participer au programme bêta](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

![Beta](images/beta.png) IBM publie des services, des fonctions et un support de langue pour votre évaluation qui sont classifiés comme version bêta. Ces fonctions peuvent être instables, changer fréquemment ou être arrêtées dans un délai très court. Les fonctions bêta, qui risquent de ne pas fournir le même niveau de performance ou de compatibilité que les fonctions de l'édition GA (General Availability), ne sont pas destinées à être utilisées dans un environnement de production.

Vous pouvez ajouter une compétence de recherche à un assistant. Pour plus d'informations sur les limites de chaque forfait, reportez-vous à la rubrique [Limites de compétence](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits).

La compétence de recherche recherche des informations dans une collection de données que vous avez créée à l'aide du service {{site.data.keyword.discoveryshort}}. {{site.data.keyword.discoveryshort}} est un service qui analyse, convertit et normalise vos données non structurées. Le service applique l'analyse de données et l'intuition cognitive pour enrichir vos données de sorte que vous puissiez plus facilement trouver et récupérer des informations significatives à partir de celles-ci ultérieurement. Pour en savoir plus sur {{site.data.keyword.discoveryshort}}, reportez-vous à la [documentation produit![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/discovery?topic=discovery-about).

Le service {{site.data.keyword.discoveryfull}} est déclenché des manières suivantes :

- **Noeud Anything else** : recherche une réponse pertinente dans une source de données externe lorsqu'aucun des noeuds de dialogue ne peut répondre à la requête de l'utilisateur. Au lieu d'afficher un message standard, tel que `Je ne sais pas comment vous aider.` l'assistant peut dire `Ces informations peuvent peut-être vous aider :` suivi de la transmission renvoyée par la recherche. Si une compétence de recherche est liée à votre assistant, chaque fois que le noeud `anything_else` est déclenché, plutôt que d'afficher la réponse du noeud, une recherche est effectuée. L'assistant transmet l'entrée utilisateur en tant que requête à votre compétence de recherche et renvoie les résultats de la recherche en tant que réponse. 
- **Type de réponse de recherche ** : si vous ajoutez un type de réponse de recherche à un noeud de dialogue, le service extrait une transmission d'une source de données externe et le renvoie en tant que réponse à une question particulière. Ce type de recherche se produit uniquement lorsque le noeud de dialogue individuel est traité. Cette approche est utile si vous souhaitez restreindre une requête utilisateur avant d'effectuer une recherche. Par exemple, la branche de dialogue peut collecter des informations sur le type d'appareil que le client souhaite acheter. Lorsque vous connaissez la marque et le modèle, vous pouvez ensuite envoyer un mot clé de modèle dans la requête soumise à la compétence de recherche et obtenir de meilleurs résultats. 
- **Compétence de recherche uniquement** : si seule une compétence de recherche est liée à un assistant et qu'aucune compétence de dialogue n'est liée à l'assistant, une requête de recherche est alors soumise au service {{site.data.keyword.discoveryshort}} lorsqu'une entrée utilisateur est reçue d'un des canaux d'intégration de l'assistant.

## Création d'une compétence de recherche 
{: #skill-search-add-task}

Si vous ne l'avez pas déjà fait, suivez les étapes préalables décrites dans le [Tutoriel d'initiation](/docs/services/assistant?topic=assistant-getting-started#getting-started-prerequisites) pour créer une instance de service {{site.data.keyword.conversationshort}} et lancer l'outil {{site.data.keyword.conversationshort}}.

Vous utilisez l'outil {{site.data.keyword.conversationshort}} pour créer des compétences. Procédez comme suit pour créer une compétence de recherche :

1.  Cliquez sur l'onglet **Skills**.

1.  Cliquez sur **Create new**.

1.  Cliquez sur **Add** pour créer une *compétence de recherche*.

1.  Spécifiez les détails de la nouvelle compétence :
    - **Name** : nom qui ne doit pas excéder 100 caractères. Le nom est obligatoire.
    - **Description** : description facultative qui ne doit pas excéder 200 caractères.

1.  Cliquez sur **Create**.

Les étapes restantes diffèrent selon que vous avez ou non accès à une instance de service {{site.data.keyword.discoveryshort}} existante avec des collections créées ou non. Suivez la procédure appropriée à votre situation :

- [Connexion à une instance Watson Discovery existante ](#skill-search-add-connect-discovery)
- [Création d'une instance Watson Discovery ](#skill-search-add-create-discovery)

## Connexion à une instance de service Watson Discovery existante  
{: #skill-search-add-connect-discovery}

1.  Choisissez l'instance de service {{site.data.keyword.discoveryshort}} à partir de laquelle vous souhaitez extraire des informations.
{: #choose-d-instance}

    Toutes les instances de service {{site.data.keyword.discoveryshort}} auxquelles vous avez accès apparaissent dans la liste.

    Si vous voyez un avertissement indiquant que certaines de vos instances de service {{site.data.keyword.discoveryshort}} ne disposent pas de données d'identification, cela signifie que vous avez accès à au moins une instance que vous n'avez jamais ouverte directement à partir du tableau de bord {{site.data.keyword.cloud_notm}}. Vous devez accéder à une instance de service pour que les données d'identification puissent être créées. Et les données d'identification doivent exister pour que {{site.data.keyword.conversationshort}} puisse établir une connexion à l'instance de service {{site.data.keyword.discoveryshort}} en votre nom. Si vous pensez qu'une instance de service {{site.data.keyword.discoveryshort}} doit être répertoriée alors qu'elle ne l'est pas, ouvrez l'instance à partir du tableau de bord {{site.data.keyword.cloud_notm}} directement pour générer les données d'identification.

1.  Indiquez la collection de données à utiliser en effectuant l’une des opérations suivantes :
{: #pick-data-collection}

    - Choisissez une collection de données existante.

      Vous pouvez cliquer sur le lien *Open in Discovery* pour examiner la configuration d'une collection de données avant de choisir celle à utiliser.

      Accédez à [Configure the search](#beta-search-skill-add-configure).

    - Si vous ne possédez pas de collection ou si vous ne souhaitez pas utiliser l'une des collections de données répertoriées, cliquez sur **Create a new collection** pour en ajouter une. Suivez la procédure de la rubrique [Créer une collection de données](#beta-search-skill-add-create-discovery-collection).

      Le bouton **Create a new collection** ne s'affiche pas si vous avez atteint le nombre maximal de collections que vous êtes autorisé à créer en fonction de votre forfait de service {{site.data.keyword.discoveryshort}}. Pour plus d'informations sur les limites des forfaits, reportez-vous à la rubrique [Forfaits de tarification {{site.data.keyword.discoveryshort}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/discovery/discovery-about?topic=discovery-discovery-pricing-plans).

## Création d'une instance de service Watson Discovery  
{: #skill-search-add-create-discovery}

1.  Pour créer une instance de service {{site.data.keyword.discoveryshort}}, cliquez sur **Create new collection**.

    Une instance du service {{site.data.keyword.discoveryshort}} est créée pour vous et une page de configuration s'ouvre pour la nouvelle instance de service {{site.data.keyword.discoveryshort}}.

    Une instance de forfait Lite du service est fournie dans {{site.data.keyword.Bluemix_notm}}, quel que soit le forfait de service {{site.data.keyword.conversationshort}} que vous utilisez. Si vous souhaitez créer l'instance de service {{site.data.keyword.discoveryshort}} dans le cadre d'un autre forfait, arrêtez-vous ici. Créez l'instance de service directement à partir du [Catalogue {{site.data.keyword.Bluemix_notm}}![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/catalog/services/discovery), et suivez la procédure [Se connecter à une instance de service {{site.data.keyword.discoveryshort}} existante](#skill-search-add-connect-discovery) à la place.
    {: important}

1.  Vérifiez les conditions d'utilisation de l'instance, puis cliquez sur **Accept** pour continuer.

1.  [Créez une collection de données](#skill-search-add-create-discovery-collection).

## Création d'une collection de données 
{: #skill-search-add-create-discovery-collection}

Ne tentez pas d'ajouter la source de données pré-enrichie nommée *Watson Discovery News* à votre instance. Ce type de données ne peut pas être recherché à partir de {{site.data.keyword.conversationshort}}.
{: important}

1.  Pour créer une collection {{site.data.keyword.discoveryshort}}, effectuez l'une des opérations suivantes :

      - Pour créer une collection à partir de données stockées dans un type de source de données pour lequel {{site.data.keyword.discoveryshort}} fournit une prise en charge intégrée, cliquez sur **Connect to data source**.

        1.  Sélectionnez un type de source de données.
        1.  Fournissez les informations requises pour la source de données choisie, puis cliquez sur **Connect**.

            Pour plus d'informations, reportez-vous à la rubrique [Connexion aux sources de données ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/discovery?topic=discovery-sources). 
        1.  Indiquez la fréquence à laquelle vous souhaitez que les données de la source de données soient synchronisées avec la collection que vous créez dans {{site.data.keyword.discoveryshort}}.
        1.  Spécifiez les informations que vous souhaitez extraire de la source de données et inclure dans votre collection {{site.data.keyword.discoveryshort}}.

            Les options affichées diffèrent en fonction du type de source de données. 

            - Pour une source de données Salesforce, sélectionnez les types d'objet que vous souhaitez extraire des documents source. Vous pouvez sélectionner un [Type d'objet de cas ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.salesforce.com/docs/atlas.en-us.object_reference.meta/object_reference/sforce_api_objects_case.htm#!) représentant un *cas*, qui est un problème client, par exemple.
            - Pour une source de données Sharepoint, vous spécifiez les chemins. 
            - Pour les référentiels de fichiers, vous spécifiez des répertoires ou des fichiers.  

        1.  Cliquez sur **Save and sync data**.

            La collection de données est créée. Une fois le processus terminé, une page de récapitulatif est affichée dans l'outil {{site.data.keyword.discoveryshort}} dans un onglet de navigateur Web distinct.
        1.  Cliquez sur **Configure your skill in {{site.data.keyword.conversationshort}}** pour revenir à l'outil {{site.data.keyword.conversationshort}}.

      - Pour créer une collection en téléchargeant des documents, cliquez sur **Upload your own data**.

        1.  Vous définissez d'abord la collection, puis vous téléchargez les documents. Fournissez les informations suivantes :

            - Nom de la collection. Le nom doit être unique pour cette instance de service. 
            - Configuration. Vous pouvez choisir d'utiliser un modèle de configuration par défaut ou une configuration enregistrée. Pour plus d'informations sur les configurations, reportez-vous à la rubrique [Configuration de votre service ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/discovery?topic=discovery-configservice).

            - Langue. Sélectionnez la langue des fichiers que vous ajouterez à cette collection. Pour plus d’informations sur les langues prises en charge par {{site.data.keyword.discoveryshort}}, reportez-vous à la rubrique [Support de langue ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/discovery?topic=discovery-language-support). 
        1.  Téléchargez des documents.  

            Les types de fichiers pris en charge incluent les fichiers PDF, HTML, JSON et DOC. Pour plus d'informations, reportez-vous à la rubrique [Ajout de contenu ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/discovery?topic=discovery-addcontent). {: note}

            Aucune synchronisation en cours des documents téléchargés n'est disponible. Si vous souhaitez prendre en compte les modifications apportées à un document, téléchargez une version ultérieure du document. 

Attendez que la collection soit entièrement ingérée avant de revenir à {{site.data.keyword.conversationshort}}.

## Configuration de la recherche 
{: #skill-search-add-configure}

1.  Dans la page de la compétence de recherche {{site.data.keyword.conversationshort}}, cliquez sur **Configure**.

1.  Rédigez différents messages à partager avec les utilisateurs en fonction du succès de la recherche. 

    <table>
    <caption>Messages de résultats de la recherche  </caption>
    <tr>
      <th>Nom de zone</th>
      <th>Scénario </th>
      <th>Exemple de message</th>
    </tr>
    <tr>
      <td>Message</td>
      <td>Les résultats de la recherche sont renvoyés </td>
      <td>J'ai trouvé ces informations qui pourraient être utiles : </td>
    </tr>
    <tr>
      <td>No results found</td>
      <td>Aucun résultat de recherche n'a été trouvé</td>
      <td>J'ai cherché dans ma base de connaissances des informations susceptibles de répondre à votre requête, mais je n'ai rien trouvé d'intéressant à partager. </td>
    </tr>
    <tr>
      <td>Error message</td>
      <td>Le service n'a pas pu effectuer la recherche pour une raison donnée </td>
      <td>J'aurais peut-être des informations susceptibles de répondre à votre requête, mais je ne peux pas effectuer de recherches dans ma base de connaissances pour le moment. </td>
    </tr>
    </table>

1.  Choisissez les zones de collection {{site.data.keyword.discoveryshort}} à partir desquelles vous souhaitez extraire le texte.

    Les zones disponibles diffèrent en fonction des données que vous avez acquises et de la configuration que vous avez utilisée pour les acquérir. 

    Chaque résultat de recherche peut contenir les informations suivantes :  

    - **Title** : titre du résultat de la recherche. Utilisez le titre, le nom ou le type de zone similaire de la collection comme titre du résultat de la recherche.  

      Un élément autre que `None` doit être sélectionné pour que les intégrations Facebook et Slack affichent la réponse.
    - **Body** : description du résultat de la recherche. Utilisez un extrait, un récapitulatif ou une zone en surbrillance de la collection comme corps du résultat de la recherche.  

      Un élément autre que `None` doit être sélectionné pour que les intégrations Facebook et Slack affichent la réponse.
    - **Url** : lien hypertexte vers l'objet de données d'origine dans sa source de données native. La plupart des sources de données en ligne fournissent des URL publiques à référence automatique pour les objets du magasin prenant en charge l'accès direct. 

      L'URL obtenue doit être valide et accessible pour que l'intégration Slack puisse l'inclure dans la réponse et pour que l'intégration Facebook affiche la réponse. `None` est une sélection acceptable pour les intégrations Facebook et Slack.

    Pour obtenir de l'aide, reportez-vous à la rubrique [Conseils pour la sélection de la zone de collection](#skill-search-add-field-tips).
  
     Vous devez choisir une valeur autre que `None` pour au moins une des options. 

    Si aucune option n'est disponible dans les zones déroulantes, vous devrez peut-être donner à {{site.data.keyword.discoveryshort}} plus de temps pour terminer la création de la collection. Faute de quoi, votre collection risque de ne contenir aucun document ou d’avoir des erreurs d’ingestion qu’il faut régler en premier.  

1.  Dans le volet d'aperçu, entrez un message de test pour afficher les résultats renvoyés lorsque vos choix de configuration sont appliqués à la recherche. Faites les ajustements nécessaires. 

1.  Cliquez sur **Create**.

Si vous souhaitez modifier la configuration ultérieurement, ouvrez à nouveau la compétence de recherche et apportez des modifications. Vous n'avez pas besoin de sauvegarder les modifications au fur et à mesure que vous les apportez ; ils sont automatiquement appliqués. Lorsque vous êtes satisfait des résultats de la recherche, cliquez sur **Save** pour terminer la configuration de la compétence de recherche.

## Etapes suivantes
{: #skill-search-add-next-steps}

Une fois la compétence créée, elle apparaît sous la forme d'une vignette sur la page Skills.

La compétence de recherche ne peut pas interagir avec les clients jusqu'à ce qu'elle soit ajoutée à un assistant et que celui-ci soit déployé. Reportez-vous à la rubrique [Création d’assistants](/docs/services/assistant?topic=assistant-assistant-add).

Lorsque vous liez une compétence de dialogue et une compétence de recherche à un assistant, la compétence de recherche est automatiquement déclenchée si l'entrée utilisateur est traitée par la compétence de dialogue et ne peut être traitée par aucun de ses noeuds de dialogue. Plutôt que de répondre par une réponse générique du noeud `anything_else`, une recherche utilisant l'entrée utilisateur en tant que chaîne de requête est lancée.

Si vous le souhaitez, vous pouvez définir une requête de recherche spécifique à appeler en réponse à une condition de noeud particulière.  Pour ce faire, ajoutez un type de réponse de recherche au noeud du dialogue. Pour plus d'informations, reportez-vous à la rubrique [Réponses](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia).

Si vous lancez un type de recherche à partir de votre compétence de dialogue, testez le dialogue pour vous assurer que la recherche est déclenchée comme prévu. Par exemple, si vous n'utilisez pas les types de réponse à la recherche, vérifiez qu'une recherche est déclenchée uniquement lorsqu'aucun noeud de dialogue existant ne peut traiter l'entrée utilisateur. Et chaque fois qu'une recherche est déclenchée, assurez-vous qu'elle renvoie des résultats significatifs. 

### Conseils pour la sélection des zones de collection 
{: #skill-search-add-field-tips}

Les zones de collection appropriées pour l'extraction des données varient en fonction de la source de données de votre collection et de la configuration utilisée pour l'acquisition de la source de données. Pour plus d'informations sur la structure des documents de votre collection, y compris les noms des zones contenant des informations que vous souhaitez extraire, ouvrez la collection dans l'outil {{site.data.keyword.discoveryshort}}, puis cliquez sur **View data schema**.

Le tableau suivant répertorie les zones de collections que vous pouvez essayer dès le début. Ces suggestions supposent que vous avez utilisé le modèle de configuration par défaut lors de la création de la collection.  

| Type de source de données  | Titre | Corps | Url |
|--------------------|-------|------|-----|
| Documents PDF téléchargés | enriched_text.concepts.text | text | Aucune |
| Box                | name | description | listing_url |

Les zones de collection sont créées lors de la création de la collection. Pour plus d'informations sur les zones générées lorsque vous utilisez le modèle de configuration par défaut pour l'ingestion, telles que `enriched_text.concepts.text`, reportez-vous à la rubrique [Configuration de votre service > Ajout d'enrichissements ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](/docs/services/discovery?topic=discovery-configservice#adding-enrichments).

### Ajout de la compétence à un assistant  
{: #skill-search-add-to-assistant}

Vous pouvez ajouter une compétence à un assistant. Vous devez ouvrir la vignette de l’assistant et ajouter la compétence à l’assistant à partir de la page de configuration de l’assistant ; vous ne pouvez pas choisir l'assistant qui utilisera la compétence dans la page de configuration de la compétence. 

Une compétence de recherche peut être utilisée par plusieurs assistants.

1.  Dans l'onglet Assistants, cliquez pour ouvrir la vignette de l'assistant auquel vous souhaitez ajouter la compétence. 

1.  Cliquez sur **Add Search Skill**.

1.  Cliquez sur **Add existing skill**.

    Cliquez sur la compétence que vous souhaitez ajouter parmi les compétences disponibles affichées. 

Configurez au moins un canal d'intégration de test. Vous ne pouvez pas tester la compétence de recherche à partir du panneau "Try it out". Testez la compétence d'un canal d'intégration en entrant des requêtes qui déclenchent la recherche. Assurez-vous que la recherche est correctement déclenchée et renvoie des résultats pertinents. 

L’intégration de liens partageables ne fonctionne pas actuellement pour les assistants possédant une compétence de recherche.
{: important}
