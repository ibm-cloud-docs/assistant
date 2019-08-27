---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-06"

keywords: intent, intent conflicts, annotate

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

# Définition d'intentions
{: #intents}

Les ***intentions*** sont des objectifs exprimés dans une entrée d'un client, par exemple, répondre à une question ou régler une facture. En reconnaissant l'intention exprimée dans une entrée d'un client, le service {{site.data.keyword.conversationshort}} peut choisir le flux de dialogue approprié pour y répondre.
{: shortdesc}

<iframe class="embed-responsive-item" id="youtubeplayer" title="Utilisation des intentions" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/OPdOCUPGMIQ" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Présentation de la création d’intention
{: #intents-described}

- Planifiez les intentions de votre application.

  Examinez ce que vos clients pourraient vouloir faire et ce que vous souhaitez que votre application puisse gérer en leur nom. Par exemple, vous souhaiterez peut-être que votre application aide vos clients à effectuer un achat. Si tel est le cas, vous pouvez ajouter l'intention `#buy_something`. (Le signe `#` ajouté en tant que préfixe au nom de l’intention permet de l’identifier clairement comme étant une intention.)

- Enseignez vos intentions à Watson.

  Une fois que vous avez décidé des demandes métier que votre application doit traiter pour vos clients, vous devez les enseigner à Watson. Pour chaque objectif métier (tel que `#buy_something`), vous devez fournir au moins 5 exemples d'énoncés que vos clients utilisent généralement pour indiquer leur objectif. Par exemple, `Je souhaite effectuer un achat.`
  
  Dans l'idéal, recherchez des exemples d'énoncés utilisateur réels que vous pouvez extraire des processus métier existants. Les exemples d'utilisateurs doivent être adaptés à votre activité spécifique. Par exemple, si vous êtes une compagnie d’assurance, un exemple d’utilisateur pourrait ressembler davantage à ceci : `Je souhaite souscrire un nouveau contrat d’assurance XYZ.`
  
  Les exemples que vous fournissez sont utilisés par l'assistant pour créer un modèle d'apprentissage automatique capable de reconnaître les mêmes types d'énoncés et des types similaires et de les mapper avec l'intention appropriée. 

Commencez avec quelques intentions et testez-les au fur et à mesure que vous développez de manière itérative la portée de l'application.

![Forfait Plus ou Premium uniquement](images/plus.png) Si vous avez déjà des transcriptions de discussion d'un centre d'appels ou des demandes de clients que vous avez recueillies à partir d'une application en ligne, mettez ces données à votre service. Partagez les énoncés réels de vos clients avec Watson et laissez Watson vous recommander les meilleures intentions et les meilleurs exemples d'intentions utilisateur répondant à vos besoins. Pour plus d'informations, reportez-vous à la rubrique [Obtention d'aide pour définir les intentions](/docs/services/assistant?topic=assistant-intent-recommendations).

## Création d'intentions
{: #intents-create-task}

1.  Ouvrez votre compétence de dialogue. La compétence s'ouvre sur la page **Intents**.

1.  Sélectionnez **Create intent**.

1.  Dans la zone **Intent name**, tapez un nom pour l'intention.
    - Le nom d'intention peut contenir des lettres (au format Unicode), des nombres, des traits de soulignement, des traits d'union et des points.
    - Le nom ne peut pas correspondre à `..` ni à aucune autre chaîne composée uniquement de points.
    - Les noms d'intention ne peuvent pas contenir d'espaces et ne doivent pas excéder 128 caractères. Voici quelques exemples de noms d'intention :
        - `#weather_conditions`
        - `#pay_bill`
        - `#escalate_to_agent`

    Un signe dièse `#` est automatiquement ajouté au nom de l'intention afin d'aider à identifier le terme comme une intention. Il n'est pas nécessaire de l'ajouter.
    {: tip}

    Vous pouvez éventuellement ajouter une description de l'intention dans la zone **Description**.

1.  Sélectionnez **Create intent** pour sauvegarder votre nom d'intention.

    ![Capture d'écran illustrant une nouvelle définition d'intention](images/create_intent.png)

1.  Ensuite, dans la zone **Add user example**, tapez le texte d'un exemple d'utilisateur pour l'intention. Un exemple peut être n'importe quelle chaîne de 1024 caractères au maximum. Les énoncés suivants pourraient être des exemples de l'intention `#pay_bill` :
    - `Je dois payer ma facture.`
    - `Payer le solde de mon compte`
    - `effectuer un paiement`

    Pour en savoir plus sur l'impact de l'inclusion de références dans les entités de vos exemples d'utilisateurs, reportez-vous à la rubrique [Traitement des références d'entités](#intents-entity-references).
    {: tip}

    Les noms d'intention et l'exemple de texte peuvent être exposés dans des URL lorsqu'une application interagit avec {{site.data.keyword.conversationshort}}. N'ajoutez pas d'informations sensibles ou personnelles dans ces artefacts.
    {: important}

1.  Cliquez sur **Add example** pour sauvegarder l'exemple utilisateur. 

1.  Répétez le même processus pour ajouter d'autres exemples.

    Indiquez au moins cinq exemples pour chaque intention.
    {: important}

    ![Forfait Plus ou Premium uniquement](images/plus.png) Pour obtenir de l'aide sur la création d'un exemple utilisateur, reportez-vous à la rubrique [Obtenir des recommandations sur les exemples utilisateur d'intention](/docs/services/assistant?topic=assistant-intent-recommendations#intent-recommendations-get-example-recommendations). 

1.  Lorsque vous avez terminé d'ajouter des exemples, cliquez sur ![icône de fermeture (représentée par une flèche)](images/close_arrow.png) pour terminer la création de l'intention. 

Le système commence à se former sur l'intention et les exemples d'utilisateurs que vous avez ajoutés.

## Traitement des références d'entités
{: #intents-entity-references}

Lorsque vous incluez une mention d'entité dans un exemple d'utilisateur, le modèle d'apprentissage machine utilise les informations de différentes manières dans les scénarios suivants :

- [Référence à des valeurs d'entité et des synonymes dans des exemples d'intention](#intents-related-entities)
- [Mention annotée](#intents-annotated-mentions)
- [Référence directe à un nom d'entité dans un exemple d'intention](#intents-entity-as-example)

### Référence à des valeurs d'entité et des synonymes dans des exemples d'intention
{: #intents-related-entities}

Si vous avez défini, ou envisagez de définir, des entités liées à cette intention, mentionnez les valeurs d'entité ou les synonymes dans certains exemples. Cela vous permettra d'établir une relation entre l'intention et les entités. Il s'agit d'une relation faible, mais elle informe le modèle.

![Capture d'écran illustrant une définition d'intention](images/define_intent.png)

*Important* :

  - Les exemples de données d'intention doivent être représentatifs et typiques des données fournies par vos utilisateurs. Des exemples peuvent être collectés à partir de données utilisateur réelles ou auprès de personnes qui sont des spécialistes dans votre domaine spécifique. La nature précise et représentative des données est primordiale.
  - Les données d'apprentissage et les données de test (pour l'évaluation) doivent refléter la distribution des intentions dans une utilisation réelle. Généralement, les intentions plus fréquentes possèdent un plus grand nombre d'exemples et une meilleure couverture de réponse.
  - Vous pouvez inclure de la ponctuation dans l'exemple de texte tant que cela paraît naturel. Si vous pensez que certains utilisateurs expriment leurs intentions avec des exemples comportant de la ponctuation, contrairement à d'autres utilisateurs, incluez les deux versions. Généralement, plus le champ d'application sera élargi à différents canevas, plus la réponse sera appropriée.

### Mentions annotées
{: #intents-annotated-mentions}

Lorsque vous définissez des entités, vous pouvez annoter les mentions de l'entité directement à partir de vos exemples d'utilisateurs d'intention existants. Une relation que vous identifiez de la sorte entre l'intention et l'entité *n'est pas* utilisée par le modèle de classification d'intention. Toutefois, lorsque vous ajoutez la mention à l'entité, elle est également ajoutée à cette entité en tant que nouvelle valeur. Et lorsque vous ajoutez la mention à une valeur d'entité existante, elle est également ajoutée à cette valeur d'entité en tant que nouveau synonyme. La classification d'intention utilise ces types de références de dictionnaire dans les exemples d'utilisateur d'intention pour établir une référence faible entre une intention et une entité.

Pour plus d'informations sur les entités contextuelles, reportez-vous à la rubrique [Ajout d'entités contextuelles](/docs/services/assistant?topic=assistant-entities#entities-create-annotation-based).

### Référence directe à un nom d'entité dans un exemple d'intention
{: #intents-entity-as-example}

Cette approche est avancée. Si elle est utilisée, elle doit être utilisée de manière cohérente.
{: note}

Vous pouvez choisir de référencer directement des entités dans vos exemples d'intention. Supposons que vous ayez une entité appelée `@PhoneModelName`, qui contient les valeurs *Galaxy S8*, *Moto Z2*, *LG G6* et *Google Pixel 2*. Lorsque vous créez une intention, par exemple, `#order_phone`, vous pouvez fournir des données d'apprentissage telles que les suivantes :

- Puis-je obtenir un `@PhoneModelName` ?
- Aidez-moi à commander un `@PhoneModelName`.
- Avez-vous le `@PhoneModelName` en stock ?
- Ajoutez un `@PhoneModelName` à ma commande.

![Capture d'écran illustrant la définition d'une intention](images/define_intent_entity.png)

Actuellement, vous pouvez uniquement référencer directement des entités de synonyme que vous définissez (les valeurs de canevas sont ignorées). Vous ne pouvez pas utiliser des [entités de système](/docs/services/assistant?topic=assistant-system-entities).

Si vous choisissez de référencer une entité en tant qu'exemple d'intention (par exemple, `@PhoneModelName`) *n'importe où* dans vos données d'apprentissage, elle annule la valeur correspondant à l'utilisation d'une référence directe (par exemple, *Galaxy S8*) partout ailleurs dans un exemple d'intention. Toutes les intentions utiliseront ensuite l'approche "entité en tant qu'exemple d'intention". Vous ne pouvez pas appliquer cette approche à une intention spécifique uniquement.
{: important}

En pratique, cela signifie que si vous aviez déjà entraîné la plupart de vos intentions sur la base de références directes (*Galaxy S8*) et que vous utilisez à présent des références d'entité (`@PhoneModelName`) pour une seule intention, ce changement a une incidence sur votre entraînement précédent. Si vous choisissez d'utiliser des références `@Entity`, vous devez remplacer toutes les références directes précédentes par des références `@Entity`.

Définir un exemple d'intention avec une entité `@Entity` pour laquelle 10 valeurs sont définies **ne revient pas** à spécifier 10 fois cet exemple d'intention. Le service {{site.data.keyword.conversationshort}} n'accorde pas beaucoup d'importance à cette syntaxe d'exemple d'intention.

## Test de vos intentions
{: #intents-test}

Lorsque vous avez fini de créer de nouvelles intentions, vous pouvez tester le système pour voir s'il reconnaît vos intentions comme prévu.

1.  Cliquez sur l'icône ![Ask Watson](images/ask_watson.png).

1.  Sur le panneau "Try it out", tapez une question ou une autre chaîne de texte et appuyez sur Entrée pour voir quelle intention est reconnue. Si l'intention reconnue n'est pas celle qui est prévue, vous pouvez améliorer votre modèle en ajoutant ce texte comme exemple dans l'intention appropriée.

    Si vous avez récemment modifié votre compétence, il est possible qu'un message indiquant que le système est toujours en phase d'entraînement s'affiche. Lorsque cela se produit, attendez la fin de la phase d'entraînement avant de lancer les tests :
    {: tip}

    ![Capture d'écran illustrant le message qui s'affiche lorsque Watson est en cours d'entraînement](images/training.png)

    La réponse indique quelle intention a été reconnue à partir de votre entrée.

    ![Capture d'écran illustrant le test des intentions](images/test_intents.png)

1.  Si le système ne reconnaît pas l'intention appropriée, vous pouvez corriger cela. Pour ce faire, sélectionnez l'intention affichée et choisissez l'intention correcte dans la liste. Une fois la correction soumise, le système reprend automatiquement son entraînement pour incorporer les nouvelles données.

    ![Capture d'écran illustrant la correction d'une intention reconnue](images/correct_intent.png)

1.  {: #intents-mark-irrelevant}Si l'entrée n'est liée à aucune des intentions de votre application, vous pouvez former l'assistant en sélectionnant l'intention affichée, puis en cliquant sur **Mark as irrelevant**.

    ![Capture d'écran illustrant l'option Mark as irrelevant](images/irrelevant.png)

    Pour plus d'informations sur cette action, reportez-vous à la rubrique [Enseigner à votre assistant les sujets à ignorer](/docs/services/assistant?topic=assistant-logs#logs-mark-irrelevant).

Si vos intentions ne sont pas correctement reconnues, songez à effectuer les modifications suivantes :

- Ajoutez le texte non reconnu comme exemple dans l'intention correcte.
- Déplacez les exemples d'une intention vers une autre.
- Déterminez si vos intentions sont trop similaires, puis redéfinissez-les le cas échéant.

## Option Absolute scoring
{: #intents-absolute-scoring}

Le service {{site.data.keyword.conversationshort}} évalue la cote de confiance de chaque intention isolément et non par rapport à d'autres intentions. Cette approche ajoute de la flexibilité. Plusieurs intentions peuvent être détectées dans une seule entrée utilisateur. Cela signifie également que le système peut ne pas renvoyer d'intention du tout. Si l'intention principale a une cote de confiance faible (inférieure à 0,2), l'intention supérieure est incluse dans le tableau d'intentions renvoyé par l'API, mais les noeuds conditionnés par l'intention ne sont pas déclenchés. Si vous souhaitez détecter le cas où aucune intention ayant de bonnes cotes de confiance n'a été détectée, utilisez la condition spéciale `irrelevant` dans votre noeud de dialogue. Pour plus d'informations, reportez-vous à la rubrique [Conditions spéciales](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-special-conditions).

A mesure que les cotes de confiance d'intention changent, vos dialogues peuvent nécessiter une restructuration. Par exemple, si un noeud de dialogue utilise une intention dans sa condition, et que la cote de confiance de l'intention commence à descendre régulièrement en dessous de 0,2, le noeud de dialogue cesse d'être traité. Si la cote de confiance change, le comportement du dialogue peut également changer.

## Nombre limite d'intentions
{: #intents-limits}

Le nombre d'intentions et d'exemples que vous pouvez créez dépend du type de votre forfait {{site.data.keyword.conversationshort}} :

| Forfait     | Intentions par compétence | Exemples par compétence |
|------------------|------------------:|-------------------:|
| Premium          |             2 000 |             25 000 |
| Plus             |             2 000 |             25 000 |
| Standard         |             2 000 |             25 000 |
| Lite, Plus Trial |               100 |             25 000 |
{: caption="Détails du forfait" caption-side="top"}

## Edition d'intentions
{: #intents-edit}

Vous pouvez cliquer sur n'importe quelle intention de la liste pour l'ouvrir et l'éditer. Vous pouvez effectuer les modifications suivantes :

- Renommer l'intention
- Supprimer l'intention
- Ajouter, éditer, ou supprimer des exemples
- Déplacer un exemple vers une autre intention

Vous pouvez passer du nom d'intention à chaque exemple à l'aide de la touche de tabulation, et le cas échéant, éditer les exemples.

Pour déplacer ou supprimer un exemple, cochez la case qui lui est associée, puis cliquez sur **Move** ou **Delete**.

  ![Capture d'écran illustrant la procédure de déplacement ou de suppression d'un exemple](images/move_example.png)

## Recherche d'intentions
{: #intents-search}

Utilisez la fonction de recherche pour trouver des exemples d'utilisateur, des noms d'intention et des descriptions.

1.  Dans la page **Intents**, cliquez sur l'icône de recherche.

    ![Icône de recherche dans l'en-tête de page Intents](images/header-search-icon.png)

1.  Entrez un terme ou une phrase de recherche.

    ![Terme de recherche pour une intention](images/searchint_1.png)

Les intentions contenant votre terme de recherche, avec des exemples correspondants, s'affichent.

  ![Résultats renvoyés par la fonction de recherche d'intention](images/searchint_2.png)

## Exportation d'intentions
{: #intents-export}

Vous pouvez exporter un certain nombre d'intentions vers un fichier CSV, de manière à pouvoir les importer et les réutiliser pour une autre application {{site.data.keyword.conversationshort}}.

1.  Dans la page **Intents**, sélectionnez les intentions souhaitées dans la liste, puis cliquez sur **Export**.

    ![Option Export](images/ExportIntent.png)

## Importation d'intentions et d'exemples
{: #intents-import}

Si vous possédez un grand nombre d'intentions et d'exemples, vous trouverez peut-être plus facile de les importer à partir d'un fichier CSV que de les définir un par un. Veillez à supprimer toutes les données personnelles des exemples d'utilisateurs que vous avez inclus dans le fichier.

Vous pouvez également télécharger un fichier contenant des énoncés utilisateur bruts (à partir des journaux du centre d'appel, par exemple) et laisser Watson rechercher des candidats pour des exemples d'utilisateur à partir des données. Pour plus d'informations, reportez-vous à la rubrique [Ajout d’exemples à partir de fichiers journaux](/docs/services/assistant?topic=assistant-intent-recommendations#intent-recommendations-get-example-recommendations). Cette fonctionnalité est disponible pour les utilisateurs du forfait Plus ou Premium uniquement.

1.  Collectez les intentions et les exemples dans un fichier CSV ou exportez-les à partir d'une feuille de calcul dans un fichier CSV. Le format requis pour chaque ligne du fichier est le suivant :

    ```
    <example>,<intent>
    ```
    {: screen}

    Où `<example>` est le texte d'un exemple utilisateur, et `<intent>` est le nom de l'intention à laquelle vous souhaitez que l'exemple corresponde. Par exemple :

    ```
    Quelles sont les conditions météorologiques actuelles ?,weather_conditions
    Est-ce qu'il pleut ?,weather_conditions
    Quelle est la température ?,weather_conditions
    Où est votre emplacement le plus proche ?,find_location
    Avez-vous un magasin à Raleigh ?,find_location
    ```
    {: screen}

    **Important :** sauvegardez le fichier CSV au format UTF-8 et sans marque d'ordre d'octet.

1.  Dans la page **Intents**, cliquez sur l'icône *Import*, ![Icône d'importation](images/importGA.png), puis faites glisser un fichier ou naviguez pour sélectionner un fichier sur votre ordinateur.

    ![Option Import](images/ImportIntent.png)

    **Important :** le fichier CSV ne doit pas excéder 10 Mo. Si votre fichier CSV est plus gros, songez à le fractionner en plusieurs fichiers et à les importer séparément.

    Le fichier est validé et importé et le système commence à s'entraîner lui-même en utilisant les nouvelles données.

Vous pouvez visualiser les intentions importées et les exemples correspondants sur l'onglet **Intents**. Vous devrez peut-être actualiser la page pour voir les nouvelles entités et les nouveaux exemples.

## Résolution des conflits d’intention ![Forfait Plus or Premium uniquement](images/plus.png)
{: #intents-resolve-conflicts}

Cette fonction est disponible uniquement pour les utilisateurs Plus ou Premium.
{: note}

L'application {{site.data.keyword.conversationshort}} détecte un conflit lorsque deux ou plusieurs exemples d'intention dans des intentions *distinctes* sont si similaires que {{site.data.keyword.conversationshort}} ne sait pas quelle intention utiliser.

Pour résoudre les conflits :

1.  Dans la page **Intents**, examinez toutes les intentions avec des conflits.

    ![Conflits dans la liste d'intentions](images/ConflictIntent1.png)

    Utilisez l'option à bascule **Show only conflicts** pour afficher uniquement la liste des intentions avec conflits.
    {: tip}

    ![Vue des conflits uniquement](images/ConflictIntent2.png)

1.  Ouvrez un conflit d'intention. Pour l'exemple d'intention à l'origine du conflit, cliquez sur **Resolve conflict**.

    ![Exemple d'intention conflictuelle](images/ConflictIntent3.png)

1.  Vous avez maintenant la possibilité de déplacer un exemple en conflit vers une autre intention ou de supprimer complètement l'exemple en conflit.

    Dans ce cas, les exemples `Annuler ma commande` et `Je veux annuler ma commande` figurent dans l'intention `#cancel` et l'intention `#eCommerce_Cancel_Product_Order`.

    ![Exemple d'intention conflictuelle](images/ConflictIntent4.png)

    Les exemples d'utilisateurs supplémentaires sont des exemples d'apprentissage qui ne sont pas nécessairement en conflit, mais sont similaires aux exemples en conflit. Ils sont présentés afin de fournir le contexte pour résoudre le conflit.

1.  Sélectionnez les exemples `Annuler ma commande` et `Je veux annuler ma commande` et déplacez-les de l'intention `#cancel` vers l'intention `#eCommerce_Cancel_Product_Order` :

    ![Exemple d'intention conflictuelle](images/ConflictIntent5.png)

1.  Lorsque vous décidez où placer un exemple, recherchez l'intention qui comporte des exemples de synonymes ou de quasi synonymes.

    Gardez chaque intention aussi distincte et centrée sur un objectif que possible. Si deux intentions avec plusieurs exemples d'utilisateurs se chevauchent, peut-être n'avez-vous pas besoin de deux intentions distinctes. Vous pouvez déplacer ou supprimer des exemples d'utilisateurs qui ne se chevauchent pas directement dans une intention, puis supprimer l'autre intention.
    {: tip}

    Sélectionnez les autres exemples dans l'intention `#cancel` et supprimez-les :

    ![Exemple d'intention conflictuelle](images/ConflictIntent6.png)

1.  Cliquez sur le bouton **Submit** pour résoudre le conflit :

    ![Exemple d'intention conflictuelle](images/ConflictIntent7.png)

    L'option *Reset* vous permet de recommencer en déplaçant l'exemple de conflit entre les intentions. L'option *Cancel* vous ramène à la page d'intention.

Vous avez résolu un conflit et vous pouvez continuer à vérifier les autres intentions comportant des conflits

Regardez cette vidéo pour en savoir plus.

<iframe class="embed-responsive-item" id="youtubeplayer0" title="Présentation de la résolution des conflits d'intentions" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/9gQtjCBxjdc?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Suppression d'intentions
{: #intents-delete}

Vous pouvez sélectionner un certain nombre d'intentions à supprimer.

**IMPORTANT** : en supprimant les intentions, vous supprimez également tous les exemples associés et ces éléments ne peuvent pas être récupérés ultérieurement. Tous les noeuds de dialogue qui font référence à ces intentions doivent être mis à jour manuellement de manière à ne plus faire référence au contenu supprimé.

1.  Dans la page **Intents**, sélectionnez les intentions souhaitées dans la liste, puis cliquez sur **Delete**.

    ![Option Delete](images/DeleteIntent.png)
