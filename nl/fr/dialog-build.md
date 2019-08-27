---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-29"

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
{:table: .aria-labeledby="caption"}

# Création d'un dialogue
{: #dialog-build}

Le dialogue définit ce que votre assistant répond aux clients, en fonction de ce qu'il croit que le client veut.
{: shortdesc}

## Création d'un dialogue
{: #dialog-build-task}

Pour créer un dialogue, procédez comme suit :

1.  Cliquez sur l'onglet **Dialog**, puis cliquez sur **Create dialog**.

    Lorsque vous ouvrez l'éditeur de dialogue pour la première fois, les noeuds suivants sont déjà créés pour vous :

    - **Welcome** : premier noeud. Il contient un message d'accueil qui s'affiche lorsque vos utilisateurs interagissent pour la première fois avec l'assistant. Le message d'accueil peut être édité.

    Ce noeud n'est pas déclenché dans les flux de dialogue initiés par les utilisateurs. Par exemple, les dialogues utilisés dans les intégrations à des canaux tels que Facebook ou Slack ignorent les noeuds avec la condition spéciale `welcome`. Pour plus d'informations, reportez-vous à la rubrique [Initialisation de dialogue](/docs/services/assistant?topic=assistant-dialog-start).
    {: note}

    - **Anything else** : noeud final. Il contient les phrases qui sont utilisées pour répondre aux utilisateurs lorsque leur entrée n'est pas reconnue. Il est possible de remplacer les réponses fournies ou d'en ajouter d'autres ayant une signification similaire afin d'ajouter de la diversité à la conversation. Vous pouvez également spécifier si l'assistant doit renvoyer chacune des réponses définies les unes après les autres ou si elles doivent être renvoyées de façon aléatoire.
1.  Pour ajouter d'autres noeuds à l'arborescence de dialogue, cliquez sur l'icône **Autres options** ![icône Autres options](images/kabob.png) sur le noeud **Welcome**, puis sélectionnez **Add node below**.
1.  Dans la zone **Si l'assistant reconnaît**, entrez une condition, qui, lorsqu'elle est satisfaite, déclenche le traitement du noeud par l'assistant. 

    Pour commencer, vous souhaitez généralement ajouter une intention en tant que condition. Par exemple, si vous ajoutez `#open_account` ici, cela signifie que vous souhaitez que la réponse spécifiée dans ce nœud soit renvoyée à l'utilisateur si l'entrée saisie indique que l'utilisateur souhaite ouvrir un compte.

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

    La condition que vous définissez ne doit pas excéder 2 048 caractères.

    Pour savoir comment tester les valeurs définies dans des conditions, reportez-vous à la rubrique [Conditions](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-conditions).
1.  **Facultatif** : si vous souhaitez collecter plusieurs éléments d'informations auprès de l'utilisateur au sein de ce noeud, cliquez sur **Customize**, puis activez **Slots**. Pour plus d'informations, reportez-vous à la section [Collecte d'informations à l'aide d'attributs](/docs/services/assistant?topic=assistant-dialog-slots).
1.  Entrez une réponse.
    - Ajoutez le texte ou les éléments multimédia que l'assistant doit afficher comme réponse à l'utilisateur.
    - Si vous souhaitez définir des réponses différentes en fonction de certaines conditions, cliquez sur **Customize** puis activez **Multiple responses**.
    - Pour plus d'informations sur les réponses conditionnelles, les réponses enrichies ou pour savoir comment ajouter de la diversité, reportez-vous à la section [Réponses](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses).

1.  Spécifiez ce qu'il convient de faire une fois que le noeud en cours est traité. Vous avez le choix entre les différentes options suivantes :

    - **Wait for user input** : l'assistant attend que l'utilisateur fournisse une nouvelle entrée.
    - **Skip user input** : l'assistant accède directement au premier noeud enfant. Cette option est disponible uniquement si le noeud en cours a au moins un noeud enfant.
    - **Jump to** : l'assistant poursuit le dialogue en traitant le noeud que vous spécifiez. Vous pouvez choisir si l'assistant doit évaluer la condition du noeud cible ou accéder directement à la réponse du noeud cible. Pour plus d'informations, reportez-vous à la rubrique [Configuration de l'action Jump to](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-jump-to-config).

