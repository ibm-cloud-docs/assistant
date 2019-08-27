---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-11"

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

# Modification d'un dialogue à l'aide de l'API
{: #api-dialog-modify}

L'API REST {{site.data.keyword.conversationshort}} prend en charge la modification de votre dialogue à l'aide d'un programme, sans utiliser l'outil {{site.data.keyword.conversationshort}}. Vous pouvez utiliser l'API /dialog_nodes pour créer, modifier ou supprimer des noeuds de dialogue.

Gardez à l'esprit que le dialogue est une arborescence de noeuds interconnectés et qu'un certain nombre de règles doivent être respectées pour que ce dialogue soit valide. Cela signifie que toute modification apportée à un noeud de dialogue peut avoir des effets en cascade sur d'autres noeuds ou sur la structure de votre dialogue. Avant d'utiliser l'API /dialog_nodes pour modifier votre dialogue, vous devez être certain de bien comprendre les conséquences de cette modification sur le reste de votre dialogue. Vous pouvez faire une copie de sauvegarde du dialogue en cours en exportant la compétence conversationnelle dans laquelle il réside. Pour plus d'informations, reportez-vous à la rubrique [Téléchargement d'une compétence](/docs/services/assistant?topic=assistant-skill-add#download-skill).

Un dialogue valide est toujours conforme aux critères suivants :

- Chaque noeud de dialogue possède un ID unique (propriété `dialog_node`).
- Un noeud enfant est conscient de l'existence de son noeud parent (propriété `parent`). En revanche, un noeud parent n'est pas conscient de l'existence de ses enfants.
- Un noeud est conscient de l'existence de l'élément apparenté qui le précède, le cas échéant (propriété `previous_sibling`). Cela signifie que tous les éléments apparentés qui partagent le même parent constituent une liste liée, dans laquelle chaque noeud pointe vers le noeud précédent.
- Un seul enfant d'un parent donné peut être le premier élément apparenté (la valeur de l'attribut `previous_sibling` qui lui est associé est null).
- Un noeud ne peut pas pointer vers un élément apparenté précédent si celui-ci est un enfant d'un autre parent.
- Deux noeuds ne peuvent pas pointer vers le même élément apparenté précédent.
- Un noeud peut spécifier un autre noeud qui doit être exécuté ensuite (propriété `next_step`).
- Un noeud ne peut pas être son propre parent ou son propre élément apparenté.
- Un noeud doit avoir une propriété de type contenant l'une des valeurs répertoriées ci-après. Si aucune propriété de type n'est spécifiée, le type est `standard`.

  - `event_handler` : gestionnaire qui est défini pour un noeud de trame ou pour un noeud d'attribut individuel.

    A partir de l'outil, vous pouvez définir un gestionnaire de noeud de trame en cliquant sur le lien **Manage handlers** à partir d'un noeud comportant des attributs. (L'interface utilisateur de l'outil n'expose pas le gestionnaire d'événements de niveau attribut, mais vous pouvez en définir un via l'API.)

  - `frame` : noeud comportant un ou plusieurs noeuds enfant de type `slot`. Tout noeud d'attribut enfant requis doit être renseigné pour que le service puisse quitter le noeud de trame.

    Le type de noeud de trame est représenté en tant que noeud avec des attributs dans l'outil. Le noeud qui contient les attributs est représenté en tant que noeud de type=`frame`. Il est le noeud parent de chaque attribut, représenté en tant que noeud enfant de type `slot`.

  - `response_condition` : réponse conditionnelle.

    Dans l'outil, vous pouvez ajouter une ou plusieurs réponses conditionnelles à un noeud. Chaque réponse conditionnelle que vous définissez est représentée dans l'objet JSON sous-jacent en tant que noeud individuel de type=`response_condition`.

  - `slot` : noeud enfant d'un noeud de type `frame`.

    Ce type de noeud est représenté dans l'outil comme étant l'un des multiples attributs ajoutés à un seul noeud. Ce dernier est représenté dans l'objet JSON en tant que noeud parent de type `frame`.

  - `standard` : noeud de dialogue typique. Il s'agit du type par défaut.

- Pour les noeuds de type `slot` ayant le même noeud parent, l'ordre des éléments apparentés (spécifié par la propriété `previous_sibling`) reflète l'ordre dans lequel les attributs seront traités.
- Un noeud de type `slot` doit avoir un noeud parent de type `frame`.
- Un noeud de type `frame` doit avoir au moins un noeud enfant de type `slot`.
- Un noeud de type `response_condition` doit avoir un noeud parent de type `standard` ou `frame`.
- Les noeuds de type `response_condition` et les noeuds de type `event_handler` ne peuvent pas avoir d'enfants.
- Un noeud de type `event_handler` doit également avoir une propriété `event_name` contenant l'une des valeurs suivantes pour identifier le type d'événement de noeud :

  - `filled` : définit ce qu'il convient de faire si l'utilisateur fournit une valeur répondant aux conditions spécifiées dans la zone *Check for* d'un attribut et que l'attribut est renseigné. Un gestionnaire portant ce nom est présent uniquement si une condition Found est définie pour l'attribut.
  - `focus` : définit la question à afficher pour inviter l'utilisateur à fournir les informations requises par l'attribut. Un gestionnaire portant ce nom est présent uniquement si l'attribut est requis.
  - `generic` : définit une condition à surveiller capable de répondre aux questions sans rapport que les utilisateurs peuvent poser lorsqu'ils renseignent un attribut ou un noeud avec des attributs.
  - `input` : met à jour le contexte de message afin d'inclure une variable contextuelle avec la valeur qui est collectée auprès de l'utilisateur pour renseigner l'attribut. Un gestionnaire portant ce nom doit être présent pour chaque attribut dans le noeud de trame.
  - `nomatch` : définit ce qu'il convient de faire si la réponse d'un utilisateur à la demande d'attribut ne contient pas une valeur valide. Un gestionnaire portant ce nom est présent uniquement si une condition Not found est définie pour l'attribut.

  Le diagramme suivant illustre l'emplacement dans l'interface utilisateur de l'outil où vous devez définir le code qui est déclenché pour chaque événement nommé.

  ![Emplacement dans l'interface utilisateur où le code qui est déclenché par des gestionnaires d'événements nommés est créé](images/api-event-handlers.png)

- Un noeud de type `event_handler` avec la valeur `generic` affectée au paramètre event_name peut avoir un parent de type `slot` ou `frame`.
- Un noeud de type `event_handler` avec la valeur `focus`, `input`, `filled` ou `nomatch` affectée au paramètre event_name doit avoir un parent de type `slot`.
- Si plus d'un gestionnaire d'événements (type event_handler) portant le même nom d'événement (paramètre event_name) est associé au même noeud parent, l'ordre des éléments apparentés reflète l'ordre dans lequel les gestionnaires d'événements seront exécutés.
- L'ordre d'exécution des noeuds `event_handler` ayant le même noeud d'attribut parent est le même, quel que soit l'emplacement des définitions de noeud. Les événements sont déclenchés dans cet ordre par nom d'événement (paramètre event_name) :

  1. focus
  1. input
  1. filled
  1. generic*
  1. nomatch

  *Si un noeud de type `event_handler` avec la valeur `generic` affectée au paramètre event_name est défini pour cet attribut ou pour la trame parent, il est exécuté entre les noeuds event_handler filled et nomatch.

Les exemples ci-après montrent comment diverses modifications peuvent entraîner des modifications en cascade.

## Création d'un noeud
{: #api-dialog-modify-create-node}

Voici une arborescence de dialogue simple :

![Exemple de dialogue](images/dialog_api_1.png)

Nous pouvons créer un nouveau noeud en effectuant une demande POST vers /dialog_nodes avec le corps suivant :

```json
{
  "dialog_node": "node_8"
}
```

Le dialogue obtenu se présente comme suit :

![Exemple de dialogue 2](images/dialog_api_2.png)

**node_8** ayant été créé sans qu'aucune valeur ne soit spécifiée pour `parent` ou `previous_sibling`, il est désormais le premier noeud dans le dialogue. Remarque : outre la création de **node_8**, le service a également modifié **node_1**, et la propriété `previous_sibling` associée à ce dernier pointe vers le nouveau noeud.

Vous pouvez créer un noeud à un autre endroit dans le dialogue en spécifiant le parent et l'élément apparenté précédent :

```json
{
  "dialog_node": "node_9",
  "parent": "node_2",
  "previous_sibling": "node_5"
}
```

Les valeurs que vous spécifiez pour `parent` et `previous_sibling` doivent être valides :

- Les deux valeurs doivent faire référence à des noeuds existants.
- Le parent spécifié doit être identique au parent de l'élément apparenté précédent (ou avoir pour valeur `null`, si l'élément apparenté précédent n'a pas de parent).
- Le parent ne peut pas être un noeud de type `response_condition` ou `event_handler`.

Le dialogue obtenu se présente comme suit :

![Exemple de dialogue 3](images/dialog_api_3.png)

Outre la création de **node_9**, le service met automatiquement à jour la propriété `previous_sibling` de *node_6* de sorte qu'elle pointe vers le nouveau noeud.

## Déplacement d'un noeud vers un autre parent
{: #api-dialog-modify-change-parent}

Déplaçons **node_5** vers un autre parent en utilisant la méthode POST /dialog_nodes/node_5 avec le corps suivant :

```json
{
  "parent": "node_1"
}
```

La valeur spécifiée pour `parent` doit être valide :
- Elle doit faire référence à un noeud existant.
- Elle ne doit pas faire référence au noeud en cours de modification (un noeud ne peut pas être son propre parent).
- Elle ne doit pas faire référence à un descendant du noeud en cours de modification.
- Elle ne doit pas faire référence à un noeud de type `response_condition` ou `event_handler`.

La structure ainsi modifiée se présente comme suit :

![Exemple de dialogue 4](images/dialog_api_4.png)

Plusieurs choses se sont produites :
- Lors du déplacement de **node_5** vers son nouveau parent, **node_7** a également été déplacé (car la valeur `parent` associée à **node_7** est restée inchangée). Lorsque vous déplacez un noeud, tous ses descendants restent avec lui.
- Comme vous n'avez pas spécifié de valeur `previous_sibling` pour **node_5**, celui est désormais le premier élément apparenté sous **node_1**.
- La propriété `previous_sibling` de **node_4** a été mise à jour et remplacée par `node_5`.
- La propriété `previous_sibling` de **node_9** a été mise à jour et remplacée par `null`, car ce noeud est désormais le premier élément apparenté sous **node_2**.

## Remise en séquence d'éléments apparentés
{: #api-dialog-modify-change-sibling}

A présent, faisons du premier élément apparenté, **node_5**, le second élément apparenté. Pour ce faire, nous pouvons utiliser la méthode POST /dialog_nodes/node_5 avec le corps suivant :

```json
{
  "previous_sibling": "node_4"
}
```

Lorsque vous modifiez `previous_sibling`, la nouvelle valeur doit être valide :
- Elle doit faire référence à un noeud existant
- Elle ne doit pas faire référence au noeud en cours de modification (un noeud ne peut pas être son propre élément apparenté).
- Elle doit faire référence à un enfant du même parent (tous les éléments apparentés doivent avoir le même parent).

La structure ainsi modifiée se présente comme suit :

![Exemple de dialogue 5](images/dialog_api_5.png)

Remarque : une fois encore, **node_7** a été déplacé avec son parent. De plus, **node_4** a été modifié et la valeur de la propriété `previous_sibling` qui lui est associée a été remplacée par `null`, car il est désormais le premier élément apparenté.

## Suppression d'un noeud
{: #api-dialog-modify-delete-node}

A présent, supprimons **node_1** en utilisant la méthode DELETE /dialog_nodes/node_1.

Le résultat obtenu est le suivant :

![Exemple de dialogue 6](images/dialog_api_6.png)

Remarque : **node_1**, **node_4**, **node_5** et **node_7** ont tous été supprimés. Lorsque vous supprimez un noeud, tous ses descendants sont également supprimés. Par conséquent, si vous supprimez un noeud racine, c'est toute une branche de l'arborescence que vous supprimez. Les autres références au noeud supprimé (par exemple, les références `next_step`) sont remplacées par `null`.

En outre, **node_2** est mis à jour de manière à pointer vers **node_8**, qui est son nouvel élément apparenté.

## Changement de nom d'un noeud
{: #api-dialog-modify-rename-node}

Enfin, renommons **node_2** en utilisant la méthode POST /dialog_nodes/node_2 avec le corps suivant :

```json
{
  "dialog_node": "node_X"
}
```

![Exemple de dialogue 7](images/dialog_api_7.png)

La structure du dialogue n'a pas changé, mais là encore, plusieurs noeuds ont été modifiés afin de refléter le nouveau nom :

- Les propriétés `parent` de **node_9** et **node_6**
- La propriété `previous_sibling` de **node_3**

Les autres références au noeud supprimé (par exemple, les références `next_step`) ont également été modifiées.
