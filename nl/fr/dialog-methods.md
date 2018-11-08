---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-05"

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

# Méthodes de langage d'expression

Vous pouvez traiter les valeurs extraites des énoncés d'utilisateur que vous souhaitez référencer dans une variable contextuelle, une condition, ou ailleurs dans la réponse.
{: shortdesc}

## Syntaxe d'évaluation

Pour développer des valeurs de variable au sein d'autres variables, ou appliquer des méthodes à un texte de sortie ou à des variables contextuelles, utilisez la syntaxe d'expression `<? expression ?>`. Par exemple :

- **Incrémentation d'une propriété numérique**
    - `"output":{"number":"<? output.number + 1 ?>"}`
- **Appel d'une méthode sur un objet**
    - `"context":{"toppings": "<? context.toppings.append( 'onions' ) ?>"}`

Les sections ci-après décrivent les méthodes que vous pouvez utiliser pour traiter des valeurs. Elles sont organisées par type de données :

- [Tableaux](dialog-methods.html#arrays)
- [Date et heure ](dialog-methods.html#date-time)
- [Nombres](dialog-methods.html#numbers)
- [Objets](dialog-methods.html#objects)
- [Chaînes](dialog-methods.html#strings)

## Tableaux
{: #arrays}

Vous ne pouvez pas utiliser les méthodes ci-après pour rechercher une valeur dans un tableau dans une condition de noeud ou une condition de réponse au sein du noeud dans lequel vous définissez les valeurs de tableau.

### JSONArray.append(object)

Cette méthode ajoute une nouvelle valeur à l'élément JSONArray et renvoie l'élément JSONArray modifié.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Effectuez la mise à jour suivante :

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
  }
}
```
{: codeblock}

Résultat :

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ketchup", "tomatoes"]
  }
}
```
{: codeblock}

### JSONArray.contains(object value)

Cette méthode renvoie la valeur true si l'élément JSONArray d'entrée contient la valeur d'entrée.

Pour le contexte d'exécution de dialogue suivant, qui est défini par un noeud précédent ou une application externe :

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Condition de noeud ou de réponse de dialogue :

```json
$toppings_array.contains('ham')
```
{: codeblock}

Résultat : `True`, car le tableau contient l'élément ham.

### JSONArray.get(integer)

Cette méthode renvoie un index d'entrée à partir de l'élément JSONArray.

Pour le contexte d'exécution de dialogue suivant, qui est défini par un noeud précédent ou une application externe :

```json
{
  "context": {
    "name": "John",
    "nested": {
      "array": [ "one", "two" ]
    }
  }
}
```
{: codeblock}

Condition de noeud ou de réponse de dialogue :

```json
$nested.array.get(0).getAsString().contains('one')
```
{: codeblock}

Résultat : `True`, car le tableau imbriqué contient `one` comme valeur.

Réponse :

```json
"output": {
    "text": {
      "values": [
        "The first item in the array is <?$nested.array.get(0)?>"
      ],
      "selection_policy": "sequential"
    }
  }
```
{: codeblock}

### JSONArray.getRandomItem()

Cette méthode renvoie un élément aléatoire à partir de l'élément JSONArray d'entrée.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Sortie du noeud de dialogue :

```json
{
  "output": {
    "text": "<? $toppings_array.getRandomItem() ?> is a great choice!"
  }
}
```
{: codeblock}

Résultat : `"ham is a great choice!"` ou `"onion is a great choice!"` ou `"olives is a great choice!"`

**Remarque :** le texte de sortie résultant est choisi de manière aléatoire.

### JSONArray.join(string delimiter)

Cette méthode joint toutes les valeurs de ce tableau à une chaîne. Les valeurs sont converties en chaîne et délimitées par le délimiteur d'entrée.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Sortie du noeud de dialogue :

```json
{
  "output": {
    "text": "This is the array: <? $toppings_array.join(';') ?>"
  }
}
```
{: codeblock}

Résultat :

```json
This is the array: onion;olives;ham;
```
{: codeblock}

### JSONArray.remove(integer)

Cette méthode retire l'élément de la position d'index dans l'élément JSONArray et renvoie l'élément JSONArray ainsi mis à jour.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Effectuez la mise à jour suivante :

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.remove(0) ?>"
  }
}
```
{: codeblock}

Résultat :

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: codeblock}

### JSONArray.removeValue(object)

Cette méthode retire la première occurrence de la valeur dans l'élément JSONArray et renvoie l'élément JSONArray ainsi mis à jour.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Effectuez la mise à jour suivante :

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
  }
}
```
{: codeblock}

Résultat :

```json
{
  "context": {
    "toppings_array": ["olives"]
  }
}
```
{: codeblock}

### JSONArray.set(integer index, object value)

Cette méthode affecte la valeur d'entrée à l'index d'entrée de l'élément JSONArray et renvoie l'élément JSONArray ainsi modifié.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "toppings_array": ["onion", "olives", "ham"]
  }
}
```
{: codeblock}

Sortie du noeud de dialogue :

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.set(1,'ketchup')?>"
  }
}
```
{: codeblock}