1.  **Facultatif** : si vous souhaitez que ce noeud soit pris en compte lorsque les utilisateurs reçoivent un ensemble de choix de noeuds lors de l'exécution et qu'il leur est demandé de choisir celui qui correspond le mieux à leur objectif, ajoutez une brève description de l'objectif utilisateur géré par ce noeud dans la zone **nom de noeud externe**. Par exemple, *Open an account*.

    ![Forfait Plus ou Premium uniquement](images/plus.png) La zone *nom de noeud externe* n'est affichée que pour les utilisateurs du forfait Plus ou Premium. Pour plus d'informations, reportez-vous à la rubrique [Désambiguïsation](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).

1.  **Facultatif** : donnez un nom au noeud.

    Le nom de noeud de dialogue peut contenir des lettres (au format Unicode), des nombres, des espaces, des traits de soulignement, des traits d'union et des points.

    Le fait de nommer un noeud permet de mémoriser plus facilement son objectif et de le localiser plus facilement lorsqu'il est réduit. Si vous n'indiquez pas de nom, la condition du noeud est utilisée comme nom.

1.  Pour ajouter d'autres noeuds, sélectionnez un noeud dans l'arborescence, puis cliquez sur l'icône **Autres options** ![icône Autres options](images/kabob.png).
    - Pour créer un noeud homologue qui est vérifié ensuite si la condition relative au noeud existant n'est pas satisfaite, sélectionnez **Add node below**.
    - Pour créer un noeud homologue qui est vérifié avant la condition relative au noeud existant, sélectionnez **Add node above**.
    - Pour créer un noeud enfant sur le noeud sélectionné, sélectionnez **Add child node**. Un noeud enfant est traité après son noeud parent.
    - Pour copier le noeud en cours, sélectionnez **Duplicate**.

    Pour plus d'informations sur l'ordre dans lequel les noeuds de dialogue sont traités, reportez-vous à la section [Présentation du dialogue](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-flow).
1.  Testez le dialogue à mesure que vous le créez.
   Pour plus d'informations, reportez-vous à la section [Test de votre dialogue](#dialog-build-test).

