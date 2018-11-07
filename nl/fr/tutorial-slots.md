---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-24"

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

# Tutoriel : Ajout d'un noeud avec attributs à un dialogue
{: #tutorial-slots}

Dans ce tutoriel, vous allez ajouter des attributs à un noeud de dialogue afin de collecter plusieurs éléments d'informations auprès d'un utilisateur au sein d'un seul noeud. Le noeud que vous créez collectera les informations nécessaires pour réserver une table dans un restaurant.
{: shortdesc}

## Objectifs du tutoriel

A la fin du tutoriel, vous saurez comment effectuer les opérations suivantes :

- Définir les intentions et les entités requises par votre dialogue
- Ajouter des attributs à un noeud de dialogue
- Tester le noeud avec attributs

### Durée
Ce tutoriel dure environ 30 minutes. 

### Prérequis

Avant de commencer, exécutez le [tutoriel d'initiation](getting-started.html). Vous utiliserez l'espace de travail du tutoriel {{site.data.keyword.conversationshort}} que vous avez créé et ajouterez des noeuds au dialogue simple que vous avez créé dans le cadre de l'exercice d'initiation. 

**Remarque** : vous pouvez également démarrer avec un nouvel espace de travail, si vous le souhaitez. Dans ce cas, vous devez prendre soin de créer l'espace de travail avant de commencer à suivre ce tutoriel. 

## Etape 1 : Ajout d'intentions et d'exemples
{: #add-intent}

Ajoutez une intention sur l'onglet Intents. Une intention est l'objectif ou la finalité exprimé dans l'entrée utilisateur. Vous allez ajouter une intention #reservation qui reconnaît l'entrée utilisateur indiquant que l'utilisateur souhaite réserver une table dans un restaurant. 

1.  Sur la page Intents de l'espace de travail du tutoriel, cliquez sur **Add intent**.
1.  Ajoutez le nom d'intention suivant, puis cliquez sur **Create intent** :

    ```json
    reservation
    ```
    {: screen}

    L'intention #reservation est ajoutée. Un signe dièse (`#`) est ajouté en préfixe au nom d'intention pour l'étiqueter en tant qu'intention. Cette convention de dénomination vous aide, ainsi que les autres personnes, à reconnaître l'intention en tant qu'intention. Aucun exemple d'énoncé d'utilisateur ne lui est associé pour l'instant.
1.  Dans la zone **Add user examples**, tapez l'énoncé suivant, puis cliquez sur **Add example** :

    ```json
    i'd like to make a reservation
    ```
    {: screen}

1.  Ajoutez ces exemples supplémentaires pour aider Watson à reconnaître l'intention `#reservation`. 

    ```json
    I want to reserve a table for dinner
    Can 3 of us get a table for lunch?
    do you have openings for next Wednesday at 7?
    Is there availability for 4 on Tuesday night?
    i'd like to come in for brunch tomorrow
    can i reserve a table?
    ```
    {: screen}

1.  Cliquez sur l'icône de **fermeture** (![icône en forme de flèche](images/close_arrow.png)) pour terminer l'ajout de l'intention `#reservation` et de ses exemples d'énoncé. 

## Etape 2 : Ajout d'entités
{: #add-entity}

Une définition d'entité inclut un groupe de *valeurs* d'entité représentant le vocabulaire qui est souvent utilisé dans le contexte d'une intention donnée. En définissant des entités, vous pouvez aider le service à identifier des références dans l'entrée utilisateur qui sont liées à des intentions d'intérêt. Au cours de cette étape, vous allez activer des entités de système qui peuvent reconnaître des références d'heure, de date et de nombre. 

1.  Cliquez sur **Entities** pour ouvrir la page Entities.
1.  Activez les entités de système qui peuvent reconnaître des références de date, d'heure et de nombre dans l'entrée utilisateur. Cliquez sur l'onglet **System entities**, puis activez les entités suivantes : 

    - `@sys-time`
    - `@sys-date               `
    - `@sys-number               `

Vous avez activé les entités de système @sys-date, @sys-time et @sys-number. A présent, vous pouvez les utiliser dans votre dialogue. 

## Etape 3 : Ajout d'un noeud de dialogue avec attributs
{: #add-dialog-with-slots}

Un noeud de dialogue représente le début d'une conversation d'un dialogue entre le service et l'utilisateur. Le noeud contient une condition qui doit être remplie pour qu'il puisse être traité par le service. Au minimum, il comporte aussi une réponse. Par exemple, une condition de noeud peut rechercher l'intention `#hello` dans l'entrée utilisateur et répondre `Bonjour. Que puis-je faire pour vous ?` Cet exemple est la forme la plus simple d'un noeud de dialogue, contenant une seule condition et une seule réponse. Vous pouvez définir des dialogues complexes en ajoutant des réponses conditionnelles à un noeud, en ajoutant des noeuds enfant qui prolongent l'échange avec l'utilisateur, etc. (Si vous souhaitez en savoir plus sur les dialogues complexes, vous pouvez suivre le tutoriel [Création d'un dialogue complexe](tutorial.html).) 

Le noeud que vous allez ajouter au cours de cette étape est celui qui contient des attributs. Les attributs fournissent un format structuré via lequel vous pouvez demander et sauvegarder plusieurs éléments d'informations auprès d'un utilisateur au sein d'un seul noeud. Ils sont utiles lorsque vous avez en tête une tâche spécifique et que vous avez besoin d'éléments d'information essentielles de la part de l'utilisateur avant de pouvoir l'exécuter. Pour plus d'informations, reportez-vous à la rubrique [Collecte d'informations à l'aide d'attributs](dialog-slots.html). 

Le noeud que vous ajoutez va collecter les informations requises pour effectuer une réservation dans un restaurant.

1.  Cliquez sur l'onglet **Dialogs** pour ouvrir l'arborescence de dialogue. 
1.  Cliquez sur l'icône Autres options ![Autres options](images/kabob.png) sur le noeud **#greeting**, puis sélectionnez **Add node below**.
1.  Commencez à taper `#reservation` dans la zone de condition, puis sélectionnez-la dans la liste.
    Ce noeud sera évalué si l'entrée utilisateur correspond à l'intention `#reservation`. 
1.  Cliquez sur **Customize**, cliquez sur l'option à bascule **Slots** pour l'activer (**on**), puis cliquez sur **Apply**.

    ![Illustration montrant le dialogue customize permettant d'activer les attributs.](images/slots-toggle-on.png)
1.  Définissez les attributs suivants :

    <table>
    <caption>Détails d'attribut</caption>
    <tr>
      <th>Zone Check for</th>
      <th>Zone Save it as</th>
      <th>Zone If not present, ask</th>
    </tr>
    <tr>
      <td>@sys-date</td>
      <td>$date</td>
      <td>What day would you like to come in?</td>
    </tr>
    <tr>
      <td>@sys-time</td>
      <td>$time</td>
      <td>What time do you want the reservation to be made for?</td>
    </tr>
    </tr>
    <tr>
      <td>@sys-number</td>
      <td>$guests</td>
      <td>How many people will be dining?</td>
    </tr>
    </table>

1.  Comme réponse, spécifiez `OK. I am making you a reservation for $guests on $date at $time.`

    ![Illustration montrant chaque attribut et la réponse correspondante dans l'outil.](images/slots-simple-node.png)

1.  Cliquez sur l'icône de ![fermeture](images/close.png) pour fermer la vue d'édition de noeud.

## Etape 4 : Test du dialogue

1.  Cliquez sur l'icône ![Demander à Watson](images/ask_watson.png) pour ouvrir le panneau de discussion.
1.  Tapez `i want to make a reservation`.

    Le bot reconnaît l'intention #reservation et répond avec l'invite pour le premier attribut, `What day would you like to come in?`.

1.  Tapez `Friday`.

    Le bot reconnaît la valeur et l'utilise afin de renseigner la variable contextuelle $date pour le permier attribut. Il affiche ensuite l'invite pour l'attribut suivant, `What time do you want the reservation to be made for?`

1.  Tapez `5pm`.

    Le bot reconnaît la valeur et l'utilise afin de renseigner la variable contextuelle $time pour le deuxième attribut. Il affiche ensuite l'invite pour l'attribut suivant, `How many people will be dining?`

1.  Tapez `6`.

    Le bot reconnaît la valeur et l'utilise afin de renseigner la variable contextuelle $guests pour le troisième attribut. A présent que tous les attributs sont renseignés, il affiche la réponse de noeud, `OK. I am making you a reservation for 6 on 2017-12-29 at 17:00:00.`

![Illustration montrant un dialogue de test dans le panneau Try it out qui renseigne les attributs de noeud.](images/slots-test-simple-node.png)

Opération réussie ! Félicitations, vous avez créé un noeud avec attributs.

## Récapitulatif

Dans ce tutoriel, vous avez créé un noeud avec attributs qui peut capturer les informations nécessaires pour réserver une table dans un restaurant.

## Etapes suivantes

Améliorez l'expérience des utilisateurs qui interagissent avec le noeud. Exécutez le tutoriel de suivi, [Amélioration d'un noeud avec attributs](tutorial-slots-complex.html). Il couvre des améliorations simples, par exemple, comment reformater les valeurs de date (2017-12-28) et d'heure (17:00:00) qui sont renvoyées par le système. Il aborde également des tâches plus complexes, par exemple, ce qu'il convient de faire si l'utilisateur ne fournit pas le type de valeur attendu par votre dialogue pour un attribut. 
