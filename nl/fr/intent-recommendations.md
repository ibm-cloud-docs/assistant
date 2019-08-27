---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-19"

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

# Obtention d'aide pour définir les intentions ![Forfait Plus ou Premium uniquement](images/plus.png)
{: #intent-recommendations}

Si vous possédez des données existantes de transcription de discussions par chat de support client, laissez Watson analyser ces données pour comprendre les besoins client auxquels votre équipe de support consacre le plus clair de son temps. Watson recommande ensuite les intentions et les exemples utilisateur que vous pouvez utiliser pour former votre assistant afin qu'il puisse reconnaître les demandes identiques et similaires à l'avenir.
{: shortdesc}

Cette fonction est disponible pour les utilisateurs du forfait Plus ou Premium.
{: note}

Les besoins des clients sont représentés dans {{site.data.keyword.conversationshort}} en tant qu'*intentions*. Si vous n'avez pas encore défini d'intention, vous pouvez commencer plus rapidement en demandant de l'aide à Watson. Téléchargez des fichiers contenant des énoncés client à partir des transcriptions du centre d'appel à faire analyser par le service {{site.data.keyword.conversationshort}}. Sur la base des informations qu’il découvre, Watson recommande un ensemble d’intentions de base que vous devez définir pour couvrir les besoins les plus courants de vos clients.  

Dans la mesure où les sujets que vos clients veulent aborder évoluent, vous pouvez utiliser la fonctionnalité de recommandations sur les exemples utilisateur d'intention pour vous aider à conserver vos intentions à jour et pertinentes dans le temps. 

La vidéo suivante donne une présentation de 3,5 minutes des recommandations d'intentions et d'exemples utilisateur d'intention. 

<iframe class="embed-responsive-item" id="youtubeplayer" title="Présentation des recommandations d'intention" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/64h59KqDY98?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Pour en savoir plus sur la façon dont les recommandations d'intention peuvent vous aider à générer un bot exploitable plus rapidement, [lisez cet article ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://medium.com/ibm-watson/let-watson-train-your-virtual-assistant-57bd53b12bc3){: new_window}.

Explorez vos données existantes pour effectuer l'une des tâches suivantes : 

- [Obtention de recommandations d'intention ](#intent-recommendations-get-intent-recommendations)
- [Obtention de recommandations d'exemples utilisateur d'intention ](#intent-recommendations-get-example-recommendations)

Pour plus d'informations sur le support de langue de cette fonction, reportez-vous à la rubrique [Langues prises en charge](/docs/services/assistant?topic=assistant-language-support).

## Création d'un fichier source exemple utilisateur
{: #intent-recommendations-log-files-add}

Pour que Watson puisse analyser vos données, vous devez fournir les données au format correct. Créez un fichier CSV avec un énoncé client par ligne. Dans l'idéal, les énoncés sont de courtes expressions extraites des transcriptions de votre centre d'appel et contenant les questions et les demandes réelles posées par les clients. Chaque fichier source exemple utilisateur peut être d'une taille maximale de 20 Mo.

Suivez ces instructions supplémentaires :

  - Supprimez toutes les données sensibles des énoncés que vous avez inclus dans le fichier.

    Les données sensibles incluent toutes les informations relatives à une personne physique identifiable, telles que les noms, les adresses électroniques, les identifiants client et les données réglementées telles que les informations de santé protégées.
  - N'incluez pas d'énoncés dépassant 1 024 caractères. Les énoncés plus longs sont tronqués.
  - Le fichier doit contenir au moins 100 énoncés ; un fichier de 500 énoncés ou plus vous donnera de meilleurs résultats.
  - Si un énoncé contient une virgule, placez-le entre guillemets.
  - Le fichier CSV doit inclure une seule colonne.
  - Supprimez tout ce qui n'est pas un énoncé généré par le client, y compris les réponses ou notes de l'agent humain.

  Par exemple :

  ```
  Qu'advient-il de mon assurance si je vends ma voiture ?
  je voudrais acheter une maison.
  Comment puis-je ajouter une personne à mon forfait ?
  "D'abord, je veux savoir si je suis déjà inscrit."
  ```

Tous les fichiers que vous téléchargez sont partagés entre toutes les compétences de l'instance de service actuelle. Les énoncés de tous les fichiers disponibles sont analysés lorsque vous demandez à la fois des recommandations d'intention et des recommandations sur les exemples utilisateur d'intention.

## Obtention de recommandations d'intention auprès de Watson
{: #intent-recommendations-get-intent-recommendations}

Laissez Watson analyser les transcriptions de vos discussions par chat avec le centre d’appel et vous recommander des intentions initiales avec lesquelles vous pourriez commencer. Si vous avez déjà créé certaines intentions, laissez Watson analyser vos journaux et comparer ses conclusions avec vos intentions existantes pour identifier les lacunes dans vos données d'apprentissage et suggérer de nouvelles intentions pour les remplir. 

Pour utiliser cette fonction, téléchargez un fichier avec des énoncés client. Watson évalue ces énoncés et identifie les problèmes courants que les clients mentionnent fréquemment. {{site.data.keyword.conversationshort}} affiche ensuite un ensemble d'intentions discrètes qui capturent les tendances des besoins des clients. Vous pouvez consulter chaque intention recommandée et les exemples d'utilisateurs correspondants pour choisir celles que vous souhaitez ajouter à vos données d'apprentissage.

## Obtention de recommandations d'intention
{: #intent-recommendations-get-intent-recommendations-task}

Avant de commencer, créez un fichier CSV avec vos données. Reportez-vous à la rubrique [Création d'un fichier source exemple utilisateur](#intent-recommendations-log-files-add).

Pour obtenir des recommandations d'intention, procédez comme suit :

1.  Ouvrez votre compétence de dialogue. La compétence s'ouvre sur la page **Intents**.

1.  Cliquez sur **Get recommendations**.

1.  **First time only** : cliquez sur **Add file**, puis cliquez sur **Choose a file** pour rechercher le fichier CSV que vous avez créé précédemment et sélectionnez-le.

    Donnez à Watson le temps d'analyser vos données et de regrouper les énoncés. 

1.  Consultez les intentions recommandées par Watson. 

    Les recommandations d'intention sont triées comme suit :

    1.  Intentions avec les énoncés les plus fréquents. La fréquence des énoncés associés à ces intentions suggère qu'ils identifient les points de douleur client les plus courants.
    1.  Différence entre les intentions et les intentions que vous avez déjà ajoutées à votre compétence. Leur caractère unique suggère qu'elles peuvent répondre aux besoins des clients qui ne sont pas satisfaits actuellement. 
    1.  Dans quelle mesure les exemples utilisateur dans l'intention sont semblables les uns aux autres, ce qui indique la force de l'intention.

    Vous pouvez survoler une intention pour voir quelques exemples des énoncés associés. Si vous souhaitez voir une liste de tous les groupements d'intention, cliquez sur **Show all recommendations**.

1.  Cliquez sur une intention pour afficher l'ensemble des exemples utilisateur qui lui sont associés et effectuer l'une des actions suivantes :

    Désélectionnez les énoncés que vous ne souhaitez pas ajouter en tant qu'exemples utilisateur.

    - Pour ajouter l'intention recommandée avec les énoncés sélectionnés en tant qu'exemples utilisateur, cliquez sur **Create intent**.
    - Pour ajouter les énoncés sélectionnés de l'intention recommandée à l'une de vos intentions existantes en tant qu'exemples utilisateur, cliquez sur **Add to existing intent**, choisissez l'intention, puis cliquez sur **Add**.

Les intentions et les exemples d'utilisateur d'intention que vous ajoutez de cette manière comptent dans votre total d'exemples d'utilisateur d'intention et d'intention pour lesquels il existe des limites pour chaque forfait. Pour plus d'informations, reportez-vous à la rubrique [Limites d'intention](/docs/services/assistant?topic=assistant-intents#intents-limits).

Dans la mesure où les sujets que vos clients veulent aborder évoluent, vous pouvez utiliser la fonctionnalité de recommandations sur les exemples utilisateur d'intention pour vous aider à conserver vos intentions à jour et pertinentes dans le temps.

## Obtention de recommandations sur les exemples utilisateur d'intention
{: #intent-recommendations-get-example-recommendations}

Les données que vous fournissez à Watson pour rechercher des exemples utilisateur d'intention ne doivent pas nécessairement associer les exemples aux intentions. Il vous suffit de fournir les énoncés client bruts et de laisser Watson choisir ceux qui conviennent à l'intention en cours. Pour chaque intention, Watson analyse les mêmes données pour rechercher des exemples utilisateur appropriés pour cette intention. 

Avant de commencer, créez un fichier CSV avec vos données. Reportez-vous à la rubrique [Création d'un fichier source exemple utilisateur](#intent-recommendations-log-files-add).

1.  Suivez les étapes décrites dans [Création d'intentions](/docs/services/assistant?topic=assistant-intents#intents-creating-intents-task) pour créer une intention.

1.  Ajoutez au moins 5 exemples utilisateur illustrant la gamme complète d'énoncés types que les clients pourraient éventuellement formuler pour déclencher cette intention. 

    Ces exemples utilisateur de base enseignent à Watson les types d'énoncés à rechercher dans les fichiers que vous téléchargez. 

1.  Cliquez sur **Show recommendations**.

    Les fichiers source d'exemples utilisateur sont partagés entre les compétences d'une instance de service. Si vos collègues possèdent des compétences dans la même instance et téléchargent des fichiers, leurs fichiers sont ajoutés à votre collection de *fichiers source d'exemples utilisateur* partagée.

1.  **First time only** : cliquez sur **Upload files**, puis cliquez sur **Choose a file** pour rechercher le fichier CSV que vous avez créé précédemment et sélectionnez-le.

    Si le fichier que vous avez téléchargé contient des énoncés pour tous les types d'intentions ne pose pas de problème. Watson sait sur quelle intention vous travaillez et trouve des exemples appropriés à recommander pour cette intention spécifique.  

    Une fois le fichier chargé et traité par Watson, les énoncés recommandés sont affichés. Si aucune recommandation n’est formulée, le fichier ne contient pas d’exemples correspondant à cette intention.

1.  Si le fichier ne peut fournir aucune recommandation utile pour toutes vos intentions, vous pouvez essayer un autre ensemble d'énoncés en téléchargeant un autre fichier. Pour plus d'informations, reportez-vous à la rubrique [Gestion des fichiers téléchargés](#intent-recommendations-manage-files).

1.  Une fois que Watson vous a présenté des recommandations, sélectionnez les énoncés que vous souhaitez ajouter comme exemples utilisateur pour cette intention, puis cliquez sur **Add**. Ou cliquez sur **Next set** pour examiner davantage d'énoncés.
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

    Si aucune intention n'a encore été ajoutée, dans la page Intents principale, développez la section *Intent recommendations*, cliquez sur **Get recommendations**, cliquez sur **Show all recommendations**, puis sur **View Files**.

1.  Vous pouvez effectuer les actions suivantes :

    - Pour ajouter un fichier, cliquez sur **Add Files**, puis recherchez un fichier et sélectionnez-le.
    - Pour supprimer un fichier, vous devez supprimer tous les fichiers qui ont été téléchargés dans toutes les compétences associées à cette instance de service ; vous ne pouvez pas supprimer un seul fichier. Tout d’abord, assurez-vous que personne n’utilise les fichiers, puis cliquez sur **Delete all** pour supprimer tous les fichiers téléchargés.

1.  Fermez la page des *fichiers source d'exemples utilisateur*.
