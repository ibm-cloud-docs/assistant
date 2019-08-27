---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-12"

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

# Méthodes de langage d'expression
{: #dialog-methods}

Vous pouvez traiter les valeurs extraites des énoncés utilisateur que vous souhaitez référencer dans une variable contextuelle, une condition, ou ailleurs dans la réponse.
{: shortdesc}

## Où utiliser la syntaxe d'expression
{: #dialog-methods-evaluation-syntax}

Pour développer des valeurs de variable au sein d'autres variables, ou appliquer des méthodes à un texte de sortie ou à des variables contextuelles, utilisez la syntaxe d'expression `<? expression ?>`. Par exemple :

- **Référencement d'une entrée utilisateur à partir d'une réponse textuelle d'un nœud de dialogue**

  ```bash
  Vous avez dit <? input.text ?>.
  ```
  {: codeblock}

- **Incrémentation d'une propriété numérique à partir de l'éditeur JSON**

    ```json
    "output":{"number":"<? output.number + 1 ?>"}
    ```
    {: codeblock}

- **Ajout d'un élément à un tableau de variables contextuelles à partir de l'éditeur de contexte**

| Nom de la variable contextuelle | Valeur de la variable contextuelle |
|-----------------------|------------------------|
| `toppings` | `<? context.toppings.append( 'onions' ) ?>` |

Vous pouvez également utiliser des expressions SpEL dans les conditions de nœud de dialogue et les conditions de réponse de nœud de dialogue. Lorsqu'une expression est utilisée dans une condition, la syntaxe `<? ?>` environnante n'est pas requise.

- **Vérification d'une valeur d'entité spécifique à partir d'une condition de noeud de dialogue**

  ```bash
  @city.toLowerCase() == 'paris'
  ```
  {: codeblock}

- **Vérification d'une plage de dates spécifique à partir d'une condition de réponse de nœud de dialogue**

  ```bash
  @sys-date.after(today())
  ```
  {: codeblock}

Les sections ci-après décrivent les méthodes que vous pouvez utiliser pour traiter des valeurs. Elles sont organisées par type de données :

- [Tableaux](#dialog-methods-arrays)
- [Date et heure ](#dialog-methods-date-time)
- [Nombres](#dialog-methods-numbers)
- [Objets](#dialog-methods-objects)
- [Chaînes](#dialog-methods-strings)

## Tableaux
{: #dialog-methods-arrays}

Vous ne pouvez pas utiliser les méthodes ci-après pour rechercher une valeur dans un tableau dans une condition de noeud ou une condition de réponse au sein du noeud dans lequel vous définissez les valeurs de tableau.

### JSONArray.append(object)
{: #dialog-methods-arrays-append}

Cette méthode ajoute une nouvelle valeur à l'élément JSONArray et renvoie l'élément JSONArray modifié.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "toppings_array": ["oignons", "olives"]
  }
}
```
{: codeblock}

Effectuez la mise à jour suivante :

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.append('ketchup', 'tomates') ?>"
  }
}
```
{: codeblock}

Résultat :

```json
{
  "context": {
    "toppings_array": ["oignons", "olives", "ketchup", "tomates"]
  }
}
```
{: codeblock}

### JSONArray.clear()
{: #dialog-methods-arrays-clear}

Cette méthode efface toutes les valeurs du tableau et renvoie la valeur null.

Utilisez l'expression suivante dans la sortie pour définir une zone qui efface un tableau que vous avez enregistré dans une variable contextuelle ($toppings_array) de ses valeurs.

```json
{
  "output": {
    "array_eraser": "<? $toppings_array.clear() ?>"
  }
}
```
{: codeblock}

Si vous faites ensuite référence à la variable contextuelle $toppings_array, elle renvoie uniquement '[]'.

### JSONArray.contains(Object value)
{: #dialog-methods-arrays-contains}

Cette méthode renvoie la valeur true si l'élément JSONArray d'entrée contient la valeur d'entrée.

Pour le contexte d'exécution de dialogue suivant, qui est défini par un noeud précédent ou une application externe :

```json
{
  "context": {
    "toppings_array": ["oignons", "olives", "jambon"]
  }
}
```
{: codeblock}

Condition de noeud ou de réponse de dialogue :

```json
$toppings_array.contains('jambon')
```
{: codeblock}

Résultat : `True`, car le tableau contient l'élément jambon.

### JSONArray.containsIntent(String intent_name, Double min_score, [Integer top_n])
{: #dialog-methods-arrays-containsIntent}

Cette méthode renvoie `true` si le tableau JSONArray, `intents`, contient spécifiquement l'intention indiquée et que cette intention possède une cote de confiance égale ou supérieure à la cote minimale spécifiée. Vous pouvez éventuellement spécifier un nombre pour indiquer que l'intention doit être incluse dans le nombre d'éléments supérieurs du tableau.

Cette méthode renvoie `false` si l'intention spécifiée ne figure pas dans le tableau, si sa cote de confiance n'est pas supérieure ou égale à la cote de confiance minimale, ou si l'intention est inférieure dans le tableau à l'emplacement d'index spécifié.

Le service génère automatiquement un tableau d'`intentions` qui répertorie les intentions que le service détecte dans l'entrée chaque fois que l'entrée utilisateur est soumise. Le tableau répertorie en premier lieu toutes les intentions détectées par le service dans l'ordre de confiance la plus élevée.

Vous pouvez utiliser cette méthode dans une condition de noeud non seulement pour vérifier la présence d'une intention, mais également pour définir un seuil de cote de confiance à respecter afin que le noeud puisse être traité et que sa réponse soit renvoyée.

Par exemple, utilisez l'expression suivante dans une condition de noeud lorsque vous souhaitez déclencher le noeud de dialogue uniquement lorsque les conditions suivantes sont remplies :

- L'intention `#General_Ending` est présente.
- La cote de confiance de l'intention `#General_Ending` est supérieure à 80 %.
- L'intention `#General_Ending` est l'une des 2 meilleures intentions du tableau d'intentions.

```bash
intents.containsIntent("General_Ending", 0.8, 2)
```
{: codeblock}

### JSONArray.filter(temp, "temp.property operator comparison_value")
{: #dialog-methods-arrays-filter}

Filtre un tableau en comparant chaque valeur d'élément de tableau à une valeur que vous spécifiez. Cette méthode est similaire à une [projection de collection](#dialog-methods-collection-projection). Une projection de collection renvoie un tableau filtré basé sur un nom dans une paire nom-valeur d'élément de tableau. La méthode de filtrage renvoie un tableau filtré basé sur une valeur d'une paire nom-valeur d'élément de tableau.

L'expression de filtre comprend les valeurs suivantes :

- `temp` : nom d'une variable utilisée temporairement lorsque chaque élément du tableau est évalué. Par exemple, `city`.
- `property` : propriété d'élément que vous souhaitez comparer à `comparison_value`. Spécifiez la propriété en tant que propriété de la variable temporaire nommée dans le premier paramètre. Utilisez la syntaxe : `temp.property`. Par exemple, si `latitude` est un nom d'élément valide pour une paire nom-valeur du tableau, spécifiez la propriété sous la forme `city.latitude`.
- `operator` : opérateur à utiliser pour comparer la valeur de la propriété à `comparison_value`.

    Les opérateurs pris en charge sont :

    <table>
    <caption>Opérateurs de filtre pris en charge</caption>
    <tr>
      <th>Opérateur</th>
      <th>Description</th>
    </tr>
    <tr>
      <td>`==`</td>
      <td>Est égal à</td>
    </tr>
    <tr>
      <td>`>`</td>
      <td>Est supérieur à</td>
    </tr>
    <tr>
      <td>`<`</td>
      <td>Est inférieur à</td>
    </tr>
    <tr>
      <td>`>=`</td>
      <td>Est supérieur ou égal à</td>
    </tr>
    <tr>
      <td>`<=`</td>
      <td>Est inférieur ou égal à</td>
    </tr>
    <tr>
      <td>`!=`</td>
      <td>N’est pas égal à</td>
    </tr>
    </table>

- `comparison_value` : valeur à laquelle vous souhaitez comparer chaque valeur de propriété d'élément de tableau. Pour spécifier une valeur pouvant changer en fonction de l'entrée utilisateur, utilisez une variable contextuelle ou une entité comme valeur. Si vous spécifiez une valeur pouvant varier, ajoutez une logique pour garantir que la valeur `comparison_value` est valide au moment de l'évaluation, faute de quoi une erreur se produit.

#### Exemple de filtre 1

Par exemple, vous pouvez utiliser la méthode du filtre pour évaluer un tableau contenant un ensemble de noms de villes et leur population afin de renvoyer un tableau plus petit ne contenant que des villes de plus de 5 millions d’habitants.

La variable contextuelle `$cities` suivante contient un tableau d'objets. Chaque objet contient une propriété `name` et `population`.

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Rome",
      "population":2868104
   },
   {
      "name":"Beijing",
      "population":20693000
   },
   {
      "name":"Paris",
      "population":2241346
   }
]
```
{: codeblock}

Dans l'exemple suivant, le nom arbitraire de la variable temporaire est `city`. L'expression SpEL filtre le tableau `$cities` pour n'inclure que les villes de plus de 5 millions d'habitants :

```bash
$cities.filter("city", "city.population > 5000000")
```
{: codeblock}

L'expression renvoie le tableau filtré suivant :

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Beijing",
      "population":20693000
   }
]
```
{: codeblock}

Vous pouvez utiliser une projection de collection pour créer un nouveau tableau n'incluant que les noms de ville du tableau renvoyés par la méthode de filtrage. Vous pouvez ensuite utiliser la méthode `join` pour afficher les deux valeurs d'élément de nom du tableau sous forme de chaîne (String) et séparer les valeurs par une virgule et un espace.

```bash
Les villes de plus de 5 millions d’habitants sont <?  T(String).join(", ",($cities.filter("city", "city.population > 5000000")).![name]) ?>.
```
{: codeblock}

La réponse obtenue est la suivante : ` Les villes de plus de 5 millions d’habitants sont Tokyo et Beijing`

#### Exemple de filtre 2

La méthode de filtrage a l'avantage de ne pas nécessiter de coder la valeur `comparison_value`. Dans cet exemple, la valeur codée 5000000 est remplacée par une variable contextuelle.

Dans cet exemple, la variable contextuelle `$population_min` contient le nombre `5000000`. Le nom arbitraire de la variable temporaire est `city`. L'expression SpEL filtre le tableau `$cities` pour n'inclure que les villes de plus de 5 millions d'habitants :

```bash
$cities.filter("city", "city.population > $population_min")
```
{: codeblock}

L'expression renvoie le tableau filtré suivant :

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   },
   {
      "name":"Beijing",
      "population":20693000
   }
]
```
{: codeblock}

Lorsque vous comparez des valeurs numériques, veillez à définir la variable contextuelle impliquée dans la comparaison sur une valeur valide avant le déclenchement de la méthode de filtrage. Notez que `null` peut être une valeur valide si l’élément de tableau avec lequel vous la comparez peut la contenir. Par exemple, si la paire nom-valeur de population pour Tokyo est `"population":null`, et que l'expression de comparaison est `"city.population == $population_min"`, `null` serait une valeur valide pour la variable contextuelle `$population_min`.
{: tip}

Vous pouvez utiliser une expression de réponse de noeud de dialogue telle que :

```bash
Les villes de plus de $population_min habitants sont <?  T(String).join(", ",($cities.filter("city", "city.population > $population_min")).![name]) ?>.
```
{: codeblock}

La réponse obtenue est la suivante : ` Les villes de plus de 5000000 habitants sont Tokyo et Beijing`

#### Exemple de filtre 3

Dans cet exemple, un nom d'entité est utilisé comme `comparison_value`. L'entrée utilisateur est `Quel est le nombre d'habitants de Tokyo ?` Le nom arbitraire de la variable temporaire est `y`. Vous avez créé une entité nommée `@city` qui reconnaît les noms de ville, y compris `Tokyo`.

```bash
$cities.filter("y", "y.name == @city")
```

L'expression renvoie le tableau suivant :

```json
[
   {
      "name":"Tokyo",
      "population":9273000
   }
]
```
{: codeblock}

Vous pouvez utiliser un projet de collection pour obtenir un tableau contenant uniquement l'élément de population issu du tableau initial, puis utiliser la méthode `get` pour renvoyer la valeur de l'élément de population.

```bash
Le nombre d'habitants de @city est : <? ($cities.filter("y", "y.name == @city").![population]).get(0) ?>.
```
{: codeblock}

L'expression renvoie : `Le nombre d'habitants de Tokyo est 9273000.`

### JSONArray.get(Integer)
{: #dialog-methods-arrays-get}

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
  "generic":[
      {
      "values": [
        {
        "text" : "Le premier élément du tableau est <?$nested.array.get(0)?>"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
```
{: codeblock}

### JSONArray.getRandomItem()
{: #dialog-methods-arrays-getRandom}

Cette méthode renvoie un élément aléatoire à partir de l'élément JSONArray d'entrée.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "toppings_array": ["oignons", "olives", "jambon"]
  }
}
```
{: codeblock}

Sortie du noeud de dialogue :

```json
{
  "output": {
  "generic":[
      {
      "values": [
        {
    "text": "<? $toppings_array.getRandomItem() ?> est un excellent choix !"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
}
```
{: codeblock}

Résultat : `"jambon est un excellent choix !"` ou `"oignons est un excellent choix !"` ou `"olives est un excellent choix !"`

**Remarque :** le texte de sortie résultant est choisi de manière aléatoire.

### JSONArray.indexOf(value)
{: #dialog-methods-arrays-indexOf}

Cette méthode renvoie le numéro d'index de l'élément dans le tableau qui correspond à la valeur que vous spécifiez en tant que paramètre ou à `-1` si la valeur est introuvable dans le tableau. La valeur peut être une chaîne (String) (`"Ecole"`), un entier (Integer) (`8`), ou un double (`9.1`). La valeur doit être une correspondance exacte et est sensible à la casse.

Par exemple, les variables contextuelles suivantes contiennent des tableaux :

```json
{
  "context": {
    "array1": ["Marie","Agneau","Ecole"],
    "array2": [8,9,10],
    "array3": [8.1,9.1,10.1]
  }
}
```

Les expressions suivantes peuvent être utilisées pour déterminer l'index de tableau auquel la valeur est spécifiée :

```bash
<? $array1.indexOf("Marie") ?> renvoie `0`
<? $array2.indexOf(9) ?> renvoie `1`
<? $array3.indexOf(10.1) ?> renvoie `2`
```

Cette méthode peut être utile pour obtenir l'index d'un élément dans un tableau d'intentions, par exemple. Vous pouvez appliquer la méthode `indexOf` au tableau d'intentions généré chaque fois que l'entrée utilisateur est évaluée afin de déterminer le numéro d'index du tableau d'une intention spécifique.

```bash
intents.indexOf("General_Greetings")
```
{: codeblock}

Si vous souhaitez connaître la cote de confiance d'une intention spécifique, vous pouvez transmettre l'expression ci-dessus en tant que valeur d'*`index`* à une expression avec la syntaxe `intents[`*`index`*`].confidence`. Par exemple :

```bash
intents[intents.indexOf("General_Greetings")].confidence
```
{: codeblock}

### JSONArray.join(String delimiter)
{: #dialog-methods-arrays-join}

Cette méthode joint toutes les valeurs de ce tableau à une chaîne. Les valeurs sont converties en chaîne et délimitées par le délimiteur d'entrée.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "toppings_array": ["oignons", "olives", "jambon"]
  }
}
```
{: codeblock}

Sortie du noeud de dialogue :

```json
{
  "output": {
  "generic":[
      {
      "values": [
        {
    "text": "Voici le tableau : <? $toppings_array.join(';') ?>"
        }
      ],
      "response_type": "text",
      "selection_policy": "sequential"
    }
  ]
  }
}
```
{: codeblock}

Résultat :

```json
Voici le tableau : oignons;olives;jambon;
```
{: codeblock}

Si une entrée utilisateur mentionne plusieurs garnitures et que vous avez défini une entité nommée `@toppings` pouvant reconnaître les mentions de garnitures, vous pouvez utiliser l'expression suivante dans la réponse pour répertorier les garnitures mentionnées :

```json
Donc, vous voulez <? @toppings.values.join(',') ?>.
```
{: codeblock}

Si vous définissez une variable qui stocke plusieurs valeurs dans un tableau JSON, vous pouvez renvoyer un sous-ensemble de valeurs à partir du tableau, puis utiliser la méthode join() pour les formater correctement.

#### Projection de collection
{: #dialog-methods-collection-projection}

Une expression SpEL de `projection de collection` extrait une sous-collection d'un tableau contenant des objets. La syntaxe d'une projection de collection est `array_that_contains_value_sets.![value_of_interest]`.

Par exemple, la variable contextuelle suivante définit un tableau JSON qui stocke des informations de vol. Il y a deux points de données par vol, l'heure et le code de vol.

```json
"flights_found": [
  {
    "time": "10:00",
    "flight_code": "OK123"
  },
  {
    "time": "12:30",
    "flight_code": "LH421"
  },
  {
    "time": "16:15",
    "flight_code": "TS4156"
  }
]
```
{: codeblock}

Pour renvoyer les codes de vol uniquement, vous pouvez créer une expression de projection de collection à l'aide de la syntaxe suivante :

```
<? $flights_found.![flight_code] ?>
```

Cette expression renvoie un tableau des valeurs `flight_code` sous la forme `["OK123","LH421","TS4156"]`. Pour plus d'informations, reportez-vous à la [documentation sur la projection de collection SpEL](https://docs.spring.io/spring/docs/3.0.x/reference/expressions.html).

Si vous appliquez la méthode `join()` aux valeurs du tableau renvoyé, les codes de vol sont affichés sous forme de liste de valeurs séparées par des virgules. Par exemple, vous pouvez utiliser la syntaxe suivante dans une réponse :

```
Les vols correspondant à vos critères sont :
  <? T(String).join(",", $flights_found.![flight_code]) ?>.
```
{: codeblock}

Résultat : `Les vols correspondant à vos critères sont : OK123,LH421,TS4156.`

### JSONArray.joinToArray(template)
{: #dialog-methods-arrays-joinToArray}

Cette méthode applique le format que vous définissez dans un modèle au tableau et renvoie un tableau mis en forme conformément à vos spécifications. Cette méthode est utile pour appliquer la mise en forme aux valeurs de tableau que vous souhaitez renvoyer dans une réponse de dialogue, par exemple.

Le modèle peut être spécifié en tant que chaîne (String), objet JSON (JSON Object) ou tableau JSON (JSON Array). Pour référencer les valeurs du tableau que vous modifiez dans le modèle, respectez les conventions syntaxiques suivantes :

- `%` : Représente le début ou la fin d'un élément ou d'une propriété d'élément que vous voulez renvoyer du tableau en cours d'édition.
- `e` : représente temporairement l'élément de tableau auquel vous souhaitez appliquer la mise en forme. Ce nom de variable temporaire `e` ne peut pas être modifié.

Par exemple, une variable contextuelle contient un tableau avec une liste des détails de vol pour trois vols.

```json
"flights": [
      {
        "flight": "DL1040",
        "origin": "JFK",
        "carrier": "Alitalia",
        "duration": 485,
        "destination": "FCO",
        "arrival_date": "2019-02-03",
        "arrival_time": "07:00",
        "departure_date": "2019-02-02",
        "departure_time": "16:45"
      },
      {
        "flight": "DL1710",
        "origin": "JFK",
        "carrier": "Delta",
        "duration": 379,
        "destination": "LAX",
        "arrival_date": "2019-02-02",
        "arrival_time": "10:19",
        "departure_date": "2019-02-02",
        "departure_time": "07:00"
      },
      {
        "flight": "DL4379",
        "origin": "BOS",
        "carrier": "Virgin Atlantic",
        "duration": 385,
        "destination": "LHR",
        "arrival_date": "2019-02-03",
        "arrival_time": "09:05",
        "departure_date": "2019-02-02",
        "departure_time": "21:40"
      }
    ]
```
{: codeblock}

Vous souhaitez renvoyer uniquement la liste des codes de vol. Pour extraire uniquement la valeur de l'élément `flight` de chaque tableau et la renvoyer dans une liste, vous pouvez utiliser l'expression suivante :

```
Les vols disponibles sont <? $flights.joinToArray("%e.flight%"). ?>
```
{: codeblock}

La réponse du noeud de dialogue est `Les vols disponibles sont ["DL1040","DL1710","DL4379"].`

Pour afficher le tableau sous forme de texte, utilisez la méthode `join` dans l'expression suivante :

```
Les vols disponibles sont <? $flights.joinToArray("%e.flight%").join(", "). ?>
```
{: codeblock}

La réponse est `Les vols disponibles sont DL1040, DL1710, DL4379.`

#### Modèle complexe
{: #dialog-methods-complex-template}

Pour créer un modèle plus complexe, au lieu de spécifier directement les détails du modèle dans le paramètre de méthode, vous pouvez créer une variable contextuelle.

Cette variable contextuelle de modèle contient un sous-ensemble des éléments du tableau et les fait précéder d'intitulés, de sorte que les informations seront affichées dans une liste lisible dans la réponse :

```json
"template": "<br/>Numéro de vol : %e.flight% <br/> Compagnie aérienne : %e.carrier% <br/> Date de départ : %e.departure_date% <br/> Heure de départ : %e.departure_time% <br/> Heure d'arrivée : %e.arrival_time% <br/>"
```
{: codeblock}

La balise HTML `<br/>` de saut de ligne *n'est pas* affichée par certains des canaux d'intégration, notamment Facebook et Slack.
{: note}

Utilisez cette expression dans la réponse du noeud de dialogue pour appliquer le modèle défini dans `$template` au tableau dans `$flights`.

```
Les informations de vol sont <? $flights.joinToArray($template).join(" ") ?>
```
{: codeblock}

La réponse ressemble à ceci :

```
Les informations de vol sont
Numéro de vol : DL1040
Compagnie aérienne : Alitalia
Date de départ : 2019-02-02
Heure de départ : 16:45
Heure d'arrivée : 07:00

Numéro de vol : DL1710
Compagnie aérienne : Delta
Date de départ : 2019-02-02
Heure de départ : 07:00
Heure d'arrivée : 10:19

Numéro de vol : DL4379
Compagnie aérienne : Virgin Atlantic
Date de départ : 2019-02-02
Heure de départ : 21:40
Heure d'arrivée : 09:05
```
{: screen}

L'avantage d'utiliser cette méthode est que la fréquence à laquelle les valeurs du tableau changent ou si le nombre d'éléments dans le tableau augmente importe peu. Tant que chaque élément du tableau contient au moins le sous-ensemble de propriétés référencées par le modèle, l'expression est valide.

#### Exemple de modèle d'objet JSON
{: #dialog-methods-object-template}

Dans cet exemple, la variable contextuelle de modèle est définie en tant qu'objet JSON qui extrait le numéro de vol, ainsi que les dates et les heures d'arrivée et de départ de chacun des éléments de vol spécifiés dans le tableau de la variable contextuelle `$flights`. Vous pouvez utiliser cette approche pour appliquer un formatage standard aux détails de vol pour des vols gérés par deux compagnies différentes et qui mettent en forme les informations de vol différemment dans leurs services Web, par exemple.

```json
"template": {
      "departure": "Le vol %e.flight% part le %e.departure_date% à %e.departure_time%.",
      "arrival": "Le vol %e.flight% arrive le %e.arrival_date% à %e.arrival_time%."
    }
```
{: codeblock}

Vous souhaiterez peut-être concevoir votre application client personnalisée pour lire les objets du tableau renvoyé et formater les valeurs correctement pour la réponse de votre assistant. Votre réponse de noeud de dialogue peut renvoyer l'objet de détails d'arrivée de vol sous forme de tableau à l'aide de cette expression :

```
<? $flights.joinToArray($template) ?>
```
{: screen}

Voici la réponse du noeud de dialogue :

```json
[
  {
    "arrival":"Le vol DL1040 arrive le 2019-02-03 à 07:00.",
    "departure":"Le vol DL1040 part le 2019-02-02 à 16:45."
    },
  {
    "arrival":"Le vol DL1710 arrive le 2019-02-02 à 10:19.",
    "departure":"Le vol DL1710 part le 2019-02-02 à 07:00."
    },
  {
    "arrival":"Le vol DL4379 arrive le 2019-02-03 à 09:05.",
    "departure":"Le vol DL4379 part le 2019-02-02 à 21:40."
  ]
  ```

Notez que l'ordre des éléments `arrival` et `departure` est interverti dans la réponse. Le service réorganise généralement les éléments dans un objet JSON. Si vous souhaitez que les éléments soient renvoyés dans un ordre spécifique, définissez le modèle en utilisant plutôt un tableau JSON (JSON Array) ou une valeur de chaîne (String).

### JSONArray.remove(Integer)
{: #dialog-methods-arrays-remove}

Cette méthode retire l'élément de la position d'index dans l'élément JSONArray et renvoie l'élément JSONArray ainsi mis à jour.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "toppings_array": ["oignons", "olives"]
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
{: #dialog-methods-arrays-removeValue}

Cette méthode retire la première occurrence de la valeur dans l'élément JSONArray et renvoie l'élément JSONArray ainsi mis à jour.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "toppings_array": ["oignons", "olives"]
  }
}
```
{: codeblock}

Effectuez la mise à jour suivante :

```json
{
  "context": {
    "toppings_array": "<? $toppings_array.removeValue('oignons') ?>"
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

### JSONArray.set(Integer index, Object value)
{: #dialog-methods-arrays-set}

Cette méthode affecte la valeur d'entrée à l'index d'entrée de l'élément JSONArray et renvoie l'élément JSONArray ainsi modifié.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "toppings_array": ["oignons", "olives", "jambon"]
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
    "toppings_array": ["oignons", "ketchup", "jambon"]
  }
}
```
{: codeblock}

### JSONArray.size()
{: #dialog-methods-arrays-size}

Cette méthode renvoie la taille de l'élément JSONArray sous la forme d'un entier.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "toppings_array": ["oignons", "olives"]
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
{: #dialog-methods-arrays-split}

Cette méthode fractionne la chaîne d'entrée à l'aide de l'expression régulière d'entrée. Le résultat obtenu est un élément JSONArray composé de chaînes.

Pour l'entrée suivante :

```
"bananes;pommes;poires"
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
    "array": [ "bananes", "pommes", "poires" ]
  }
}
```
{: codeblock}

### Prise en charge de com.google.gson.JsonArray
{: #dialog-methods-arrays-com-google-gson-JsonArray}

En plus des méthodes intégrées, vous pouvez utiliser des méthodes standard de la classe `com.google.gson.JsonArray`.

#### Nouveau tableau
{: #dialog-methods-arrays-new}

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
{: #dialog-methods-date-time}

Plusieurs méthodes sont disponibles pour les dates et les heures.

Pour plus d'informations sur la procédure permettant de reconnaître et d'extraire des informations de date et d'heure à partir d'une entrée utilisateur, reportez-vous à la rubrique [Entités @sys-date et @sys-time](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-date-time).

### .after(date-heure de type Chaîne)
{: #dialog-methods-dates-after}

Détermine si la valeur date-heure figure après l'argument date-heure.

### .before(date-heure de type Chaîne)
{: #dialog-methods-dates-before}

Détermine si la valeur date-heure figure avant l'argument date-heure.

Par exemple :
- @sys-time.before('12:00:00')
- @sys-date.before('2016-11-21')

- Si elle compare différents éléments, par exemple, `time vs. date`, `date vs. time` et `time vs. date and time`, la méthode renvoie la valeur false et une exception est générée dans le journal JSON des réponses, `output.log_messages`.

  Par exemple, `@sys-date.before(@sys-time)`.
- Si elle compare `date and time vs. time`, la méthode ignore la date et compare uniquement les heures.

### now()
{: #dialog-methods-dates-now}

Renvoie une chaîne avec la date et l'heure en cours au format `aaaa-MM-jj HH:mm:ss`.

- Fonction statique.
- Les autres méthodes dates-heure peuvent être appelées sur les valeurs date-heure qui sont renvoyées par cette fonction et peuvent être transmises en tant qu'arguments.
- Si la variable contextuelle `$timezone` est définie, cette fonction renvoie des dates et des heures exprimées dans le fuseau horaire du client.

Exemple de noeud de dialogue avec `now()` utilisé dans la zone de sortie :

```json
{
  "conditions": "#what_time_is_it",
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "<? now() ?>"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Exemple de `now()` dans des conditions de noeud (pour décider si c'est encore le matin) :

```json
{
  "conditions": "now().before('12:00:00')",
  "output": {
      "generic":[
      {
        "values": [
          {
          "text": "Bonjour !"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
        }
      ]
  }
}
```
{: codeblock}

### .reformatDateTime(String format)
{: #dialog-methods-dates-reformatDateTime}

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
{: #dialog-methods-dates-sameMoment}

- Détermine si la valeur date-heure est identique à l'argument date-heure.

### .sameOrAfter(String date/time)
{: #dialog-methods-dates-sameOrAfter}

- Détermine si la valeur date-heure figure après ou est identique à l'argument date-heure.
- Semblable à `.after()`.

### .sameOrBefore(String date/time)
{: #dialog-methods-dates-sameOrBefore}

- Détermine si la valeur date-heure figure avant ou est identique à l'argument date-heure.

### today()
{: #dialog-methods-dates-today}

Renvoie une chaîne avec la date en cours au format `aaaa-MM-jj`.

- Fonction statique.
- Les autres méthodes de date peuvent être appelées sur les valeurs date qui sont renvoyées par cette fonction et peuvent être transmises en tant qu'arguments.
- Si la variable contextuelle `$timezone` est définie, cette fonction renvoie des dates exprimées dans le fuseau horaire du client. Sinon, le fuseau horaire `GMT` est utilisé.

Exemple de noeud de dialogue `today()` utilisé dans la zone de sortie :

```json
{
  "conditions": "#what_day_is_it",
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "La date du jour est <? today() ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Résultat : `La date du jour est 2018-03-09.`

## Calculs de date et heure
{: #dialog-methods-date-time-calculations}

Utilisez les méthodes suivantes pour calculer une date.

| Méthode                  | Description |
|-------------------------|-------------|
| `<date>.minusDays(n)`   | Renvoie la date du jour n nombre de jours avant la date spécifiée. |
| `<date>.minusMonths(n)` | Renvoie la date du jour n nombre de mois avant la date spécifiée. |
| `<date>.minusYears(n)`  | Renvoie la date du jour n nombre d'années avant la date spécifiée. |
| `<date>.plusDays(n)`   | Renvoie la date du jour n nombre de jours après la date spécifiée. |
| `<date>.plusMonths(n)` | Renvoie la date du jour n nombre de mois après la date spécifiée. |
| `<date>.plusYears(n)`  | Renvoie la date du jour n nombre d'années après la date spécifiée. |

où `<date>` est spécifié au format `aaaa-MM-jj` ou `aaaa-MM-jj HH:mm:ss`.

Pour obtenir la date de demain, spécifiez l'expression suivante :

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "La date de demain est <? today().plusDays(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Résultat si la date du jour est le 9 mars 2018 : `La date de demain est 2018-03-10.`

Pour obtenir la date du jour dans une semaine à partir d'aujourd'hui, spécifiez l'expression suivante :

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "La date de la semaine prochaine est <? @sys-date.plusDays(7) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Résultat si la date capturée par l'entité @sys-date est la date du jour, soit le 9 mars 2018 : `La date de la semaine prochaine est 2018-03-16.`

Pour obtenir la date du mois dernier, spécifiez l'expression suivante :

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Le mois dernier la date était <? today().minusMonths(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Résultat si la date du jour est le 9 mars 2018 : `Le mois dernier la date était 2018-02-9.`

Utilisez les méthodes suivantes pour calculer l'heure.

| Méthode                  | Description |
|-------------------------|-------------|
| `<time>.minusHours(n)`   | Renvoie l'heure n heures avant l'heure spécifiée. |
| `<time>.minusMinutes(n)` | Renvoie l'heure n minutes avant l'heure spécifiée. |
| `<time>.minusSeconds(n)`  | Renvoie l'heure n secondes avant l'heure spécifiée. |
| `<time>.plusHours(n)`   | Renvoie l'heure n heures après l'heure spécifiée. |
| `<time>.plusMinutes(n)` | Renvoie l'heure n minutes après l'heure spécifiée. |
| `<time>.plusSeconds(n)`  | Renvoie l'heure n secondes après l'heure spécifiée. |

où `<time>` est spécifié au format `HH:mm:ss`.

Pour obtenir l'heure dans une heure, spécifiez l'expression suivante :

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Dans une heure, il sera <? now().plusHours(1) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Résultat s'il est 8 heures du matin : `Dans une heure, il sera 09:00:00.`

Pour obtenir l'heure il y a 30 minutes, spécifiez l'expression suivante :

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Une demi-heure avant @sys-time, il était <? @sys-time.minusMinutes(30) ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Résultat si l'heure capturée par l'entité @sys-time est 8 heures du matin : `Une demi-heure avant 08:00:00, il était 07:30:00.`

Pour reformater l'heure renvoyée, vous pouvez utiliser l'expression suivante :

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Il y a 6 heures, il était <? now().minusHours(6).reformatDateTime('h:mm a') ?>."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Résultat s'il est 14h19 : `Il y a 6 heures, il était 8:19 AM.`

### Utilisation des intervalles de temps
{: #dialog-methods-time-spans}

Pour afficher une réponse selon que la date du jour se situe ou non dans une période donnée, vous pouvez utiliser une combinaison de méthodes liées au temps. Par exemple, si vous proposez annuellement une offre spéciale pendant les fêtes de fin d'année, vous pouvez vérifier si la date du jour se situe entre le 25 novembre et le 24 décembre de cette année. Commencez par définir les dates d’intérêt en tant que variables contextuelles.

Dans les expressions suivantes de variable contextuelle de date de début et de fin, la date est construite en concaténant la valeur de l'année en cours dérivée de manière dynamique avec des valeurs de mois et de jour codées.

```json
"context": {
   "end_date": "<? now().reformatDateTime('Y') + '-12-24' ?>",
   "start_date": "<? now().reformatDateTime('Y') + '-11-25' ?>"
 }
```

Dans la condition de réponse, vous pouvez indiquer que vous souhaitez afficher la réponse uniquement si la date du jour se situe entre les dates de début et de fin que vous avez définies en tant que variables contextuelles.

`now().after($start_date) && now().before($end_date)`

### Prise en charge de java.util.Date
{: #dialog-methods-dates-java-util-date}

En plus des méthodes intégrées, vous pouvez utiliser des méthodes standard de la classe `java.util.Date`.

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
{: #dialog-methods-numbers}

Ces méthodes vous permettent d'obtenir et de reformater les valeurs numériques.

Pour plus d'informations sur les entités de système qui peuvent reconnaître et extraire des nombres à partir d'une entrée utilisateur, reportez-vous à la rubrique [Entité @sys-number](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-number).

Si vous voulez que le service reconnaisse des formats numériques spécifiques dans une entrée utilisateur, tels que des références de numéro d'ordre, pensez à créer une entité de canevas pour les capturer. Pour plus d'informations, reportez-vous à la rubrique [Création d'entités](/docs/services/assistant?topic=assistant-entities).

Si vous souhaitez modifier l’emplacement décimal d’un nombre, par exemple, pour reformater un nombre en tant que valeur monétaire, reportez-vous à la [méthode String format()](#java.lang.String).

### toDouble()
{: #dialog-methods-numbers-toDouble}

  Convertit l'objet ou la zone en type Nombre double. Vous pouvez appeler cette méthode sur n'importe quel objet ou sur n'importe quelle zone. Si la conversion échoue, *null* est renvoyé.

### toInt()
{: #dialog-methods-numbers-toInt}

  Convertit l'objet ou la zone en type Nombre entier. Vous pouvez appeler cette méthode sur n'importe quel objet ou sur n'importe quelle zone. Si la conversion échoue, *null* est renvoyé.

### toLong()
{: #dialog-methods-numbers-toLong}

  Convertit l'objet ou la zone en type Nombre long. Vous pouvez appeler cette méthode sur n'importe quel objet ou sur n'importe quelle zone. Si la conversion échoue, *null* est renvoyé.

  Si vous spécifiez un type numérique Long dans une expression SpEL, vous devez ajouter un `L` au numéro pour l'identifier comme tel. Par exemple, `5000000000L`. Cette syntaxe est requise pour les nombres qui ne font pas partie du type Entier 32 bits. Par exemple, les nombres supérieurs à 2^31 (2,147,483,648) ou inférieurs à -2^31 (-2,147,483,648) sont considérés comme étant de type numérique Long. Les types numériques Long ont une valeur minimale de -2^63 et une valeur maximale de 2^63-1.

### Prise en charge des nombres Java
{: #dialog-methods-numbers-java}

### java.lang.Math()
{: #dialog-methods-numbers-java-lang-math}

Effectue des opérations numériques de base.

Vous pouvez utiliser les méthodes de classe, notamment les suivantes :

- max()

```json
{
  "context": {
    "bigger_number": "<? T(Math).max($number1,$number2) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Le nombre supérieur est $bigger_number."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
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
    "generic":[
      {
        "values": [
          {
          "text": "Le nombre inférieur est $smaller_number."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
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
    "generic":[
      {
        "values": [
          {
          "text": "Le nombre $base puissance deux est $power_of_two."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
}
```
{: codeblock}

Pour plus d'informations sur les autres méthodes, reportez-vous à la [documentation de référence java.lang.Math ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.oracle.com/javase/7/docs/api/java/lang/Math.html).

### java.util.Random()
{: #dialog-methods-numbers-java-util-random}

Renvoie un nombre aléatoire. Vous pouvez utiliser l'une des options de syntaxe suivantes :

- Pour renvoyer une valeur booléenne aléatoire (true ou false), utilisez `<?new Random().nextBoolean()?>`.
- Pour renvoyer un nombre double aléatoire compris entre 0 (inclus) et 1 (exclu), utilisez `<?new Random().nextDouble()?>`
- Pour renvoyer un nombre entier aléatoire compris entre 0 (inclus) et un nombre que vous spécifiez, utilisez `<?new Random().nextInt(n)?>` où n est le haut de la plage de chiffres souhaitée + 1. Par exemple, si vous voulez renvoyer un nombre aléatoire compris entre 0 et 10, spécifiez `<?new Random().nextInt(11)?>`. 
- Pour renvoyer un nombre entier aléatoire à partir de la plage de valeurs entières (-2147483648 à 2147483648), utilisez `<?new Random().nextInt()?>`. 

Par exemple, vous pouvez créer un noeud de dialogue qui est déclenché par l'intention #random_number. La première condition de réponse peut se présenter comme suit :

```json
Condition = @sys-number
{
  "context": {
    "answer": "<? new Random().nextInt(@sys-number.numeric_value + 1) ?>"
  },
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Voici un nombre aléatoire compris entre 0 et @sys-number.literal : $answer."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
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
{: #dialog-methods-objects}

### JSONObject.clear()
{: #dialog-methods-objects-jsonobject-clear}

Cette méthode efface toutes les valeurs de l'objet JSON et renvoie la valeur null.

Par exemple, vous souhaitez effacer les valeurs actuelles de la variable contextuelle $user.

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

Utilisez l'expression suivante dans la sortie pour définir une zone qui efface l'objet de ses valeurs.

```json
{
  "output": {
    "object_eraser": "<? $user.clear() ?>"
  }
}
```
{: codeblock}

Si vous faites ensuite référence à la variable contextuelle $user, elle renvoie uniquement `{}`.

Vous pouvez utiliser la méthode `clear()` sur les objets JSON `context` ou `output` dans le corps de l'appel d'API `/message`.

#### Effacement du contexte
{: #dialog-methods-clearing-context}

Lorsque vous utilisez la méthode `clear()` pour effacer l'objet `context`, elle efface **toutes** les variables à l'exception des variables suivantes :

 - `context.conversation_id`
 - `context.timezone`
 - `context.system`

**Avertissement** : "toutes les valeurs de variable contextuelle" signifie :

  - Toutes les valeurs par défaut définies pour les variables des noeuds ayant été déclenchés pendant la session en cours.
  - Toutes les mises à jour des valeurs par défaut avec les informations fournies par l'utilisateur ou des services externes pendant la session en cours.

Pour utiliser la méthode, vous pouvez la spécifier dans une expression d'une variable que vous définissez dans l'objet de sortie. Par exemple :

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Réponse pour ce noeud."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ],
    "context_eraser": "<? context.clear() ?>"
  }
}

```

#### Effacement de la sortie
{: #dialog-methods-clearing-output}

Lorsque vous utilisez la méthode `clear()` pour effacer l'objet `output`, elle efface toutes les variables sauf celle que vous utilisez pour effacer l'objet de sortie et toutes les réponses textuelles que vous définissez dans le noeud en cours. Elle n'efface pas non plus les variables suivantes :

- `output.nodes_visited`
- `output.nodes_visited_details`

Pour utiliser la méthode, vous pouvez la spécifier dans une expression d'une variable que vous définissez dans l'objet de sortie. Par exemple :

```json
{
  "output": {
    "generic":[
      {
        "values": [
          {
          "text": "Bonne journée !"
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ],
    "output_eraser": "<? output.clear() ?>"
  }
}
```

Si un noeud antérieur dans l’arborescence définit la réponse textuelle `Je serais heureux de vous aider.`, puis passe à un noeud avec l’objet de sortie JSON défini ci-dessus, seul le message `Bonne journée.` apparaît comme réponse. La sortie `Je serais heureux de vous aider.` n’est pas affichée car elle est effacée et remplacée par la réponse textuelle du noeud qui appelle la méthode `clear()`.

### JSONObject.has(String)
{: #dialog-methods-objects-jsonobject-has}

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

### JSONObject.remove(String)
{: #dialog-methods-objects-jsonobject-remove}

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
{: #dialog-methods-objects-com-google-gson-JsonObject}

En plus des méthodes intégrées, vous pouvez utiliser des méthodes standard de la classe `com.google.gson.JsonObject`.

## Chaînes
{: #dialog-methods-strings}

Il existe des méthodes qui vous aident à gérer du texte.

Pour plus d'informations sur la procédure permettant de reconnaître et d'extraire certains types de chaîne, par exemple, des noms de personne et des lieux, à partir d'une entrée utilisateur, reportez-vous à la rubrique [Entités de système](/docs/services/assistant?topic=assistant-system-entities).

**Remarque :** pour les méthodes impliquant des expressions régulières, reportez-vous à l'article [RE2 Syntax reference ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/google/re2/wiki/Syntax){: new_window} qui contient des informations détaillées sur la syntaxe à utiliser lorsque vous spécifiez l'expression régulière.

### String.append(Object)
{: #dialog-methods-strings-append}

Cette méthode ajoute un objet d'entrée à la chaîne sous forme de chaîne et renvoie une chaîne modifiée.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "my_text": "Ceci est un texte."
  }
}
```
{: codeblock}

La syntaxe suivante :

```json
{
  "context": {
    "my_text": "<? $my_text.append(' Texte supplémentaire.') ?>"
  }
}
```
{: codeblock}

Génère la sortie suivante :

```json
{
  "context": {
    "my_text": "Ceci est un texte. Texte supplémentaire."
  }
}
```
{: codeblock}

### String.contains(String)
{: #dialog-methods-strings-contains}

Cette méthode renvoie la valeur true si la chaîne contient la sous-chaîne d'entrée.

Entrée : "Oui j'aimerais y aller."

La syntaxe suivante :

```json
{
  "conditions": "input.text.contains('Oui')"
}
```
{: codeblock}

Résultat : la condition est `true`.

### String.endsWith(String)
{: #dialog-methods-strings-endsWith}

Cette méthode renvoie la valeur true si la chaîne se termine par la sous-chaîne d'entrée.

Pour l'entrée suivante :

```
"Comment vous appelez-vous ?".
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
{: #dialog-methods-strings-extract}

Cette méthode renvoie une chaîne de l'entrée qui correspond au canevas de groupe d'expressions régulières que vous spécifiez. Elle renvoie une chaîne vide si aucune correspondance n'est trouvée.

Cette méthode est conçue pour extraire des correspondances pour différents groupes de canevas d'expression régulière, et non pour des correspondances différentes pour un canevas d'expression régulière. Pour rechercher des correspondances différentes, reportez-vous à la méthode [getMatch](#dialog-methods-strings-getMatch).
{: note}

Dans cet exemple, la variable contextuelle permet de sauvegarder une chaîne qui correspond au groupe de canevas d'expression régulière que vous spécifiez. Dans l'expression, deux groupes de canevas d'expression régulière sont définis, chacun entre parenthèses. Il existe un troisième groupe inhérent qui est composé des deux groupes. Il s'agit du premier groupe d'expressions régulières(groupIndex 0) ; il correspond à une chaîne contenant le groupe de nombres complets et le groupe de textes. Le deuxième groupe d'expressions régulières (groupIndex 1) correspond à la première occurrence d'un groupe de nombres. Le troisième groupe (groupIndex 2) correspond à la première occurrence d'un groupe de textes après un groupe de nombres. 

```json
{
  "context": {
    "number_extract": "<? input.text.extract('([\\d]+)(\\b [A-Za-z]+)',n) ?>"
  }
}
```
{: codeblock}

Lorsque vous spécifiez l'expression régulière dans JSON, vous devez fournir deux barres obliques inversées (\\). Si vous spécifiez cette expression dans une réponse de noeud, vous avez besoin d'une seule barre oblique inverse. Par exemple : 

`<? input.text.extract('([\d]+)(\b [A-Za-z]+)',n) ?>`

Entrée :

```
"Bonjour 123 je suis 456".
```
{: codeblock}

Résultat :

- Lorsque n=`0`, la valeur est `123 je`.
- Lorsque n=`1`, la valeur est `123`.
- Lorsque n=`2`, la valeur est `je`.

### String.find(String regexp)
{: #dialog-methods-strings-find}

Cette méthode renvoie la valeur true si l'un des segments de la chaîne correspond à l'expression régulière d'entrée.  Vous pouvez appeler cette méthode sur un élément JSONArray ou JSONObject, le tableau ou l'objet sera converti en chaîne avant la comparaison.

Pour l'entrée suivante :

```
"Bonjour 123456".
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

### String.getMatch(String regexp, Integer matchIndex)
{: #dialog-methods-strings-getMatch}

Cette méthode renvoie une chaîne de l'entrée qui correspond à l'occurrence du canevas de groupe d'expressions régulières que vous spécifiez. Cette méthode renvoie une chaîne vide si aucune correspondance n'est trouvée.

Lorsque des correspondances sont trouvées, elles sont ajoutées à ce que vous pouvez considérer comme un *tableau de correspondances*. Si vous souhaitez renvoyer la troisième correspondance, car le nombre d'éléments du tableau commence à 0, spécifiez 2 comme valeur `matchIndex`. Par exemple, si vous entrez une chaîne de texte avec trois mots correspondant au modèle spécifié, vous pouvez renvoyer la première, la deuxième ou la troisième correspondance uniquement en spécifiant sa valeur d'index. 

Dans l'expression suivante, vous recherchez un groupe de nombres dans l'entrée. Cette expression enregistre la deuxième chaîne de correspondance de canevas dans la variable contextuelle `$second_number`, car la valeur d'index 1 est spécifiée.

```json
{
  "context": {
    "second_number": "<? input.text.getMatch('([\\d]+)',1) ?>"
  }
}
```
{: codeblock}

Lorsque vous spécifiez l'expression dans la syntaxe JSON, vous devez fournir deux barres obliques inversées (\\). Si vous spécifiez cette expression dans une réponse de noeud, vous avez besoin d'une seule barre oblique inversée.  

Par exemple : 

`<? input.text.getMatch('([\d]+)',1) ?>`

- Entrée utilisateur :

  ```
  "Bonjour 123 j'ai dit 456 et 8910".
  ```
  {: codeblock}

- Résultat : `456`

Dans cet exemple, l'expression recherche le troisième bloc de texte dans l'entrée.

`<? input.text.getMatch('(\b [A-Za-z]+)',2) ?>`

Pour la même entrée utilisateur, cette expression renvoie `and`.

### String.isEmpty()
{: #dialog-methods-strings-isEmpty}

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
{: #dialog-methods-strings-length}

Cette méthode renvoie le nombre de caractères de la chaîne.

Pour l'entrée suivante :

```
"Bonjour"
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

### String.matches(String regexp)
{: #dialog-methods-strings-matches}

Cette méthode renvoie la valeur true si la chaîne correspond à l'expression régulière d'entrée.

Pour l'entrée suivante :

```
"Hello".
```
{: codeblock}

La syntaxe suivante :

```json
{
  "conditions": "input.text.matches('^Bonjour$')"
}
```
{: codeblock}

Résultat : la condition est true car le texte d'entrée correspond à l'expression régulière `\^Bonjour\$`.

### String.startsWith(String)
{: #dialog-methods-strings-startsWith}

Cette méthode renvoie la valeur true si la chaîne débute par la sous-chaîne d'entrée.

Pour l'entrée suivante :

```
"Comment vous appelez-vous ?".
```
{: codeblock}

La syntaxe suivante :

```json
{
  "conditions": "input.text.startsWith('Comment')"
}
```
{: codeblock}

Résultat : la condition est `true`.

### String.substring(Integer beginIndex, Integer endIndex)
{: #dialog-methods-strings-substring}

Cette méthode extrait une sous-chaîne débutant par le caractère situé à `beginIndex` et se terminant par caractère défini pour l'index avant `endIndex`.
Le caractère endIndex n'est pas inclus.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "my_text": "Ceci est un texte."
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
    "my_text": "est un texte."
  }
}
```
{: codeblock}

### String.toLowerCase()
{: #dialog-methods-strings-toLowerCase}

Cette méthode renvoie la chaîne d'origine convertie en lettres minuscules.

Pour l'entrée suivante :

```
"Ceci est UN CHIEN !"
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
    "input_lower_case": "this is a dog!"
  }
}
```
{: codeblock}

### String.toUpperCase()
{: #dialog-methods-strings-toUpperCase}

Cette méthode renvoie la chaîne d'origine convertie en lettres majuscules.

Pour l'entrée suivante :

```
"salut".
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
    "input_upper_case": "SALUT"
  }
}
```
{: codeblock}

### String.trim()
{: #dialog-methods-strings-trim}

Cette méthode enlève les espaces au début et à la fin de la chaîne et renvoie la chaîne ainsi modifiée.

Pour le contexte d'exécution de dialogue suivant :

```json
{
  "context": {
    "my_text": "   quelque chose est ici    "
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
    "my_text": "quelque chose est ici"
  }
}
```
{: codeblock}

### Prise en charge de java.lang.String
{: #dialog-methods-strings-java-lang-String-format}

En plus des méthodes intégrées, vous pouvez utiliser des méthodes standard de la classe `java.lang.String`.

#### java.lang.String.format()
{: #dialog-methods-strings-java-lang-String-format}

Vous pouvez appliquer la méthode `format()` de chaîne Java à du texte. Pour plus d'informations sur la syntaxe à utiliser pour spécifier les détails de format, reportez-vous à l'article de référence [java.util.formatter![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://docs.oracle.com/javase/7/docs/api/java/util/Formatter.html#syntax){: new_window}.

Par exemple, l'expression suivante prend trois entiers décimaux (1, 1 et 2) et les ajoute à une phrase.

```json
{
  "formatted String": "<? T(java.lang.String).format('%d + %d equals %d', 1, 1, 2) ?>"
}
```
{: codeblock}

Résultat : `1 + 1 equals 2`.

Pour modifier la position décimale d'un nombre, utilisez la syntaxe suivante :

```
{
  <? T(String).format('%.2f',<number to format>) ?>
}
```
{: codeblock}

Par exemple, si la variable $number devant être formatée en dollars américains est `4.5`, une réponse du type `Votre total est $<? T(String).format('%.2f',$number) ?>` renvoie `Votre total est $4.50.`

## Conversion du type de données indirecte
{: #dialog-methods-indirect-type-conversion}

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

Vous pouvez ensuite exécuter des méthodes de tableau sur la variable $array, par exemple, `<? $array.removeValue('two') ?>` mais pas la variable $array_in_string. 
