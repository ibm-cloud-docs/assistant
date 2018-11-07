---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-20"

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

# Notes sur l'édition

## Gestion des versions d'API du service
{: shortdesc}

Les demandes d'API nécessitent un paramètre de version qui prend une date au format `version=AAAA-MM-JJ`. Chaque fois que nous modifions l'API de manière irréversible, nous publions une nouvelle version mineure de l'API.

Envoyez le paramètre de version avec chaque demande d'API. Le service utilise la version d'API correspondant à la date que vous spécifiez ou la toute dernière version avant cette date. Ne laissez pas la date du jour par défaut. A la place, spécifiez une date correspondant à une version qui est compatible avec votre application et ne la changez pas tant que votre application n'est pas prête pour une version ultérieure.

La version actuelle est `2018-02-16`.

Le panneau "Try it out" des outils {{site.data.keyword.conversationshort}} utilise la dernière version d'API. 

## Fonctions bêta

IBM publie des services, des fonctions et un support de langue pour votre évaluation qui sont classifiés comme version bêta. Ces fonctions peuvent être instables, changer fréquemment ou être arrêtées dans un délai très court. Les fonctions bêta, qui risquent de ne pas fournir le même niveau de performance ou de compatibilité que les fonctions de l'édition GA (General Availability), ne sont pas destinées à être utilisées dans un environnement de production. Les fonctions bêta ne sont prises en charge que sur le site [developerWorks Answers ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/answers/topics/watson-conversation/){: new_window}.

