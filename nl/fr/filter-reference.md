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

# Référence pour la requête de filtre
{: #filter-reference}

L'API REST du service {{site.data.keyword.conversationshort}} offre des capacités de recherche puissantes dans des journaux via des requêtes de filtre. Vous pouvez utiliser le paramètre `filter` de l'API /logs pour rechercher dans le journal de votre compétence des événements qui correspondent à une requête spécifiée.

Le paramètre `filter` est une requête pouvant être placée dans la mémoire cache qui permet d'afficher uniquement les correspondances avec le filtre spécifié. Vous pouvez effectuer un filtrage sur n'importe quel objet qui faire partie du modèle de réponse JSON (par exemple, le texte entré par l'utilisateur, les intentions et les entités détectées ou la cote de confiance).

Pour voir des exemples des différents types de requêtes de filtre, reportez-vous à la section [Exemples](#filter-reference-examples).

Pour plus d'informations sur la méthode `GET` /logs et son modèle de réponse, reportez-vous au [site de référence pour l'API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/apidocs/assistant?curl=#list-log-events-in-a-workspace){: new_window}.

## Syntaxe de la requête de filtre
{: #filter-reference-syntax}

L'exemple suivant illustre le format général d'une requête de filtre :

| Emplacement           | Opérateur de requête | Terme         |
|:------------------:|:--------------:|:------------:|
|`request.input.text`|`::`            |`"IBM Watson"`|

- L'_emplacement_ identifie la zone sur laquelle doit porter le filtrage (en l'occurrence, `request.input.text`).
- L'_opérateur de requête_ spécifie le type de correspondance que vous souhaitez utiliser (fonction Fuzzy Matching ou Exact Matching).
- Le _terme_ indique l'expression ou la valeur que vous souhaitez utiliser afin d'évaluer la zone pour correspondance. Le terme peut contenir un texte littéral et des opérateurs, comme décrit dans la [section suivante](#filter-reference-operators).

La syntaxe requise pour le filtrage par intention ou entité est légèrement différente de celle utilisée pour le filtrage sur d'autres zones. Pour plus d'informations, reportez-vous à la section [Filtrage sur des intentions ou des entités](#filter-reference-intent-entity).

**Remarque :** la syntaxe de la requête de filtre utilise certains caractères qui ne sont pas autorisés dans des requêtes HTTP. Assurez-vous que tous les caractères spéciaux, y compris les espaces et les apostrophes, sont codés sous la forme d'une URL lorsqu'ils sont envoyés en tant que partie d'une requête HTTP. Par exemple, le filtre `response_timestamp<2016-11-01` devrait être spécifié comme suit : `response_timestamp%3C2016-11-01`.

## Opérateurs
{: #filter-reference-operators}

Vous pouvez utiliser les opérateurs suivants dans votre requête de filtre :

| Opérateur | Description |
|:-------------------:|-----------|
| `:` | Opérateur pour une requête de correspondance floue. Préfixez le terme de requête avec `:` si vous souhaitez établir une correspondance avec n'importe quelle valeur contenant le terme de requête ou une variante grammaticale du terme de requête. La fonction Fuzzy Matching est disponible pour le texte d'entrée utilisateur, le texte de sortie utilisateur et les valeurs d'entité. |
| `::` | Opérateur pour une requête de correspondance exacte. Préfixez le terme de requête avec `::` si vous souhaitez établir une correspondance uniquement avec les valeurs parfaitement identiques au terme de requête. |
| `:!` | Opérateur pour une requête de correspondance floue négative. Préfixez le terme de requête avec `:!` si vous souhaitez établir une correspondance uniquement avec les valeurs qui ne contiennent _pas_ le terme de requête ou une variante grammaticale du terme de requête. |
| `::!` | Opérateur pour une requête de correspondance exacte négative. Préfixez le terme de requête avec `::!` si vous souhaitez établir une correspondance uniquement avec les valeurs qui ne sont _pas_ tout à fait identiques au terme de requête. |
| `<=`, `>=`, `>`, `<` | Opérateurs de comparaison. Préfixez le terme de requête avec ces opérateurs pour établir une correspondance en fonction d'une comparaison arithmétique. |
| `\` | Opérateur de mise en échappement. Utilisez cet opérateur dans des requêtes incluant des caractères de commande qui autrement, seraient interprétés comme des opérateurs. Par exemple, `\!hello` correspondrait au texte `!hello`. |
| `""` | Expression littérale. Permet d'englober un terme de requête qui contient des espaces ou d'autres caractères spéciaux. Aucun des caractères spéciaux inclus entre les guillemets n'est analysé par l'API. |
| `~` | Correspondance approximative. Ajoutez cet opérateur suivi de `1` ou `2` à la fin du terme de requête afin de spécifier le nombre autorisé de différences de caractère unique entre la chaîne de requête et une correspondance dans l'objet de réponse. Par exemple, `car~1` correspondrait à`car`, `cat` ou `cars`, mais pas à `cats`. Cet opérateur n'est pas valide pour le filtrage sur `log_id` ou sur des zones de date ou d'heure ou avec la fonction Fuzzy Matching. |
| `*` | Opérateur de caractère générique. Correspond à n'importe quelle séquence de zéro caractère ou plus. Cet opérateur n'est pas valide pour le filtrage sur `log_id` ou sur des zones de date ou d'heure. |
| `()`, `[]` | Opérateurs de regroupement. Permettent d'englober un regroupement logique de plusieurs expressions avec des opérateurs booléens. |
| `|` | Opérateur _or_ booléen. |
| `,` | Opérateur _and_ booléen. |

### Filtrage sur des intentions ou des entités
{: #filter-reference-intent-entity}

En raison du mode de stockage spécifique pour les intentions et les entités, la syntaxe de filtrage sur une intention ou une entité spécifique est différente de celle utilisée pour les autres zones du fichier JSON renvoyé. Pour spécifier une zone d'`intention` ou d'`entité` au sein d'une collectiond'`intentions` ou d'`entités`, vous devez utiliser l'opérateur de correspondance `:` au lieu d'un point.

Par exemple, la requête suivante établit une correspondance avec tous les événements consignés pour lesquels la réponse inclut une intention détectée nommée `hello` :

`response.intents:intent::hello`

Notez l'utilisation de l'opérateur `:` au lieu d'un point (intents:intent).

Utilisez le même canevas pour établir une correspondance avec n'importe quelle zone d'une intention ou d'une entité détectée dans la réponse. Par exemple, la requête suivante établit une correspondance avec tous les événements consignés pour lesquels la réponse inclut une entité détectée nommée avec la valeur `soda` :

`response.entities:value::soda`

De même, vous pouvez effectuer un filtrage sur des intentions ou des entités envoyées en tant que partie de la requête, comme dans l'exemple suivant :

`request.intents:intent::hello`

Le filtrage sur des intentions fonctionne sur toutes les intentions détectées. Pour effectuer un filtrage uniquement sur l'intention détectée ayant la cote de confiance la plus élevée, vous pouvez utiliser la syntaxe abrégée `response.top_intent`. Par exemple :

`response.top_intent::goodbye`

### Filtrage par ID client 
{: #filter-reference-customer-id}

Pour filtrer par ID client, utilisez l'emplacement spécial `customer_id`. (Pour plus d'informations sur l'étiquetage des messages avec un ID client, voir [Sécurité des informations](/docs/services/assistant?topic=assistant-information-security)).

### Filtrage sur d'autres zones
{: #filter-reference-fields}

Pour effectuer un filtrage sur n'importe quelle autre zone contenue dans les données de journal, spécifiez son emplacement sous la forme d'un chemin identifiant les niveaux d'objets imbriqués dans la réponse JSON de l'API /logs. Utilisez des points (`.`) pour spécifier des niveaux successifs d'imbrication dans les données JSON. Par exemple, l'emplacement `request.input.text` identifie la zone de texte d'entrée utilisateur comme illustré dans le fragment JSON suivant :

```json
  "request": {
    "input": {
      "text": "Good morning"
    }
  }
```

## Exemples
{: #filter-reference-examples}

Les exemples suivants illustrent diverses types de requêtes qui utilisent cette syntaxe :

| Description | Requête |
|---------|-----------|
| La date de la réponse est une date du mois de juillet 2017. | `response_timestamp>=2017-07-01,response_timestamp<2017-08-01` |
| L'horodatage de la réponse est antérieur à `2016-11-01T04:00:00.000Z`. | `response_timestamp<2016-11-01T04:00:00.000Z` |
| Le message est étiqueté avec l'ID client `my_id`. | `customer_id::my_id` |
| Le texte d'entrée utilisateur contient le mot "order" ou une variante grammaticale (par exemple, `orders` ou `ordering`). | `request.input.text:order` |
| Un nom d'intention dans la réponse correspond exactement à `place_order`. | `response.intents:intent::place_order` |
| Un nom d'entité dans la réponse correspond exactement à `beverage`.  | `response.entities:entity::beverage` |
| Le texte d'entrée utilisateur ne contient pas le mot "order" ou une variante grammaticale. | `request.input.text:!order` |
| Le nom de l'intention détectée ayant la cote de confiance la plus élevée ne correspond pas tout à fait à `hello`. | `response.top_intent::!hello` |
| Le texte d'entrée utilisateur contient la chaîne `!hello`. | `request.input.text:\!hello` |
| Le texte d'entrée utilisateur contient la chaîne `IBM Watson`. | `request.input.text:"IBM Watson"` |
| Le texte d'entrée utilisateur contient une chaîne qui ne comporte pas plus de 2 différences de caractère unique par rapport à `Watson`. | `request.input.text:Watson~2` |
| Le texte d'entrée utilisateur contient une chaîne `comm`, suivie de zéro ou plus caractères supplémentaires, suivi(s) de `s`. | `request.input.text:comm*s` |
| L'ID de déploiement dans le contexte correspond à `my_app`. | `request.context.metadata.deployment::my_app` |
|Une intention dans la réponse a une cote de confiance supérieure à 0.8. | `response.intents:confidence>0.8` |
| Un nom d'intention dans la réponse est identique à `hello` ou `goodbye`. | `response.intents:intent::(hello|goodbye)` |
| Une intention dans la réponse comporte le nom `hello` et a une cote de confiance supérieure ou égale à 0.8. | `response.intents:(intent:hello,confidence>=0.8)` |
| Un nom d'intention dans la réponse est identique à `order` et un nom d'entité dans la réponse est identique à `beverage`. | `[response.intents:intent::order,response.entities:entity::beverage]` |
