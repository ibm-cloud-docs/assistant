---

copyright:
  years: 2015, 2018
lastupdated: "2018-02-13"

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

# Onglet Recommendations
Cet onglet présente les solutions recommandées pour améliorer votre système.
{: shortdesc}

![Onglet Recommendations](images/RecommendTop.png)

Cette fonction existe en version bêta uniquement.
{: tip}

Cette fonction est disponible uniquement pour les utilisateurs Premium.
{: tip}

Lorsque vous analysez les conversations entre des utilisateurs et votre espace de travail et prenez en considération les données d'apprentissage et le degré de certitude des réponses actuels de votre système, vous obtenez des actions que vous pouvez facilement exécuter afin d'améliorer efficacement votre espace de travail.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/scMu66AvZtY" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

Les recommandations sont générées la nuit et nécessitent un volume important de messages utilisateur, par exemple, plus de 50.
{: tip}

## Recommandation Improve existing intents
Cette recommandation consiste à extraire des phrases individuelles entrées par des utilisateurs et non reconnues par le système et à vous les présenter pour que vous puissiez sélectionner une intention pour chaque phrase. Cela aidera votre espace de travail à mieux comprendre ce que vos utilisateurs se disent.

Cliquez sur **Start** pour commencer à identifier des intentions. ![Page Improve existing intents](images/rec_improve_intent.png)

Lorsque vous accédez à ou quittez la page **Improve existing intents**, la barre de progression affiche le nombre de phrases sur lesquelles vous êtes intervenu dans la session en cours, par rapport au nombre total de phrases consignées pendant la journée. Notez que si vous quittez et accédez de nouveau à cette page, la barre de progression repart de `zéro`. Cela ne signifie pas que votre travail précédent à été perdu, simplement, il ne sera pas pris en compte dans la progression de la session en cours. 

Sélectionnez la meilleure intention pour une phrase dans la liste fournie ou sélectionnez *Mark as irrelevant*. Les phrases sont ajoutées aux intentions en tant qu'exemples (ajoutées en tant que données d'apprentissage) dès que vous cliquez sur **Save**.

Le bouton *Skip to Next* vous permet d'ignorer la phrase en cours et de passer à la suivante. La phrase ignorée n'apparaît plus si vous quittez et accédez de nouveau à la page **Improve existing intents** au cours de la même journée, mais elle est susceptible de réapparaître les jours suivants.

![Page Improve existing intents - section d'édition](images/rec_improve_intent2.png)