## Modèles mis à jour
{: #updated-models}

Les algorithmes {{site.data.keyword.conversationshort}} peuvent être régulièrement affinés et mis à jour en fonction des commentaires en retour, des améliorations scientifiques et d'autres facteurs, afin d'améliorer sans cesse les performances. Les mises à jour apportées au modèle seront communiquées dans ces notes sur l'édition.

Les modèles existants ayant été formés ne seront pas immédiatement impactés, mais les modèles arrivés à expiration seront mis à jour vers le modèle en cours 60 jours après la mise à disposition d'un nouveau modèle, si vous n'avez pas encore effectué cette mise à jour.

> **Remarque :** cette instruction de mise à jour s'applique uniquement aux langues et aux fonctions officiellement disponibles.

## Modifications
{: #change-log}

Les nouvelles fonctions et modifications apportées au service et décrites ci-après sont disponibles.

### 16 février 2018
{: #16February2018}

- **Traçage de noeud de dialogue** : lorsque vous utilisez le panneau "Try it out" pour tester un dialogue, une icône d'emplacement s'affiche en regard de chaque réponse. Vous pouvez cliquer sur l'icône dans le but de mettre en évidence le chemin parcouru par le service dans l'arborescence de dialogue pour arriver à la réponse. Pour plus d'informations, voir [Création d'un dialogue](dialog-build.html#test). 

- **Nouvelle version d'API** : la version d'API actuelle est désormais `2018-02-16`. Cette version comprend les modifications suivantes :

  - Un nouveau paramètre `include_audit` est désormais pris en charge sur la plupart des demandes GET. Il s'agit d'un paramètre booléen facultatif qui indique si la réponse doit inclure les propriétés d'audit (horodatages `created` et `updated`). La valeur par défaut est
`false`. (Si vous utilisez une version d'API antérieure à `2018-02-16`, la valeur par défaut est `true`.) Pour plus d'informations, reportez-vous à la documentation [API Reference ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.

  - Les réponses des appels d'API qui utilisent la nouvelle version incluent uniquement les propriétés ayant des valeurs non `null`. 

  - Les propriétés `output.nodes_visited` et `output.nodes_visited_details` des réponses de message incluent désormais des noeuds des types suivants, lesquels étaient auparavant omis : 

    - Noeuds avec `type`=`response_condition`
    - Noeuds avec `type`=`event_handler` et `event_name`=`input`

### 9 février 2018
{: #9February2018}

- **Entités de système néerlandaises (Bêta)** : le support de langue néerlandaise a été amélioré pour inclure la disponibilité des [entités de système](system-entities.html) dans une version bêta. Pour plus d'informations, reportez-vous à la rubrique [Langues prises en charge](lang-support.html). 

### 29 janvier 2018
{: #29January2018}

- L'API REST {{site.data.keyword.conversationshort}} prend désormais en charge de nouveaux paramètres de demande :
  - Utilisez le paramètre `append` lorsque vous mettez à jour un espace de travail pour indiquer si les nouvelles données d'espace de travail ne doivent pas écraser les données existantes mais y être ajoutées. Pour plus d'informations, reportez-vous à la rubrique [Update workspace ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#update_workspace){: new_window}.
  - Utilisez le paramètre `nodes_visited_details` lorsque vous envoyez un message pour indiquer si la réponse doit inclure d'autres informations de diagnostic sur les noeuds ayant été visités durant le traitement du message. Pour plus d'informations, reportez-vous à la rubrique [Send message ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#send_message){: new_window}.

### 23 janvier 2018
{: #23January2018}

- **Impossible d'extraire une liste d'espaces de travail** : si ce message ou des messages d'erreur similaires s'affichent alors que vous utilisez les outils, il se peut que votre session ait expiré. Déconnectez-vous en choisissant **Log out** à partir de l'icône d'**informations utilisateur** ![Menu de l'icône d'informations utilisateur](images/user-icon.png), puis reconnectez-vous. 

### 8 décembre 2017
{: #8December2017}

- **Autoriser l'accès aux données de journal dans les instances (utilisateurs Premium uniquement)** : si vous êtes un utilisateur {{site.data.keyword.conversationshort}} Premium, vos instances premium peuvent éventuellement être configurées pour autoriser l'accès aux données de journal à partir des espaces de travail qu'elles comportent. 

- **Copier des noeuds** : vous pouvez désormais dupliquer un noeud afin de réaliser une copie de ce dernier et de ses enfants. Cette fonction est utile si vous créez un noeud avec une logique utile que vous souhaitez réutiliser ailleurs dans votre dialogue. Pour plus d'informations, reportez-vous à la rubrique [Copie d'un noeud de dialogue](dialog-build.html#copy-node). 

- **Capturer des groupes dans des entités de canevas** : vous pouvez identifier des groupes dans le canevas d'expression régulière que vous définissez pour une entité. Identifier des groupes est utile si vous souhaitez pouvoir vous référer ultérieurement à une sous-section du canevas. Par exemple, votre entité peut avoir un canevas d'expression régulière qui capture des numéros de téléphone américains. Si vous identifiez le segment d'indicatif régional du canevas de numéro en tant que groupe, vous pouvez ensuite faire référence à ce groupe pour accéder uniquement au segment d'indicatif régional d'un numéro de téléphone. Pour plus d'informations, reportez-vous à la rubrique[Définition d'entités](entities.html#creating-entities). 

### 6 décembre 2017
{: #6December2017}

- **Intégration {{site.data.keyword.openwhisk}} (Bêta)** : appelez les actions {{site.data.keyword.openwhisk}} (anciennement IBM OpenWhisk) directement à partir d'un noeud de dialogue. Cette fonction vous permet, par exemple, d'appeler une action pour extraire des informations météorologiques à partir d'un noeud de dialogue, puis définir une condition sur les informations renvoyées dans la réponse de dialogue. Actuellement, vous pouvez appeler une action à partir d'une instance {{site.data.keyword.openwhisk_short}} qui est hébergée dans la région Sud des Etats-Unis à partir des instances {{site.data.keyword.conversationshort}} qui sont hébergées dans la région Sud des Etats-Unis. Pour plus d'informations, reportez-vous à la rubrique [Procédure permettant de passer des appels de programmation à partir d'un noeud de dialogue](dialog-actions.html). 

### 5 décembre 2017
{: #5December2017}

- **Interface utilisateur repensée pour les intentions et les entités** : les onglets `Intents` et `Entities` ont été repensés de manière à fournir un flux de travail plus efficace et plus facile lors de la création et de l'édition d'entités et d'intentions. Pour plus d'informations sur l'utilisation de ces onglets, reportez-vous aux rubriques [Définition d'intentions](intents.html#defining-intents) et [Définition d'entités](entities.html#defining-entities). 

### 30 novembre 2017
{: #30November2017}

- **Support des chiffres arabes orientaux** : les chiffres arabes orientaux sont désormais pris en charge dans les entités de système arabes. 

### 29 novembre 2017
{: #29November2017}

- **Amélioration de la compréhension des entrées utilisateur dans les espaces de travail** : vous pouvez désormais améliorer un espace de travail avec des énoncés qui ont été envoyés à d'autres espaces de travail au sein de votre instance. Par exemple, si vous possédez plusieurs versions d'espaces de travail de production et d'espaces de travail de développement, vous pouvez utiliser les mêmes données d'énoncé pour améliorer n'importe lequel de ces espaces de travail. Pour plus d'informations, reportez-vous aux rubriques [Amélioration des espaces de travail](logs.html#deploy_id) et [Sélection d'une source de données](logs_convo.html#select-source). 

### 20 novembre 2017
{: #20November2017}

- **Conformité à la norme GB18030** : GB18030 est une norme chinoise qui spécifie une page de codes étendue destinée au marché chinois. Cette norme de page de codes est importante pour l'industrie des logiciels car le China National Information Technology Standardization Technical Committee exige que toute application logicielle commercialisée sur le marché chinois après le 1er septembre 2001 respecte la norme GB18030. Le service {{site.data.keyword.conversationshort}} prend en charge ce codage et est certifié conforme à la norme GB18030. 

### 9 novembre 2017
{: #9November2017}

- **Les exemples d'intention peuvent référencer directement des entités** : vous pouvez désormais spécifier une référence d'entité directement dans un exemple d'intention. Cette référence d'entité, ainsi que l'ensemble de ses valeurs ou de ses synonymes, est utilisée par le classifieur de service {{site.data.keyword.conversationshort}} pour l'entraînement de l'intention. Pour plus d'informations, reportez-vous à la section [*Référencement direct d'une entité @Entity en tant qu'exemple d'intention*](intents.html#entity-as-example) dans la rubrique [Intentions](intents.html). 

  **Remarque** : actuellement, vous pouvez uniquement référencer directement les entités fermées que vous définissez. Vous ne pouvez pas référencer directement les [entités de canevas](entities.html#pattern-entities) ou les [entités de système](system-entities.html).

### 8 novembre 2017
{: #8November2017}

- **Connecteur {{site.data.keyword.conversationshort}}** : vous pouvez utiliser le nouveau connecteur{{site.data.keyword.conversationshort}} pour connecter votre espace de travail à une application Slack ou Facebook Messenger que vous possédez, en la rendant disponible en tant qu'agent conversationnel avec lequel les utilisateurs Slack ou Facebook Messenger peuvent interagir. Cette option est disponible uniquement pour la région {{site.data.keyword.Bluemix_notm}} du Sud des Etats-Unis. Pour plus d'informations, reportez-vous à la rubrique [Déploiement sur un canal avec le connecteur {{site.data.keyword.conversationshort}}](conversation-connector.html).

### 3 novembre 2017
{: #3November2017}

- **Mises à jour de dialogue** : les mises à jour suivantes vous facilitent la création d'un dialogue. (Pour plus d'informations, reportez-vous à la rubrique [Création d'un dialogue](dialog-build.html).) 

    - Vous pouvez ajouter une condition à un attribut afin de rendre ce dernier obligatoire uniquement dans certaines conditions. Par exemple, vous pouvez créer un attribut qui demande le nom d'une épouse uniquement si un attribut (obligatoire) précédent sollicitant des informations sur l'état civil indique que l'utilisateur est marié. 

    - Vous pouvez désormais choisir **Skip user input** comme étape suivante pour un noeud. Lorsque vous choisissez cette option, une fois que le noeud en cours est traité, le service passe directement au premier noeud enfant du noeud en cours. Cette option est semblable à l'option de l'étape suivante *Jump to* existante, à ceci près qu'elle offre davantage de souplesse. Vous n'avez pas besoin de spécifier le noeud exact auquel passer directement. Lors de l'exécution, le service passe toujours directement au noeud qui est le premier noeud enfant, quel qu'il soit, même si les noeuds enfant sont réorganisés ou que de nouveaux noeuds sont ajoutés une fois le comportement d'étape suivant défini. 

    - Vous pouvez ajouter des réponses conditionnelles pour des attributs. Pour les réponses Found et Not found, vous pouvez personnaliser la façon dont le service répond selon que certaines conditions sont remplies ou pas. Cette fonction vous permet de rechercher d'éventuelles interprétations erronées et de les corriger avant de sauvegarder la valeur fournie par l'utilisateur dans la variable contextuelle de l'attribut. Par exemple, si l'attribut sauvegarde l'âge de l'utilisateur et utilise `@sys-number` dans la zone *Check for* pour le capturer, vous pouvez ajouter une condition qui recherche les nombres au-dessus de 100 et fournit une réponse semblable à *Please provide a valid age in years.* Pour plus d'informations, reportez-vous à la rubrique [Ajout de conditions aux réponses Found et Not found](dialog-slots.html#slot-handler-next-steps). 

    - L'interface que vous utilisez pour ajouter des réponses conditionnelles à un noeud a été repensée afin d'afficher plus facilement chacune des conditions et la réponse associée. Pour ajouter des réponses conditionnelles de niveau noeud, cliquez sur **Customize**, puis activez l'option **Multiple responses**. 

     **Remarque** : l'option à bascule **Multiple responses** active ou désactive la fonction pour la réponse de niveau noeud uniquement. Elle ne contrôle pas la possibilité de définir des réponses conditionnelles pour un attribut. Le paramétrage de plusieurs réponses pour un attribut est contrôlé séparément. 

    - Afin de conserver la page sur laquelle vous éditez un attribut simple, vous devez désormais sélectionner des options de menu pour a.) ajouter une condition qui doit être remplie pour que l'attribut soit traité, et b.) ajouter des réponses conditionnelles pour les conditions Found et Not found pour un attribut. Sauf si vous choisissez d'ajouter cette fonctionnalité supplémentaire, les zones de condition d'attribut et de réponses multiples ne s'affichent pas, ce qui désencombre la page et la rend plus facile à utiliser. 

### 25 octobre 2017
{: #25October2017}

- **Mises à jour apportées au chinois simplifié** : le support de langue a été amélioré pour le chinois simplifié. Cela inclut des améliorations au niveau de la classification des intentions à l'aide d'imbrications de mots au niveau des caractères, et la disponibilité des entités de système. Notez que les modèles d'apprentissage du service {{site.data.keyword.conversationshort}} ont peut-être été mis à jour dans le cadre de cette amélioration, par conséquent, lorsque vous formerez à nouveau votre modèle, les modifications seront appliquées. Pour plus d'informations, reportez-vous à la section [Modèles mis à jour](release-notes.html#updated-models).
- **Mises à jour apportées à l'espagnol** : des améliorations ont été apportées à la classification des intentions en espagnol, pour de très grands fichiers.

### 11 octobre 2017
{: #11October2017}

- **Mises à jour apportées au coréen** : le support de langue a été amélioré pour le coréen. Notez que les modèles d'apprentissage du service {{site.data.keyword.conversationshort}} ont peut-être été mis à jour dans le cadre de cette amélioration, par conséquent, lorsque vous formerez à nouveau votre modèle, les modifications seront appliquées. Pour plus d'informations, reportez-vous à la section [Modèles mis à jour](release-notes.html#updated-models).

### 3 octobre 2017
{: #3October2017}

- **Entités basées sur des canevas (bêta)** : vous pouvez désormais définir des canevas spécifiques pour une entité, en utilisant des expressions régulières. Cela peut vous permettre d'identifier plus facilement les entités qui suivent un canevas défini, par exemple, une référence SKU ou des numéros de pièces détachées, des numéros de téléphone ou des adresses électroniques. Pour plus d'informations, reportez-vous à la section sur les [entités basées sur des canevas](entities.html#pattern-entities).
    - Vous pouvez ajouter des synonymes ou des canevas pour une valeur d'entité, mais pas les deux.
    - Chaque valeur d'entité peut être associée à 5 canevas au maximum.
    - Chaque canevas (expression régulière) est limité à 128 caractères.
    - Les canevas ne sont actuellement pas pris en charge pour l'importation ou l'exportation d'un fichier CSV.
    - L'API REST ne prend pas en charge l'accès direct aux canevas, mais vous pouvez extraire ou modifier des canevas à l'aide du point final `/values`.

- **Fonction Fuzzy matching filtrée par dictionnaire (anglais uniquement)** : une version améliorée de la fonction Fuzzy Matching pour les entités est désormais disponible, en anglais. Cette amélioration empêche la capture de certains mots anglais valides courants comme correspondances floues pour une entité donnée. Par exemple, la fonction Fuzzy Matching ne fera pas correspondre la valeur d'entité `like` à `hike` ou `bike`, qui sont des mots anglais valides, mais continuera d'établir des correspondances avec des exemples, tels que `lkie` ou `oike`.

### 27 septembre 2017
{: #27September2017}

- **Mises à jour apportées au générateur de condition** : la commande qui vous permet de définir une condition dans un noeud de dialogue a été améliorée. Par exemple, désormais, les noms de variable contextuelle disponibles s'affichent après que le caractère $ est saisi pour commencer à ajouter une variable contextuelle.

### 31 août 2017
{: #31August2017}

- **Rétromigration de la section Improve** : la mesure de temps de conversation médiane, et les filtres correspondants, ont été temporairement retirés de la page Overview dans la section Improve. Ce retrait a pour but d'empêcher que le calcul de certaines mesures n'entraîne l'affichage d'informations inexactes dans la mesure de temps de conversion médiane et le graphique qui représente les conversations sur la durée. IBM déplore d'avoir eu à retirer cette fonctionnalité de l'outil mais elle se doit de faire en sorte que les informations fournies aux utilisateurs soient exactes.
- **Noms de noeud de dialogue** : vous pouvez désormais affecter n'importe quel nom à un noeud de dialogue ; il n'a pas besoin d'être unique. Et vous pouvez modifier le nom de noeud ultérieurement sans que cela n'impacte la façon dont le noeud est référencé en interne. Le nom que vous indiquez est sauvegardé en tant qu'attribut de vignette du noeud dans le fichier JSON de l'espace de travail et le système utilise un ID unique qui est stocké dans l'attribut de nom pour faire référence à ce noeud.

### 23 août 2017
{: #23August2017}

- **Mises à jour apportées au coréen, au japonais et à l'italien** : le support de langue a été amélioré pour le coréen, le japonais et l'italien. Notez que les modèles d'apprentissage du service {{site.data.keyword.conversationshort}} ont peut-être été mis à jour dans le cadre de cette amélioration, par conséquent, lorsque vous formerez à nouveau votre modèle, les modifications seront appliquées. Pour plus d'informations, reportez-vous à la section [Modèles mis à jour](release-notes.html#updated-models).

### 10 août 2017
{: #10August2017}

- **Normalisation des accents** : dans un paramétrage conversationnel, les utilisateurs peuvent ou non utiliser des accents lors de l'interaction avec le service {{site.data.keyword.conversationshort}}. Ainsi, une mise à jour a été apportée à l'algorithme de sorte que les versions accentuées et non accentuées des mots puissent être interprétées de la même manière pour la détection d'intention et la reconnaissance d'entité.

  Toutefois, dans certaines langues, telles que l'espagnol, certains accents peuvent modifier le sens de l'entité. Par conséquent, pour la détection d'entité, même si l'entité d'origine comporte implicitement un accent, le service peut aussi établir une correspondance avec la version non accentuée de la même entité, mais avec une cote de confiance légèrement inférieure.

  Par exemple, avec le mot `barrió`, ui est accentué et correspond au participe passé du verbe `barrer` (balayer, en français), le service peut également établir une correspondance avec le mot `barrio` (quartier, en français), mais avec une cote de confiance légèrement inférieure. 

  Le système fournit la cote de confiance la plus élevée dans les entités pour lesquelles des correspondances exactes sont trouvées. Par exemple, `barrio` ne sera pas détecté si `barrió` figure dans l'ensemble d'entraînement et `barrió` ne sera pas détecté si `barrio` figure dans l'ensemble d'entraînement.

  Vous êtes supposé entraîner le système à reconnaître les mots avec les caractères et les accents appropriés. Par exemple, si vous vous attendez à recevoir la réponse `barrió`, vous devez ajouter `barrió` dans l'ensemble d'entraînement.

  Bien qu'il ne s'agisse pas d'accents, la même règle s'applique aux mots qui comportent, par exemple, la lettre espagnole `ñ` et non la lettre `n`, par exemple, `uña` et `una`. En l'occurrence, la lettre `ñ` n'est pas juste une lettre `n` dotée d'un accent, mais réellement une lettre spécifique de l'alphabet espagnol.

  Vous pouvez activer la fonction Fuzzy Matching si vous pensez que vos clients n'utiliseront pas les accents de manière appropriée ou feront des fautes d'orthographe (par exemple, en tapant un `n` au lieu d'un `ñ`), ou vous pouvez les exclure explicitement des exemples d'entraînement.

  **Remarque :** la normalisation des accents est activée pour le portugais, l'espagnol, le français et le tchèque.

- **Indicateur d'exclusion pour les espaces de travail** : l'API REST {{site.data.keyword.conversationshort}} prend désormais en charge un indicateur d'exclusion pour les espaces de travail. Cet indicateur spécifie que des données d'apprentissage d'espace de travail, telles que des intentions et des entités, ne doivent pas être utilisées par IBM pour les améliorations générales du service. Pour plus d'informations, reportez-vous à la documentation [API Reference ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#data-collection){: new_window}

### 7 août 2017
{: #7August2017}

- **Interprétation des dates `Next` et `last`** : le service {{site.data.keyword.conversationshort}} interprète les dates `last` et `next` comme renvoyant au jour suivant ou au jour précédent référencé le plus proche, ce qui peut correspondre à la même semaine ou à une semaine précédente. Pour plus d'informations, reportez-vous à la rubrique sur les [entités de système](system-entities.html#sys-datetime).

### 3 août 2017
{: #3August2017}

- **Fonction Fuzzy Matching pour d'autres langues (bêta)** : la fonction Fuzzy Matching pour les entités est désormais disponible pour d'autres langues, comme indiqué dans la rubrique [Langues prises en charge](lang-support.html). 
- **Fonction Partial Matching (bêta - anglais uniquement)** : la fonction Fuzzy Matching suggère automatiquement des synonymes de type sous-chaîne présents dans les entités définies par l'utilisateur et affecte une cote de confiance inférieure par rapport à la correspondance d'entité exacte. Pour plus d'informations, reportez-vous à la section sur la [fonction Fuzzy matching](entities.html#fuzzy-matching).

### 28 juillet 2017
{: #28July2017}

- Lorsque vous définissez des préférences bidirectionnelles pour l'outil, vous pouvez désormais spécifier le sens de l'interface graphique.
- Le schéma de couleurs de l'outil a été mis à jour pour être cohérent avec les autres services et produits Watson.

### 19 juillet 2017
{: #19July2017}

- L'API REST {{site.data.keyword.conversationshort}} prend désormais en charge l'accès aux noeuds de dialogue. Pour plus d'informations, reportez-vous à la documentation [API Reference ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/watson/developercloud/conversation/api/v1/#dialog_nodes){: new_window}.

### 14 juillet 2017
{: #14July2017}

- La fonctionnalité des attributs de dialogue a été améliorée. Par exemple, une propriété nommée *slot_in_focus* a été ajoutée afin de vous permettre de définir une condition qui s'applique à un attribut seulement. Pour plus d'informations, reportez-vous à la section [Collecte d'informations à l'aide d'attributs](/docs/services/conversation/dialog-slots.html).

### 12 juillet 2017
{: #12July2017}

- **Prise en charge du tchèque** : le support de la langue tchèque est désormais disponible. Pour plus d'informations, reportez-vous à la rubrique [Langues prises en charge](lang-support.html). 

### 11 juillet 2017
{: #11July2017}

- **Option Test in Slack** : vous pouvez utiliser la nouvelle option **Test in Slack** pour déployer rapidement votre espace de travail en tant qu'utilisateur bot Slack à des fins de test. Cette option est disponible uniquement pour la région {{site.data.keyword.Bluemix_notm}} du Sud des Etats-Unis. Pour plus d'informations, reportez-vous à la rubrique [Utilisation de l'option Test in Slack](test-deploy.html).
- **Mises à jour apportées à l'arabe** : le support de la langue arabe a été amélioré et comprend désormais l'option d'évaluation absolue pour chaque intention (Absolute scoring) ainsi que l'option permettant de marquer des intentions comme non pertinentes (Mark as irrelevant). Pour plus d'informations, reportez-vous à la rubrique [Langues prises en charge](lang-support.html). Notez que les modèles d'apprentissage du service {{site.data.keyword.conversationshort}} ont peut-être été mis à jour dans le cadre de cette amélioration, par conséquent, lorsque vous formerez à nouveau votre modèle, les modifications seront appliquées. Pour plus d'informations, reportez-vous à la section [Modèles mis à jour](release-notes.html#updated-models).

### 23 juin 2017
{: #23June2017}

- **Mises à jour apportées au coréen** : le support de la langue coréenne a été amélioré. Pour plus d'informations, reportez-vous à la rubrique [Langues prises en charge](lang-support.html). Notez que les modèles d'apprentissage du service {{site.data.keyword.conversationshort}} ont peut-être été mis à jour dans le cadre de cette amélioration, par conséquent, lorsque vous formerez à nouveau votre modèle, les modifications seront appliquées. Pour plus d'informations, reportez-vous à la section [Modèles mis à jour](release-notes.html#updated-models).

### 22 juin 2017
{: #22June2017}

- **Lancement des attributs** : la collecte de plusieurs éléments d'informations auprès d'un utilisateur dans un seul noeud est désormais facilitée par l'ajout d'attributs. Auparavant, vous deviez créer plusieurs noeuds de dialogue pour couvrir toutes les combinaisons possibles de spécification d'informations par les utilisateurs. Grâce aux attributs, vous pouvez configurer un seul noeud qui sauvegarde les informations fournies par l'utilisateur et invite ce dernier à fournir les informations manquantes. Pour plus d'informations, reportez-vous à la section [Collecte d'informations à l'aide d'attributs](dialog-slots.html).
- **Arborescence de dialogue simplifiée** : l'arborescence de dialogue a été repensée afin de la rendre plus facile à utiliser. La vue de l'arborescence est plus compacte, ce qui vous permet de voir plus facilement où vous vous trouvez dans cette arborescence. Et, les liens entre les noeuds sont représentés de telle manière que les relations entre les noeuds sont plus faciles à comprendre.

### 21 juin 2017
{: #21June2017}

- **Prise en charge de l'arabe** : le support de la langue arabe est désormais officiellement disponible. Pour plus d'informations, reportez-vous à la section [Configuration des langues bidirectionnelles](lang-support.html#configuring-bi-directional).
- **Mises à jour apportées à toutes les langues** : les algorithmes du service {{site.data.keyword.conversationshort}} ont été mis à jour de manière à améliorer l'ensemble du support de langue. Pour plus d'informations, reportez-vous à la rubrique [Langues prises en charge](lang-support.html).

### 16 juin 2017
{: #16June2017}

- **Page Recommendations (Bêta - utilisateur Premium uniquement)** : le panneau Improve inclut également une page **Recommendations** qui vous propose des méthodes pour améliorer votre système en analysant les conversations entre les utilisateurs et votre agent conversationnel et en tenant compte des données d'apprentissage et du degré de certitude des réponses actuels de votre système. 

### 14 juin 2017
{: #14June2017}

- **Fonction Fuzzy Matching pour d'autres langues (bêta)** : la fonction Fuzzy Matching pour les entités est désormais disponible pour d'autres langues, comme indiqué dans la rubrique [Langues prises en charge](lang-support.html). Vous pouvez activer la fonction Fuzzy Matching par entité afin d'améliorer la capacité du service à reconnaître les termes dans les entrées utilisateur dont la syntaxe est similaire à l'entité, mais pas forcément identique. La fonction est capable de mapper une entrée utilisateur à l'entité correspondante en dépit de la présence de fautes d'orthographe ou de légères différences de syntaxe. Par exemple, si vous définissez giraffe comme synonyme d'une entité d'animal et que l'entrée utilisateur contient les termes giraffes ou girafe, la fonction Fuzzy Matching est capable de mapper correctement le terme à l'entité d'animal. Pour plus d'informations, reportez-vous à la section sur la [fonction Fuzzy matching](entities.html#fuzzy-matching).

### 13 juin 2017
{: #13June2017}

- **Page User conversations** : le panneau Improve comprend désormais une page intitulée **User conversations**, qui fournit une liste d'interactions utilisateur avec votre agent conversationnel pouvant être filtrées par mot clé, intention, entité ou nombre de jours. Vous pouvez ouvrir des conversations individuelles pour corriger des intentions ou pour ajouter des valeurs ou des synonymes d'entité.
- **Modification relative aux expressions régulières** : les expressions régulières qui sont prises en charge par des fonctions SpEL, telles que find, matches, extract, replaceFirst, replaceAll et split, ont été modifiées. Un groupe de constructions d'expressions régulières n'est plus autorisé, notamment, les constructions d'assertion avant, d'assertion arrière, de répétition possessive et de référence arrière. Cette modification a pour but d'éviter un risque lié à la sécurité dans la bibliothèque d'expression régulière d'origine.

### 12 juin 2017
{: #12June2017}

- Le nombre maximal d'espaces de travail que vous pouvez créer à l'aide du plan **Lite** (anciennement appelé forfait gratuit) est passé de 3 à 5.
- Vous pouvez désormais affecter n'importe quel nom à un noeud de dialogue ; il n'a pas besoin d'être unique. Et vous pouvez modifier le nom de noeud ultérieurement sans que cela n'impacte la façon dont le noeud est référencé en interne. Le nom que vous spécifiez est interprété comme un alias et le système utilise son propre identificateur interne pour faire référence au noeud.
- Vous ne pouvez plus modifier la langue d'un espace de travail après la création de celui-ci en éditant ses caractéristiques. Si vous devez modifier la langue, vous pouvez exporter l'espace de travail en tant que fichier JSON, mettre à jour la propriété de langue, puis importer le fichier JSON en tant que nouvel espace de travail.

### 6 juin 2017
{: #6June2017}

- **A propos du service** : une nouvelle page intitulée *Learn about {{site.data.keyword.watson}} {{site.data.keyword.conversationshort}}* est disponible. Elle fournit des informations d'initiation et des liens vers la documentation du service et d'autres ressources utiles. Pour ouvrir la page, cliquez sur l'icône ![i pour informations.](images/info.png) dans l'en-tête de page.
- **Exportation et suppression en bloc** : vous pouvez désormais exporter simultanément plusieurs intentions ou entités vers un fichier CSV, afin de pouvoir les importer et les réutiliser pour une autre application {{site.data.keyword.conversationshort}}. Vous pouvez également sélectionner simultanément plusieurs entités ou intentions pour une suppression en bloc.
- **Mises à jour apportées au coréen** : les marqueurs sémantiques coréens ont été mis à jour pour assurer le support du langage familier. IBM continue de s'employer à améliorer la reconnaissance et la classification d'entités.
- **Prise en charge d'émoticônes** : les émoticônes ajoutées aux exemples d'intention ou en tant que valeurs d'entité seront désormais correctement classifiées/extraites. **Remarque** : seules les émoticônes qui sont incluses dans vos données d'apprentissage seront correctement et systématiquement identifiées ; il se peut que des émoticônes similaires ayant des teintes de couleur différentes ou présentant d'autres variations ne soient pas correctement classifiées.
- **Fonction Stemming pour les entités (bêta - anglais uniquement)** : la fonction bêta Fuzzy Matching reconnaît des entités et établit des correspondances en fonction du format de réduction au radical de la valeur d'entité. Par exemple, cette fonction reconnaît correctement 'bananas' comme étant similaire à 'banana' et 'run' comme étant similaire à 'running' car ils ont le même radical. Pour plus d'informations, reportez-vous à la section sur la [fonction Fuzzy Matching](entities.html#fuzzy-matching).
- **Progression de l'importation d'espace de travail** : lorsque vous importez un espace de travail à partir d'un fichier JSON, une vignette représentant l'espace de travail et contenant des informations sur la progression de l'importation s'affiche immédiatement.
- **Temps d'entraînement réduit** : plusieurs modèles sont désormais formés en parallèle, ce qui réduit considérablement le temps d'entraînement pour les grands espaces de travail. 

### 26 mai 2017
{: #26May2017}

- La version d'API actuelle est désormais `2017-05-26`. Cette version comprend les modifications suivantes :
    - Le schéma des objets ErrorResponse a changé. Cette modification a un impact sur tous les noeuds finaux et sur toutes les méthodes. Pour plus d'informations, reportez-vous à la documentation [API Reference ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.
    - Le schéma interne utilisé pour représenter les noeuds de dialogue dans le fichier JSON d'un espace de travail exporté a changé. Si vous utilisez l'API `2017-05-26` pour importer un espace de travail qui a été exporté à l'aide d'une version antérieure, l'importation de certains noeuds de dialogue peut ne pas aboutir. Pour optimiser vos résultats, importez toujours un espace de travail en utilisant la même version que celle qui a servi à l'exportation de cet espace de travail.

### 25 mai 2017
{: #25May2017}

- Vous pouvez désormais gérer des variables contextuelles dans le panneau "Try it out". Cliquez sur le lien **Manage context** pour ouvrir un nouveau panneau dans lequel vous pouvez définir et vérifier les valeurs des variables contextuelles lorsque vous testez le dialogue. Pour plus d'informations, reportez-vous à la section [Test de votre dialogue](dialog-test.html).

### 16 mai 2017
{: #16May2017}

- Un exemple d'espace de travail de tableau de bord de voiture, intitulé **Car Dashboard**, est désormais disponible lorsque vous ouvrez l'outil. Pour utiliser cet exemple comme point de départ pour votre propre espace de travail, éditez-le. En revanche, si vous souhaitez l'utiliser pour plusieurs espaces de travail, dupliquez-le. L'exemple d'espace de travail n'est pas pris en compte dans le nombre limite d'espaces de travail tant que vous ne l'utilisez pas.
- Il est désormais plus facile de naviguer dans l'outil. Les options du menu de navigation sont disponibles sur le côté et non plus dans la partie supérieure de la page principale. La partie supérieure de la page contient des liens d'élément de navigation qui vous indiquent où vous vous trouvez. Vous pouvez désormais basculer entre les instances de service à partir de la page Workspaces. Pour vous y rendre rapidement, cliquez sur **Back to workspaces** dans le menu de navigation. Si vous avez plusieurs instances de service, le nom de l'instance en cours est affiché. Vous pouvez cliquer sur le lien **Change** à côté de ce nom pour choisir une autre instance.
- Lorsque vous créez un dialogue, celui-ci comporte désormais deux noeuds qui ont été ajoutés pour vous : 1) le noeud **Welcome** en haut de l'arborescence du dialogue qui contient le message d'accueil à afficher pour l'utilisateur et 2) le noeud **Anything else** au bas de l'arborescence qui intercepte et répond à toutes les demandes utilisateur qui ne sont pas reconnues par les autres noeuds du dialogue. Pour plus d'informations, reportez-vous à la rubrique [Création d'un dialogue](dialog-create.html).
- Lorsque vous testez un dialogue dans le panneau "Try it out", vous pouvez désormais rechercher et soumettre à nouveau un énoncé de test récent en appuyant sur la touche de déplacement vers le haut pour faire défiler les entrées précédentes.
- Le support expérimental de 5 entités de système (`@sys-date`, `@sys-time`, `@sys-currency`, `@sys-number`, `@sys-percentage`) pour la langue coréenne est désormais disponible. Il existe des problèmes connus pour certaines des entités numériques, et un support limité pour les entrées en langage familier.
- Une page Overview est disponible sur l'onglet Improve. La page fournit un récapitulatif des interactions avec votre bot. Vous pouvez visualiser le volume de trafic pour une période donnée, ainsi que les intentions et les entités qui ont été le plus souvent reconnues dans des conversations utilisateur. Pour plus d'informations, reportez-vous à la rubrique [Utilisation de la page Overview](logs_oview.html).

### 27 avril 2017
{: #27April2017}

- Les entités de système suivantes sont désormais disponibles en tant que fonctions bêta en anglais uniquement :
    - sys-location : reconnaît des références à des lieux, tels que des villes, des agglomérations et des pays, dans des énoncés d'utilisateur.
    - sys-person : reconnaît des références à des noms de personne (nom de famille et prénom) dans des énoncés d'utilisateur.

    Pour plus d'informations, reportez-vous à la rubrique de référence sur les [entités de système](system-entities.html).
- La fonction Fuzzy Matching pour les entités est une fonction bêta qui est désormais disponible en anglais. Vous pouvez activer la fonction Fuzzy Matching par entité afin d'améliorer la capacité du service à reconnaître les termes dans les entrées utilisateur dont la syntaxe est similaire à l'entité, mais pas forcément identique. La fonction est capable de mapper une entrée utilisateur à l'entité correspondante en dépit de la présence de fautes d'orthographe ou de légères différences de syntaxe. Par exemple, si vous définissez **giraffe** comme synonyme d'une entité d'animal et que l'entrée utilisateur contient les termes *giraffes* ou *girafe*, la fonction Fuzzy Matching est capable de mapper correctement le terme à l'entité d'animal. Pour plus d'informations, reportez-vous à la section portant sur la fonction `Fuzzy Matching` dans la rubrique [Définition d'entités](entities.html#fuzzy-matching). 

### 18 avril 2017
{: #18April2017}

- L'API REST {{site.data.keyword.conversationshort}} prend désormais en charge l'accès aux ressources suivantes :
    - entités
    - valeurs d'entité
    - synonymes de valeur d'entité
    - journaux

    Pour plus d'informations, reportez-vous à la documentation [API Reference ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.
- Le comportement de la méthode `POST` /messages a modifié le traitement des entités et des intentions spécifiées dans le cadre de l'entrée du message :
    - Si vous spécifiez des intentions dans une entrée, le service utilise ces intentions, mais il fait appel au traitement du langage naturel pour détecter des entités dans l'entrée utilisateur.
    - Si vous spécifiez des entités dans une entrée, le service utilise ces entités, mais il fait appel au traitement du langage naturel pour détecter des intentions dans l'entrée utilisateur.

        Le comportement n'a pas été modifié pour les messages qui spécifient à la fois des intentions et des entités ou pour les messages qui n'en spécifient aucune.
- L'option permettant de marquer l'entrée utilisateur comme non pertinente (Mark as irrelevant) est désormais disponible pour toutes les langues prises en charge. Il s'agit d'une fonction bêta.
- Un nouvel onglet, nommé Credentials, constitue un espace dans lequel sont regroupées toutes les informations dont vous avez besoin pour connecter votre application à un espace de travail (par exemple, les données d'identification du service et l'ID de l'espace de travail), ainsi que d'autres options de déploiement. Pour accéder à l'onglet Credentials pour votre espace de travail, cliquez sur l'icône ![Menu](images/Menu_16.png) et sélectionnez **Credentials**.

### 9 mars 2017
{: #9March2017}

L'API REST {{site.data.keyword.conversationshort}} prend désormais en charge l'accès aux ressources suivantes :

- espaces de travail
- intentions
- exemples
- contre-exemples

Pour plus d'informations, reportez-vous à la documentation [API Reference ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.

### 7 mars 2017
{: #7March2017}

- L'utilisation de `.` ou de `..` comme nom d'intention peut entraîner des problèmes et n'est plus prise en charge.

    Vous ne pouvez pas renommer ou supprimer une intention portant ce nom ; pour changer le nom, exportez votre intention dans un fichier, renommez-la dans le fichier, puis importez le fichier ainsi modifié dans votre espace de travail.

    Les clients payants peuvent contacter le support pour un changement de base de données.

### 1 mars 2017
{: #1March2017}

- Les entités de système sont désormais activées en allemand.

### 22 février 2017
{: #22February2017}

- Les messages sont désormais limités à 2 048 caractères.

### 3 février 2017
{: #3February2017}

- Nous avons modifié la façon dont les intentions sont notées et ajouté la fonction de marquage des entrées comme non pertinentes à votre application. Pour plus d'informations, reportez-vous à la section portant sur l'option `Mark as irrelevant` dans la rubrique [Définition d'intentions](intents.html). 

- Cette édition a introduit une modification majeure dans l'espace de travail. Pour bénéficier de cette modification, vous devez mettre à niveau manuellement votre espace de travail. Pour ce faire, procédez comme suit :

  1.  [Dupliquez votre espace de travail](configure-workspace.html#exporting-and-copying-workspaces).
  1.  Mettez à niveau l'espace de travail en double en cliquant sur l'icône Upgrade (![Icône Upgrade](images/upgrade.png)).
  1.  Testez l'espace de travail mis à niveau. 
  1.  Lorsque vous avez terminé les tests et qu'aucune anomalie n'a été constatée, appliquez la mise à niveau à votre application en modifiant l'appel d'API de message pour que la date **2017-02-03** ou une date ultérieure soit utilisée. 

- Le traitement des actions **Jump to** a été modifié pour empêcher que des boucles ne se produisent dans certaines conditions. Auparavant, si vous passiez directement à la condition d'un noeud et que ni ce noeud ni aucun de ses noeuds homologues n'avaient une condition pour laquelle la valeur true avait été renvoyée, le système accédait directement au noeud de niveau racine et recherchait un noeud dont la condition correspondait à l'entrée. Dans certaines situations, ce traitement créait une boucle, ce qui empêchait le dialogue de progresser.

  Dans le cadre du nouveau processus, si le noeud cible et ses homologues ne renvoient pas la valeur true, l'échange de dialogue prend fin. Si vous souhaitez réimplémenter l'ancien modèle, ajoutez un noeud homologue final avec une condition `true`. Dans la réponse, utilisez une action **Jump to** qui cible la condition du premier noeud au niveau racine de l'arborescence de votre dialogue.

### 11 janvier 2017
{: #11January2017}

- Dans cette édition, vous pouvez personnaliser les titres de noeud dans un dialogue.

### 22 décembre 2016
{: #22December2016}

- Dans cette édition, les noeuds de dialogue comprennent désormais une nouvelle section contenant `leur titre`. Le `titre d'un noeud` n'est pas personnalisable. Lorsque le noeud de dialogue est réduit, sa `condition` est indiquée comme `titre`. Si le noeud ne comporte pas de `condition`, la mention "Untitled Node" apparaît en guise de titre.

### 19 décembre 2016
{: #19December2016}

Plusieurs modifications ont été apportées au générateur de dialogue afin de le rendre plus intuitif et facile à utiliser :

- Une vue d'édition élargie facilite l'affichage de tous les détails d'un noeud pendant que vous le gérez.
- Un noeud peut contenir plusieurs réponses, chacune d'elles étant déclenchées par une condition distincte. Pour plus d'informations, reportez-vous à la section sur les [réponses multiples](dialog-overview.html#responses).

### 5 décembre 2016
{: #5December2016}

- Les nouvelles langues suivantes sont prises en charge, toutes en mode expérimental : allemand, chinois traditionnel, chinois simplifié et néerlandais.
- Les deux nouvelles entités de système suivantes sont disponibles : @sys-date et @sys-time. Pour plus d'informations, reportez-vous à la rubrique sur les [entités de système](system-entities.html).

### 21 octobre 2016
{: #21October2016}

- Le service {{site.data.keyword.conversationshort}} fournit désormais des entités de système ; il s'agit d'entités communes que vous pouvez utiliser pour n'importe quel scénario d'utilisation. Pour plus d'informations, reportez-vous à la section `Activation des entités de système` dans la rubrique [Définition d'entités](entities.html). 
- Vous pouvez désormais afficher un historique des conversations avec les utilisateurs sur la page Improve. Cela doit vous permettre de comprendre le comportement de votre bot. Pour plus d'informations, reportez-vous à la rubrique [A propos du composant Improve](logs.html).
- Vous pouvez désormais importer des entités à partir d'un fichier CSV, ce qui peut être utile si vous avez beaucoup d'entités. Pour plus d'informations, reportez-vous à la section `Importation d'entités` dans la rubrique [Définition d'entités](entities.html). 

### 20 septembre 2016
{: #20September2016}

**Nouvelle version** : 2016-09-20

Pour bénéficier des modifications spécifiques d'une nouvelle version, remplacez la valeur du paramètre `version` par la nouvelle date. Si vous n'êtes pas prêt à passer à cette version, ne modifiez pas la date de votre version.

- Version **2016-09-20** : `dialog_stack`, qui était un tableau de chaînes, a été remplacé par un tableau d'objets JSON.

### 29 août 2016
{: #29August2016}

- Vous pouvez déplacer des noeuds de dialogue d'une branche vers une autre, en tant que noeuds de même niveau ou noeuds homologues. Pour plus d'informations, reportez-vous à la rubrique [Déplacement d'un noeud de dialogue](dialog-build.html#move-node).
- Vous pouvez développer la fenêtre d'édition JSON.
- Vous pouvez visualiser les journaux de discussion relatifs aux conversations de votre bot afin de mieux comprendre son comportement. Vous pouvez filtrer l'affichage par intentions, entités, date et heure. Pour plus d'informations, reportez-vous à la rubrique [A propos du composant Improve](logs.html).

### 11 juillet 2016
{: #21July2016}

- Cette édition officiellement disponible vous permet de gérer des entités et des dialogues afin de créer un bot totalement opérationnel.

### 18 mai 2016
{: #18May2016}

- Cette édition expérimentale de {{site.data.keyword.conversationshort}} introduit l'interface utilisateur et vous permet de gérer des espaces de travail, des intentions et des exemples.
