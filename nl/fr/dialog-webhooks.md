---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-09"

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

# Création d'un appel par programmation à partir d'un noeud de dialogue 
{: #dialog-webhooks}

Pour effectuer un appel par programmation, définissez un webhook qui envoie un appel de demande POST à une application externe qui exécute une fonction de programmation. Vous pouvez ensuite appeler le webhook à partir d'un ou plusieurs noeuds de dialogue.

Un webhook est un mécanisme qui vous permet de faire appel à un programme externe en fonction de ce qui se passe dans votre programme. Lorsqu'il est utilisé dans une compétence de dialogue, un webhook est déclenché lorsque l'assistant traite un noeud dont le webhook est activé. Le webhook collecte les données que vous spécifiez ou que vous collectez auprès de l'utilisateur pendant la conversation et que vous enregistrez dans des variables contextuelles. Il envoie les données dans le cadre d'une demande POST HTTP à l'URL que vous spécifiez dans le cadre de votre définition de webhook. L'URL qui reçoit le webhook est le programme d'écoute. Elle effectue une action prédéfinie à l'aide des informations que vous lui transmettez comme indiqué dans la définition de webhook et peut éventuellement renvoyer une réponse.

Regardez cette vidéo pour en savoir plus.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Démonstration Webhooks" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/j8TBqD2rx2o?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Vous pouvez utiliser un webhook pour effectuer les types de tâches suivants : 

- Valider les informations que vous avez collectées auprès de l'utilisateur.
- Interagir avec un service Web externe pour obtenir des informations. Par exemple, vous pouvez vérifier l'heure d'arrivée prévue d'un vol d'un service aérien ou obtenir des prévisions d'un service météo.
- Envoyer des demandes à une application externe, telle qu'un site de réservation de restaurant, pour effectuer une transaction simple pour le compte de l'utilisateur.
- Déclencher une notification par SMS. 
- Déclencher une action Web {{site.data.keyword.openwhisk}}.

Vous ne pouvez pas utiliser un webhook pour appeler une action {{site.data.keyword.openwhisk_short}} qui utilise une authentification IAM (Identity and Access Management) basée sur un jeton.
{: note}

Pour plus d'informations sur l'appel d'une application client, reportez-vous à la rubrique [Appel d'une application client à partir d'un noeud de dialogue](/docs/services/assistant?topic=assistant-dialog-actions-client).

## Définition du webhook
{: #dialog-webhooks-create}

Vous pouvez définir une URL webhook pour une compétence de dialogue, puis appeler le webhook à partir d'un ou de plusieurs nœuds de dialogue. 

L'appel par programmation au service externe doit répondre aux conditions suivantes :

- L'appel doit être une demande HTTP POST.
- Le format de la demande et de la réponse doit être JSON. Par exemple : `Content-Type: application/json`.
- L'appel doit revenir en **5 secondes ou moins**.

  Pour les services moins efficaces que vous devez appeler, vous pouvez gérer l'appel via une application client et transmettre les informations au dialogue en tant qu'étape distincte. Pour plus d'informations, reportez-vous à la rubrique [Appel d'une application client à partir d'un noeud de dialogue](/docs/services/assistant?topic=assistant-dialog-actions-client).
  {: tip}

Pour ajouter les détails du webhook, procédez comme suit : 

1.  A partir de la compétence où vous voulez ajouter le webhook, cliquez sur l'onglet **Options**.

1.  Cliquez sur **Webhooks**.

1.  Dans la zone **URL**, ajoutez l'URL de l'application externe à laquelle vous souhaitez envoyer des appels de demande POST HTTP.

    Par exemple, pour appeler le service Language Translator, spécifiez l'URL de votre instance de service.

    ```bash
    https://gateway.watsonplatform.net/language-translator/api/v3/translate?version=2018-05-01
    ```
    {: codeblock}

    Si l'application externe que vous appelez renvoie une réponse, elle doit pouvoir renvoyer une réponse au format JSON. Pour le service Language Translator, par exemple, vous devez spécifier le format dans lequel vous souhaitez que le résultat soit renvoyé. Pour ce faire, transmettez un en-tête au service. 

1.  Dans la section Headers, ajoutez les en-têtes que vous souhaitez transmettre au service, l'un après l'autre, en cliquant sur **Add header**.

    Par exemple, ajoutez un en-tête qui indique que vous souhaitez que la valeur obtenue soit renvoyée au format JSON.

    <table>
    <caption>Exemple d'en-tête</caption>
      <tr>
      <th>Nom d'en-tête </th>
      <th>Valeur d'en-tête</th>
      </tr>
      <tr>
      <td>Content-Type</td>
      <td>application/json</td>
      </tr>
    </table>
    
1.  Si le service externe exige que vous transmettiez les données d'authentification de base avec la demande, fournissez ces données. Cliquez sur **Add authorization**, ajoutez vos données d'identification aux zones **User name** et **Password**, puis cliquez sur **Save**. 

    Le produit crée une chaîne ASCII codée en base 64 à partir des données d'identification et génère un en-tête qu'il ajoute automatiquement à la page. 

    <table>
    <caption>Exemple d'en-tête</caption>
      <tr>
      <th>Nom d'en-tête </th>
      <th>Valeur d'en-tête</th>
      </tr>
      <tr>
      <td>Autorisation</td>
      <td>De base `<encoded-api-key>`</td>
      </tr>
    </table>
    
    A des fins de test, vous pouvez soumettre une clé d'API sur l'authentification de base. Cliquez sur **Add authorization**, puis ajoutez `apikey` dans la zone **User name** et collez la valeur de clé d'API dans la zone**Password**. Cliquez sur **Save**.
    {: tip}

Les détails du webhook sont sauvegardés automatiquement.

## Ajout d'un appel webhook à un noeud de dialogue 
{: #dialog-webhooks-dialog-node-callout}

Pour utiliser un webhook à partir d'un noeud de dialogue, vous devez activer les webhooks sur le noeud, puis ajouter des détails pour l'appel. 

1.  Cliquez sur l'onglet **Dialog**.

1.  Recherchez le noeud de dialogue dans lequel vous souhaitez ajouter un appel. L'appel au webhook se produit chaque fois que ce noeud est déclenché lors d'une conversation avec un utilisateur.

    Par exemple, vous souhaiterez peut-être envoyer un appel au webhook à partir du noeud `#General_Greetings` .

1.  Cliquez pour ouvrir le noeud de dialogue, puis cliquez sur **Customize**.

1.  Faites défiler jusqu'à la section *Webhooks*, basculez sur **On**, puis cliquez sur **Apply**.

    Si vous ne l'aviez pas déjà activé, le paramètre *Multiple conditional responses* est activé automatiquement et vous ne pouvez pas le désactiver. Ce paramètre est activé pour prendre en charge l'ajout de différentes réponses en fonction du succès ou de l'échec de l'appel webhook. Si vous aviez déjà spécifié une réponse pour le noeud, celle-ci devient la première réponse conditionnelle.
    {: note}

1.  Ajoutez les données que vous souhaitez transmettre à l'application externe en tant que paires clé et valeur dans la section *Parameters*.

    Par exemple, si vous appelez le service Language Translator, vous devez fournir des valeurs pour les paramètres suivants : 

    <table>
    <caption>Exemple de paramètre</caption>
      <tr>
        <th>Clé</th>
        <th>Valeur</th>
        <th>Description</th>
      </tr>
      <tr>
        <td>model_id</td>
        <td>"en-es"</td>
        <th>Identifie les langues d'entrée et de sortie. Dans cet exemple, la demande est pour du texte en anglais (en) à traduire en espagnol (es).</th>
      </tr>
      <tr>
        <td>text</td>
        <td>"How are you?"</td>
        <th>Ce paramètre contient la chaîne de texte que le service doit traduire. Vous pouvez coder cette valeur, transmettre une variable contextuelle, par exemple $saved_text, ou transmettre l'entrée utilisateur directement au service, en spécifiant `<? input.text ?>` comme valeur.</th>
      </tr>
    </table>

    Dans les cas d'utilisation plus complexes, vous pouvez collecter des informations lors d'une conversation avec un utilisateur concernant ses projets de voyage, par exemple. Vous pouvez collecter des dates et des informations de destination et les sauvegarder dans des variables contextuelles que vous pouvez transmettre à une application externe en tant que paramètres.

    <table>
    <caption>Exemple de paramètres de voyage</caption>
      <tr>
        <th>Clé</th>
        <th>Valeur</th>
      </tr>
      <tr>
        <td>depart_date</td>
        <td>$departure</td>
      </tr>
    <tr>
        <td>arrive_date</td>
        <td>$arrival</td>
      </tr>
    <tr>
        <td>origin</td>
        <td>$origin</td>
      </tr>
    <tr>
        <td>destination</td>
        <td>$destination</td>
      </tr>
    </table>

1.  Toute réponse apportée par l'appel est sauvegardée dans la variable de retour. Vous pouvez renommer la variable automatiquement ajoutée à la zone **Return variable**. Si l'appel génère une erreur, cette variable est définie sur `null`. 

    Le nom de la variable générée a la syntaxe `webhook_result_n`, où le `_n` ajouté est incrémenté chaque fois que vous ajoutez un appel webhook à un noeud de boîte de dialogue. Cette convention de dénomination garantit que les noms de variable contextuelle sont uniques dans la compétence de dialogue. Si vous modifiez le nom, veillez à utiliser un nom unique.

1.  Dans la section des réponses conditionnelles, deux conditions de réponse sont ajoutées automatiquement, une réponse indiquant que l'appel Webhook a abouti et qu'une variable de retour est renvoyée. Et une réponse à afficher lorsque l'appel échoue. Vous pouvez éditer ces réponses et ajouter d'autres réponses conditionnelles au noeud.

    Si le rappel renvoie une réponse et que vous connaissez le format de la réponse JSON, vous pouvez éditer la réponse du noeud de dialogue pour inclure uniquement la section de la réponse que vous souhaitez partager avec les utilisateurs. 
    
    Par exemple, le service Language Translator renvoie un objet comme celui-ci : 
    
    ```json
       {
       "translations":[
          {"translation":"¿Cómo estás?"}
       ],
       "word_count":3,
       "character_count":12
       }
    ```
    {: codeblock}
    
    Utilisez une expression SpEL qui extrait uniquement la valeur de texte traduite.

    <table>
    <caption>Exemple de réponses conditionnelles</caption>
      <tr>
        <th>Condition</th>
        <th>Réponse</th>
      </tr>
      <tr>
        <td>$webhook_result_1</td>
        <td>Your words in Espagnol : <? $webhook_result_1.translations[0].translation ?>.</td>
      </tr>
      <tr>
        <td>anything_else</td>
        <td>L'appel vers l'application externe a échoué. Veuillez réessayer ultérieurement.</td>
      </tr>
    </table>

    Si vous utilisez le format recommandé pour la réponse et que la réponse de traduction affichée précédemment est renvoyée, la réponse de l'assistant à l'utilisateur serait la suivante: `Your words in Espagnol : ¿Cómo estás?`

1.  Lorsque vous avez terminé, cliquez sur le X pour fermer le nœud. Vos modifications sont automatiquement sauvegardées.

## Test des webhooks
{: #dialog-webhooks-test}

Lorsque vous ajoutez d'abord un appel webhook, il peut être utile de voir exactement ce qui est renvoyé dans la réponse à partir de l'application externe, des données et de leur format. Pour ce faire, ajoutez cette expression en tant que réponse textuelle à la réponse conditionnelle d'appel réussi : `$webhook_result_n` où `n` est le nombre approprié pour le webhook que vous testez.

Cette réponse renvoie le corps complet de la variable de retour afin que vous puissiez voir ce que l'appel renvoie et décider quoi partager avec l'utilisateur. Vous pouvez ensuite utiliser les méthodes décrites dans [Méthodes de langage d'expression](/docs/services/assistant?topic=assistant-dialog-methods) pour extraire de la réponse uniquement les informations qui vous intéressent.

Vérifiez si certaines entrées utilisateur peuvent générer des erreurs dans l'appel et définissez des méthodes permettant de gérer ces situations. Les erreurs générées par l'application externe sont stockées dans `output.webhook_error.<result_variable>`. Vous pouvez utiliser une réponse conditionnelle de la sorte pendant que vous testez pour capturer ces erreurs : 

| Condition | Réponse |
|-----------|----------|
| output.webhook_error | L'appel a généré cette erreur : <? output.webhook_error.webhook_result_1 ?>. |

Par exemple, vous pouvez ne pas authentifier correctement la demande (401) ou tenter de transmettre un paramètre avec un nom déjà utilisé par l'application externe. Testez le webhook pour découvrir et corriger ces types d’erreurs avant de déployer le webhook. 

## Suppression d'un webhook
{: #dialog-webhooks-delete}

Si vous décidez que vous ne souhaitez pas effectuer d'appel Webhook à partir d'un noeud de boîte de dialogue, ouvrez la page *Customize* du noeud, puis désactivez Webhooks (**Off**).

La section *Parameters* et la zone **Return variable** sont supprimées de l'éditeur de noeud de dialogue. Cependant, toutes les réponses conditionnelles ajoutées pour vous ou que vous avez vous-même ajoutées sont conservées. 

La section *Multiple conditioned responses* est à nouveau modifiable. Vous pouvez choisir de désactiver la fonction. Dans ce cas, seule la première réponse conditionnelle est enregistrée en tant que réponse textuelle unique du noeud. 

Pour modifier le service externe que vous appelez à partir de noeuds de dialogue, modifiez les détails de Webhook définis sur la page Webhooks de l'onglet **Options**. Si le nouveau service s'attend à ce que différents paramètres lui soient transmis, veillez à mettre à jour tous les noeuds de dialogue qui l'appellent. 

## Appel d'IBM Cloud Functions
{: #dialog-webhooks-cf}

Vous écrivez l'URL webhook et fournissez des en-têtes différemment selon que vous appelez une action standard ou une action Web. 

### Appel d'une action web 
{: #dialog-webhooks-cf-web-action}

Les conseils suivants vous aideront à appeler une action Web {{site.data.keyword.openwhisk_short}} à partir de votre dialogue.  

1.  Ouvrez la page **Options** de la compétence, puis cliquez sur **Webhooks**.

1.  Dans la zone **URL**, ajoutez l'URL de l'application externe à laquelle vous souhaitez envoyer des appels de demande POST HTTP.

    Par exemple, pour appeler une action Web {{site.data.keyword.openwhisk_short}}, spécifiez l'URL de l'action Web publique. Par exemple :

    ```bash
    https://us-south.functions.cloud.ibm.com/api/v1/web/my_org_dev/default/Hello%20World.json
    ```
    {: codeblock}

    Si l'application externe que vous appelez renvoie une réponse, elle doit pouvoir renvoyer une réponse au format JSON. 

    Notez que l'URL de la demande dans cet exemple se termine par `.json`. En spécifiant cette extension, vous tirez parti d'une fonctionnalité d'actions Web qui vous permet de spécifier le type de contenu souhaité de la réponse. La spécification de ce type d'extension garantit que, si les actions Web peuvent renvoyer des réponses dans plusieurs formats, une réponse JSON est renvoyée. Pour plus d'informations, reportez-vous à la rubrique [Fonctions supplémentaires](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_extra){: new_window}.
    {: tip}

1.  Il n'est pas nécessaire d'ajouter d'en-tête.

    Les actions Web {{site.data.keyword.openwhisk_short}} ne doivent pas nécessairement être authentifiées. Vous n'avez donc pas besoin de définir un en-tête d'autorisation. 

    Les détails du webhook sont sauvegardés automatiquement.

1.  Cliquez sur l'onglet **Dialog**.

1.  Cliquez pour ouvrir le noeud de dialogue à partir duquel vous souhaitez appeler l'action Web, puis cliquez sur **Customize**.

1.  Faites défiler jusqu'à la section *Webhooks*, basculez sur **On**, puis cliquez sur **Apply**.

1.  Ajoutez les données que vous souhaitez transmettre à l'application externe en tant que paires clé et valeur dans la section *Parameters*.

    Par exemple, si vous appelez l'action Web Hello de {{site.data.keyword.openwhisk_short}}, vous pouvez ajouter les informations suivantes pour transmettre le paramètre de message accepté par cette application : 

    <table>
    <caption>Exemple de paramètre</caption>
      <tr>
        <th>Clé</th>
        <th>Valeur</th>
      </tr>
      <tr>
        <td>message</td>
        <td>"hello"</td>
      </tr>
    </table>

    Lorsque vous appelez une action Web {{site.data.keyword.openwhisk_short}}, vous ne pouvez pas transmettre de paramètres avec la même clé que les paramètres définis dans le cadre de l'action Web. Pour plus d'informations, reportez-vous à la rubrique [Paramètres protégés](/docs/openwhisk?topic=cloud-functions-actions_web#actions_web_protect){: new_window}.
    {: note}

1.  Vous pouvez modifier la réponse du noeud de dialogue pour n'inclure que la section de la réponse que vous souhaitez afficher aux utilisateurs.  

    L'action Web Hello World de {{site.data.keyword.openwhisk_short}} inclut une paire nom et valeur de message dans sa réponse, ainsi que d'autres informations. Pour afficher uniquement le contenu du message, vous pouvez utiliser la syntaxe `<return-variable>.message` pour extraire la section de message uniquement à partir de l'objet de réponse. 

    <table>
    <caption>Exemple de réponses conditionnelles</caption>
      <tr>
        <th>Condition</th>
        <th>Réponse</th>
      </tr>
      <tr>
        <td>$webhook_result_1</td>
        <td>L'application a renvoyé "$webhook_result_1.message".</td>
      </tr>
      <tr>
        <td>anything_else</td>
        <td>L'appel vers l'application externe a échoué. Veuillez réessayer ultérieurement.</td>
      </tr>
    </table>

1.  Lorsque vous avez terminé, cliquez sur le X pour fermer le noeud. Vos modifications sont automatiquement sauvegardées.

### Appel d'une action standard
{: #dialog-webhooks-cf-action}

Vous pouvez appeler une action gérée par Cloud Foundry, mais pas une action utilisant une authentification IAM (Identity and Access Management) basée sur un jeton. 

Les actions {{site.data.keyword.openwhisk_short}} prennent en charge les appels synchrones ou asynchrones. Toutefois, vous ne pouvez pas effectuer d'appel asynchrone vers une action {{site.data.keyword.openwhisk_short}} avec un webhook. Lorsque vous envoyez une demande asynchrone, seul un ID d'activation est renvoyé.

Pour passer un appel synchrone à une action {{site.data.keyword.openwhisk_short}} gérée par Cloud Foundry, procédez comme suit :

1.  A partir de la compétence où vous voulez ajouter le webhook, cliquez sur l'onglet **Options**.

1.  Cliquez sur **Webhooks**.

1.  Dans la zone **URL**, spécifiez l'URL de l'action. 

    Ajoutez un paramètre `?blocking=true` à l'URL d'action pour forcer un appel synchrone à effectuer.

    Cette syntaxe envoie une demande synchrone :

    ```bash
    https://us-south.functions.cloud.ibm.com/api/v1/namespaces/my_org_dev/actions/Hello%20World?blocking=true
    ```
    {: codeblock}

    Lorsque vous appelez une action, vous devez fournir la clé d'API qui est associée à l'action en tant qu'en-tête, qui est décrite à l'étape suivante.

1.  Fournissez les détails d'autorisation avec la demande.

    Pour appeler une action {{site.data.keyword.openwhisk_short}} gérée par Cloud Foundry, procédez comme suit :

    1.  Obtenez la clé d'API. Sur la page Endpoint de {{site.data.keyword.openwhisk_short}}, recherchez la section CURL et cliquez sur l'icône en forme d'oeil pour afficher les détails de la clé. Copiez la clé `user ID:password` répertoriée après le paramètre `-u` dans la commande curl. 
    
    1.  Cliquez sur **Add authorization**, ajoutez vos données d'identification aux zones **User name** et **Password**, puis cliquez sur **Save**. 

    Les données d'identification sont codées et un en-tête est généré et ajouté à la page pour vous.  
    
    ![Illustration de la zone URL et la section Headers de la page Options.](images/webhook-to-cfaction.png)

    <!-- - If you are calling a {{site.data.keyword.openwhisk_short}} action that is managed by IBM Cloud Identity and Access Management (IAM) instead of CLoud Foundry, then you must provide an IAM bearer token to authenticate the request. 
    
      1. Follow the instructions in [Passing an IBM Cloud IAM token to authenticate with a service's API](/docs/iam?topic=iam-iamapikeysforservices#token_auth). 
      
      1. To pass the IAM bearer token, use a header like this:
    
         <table>
         <caption>Header example</caption>
           <tr>
             <th>Header name</th>
             <th>Header value</th>
           </tr>
           <tr>
             <td>Authorization</td>
             <td>Bearer `<IAM token>`</td>
           </tr>
         </table>

        For more information about how to manage IAM tokens, see this [IBM Developer article](https://developer.ibm.com/tutorials/accessing-iam-based-services-from-ibm-cloud-functions/){: external}.
    -->
Les détails du webhook sont sauvegardés automatiquement.

1.  Cliquez sur l'onglet **Dialog**.

1.  Cliquez pour ouvrir le noeud de dialogue à partir duquel vous souhaitez appeler l'action Web, puis cliquez sur **Customize**.

1.  Faites défiler jusqu'à la section *Webhooks*, basculez sur **On**, puis cliquez sur **Apply**.

1.  Ajoutez les données que vous souhaitez transmettre à l'application externe en tant que paires clé et valeur dans la section *Parameters*.

    Lorsque vous appelez une action {{site.data.keyword.openwhisk_short}}, vous *pouvez* transmettre des paramètres avec la même clé que les paramètres définis dans le cadre de l'action. Les paramètres ne sont pas réservés comme pour les actions Web.

1.  Vous pouvez modifier la réponse du noeud de dialogue pour n'inclure que la section de la réponse que vous souhaitez afficher aux utilisateurs.  

    Par exemple, dans la section des réponses conditionnelles, vous pouvez utiliser une expression avec la syntaxe `$webhook_result.response.result.message` pour extraire uniquement le message renvoyé. 

    <table>
    <caption>Exemple de réponses conditionnelles</caption>
      <tr>
        <th>Condition</th>
        <th>Réponse</th>
      </tr>
      <tr>
        <td>$webhook_result_1</td>
        <td>L'application a renvoyé "$webhook_result.response.result.message".</td>
      </tr>
      <tr>
        <td>anything_else</td>
        <td>L'appel vers l'application externe a échoué. Veuillez réessayer ultérieurement.</td>
      </tr>
    </table>

1.  Lorsque vous avez terminé, cliquez sur le X pour fermer le noeud. Vos modifications sont automatiquement sauvegardées.
