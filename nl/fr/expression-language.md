---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-16"

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

# Expressions permettant d'accéder à des objets

Vous pouvez écrire des expressions permettant d'accéder à des objets et à des propriétés d'objets à l'aide du langage SpEL (Spring Expression). Pour plus d'informations, reportez-vous à la rubrique [Spring Expression Language (SpEL) ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window}.
{: shortdesc}

## Syntaxe d'évaluation

Pour développer des valeurs de variable au sein d'autres variables, ou appeler des méthodes sur des properties et des objets globaux, utilisez la syntaxe d'expression `<? expression ?>`. Par exemple :

- **Développement d'une propriété**
    - `"output":{"text":"Votre nom est <? context.userName ?>"}`

- **Appel de méthodes sur des propriétés d'objets globaux**
    - `"context":{"email": "<? @email.literal ?>"}`

## Syntaxe abrégée
{: #shorthand-syntax}

Apprenez à référencer rapidement les objets suivants à l'aide de la syntaxe abrégée SpEL :

- [Variables contextuelles](expression-language.html#shorthand-context)
- [Entités](expression-language.html#shorthand-entities)
- [Intentions](expression-language.html#shorthand-intents)

### Syntaxe abrégée pour les variables contextuelles
{: #shorthand-context}

Le tableau suivant présente des exemples de la syntaxe abrégée que vous pouvez utiliser pour écrire des variables contextuelles dans des expressions de condition.

| Syntaxe abrégée           | Syntaxe complète en SpEL                |
|----------------------------|-----------------------------------------|
| `$card_type`               | `context['card_type']`                  |
| `$(card-type)`             | `context['card-type']`                  |
| `$card_type:VISA`          | `context['card_type'] == 'VISA'`        |
| `$card_type:(MASTER CARD)` | `context['card_type'] == 'MASTER CARD'` |

Vous pouvez inclure des caractères spéciaux, par exemple, des traits d'union ou des points, dans des noms de variable contextuelle. Cependant, cela peut entraîner des problèmes lors de l'évaluation de l'expression SpEL. Le trait d'union peut être interprété comme un signe moins, par exemple. Pour éviter ce genre de problèmes, référencez la variable en utilisant la syntaxe d'expression complète ou la syntaxe abrégée `$(variable-name)` et n'utilisez pas les caractères spéciaux suivants dans le nom :

- Des parenthèses ()
- Plusieurs apostrophes ''
- Des guillemets "

### Syntaxe abrégée pour les entités
{: #shorthand-entities}

Le tableau suivant présente des exemples de la syntaxe abrégée que vous pouvez utiliser pour faire référence à des entités :

| Syntaxe abrégée    | Syntaxe complète en SpEL                      |
|---------------------|------------------------------------------|
| `@year`             | `entities['year']?.value`                |
| `@year == 2016`     | `entities['year']?.value == 2016`        |
| `@year != 2016`     | `entities['year']?.value != 2016`        |
| `@city == 'Boston'` | `entities['city']?.value == 'Boston'`    |
| `@city:Boston`      | `entities['city']?.contains('Boston')`   |
| `@city:(New York)`  | `entities['city']?.contains('New York')` |

En SpEL, le point d'interrogation `(?)` empêche une exception de pointeur null d'être déclenchée lorsqu'un objet d'entité est null.

Si la valeur d'entité que vous souhaitez rechercher contient un caractère `)`, vous ne pouvez pas utiliser l'opérateur `:` pour la comparaison.  Par exemple, si vous souhaitez vérifier si l'entité de ville est `Dublin (Ohio)`, vous devez utiliser `@city == 'Dublin (Ohio)'` au lieu de `@city:(Dublin (Ohio))`.

### Syntaxe abrégée pour les intentions
{: #shorthand-intents}

Le tableau suivant présente des exemples de la syntaxe abrégée que vous pouvez utiliser pour faire référence à des intentions :

| Syntaxe abrégée        | Syntaxe complète en SpEL |
|-------------------------|---------------------|
| `#help`                 | `intent == 'help'`  |
| `! #help`               | `intent != 'help'`  |
| `NOT #help`             | `intent != 'help'`  |
| `#help` ou `#i_am_lost` | <code>(intent == 'help' \|\| intent == 'I_am_lost')</code> |

## Variables globales intégrées
{: #builtin-vars}

Vous pouvez utiliser le langage d'expression pour extraire les informations de propriété des variables globales suivantes :

| Variable globale      | Définition |
|----------------------|------------|
| *context*            | Partie d'objet JSON du message de conversation traité. |
| *entities[ ]*        | Liste d'entités prenant en charge l'accès par défaut au 1er élément. |
| *input*              | Partie d'objet JSON du message de conversation traité. |
| *intents[ ]*         | Liste d'intentions prenant en charge l'accès par défaut au premier élément. |
| *output*             | Partie d'objet JSON du message de conversation traité. |

## Accès à des entités
{: #access-entity}

Le tableau d'intentions contient une ou plusieurs intentions qui sont reconnues dans l'entrée utilisateur. 

Lorsque vous testez votre dialogue, vous pouvez afficher les détails des entités qui sont reconnues dans les entrées utilisateur en spécifiant cette expression dans une réponse de noeud de dialogue :

```json
<? entities ?>
```
{: codeblock}

Pour l'entrée utilisateur *Hello now*, le service reconnaît les entités de système @sys-date et @sys-time, par conséquent, la réponse contient les objets d'entité suivants :

```json
[
{"entity":"sys-date","location":[6,9],"value":"2017-08-07",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}},
{"entity":"sys-time","location":[6,9],"value":"15:01:00",
  "confidence":1,"metadata":{"calendar_type":"GREGORIAN",
  "timezone":"America/New_York"}}
]
```
{: codeblock}

### Importance de l'emplacement des entités dans l'entrée

Utilisez l'expression SpEL complète si l'emplacement des entités dans l'entrée a de l'importance. La condition `entities['city']?.contains('Boston')` renvoie la valeur true lorsqu'au moins une entité de ville 'Boston' est trouvée dans toutes les entités @city, quel que soit son emplacement.

Par exemple, un utilisateur soumet l'entrée suivante : `"Je souhaite me rendre de Toronto à Boston."` Les entités `@city:Toronto` et `@city:Boston` sont détectées et représentées dans les entités suivantes :

- `entities.city[0].value = 'Toronto'`
- `entities.city[1].value = 'Boston'`

La condition `@city.contains('Boston')` dans un noeud de dialogue renvoie la valeur true même si l'entité Boston est détectée en deuxième.

### Propriétés d'entité

Un groupe de propriétés est associé à chaque entité. Vous pouvez accéder aux informations sur une entité via ses propriétés.

| Propriété              | Définition | Conseils d'utilisation |
|-----------------------|------------|------------|
| *confidence*          | Pourcentage décimal qui représente la confiance du service dans l'entité reconnue. La cote de confiance relative à une entité est 0 ou 1, sauf si vous avez activé la fonction Fuzzy Matching pour les entités. Lorsque la fonction Fuzzy Matching est activée, le seuil de la cote de confiance par défaut est 0.3. Que la fonction Fuzzy Matching soit ou non activée, les entités de système ont toujours une cote de confiance égale à 1.0. | Vous pouvez utiliser cette propriété dans une condition de sorte que celle-ci renvoie la valeur false si la cote de confiance n'est pas supérieure à un pourcentage que vous spécifiez. |
| *location*            | Un décalage de caractère basé sur des zéros indiquant où les valeurs d'entité détectées commencent et finissent dans le texte d'entrée. | Utilisez `.literal` pour extraire le passage de texte entre les valeurs de début et de fin qui sont stockées dans la propriété location. |
| *value*               | Valeur d'entité identifiée dans l'entrée. | Cette propriété renvoie la valeur d'entité telle qu'elle est définie dans les données d'apprentissage, même si la correspondance a été établie avec l'un des synonymes qui lui sont associés. Vous pouvez utiliser `.values` pour capturer plusieurs occurrences d'une entité qui peuvent être présentes dans l'entrée utilisateur. |

### Exemples d'utilisation de propriété d'entité
Dans les exemples ci-dessous, l'espace de travail contient une entité d'aéroport ayant pour valeur JFK et le synonyme 'aéroport Kennedy". L'entrée utilisateur est la suivante : *Je souhaite me rendre à l'aéroport Kennedy*.

- Afin de renvoyer une réponse spécifique si l'entité 'JFK' est reconnue dans l'entrée utilisateur, vous pouvez ajouter cette expression à la condition de réponse :
  `entities.airport[0].value == 'JFK'`
  or
  `@airport = "JFK"`
- Afin de renvoyer le nom d'entité tel qu'il a été spécifié par l'utilisateur dans la réponse de dialogue, utilisez la propriété .literal :
  `Vous souhaitez donc vous rendre à l' <?entities.airport[0].literal?>...`
  ou
  `Vous souhaitez donc vous rendre à l'@airport.literal ...`

  Avec ces deux formats, la réponse renvoyée est 'Vous souhaitez donc vous rendre à l'aéroport Kennedy...'. 

- Des expressions comme `@airport:(JFK)` ou `@airport.contains('JFK')` font toujours référence à la **valeur** de l'entité (`JFK` dans cet exemple).
- Pour restreindre les termes qui sont identifiés comme des aéroports dans l'entrée lorsque la fonction Fuzzy Matching est activée, vous pouvez spécifier cette expression dans une condition de noeud, par exemple :`@airport && @airport.confidence > 0.7`. Le noeud ne sera exécuté que si le service est confiant à 70 % que le texte d'entrée contient une référence à un aéroport.

Dans l'exemple suivant, l'entrée utilisateur est *Est-il possible de changer des devises à JFK, Logan et O'Hare ?*

- Afin de capturer plusieurs occurrences d'un type d'entité dans l'entrée utilisateur, utilisez une syntaxe semblable à celle présentée ci-dessous :

    ```json
    "context":{
      "airports":"@airport.values"
    }
    ```

  Pour faire référence ultérieurement à la liste capturée dans une réponse de dialogue, utilisez la syntaxe suivante :
  `Vous avez demandé des informations sur les aéroports suivants : <? $airports.join(', ') ?>.`
  Cela s'affiche comme suit :
  `Vous avez demandé des informations sur les aéroports suivants : JFK, Logan, O'Hare.`

## Accès à des intentions
{: #access-intent}

Le tableau d'intentions contient une ou plusieurs intentions qui sont reconnues dans l'entrée utilisateur, triées par ordre croissant de côte de confiance. 

Chaque intention ne contient qu'une seule propriété, nommée `confidence`. La propriété confidence est un pourcentage décimal qui représente la cote de confiance du service dans l'intention reconnue.

Lorsque vous testez votre dialogue, vous pouvez afficher les détails des intentions qui sont reconnues dans les entrées utilisateur en spécifiant cette expression dans une réponse de noeud de dialogue :

```json
<? intents ?>
```
{: codeblock}

Pour l'entrée utilisateur *Hello now*, le service trouve une correspondance exacte avec l'intention #greeting. Par conséquent, il répertorie en premier les détails de l'objet d'intention #greeting. La réponse inclut également les 10 autres premières intentions définies dans la compétence, quelle que soit leur cote de confiance. (Dans cet exemple, la cote de confiance du service pour les autres intentions a pour valeur 0 car la première intention est une correspondance exacte.) Les 10 premières intentions sont renvoyées car le panneau"Try it out" envoie le paramètre `alternate_intents:true` avec sa demande. Si vous utilisez directement l'API et que vous souhaitez voir les 10 premiers résultats, prenez soin de spécifier ce paramètre dans votre appel. Si `alternate_intents` a pour valeur false (valeur par défaut), seules les intentions ayant une cote de confiance supérieure à 0.2 sont renvoyées dans le tableau. 

```json
[{"intent":"greeting","confidence":1},
{"intent":"yes","confidence":0},
{"intent":"pizza-order","confidence":0}]
```
{: codeblock}

Les exemples suivants montrent comment rechercher une valeur d'intention :

- `intents[0] == 'Help'`
- `intent == 'Help'`

`intent == 'help'` est différent de `intents[0] == 'help'` car `intent == 'help'` n'émet pas une exception si aucune intention n'est détectée. L'intention renvoie la valeur true uniquement si sa cote de confiance est supérieure à un seuil.  Le cas échéant, vous pouvez spécifier une cote de confiance personnalisée pour une condition, par exemple, `intents.size() > 0 && intents[0] == 'help' && intents[0].confidence > 0.1`

## Accès à une entrée
{: #access-input}

L'objet JSON d'entrée contient une seule propriété, text. La propriété text représente le texte de l'entrée utilisateur.

### Exemples d'utilisation de propriété d'entrée

L'exemple suivant montre comment accéder à une entrée :

- Pour exécuter un noeud si l'entrée utilisateur est "Yes", ajoutez l'expression suivante au condition de noeud :
  `input.text == 'Yes'`

Vous pouvez utiliser n'importe laquelle des [méthodes String](/docs/services/conversation/dialog-methods.html#strings) pour évaluer ou manipuler le texte de l'entrée utilisateur. Par exemple :

- Pour vérifier si l'entrée utilisateur contient "Yes", utilisez : `input.text.contains( 'Yes' )`.
- La valeur true est renvoyée si l'entrée utilisateur est un nombre : `input.text.matches( '[0-9]+' )`.
- Pour vérifier si la chaîne d'entrée contient dix caractères, utilisez : `input.text.length() == 10`.