## Test de votre dialogue
{: #dialog-build-test}

Si vous apportez des modifications à votre dialogue, vous pouvez les tester à tout moment pour voir de quelle façon le dialogue répond à une entrée.

Les requêtes que vous soumettez via le panneau "Try it out" génèrent des appels d'API `/message`, mais elles ne sont pas consignées et ne comportent pas de frais.

1.  Dans l'onglet Dialog, cliquez sur l'icône ![Try it](images/ask_watson.png).
1.  Dans le panneau de discussion, tapez un texte et appuyez sur Entrée.

    Avant de tester le dialogue, assurez-vous que le système a terminé de s'entraîner avec les dernières modifications que vous avez effectuées. Si tel n'est pas le cas, un message s'affiche dans le panneau *Try it out* :
    {: tip}

    ![Capture d'écran du message indiquant que Watson est en train de s'entraîner](images/training.png)
1.  Vérifiez la réponse pour voir si le dialogue a correctement interprété votre entrée et choisi la bonne réponse.

    La fenêtre de discussion indique quelles intentions et quelles entités ont été reconnues dans l'entrée.

    ![Capture d'écran illustrant la sortie du dialogue de test](images/test_dialog_output.png)

    Si vous souhaitez modifier une entité reconnue à partir de l'entrée, cliquez sur le nom de l'entité pour l'ouvrir dans la page Entities. Si l'intention incorrecte est reconnue, vous pouvez cliquer sur la flèche en regard du nom de l'intention pour la corriger ou marquer le sujet comme non pertinent. Pour plus d'informations, reportez-vous à la rubrique [Amélioration des données d'apprentissage](/docs/services/assistant?topic=assistant-logs#logs-fix-data).

1.  Si vous souhaitez savoir quel noeud de l'arborescence de dialogue a déclenché une réponse, cliquez sur l'icône d'**emplacement** ![Emplacement](images/location.png) qui lui est associée. Si vous ne vous trouvez pas encore dans l'onglet Dialog, ouvrez-le.

        Le noeud source est mis en évidence et la route empruntée par l'assistant dans l'arborescence pour y accéder est mise en évidence. La mise en évidence est conservée jusqu'à ce que vous exécutiez une autre action, telle que la saisie d'une nouvelle entrée de test.

1.  Pour vérifier ou définir la valeur d'une variable contextuelle, cliquez sur le lien **Manage context**.

    Les variables contextuelles que vous avez éventuellement définies dans le dialogue s'affichent.

    De plus, une variable contextuelle `$timezone` est répertoriée. L'interface utilisateur du panneau *Try it out* récupère les paramètres régionaux de l'utilisateur à partir du navigateur Web et les utilise pour définir la variable contextuelle `$timezone`. Cette variable contextuelle facilite le traitement des références temporelles dans les échanges du dialogue de test. Songez à faire la même chose dans votre application utilisateur. Si cette variable contextuelle n'est pas spécifiée, c'est l'heure moyenne de Greenwich qui est utilisée.

    Vous pouvez ajouter une variable et définir sa valeur pour voir comment le dialogue répond lors de l'échange suivant du dialogue de test. Cette fonctionnalité est utile si, par exemple, le dialogue est configuré pour afficher différentes réponses en fonction d'une valeur de variable contextuelle qui est fournie par l'utilisateur.

    1.  Pour ajouter une variable contextuelle, spécifiez le nom de la variable et appuyez sur **Entrée**.
    1.  Pour définir une valeur par défaut pour la variable contextuelle, localisez la variable contextuelle que vous avez ajoutée dans la liste, puis attribuez-lui une valeur.

    Pour plus d'informations, reportez-vous à la section [Variables contextuelles](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context).

1.  Continuez d'interagir avec le dialogue pour voir de quelle façon la conversation circule à travers lui.
    - Pour rechercher et soumettre à nouveau un énoncé de test, vous pouvez appuyer sur la touche de déplacement vers le haut pour faire défiler les entrées récentes.
    - Pour retirer les précédents énoncés de test sur le panneau de discussion et recommencer, cliquez sur le lien **Clear**. Cette action permet non seulement de retirer les énoncés et les réponses de test, mais également les valeurs des variables contextuelles qui ont été définies suite à vos interactions avec le dialogue.

### Etape suivante
{: #dialog-build-next-steps}

Si vous constatez que les intentions ou les entités reconnues sont incorrectes, vous devrez peut-être modifier vos définitions d'intention ou d'entité.

Si les intentions et les entités reconnues sont correctes, mais les noeuds déclenchés dans votre dialogue sont incorrects, vérifiez que vos conditions sont correctement écrites.

Pour obtenir des conseils pour vous aider à démarrer, reportez-vous à la rubrique [Conseils de création de dialogue](/docs/services/assistant?topic=assistant-dialog-tips).

Si vous êtes prêt à exploiter la conversation pour aider vos utilisateurs, intégrez votre assistant à une plateforme de messagerie ou à une application personnalisée. Reportez-vous à la rubrique [Ajout d'intégrations](/docs/services/assistant?topic=assistant-deploy-integration-add).

## Nombre limite de noeuds de dialogue
{: #dialog-build-node-limits}

Le nombre de noeuds de dialogue que vous pouvez créez par compétence dépend du type de votre forfait.

| Forfait     | Nombre de noeuds de dialogue par compétence     |
|------------------|---------------------------:|
| Premium          |                    100 000 |
| Plus             |                    100 000 |
| Standard         |                    100 000 |
| Plus Trial       |                     25 000 |
| Lite             |                     100`*` |
{: caption="Détails du forfait" caption-side="top"}

Les noeuds de dialogue welcome et anything_else préremplis dans l'arborescence sont pris en compte dans le total.

Profondeur d'arborescence maximale : le dialogue prend en charge 2 000 descendants de noeud de dialogue ; le dialogue est plus performant si ce nombre n'excède pas 20.

`*` Les limites sont passées de 25 000 à 100 pour les forfaits Lite le 1er décembre 2018. Les utilisateurs d'instances de service créées avant la modification de la limite ont jusqu'au 1er juin 2019 pour mettre à niveau leur forfait ou modifier les dialogues dans les compétences des instances de service existantes afin de respecter les nouvelles exigences de limite.

Pour connaître le nombre de noeuds de dialogue dans une compétence de dialogue, effectuez l'une des opérations suivantes :

- Si elle n'est pas déjà associée à un assistant, ajoutez la compétence de dialogue à un assistant, puis affichez la vignette de compétence à partir de la page principale de celui-ci. La section *Données formées* répertorie le nombre de nœuds de dialogue.
- Envoyez une demande GET au noeud final de l'API /dialog_nodes et incluez le paramètre `include_count=true`. Par exemple :

  ```curl
  curl -u "apikey:{apikey}" "https://gateway.watsonplatform.net/assistant/api/v1/workspaces/{workspace_id}/dialog_nodes?version=2018-09-20&include_count=true"
  ```

  Dans la réponse, l'attribut `total` de l'objet `pagination` contient le nombre de nœuds du dialogue.

Si le total semble plus important que prévu, il se peut que le dialogue que vous avez créé à partir de l'application soit converti en un objet JSON. Certaines zones qui semblent faire partie d'un noeud unique sont en réalité structurées en tant que noeuds de dialogue distincts dans l'objet JSON sous-jacent.

  - Chaque noeud et dossier est représenté comme son propre noeud.
  - Chaque réponse conditionnelle associée à un noeud unique de dialogue est représentée sous la forme d'un noeud individuel.
  - Pour un noeud avec attributs, chaque attribut, réponse d'attribut trouvé, réponse d'attribut non trouvé, gestionnaire d'attribut et, si elle est définie, la réponse "Prompt for everything" est un noeud individuel. En effet, un noeud avec trois attributs peut être équivalent à onze noeuds de dialogue.

## Recherche dans votre dialogue
{: #dialog-build-search}

Vous pouvez rechercher dans le dialogue un ou plusieurs noeuds de dialogue qui mentionnent un mot ou une expression donnés.

1.  Sélectionnez l'icône de recherche : ![Icône de recherche](images/search_icon.png)

1.  Entrez un terme ou une phrase de recherche.

    La première fois que vous effectuez une recherche, un index est créé. Vous pouvez être invité à patienter pendant que le texte des noeuds de dialogue est indexé.
    {: note}

Les noeuds contenant votre terme de recherche, avec des exemples correspondants, s'affichent. Sélectionnez n'importe quel résultat pour l'ouvrir et l'éditer.

  ![Résultats renvoyés par la fonction de recherche d'intention](images/search_dialog.png)

## Recherche d'un noeud de dialogue au moyen de son ID
{: #dialog-build-get-node-id}

Vous pouvez rechercher un noeud de dialogue par son ID de noeud. Entrez l'ID de noeud complet dans la zone de recherche. Vous souhaiterez peut-être rechercher le noeud de dialogue qui est associé à un ID de noeud connu si vous vous trouvez dans l'une des situations suivantes :

- Vous passez en revue des journaux, et un journal fait référence à une section du dialogue au moyen de son ID de noeud.
- Vous souhaitez mapper les ID de noeud répertoriés dans la propriété `nodes_visited` de la sortie du message d'API aux noeuds qui sont affichés dans l'arborescence de votre dialogue.
- Un message d'erreur d'exécution de dialogue vous informe d'une erreur de syntaxe et identifie le noeud à corriger par son ID.

Une autre façon de découvrir un noeud en fonction de son ID de noeud consiste à effectuer les étapes suivantes :

1.  A partir de l'onglet Dialog, sélectionnez n'importe quel noeud dans l'arborescence de votre dialogue.
1.  Fermez la vue d'édition si elle est ouverte pour le noeud en cours.
1.  Dans la zone d'emplacement du navigateur Web, une URL doit s'afficher avec la syntaxe suivante :

    `https://assistant-location.watsonplatform.net/location/instance-id/workspaces/workspace-id/build/dialog#node=node-id`

1.  Editez l'URL en remplaçant la valeur `node-id` en cours par l'ID du noeud que vous recherchez, puis soumettez la nouvelle URL.
1.  Si nécessaire, remettez en évidence l'URL éditée, puis soumettez-la à nouveau.

La page effectue une actualisation et le noeud de dialogue portant l'ID de noeud que vous avez spécifié est mis en évidence. Si l'ID de noeud concerne un attribut, une condition d'attribut Found ou Not found, un gestionnaire d'attributs ou une réponse conditionnelle, le noeud dans lequel l'attribut ou la réponse conditionnelle est défini est mis en évidence et le modal correspondant s'affiche.

Si le noeud est toujours introuvable, vous pouvez exporter la compétence de dialogue et utiliser un éditeur JSON pour effectuer une recherche dans le fichier JSON de la compétence.
{: tip}

## Copie d'un noeud de dialogue
{: #dialog-build-copy-node}

Vous pouvez dupliquer un noeud afin d'en créer une copie exacte en tant que noeud homologue juste au-dessous dans l'arborescence de dialogue. Le noeud copié proprement dit prend le même nom que le noeud d'origine, mais la mention `- copy`*`n`* y est accolée (*`n`* est un nombre commençant par 1). Si vous dupliquez le même noeud plus d'une fois, la valeur *`n`* du nom est incrémentée de un pour chaque copie afin de vous permettre de la distinguer des autres. Si le noeud n'a pas de nom, il prend le nom `copy`*`n`*.

Lorsque vous dupliquez un noeud qui a des noeuds enfant, ces derniers sont également dupliqués. Les noeuds enfant copiés portent le même nom que les noeuds enfant d'origine. Seule la référence `copy` dans le nom du noeud parent permet de distinguer un noeud enfant copié d'un noeud enfant d'origine.

1.  Sur le noeud que vous souhaitez copier, cliquez sur l'icône **Autres options** ![Autres options](images/kabob.png), puis sélectionnez **Duplicate**.
1.  Pensez à renommer les noeuds copiés ou à éditer leurs conditions pour les rendre distinctes.

## Déplacement d'un noeud de dialogue
{: #dialog-build-move-node}

Chaque noeud que vous créez peut être déplacé ailleurs dans l'arborescence de dialogue.

Vous souhaiterez peut-être déplacer un noeud précédemment créé vers une autre zone du flux dans le modifier la conversation. Vous pouvez déplacer des noeuds pour qu'ils deviennent des éléments apparentés ou des homologues dans une autre branche.

1.  Sur le noeud que vous souhaitez déplacer, cliquez sur l'icône **Autres options** ![icône Autres options](images/kabob.png), puis sélectionnez **Move**.
1.  Sélectionnez un noeud cible dans l'arborescence près de l'emplacement où vous souhaitez déplacer ce noeud. Indiquez si vous souhaitez placer ce noeud avant ou après le noeud cible ou en faire un enfant du noeud cible.

## Organisation du dialogue à l'aide de dossiers
{: #dialog-build-folders}

Vous pouvez regrouper des noeuds de dialogue en les ajoutant à un dossier. Les raisons de vouloir regrouper des noeuds sont nombreuses, notamment :

- Pour conserver ensemble des noeuds qui traitent d'un sujet similaire afin de pouvoir les trouver plus facilement. Par exemple, vous souhaiterez peut-être regrouper dans un dossier *Compte utilisateur* les noeuds qui traitent de questions concernant les comptes utilisateur et regrouper dans un dossier *Paiement* les noeuds qui gèrent les requêtes liées aux paiements.
- Pour regrouper un ensemble de noeuds qui ne doivent être traités par le dialogue que si une condition spécifique est remplie. Utilisez une condition, par exemple, `$isPlatinumMember`, pour regrouper des noeuds offrant des services supplémentaires qui ne doivent être traités que si l'utilisateur en cours est autorisé à recevoir les services supplémentaires.
- Pour rendre les noeuds invisibles pour le contexte d'exécution pendant que vous travaillez dessus. Vous pouvez ajouter les noeuds à un dossier avec une condition `false` afin d'empêcher leur traitement.

Ces caractéristiques du dossier ont un impact sur la façon dont les noeuds d'un dossier sont traités :

- Condition : si aucune condition n'est spécifiée, l'assistant traite directement les noeuds du dossier. Si une condition est spécifiée, l'assistant évalue d'abord la condition de dossier afin de déterminer s'il convient de traiter les noeuds contenus dans le dossier. 
- Personnalisations : tous les paramètres de configuration que vous appliquez au dossier sont hérités par les noeuds du dossier. Si vous modifiez les paramètres de digression du dossier, par exemple, les modifications sont héritées par tous les noeuds présents dans le dossier.
- Hiérarchie d'arborescence : les noeuds présents dans un dossier sont traités en tant que noeuds racine ou enfant selon que le dossier est ajouté à l'arborescence de dialogue au niveau racine ou enfant. Tous les noeuds de niveau racine que vous ajoutez au dossier de niveau racine continuent de fonctionner en tant que noeuds racine ; ils ne deviennent pas des noeuds enfant du dossier, par exemple. Toutefois, si vous déplacez un noeud de niveau racine dans un dossier qui est un enfant d'un autre noeud, le noeud racine devient alors un enfant de cet autre noeud.

Les dossiers n'ont aucun impact sur l'ordre d'évaluation des noeuds. Les noeuds continuent d'être traités du premier au dernier. A mesure que l'assistant parcourt l'arborescence vers le bas, dès qu'il trouve un dossier, si le dossier n'a pas de condition ou s'il a pour valeur true, il traite immédiatement le premier noeud du dossier, puis il continue de parcourir l'arborescence vers le bas. Si un dossier n'a pas de condition de dossier, il est transparent pour l'assistant et chaque noeud du dossier est traité comme tout autre noeud individuel de l'arborescence. 

### Ajout d'un dossier
{: #dialog-build-folders-add}

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

    - Pour ajouter un nouveau noeud de dialogue au dossier, cliquez sur l'icône **Autres options** ![icône Autres options](images/kabob.png) sur le dossier, puis sélectionnez **Add node to folder**.

      Le noeud de dialogue est ajouté à la fin de l'arborescence de dialogue dans le dossier.

### Suppression d'un dossier
{: #dialog-build-folders-delete}

Vous pouvez supprimer un dossier seulement ou supprimer le dossier et tous les noeuds de dialogue qu'il contient.

Pour supprimer un dossier, procédez comme suit :

1.  Dans la vue d'arborescence de l'onglet **Dialog**, recherchez le dossier que vous souhaitez supprimer.

1.  Cliquez sur l'icône **Autres options** ![icône Autres options](images/kabob.png) sur le dossier, puis sélectionnez **Delete**.

1.  Effectuez l'une des tâches suivantes :

    - Pour supprimer le dossier seulement et conserver les noeuds de dialogue qu'il contient, désélectionnez la case **Delete the nodes inside the folder**, puis cliquez sur **Yes, delete it**.
    - Pour supprimer le dossier et tous les noeuds de dialogue qu'il contient, cliquez sur **Yes, delete it**.

Si vous avez supprimé le dossier seulement, les noeuds qu'il contenait sont affichés dans l'arborescence de dialogue à l'endroit où le dossier se trouvait.
