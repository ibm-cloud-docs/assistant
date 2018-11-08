---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-07"

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

# Langues prises en charge
Le service {{site.data.keyword.conversationshort}} prend en charge les langues indiquées ci-après. Certaines fonctions du service sont prises en charge à des degrés divers pour chaque langue.
{: shortdesc}

Dans le tableau suivant, le niveau de prise en charge de la fonction et de la langue est indiqué selon les codes suivants :

- **GA** : la fonction est officiellement disponible et prise en charge pour cette langue. A noter que les fonctions peuvent continuer d'être mises à jour alors qu'elles sont officiellement disponibles.
- **Bêta** : la fonction est prise en charge uniquement en tant que version bêta et continue de faire l'objet de tests avant qu'elle ne soit officiellement disponible dans cette langue.
- **Vide** : lorsqu'aucun code n'est indiqué, cela signifie qu'une fonction n'est pas disponible dans cette langue.

|                  | **[Définition d'intentions](intents.html)**, d'**[entités](entities.html)** et d'un **[dialogue](dialog-build.html)** | **[Options Absolute scoring et Mark as irrelevant](intents.html#mark-irrelevant)** | **Entités de système ([number](system-entities.html#sys-number), [currency](system-entities.html#sys-currency), [percentage](system-entities.html#sys-percentage), [date, time](system-entities.html#sys-datetime))** | **[Fonction Fuzzy Matching](entities.html#fuzzy-matching)** |
|:---|:---:|:---:|:---:|:---:|
| **Anglais (en)**                   | GA | GA | GA </br> Bêta ([location](system-entities.html#sys-location), [person](system-entities.html#sys-person)) | Bêta (Stemming, misspelling et partial match) |
| **Arabe (ar)**                    | GA | Bêta | Bêta | Bêta (Misspelling uniquement) |
| **Chinois (simplifié) (zh-cn)**   | Bêta | Bêta | Bêta |  |
| **Chinois (traditionnel) (zh-tw)**  | Bêta | Bêta |  |  |
| **Tchèque (cs)**                     | Bêta | Bêta | Bêta | Bêta (Misspelling uniquement) |
| **Néerlandais (nl)**                     | Bêta | Bêta | Bêta |  |
| **Français (fr)**                    | GA | GA | GA | Bêta (Misspelling uniquement) |
| **Allemand (de)**                    | GA | GA | GA | Bêta (Misspelling uniquement) |
| **Italien (it)**                   | GA | GA | GA | Bêta (Misspelling uniquement) |
| **Japonais (ja)**                  | GA | GA | GA | Bêta (Misspelling uniquement) |
| **Coréen (ko)**                    | GA | GA | GA | Bêta (Misspelling uniquement) |
| **Portugais (brésil) (pt-br)** | GA | GA | GA | Bêta (Misspelling uniquement) |
| **Espagnol (es)**                   | GA | GA | GA | Bêta (Misspelling uniquement) ||

**Remarque :** le service {{site.data.keyword.conversationshort}} prend en charge plusieurs langues, mais l'interface de l'outil proprement dite (descriptions, libellés, etc.) est en anglais. Toutes les langues prises en charge peuvent être utilisées pour la saisie et l'entraînement via l'interface en anglais.

**Conformité à la norme GB18030** : GB18030 est une norme chinoise qui spécifie une page de codes étendue destinée au marché chinois. Cette norme de page de codes est importante pour l'industrie des logiciels car le China National Information Technology Standardization Technical Committee exige que toute application logicielle commercialisée sur le marché chinois après le 1er septembre 2001 respecte la norme GB18030. Le service {{site.data.keyword.conversationshort}} prend en charge ce codage et est certifié conforme à la norme GB18030. 

## Modification de la langue d'un espace de travail

Une fois qu'un espace de travail est créé, sa langue ne peut pas être modifiée. S'il doit impérativement modifier la langue d'un espace de travail, l'utilisateur doit télécharger celui-ci. Il doit ensuite utiliser un éditeur de texte pour éditer le fichier JSON obtenu à l'issue du téléchargement et rechercher une propriété JSON nommée `language`.

La langue d'origine de l'espace de travail doit être définie pour la propriété `language`, par exemple, la valeur `en` pour l'anglais. Modifiez la valeur de cette propriété, en la remplaçant par la langue souhaitée (`fr` pour le français, `de` pour l'allemand, etc.). Sauvegardez les modifications effectuées dans le fichier JSON et importez le fichier ainsi modifié dans votre instance de service {{site.data.keyword.conversationshort}}.

## Configuration des langues bidirectionnelles
{: #configuring-bi-directional}

Pour les langues bidirectionnelles, telles que l'arabe, vous mouvez modifier vos préférences d'espace de travail en conséquence. Sur l'onglet de votre espace de travail, sélectionnez le menu déroulant *Actions*, puis l'option **Bidi preferences** (cette option s'affiche uniquement dans les espaces de travail pour lesquels une langue bidirectionnelle est définie) :

![Option de menu Bidi preferences](images/bidi_prefs.png)

Sélectionnez l'une des options suivantes pour votre espace de travail :

- **GUI Direction** : indique le sens de la disposition des éléments, tels que les boutons ou les menus, dans l'interface graphique. Choisissez`LTR` (de gauche à droite) ou `RTL` (de droite à gauche). Si cette option n'est pas spécifiée, l'outil applique le paramétrage du sens de l'interface graphique défini dans le navigateur Web.
- **Text Direction** : indique le sens du texte lors de la saisie. Choisissez `LTR` (de gauche à droite) ou `RTL` (de droite à gauche) ou sélectionnez `Auto` pour activer la sélection automatique du sens du texte en fonction des paramètres de votre système. Avec l'option `None`, le texte s'affiche de gauche à droite.
- **Numeric Shaping** : indique quelle forme numérique utiliser lors de la présentation de chiffres ordinaires. Choisissez `Nominal`, `Arabic-Indic` ou `Arabic-European`. Avec l'option `None`, les chiffres s'affichent au format occidental.
- **Calendar Type** : indique le mode de filtrage des dates dans l'interface utilisateur de l'espace de travail. Choisissez `Islamic-Civil`, `Islamic-Tabular`, `Islamic-Umm al-Qura` ou `Gregorian`. **Remarque** : ce paramètre ne s'applique pas au panneau "Try it out".

![Options de préférences bidirectionnelles](images/bidi_opts.png)

Lorsque vous avez terminé de choisir vos options, cliquez sur **Update** pour les sauvegarder et revenir à l'onglet de l'espace de travail.

## Utilisation des caractères accentués
{: #working-with-accents}

Dans un paramétrage conversationnel, les utilisateurs peuvent ou non utiliser des accents lors de l'interaction avec le service {{site.data.keyword.conversationshort}}. Ainsi, les versions accentuées et non accentuées de mots peuvent être interprétées de la même manière pour la détection d'intention et la reconnaissance d'entité.

Toutefois, dans certaines langues, telles que l'espagnol, certains accents peuvent modifier le sens de l'entité. Par conséquent, pour la détection d'entité, même si l'entité d'origine comporte implicitement un accent, le service peut aussi établir une correspondance avec la version non accentuée de la même entité, mais avec une cote de confiance légèrement inférieure.

Par exemple, avec le mot "barrió", qui est accentué et correspond au participe passé du verbe "barrer" (balayer, en français), le service peut également établir une correspondance avec le "barrio" (quartier, en français), mais avec une cote de confiance légèrement inférieure.

Le système fournit la cote de confiance la plus élevée dans les entités pour lesquelles des correspondances exactes sont trouvées. Par exemple, `barrio` ne sera pas détecté si `barrió` figure dans l'ensemble d'entraînement et `barrió` ne sera pas détecté si `barrio` figure dans l'ensemble d'entraînement.

Vous êtes supposé entraîner le système à reconnaître les mots avec les caractères et les accents appropriés. Par exemple, si vous vous attendez à recevoir la réponse `barrió`, vous devez ajouter `barrió` dans l'ensemble d’entraînement.

Bien qu'il ne s'agisse pas d'accents, la même règle s'applique aux mots qui comportent, par exemple, la lettre espagnole `ñ` et non la lettre `n`, par exemple, "uña" et "una". En l'occurrence, la lettre `ñ` n'est pas juste une lettre `n` dotée d'un accent, mais réellement une lettre spécifique de l'alphabet espagnol.

Vous pouvez activer la fonction Fuzzy matching si vous pensez que vos clients n'utiliseront pas les accents de manière appropriée ou feront des fautes d'orthographe (par exemple, en tapant un `n` au lieu d'un `ñ`), ou vous pouvez les exclure explicitement des exemples d'entraînement.
