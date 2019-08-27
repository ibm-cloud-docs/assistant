---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-01"

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

# Appel d'une application client ![BETA](images/beta.png)
{: #dialog-actions-client}

Ajoutez à votre noeud de dialogue une action client qui met en pause le dialogue afin qu'un processus côté client puisse s'exécuter et renvoyer un résultat dans le cadre du traitement effectué au cours d'un échange de dialogue.
{: shortdesc}

Une action client définit un appel de programmation dans un format standardisé que votre application client externe peut comprendre. Votre application client externe doit utiliser les informations fournies pour exécuter l'appel ou la fonction de programmation et renvoyer le résultat dans le dialogue. 

Cette action ne réalise pas d'appel direct. Elle indique essentiellement au dialogue de faire une pause ici et d’attendre que l’application client externe effectue une action. Le programme exécuté par l'application client peut être celui que vous choisissiez. Veillez à spécifier le nom de l'appel et les détails du paramètre, ainsi que le nom de la variable du message d'erreur, conformément aux règles de formatage JSON décrites plus loin dans cette rubrique. 

Vous pouvez appeler une application client pour effectuer les types de tâches suivants :

- Valider les informations que vous avez collectées auprès de l'utilisateur.
- Effectuer des calculs ou des manipulations de chaîne sur l'entrée utilisateur trop complexes pour être gérés par les méthodes d'expression SpEL prises en charge.
- Obtenir des données à partir d'une autre application ou d'un autre service.

Pour plus d'informations sur l'appel d'un service externe, par exemple une action Web {{site.data.keyword.openwhisk_short}}, reportez-vous à la rubrique [Création d'un appel par programmation à partir d'un noeud de dialogue](/docs/services/assistant?topic=assistant-dialog-webhooks).

Le diagramme suivant illustre la façon dont vous pouvez utiliser un appel client pour obtenir des prévisions météorologique et les renvoyer à l'utilisateur.

![Illustration représentant une question portant sur des prévisions météorologiques et le dialogue envoyant une demande à une application client, qui l'envoie au service externe](images/forecast.png)

Notez que l'action client appelle le programme `MyWeatherFunction`, qui est un programme exécuté par l'application client. L'application client effectue l'appel au service Web externe (`/weather`) pour obtenir les informations de prévision réelles. L'application client renvoie ensuite la réponse au dialogue. Lorsque vous ajoutez une action client à un dialogue, il doit exister une application client qui effectue le traitement proprement dit ou qui transmet des informations entre votre dialogue et tous les services de back-end externes que vous souhaitez utiliser. 

## Procédure
{: #dialog-actions-client-call}

Pour passer un appel de programmation à une application client à partir d'un noeud de dialogue, procédez comme suit :

1.  Dans le noeud de dialogue à partir duquel vous souhaitez passer un appel de programmation, ouvrez l'éditeur JSON.

    - Pour passer un appel de programmation qui est exécuté après l'évaluation de la réponse pour un noeud, ouvrez l'éditeur JSON pour la réponse de noeud.

      ![Illustration montrant comment accéder à l'éditeur JSON associé à une réponse de noeud standard.](images/contextvar-json-response.png)

      Si le paramètre **Multiple responses** est activé (**On**) pour le noeud, vous devez d'abord cliquer sur l'icône d'**édition de réponse** ![Edition de réponse](images/edit-slot.png) pour que le menu **Options** ![Advanced response](images/kabob.png) soit visible.

      ![Illustration montrant comment accéder à l'éditeur JSON associé à un noeud standard doté de plusieurs réponses conditionnelles activées.](images/contextvar-json-multi-response.png)

    Si vous souhaitez afficher ou traiter de manière plus approfondie la réponse à partir du même échange de dialogue, vous devez ajouter un second noeud qui effectue cette action, puis y accéder à partir de ce noeud.
    {: tip}

    - Pour passer un appel pouvant être utilisé par un attribut individuel, cliquez sur l'icône d'**édition d'attribut** ![icône d'édition d'attribut](images/edit-slot.png) pour l'attribut, puis exécutez l'une des actions suivantes :

      - Pour passer un appel de programmation qui s'exécute après que la condition d'attribut renvoie la valeur true, ouvrez l'éditeur JSON qui est associé à la condition d'attribut.

        ![Illustration montrant comment accéder à l'éditeur JSON associé à une condition d'attribut.](images/contextvar-json-slot-condition.png)

      - Pour passer un appel de programmation qui s'exécute après que l'attribut est rempli, ouvrez l'éditeur JSON qui est associé à la response Found. Pour ce faire, à partir du menu **Options** ![icône Options](images/kabob.png) pour l'attribut, cliquez sur **Enable conditional responses**. Pour la réponse Found, cliquez sur l'icône d'**édition de réponse** ![Edition de réponse](images/edit-slot.png). A partir du menu **Options** ![icône Options](images/kabob.png) pour la réponse Found, cliquez sur **Open JSON editor**.

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
          "type":"client",
          "parameters": {
            "<parameter_name>":"<parameter_value>",
            "<parameter_name>":"<parameter_value>"
          },
          "result_variable": "<result_variable_name>"
        }
      ],
      "output": {
        "text": "response text"
      }
    }
    ```
    {: codeblock}

    Le tableau `actions` spécifie les appels de programmation qui doivent être passés à partir du dialogue. Il peut définir jusqu'à cinq appels de programmation distincts. Indiquez les paires nom-valeur suivantes dans le tableau JSON :

    - `<actionName>` : obligatoire. Nom de l'action ou du service à appeler. Spécifiez un nom en utilisant la syntaxe de votre choix. Il s'agit de spécifier un nom que votre application client reconnaîtra et saura gérer. Le nom ne peut pas comporter plus de 256 caractères.

      Exemple : `calculateRate`

    - `<type>` : indique le type d'appel à effectuer. Facultatif : spécifiez `client`, qui est la valeur par défaut.

      Envoie une réponse de message avec des informations d'appel de programmation dans un format standardisé que votre application client externe comprend. Votre application client doit utiliser les informations fournies pour exécuter l'appel ou la fonction de programmation et renvoyer le résultat dans le dialogue. L'objet JSON dans le corps de réponse spécifie le service ou la fonction à appeler, les éventuels paramètres associés à transmettre avec l'appel, et le format du résultat à renvoyer.

    - `<action_parameters>` : tout paramètre attendu par le programme externe, spécifié en tant qu'objet JSON. Les paramètres ne sont requis que s'ils sont exigés par le programme externe.

    - `<result_variable_name>` : nom à utiliser pour référencer l'objet JSON qui est renvoyé par le service ou le programme externe. Le résultat est ajouté à la section de contexte de la réponse `/message`. En d'autres termes, le résultat est stocké sous la forme d'une variable contextuelle de manière à pouvoir être affiché dans la réponse de noeud ou être accessibles par des noeuds de dialogue déclenchés ultérieurement. Toute valeur existante pour la variable contextuelle est remplacée par la valeur qui est renvoyée par l'action. Vous pouvez spécifier `result_variable_name` en utilisant la syntaxe suivante :

      - `my_result`
      - `$my_result`

      Le nom ne peut pas comporter plus de 64 caractères. Le nom de variable ne peut pas contenir les caractères suivants : des parenthèses `()`, des crochets(`[]`), une apostrophe (`'`), des guillemets (`"`) ou une barre oblique inversée (`\`).

      Si vous souhaitez sauvegarder le résultat dans la section de sortie ou d'entrée de la réponse `/message`, vous pouvez ajouter en préfixe l'un des mots clés d'emplacement suivants à `result_variable_name` :

       - `output.` : ajoute le résultat à la section de sortie de la réponse /message. Par exemple, `output.my_result`.
       - `input.` : ajoute le résultat à la section d'entrée de la réponse /message. Par exemple, `input.my_result`.

      Vous pouvez également spécifier un préfixe de mot clé d'emplacement `context.` . Par exemple, `context.my_result`. Toutefois, cela n'est pas obligatoire car le résultat est ajouté au contexte par défaut.

      Vous pouvez inclure des points dans le nom de variable pour créer un objet JSON imbriqué. Par exemple, vous pouvez définir ces variables pour capturer des résultats à partir de deux demandes distinctes vers un service météorologique afin d'obtenir des prévisions pour aujourd'hui et pour demain :

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

      Si plusieurs actions d'un tableau d'actions JSON ajoutent le résultat de leur appel de programmation à la même variable contextuelle, l'ordre dans lequel le contexte est mis à jour compte. Pour chaque type d'action, l'ordre dans lequel les actions sont définies dans le tableau détermine l'ordre dans lequel la valeur de la variable contextuelle est définie. La valeur de la variable contextuelle renvoyée par la dernière action du tableau écrase les valeurs calculées par d'autres actions.

## Exemple d'appel client
{: #dialog-actions-client-example}

L'exemple ci-après illustre un appel passé vers un service météorologique externe. Il est ajouté à l'éditeur JSON qui est associé à la réponse de noeud. Le temps que la réponse de niveau noeud se déclenche, les attributs auront collecté et stocké les informations de date et d'emplacement fournies par l'utilisateur. Cet exemple suppose que le programme appelé est nommé `MyWeatherFunction`, accepte les paramètres `location` et `date` et renvoie un objet JSON `{"forecast": "<value>"}`.

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

Normalement, le service est renvoyé au client uniquement à partir d'une demande POST `/message` lorsqu'une nouvelle entrée utilisateur est requise, par exemple, après l'exécution d'un parent et avant l'exécution de l'un de ses noeuds enfant. Toutefois, si vous ajoutez une action client à un noeud, une fois l'évaluation terminée, le service est toujours renvoyé au client de sorte que le résultat de l'appel d'action puisse être renvoyé. Pour éviter d'attendre une entrée utilisateur lorsque cela n'a pas lieu d'être, par exemple, pour un noeud qui est configuré pour accéder directement à un noeud enfant, le service ajoute la valeur suivante au contexte de message :

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
