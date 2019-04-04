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

# Sauvegarde et restauration des données 
{: #backup}

Sauvegardez et restaurez vos données en les exportant, puis en les important.
{: shortdesc}

Vous pouvez exporter les données suivantes à partir d'une instance de service {{site.data.keyword.conversationshort}} :

- Données d'apprentissage de compétence de dialogue (intentions et entités) 
- Dialogue de compétence de dialogue  

Vous ne pouvez pas exporter les données suivantes : 

<!--- Search skill -->
- Assistant, y compris les intégrations configurées 

## Conservation des journaux
{: #backup-retain-logs}

Si vous souhaitez stocker les journaux des conversations des utilisateurs avec votre assistant, vous pouvez utiliser l'API `/logs` pour exporter vos données de journal. Pour plus d'informations, reportez-vous à la rubrique [API reference ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/apidocs/assistant#list-log-events-in-a-workspace). 

Pour obtenir l'ID d'espace de travail d'une compétence, dans la vignette de compétence, cliquez sur l'icône ![ouvrir et fermer la liste d'options](images/kabob-beta.png), puis choisissez **View API Details**.
{: tip}

Les journaux sont stockés pendant une durée différente en fonction de votre forfait de service. Par exemple, les forfaits Lite fournissent uniquement les journaux des 7 derniers jours. Pour plus d'informations, reportez-vous à la rubrique [Limites des journaux](/docs/services/assistant?topic=assistant-logs#logs-limits). 

Vous ne pouvez pas importer les journaux d’une compétence dans une autre.  

## Exportation d'une compétence de dialogue 
{: #backup-export-skill}

Pour sauvegarder les données d'une compétence de dialogue, exportez cette compétence sous forme de fichier JSON et stockez le fichier JSON. 

1.  Recherchez la vignette de compétence de dialogue sur la page Skills ou sur la page de configuration d'un assistant utilisant la compétence. 

1.  Cliquez sur l'icône ![ouvrir et fermer la liste d'options](images/kabob-beta.png), puis choisissez **Download JSON**. 

1.  Spécifiez un nom pour le fichier JSON et l'emplacement de sauvegarde, puis cliquez sur **Save**.

Vous pouvez également utiliser l'API `/workspaces` pour exporter une compétence de dialogue. Incluez le paramètre `export=true` dans la demande d'espace de travail GET. Pour plus d'informations, reportez-vous à la documentation [API reference![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/apidocs/assistant#get-information-about-a-workspace).

## Importation d'une compétence de dialogue 
{: #backup-import-skill}

Pour rétablir une copie de sauvegarde d'une compétence de dialogue exportée à partir d'une autre instance de service ou d'un autre environnement, créez une nouvelle compétence de dialogue en important le fichier JSON de la compétence de dialogue que vous avez exportée. 

Si le service {{site.data.keyword.conversationshort}} change entre le moment où vous exportez la compétence et le moment où l'importez, en raison de mises à jour fonctionnelles régulièrement appliquées aux instances dans des environnements de distribution continue hébergés dans le cloud, votre compétence importée peut fonctionner différemment.
{: important}

1.  Cliquez sur l'onglet **Skills**.

1.  Cliquez sur **Create new**.

1.  Cliquez sur **Import skill**, puis cliquez sur **Choose JSON File**, puis sélectionnez le fichier JSON à importer.

    **Important :**

    - Le fichier JSON importé doit utiliser le codage UTF-8, sans codage BOM (Byte Order Mark).
    - Un fichier JSON de compétence ne doit pas excéder 10 Mo. Si la compétence que vous devez importer est plus grande, vous avez la possibilité d'importer les intentions et les entités séparément après avoir importé la compétence. (Vous pouvez également importer des compétences plus grandes à l'aide de l'API REST. Pour plus d'informations, reportez-vous à la [Référence d'API ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://cloud.ibm.com/apidocs/assistant#create-workspace){: new_window}.)
    - Le fichier JSON ne peut pas contenir de tabulations, de retours à la ligne ou de retours chariot.

    Sélectionnez **Everything (Intents, Entities, and Dialog)** si vous souhaitez importer une copie entière de la compétence exportée.

    Cliquez sur **Import**.

    Si vous ne parvenez pas à importer une compétence, reportez-vous à la rubrique [Traitement des incidents liés à l'importation de compétences](/docs/services/assistant?topic=assistant-skill-add#skill-add-import-errors).

1.  Spécifiez les détails de la compétence :

    - **Name** : nom qui ne doit pas excéder 100 caractères. Le nom est obligatoire.
    - **Description** : description facultative qui ne doit pas excéder 200 caractères.
    - **Language** : langue dans laquelle les entrées utilisateur seront saisies et que la compétence sera formée à comprendre. La valeur par défaut est l'anglais.

Une fois la compétence créée, elle apparaît sous la forme d'une vignette sur la page Skills.

## Recréation de votre assistant 
{: #backup-recreate-assistant}

Vous pouvez maintenant recréer votre assistant. Vous pouvez ensuite lier votre compétence de dialogue importée à l’assistant et configurer ses intégrations.  

Pour plus d'informations, reportez-vous aux rubriques [Création d'un assistant](/docs/services/assistant?topic=assistant-assistant-add) et [Ajout d'intégrations](/docs/services/assistant?topic=assistant-deploy-integration-add#deploy-integration-add-task). 

## Mise à jour de vos applications client
{: #backup-update-api}

Lorsque vous importez une compétence de dialogue que vous avez exportée, une nouvelle compétence est créée. La nouvelle compétence a un nouvel ID d'espace de travail. Si vous disposez d'applications client utilisant l'API v1 pour accéder à cette compétence, vous devez mettre à jour toutes les références d'ID d'espace de travail pour utiliser le nouvel ID d'espace de travail. 

Lorsque vous recréez votre assistant, un nouvel ID d’assistant lui est attribué. Si des applications client existantes utilisent l’API v2 pour accéder à l’assistant, vous devez mettre à jour toutes les références d’ID d’assistant pour utiliser le nouvel ID d’assistant. 