Résultat :

```json
{
  "context": {
    "toppings_array": ["onion", "ketchup", "ham"]
  }
}
```
{: codeblock}

### JSONArray.size()

Cette méthode renvoie la taille de l'élément JSONArray sous la forme d'un entier.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "toppings_array": ["onion", "olives"]
  }
}
```
{: codeblock}

Effectuez la mise à jour suivante :

```json
{
  "context": {
    "toppings_array_size": "<? $toppings_array.size() ?>"
  }
}
```
{: codeblock}

Résultat :

```json
{
  "context": {
    "toppings_array_size": 2
  }
}
```
{: codeblock}

### JSONArray split(expression régulière de type Chaîne)

Cette méthode fractionne la chaîne d'entrée à l'aide de l'expression régulière d'entrée. Le résultat obtenu est un élément JSONArray composée de chaînes.

Pour l'entrée suivante :

```
"bananas;apples;pears"
```
{: codeblock}

La syntaxe suivante :

```json
{
  "context": {
    "array": "<?input.text.split(";")?>
  }
}
```
{: codeblock}

Génère la sortie suivante :

```json
{
  "context": {
    "array": [ "bananas", "apples", "pears" ]
  }
}
```
{: codeblock}

### Prise en charge de com.google.gson.JsonArray
{: #com.google.gson.JsonArray}

En plus des méthodes intégrées, vous pouvez utiliser des méthodes standard de la classe `com.google.gson.JsonArray`. 

#### Nouveau tableau

new JsonArray().append('value')

Pour définir un nouveau tableau qui sera renseigné avec des valeurs fournies par les utilisateurs, vous pouvez instancier un tableau. Vous devez également ajouter une valeur de marque de réservation au tableau lorsque vous l'instanciez. Pour ce faire, vous pouvez utiliser la syntaxe suivante :

```json
{
  "context":{
    "answer": "<? output.answer?:new JsonArray().append('temp_value') ?>"
  }
```
{: codeblock}

## Date et heure
{: #date-time}

Plusieurs méthodes sont disponibles pour les dates et les heures.

Pour plus d'informations sur la procédure permettant de reconnaître et d'extraire des informations de date et d'heure à partir d'une entrée utilisateur, reportez-vous à la rubrique [Entités @sys-date et @sys-time](system-entities.html#sys-datetime).

### .after(date-heure de type Chaîne)

- Détermine si la valeur date-heure figure après l'argument date-heure.
- Semblable à `.before()`.

### .before(date-heure de type Chaîne)

- Par exemple :
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')
- Détermine si la valeur date-heure figure avant l'argument date-heure.
- Si elle compare différents éléments, par exemple, `time vs. date`, `date vs. time` et `time vs. date and time`, la méthode renvoie la valeur false et une exception est générée dans le journal JSON des réponses, `output.log_messages`.
    - Par exemple, `@sys-date.before(@sys-time)`.
- Si elle compare `date and time vs. time`, la méthode ignore la date et compare uniquement les heures.

### now()

- Fonction statique.
- Renvoie une chaîne avec la date et l'heure en cours au format `aaaa-MM-jj HH:mm:ss`.
- Les autres méthodes dates-heure peuvent être appelées sur les valeurs date-heure qui sont renvoyées par cette fonction et peuvent être transmises en tant qu'arguments.
- Si la variable contextuelle `$timezone` est définie, cette fonction renvoie des dates et des heures exprimées dans le fuseau horaire du client.

Exemple de noeud de dialogue avec `now()` utilisé dans la zone de sortie :

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "text": "<? now() ?>"
   }
}
```
{: codeblock}

Exemple de `now()` dans des conditions de noeud (pour décider si c'est encore le matin) :

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
    "text": "Good morning!"
   }
}
```
{: codeblock}

### .reformatDateTime(String format)

Met en forme les chaînes de date et d'heure au format souhaité pour la sortie utilisateur.

Renvoie une chaîne mise en forme selon le format spécifié :

- `MM/jj/aaaa` pour 12/31/2016
- `h a` pour 10pm

Pour renvoyer le jour de la semaine :

- `E` pour Tuesday
- `u` pour l'index de jour (1 = Monday, ..., 7 = Sunday)

Par exemple, cette définition de variable contextuelle crée une variable $time qui sauvegarde la valeur 17:30:00 sous *5:30 PM*.

```json
{
  "context": {
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

Le format suit les règles Java [SimpleDateFormat ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](http://docs.oracle.com/javase/6/docs/api/java/text/SimpleDateFormat.html){: new_window}. 

**Remarque** : lorsque le formatage porte uniquement sur l'heure, la date est interprétée comme `1970-01-01`.

### .sameMoment(String date/time)

- Détermine si la valeur date-heure est identique à l'argument date-heure.

### .sameOrAfter(String date/time)

- Détermine si la valeur date-heure figure après ou est identique à l'argument date-heure.
- Semblable à `.after()`.

### .sameOrBefore(String date/time)

- Détermine si la valeur date-heure figure avant ou est identique à l'argument date-heure.

### Prise en charge de java.util.Date
{: #java.util.Date}

En plus des méthodes intégrées, vous pouvez utiliser des méthodes standard de la classe `java.util.Date`. 

#### Calculs de date

Pour obtenir la date du jour dans une semaine à partir d'aujourd'hui, vous pouvez utiliser la syntaxe suivante :

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))) ?>"
  }
}
```
{: codeblock}

Cette expression extrait d'abord la date du jour en millisecondes (depuis le 1er janvier 1970, à 00:00:00 GMT). Elle calcule également le nombre de millisecondes dans 7 jours. (La formule `(24*60*60*1000L)` représente un jour en millisecondes.) Elle ajoute ensuite 7 jours à la date du jour. Le résultat obtenu correspond à la date complète du jour dans une semaine à partir d'aujourd'hui. Par exemple, `Fri Jan 26 16:30:37 UTC 2018`. Notez que l'heure est exprimée dans le fuseau horaire UTC (Temps Universel Coordonné). Vous pouvez toujours remplacer le 7 par une variable (par exemple, `$number_of_days`) que vous pouvez transmettre. Assurez-vous simplement que la valeur de cette expression soit définie avant son évaluation. 

Si vous souhaitez pouvoir ensuite comparer la date avec une autre date qui est générée par le service, vous devez reformater la date. Les entités de système (`@sys-date`) et d'autres méthodes intégrées (`now()`) convertissent les dates au format `yyyy-MM-dd` 

```json
{
  "context": {
    "week_from_today": "<? new Date(new Date().getTime() +
      (7 * (24*60*60*1000L))).format('yyyy-MM-dd') ?>"
  }
}
```
{: codeblock}

Après le reformatage de la date, le résultat est `2018-01-26`. A présent, vous pouvez utiliser une expression, telle que `@sys-date.after($week_from_today)`, dans une condition de réponse afin de comparer une date spécifiée dans une entrée utilisateur avec la date sauvegardée dans la variable contextuelle. 

L'expression suivante calcule l'heure qu'il sera dans 3 heures :

```json
{
  "context": {
    "future_time": "<? new Date(new Date().getTime() + (3 * (60*60*1000L)) -
      (5 * (60*60*1000L))).format('h:mm a') ?>"
  }
}
```
{: codeblock}

La valeur `(60*60*1000L)` représente une heure en millisecondes. Cette expression ajoute 3 heures à l'heure en cours. Ensuite, elle recalcule l'heure à partir d'un fuseau horaire UTC vers un fuseau horaire EST en soustrayant 5 heures de l'heure. Elle reformate également les valeurs de date afin d'inclure les heures et les minutes. 

## Nombres
{: #numbers}

Ces méthodes vous permettent d'obtenir et de reformater les valeurs numériques. 

Pour plus d'informations sur les entités de système qui peuvent reconnaître et extraire des nombres à partir d'une entrée utilisateur, reportez-vous à la rubrique [Entité @sys-number](system-entities.html#sys-number).

Si vous voulez que le service reconnaisse des formats numériques spécifiques dans une entrée utilisateur, tels que des références de numéro d'ordre, pensez à créer une entité de canevas pour les capturer. Pour plus d'informations, reportez-vous à la rubrique [Création d'entités](entities.html#creating-entities). 

### toDouble()

  Convertit l'objet ou la zone en type Nombre double. Vous pouvez appeler cette méthode sur n'importe quel objet ou sur n'importe quelle zone. Si la conversion échoue, *null* est renvoyé.

### toInt()

  Convertit l'objet ou la zone en type Nombre entier. Vous pouvez appeler cette méthode sur n'importe quel objet ou sur n'importe quelle zone. Si la conversion échoue, *null* est renvoyé.

### toLong()

  Convertit l'objet ou la zone en type Nombre long. Vous pouvez appeler cette méthode sur n'importe quel objet ou sur n'importe quelle zone. Si la conversion échoue, *null* est renvoyé.

  Si vous spécifiez un type numérique Long dans une expression SpEL, vous devez ajouter un `L` au numéro pour l'identifier comme tel. Par exemple, `5000000000L`. Cette syntaxe est requise pour les nombres qui ne font pas partie du type Entier 32 bits. Par exemple, les nombres supérieurs à 2^31 (2,147,483,648) ou inférieurs à -2^31 (-2,147,483,648) sont considérés comme étant de type numérique Long. Les types numériques Long ont une valeur minimale de -2^63 et une valeur maximale de 2^63-1.

### Prise en charge des nombres Java
{: #java.lang.Number}

### java.lang.Math()

Effectue des opérations numériques de base.

Vous pouvez utiliser les méthodes de classe, notamment les suivantes :

- max()

```json
{
  "context": {
    "bigger_number": "<? T(Math).max($number1,$number2) ?>"
  },
  "output": {
    "text": {
      "values": [
        "The bigger number is $bigger_number."
      ],
      "selection_policy": "sequential"
    }
  }
}
```
{: codeblock}

- min()

```json
{
  "context": {
    "smaller_number": "<? T(Math).min($number1,$number2) ?>"
  },
  "output": {
    "text": {
      "values": [
        "The smaller number is $smaller_number."
      ],
      "selection_policy": "sequential"
    }
  }
}
```
{: codeblock}

- pow()

```json
{
  "context": {
    "power_of_two": "<? T(Math).pow($base.toDouble(),2.toDouble()) ?>"
  },
  "output": {
    "text": {
      "values": [
        "Your number $base to the second power is $power_of_two."
      ],
      "selection_policy": "sequential"
    }
  }
}
```
{: codeblock}

Pour plus d'informations sur les autres méthodes, reportez-vous à la [documentation de référence java.lang.Math](https://docs.oracle.com/javase/7/docs/api/java/lang/Math.html). 

### java.util.Random()

Renvoie un nombre aléatoire. Vous pouvez utiliser l'une des options de syntaxe suivantes :

- Pour renvoyer une valeur booléenne aléatoire (true ou false), utilisez `<?new Random().nextBoolean()?>`.
- Pour renvoyer un nombre double aléatoire compris entre 0 (inclus) et 1 (exclus), utilisez `<?new Random().nextDouble()?>`
- Pour renvoyer un nombre entier aléatoire compris entre 0 (inclus) et un nombre que vous spécifiez, utilisez `<?new Random().nextInt(n)?>`, où n est la limite supérieure de la plage numérique souhaitée + 1.
  Par exemple, si vous voulez renvoyer un nombre aléatoire compris entre 0 et 10, spécifiez `<?new Random().nextInt(11)?>`.
- Pour renvoyer un nombre entier aléatoire à partir de la plage de valeurs entières (-2147483648 à 2147483648), utilisez `<?new Random().nextInt()?>`.

Par exemple, vous pouvez créer un noeud de dialogue qui est déclenché par l'intention #random_number. La première condition de réponse peut se présenter comme suit :

```json
Condition = @sys-number
{
  "context": {
    "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
  },
  "output": {
    "text": {
      "values": [
        "Here's a random number between 0 and @sys-number.literal: $answer."
      ],
      "selection_policy": "sequential"
    }
  }
}
```
{: codeblock}

Pour plus d'informations sur les autres méthodes, reportez-vous à la [documentation de référence java.util.Random](https://docs.oracle.com/javase/7/docs/api/java/util/Random.html). 

Vous pouvez également utiliser des méthodes standard des classes suivantes :

- `java.lang.Byte`
- `java.lang.Integer`
- `java.lang.Long`
- `java.lang.Double`
- `java.lang.Short`
- `java.lang.Float`

## Objets
{: #objects}

### JSONObject.has(string)

Cette méthode renvoie la valeur true si l'élément JSONObject complexe contient une propriété du nom d'entrée.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

Sortie du noeud de dialogue :

```json
{
  "conditions": "$user.has('first_name')"
}
```
{: codeblock}

Résultat : la condition est true car l'objet utilisateur contient la propriété `first_name`.

### JSONObject.remove(string)

Cette méthode retire une propriété du nom de l'élément `JSONObject` d'entrée. L'élément `JSONElement` qui est renvoyé par cette méthode est l'élément `JSONElement` en cours de retrait.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "user": {
      "first_name": "John",
      "last_name": "Snow"
    }
  }
}
```
{: codeblock}

