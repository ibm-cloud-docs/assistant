---

copyright:
  years: 2015, 2019
lastupdated: "2019-10-31"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:pre: .pre}
{:preview: .preview}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:gif: data-image-type='gif'}

# Beta: New irrelevance detection
{: #irrelevance-detection}

Enable the new irrelevant topic detection feature to help your dialog skill recognize when a user asks about topics that it is not designed to answer, and to do so with confidence earlier in the development process.
{: shortdesc}

This feature is available for use by participants in the early access program only. To find out how to request access, see [Participate in the early access program](/docs/services/assistant?topic=assistant-feedback#feedback-beta).
{: preview}

When you participate in the early access program, IBM gives you early access to features for your evaluation. These features are classified as beta, which means they might be unstable, might change frequently, and might be discontinued with short notice. Beta features also might not provide the same level of performance or compatibility that generally available features provide and are not intended for use in a production environment. Beta features are supported only on the [IBM Developer Answers](https://developer.ibm.com/answers/topics/watson-assistant/){: external}.

This feature helps your dialog skill recognize subjects that you do not want it to address, even if you haven't explicitly taught it about what to ignore by marking specific user utterances as irrelevant.

This setting can be enabled for English-language dialog skills only.
{: note}

## Enabling the new irrelevance detection feature
{: #irrelevance-detection-enable}

To enable the enhanced irrelevant intent detection feature, complete the following steps:

1.  From the Skills page, open your skill.
1.  From the skill menu, click **Options**.
1.  On the *Irrelevance detection* page, choose **Enhanced**.

To test the new detection mechanism in the "Try it out" pane, submit one or more utterances that have absolutely nothing to do with your training data. The new mechanism helps your skill to correctly recognize that the test utterances do not map to any of the intents defined in your training data, and classifies them as being `Irrelevant`.

## How detection works
{: #irrelevance-detection-how-it-works}

The algorithmic models that help your assistant understand what your users say are built from two key pieces of information:

- Subjects you want the assistant to address. For example, questions about order shipments for an assistant that manages product orders.

  You teach your assistant about these subjects by defining intents and providing lots of sample user utterances that articulate the intents so your assistant can recognize these and similar requests as examples of input for it to handle.
- Subjects you want the assistant to ignore. For example, questions about politics for an assistant that makes pet grooming appointments exclusively.

  You teach your assistant about subjects to ignore by marking utterances that discuss subjects which are out of scope for your application as being irrelevant. Such utterances become counterexamples for the model. 

The best way to build an assistant that understands your domain and the specific needs of your customers is to take the time to build good training data, especially data that includes counterexamples.

To bridge any gaps you might have in your counterexample data as you start to build your skill, you can enable this new irrelevant intent detection feature. When enabled, an alternative method for evaluating the relevance of a newly submitted utterance is triggered in addition to the standard method. 

The supplemental method examines the structure of the new utterance and compares it to the structure of the user example utterances in your training data. This alternate approach helps skills that have few or no counterexamples recognize irrelevant utterances. It is likely to have less of an effect for skills that have a sufficient number of counterexamples defined already. 

Note that the new method relies on structural information that is based on data from outside your skill. So, while the new method can be useful as you are starting out, to build an assistant that provides a more customized experience, you want it to use information from data that is derived from within the application's domain. The way to ensure that your assistant does so is by adding your own counterexamples. For more information, see [Teaching your skill about topics to ignore](/docs/services/assistant?topic=assistant-logs#logs-mark-irrelevant).
