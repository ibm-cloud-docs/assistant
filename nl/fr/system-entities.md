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

# Détails des entités de système
{: #system-entities}

Cette rubrique fournit des informations complètes sur les entités de système disponibles. Pour plus d'informations sur les entités de système et leur utilisation, reportez-vous à la section "Activation des entités de système" dans la rubrique [Définition d'entités](/docs/services/assistant?topic=assistant-entities#entities-enable-system-entities).
{: shortdesc}

Des entités de système sont disponibles pour les langues mentionnées dans la rubrique [Langues prises en charge](/docs/services/assistant?topic=assistant-language-support).

## Entité @sys-currency
{: #system-entities-sys-currency}

L'entité de système @sys-currency détecte des valeurs de devise qui sont exprimées dans un énoncé par un symbole monétaire ou des termes propres aux devises. Une valeur numérique est renvoyée.

### Formats reconnus
{: #system-entities-sys-currency-formats}

- 20 cents
- Five dollars
- $10

### Métadonnées
{: #system-entities-sys-currency-metadata}

- `.numeric_value` : valeur numérique canonique sous la forme d'un entier ou d'un double dans des unités de base.
- `.unit` : code de devise d'unité de base (par exemple, 'USD' ou 'EUR').

### Valeurs renvoyées
{: #system-entities-sys-currency-returns}

Pour l'entrée `twenty dollars` ou `$1,234.56`, @sys-currency renvoie les valeurs suivantes :

| Attribut                   | Type   | Valeurs renvoyées pour `twenty dollars` | Valeurs renvoyées pour `$1,234.56` |
|-----------------------------|--------|-------------------------------|-------------------------:|
| @sys-currency               | Chaîne | 20                            |                  1234.56 |
| @sys-currency.literal       | Chaîne | twenty dollars                |                $1,234.56 |
| @sys-currency.numeric_value | number | 20                            |                  1234.56 |
| @sys-currency.location      | Tableau  | [0,14]                        |                    [0,9] |
| @sys-currency.unit          | Chaîne | USD*                          |                      USD |

*@sys-currency.unit renvoie toujours le code de devise ISO composé de 3 lettres.

Pour l'entrée `veinte euro` ou <code>&euro;1.234,56</code>, en espagnol, @sys-currency renvoie les valeurs suivantes :

| Attribut                   | Type   | Valeurs renvoyées pour `veinte euro` | Valeurs renvoyées pour <code>&euro;1.234,56</code> |
|-----------------------------|--------|-----------------------------|-------------------------:|
| @sys-currency               | Chaîne | 20                          |                  1234.56 |
| @sys-currency.literal       | Chaîne | veinte euro                 |                &euro;1.234,56 |
| @sys-currency.numeric_value | number | 20                          |                  1234.56 |
| @sys-currency.location      | Tableau  |[0,11]                       |                     [0,9]|
| @sys-currency.unit          | Chaîne | EUR*                        |                     EUR  |
*@sys-currency.unit renvoie toujours le code de devise ISO composé de 3 lettres.

Les résultats obtenus pour les autres langues et devises nationales prises en charge sont équivalents.

### Conseils relatifs à l'utilisation de @system-currency
{: #system-entities-currencty-usage-tips}

- Les valeurs de devise sont également reconnues en tant qu'instances d'entités @sys-number. Si vous utilisez des conditions distinctes pour rechercher des valeurs de devise et des nombres, placez celle qui permet de rechercher des devises au-dessus de celle qui permet de rechercher des nombres.

- Si vous utilisez l'entité @sys-currency comme condition de noeud et que l'utilisateur spécifie `$0` comme valeur, la valeur est correctement reconnue en tant que devise, mais la condition renvoie le nombre zéro et non la devise zéro. Par conséquent, elle ne renvoie pas la réponse attendue. Pour que les zéros soient correctement interprétés lors de la recherche des valeurs de devise, utilisez à la place la syntaxe d'expression SpEL complète `entities['sys-currency']?.value` dans la condition de noeud.

## Entités @sys-date et @sys-time
{: #system-entities-sys-date-time}

L'entité de système `@sys-date` extrait des mentions, telles que `Friday`, `today` ou `November 1`. La valeur de cette entité stocke la date déduite correspondante sous la forme d'une chaîne au format "aaaa-MM-jj". Par exemple, "2016-11-21". Le système complète les éléments manquants d'une date (par exemple, l'année pour "November 21") avec les valeurs de date en cours.

**Remarque :** pour l'environnement local anglais uniquement, le comportement système par défaut pour l'entrée de date est MM/JJ/AAAA. Ce format est remplacé par JJ/MM/AAAA uniquement si les deux premiers nombres sont supérieurs à 12. La valeur stockée, elle, reste au format "aaaa-MM-jj".

L'entité de système `@sys-time` extrait des mentions, telles que `2pm`, `at 4` ou `15:30`. La valeur de cette entité stocke l'heure sous la forme d'une chaîne au format "HH:mm:ss". Par exemple, "13:00:00."

### Mentions de date et d'heure
{: #system-entities-date-time-mentions}

Les mentions de date et d'heure, telles que `now` ou `two hours from now`, sont extraites sous la forme de deux mentions d'entité distinctes, `@sys-date` et `@sys-time`. Ces deux mentions ne sont pas liées les unes aux autres, mais elles partagent la même chaîne littérale qui correspond à la mention de date et d'heure complète.

Les mentions de date et d'heure composées de plusieurs mots, telles que `on Monday at 4pm`, sont également extraites sous la forme de deux mentions, @sys-date et @sys-time. Lorsqu'elles sont mentionnées ensemble de manière consécutive, elles partagent également une chaîne littérale unique qui correspond à la mention de date et d'heure complète.

### Plages de dates et d'heures
{: #system-entities-date-time-ranges}

Les mentions d'une plage de dates, telles que `the weekend`, `next week` ou `from Monday to Friday`, sont extraites sous la forme d'une paire de mentions d'entités `@sys-date` qui indiquent le début et la fin de la plage de dates. De même, les mentions de plages d'heures, telles que `from 2 to 3`, sont extraites sous la forme de deux entités `@sys-time` qui indiquent les heures de début et de fin. Les deux entités de la paire partagent une chaîne littérale qui correspond à la mention de la plage de dates ou d'heures complète.

### Dates et heures `Last` `Next`
{: #system-entities-last-next}

Dans certains environnements locaux, une phrase telle que "last Monday" est utilisée pour spécifier le lundi de la semaine précédente uniquement. En revanche, dans les autres environnements locaux, "last Monday" est utilisé pour spécifier le dernier jour qui était un lundi, en l'occurrence, il peut s'agir de la même semaine ou de la semaine précédente.

Prenons l'exemple de Friday June 16, dans certains environnements locaux, "last Monday" peut aussi bien faire référence à June 12 qu'à June 5, tandis que dans d'autres environnements locaux, cela fait uniquement référence à June 5 (la semaine précédente). Cette logique s'applique également dans le cas d'une phrase telle que "next Monday".

Le service {{site.data.keyword.conversationshort}} interprète les dates "last" et "next" comme renvoyant au jour suivant ou au jour précédent référencé le plus proche, ce qui peut correspondre à la même semaine ou à une semaine précédente.

Pour les phrases temporelles, telles que "for the last 3 days" ou "in the next 4 hours", la logique est semblable. Prenons l'exemple de "in the next 4 hours". Dans ce cas, deux entités `@sys-time` sont générées, l'une pour l'heure en cours et l'autre pour les quatre heures postérieures à l'heure en cours.

### Fuseaux horaires
{: #system-entities-time-zones}

Les mentions de date ou d'heure qui sont relatives à l'heure en cours sont résolues par rapport à un fuseau horaire choisi. Le fuseau horaire défini par défaut est UTC (GMT). Cela signifie que par défaut, pour les clients d'API REST situés dans des fuseaux horaires autres que UTC, la valeur `now` extraite dépendra de l'heure UTC en cours.

Le client d'API REST peut éventuellement ajouter le fuseau horaire local comme variable contextuelle `$timezone`. Cette variable contextuelle doit être envoyée avec chaque demande de client. Par exemple, la valeur `$timezone` doit être `America/Los_Angeles`, `EST` ou `UTC`. Pour obtenir la liste complète des fuseaux horaires pris en charge, reportez-vous à la rubrique [Fuseaux horaires pris en charge](/docs/services/assistant?topic=assistant-time-zones).

Lorsque la variable `$timezone` est fournie, la valeur des mentions @sys-date et @sys-time relatives est calculée en fonction du fuseau horaire du client et non du fuseau horaire UTC.

#### Exemples de mentions relatives à des fuseaux horaires
{: #system-entities-time-zone-examples}

- now
- in two hours
- today
- tomorrow
- 2 days from now

### Formats reconnus
{: #system-entities-sys-date-time-formats}

- November 21
- 10:30
- at 6 pm
- this weekend

### Valeurs renvoyées
{: #system-entities-sys-date-time-returns}

Pour l'entrée `November 21` @sys-date renvoie les valeurs suivantes :

| Attribut               | Type   | Valeurs renvoyées pour `November 21` |
|-------------------------|--------|---------------------------:|
| @sys-date.literal       | Chaîne |                November 21 |
| @sys-date               | Chaîne |                20xx-11-21 *|
| @sys-date.location      | Tableau |                     [0,11]  |
| @sys-date.calendar_type | Chaîne |                  GREGORIAN |

- @sys-date renvoie toujours la date au format suivant : aaaa-MM-jj.
- \* renvoie la date correspondante suivante. Si cette date est déjà passée, la date de l'année suivante est renvoyée.

Pour l'entrée `at 6 pm` @sys-time renvoie les valeurs suivantes :

| Attribut               | Type   | Valeurs renvoyées pour `at 6 pm` |
|-------------------------|--------|-----------------------:|
| @sys-time.literal       | Chaîne |                at 6 pm |
| @sys-time               | Chaîne |               18:00:00 |
| @sys-time.location      | Tableau |                   [0,7]|
| @sys-time.calendar_type | Chaîne |              GREGORIAN |

- @sys-time renvoie toujours l'heure au format suivant : HH:mm:ss.

Pour plus d'informations sur le traitement des valeurs de date et d'heure, reportez-vous à la section de référence sur la méthode [Date et heure](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-date-time).
{: tip}

## Entité @sys-location
{: #system-entities-sys-location}

**Version bêta. Pour les langues mentionnées dans la rubrique [Langues prises en charge](/docs/services/assistant?topic=assistant-language-support)**, l'entité de système @sys-location extrait des noms de lieu (pays, département, ville, village, etc.) de l'entrée utilisateur. La valeur de l'entité n'est pas une valeur système standard du lieu.

### Formats reconnus
{: #system-entities-sys-location-formats}

- Boston
- U.S.A.
- New South Wales

Pour plus d'informations sur le traitement des valeurs de type Chaîne, reportez-vous à la section de référence sur la méthode [Chaînes](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings).
{: tip}

## Entité @sys-number
{: #system-entities-sys-number}

L'entité de système @sys-number détecte des nombres qui sont écrits à l'aide de chiffres ou de mots. Dans les deux cas, une valeur numérique est renvoyée.

### Formats reconnus
{: #system-entities-sys-number-formats}

- 21
- twenty one
- 3.13

### Métadonnées
{: #system-entities-sys-number-metadata}

- `.numeric_value` - valeur numérique canonique sous la forme d'un entier ou d'un double.

### Valeurs renvoyées
{: #system-entities-sys-number-returns}

Pour l'entrée `twenty` ou `1,234.56`, @sys-number renvoie les valeurs suivantes :

| Attribut                   | Type   | Valeurs renvoyées pour `twenty` | Valeurs renvoyées pour `1,234.56` |
|-----------------------------|--------|-------------------|------------------------:|
| @sys-number               | Chaîne | 20                |                 1234.56 |
| @sys-number.literal       | Chaîne | twenty            |                1,234.56 |
| @sys-number.location      | Tableau |  [0,6]             |                    [0,8]|
| @sys-number.numeric_value | number | 20                |                 1234.56 |

Pour l'entrée `veinte` ou `1.234,56`, en espagnol, @sys-number renvoie les valeurs suivantes :

| Attribut                   | Type   | Valeurs renvoyées pour `veinte` | Valeurs renvoyées pour `1.234,56` |
|-----------------------------|--------|-----------------------|------------------------:|
| @sys-number               | Chaîne | 20                    |                 1234.56 |
| @sys-number.literal       | Chaîne | veinte                |                1.234,56 |
| @sys-number.location      | Tableau  | [0,6]                 |                   [0,8] |
| @sys-number.numeric_value | number | 20                    |                 1234.56 |

Les résultats obtenus pour les autres langues prises en charge sont équivalents.

### Conseils relatifs à l'utilisation de @system-number
{: #system-entities-sys-number-usage-tips}

- Si vous utilisez l'entité @sys-number comme condition de noeud et que l'utilisateur spécifie zero comme valeur, la valeur 0 est correctement reconnue en tant que nombre, mais la condition renvoie la valeur false et ne peut pas renvoyer correctement la réponse associée. Pour que les zéros soient correctement interprétés lors de la recherche des nombres, utilisez à la place la syntaxe d'expression SpEL complète `entities['sys-number']?.value` dans la condition de noeud.

- Si vous utilisez @sys-number pour comparer des valeurs numériques dans une condition, prenez soin d'inclure séparément une recherche de la présence d'un nombre proprement dit. Si aucun nombre n'est trouvé, @sys-number renvoie la valeur null, et par conséquent, votre comparaison pourrait renvoyer la valeur true, même si aucune nombre n'est présent.

  Par exemple, n'utilisez pas `@sys-number<4` tout seul car si aucun nombre n'est trouvé, `@sys-number` renvoie la valeur null. La valeur null étant inférieure à 4, la condition renvoie la valeur true même si aucun nombre n'est présent.

  Utilisez `@sys-number AND @sys-number<4` à la place. Si aucun nombre n'est présent, la première condition renvoie la valeur false, et par conséquent, la condition entière renvoie la valeur false.

Pour plus d'informations sur le traitement des valeurs numériques, reportez-vous à la section de référence sur la méthode [Nombres](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-numbers).
{: tip}

## Entité @sys-percentage
{: #system-entities-sys-percentage}

L'entité de système @sys-percentage détecte des pourcentages qui sont exprimés dans un énoncé par le symbole de pourcentage ou le mot `percent`. Dans les deux cas, une valeur numérique est renvoyée.

### Formats reconnus
{: #system-entities-sys-percentage-formats}

- 15%
- 10 percent

### Métadonnées
{: #system-entities-sys-percentage-metadata}

`.numeric_value` : valeur numérique canonique sous la forme d'un entier ou d'un double.

### Valeurs renvoyées
{: #system-entities-sys-percentage-returns}

Pour l'entrée `1,234.56%`, @sys-percentage renvoie les valeurs suivantes :

| Attribut                     | Type   | Valeurs renvoyées pour `1,234.56%` |
|-------------------------------|--------|-------------------------:|
| @sys-percentage               | Chaîne |                  1234.56 |
| @sys-percentage.literal       | Chaîne |                1,234.56% |
| @sys-percentage.location      | Tableau  |                    [0,9] |
| @sys-percentage.numeric_value | number |                  1234.56 |

Pour l'entrée `1.234,56%`, en espagnol, @sys-currency renvoie les valeurs suivantes :

| Attribut                     | Type   | Valeurs renvoyées pour `1.234,56%` |
|-------------------------------|--------|-------------------------:|
| @sys-percentage               | Chaîne |                  1234.56 |
| @sys-percentage.literal       | Chaîne |                1.234,56% |
| @sys-percentage.location      | Tableau  |                    [0,9] |
| @sys-percentage.numeric_value | number |                  1234.56 |

Les résultats obtenus pour les autres langues prises en charge sont équivalents.

### Conseils relatifs à l'utilisation de @system-percentage
{: #system-entities-sys-percentage-usage-tips}

- Les valeurs de pourcentage sont également reconnues en tant qu'instances d'entités @sys-number. Si vous utilisez des conditions distinctes pour rechercher des valeurs de pourcentage et des nombres, placez celle qui permet de rechercher un pourcentage au-dessus de celle qui permet de rechercher des nombres.

- Si vous utilisez l'entité @sys-percentage comme condition de noeud et que l'utilisateur spécifie `0%` comme valeur, la valeur est correctement reconnue en tant que pourcentage, mais la condition renvoie le nombre zéro et non le pourcentage 0%. Pour que les zéros soient correctement interprétés lors de la recherche des pourcentages, utilisez à la place la syntaxe d'expression SpEL complète `entities['sys-percentage']?.value` dans la condition de noeud.

- Si vous entrez une valeur telle que `1-2%`, les valeurs `1%` et `2%` sont renvoyées sous la forme d'entités de système. L'index correspond à la plage complète entre 1% et 2%, et les deux entités ont le même index.

## Entité @sys-person
{: #system-entities-sys-person}

**Version bêta. Pour les langues mentionnées dans la rubrique [Langues prises en charge](/docs/services/assistant?topic=assistant-language-support)**, l'entité de système @sys-person extrait des noms de l'entrée utilisateur. Les noms sont reconnus individuellement, ainsi, "Joe" ne sera pas interprété comme "Joseph", ou inversement. La valeur de l'entité n'est pas une valeur système standard du nom.

### Formats reconnus
{: #system-entities-sys-person-formats}

- Ronald
- Jane Doe
- Vijay

Pour plus d'informations sur le traitement des valeurs de type Chaîne, reportez-vous à la section de référence sur la méthode [Chaînes](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings).
{: tip}
