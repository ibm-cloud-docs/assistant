---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-09"

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

# Planification de vos intentions et entités
{: #planning-your-entities}

Afin de planifier les *intentions* pour votre application, vous devez prendre en compte ce que vos clients pourraient vouloir faire et ce que vous voulez que votre application soit capable de gérer. Choisir l'intention appropriée pour une entrée utilisateur est la première étape du processus permettant de fournir une réponse utile. Les intentions que vous identifiez pour votre application déterminent les flux de dialogue que vous devez créer. Elles peuvent également déterminer les systèmes de back end qui doivent être intégrés à votre application afin de lui permettre de répondre à des demandes émises par des clients (par exemple, des bases de données client ou des systèmes de paiement).

Une *entité* représente un terme ou un objet présent dans l'entrée utilisateur qui fournit des précisions ou un contexte spécifique pour une intention donnée. Si des intentions représentent des verbes (une action que l'utilisateur souhaite effectuer), les entités représentent des noms (par exemple, l'objet ou le contexte d'une action). Grâce aux entités, une seule et même intention peut représenter plusieurs actions spécifiques. Une entité définit une classe d'objets, avec des valeurs spécifiques représentant d'éventuels objets de cette classe.

En gros, si vous souhaitez capturer une demande ou effectuer une action, utilisez une intention. Si vous souhaitez capturer des informations qui peuvent affecter la façon dont vous allez répondre à une demande ou une action, utilisez une entité.

Par exemple, imaginons que vous souhaitiez créer une application de tableau de bord de voiture intelligente permettant aux utilisateurs d'activer ou de désactiver des dispositifs :

1.  Rassemblez le plus grand nombre de questions, commandes ou autres entrées possibles auprès de véritables clients. Le fait d'utiliser des entrées provenant de véritables personnes au lieu de demander à des spécialistes de créer des listes d'énoncés possibles, permet d'obtenir une image plus précise des entrées attendues. N'oubliez pas que les clients peuvent formuler un même type de demande de différentes façons. Ainsi, les exemples d'énoncé suivants correspondent tous à des demandes visant à désactiver un dispositif :

    - `Headlights off`
    - `Turn the radio off`
    - `Stop the air conditioner`

1.  Une fois que vous avez collecté des exemples, triez-les dans des catégories en fonction des capacités que votre application doit prendre en charge ; ces catégories représentent les intentions que vous allez définir, tandis que les exemples aideront votre application à identifier ces intentions dans de nouvelles entrées.

    Vous ne devez pas créer d'intentions qui se ressemblent trop. Le service {{site.data.keyword.conversationshort}} pourrait avoir des difficultés à distinguer des intentions similaires. Si vous trouvez que plusieurs de vos intentions ont une signification similaire, vous pouvez essayer de les rassembler dans une seule intention, puis utiliser des entités afin de fournir plusieurs réponses possibles pour cette intention.
    {: tip}

    Dans la mesure où les exemples précédents représentent tous la même intention (désactiver quelque chose), vous pouvez nommer cette catégorie `#turn_off`.
    Gardez à l'esprit qu'il n'est pas nécessaire que l'entrée d'un client corresponde exactement à l'un de vos exemples ; les intentions sont reconnues à l'aide du traitement automatique du langage naturel.
    {: tip}

1.  Utilisez ensuite une entité pour représenter le dispositif que l'utilisateur veut désactiver. Pour ce faire, vous pouvez créer une entité nommée `@accessory` et lui attribuer les valeurs possibles suivantes :

    - `headlights`
    - `radio`
    - `air conditioner`

    Pour chaque valeur, vous pouvez également spécifier des synonymes. Par exemple, vous pouvez indiquer `AC` et `cooling` comme synonymes de `air conditioner` ; ces trois chaînes seront interprétées comme une seule et même valeur. Le fait de répertorier différentes formulations employées par un utilisateur pour faire référence à la même valeur permet d'améliorer le niveau d'exactitude de votre application.
1.  Lorsque l'entrée utilisateur est reçue, la conversation reconnaît à la fois les intentions et les entités. Votre flux de dialogue peut alors les utiliser pour fournir la réponse la plus appropriée. Il est recommandé de créer une entité uniquement pour quelque chose d'important en ce qui concerne la façon dont l'application répond à une intention.
    Outre les entités que vous définissez, le service fournit un groupe d'entités de système qui sont déjà définies et disponibles pour vous. Pour plus d'informations, reportez-vous à la rubrique [Activation des entités de système](entities.html#enable_system_entities).
    {: tip}

1.  Continuez d'affiner vos intentions, vos entités et vos exemples selon vos besoins. Ne considérez pas votre groupe d'intentions et d'entités comme un produit final. Lorsque vous [créerez vos dialogues](dialog-build.html), il est fort probable que vous identifierez d'autres intentions et entités à ajouter. Vous pouvez également continuer de collecter des entrées auprès de nouveaux clients ; ce processus itératif améliore la précision avec laquelle votre application reconnaît les intentions et les entités.
