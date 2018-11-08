---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-25"

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

# Utilisation de la page Overview

La page Overview du panneau **Improve** fournit un récapitulatif des conversations entre les utilisateurs et votre espace de travail. Vous pouvez visualiser le volume de trafic pour une période donnée, ainsi que les intentions et les entités qui ont été le plus souvent reconnues dans des conversations utilisateur.
{: shortdesc}

Les statistiques affichées sur la page Overview couvrent une période plus étendue que la période pendant laquelle les journaux des conversations utilisateur sont conservés. Ces statistiques représentent le trafic externe - appels d'utilisateurs ou d'API - qui a interagi avec votre espace de travail ; elles n'incluent pas les interactions à partir du panneau *Try it out* dans l'outil. 

Vous pouvez utiliser la page Overview pour répondre à des questions telles que les suivantes :

* Quels sont les mois qui ont enregistré le plus grand nombre et le plus petit nombre de conversations au cours de l'année dernière ?
* Quel est le nombre moyen de conversations qui a été enregistré par semaine au cours du premier trimestre ?
* Quelles intentions sont apparues le plus souvent au cours de la semaine passée ?
* Quelles valeurs d'entité ont été reconnues le plus souvent au cours du mois de septembre ?

Pour ouvrir la page Overview, sélectionnez **Overview** dans la barre de navigation. Si l'onglet **Overview** ne s'affiche pas, utilisez le menu ![Menu](images/Menu_16.png) pour ouvrir la page.

  ![Page Overview](images/oview.png)

La partie supérieure de la page comprend les commandes suivantes :

* *Refresh data* : vous permet d'actualiser immédiatement les statistiques de la page Overview. La page Overview indique à quel moment les données qu'elle présente ont été mises à jour pour la dernière fois. Vous pouvez sélectionner **Refresh data** si vous pensez que des données plus récentes sont peut-être disponibles.
* Commande de période : cette commande permet de choisir la période pour laquelle les données sont affichées.  Cette commande affecte toutes les données affichées sur la page, pas uniquement le nombre de conversations présenté dans le graphique, mais également les statistiques affichées avec le graphique, et les listes recensant les intentions et les entités les plus souvent reconnues (Top intents et Top entities).

  ![Commande de période](images/oview-time.png)

Vous pouvez choisir d'afficher les données pour une seule journée, une semaine, un mois, un trimestre, ou une année.  Dans chaque cas, les points de données affichés sur le graphique s'ajustent par rapport à une période de mesure appropriée.  Par exemple, lorsque vous visualisez un graphique pour une journée, les données sont présentées sous forme de valeurs horaires, mais lorsque vous visualisez un graphique pour une semaine, les données sont affichées par jour.  Un semaine court toujours du dimanche jusqu'au samedi.  Vous ne pouvez pas créer de périodes personnalisées, telles qu'une semaine qui court du jeudi au mercredi suivant ou un mois qui commence à une date autre que le 1er.

## Graphique All conversations

Un graphique affiche le nombre total de conversations pour la plage de dates sélectionnée.

**Remarque :** une 'conversation' est considérée comme n'importe quelle interaction avec l'espace de travail. Par conséquent, s'il existe une conversation où le service commence en disant : `Hi, how can I help you?` et que l'utilisateur ferme son navigateur sans répondre, cette conversation est incluse dans le nombre total de conversations. 

Vous pouvez sélectionner **View logs** afin d'ouvrir la page [User conversations](logs_convo.html), avec la plage de dates filtrée de façon à correspondre de la période que vous avez sélectionnée pour la page Overview. La page [User conversations](logs_convo.html) affiche le nombre total d'*énoncés*. Un énoncé est un message unique que l'utilisateur envoie à l'espace de travail. Chaque conversation peut être constituée de plusieurs énoncés. Par conséquent, le nombre de résultats sur la page [User conversations](logs_convo.html) est différent du nombre de conversations affiché sur la page Overview.

**Remarque** : selon votre plan et la plage de dates que vous avez sélectionnée, il se peut qu'aucune donnée n'apparaisse. Par exemple, le [plan de service standard](logs_convo.html#log-limits) de {{site.data.keyword.conversationshort}} conserve les conversations uniquement pendant 30 jours. Si vous choisissez une plage de dates de plus de 30 jours, aucune donnée ne s'affiche. 

Lorsque vous visualisez le graphique, vous pouvez cliquer sur un point de données individuel pour voir la valeur numérique, comme illustré ci-dessous :

![Point de données unique](images/oview-point.png)

Au-dessous du graphique figurent des statistiques relatives aux données affichées :

* *Total conversations* : nombre total de conversations qui ont eu lieu pendant cette période.
* *Max. conversations* : nombre maximal de conversations pour un point de données unique au cours de cette période.
* *Weak understanding* : nombre d'énoncés individuels avec un faible niveau de compréhension. Ces énoncés ne sont pas classifiés par une intention et ne contiennent pas d'entités connues. Elles peuvent s'avérer utiles pour identifier d'éventuels problèmes au niveau du dialogue.

## Listes Top intents et Top entities

Vous pouvez également afficher les intentions et les entités qui ont été reconnues le plus souvent durant la période spécifiée. Par défaut, les trois intentions et les trois entités les plus reconnues sont affichées, mais vois pouvez changer cela et indiquer un nombre plus élevé, comme 5 ou 10.

* *Top intents* : les intentions sont affichées dans une liste simple.  Vous pouvez non seulement afficher le nombre de fois qu'une intention a été reconnue, mais également utiliser lien **View logs** pour ouvrir la page User conversations avec la plage de dates filtrée de façon à correspondre aux données que vous visualisez, et l'intention filtrée de façon à correspondre à l'intention sélectionnée.

* *Top entities* : les entités sont affichées dans un graphique à barres. En sélectionnant la barre de chaque entité, vous affichez le nombre représenté par cette barre.

  ![Bulles de données d'entité](images/oview-entity.png)

  Sélectionnez **Show top values** pour afficher la liste des valeurs les plus courantes qui ont été identifiées pour cette entité durant la période. Sélectionnez **View logs** pour ouvrir la page [User conversations](logs_convo.html) avec la plage de dates filtrée de façon à correspondre aux données que vous visualisez, et l'entité filtrée de façon à correspondre à l'entité sélectionnée.
