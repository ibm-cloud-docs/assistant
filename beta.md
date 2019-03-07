---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-07"

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

# Beta
{: #beta}

![Beta](images/beta.png) IBM releases services, features, and language support for your evaluation that are classified as beta. These features might be unstable, might change frequently, and might be discontinued with short notice. Beta features also might not provide the same level of performance or compatibility that generally available features provide and are not intended for use in a production environment. Beta features are supported only on the [IBM Developer Answers ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/watson-assistant/){: new_window}.

## Available beta features
{: #beta-features}

The following features are available for use by participants in the beta program only. To find out how to request access, see [Participate in the beta program](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

- How you work with search skills has changed. You can now add one search skill and one dialog skill to the same assistant. When you add both, the search is triggered if the user input cannot be addressed by any of the nodes in the dialog of the dialog skill. You can learn more from the following topics:

  - [Search skill](/docs/services/assistant?topic=assistant-skill-search-add)
  - [Dialog skill](/docs/services/assistant?topic=assistant-skill-dialog-add)

  When this feature is released, it will be available to Plus or Premium plan users only.

- The user interface of the Dialog builder has been updated to use the React JavaScript library. Dialog functions are now provided in encapsulated components that manage their own state, which results in a more responsive user experience.

- You can configure a skill to correct misspellings in user input. See [Correcting user input](/docs/services/assistant?topic=assistant-dialog-runtime-spell-check) for more details.

- Leverage existing customer support chat transcripts to find the appropriate set of intents and user examples to use to train your assistant. See [Get help building intents](/docs/services/assistant?topic=assistant-intent-recommendations#intent-recommendations-get-intent-recommendations) for more details.

- Try out the new system entities. The number-based system entities have been revised to be better able to recognize date, time, and number mentions. See [New system entities](/docs/services/assistant?topic=assistant-beta-system-entities) for more information.
