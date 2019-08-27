---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-17"

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
{:gif: data-image-type='gif'}

# Nouvelles entités de système ![Bêta](images/beta.png)
{: #beta-system-entities}

Activez les nouvelles entités de système pour tirer parti des améliorations apportées aux entités de système numériques fournies par IBM.

Actuellement, ce paramètre peut être activé pour les compétences de dialogue écrites en anglais ou en allemand uniquement.
{: note}

Les nouvelles entités de système peuvent reconnaître des mentions plus nuancées dans l'entrée utilisateur. Par exemple, `@sys-date` peut calculer la date d'un jour férié national, par exemple, `Thanksgiving`, lorsqu'il est mentionné par son nom. Et `@sys-date` peut reconnaître qu'une année est spécifiée dans le cadre d'une date mentionnée dans l'entrée utilisateur. Ces améliorations permettent également à votre assistant de distinguer plus facilement les nombreuses entités de système basées sur des nombres. Par exemple, une mention de date, telle que `10 mai`, qui est reconnue comme étant `@sys-date` n'est pas également identifiée comme une mention `@sys-number`.

Les entités de système `@sys-location` et `@sys-person` n'ont pas été modifiées.
{: note}

## Activation des nouvelles entités de système
{: #beta-system-entities-enable}

Pour activer les nouvelles entités de système, procédez comme suit :

1.  Dans la page Skills, ouvrez votre compétence.
1.  Cliquez sur l'onglet **Options**.
1.  Cliquez sur **System Entities**, puis choisissez **Try beta**.
1.  Cliquez sur l'onglet **Entities**, puis cliquez sur **System entities**.
1.  Mettez sous tension les entités de système que vous souhaitez utiliser dans votre dialogue.

Testez les nouvelles entités de système en ajoutant une ou plusieurs d'entre elles à des conditions de noeud de dialogue ou à la condition des réponses conditionnelles d'un noeud de dialogue. Ensuite, dans le panneau "Try it out", soumettez les énoncés de l'utilisateur qui déclenchent les nœuds que vous avez ajoutés. 

Si vous décidez de préférer le comportement de la version précédente des entités de système, vous pouvez arrêter d'utiliser la version bêta à tout moment. Revenez à la page **System entities** de l'onglet **Options** et choisissez **Keep existing**. Donnez à Watson le temps de s'entraîner à nouveau.

Si vous décidez de continuer à utiliser la version bêta, passez en revue les noeuds de dialogue existants qui conditionnent ou renvoient des valeurs d'entité de système afin de déterminer si vous devez apporter des modifications. Par exemple, certaines mentions ont déjà été classées dans plus d'un type d'entité système. Maintenant, si une devise est mentionnée, par exemple, elle est classifiée `@sys-currency` uniquement. Vous pouvez être en mesure de simplifier la logique que vous avez utilisée avant de contourner le comportement. Ou vous devrez peut-être réviser la logique que vous avez ajoutée et qui repose sur l'ancien comportement.

### Nouvelles propriétés d'entité de système
{: #beta-system-entities-props}

Pour améliorer les entités de système, de nouvelles propriétés ont été ajoutées aux objets d'entités de système basées sur un nombre.

Le tableau suivant récapitule les nouvelles propriétés qui ont été ajoutées. Pour afficher les propriétés associées aux entités de système en cours, reportez-vous à la rubrique [Détails des entités de système](/docs/services/assistant?topic=assistant-system-entities).

Tableau 1. Nouvelles propriétés d'entité de système

| Entité de système | Propriété | Description |
|---------------|----------|-------------|
| `@sys-date` | `alternatives` | Capture des dates autres que celle enregistrée en tant que valeur `@sys-date` que l'utilisateur aurait pu vouloir indiquer lorsque la date n'est pas clairement indiquée. Par exemple, si la date du jour est le 10 mars 2019 et que l'utilisateur entre `le 1er mars`, la date du 1er mars de l'année suivante est enregistrée en tant que `@sys-date` (`2020-03-01`). Cependant, l'utilisateur aurait pu signifier le 1er mars 2019. Le système enregistre également la date de cette année (`2019-03-01`) en tant que valeur de date alternative. Les valeurs alternatives sont sauvegardées en tant qu'objet qui contient une valeur et une confiance pour chaque date alternative et est stocké dans un tableau JSON. Pour obtenir une valeur alternative, utilisez la syntaxe suivante : `@sys-date.alternative`. Dans la plupart des cas, la date future est enregistrée en tant que valeur `@sys-date` et la date passée est enregistrée en tant que valeur alternative. Cela s'applique aux jours de la semaine lorsque le jour de la semaine est spécifié sans article tel que `le` ou adjectif tel que `courant` ou `prochain`. Par exemple, `êtes-vous ouvert le lundi ?` Toutefois, si un utilisateur spécifie un jour de la semaine dans une phrase telle que `le lundi` ou `lundi prochain`, la date est supposée être une date future et aucune date de remplacement n'est créée pour celle-ci. Par exemple, si aujourd'hui est mardi et qu'un utilisateur entre `ce mardi` ou `mardi prochain`, `@sys-date` est défini sur la date du mardi de la semaine prochaine et aucune date de remplacement n'est créée. |
| `@sys-date` | `datetime_link` | Si cette entité apparaît, indique que l'entrée utilisateur mentionne une date et une heure ensemble, ce qui implique que la date et l'heure sont liées. Les valeurs d'index de début et de fin de la chaîne incluant la date et l'heure sont enregistrées dans le nom du lien. Par exemple, si l'entrée est, `Etes-vous ouvert aujourd'hui à 5 heures ?`, `aujourd'hui` est reconnu comme référence de date à l'emplacement `[13,18]` et `à 5 heures` est reconnu comme heure de référence à l'emplacement `[19,23]`. Une valeur d'emplacement `[13,23]`, qui couvre à la fois les mentions de date et d'heure, est ajoutée au lien datetime_link résultant. Elle est nommée `datetime_link_13_23` et est incluse dans le résultat des entités de système `@sys-date` et `@sys-time` qui participent à la relation. |
| `@sys-date` | `festival` | Reconnaît les noms de congés spécifiques à l'environnement local et peut renvoyer la date du jour férié à venir, tel que `Thanksgiving`, `Noël` et `Halloween`. Utilisez toutes les lettres minuscules si vous avez besoin de spécifier un nom de jour férié, dans une condition, par exemple (`@sys-date.festival == 'thanksgiving'`). |
| `@sys-date` | `granularity` | Reconnaît les mentions de délais, par exemple `année prochaine` (=`année`). Les options sont `jour`, `fin de semaine`, `semaine`, `quinzaine`, `mois`, `trimestre ` et `année`. |
| `@sys-date` | `interpretation` | Objet renvoyé par votre assistant, contenant de nouvelles zones qui augmentent la précision du classifieur d'entité de système. Vous pouvez omettre l'`interprétation` de l'expression que vous utilisez pour faire référence à ses zones. Par exemple, vous pouvez accéder à la valeur de la zone `festival` dans l'objet d'interprétation à l'aide de la syntaxe abrégée `<? @sys-date.festival ?>`. |
| `@sys-date` | `range_link` | Si cette entité apparaît, indique que l'entrée utilisateur contient une syntaxe suggérant qu'une plage de dates est spécifiée. Par exemple, l'entrée peut être `Je voyagerai du 15 au 22 mars.`. Chacune des deux entités de système `@sys-date` détectées possède une propriété `range_link` dans sa sortie. Des informations supplémentaires sont fournies, notamment le rôle joué par chaque entité `@sys-date` dans la relation de plage. Par exemple, la date de début a un type de rôle `date_from` et la date de fin a un type de rôle `date_to`. Pour vérifier une valeur de rôle, vous pouvez utiliser la syntaxe `@sys-date.role?.type == 'date_from'`. Si l'entrée utilisateur implique une plage, mais qu'une seule date est spécifiée, la propriété `range_link` n'est pas renvoyée, mais un type de rôle est renvoyé. Par exemple, si l'utilisateur demande `Avez-vous été ouvert depuis hier ?` La date d'hier est reconnue comme mention `@sys-date` et un rôle de type `date_from` est renvoyé pour cette date. Un élément `range_modifier` qui identifie le mot dans l'entrée qui déclenche l'identification d'une plage est également renvoyé. Dans cet exemple, le modificateur est `since`. |
| `@sys-date` | `relative_year`, `relative_month`, `relative_week`, `relative_weekend`, `relative_day` | Reconnaît et capture les mentions de valeurs de date relatives dans l'entrée utilisateur, telles que `il y a deux semaines` (`relative_week = -2`), ou `ce week-end (relative_weekend = 0)` ou `aujourd'hui` (`relative_day = 0`). |
| `@sys-date` | `specific_year`, `specific_quarter`, `specific_month`, `specific_day`, `specific_day_of_week` | Reconnaît et capture les mentions de valeurs de date spécifiques dans l'entrée de l'utilisateur, telles que `2020` (`specific_year = 2020`), `T2` (`specific_quarter = 2`), `Mars` (`specific_month = 3`), `Lundi` (`specific_day_of_week = monday`) ou `3e` (`specific_day = 3`). |
| `@sys-number` | `interpretation` | Objet renvoyé par votre assistant, contenant de nouvelles zones qui augmentent la précision du classifieur d'entité de système. Vous pouvez omettre l'`interprétation` de l'expression que vous utilisez pour faire référence à ses zones. Par exemple, vous pouvez accéder à la valeur de la zone `range_link` dans l'objet d'interprétation à l'aide de la syntaxe abrégée `<? @sys-date.range_link ?>`. |
| `@sys-number` | `range_link` | Si cette entité apparaît, indique que l'entrée utilisateur contient une syntaxe suggérant qu'une plage de numéros est spécifiée. Par exemple, l'entrée peut être `Je suis intéressé par la plage de 5 à 7.`. Chacune des deux entités de système `@sys-number` détectées possède une propriété `range_link` dans sa sortie. Des informations supplémentaires sont fournies, notamment le rôle joué par chaque entité `@sys-number` dans la relation de plage. Par exemple, le numéro de début a un type de rôle `number_from` et le numéro de fin a un type de rôle `number_to`. Pour vérifier une valeur de rôle, vous pouvez utiliser la syntaxe `@sys-number.role?.type == 'number_from'`. |
| `@sys-time` | `alternatives` | Capture des heures autres que celle enregistrée en tant que valeur `@sys-time` que l'utilisateur aurait pu vouloir indiquer lorsque l'heure n'est pas clairement indiquée. Par exemple, lorsqu'un utilisateur dit que `La réunion est à 5 heures`, le système calcule l'heure sous la forme 5:00:00 ou 5 heures du matin et la sauvegarde en tant que `@sys-time`. Toutefois, il se peut que l'utilisateur ait voulu dire 17:00:00 ou 17 heures. Le système sauvegarde également la dernière heure en tant que valeur d'heure de remplacement. Si l'utilisateur entre `La réunion est à 17 heures`, `@sys-time` est défini sur 17:00:00 et aucune valeur de remplacement n'est créée, car il n'y a pas de confusion entre 5 heures et 17 heures. Les valeurs de remplacement sont sauvegardées en tant qu'objet qui contient une valeur et une confiance pour chaque heure de remplacement et qui est stocké dans un tableau JSON. Pour obtenir une valeur de remplacement, utilisez la syntaxe suivante : `@sys-time.alternative`. |
| `sys-time` | `datetime_link` | Si cette entité apparaît, indique que l'entrée utilisateur mentionne une date et une heure ensemble, ce qui implique que la date et l'heure sont liées. Les valeurs d'index de début et de fin de la chaîne incluant la date et l'heure sont enregistrées dans le nom du lien. Par exemple, si l'entrée est, `Etes-vous ouvert aujourd'hui à 5 heures ?`, `aujourd'hui` est reconnu comme référence de date à l'emplacement `[13,18]` et `à 5 heures` est reconnu comme heure de référence à l'emplacement `[19,23]`. Une valeur d'emplacement `[13,23]`, qui couvre à la fois les mentions de date et d'heure, est ajoutée au lien datetime_link résultant. Elle est nommée `datetime_link_13_23` et est incluse dans le résultat des entités de système `@sys-date` et `@sys-time` qui participent à la relation. |
| `@sys-time` | `granularity` | Reconnaît les mentions de temps, telles que `maintenant` (=`instant`) ou `3 heures` ou `midi` (=`hour`) ou `17:00:00` (=`second`). Les options sont `heure`, `minute`, `seconde` et `instant`. |
| `@sys-time` | `interpretation` | Objet renvoyé par votre assistant, contenant de nouvelles zones qui augmentent la précision du classifieur d'entité de système. Vous pouvez omettre l'`interprétation` de l'expression que vous utilisez pour faire référence à ses zones. Par exemple, vous pouvez accéder à la valeur de la zone `granularity` dans l'objet d'interprétation à l'aide de la syntaxe abrégée `<? @sys-time.granularity ?>`. |
| `@sys-time` | `part_of_day` | Reconnaît les termes qui représentent les moments de la journée, tels que `matin`, `après-midi`, `soir`, `nuit` ou `maintenant`. Définit également des heures arbitraires, telles que `9:00:00` pour le matin, `15:00:00` pour l'après-midi, `18:00:00` pour le soir et `22:00:00` pour la nuit. |
| `@sys-time` | `range_link` | Si cette entité apparaît, indique que l'entrée utilisateur contient une syntaxe suggérant qu'une plage temporelle est spécifiée. Par exemple, l'entrée peut être `Etes-vous ouvert de 9 heures à 11 heures ?` Chacune des deux entités de système `@sys-time` détectées possède une propriété `range_link` dans sa sortie. Des informations supplémentaires sont fournies, notamment le rôle joué par chaque entité `@sys-time` dans la relation de plage. Par exemple, l'heure de début a un type de rôle `time_from` et l'heure de fin a un type de rôle `time_to`. Pour vérifier une valeur de rôle, vous pouvez utiliser la syntaxe `@sys-time.role?.type == 'time_from'`. Si l'entrée utilisateur implique une plage, mais qu'une seule heure est spécifiée, la propriété `range_link` n'est pas renvoyée, mais un type de rôle est renvoyé. Par exemple, si l'utilisateur demande `Etes-vous ouvert jusqu'à 21 heures`, 21 heures est reconnu comme la mention `@sys-time` et un rôle de type `time_to` est renvoyé. Un élément `range_modifier` qui identifie le mot dans l'entrée qui déclenche l'identification d'une plage est également renvoyé. Dans cet exemple, le modificateur est `jusqu'à`. |
| `@sys-time` | `relative_hour`, `relative_minute`, `relative_second` | Reconnaît les mentions relatives du temps, telles que `il y a 5 heures` (`relative_hour = -5`),`dans deux minutes`(`relative_minute = 2`) ou `dans une seconde` (`relative_second = 1`). |
| `@sys-time` | `specific_hour`, `specific_minute`, `specific_second` | Reconnaît des mentions spécifiques du temps, comme `à 5 heures` (`specific_hour = 5`), `à 2h30`(`specific_minute = 30`) ou `23:30:22` (`specific_second = 22`). |
{: caption="Nouvelles propriétés d'entité de système" caption-side="top"}

Les valeurs du tableau suivant ne sont pas des propriétés de l'objet entité. Il s'agit de valeurs d'aide que vous pouvez utiliser dans le produit pour extraire rapidement les détails de date ou d'heure des nouvelles entités de système. 

Tableau 2. Fonctions auxiliaires des entités de système

| Entité de système | Valeur de l'auxiliaire | Description |
|---------------|----------|-------------|
| `@sys-date` | `day` | Renvoie le jour spécifié dans la date sous la forme d'une valeur numérique. Par exemple, si la date est `5 décembre 2019`, cette entité renvoie `5`. |
| `@sys-date` | `day_of_week` | Evalue la date de détermination du jour de la semaine. Stocke le jour de la semaine en minuscules. Par exemple, `Dimanche` est stocké en tant que `dimanche`. Utilisez une initiale en minuscule pour rechercher un nom de jour de la semaine dans une condition. Par exemple : `@sys-date.day_of_week == 'dimanche'` |
| `@sys-date` | `month` | Renvoie le mois spécifié dans la date sous la forme d'une valeur numérique. Par exemple, si la date est `5 décembre 2019`, cette entité renvoie `12`. |
| `@sys-date` | `year` | Renvoie l'année spécifiée dans la date sous la forme d'une valeur numérique. Par exemple, si la date est `5 décembre 2019`, cette entité renvoie `2019`. |
| `@sys-time` | `hour` | Renvoie l'heure spécifiée dans l'horodatage sous la forme d'une valeur numérique. Par exemple, si l'horodatage est `17:30:10`, cette entité renvoie `17` pour représenter 17 heures. |
| `@sys-time` | `minute` | Renvoie la valeur de minutes spécifiée dans l'horodatage. `30`. Si aucune minute n'est spécifiée, cette valeur auxiliaire renvoie `0`.|
| `@sys-time` | `second` | Renvoie la valeur de secondes spécifiée dans l'horodatage. `10`. Si aucune valeur seconde n'est spécifiée, cette valeur auxiliaire renvoie `0`.|
{: caption="Fonctions auxiliaires des entités de système" caption-side="top"}

### Exemples d'utilisation
{: #beta-system-entities-examples}

Vous pouvez utiliser les types d'expressions suivants pour tirer parti des nouvelles propriétés dans les entités de système améliorées. Le tableau présente les résultats renvoyés lorsqu'un utilisateur mentionne la date `4 juillet 2019` et l'heure `15:30`. Vous pouvez utiliser ces expressions dans les réponses texte de dialogue sans les entourez de la syntaxe `<? ?>`.

Tableau 3. Utilisation des nouvelles entités de système pour obtenir des détails sur la date et l'heure 

| Syntaxe d'expression SpEL | Résultat |
|------------------------|---------|
| `@sys-date.year` | `2019` |
| `@sys-date.month` | `7` |
| `@sys-date.day` | `4` |
| `@sys-date.day_of_week` | `jeudi` |
| `@sys-time.hour` | `15` |
| `@sys-time.minute` | `30` |
| `@sys-time.second` | `0` |
{: caption="Utiliser les nouvelles entités système pour obtenir des informations détaillées de date et d'heure" caption-side="top"}

Dans un noeud qui dépend de l'intention `#Customer_Care_Store_Hours`, vous pouvez ajouter des réponses conditionnelles qui utilisent de nouvelles propriétés d'entité de système pour fournir des réponses légèrement différentes sur les heures d'ouverture du magasin, en fonction de la demande de l'utilisateur. 

Vous ne souhaitez probablement pas utiliser toutes ces réponses conditionnelles dans un dialogue réel ; elles sont décrites ici simplement pour illustrer ce qui est possible.
{: tip}

Tableau 4. Utilisation de nouvelles entités de système dans les réponses conditionnelles

| Syntaxe de condition de réponse conditionnelle | Description | Exemple de texte de réponse |
|--------------------------------|-------------|----------|
| `@sys-date.festival == 'thanksgiving'` | Vérifie si le client demande les horaires d'ouverture le jour de Thanksgiving en particulier. | Nous distribuons des repas gratuits aux personnes dans le besoin le jour de Thanksgiving. |
| `@sys-date.festival` | Vérifie si le client demande des horaires d'ouverture pour un autre jour férié. | Nous sommes fermés le jour de Noël, le 4 juillet et le jour du Président. Pour les autres jours fériés, nous sommes ouverts de 10h à 17h. |
| `@sys-time.part_of_day == 'nuit'` | Vérifie si l'utilisateur inclut des termes qui mentionnent la nuit comme moment de la journée dans l'entrée. | Nous ne sommes pas ouverts tard ; nous fermons à 21 heures la plupart des jours. |
| `@sys-date.datetime_link && input.text.contains('aujourd'hui') && now().sameOrAfter(@sys-time)` | Vérifie si l'entrée contient une phrase telle que `aujourd'hui à 8 heures`. Vérifie également si l'élément `@sys-time` détecté est antérieur à l'heure courante de la journée. Vous pouvez ajouter une variable contextuelle à la réponse conditionnelle qui crée une variable `$new_time` avec la valeur `<? @sys-time.plusHours(12) ?>`. Vous pouvez utiliser cette approche pour créer une variable de date qui capture l'heure la plus probable énoncée par un utilisateur qui, à midi, demande : `Etes-vous ouvert aujourd'hui à 8 heures ?` Le système suppose que l'utilisateur veut dire 8h00 et non 20h00. | Vous voulez dire aujourd'hui à $new_time, n'est-ce pas ? |
| `@sys-time.range_link && entities[0].role?.type == 'time_from' && entities[1].role?.type == 'time_to'` | Vérifie si l'entrée contient une plage de temps. La condition garantit également que la première entité de système `@sys-time` répertoriée dans le tableau des entités est l'heure de début et que la seconde est l'heure de fin avant de les inclure dans la réponse. | Vous voulez savoir si nous sommes ouverts entre `<? entities[0] ?>` et `<? entities[1] ?>`, c'est bien cela ? |
| `@sys-time && entities.size() < 2 && entities[0].role?.type == 'time_to'` | Vérifie si l'entrée contient une mention `@sys-time` qui comporte un type de rôle `time_to`. Si l'entrée utilisateur correspond à cette condition, elle suggère que l'utilisateur a spécifié l'heure de fin d'une plage de temps ouverte. Par exemple, la réponse est déclenchée par l'entrée `Etes-vous ouvert jusqu'à 21 heures ?` Cette entrée concorde, car elle n'identifie qu'une seule heure dans une plage temporelle, et la mention a le rôle `time_to`. | Voulez-vous dire à partir de maintenant (`<? now().reformatDateTime('h:mm a') ?>`) jusqu'à `<? @sys-time.reformatDateTime('h:mm a') ?>` ? |
| `@sys-date.day_of_week == 'dimanche'` | Vérifie si une date spécifique que demande l'utilisateur est un dimanche. | Nous sommes fermés le dimanche. |
| `@sys-date.specific_day_of_week == 'lundi' && @sys-date.alternative` | Vérifie si l'utilisateur a mentionné le jour de la semaine `lundi` dans sa requête. Par exemple, `Etes-vous ouvert le lundi ?` La condition vérifie également si des dates de remplacement ont été stockées. Des dates de remplacement sont créées lorsque votre assistant n'est pas tout à fait sûr du lundi demandé par l'utilisateur, il stocke donc les dates des autres lundis également. Lorsqu'un utilisateur spécifie un jour de la semaine, votre assistant part du principe que l'utilisateur signifie l'occurrence future du jour (le lundi à venir). Vous pouvez ajouter une réponse qui vérifie à nouveau la date demandée, en utilisant la valeur de remplacement détectée. Dans ce cas, la date de remplacement est celle du lundi précédent. | Voulez-vous dire `@sys-date` ou `@sys-date.alternative` ? |
| `true` | Répond à toute autre demande d'informations sur les horaires d'ouverture du magasin. | Nous sommes ouverts de 9 heures à 21 heures du lundi au samedi. |
{: caption="Utiliser les nouvelles entités de système dans les réponses conditionnelles" caption-side="top"}
