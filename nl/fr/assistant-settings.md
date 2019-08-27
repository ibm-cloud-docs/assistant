---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-29"

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

# Modification du paramètre de délai d'attente d'inactivité ![Forfait Plus ou Premium uniquement](images/plus.png)
{: #assistant-settings}

Lorsqu'un utilisateur interagit avec votre assistant via l'une des intégrations incorporées, la session de discussion s'achève au bout d'un laps de temps spécifique pendant lequel l'utilisateur n'interagit pas avec l'assistant. Vous pouvez spécifier le délai d'attente avant la réinitialisation d'une session de discussion en modifiant le paramètre de délai d'attente d'inactivité.
{: shortdesc}

Lorsqu'une session de discussion est réinitialisée, le dialogue perd toutes les informations contextuelles qu'il a enregistrées lors de l'échange précédent avec l'utilisateur. Par exemple, si le dialogue demande le nom de l'utilisateur, puis appelle l'utilisateur par ce nom tout au long du reste de la conversation, une fois la session de discussion terminée et une nouvelle session lancée, le dialogue redémarre en demandant à nouveau le nom de l'utilisateur. 

## Limites de session
{: #assistant-settings-session-limits}

La durée autorisée pour un délai d'attente d'inactivité varie en fonction du type de forfait de l'instance de service. Le tableau suivant répertorie les limites.

| Forfait de service | Période d'inactivité par défaut de session de discussion | Période d'inactivité maximale de session de discussion |
|--------------|--------------------------------:|----------------------------:|
| Premium      |                      60 minutes |                    24 heures |
| Plus         |                      60 minutes |                    24 heures |
| Standard     |                       5 minutes |                   5 minutes |
| Plus Trial   |                       5 minutes |                   5 minutes |
| Lite*        |                       5 minutes |                   5 minutes |
{: caption="Détails de forfait de service" caption-side="top"}

## Modification du paramètre
{: #assistant-settings-timeout-task}

1.  Cliquez sur le menu de votre assistant, puis choisissez **Settings**.

1.  Modifiez l'heure spécifiée dans la zone **Timeout limit**.

1.  Fermez la page des paramètres. Votre modification est automatiquement sauvegardée. 
