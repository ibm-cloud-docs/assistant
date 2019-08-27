---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-06"

keywords: entity, entity value, contextual entity, dictionary entity, pattern entity, entity synonym, annotate mentions

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

# Création d'entités
{: #entities}

Les ***entités*** représentent les informations de l'entrée utilisateur qui sont pertinentes pour l'objectif de l'utilisateur.

Si des intentions représentent des verbes (une action que l'utilisateur souhaite effectuer), les entités représentent des noms (l'objet ou le contexte de cette action). Par exemple, lorsque l'*intention* est d’obtenir une prévision météorologique, les *entités* d’emplacement et de date pertinentes sont requises pour que l’application puisse renvoyer une prévision exacte.

La reconnaissance des entités dans l'entrée utilisateur vous aide à élaborer des réponses plus utiles et plus ciblées. Soit, par exemple, l'intention `#buy_something`. Lorsqu'un utilisateur exprime une demande qui déclenche l'intention `#buy_something`, la réponse de l'assistant doit refléter une compréhension de ce qu'est le produit (*something*) que le client souhaite acheter. Vous pouvez ajouter une entité `@produit`, puis l'utiliser pour extraire des informations de l'entrée utilisateur concernant le produit qui intéresse le client. (Le signe `@` précédant le nom de l'entité aide à l'identifier clairement en tant qu'entité.)

Enfin, vous pouvez ajouter plusieurs réponses à votre arborescence de dialogue avec une formulation différente en fonction de la valeur de `@produit` détectée dans la demande de l'utilisateur.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Utilisation des entités" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/o-uhdw6bIyI" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Présentation de l'évaluation des entités
{: #entities-described}

L'assistant détecte les entités dans l'entrée utilisateur à l'aide de l'une des méthodes d'évaluation suivantes :  

### Méthode basée sur un dictionnaire
{: #entities-dictionary-overview}

L'assistant recherche dans l'entrée utilisateur les termes qui correspondent aux valeurs, aux synonymes ou aux canevas que vous définissez pour l'entité.

- **Entité de synonyme** : vous définissez une catégorie de termes en tant qu'entité (`couleur`), puis une ou plusieurs valeurs dans cette catégorie (`bleu`). Pour chaque valeur, vous spécifiez un groupe de synonymes (`turquoise`, `marine`). Vous pouvez également choisir des synonymes à ajouter à partir des recommandations que Watson vous a faites.

    Au moment de l'exécution, l'assistant reconnaît dans l'entrée utilisateur les termes qui correspondent exactement aux valeurs ou aux synonymes que vous avez définis pour l'entité en tant que mentions de cette entité.
- **Entité de canevas** : vous définissez une catégorie de termes en tant qu'entité (`info_contact`), puis une ou plusieurs valeurs dans cette catégorie (`email`). Pour chaque valeur, vous spécifiez une expression régulière qui définit le canevas textuel de mentions de ce type de valeur. Pour une valeur d'entité `email`, vous pouvez spécifier une expression régulière qui définit un canevas `texte@texte.com`.

     Au moment de l'exécution, l'assistant recherche les modèles correspondant à votre expression régulière dans l'entrée utilisateur et identifie les correspondances en tant que mentions de cette entité.
- **Entité système** : entités synonymes préconstruites pour vous par IBM. Elles couvrent les catégories couramment utilisées, telles que les nombres, les dates et les heures. Vous permettez simplement à une entité système de commencer à l'utiliser.

### Méthode basée sur les annotations
{: #entities-annotations-overview}

Lorsque vous définissez une entité basée sur des annotations, également appelée entité contextuelle, un modèle est formé à la fois sur le *terme annoté* et le *contexte* dans lequel le terme est utilisé dans la phrase que vous annotez. Ce nouveau modèle d’entité contextuelle permet à l'assistant de calculer une cote de confiance identifiant la probabilité qu’un mot ou une expression soit une instance d’une entité, en fonction de la façon dont il est utilisé dans l'entrée de l’utilisateur. 

- **Entité contextuelle** : vous définissez tout d'abord une catégorie de termes en tant qu'entité (`produit`). Ensuite, vous accédez à la page *Intents* et parcourez vos exemples d’utilisateurs d'intentions existants pour rechercher les mentions de l’entité et les étiqueter en tant que telles. Par exemple, vous pouvez aller à l’intention `#buy_something` et rechercher un exemple d’utilisateur indiquant `Je souhaite acheter un sac Coach`. Vous pouvez indiquer `sac Coach` comme mention de l'entité `@produit`

    Pour les besoins de la formation, le terme annoté, `sac Coach`, est ajouté en tant que valeur de l’entité `@produit`.

    Au moment de l'exécution, l'assistant évalue les termes en fonction du contexte dans lequel ils sont utilisés dans la phrase uniquement. Si la structure d'une demande d'utilisateur qui mentionne le terme correspond à la structure d'un exemple d'utilisateur d'intention dans lequel une mention est étiquetée, l'assistant interprète le terme comme une mention de ce type d'entité. Par exemple, l'entrée utilisateur peut inclure l'énoncé suivant : `Je souhaite acheter un sac Gucci`. En raison de la similitude de la structure de cette phrase avec l'exemple utilisateur que vous avez annoté (`Je souhaite acheter un sac Coach`), l'assistant reconnaît `sac Gucci` en tant que mention de l'entité `@produit`.

    Lorsqu'un modèle d'entité contextuelle est utilisé pour une entité, l'assistant *ne recherche pas* les correspondances de texte ou de canevas exactes pour l'entité dans l'entrée utilisateur, mais se concentre plutôt sur le contexte de la phrase où l'entité est mentionnée. 

    Si vous choisissez de définir des valeurs d'entité à l'aide d'annotations, ajoutez au moins 10 annotations par entité pour donner au modèle d'entité contextuelle suffisamment de données pour être fiable.

Pour en savoir plus sur les entités contextuelles, [lisez cet article ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://medium.com/ibm-watson/contextual-entities-with-ibm-watson-assistant-f41b2e0ca82e).

## Création d'entités
{: #entities-creating-task}

1.  Ouvrez votre compétence de dialogue et cliquez sur l'onglet **Entities**. Si l'onglet **Entities** ne s'affiche pas, utilisez le menu ![Menu](images/Menu_16.png) pour ouvrir la page.

1.  Cliquez sur **Create entity**.

    Vous pouvez également cliquer sur **System entities** pour effectuer une sélection dans une liste d'entités courantes, fournie par {{site.data.keyword.IBM_notm}}, qui s'applique à n'importe quel scénario d'utilisation. Pour plus d'informations, reportez-vous à la rubrique [Activation des entités de système](#entities-enable-system-entities).

1.  Dans la zone **Entity name**, tapez un nom descriptif pour l'entité.

    Le nom d'entité peut contenir des lettres (au format Unicode), des nombres, des traits de soulignement et des traits d'union. Par exemple :
    - `@location`
    - `@menu_item`
    - `@product`

    Le nom ne doit pas comporter d'espace. Le nom ne peut pas comporter plus de 64 caractères. Ne commencez pas le nom par la chaîne `sys-` car elle est réservée aux entités système.

    Le signe `@` est ajouté avant le nom de l'entité afin d'identifier automatiquement le terme en tant qu'entité. Il n'est pas nécessaire de l'ajouter.
    {: tip}

1.  Cliquez sur **Create entity**.

    ![Capture d'écran illustrant la création d'une entité](images/create_entity.png)

1.  Pour cette entité, indiquez si vous souhaitez que l'assistant utilise une approche basée sur un dictionnaire ou sur des annotations pour en trouver les mentions, puis suivez la procédure appropriée. 

    **Pour chaque entité que vous créez, choisissez uniquement un type d'entité à utiliser.** Dès que vous ajoutez une annotation à une entité, le modèle contextuel est initialisé et devient l'approche principale pour analyser les entrées utilisateur afin de trouver des mentions de cette entité. Le contexte dans lequel la mention est utilisée dans l'entrée utilisateur a priorité sur les correspondances exactes éventuellement présentes. Pour plus d'informations sur la façon dont chaque type est évalué, reportez-vous à la rubrique [Présentation de l'évaluation d'entité](#entities-described).

    - [Entités basées sur un dictionnaire](#entities-create-dictionary-based)
    - [Entités basées sur des annotations](#entities-create-annotation-based)

## Ajout d'entités basées sur un dictionnaire
{: #entities-create-dictionary-based}

Les entités basées sur un dictionnaire sont celles pour lesquelles vous définissez des termes, des synonymes ou des modèles spécifiques. Au moment de l'exécution, l'assistant trouve les mentions d'entité uniquement lorsqu'un terme de l'entrée utilisateur correspond exactement (ou étroitement si la correspondance approximative est activée) à la valeur ou à l'un de ses synonymes. 

1.  Dans la zone **Value name**, tapez le texte d'une valeur possible pour l'entité et appuyez sur la touche `Entrée`. Une valeur d'entité peut être n'importe quelle chaîne de 64 caractères au maximum.

    **Important :** n'ajoutez pas d'informations sensibles ou personnelles dans les noms ou les valeurs d'entité. En effet, les noms et les valeurs peuvent être exposés dans les URL d'une application.

1.  Si vous souhaitez que l'assistant reconnaisse les termes avec une syntaxe similaire à la valeur d'entité et aux synonymes que vous spécifiez, mais sans exiger de correspondance exacte, cliquez sur l'option à bascule **Fuzzy Matching** pour l'activer.

    Cette fonction est disponible pour les langues mentionnées dans la rubrique [Langues prises en charge](/docs/services/assistant?topic=assistant-language-support).

    **Fuzzy matching**
    {: #entities-fuzzy-matching}

    La fonction Fuzzy Matching comporte les composants suivants :

    - *Stemming* : avec cette option, la fonction reconnaît la forme réduite au radical de valeurs d'entité qui ont plusieurs formes grammaticales. Par exemple, la réduction au radical de 'bananes' serait 'banane', tandis que la réduction au radical de 'running' serait 'run'.
    - *Misspelling* : avec cette option, la fonction est capable de mapper une entrée utilisateur à l'entité correspondante en dépit de la présence de fautes d'orthographe ou de légères différences de syntaxe. Par exemple, si vous définissez *giraffe* comme synonyme d'une entité d'animal et que l'entrée utilisateur contient les termes *giraffes* ou *girafe*, la fonction Fuzzy Matching est capable de mapper correctement le terme à l'entité d'animal.
    - *Partial Matching* : avec cette option, la fonction suggère automatiquement des synonymes de type sous-chaîne présents dans les entités définies par l'utilisateur et affecte une cote de confiance inférieure par rapport à la correspondance d'entité exacte.

    Pour l'anglais, la fonction Fuzzy Matching empêche la capture de certains mots anglais valides courants comme correspondances floues pour une entité donnée. Ce dispositif utilise des mots de dictionnaire anglais standard. Vous pouvez également définir une valeur/un synonyme d'entité en anglais, et la fonction Fuzzy Matching établira une correspondance uniquement avec la valeur/le synonyme d'entité que vous avez défini. Par exemple, la fonction Fuzzy Matching peut établir une correspondance entre le terme `unsure` et `insurance` ; mais, si `unsure` est défini en tant que valeur/synonyme pour une entité, telle que `@option`, le terme `unsure` sera toujours mis en correspondance avec `@option` et non avec `insurance`.
    {: note}

    Votre paramètre Fuzzy Matching n'a aucun impact sur les recommandations de synonymes. Même si Fuzzy Matching est activée, des synonymes sont suggérés pour la valeur exacte que vous spécifiez uniquement, pas la valeur et les légères variantes de la valeur.

1.  Une fois que vous avez entré un nom de valeur, vous pouvez ajouter n'importe quel synonyme, ou définir des canevas spécifiques, pour cette valeur d'entité en sélectionnant `Synonyms` ou `Patterns` dans le menu déroulant *Type*.

    ![Sélecteur de type pour une valeur](images/value_type.png)

    **Remarque :** vous pouvez ajouter des synonymes *ou* des canevas pour une valeur d'entité, mais pas les deux.

    ***Synonymes***
    {: #entities-synonyms}

    - Dans la zone **Synonyms**, tapez n'importe quel synonyme pour la valeur d'entité. Un synonyme peut être n'importe quelle chaîne de 64 caractères au maximum.

      ![Capture d'écran illustrant la définition d'une entité](images/define_entity.png)

      Le service {{site.data.keyword.conversationshort}} peut également recommander des synonymes pour vos valeurs d'entité. L'outil de recommandation recherche les synonymes associés en fonction de la similarité contextuelle extraite d'un vaste ensemble d'informations existantes, y compris de grandes sources de texte écrit, et utilise des techniques de traitement de langage naturel pour identifier des mots similaires aux synonymes existants dans votre valeur d'entité.

    - Cliquez sur **Show recommendations**.

    - Le service {{site.data.keyword.conversationshort}} formulera plusieurs recommandations pour les synonymes. Les termes sont affichés en minuscules, mais l'assistant reconnaît les mentions des synonymes, qu’elles soient spécifiées en minuscules ou en majuscules. 

      Plus vos synonymes de valeur d'entité sont cohérents, plus vos recommandations seront pertinentes et ciblées. Par exemple, plusieurs mots sont centrés sur un thème, vous obtiendrez de meilleures suggestions que si vous aviez un ou deux mots aléatoires.
      {: tip}

      ![Ecran de recommandation sur les synonymes 2](images/synonym_2.png)

    - Sélectionnez les synonymes à inclure, puis cliquez sur **Add selected**.

      Vous devez cliquer sur le bouton **Add selected** pour que les synonymes que vous avez sélectionnés soient ajoutés. Si vous passez au groupe suivant sans cliquer d'abord sur ce bouton, vos sélections sont perdues.

      ![Ecran de recommandation sur les synonymes 3](images/synonym_3.png)

    - Le service {{site.data.keyword.conversationshort}} ajoute ces synonymes à votre entité et propose des synonymes supplémentaires.

      Si vous ne recevez aucune recommandation de synonyme supplémentaire, il est possible que votre entité soit déjà bien définie ou qu'elle comporte un contenu que l'agent de recommandation n'est pas en mesure de développer.
      {: tip}

      Si vous choisissez de ne pas sélectionner un synonyme recommandé, le système traite ce terme comme un terme qui ne vous intéresse pas et modifie le prochain ensemble de recommandations que vous verrez lorsque vous appuierez sur `Add selected` ou `Next set`. Cette inférence ne persiste que lorsque vous choisissez des synonymes. Les informations sur les synonymes ignorés ne sont utilisées à aucune autre fin par l'assistant.
      {: note}

      ![Ecran de recommandation sur les synonymes 4](images/synonym_4.png)

      Continuez à ajouter des synonymes comme vous le souhaitez. Lorsque vous avez terminé d'accepter les recommandations, cliquez sur le **X** pour fermer la fenêtre.

    ***Canevas***
    {: #entities-patterns}

    - La zone **Patterns** vous permet de définir des canevas spécifiques pour une valeur d'entité. Un canevas **doit** être entré sous la forme d'une expression régulière dans la zone.

      - Chaque valeur d'entité peut être associée à 5 canevas au maximum.
      - Chaque canevas (expression régulière) est limité à 512 caractères.

      ![Capture d'écran illustrant la définition d'une entité de canevas](images/patternents1.png)
      {: #entities-pattern-entities}

      Comme dans cet exemple, les canevas des valeurs phone, email et website peuvent être définis comme suit pour l'entité *ContactInfo* :
      - Phone
        - `localPhone` : `(\d{3})-(\d{4})`, par exemple, 426-4968
        - `fullUSphone` : `(\d{3})-(\d{3})-(\d{4})`, par exemple, 800-426-4968
        - `internationalPhone` : `^(\(?\+?[0-9]*\)?)?[0-9_\- \(\)]*$`, par exemple, +44 1962 815000
      - `email` : `\b[A-Za-z0-9._%+-]+@([A-Za-z0-9-]+\.)+[A-Za-z]{2,}\b`, par exemple, name@ibm.com
      - `website` : `(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$`, par exemple, https://www.ibm.com

      Souvent, avec des entités de canevas, il est nécessaire de stocker le texte correspondant au canevas dans une variable contextuelle (ou une variable d'action) à partir d'une arborescence de dialogue. Pour plus d'informations, reportez-vous à la rubrique [Définition d'une variable contextuelle](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-var-define).

      Imaginez que vous demandiez à un utilisateur son adresse électronique. La condition de noeud de dialogue contiendra une condition semblable à `@contactInfo:email`. Pour pouvoir affecter comme variable contextuelle l'adresse électronique saisie par l'utilisateur, vous pouvez utiliser la syntaxe suivante afin de capturer la correspondance de canevas dans la section de réponse du noeud de dialogue :

      <table>
      <caption>Sauvegarde d'un canevas</caption>
        <tr>
          <th>Variable</th>
          <th>Valeur</th>
        </tr>
        <tr>
          <td>email</td>
          <td>`<? @contactInfo.literal ?>`</td>
        </tr>
      </table>

      ***Groupes de capture***
      {: #entities-capture-group}

      Pour les expressions régulières, toute partie d'un canevas au sein d'une paire de parenthèses normales sera capturée en tant que groupe. Par exemple, l'entité `@ContactInfo` comporte une valeur de canevas nommée `fullUSphone` qui contient trois groupes de capture :

      - `(\d{3})` - Indicatif régional américain
      - `(\d{3})` - Préfixe
      - `(\d{4})` - Numéro de ligne

      Le regroupement peut être utile si, par exemple, vous souhaitez que le service {{site.data.keyword.conversationshort}} demande aux utilisateurs leur numéro de téléphone, puis utilise uniquement l'indicatif régional du numéro fourni dans une réponse.

      Pour pouvoir affecter comme variable contextuelle l'indicatif régional saisi par l'utilisateur, vous pouvez utiliser la syntaxe suivante afin de capturer la correspondance de groupe dans la section de réponse du noeud de dialogue :

      <table>
      <caption>Sauvegarde d'un groupe de capture</caption>
        <tr>
          <th>Variable</th>
          <th>Valeur</th>
        </tr>
        <tr>
          <td>area_code</td>
          <td>`<? @ContactInfo.groups[1] ?>`</td>
        </tr>
      </table>

      Pour plus d'informations sur l'utilisation des groupes de capture dans votre dialogue, reportez-vous à la rubrique [Stockage et reconnaissance des groupes de canevas d'entités dans l'entrée](/docs/services/assistant?topic=assistant-dialog-tips#dialog-tips-get-pattern-groups).

      Le moteur de correspondance de canevas utilisé par le service {{site.data.keyword.conversationshort}} présente des limitations en matière de syntaxe, nécessaires pour éviter tout problème de performance susceptible de se produire lors de l'utilisation d'autres moteurs d'expression régulière.

      - Les canevas d'entité ne peuvent pas contenir :
        - Des répétitions positives (par exemple, `x*+`)
        - Des références arrières (par exemple, `\g1`)
        - Des branches conditionnelles (par exemple, `(?(cond)true)`)
      - Lorsqu'une entité de canevas commence ou se termine par un caractère Unicode et inclut des limites de mot, par exemple, `\bš\b`, la correspondance de canevas ne correspond pas exactement à la limite de mot. Dans cet exemple, pour l'entrée `š zkouška`, la correspondance renvoie `Group 0: 6-7 š` (`š zkou`_**`š`**_`ka`) au lieu de renvoyer la chaîne appropriée `Group 0: 0-1 š` (_**`š`**_ `zkouška`).

      Le moteur d'expression régulière est librement inspiré du moteur d'expression régulière Java. Le service {{site.data.keyword.conversationshort}} produira une erreur si vous essayez de télécharger un canevas non pris en charge, que ce soit via l'API ou à partir de l'interface utilisateur de {{site.data.keyword.conversationshort}}.

1.  Cliquez sur **Add value** et répétez le processus afin d'ajouter d'autres valeurs d'entité.

1.  Lorsque vous avez terminé d'ajouter des valeurs d'entité, cliquez sur l'![icône Fermer (représentée par une flèche)](images/close_arrow.png) pour terminer la création de l'entité.

L'entité que vous avez créée est ajoutée à l'onglet **Entities** et le système commence à s'entraîner lui-même en utilisant les nouvelles données.

## Ajout d'entités contextuelles
{: #entities-create-annotation-based}

Les entités basées sur les annotations sont celles pour lesquelles vous annotez les occurrences de l'entité dans des exemples de phrases afin d'enseigner à l'assistant le contexte dans lequel l'entité est généralement utilisée. 

Pour former un modèle d'entité contextuelle, vous pouvez tirer parti de vos exemples d'intention, qui fournissent des phrases facilement disponibles à annoter.

L'utilisation d'exemples d'utilisateur d'intention pour définir des entités contextuelles n'a aucune incidence sur la classification de cette intention. Cependant, les mentions d'entité que vous étiquetez sont également ajoutées à cette entité en tant que synonymes. De plus, la classification d'intention utilise des mentions de synonyme dans les exemples d'utilisateur d'intention pour établir une référence faible entre une intention et une entité.
{: note}

1.  A partir de votre compétence de dialogue, cliquez sur l'onglet **Intents**.

1.  Cliquez sur une intention pour l'ouvrir.

    Pour cet exemple, l'intention `#place_order` définit la fonction de commande d'un détaillant en ligne.

    ![Sélectionnez l'intention #place_order](images/oe-intent.png)

1.  Consultez les exemples d'intention pour les mentions d'entités potentielles. Mettez en surbrillance une mention d’entité potentielle parmi les exemples d’intention.

    Dans cet exemple, `ordinateur` est la mention de l'entité.

    ![Exemples d'intentions de révision](images/oe-intent-review.png)

    L'icône ![Edit](images/oe-intent-edit.png) permet de modifier un exemple d'utilisateur d'intention. Elle n'est pas liée à l'ajout d'annotations.
    {: tip}

1.  Une boîte de recherche s'ouvre pour vous permettre de rechercher l'entité dont le mot ou l'expression en surbrillance est une mention.

    ![Etat initial de la zone Search](images/oe-intent-search1.png)

    Dans cet exemple, la recherche sur `prod` renvoie des correspondances pour l'entité`@produit`.

    ![Zone de recherche avec paramètre de recherche prod](images/oe-intent-search2.png)

    Si l'entité a des valeurs d'entité existantes, elles sont affichées à titre informatif uniquement. Vous ajoutez l'annotation à l'entité et non à une valeur d'entité spécifique.

    Si vous souhaitez enseigner au modèle que la mention est synonyme d'une valeur d'entité existante, vous pouvez l'associer à une valeur d'entité spécifique.
    {: important}

    Pour associer la mention à une valeur d'entité spécifique, procédez comme suit :

    1.  Saisissez le nom complet de l’entité et sa valeur dans la zone de recherche. Par exemple, entrez `@produit:IT`.
    1.  Lorsque la valeur de l'entité est affichée dans le menu déroulant, sélectionnez-la.

1.  Sélectionnez l'entité à laquelle vous souhaitez ajouter l'annotation.

    Dans cet exemple, `ordinateur` est ajouté en tant qu'annotation pour l'entité `@produit`.

    Créez *au moins* 10 annotations pour chaque entité contextuelle ; plusieurs annotations sont recommandées pour une utilisation en production.
    {: important}

1.  Si aucune des entités n'est appropriée, vous pouvez créer une nouvelle entité en choisissant **@(créer une entité)**.

1.  Répétez cette procédure pour chaque entité que vous souhaitez annoter.

    Veillez à annoter chaque mention d’un type d’entité apparaissant dans les exemples d’utilisateur que vous éditez. Pour plus d'informations, reportez-vous à la rubrique [Ce que vous n'annotez pas compte](#entities-counter-examples).
    {: important}

1.  Cliquez à présent sur l'annotation que vous venez de créer. Une fenêtre s'ouvre et indique `Go to: <entity-name>`. Cliquez sur ce lien pour accéder directement à l'entité.

    ![Vérification de la valeur ordinateur pour l'entité de produit](images/oe-verify-value.png)

    L'annotation est ajoutée à l'entité à laquelle vous l'avez associée et le système commence à se former sur les nouvelles données.

    Le terme que vous avez annoté est ajouté à l'entité en tant que nouvelle valeur de dictionnaire. Si vous avez associé le terme annoté à une valeur d'entité existante, le terme est ajouté en tant que synonyme de cette valeur d'entité et non en tant que valeur d'entité indépendante.
    {: important}

1.  Pour afficher toutes les mentions annotées pour une entité particulière, dans la page de configuration de l'entité, cliquez sur l'onglet **Annotation**.

    ![Sélecteur de vue d'annotation en surbrillance](images/oe-annotate2.png)

    Les entités contextuelles comprennent les valeurs que vous n'avez pas explicitement définies. Le système établit des prédictions sur les valeurs d'entité supplémentaires en fonction de la façon dont vos exemples d'utilisateurs sont annotés, et utilise ces valeurs pour former d'autres entités. Tous les exemples d'utilisateurs similaires sont ajoutés à la vue *Annotation*, afin que vous puissiez voir comment cette option affecte la formation.
    {: note}

    Si vous ne souhaitez pas que vos entités contextuelles utilisent cette compréhension élargie des valeurs d'entité, sélectionnez tous les exemples d'utilisateur dans la vue *Annotation* pour cette entité, puis cliquez sur **Delete**.

La vidéo suivante montre comment annoter des mentions d'entités.

<iframe class="embed-responsive-item" id="youtubeplayer0" title="Annotation de mentions d'entité" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/3WjzJpLsnhQ" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Pour parcourir un tutoriel vous expliquant comment définir des entités contextuelles avant d'ajouter la vôtre, accédez à [Tutoriel : Définition d'entités contextuelles ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/garage/demo/try-watson-assistant-contextual-entities){: new_window}.

### Ce que vous n'annotez pas compte
{: #entities-counter-examples}

Si vous avez un exemple d'intention avec une annotation et qu'un autre mot de cet exemple correspond à la valeur ou à un synonyme de la même entité, mais que la valeur *n'est pas* annotée, cette omission a un impact. Le modèle tire également des leçons du contexte du terme que vous n'avez pas annoté. Par conséquent, si vous identifiez un terme en tant que mention d'une entité dans un exemple d'utilisateur, veillez également à étiqueter toutes les autres mentions applicables.

1.  L'intention `#Customer_Care_Appointments` comprend deux exemples d'intention avec le mot `visite`.

    ![Exemples d'Intention de visite](images/oe-counter-1.png)

1.  Dans le premier exemple, vous souhaitez annoter le mot `visite` en tant que valeur de l'entité `@réunion`. Cela rend `visite` équivalent à d’autres valeurs d’entité `@réunion` telles que `rendez-vous`, comme dans "J'aimerais prendre rendez-vous" ou "J'aimerais planifier une visite".

    ![Entité @réunion](images/oe-counter-2.png)

1.  Pour le deuxième exemple, le mot `visite` est utilisé dans un contexte différent de celui d'une réunion. Dans ce cas, vous pouvez sélectionner le mot `rendez-vous` dans l'exemple d'intention et l'annoter en tant que valeur de l'entité `@réunion`. Le modèle tire des leçons du fait que le mot `visite` dans le même exemple n'est pas annoté.

    ![Visite non sélectionnée](images/oe-counter-3.png)

## Activation des entités de système
{: #entities-enable-system-entities}

Le service {{site.data.keyword.conversationshort}} fournit un certain nombre d'*entités de système* ; il s'agit d'entités communes que vous pouvez utiliser pour n'importe quelle application. Le fait d'activer une entité de système permet de rendre possible le remplissage rapide de votre compétence avec des données d'apprentissage communes à un grand nombre de scénarios d'utilisation.

Les entités de système peuvent être utilisées pour reconnaître un large éventail de valeurs pour les types d'objet qu'elles représentent. Par exemple, l'entité de système `@sys-number` correspond à n'importe quelle valeur numérique, y compris des nombres entiers, des fractions décimales ou même des nombres écrits sous forme de mots.

Les entités de système sont gérées de manière centralisée, par conséquent, toutes les mises à jour sont disponibles automatiquement. Vous ne pouvez pas modifier les entités de système.

1.  Sur la page Entities, cliquez sur **System entities**. 

    ![Capture d'écran de l'onglet "System entities"](images/system_entities_1.png)

1.  Parcourez la liste des entités de système afin de choisir celles qui sont utiles pour votre application.
    - Pour afficher davantage d'informations sur une entité de système, y compris des exemples d'entrée correspondante, cliquez sur l'entité dans la liste.
    - Pour obtenir des informations complètes sur les entités de système disponibles, reportez-vous à la rubrique [Entités de système](/docs/services/assistant?topic=assistant-system-entities).

1.  Cliquez sur le bouton bascule en regard d'une entité de système pour l'activer ou la désactiver.

Une fois que vous avez activé les entités de système, le service {{site.data.keyword.conversationshort}} recommence à s'entraîner. Une fois l'entraînement terminé, vous pouvez utiliser les entités.

## Nombre limite d'entités
{: #entities-limits}

Le nombre d'entités, de valeurs d'entité et de synonymes que vous pouvez créez dépend de votre forfait de service {{site.data.keyword.conversationshort}} :

| Forfait de service      | Nombre d'entités par compétence | Nombre de valeurs d'entité par compétence | Nombre de synonymes d'entité par compétence |
|-------------------|-------------------:|------------------------:|--------------------------:|
| Premium | 1 000 | 100 000 | 100 000 |
| Plus | 1 000 | 100 000 |                   100 000 |
| Standard | 1 000 | 100 000 | 100 000 |
| Lite, Plus Trial | 25 | 100 000 | 100 000 |
{: caption="Détails de forfait de service" caption-side="top"}

Les entités de système que vous activez pour utilisation sont prises en compte dans le nombre total d'entités utilisées de votre forfait.

| Forfait de service | Entités contextuelles et annotations |
|--------------|------------------------------------:|
| Premium      |        30 entités contextuelles avec 3000 annotations |
| Plus         |        20 entités contextuelles avec 2000 annotations |
| Standard     |        20 entités contextuelles avec 2000 annotations |
| Lite, Plus Trial |    10 entités contextuelles avec 1000 annotations |
{: caption="Détails de forfait de service (suite)" caption-side="top"}

## Edition d'entités
{: #entities-edit}

Vous pouvez cliquer sur n'importe quelle entité de la liste pour l'ouvrir et l'éditer. Vous pouvez renommer ou supprimer des entités, et vous pouvez ajouter, éditer ou supprimer des valeurs, des synonymes ou des canevas.

Si vous modifiez le type d'entité `synonym` et que vous le remplacez par `pattern`, ou inversement, les valeurs existantes sont converties, mais elles peuvent ne pas être utiles en l'état.
{: note}

## Recherche d'entités
{: #entities-search}

Utilisez la fonction de recherche pour trouver des noms, des valeurs et des synonymes d'entité.

1.  Dans la page **Entities**, cliquez sur l'icône de recherche.

    ![Icône de recherche dans l'en-tête de page Intents](images/header-search-icon.png)

    Les entités de système ne peuvent pas faire l'objet d'une recherche.
    {: note}

1.  Entrez un terme ou une phrase de recherche.

    ![Terme de recherche pour une entité](images/searchent_1.png)

Les entités contenant votre terme de recherche, avec des exemples correspondants, s'affichent.

  ![Résultats renvoyés par la fonction de recherche d'entité](images/searchent_2.png)

## Exportation d'entités
{: #entities-export}

Vous pouvez exporter un certain nombre d'entités vers un fichier CSV, de manière à pouvoir les importer et les réutiliser pour une autre application {{site.data.keyword.conversationshort}}.

- Les informations de canevas sont incluses dans l'exportation CSV. Toute chaîne encapsulée avec `/` sera considérée comme un canevas (par opposition à un synonyme).
- Les annotations associées aux entités contextuelles ne sont pas exportées. Vous devez exporter l'intégralité de la compétence de dialogue pour capturer à la fois la valeur d'entité et les annotations associées.

1.  Sélectionnez les entités souhaitées, puis cliquez sur **Export**.

    ![Bouton Export pour une entité](images/ExportEntity.png)

## Importation d'entités
{: #entities-import}

Si vous possédez un grand nombre d'entités, vous trouverez peut-être plus facile de les importer à partir d'un fichier CSV que de les définir une par une. 

Les annotations d'entité ne sont pas incluses dans l'importation d'un fichier CSV d'entité. Vous devez importer l'intégralité de la compétence de dialogue pour conserver les annotations associées à une entité contextuelle de cette compétence. Si vous exportez et importez uniquement des entités, toutes les entités contextuelles que vous avez exportées sont traitées comme des entités basées sur un dictionnaire une fois que vous les avez importées.
{: note}

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

    Sauvegardez le fichier CSV au format UTF-8 et sans marque d'ordre d'octet. Le fichier CSV ne doit pas excéder 10 Mo. Si votre fichier CSV est plus gros, songez à le fractionner en plusieurs fichiers et à les importer séparément.  Ouvrez votre compétence de dialogue et cliquez sur l'onglet **Entities**.
    {: tip}

1.  Cliquez sur ![Import](images/importGA.png), puis faites glisser un fichier, ou recherchez un fichier sur votre ordinateur. Le fichier est validé et importé et le système commence à s'entraîner lui-même en utilisant les nouvelles données.

Vous pouvez visualiser les entités importées sur l'onglet Entities. Vous devrez peut-être actualiser la page pour voir les nouvelles entités.

## Suppression d'entités
{: #entities-delete}

Vous pouvez sélectionner un certain nombre d'entités à supprimer.

**Important** : lorsque vous supprimez des entités, vous supprimez également toutes les valeurs, tous les synonymes ou tous les canevas qui leur sont associés, et ces éléments ne peuvent plus être extraits ultérieurement. Tous les noeuds de dialogue qui font référence à ces entités ou ces valeurs doivent être mis à jour manuellement de manière à ne plus faire référence au contenu supprimé.

1.  Sélectionnez les entités que vous souhaitez supprimer, puis cliquez sur **Delete**.

    ![Bouton Delete pour une entité](images/DeleteEntity.png)
