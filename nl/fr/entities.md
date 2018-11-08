---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-30"

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

# Définition d'entités

Les ***entités*** représentent une classe d'objet ou un type de données pertinent pour l'objectif d'un utilisateur. En reconnaissant les entités qui sont mentionnées dans l'entrée utilisateur, le service {{site.data.keyword.conversationshort}} peut choisir les actions spécifiques à exécuter pour satisfaire une intention.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/kAZ9m-oCKxM" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Nombre limite d'entités
{: #entity-limits}

Le nombre d'entités, de valeurs d'entité et de synonymes que vous pouvez créez dépend de votre plan de service {{site.data.keyword.conversationshort}} :

| Plan de service      | Nombre d'entités par espace de travail | Nombre de valeurs d'entité par espace de travail | Nombre de synonymes d'entité par espace de travail |
|-------------------|-----------------------:|----------------------------:|--------------------------------:|
| Standard/Premium |                          1000 |                            100 000 |                              100 000 |
| Lite              |                            25 |                            100 000 |                              100 000 |

Les entités de système que vous activez pour utilisation sont prises en compte dans le nombre total d'entités utilisées de votre plan.

## Création d'entités
{: #creating-entities}

Utilisez l'outil {{site.data.keyword.conversationshort}} pour créer des entités.

1.  Dans l'outil {{site.data.keyword.conversationshort}}, ouvrez votre espace de travail et cliquez sur l'onglet **Entities**. Si l'onglet **Entities** ne s'affiche pas, utilisez le menu ![Menu](images/Menu_16.png) pour ouvrir la page.

1.  Cliquez sur **Add entity**.

    Vous pouvez également cliquer sur **Use System Entities** pour effectuer une sélection dans une liste d'entités courantes, fournie par {{site.data.keyword.IBM_notm}}, qui s'applique à n'importe quel scénario d'utilisation. Pour plus d'informations, reportez-vous à la rubrique [Activation des entités de système](#enable_system_entities).

1.  Dans la zone **Entity name**, tapez un nom descriptif pour l'entité. 

    Le nom d'entité peut contenir des lettres (au format Unicode), des nombres, des traits de soulignement et des traits d'union. Par exemple :
    - `@location`
    - `@menu_item`
    - `@product`

    N'ajoutez pas le caractère `@` dans les noms d'entité lorsque vous les créez dans l'outil {{site.data.keyword.conversationshort}}. Les noms d'entité ne peuvent pas contenir d'espaces ou excéder 64 caractères. De plus, les noms d'entité ne peuvent pas débuter par la chaîne `sys-`, réservée aux entités de système.
    {: tip}

1.  Sélectionnez **Create entity**.

    ![Capture d'écran illustrant la création d'une entité](images/create_entity.png)

1.  Dans la zone **Value name**, tapez le texte d'une valeur possible pour l'entité et appuyez sur la touche `Entrée`. Une valeur d'entité peut être n'importe quelle chaîne de 64 caractères au maximum.

    > **Important :** n'ajoutez pas d'informations sensibles ou personnelles dans les noms ou les valeurs d'entité. En effet, les noms et les valeurs peuvent être exposés dans les URL d'une application.

1.  Pour la fonction **Fuzzy Matching**, cliquez sur le bouton afin de sélectionner l'option On ou Off ; par défaut, cette fonction est désactivée (Off). Cette fonction est disponible pour les langues mentionnées dans la rubrique [Langues prises en charge](lang-support.html).
 {: #fuzzy-matching}

    Vous pouvez activer la fonction Fuzzy Matching (correspondance floue) afin d'améliorer la capacité du service à reconnaître les termes dans les entrées utilisateur dont la syntaxe est similaire à l'entité, mais pas forcément identique. La fonction Fuzzy Matching comporte trois options : Stemming (réduction au radical), Misspelling (fautes de frappe) et Partial Matching (correspondance partielle) :
    - *Stemming* : avec cette option, la fonction reconnaît la forme réduite au radical de valeurs d'entité qui ont plusieurs formes grammaticales. Par exemple, la réduction au radical de 'bananas' serait 'banana', tandis que la réduction au radical de 'running' serait 'run'.
    - *Misspelling* : avec cette option, la fonction est capable de mapper une entrée utilisateur à l'entité correspondante en dépit de la présence de fautes d'orthographe ou de légères différences de syntaxe. Par exemple, si vous définissez *giraffe* comme synonyme d'une entité d'animal et que l'entrée utilisateur contient les termes *giraffes* ou *girafe*, la fonction Fuzzy Matching est capable de mapper correctement le terme à l'entité d'animal. 
    - *Partial Matching* : avec cette option, la fonction suggère automatiquement des synonymes de type sous-chaîne présents dans les entités définies par l'utilisateur et affecte une cote de confiance inférieure par rapport à la correspondance d'entité exacte.

    **Remarque** : pour l'anglais, la fonction Fuzzy Matching empêche la capture de certains mots anglais valides courants comme correspondances floues pour une entité donnée. Ce dispositif utilise des mots de dictionnaire anglais standard.
Vous pouvez également définir une valeur/un synonyme d'entité en anglais, et la fonction Fuzzy Matching établira une correspondance uniquement avec la valeur/le synonyme d'entité que vous avez défini. Par exemple, la fonction Fuzzy Matching peut établir une correspondance entre le terme `unsure` et `insurance` ; mais, si `unsure` est défini en tant que valeur/synonyme pour une entité, telle que `@option`, le terme `unsure` sera toujours mis en correspondance avec `@option` et non avec `insurance`.

1.  Une fois que vous avez entré un nom de valeur, vous pouvez ajouter n'importe quel synonyme, ou définir des canevas spécifiques, pour cette valeur d'entité en sélectionnant `Synonyms` ou `Patterns` dans le menu déroulant *Type*. 

    ![Sélecteur de type pour une valeur](images/value_type.png)

    > **Remarque :** vous pouvez ajouter des synonymes *ou* des canevas pour une valeur d'entité, mais pas les deux.

    - Dans la zone **Synonyms**, tapez n'importe quel synonyme pour la valeur d'entité. Un synonyme peut être n'importe quelle chaîne de 64 caractères au maximum.

      ![Capture d'écran illustrant la définition d'une entité](images/define_entity.png)

    - La zone **Patterns** vous permet de définir des canevas spécifiques pour une valeur d'entité. Un canevas **doit** être entré sous la forme d'une expression régulière dans la zone.

      ![Capture d'écran illustrant la définition d'une entité de canvas](images/patternents1.png)
      {: #pattern-entities}

      Comme dans cet exemple, les canevas des valeurs phone, email et website peuvent être définis comme suit pour l'entité *ContactInfo* :
      - Phone
        - `localPhone` : `(\d{3})-(\d{4})`, par exemple, 426-4968
        - `fullUSphone` : `(\d{3})-(\d{3})-(\d{4})`, par exemple, 800-426-4968
        - `internationalPhone` : `^(\(?\+?[0-9]*\)?)?[0-9_\- \(\)]*$`, par exemple, +44 1962 815000
      - `email` : `\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}\b`, par exemple, name@ibm.com
      - `website` : `(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$`, par exemple, https://www.ibm.com

      Souvent, avec des entités de canevas, il est nécessaire de stocker le texte correspondant au canevas dans une variable contextuelle (ou une variable d'action) à partir d'une arborescence de dialogue.

      Imaginez que vous demandiez à un utilisateur son adresse électronique. La condition de noeud de dialogue contiendra une condition semblable à `@contactInfo:email`. Pour pouvoir affecter comme variable contextuelle l'adresse électronique saisie par l'utilisateur, vous pouvez utiliser la syntaxe suivante afin de capturer la correspondance de canevas dans la section de réponse du noeud de dialogue :

      ```json
      {
          "context" : {
              "email": "<? @contactInfo.literal ?>"
          }
      }
      ```
      {: screen}
      {: #capture-group}

      *Groupes de capture* - Pour les expressions régulières, toute partie d'un canevas au sein d'une paire de parenthèses normales sera capturée en tant que groupe. Par exemple, la valeur d'entité `fullUSphone` contient trois groupes capturés :

        - `(\d{3})` - Indicatif régional américain
        - `(\d{3})` - Préfixe
        - `(\d{4})` - Numéro de ligne

      Le regroupement peut être utile si, par exemple, vous souhaitez que le service {{site.data.keyword.conversationshort}} demande aux utilisateurs leur numéro de téléphone, puis utilise uniquement l'indicatif régional du numéro fourni dans une réponse. 

      Pour pouvoir affecter comme variable contextuelle l'indicatif régional saisi par l'utilisateur, vous pouvez utiliser la syntaxe suivante afin de capturer la correspondance de groupe dans la section de réponse du noeud de dialogue : 

        ```json
        {
            "context" : {
                "area_code": "<? @fullUSphone.groups[1] ?>"
            }
        }
        ```
       {: screen}

      Pour plus d'informations sur l'utilisation de groupes de capture dans le contexte d'exécution de dialogue, reportez-vous à la rubrique [Stockage de valeurs d'entité de canevas dans des variables contextuelles](dialog-overview-context-groups.html).

      Le moteur de correspondance de canevas utilisé par le service {{site.data.keyword.conversationshort}} présente des limitations en matière de syntaxe, nécessaires pour éviter tout problème de performance susceptible de se produire lors de l'utilisation d'autres moteurs d'expression régulière.
        - Les canevas d'entité ne peuvent pas contenir :
          - Des répétitions positives (par exemple, `x*+`)
          - Des références arrières (par exemple, `\g1`)
          - Des branches conditionnelles (par exemple, `(?(cond)true)`)
        - Lorsqu'une entité de canevas commence ou se termine par un caractère Unicode et inclut des limites de mot, par exemple, `\bš\b`, la correspondance de canevas ne correspond pas exactement à la limite de mot. Dans cet exemple, pour l'entrée `š zkouška`, la correspondance renvoie `Group 0: 6-7 š` (`š zkou`_**`š`**_`ka`) au lieu de renvoyer la chaîne appropriée `Group 0: 0-1 š` (_**`š`**_ `zkouška`). 

      Le moteur d'expression régulière est librement inspiré du moteur d'expression régulière Java. Le service {{site.data.keyword.conversationshort}} produira une erreur si vous essayez de télécharger un canevas non pris en charge, que ce soit via l'API ou à partir de l'interface utilisateur d'outils du service {{site.data.keyword.conversationshort}}.

1.  Cliquez sur **Add value** et répétez le processus afin d'ajouter d'autres valeurs d'entité. 

1.  Lorsque vous avez terminé d'ajouter des valeurs d'entité, sélectionnez l'![icône Fermer (représentée par une flèche)](images/close_arrow.png) pour terminer la création de l'entité. 

### Résultats

L'entité que vous avez créée est ajoutée à l'onglet **Entities** et le système commence à s'entraîner lui-même en utilisant les nouvelles données.

## Edition d'entités

Vous pouvez cliquer sur n'importe quelle entité de la liste pour l'ouvrir et l'éditer. Vous pouvez renommer ou supprimer des entités, et vous pouvez ajouter, éditer ou supprimer des valeurs, des synonymes ou des canevas.

> **Remarque** : si vous modifiez le type d'entité `synonym` et que vous le remplacez par `pattern`, ou inversement, les valeurs existantes sont converties, mais elles peuvent ne pas être utiles en l'état. 

## Recherche d'entités

Utilisez la fonction de recherche pour trouver des noms, des valeurs et des synonymes d'entité. 

1.  Sélectionnez l'onglet **Entities** dans la barre de navigation, puis *My Entities*.

    ![Présentation de l'onglet Entities](images/entity_oview.png)

    **Remarque** : les entités de système ne peuvent pas faire l'objet d'une recherche. 

1.  Sélectionnez l'icône de recherche : ![Icône de recherche](images/search_icon.png)

1.  Entrez un terme ou une phrase de recherche. 

    ![Terme de recherche pour une entité](images/searchent_1.png)

    **Remarque** : lors de votre première recherche, un index est créé ; il se peut qu'un message s'affiche pour vous inviter à patienter pendant l'indexation de votre contenu. 

### Résultats

Les entités contenant votre terme de recherche, avec des exemples correspondants, s'affichent. Sélectionnez n'importe quel résultat pour l'ouvrir et l'éditer. 

  ![Résultats renvoyés par la fonction de recherche d'entité](images/searchent_2.png)

## Importation d'entités

Si vous possédez un grand nombre d'entités, vous trouverez peut-être plus facile de les importer à partir d'un fichier CSV que de les définir une par une dans l'outil {{site.data.keyword.conversationshort}}.

1.  Collectez les entités dans un fichier CSV ou exportez-les à partir d'une feuille de calcul dans un fichier CSV. Le format requis pour chaque ligne du fichier est le suivant :

    ```
    <entity>,<value>,<synonyms>
    ```
    {: screen}

    où &lt;entity&gt; est le nom d'une entité, &lt;value&gt; est une valeur de l'entité et &lt;synonyms&gt; est une liste de synonymes séparés par des virgules pour cette valeur.

    ```
    weekday,Monday,Mon
    weekday,Tuesday,Tue,Tues
    weekday,Wednesday,Wed
    weekday,Thursday,Thur,Thu,Thurs
    weekday,Friday,Fri
    weekday,Saturday,Sat
    weekday,Sunday,Sun
    month,January,Jan
    month,February,Feb
    month,March,Mar
    month,April,Apr
    month,May
    ```
    {: screen}

    Les canevas sont également pris en charge pour l'importation d'un fichier CSV. Toute chaîne encapsulée avec `/` sera considérée comme un canevas (par opposition à un synonyme). 

    ```
    ContactInfo,localPhone,/(\d{3})-(\d{4})/
    ContactInfo,fullUSphone,/(\d{3})-(\d{3})-(\d{4})/
    ContactInfo,internationalPhone,/^(\(?\+?[0-9]*\)?)?[0-9_\- \(\)]*$/
    ContactInfo,email,/\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}\b/
    ContactInfo,website,/(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/
    ```
    {: screen}

    Sauvegardez le fichier CSV au format UTF-8 et sans marque d'ordre d'octet. Le fichier CSV ne doit pas excéder 10 Mo. Si votre fichier CSV est plus gros, songez à le fractionner en plusieurs fichiers et à les importer séparément.  Dans l'outil {{site.data.keyword.conversationshort}}, ouvrez votre espace de travail, puis cliquez sur l'onglet **Entities**.
    {: tip}

1.  Cliquez sur ![Import](images/importGA.png), puis faites glisser un fichier, ou recherchez un fichier sur votre ordinateur. Le fichier est validé et importé et le système commence à s'entraîner lui-même en utilisant les nouvelles données.

### Résultats

Vous pouvez visualiser les entités importées sur l'onglet Entities. Vous devrez peut-être actualiser la page pour voir les nouvelles entités.

## Exportation d'entités
{: #export_entities}

Vous pouvez exporter un certain nombre d'entités vers un fichier CSV, de manière à pouvoir les importer et les réutiliser pour une autre application {{site.data.keyword.conversationshort}}. 

Les canevas sont pris en charge pour l'exportation d'un fichier CSV. Toute chaîne encapsulée avec `/` sera considérée comme un canevas (par opposition à un synonyme). 
{: tip}

1.  Sélectionnez les entités souhaitées, puis choisissez **Export**. 

    ![Bouton Export pour une entité](images/ExportEntity.png)

## Suppression d'entités
{: #delete_entities}

Vous pouvez sélectionner un certain nombre d'entités à supprimer.

**Important** : lorsque vous supprimez des entités, vous supprimez également toutes les valeurs, tous les synonymes ou tous les canevas qui leur sont associés, et ces éléments ne peuvent plus être extraits ultérieurement. Tous les noeuds de dialogue qui font référence à ces entités ou ces valeurs doivent être mis à jour manuellement de manière à ne plus faire référence au contenu supprimé.

1.  Sélectionnez les entités souhaitées, puis choisissez **Delete**. 

    ![Bouton Delete pour une entité](images/DeleteEntity.png)

## Activation des entités de système
{: #enable_system_entities}

Le service {{site.data.keyword.conversationshort}} fournit un certain nombre d'*entités de système* ; il s'agit d'entités communes que vous pouvez utiliser pour n'importe quelle application. Le fait d'activer une entité de système permet de rendre possible le remplissage rapide de votre espace de travail avec des données d'apprentissage communes à un grand nombre de scénarios d'utilisation.

Les entités de système peuvent être utilisées pour reconnaître un large éventail de valeurs pour les types d'objet qu'elles représentent. Par exemple, l'entité de système `@sys-number` correspond à n'importe quelle valeur numérique, y compris des nombres entiers, des fractions décimales ou même des nombres écrits sous forme de mots.

Les entités de système sont gérées de manière centralisée, par conséquent, toutes les mises à jour sont disponibles automatiquement. Vous ne pouvez pas modifier les entités de système.

1.  Sur l'onglet Entities, cliquez sur **System entities**.

    ![Capture d'écran de l'onglet "System entities"](images/system_entities_1.png)

1.  Parcourez la liste des entités de système afin de choisir celles qui sont utiles pour votre application.
    - Pour afficher davantage d'informations sur une entité de système, y compris des exemples d'entrée correspondante, cliquez sur l'entité dans la liste.
    - Pour obtenir des informations complètes sur les entités de système disponibles, reportez-vous à la rubrique [Entités de système](system-entities.html).

1.  Cliquez sur le bouton bascule en regard d'une entité de système pour l'activer ou la désactiver.

### Résultats

Une fois que vous avez activé les entités de système, le service {{site.data.keyword.conversationshort}} recommence à s'entraîner. Une fois l'entraînement terminé, vous pouvez utiliser les entités.
