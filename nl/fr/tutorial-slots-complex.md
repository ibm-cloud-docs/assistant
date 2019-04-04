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

# Tutoriel : Amélioration d'un noeud avec attributs
{: #tutorial-slots-complex}

Dans ce tutoriel, vous allez améliorer un noeud simple avec attributs qui collecte les informations nécessaires pour réserver une table dans un restaurant.
{: shortdesc}

## Objectifs du tutoriel
{: #tutorial-slots-complex-objectives}

A la fin du tutoriel, vous saurez comment effectuer les opérations suivantes :

- Tester un noeud avec attributs
- Ajouter des conditions de réponse qui traitent les interactions d'utilisateur courantes
- Anticiper et traiter les entrées utilisateur hors sujet
- Traiter les réponses d'utilisateur inattendues

### Durée
{: #tutorial-slots-complex-duration}

Ce tutoriel dure environ 2 à 3 heures.

### Prérequis
{: #tutorial-slots-complex-prereqs}

Avant de commencer, exécutez le tutoriel [Ajout d'un noeud avec attributs à un dialogue](/docs/services/assistant?topic=assistant-tutorial-slots). Avant de commencer ce tutoriel, vous devez exécuter le premier tutoriel portant sur les attributs car ce sont les attributs créés au cours de ce dernier qui serviront pour le tutoriel Ajout d'un noeud avec attributs à un dialogue.

## Etape 1 : Amélioration du format des réponses
{: #tutorial-slots-complex-fix-format}

Lorsque les valeurs d'entité de système de date et d'heure sont sauvegardées, elles sont converties en un format standardisé. Celui-ci est utilisé pour effectuer des calculs sur les valeurs, mais vous ne souhaiterez peut-être pas exposer ce reformatage aux utilisateurs. Au cours de cette étape, vous allez reformater les valeurs de date (`2017-12-29`) et d'heure (`19:00:00`) qui sont référencées par le dialogue.

1.  Pour reformater la valeur de variable contextuelle $date, cliquez sur l'icône d'**édition de réponse** ![Edition de réponse](images/edit-slot.png) pour l'attribut @sys-date.

1.  Dans le menu **Autres options** ![icône Autres options](images/kabob.png), sélectionnez **Open JSON editor**, puis éditez l'objet JSON qui définit la variable contextuelle. Ajoutez une méthode qui reformate la date en convertissant la valeur `2017-12-29` en un jour complet de la semaine, suivi du jour et du mois complets. Editez l'objet JSON comme suit :

    ```json
    {
      "context": {
        "date": "<? @sys-date.reformatDateTime('EEEE d MMMM') ?>"
      }
    }
    ```
    {: codeblock}

    EEEE indique que vous souhaitez énoncer le jour de la semaine. Si vous spécifiez EEE, le jour de la semaine sera raccourci, et, Vend sera utilisé pour Vendredi, par exemple. MMMM indique que vous souhaitez énoncer le mois. Là encore, si vous spécifiez MMM, le mois sera raccourci, et Déc sera utilisé pour Décembre, par exemple.

1.  Cliquez sur **Save**.

1.  Pour modifier le format dans lequel la valeur d'heure est stockée dans la variable contextuelle $time de manière à utiliser l'heure, les minutes et indiquer AM ou PM, cliquez sur l'icône d'**édition de réponse** ![Edition de réponse](images/edit-slot.png) pour l'attribut @sys-time.

1.  Dans le menu **Autres options** ![icône Autres options](images/kabob.png), sélectionnez **Open JSON editor**, puis éditez l'objet JSON qui définit la variable contextuelle comme suit : 

    ```json
    {
      "context": {
        "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
      }
    }
    ```
    {: codeblock}

1.  Cliquez sur **Save**.

1.  Testez à nouveau le noeud. Ouvrez le panneau "Try it out" et cliquez sur **Clear** pour supprimer les valeurs de variable contextuelle que vous aviez spécifiées lors du précédent test du noeud avec attributs. Pour voir l'impact de vos modifications, utilisez le script suivant :

    <table>
    <caption>Détails de script</caption>
    <tr>
      <th>Locuteur</th>
      <th>Enoncé</th>
    </tr>
    <tr>
      <td>Vous</td>
      <td>je veux faire une réservation </td>
    </tr>
    <tr>
      <td>Watson</td>
      <td>Quel jour voudriez-vous venir ?</td>
    </tr>
    <tr>
      <td>Vous</td>
      <td>Vendredi</td>
    </tr>
    <tr>
      <td>Watson</td>
      <td>Pour quelle heure voulez-vous que la réservation soit faite ? </td>
    </tr>
    <tr>
      <td>Vous</td>
      <td>19h00</td>
    </tr>
    <tr>
      <td>Watson</td>
      <td>Combien serez-vous pour dîner ? </td>
    </tr>
    <tr>
      <td>Vous</td>
      <td>6</td>
    </tr>
    </table>

    Cette fois-ci, Watson répond : `Entendu. Je réserve pour 6 le Vendredi 29 Décembre à 19:00.`

Vous avez réussi à améliorer le format que le dialogue utilise pour faire référence aux valeurs de variable contextuelle dans ses réponses. Désormais, le dialogue utilise `Vendredi 29 Décembre` au lieu de `2017-12-29`. Et, il utilise `19:00` au lieu de `19:00:00`. Pour en savoir plus sur les autres méthodes SpEL que vous pouvez utiliser avec les valeurs de date et d'heure, reportez-vous à la rubrique [Méthodes de traitement de valeurs](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-date-time).

## Etape 2 : Réclamation de toutes les informations en même temps
{: #tutorial-slots-complex-ask-for-everything}

Maintenant que vous avez testé le dialogue plusieurs fois, vous avez peut-être remarqué qu'il peut être incommodant d'avoir à répondre aux demandes d'attribut une par une. Pour éviter aux utilisateurs d'avoir à fournir un élément d'information à la fois, vous pouvez demander dès le début tous les éléments d'information dont vous avez besoin. Ainsi, l'utilisateur aura la possibilité de fournir la totalité ou une partie des informations dans une seule entrée.

Le noeud avec attributs est conçu pour rechercher et sauvegarder toutes les valeurs d'attribut que l'utilisateur fournit alors que le noeud actuel est en cours de traitement. Vous pouvez aider les utilisateurs à tirer parti de cette conception en leur indiquant quelles valeurs spécifier.

Dans cette étape, vous allez apprendre à demander toutes les informations en même temps.

1.  Depuis le noeud principal avec attributs, cliquez sur **Customize**.

1.  Cochez la case **Prompt for everything** pour activer l'invite initiale, puis cliquez sur **Apply**.

   ![Illustration montrant la sélection de la case à cocher Prompt for everything dans le dialogue Customize.](images/slots-prompt-for-everything.png)

1.  De retour dans la vue d'édition de noeud, faites défiler l'écran jusqu'à la zone nouvellement ajoutée **If no slots are pre-filled, ask this first**. Ajoutez l'invite initiale suivante pour le noeud : `Je peux prendre votre réservation. Il vous suffit de me dire le jour et l'heure de la réservation, et pour combien de personnes.`

1.  Cliquez sur l'icône de ![fermeture](images/close.png) pour fermer la vue d'édition de noeud.

1.  Testez cette modification à l'aide du panneau "Try it out". Ouvrez ce panneau, puis cliquez sur **Clear** pour les valeurs de variable contextuelle d'attribut issues du précédent test.

1.  Entrez `Je souhaiterais réserver une table.`

    A présent, le dialogue répond : `Je peux prendre votre réservation. Il vous suffit de me dire le jour et l'heure de la réservation, et pour combien de personnes.`

1.  Entrez `c'est pour samedi. Nous serons 2 pour 20h`

    Le dialogue répond : `Entendu. Je réserve pour 2 Samedi à 20h:00.`

    ![Illustration montrant le panneau Try it out dans lequel l'utilisateur fournit toutes les informations dans une seule entrée.](images/slots-everything-tested.png)

Si l'utilisateur fournit l'une quelconque des valeurs d'attribut dans son entrée initiale, l'invite demandant toutes les informations ne s'affiche pas. Par exemple, l'entrée initiale de l'utilisateur peut être `Je veux faire une réservation pour ce vendredi soir.` Dans ce cas, l'invite initiale est ignorée car vous ne souhaitez pas demander des informations que l'utilisateur a déjà fournies, en l'occurrence, la date (`Vendredi`). A la place, le dialogue affiche l'invite pour l'attribut suivant non renseigné.
{: note}

## Etape 3 : Traitement approprié des zéros 
{: #tutorial-slots-complex-recognize-zero}

Lorsque vous utilisez l'entité système `sys-number` dans une condition d'attribut, les zéros ne sont pas correctement traités. Au lieu de définir la variable contextuelle que vous définissez pour l'attribut sur 0, le service définit la variable contextuelle sur false. En conséquence, l’attribut ne pense pas qu’il est complété et invite l’utilisateur à entrer un nombre jusqu’à ce qu'il spécifie un nombre autre que zéro. 

1.  Testez le noeud afin de mieux comprendre le problème. Ouvrez le panneau "Try it out" et cliquez sur **Clear** pour supprimer les valeurs de variable contextuelle que vous aviez spécifiées lors du précédent test du noeud avec attributs. Utilisez le script suivant :

    <table>
    <caption>Détails de script</caption>
    <tr>
      <th>Locuteur</th>
      <th>Enoncé</th>
    </tr>
    <tr>
      <td>Vous</td>
      <td>je veux faire une réservation </td>
    </tr>
    <tr>
      <td>Watson</td>
      <td>Je peux prendre votre réservation. Il vous suffit de me dire le jour et l'heure de la réservation, et pour combien de personnes.</td>
    </tr>
    <tr>
      <td>Vous</td>
      <td>Nous voulons dîner le 23 mai à 20h. Il y aura 0 convive. </td>
    </tr>
    <tr>
      <td>Watson</td>
      <td>Combien serez-vous pour dîner ? </td>
    </tr>
    <tr>
      <td>Vous</td>
      <td>0</td>
    </tr>
    <tr>
      <td>Watson</td>
      <td>Combien serez-vous pour dîner ? </td>
    </tr>
    </table>

    Vous serez bloqué dans cette boucle jusqu'à ce que vous spécifiiez un nombre autre que 0. 

1.  Pour vous assurer que l'attribut traite correctement les zéros, remplacez la condition d'attribut `@sys-number` par `@sys-number || @sys-number:0`.

1.  Cliquez sur l'icône d'**édition de réponse** ![Edition de réponse](images/edit-slot.png) pour l'attribut.

1.  Lorsque la variable contextuelle est créée, elle utilise automatiquement la même expression que celle spécifiée pour la condition d'attribut. Cependant, la variable contextuelle doit enregistrer un nombre uniquement. Editez la valeur qui a été enregistrée en tant que variable contextuelle pour en retirer l'opérateur `OR`. Dans le menu **Autres options** ![icône Autres options](images/kabob.png), sélectionnez **Open JSON editor**, puis éditez l'objet JSON qui définit la variable contextuelle. Remplacez la variable `"guests":"@sys-number || @sys-number:0"` pour utiliser la syntaxe suivante :

    ```json
    {
      "context": {
        "guests": "@sys-number"
      }
    }
    ```
    {: codeblock}

1.  Cliquez sur **Save**.

1.  Testez à nouveau le noeud. Ouvrez le panneau "Try it out" et cliquez sur **Clear** pour supprimer les valeurs de variable contextuelle que vous aviez spécifiées lors du précédent test du noeud avec attributs. Pour voir l'impact de vos modifications, utilisez le script suivant :

    <table>
    <caption>Détails de script</caption>
    <tr>
      <th>Locuteur</th>
      <th>Enoncé</th>
    </tr>
    <tr>
      <td>Vous</td>
      <td>je veux faire une réservation </td>
    </tr>
    <tr>
      <td>Watson</td>
      <td>Je peux prendre votre réservation. Il vous suffit de me dire le jour et l'heure de la réservation, et pour combien de personnes.</td>
    </tr>
    <tr>
      <td>Vous</td>
      <td>Nous voulons dîner le 23 mai à 20h. Il y aura 0 convive. </td>
    </tr>
    </table>

    Cette fois-ci, Watson répond : `Entendu. Je réserve pour 0 Mercredi 23 mai à 20h:00.`

Vous avez correctement formaté l'attribut numérique pour qu'il traite correctement les zéros. Bien sûr, vous pouvez aussi refuser que le noeud accepte un zéro en tant que nombre valide de convives. Vous apprendrez à valider les valeurs spécifiées par les utilisateurs à l’étape suivante. 

## Etape 4 : Validation des entrées utilisateur
{: #tutorial-slots-complex-slot-conditions}

Jusqu'ici, nous sommes partis du principe que l'utilisateur fournira les types de valeur appropriés pour les attributs. En réalité, ce n'est pas toujours le cas. Vous pouvez prendre en compte les fois où les utilisateurs sont susceptibles de fournir une valeur non valide en ajoutant des réponses conditionnelles aux attributs. Dans cette étape, vous allez utiliser des réponses d'attribut conditionnelles pour effectuer les tâches suivantes :

- Vérifier que la date demandée n'est pas dépassée.
- Vérifier si une heure de réservation demandée se situe dans la période de service.
- Confirmer l'entrée utilisateur.
- Assurez-vous que le nombre de convives indiqué est supérieur à zéro. 
- Indiquer que vous remplacez une valeur par une autre.

Pour valider l'entrée utilisateur, procédez comme suit :

1.  A partir de la vue d'édition du noeud avec attributs, cliquez sur l'icône d'**édition d'attribut** ![Edition d'attribut](images/edit-slot.png) pour l'attribut `@sys-date`.

1.  Dans le menu **Options** ![icône Autres options](images/kabob.png) dans l'en-tête *Configure slot 1*, sélectionnez **Enable conditional responses**.

1.  Dans la section **Found**, ajoutez une réponse conditionnelle en cliquant sur l'icône d'**édition de réponse** ![Edition de réponse](images/edit-slot.png).

1.  Ajoutez la condition et la réponse suivantes pour vérifier si la date spécifiée par l'utilisateur est antérieure à la date du jour :

    <table>
    <caption>Détails de la première réponse conditionnelle pour l'attribut 1</caption>
    <tr>
      <th>Condition</th>
      <th>Réponse</th>
      <th>Action</th>
    </tr>
    <tr>
      <td>`@sys-date.before(now())`</td>
      <td>Vous ne pouvez pas faire de réservation pour une journée passée. </td>
      <td>Clear slot and prompt again</td>
    </tr>
    </table>

1.  Ajoutez une deuxième réponse conditionnelle qui s'affiche si l'utilisateur fournit une date valide. Ce type de confirmation simple permet à l'utilisateur de savoir que sa réponse a été comprise.

    <table>
    <caption>Détails de la deuxième réponse conditionnelle pour l'attribut 1</caption>
    <tr>
      <th>Condition</th>
      <th>Réponse</th>
      <th>Action</th>
    </tr>
    <tr>
      <td>`true`</td>
      <td>$date</td>
      <td>Move on</td>
    </tr>
    </table>

1.  A partir de la vue d'édition du noeud avec attributs, cliquez sur l'icône d'**édition d'attribut** ![Edition d'attribut](images/edit-slot.png) pour l'attribut `@sys-time`.

1.  Dans le menu **Options** ![icône Autres options](images/kabob.png) dans l'en-tête *Configure slot 2*, sélectionnez **Enable conditional responses**.

1.  Dans la section **Found**, ajoutez une réponse conditionnelle en cliquant sur l'icône d'**édition de réponse** ![Edition de réponse](images/edit-slot.png).

1.  Ajoutez les conditions et les réponses suivantes pour vérifier si l'heure spécifiée par l'utilisateur se situe dans la fenêtre de temps autorisée :

    <table>
    <caption>Détails de la réponse conditionnelle pour l'attribut 2</caption>
    <tr>
      <th>Condition</th>
      <th>Réponse</th>
      <th>Action</th>
    </tr>
    <tr>
      <td>`@sys-time.after('21:00:00')`</td>
      <td>Notre dernier service est à 21h. </td>
      <td>Clear slot and prompt again</td>
    </tr>
    <tr>
      <td>`@sys-time.before('09:00:00')`</td>
      <td>Notre premier service est à 9 heures. </td>
      <td>Clear slot and prompt again</td>
    </tr>
    </table>

1.  Ajoutez une troisième réponse conditionnelle qui s'affiche si l'utilisateur fournit une heure valide qui se situe dans la fenêtre de temps autorisée. Ce type de confirmation simple permet à l'utilisateur de savoir que sa réponse a été comprise.

    <table>
    <caption>Détails de la troisième réponse conditionnelle pour l'attribut 2</caption>
    <tr>
      <th>Condition</th>
      <th>Réponse</th>
      <th>Action</th>
    </tr>
    <tr>
      <td>`true`</td>
      <td>Entendu, la réservation est pour $time.</td>
      <td>Move on</td>
    </tr>
    </table>

1.  Editez l'attribut @sys-number pour valider la valeur fournie par l'utilisateur de la manière suivante :

    - Vérifiez que le nombre de convives spécifié est supérieur à zéro.
    - Anticipez et traitez le cas où l'utilisateur change le nombre de convives. 

      Si, à tout moment durant le traitement du noeud avec attributs, l'utilisateur change une valeur d'attribut, la valeur de variable contextuelle d'attribut correspondante est mise à jour. Cela dit, il peut être utile d'informer l'utilisateur que la valeur est remplacée, non seulement pour lui fournir des commentaires en retour précis, mais également pour lui permettre de corriger éventuellement la modification. 

1.  A partir de la vue d'édition du noeud avec attributs, cliquez sur l'icône d'**édition d'attribut** ![Edition d'attribut](images/edit-slot.png) pour l'attribut `@sys-number`.

1.  Dans le menu **Options** ![icône Autres options](images/kabob.png) dans l'en-tête *Configure slot 3*, sélectionnez **Enable conditional responses**.

1.  Dans la section **Found**, ajoutez une réponse conditionnelle en cliquant sur l'icône d'![édition de réponse](images/edit-slot.png), puis ajoutez la condition et la réponse suivantes :

    <table>
    <caption>Détails de la réponse conditionnelle pour l'attribut 3</caption>
    <tr>
      <th>Condition</th>
      <th>Réponse</th>
      <th>Action</th>
    </tr>
    <tr>
      <td>`entities['sys-number']?.value == 0`</td>
      <td>Veuillez indiquez un nombre supérieur à 0.</td>
      <td>Clear slot and prompt again</td>
    </tr>
    <tr>
      <td>`(event.previous_value != null) && (event.previous_value != event.current_value)`</td>
      <td>Entendu, mise à jour du nombre de convives `<? event.previous_value ?>` à `<? event.current_value ?>`.</td>
      <td>Move on</td>
    </tr>
    <tr>
      <td>`true`</td>
      <td>Entendu. La réservation est prise pour $guests convives.</td>
      <td>Move on</td>
    </tr>
    </table>

## Etape 5 : Ajout d'un attribut de confirmation
{: #tutorial-slots-complex-confirmation-slot}

Vous souhaiterez peut-être repenser votre dialogue afin d'appeler un système de réservation externe et effectuer réellement une réservation pour l'utilisateur dans le système. Avant que votre application n'exécute cette action, vous souhaiterez sûrement confirmer auprès de l'utilisateur que le dialogue a bien compris les détails de la réservation. Pour ce faire, ajoutez un attribut de confirmation au noeud.

1.  L'attribut de confirmation s'attend à recevoir une réponse par oui ou par non de la part de l'utilisateur. Vous devez entraîner le dialogue à reconnaître une intention affirmative (#oui) ou négative (#non) dans la première entrée utilisateur.

1.  Cliquez sur l'onglet **Intents** pour revenir à la page Intents. Ajoutez les intentions et les exemples d'énoncés suivants :

- `#oui`

   ```json
   Oui
   Bien sûr
   J'aimerais bien
   Oui, je vous en prie
   Oui, s'il vous plaît.
   Entendu
   Ca me semble bien.
   ```
   {: screen}

   ![Illustration de l'intention #oui](images/slots-yes-intent.png)

- `#non`

   ```json
   Non
   Non merci.
   S'il vous plaît, non.
   Je vous en prie, non !
   Ce n'est pas du tout ce que je veux
   Absolument pas.
   En aucune façon
   ```
   {: screen}

   ![Illustration de l'intention #non](images/slots-no-intent.png)

1.  Revenez à l'onglet **Dialog**, puis cliquez pour éditer le noeud avec attributs. Cliquez sur **Add slot** pour ajouter un quatrième attribut, puis spécifiez les valeurs suivantes pour ce dernier :

    <table>
    <caption>Détails de l'attribut de confirmation</caption>
    <tr>
      <th>Zone Check for</th>
      <th>Zone Save it as</th>
      <th>Zone If not present, ask</th>
    </tr>
    <tr>
      <td>`(#oui || #non) && slot_in_focus`</td>
      <td>$confirmation</td>
      <td>Je vais vous réserver une table pour $guests le $date à $time. Vous confirmez votre commande ?</td>
    </tr>
    </table>

    Cette condition recherche l'une ou l'autre des réponses. Vous devrez indiquer ce qui se produit ensuite selon que l'utilisateur répond par l'affirmative (Oui) ou par la négative (Non) à l'aide des réponses d'attribut conditionnelles. La propriété `slot_in_focus` force la portée de cette condition à s'appliquer uniquement à l'attribut en cours. Ce paramétrage empêche que cet attribut ne soit déclenché par des instructions aléatoires formulées éventuellement par les utilisateurs et pouvant correspondre à une intention `#oui` ou `#non`.

    Par exemple, l'utilisateur peut répondre à l'attribut demandant le nombre de convives en déclarant : `Oui, nous serons 5.`. Vous ne souhaitez pas que l'attribut de confirmation soit accidentellement renseigné avec le mot `Oui` inclus dans cette réponse. En ajoutant la propriété`slot_in_focus` à la condition, le terme Oui ou Non indiqué par l'utilisateur est appliqué à cet attribut uniquement lorsque l'utilisateur répond spécifiquement à l'invite de celui-ci.

1.  Cliquez sur l'icône d'**édition d'attribut** ![Edition d'attribut](images/edit-slot.png). Dans le menu **Options** ![icône Autres options](images/kabob.png) dans l'en-tête *Configure slot 4*, sélectionnez **Enable conditional responses**.

1.  Dans l'invite **Found**, ajoutez une condition qui recherche une réponse Non (`#non`). Utilisez la réponse `Bien. Recommençons. Je vais essayer de suivre cette fois.` Sinon, vous pouvez considérer que l'utilisateur a confirmé les détails de la réservation et effectuer la réservation.

    Lorsque l'intention `#non` est trouvée, vous devez également ramener à la valeur null les variables contextuelles que vous aviez précédemment sauvegardées, afin de pouvoir demander à nouveau les informations. Vous pouvez utiliser l'éditeur JSON pour réinitialiser les valeurs de variable contextuelle. Cliquez sur l'icône d'**édition de réponse** ![Edition de réponse](images/edit-slot.png) pour la réponse conditionnelle que vous venez d'ajouter. A partir du menu **Options** ![Réponse avancée](images/kabob.png), cliquez sur **Open JSON editor**. Ajoutez un bloc `context` qui affecte la valeur `null` aux variables contextuelles d'attribut, comme illustré ci-dessous :

    ```json
    {
      "output":{
        "text": {
          "values": [
            "Bien. Recommençons. Je vais essayer de suivre cette fois."
          ]
        }
      },
  "context":{
        "date": null,
        "time": null,
        "guests": null
      }
    }
    ```
    {: codeblock}

1.  Cliquez sur **Back**, puis sur **Save**.

1.  Cliquez sur l'icône d'**édition d'attribut** ![Edition d'attribut](images/edit-slot.png) à nouveau pour l'attribut de confirmation. Dans l'invite **Not found**, précisez que vous attendez une réponse Yes ou No de la part de l'utilisateur. Ajoutez une réponse avec les valeurs suivantes :

    <table>
    <caption>Détails de la réponse Not found</caption>
    <tr>
      <th>Condition</th>
      <th>Réponse</th>
    </tr>
    <tr>
      <td>`true`</td>
      <td>Répondez par Oui pour indiquer que vous souhaitez que la réservation soit faite telle quelle ou par Non pour indiquer que vous ne le souhaitez pas. </td>
    </tr>
    </table>

1.  Cliquez sur **Save**.

1.  A présent que vous avez des réponses de confirmation pour des valeurs d'attribut et que vous demandez toutes les informations à la fois, vous constaterez peut-être que les réponses d'attribut individuelles s'affichent avant la réponse d'attribut de confirmation, ce qui peut paraître répétitif pour les utilisateurs. Editez les réponses trouvées pour l'attribut afin d'empêcher qu'elles ne s'affichent dans certaines conditions.

1.  Modifiez la condition `true` qui est spécifiée dans le fragment JSON pour la dernière réponse conditionnelle dans l'attribut @sys-date et remplacez-la par `!($time && $guests)`. Par exemple :

    <table>
    <caption>Détails de la deuxième réponse conditionnelle pour l'attribut 1</caption>
    <tr>
      <th>Condition</th>
      <th>Réponse</th>
      <th>Action</th>
    </tr>
    <tr>
      <td>`!($time && $guests)`</td>
      <td>$date</td>
      <td>Move on</td>
    </tr>
    </table>

1.  Modifiez la condition `true` qui est spécifiée dans le fragment JSON pour la dernière réponse conditionnelle dans l'attribut @sys-time et remplacez-la par `!($date && $guests)`. Par exemple :

    <table>
    <caption>Détails de la troisième réponse conditionnelle pour l'attribut 2</caption>
    <tr>
      <th>Condition</th>
      <th>Réponse</th>
      <th>Action</th>
    </tr>
    <tr>
      <td>`!($date && $guests)`</td>
      <td>Entendu, la réservation est pour $time.</td>
      <td>Move on</td>
    </tr>
    </table>

1.  Modifiez la condition `true` qui est spécifiée dans le fragment JSON pour la dernière réponse conditionnelle dans l'attribut @sys-number et remplacez-la par `!($date && $time)`. Par exemple :

    <table>
    <caption>Détails de la deuxième réponse conditionnelle pour l'attribut 3</caption>
    <tr>
      <th>Condition</th>
      <th>Réponse</th>
      <th>Action</th>
    </tr>
    <tr>
      <td>`!($date && $time)`</td>
      <td>Entendu. La réservation est prise pour $guests convives.</td>
      <td>Move on</td>
    </tr>
    </table>

Si vous ajoutez d'autres attributs ultérieurement, vous devez éditer ces conditions afin de prendre en compte les variables contextuelles qui sont associées à ces attributs. Si vous n'incluez pas d'attribut de confirmation, vous pouvez spécifier `!all_slots_filled` uniquement, et cela restera valide quel que soit le nombre d'attributs que vous ajoutez ensuite.

## Etape 6 : Réinitialisation des valeurs de variable contextuelle d'attribut
{: #tutorial-slots-complex-reset-variables}

Vous aurez peut-être remarqué qu'avant chaque test, vous devez d'abord effacer les valeurs de variable contextuelle qui ont été créées au cours du test précédent. Cette opération est obligatoire car le noeud avec attributs demande uniquement aux utilisateurs des informations qu'il considère comme manquantes. Si les variables contextuelles d'attribut sont toutes remplies avec des valeurs valides, aucune invite ne s'affichera. Il en va de même pour le dialogue lors de la phase d'exécution. Vous devez créer dans le dialogue un mécanisme qui rétablit la valeur null pour les variables contextuelles d'attribut de sorte que les attributs puissent être à nouveau renseignés par l'utilisateur suivant. Pour ce faire, vous allez ajouter au noeud avec attributs un noeud parent qui affecte la valeur null aux variables contextuelles.

1.  Dans l'arborescence du dialogue, cliquez sur l'icône **Autres options** ![icône Autres options](images/kabob.png) sur le noeud avec attributs, puis sélectionnez **Add node above**.

1.  Spécifiez `#réservation` comme condition du nouveau noeud. (Il s'agit de la même condition que celle utilisée par le noeud avec attributs, mais vous modifierez la condition pour le noeud avec attributs ultérieurement au cours de cette procédure.)

1.  Cliquez sur l'icône **Options** ![icône Autres options](images/kabob.png) en regard de la réponse de noeud, puis cliquez sur **Open JSON editor**. Ajoutez une entrée pour chaque variable contextuelle d'attribut que vous avez définie dans le noeud avec attributs et affectez-lui la valeur `null`.

    ```json
    {
      "context": {
        "date": null,
        "time": null,
        "guests": null,
        "confirmation": null
      },
      "output": {}
    }
    ```
    {: codeblock}

    ![Illustration montrant l'arborescence de dialogue avec deux noeuds #réservation conditionnés, le premier noeud ayant la valeur null affectée aux variables contextuelles d'attribut](images/slots-adding-parent.png)

1.  Cliquez pour éditer l'autre noeud #réservation, celui que vous avez créé précédemment et auquel vous avez ajouté les attributs.

1.  Remplacez la condition de noeud `#réservation` par `($date == null && $time == null)`, puis fermez la vue d'édition de noeud en cliquant sur l'icône de ![fermeture](images/close.png).

1.  Cliquez sur l'icône **Autres options** ![icône Autres options](images/kabob.png) sur le noeud avec attributs, puis sélectionnez **Move**.

    ![Illustration montrant l'arborescence de dialogue dans laquelle l'utilisateur clique sur l'action Move sur le noeud avec attributs.](images/slots-move-node.png)

1.  Sélectionnez le noeud `#réservation` comme cible du déplacement, puis choisissez **As child node** dans le menu.

1.  Cliquez pour éditer le noeud `#réservation`. Dans la section *And finally*, remplacez l'action *Wait for user input* par **Skip user input**.

    ![Illustration montrant le dialogue réorganisé afin d'inclure un noeud racine avec la condition #réservation et une action de saut (Jump to) configurée pour accéder directement à son noeud enfant, qui est le noeud avec attributs](images/slots-skip-user-input.png)

    Lorsque l'entrée d'un utilisateur correspond à l'intention `#réservation`, ce noeud est déclenché. La valeur null est affectée à toutes les variables contextuelles d'attribut, puis le dialogue accède directement au noeud avec attributs afin de le traiter.

## Etape 7 : Permettre aux utilisateurs de quitter le processus
{: #tutorial-slots-complex-handler}

Ajouter un noeud avec attributs est une action puissante car elle permet aux utilisateurs de continuer à fournir les informations dont vous avez besoin pour leur renvoyer une réponse significative ou effectuer une action en leur nom. Toutefois, il peut arriver qu'un utilisateur qui est en train de communiquer les détails relatifs à une réservation décide de ne pas poursuivre la réservation. Vous devez permettre aux utilisateurs de quitter facilement le processus. Pour ce faire, ajoutez un gestionnaire d'attributs pouvant détecter que l'utilisateur souhaite quitter processus et pouvant quitter le noeud sans enregistrer les valeurs qui ont été collectées.

1.  Vous devez entraîner le dialogue à reconnaître une intention #quitter dans la première entrée utilisateur.

1.  Cliquez sur l'onglet **Intents** pour revenir à la page Intents. Ajoutez l'intention #quitter avec les exemples d'énoncé suivants :

    ```json
    Je veux arrêter
    Sortie !
    Annuler ce processus
    J'ai changé d'avis. Je ne veux pas faire de réservation.
    Arrêtez la réservation
    Attendez, annulez.
    Laissez tomber.
    ```
    {: screen}

    ![Illustration de l'intention #quitter](images/slots-exit-intent.png)

1.  Revenez dans le dialogue en cliquant sur l'onglet **Dialog**. Cliquez pour ouvrir le noeud avec attributs, puis cliquez sur **Manage handlers**.

    ![Illustration montrant le lien Manage handlers sur le noeud avec attributs](images/slots-manage-handlers.png)

1.  Ajoutez les valeurs suivantes aux zones :

    <table>
    <caption>Détails du gestionnaire de niveau noeud</caption>
    <tr>
      <th>Condition</th>
      <th>Réponse</th>
      <th>Action</th>
    </tr>
    <tr>
      <td>`#quitter`</td>
      <td>Entendu, nous nous arrêterons là. Aucune réservation ne sera effectuée.</td>
      <td>Skip to response </td>
    </tr>
    </table>

    L'action **Skip to response** passe directement à la réponse de niveau noeud sans afficher aucune des invites associées aux attributs non renseignés restants.

1.  Cliquez sur **Back**, puis sur **Save**.

1.  A présent, vous devez éditer la réponse de niveau noeud de sorte qu'elle reconnaisse qu'un utilisateur souhaite quitter le processus au lieu d'effectuer la réservation. Ajoutez une réponse conditionnelle pour le noeud.

    A partir de la vue d'édition du noeud avec attributs, cliquez sur **Customize**, cliquez sur l'option à bascule **Multiple responses** pour l'activer (**on**), puis cliquez sur **Apply**.

    ![Illustration montrant l'option à bascule Multiple responses après qu'elle a été activée](images/slots-multi-responses.png)

1.  Faites défiler l'écran jusqu'à la section de réponse du noeud avec attributs, puis cliquez sur **Add response**.

1.  Ajoutez les valeurs suivantes aux zones :

    <table>
    <caption>Détails de la réponse conditionnelle de niveau noeud</caption>
    <tr>
      <th>Condition</th>
      <th>Réponse</th>
    </tr>
    <tr>
      <td>`has_skipped_slots`</td>
      <td>J'ai hâte de vous aider lors de votre prochaine réservation. Bonne journée. </td>
    </tr>
    </table>

    La condition `has_skipped_slots` vérifie les propriétés du noeud avec attributs pour voir si l'un des attributs a été ignoré. Le gestionnaire`#quitter` ignore tous les attributs restants afin d'accéder directement à la réponse de noeud. Par conséquent, lorsque la propriété `has_skipped_slots` est présente, vous savez que l'intention `#quitter` a été déclenchée et que le dialogue peut afficher une autre réponse.

    Si vous configurez plusieurs attributs dans le but d'ignorer d'autres attributs ou si vous configurez un autre gestionnaire d'événements de niveau noeud dans le but d'ignorer des attributs, vous devez utiliser une autre approche pour vérifier si l'intention #quitter a été déclenchée. Pour connaître cette autre méthode, reportez-vous à la rubrique [Traitement des demandes de sortie d'un processus](/docs/services/assistant?topic=assistant-dialog-slots#dialog-slots-node-level-handler).
    {: note}

1.  Vous souhaitez que le service recherche la propriété `has_skipped_slots` avant d'afficher la réponse de niveau noeud standard. Déplacez la réponse conditionnelle `has_skipped_slots` vers le haut de sorte qu'elle soit traitée avant la réponse conditionnelle d'origine, sinon elle ne sera jamais déclenchée. Pour ce faire, cliquez sur la réponse que vous venez d'ajouter, utilisez la **flèche vers le haut** pour la déplacer vers le haut, puis cliquez sur **Save**.

1.  Testez cette modification en utilisant le script suivant dans le panneau "Try it out" :

    <table>
    <caption>Détails de script</caption>
    <tr>
      <th>Locuteur</th>
      <th>Enoncé</th>
    </tr>
    <tr>
      <td>Vous</td>
      <td>je veux faire une réservation </td>
    </tr>
    <tr>
      <td>Watson</td>
      <td>Je peux prendre votre réservation. Il vous suffit de me dire le jour et l'heure de la réservation, et pour combien de personnes.</td>
    </tr>
    <tr>
      <td>Vous</td>
      <td>c'est pour 5 personnes </td>
    </tr>
    <tr>
      <td>Watson</td>
      <td>Entendu. La réservation est pour 5 personnes. Quel jour voudriez-vous venir ?</td>
    </tr>
    <tr>
      <td>Vous</td>
      <td>Laissez tomber</td>
    </tr>
    <tr>
      <td>Watson</td>
      <td>Entendu, nous nous arrêterons là. Aucune réservation ne sera effectuée. J'ai hâte de vous aider lors de votre prochaine réservation. Bonne journée. </td>
    </tr>
    </table>

## Etape 8 : Application d'une valeur valide si l'utilisateur ne parvient pas à en fournir une après plusieurs tentatives

Dans certains cas, il peut arriver qu'un utilisateur ne comprenne pas ce que vous lui demandez. Il se peut qu'il réponde de façon répétée en utilisant des types de valeur incorrects. Pour anticiper cette possibilité, vous pouvez ajouter un compteur pour l'attribut. Après 3 échecs successifs de l'utilisateur à fournir une valeur valide, vous pouvez appliquer une valeur à l'attribut pour le compte de l'utilisateur et poursuivre.

Pour les informations $time, vous allez définir une instruction de suivi qui s'affiche lorsque l'utilisateur ne fournit pas une heure valide.

1.  Créez une variable contextuelle pouvant suivre le nombre de fois où l'utilisateur fournit une valeur qui ne correspond pas au type de valeur que l'attribut attend. Vous souhaitez que la variable contextuelle soit initialisée et prenne la valeur 0 avant que le noeud avec attributs soit traité, par conséquent, vous allez l'ajouter au noeud `#réservation` parent.

1.  Cliquez pour éditer le noeud `#réservation`. Ouvrez l'éditeur JSON associé à la réponse de noeud en cliquant sur l'icône**Options** ![icône Autres options](images/kabob.png) dans la section de réponse et en choisissant **Open JSON editor**. Ajoutez une variable contextuelle nommée `counter` au bas du bloc `"context"` existant, au-dessous de la variable `confirmation`. Affectez la valeur `0` à la variable `counter`.

       ```json
       {
         "context": {
           "date": null,
           "time": null,
           "guests": null,
           "confirmation": null,
           "counter": 0
         },
         "output": {}
       }
       ```
       {: codeblock}

1.  Dans l'arborescence, développez le noeud `#réservation`, puis cliquez pour éditer le noeud avec attributs. 

1.  Cliquez sur l'icône d'**édition d'attribut** ![Edition d'attribut](images/edit-slot.png) pour l'attribut `@sys-time`.

1.  Dans le menu **Options** ![icône Autres options](images/kabob.png) dans l'en-tête *Configure slot 2*, sélectionnez **Enable conditional responses**.

1.  Dans la section **Not found**, ajoutez une réponse conditionnelle.

    <table>
    <caption>Détails de la réponse Not found</caption>
    <tr>
      <th>Condition</th>
      <th>Réponse</th>
    </tr>
    <tr>
      <td>`true`</td>
      <td>Veuillez indiquer l'heure à laquelle vous souhaitez venir. Le restaurant peut vous accueillir de 9h00 à 21h00. </td>
    </tr>
    </table>

1.  Ajoutez un 1 à la variable `counter` à chaque fois que cette réponse est déclenchée. N'oubliez pas que cette réponse est déclenchée uniquement lorsque l'utilisateur ne fournit pas une valeur d'heure valide. Cliquez sur l'icône d'**édition de réponse** ![Edition de réponse](images/edit-slot.png).

1.  Cliquez sur l'icône **Options** ![icône Autres options](images/kabob.png), puis sélectionnez **Open JSON editor**. Ajoutez la définition de variable contextuelle suivante :

    ```json
    {
      "output": {
        "text": {
          "values": [
            "Veuillez indiquer l'heure à laquelle vous souhaitez venir.
              Le restaurant peut vous accueillir de 9h00 à 21h00."
          ]
        }
      },
"context": {
        "counter": "<? context['counter'] + 1 ?>"
      }
    }
    ```
    {: codeblock}

    Cette expression ajoute un 1 au compteur en cours.

1.  Cliquez sur **Back**, puis sur **Save**.

1.  Rouvrez l'attribut @sys-time en cliquant sur l'icône d'**édition d'attribut** ![Edition d'attribut](images/edit-slot.png).

    Vous allez ajouter une deuxième réponse conditionnelle à la section **Not found** qui vérifie si le compteur est supérieur à 1, ce qui indique que l'utilisateur a déjà fourni 3 fois une réponse non valide. Dans ce cas, le dialogue affecte à la valeur d'heure au nom de l'utilisateur l'heure qui est généralement la plus utilisée lorsqu'il s'agit de réserver une table pour dîner dans un restaurant. Ne vous inquiétez pas, l'utilisateur aura la possibilité de modifier la valeur d'heure lorsque l'attribut de confirmation se déclenchera. Cliquez sur **Add a response**.

1.  Ajoutez la condition et la réponse suivantes :

    <table>
    <caption>Détails de la réponse Not found</caption>
    <tr>
      <th>Condition</th>
      <th>Réponse</th>
    </tr>
    <tr>
      <td>`$counter > 1`</td>
      <td>Vous semblez avoir du mal à choisir une heure. Je vais prendre la réservation à 20h00 pour vous.</td>
    </tr>
    </table>

    Vous devez affecter la valeur 20h00 à la variable $time, par conséquent, cliquez sur l'icône d'**édition de réponse** ![Edition de réponse](images/edit-slot.png). Sélectionnez **Open JSON editor**, ajoutez la définition de variable contextuelle, puis cliquez sur **Back**.

    ```json
    {
      "output": {
        "text": {
          "values": [
            "Vous semblez avoir du mal à choisir une heure.
              Je vais prendre la réservation à 20h00 pour vous."
          ]
        }
      },
"context": {
        "time": "<? '20:00:00'.reformatDateTime('h:mm a') ?>"
      }
    }
    ```
    {: codeblock}

1.  La réponse conditionnelle que vous venez d'ajouter comporte une condition plus précise que la condition true utilisée par la première réponse conditionnelle. Vous devez déplacer cette réponse de sorte qu'elle apparaisse avant la réponse conditionnelle d'origine, sinon elle ne sera pas déclenchée. Cliquez sur la réponse que vous venez d'ajouter et utilisez la flèche vers le haut pour la déplacer vers le haut, puis cliquez sur **Save**.

1.  Testez vos modifications en utilisant le script suivant :

| Locuteur | Enoncé |
|---------|-----------|
| Vous     |je veux faire une réservation |
| Watson  |Je peux prendre votre réservation. Il vous suffit de me dire le jour et l'heure de la réservation, et pour combien de personnes.|
| Vous     | demain |
| Watson  | Vendredi, Décembre 29. Pour quelle heure voulez-vous que la réservation soit faite ? |
| Vous     | orange |
| Watson  |Veuillez indiquer l'heure à laquelle vous souhaitez venir. Le restaurant peut vous accueillir de 9h00 à 21h00. |
| Vous     | rose |
| Watson  |Veuillez indiquer l'heure à laquelle vous souhaitez venir. Le restaurant peut vous accueillir de 9h00 à 21h00. |
| Vous     | violet |
| Watson  |Vous semblez avoir du mal à choisir une heure. Je vais prendre la réservation à 20h00 pour vous. Combien serez-vous pour dîner ? |

## Etape 9 : Connexion à un service externe
{: #tutorial-slots-complex-action}

A présent que votre dialogue peut collecter et confirmer les détails de la réservation d'un utilisateur, vous pouvez appeler un service externe pour réserver réellement une table dans le système du restaurant ou via un service de réservations en ligne pour plusieurs restaurations. Pour plus d'informations, reportez-vous à la rubrique [Procédure permettant de passer des appels de programmation à partir d'un noeud de dialogue](/docs/services/assistant?topic=assistant-dialog-actions).

Dans la logique qui appelle le service de réservation, prenez soin de rechercher la zone `has_skipped_slots` et si elle existe, ne poursuivez pas la réservation.

### Récapitulatif
{: #tutorial-slots-complex-summary}

Dans ce tutoriel, vous avez testé un noeud avec attributs et apporté des modifications qui optimisent l'interaction de celui-ci avec des utilisateurs réels. Pour en savoir plus à ce sujet, reportez-vous à la rubrique [Collecte d'informations à l'aide d'attributs](/docs/services/assistant?topic=assistant-dialog-slots).

## Etapes suivantes
{: #tutorial-slots-complex-deploy}

Déployez votre compétence de dialogue en la connectant d’abord à un assistant, puis en déployant l’assistant. Cette opération peut s'effectuer de plusieurs manières. Pour plus d'informations, reportez-vous à la rubrique [Ajout d'intégrations](/docs/services/assistant?topic=assistant-deploy-integration-add). 
