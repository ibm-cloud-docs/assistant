---

copyright:
  years: 2015, 2019
lastupdated: "2019-05-28"

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

# Intégration à un widget de discussion hébergé sur le Web
{: #deploy-web-link}

Si vous ne désactivez pas le lien d'aperçu, l'assistant est immédiatement disponible pour test à partir d'une page Web.
{: shortdesc}

L’assistant est implémenté comme un widget de discussion intégré automatiquement dans une simple page Web de marque IBM. Vous pouvez tester la compétence de dialogue que vous avez ajoutée à l'assistant en saisissant du texte dans le widget de discussion. Vous pouvez également partager l'URL de la page avec d'autres personnes afin de vous aider à tester et à obtenir des commentaires sur l'assistant.

Contrairement aux tests effectués à l'aide du panneau "Try it out", tous les appels d'API résultant de vos interactions avec l'assistant hébergé par l'URL Preview Link sont facturés. 

## Utilisation de l’intégration Preview Link pour tester votre assistant
{: #deploy-web-link-try}

Pour tester l'assistant à partir d'un widget de discussion hébergé sur le Web, procédez comme suit :

1.  Dans l'onglet Assistants, cliquez pour ouvrir la vignette de l'assistant que vous souhaitez tester.

1.  Dans la section *Integrations*, cliquez sur la vignette **Preview Link**.

    Si vous n'avez pas activé le lien d'aperçu lors de la création de l'assistant, cliquez sur **Add integration**, puis cliquez sur la vignette d'intégration **Preview Link**.

1.  **Facultatif** : modifiez le nom et la description de la page Web d’aperçu.

1.  Cliquez sur le lien URL affiché pour ouvrir la page de test.

    Un onglet de navigateur Web distinct s'ouvre et contient une implémentation de widget de discussion de votre assistant.

    Si votre instance de service a été créée dans le centre de données du Royaume-Uni avant le 13 décembre 2018 ou à Sydney avant le 7 mai 2018, vous devez modifier l'URL du lien d'aperçu. L'URL comprend un code d'emplacement pour le centre de données où l'instance est hébergée. Les instances de Londres et de Sydney ayant été syndiquées à Dallas avant les dates spécifiées, vous devez remplacer la référence `eu-gb` ou `au-syd` dans l'URL par `us-south` pour que la page Web s'affiche correctement.
    {: important}

1.  Soumettez les énoncés de test pour voir comment l’assistant répond.

    Aucune réponse n'est renvoyée avant que vous ayez créé une compétence de dialogue et l'ayez ajoutée à l'assistant.

    Le flux de dialogue pour la session en cours est redémarré après 60 minutes d'inactivité (5 minutes pour les forfaits Lite et Standard). Cela signifie que si un utilisateur cesse d'interagir avec l'assistant, au bout de 60 (ou 5) minutes, toutes les valeurs de variable contextuelles définies lors de la conversation précédente sont définies sur null ou sur leurs valeurs par défaut.

1.  Après les tests, vous pouvez fermer l'onglet du navigateur pour quitter la page Web publique.

1.  Cliquez sur **Save Changes** pour enregistrer les modifications apportées à l'intégration du lien d'aperçu et fermer la page ou cliquez sur **X** pour fermer la page sans enregistrer.

## Considérations sur les dialogues
{: #deploy-web-link-dialog}

Les réponses enrichies que vous ajoutez à un dialogue sont affichées dans le widget de discussion hébergé sur le Web comme prévu, avec les exceptions suivantes :

- **Connect to human agent** : ce type de réponse est ignoré.

- **Pause** : ce type de réponse met en pause l'activité de l'assistant dans le widget de discussion. Cependant, l'activité ne reprend pas après la pause, sauf si une autre réponse est déclenchée. Chaque fois que vous incluez un type de réponse `pause`, ajoutez un autre type de réponse, tel que `text`, après celui-ci.

Pour plus d'informations sur les types de réponse, reportez-vous à la rubrique [Réponses enrichies](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia).

## Ajout d’une intégration Preview Link
{: #deploy-web-link-add-more}

Si vous avez accidentellement supprimé l'intégration de lien d'aperçu créée automatiquement pour vous ou si vous souhaitez créer une autre page Web publique distincte, vous pouvez ajouter une intégration de lien d'aperçu.

1.  Dans l'onglet Assistant, cliquez pour ouvrir la vignette de l'assistant que vous souhaitez déployer.

1.  Accédez à la section **Integrations**, puis cliquez sur **Add integration**.

1.  Cliquez sur la vignette d'intégration **Preview Link**.

1.  Modifiez éventuellement le nom et la description de l'intégration du lien d'aperçu, puis cliquez sur **Create**.

    Une URL est ajoutée à la page. Cliquez dessus pour ouvrir la page Web d'aperçu.