Sortie du noeud de dialogue :

```json
{
  "context": {
    "attribute_removed": "<? $user.remove('first_name') ?>"
  }
}
```
{: codeblock}

Résultat :

```json
{
  "context": {
    "user": {
      "last_name": "Snow"
    },
    "attribute_removed": {
      "first_name": "John"
    }
  }
}
```
{: codeblock}

### Prise en charge de com.google.gson.JsonObject
{: #com.google.gson.JsonObject}

En plus des méthodes intégrées, vous pouvez utiliser des méthodes standard de la classe `com.google.gson.JsonObject`. 

## Chaînes
{: #strings}

Il existe des méthodes qui vous aident à gérer du texte.

Pour plus d'informations sur la procédure permettant de reconnaître et d'extraire certains types de chaîne, par exemple, des noms de personne et des lieux, à partir d'une entrée utilisateur, reportez-vous à la rubrique [Entités de système](system-entities.html).

**remarque :** pour les méthodes impliquant des expressions régulières, reportez-vous à l'article [RE2 Syntax reference ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/google/re2/wiki/Syntax){: new_window} qui contient des informations détaillées sur la syntaxe à utiliser lorsque vous spécifiez l'expression régulière. 

### String.append(object)

Cette méthode ajoute un objet d'entrée à la chaîne sous forme de chaîne et renvoie une chaîne modifiée.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

La syntaxe suivante :

```json
{
  "context": {
    "my_text": "<? $my_text.append(' More text.') ?>"
  }
}
```
{: codeblock}

Génère la sortie suivante :

```json
{
  "context": {
    "my_text": "This is a text. More text."
  }
}
```
{: codeblock}

### String.contains(string)

Cette méthode renvoie la valeur true si la chaîne contient la sous-chaîne d'entrée.

Entrée : "Yes, I'd like to go."

La syntaxe suivante :

```json
{
  "conditions": "input.text.contains('Yes')"
}
```
{: codeblock}

Résultat : la condition est `true`.

### String.endsWith(string)

Cette méthode renvoie la valeur true si la chaîne se termine par la sous-chaîne d'entrée.

Pour l'entrée suivante :

```
"What is your name?".
```
{: codeblock}

La syntaxe suivante :

```json
{
  "conditions": "input.text.endsWith('?')"
}
```
{: codeblock}

Résultat : la condition est `true`.

### String.extract(String regexp, Integer groupIndex)

Cette méthode renvoie une chaîne extraite par index de groupe spécifié de l'expression régulière d'entrée.

Pour l'entrée suivante :

```
"Hello 123456".
```
{: codeblock}

La syntaxe suivante :

```json
{
  "context": {
    "number_extract": "<? input.text.extract('[\\d]+',0) ?>"
  }
}
```
{: codeblock}

  **Important :** pour que `\\d` soit interprété comme une expression régulière, vous devez mettre en échappement les deux barres obliques inversées en ajoutant `\\` : `\\\\d`

Résultat :

```json
{
  "context": {
    "number_extract": "123456"
  }
}
```
{: codeblock}

### String.find(string regexp)

Cette méthode renvoie la valeur true si l'un des segments de la chaîne correspond à l'expression régulière d'entrée.  Vous pouvez appeler cette méthode sur un élément JSONArray ou JSONObject, le tableau ou l'objet sera converti en chaîne avant la comparaison.

Pour l'entrée suivante :

```
"Hello 123456".
```
{: codeblock}

La syntaxe suivante :

```json
{
  "conditions": "input.text.find('^[^\d]*[\d]{6}[^\d]*$')"
}
```
{: codeblock}

Résultat : la condition est true car la partie numérique du texte d'entrée correspond à l'expression régulière `^[^\d]*[\d]{6}[^\d]*$`.

### String.isEmpty()

Cette méthode renvoie la valeur true si la chaîne est une chaîne vide, mais pas null.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "my_text_variable": ""
  }
}
```
{: codeblock}

La syntaxe suivante :

```json
{
  "conditions": "$my_text_variable.isEmpty()"
}
```
{: codeblock}

Résultat : la condition est `true`.

### String.length()

Cette méthode renvoie le nombre de caractères de la chaîne.

Pour l'entrée suivante :

```
"Hello"
```
{: codeblock}

La syntaxe suivante :

```json
{
  "context": {
    "input_length": "<? input.text.length() ?>"
  }
}
```
{: codeblock}

Génère la sortie suivante :

```json
{
  "context": {
    "input_length": 5
  }
}
```
{: codeblock}

### String.matches(string regexp)

Cette méthode renvoie la valeur true si la chaîne correspond à l'expression régulière d'entrée.

Pour l'entrée suivante :

```
"Hello".
```
{: codeblock}

La syntaxe suivante :

```json
{
  "conditions": "input.text.matches('^Hello$')"
}
```
{: codeblock}

Résultat : la condition est true car le texte d'entrée correspond à l'expression régulière `\^Hello\$`.

### String.startsWith(string)

Cette méthode renvoie la valeur true si la chaîne débute par la sous-chaîne d'entrée.

Pour l'entrée suivante :

```
"What is your name?".
```
{: codeblock}

La syntaxe suivante :

```json
{
  "conditions": "input.text.startsWith('What')"
}
```
{: codeblock}

Résultat : la condition est `true`.

### String.substring(int beginIndex, int endIndex)

Cette méthode extrait une sous-chaîne débutant par le caractère situé à `beginIndex` et se terminant par caractère défini pour l'index avant `endIndex`.
Le caractère endIndex n'est pas inclus.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "my_text": "This is a text."
  }
}
```
{: codeblock}

