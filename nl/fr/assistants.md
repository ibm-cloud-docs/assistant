---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

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

# Assistants
{: #assistants}

Un assistant est un bot cognitif que vous pouvez personnaliser en fonction des besoins de votre entreprise et que vous pouvez déployer sur plusieurs canaux pour apporter de l'aide à vos clients où et quand ils en ont besoin.
{: shortdesc}

![Compétences](images/skill-icon.png) Vous pouvez personnaliser l’assistant en y ajoutant les compétences dont il a besoin pour satisfaire les objectifs de vos clients.

Ajoutez une compétence de dialogue capable de comprendre et de traiter les questions ou les demandes pour lesquelles vos clients ont généralement besoin d’aide. Vous fournissez des informations sur les sujets ou les tâches sur lesquels vos utilisateurs vous posent des questions et sur la manière dont ils le font. Le service crée de manière dynamique un modèle d’apprentissage automatique conçu pour comprendre les demandes identiques et similaires des utilisateurs. 

| Arborescence de dialogue | Interface graphique |
|-------------|-------------------------:|
|Vous pouvez utiliser des outils graphiques pour créer un dialogue que votre assistant lira lors de ses interactions avec les utilisateurs, un dialogue qui simule une conversation réelle. Le dialogue décrit les objectifs clients communs que vous lui apprenez à reconnaître et fournit des réponses utiles. | ![Exemple d'arborescence de dialogue avec un exemple de contenu](images/dialog-depiction.png) |

La compétence de dialogue même est définie en texte, mais vous pouvez l'intégrer aux services Watson Speech to Text et Watson Text to Speech qui permettent aux utilisateurs d'interagir avec votre assistant verbalement. 

![Données d’apprentissage prêtes à l'emploi](images/oob.png) Si vous souhaitez commencer rapidement, ajoutez des données d’apprentissage prédéfinies à votre compétence de dialogue pour que votre assistant puisse commencer à aider vos clients avec les bases.

![IBM Cloud](images/cloud.png) L’assistant est un bot entièrement hébergé géré par {{site.data.keyword.cloud_notm}}, ce qui signifie que vous n'avez pas à vous soucier de la configuration ou de la maintenance d'une infrastructure pour le prendre en charge.

| Intégrations       | Canaux  |
|--------------------|:----------|
|Vous pouvez déployer l'assistant via plusieurs interfaces, y compris les canaux de messagerie existants, tels que Slack et Facebook Messenger, en quelques étapes seulement. Si vous souhaitez concevoir une application personnalisée qui l’intègre, vous pouvez appeler directement les API sous-jacentes pour ce faire. | ![Méthodes d'intégration comprenant Slack, Facebook Messenger, une application Web ou l'intégration d'un agent humain](images/integrations.png) |

Pour commencer, reportez-vous à la rubrique [Création d'un assistant](/docs/services/assistant?topic=assistant-assistant-add).
