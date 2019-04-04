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

# Intégration à Facebook Messenger  
{: #deploy-facebook}

Facebook Messenger est une application de messagerie mobile qui aide les entreprises et les clients à communiquer directement entre eux.
{: shortdesc}

Après avoir configuré une compétence de dialogue et l'avoir ajoutée à un assistant, vous pouvez intégrer ce dernier à Facebook Messenger.  

Il n’existe actuellement aucun mécanisme permettant d’identifier les utilisateurs qui interagissent avec l’assistant via Facebook Messenger, ce qui signifie qu’il n’y a aucun moyen d’identifier ou de supprimer les données associées à un utilisateur spécifique. N'utilisez pas cette méthode d'intégration pour les déploiements qui doivent être conformes au RGPD. Pour plus d'informations, reportez-vous à la rubrique [Sécurité des informations](/docs/services/assistant?topic=assistant-information-security).
{: important}

1.  Dans l'onglet Assistants, cliquez pour ouvrir la vignette de l'assistant que vous souhaitez déployer. 

1.  Dans la section Integrations, cliquez sur **Add Integration**.

1.  Cliquez sur le bouton **Select Integration** pour *Facebook Messenger*.

1.  Suivez les instructions fournies à l'écran pour effectuer le processus d'intégration. 

Si vous souhaitez suivre les étapes de déploiement tout au long du processus de déploiement, regardez cette vidéo. 

<iframe class="embed-responsive-item" id="youtubeplayer" title="Procédure pas à pas du déploiement de Facebook" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/8o-FFU5sYNM?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Durée : 8 minutes

## Considérations sur les dialogues 
{: #deploy-facebook-dialog}

Les réponses enrichies que vous ajoutez à un dialogue sont affichées dans une application Facebook comme prévu, avec les exceptions suivantes : 

- **Connect to human agent** : ce type de réponse est ignoré.

- **Image** : ce type de réponse incorpore une image dans la réponse. Le titre et la description ne sont pas affichés avant l'image, que vous les spécifiiez ou non. 

- **Option** : ce type de réponse affiche une liste d'options parmi lesquelles l'utilisateur peut choisir. 

  - Un titre est **requis** et est affiché avant la liste des options.
  - Aucune description n'est affichée, que vous en spécifiiez une ou non. 
  - Lorsqu'un utilisateur clique sur l'un des boutons, ses choix disparaissent et sont remplacés par l'entrée utilisateur générée par le choix de l'utilisateur. Si l'assistant ou l'utilisateur saisit une nouvelle entrée, l'entrée générée par le bouton disparaît. Par conséquent, si vous incluez plusieurs types de réponse dans une seule réponse, positionnez le type de réponse d'option en dernier. Faute de quoi, le contenu des réponses suivantes, tel que le texte d'un type de réponse textuel, remplacera le texte généré par le bouton.  

- **Pause** : ce type de réponse met en pause l'activité de l'assistant dans Messenger. Cependant, l'activité ne reprend pas après la pause, sauf si un autre type de réponse est déclenché après celui-ci. Chaque fois que vous incluez ce type de réponse, ajoutez-en un autre, différent, tel qu'une réponse textuelle, et positionnez-le après celui-ci. 

Pour plus d'informations sur les types de réponse, reportez-vous à la rubrique [Réponses enrichies](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia). 

## Discussion avec l'assistant
{: #deploy-facebook-try}

Pour démarrer une discussion avec l'assistant, procédez comme suit : 

1.  Ouvrez Facebook Messenger.  
1.  Saisissez le nom de la page que vous avez créée.
1.  Une fois la page affichée, cliquez dessus, puis commencez à discuter avec l'assistant. 

Le noeud Bienvenue de votre dialogue n'est pas traité par l'intégration de Facebook Messenger. Le message de bienvenue ne s'affiche pas dans la discussion Facebook, contrairement au panneau "Try it out" de l'outil ou à la page Web d'intégration Preview Link. Il n'est pas déclenché à partir d'ici car les noeuds associés à la condition spéciale `welcome` sont ignorés dans les flux de dialogue démarrés par les utilisateurs. Facebook Messenger attend que l'utilisateur débute la conversation. Si vous devez définir des valeurs par défaut pour les variables contextuelles au début de votre conversation, ne les définissez pas dans le noeud de bienvenue. Pour plus d'informations, reportez-vous à la rubrique [Démarrage du dialogue](/docs/services/assistant?topic=assistant-dialog-start).
{: note}

Le flux de dialogue pour la session en cours est redémarré après 60 minutes d'inactivité (5 minutes pour les forfaits Lite et Standard). Cela signifie que si un utilisateur cesse d'interagir avec l'assistant, au bout de 60 (ou 5) minutes, toutes les valeurs de variable contextuelles définies lors de la conversation précédente sont définies sur null ou sur leurs valeurs par défaut. 
