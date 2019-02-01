---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-01"
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

# Beta
{: #beta}

![Beta](images/beta.png) IBM releases services, features, and language support for your evaluation that are classified as beta. These features might be unstable, might change frequently, and might be discontinued with short notice. Beta features also might not provide the same level of performance or compatibility that generally available features provide and are not intended for use in a production environment. Beta features are supported only on the [IBM Developer Answers ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/watson-assistant/){: new_window}.

## Available beta features
{: #beta-features}

- You can now save multiple versions of a dialog skill.

  To take a snapshot of the current training data and dialog, click **Save new version**. See [Creating skill versions](beta-versions.html) for more details.

- How you work with search skills has changed. You can now add one search skill and one dialog skill to the same assistant. When you add both, the search is triggered if the user input cannot be addressed by any of the nodes in the dialog of the dialog skill. You can learn more from the following topics:
  - [Search skill](beta-skill-search-add.html)
  - [Dialog skill](beta-skill-dialog-add.html)

  When this feature is released, it will be available to Plus or Premium plan users only.

- [Intercom integration](beta-deploy-intercom.html)

  When this feature is released, it will be available to Plus or Premium plan users only.

- The user interface of the Dialog builder has been updated to use the React JavaScript library. Dialog functions are now provided in encapsulated components that manage their own state, which results in a more responsive user experience.

- You can configure a skill to correct misspellings in user input. See [Correcting user input](beta-spell-check.html) for more details.