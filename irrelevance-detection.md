---

copyright:
  years: 2015, 2020
lastupdated: "2020-10-09"

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

# Defining what's irrelevant
{: #irrelevance-detection}

Teach your dialog skill to recognize when a user asks about topics that it is not designed to answer.
{: shortdesc}

To teach your assistant about subjects it should ignore, you can review your user conversation logs to mark utterances that discuss off-topic subjects as irrelevant.

Intents that are marked as irrelevant are saved as counterexamples in the JSON workspace, and are included as part of the training data. They teach your assistant to explicitly not answer utterances of this type.

While testing your dialog, you can mark an intent as irrelevant directly from the *Try it out* pane.

![Mark as irrelevant screen capture](images/irrelevant.png)

Be certain before you designate an input as irrelevant.

- There is no way to access or change the inputs from the user interface later.
- The only way to reverse the identification of an input as being irrelevant is to use the same input in a test integration channel, and then explicitly assign it to an intent.

Often there are subjects that you expect customers to ask and that you want your assistant to address eventually, but that you aren't ready to fully implement yet. Instead of adding those topics as counterexamples which can be hard to find later, capture the customer input examples as new intents. But don't link dialog nodes to the intents until you're ready. If customers ask about one of these topics in the meantime, the anything_else node is triggered to explain that the assistant can't help with the current request, but can help them with other things.

## Enabling irrelevance detection
{: #irrelevance-detection-enable}

The *irrelevance detection* feature helps your dialog skill recognize subjects that you do not want it to address, even if you haven't explicitly taught it about what to ignore. This feature helps your skill recognize irrelevant inputs earlier in the development process.

To learn more about the benefits of this feature, read the [Why Zero-Effort Irrelevance is Relevant](https://medium.com/ibm-watson/enhanced-offtopic-90b2dadf0ef1){: external} blog post.

Irrelevance detection is enabled automatically for all new English-language dialog skills only. It is disabled for existing skills. You might choose not to enable it if you already dedicated time and effort to training your skill to recognize irrelevant user inputs.

To enable the enhanced irrelevance detection feature, complete the following steps:

1.  From the Skills page, open your skill.
1.  From the skill menu, click **Options**.
1.  On the *Irrelevance detection* page, choose **Enhanced**.

To test the new detection mechanism in the "Try it out" pane, submit one or more utterances that have absolutely nothing to do with your training data. The new mechanism helps your skill to correctly recognize that the test utterances do not map to any of the intents defined in your training data, and classifies them as being `Irrelevant`.

## How irrelevance detection works
{: #irrelevance-detection-how-it-works}

The algorithmic models that help your assistant understand what your users say are built from two key pieces of information:

- Subjects you want the assistant to address. For example, questions about order shipments for an assistant that manages product orders.

  You teach your assistant about these subjects by defining intents and providing lots of sample user utterances that articulate the intents so your assistant can recognize these and similar requests as examples of input for it to handle.
- Subjects you want the assistant to ignore. For example, questions about politics for an assistant that makes pet grooming appointments exclusively.

  You teach your assistant about subjects to ignore by marking utterances that discuss subjects which are out of scope for your application as being irrelevant. Such utterances become counterexamples for the model. 

The best way to build an assistant that understands your domain and the specific needs of your customers is to take the time to build good training data, especially data that includes counterexamples.

Irrelevance detection is designed to bridge any gaps you might have in your counterexample data as you start to build your skill. When enabled, an alternative method for evaluating the relevance of a newly submitted utterance is triggered in addition to the standard method. 

The supplemental method examines the structure of the new utterance and compares it to the structure of the user example utterances in your training data. This alternate approach helps skills that have few or no counterexamples recognize irrelevant utterances. It is likely to have less of an effect for skills that have a sufficient number of counterexamples defined already. 

Note that the new method relies on structural information that is based on data from outside your skill. So, while the new method can be useful as you are starting out, to build an assistant that provides a more customized experience, you want it to use information from data that is derived from within the application's domain. The way to ensure that your assistant does so is by adding your own counterexamples.

## Counterexample limits
{: #irrelevance-detection-limits}

The maximum number of counterexamples that you can create for any plan type is 25,000.
