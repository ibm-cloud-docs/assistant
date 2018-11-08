---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-15"

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

# Procédure permettant de passer des appels de programmation à partir d'un noeud de dialogue ![Version bêta](images/beta.png)
{: #dialog-actions}

Définissez des actions qui peuvent passer des appels de programmation à des applications ou des services externes et renvoyer un résultat dans le cadre du traitement qui se produit au sein d'un échange de dialogue.
{: shortdesc}

Vous pouvez utiliser un service externe pour valider les informations que vous avez collectées auprès de l'utilisateur, ou effectuer des calculs ou des manipulations de chaîne dans  l'entrée qui sont trop complexes pour être traités via les expressions et les méthodes SpEL prises en charge. Ou, vous pouvez interagir avec un service Web externe pour obtenir des informations, par exemple un service de trafic aérien, afin de vérifier l'heure d'arrivée prévue d'un vol, ou un service météorologique, afin d'obtenir des prévisions. Vous pouvez même interagir avec une application externe, par exemple, le site de réservation d'un restaurant, afin de finaliser une transaction simple au nom de l'utilisateur. 

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/y0A6X-KNoB8?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Lorsque vous définissez l'appel de programmation, choisissez l'un des types suivants :

- **client** : définit un appel de programmation dans un format standardisé que votre application client externe peut utiliser pour effectuer l'appel ou la fonction de programmation et renvoyer le résultat au dialogue. 
- **server** : appelle une action {{site.data.keyword.openwhisk_short}} directement et renvoie le résultat au dialogue. 

    Actuellement, vous pouvez appeler une action {{site.data.keyword.openwhisk_short}} à partir d'instances {{site.data.keyword.conversationshort}} qui sont hébergées dans la région Sud des Etats-Unis ou Allemagne. 

    **Remarque** : l'instance {{site.data.keyword.openwhisk_short}} utilisée est celle qui est hébergée dans la même région (Sud des Etats-Unis ou Allemagne). Par conséquent, ne définissez pas une action dans une instance {{site.data.keyword.openwhisk_short}} qui est hébergée en Allemagne si vous prévoyez d'y accéder à partir d'une instance de service {{site.data.keyword.conversationshort}} hébergée dans le Sud des Etats-Unis, par exemple : 

    **Important** : utilisez cette méthode uniquement pour passer un appel vers une action {{site.data.keyword.openwhisk_short}} dont vous savez qu'elle peut renvoyer un résultat en **moins de 5 secondes**. La demande émise vers {{site.data.keyword.openwhisk_short}} dépasse le délai d'attente fixé si un appel de service individuel dure plus longtemps. Et si votre dialogue passe plus d'un appel vers un service externe, le temps total alloué pour les appels est de 7 secondes. Si les trois premiers appels aboutissent chacun dans un délai de 2 secondes et que le quatrième appel dure plus d'1 seconde, ce dernier est interrompu, et le message d'erreur relatif à l'appel indique qu'il n'a pas abouti. Pour les services moins efficaces que vous devez appeler, gérez l'appel via votre application client et transmettez les informations au dialogue de manière distincte. 

## Procédure
{: #call-action}

Pour passer un appel de programmation à partir d'un noeud de dialogue, procédez comme suit :

1.  Dans le noeud de dialogue à partir duquel vous souhaitez passer un appel de programmation, ouvrez l'éditeur JSON. 

    - Pour passer un appel de programmation qui est exécuté après l'évaluation de la réponse pour un noeud, ouvrez l'éditeur JSON pour la réponse de noeud. 

      ![Illustration montrant comment accéder à l'éditeur JSON associé à une réponse de noeud standard.](images/contextvar-json-response.png)

      Si le paramètre **Multiple responses** est activé (**On**) pour le noeud, vous devez d'abord cliquer sur l'icône d'**édition de réponse** ![Edition de réponse](images/edit-slot.png) pour que le menu **Options** ![Réponse avancée](images/kabob.png) soit visible. 

      ![Illustration montrant comment accéder à l'éditeur JSON associé à un noeud standard pour lequel la fonction de réponses conditionnelles multiples est activée.](images/contextvar-json-multi-response.png)

    Si vous souhaitez afficher ou traiter de manière plus approfondie la réponse à partir du service externe au sein du même échange de dialogue, vous devez ajouter un second noeud qui effectue cette action, puis y accéder à partir de ce noeud.
    {: tip}

    - Pour passer un appel pouvant être utilisé par un attribut individuel, cliquez sur l'icône d'**édition d'attribut** ![icône d'édition d'attribut](images/edit-slot.png) pour l'attribut, puis exécutez l'une des actions suivantes :

      - Pour passer un appel de programmation qui est exécuté après que la condition d'attribut renvoie la valeur true, ouvrez l'éditeur JSON qui est associé à la condition d'attribut. 

        ![Illustration montrant comment accéder à l'éditeur JSON associé à une condition d'attribut.](images/contextvar-json-slot-condition.png)

      - Pour passer un appel de programmation qui est exécuté après que l'attribut est rempli, ouvrez l'éditeur JSON qui est associé à la response Found. Pour ce faire, à partir du menu **Options** ![icône Options](images/kabob.png) pour l'attribut, cliquez sur **Enable conditional responses**. Pour la réponse Found, cliquez sur l'icône d'**édition de réponse** ![Edition de réponse](images/edit-slot.png). A partir du menu **Options** ![icône Options](images/kabob.png) pour la réponse Found, cliquez sur **Open JSON editor**.

        ![Illustration montrant comment accéder à l'éditeur JSON associé à la réponse conditionnelle d'un attribut.](images/contextvar-json-slot-multi-response.png)

1.  Utilisez la syntaxe suivante pour définir l'appel de programmation :

    ```json
    {
      "context": {
        "variable_name" : "variable_value"
      },
      "actions": [
        {
          "name":"<actionName>",
          "type":"client | server",
          "parameters": {
            "<parameter_name>":"<parameter_value>",
            "<parameter_name>":"<parameter_value>"
          },
          "result_variable": "<result_variable_name>",
          "credentials": "<reference_to_credentials>"
        }
      ],
      "output": {
        "text": "response text"
      }
    }
    ```
    {: codeblock}

    Le tableau `actions` spécifie les appels de programmation qui doivent être passés à partir du dialogue. Il peut définir jusqu'à 5 appels de programmation distincts. Indiquez les paires nom-valeur suivantes dans le tableau JSON : 

    - `<actionName>` : obligatoire. Nom de l'action ou du service à appeler. Le nom ne peut pas comporter plus de 64 caractères.

       - Pour les types d'action client, spécifiez un nom en utilisant la syntaxe de votre choix. Il s'agit de spécifier un nom que votre application client reconnaîtra et saura gérer. 

          Exemple : `calculateRate`

       - Pour les types d'action {{site.data.keyword.openwhisk_short}} (serveur), utilisez cette syntaxe pour fournir le nom qualifié complet de l'action : `/<namespace>/[<package-name>]/<action name>`

         - Si l'action fait partie d'un package, les informations `<package-name>` sont requises, sinon, elles ne sont pas obligatoires. 
         - Si vous appelez une séquence d'actions, spécifiez `<sequence name>` à la place de `<action name>`.
         - La syntaxe généralement utilisée pour l'espace de nom d'une action définie par l'utilisateur est la suivante : `<myIBMCloudOrganizationID>_<myIBMCloudSpace>`. Exemple : `/jdoeorg_prod10/search flights`
         - Les actions fournies avec {{site.data.keyword.openwhisk_short}} comportent souvent l'espace de nom `whisk.system`, mais vous devez toujours vérifier l'espace de nom. Exemple : `/whisk.system/weather/forecast`

    - `<type>` : indique le type d'appel à effectuer. Choisissez l'un des types suivants :

      - **client** : envoie une réponse de message avec des informations d'appel de programmation dans un format standardisé que votre application client externe peut utiliser pour passer l'appel ou effectuer la fonction et obtenir un résultat au nom du dialogue. L'objet JSON dans le corps de réponse spécifie le service ou la fonction à appeler, les éventuels paramètres associés à transmettre avec l'appel, ainsi que le mode de renvoi du résultat. 

      - **server** : appelle une action {{site.data.keyword.openwhisk_short}} (une ou plusieurs) directement. Vous devez définir l'action proprement dite de manière distincte en utilisant {{site.data.keyword.openwhisk}}. Pour plus d'informations, reportez-vous à la section [Création d'une action {{site.data.keyword.openwhisk_short}} action](dialog-actions.html#create-action) ci-après. 

      La spécification du type est facultative. La valeur par défaut est `client`.

    - `<action_parameters>` : tout paramètre attendu par le programme externe, spécifié en tant qu'objet JSON. Les paramètres ne sont requis que s'ils sont exigés par le programme externe. 

    - `<result_variable_name>` : nom à utiliser pour référencer l'objet JSON qui est renvoyé par le service ou le programme externe. Le résultat est ajouté à la section de contexte de la réponse /message. En d'autres termes, le résultat est stocké sous la forme d'une variable contextuelle de manière à pouvoir être affiché dans la réponse de noeud ou être accessibles par des noeuds de dialogue déclenchés ultérieurement. Toute valeur existante pour la variable contextuelle est remplacée par la valeur qui est renvoyée par l'action. Vous pouvez spécifier `result_variable_name` en utilisant la syntaxe suivante : 

      - `my_result`
      - `$my_result`

      Le nom ne peut pas comporter plus de 64 caractères. Le nom de variable ne peut pas contenir les caractères suivants : des parenthèses `()`, des crochets(`[]`), une apostrophe (`'`), des guillemets (`"`) ou une barre oblique inversée (`\`).

      Si vous souhaitez sauvegarder le résultat dans la section de sortie ou d'entrée de la réponse /message, vous pouvez ajouter en préfixe l'un des mots clés d'emplacement suivants à `result_variable_name` :

       - `output.` : ajoute le résultat à la section de sortie de la réponse /message. Par exemple, `output.my_result`.
       - `input.` :  ajoute le résultat à la section d'entrée de la réponse /message. Par exemple, `input.my_result`.

      Vous pouvez également spécifier un préfixe de mot clé d'emplacement `context.` . Par exemple, `context.my_result`. Toutefois, cela n'est pas obligatoire car le résultat est ajouté au contexte par défaut. 

      Vous pouvez inclure des points dans le nom de variable pour créer un objet JSON imbriqué. Par example, vous pouvez définir ces variables pour capturer des résultats à partir de deux demandes distinctes vers un service météorologique afin d'obtenir des prévisions pour aujourd'hui et pour demain : 

      - `context.weather.today`
      - `context.weather.tomorrow`

      Les résultats (valeurs de paramètre `temp` et `rain`) sont stockés dans le contexte de cette structure :
      ```json
      {
        "weather": {
          "today": {
            "temp": "20",
            "rain": "30"
          },
          "tomorrow": {
            "temp": "23",
            "rain": "80"
          }
        }
      }
      ```
      {: codeblock}

      Si plusieurs actions d'un tableau d'actions JSON ajoutent le résultat de leur appel de programmation à la même variable contextuelle, l'ordre dans lequel le contexte est mis à jour compte :

      1. Si vous avez une combinaison d'actions serveur et client dans le tableau, le service traite d'abord les actions de type de serveur. Par conséquent, la valeur qui est calculée pour la variable contextuelle par la dernière action de type de client du tableau écrase la valeur calculée pour elle par n'importe quelle action de type de serveur. 
      1. Pour chaque type d'action, l'ordre dans lequel les actions sont définies dans le tableau détermine l'ordre dans lequel la valeur de la variable contextuelle est définie. La valeur de la variable contextuelle renvoyée par la dernière action du tableau écrase les valeurs calculées par d'autres actions. 

    - `<reference_to_credentials>` : nom de l'objet dans lequel les données d'identification {{site.data.keyword.openwhisk_short}} sont stockées. Requis uniquement pour les actions de serveur. Ces données d'identification sont utilisées pour accéder à l'instance {{site.data.keyword.openwhisk_short}} sur laquelle l'action s'exécute. Il ne s'agit pas de vos données d'identification {{site.data.keyword.Bluemix_notm}}. 

      Pour découvrir les données d'identification, procédez comme suit :
      1.  Accédez à la page [{{site.data.keyword.openwhisk_short}} API key ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/openwhisk/learn/api-key){: new_window}. 

          - Si vous n'avez pas encore de compte, créez-en. 
          - Si vous n'êtes pas connecté, connectez-vous. 

      1.  Cliquez sur l'icône d'**affichage de clé d'authentification** ![Affichage de clé d'authentification](images/show-auth-icon.png) pour afficher les données d'identification. Le segment situé avant le signe deux-points (:) est votre ID utilisateur. Le segment situé après le signe deux-points est votre mot de passe.

      **Attention**: tous les frais encourus lors de l'exécution de l'action sont facturés à la personne qui possède ces données d'identification.

      Pour protéger les données d'identification, ne les stockez pas dans l'espace de travail {{site.data.keyword.conversationshort}}. En revanche, vous pouvez les transmettre à partir de l'application client dans le cadre du contexte. Vous pouvez empêcher le stockage des informations dans des journaux Watson en imbriquant votre variable contextuelle au sein de la section $private du contexte de message. Par exemple : `$private.my_credentials`.

      Les objets de données d'identification que vous définissez doivent contenir les paramètres `user` et `password`.

      ```json
      {
        "user":"5tj3b41j-bf3j-5d92-24g9-4a7769ab12af",
        "password":"y65gqSTSRzqE..."
      }
      ```
      {: codeblock}

      Lors du test du dialogue, vous pouvez définir temporairement la variable contextuelle `$private.my_credentials` avec vos valeurs de nom d'utilisateur et de mot de passe {{site.data.keyword.openwhisk_short}} réelles en cliquant sur **Manage context** à partir du panneau "Try it out" dans les outils. 

      ![Illustration montrant de quelle façon la variable contextuelle $private.my_credentials est définie dans l'interface de gestion de contexte Try it out](images/testing-creds.png)

## Création d'une action {{site.data.keyword.openwhisk_short}}
{: #create-action}

Si vous choisissez de définir un appel de programmation de type de serveur, avant de pouvoir l'appeler à partir d'un dialogue, vous devez créer l'action dans {{site.data.keyword.openwhisk}}. Si vous définissez un appel de programmation de type de client, ignorez cette procédure. 

**Remarque :** l'exécution d'une action {{site.data.keyword.openwhisk_short}} peut entraîner un coût. Pour plus d'informations, reportez-vous à la page [Tarification ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/openwhisk/learn/pricing){: new_window}. {{site.data.keyword.openwhisk_short}} ne fait pas la distinction entre les appels effectués depuis le panneau "Try it out" durant le test et les appels passés à partir d'une application dans un environnement de production. 

Pour créer une action {{site.data.keyword.openwhisk_short}}, procédez comme suit :

1.  Accédez à l'[éditeur {{site.data.keyword.openwhisk_short}} en ligne ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.ng.bluemix.net/openwhisk/create){: new_window}, dans lequel vous pouvez écrire votre code directement dans votre navigateur. 

    Il existe également une [interface de ligne de commande ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/openwhisk/learn/cli){: new_window} que vous pouvez installer afin de vous permettre de définir une action à l'aide du code que vous écrivez localement. 

1.  Créez une action {{site.data.keyword.openwhisk_short}} à l'aide de l'un des langages de programmation pris en charge. Pour plus d'informations, reportez-vous à la [documentation {{site.data.keyword.openwhisk_short}}![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.ng.bluemix.net/docs/openwhisk/openwhisk_actions.html){: new_window}. 

    Gardez à l'esprit les astuces suivantes :

    - Reportez-vous à l'[exemple d'action {{site.data.keyword.openwhisk_short}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.ng.bluemix.net/docs/openwhisk/openwhisk_actions.html#openwhisk_apicall_action){: new_window} pour voir comment appeler un service externe. 
    - Afin de passer un appel au service Watson, utilisez le [logiciel SDK Watson Developer Cloud ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/watson-developer-cloud){: new_window} pour la langue que vous souhaitez utiliser. 
    - Assurez-vous que votre action {{site.data.keyword.openwhisk_short}} accepte n'importe quel paramètre d'entrée en tant qu'objet JSON et renvoie n'importe quelle sortie sous la forme d'un objet JSON. 
    - Si vous utilisez Node.js pour écrire votre action {{site.data.keyword.openwhisk_short}}, prenez soin d'utiliser `Promise` pour le traitement asynchrone. Prenez soin également de renvoyer le résultat final à partir de la fonction `main`. 

    Vous pouvez aussi créer une séquence d'actions.
    {: tip}

## Traitement des erreurs

Si l'action {{site.data.keyword.openwhisk_short}} détecte une erreur, le message d'erreur est renvoyé au dialogue et est stocké en tant que propriété de la variable de réponse nommée `cloud_functions_call_error`. L'erreur peut se produire si votre action {{site.data.keyword.openwhisk_short}} ne peut pas obtenir une réponse d'un service externe, ou si l'action Cloud Function échoue, par exemple. Si les données d'identification de Cloud Function ne sont pas fournies ou sont incorrectes, une erreur est renvoyée. Cette variable contextuelle est utilisée uniquement pour les actions de serveur ; dans votre application client, pensez à créer un objet similaire qui capture des informations d'erreur et les renvoie au dialogue en tant que variable contextuelle. 

Vous pouvez définir une condition pour la réponse de noeud de dialogue de manière à d'abord rechercher les erreurs. Par exemple, vous pouvez vous assurer que la réponse qui référence un résultat d'action {{site.data.keyword.openwhisk_short}} apparaît uniquement si aucune erreur n'est détectée en ajoutant l'expression suivante à la condition de réponse : 

```json
  $forecast_result.cloud_functions_call_error == null
```
{: codeblock}

Pour un appel de programmation de type de client, vous pouvez transmettre des informations relatives au traitement des erreurs en définissant une variable contextuelle, par exemple, `action_error`. Vous pouvez les retransmettre au service via la variable de résultat. Ensuite, vous pouvez afficher une réponse uniquement si aucune erreur n'a été détectée en définissant une condition de réponse semblable à la suivante : 

```json
  $forecast_result.action_error == null
```
{: codeblock}

## Exemple d'appel client
{: #action-client-example}

L'exemple ci-après illustre un appel passé vers un service météorologique externe. Il est ajouté à l'éditeur JSON qui est associé à la réponse de noeud. Le temps que la réponse de niveau noeud se déclenche, les attributs auront collecté et stocké les informations de date et d'emplacement fournies par l'utilisateur. Cet exemple part du principe que le service qui sera appelé comporte un point d'extrémité nommé `/weather`, accepte les paramètres `location` et `date` et renvoie un objet JSON, `{"forecast": "<value>"}`.

 ``` json
{
  "actions": [
    {
      "name": "MyWeatherFunction",
      "type": "client",
      "parameters": {
        "date": "$date",
        "location": "$location"
      },
      "result_variable": "context.my_forecast"
    }
  ]
}
```
{: codeblock}

Normalement, le service est renvoyé au client uniquement à partir d'une demande POST /message lorsqu'une nouvelle entrée utilisateur est requise, par exemple, après l'exécution d'un parent et avant l'exécution de l'un de ses noeuds enfant. Toutefois, si vous ajoutez une action client à un noeud, une fois l'évaluation terminée, le service est toujours renvoyé au client de sorte que le résultat de l'appel d'action puisse être renvoyé. Pour éviter d'attendre une entrée utilisateur lorsque cela n'a pas lieu d'être, par exemple, pour un noeud qui est configuré pour accéder directement à un noeud enfant, le service ajoute la valeur suivante au contexte de message :

```json
  {
    "context": {
      "skip_user_input": true
    }
  }
```
{: codeblock}

Si vous souhaitez que le client effectue une action, sans obtenir d'entrée utilisateur, vous pouvez suivre la même convention et ajouter la variable contextuelle `skip_user_input` au noeud parent pour communiquer cette information à l'application client.

Votre application client doit toujours rechercher la variable contextuelle `skip_user_input`. Si elle est présente, il sait qu'il ne doit pas demander de nouvelle entrée à l'utilisateur, mais en revanche, qu'il doit exécuter l'action, ajouter son résultat au message et le retransmettre au service. La nouvelle demande POST /message doit inclure le message renvoyé par la précédente réponse POST /message (autrement dit, le contexte, l'entrée, les intentions, les entités et éventuellement la section de sortie) et, à la place de l'objet JSON qui définit l'appel de programmation à passer, elle doit inclure le résultat qui a été renvoyé par l'appel de programmation.

Dans un noeud enfant auquel vous accédez directement après ce noeud, ajoutez la réponse à afficher à l'utilisateur :

``` json
{
  "output": {
    "text": {
      "values": [
        "It will be $my_forecast $date.literal in $location.literal."
      ]
    }
  }
```
{: codeblock}

Le diagramme suivant illustre la façon dont vous pouvez utiliser un appel client pour obtenir des prévisions météorologique et les renvoyer à l'utilisateur.

![Illustration montrant une question posée au sujet de prévisions météorologiques et le renvoi par le dialogue de la réponse à une application client, laquelle l'envoie ensuite au service externe](images/forecast.png)
## Server call example
{: #action-server-example}

L'exemple ci-après illustre un appel passé vers une action {{site.data.keyword.openwhisk_short}}. Cet exemple montre comment utiliser l'action {{site.data.keyword.openwhisk_short}} `echo` qui est définie dans le package [Utilities package ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/docs/openwhisk/openwhisk_actions.html#openwhisk_create_action_sequence){: new_window} fourni avec le service. L'action prend une chaîne de texte et la renvoie.
``` json
    {
  "actions": [
    {
      "name": "/whisk.system/utils/echo",
      "type":"server",
      "parameters": {
        "message": "<?input.text?>"
      },
      "result_variable": "context.my_input_returned",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

La sortie de l'action {{site.data.keyword.openwhisk_short}}, qui est stockée dans la variable `context.my_input_returned`, est désormais accessible par les noeuds de dialogue suivants.
 ``` json
    {
  "output": {
    "text": {
      "values": [
        "Your input was: $my_input_returned."
      ]
    }
  }
}
```
{: codeblock}

Le diagramme ci-après montre comment passer un appel vers une action {{site.data.keyword.openwhisk_short}} avec un exemple simple qui appelle le service echo {{site.data.keyword.openwhisk_short}} intégré. Il demande à l'utilisateur une entrée et la transmet au service echo. Le service echo renvoie le même texte, qui est affiché pour l'utilisateur.

![Illustration montrant la saisie d'un texte et l'envoi par le dialogue de la demande au service {{site.data.keyword.openwhisk_short}}, puis le renvoi du texte au dialogue.](images/echo-via-cf.png)

### Exemple d'action echo

Pour voir un espace de travail avec un dialogue qui est déjà configuré pour appeler l'action echo {{site.data.keyword.openwhisk_short}} intégrée, procédez comme suit :
1.  Téléchargez le fichier [CloudFunctionsEcho.json ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/watson-developer-cloud/community/raw/master/conversation/cloud-functions-echo.json){: new_window}.
1.  Importez le fichier JSON en tant que nouvel espace de travail.
1.  Passez en revue le dialogue pour voir comment l'appel vers l'action Echo est spécifié.
1.  Sur le panneau "Try it out", cliquez sur **Manage context**, puis (temporairement) affectez aux variables contextuelles votre nom d'utilisateur et votre mot de passe {{site.data.keyword.openwhisk_short}}.
```json
    $private.my_credentials
    {
      "user":"<your-CF-instance-username>",
      "password":"<your-CF-instance-password>"
    }
    ```
    {: codeblock}

1.  Testez le dialogue en saisissant des entrées.

    Le service utilisera l'action echo {{site.data.keyword.openwhisk_short}} pour vous répéter ce que vous entrez.

## Exemple d'appel de serveur avancé
{: #advanced-action-server-example}

Vous pouvez appeler plusieurs actions à partir d'un seul flux de dialogue. En fait, vous pouvez appeler jusqu'à cinq actions au sein d'un objet JSON `actions` dans un seul noeud de dialogue. Toutefois, les actions de type de serveur qui sont définies dans un tableau JSON `actions` sont toutes traitées en parallèle. Par conséquent, vous ne pouvez pas appeler une action de type de serveur et transmettre le résultat depuis cette dernière vers la seconde action de type de serveur dans le même bloc `actions`. Le meilleur moyen d'appeler des actions de serveur dans un ordre spécifique est d'utiliser une séquence {{site.data.keyword.openwhisk_short}}. Lors de la phase d'exécution, cette approche est plus rapide car le dialogue a juste à passer un seul appel externe pour exécuter plusieurs actions. Pour utiliser une séquence, il vous suffit de référencer le nom de séquence à la place d'un nom d'action dans la définition de bloc `actions`. Vous pouvez aussi appeler la première action de type de serveur à partir d'un noeud et accéder directement à un noeud enfant qui appelle l'action de type de serveur suivante.

Si vous définissez un tableau `actions` comportant à la fois des actions de type de client et des actions de type de serveur, lorsque le noeud de dialogue est exécuté, les actions de client ne sont pas envoyées au client tant que toutes les actions de type de serveur n'ont pas été traitées.
{: tip}

Les exemples suivants illustrent un appel vers une action {{site.data.keyword.openwhisk_short}} :

Cet exemple montre comment utiliser une action {{site.data.keyword.openwhisk_short}} pour appeler un service externe qui prend un nom de ville et renvoie les coordonnées de latitude et de longitude de l'emplacement fourni.

``` json
{
  "actions": [
        {
      "name": "/jdoeorg_prod/get coordinates",
      "type":"server",
      "parameters": {
        "location": "$location"
      },
      "result_variable": "context.my_coordinates",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

La variable contextuelle `$my_coordinates` sauvegarde les deux valeurs qui sont renvoyées par le service `get coordinates` comme suit :

```json
{
 "lat":"42.3611",
 "long":"-71.0571"
}
```
{: codeblock}

Cet exemple montre comment utiliser l'action {{site.data.keyword.openwhisk_short}} `forecast` qui est définie dans le package [Weather package ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://console.bluemix.net/docs/openwhisk/openwhisk_weather.html#openwhisk_catalog_weather){: new_window} fourni avec le service {{site.data.keyword.openwhisk_short}}. L'action attend des coordonnées de latitude et de longitude et une période. Elle renvoie un objet JSON avec les prévisions météorologiquespour le lieu spécifié et la période indiquée. Les coordonnées, qui sont renvoyées par l'action précédente, sont spécifiées comme suit : `$my_coordinates.lat` et  `$my_coordinates.long`.
 ``` json
{
  "actions": [
    {
      "name": "/whisk.system/weather/forecast",
      "type":"server",
      "parameters": {
        "latitude": "$my_coordinates.lat",
        "longitude": "$my_coordinates.long",
        "timePeriod": "$period",
        "username": "$private.my_weather_service.username",
        "password": "$private.my_weather_service.password"
      },
      "result_variable": "context.forecasts",
      "credentials":"$private.my_credentials"
    }
  ]
}
```
{: codeblock}

**Remarque** : un nom d'utilisateur et un mot de passe sont répertoriés en tant que paramètres. Leur présence est due au fait qu'ils sont requis par cette action spécifique ; ils définissent les données d'identification requises par le service Weather externe que l'action fournie appelle sur le système de back end. Ils sont différents des données d'identification de compte IBM Cloud Function. Prenez les mesures nécessaires pour assurer la confidentialité de ces données d'identification.

La sortie de l'action {{site.data.keyword.openwhisk_short}}, qui est stockée dans la variable `context.forecasts`, est désormais accessible par les noeuds de dialogue suivants.
 ``` json
{
  "output": {
    "text": {
      "values": [
        "For the next $period in $location, you can expect $forecasts."
      ]
    }
  }
}
```
{: codeblock}

Le diagramme ci-après illustre une interaction complexe. Un utilisateur demande des prévisions météorologiques. Les attributs du noeud de dialogue invitent l'utilisateur à entrer le lieu et la période. Pour appeler le service {{site.data.keyword.openwhisk_short}} de prévisions météorologiques, vous devez fournir des coordonnées géographiques. Par conséquent, le dialogue commence par obtenir les détails de latitude et de longitude pour le lieu indiqué par l'utilisateur en envoyant une demande au service {{site.data.keyword.openwhisk_short}}, qui la transmet à un service externe qui renvoie les coordonnées. Dans un noeud suivant, le dialogue envoie les coordonnées nouvellement obtenues, ainsi que la période fournie par l'utilisateur au service de prévisions météorologiques intégré à {{site.data.keyword.openwhisk_short}} pour obtenir les détails relatifs aux prévisions météorologiques. Le dialogue répond alors à la question de l'utilisateur en affichant le résultat des prévisions météorologiques qui a été fourni par le service de prévisions météorologiques {{site.data.keyword.openwhisk_short}}.
![Shows someone asking for a weather forecast, and the dialog sending the request to the Cloud Function service to pass it to the external service.](images/forecast-via-cf.png)
