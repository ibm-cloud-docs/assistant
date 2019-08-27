---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-30"

subcollection: assistant


---

{:curl: #curl .ph data-hd-programlang='curl'}
{:external: target="_blank" .external}
{:javascript: #javascript .ph data-hd-programlang='javascript'}
{:java: #java .ph data-hd-programlang='java'}
{:python: #python .ph data-hd-programlang='python'}
{:swift: data-hd-programlang='swift'}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:codeblock: .codeblock}
{:download: .download}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}

# Migration vers l'API v2
{: #api-migration}

L'API d'exécution Assistant v2, qui prend en charge l'utilisation d'assistants et de compétences, a été introduite en novembre 2018. Cette API offre des avantages significatifs par rapport à l'API d'exécution v1, notamment la gestion automatique de l'état, la facilité de déploiement, la gestion des versions de compétences et la disponibilité de nouvelles fonctionnalités telles que la compétence de recherche. 

L'API v2 est disponible pour tous les utilisateurs, quel que soit le plan de service, sans frais supplémentaires. 

L'API v2 ne prend actuellement en charge que les interactions d'exécution avec un assistant existant. Les applications de création qui créent ou modifient des espaces de travail devraient continuer à utiliser l'API v1.
{: note}

## Présentation

Avec l’API v2, votre application client communique avec un assistant plutôt que directement avec un espace de travail. Un assistant est une nouvelle couche d'orchestration qui offre plusieurs nouvelles fonctionnalités, notamment la gestion automatique de l'état, la gestion des versions de compétences, un déploiement simplifié et des compétences de recherche (pour les forfaits Plus et Premium). Votre espace de travail existant (désormais appelé _compétence de dialogue_) continue de fonctionner comme avant, mais les nouvelles fonctionnalités sont fournies par la nouvelle couche d'assistant. 

Toutes les communications avec un assistant se déroulent dans le contexte d'une _session_, ce qui conserve l'état de la conversation pendant toute la durée de la conversation. Les données d'état, y compris les variables contextuelles définies par votre dialogue ou votre application client, sont automatiquement stockées par {{site.data.keyword.conversationshort}}, sans aucune action de la part de votre application. 

Les données d'état persistent jusqu'à ce que vous supprimiez explicitement la session, ou jusqu'à ce que la session arrive à expiration en raison de l'inactivité.

Si vous disposez d'une application existante qui utilise l'API v1 pour envoyer l'entrée utilisateur directement vers un espace de travail, la migration de votre application pour utiliser l'API v2 est un processus simple. 

## Configuration d'un assistant 

L'API d'exécution v2 envoie des messages à un assistant, qui les achemine vers votre compétence de dialogue (anciennement espace de travail). Pour configurer un assistant, utilisez l'interface utilisateur {{site.data.keyword.conversationshort}} :

1. Cliquez sur l'onglet **Skills**. Vérifiez que votre espace de travail est affiché en tant que compétence disponible. (Tous les espaces de travail existants pour votre instance de service sont automatiquement convertis en compétences dans l'interface utilisateur {{site.data.keyword.conversationshort}}. Cette conversion ne modifie pas l'espace de travail sous-jacent.)

1. Cliquez sur l'onglet **Assistants**. Cliquez sur **Create assistant** pour créer un assistant. Lorsque vous êtes invité à ajouter des compétences, cliquez sur **Add dialog skill** et sélectionnez la compétence de dialogue qui correspond à votre espace de travail.

  Pour plus d'informations sur la création d'assistants, reportez-vous à la rubrique [Création d'un assistant](https://cloud.ibm.com/docs/services/assistant?topic=assistant-assistant-add).

1. Une fois votre nouvel assistant créé, cliquez sur le menu ![Menu](images/kebab-react.png), puis sélectionnez **Settings**.

1. Sur la page **Assistant Settings**, recherchez l'ID assistant. Votre application utilise cet ID (au lieu d'un ID d'espace de travail) pour communiquer avec l'assistant. Les données d'identification du service sont les mêmes pour les API v1 et v2.

  Actuellement, il n'existe aucune prise en charge d'API pour l'extraction d'un ID assistant. Pour trouver l'ID assistant, vous devez utiliser l'interface utilisateur {{site.data.keyword.conversationshort}}.
  {: note}

## Appel de l'API d'exécution v2 

Après avoir créé un assistant, vous pouvez mettre à jour votre application client pour qu'elle utilise l'API d'exécution v2 au lieu de l'API d'exécution v1. 

1. Avant d’envoyer le premier message d’une conversation, utilisez la méthode [**Créer une session**](https://cloud.ibm.com/apidocs/assistant-v2#create-a-session){: external} v2 pour créer une session. Sauvegardez l'ID de session renvoyé :

  ```javascript
  service
  .createSession({
    assistant_id: assistantId,
  })
  .then(res => {
    sessionId = res.session_id;
  })
  ```
  {: codeblock}
  {: javascript}

  ```python
  session_id = service.create_session(
    assistant_id = assistant_id
).get_result()['session_id']
  ```
  {: codeblock }
  {: python }

  ```java
CreateSessionOptions createSessionOptions = new CreateSessionOptions.Builder(assistantId).build();
    SessionResponse session = service.createSession(createSessionOptions).execute().getResult();
    String sessionId = session.getSessionId();

    ```
  {: codeblock}
  {: java}

1. Utilisez la méthode [**Envoyer une entrée utilisateur à l'assistant**](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant){: external} v2 pour envoyer une entrée utilisateur à l'assistant. Au lieu de spécifier l'ID d'espace de travail comme vous l'avez fait avec l'API v1, vous spécifiez l'ID d'assistant et l'ID de session : 

  ```javascript
  service
    .message({
      assistant_id: assistantId,
      session_id: sessionId,
      input: messageInput
    })
  ```
  {: codeblock}
  {: javascript}

  ```python
  response = service.message(
      assistant_id,
      session_id,
      input = message_input
  ).get_result()
  ```
  {: codeblock}
  {: python}

  ```java
  MessageInput input = new MessageInput.Builder().text(inputText).build();
      MessageOptions messageOptions = new MessageOptions.Builder(assistantId, sessionId)
    .input(input)
    .build();
  MessageResponse response = service.message(messageOptions)
    .execute()
    .getResult();
  ```
  {: codeblock}
  {: java}

  La structure de message de base n'a pas changé ; en particulier, l'entrée utilisateur est toujours envoyée en tant que `input.text`.

1. Une fois la conversation terminée, utilisez la méthode [**Supprimer une session**](https://cloud.ibm.com/apidocs/assistant-v2#delete-session){: external} v2 pour supprimer la session.

  ```javascript
  service
    .deleteSession({
      assistant_id: assistantId,
      session_id: sessionId,
    })
  ```
  {: codeblock}
  {: javascript}

  ```python
  service.delete_session(
    assistant_id = assistant_id,
    session_id = session_id
)
  ```
  {: codeblock}
  {: python}

  ```java
  DeleteSessionOptions deleteSessionOptions = new DeleteSessionOptions.Builder(assistantId, sessionId
    .build();
  service.deleteSession(deleteSessionOptions).execute();
  ```
  {: codeblock}
  {: java}

   Si vous ne supprimez pas explicitement la session, celle-ci sera automatiquement supprimée après le délai d'expiration configuré. (La durée du délai d'attente dépend de votre forfait ; pour plus d'informations, reportez-vous à la rubrique [Limites de session](/docs/services/assistant?topic=assistant-assistant-settings#assistant-settings-session-limits).)

Pour voir des exemples d'API v2 dans le contexte d'une application client simple, reportez-vous à la rubrique [Création d'une application client](/docs/services/assistant?topic=assistant-api-client).

## Traitement du format de réponse v2

Votre application devra peut-être être mise à jour pour gérer le format de réponse de l'exécution v2, selon les éléments de la réponse auxquels votre application doit accéder : 

- La sortie pour tous les types de réponse (tels que `text` et `option`) est toujours renvoyée dans l'objet `output.generic`. Le code d'application permettant de traiter ces réponses doit fonctionner sans modification. 

- Les intentions et les entités détectées sont maintenant renvoyées dans le cadre de l’objet `output`, plutôt qu’à la racine du code JSON de la réponse. 

- Le contexte de conversation est maintenant organisé en deux objets :

  - Le **contexte global** contient les données de contexte de niveau système partagées par toutes les compétences utilisées par l'assistant.

  - Le **contexte de compétence** contient toutes les variables contextuelles définies par l'utilisateur, utilisées par votre compétence de dialogue. 

  Cependant, gardez à l'esprit que les données d'état, y compris le contexte de conversation, sont maintenant gérées par l'assistant. Votre application n'a donc peut-être pas besoin d'accéder au contexte (reportez-vous à la rubrique [Laisser l'assistant gérer l'état](#api-migration-state)).

Reportez-vous à la [ référence de l'API](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant){: external} v2 pour une documentation complète du format de réponse v2.

## Laisser l'assistant gérer l'état
{: #api-migration-state}

Pour la plupart des applications, vous pouvez maintenant supprimer tout code inclus dans le but de gérer l'état. Il n'est plus nécessaire de sauvegarder le contexte et de le renvoyer à {{site.data.keyword.conversationshort}} à chaque tour de conversation. Le contexte est automatiquement géré par {{site.data.keyword.conversationshort}} et votre dialogue peut y accéder comme auparavant. 

Notez qu'avec l'API v2, le contexte n'est pas inclus par défaut dans les réponses à l'application client. Cependant, votre code peut toujours accéder aux variables contextuelles si nécessaire : 

- Vous pouvez toujours envoyer un objet `context` dans le cadre de l'entrée du message. Toutes les variables contextuelles que vous incluez sont stockées dans le cadre du contexte géré par {{site.data.keyword.conversationshort}}. (Si la variable contextuelle que vous envoyez existe déjà dans le contexte, la nouvelle valeur remplace la valeur stockée précédemment.)

  Assurez-vous que l'objet de contexte que vous envoyez est conforme au format v2. Toutes les variables contextuelles définies par l'utilisateur envoyées par votre application doivent faire partie du contexte de compétence. En règle générale, la seule variable contextuelle globale à définir est `system.user_id`, utilisée par les forfaits Plus et Premium à des fins de facturation. 

- Vous pouvez toujours extraire des variables contextuelle à partir du contexte global ou des compétences. Pour que l'objet `context` soit inclus dans les réponses au message, utilisez la propriété **return_context** dans les options de saisie du message. Pour plus d'informations, reportez-vous à la rubrique [Accès aux données contextuelles](/docs/services/assistant?topic=assistant-api-client-get-context).
