---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-09"

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
{:table: .aria-labeledby="caption"}

# Présentation du dialogue
{: #dialog-overview}

Le dialogue utilise les intentions et les entités qui sont identifiées dans l'entrée utilisateur, plus le contexte issu de l'application, pour interagir avec l'utilisateur et, in fine, fournir une réponse utile.
{: shortdesc}

La réponse peut être la réponse à une question, telle que `Où puis-je trouver de l'essence ?` ou l'exécution d'une commande, comme allumer la radio. L'intention et l'entité peuvent suffire pour identifier la réponse appropriée, ou bien le dialogue peut demander à l'utilisateur de saisir davantage d'informations pour répondre correctement. Par exemple, si un utilisateur demande `Où puis-je trouver de la nourriture ?`, vous souhaiterez peut-être lui demander de préciser s'il recherche un restaurant ou un magasin d'alimentation, pour dîner sur place ou pour emporter, etc. Vous pouvez réclamer davantage d'informations dans une réponse textuelle et créer un ou plusieurs noeuds enfant pour traiter la nouvelle entrée.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/oQUpejt6d84?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Le dialogue est représenté graphiquement dans l'outil {{site.data.keyword.conversationshort}} sous la forme d'une arborescence. Créez une branche pour traiter chaque intention qui doit être gérée par votre conversation. Une branche est composée de plusieurs noeuds.

## Noeuds de dialogue

Chaque noeud de dialogue contient au moins une condition et une réponse.

![Graphique représentant une entrée utilisateur reliée par une flèche droite à une boîte contenant l'instruction If: CONDITION, Then: REPONSE](images/node1-empty.png)

- Condition : indique les informations que l'entrée utilisateur doit contenir pour déclencher ce noeud dans le dialogue. Ces informations peuvent être une intention spécifique, une valeur d'entité ou une valeur de variable contextuelle. Pour plus d'informations, reportez-vous à la section [Conditions](dialog-runtime.html#conditions).
- Réponse : énoncé que le service utilise pour répondre à l'utilisateur. La réponse peut aussi être configurée pour déclencher des actions par programmation. Pour plus d'informations, reportez-vous à la section [Réponses](#responses).

Le noeud est basé sur une construction if/then : si cette condition a pour valeur true, renvoyer cette réponse.

Par exemple, le noeud ci-dessous est déclenché si la fonction de traitement du langage naturel du service détermine que l'entrée utilisateur contient l'intention `#cupcake-menu`. Suite au déclenchement du noeud, le service fournit une réponse appropriée.

![Graphique représentant la question posée par l'utilisateur au sujet des parfums proposés pour les cupcakes. La condition If est #cupcake-menu et la réponse Then correspond à une liste répertoriant les parfums des cupcakes.](images/node1-simple.png)

Un noeud comportant une condition et une réponse peut gérer des demandes d'utilisateur simples. Mais, la plupart du temps, les utilisateurs posent des questions plus sophistiquées ou demandent de l'aide pour des tâches plus complexes. Vous pouvez ajouter des noeuds enfant qui demandent à l'utilisateur de fournir les informations supplémentaires dont le service a besoin.

![Graphique représentant le premier noeud du dialogue demandant à l'utilisateur s'il souhaite acheter des cupcakes ordinaires ou sans gluten. Ce noeud est associé à deux noeuds enfant qui fournissent une réponse différente selon la réponse de l'utilisateur.](images/node1-children.png)

## Flux de dialogue

Le dialogue que vous créez est traité par le service depuis le premier noeud jusqu'au dernier noeud de l'arborescence. 

![Une flèche vers le bas est représentée à côté de 3 noeuds pour montrer que le dialogue suit son chemin depuis le premier noeud jusqu'au dernier noeud](images/node-flow-down.png)

A mesure que le service se déplace vers le bas dans l'arborescence, s'il détecte une condition qui est satisfaite, il déclenche ce noeud. Il se déplace alors le long du noeud déclenché afin de comparer l'entrée utilisateur à d'éventuelles conditions de noeud enfant. A mesure qu'il effectue ces comparaisons avec des noeuds enfant, il se déplace de nouveau depuis le premier noeud jusqu'au dernier noeud. 

Le service continue de suivre son chemin à travers l'arborescence du dialogue depuis le premier noeud jusqu'au dernier noeud, le long de chaque noeud déclenché, puis depuis le premier noeud enfant jusqu'au dernier noeud enfant et le long de chaque noeud enfant déclenché jusqu'à ce qu'il atteigne le dernier noeud de la branche qu'il parcourt. 

![Graphique représentant une flèche portant le numéro 1 qui part du premier noeud racine et qui est dirigée vers le dernier noeud racine, une flèche portant le numéro 2 et qui s'étend sur la longueur d'un noeud déclenché, et une flèche portant le numéro 3 qui part du premier noeud enfant et qui est dirigée vers le dernier noeud enfant du noeud déclenché.](images/node-flow.png)

Lorsque vous commencez à créer le dialogue, vous devez déterminer les branches que vous souhaitez inclure, et l'endroit où vous souhaitez les placer. L'ordre des branches est important car les noeuds sont évalués du premier jusqu'au dernier. Le premier noeud racine dont la condition correspond à l'entrée est utilisé ; les autres noeuds ajoutés ultérieurement dans l'arborescence ne sont pas déclenchés. 

Lorsque le service atteint la fin d'une branche, ou s'il ne trouve aucune condition qui renvoie la valeur true à partir de l'ensemble de noeuds enfant en cours sur lequel porte son évaluation, il repasse dans la base de l'arborescence. Et, là encore, le service traite les noeuds racine, du premier jusqu'au dernier. Si aucune des conditions ne renvoie la valeur true, la réponse émise par le dernier noeud de l'arborescence, qui possède généralement une condition `anything_else` spéciale qui renvoie toujours la valeur true, est renvoyée. 

Vous pouvez interrompre le flux "du premier au dernier" standard en personnalisant ce qu'il se produit une fois qu'un noeud est traité. Par exemple, vous pouvez configurer un noeud pour qu'il accède directement à un autre noeud une fois qu'il a été traité, même si l'autre noeud est positionné avant dans l'arborescence. Pour plus d'informations, reportez-vous à la rubrique [Définition de l'étape suivante](dialog-overview.html#jump-to). 

La façon dont vous configurez les paramètres de digression pour chaque noeud peut aussi avoir un impact sur la façon dont les utilisateurs se déplacent à travers les noeuds lors de l'exécution. Si vous permettez les digressions à partir de la plupart des noeuds, les utilisateurs peuvent passer d'un noeud à un autre et revenir au noeud initial plus facilement. Pour plus d'informations, reportez-vous à la rubrique [Digressions](dialog-runtime.html#digressions). 

## Conditions
{: #conditions}

Une condition de noeud détermine si ce noeud est utilisé dans la conversation. Les conditions de réponse déterminent la réponse qui doit s'afficher pour un utilisateur.

- [Artefacts de condition](dialog-overview.html#condition-artifacts)
- [Détails de syntaxe de condition](dialog-overview.html#condition-syntax)
- [Conseils relatifs à l'utilisation des conditions](dialog-overview.html#condition-tips)

### Artefacts de condition
{: #condition-artifacts}

Vous pouvez utiliser un ou plusieurs des artefacts suivants, dans n'importe quelle combinaison, pour définir une condition :

- **Variable contextuelle** : le noeud est utilisé si l'expression de variable contextuelle que vous spécifiez a pour valeur true. Utilisez la syntaxe `$variable_name:value` ou `$variable_name == 'value'`. Par exemple, `$city:Boston` vérifie si la variable contextuelle`$city` contient la valeur `Boston`. Si tel est le cas, le noeud ou la réponse est traité.

  Vous ne devez pas définir une condition de noeud ou de réponse en fonction de la valeur d'une variable contextuelle dans le même noeud de dialogue que celui dans lequel vous définissez cette valeur de variable contextuelle.
  {: tip}

  Pour plus d'informations sur les variables contextuelles, reportez-vous à la rubrique [Variables contextuelles](dialog-runtime.html#context).

- **Entité** : le noeud est utilisé dès lors qu'une valeur ou un synonyme de l'entité est reconnu dans l'entrée utilisateur. Utilisez la syntaxe `@entity_name`. Par exemple, `@city` vérifie si certains des noms de ville qui sont définis pour l'entité @city ont été détectés dans l'entrée utilisateur. Si tel est le cas, le noeud ou la réponse est traité.

  Prenez soin de créer un noeud homologue pour gérer les situations où les valeurs ou les synonymes de l'entité ne sont pas du tout reconnus.
  {: tip}

  Pour plus d'informations sur les entités, reportez-vous à la rubrique [Définition d'entités](entities.html).

- **Valeur d'entité** : le noeud est utilisé si la valeur d'entité est détectée dans l'entrée utilisateur. Utilisez la syntaxe`@entity_name:value`, puis spécifiez une valeur définie pour l'entité, et non un synonyme. Par exemple : `@city:Boston` vérifie si le nom de ville spécifique `Boston` a été détecté dans l'entrée utilisateur. 

  Si l'entité est une entité de canevas avec des groupes de capture, vous pouvez rechercher une correspondance de valeur de groupe donnée. Par exemple, vous pouvez utiliser la syntaxe `@us_phone.groups[1] == '617'`.
  Pour plus d'informations, reportez-vous à la rubrique [Stockage de valeurs d'entité de canevas dans des variables contextuelles](dialog-runtime.html#context-pattern-entities). 

  Si vous recherchez l'entité, sans indiquer de valeur spécifique pour cette dernière, dans un noeud homologue, prenez soin de placer ce noeud (qui recherche une valeur d'entité spécifique) avant le noeud homologue qui recherche uniquement l'entité.
Sinon, ce noeud ne sera jamais évalué.
  {: tip}

- **Intention** : la condition la plus simple correspond à une seule et même intention. Le noeud est utilisé si l'entrée utilisateur est mappée à cette intention. Utilisez la syntaxe `#intent_name`. Par exemple, `#weather` vérifie si l'intention détectée dans l'entrée utilisateur est `weather`. Si tel est le cas, le noeud est traité.

  Pour plus d'informations sur les intentions, reportez-vous à la rubrique [Définition d'intentions](intents.html).

- **Condition spéciale** : conditions qui sont fournies avec le service et que vous pouvez utiliser pour effectuer des fonctions de dialogue de base.

| Syntaxe de condition     | Description |
|----------------------|-------------|
| `anything_else`      | Vous pouvez utiliser cette condition à la fin du dialogue pour qu'elle soit traitée lorsque l'entrée utilisateur ne correspond à aucun autre noeud du dialogue. Le noeud **Anything else** est déclenché par cette condition. |
| `conversation_start` | A l'instar de **welcome**, cette condition renvoie la valeur true lors du premier échange du dialogue. Contrairement à **welcome**, elle renvoie la valeur true, que la demande initiale émise par l'application contienne ou non l'entrée utilisateur. Un noeud comportant la condition **conversation_start** peut être utilisé pour initialiser des variables contextuelles ou pour effectuer d'autres tâches au début du dialogue. |
| `false`              | Cette condition prend toujours la valeur false. Vous pouvez l'utiliser au début d'une branche en cours de développement, afin d'empêcher son utilisation, ou comme la condition d'un noeud qui fournit une fonction de base et qui est utilisé uniquement comme cible d'une action **Jump to**. |
| `irrelevant`         | Cette condition renvoie la valeur true si l'entrée utilisateur est déterminée comme non pertinente par le service {{site.data.keyword.conversationshort}}. |
| `true`               | Cette condition renvoie toujours la valeur true. Vous pouvez l'utiliser à la fin d'une liste de noeuds ou de réponses afin de capturer les réponses qui ne correspondaient à aucune des conditions précédentes. |
| `welcome`            | Cette condition renvoie la valeur true lors du premier échange de dialogue (lorsque la conversation démarre), uniquement si la demande initiale émise par l'application ne contient aucune entrée utilisateur. Elle prend la valeur false dans tous les échanges de dialogue suivants. Le noeud **Welcome** est déclenché par cette condition. En général, un noeud comportant cette condition est utilisé comme message d'accueil, par exemple, `Bienvenue dans notre application de commande de pizza.` |
{: caption="Conditions spéciales" caption-side="top"}

### Détails de syntaxe de condition
{: #condition-syntax}

Utilisez l'une des options de syntaxe suivantes pour créer des expressions valides dans des conditions :

- Des notations sténographiques, qui permettent de faire référence à des intentions, des entités et des variables contextuelles. Reportez-vous à la rubrique [Accès à des objets en vue de leur évaluation](expression-language.html).

- Le langage d'expression SpEL, qui prend en charge l'interrogation et la manipulation d'un graphe d'objets lors de l'exécution. Pour plus d'informations, reportez-vous à l'article [Spring Expression Language (SpEL) language ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}.

Vous pouvez utiliser des expressions régulières pour comparer des valeurs à des conditions. Pour trouver une chaîne correspondante, par exemple, vous pouvez utiliser la méthode `String.find`. Pour plus d'informations, reportez-vous à la rubrique [Méthodes](dialog-methods.html).

### Conseils relatifs à l'utilisation des conditions
{: #condition-tips}

- **Recherche de valeurs contenant des caractères spéciaux** : si vous souhaitez vérifier si une entité ou une variable contextuelle contient une valeur et que cette dernière comporte un caractère spécial, par exemple, une apostrophe ('), vous devez placer la valeur recherchée entre parenthèses. Par exemple, pour vérifier si une entité ou une variable contextuelle contient le nom `O'Reilly`, vous devez placer celui-ci entre parenthèses. 

  `@person:(O'Reilly)` et `$person:(O'Reilly)`

  Le service convertit ces références abrégées en expressions SpEL complètes, présentées ci-dessous :

  `entities['person']?.contains('O''Reilly')` et `context['person'] == 'O''Reilly'`

  **Remarque** : SpEL utilise une seconde apostrophe pour mettre en échappement l'apostrophe incluse dans le nom. 

- **Recherche de valeurs numériques** : lorsque vous utilisez des variables numériques, assurez-vous qu'elles comportent des valeurs. Si une variable ne comporte pas de valeur, elle est considérée comme ayant une valeur null (0) dans une comparaison numérique.

  Par exemple, si vous recherchez la valeur d'une variable avec la condition `@price < 100`et que l'entité @price a une valeur null, la condition prend la valeur `true` car la valeur 0 est inférieure à 100, même si le prix n'a jamais été défini. Pour empêcher la recherche des variables null, utilisez une condition telle que `@price AND @price < 100`. Si aucune valeur n'est associée à `@price`, cette condition renvoie correctement la valeur false. 

- **Recherche d'intentions ayant un canevas de nom d'intention spécifique** : vous pouvez utiliser une condition qui recherche des intentions correspondant à un canevas. Par exemple, pour rechercher des intentions détectées dont les noms commencent par 'User_', vous pouvez utiliser une syntaxe telle que la suivante dans la condition :

  `intents[0].intent.startsWith("User_")`

  Toutefois, lorsque vous faites cela, toutes les intentions détectées sont prises en compte, même celles dont la cote de confiance est inférieure à 0.2. Vérifiez également que les intentions qui sont considérées comme non pertinentes par Watson sur la base de leur cote de confiance ne sont pas renvoyées. Pour ce faire, modifiez la condition comme suit :

  `!irrelevant && intents[0].intent.startsWith("User_")`

- **Impact de la fonction Fuzzy Matching sur la reconnaissance d'entité** : si vous utilisez une entité comme condition et que la fonction Fuzzy Matching est activée, `@entity_name` renvoie la valeur true uniquement si la cote de confiance relative à la correspondance est supérieure à 30 %. Autrement dit, seulement si `@entity_name.confidence > .3`. 

- **Traitement de plusieurs entités dans une entrée** : si vous souhaitez évaluer uniquement la valeur de la première instance détectée pour un type d'entité, vous pouvez utiliser la syntaxe `@entity == 'specific-value'` à la place du format `@entity:(specific-value)`. 

  Par exemple, lorsque vous utilisez `@appliance == 'air conditioner'`, vous évaluez uniquement la valeur de la première entité `@appliance` détectée. Mais, lorsque la syntaxe `@appliance:(air conditioner)` est utilisée, elle prend la forme développée `entity['appliance'].contains('air conditioner')`, et une correspondance est établie chaque fois qu'au moins une entité `@appliance` avec la valeur 'air conditioner' est détectée dans l'entrée utilisateur.

## Réponses
{: #responses}

La réponse de dialogue décrit comment répondre à l'utilisateur.

Vous pouvez répondre en utilisant l'un des types de réponse suivants :

- [Réponse textuelle simple](#simple-text)
- [Réponses conditionnelles](#multiple)
- [Réponse complexe](#complex)
- [Réponse multimédia](#multimedia)

### Réponse textuelle simple
{: #simple-text}

Si vous souhaitez fournir une réponse textuelle, il vous suffit d'entrer le texte que le service doit afficher pour l'utilisateur.

![Graphique représentant un noeud avec la question posée par un utilisateur : Où êtes-vous ? et la réponse formulée par le dialogue : Nous n'avons pas de point de vente physique, mais avec une connexion Internet, vous pouvez acheter nos produits depuis n'importe où.](images/response-simple.png)

Si vous incluez une adresse électronique dans la réponse, vous devez mettre en échappement l'arrobas (`@`) à l'aide d'une barre oblique inversée (`\`). Par exemple, `Envoyez-nous vos commentaires à l'adresse feedback\@example.com.` De même, si vous incluez un signe dièse (`#`) dans la réponse, vous devez le mettre en échappement. Par exemple, `Nous sommes le \#1 des vendeurs de langoustes de l'état du Maine.` Les noms d'entité commencent par `@` et les noms d'intention commencent par `#`. La mise en échappement de ces symboles protège le service contre toute mauvaise interprétation du texte de réponse.
{: tip}

#### Ajout de variations
{: #variety}

Si vos utilisateurs utilisent fréquemment votre service de conversation, ils peuvent finir par se lasser d'entendre toujours le même message d'accueil et les mêmes réponses.  Vous pouvez ajouter des *variations* à vos réponses de sorte que votre conversation puisse répondre de différentes manières à une même condition.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/nAlIW3YPrAs?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Dans l'exemple ci-dessous, la réponse fournie par le service aux questions relatives à l'emplacement d'un magasin diffère d'une interaction à l'autre :

![Graphique représentant un noeud avec la question posée par un utilisateur : Où êtes-vous ? et les trois réponses différentes formulées par le dialogue.](images/variety.png)

Vous pouvez choisir de faire alterner les variations de réponse de manière séquentielle ou aléatoire. Par défaut, les réponses alternent de manière séquentielle, comme si elles avaient été choisies dans une liste ordonnée.

### Réponses conditionnelles
{: #multiple}

Un seul et même noeud de dialogue peut fournir différentes réponses, chacune d'elles étant déclenchées par une condition différente.  Utilisez cette approche pour prendre en charge plusieurs scénarios dans un seul et même noeud.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/KcvVQAsnhLM?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Le noeud comporte toujours une condition principale, qui sert à l'utilisation du noeud et au traitement des conditions et des réponses qu'il contient.

Dans l'exemple ci-dessous, le service utilise des informations qu'il a collectées précédemment au sujet de l'endroit où se trouve un utilisateur afin de personnaliser sa réponse, et il fournit des informations sur le magasin le plus proche de l'utilisateur. Pour savoir comment stocker les informations collectées auprès de l'utilisateur, reportez-vous à la section[Variables contextuelles](dialog-runtime.html#context).

![Graphique représentant un noeud avec la question posée par un utilisateur : Où êtes-vous ? et les trois réponses différentes formulées par le dialogue en fonction des conditions qui utilisent les informations de la variable contextuelle $state pour spécifier des lieux qui se trouvent dans ces départements.](images/multiple-responses.png)

A présent, ce seul et même noeud fournit les mêmes fonctions que quatre noeuds distincts.

Pour ajouter des réponses conditionnelles à un noeud, cliquez sur **Customize**, puis sur le bouton à bascule **Multiple responses** pour l'activer (**on**).

Les conditions définies dans un noeud sont évaluées dans un ordre défini, tout comme les noeuds.  Prenez soin de spécifier vos conditions et vos réponses dans l'ordre souhaité.  Si vous devez modifier cet ordre, sélectionnez une condition et déplacez-la vers le haut ou vers le bas dans la liste à l'aide des flèches affichées. Si vous souhaitez mettre à jour le contexte, vous devez le faire dans l'éditeur JSON pour chacune des réponses. Il n'existe pas d'éditeur JSON commun à toutes les réponses. Si vous avez associé une action **Jump to** au noeud, le passage ne se produit que lorsque toutes les réponses sont traitées.
{: tip}

### Réponse complexe
{: #complex}

Pour spécifier une réponse plus complexe, vous pouvez utiliser l'éditeur JSON pour spécifier la réponse dans la propriété `"output":{}`.

Pour inclure une valeur de variable contextuelle dans la réponse, utilisez la syntaxe `$variable-name` pour la spécifier. Pour plus d'informations, reportez-vous à la section [Variables contextuelles](dialog-runtime.html#context).

```json
{
  "output": {
    "text": "Hello $user"
  }
}
```
{: codeblock}

Pour spécifier plusieurs instructions à afficher sur des lignes distinctes, définissez la sortie sous la forme d'un tableau JSON.

```json
{
  "output": {
    "text": ["Hello there.", "How are you?"]
  }
}
```
{: codeblock}

La première phrase est affichée sur une ligne et la deuxième phrase est présentée sous la forme d'une nouvelle ligne après la première. 

Pour implémenter un comportement plus complexe, vous pouvez définir le texte de sortie comme un objet JSON complexe. Par exemple, vous pouvez utiliser un objet complexe dans la sortie JSON afin de reproduire le comportement qui consiste à ajouter des variations de réponse au noeud. Vous pouvez inclure les propriétés suivantes dans l'objet complexe :

- **values** : tableau JSON composé de plusieurs chaînes représentant différentes versions d'un texte de sortie que ce noeud de dialogue peut renvoyer. L'ordre dans lequel les valeurs du tableau sont renvoyées dépend de l'attribut `selection_policy`.

- **selection_policy** : les valeurs suivantes sont valides :

    - **random** : le système sélectionne un texte de sortie de manière aléatoire dans le tableau `values` et ne répète pas deux fois de suite le même texte de sortie. Prenons l'exemple d'un objet output.text qui contient trois valeurs. Pour les trois premières fois, une valeur aléatoire est sélectionnée mais jamais répétée. Une fois que toutes les valeurs de sortie sont définies, le système sélectionne une autre valeur de manière aléatoire et répète le processus.

        ```json
        {
            "output":{
                "text":{
                    "values":["Hello.","Hi.","Howdy!"],
                    "selection_policy":"random"
                }
            }
        }
        ```
        {: codeblock}

    Le système renvoie l'une de ces trois versions du message d'accueil, choisie de façon aléatoire. Au déclenchement suivant de la réponse, un autre message d'accueil pris dans cette liste s'affiche. Là encore, le message d'accueil est choisi de façon aléatoire, mais celui qui a été utilisé la fois précédente n'est volontairement pas répété.

    - **sequential** : le système renvoie le premier texte de sortie la première fois que le noeud de dialogue est déclenché, le deuxième texte de sortie la deuxième fois que le noeud est déclenché, et ainsi de suite.

        ```json
        {
            "output":{
                "text":{
                    "values":["Hello.", "Hi.", "Howdy!"],
                    "selection_policy":"sequential"
                }
            }
        }
        ```
        {: codeblock}

- **append** : indique si une valeur doit être ajoutée à un tableau ou si les valeurs du tableau doivent être écrasées par la ou les nouvelles valeurs. Lorsque cette propriété a pour valeur false, la sortie collectée dans les noeuds de dialogue précédemment exécutés est écrasée par la valeur de texte spécifiée dans ce noeud spécifique.

    ```json
    {
        "output":{
            "text":{
                "values": ["Hello."],
                "append":false
            }
        }
    }
    ```
    {: codeblock}

    En l'occurrence, tous les autres textes de sortie sont écrasés par ce texte de sortie.

Les principes qui régissent le comportement par défaut sont les suivants : `selection_policy = random` et `append = true`. Lorsque le tableau de valeurs contient plus d'un élément, le texte de sortie est sélectionné de façon aléatoire dans cette liste d'éléments.

### Réponse multimédia
{: #multimedia}

Si vous prévoyez d'intégrer le dialogue à Slack ou Facebook Messenger à l'aide du connecteur {{site.data.keyword.conversationshort}}, vous pouvez spécifier des réponses de noeud de dialogue incluant des éléments multimédia ou interactifs tels que des boutons cliquables. 

Pour plus d'informations, reportez-vous à la rubrique [Réponses multimédia](dialog-multimedia.html). 

## Définition de l'étape suivante
{: #jump-to}

Après avoir formulé la réponse spécifiée, vous pouvez demander au service d'effectuer l'une des actions suivantes :

- **Wait for user input** : le service attend que l'utilisateur fournisse une nouvelle entrée sollicitée par la réponse. Par exemple, la réponse peut poser une question à laquelle l'utilisateur doit répondre par oui ou par non. Le dialogue ne progresse pas tant que l'utilisateur ne fournit pas une autre entrée.
- **Skip user input** : utilisez cette option pour ignorer le message réclamant une entrée utilisateur et accéder directement au premier noeud enfant du noeud en cours. 

  **Remarque** : le noeud en cours doit avoir au moins un noeud enfant pour que cette option soit disponible. 

- **Jump to another dialog node** : utilisez cette option pour ignorer le message réclamant une entrée utilisateur et lorsque vous souhaitez que la conversion accède directement à un noeud de dialogue complètement différent. Vous pouvez utiliser une action *Jump to* pour acheminer le flux vers un noeud de dialogue commun à partir de plusieurs emplacements dans l'arborescence, par exemple. 

  **Remarque** : le noeud cible auquel vous souhaitez accéder directement doit exister avant que l'action Jump to puisse être configurée pour l'utiliser. 

### Configuration de l'action Jump to
{: #jump-to-config}

Si vous choisissez de passer directement à un autre noeud, vous devez spécifier si l'action cible la section **réponse** ou la section **condition** du noeud de dialogue sélectionné. 

- **Réponse** : si l'instruction cible la section réponse du noeud de dialogue sélectionné, celle-ci est exécutée immédiatement. Autrement dit, le système n'évalue pas la condition du noeud de dialogue sélectionné et exécute immédiatement la réponse du noeud de dialogue sélectionné. 

  Cibler la réponse est utile pour enchaîner plusieurs noeuds de dialogue. La réponse est traitée comme si la condition de ce noeud de dialogue avait pour valeur true. Si le noeud de dialogue sélectionné comporte une autre action **Jump to**, celle-ci est également exécutée immédiatement.

- **Condition** : si l'instruction cible la section condition du noeud de dialogue sélectionné, le service vérifie d'abord si la condition du noeud ciblé renvoie la valeur true. 
    - Si la condition renvoie la valeur true, le système traite le noeud cible immédiatement. 
    - Si la condition ne renvoie pas la valeur true, le système passe au noeud apparenté suivant du noeud cible afin d'évaluer sa condition et répète ce processus jusqu'à ce qu'il trouve un noeud de dialogue avec une condition qui renvoie la valeur true. 
    - Si le système traite tous les éléments apparentés et qu'aucune des conditions ne renvoie la valeur true, la stratégie de rétromigration de base est utilisée et le dialogue évalue les noeuds du niveau de base de l'arborescence. 

    Cibler la condition est utile pour enchaîner les conditions de noeuds de dialogue. Par exemple, vous souhaiterez peut-être vérifier d'abord si l'entrée contient une intention, par exemple, `#turn_on`, et si tel est le cas, vous souhaiterez peut-être vérifier si l'entrée contient des entités, par exemple, `@lights`, `@radio` ou `@wipers`. Enchaîner les conditions permet de structurer des arborescences de dialogue plus vastes.

## Informations complémentaires

Pour obtenir des informations sur le langage d'expression utilisé par un dialogue, et sur les méthodes, les entités de système, ainsi que d'autres informations utiles, reportez-vous à la section **Reference** dans le panneau de navigation. 