La syntaxe suivante :

```json
{
  "context": {
    "my_text": "<? $my_text.substring(5, $my_text.length()) ?>"
  }
}
```
{: codeblock}

Génère la sortie suivante :

```json
{
  "context": {
    "my_text": "is a text."
  }
}
```
{: codeblock}

### String.toLowerCase()

Cette méthode renvoie la chaîne d'origine convertie en lettres minuscules.

Pour l'entrée suivante :

```
"This is A DOG!"
```
{: codeblock}

La syntaxe suivante :

```json
{
  "context": {
    "input_lower_case": "<? input.text.toLowerCase() ?>"
  }
}
```
{: codeblock}

Génère la sortie suivante :

```json
{
  "context": {
    "input_upper_case": "this is a dog!"
  }
}
```
{: codeblock}

### String.toUpperCase()

Cette méthode renvoie la chaîne d'origine convertie en lettres majuscules.

Pour l'entrée suivante :

```
"hi there".
```
{: codeblock}

La syntaxe suivante :

```json
{
  "context": {
    "input_upper_case": "<? input.text.toUpperCase() ?>"
  }
}
```
{: codeblock}

Génère la sortie suivante :

```json
{
  "context": {
    "input_upper_case": "HI THERE"
  }
}
```
{: codeblock}

