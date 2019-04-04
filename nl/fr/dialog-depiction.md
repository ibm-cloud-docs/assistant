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

# Description du dialogue 
{: #dialog-depiction}

![Exemple d'arborescence de dialogue avec un exemple de contenu](images/dialog-depiction-full.png)

Ce diagramme montre une maquette d'une arborescence de dialogue construite avec l'outil d'édition de dialogue de l'interface utilisateur graphique. Il contient deux noeuds de dialogue racine. Une arborescence de dialogue type aurait probablement beaucoup plus de noeuds, mais cette description donne un aperçu de ce à quoi un sous-ensemble de noeuds pourrait ressembler. 

- Le premier noeud racine est conditionné par une valeur d'intention. Il comporte deux noeuds enfants qui sont chacun conditionnés par une valeur d'entité. Le deuxième noeud enfant définit deux réponses. La première réponse est renvoyée à l'utilisateur si la valeur de la variable contextuelle correspond à la valeur spécifiée dans la condition. Si tel n'est pas le cas, la deuxième réponse est renvoyée. 

  Ce type de noeud standard est utile pour capturer les questions relatives à un sujet donné, puis, dans la réponse racine, pour poser une question de suivi traitée par les noeuds enfants. Par exemple, il peut reconnaître une question d'utilisateur sur les remises et poser une question complémentaire sur le fait de savoir si l'utilisateur est membre d'une association avec laquelle la société a mis en place des arrangements de réduction spéciaux. Les noeuds enfants fournissent alors des réponses différentes en fonction de la réponse de l'utilisateur à la question sur l'appartenance à une association. 

- Le second noeud racine est un noeud avec attributs. Il est également conditionné par une valeur d'intention. Il définit un ensemble d'attributs, un pour chaque information que vous souhaitez collecter auprès de l'utilisateur. Chaque attributs pose une question pour obtenir la réponse de l'utilisateur. Il recherche une valeur d'entité spécifique dans la réponse de l'utilisateur à l'invite, qu'il enregistre ensuite dans une variable contextuelle d'attribut. 

  Ce type de noeud est utile pour collecter les informations dont vous pourriez avoir besoin pour effectuer une transaction pour le compte de l'utilisateur. Par exemple, si l'intention de l'utilisateur est de réserver un vol, les attributs peuvent collecter les informations sur le départ et la destination, les dates de voyage, etc.  
