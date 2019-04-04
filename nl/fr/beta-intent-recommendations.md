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

# Obtention d'aide pour construire des intentions ![Bêta](images/beta.png) ![Plus ou Premium uniquement](images/premium.png)
{: #beta-intent-recommendations}

Si vous possédez des données existantes de transcription de discussions par chat de support client, laissez Watson analyser ces données pour comprendre les besoins client auxquels votre équipe de support consacre le plus clair de son temps.
{: shortdesc}

Cette fonctionnalité est uniquement disponible pour les participants du programme bêta. Pour savoir comment demander un accès, reportez-vous à la rubrique [Participer au programme bêta](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

![Beta](images/beta.png) IBM publie des services, des fonctions et un support de langue pour votre évaluation qui sont classifiés comme version bêta. Ces fonctions peuvent être instables, changer fréquemment ou être arrêtées dans un délai très court. Les fonctions bêta, qui risquent de ne pas fournir le même niveau de performance ou de compatibilité que les fonctions de l'édition GA (General Availability), ne sont pas destinées à être utilisées dans un environnement de production.

Lorsqu'elle sera disponible, cette fonctionnalité sera destinée aux utilisateurs des forfaits Plus ou Premium et fonctionnera uniquement avec des énoncés en anglais.
{: tip}

Les besoins des clients sont représentés dans {{site.data.keyword.conversationshort}} en tant qu'*intentions*. Si vous n'avez pas encore défini d'intention, vous pouvez commencer plus rapidement en demandant de l'aide à Watson. Téléchargez des fichiers contenant des énoncés utilisateur à partir des transcriptions du centre d'appel à faire analyser par le service {{site.data.keyword.conversationshort}}. Sur la base des informations qu’il découvre, le service recommande un ensemble d’intentions de base que vous devez définir pour couvrir les besoins les plus courants de vos clients.  

Dans la mesure où les sujets que vos clients veulent aborder évoluent, vous pouvez utiliser la fonctionnalité de recommandations sur les exemples utilisateur d'intention pour vous aider à conserver vos intentions à jour et pertinentes dans le temps. 

Explorez vos données existantes pour effectuer l'une des tâches suivantes : 

- [Obtention de recommandations d'intention ](#beta-intent-recommendations-get-intent-recommendations)
- [Obtention de recommandations d'exemples utilisateur d'intention ](#beta-intent-recommendations-get-example-recommendations)

## Téléchargement de vos fichiers
{: #beta-intent-recommendations-log-files-add}

Avant de commencer, créez un fichier à fournir au service. Il doit s'agir d'un fichier CSV (valeurs séparées par des virgules), contenant un énoncé utilisateur par ligne. Dans l'idéal, les énoncés sont de courtes expressions extraites des transcriptions de votre centre d'appel et contenant les questions et les demandes réelles posées par les clients. La taille maximale de chaque fichier d'énoncés utilisateur peut atteindre 20 Mo. 

Suivez ces instructions supplémentaires : 

  - Supprimez toutes les données sensibles des énoncés que vous avez inclus dans le fichier. 

Les données sensibles incluent toutes les informations relatives à une personne physique identifiable, telles que les noms, les adresses électroniques, les identifiants client et les données réglementées telles que les informations de santé protégées.
  - N'incluez pas d'énoncés utilisateur dépassant 1 024 caractères. Les énoncés plus longs sont tronqués. 
  - Le fichier doit contenir au moins 100 énoncés ; un fichier de 500 énoncés ou plus vous donnera de meilleurs résultats.  
  - Si un énoncé contient une virgule, placez-le entre guillemets. 
  - Le fichier CSV doit inclure une seule colonne. 
  - Supprimez tout ce qui n'est pas un énoncé généré par l'utilisateur, y compris les réponses ou notes de l'agent humain.

  Par exemple :

  ```
  Qu'advient-il de mon assurance si je vends ma voiture ?
  je voudrais acheter une maison.
  Comment puis-je ajouter une personne à mon forfait ?
  "D'abord, je veux savoir si je suis déjà inscrit."
  ```

Tous les fichiers que vous téléchargez sont partagés entre toutes les compétences de l'instance de service actuelle. Les énoncés de tous les fichiers disponibles sont analysés lorsque vous demandez à la fois des recommandations d'intention et des recommandations sur les exemples utilisateur d'intention. 

## Obtention de recommandations d'intention auprès de Watson 
{: #beta-intent-recommendations-get-intent-recommendations}

Lancez-vous rapidement en permettant au service d’analyser les transcriptions de vos discussions par chat avec le centre d’appel et de vous recommander des intentions initiales avec lesquelles vous pourriez commencer. Si vous avez déjà créé certaines intentions, laissez le service analyser vos journaux et comparer ses conclusions avec vos intentions existantes pour identifier les lacunes dans vos données d'apprentissage et suggérer de nouvelles intentions pour les combler. 

Pour utiliser cette fonctionnalité, téléchargez un ou plusieurs fichiers contenant les énoncés soumis par des utilisateurs réels lorsqu'ils demandent de l'aide. Le fichier peut par exemple répertorier les demandes utilisateur extraites des journaux du centre d'appel. Le service évalue ces énoncés et identifie les problèmes courants que le client mentionne fréquemment. L'outil {{site.data.keyword.conversationshort}} affiche ensuite un ensemble d'intentions discrètes qui capturent ces tendances en matière de besoins utilisateur. Vous pouvez consulter chaque intention recommandée et les exemples d'utilisateurs correspondants pour choisir celles que vous souhaitez ajouter à vos données d'apprentissage. 

## Obtention de recommandations d'intention 
{: #beta-intent-recommendations-get-intent-recommendations-task}

Pour obtenir des recommandations d'intention, procédez comme suit : 

1.  Dans l'outil {{site.data.keyword.conversationshort}}, ouvrez votre compétence de dialogue, puis cliquez sur l'onglet **Intents** dans la barre de navigation. Si l'onglet **Intents** ne s'affiche pas, utilisez le menu ![Menu](images/Menu_16.png) pour ouvrir la page.

1.  Cliquez sur **Get recommendations**.

1.  **First time only** : cliquez sur **Add file**, puis cliquez sur **Choose a file** pour rechercher le fichier CSV que vous avez créé précédemment et sélectionnez-le.

    Donnez au service le temps d'analyser vos données et de regrouper les énoncés. 

1.  Consultez les intentions recommandées par le service. 

    Les besoins client les plus fréquents sont capturés dans les intentions énumérées en premier. Vous pouvez survoler une intention pour voir quelques exemples des énoncés associés. Si vous souhaitez voir une liste de tous les groupements d'intention, cliquez sur **Show all recommendations**.

1.  Cliquez sur une intention pour afficher l'ensemble des exemples utilisateur qui lui sont associés et effectuer l'une des actions suivantes :

    Désélectionnez les énoncés que vous ne souhaitez pas ajouter en tant qu'exemples utilisateur. 

    - Pour ajouter l'intention recommandée avec les énoncés sélectionnés en tant qu'exemples utilisateur, cliquez sur **Create intent**.
    - Pour ajouter les énoncés sélectionnés de l'intention recommandée à l'une de vos intentions existantes en tant qu'exemples utilisateur, cliquez sur **Add to existing intent**, choisissez l'intention, puis cliquez sur **Add**.

Les intentions et les exemples d'utilisateur d'intention que vous ajoutez de cette manière comptent dans votre total d'exemples d'utilisateur d'intention et d'intention pour lesquels il existe des limites pour chaque forfait. Pour plus d'informations, reportez-vous à la rubrique [Limites d'intention](/docs/services/assistant?topic=assistant-intents#intents-limits).

## Obtention de recommandations sur les exemples utilisateur d'intention
{: #beta-intent-recommendations-get-example-recommendations}

La vidéo suivante donne un aperçu de 2 minutes sur la procédure à suivre pour obtenir des recommandations sur les exemples utilisateur d'intention. 

<iframe class="embed-responsive-item" id="youtubeplayer" title="Recommandations sur les exemples utilisateur d'intention" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/L3FI8KeZfsc?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Pour obtenir des exemples utilisateur d'intentions existantes, vous pouvez utiliser les mêmes fichiers que ceux téléchargés précédemment. Il n'est pas nécessaire d'associer les exemples à des intentions. Il vous suffit de fournir les énoncés utilisateur bruts et de laisser le service choisir ceux qui conviennent à l'intention en cours. Pour chaque intention, le service analyse les mêmes données pour rechercher des exemples utilisateur appropriés pour cette intention. 

1.  Suivez les étapes décrites dans [Création d'intentions](/docs/services/assistant?topic=assistant-intents#intents-creating-intents-task) pour créer une intention.

1.  Ajoutez au moins 5 exemples utilisateur illustrant la gamme complète d'énoncés types que les utilisateurs pourraient éventuellement formuler pour déclencher cette intention. 

     Ces exemples utilisateur de base enseignent au service les types d'énoncés à rechercher dans les fichiers que vous téléchargez. 

1.  Cliquez sur **Show recommendations**.

    Les fichiers source d'exemples utilisateur sont partagés entre les compétences d'une instance de service. Si vos collègues possèdent des compétences dans la même instance et téléchargent des fichiers, leurs fichiers sont ajoutés à votre collection de *fichiers source d'exemples utilisateur* partagée.

1.  **First time only** : cliquez sur **Upload files**, puis cliquez sur **Choose a file** pour rechercher le fichier CSV que vous avez créé précédemment et sélectionnez-le.

    Si le fichier que vous avez téléchargé contient des énoncés pour tous les types d'intentions ne pose pas de problème. Le service sait sur quelle intention vous travaillez et trouve des exemples appropriés à recommander pour cette intention spécifique.  

    Une fois le fichier chargé et traité par le service, les énoncés recommandés sont affichés. Si aucune recommandation n’est formulée, le fichier ne contient pas d’exemples correspondant à cette intention.  

1.  Si le fichier ne peut fournir aucune recommandation utile pour toutes vos intentions, vous pouvez essayer un autre ensemble d'énoncés en téléchargeant un autre fichier. Pour plus d'informations, reportez-vous à la rubrique [Gestion des fichiers téléchargés](#beta-intent-recommendations-manage-files).

1.  Une fois que le service vous a présenté des recommandations, sélectionnez les énoncés que vous souhaitez ajouter comme exemples utilisateur pour cette intention, puis cliquez sur **Add**. Ou cliquez sur **Next set** pour examiner davantage d'énoncés.
1.  Si vous souhaitez rechercher vous-même des exemples utilisateur dans le fichier CSV, cliquez sur l'onglet **Search Logs**, entrez un mot clé sur lequel baser la recherche, puis appuyez sur **Entrée**.

    Suivez les instructions suivantes relatives à la syntaxe des requêtes de recherche :  

    - Les opérateurs booléens (tels que `AND` et `OR`) sont pris en charge.
    - Ajoutez du texte entre guillemets pour rechercher une correspondance exacte ("cettechainedoitexister").
    - Vous pouvez utiliser des expressions régulières, telles que `*ment` pour rechercher tous les termes se terminant par `ment`.
    - Les caractères suivants sont utilisés en tant qu’opérateurs d’expressions régulières :  

      `+ - = && || > < ! ( ) { } [ ] ^ " ~ * ? : \ /`

      Si vous souhaitez en inclure un dans un terme de recherche sans qu'il soit traité en tant qu'opérateur, vous devez le faire précéder d'une barre oblique inversée (`\`).

Les exemples utilisateur que vous ajoutez de cette manière sont comptabilisés dans votre total d'exemples utilisateur d'intention pour lesquels il existe des limites pour chaque forfait. Pour plus d'informations, reportez-vous à la rubrique [Limites d'intention](/docs/services/assistant?topic=assistant-intents#intents-limits).

## Gestion des fichiers téléchargés
{: #beta-intent-recommendations-manage-files}

Après avoir chargé au moins un fichier, vous pouvez ouvrir la collection de *fichiers source d'exemples utilisateur* pour gérer vos fichiers. Les fichiers téléchargés sont ajoutés à une collection de fichiers partagée par l'instance de service {{site.data.keyword.conversationshort}} en cours.

1.  Pour accéder à cette collection, cliquez pour ouvrir une intention, cliquez sur **Show recommendations**, puis sur **View Files** dans la barre latérale.

    Si aucune intention n'a encore été ajoutée, dans la page Intents principale, développez la section *Intent recommendations*, cliquez sur **Get recommendations**, cliquez sur **Show all recommendations**, puis sur **View Files**.

1.  Vous pouvez effectuer les actions suivantes :

    - Pour ajouter un fichier, cliquez sur **Add Files**, puis recherchez un fichier et sélectionnez-le. 
    - Pour supprimer un fichier, vous devez supprimer tous les fichiers qui ont été téléchargés dans toutes les compétences associées à cette instance de service ; vous ne pouvez pas supprimer un seul fichier. Tout d’abord, assurez-vous que personne n’utilise les fichiers, puis cliquez sur **Delete all** pour supprimer tous les fichiers téléchargés.

1.  Fermez la page des *fichiers source d'exemples utilisateur*.
