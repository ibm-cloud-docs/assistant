---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-31"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
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

# Sécurité des informations
{: #information-security}

IBM se donne pour mission de fournir à ses clients et partenaires des solutions innovantes de confidentialité, de sécurité et de gouvernance des données.
{: shortdesc}

**Avis :**
les clients sont responsables de leur propre conformité aux différentes lois et réglementations, y compris le règlement général de l'Union européenne sur la protection des données (RGPD). Il relève de la seule responsabilité du client de consulter les services juridiques compétents aussi bien pour identifier et interpréter les lois et règlements susceptibles d’affecter son activité, que pour toute action à entreprendre pour se mettre en conformité avec ces lois et réglementations.

Les produits, services et autres fonctionnalités décrits ici ne sont pas adaptés à toutes les situations client et ne pourront être proposés que sous réserve de disponibilité. IBM ne donne aucun avis juridique, comptable ou d'audit et ne garantit pas que ses produits ou services permettent aux clients de se conformer aux lois ou réglementations applicables.

Si vous avez besoin d'une assistance RGPD pour les ressources {{site.data.keyword.cloud}} {{site.data.keyword.watson}} qui sont créées

- Pour les pays de l'Union européenne, reportez-vous à [Requesting support for IBM Cloud Watson resources created in the European Union![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/docs/services/watson/getting-started-gdpr-sar#request-EU){: new_window}.
- Pour les pays hors Union européenne, reportez-vous à [Requesting support for resources outside the European Union![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/docs/services/watson/getting-started-gdpr-sar#request-non-EU){: new_window}.

## Règlement général sur la protection des données (RGPD) de l'Union Européenne
{: #information-security-gdpr}

IBM se donne pour mission de fournir à ses clients et partenaires des solutions innovantes de confidentialité, de sécurité et de gouvernance des données pour les accompagner dans leur mise en conformité au RGPD.

Pour en savoir plus sur le parcours préparatoire au RGPD d'IBM ainsi que sur nos session proposées et offres liées au RGPD pour la prise en charge de votre parcours de conformité, cliquez sur [ici ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](../../icons/launch-glyph.svg "Icône de lien externe")](http://www.ibm.com/gdpr){: new_window}.

## Health Insurance Portability and Accountability Act (HIPAA)
{: #information-security-hipaa}

La prise en charge de la loi américaine HIPAA (Health Insurance Portability and Accountability Act ) est disponible pour les forfaits Premium hébergés à Washington, DC, créés le ou après le 1er avril 2019. Pour plus d'informations, reportez-vous à la rubrique [Activation des paramètres pris en charge dans l'Union Européenne et pour la loi HIPAA](/docs/account?topic=account-eu-hipaa-supported#eu-hipaa-supported){: external}.

N'ajoutez pas de renseignements personnels sur la santé aux données d'apprentissage (entités et intentions, y compris les exemples utilisateur) que vous créez. En particulier, veillez à supprimer tout renseignement personnel sur la santé des fichiers contenant de véritables énoncés utilisateur que vous téléchargez pour rechercher des recommandations d’intention ou d’exemple utilisateur d’intention. 

## Etiquetage et suppression de données dans Watson Assistant
{: #information-security-gdpr-wa}

N'ajoutez pas de données personnelles aux données d'apprentissage (entités et intentions, y compris les exemples utilisateur) que vous créez. En particulier, veillez à supprimer toutes les informations personnellement identifiables des fichiers contenant des énoncés utilisateur réels que vous téléchargez pour y rechercher les recommandations d'exemples utilisateur.

**Remarque :** les fonctions expérimentales et bêta ne sont pas destinées à un usage en environnement de production. Il n'est donc pas garanti qu'elles fonctionnent comme prévu lors de l'utilisation de l'étiquetage et de la suppression de données. Les fonctions expérimentales et bêta ne doivent pas être utilisées lors de l'implémentation d'une solution nécessitant l'étiquetage et la suppression de données.

Si vous devez supprimer les données d'un message de client dans une instance {{site.data.keyword.conversationshort}}, vous pouvez le faire en fonction de l'ID du client, à condition d'associer le message à un ID client lorsque le message est envoyé à {{site.data.keyword.conversationshort}}. 

**Remarque :** les fonctions Preview Link et d'intégration automatique de Facebook ne prennent pas en charge l'étiquetage et donc la suppression des données en fonction de l'ID client. Ces fonctions ne doivent pas être utilisées dans une solution qui nécessite la capacité à supprimer en fonction de l'ID client.

### Avant de commencer
{: #information-security-delete-user-data-prereqs}

Pour pouvoir supprimer les données de message associées à un utilisateur spécifique, vous devez d'abord associer tous les messages à un **ID client** unique pour chaque utilisateur. Pour spécifier l'**ID client** pour tout messages envoyé à l'aide de l'API `/message`, incluez la propriété `X-Watson-Metadata: customer_id` dans votre en-tête. Par exemple :

```
curl -X POST -u "apikey:3Df... ...Y7Pc9"
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id=abc'
 --data
   '{"input":{"text":"hello"}}'
  'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

La chaîne `customer_id` ne peut pas inclure le point-virgule (`;`) ni le signe égal (`=`). Vous devez vous assurer que chaque propriété `ID client` est unique parmi vos clients.
{: note}

Vous pouvez transmettre plusieurs valeurs **ID client** avec des paires `customer_id={value}` séparées par des points-virgules. Par exemple : `'X-Watson-Metadata: customer_id=abc;customer_id=xyz'`

Si vous ajoutez une compétence de recherche à un assistant, l'entrée utilisateur soumise à cet assistant est transmise au service {{site.data.keyword.discoveryshort}} en tant que requête de recherche. Si l'intégration {{site.data.keyword.conversationshort}} fournit un ID client, l'en-tête de la demande d'API /message obtenue inclut cet ID client, lequel est transmis à la demande d'API /query {{site.data.keyword.discoveryshort}}. Pour supprimer les données de requête associées à un client spécifique, vous devez envoyer une demande de suppression directement à l'instance de service {{site.data.keyword.discoveryshort}} associée à votre assistant. Pour plus d'informations, reportez-vous à la rubrique sur la [sécurité des informations](https://cloud.ibm.com/docs/services/discovery/information-security#gdpr-discovery) {{site.data.keyword.discoveryshort}}.

### Interrogation des données utilisateur
{: #information-security-query-customer-id}

Utilisez le paramètre `filter` de la méthode v1 `/logs` pour rechercher des données utilisateur spécifiques dans le journal des applications. Par exemple, pour rechercher des données spécifiques d'un ID client (`customer_id`) correspondant au meilleur client (`my_best_customer`), la requête peut être :

``` curl
curl -X GET -u "apikey:3Df... ...Y7Pc9"
'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/logs?version=2018-09-20&filter=customer_id::my_best_customer'
```
{: codeblock}

Pour plus d'informations, reportez-vous à la rubrique [Référence pour la requête de filtre](/docs/services/assistant?topic=assistant-filter-reference).

### Suppression de données
{: #information-security-delete-data}

Pour supprimer les données du journal des messages associées à un utilisateur spécifique que l'assistant a pu stocker, utilisez la méthode d'API v1 `DELETE /user_data`. Spécifiez l'ID client de l'utilisateur en transmettant le paramètre `customer_id` avec la demande.

Seules les données ajoutées à l'aide du noeud final de l'API `POST /message` et dotées d'un ID client associé peuvent être supprimées à l'aide de cette méthode. Les données ajoutées par d'autres méthodes ne peuvent pas être supprimées en fonction de l'ID client. Par exemple, les entités et les intentions qui ont été ajoutées à partir des conversations client ne peuvent pas être supprimées de cette manière. Les données personnelles ne sont pas prises en charge pour ces méthodes.

**IMPORTANT** : la spécification d'un `customer_id` supprimera *tous* les messages contenant ce paramètre `customer_id`, qui ont été reçus avant la demande de suppression, dans l’ensemble de votre instance {{site.data.keyword.conversationshort}}, et pas uniquement dans le cadre d’une compétence.

Par exemple, pour supprimer toute donnée de message associée à un utilisateur comportant l'ID client `abc` de votre instance {{site.data.keyword.conversationshort}}, envoyez la commande cURL suivante :

```
curl -X DELETE -u "apikey:3Df... ...Y7Pc9"
 'https://gateway-eu-de.watsonplatform.net/assistant/api/v1/user_data?customer_id=abc&version=2018-09-20'
```
{: codeblock}

Un objet JSON vide `{}` est renvoyé.

Pour plus d'informations, reportez-vous à la rubrique [Référence d'API](https://cloud.ibm.com/apidocs/assistant?curl=#delete-labeled-data).

**Remarque :** les demandes de suppression sont traitées par lots et peuvent durer jusqu'à 24 heures.
