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

# Tutoriel : Création d'un dialogue complexe
{: #tutorial}

Dans ce tutoriel, vous allez utiliser le service {{site.data.keyword.conversationshort}} pour créer un dialogue qui permettra aux utilisateurs d'interagir avec un tableau de bord de voiture intelligente.
{: shortdesc}

## Objectifs du tutoriel

A la fin du tutoriel, vous saurez comment effectuer les opérations suivantes :

- Définir des entités
- Planifier un dialogue
- Utiliser des conditions de noeud et de réponse dans un dialogue

### Durée
Ce tutoriel dure environ 2 à 3 heures.

### Prérequis

Avant de commencer, exécutez le [tutoriel d'initiation](getting-started.html). 

Vous utiliserez l'espace de travail du tutoriel {{site.data.keyword.conversationshort}} que vous avez créé et ajouterez des noeuds au dialogue simple que vous avez créé dans le cadre de l'exercice d'initiation. 

## Etape 1 : Ajout d'intentions et d'exemples
{: #intents}

Ajoutez une intention sur l'onglet Intents. Une intention est l'objectif ou la finalité exprimé dans l'entrée utilisateur.

1.  Sur la page Intents de l'espace de travail du tutoriel {{site.data.keyword.conversationshort}}, cliquez sur **Add intent**.
1.  Ajoutez le nom d'intention suivant, puis cliquez sur **Create intent** :

    ```
    turn_on
    ```
    {: codeblock}

    Un caractère `#` est ajouté en préfixe au nom d'intention que vous spécifiez. L'intention `#turn_on` indique que l'utilisateur souhaite allumer un dispositif, tel que la radio, les essuie-glaces ou les phares.
1.  Dans la zone **Add user example**, tapez l'énoncé suivant, puis cliquez sur **Add example** :

    ```
    I need lights
    ```
    {: codeblock}

1.  Ajoutez les 5 exemples supplémentaires suivants pour aider Watson à reconnaître l'intention `#turn_on` :

    ```
    Play some tunes
    Turn on the radio
    turn on
    Air on please
    Crank up the AC
    Turn on the headlights
    ```
    {: codeblock}

1.  Cliquez sur l'icône de **fermeture** (![icône en forme de flèche](images/close_arrow.png)) pour terminer l'ajout de l'intention `#turn_on`. 

Vous possédez maintenant trois intentions, à savoir l'intention `#turn_on` que vous venez d'ajouter et les intentions `#hello` et `#goodbye` qui ont été ajoutées lorsque vous avez exécuté l'étape prérequise qui consistait à suivre le *tutoriel d'initiation*. Chaque intention comporte un ensemble d'exemples d'énoncé qui facilitent l'entraînement de Watson à reconnaître les intentions dans l'entrée utilisateur. 

## Etape 2 : Ajout d'entités
{: #entities}

Une définition d'entité inclut un groupe de *valeurs* d'entité qui peuvent être utilisées pour déclencher des réponses différentes. Chaque valeur d'entité peut avoir plusieurs *synonymes*, qui définissent différentes spécifications possibles pour une même valeur dans l'entrée utilisateur.

Créez des entités pouvant apparaître dans l'entrée utilisateur qui comporte l'intention #turn_on intent afin de représenter le dispositif que l'utilisateur veut allumer.

1.  Cliquez sur l'onglet **Entities** pour ouvrir la page Entities.
1.  Cliquez sur **Add entity**.
1.  Ajoutez le nom d'entité suivant et appuyez sur Entrée :

    ```
    appliance
    ```
    {: codeblock}

    Un caractère `@` est ajouté en préfixe au nom d'entité que vous spécifiez. L'entité `@appliance` représente le dispositif d'une voiture qu'un utilisateur veut allumer.
1.  Ajoutez la valeur suivante à la zone **Value name ** :

    ```
    radio
    ```
    {: codeblock}

    La valeur représente un dispositif spécifique que les utilisateurs voudront peut-être allumer.
1.  Ajoutez d'autres manières de spécifier l'entité de dispositif radio dans la zone **Synonyms**. Appuyez sur la touche de **tabulation** pour mettre la zone en évidence et entrez les synonymes ci-dessous. Appuyez sur **Entrée** après chaque synonyme. 

    ```
    music
    tunes
    ```
    {: codeblock}

1.  Cliquez sur **Add value** afin de terminer de définir la valeur `radio` pour l'entité `@appliance`. 
1.  Ajoutez d'autres types de dispositif. 

    - Valeur : `headlights`. Synonyme : `lights`.
    - Valeur : `air conditioning`. Synonymes : `air` et `AC`.

1.  Cliquez sur le bouton à bascule afin d'activer (**on**) la fonction Fuzzy Matching pour l'entité `@appliance`.
    Ce paramétrage permettra au service de reconnaître les références aux entités dans l'entrée utilisateur même si la façon dont les entités sont spécifiées ne correspond pas exactement à la syntaxe que vous utilisez ici.
1.  Cliquez sur l'icône de **Fermeture** (![icône en forme de flèche](images/close_arrow.png)) pour terminer l'ajout de l'entité  `@appliance`. 
1.  Répétez les étapes 2 à 8 pour créer l'entité @`genre` avec la fonction Fuzzy Matching activée, ainsi que les valeurs et les synonymes ci-dessous :

    - Valeur : `classical`. Synonyme : `symphonic`.
    - Valeur : `rhythm and blues` Synonyme : `r&b`.
    - Valeur : `rock`. Synonyme : `rock & roll`, `rock and roll` et `pop`.

Vous avez défini deux entités : `@appliance` (qui représente un dispositif que le bot peut allumer) et `@genre` (qui représente un genre de musique que l'utilisateur peut choisir d'écouter).

Lorsque l'entrée utilisateur est reçue, le service {{site.data.keyword.conversationshort}} identifie les intentions et les entités. Vous pouvez à présent définir un dialogue qui utilise des entités et des réponses pour choisir la réponse appropriée.

## Etape 3 : Création d'un dialogue complexe
{: #complex-dialog}

Dans ce dialogue complexe, vous allez créer des branches de dialogue destinées à gérer l'intention #turn_on que vous avez définie précédemment.

### Ajout d'un noeud racine pour #turn_on
Créez une branche de dialogue pour répondre à l'intention #turn_on. Commencez par créer le noeud racine :

1.  Cliquez sur l'icône Autres options ![Autres options](images/kabob.png) sur le noeud **#hello**, puis sélectionnez **Add node below**.
1.  Commencez par taper `#turn_on` dans la zone de condition, puis sélectionnez-la dans la liste.
    Cette condition est déclenchée dès lors qu'une entrée correspondant à l'intention #turn_est reçue.
1.  N'entrez pas de réponse dans ce noeud. Cliquez sur l'icône de ![fermeture](images/close.png) pour fermer la vue d'édition de noeud.

### Scénarios
Le dialogue doit déterminer quel dispositif l'utilisateur veut allumer. Pour cela, créez plusieurs réponses en fonction de conditions supplémentaires.

Il existe trois scénarios possibles, en fonction des intentions et des entités que vous avez définies :

**Scénario 1** : l'utilisateur veut mettre de la musique. Dans ce cas, le bot doit demander à l'utilisateur quel genre de musique il souhaite écouter.

**Scénario 2** : l'utilisateur veut allumer n'importe quel autre dispositif valide. Dans ce cas, le bot répète le nom du dispositif demandé dans un message indiquant que le dispositif est en cours d'allumage.

**Scénario 3** : l'utilisateur ne spécifie pas un nom de dispositif pouvant être reconnu. Dans ce cas, le bot doit demander à l'utilisateur de préciser ce qu'il a voulu dire.

Ajoutez des noeuds qui vérifient ces conditions de scénario dans l'ordre indiqué de sorte que le dialogue évalue la condition la plus spécifique en premier.

### Etude du scénario 1

Ajoutez des noeuds pertinents pour le scénario 1 au cours duquel l'utilisateur souhaite mettre de la musique. En réponse, le bot doit demander à l'utilisateur d'indiquer le genre de musique qu'il veut écouter.

#### Ajout d'un noeud enfant qui vérifie si le type de dispositif est music

1.  Cliquez sur l'icône Autres options ![Autres options](images/kabob.png) sur le noeud **#turn_on**, puis sélectionnez **Add child node**.
1.  Dans la zone de condition, entrez `@appliance:radio`.
    Cette condition renvoie la valeur true si la valeur de l'entité @appliance est `radio` ou l'un de ses synonymes, tels qu'ils sont définis sur l'onglet Entities.
1.  Dans la zone de réponse, entrez `What kind of music would you like to hear?`
1.  Nommez le noeud `Music`.
1.  Cliquez sur l'icône de ![fermeture](images/close.png) pour fermer la vue d'édition de noeud.

#### Ajout d'un saut entre le noeud #turn_on et le noeud Music

Passez directement du noeud `#turn on` au noeud `Music` sans demander aucune entrée utilisateur. Pour cela, vous pouvez utiliser une action **Jump to**.

1.  Cliquez sur l'icône Autres options ![Autres options](images/kabob.png) sur le noeud **#turn_on**, puis sélectionnez **Jump to**.
1.  Sélectionnez le noeud enfant **Music**, puis sélectionnez **If bot recognizes (condition)** pour indiquer que vous souhaitez traiter la condition du noeud Music.

![Action Jump to (vue antérieure)](images/tut-dialog-jumpto.png)

Notez que vous avez dû créer le noeud cible (noeud auquel vous souhaitez passer directement) avant d'ajouter l'action **Jump to**.

Une fois que vous avez créé la relation Jump to, la nouvelle entrée ci-dessous apparaît dans l'arborescence :

![Action Jump to (vue postérieure)](images/tut-dialog-jump2.png)

#### Ajout d'un noeud enfant qui recherche le genre de musique

A présent, ajoutez un noeud destiné à traiter le type de musique demandé par l'utilisateur.

1.  Cliquez sur l'icône Autres options ![Autres options](images/kabob.png) sur le noeud **Music**, puis sélectionnez **Add child node**.
    Ce noeud enfant est évalué seulement après que l'utilisateur a répondu à la question relative au type de musique qu'il veut écouter. Etant donné qu'une entrée utilisateur est nécessaire préalablement à ce noeud, il n'y a pas lieu d'utiliser l'action **Jump to**.
1.  Ajoutez `@genre` à la zone de condition.  Cette condition renvoie la valeur true chaque fois qu'une valeur valide est détectée pour l'entité @genre.
1.  Entrez `OK! Playing @genre.` comme réponse. Cette réponse rappelle la valeur de genre que l'utilisateur a fournie.

#### Ajout d'un noeud qui gère les types de genre non reconnus dans les réponses utilisateur

Ajoutez un noeud destiné à répondre lorsque l'utilisateur ne spécifie pas une valeur reconnue pour @genre.

1.  Cliquez sur l'icône Autres options ![Autres options](images/kabob.png) sur le noeud *@genre* puis sélectionnez **Add node below** pour créer un noeud homologue.
1.  Entrez `true` dans la zone de condition.
    La condition true est une condition spéciale. Elle indique que si le flux de dialogue atteint ce noeud, il doit toujours renvoyer la valeur true. (Si l'utilisateur spécifie une valeur valide pour @genre, ce noeud ne sera jamais atteint.)
1.  Entrez `I'm sorry, I don't understand. I can play classical, rhythm and blues, or rock music.` comme réponse.

Cela s'applique à tous les cas où l'utilisateur demande à mettre de la musique.

#### Test du dialogue pour la musique

1.  Cliquez sur l'icône ![Demander à Watson](images/ask_watson.png) pour ouvrir le panneau de discussion.
1.  Tapez `Play music`.
    Le bot reconnaît l'intention #turn_on et l'entité @appliance:music et répond en demandant à l'utilisateur de préciser le genre de musique qu'il souhaite écouter.

1.  Tapez une valeur valide pour @genre (par exemple, `rock`).
    Le bot reconnaît l'entité @genre et répond de manière appropriée.

    ![Illustration représentant une réponse positive à une demande formulée par l'utilisateur pour écouter de la musique](images/tut-test-music.png)

1.  Tapez à nouveau `Play music`, mais cette fois-ci, spécifiez une réponse non valide pour le genre. Le bot répond en indiquant qu'il ne comprend pas la réponse formulée.

### Etude du scénario 2

Nous allons ajouter des noeuds pertinents pour le scénario 2 au cours duquel l'utilisateur souhaite allumer un autre dispositif valide. Dans ce cas, le bot répète le nom du dispositif demandé dans un message indiquant que le dispositif est en cours d'allumage.

#### Ajout d'un noeud enfant qui recherche n'importe quel dispositif

Ajoutez un noeud qui se déclenche lorsque l'utilisateur fournit n'importe quelle autre valeur valide pour @appliance.
Pour les autres valeurs @appliance, le bot n'a pas besoin de demander d'entrée supplémentaire. Il renvoie simplement une réponse positive.

1.  Cliquez sur l'icône Autres options ![Autres options](images/kabob.png) sur le noeud **Music**, puis sélectionnez **Add node below** pour créer un noeud homologue qui est évalué après la condition @appliance:music.
1.  Entrez `@appliance` comme condition de noeud.
    Cette condition se déclenche si l'entrée utilisateur inclut n'importe quelle valeur reconnue pour l'entité @appliance à part music.
1.  Entrez `OK! Turning on the @appliance.` comme réponse.
    Cette réponse rappelle la valeur de dispositif que l'utilisateur a fournie.

#### Test du dialogue pour les autres dispositifs

1.  Cliquez sur l'icône ![Demander à Watson](images/ask_watson.png) pour ouvrir le panneau de discussion.
1.  Tapez `lights on`.

    Le bot reconnaît l'intention #turn_on et l'entité @appliance:headlights et formule la réponse `OK, turning on the headlights`.

    ![Illustration représentant une réponse positive à une demande formulée par l'utilisateur pour allumer les phares](images/tut-test-lights.png)

1.  Tapez `turn on the air`.

    Le bot reconnaît l'intention #turn_on et l'entité @appliance:(air conditioning) et formule la réponse `OK, turning on the air conditioning.`

1.  Essayez les variantes sur toutes les commandes prises en charge en fonction des exemples d'énoncé et des synonymes d'entité que vous avez définis.

### Etude du scénario 3

A présent, ajoutez un noeud homologue qui se déclenche si l'utilisateur ne spécifie pas un type de dispositif valide.

1.  Cliquez sur l'icône Autres options ![Autres options](images/kabob.png) sur le noeud **@appliance**, puis sélectionnez **Add node below** pour créer un noeud homologue qui est évalué après la condition @appliance.
1.  Entrez `true` dans la zone de condition.
    (Si l'utilisateur spécifie une valeur valide pour @appliance, ce noeud ne sera jamais atteint.)
1.  Entrez `I'm sorry, I'm not sure I understood you. I can turn on music, headlights, or air conditioning.` comme réponse.

#### Tests supplémentaires

1.  Essayez d'autres variantes d'énoncé pour tester le dialogue.

    Si le bot ne reconnaît pas la bonne intention, vous pouvez relancer son entraînement directement à partir de la fenêtre de discussion. Sélectionnez la flèche en regard de l'intention incorrecte et choisissez l'intention appropriée dans la liste.

    ![Animation illustrant le choix d'une autre intention et la relance du processus d'entraînement](images/tut-change-intent.gif)

Vous pouvez éventuellement passer en revue l'espace de travail **Car Dashboard - Sample** pour voir ce même scénario d'utilisation enrichi par un dialogue plus long et des fonctionnalités supplémentaires. 

1.  Cliquez sur le bouton de **retour dans les espaces de travail** ![Illustration du bouton de retour dans les espaces de travail dans le menu](images/workspaces-button.png) dans le menu de navigation.

1.  Sur la vignette **Car Dashboard - Sample**, cliquez sur **Edit sample**.

## Etapes suivantes
{: #deploy}

A présent que vous avez créé et testé votre espace de travail, vous pouvez le déployer en le connectant à une interface utilisateur. Cette opération peut s'effectuer de plusieurs manières.

### Utilisation de l'option Test in Slack

Vous pouvez utiliser l'outil de déploiement de test pour [déployer votre espace de travail](test-deploy.html) sous la forme d'un agent conversationnel dans un canal Slack en seulement quelques étapes. Cette option est le moyen le plus rapide et le plus facile de déployer votre espace de travail à des fins de test, mais elle comporte quelques limitations.

### Création de votre propre application frontale

Vous pouvez utiliser les logiciels SDK Watson pour [créer votre propre](develop-app.html) application frontale que vous connecterez à votre espace de travail à l'aide de l'API REST {{site.data.keyword.conversationshort}}. 

### Déploiement sur des médias sociaux ou des canaux de transmission de messages

Vous pouvez utiliser l'[infrastructure Botkit](integrations.html) pour créer une application bot que vous pourrez intégrer à des médias sociaux et à des canaux de transmission de messages, tels que Slack, Facebook et Twilio.
