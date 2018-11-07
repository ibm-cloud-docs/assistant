---

copyright:
  years: 2015, 2018
lastupdated: "2018-03-19"

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

# Création d'un dialogue
{: #dialog-build}

Utilisez l'outil {{site.data.keyword.conversationshort}} pour créer votre dialogue.
{: shortdesc}

## Nombre limite de noeuds de dialogue
{: #dialog-node-limits}

Le nombre de noeuds de dialogue que vous pouvez créez dépend de votre plan de service.

| Plan de service     | Nombre de noeuds de dialogue par espace de travail |
|------------------|---------------------------:|
| Standard/Premium |                    100 000 |
| Lite             |                     25 000 |
{: caption="Détails de plan de service" caption-side="top"}

Profondeur d'arborescence maximale : le service prend en charge 2 000 descendants de noeud de dialogue ; les outils sont plus performants si ce nombre n'excède pas 20.

## Procédure
{: #dialog-procedure}

Pour créer un dialogue, procédez comme suit :

1.  Ouvrez la page **Build** dans la barre de navigation, cliquez sur l'onglet **Dialog**, puis sur **Create**.

    Lorsque vous ouvrez le générateur de dialogue pour la première fois, les noeuds suivants sont déjà créés pour vous :
    - **Welcome** : permier noeud. Il contient un message d'accueil qui s'affiche lorsque vos utilisateurs interagissent pour la première fois avec le service. Le message d'accueil peut être édité.
    - **Anything else** : noeud final. Il contient les phrases qui sont utilisées pour répondre aux utilisateurs lorsque leur entrée n'est pas reconnue. Il est possible de remplacer les réponses fournies ou d'en ajouter d'autres ayant une signification similaire afin d'ajouter de la diversité à la conversation. Vous pouvez également spécifier si le service doit renvoyer chacune des réponses définies les unes après les autres ou si elles doivent être renvoyées de façon aléatoire.
1.  Pour ajouter d'autres noeuds à l'arborescence de dialogue, cliquez sur l'icône **Autres options** ![icône Autres options](images/kabob.png) sur le noeud **Welcome**, puis sélectionnez **Add node below**.
1.  Entrez une condition, qui, lorsqu'elle est satisfaite, déclenche le traitement du noeud par le service.

    Lorsque vous commencez à définir une condition, une zone comportant les options possibles s'affiche. Vous pouvez entrer l'un des caractères suivants, puis choisir une valeur dans la liste des options qui s'affiche.

    <table>
    <caption>Syntaxe de générateur de conditions</caption>
    <tr>
      <th>Caractère</th>
      <th>Valeurs définies dans des listes pour ces types d'artefact</th>
    </tr>
    <tr>
      <td>`#`</td>
      <td>intentions</td>
    </tr>
    <tr>
      <td>`@`</td>
      <td>entités</td>
    </tr>
    <tr>
      <td>`@{entity-name}:`</td>
      <td>valeurs {entity-name}</td>
    </tr>
    <tr>
      <td>`$`</td>
      <td>variables contextuelles que vous avez définies ou référencées ailleurs dans le dialogue</td>
    </tr>
    </table>

    Vous pouvez créer une nouvelle intention, entité, valeur d'entité ou variable contextuelle en définissant une nouvelle condition qui l'utilise. Si vous créez un artefact de cette façon, prenez soin de revenir en arrière et d'effectuer les autres étapes requises pour la création complète de l'artefact, par exemple, en définissant des exemples d'énoncé pour une intention.

    Pour définir un noeud qui se déclenche en fonction de plusieurs conditions, entrez une condition, puis cliquez sur le signe plus (+) en regard de celle-ci. Si vous souhaitez appliquer un opérateur `OR` aux conditions multiples au lieu de l'opérateur `AND`, cliquez sur la mention `and` affichée entre les zones pour modifier le type d'opérateur. Les opérations AND sont exécutées avant les opérations OR, mais vous pouvez modifier l'ordre en utilisant des parenthèses. Par exemple :
    `$isMember:true AND ($memberlevel:silver OR $memberlevel:gold)`

    La condition que vous définissez ne doit pas excéder 500 caractères.

    Pour savoir comment tester les valeurs définies dans des conditions, reportez-vous à la rubrique [Conditions](dialog-overview.html#conditions).
1.  **Facultatif** : si vous souhaitez collecter plusieurs éléments d'informations auprès de l'utilisateur au sein de ce noeud, cliquez sur **Customize**, puis activez **Slots**. Pour plus d'informations, reportez-vous à la section [Collecte d'informations à l'aide d'attributs](dialog-slots.html).
1.  Entrez une réponse.
    - Ajoutez le texte que le service doit afficher comme réponse.
    - Si vous souhaitez définir des réponses différentes en fonction de certaines conditions, cliquez sur **Customize** puis activez **Multiple responses**.
    - Pour plus d'informations sur les réponses conditionnelles ou pour savoir comment ajouter de la diversité, reportez-vous à la section [Réponses](dialog-overview.html##responses).

1.  Spécifiez ce qu'il convient de faire une fois que le noeud en cours est traité. Vous avez le choix entre les différentes options suivantes :

    - **Wait for user input** : le service attend que l'utilisateur fournisse une nouvelle entrée. 
    - **Skip user input** : le service accède directement au premier noeud enfant. Cette option est disponible uniquement si le noeud en cours a au moins un noeud enfant. 
    - **Jump to** : le service poursuit le dialogue en traitant le noeud que vous spécifiez. Vous pouvez choisir si le service doit évaluer la condition du noeud cible ou accéder directement à la réponse du noeud cible. Pour plus d'informations, reportez-vous à la rubrique [Configuration de l'action Jump to](dialog-overview.html#jump-to-config). 

1.  **Facultatif** : donnez un nom au noeud.

    Le nom de noeud de dialogue peut contenir des lettres (au format Unicode), des nombres, des espaces, des traits de soulignement, des traits d'union et des points.

    Le fait de nommer un noeud permet de mémoriser plus facilement son objectif et de le localiser plus facilement lorsqu'il est réduit. Si vous n'indiquez pas de nom, la condition du noeud est utilisée comme nom.

1.  Pour ajouter d'autres noeuds, sélectionnez un noeud dans l'arborescence, puis cliquez sur l'icône **Autres options** ![icône Autres options](images/kabob.png).
    - Pour créer un noeud homologue qui est vérifié ensuite si la condition relative au noeud existant n'est pas satisfaite, sélectionnez **Add node below**.
    - Pour créer un noeud homologue qui est vérifié avant la condition relative au noeud existant, sélectionnez **Add node above**.
    - Pour créer un noeud enfant sur le noeud sélectionné, sélectionnez **Add child node**. Un noeud enfant est traité après son noeud parent.
    - Pour copier le noeud en cours, sélectionnez **Duplicate**.

    Pour plus d'informations sur l'ordre dans lequel les noeuds de dialogue sont traités, reportez-vous à la section [Présentation du dialogue](dialog-overview.html#dialog-flow).
1.  Testez le dialogue à mesure que vous le créez.
   Pour plus d'informations, reportez-vous à la section [Test de votre dialogue](#test).

## Test de votre dialogue
{: #test}

Si vous apportez des modifications à votre dialogue, vous pouvez le tester à tout moment pour voir de quelle façon il répond à une entrée.

1.  Dans l'onglet Dialog, cliquez sur l'icône ![Demander à Watson](images/ask_watson.png).
1.  Dans le panneau de discussion, tapez un texte et appuyez sur Entrée.

    Avant de tester le dialogue, assurez-vous que le système a terminé de s'entraîner avec les dernières modifications que vous avez effectuées. Si tel n'est pas le cas, un message s'affiche dans le panneau *Try it out* :
    {: tip}

    ![Capture d'écran du message indiquant que Watson est en train de s'entraîner](images/training.png)
1.  Vérifiez la réponse pour voir si le dialogue a correctement interprété votre entrée et choisi la bonne réponse. 

    La fenêtre de discussion indique quelles intentions et quelles entités ont été reconnus dans l'entrée :

    ![Capture d'écran illustrant la sortie du dialogue de test](images/test_dialog_output.png)
1.  Si vous souhaitez savoir quel noeud de l'arborescence de dialogue a déclenché une réponse, cliquez sur l'icône d'**emplacement** ![Emplacement](images/location.png) qui lui est associée. Si vous ne vous trouvez pas encore dans l'onglet Dialog, ouvrez-le. 

    Le noeud source est mis en évidence et la route empruntée par le service dans l'arborescence pour y accéder est mise en évidence. La mise en évidence est conservée jusqu'à ce que vous exécutiez une autre action, telle que la saisie d'une nouvelle entrée de test.
1.  Pour vérifier ou définir la valeur d'une variable contextuelle, cliquez sur le lien **Manage context**.

    Les variables contextuelles que vous avez éventuellement définies dans le dialogue s'affichent.

    De plus, une variable contextuelle `$timezone` est répertoriée. L'interface utilisateur du panneau *Try it out* récupère les paramètres régionaux de l'utilisateur à partir du navigateur Web et les utilise pour définir la variable contextuelle `$timezone`. Cette variable contextuelle facilite le traitement des références temporelles dans les échanges du dialogue de test. Songez à faire la même chose dans votre application utilisateur. Si cette variable contextuelle n'est pas spécifiée, c'est l'heure moyenne de Greenwich qui est utilisée.

    Vous pouvez ajouter une variable et définir sa valeur pour voir comment le dialogue répond lors de l'échange suivant du dialogue de test. Cette fonctionnalité est utile si, par exemple, le dialogue est configuré pour afficher différentes réponses en fonction d'une valeur de variable contextuelle qui est fournie par l'utilisateur.

    1.  Pour ajouter une variable contextuelle, spécifiez le nom de la variable et appuyez sur **Entrée**.
    1.  Pour définir une valeur par défaut pour la variable contextuelle, localisez la variable contextuelle que vous avez ajoutée dans la liste, puis attribuez-lui une valeur.

    Pour plus d'informations, reportez-vous à la section [Variables contextuelles](dialog-runtime.html#context).

1.  Continuez d'interagir avec le dialogue pour voir de quelle façon la conversation circule à travers lui.
    - Pour rechercher et soumettre à nouveau un énoncé de test, vous pouvez appuyer sur la touche de déplacement vers le haut pour faire défiler les entrées récentes.
    - Pour retirer les précédents énoncés de test sur le panneau de discussion et recommencer, cliquez sur le lien **Clear**. Cette action permet non seulement de retirer les énoncés et les réponses de test, mais également les valeurs des variables contextuelles qui ont été définies suite à vos interactions avec le dialogue. Les variables contextuelles que vous définissez ou modifiez explicitement ne sont pas retirées.

### Etape suivante

Si vous constatez que les intentions ou les entités reconnues sont incorrectes, vous devrez peut-être modifier vos définitions d'intention ou d'entité.

Si les intentions et les entités reconnues sont correctes, mais les noeuds déclenchés dans votre dialogue sont incorrects, vérifiez que vos conditions sont correctement écrites. 

## Copie d'un noeud de dialogue
{: #copy-node}

Vous pouvez dupliquer un noeud afin d'en créer une copie exacte en tant que noeud homologue juste au-dessous dans l'arborescence de dialogue. Le noeud copié proprement dit prend le même nom que le noeud d'origine, mais la mention `- copy`*`n`* y est accolée (*`n`* est un nombre commençant par 1). Si vous dupliquez le même noeud plus d'une fois, la valeur *`n`* du nom est incrémentée de un pour chaque copie afin de vous permettre de la distinguer des autres. Si  le noeud n'a pas de nom, il prend le nom `copy`*`n`*.

Lorsque vous dupliquez un noeud qui a des noeuds enfant, ces derniers sont également dupliqués. Les noeuds enfant copiés portent le même nom que les noeuds enfant d'origine. Seule la référence `copy` dans le nom du noeud parent permet de distinguer un noeud enfant copié d'un noeud enfant d'origine. 

1.  Sur le noeud que vous souhaitez copier, cliquez sur l'icône **Autres options** ![Autres options](images/kabob.png), puis sélectionnez **Duplicate**.
1.  Pensez à renommer les noeuds copiés ou à éditer leurs conditions pour les rendre distinctes.

## Déplacement d'un noeud de dialogue
{: #move-node}

Chaque noeud que vous créez peut être déplacé ailleurs dans l'arborescence de dialogue.

Vous souhaiterez peut-être déplacer un noeud précédemment créé vers une autre zone du flux dans le modifier la conversation. Vous pouvez déplacer des noeuds pour qu'ils deviennent des éléments apparentés ou des homologues dans une autre branche.

1.  Sur le noeud que vous souhaitez déplacer, cliquez sur l'icône **Autres options** ![icône Autres options](images/kabob.png), puis sélectionnez **Move**.
1.  Sélectionnez un noeud cible dans l'arborescence près de l'emplacement où vous souhaitez déplacer ce noeud. Indiquez si vous souhaitez placer ce noeud avant ou après le noeud cible ou en faire un enfant du noeud cible. 

## Organisation du dialogue avec des dossiers
{: #folders}

Vous pouvez regrouper des noeuds de dialogue en les ajoutant à un dossier. Les raisons de vouloir regrouper des noeuds sont nombreuses, notamment :

- Pour conserver ensemble des noeuds qui traitent d'un sujet similaire afin de pouvoir les trouver plus facilement. Par exemple, vous souhaiterez peut-être regrouper dans un dossier *Compte utilisateur* les noeuds qui traitent de questions concernant les comptes utilisateur et regrouper dans un dossier *Paiement* les noeuds qui gèrent les requêtes liées aux paiements. 
- Pour regrouper un ensemble de noeuds qui ne doivent être traités par le dialogue que si une condition spécifique est remplie. Utilisez une condition, par exemple, `$isPlatinumMember`, pour regrouper des noeuds offrant des services supplémentaires qui ne doivent être traités que si l'utilisateur en cours est autorisé à recevoir les services supplémentaires. 
- Pour rendre les noeuds invisibles pour le contexte d'exécution pendant que vous travaillez dessus. Vous pouvez ajouter les noeuds à un dossier avec une condition `false` afin d'empêcher leur traitement. 
- Pour appliquer les mêmes paramètres de configuration de digression dans un noeud à plusieurs noeuds racine à la fois. Pour plus d'informations, reportez-vous à la rubrique [Digressions](dialog-runtime.html#digressions). 

Ces caractéristiques du dossier ont un impact sur la façon dont les noeuds d'un dossier sont traités :

- Condition : si une condition est spécifiée, le service évalue d'abord la condition de dossier afin de déterminer s'il convient de traiter les noeuds contenus dans le dossier. 
- Personnalisations : tous les paramètres de configuration que vous appliquez au dossier sont hérités par les noeuds du dossier. Si vous modifiez les paramètres de digression du dossier, par exemple, les modifications sont héritées par tous les noeuds présents dans le dossier. 
- Hiérarchie d'arborescence : les noeuds présents dans un dossier sont traités en tant que noeuds racine ou enfant selon que le dossier est ajouté à l'arborescence de dialogue au niveau racine ou enfant. Tous les noeuds de niveau racine que vous ajoutez au dossier de niveau racine continuent de fonctionner en tant que noeuds racine ; ils ne deviennent pas des noeuds enfant du dossier, par exemple. Toutefois, si vous déplacez des noeuds de niveau racine dans un dossier qui est un enfant d'un autre noeud, les noeuds racine deviennent des enfants de cet autre noeud. 

Les dossiers n'ont aucun impact sur l'ordre d'évaluation des noeuds. Les noeuds continuent d'être traités du premier au dernier. A mesure que le service parcourt l'arborescence vers le bas, dès qu'il trouve un dossier, si la condition de dossier a pour valeur true, il traite immédiatement le premier noeud du dossier, puis il continue de parcourir l'arborescence vers le bas. Si un dossier n'est pas associé à une condition de dossier, il est transparent pour le service.

### Ajout d'un dossier
{: #folders-add}

Pour ajouter un dossier à une arborescence de dialogue, procédez comme suit :

1.  Dans la vue d'arborescence de l'onglet **Dialog**, cliquez sur **Add folder**.

    Le dossier est ajouté à la fin de l'arborescence de dialogue, juste avant le noeud **Anything else**. Sauf si un noeud existant dans l'arborescence est sélectionné, auquel cas, il est ajouté sous le noeud sélectionné. 

    Si vous souhaitez ajouter le dossier ailleurs dans l'arborescence, à partir du noeud situé au-dessus de l'emplacement où vous souhaitez l'ajouter, cliquez sur l'icône **Autres options** ![Autres options](images/kabob.png), puis sélectionnez **Add folder**.

    Vous pouvez ajouter un dossier au-dessous d'un noeud enfant dans une branche de dialogue existante. Pour ce faire, cliquez sur l'icône **Autres options** ![icône Autres options](images/kabob.png) sur le noeud enfant, puis sélectionnez **Add folder**.

    Le dossier s'ouvre dans la vue édition. 

1.  **Facultatif** : donnez un nom au dossier. 

1.  **Facultatif** : définissez une condition pour le dossier. 

    Si vous ne spécifiez pas une condition, la valeur `true` est utilisée, ce qui signifie que les noeuds présents dans le dossier seront toujours traités. 

1.  Ajoutez des noeuds de dialogue au dossier. 

    - Pour ajouter des noeuds de dialogue au dossier, vous devez les déplacer un par un vers le dossier. 

      Sur le noeud que vous souhaitez déplacer, cliquez sur l'icône **Autres options** ![icône Autres options](images/kabob.png), sélectionnez **Move**, puis cliquez sur le dossier. Sélectionnez **To folder** comme cible du déplacement. 

      A mesure que vous déplacez des noeuds, ils sont ajoutés au début de l'arborescence dans le dossier. Par conséquent, si vous souhaitez conserver l'ordre d'un ensemble de noeuds de dialogue racine consécutifs, par exemple, déplacez-les en commençant par le dernier noeud.
      {: tip}

    - Pour ajouter un nouveau noeud de dialogue au dossier, cliquez sur l'icône **Autres options** ![icône Autres options](images/kabob.png) sur le dossier, puis sélectionnez  **Add node to folder**.

      Le noeud de dialogue est ajouté à la fin de l'arborescence de dialogue dans le dossier. 

### Suppression d'un dossier
{: #folders-delete}

Vous pouvez supprimer un dossier seulement ou supprimer le dossier et tous les noeuds de dialogue qu'il contient. 

Pour supprimer un dossier, procédez comme suit :

1.  Dans la vue d'arborescence de l'onglet **Dialog**, recherchez le dossier que vous souhaitez supprimer. 

1.  Cliquez sur l'icône **Autres options** ![icône Autres options](images/kabob.png) sur le dossier, puis sélectionnez **Delete**.

1.  Effectuez l'une des tâches suivantes :

    - Pour supprimer le dossier seulement et conserver les noeuds de dialogue qu'il contient, désélectionnez la case **Delete the nodes inside the folder**, puis cliquez sur **Yes, delete it**.
    - Pour supprimer le dossier et tous les noeuds de dialogue qu'il contient, cliquez sur **Yes, delete it**.

Si vous avez supprimé le dossier seulement, les noeuds qu'il contenait sont affichés dans l'arborescence de dialogue à l'endroit où le dossier se trouvait. 

## Recherche d'un noeud de dialogue au moyen de son ID
{: #get-node-id}

Vous souhaiterez peut-être rechercher le noeud de dialogue qui est associé à un ID de noeud connu si vous vous trouvez dans l'une des situations suivantes :

- Vous passez en revue des journaux, et un journal fait référence à une section du dialogue au moyen de son ID de noeud.
- Vous souhaitez mapper les ID de noeud répertoriés dans la propriété `nodes_visited` de la sortie du message d'API aux noeuds qui sont affichés dans l'arborescence de votre dialogue.
- Un message d'erreur d'exécution de dialogue vous informe d'une erreur de syntaxe et identifie le noeud à corriger par son ID.

Pour rechercher un noeud à partir de son ID, procédez comme suit :

1.  A partir de l'onglet Dialog de l'outil, sélectionnez n'importe quel noeud dans l'arborescence de votre dialogue.
1.  Fermez la vue d'édition si elle est ouverte pour le noeud en cours.
1.  Dans la zone d'emplacement du navigateur Web, une URL doit s'afficher avec la syntaxe suivante :

    `    https://watson-conversation.ng.bluemix.net/space/instance-id/workspaces/workspace-id/build/dialog#node=node-id
    `

1.  Editez l'URL en remplaçant la valeur `node-id` en cours par l'ID du noeud que vous recherchez, puis soumettez la nouvelle URL. 
1.  Si nécessaire, remettez en évidence l'URL éditée, puis soumettez-la à nouveau.

L'outil effectue une actualisation et le noeud de dialogue portant l'ID de noeud que vous avez spécifié est mis en évidence. Si l'ID de noeud concerne un attribut, un gestionnaire d'attributs ou une réponse conditionnelle, le noeud dans lequel l'attribut ou la réponse conditionnelle est défini est mis en évidence et le modal correspondant s'affiche. 

**Remarque** : si le noeud est toujours introuvable, vous pouvez exporter l'espace de travail et utiliser un éditeur JSON pour effectuer une recherche dans le fichier JSON d'espace de travail. 
