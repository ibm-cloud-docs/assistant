---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-24"

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

# Utilisation de l'option Test in Slack

Vous pouvez utiliser l'outil de déploiement de test pour intégrer votre espace de travail {{site.data.keyword.conversationshort}} dans une équipe Slack en tant qu'utilisateur bot. Utilisez cette méthode si vous souhaitez effectuer rapidement un test en utilisant un bot Slack en tant qu'interface utilisateur pour votre espace de travail.

L'outil de déploiement de test utilise le service {{site.data.keyword.openwhisk}} pour déployer une application Slack préconfigurée sur votre équipe en tant qu'utilisateur bot. Cette application gère la communication avec vos espaces de travail {{site.data.keyword.conversationshort}}. 

![Diagramme de présentation du déploiement de test](images/testdeploy_diagram.png)

Notez que l'outil de déploiement de test est soumis à certaines limitations :

- Vous ne pouvez pas utiliser cet outil dans le but de publier une application pour qu'elle soit utilisée par d'autres équipes.
- Si vous utilisez cette méthode pour déployer plus d'un espace de travail sur la même équipe, tous les espaces de travail répondront au nom d'utilisateur `@ibmwatson_bot`. Il est recommandé d'utiliser cet outil pour déployer un seul espace de travail à la fois sur chaque équipe Slack.
- Vous devez disposer des droits nécessaires pour installer des applications à votre équipe Slack. Adressez-vous à votre administrateur Slack si vous n'êtes pas certain de posséder ces droits.
- L'application Slack préconfigurée doit être utilisée exclusivement pour des tests, et il se peut qu'elle ne soit pas disponible tout le temps.
- En raison des restrictions relatives à {{site.data.keyword.openwhisk_short}}, cet outil est actuellement disponible uniquement pour la région {{site.data.keyword.Bluemix_notm}} du Sud des Etats-Unis.

Pour installer votre application en tant qu'utilisateur bot :

1. Dans l'outil {{site.data.keyword.conversationshort}}, ouvrez l'espace de travail que vous souhaitez tester dans Slack.
1. Cliquez sur l'icône de menu dans l'angle supérieur gauche, puis sélectionnez **Deploy**. La page Deploy Options s'affiche.

   ![Option de menu Deploy](images/deploy_menu_testdeploy.png)

1. Sous **Deploy with {{site.data.keyword.openwhisk_short}}**, cliquez sur **Test in Slack** et suivez les instructions qui s'affichent.

   ![Bouton Test in Slack](images/testdeploy_testinslack.png)

## Discussion avec le bot

Une fois le processus de déploiement terminé, vous pouvez utiliser le nom d'utilisateur `@ibmwatson_bot` pour interagir avec votre espace de travail {{site.data.keyword.conversationshort}}, comme vous le feriez avec n'importe quel autre bot Slack.

Gardez à l'esprit que le bot déployé sur votre équipe conserve l'état de chaque utilisateur au sein d'un canal particulier. Cela signifie que les variables éventuellement stockées dans le contexte du dialogue sont conservées indéfiniment, sauf si votre dialogue les efface.

Si vous avez besoin de pouvoir réinitialiser la conversation avec un état de début connu, vous devez le faire au sein de votre dialogue. Assurez-vous que votre dialogue possède un noeud qui est exécuté à la fin de la conversation ou au moment (quel qu'il soit) auquel vous devez recommencer. Mettez à jour l'objet JSON pour ce noeud afin de réinitialiser toutes les variables contextuelles avec les valeurs de début appropriées. (Si nécessaire, vous pouvez utiliser les actions **Jump to** ou une intention "end conversation" spéciale pour exécuter cette noeud.)

Par exemple, si votre espace de travail utilise une variable contextuelle appelée `drink_order` pour stocker la sélection de boissons d'un utilisateur, vous pouvez utiliser la méthode `context.remove` pour supprimer cette variable lorsque la conversation prend fin :

```json
"context": {
   "reset_drink_order": "<?context.remove('drink_order')?>"
 }
```
{: codeblock}

Pour plus d'informations sur la modification des valeurs de variable contextuelle, reportez-vous à la rubrique [Mise à jour d'une valeur de variable contextuelle](dialog-overview.html#updating-a-context-variable-value).

**Remarque :** lorsque vous avez fini de tester votre espace de travail, vous pouvez supprimer le déploiement de test en revenant à l'outil de déploiement de test et en cliquant sur **Delete test**. N'oubliez pas que vous devez également annuler l'autorisation de l'application bot dans votre équipe Slack de manière séparée.
