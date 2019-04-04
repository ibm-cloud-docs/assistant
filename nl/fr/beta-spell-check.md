---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-11"

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
{:gif: data-image-type='gif'}

# Correction de l'entrée utilisateur  
{: #beta-spell-check}

Cette fonctionnalité est uniquement disponible pour les participants du programme bêta. Pour savoir comment demander un accès, reportez-vous à la rubrique [Participer au programme bêta](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

![Beta](images/beta.png) IBM publie des services, des fonctions et un support de langue pour votre évaluation qui sont classifiés comme version bêta. Ces fonctions peuvent être instables, changer fréquemment ou être arrêtées dans un délai très court. Les fonctions bêta, qui risquent de ne pas fournir le même niveau de performance ou de compatibilité que les fonctions de l'édition GA (General Availability), ne sont pas destinées à être utilisées dans un environnement de production.

Activez la fonction *de vérification orthographique* pour corriger les erreurs d’orthographe commises par les utilisateurs dans les énoncés qu’ils soumettent en entrée. Lorsque la vérification orthographique est activée, les mots mal orthographiés sont automatiquement corrigés. Et ces mots corrigés sont utilisés pour évaluer l'entrée. Lorsqu'il reçoit des informations plus précises, le service peut reconnaître plus souvent les mentions d'entités et comprendre l'intention de l'utilisateur. 

Actuellement, ce paramètre peut être activé uniquement pour les compétences de dialogue en anglais.
{: note}

Avec la vérification orthographique activée, l'entrée utilisateur est corrigée de la manière suivante :

- Entrée initiale : `premette-moi de demader une adhéson`
- Entrée corrigée : `permettez-moi de demander une adhésion`

Lorsque le service détermine s'il faut corriger l'orthographe d'un mot, il ne repose pas sur un simple processus de recherche dans un dictionnaire. Au lieu de cela, il utilise une combinaison de traitement du langage naturel et de modèles probabilistes pour déterminer si un terme est mal orthographié et doit être corrigé.  

## Activation de la vérification orthographique 
{: #beta-spell-check-enable}

Pour activer la fonction de vérification orthographique, procédez comme suit : 

1.  Dans la page Skills, recherchez votre compétence.
1.  Cliquez sur l'icône ![ouvrir et fermer la liste d'options](images/kabob-beta.png), puis choisissez **Settings**. 
1.  Dans la page de paramètres *Spell Check*, activez **Spell check auto-correction**.

### Test de la correction orthographique 
{: #beta-spell-check-test}

1.  Dans le panneau "Try it out", soumettez un énoncé comprenant des mots mal orthographiés. 

    Si les mots de votre entrée sont mal orthographiés, ils sont automatiquement corrigés et une icône ![correction automatique](images/auto-correct.png) s'affiche. L'énoncé corrigé est souligné.
1.  Survolez l'énoncé souligné pour voir la formulation initiale. 

Si certains termes mal orthographiés doivent être corrigés par le service mais que ce n’est pas le cas, passez en revue les règles utilisées par le service pour décider de corriger un mot ou non, afin de vérifier si ce mot appartient à la catégorie de mots que le service ne modifie pas intentionnellement. 

Pour éviter la surcorrection, le service ne corrige pas l'orthographe des types d'entrée suivants :  

- Mots en majuscules 
- Emoticônes 
- Entités de localisation, telles que les départements et les adresses urbaines  
- Nombres et unités de mesure ou de temps  
- Noms propres, tels que prénoms communs ou noms de société  
- Texte entre guillemets  
- Mots contenant des caractères spéciaux, tels que les traits d'union (-), les astérisques (*) et les perluètes (&)
- Mots *appartenant* à cette compétence, ce qui signifie des mots ayant une signification implicite, car ils apparaissent dans les valeurs d'entité, les synonymes d'entité ou les exemples utilisateur d'intention. 

Si le mot qui n'est pas corrigé ne fait manifestement pas partie de ces types d'entrées, il peut être intéressant de vérifier si la correspondance partielle (Fuzzy Matching) est activée pour l'entité. 

#### Quel est le lien entre la correction orthographique et la correspondance partielle ?  
{: #beta-spell-check-vs-fuzzy-matching}

La correspondance partielle aide le service à reconnaître les mentions d'entités basées sur un dictionnaire dans les entrées utilisateur. Elle utilise une approche de recherche dans le dictionnaire pour faire correspondre un mot de l'entrée utilisateur à une valeur d'entité existante ou à un synonyme dans les données d'apprentissage de la compétence. Par exemple, si l'utilisateur entre `livres`, et que vos données d'apprentissage contiennent le synonyme d'entité `livre`, la correspondance partielle reconnaît que ces deux termes désignent la même chose.

Lorsque vous activez la vérification orthographique et la correspondance partielle, la fonction de correspondance partielle s'exécute avant le déclenchement de la vérification orthographique. Si elle trouve un terme pouvant correspondre à une valeur d'entité de dictionnaire existante ou à un synonyme, elle ajoute ce terme à la liste des mots qui *appartiennent* à la compétence, et ne doivent donc pas être corrigés. De même, si un utilisateur entre une phrase du type `Je souhaite acheter un livrre`, la correspondance partielle reconnaît que le terme `livrre` signifie la même chose que votre synonyme d'entité `livre` et l'ajoute à la liste des mots protégés. Par conséquent, le service *ne corrige pas* l’orthographe de `livrre`.

#### Fonctionnement
{: #beta-spell-check-how-it-works}

Normalement, l’entrée utilisateur est enregistrée telle quelle dans la zone `text` de l’objet `input` du message. Si et seulement si l'entrée utilisateur est corrigée d’une manière ou d’une autre, une zone est créée dans l’objet `input`, appelée `original_text`. Cette zone stocke l'entrée initiale de l'utilisateur qui comprend tous les mots mal orthographiés. Et le texte corrigé est ajouté à la zone `input.text`.
