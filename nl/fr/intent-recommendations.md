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

# Obtention d'aide auprès de Watson 
{: #intent-recommendations}

Si vous disposez de transcriptions de journal de discussion, provenant par exemple d'un centre de support client, contenant des énoncés utilisateur réels, laissez le service exploiter vos données existantes à la recherche d'exemples de candidats d'utilisateur d'intention. 

## Téléchargement de vos fichiers
{: #intent-recommendations-log-files-add}

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

Tous les fichiers que vous téléchargez sont partagés entre toutes les compétences de l'instance de service actuelle. Les énoncés de tous les fichiers disponibles sont analysés lorsque vous demandez des recommandations sur les exemples utilisateur d'intention. 

## Obtention de recommandations sur les exemples utilisateur d'intention
{: #intent-recommendations-get-example-recommendations}

La vidéo suivante donne un aperçu de 2 minutes sur la procédure à suivre pour obtenir des recommandations sur les exemples utilisateur d'intention. 

<iframe class="embed-responsive-item" id="youtubeplayer" title="Recommandations sur les exemples utilisateur d'intention" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/L3FI8KeZfsc?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Il n'est pas nécessaire d'associer les exemples à des intentions dans les fichiers que vous téléchargez. Il vous suffit de fournir les énoncés utilisateur bruts et de laisser le service choisir ceux qui conviennent à l'intention en cours. Pour chaque intention, le service analyse les mêmes données pour rechercher des exemples utilisateur appropriés pour cette intention. 

1.  Suivez les étapes décrites dans [Création d'intentions](/docs/services/assistant?topic=assistant-intents#intents-creating-intents-task) pour créer une intention.

1.  Ajoutez au moins 5 exemples utilisateur illustrant la gamme complète d'énoncés types que les utilisateurs pourraient éventuellement formuler pour déclencher cette intention. 

    Ces exemples utilisateur de base enseignent au service les types d'énoncés à rechercher dans les fichiers que vous téléchargez. 

1.  Cliquez sur **Show recommendations**.

    Les fichiers source d'exemples utilisateur sont partagés entre les compétences d'une instance de service. Si vos collègues possèdent des compétences dans la même instance et téléchargent des fichiers, leurs fichiers sont ajoutés à votre collection de *fichiers source d'exemples utilisateur* partagée.

1.  **First time only** : cliquez sur **Upload files**, puis cliquez sur **Choose a file** pour rechercher le fichier CSV que vous avez créé précédemment et sélectionnez-le.

    Si le fichier que vous avez téléchargé contient des énoncés pour tous les types d'intentions ne pose pas de problème. Le service sait sur quelle intention vous travaillez et trouve des exemples appropriés à recommander pour cette intention spécifique.  

    Une fois le fichier chargé et traité par le service, les énoncés recommandés sont affichés. Si aucune recommandation n’est formulée, le fichier ne contient pas d’exemples correspondant à cette intention.  

1.  Si le fichier ne peut fournir aucune recommandation utile pour toutes vos intentions, vous pouvez essayer un autre ensemble d'énoncés en téléchargeant un autre fichier. Pour plus d'informations, reportez-vous à la rubrique [Gestion des fichiers téléchargés](#intent-recommendations-manage-files).

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
{: #intent-recommendations-manage-files}

Après avoir chargé au moins un fichier, vous pouvez ouvrir la collection de *fichiers source d'exemples utilisateur* pour gérer vos fichiers. Les fichiers téléchargés sont ajoutés à une collection de fichiers partagée par l'instance de service {{site.data.keyword.conversationshort}} en cours.

1.  Pour accéder à cette collection, cliquez pour ouvrir une intention, cliquez sur **Show recommendations**, puis sur **View Files** dans la barre latérale.

1.  Vous pouvez effectuer les actions suivantes :

    - Pour ajouter un fichier, cliquez sur **Add Files**, puis recherchez un fichier et sélectionnez-le. 
    - Pour supprimer un fichier, vous devez supprimer tous les fichiers qui ont été téléchargés dans toutes les compétences associées à cette instance de service ; vous ne pouvez pas supprimer un seul fichier. Tout d’abord, assurez-vous que personne n’utilise les fichiers, puis cliquez sur **Delete all** pour supprimer tous les fichiers téléchargés.

1.  Fermez la page des *fichiers source d'exemples utilisateur*.