### String.trim()

Cette méthode enlève les espaces au début et à la fin de la chaîne et renvoie la chaîne ainsi modifiée.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "my_text": "   something is here    "
  }
}
```
{: codeblock}

La syntaxe suivante :

```json
{
  "context": {
    "my_text": "<? $my_text.trim() ?>"
  }
}
```
{: codeblock}

Génère la sortie suivante :

```json
{
  "context": {
    "my_text": "something is here"
  }
}
```
{: codeblock}

### Prise en charge de java.lang.String
{: #java.lang.String}

En plus des méthodes intégrées, vous pouvez utiliser des méthodes standard de la classe `java.lang.String`. 

#### java.lang.String.format()

Vous pouvez appliquer la méthode `format()` de chaîne Java à du texte. Pour plus d'informations sur la syntaxe à utiliser pour spécifier les détails de format, reportez-vous à l'article de référence [java.util.formatter![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter.html#syntax){: new_window}. 

Par exemple, l'expression suivante prend trois entiers décimaux (1, 1 et 2) et les ajoute à une phrase. 

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d equals %d', 1, 1, 2) ?>"
}
```
{: codeblock}

Résultat : `1 + 1 equals 2`.

## Conversion du type de données indirecte

Lorsque vous ajoutez une expression dans le texte, dans le cadre d'une réponse de noeud, par exemple, la valeur est affichée sous forme de chaîne. Si vous souhaitez que l'expression soit affichée dans son type de données d'origine, ne placez pas du texte autour. 

