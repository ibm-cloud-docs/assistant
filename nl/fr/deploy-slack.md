---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-01"

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

# Intégration à Slack
{: #deploy-slack}

Slack est une application de messagerie cloud qui aide les utilisateurs à collaborer les uns avec les autres.
{: shortdesc}

Après avoir configuré une compétence de dialogue et l'avoir ajoutée à un assistant, vous pouvez intégrer ce dernier à Slack. 

1.  Dans l'onglet Assistants, cliquez pour ouvrir la vignette de l'assistant que vous souhaitez déployer. 

1.  Dans la section Integrations, cliquez sur **Add Integration**.

1.  Cliquez sur le bouton **Select Integration** pour *Slack*.

1.  Suivez les instructions fournies à l'écran pour effectuer le processus d'intégration. 

Si vous souhaitez suivre les étapes de déploiement tout au long du processus de déploiement, regardez cette vidéo. 

<iframe class="embed-responsive-item" id="youtubeplayer" title="Procédure pas à pas du déploiement de Slack" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/RBGBPJ8h4HQ?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Durée : 9,5 minutes

## Considérations sur les dialogues 
{: #deploy-slack-dialog}

Les réponses enrichies que vous ajoutez à un dialogue sont affichées dans un canal Slack comme prévu, avec les exceptions suivantes : 

- **Connect to human agent** : ce type de réponse est ignoré.

- **Image** : un titre d'image est obligatoire. Si vous ne fournissez pas de titre, l'image ne s'affiche pas.

- **Option** : ce type de réponse affiche une liste d'options parmi lesquelles l'utilisateur peut choisir. 

  - Un titre est **requis** et est affiché avant la liste des options.
  - Aucune description n'est affichée, que vous en spécifiiez une ou non. 
  - Lorsqu'un utilisateur clique sur l'un des boutons, ses choix disparaissent et sont remplacés par l'entrée utilisateur générée par le choix de l'utilisateur. Si vous incluez plusieurs types de réponse dans une seule réponse, positionnez le type de réponse d'option en dernier. Faute de quoi, la sortie contiendra une combinaison de réponses et d'entrées utilisateur susceptibles de semer la confusion chez l'utilisateur.

- **Pause** : ce type de réponse met en pause l'activité de l'assistant dans le canal Slack. Cependant, aucun indicateur visible n’est affiché pour indiquer que l’assistant est en pause, que vous choisissiez ou non d’afficher la saisie. 

Pour plus d'informations sur les types de réponse, reportez-vous à la rubrique [Réponses enrichies](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia). 

## Discussion avec l'assistant
{: #deploy-slack-try}

Pour démarrer une discussion avec l'assistant, procédez comme suit : 

1.  Ouvrez Slack et accédez à l'espace de travail associé à votre application. 
1.  Cliquez sur l'application que vous avez créée dans la section Applications. 
1.  Discutez avec l’assistant. 

Le noeud Bienvenue de votre dialogue n'est pas traité par l'intégration de Slack. Le message de bienvenue ne s'affiche pas dans le canal Slack, contrairement au panneau "Try it out" de l'outil ou à la page Web d'intégration Preview Link. Il n'est pas déclenché à partir d'ici car les noeuds associés à la condition spéciale `welcome` sont ignorés dans les flux de dialogue démarrés par les utilisateurs. Slack attend que l'utilisateur débute la conversation. Si vous devez définir des valeurs par défaut pour les variables contextuelles au début de votre conversation, ne les définissez pas dans le noeud de bienvenue. Pour plus d'informations, reportez-vous à la rubrique [Démarrage du dialogue](/docs/services/assistant?topic=assistant-dialog-start).
{: note}

Le flux de dialogue pour la session en cours est redémarré après 60 minutes d'inactivité (5 minutes pour les forfaits Lite et Standard). Cela signifie que si un utilisateur cesse d'interagir avec l'assistant, au bout de 60 (ou 5) minutes, toutes les valeurs de variable contextuelles définies lors de la conversation précédente sont définies sur null ou sur leurs valeurs par défaut. 
