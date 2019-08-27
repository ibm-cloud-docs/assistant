---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-12"

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

# Notes sur l'édition
{: #release-notes}

## Gestion des versions d'API du service
{: #release-notes-api-version}

Les demandes d'API nécessitent un paramètre de version qui prend une date au format `version=AAAA-MM-JJ`. Chaque fois que nous modifions l'API de manière irréversible, nous publions une nouvelle version mineure de l'API.

Envoyez le paramètre de version avec chaque demande d'API. {{site.data.keyword.conversationshort}} utilise la version d'API correspondant à la date que vous spécifiez ou la toute dernière version avant cette date. Ne laissez pas la date du jour par défaut. A la place, spécifiez une date correspondant à une version qui est compatible avec votre application et ne la changez pas tant que votre application n'est pas prête pour une version ultérieure.

- La version actuelle de V1 est `2019-02-28`.
- La version actuelle de V2 est `2019-02-28`.
- Le panneau "Try it out" de la compétence de dialogue utilise la version `2018-07-10`.
- Le panneau "Try it out" de la compétence de recherche utilise la version d'API {{site.data.keyword.discoveryshort}}, `2018-12-03`.

## Fonctions bêta
{: #release-notes-beta}

IBM publie des services, des fonctions et un support de langue pour votre évaluation qui sont classifiés comme version bêta. Ces fonctions peuvent être instables, changer fréquemment ou être arrêtées dans un délai très court. Les fonctions bêta, qui risquent de ne pas fournir le même niveau de performance ou de compatibilité que les fonctions de l'édition GA (General Availability), ne sont pas destinées à être utilisées dans un environnement de production. Les fonctions bêta ne sont prises en charge que sur le site [IBM Developer Answers ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://developer.ibm.com/answers/topics/watson-assistant/){: new_window}.

## Modèles mis à jour
{: #release-notes-updated-models}

Les algorithmes {{site.data.keyword.conversationshort}} peuvent être régulièrement affinés et mis à jour en fonction des commentaires en retour, des améliorations scientifiques et d'autres facteurs, afin d'améliorer sans cesse les performances. Les mises à jour apportées au modèle seront communiquées dans ces notes sur l'édition.

Les modèles existants ayant été formés ne seront pas immédiatement impactés, mais les modèles arrivés à expiration seront mis à jour vers le modèle en cours 60 jours après la mise à disposition d'un nouveau modèle, si vous n'avez pas encore effectué cette mise à jour.

**Remarque :** cette instruction de mise à jour s'applique uniquement aux langues et aux fonctions officiellement disponibles.

Les nouvelles fonctionnalités et modifications suivantes sont disponibles dans {{site.data.keyword.conversationshort}}.
Consultez notre [blog ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://medium.com/ibm-watson/assistant/home) pour rechercher des informations détaillées sur les avantages des nouvelles fonctionnalités pour votre entreprise.

## 12 août 2019
{: #12August2019}

- **Nouvelle méthode de dialogue** : la méthode `getMatch` a été ajoutée. Vous pouvez utiliser cette méthode pour extraire une occurrence spécifique d'un modèle d'expression régulière qui se reproduit dans l'entrée utilisateur. Pour plus de détails, reportez-vous à la rubrique des [méthodes de dialogue](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings-getMatch).

## 9 août 2019
{: #9August2019}

- **Visite guidée du produit** : pour certains utilisateurs novices, une nouvelle visite guidée du produit est présentée, que l'utilisateur peut choisir de suivre pour effectuer les premières étapes de la création d'un assistant.

## 6 août 2019
{: #6August2019}

- Les appels Webhook et les améliorations de la page Dialog sont disponibles à Dallas. 

## 1er août 2019
{: #1August2019}

Les mises à jour suivantes sont disponibles à tous les emplacements, sauf Dallas pour le moment.
{: important}

- **Les appels Webhook sont disponibles** : ajoutez des appels webhook aux noeuds de dialogue pour effectuer des appels de programmation vers une application externe dans le cadre du flux de conversation. La nouvelle prise en charge de webhook simplifie le processus de mise en oeuvre des appels. (Aucun autre objet JSON `action` n'est requis.) Pour plus d'informations, reportez-vous à la rubrique [Création d'un appel par programmation à partir d'un noeud de dialogue](/docs/services/assistant?topic=assistant-dialog-webhooks).

- **Amélioration de la réactivité de la page de dialogue** : dans toutes les instances de service, l'interface utilisateur de la page Dialog a été mise à jour pour utiliser une nouvelle bibliothèque JavaScript qui augmente la réactivité de la page. En conséquence, l'apparence de certains éléments de l'interface graphique, tels que les boutons, a légèrement changé, mais la fonction n'a pas changé. 

## 31 juillet 2019
{: #31July2019}

- **La compétence de recherche et la correction automatique sont généralement disponibles **: les fonctionnalités compétence de recherche et correction orthographique automatique, qui étaient auparavant disponibles en tant que fonctionnalités bêta, sont désormais commercialisées. 

  - Les compétences de recherche ne peuvent être créées que par les utilisateurs des forfaits Plus ou Premium.  
  - Vous pouvez activer la correction automatique uniquement pour les compétences de dialogue en anglais. Elle est automatiquement activée pour les nouvelles compétences de dialogue en anglais.


## 26 juillet 2019
{: #26July2019}

- **Le problème relatif aux compétences manquantes est résolu** : dans certains cas, les espaces de travail créés via l'API uniquement n'étaient pas affichés lorsque vous avez ouvert l'interface utilisateur de {{site.data.keyword.conversationshort}}. Ce problème a été traité. Tous les espaces de travail que vous créez à l'aide de l'API sont affichés en tant que compétences de dialogue lorsque vous ouvrez l'interface utilisateur. 

## 23 juillet 2019
{: #23July2019}

- **La recherche de la page Dialog est corrigée** : dans certaines compétences, la fonction de recherche ne fonctionnait pas dans la page Dialog. Le problème est maintenant corrigé.

## 17 juillet 2019
{: #17July2019}

- **Limite de choix de désambiguïsation** : vous pouvez désormais définir le nombre maximal d'options à afficher pour les utilisateurs lorsque l'assistant leur demande de clarifier ce qu'ils souhaitent faire. Pour plus d'informations sur la désambiguïsation, reportez-vous à la rubrique [Désambiguïsation](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).

- **Problème de recherche dans les dialogues** : dans certaines compétences, la fonction de recherche ne fonctionne pas dans la page Dialog. Une nouvelle bibliothèque d'interface utilisateur, qui augmente la réactivité de la page, est déployée par phases vers les instances de service existantes. Ce problème de recherche concerne uniquement les compétences de dialogue pour lesquelles la nouvelle bibliothèque n'est pas encore activée. 

- **Problème de compétences manquantes** : dans certains cas, les espaces de travail créés via l'API uniquement ne sont pas affichés lorsque vous ouvrez l'interface utilisateur de {{site.data.keyword.conversationshort}}. Normalement, ces espaces de travail sont affichés en tant que compétences de dialogue. Si vous ne voyez pas vos compétences dans l'interface utilisateur, ne vous inquiétez pas, ils sont toujours là.Contactez le support pour signaler le problème afin que l'équipe puisse activer l'affichage correct des espaces de travail. 

<!--- **Premium plan maximum inactivity period increases**: The maximum time that a session can persist after a user stops interacting with the assistant increased from 1 day to 7 days (168 hours).
-->
## 15 juillet 2019
{: #15July2019}

- **Mise à niveau des entités de système numériques disponible à Dallas ![Bêta](images/beta.png)** : les nouvelles entités système sont désormais également disponibles en tant que fonctionnalité bêta pour les instances hébergées à Dallas. Reportez-vous à la rubrique [Nouvelles entités de système](/docs/services/assistant?topic=assistant-beta-system-entities)

## 12 juin 2019
{: #12June2019}

- **Mise à niveau des entités de système numériques ![Bêta](images/beta.png)** : de nouvelles entités de système sont disponibles en tant que fonctionnalité bêta que vous pouvez activer dans les dialogues écrits en anglais ou en allemand. Les entités de système révisées offrent une meilleure compréhension de la date et de l'heure. Elles peuvent reconnaître les plages de dates et de nombres, les références de jours fériés et classer les mentions avec plus de précision. Par exemple, une date telle que `May 15` est reconnue comme une mention de date (`@sys-date:2019-05-15`), et *n'est pas* également identifiée comme une référence numérique (`@sys-number:15`). Reportez-vous à la rubrique [Nouvelles entités de système](/docs/services/assistant?topic=assistant-beta-system-entities)

  Vous ne pouvez pas essayer ces entités de système dans les instances hébergées à Dallas actuellement, sauf si vous faites partie du programme bêta. Pour plus d'informations sur la participation au programme bêta, reportez-vous à la rubrique [Participer au programme bêta](/docs/services/assistant?topic=assistant-feedback#feedback-beta).
  {: note}

- **Un forfait d'essai Plus est disponible** : vous pouvez utiliser le forfait d'essai gratuit Plus pour tester les fonctionnalités du forfait Plus et vous aider dans votre décision d'achat. La durée de l'essai est de 30 jours. Une fois la période d’essai terminée, si vous n’effectuez pas de mise à niveau vers un forfait Plus, votre instance d'essai Plus est convertie en instance de forfait Lite. 

## 23 mai 2019
{: #23May2019}

Les mises à jour suivantes sont disponibles à tous les emplacements, sauf Dallas pour le moment.
{: important}

- **Navigation mise à jour** : la page d'accueil a été supprimée et l'ordre des onglets Assistants et Skills a été inversé. Le nouvel ordre des onglets vous encourage à commencer votre travail de développement en créant un assistant, puis une compétence.  

- **Les paramètres de désambiguïsation ont été déplacés** : la bascule permettant d'activer la désambiguïsation, une fonctionnalité disponible uniquement pour les utilisateurs des forfaits Plus et Premium, a été déplacée. Le bouton **Settings** a été supprimé de la page **Dialog**. Vous pouvez maintenant activer la désambiguïsation et la configurer à partir de l'onglet **Options** de la compétence. 

- **Une visite guidée est maintenant disponible** : une visite guidée du produit est désormais affichée lorsqu'une instance de service est créée. Les nouveaux utilisateurs bénéficient également d’une aide au début de leur développement. Un assistant est créé pour eux automatiquement. Des fenêtres contextuelles d’information sont affichées pour présenter les fonctionnalités de l’interface utilisateur du produit et guider le nouvel utilisateur vers la première étape essentielle de la création d’une compétence de dialogue.  

## 10 avril 2019
{: #10April2019}

- **La correction automatique est désormais disponible ![Bêta](images/beta.png)** : la correction automatique est une fonctionnalité bêta qui aide votre assistant à comprendre les souhaits de vos clients. Elle corrige les fautes d'orthographe dans l'entrée que les clients soumettent avant que l'entrée ne soit évaluée. Avec une saisie plus précise, votre assistant peut plus facilement reconnaître les mentions d'entités et comprendre l'intention du client. Pour plus d'informations, reportez-vous à la rubrique [Correction de l'entrée utilisateur](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-spell-check).

## 22 mars 2019
{: #22March2019}

- **Présentation de la compétence de recherche ![Bêta](images/beta.png)** : une compétence de recherche vous permet de rendre votre assistant utile aux clients plus rapidement. Les demandes des clients que vous n'aviez pas prévues et que vous n'avez donc pas construites pour gérer la logique de dialogue peuvent donner lieu à des réponses utiles. Au lieu de dire qu'il ne peut pas aider, l'assistant peut interroger une source de données externe pour trouver des informations pertinentes à partager dans sa réponse. Au fil du temps, vous pouvez créer des réponses de dialogue pour répondre aux questions des clients nécessitant des questions complémentaires afin de clarifier le sens de l'utilisateur ou pour lesquelles une réponse courte et claire convient. Et vous pouvez utiliser les réponses aux compétences de recherche pour répondre à davantage de requêtes de clients ouvertes nécessitant une explication plus longue. Cette fonctionnalité bêta est uniquement disponible pour les utilisateurs des forfaits de services Premium et Plus. ![Forfait Plus ou Premium uniquement](images/plus.png)

  Pour plus d'informations, reportez-vous à la rubrique [Génération d'une compétence de recherche](/docs/services/assistant?topic=assistant-skill-search-add).

## 14 mars 2019
{: #14March2019}

- **Demandez à Watson de vous aider à créer vos intentions** : utilisez la technologie d’apprentissage automatique Watson pour vous aider à choisir les bonnes intentions pour votre assistant. Watson analyse vos données de journal de centre d'appels existantes pour identifier les questions et les demandes des clients les plus fréquentes. Il recommande ensuite les intentions et les exemples utilisateur que vous pouvez utiliser pour former votre assistant afin qu'il puisse reconnaître les demandes identiques et similaires à l'avenir. Une fois que vous avez déterminé les bonnes intentions à utiliser, vous pouvez les augmenter et les maintenir à jour à l'aide de la fonctionnalité de recommandations d'exemple utilisateur d'intention, qui est déjà disponible. Pour plus d'informations, reportez-vous à la rubrique [Obtention d'aide pour définir les intentions](/docs/services/assistant?topic=assistant-intent-recommendations).

## 4 mars 2019 
{: #4March2019}

- **Navigation simplifiée** : la navigation dans la barre latérale avec les onglets séparés *Build*, *Improve* et *Deploy* a été supprimée. Vous pouvez maintenant accéder à tous les outils nécessaires pour créer une compétence de dialogue à partir de la page de compétences principale.

- **La page Improve s'appelle désormais Analytics** : les métriques d'information générées par Watson à partir de conversations entre vos utilisateurs et votre assistant sont passées de l'onglet *Improve* de la barre latérale à un nouvel onglet de la page de compétences principale appelé **Analytics**.

## 1er mars 2019
{: #1March2019}

- **Recommandations sur les exemples utilisateur d'intention en japonais![Forfaits Plus ou Premium uniquement](images/plus.png)** : vous pouvez désormais télécharger un fichier contenant des entrées utilisateur brutes en japonais, telles que des demandes utilisateur dans un journal du centre d’appels, que Watson peut analyser et explorer à la recherche de candidats d'exemples utilisateur d'intention. Reportez-vous à la rubrique [Ajout d’exemples à partir de fichiers journaux](/docs/services/assistant?topic=assistant-intent-recommendations).

## 28 février 2019
{: #28February2019}

- **Nouvelle version de l'API** : la version actuelle de l'API est désormais `2019-02-28`. Les modifications suivantes ont été apportées à cette version :

    - L'ordre dans lequel les conditions sont évaluées dans les noeuds avec attributs a changé. Auparavant, si vous disposiez d'un noeud avec attributs permettant les digressions, le noeud racine `anything_else` était déclenché avant que les conditions Not found de niveau attribut puissent être évaluées. L'ordre des opérations a été modifié pour remédier à ce problème. Désormais, lorsqu'un utilisateur digresse d'un noeud avec attributs, tous les noeuds racine, à l'exception du noeud `anything_else` sont traités. Ensuite, les conditions Not found de niveau attribut sont évaluées. Et, enfin, le noeud de niveau racine `anything_else` est traité. Pour mieux comprendre l'ordre complet des opérations sur un noeud avec attributs, reportez-vous à la rubrique [Conseils relatifs à l'utilisation des attributs](/docs/services/assistant?topic=assistant-dialog-slots#dialog-slots-node-level-handler).

    - Les chaînes commençant par un signe (#) dans les objets `context` ou `output` d'un message ne sont plus traitées comme des références d'intention.
  
      Auparavant, ces chaînes étaient automatiquement traitées comme des intentions. Par exemple, si vous avez spécifié une variable contextuelle, telle que `"color":"#FFFFFF"`, le code de couleur hexadécimal (#FFFFFF) était traité comme une intention. L'assistant vérifiait si une intention nommée #FFFFFF était détectée dans l'entrée utilisateur et, dans le cas contraire, remplaçait #FFFFFF par `false`. Ce remplacement ne se produit plus.
  
      De même, si vous incluiez un signe dièse (#) dans la chaîne de texte d'une réponse de noeud, vous deviez le mettre en échappement en le faisant précéder d'une barre oblique inversée (`\`). Par exemple, `Nous sommes le \#1 des vendeurs de langoustes de l'état du Maine.` Il n'est plus nécessaire de mettre le symbole `#` en échappement dans une réponse textuelle.

      Cette modification ne s'applique pas aux conditions de réponse de noeud ou conditionnelles. Toutes les chaînes commençant par un signe dièse (#) spécifié dans les conditions continuent d'être traitées comme des références d'intention. En outre, vous pouvez utiliser la syntaxe d'expression SpEL pour forcer le système à traiter une chaîne dans les objets `context` ou `output` d'un message comme une intention. Par exemple, spécifiez l'intention `<? #intent-name ?>`. 

## 25 février 2019
{: #25February2019}

**Amélioration de l'intégration à Slack** : vous pouvez maintenant choisir le type d'événement qui déclenchera votre assistant dans un canal Slack. Auparavant, lorsque vous intégriez votre assistant à Slack, celui-ci interagissait avec les utilisateurs via un canal de messagerie directe. Vous pouvez maintenant configurer l’assistant pour qu’il écoute les mentions et réponde s’il est mentionné dans d’autres canaux. Vous pouvez choisir d’utiliser un ou les deux types d’événement comme mécanisme par lequel votre assistant interagit avec les utilisateurs.

## 11 février 2019
{: #11February2019}

**Intégration à Intercom** : plateforme de messagerie de service client de premier plan, Intercom s'est associée à IBM pour ajouter un nouvel agent à l'équipe, un assistant Watson virtuel. Vous pouvez intégrer votre assistant à une application Intercom pour lui permettre de transmettre en toute transparence les conversations utilisateur entre votre assistant et les agents d'assistance. Cette intégration est disponible uniquement pour les utilisateurs des forfaits Plus et Premium. Pour plus d'informations, reportez-vous à la rubrique [Intégration à Intercom](/docs/services/assistant?topic=assistant-deploy-intercom).

## 8 février 2019
{: #8February2019}

- **Gérez les versions de vos compétences** : vous pouvez désormais capturer un instantané des intentions, des entités, du dialogue et des paramètres de configuration d'une compétence à des moments clés du processus de développement. La gestion des versions vous permet d'exploiter toute votre créativité. Vous pouvez déployer de nouvelles approches de conception dans un environnement de test pour les valider avant d'appliquer des mises à jour à un déploiement de production de votre assistant. Pour plus d'informations, reportez-vous à la rubrique [Création de versions de compétences](/docs/services/assistant?topic=assistant-versions).

- **Catalogue de contenu en arabe** : les utilisateurs de compétences en langue arabe peuvent désormais ajouter des intentions prédéfinies à leurs dialogues. Pour plus d'informations, reportez-vous à la rubrique [Utilisation des catalogues de contenu](/docs/services/assistant?topic=assistant-catalog).

## 17 janvier 2019
{: #17January2019}

- **La prise en charge de la langue tchèque est généralement disponible** : la prise en charge de la langue tchèque n'est plus classée en version bêta ; elle est maintenant généralement disponible. Pour plus d'informations, reportez-vous à la rubrique [Langues prises en charge](/docs/services/assistant?topic=assistant-language-support).

- **Améliorations de la prise en charge linguistique** : les composants de compréhension linguistique ont été mis à jour pour améliorer les fonctionnalités suivantes :

  - Entités système allemandes et coréennes
  - Marquage sémantique de classification d'intention pour l'arabe, le néerlandais, le français, l'italien, le japonais, le portugais et l'espagnol

## 4 janvier 2019
{: #4January2019}

- **IBM Cloud Functions sur les sites de Washington, DC et Londres** : vous pouvez désormais appeler par programmation des fonctions IBM Cloud Functions à partir du dialogue d'un assistant dans une instance de service hébergée dans les centres de données de Londres et de Washington, DC. Reportez-vous à la rubrique [Procédure permettant de passer des appels par programmation à partir d'un noeud de dialogue](/docs/services/assistant?topic=assistant-dialog-actions-client).

- ** Nouvelles méthodes de travail avec les tableaux** : les méthodes d'expression SpEL suivantes sont disponibles pour faciliter l'utilisation des valeurs de tableau dans votre dialogue :

  - **JSONArray.filter** : filtre un tableau en comparant chaque valeur du tableau à une valeur pouvant varier en fonction de l'entrée utilisateur.
  - **JSONArray.includesIntent** : vérifie si un tableau `intents` contient une intention particulière.
  - **JSONArray.indexOf** : obtient le numéro d'index d'une valeur spécifique dans un tableau.
  - **JSONArray.joinToArray** : applique le formatage aux valeurs renvoyées par un tableau.

   Pour plus d'informations, reportez-vous à la [documentation sur les méthodes de tableau](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-arrays).

## 13 décembre 2018
{: #13December2018}

- **Centre de données de Londres** : vous pouvez désormais créer des instances de service {{site.data.keyword.conversationshort}} hébergées dans le centre de données de Londres sans syndication. Pour plus d'informations, reportez-vous à la rubrique [Centres de données](/docs/services/assistant?topic=assistant-services-information#services-information-regions).

- **Modification de la limite de noeud de dialogue** : la limite de noeud de dialogue a été temporairement modifiée de 100 000 à 500 pour les nouvelles instances du forfait Standard. Ce changement de limite a ensuite été inversé. Si vous avez créé une instance du forfait Standard au cours de la période pendant laquelle la limite était en vigueur, vos dialogues pourraient être affectés. La limite s'appliquait aux compétences créées entre le 10 et le 12 décembre 2018. Les limites inférieures ont été supprimées de toutes les instances concernées en janvier. Si vous devez augmenter la limite inférieure avant cela, soumettez un ticket de demande de service.

## 1er décembre 2018
{: #1December2018}

   Pour déterminer le nombre de noeuds de dialogue dans une compétence de dialogue, effectuez l'une des opérations suivantes :

   - Dans l'outil, si elle n'est pas déjà associée à un assistant, ajoutez la compétence de dialogue à un assistant, puis affichez la vignette de compétence à partir de la page principale de celui-ci. La section *Données formées* répertorie le nombre de nœuds de dialogue.

   - Envoyez une demande GET au noeud final de l'API /dialog_nodes et incluez le paramètre `include_count=true`. Par exemple :

     ```curl
     curl -u "apikey:{apikey}" "https://gateway.watsonplatform.net/assistant/api/v1/workspaces/{workspace_id}/dialog_nodes?version=2018-09-20&include_count=true"
     ```

     Dans la réponse, l'attribut `total` de l'objet `pagination` contient le nombre de nœuds du dialogue.

     Reportez-vous à la rubrique [Traitement des incidents liés à l'importation de compétences](/docs/services/assistant?topic=assistant-skill-add#skill-add-import-errors) pour plus d'informations sur comment modifier les compétences que vous souhaitez continuer à utiliser.

## 27 novembre 2018
{: #27November2018}

- ** Un nouveau forfait de services, le forfait Plus, est disponible** : ce nouveau forfait offre des fonctionnalités haut de gamme à un prix inférieur. Contrairement aux forfaits précédents, Plus est un forfait de facturation basé sur l'utilisateur. Il mesure l'utilisation en fonction du nombre d'utilisateurs uniques qui interagissent avec votre assistant au cours d'une période donnée. Pour tirer le meilleur parti de ce forfait, si vous créez votre propre application client, concevez-la de sorte qu'elle définisse un ID unique pour chaque utilisateur et transmette l'ID utilisateur avec chaque appel de l'API /message. Pour les intégrations incorporées, l'ID de session est utilisé pour identifier les interactions de l'utilisateur avec l'assistant. Pour plus d'informations, reportez-vous à la rubrique [Forfaits basés sur l'utilisateur](/docs/services/assistant?topic=assistant-services-information#services-information-user-based-plans).

  <table>
  <caption>Limites du forfait Plus</caption>
    <tr>
      <th>Artefact</th>
      <th>Limite</th>
    </tr>
    <tr>
      <td>Assistants</td>
      <td>100</td>
    </tr>
    <tr>
       <td>Entités contextuelles</td>
       <td>20</td>
    </tr>
    <tr>
       <td>Annotations d'entités contextuelles</td>
       <td>2 000</td>
    </tr>
    <tr>
       <td>Noeuds de dialogue</td>
       <td>100 000</td>
    </tr>
    <tr>
       <td>Entités</td>
       <td>1 000</td>
    </tr>
    <tr>
       <td>Synonymes d'entité</td>
       <td>100 000</td>
    </tr>
    <tr>
       <td>Valeurs d'entité</td>
       <td>100 000</td>
    </tr>
    <tr>
       <td>Intentions</td>
       <td>2 000</td>
    </tr>
    <tr>
       <td>Exemples utilisateur d'intentions</td>
       <td>25 000</td>
    </tr>
    <tr>
       <td>Intégrations</td>
       <td>100</td>
    </tr>
    <tr>
       <td>Journaux</td>
       <td>30 jours</td>
    </tr>
    <tr>
       <td>Compétences</td>
       <td>50</td>
    </tr>
  </table>

- **Forfait Premium basé sur l'utilisateur** : le forfait Premium base désormais sa facturation sur le nombre d'utilisateurs uniques actifs. Si vous choisissez d'utiliser ce forfait, concevez toutes les applications personnalisées que vous créez pour identifier correctement les utilisateurs qui génèrent des appels d'API /message. Pour plus d'informations, reportez-vous à la rubrique [Forfaits basés sur l'utilisateur](/docs/services/assistant?topic=assistant-services-information#services-information-user-based-plans).

  Ce changement n'a pas d'incidence sur les instances du service Premium existantes ; elles continuent à utiliser les méthodes de facturation basées sur les API. Seuls les utilisateurs existants du forfait Premium verront le forfait basé sur l'API répertorié comme option du forfait *Premium (API)*.
  {: note}

  Pour plus d'informations sur tous les forfaits de service disponibles, reportez-vous aux {{site.data.keyword.conversationshort}} [options des forfaits de services ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window}.

- **Recommandations sur les exemples utilisateur d'intention ![Forfaits Plus ou Premium uniquement](images/plus.png)** : vous pouvez télécharger un fichier contenant des entrées utilisateur brutes, telles que des demandes utilisateur dans un journal du centre d’appels, que Watson peut analyser et explorer à la recherche de candidats d'exemples utilisateur d'intention. Reportez-vous à la rubrique [Ajout d’exemples à partir de fichiers journaux](/docs/services/assistant?topic=assistant-intent-recommendations#intent-recommendations-get-example-recommendations).

## 20 novembre 2018
{: #20November2018}

**Fin des recommandations** : la section Recommendations de l'onglet Improve a été supprimée. Recommendations était une fonctionnalité bêta réservée aux utilisateurs du forfait Premium. Cette section recommandait aux utilisateurs les mesures permettant d'améliorer leurs données d'apprentissage. Plutôt que d'être centralisées à un seul emplacement, les recommandations sont désormais disponibles dans les portions de l'outil où vous effectuez les modifications des données d'apprentissage. Par exemple, tout en ajoutant des synonymes d'entité, vous pouvez désormais choisir de voir une liste de termes synonymes recommandés par Watson. Si vous recherchez d'autres moyens d'analyser plus en détails vos journaux de conversation des utilisateurs, envisagez d'utiliser les blocs-notes Jupyter. Pour plus d'informations, reportez-vous à la rubrique [Tâches avancées](/docs/services/assistant?topic=assistant-logs-resources).

## 9 novembre 2018
{: #9November2018}

- **Révision majeure de l’interface utilisateur** : le service {{site.data.keyword.conversationshort}} a une nouvelle apparence et de nouvelles fonctionnalités.

  Cette version de l'outil a été évaluée par les participants au programme bêta au cours des derniers mois.

  - **Compétences** : ce que vous conceviez comme un *espace de travail* est maintenant appelé une *compétence*. Une *compétence de dialogue* est un conteneur de données et d'artefacts d'apprentissage du traitement du langage naturel qui permettent à votre assistant de comprendre les questions des utilisateurs et d'y répondre.

    **Où sont mes espaces de travail ?** Tous les espaces de travail que vous avez créés sont désormais répertoriés dans votre instance de service en tant que compétences. Cliquez sur l'onglet **Skills** pour les visualiser. Pour plus d'informations, reportez-vous à la rubrique [Compétences](/docs/services/assistant?topic=assistant-skills).

  - **Assistants** : vous pouvez maintenant publier votre compétence en deux étapes seulement. Ajoutez votre compétence à un assistant, puis configurez une ou plusieurs intégrations avec lesquelles déployer votre compétence. L’assistant ajoute une couche de fonction à votre compétence qui permet à {{site.data.keyword.conversationshort}} d’orchestrer et de gérer automatiquement le flux d’informations. Reportez-vous à la rubrique [Assistants](/docs/services/assistant?topic=assistant-assistants).

  - **Intégrations incorporées** : plutôt que d'accéder à l'onglet **Deploy** pour déployer votre espace de travail, vous ajoutez votre compétence de dialogue à un assistant, puis vous ajoutez des intégrations à l'assistant où la compétence est mise à la disposition de vos utilisateurs. Vous n'avez pas besoin de créer une application frontale personnalisée et de gérer l'état de la conversation d'un appel à l'autre. Cependant, vous pouvez toujours le faire si vous le souhaitez. Pour plus d'informations, reportez-vous à la rubrique [Ajout d'intégrations](/docs/services/assistant?topic=assistant-deploy-integration-add).

  - **Nouvelle version majeure de l'API** : une version V2 de l'API est disponible. Cette version permet d'accéder aux méthodes que vous pouvez utiliser pour interagir avec un assistant au moment de l'exécution. Plus de contexte de transmission avec chaque appel d'API ; l'état de la session est géré automatiquement dans le cadre de la couche assistant.
  
    Ce que les outils présentent comme une compétence de dialogue est en réalité un encapsuleur pour un espace de travail V1. Il n’existe actuellement aucune méthode d’API permettant de créer des compétences et des assistants avec l’API V2. Cependant, vous pouvez continuer à utiliser l'API V1 pour la création d'espaces de travail. Pour plus d'informations, reportez-vous à la rubrique [Présentation des API](/docs/services/assistant?topic=assistant-api-overview).
    {: note}

  - **Changement de source de données** : il est maintenant plus facile d'améliorer le modèle d'une compétence donnée avec les journaux de conversation utilisateur d'une autre compétence. Vous n'avez pas besoin de vous fier aux ID de déploiement, vous pouvez simplement choisir le nom de l'assistant dans lequel une compétence a été ajoutée et déployée pour utiliser ses données. Reportez-vous à la rubrique [Amélioration grâce aux assistants](/docs/services/assistant?topic=assistant-logs#logs-deploy-id).

  La vidéo suivante donne un aperçu de 2 minutes de l'outil {{site.data.keyword.conversationshort}} mis à jour.

  <iframe class="embed-responsive-item" id="youtubeplayer" title="Présentation du produit" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/OkW7gnHrw30?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

  - **Liens d'aperçu des instances de Londres** : si votre instance de service est hébergée à Londres, vous devez modifier l'URL du lien d'aperçu. L'URL comprend un code correspondant à la région où l'instance est hébergée. Dans la mesure où les instances de Londres sont syndiquées à Dallas, vous devez remplacer la référence `eu-gb` dans l'URL par `us-south` afin que la page Web d'aperçu s'affiche correctement.

## 8 novembre 2018
{: #8November2018}

- **Centre de données japonais** : vous pouvez maintenant créer des instances de service {{site.data.keyword.conversationshort}} hébergées dans le centre de données de Tokyo. Pour plus d'informations, reportez-vous à la rubrique [Centres de données](/docs/services/assistant?topic=assistant-services-information#services-information-regions).

## 30 octobre 2018
{: #30October2018}

- **Nouveau processus d'authentification d'API** : le service {{site.data.keyword.conversationshort}} est passé de l'utilisation de Cloud Foundry à une authentification IAM (Identity and Access Management) basée sur des jetons dans les régions suivantes :

  - Dallas (us-south)
  - Francfort (eu-de)

  Pour les nouvelles instances de service, vous utilisez IAM pour l'authentification. Vous pouvez transmettre un jeton bearer ou une clé d'API. Les jetons prennent en charge les demandes authentifiées sans intégrer de données d'identification de service dans chaque appel. Les clés d'API utilisent l'authentification de base.

  Pour toutes les instances de service existantes, vous continuez à utiliser les données d'identification du service (`{username}:{password}`) pour l'authentification.

  Pour plus d'informations, reportez-vous à la rubrique [Authentification des appels d'API](/docs/services/assistant?topic=assistant-services-information#services-information-authenticate-api-calls).

## 25 octobre 2018
{: #25October2018}

- **Les recommandations de synonyme d'entité sont disponibles dans d'autres langues** : la prise en charge de recommandations de synonyme a été ajoutée pour les langues française, japonaise et espagnole.

## 26 septembre 2018
{: #26September2018}

- **{{site.data.keyword.conversationfull}} est disponible dans {{site.data.keyword.icpfull}}** : reportez-vous à la documentation [{{site.data.keyword.icpfull}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/support/knowledgecenter/en/SSBS6K_2.1.0.3/featured_applications/watson_assistant.html) pour plus d'informations.

## 21 septembre 2018
{: #21September2018}

- **Nouvelle version de l'API** : la version actuelle de l'API est désormais `2018-09-20`. Dans cette version, l'attribut `errors[].path` de l'objet d'erreur renvoyé par l'API est exprimé sous la forme d'un [Pointeur JSON ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://tools.ietf.org/html/rfc6901) et non sous forme de notation à points.
- **Prise en charge des actions Web** : vous pouvez désormais appeler des actions Web {{site.data.keyword.openwhisk_short}} à partir d'un noeud de dialogue. Pour plus d'informations, reportez-vous à la rubrique [Procédure permettant de passer des appels de programmation à partir d'un noeud de dialogue](/docs/services/assistant?topic=assistant-dialog-actions-client).

## 15 août 2018
{: #15August2018}

- **Améliorations apportées à la prise en charge de la fonction Fuzzy matching pour les entités** : la fonction Fuzzy matching est entièrement prise en charge pour les entités en anglais et la fonctionnalité de vérification orthographique n'est plus une fonctionnalité uniquement bêta pour de nombreuses autres langues. Pour plus d'informations, reportez-vous à la rubrique [Langues prises en charge](/docs/services/assistant?topic=assistant-language-support).

## 6 août 2018
{: #6August2018}

- **Résolution des conflits d'intentions ![Forfait Plus ou Premium uniquement](images/plus.png)** : l'outil peut maintenant vous aider à résoudre les conflits lorsque deux ou plusieurs exemples d'utilisateur dans des intentions distinctes sont similaires. Les exemples d'utilisateurs non distincts peuvent affaiblir les données d'apprentissage et rendre plus difficile pour l'assistant de mapper les entrées utilisateur sur l'intention appropriée au moment de l'exécution. Pour plus d'informations, reportez-vous à la rubrique [Résolution des conflits d'intentions](/docs/services/assistant?topic=assistant-intents#intents-resolve-conflicts).

- **Désambiguïsation** ![Forfait Plus ou Premium uniquement](images/plus.png) : activez la désambiguïsation pour permettre à votre assistant de demander de l'aide à l'utilisateur quand il doit choisir entre deux noeuds de dialogue viables ou plus à traiter pour une réponse. Pour plus d'informations, reportez-vous à la rubrique [Désambiguïsation](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).

- **Correctif d'accès direct** : correction d'un bogue dans l'outil Dialogs qui vous empêchait de configurer un accès direct vers la réponse d'un noeud avec la condition spéciale `anything_else`.

- **Message de retour de digression** : vous pouvez maintenant spécifier le texte à afficher lorsque l'utilisateur revient à un noeud après une digression. L'utilisateur aura déjà vu l'invite du noeud. Vous pouvez modifier légèrement le message pour que les utilisateurs sachent qu'ils reviennent où ils se sont interrompus. Par exemple, spécifiez une réponse du type `Où en étions-nous ? Ah oui...` Pour plus d'informations, reportez-vous à la rubrique [Digressions](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions).

## 12 juillet 2018
{: #12July2018}

- **Types de réponses enrichies** : vous pouvez désormais ajouter des réponses enrichies à votre dialogue, contenant des éléments tels que des images ou des boutons, ainsi que du texte. Pour plus d'informations, reportez-vous à la rubrique [Réponses enrichies](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia).

- **Entités contextuelles (Bêta)** : les entités contextuelles sont des entités que vous définissez en étiquetant les mentions du type d'entité qui apparaissent dans les exemples d'utilisateur d'intention. Ces types d'entité enseignent à l'assistant non seulement les termes qui peuvent l'intéresser, mais également le contexte dans lequel ces termes apparaissent généralement dans les énoncés utilisateur. Cela permet à l'assistant de reconnaître les mentions d'entités jamais vues auparavant uniquement en fonction de la manière dont elles sont référencées dans l'entrée utilisateur. Par exemple, si vous annotez l'exemple d'utilisateur d'intention "Je veux un vol à destination de Boston" en désignant "Boston" en tant qu'entité @destination, l'assistant peut reconnaître "Chicago" en tant que @destination dans une entrée utilisateur indiquant "Je veux un vol pour Chicago." Cette fonctionnalité est actuellement disponible en anglais uniquement. Pour plus d'informations, reportez-vous à la rubrique [Ajout d'entités contextuelles](/docs/services/assistant?topic=assistant-entities#entities-create-annotation-based).

  Lorsque vous accédez à l'outil avec un navigateur Web Internet Explorer, vous ne pouvez pas étiqueter les mentions d'entités dans les exemples d'utilisateurs d'intention ni modifier le texte de ces exemples.
  {: note}

- **Recommandations d'entité** : Watson peut désormais recommander des synonymes pour vos valeurs d'entité. L'outil de recommandation recherche les synonymes associés en fonction de la similarité contextuelle extraite d'un vaste ensemble d'informations existantes, y compris de grandes sources de texte écrit, et utilise des techniques de traitement de langage naturel pour identifier des mots similaires aux synonymes existants dans votre valeur d'entité. Pour plus d'informations, reportez-vous à la rubrique [Synonymes](/docs/services/assistant?topic=assistant-entities#entities-synonyms).

- **Nouvelle version de l'API** : la version actuelle de l'API est désormais `2018-07-10`. Cette version comprend les modifications suivantes :

  - Le contenu de l'objet `output` /message n'est plus un objet JSON `text` mais un tableau `generic` prenant en charge plusieurs types de réponses enrichies, y compris `image`, `option`, `pause` et `text`.
  - La prise en charge des entités contextuelles a été ajoutée.
  - Vous ne pouvez plus ajouter de propriétés définies par l'utilisateur dans `context.metadata`. Toutefois, vous pouvez les ajouter directement à `context`.

- **Filtre de date de la page Overview** : utilisez les nouveaux filtres de date pour choisir la période d'affichage des données. Ces filtres affectent toutes les données affichées dans la page : non seulement le nombre de conversations affichées dans le graphique, mais également les statistiques affichées avec le graphique, ainsi que les listes des principales intentions et entités. Pour plus d'informations, reportez-vous à la rubrique [Contrôles](/docs/services/assistant?topic=assistant-logs-overview#logs-overview-controls).

- **Extension de la limite de canevas** : lors de l'utilisation de la zone **Patterns** pour [définir des canevas spécifiques pour une valeur d'entité,](/docs/services/assistant?topic=assistant-entities#entities-patterns), le canevas (expression régulière) est désormais limité à 512 caractères.

## 2 juillet 2018
{: #2July2018}

- **Accès directs à partir de réponses conditionnelles** : vous pouvez maintenant configurer une réponse conditionnelle pour accéder directement à un autre noeud. Pour plus d'informations, reportez-vous à la rubrique [Réponses conditionnelles](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multiple).

## 21 juin 2018
{: #21June2018}

- **Mises à jour linguistique des entités système** : la prise en charge des langues néerlandais et chinois simplifié est désormais disponible. La prise en charge de la langue néerlandaise inclut la fonction Fuzzy Matching pour les fautes d’orthographe. La prise en charge du chinois traditionnel inclut la disponibilité d'[entités système](/docs/services/assistant?topic=assistant-system-entities) en version bêta. Pour plus d'informations, reportez-vous à la rubrique [Langues prises en charge](/docs/services/assistant?topic=assistant-language-support).

## 14 juin 2018
{: #14June2018}

- **Ouverture du centre de données de Washington, DC** : vous pouvez maintenant créer des instances de service {{site.data.keyword.conversationshort}} dans le centre de données de Washington, DC. Pour plus d'informations, reportez-vous à la rubrique [Centres de données](/docs/services/assistant?topic=assistant-services-information#services-information-regions).

- **Nouveau processus d'authentification d'API** : le service {{site.data.keyword.conversationshort}} dispose d'un nouveau processus d'authentification d'API pour les instances de service hébergées dans les régions suivantes :

  - Washington, DC (us-east) à compter du 14 juin 2018
  - Sydney, Australie (au-syd) à compter du 7 mai 2018

  {{site.data.keyword.cloud}} migre vers l'authentification IAM (Identity and Access Management) par jeton.

  Pour les nouvelles instances de service dans les régions répertoriées ci-dessus, vous utilisez IAM pour l'authentification. Vous pouvez transmettre un jeton bearer ou une clé d'API. Les jetons prennent en charge les demandes authentifiées sans intégrer de données d'identification de service dans chaque appel. Les clés d'API utilisent l'authentification de base.

  Pour toutes les instances de service nouvelles et existantes dans d'autres régions, vous continuez à utiliser les données d'identification du service (`{username}:{password}`) pour l'authentification.

  Lorsque vous utilisez un kit SDK Watson, vous pouvez transmettre la clé d'API et laisser le kit SDK gérer le cycle de vie des jetons. Pour plus d'informations et des exemples, reportez-vous à [Authentification ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")]https://{DomainName}/apidocs/assistant-v2#authentication){: new_window} dans la référence de l'API.

  Si vous ne savez pas quel type d’authentification utiliser, affichez les données d’identification {{site.data.keyword.conversationshort}} en cliquant sur l’instance de service dans la section Services de la [Liste de ressources {{site.data.keyword.Bluemix_notm}} ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com){: new_window}.

## 25 mai 2018
{: #25May2018}

- **Nouvel exemple d'espace de travail** : l'exemple d'espace de travail qui vous est fourni pour exploration ou utilisation comme point de départ pour votre propre espace de travail a été modifié. L'exemple **Tableau de bord du véhicule** a été remplacé par l'exemple **Service client**. Le nouvel exemple montre comment utiliser les intentions du catalogue de contenu et d'autres fonctionnalités plus récentes pour créer un bot. Il peut répondre à des questions courantes, telles que des demandes d'informations sur les heures et les emplacements des magasins, et explique comment utiliser un noeud avec attributs pour planifier des rendez-vous en magasin.

- **Le rendu HTML a été ajouté au panneau "Try it out"** : le panneau "Try it out" affiche à présent le formatage HTML inclus dans le texte de la réponse. Auparavant, si vous incluiez un lien hypertexte en tant que balise d'ancrage HTML dans une réponse textuelle, vous voyiez la source HTML dans le panneau "Try it out" lors du test. Il ressemblait à ceci :

  `Contactez-nous au site <a href="https://www.ibm.com">ibm.com</a>.`

  A présent, le lien hypertexte est affiché de la même manière que sur une page Web. Il apparaît comme suit :

  `Contactez-nous au site ` [ibm.com](https://www.ibm.com){: new_window}.

    N'oubliez pas que vous devez utiliser le type de syntaxe approprié dans vos réponses pour l'application client sur laquelle vous allez déployer la conversation. Utilisez la syntaxe HTML uniquement si votre application client peut l'interpréter correctement. D'autres canaux d'intégration peuvent prévoir d'autres formats.

- **Modifications du déploiement** : l'option **Test in Slack** a été supprimée.

## 11 mai 2018
{: #11May2018}

- **Sécurité des informations** : la documentation inclut de nouveaux détails sur la confidentialité des données. Vous trouverez plus d'informations dans la rubrique [Sécurité des informations](/docs/services/assistant?topic=assistant-information-security).

## 7 mai 2018
{: #7May2018}

- **Ouverture du centre de données de Sydney, en Australie** : vous pouvez désormais créer des instances de service {{site.data.keyword.conversationshort}} hébergées dans le centre de données de Sydney, en Australie. Pour plus d'informations, reportez-vous à la rubrique [Centres de données globaux IBM Cloud ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe"](https://www.ibm.com/cloud/data-centers/).

## 4 avril 2018
{: #4April2018}

- **Recherche dans des dialogues** : vous pouvez désormais [explorer des noeuds de dialogue](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-search) pour rechercher un mot ou une phrase donnés.

## 15 mars 2018
{: #15March2018}

- **Présentation d'{{site.data.keyword.conversationfull}}** : la conversation {{site.data.keyword.ibmwatson}} a été renommée. Elle s'appelle désormais {{site.data.keyword.conversationfull}}. Le changement de nom reflète le fait que {{site.data.keyword.conversationshort}} a été développé pour fournir un contenu et des outils prédéfinis vous permettant de partager plus facilement les assistants virtuels que vous avez créés. Pour plus d'informations, reportez-vous à la rubrique [cet article ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://www.ibm.com/blogs/watson/2018/03/the-future-of-watson-conversation-watson-assistant/).

- **De nouvelles API REST et de nouveaux SDK sont disponibles pour Watson Assistant** : les nouvelles API sont fonctionnellement identiques aux API de conversation existantes, lesquelles continuent d'être prises en charge. Pour plus d'informations sur les API de Watson Assistant, reportez-vous à la documentation [Référence d'API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/assistant){: new_window}.

- **Améliorations des dialogues** : les fonctionnalités suivantes ont été ajoutées à l'outil de dialogue :

  - Les zones de nom et de valeur de variable simple sont désormais disponibles et peuvent être utilisées pour l'ajout de variables contextuelles ou la mise à jour de valeurs de variable contextuelle. Il n'est pas nécessaire d'ouvrir l'éditeur JSON sauf si vous le souhaitez. Pour plus d'informations, reportez-vous à la rubrique [Définition d'une variable contextuelle](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-var-define).
  - Organisez votre dialogue à l'aide de dossiers pour regrouper les noeuds de dialogue associés. Pour plus d'informations, reportez-vous à la rubrique [Organisation du dialogue à l'aide de dossiers](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-folders).
  - Une prise en charge a été ajoutée pour personnaliser la manière dont chaque noeud de dialogue participe aux digressions initiées par l'utilisateur, en dehors du flux de dialogue désigné. Pour plus d'informations, reportez-vous à la rubrique [Digressions](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions).

- **Recherche dans les intentions et les entités** : une nouvelle fonctionnalité de recherche a été ajoutée pour vous permettre [d'explorer des intentions](/docs/services/assistant?topic=assistant-intents#intents-search) afin de rechercher des exemples utilisateur, des noms d'intention ou des descriptions, ou [d'explorer des valeurs et des synonymes d'entité](/docs/services/assistant?topic=assistant-entities#entities-search).

- **Catalogues de contenu** : les nouveaux [catalogues de contenu](/docs/services/assistant?topic=assistant-catalog#catalog-add) contiennent une seule catégorie d'intentions et d'entités communes prédéfinies que vous pouvez ajouter à votre application. Par exemple, la plupart des applications nécessitent une intention #greeting-type générale qui ouvre un dialogue avec l'utilisateur. Vous pouvez l'ajouter à partir du catalogue de contenu plutôt que de créer la vôtre.

- **Métriques utilisateur améliorées** : le composant Improve a été amélioré avec des métriques utilisateur et des statistiques de journalisation supplémentaires. Par exemple, la page Overview comprend plusieurs nouveaux graphiques détaillés résumant les interactions entre les utilisateurs et votre application, la quantité de trafic pour une période donnée, ainsi que les intentions et les entités qui étaient reconnues le plus souvent dans les conversations utilisateur.

## 12 mars 2018
{: #12March2018}

- **Nouvelles méthodes de date et heure** : des méthodes ont été ajoutées pour faciliter le calcul des dates à partir du dialogue. Pour plus d'informations, reportez-vous à la rubrique [Calculs de date et heure](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-date-and-time-calculations).

## 16 février 2018
{: #16February2018}

- **Traçage de noeud de dialogue** : lorsque vous utilisez le panneau "Try it out" pour tester un dialogue, une icône d'emplacement s'affiche en regard de chaque réponse. Vous pouvez cliquer sur l'icône dans le but de mettre en évidence le chemin parcouru par l'assistant dans l'arborescence de dialogue pour arriver à la réponse. Pour plus d'informations, reportez-vous à la rubrique [Création d'un dialogue](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-test).

- **Nouvelle version d'API** : la version d'API actuelle est désormais `2018-02-16`. Cette version comprend les modifications suivantes :

  - Un nouveau paramètre `include_audit` est désormais pris en charge sur la plupart des demandes GET. Il s'agit d'un paramètre booléen facultatif qui indique si la réponse doit inclure les propriétés d'audit (horodatages `created` et `updated`). La valeur par défaut est
`false`. (Si vous utilisez une version d'API antérieure à `2018-02-16`, la valeur par défaut est `true`.) Pour plus d'informations, reportez-vous à la documentation [Référence d'API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/assistant){: new_window}.

  - Les réponses des appels d'API qui utilisent la nouvelle version incluent uniquement les propriétés ayant des valeurs non `null`.

  - Les propriétés `output.nodes_visited` et `output.nodes_visited_details` des réponses de message incluent désormais des noeuds des types suivants, lesquels étaient auparavant omis :

    - Noeuds avec `type`=`response_condition`
    - Noeuds avec `type`=`event_handler` et `event_name`=`input`

## 9 février 2018
{: #9February2018}

- **Entités de système néerlandaises (Bêta)** : le support de langue néerlandaise a été amélioré pour inclure la disponibilité des [entités de système](/docs/services/assistant?topic=assistant-system-entities) dans une version bêta. Pour plus d'informations, reportez-vous à la rubrique [Langues prises en charge](/docs/services/assistant?topic=assistant-language-support).

## 29 janvier 2018
{: #29January2018}

- L'API REST {{site.data.keyword.conversationshort}} prend désormais en charge de nouveaux paramètres de demande :
  - Utilisez le paramètre `append` lorsque vous mettez à jour un espace de travail pour indiquer si les nouvelles données d'espace de travail ne doivent pas écraser les données existantes mais y être ajoutées. Pour plus d'informations, reportez-vous à la rubrique [Update workspace ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/apidocs/assistant?curl=#update-workspace){: new_window}.
  - Utilisez le paramètre `nodes_visited_details` lorsque vous envoyez un message pour indiquer si la réponse doit inclure d'autres informations de diagnostic sur les noeuds ayant été visités durant le traitement du message. Pour plus d'informations, reportez-vous à la rubrique [Send message ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}.

## 23 janvier 2018
{: #23January2018}

- **Impossible d'extraire une liste d'espaces de travail** : si ce message ou des messages d'erreur similaires s'affichent alors que vous utilisez les outils, il se peut que votre session ait expiré. Déconnectez-vous en choisissant **Log out** à partir de l'icône d'**informations utilisateur** ![Menu de l'icône d'informations utilisateur](images/user-icon.png), puis reconnectez-vous.

## 8 décembre 2017
{: #8December2017}

- **Autoriser l'accès aux données de journal dans les instances (utilisateurs Premium uniquement)** : si vous êtes un utilisateur {{site.data.keyword.conversationshort}} Premium, vos instances premium peuvent éventuellement être configurées pour autoriser l'accès aux données de journal à partir des espaces de travail qu'elles comportent.

- **Copier des noeuds** : vous pouvez désormais dupliquer un noeud afin de réaliser une copie de ce dernier et de ses enfants. Cette fonction est utile si vous créez un noeud avec une logique utile que vous souhaitez réutiliser ailleurs dans votre dialogue. Pour plus d'informations, reportez-vous à la rubrique [Copie d'un noeud de dialogue](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-copy-node).

- **Capturer des groupes dans des entités de canevas** : vous pouvez identifier des groupes dans le canevas d'expression régulière que vous définissez pour une entité. Identifier des groupes est utile si vous souhaitez pouvoir vous référer ultérieurement à une sous-section du canevas. Par exemple, votre entité peut avoir un canevas d'expression régulière qui capture des numéros de téléphone américains. Si vous identifiez le segment d'indicatif régional du canevas de numéro en tant que groupe, vous pouvez ensuite faire référence à ce groupe pour accéder uniquement au segment d'indicatif régional d'un numéro de téléphone. Pour plus d'informations, reportez-vous à la rubrique [Définition d'entités](/docs/services/assistant?topic=assistant-entities#entities-creating-task).

## 6 décembre 2017
{: #6December2017}

- **Intégration {{site.data.keyword.openwhisk}} (Bêta)** : appelez les actions {{site.data.keyword.openwhisk}} (anciennement IBM OpenWhisk) directement à partir d'un noeud de dialogue. Cette fonction vous permet, par exemple, d'appeler une action pour extraire des informations météorologiques à partir d'un noeud de dialogue, puis définir une condition sur les informations renvoyées dans la réponse de dialogue. Actuellement, vous pouvez appeler une action à partir d'une instance {{site.data.keyword.openwhisk_short}} qui est hébergée dans la région Sud des Etats-Unis à partir des instances {{site.data.keyword.conversationshort}} qui sont hébergées dans la région Sud des Etats-Unis. Pour plus d'informations, reportez-vous à la rubrique [Procédure permettant de passer des appels de programmation à partir d'un noeud de dialogue](/docs/services/assistant?topic=assistant-dialog-actions-client).

## 5 décembre 2017
{: #5December2017}

- **Interface utilisateur repensée pour les intentions et les entités** : les onglets `Intents` et `Entities` ont été repensés de manière à fournir un flux de travail plus efficace et plus facile lors de la création et de l'édition d'entités et d'intentions. Pour plus d'informations sur l'utilisation de ces onglets, reportez-vous aux rubriques [Définition d'intentions](/docs/services/assistant?topic=assistant-intents-create-task) et [Définition d'entités](/docs/services/assistant?topic=assistant-entities#entities-creating-task).

## 30 novembre 2017
{: #30November2017}

- **Support des chiffres arabes orientaux** : les chiffres arabes orientaux sont désormais pris en charge dans les entités de système arabes.

## 29 novembre 2017
{: #29November2017}

- **Amélioration de la compréhension des entrées utilisateur dans les espaces de travail** : vous pouvez désormais améliorer un espace de travail avec des énoncés qui ont été envoyés à d'autres espaces de travail au sein de votre instance. Par exemple, si vous possédez plusieurs versions d'espaces de travail de production et d'espaces de travail de développement, vous pouvez utiliser les mêmes données d'énoncé pour améliorer n'importe lequel de ces espaces de travail. Reportez-vous à la rubrique [Amélioration grâce aux espaces de travail](/docs/services/assistant?topic=assistant-logs#logs-deploy-id).

## 20 novembre 2017
{: #20November2017}

- **Conformité à la norme GB18030** : GB18030 est une norme chinoise qui spécifie une page de codes étendue destinée au marché chinois. Cette norme de page de codes est importante pour l'industrie des logiciels car le China National Information Technology Standardization Technical Committee exige que toute application logicielle commercialisée sur le marché chinois après le 1er septembre 2001 respecte la norme GB18030. Le service {{site.data.keyword.conversationshort}} prend en charge ce codage et est certifié conforme à la norme GB18030.

## 9 novembre 2017
{: #9November2017}

- **Les exemples d'intention peuvent référencer directement des entités** : vous pouvez désormais spécifier une référence d'entité directement dans un exemple d'intention. Cette référence d'entité, ainsi que l'ensemble de ses valeurs ou de ses synonymes, est utilisée par le classifieur de service {{site.data.keyword.conversationshort}} pour l'entraînement de l'intention. Pour plus d'informations, reportez-vous à la section [*Référencement direct d'une entité @Entity en tant qu'exemple d'intention*](/docs/services/assistant?topic=assistant-intents#intents-entity-as-example) dans la rubrique [Intentions](/docs/services/assistant?topic=assistant-intents).

  Actuellement, vous pouvez uniquement référencer directement les entités fermées que vous définissez. Vous ne pouvez pas référencer directement les [entités de canevas](/docs/services/assistant?topic=assistant-entities#entities-pattern) ou les [entités de système](/docs/services/assistant?topic=assistant-system-entities).
  {: note}

## 8 novembre 2017
{: #8November2017}

- **Connecteur {{site.data.keyword.conversationshort}}** : vous pouvez utiliser le nouveau connecteur{{site.data.keyword.conversationshort}} pour connecter votre espace de travail à une application Slack ou Facebook Messenger que vous possédez, en la rendant disponible en tant qu'agent conversationnel avec lequel les utilisateurs Slack ou Facebook Messenger peuvent interagir. Cette option est disponible uniquement pour la région {{site.data.keyword.Bluemix_notm}} du Sud des Etats-Unis.

## 3 novembre 2017
{: #3November2017}

- **Mises à jour de dialogue** : les mises à jour suivantes vous facilitent la création d'un dialogue. (Pour plus d'informations, reportez-vous à la rubrique [Création d'un dialogue](/docs/services/assistant?topic=assistant-dialog-build).)

    - Vous pouvez ajouter une condition à un attribut afin de rendre ce dernier obligatoire uniquement dans certaines conditions. Par exemple, vous pouvez créer un attribut qui demande le nom d'une épouse uniquement si un attribut (obligatoire) précédent sollicitant des informations sur l'état civil indique que l'utilisateur est marié.

    - Vous pouvez désormais choisir **Skip user input** comme étape suivante pour un noeud. Lorsque vous choisissez cette option, une fois que le noeud en cours est traité, l'assistant passe directement au premier noeud enfant du noeud en cours. Cette option est semblable à l'option de l'étape suivante *Jump to* existante, à ceci près qu'elle offre davantage de souplesse. Vous n'avez pas besoin de spécifier le noeud exact auquel passer directement. Lors de l'exécution, l'assistant passe toujours directement au noeud qui est le premier noeud enfant, quel qu'il soit, même si les noeuds enfant sont réorganisés ou que de nouveaux noeuds sont ajoutés une fois le comportement d'étape suivant défini.

    - Vous pouvez ajouter des réponses conditionnelles pour des attributs. Pour les réponses Found et Not found, vous pouvez personnaliser la façon dont l'assistant répond selon que certaines conditions sont remplies ou pas. Cette fonction vous permet de rechercher d'éventuelles interprétations erronées et de les corriger avant de sauvegarder la valeur fournie par l'utilisateur dans la variable contextuelle de l'attribut. Par exemple, si l'attribut sauvegarde l'âge de l'utilisateur et utilise `@sys-number` dans la zone *Check for* pour le capturer, vous pouvez ajouter une condition qui recherche les nombres au-dessus de 100 et fournit une réponse semblable à *Please provide a valid age in years.* Pour plus d'informations, reportez-vous à la rubrique [Ajout de conditions aux réponses Found et Not found](/docs/services/assistant?topic=assistant-dialog-slots#dialog-slots-handler-next-steps).

    - L'interface que vous utilisez pour ajouter des réponses conditionnelles à un noeud a été repensée afin d'afficher plus facilement chacune des conditions et la réponse associée. Pour ajouter des réponses conditionnelles de niveau noeud, cliquez sur **Customize**, puis activez l'option **Multiple responses**.

     L'option à bascule **Multiple responses** active ou désactive la fonction pour la réponse de niveau noeud uniquement. Elle ne contrôle pas la possibilité de définir des réponses conditionnelles pour un attribut. Le paramétrage de plusieurs réponses pour un attribut est contrôlé séparément.
     {: note}

    - Afin de conserver la page sur laquelle vous éditez un attribut simple, vous devez désormais sélectionner des options de menu pour a.) ajouter une condition qui doit être remplie pour que l'attribut soit traité, et b.) ajouter des réponses conditionnelles pour les conditions Found et Not found pour un attribut. Sauf si vous choisissez d'ajouter cette fonctionnalité supplémentaire, les zones de condition d'attribut et de réponses multiples ne s'affichent pas, ce qui désencombre la page et la rend plus facile à utiliser.

## 25 octobre 2017
{: #25October2017}

- **Mises à jour apportées au chinois simplifié** : le support de langue a été amélioré pour le chinois simplifié. Cela inclut des améliorations au niveau de la classification des intentions à l'aide d'imbrications de mots au niveau des caractères, et la disponibilité des entités de système. Notez que les modèles d'apprentissage du service {{site.data.keyword.conversationshort}} ont peut-être été mis à jour dans le cadre de cette amélioration, par conséquent, lorsque vous formerez à nouveau votre modèle, les modifications seront appliquées. Pour plus d'informations, reportez-vous à la section [Modèles mis à jour](#release-notes-updated-models).
- **Mises à jour apportées à l'espagnol** : des améliorations ont été apportées à la classification des intentions en espagnol, pour de très grands fichiers.

## 11 octobre 2017
{: #11October2017}

- **Mises à jour apportées au coréen** : le support de langue a été amélioré pour le coréen. Notez que les modèles d'apprentissage du service {{site.data.keyword.conversationshort}} ont peut-être été mis à jour dans le cadre de cette amélioration, par conséquent, lorsque vous formerez à nouveau votre modèle, les modifications seront appliquées. Pour plus d'informations, reportez-vous à la section [Modèles mis à jour](#release-notes-updated-models).

## 3 octobre 2017
{: #3October2017}

- **Entités basées sur des canevas (bêta)** : vous pouvez désormais définir des canevas spécifiques pour une entité, en utilisant des expressions régulières. Cela peut vous permettre d'identifier plus facilement les entités qui suivent un canevas défini, par exemple, une référence SKU ou des numéros de pièces détachées, des numéros de téléphone ou des adresses électroniques. Pour plus d'informations, reportez-vous à la section sur les [entités basées sur des canevas](/docs/services/assistant?topic=assistant-entities#entities-pattern).
    - Vous pouvez ajouter des synonymes ou des canevas pour une valeur d'entité, mais pas les deux.
    - Chaque valeur d'entité peut être associée à 5 canevas au maximum.
    - Chaque canevas (expression régulière) est limité à 128 caractères.
    - Les canevas ne sont actuellement pas pris en charge pour l'importation ou l'exportation d'un fichier CSV.
    - L'API REST ne prend pas en charge l'accès direct aux canevas, mais vous pouvez extraire ou modifier des canevas à l'aide du point final `/values`.

- **Fonction Fuzzy matching filtrée par dictionnaire (anglais uniquement)** : une version améliorée de la fonction Fuzzy Matching pour les entités est désormais disponible, en anglais. Cette amélioration empêche la capture de certains mots anglais valides courants comme correspondances floues pour une entité donnée. Par exemple, la fonction Fuzzy Matching ne fera pas correspondre la valeur d'entité `like` à `hike` ou `bike`, qui sont des mots anglais valides, mais continuera d'établir des correspondances avec des exemples, tels que `lkie` ou `oike`.

## 27 septembre 2017
{: #27September2017}

- **Mises à jour apportées au générateur de condition** : la commande qui vous permet de définir une condition dans un noeud de dialogue a été améliorée. Par exemple, désormais, les noms de variable contextuelle disponibles s'affichent après que le caractère $ est saisi pour commencer à ajouter une variable contextuelle.

## 31 août 2017
{: #31August2017}

- **Rétromigration de la section Improve** : la métrique de temps de conversation médiane, et les filtres correspondants, ont été temporairement retirés de la page Overview dans la section Improve. Ce retrait a pour but d'empêcher que le calcul de certaines métriques n'entraîne l'affichage d'informations inexactes dans la métrique de temps de conversion médiane et le graphique qui représente les conversations sur la durée. IBM déplore d'avoir eu à retirer cette fonctionnalité de l'outil mais elle se doit de faire en sorte que les informations fournies aux utilisateurs soient exactes.
- **Noms de noeud de dialogue** : vous pouvez désormais affecter n'importe quel nom à un noeud de dialogue ; il n'a pas besoin d'être unique. Et vous pouvez modifier le nom de noeud ultérieurement sans que cela n'impacte la façon dont le noeud est référencé en interne. Le nom que vous indiquez est sauvegardé en tant qu'attribut de vignette du noeud dans le fichier JSON de l'espace de travail et le système utilise un ID unique qui est stocké dans l'attribut de nom pour faire référence à ce noeud.

## 23 août 2017
{: #23August2017}

- **Mises à jour apportées au coréen, au japonais et à l'italien** : le support de langue a été amélioré pour le coréen, le japonais et l'italien. Notez que les modèles d'apprentissage du service {{site.data.keyword.conversationshort}} ont peut-être été mis à jour dans le cadre de cette amélioration, par conséquent, lorsque vous formerez à nouveau votre modèle, les modifications seront appliquées. Pour plus d'informations, reportez-vous à la section [Modèles mis à jour](#release-notes-updated-models).

## 10 août 2017
{: #10August2017}

- **Normalisation des accents** : dans un paramétrage conversationnel, les utilisateurs peuvent ou non utiliser des accents lors de l'interaction avec le service {{site.data.keyword.conversationshort}}. Ainsi, une mise à jour a été apportée à l'algorithme de sorte que les versions accentuées et non accentuées des mots puissent être interprétées de la même manière pour la détection d'intention et la reconnaissance d'entité.

  Toutefois, dans certaines langues, telles que l'espagnol, certains accents peuvent modifier le sens de l'entité. Par conséquent, pour la détection d'entité, même si l'entité d'origine comporte implicitement un accent, l'assistant peut aussi établir une correspondance avec la version non accentuée de la même entité, mais avec une cote de confiance légèrement inférieure.

  Par exemple, avec le mot `barrió`, ui est accentué et correspond au participe passé du verbe `barrer` (balayer, en français), l'assistant peut également établir une correspondance avec le mot `barrio` (quartier, en français), mais avec une cote de confiance légèrement inférieure.

  Le système fournit la cote de confiance la plus élevée dans les entités pour lesquelles des correspondances exactes sont trouvées. Par exemple, `barrio` ne sera pas détecté si `barrió` figure dans l'ensemble d'entraînement et `barrió` ne sera pas détecté si `barrio` figure dans l'ensemble d'entraînement.

  Vous êtes supposé entraîner le système à reconnaître les mots avec les caractères et les accents appropriés. Par exemple, si vous vous attendez à recevoir la réponse `barrió`, vous devez ajouter `barrió` dans l'ensemble d’entraînement.

  Bien qu'il ne s'agisse pas d'accents, la même règle s'applique aux mots qui comportent, par exemple, la lettre espagnole `ñ` et non la lettre `n`, par exemple, `uña` et `una`. En l'occurrence, la lettre `ñ` n'est pas juste une lettre `n` dotée d'un accent, mais réellement une lettre spécifique de l'alphabet espagnol.

  Vous pouvez activer la fonction Fuzzy matching si vous pensez que vos clients n'utiliseront pas les accents de manière appropriée ou feront des fautes d'orthographe (par exemple, en tapant un `n` au lieu d'un `ñ`), ou vous pouvez les exclure explicitement des exemples d'entraînement.

  **Remarque :** la normalisation des accents est activée pour le portugais, l'espagnol, le français et le tchèque.

- **Indicateur d'exclusion pour les espaces de travail** : l'API REST {{site.data.keyword.conversationshort}} prend désormais en charge un indicateur d'exclusion pour les espaces de travail. Cet indicateur spécifie que des données d'apprentissage d'espace de travail, telles que des intentions et des entités, ne doivent pas être utilisées par IBM pour les améliorations générales du service. Pour plus d'informations, reportez-vous à la documentation [Référence d'API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/apidocs/assistant?curl=#data-collection){: new_window}

## 7 août 2017
{: #7August2017}

- **Interprétation des dates `Next` et `last`** : le service {{site.data.keyword.conversationshort}} interprète les dates `last` et `next` comme renvoyant au jour suivant ou au jour précédent référencé le plus proche, ce qui peut correspondre à la même semaine ou à une semaine précédente. Pour plus d'informations, reportez-vous à la rubrique sur les [entités de système](/docs/services/assistant?topic=assistant-system-entities#system-entities-sys-datetime).

## 3 août 2017
{: #3August2017}

- **Fonction Fuzzy Matching pour d'autres langues (bêta)** : la fonction Fuzzy Matching pour les entités est désormais disponible pour d'autres langues, comme indiqué dans la rubrique [Langues prises en charge](/docs/services/assistant?topic=assistant-language-support).
- **Fonction Partial Matching (bêta - anglais uniquement)** : la fonction Fuzzy Matching suggère automatiquement des synonymes de type sous-chaîne présents dans les entités définies par l'utilisateur et affecte une cote de confiance inférieure par rapport à la correspondance d'entité exacte. Pour plus d'informations, reportez-vous à la section sur la [fonction Fuzzy matching](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching).

## 28 juillet 2017
{: #28July2017}

- Lorsque vous définissez des préférences bidirectionnelles pour l'outil, vous pouvez désormais spécifier le sens de l'interface graphique.
- Le schéma de couleurs de l'outil a été mis à jour pour être cohérent avec les autres services et produits Watson.

## 19 juillet 2017
{: #19July2017}

- L'API REST {{site.data.keyword.conversationshort}} prend désormais en charge l'accès aux noeuds de dialogue. Pour plus d'informations, reportez-vous à la documentation [Référence d'API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/apidocs/assistant?curl=#list-dialog-nodes){: new_window}.

## 14 juillet 2017
{: #14July2017}

- La fonctionnalité des attributs de dialogue a été améliorée. Par exemple, une propriété nommée *slot_in_focus* a été ajoutée afin de vous permettre de définir une condition qui s'applique à un attribut seulement. Pour plus d'informations, reportez-vous à la section [Collecte d'informations à l'aide d'attributs](/docs/services/assistant?topic=assistant-dialog-slots).

## 12 juillet 2017
{: #12July2017}

- **Prise en charge du tchèque** : le support de la langue tchèque est désormais disponible. Pour plus d'informations, reportez-vous à la rubrique [Langues prises en charge](/docs/services/assistant?topic=assistant-language-support).

## 11 juillet 2017
{: #11July2017}

- **Option Test in Slack** : vous pouvez utiliser la nouvelle option **Test in Slack** pour déployer rapidement votre espace de travail en tant qu'utilisateur bot Slack à des fins de test. Cette option est disponible uniquement pour la région {{site.data.keyword.Bluemix_notm}} du Sud des Etats-Unis.
- **Mises à jour apportées à l'arabe** : le support de la langue arabe a été amélioré et comprend désormais l'option d'évaluation absolue pour chaque intention (Absolute scoring) ainsi que l'option permettant de marquer des intentions comme non pertinentes (Mark as irrelevant). Pour plus d'informations, reportez-vous à la rubrique [Langues prises en charge](/docs/services/assistant?topic=assistant-language-support). Notez que les modèles d'apprentissage du service {{site.data.keyword.conversationshort}} ont peut-être été mis à jour dans le cadre de cette amélioration, par conséquent, lorsque vous formerez à nouveau votre modèle, les modifications seront appliquées. Pour plus d'informations, reportez-vous à la section [Modèles mis à jour](#release-notes-updated-models).

## 23 juin 2017
{: #23June2017}

- **Mises à jour apportées au coréen** : le support de la langue coréenne a été amélioré. Pour plus d'informations, reportez-vous à la rubrique [Langues prises en charge](/docs/services/assistant?topic=assistant-language-support). Notez que les modèles d'apprentissage du service {{site.data.keyword.conversationshort}} ont peut-être été mis à jour dans le cadre de cette amélioration, par conséquent, lorsque vous formerez à nouveau votre modèle, les modifications seront appliquées. Pour plus d'informations, reportez-vous à la section [Modèles mis à jour](#release-notes-updated-models).

## 22 juin 2017
{: #22June2017}

- **Lancement des attributs** : la collecte de plusieurs éléments d'informations auprès d'un utilisateur dans un seul noeud est désormais facilitée par l'ajout d'attributs. Auparavant, vous deviez créer plusieurs noeuds de dialogue pour couvrir toutes les combinaisons possibles de spécification d'informations par les utilisateurs. Grâce aux attributs, vous pouvez configurer un seul noeud qui sauvegarde les informations fournies par l'utilisateur et invite ce dernier à fournir les informations manquantes. Pour plus d'informations, reportez-vous à la section [Collecte d'informations à l'aide d'attributs](/docs/services/assistant?topic=assistant-dialog-slots).
- **Arborescence de dialogue simplifiée** : l'arborescence de dialogue a été repensée afin de la rendre plus facile à utiliser. La vue de l'arborescence est plus compacte, ce qui vous permet de voir plus facilement où vous vous trouvez dans cette arborescence. Et, les liens entre les noeuds sont représentés de telle manière que les relations entre les noeuds sont plus faciles à comprendre.

## 21 juin 2017
{: #21June2017}

- **Prise en charge de l'arabe** : le support de la langue arabe est désormais officiellement disponible. Pour plus d'informations, reportez-vous à la section [Configuration des langues bidirectionnelles](/docs/services/assistant?topic=assistant-language-support#language-support-configuring-bi-directional).
- **Mises à jour apportées à toutes les langues** : les algorithmes du service {{site.data.keyword.conversationshort}} ont été mis à jour de manière à améliorer l'ensemble du support de langue. Pour plus d'informations, reportez-vous à la rubrique [Langues prises en charge](/docs/services/assistant?topic=assistant-language-support).

## 16 juin 2017
{: #16June2017}

- **Page Recommendations (Bêta - utilisateur Premium uniquement)** : le panneau Improve inclut également une page **Recommendations** qui vous propose des méthodes pour améliorer votre système en analysant les conversations entre les utilisateurs et votre agent conversationnel et en tenant compte des données d'apprentissage et du degré de certitude des réponses actuels de votre système.

## 14 juin 2017
{: #14June2017}

- **Fonction Fuzzy Matching pour d'autres langues (bêta)** : la fonction Fuzzy Matching pour les entités est désormais disponible pour d'autres langues, comme indiqué dans la rubrique [Langues prises en charge](/docs/services/assistant?topic=assistant-language-support). Vous pouvez activer la fonction Fuzzy Matching par entité afin d'améliorer la capacité de l'assistant à reconnaître les termes dans les entrées utilisateur dont la syntaxe est similaire à l'entité, mais pas forcément identique. La fonction est capable de mapper une entrée utilisateur à l'entité correspondante en dépit de la présence de fautes d'orthographe ou de légères différences de syntaxe. Par exemple, si vous définissez giraffe comme synonyme d'une entité d'animal et que l'entrée utilisateur contient les termes giraffes ou girafe, la fonction Fuzzy Matching est capable de mapper correctement le terme à l'entité d'animal. Pour plus d'informations, reportez-vous à la section sur la [fonction Fuzzy matching](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching).

## 13 juin 2017
{: #13June2017}

- **Page User conversations** : le panneau Improve comprend désormais une page intitulée **User conversations**, qui fournit une liste d'interactions utilisateur avec votre agent conversationnel pouvant être filtrées par mot clé, intention, entité ou nombre de jours. Vous pouvez ouvrir des conversations individuelles pour corriger des intentions ou pour ajouter des valeurs ou des synonymes d'entité.
- **Modification relative aux expressions régulières** : les expressions régulières qui sont prises en charge par des fonctions SpEL, telles que find, matches, extract, replaceFirst, replaceAll et split, ont été modifiées. Un groupe de constructions d'expressions régulières n'est plus autorisé, notamment, les constructions d'assertion avant, d'assertion arrière, de répétition possessive et de référence arrière. Cette modification a pour but d'éviter un risque lié à la sécurité dans la bibliothèque d'expression régulière d'origine.

## 12 juin 2017
{: #12June2017}

- Le nombre maximal d'espaces de travail que vous pouvez créer à l'aide du forfait **Lite** (anciennement appelé forfait gratuit) est passé de 3 à 5.
- Vous pouvez désormais affecter n'importe quel nom à un noeud de dialogue ; il n'a pas besoin d'être unique. Et vous pouvez modifier le nom de noeud ultérieurement sans que cela n'impacte la façon dont le noeud est référencé en interne. Le nom que vous spécifiez est interprété comme un alias et le système utilise son propre identificateur interne pour faire référence au noeud.
- Vous ne pouvez plus modifier la langue d'un espace de travail après la création de celui-ci en éditant ses caractéristiques. Si vous devez modifier la langue, vous pouvez exporter l'espace de travail en tant que fichier JSON, mettre à jour la propriété de langue, puis importer le fichier JSON en tant que nouvel espace de travail.

## 6 juin 2017
{: #6June2017}

- **A propos du service** : une nouvelle page intitulée *Learn about {{site.data.keyword.conversationfull}}* est disponible. Elle fournit des informations d'initiation et des liens vers la documentation du service et d'autres ressources utiles. Pour ouvrir la page, cliquez sur l'icône ![i pour informations.](images/info.png) dans l'en-tête de page.
- **Exportation et suppression en bloc** : vous pouvez désormais exporter simultanément plusieurs intentions ou entités vers un fichier CSV, afin de pouvoir les importer et les réutiliser pour une autre application {{site.data.keyword.conversationshort}}. Vous pouvez également sélectionner simultanément plusieurs entités ou intentions pour une suppression en bloc.
- **Mises à jour apportées au coréen** : les marqueurs sémantiques coréens ont été mis à jour pour assurer le support du langage familier. IBM continue de s'employer à améliorer la reconnaissance et la classification d'entités.
- **Prise en charge d'émoticônes** : les émoticônes ajoutées aux exemples d'intention ou en tant que valeurs d'entité seront désormais correctement classifiées/extraites.

  Seules les émoticônes qui sont incluses dans vos données d'apprentissage seront correctement et systématiquement identifiées ; il se peut que des émoticônes similaires ayant des teintes de couleur différentes ou présentant d'autres variations ne soient pas correctement classifiées.
  {: note}

- **Fonction Stemming pour les entités (bêta - anglais uniquement)** : la fonction bêta Fuzzy Matching reconnaît des entités et établit des correspondances en fonction du format de réduction au radical de la valeur d'entité. Par exemple, cette fonction reconnaît correctement 'bananes' comme étant similaire à 'banane' et 'run' comme étant similaire à 'running' car ils ont le même radical. Pour plus d'informations, reportez-vous à la rubrique sur la [fonction Fuzzy Matching](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching).
- **Progression de l'importation d'espace de travail** : lorsque vous importez un espace de travail à partir d'un fichier JSON, une vignette représentant l'espace de travail et contenant des informations sur la progression de l'importation s'affiche immédiatement.
- **Temps d'entraînement réduit** : plusieurs modèles sont désormais formés en parallèle, ce qui réduit considérablement le temps d'entraînement pour les grands espaces de travail.

## 26 mai 2017
{: #26May2017}

- La version d'API actuelle est désormais `2017-05-26`. Cette version comprend les modifications suivantes :
    - Le schéma des objets ErrorResponse a changé. Cette modification a un impact sur tous les noeuds finaux et sur toutes les méthodes. Pour plus d'informations, reportez-vous à la documentation [Référence d'API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/assistant){: new_window}.
    - Le schéma interne utilisé pour représenter les noeuds de dialogue dans le fichier JSON d'un espace de travail exporté a changé. Si vous utilisez l'API `2017-05-26` pour importer un espace de travail qui a été exporté à l'aide d'une version antérieure, l'importation de certains noeuds de dialogue peut ne pas aboutir. Pour optimiser vos résultats, importez toujours un espace de travail en utilisant la même version que celle qui a servi à l'exportation de cet espace de travail.

## 25 mai 2017
{: #25May2017}

- Vous pouvez désormais gérer des variables contextuelles dans le panneau "Try it out". Cliquez sur le lien **Manage context** pour ouvrir un nouveau panneau dans lequel vous pouvez définir et vérifier les valeurs des variables contextuelles lorsque vous testez le dialogue. Pour plus d'informations, reportez-vous à la section [Test de votre dialogue](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-test).

## 16 mai 2017
{: #16May2017}

- Un exemple d'espace de travail de tableau de bord de voiture, intitulé **Car Dashboard**, est désormais disponible lorsque vous ouvrez l'outil. Pour utiliser cet exemple comme point de départ pour votre propre espace de travail, éditez-le. En revanche, si vous souhaitez l'utiliser pour plusieurs espaces de travail, dupliquez-le. L'exemple d'espace de travail n'est pas pris en compte dans le nombre limite d'espaces de travail tant que vous ne l'utilisez pas.
- Il est désormais plus facile de naviguer dans l'outil. Les options du menu de navigation sont disponibles sur le côté et non plus dans la partie supérieure de la page principale. La partie supérieure de la page contient des liens d'élément de navigation qui vous indiquent où vous vous trouvez. Vous pouvez désormais basculer entre les instances de service à partir de la page Workspaces. Pour vous y rendre rapidement, cliquez sur **Back to workspaces** dans le menu de navigation. Si vous avez plusieurs instances de service, le nom de l'instance en cours est affiché. Vous pouvez cliquer sur le lien **Change** à côté de ce nom pour choisir une autre instance.
- Lorsque vous créez un dialogue, celui-ci comporte désormais deux noeuds qui ont été ajoutés pour vous : 1) le noeud **Welcome** en haut de l'arborescence du dialogue qui contient le message d'accueil à afficher pour l'utilisateur et 2) le noeud **Anything else** au bas de l'arborescence qui intercepte et répond à toutes les demandes utilisateur qui ne sont pas reconnues par les autres noeuds du dialogue. Pour plus d'informations, reportez-vous à la rubrique [Création d'un dialogue](/docs/services/assistant?topic=assistant-dialog-build).
- Lorsque vous testez un dialogue dans le panneau "Try it out", vous pouvez désormais rechercher et soumettre à nouveau un énoncé de test récent en appuyant sur la touche de déplacement vers le haut pour faire défiler les entrées précédentes.
- Le support expérimental de 5 entités de système (`@sys-date`, `@sys-time`, `@sys-currency`, `@sys-number`, `@sys-percentage`) pour la langue coréenne est désormais disponible. Il existe des problèmes connus pour certaines des entités numériques, et un support limité pour les entrées en langage familier.
- Une page Overview est disponible sur l'onglet Improve. La page fournit un récapitulatif des interactions avec votre bot. Vous pouvez visualiser le volume de trafic pour une période donnée, ainsi que les intentions et les entités qui ont été le plus souvent reconnues dans des conversations utilisateur. Pour plus d'informations, reportez-vous à la rubrique [Utilisation de la page Overview](/docs/services/assistant?topic=assistant-logs-overview).

## 27 avril 2017
{: #27April2017}

- Les entités de système suivantes sont désormais disponibles en tant que fonctions bêta en anglais uniquement :
    - sys-location : reconnaît des références à des lieux, tels que des villes, des agglomérations et des pays, dans des énoncés utilisateur.
    - sys-person : reconnaît des références à des noms de personne (nom de famille et prénom) dans des énoncés utilisateur.

    Pour plus d'informations, reportez-vous à la rubrique de référence sur les [entités de système](/docs/services/assistant?topic=assistant-system-entities).
- La fonction Fuzzy Matching pour les entités est une fonction bêta qui est désormais disponible en anglais. Vous pouvez activer la fonction Fuzzy Matching par entité afin d'améliorer la capacité de l'assistant à reconnaître les termes dans les entrées utilisateur dont la syntaxe est similaire à l'entité, mais pas forcément identique. La fonction est capable de mapper une entrée utilisateur à l'entité correspondante en dépit de la présence de fautes d'orthographe ou de légères différences de syntaxe. Par exemple, si vous définissez **giraffe** comme synonyme d'une entité d'animal et que l'entrée utilisateur contient les termes *giraffes* ou *girafe*, la fonction Fuzzy Matching est capable de mapper correctement le terme à l'entité d'animal. Pour plus d'informations, reportez-vous à la section portant sur la fonction `Fuzzy Matching` dans la rubrique [Définition d'entités](/docs/services/assistant?topic=assistant-entities#entities-fuzzy-matching).

## 18 avril 2017
{: #18April2017}

- L'API REST {{site.data.keyword.conversationshort}} prend désormais en charge l'accès aux ressources suivantes :
    - entités
    - valeurs d'entité
    - synonymes de valeur d'entité
    - journaux

    Pour plus d'informations, reportez-vous à la documentation [Référence d'API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/assistant){: new_window}.
- Le comportement de la méthode `POST` /messages a modifié le traitement des entités et des intentions spécifiées dans le cadre de l'entrée du message :
    - Si vous spécifiez des intentions dans une entrée, l'assistant utilise ces intentions, mais il fait appel au traitement du langage naturel pour détecter des entités dans l'entrée utilisateur.
    - Si vous spécifiez des entités dans une entrée, l'assistant utilise ces entités, mais il fait appel au traitement du langage naturel pour détecter des intentions dans l'entrée utilisateur.

        Le comportement n'a pas été modifié pour les messages qui spécifient à la fois des intentions et des entités ou pour les messages qui n'en spécifient aucune.
- L'option permettant de marquer l'entrée utilisateur comme non pertinente (Mark as irrelevant) est désormais disponible pour toutes les langues prises en charge. Il s'agit d'une fonction bêta.
- Un nouvel onglet, nommé Credentials, constitue un espace dans lequel sont regroupées toutes les informations dont vous avez besoin pour connecter votre application à un espace de travail (par exemple, les données d'identification {{site.data.keyword.conversationshort}} et l'ID de l'espace de travail), ainsi que d'autres options de déploiement. Pour accéder à l'onglet Credentials pour votre espace de travail, cliquez sur l'icône ![Menu](images/Menu_16.png) et sélectionnez **Credentials**.

## 9 mars 2017
{: #9March2017}

L'API REST {{site.data.keyword.conversationshort}} prend désormais en charge l'accès aux ressources suivantes :

- espaces de travail
- intentions
- exemples
- contre-exemples

Pour plus d'informations, reportez-vous à la documentation [Référence d'API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://{DomainName}/apidocs/assistant){: new_window}.

## 7 mars 2017
{: #7March2017}

- L'utilisation de `.` ou de `..` comme nom d'intention peut entraîner des problèmes et n'est plus prise en charge.

    Vous ne pouvez pas renommer ou supprimer une intention portant ce nom ; pour changer le nom, exportez votre intention dans un fichier, renommez-la dans le fichier, puis importez le fichier ainsi modifié dans votre espace de travail.

    Les clients payants peuvent contacter le support pour un changement de base de données.

## 1 mars 2017
{: #1March2017}

- Les entités de système sont désormais activées en allemand.

## 22 février 2017
{: #22February2017}

- Les messages sont désormais limités à 2 048 caractères.

## 3 février 2017
{: #3February2017}

- Nous avons modifié la façon dont les intentions sont notées et ajouté la fonction de marquage des entrées comme non pertinentes à votre application. Pour plus d'informations, reportez-vous à la section portant sur l'option `Mark as irrelevant` dans la rubrique [Définition d'intentions](/docs/services/assistant?topic=assistant-intents).

- Cette édition a introduit une modification majeure dans l'espace de travail. Pour bénéficier de cette modification, vous devez mettre à niveau manuellement votre espace de travail.

- Le traitement des actions **Jump to** a été modifié pour empêcher que des boucles ne se produisent dans certaines conditions. Auparavant, si vous passiez directement à la condition d'un noeud et que ni ce noeud ni aucun de ses noeuds homologues n'avaient une condition pour laquelle la valeur true avait été renvoyée, le système accédait directement au noeud de niveau racine et recherchait un noeud dont la condition correspondait à l'entrée. Dans certaines situations, ce traitement créait une boucle, ce qui empêchait le dialogue de progresser.

  Dans le cadre du nouveau processus, si le noeud cible et ses homologues ne renvoient pas la valeur true, l'échange de dialogue prend fin. Si vous souhaitez réimplémenter l'ancien modèle, ajoutez un noeud homologue final avec une condition `true`. Dans la réponse, utilisez une action **Jump to** qui cible la condition du premier noeud au niveau racine de l'arborescence de votre dialogue.

## 11 janvier 2017
{: #11January2017}

- Dans cette édition, vous pouvez personnaliser les titres de noeud dans un dialogue.

## 22 décembre 2016
{: #22December2016}

- Dans cette édition, les noeuds de dialogue comprennent désormais une nouvelle section contenant `leur titre`. Le `titre d'un noeud` n'est pas personnalisable. Lorsque le noeud de dialogue est réduit, sa `condition` est indiquée comme `titre`. Si le noeud ne comporte pas de `condition`, la mention "Untitled Node" apparaît en guise de titre.

## 19 décembre 2016
{: #19December2016}

Plusieurs modifications ont été apportées à l'éditeur de dialogue afin de le rendre plus intuitif et facile à utiliser :

- Une vue d'édition élargie facilite l'affichage de tous les détails d'un noeud pendant que vous le gérez.
- Un noeud peut contenir plusieurs réponses, chacune d'elles étant déclenchées par une condition distincte. Pour plus d'informations, reportez-vous à la section sur les [réponses multiples](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-responses).

## 5 décembre 2016
{: #5December2016}

- Les nouvelles langues suivantes sont prises en charge, toutes en mode expérimental : allemand, chinois traditionnel, chinois simplifié et néerlandais.
- Les deux nouvelles entités de système suivantes sont disponibles : @sys-date et @sys-time. Pour plus d'informations, reportez-vous à la rubrique sur les [entités de système](/docs/services/assistant?topic=assistant-system-entities).

## 21 octobre 2016
{: #21October2016}

- Le service {{site.data.keyword.conversationshort}} fournit désormais des entités de système ; il s'agit d'entités communes que vous pouvez utiliser pour n'importe quel scénario d'utilisation. Pour plus d'informations, reportez-vous à la section `Activation des entités de système` dans la rubrique [Définition d'entités](/docs/services/assistant?topic=assistant-entities).
- Vous pouvez désormais afficher un historique des conversations avec les utilisateurs sur la page Improve. Cela doit vous permettre de comprendre le comportement de votre bot. Pour plus d'informations, reportez-vous à la rubrique [Amélioration de votre compétence](/docs/services/assistant?topic=assistant-logs).
- Vous pouvez désormais importer des entités à partir d'un fichier CSV, ce qui peut être utile si vous avez beaucoup d'entités. Pour plus d'informations, reportez-vous à la section `Importation d'entités` dans la rubrique [Définition d'entités](/docs/services/assistant?topic=assistant-entities).

## 20 septembre 2016
{: #20September2016}

**Nouvelle version** : 2016-09-20

Pour bénéficier des modifications spécifiques d'une nouvelle version, remplacez la valeur du paramètre `version` par la nouvelle date. Si vous n'êtes pas prêt à passer à cette version, ne modifiez pas la date de votre version.

- Version **2016-09-20** : `dialog_stack`, qui était un tableau de chaînes, a été remplacé par un tableau d'objets JSON.

## 29 août 2016
{: #29August2016}

- Vous pouvez déplacer des noeuds de dialogue d'une branche vers une autre, en tant que noeuds de même niveau ou noeuds homologues. Pour plus d'informations, reportez-vous à la rubrique [Déplacement d'un noeud de dialogue](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-move-node).
- Vous pouvez développer la fenêtre d'édition JSON.
- Vous pouvez visualiser les journaux de discussion relatifs aux conversations de votre bot afin de mieux comprendre son comportement. Vous pouvez filtrer l'affichage par intentions, entités, date et heure. Pour plus d'informations, reportez-vous à la rubrique [Amélioration de votre compétence](/docs/services/assistant?topic=assistant-logs)

## 11 juillet 2016
{: #21July2016}

- Cette édition officiellement disponible vous permet de gérer des entités et des dialogues afin de créer un bot totalement opérationnel.

## 18 mai 2016
{: #18May2016}

- Cette édition expérimentale de {{site.data.keyword.conversationshort}} introduit l'interface utilisateur et vous permet de gérer des espaces de travail, des intentions et des exemples.