Par exemple, Vous pouvez ajouter l'expression suivante à une réponse de noeud de dialogue pour renvoyer les entités qui sont reconnues dans l'entrée utilisateur au format chaîne :

```json
  The entities are <? entities ?>.
```
{: codeblock}

Si l'utilisateur spécifie *Hello now* comme entrée, les entités @sys-date et @sys-time sont déclenchées par la référence `now`. L'objet Entity est un tableau, mais étant donné que l'expression est incluse dans un texte, les entités sont renvoyées au format chaîne, comme suit :

```json
  The entities are 2018-02-02, 14:34:56.
```
{: codeblock}

Si vous n'incluez pas de texte dans la réponse, un tableau est renvoyé à la place. Par exemple, si la réponse est spécifiée en tant qu'expression uniquement et qu'elle n'est pas entourée de texte. 

```
  <? entities ?>
```
{: codeblock}

Les informations d'entité sont renvoyées dans leur type de données natif, sous forme de tableau. 

```json
[
  {
    "entity":"sys-date","location":[6,9],"value":"2018-02-02","confidence":1,"metadata":{"calendar_type":"GREGORIAN","timezone":"America/New_York"}
  },
  {
    "entity":"sys-time","location":[6,9],"value":"14:33:22","confidence":1,"metadata":{"calendar_type":"GREGORIAN","timezone":"America/New_York"}
  }
  ]
```
{: codeblock}

Autre exemple, la variable contextuelle $array suivante est un tableau, mais la variable contextuelle $string_array est une chaîne :

```json
{
  "context": {
    "array": [
      "one",
      "two"
    ],
    "array_in_string": "this is my array: $array"
  }
}
```
{: codeblock}

Si vous utilisez le panneau Try it out pour vérifier les valeurs de ces variables contextuelles, ces valeurs sont spécifiées comme suit :

**$array** : `["one","two"]`

**$array_in_string** : `"this is my array: [\"one\",\"two\"]"`

Vous pouvez ensuite exécuter des méthodes de tableau sur la variable $array, par exemple, `<? $array.removeValue('two') ?>`, mais pas sur la variable $array_in_string.
