---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-05"

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

# Création d'une version de compétence
{: #versions}

Les versions vous aident à gérer le flux de travail d'un projet de développement d'une compétence de dialogue.
{: shortdesc}

Créez une version de compétence pour capturer un instantané des données d'apprentissage (intentions et entités) et dialoguez dans la compétence à des moments clés du processus de développement. Pouvoir sauvegarder une compétence en cours à un moment donné est particulièrement utile lorsque vous commencez à affiner votre assistant. Pour déterminer si une modification améliore ou diminue l'efficacité de l'assistant, vous devez souvent apporter cette modification et en vérifier l'effet en temps réel. En vous basant sur les résultats d'un déploiement d'environnement de test, vous pouvez décider en toute connaissance de cause d'apporter une modification donnée sur un assistant déployé dans un environnement de production.

Si vous disposez d'un forfait gratuit (Lite), vous ne pouvez pas créer de versions de compétences.
{: note}

Cette vidéo de deux minutes trente explique comment l'utilisation des versions peut vous aider.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Création de versions de compétences" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/FDolnBxvcZ8" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Pour en savoir plus sur la manière dont les versions peuvent améliorer le flux de travail utilisé pour créer un assistant, [lisez cet article ![Icône de lien externe](../../icons/launch-glyph.svg "Icône de lien externe")](https://medium.com/ibm-watson/watson-assistant-versions-announcement-d60869b1f5f){: new_window}.

## Création d'une version
{: #versions-create}

Vous pouvez modifier une seule version de la compétence de dialogue à la fois. La version en cours est appelée version de *développement*.

Lorsque vous enregistrez une version, les paramètres de compétence que vous avez appliqués à la version de développement sont également enregistrés.

Pour créer une version de compétence de dialogue, procédez comme suit :

1.  Dans l'en-tête de la compétence, cliquez sur **Save new version**, puis décrivez l'état actuel de la compétence.

    Ajouter une bonne description vous aidera ultérieurement à distinguer plusieurs versions les unes des autres.

1.  Cliquez sur **Save**.

Un instantané est pris de la compétence actuelle et sauvegardé sous une nouvelle version. Vous êtes toujours dans la version de développement de la compétence. Toutes les modifications que vous apportez continuent d'être appliquées à la version de développement, pas à la version que vous avez enregistrée. Pour accéder à la version que vous avez enregistrée, accédez à la page **Versions**.

## Déploiement d'une version de compétence
{: #versions-deploy}

1.  Dans l'en-tête de la compétence, cliquez sur l'onglet **Versions**.
1.  Cliquez sur l'icône ![Click to view actions](images/kebab-react.png) dans la version que vous souhaitez déployer, puis choisissez **Assign to assistant**.

    La liste des assistants auxquels vous pouvez lier cette version est affichée. La liste est limitée aux assistants qui ne possèdent aucune compétence ou qui sont associés à une version différente de cette compétence.
1.  Cochez la case correspondant à un ou plusieurs assistants, puis cliquez sur **Assign**.

Gardez une trace du moment où cette version est déployée sur un assistant et pendant combien de temps. Il est probable que vous souhaiterez analyser les conversations entre utilisateurs et cette version spécifique de la compétence. Vous pouvez obtenir ces informations à partir de la page **Analytics**. Toutefois, lorsque vous choisissez une source de données, les versions ne sont pas répertoriées. Vous devez choisir le nom de l'assistant sur lequel vous avez déployé cette version. Vous pouvez ensuite filtrer les données de métriques pour afficher uniquement les conversations qui ont eu lieu entre les dates de début et de fin de la période pendant laquelle cette version de compétence a été déployée dans l'assistant.
{: important}

## Limites de version de compétence
{: #versions-limits}

Le nombre de versions que vous pouvez créer pour une compétence dépend de votre forfait {{site.data.keyword.conversationshort}}.

| Forfait de service     | Versions par compétence |
|------------------|-------------------:|
| Premium          |                 50 |
| Plus             |                 10 |
| Standard         |                 10 |
| Plus Trial       |                 10 |
| Lite             |                  0 |
{: caption="Détails de forfait de service" caption-side="top"}

## Echange de compétences
{: #versions-swap-skills}

Si vous estimez qu'une version antérieure de la compétence reconnaissait mieux les besoins des clients et y répondait mieux qu'une version ultérieure, vous pouvez échanger la compétence liée à l'assistant pour utiliser la version antérieure.

Suivez les mêmes étapes que pour [déployer une version de compétence](#versions-deploy) afin de modifier la version liée à un assistant.

## Accès à une version sauvegardée
{: #versions-view}

Le seul moyen de visualiser une version sauvegardée consiste à écraser la version en cours de développement de la compétence par la version sauvegardée. (Mais pas avant d'avoir enregistré le travail effectué dans la version de développement en cours.)

Vous ne pouvez pas modifier une version sauvegardée. Pour atteindre le même objectif, vous pouvez utiliser une version sauvegardée en tant que base d’une nouvelle version dans laquelle vous intégrez les modifications que vous souhaitez apporter. Pour commencer le travail de développement à partir d'une version sauvegardée, écrasez la version en cours de développement de la compétence par la version sauvegardée.

1.  Enregistrez toutes les modifications que vous avez apportées à la compétence depuis la dernière création d'une version.

    Enregistrez une version de la compétence maintenant. Faute de quoi, votre travail sera perdu lorsque vous suivrez les étapes ci-après.
    {: important}

1.  Dans la version que vous souhaitez modifier, cliquez sur l'icône **Skill actions** ![Skill actions](images/kebab-react.png), puis choisissez **Revert to this version** et confirmez l'action.

    La page s'actualise pour revenir à l'état dans lequel se trouvait la compétence lors de la création de la version.

Si vous souhaitez enregistrer les modifications que vous apportez à cette version, vous devez enregistrer la compétence en tant que nouvelle version. Vous ne pouvez pas appliquer les modifications à une version déjà sauvegardée.

Lorsque vous ouvrez une compétence en cliquant sur la vignette de compétence de la page de l’assistant, la version en développement de la compétence s’affiche. Même si vous associez une version ultérieure à l’assistant, lorsque vous accédez à la compétence, sa version de développement s’ouvre.
{: important}
