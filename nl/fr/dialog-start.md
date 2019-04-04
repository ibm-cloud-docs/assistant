---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-11"

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

# Démarrage du dialogue 
{: #dialog-start}

Vous ne pouvez pas utiliser le noeud de bienvenue intégré pour démarrer un dialogue de la même manière pour toutes les intégrations. Utilisez cette solution de contournement à la place.
{: shortdesc}

La réponse que vous définissez pour le noeud de bienvenue dans le dialogue s'affiche pour initier une conversation à partir du panneau "Try it out" dans l'outil et à partir du widget de discussion dans l'intégration Preview Link. Cependant, de nombreuses autres intégrations de canaux ne l’affichent pas car les noeuds associés à la condition spéciale `welcome` sont ignorés dans les flux de dialogue lancés par les utilisateurs. Et les assistants déployés attendent généralement que les utilisateurs engagent des conversations avec eux, et non l'inverse. 

Contrairement à la condition spéciale `welcome`, la condition spéciale `conversation_start` est toujours déclenchée au début d'un dialogue. Vous pouvez utiliser une combinaison de noeuds avec ces deux conditions spéciales (`welcome` et `conversation_start`) pour gérer le début de votre dialogue de manière cohérente.

Procédez comme suit pour gérer le démarrage du dialogue : 

1.  Ajoutez un noeud de dialogue au-dessus du noeud Welcome, qui est automatiquement ajouté au début de l’arborescence lors de la création du dialogue. 

1.  Définissez la condition de ce nouveau noeud sur `conversation_start`, qui est une condition spéciale décrite précédemment. 

1.  Définissez les variables contextuelles que vous souhaitez définir avec les valeurs par défaut du dialogue dans le noeud `conversation_start`.

1.  Ne définissez pas de réponse textuelle pour ce noeud. 

1.  Configurez ce noeud pour accéder directement au noeud `Welcome` dans l'arborescence du dialogue, puis choisissez **If bot recognizes (condition)**.

![Capture d'écran de l'arborescence du dialogue avec un noeud conversation_start sautant sur un noeud de bienvenue situé en dessous.](images/dialog-start.png)

Cette conception produit un dialogue qui fonctionne comme ceci :  

- Quel que soit le type d'intégration, le noeud `conversation_start` est traité, ce qui signifie que toutes les variables contextuelles que vous définissez dans celui-ci sont initialisées.
- Dans les intégrations où l'assistant lance le processus de dialogue, le noeud `Welcome` est déclenché et sa réponse textuelle est affichée.
- Dans les intégrations où l'utilisateur lance le flux de dialogue, la première entrée utilisateur est évaluée puis traitée par le noeud qui peut fournir la meilleure réponse.  
