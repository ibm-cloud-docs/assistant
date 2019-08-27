---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-28"

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
{:gif: data-image-type='gif'}

# Conseils de création de dialogue
{: #dialog-tips}

Apprenez à créer un dialogue et à obtenir des conseils pour mener à bien des étapes plus complexes.
{: shortdesc}

Consultez les conseils suivants de concepteurs de dialogues expérimentés.

## Planification du dialogue global
{: #dialog-tips-plan}

- Planifiez la conception du dialogue que vous voulez construire avant d'ajouter un noeud de dialogue unique. Esquissez-le sur papier, si nécessaire.
- Dans la mesure du possible, basez vos décisions de conception sur des données issues de preuves et de comportements réels. N'ajoutez pas de noeuds pour gérer une situation qui, *selon vous*, pourrait se produire.
- Evitez de copier les processus métier en l’état. Ils sont rarement conversationnels.
- Si des personnes utilisent déjà un processus, examinez la façon dont elles l'abordent. Ces utilisateurs optimisent généralement le processus du point de vue de la conversation.
- Décidez du ton, de la personnalité et du positionnement de votre assistant. Reflétez systématiquement ces choix dans le dialogue que vous créez.
- Ne représentez jamais l'assistant comme étant un humain. Si les usagers croient que l’assistant est une personne, puis qu'ils découvrent que ce n’est pas le cas, ils risquent de devenir méfiants.
- Tout ne doit pas être une conversation. Parfois, un formulaire Web est plus efficace.

## Ajout de noeuds
{: #dialog-tips-nodes}

- Ajoutez un nom de noeud décrivant la fonction du noeud.

  Vous savez ce que le noeud fait actuellement, mais vous ne le saurez peut-être pas dans quelques mois. Vous-même et tous les membres de l’équipe pourront ainsi se référer à ce nom de noeud descriptif à l'avenir. Le nom du noeud est également affiché dans le journal, ce qui peut vous aider à déboguer une conversation ultérieurement.
- Pour collecter les informations nécessaires à l'exécution d'une tâche, essayez d'utiliser un noeud avec attributs plutôt qu'un ensemble de noeuds distincts pour obtenir des informations de la part des utilisateurs. Reportez-vous à la section [Collecte d'informations à l'aide d'attributs](/docs/services/assistant?topic=assistant-dialog-slots).
- Pour un flux de processus complexe, indiquez aux utilisateurs les informations qu’ils devront fournir au début du processus.
- Comprenez comment l'assistant se déplace dans l'arborescence du dialogue et l'impact des dossiers, des branches, des accès directs et des digressions sur le trajet. Reportez-vous à la rubrique [Flux de dialogue](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-flow).
- N'ajoutez pas de liens d'accès partout. Ils augmentent la complexité du flux de dialogue et rendent plus difficile son débogage ultérieur.
- Pour accéder à un noeud de la même branche que le noeud actuel, utilisez *Skip user input* et non *Jump-to*.

  Ce choix vous évite d'avoir à modifier les paramètres du noeud actuel lorsque vous supprimez ou réorganisez les noeuds enfants auxquels vous souhaitez accéder. Reportez-vous à la rubrique [Définition de l'étape suivante](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-jump-to).
- Avant d'activer les digressions à partir d'un noeud, testez les scénarios utilisateur les plus courants. Et assurez-vous que les noeuds susceptibles de faire l'objet d'une digression sont configurés pour effectuer un retour de la digression. Reportez-vous à la rubrique [Digressions](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions).

## Ajout de réponses
{: #dialog-tips-responses}

- Faites en sorte que vos réponses soient brèves et utiles.
- Reflétez l'intention de l'utilisateur dans la réponse.

  Cela garantit aux utilisateurs que le bot les comprend ou, si ce n'est pas le cas, leur donne la possibilité de corriger immédiatement un malentendu.
- Incluez des liens vers des sites externes dans les réponses uniquement si la réponse dépend de données qui changent fréquemment.
- Evitez l'utilisation excessive des boutons. Encourager les utilisateurs à choisir des options prédéfinies à partir d'un ensemble de boutons ressemble moins à une conversation réelle et diminue votre capacité à apprendre ce que les utilisateurs veulent vraiment faire. Lorsque vous laissez des utilisateurs réels formuler des demandes avec leurs propres mots, vous pouvez utiliser ces entrées pour former le système et en déduire de meilleures intentions.
- Evitez d'utiliser un groupe de noeuds alors qu'un seul noeud suffira. Par exemple, ajoutez plusieurs réponses conditionnelles à un seul noeud pour renvoyer des réponses différentes en fonction des détails fournis par l'utilisateur. Reportez-vous à la rubrique [Réponses conditionnelles](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multiple).
- Formulez soigneusement vos réponses. Vous pouvez modifier la façon dont une personne réagit à votre système simplement en modifiant la formulation de votre réponse. La modification d'une ligne de texte peut vous éviter d'avoir à écrire plusieurs lignes de code pour mettre en oeuvre une solution de programmation complexe.
- Sauvegardez fréquemment votre compétence. Reportez-vous à la rubrique [Téléchargement d'une compétence](/docs/services/assistant?topic=assistant-skill-add#skill-add-download).

## Conseils relatifs à la capture d'informations à partir des entrées utilisateur
{: #dialog-tips-user-input}

Il peut être difficile de connaître la syntaxe à utiliser dans votre noeud de dialogue afin de capturer avec précision les informations que vous souhaitez rechercher dans l'entrée utilisateur. Voici quelques approches que vous pouvez utiliser pour répondre à des objectifs communs.

- **Renvoi de l'entrée utilisateur** : vous pouvez capturer le texte exact énoncé par l'utilisateur et le renvoyer dans votre réponse. Utilisez l'expression SpEL suivante dans une réponse pour répéter le texte que l'utilisateur a spécifié dans la réponse :

  `Vous avez dit : <? input.text ?>.`

  Si la correction automatique est activée et que vous souhaitez renvoyer l'entrée utilisateur d'origine avant sa correction, vous pouvez utiliser `<? input.original_text ?>`. Toutefois, veillez à utiliser une condition de réponse qui vérifie si la zone `original_text` existe d'abord.
  {: note}

- **Détermination du nombre de mots dans l'entrée utilisateur** : vous pouvez exécuter l’une des méthodes String prises en charge sur l’objet input.text. Par exemple, vous pouvez connaître le nombre de mots d'un énoncé utilisateur à l'aide de l'expression SpEL suivante :

  `input.text.split(' ').size()`

  Pour connaître les différentes méthodes que vous pouvez utiliser, reportez-vous à la rubrique [Méthodes de langage d'expression pour String](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings).

- **Gestion d'intentions multiples** : un utilisateur saisit une entrée qui exprime le souhait d'effectuer deux tâches distinctes. `Je souhaite ouvrir un compte d'épargne et demander une carte de crédit.` Comment le dialogue reconnaît-il et traite-t-il ces deux intentions ? Reportez-vous à l'entrée [Compound questions](https://sodoherty.ai/2017/02/06/compound-questions/){: new_window} du blog de Simon O'Doherty pour connaître les stratégies que vous pouvez essayer. (Simon est un développeur de l'équipe {{site.data.keyword.conversationshort}}.)

- **Gestion d'intentions ambiguës** : un utilisateur saisit une entrée qui exprime un souhait suffisamment ambigu pour que l'assistant trouve deux noeuds ou plus avec des intentions susceptibles d'y répondre. Comment le dialogue sait-il quelle branche de dialogue il doit suivre ? Si vous activez la désambiguïsation, il peut présenter leurs options aux utilisateurs et leur demander de choisir celle qui convient. Pour plus d'informations, reportez-vous à la rubrique [Désambiguïsation](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).

- **Traitement de plusieurs entités dans une entrée** : si vous souhaitez évaluer uniquement la valeur de la première instance détectée pour un type d'entité, vous pouvez utiliser la syntaxe `@entity == 'specific-value'` à la place du format `@entity:(specific-value)`.

  Par exemple, lorsque vous utilisez `@appliance == 'air conditioner'`, vous évaluez uniquement la valeur de la première entité `@appliance` détectée. Mais, lorsque la syntaxe `@appliance:(air conditioner)` est utilisée, elle prend la forme développée `entity['appliance'].contains('air conditioner')`, et une correspondance est établie chaque fois qu'au moins une entité `@appliance` avec la valeur 'air conditioner' est détectée dans l'entrée utilisateur.

## Conseils relatifs à l'utilisation des conditions
{: #dialog-tips-condition-usage}

- **Recherche de valeurs contenant des caractères spéciaux** : si vous souhaitez vérifier si une entité ou une variable contextuelle contient une valeur et que cette dernière comporte un caractère spécial, par exemple, une apostrophe ('), vous devez placer la valeur recherchée entre parenthèses. Par exemple, pour vérifier si une entité ou une variable contextuelle contient le nom `O'Reilly`, vous devez placer celui-ci entre parenthèses.

  `@person:(O'Reilly)` et `$person:(O'Reilly)`

  L'assistant convertit ces références abrégées en expressions SpEL complètes, présentées ci-dessous :

  `entities['person']?.contains('O''Reilly')` et `context['person'] == 'O''Reilly'`

  SpEL utilise une seconde apostrophe pour mettre en échappement l'apostrophe incluse dans le nom.
  {: note}

- **Vérification de plusieurs valeurs** : si vous souhaitez rechercher plusieurs valeurs, vous pouvez créer une condition utilisant des opérateurs OR (`||`) pour répertorier plusieurs valeurs dans la condition. Par exemple, pour définir une condition qui est vraie si la variable contextuelle `$state` contient les abréviations pour Massachusetts, Maine ou New Hampshire, vous pouvez utiliser l'expression suivante :

  `$state:MA || $state:ME || $state:NH`

- **Recherche de valeurs numériques** : lors de la comparaison de nombres, assurez-vous d'abord que l'entité ou la variable que vous vérifiez possède une valeur. Si l'entité ou la variable n'a pas de valeur numérique, elle est traitée comme ayant une valeur nulle (0) dans une comparaison numérique.

  Par exemple, vous souhaitez vérifier si la valeur en dollars spécifiée par un utilisateur dans une entrée est inférieure à 100. Si vous utilisez la condition `@price < 100` et que l'entité `@price` a une valeur null, la condition prend la valeur `true` car la valeur 0 est inférieure à 100, même si le prix n'a jamais été défini. Pour éviter ce type de résultat inexact, utilisez une condition telle que `@price AND @price < 100`. Si aucune valeur n'est associée à `@price`, cette condition renvoie correctement la valeur false.

- **Recherche d'intentions ayant un canevas de nom d'intention spécifique** : vous pouvez utiliser une condition qui recherche des intentions correspondant à un canevas. Par exemple, pour rechercher des intentions détectées dont les noms commencent par 'User_', vous pouvez utiliser une syntaxe telle que la suivante dans la condition :

  `intents[0].intent.startsWith("User_")`

  Toutefois, lorsque vous faites cela, toutes les intentions détectées sont prises en compte, même celles dont la cote de confiance est inférieure à 0.2. Vérifiez également que les intentions qui sont considérées comme non pertinentes par Watson sur la base de leur cote de confiance ne sont pas renvoyées. Pour ce faire, modifiez la condition comme suit :

  `!irrelevant && intents[0].intent.startsWith("User_")`

- **Impact de la correspondance partielle (Fuzzy Matching) sur la reconnaissance d'entité** : si vous utilisez une entité comme condition et que la correspondance partielle est activée, `@entity_name` renvoie la valeur true uniquement si la cote de confiance relative à la correspondance est supérieure à 30 %. Autrement dit, seulement si `@entity_name.confidence > .3`.

## Stockage et reconnaissance des groupes de canevas d'entités dans l'entrée
{: #dialog-tips-get-pattern-groups}

Pour stocker la valeur d'une entité de canevas dans une variable contextuelle, ajoutez .literal au nom d'entité. L'utilisation de cette syntaxe permet de s'assurer que le passage de texte issu d'une entrée utilisateur qui correspondait exactement au canevas spécifié est stocké dans la variable.

| Variable   | Valeur               |
|------------|---------------------|
| email      | <? @email.literal ?> |

Pour stocker le texte d'un groupe unique dans une entité de canevas avec des groupes définis, spécifiez le numéro de tableau du groupe que vous souhaitez stocker. Supposons par exemple que le canevas d'entité est défini comme suit pour l'entité @phone_number (n'oubliez pas que les parenthèses désignent des groupes de canevas) :

`\b((958)|(555))-(\d{3})-(\d{4})\b`

Pour stocker uniquement l'indicatif régional du numéro de téléphone spécifié dans l'entrée utilisateur, vous pouvez utiliser la syntaxe suivante :

| Variable       | Valeur                         |
|----------------|-------------------------------|
| area_code      | <? @phone_number.groups[1] ?> |

Les groupes sont délimités par l'expression régulière qui est utilisée pour définir le canevas de groupe. Par exemple, si l'entrée utilisateur correspondant au canevas défini dans l'entité `@phone_number` est `958-234-3456`, les groupes suivants sont créés :

| Numéro de groupe | Valeur de moteur d'expression régulière  | Valeur de dialogue   | Explication |
|--------------|---------------------|----------------|-------------|
| groups[0]    | `958-234-3456`      | `958-234-3456` | Le premier groupe est toujours la chaîne qui correspond parfaitement. |
| groups[1]    | `((958)`l`(555))`   | `958`          | Chaîne qui correspond à l'expression régulière pour le premier groupe défini, `((958)`l`(555))`. |
| groups[2]    | `(958)`             | `958`          | Correspondance par rapport au groupe qui est inclus comme première opérande dans l'opération OR `((958)`l`(555))`. |
| groups[3]    | `(555)`             | `null`         | Pas de correspondance par rapport au groupe qui est inclus comme seconde opérande dans l'opération OR `((958)`l`(555))`. |
| groups[4]    | `(\d{3})`           | `234`          | Chaîne qui correspond à l'expression régulière définie pour le groupe. |
| groups[5]    | `(\d{4})`           | `3456`         | Chaîne qui correspond à l'expression régulière définie pour le groupe. |
{: caption="Détails de groupe" caption-side="top"}

Afin de vous aider à déchiffrer le numéro de groupe à utiliser pour capturer la section d'entrée qui vous intéresse, vous pouvez extraire des informations sur tous les groupes en même temps. Utilisez la syntaxe suivante pour créer une variable contextuelle qui renvoie un tableau de toutes les correspondances d'entité de canevas regroupées :

| Variable                 | Valeur                      |
|--------------------------|----------------------------|
| array_of_matched_groups  | <? @phone_number.groups ?> |

Utilisez le panneau "Try it out" pour entrer des valeurs de numéro de téléphone de test. Pour l'entrée `958-123-2345`, cette expression affecte à `$array_of_matched_groups` la valeur `["958-123-2345","958","958",null,"123","2345"]`.

Vous pouvez ensuite compter chaque valeur du tableau à partir de 0 afin d'obtenir le numéro de groupe correspondant.

| Valeur d'élément de tableau | Numéro d'élément de tableau |
|---------------------|----------------------|
| "958-123-2345"      | 0 |
| "958"               | 1 |
| "958"               | 2 |
| null                | 3 |
| "123"               | 4 |
| "2345"              | 5 |
{: caption="Eléments de tableau" caption-side="top"}

A partir du résultat, vous pouvez déterminer que pour capturer les quatre derniers chiffres du numéro de téléphone, vous devez regrouper #5, par exemple.

Afin de renvoyer la structure de tableau JSON qui est créée pour représenter l'entité de canevas regroupée, utilisez la syntaxe suivante :

| Variable             | Valeur                           |
|----------------------|---------------------------------|
| json_matched_groups  | <? @phone_number.groups_json ?> |

Cette expression affecte à `$json_matched_groups` le tableau JSON suivant :

```json
[
  {"group": "group_0","location": [0, 12]},
  {"group": "group_1","location": [0, 3]},
  {"group": "group_2","location": [0, 3]},
  {"group": "group_3"},
  {"group": "group_4","location": [4, 7]},
  {"group": "group_5","location": [8, 12]}
]
```
{: codeblock}

`location` est une propriété d'une entité qui utilise un décalage de caractère basé sur des zéros indiquant où la valeur d'entité détectée commence et finit dans le texte d'entrée.
{: note}

Si vous vous attendez à ce que deux numéros de téléphone soient fournis en entrée, vous pouvez rechercher deux numéros de téléphone. S'ils sont présents, utilisez la syntaxe suivante pour capturer l'indicatif régional du second numéro, par exemple :

| Variable         | Valeur                                       |
|------------------|---------------------------------------------|
| second_areacode  | <? entities['phone_number'][1].groups[1] ?> |

Si l'entrée est `Je veux changer mon numéro de téléphone 958-234-3456 et le remplacer par 555-456-5678`, la valeur de `$second_areacode` est `555`.

## Affichage des détails de l'appel d'API
{: #dialog-tips-inspect-api}

Lorsque vous testez votre dialogue avec le panneau "Try it out", vous voudrez peut-être savoir à quoi ressemblent les appels d'API sous-jacents renvoyés par le service. Vous pouvez utiliser les outils de développement fournis par votre navigateur Web pour les inspecter.

Par exemple, dans Chrome ouvrez les Outils de développement. Cliquez sur l'outil Network. La section Name répertorie plusieurs appels d'API. Cliquez sur l'appel de message associé à votre énoncé test, puis cliquez dans la colonne Response pour afficher le corps de la réponse de l'API. Il répertorie les intentions et les entités reconnues dans l'entrée utilisateur avec leurs cotes de confiance, ainsi que les valeurs des variables contextuelles au moment de l'appel. Pour afficher le corps de la réponse au format structuré, cliquez sur la colonne Preview.

![Illustration montrant comment afficher les détails de l'appel de l'API à l'aide du navigateur Web de Chrome.](images/api-browser-dev.png)
