---

copyright:
  years: 2015, 2022
lastupdated: "2022-06-16"

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

{{site.data.content.newlink}}

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

## Irrelevance detection
{: #irrelevance-detection-enable}

The *irrelevance detection* feature helps your dialog skill recognize subjects that you do not want it to address, even if you haven't explicitly taught it about what to ignore. This feature helps your skill recognize irrelevant inputs earlier in the development process.

To test irrelevance detection in the "Try it out" pane, submit one or more utterances that have absolutely nothing to do with your training data. Irrelevance deteciton helps your skill to correctly recognize that the test utterances do not map to any of the intents defined in your training data, and classifies them as being `Irrelevant`.

The algorithmic models that help your assistant understand what your users say are built from two key pieces of information:

- Subjects you want the assistant to address. For example, questions about order shipments for an assistant that manages product orders.

    You teach your assistant about these subjects by defining intents and providing lots of sample user utterances that articulate the intents so your assistant can recognize these and similar requests as examples of input for it to handle.

- Subjects you want the assistant to ignore. For example, questions about politics for an assistant that makes pet grooming appointments exclusively.

    You teach your assistant about subjects to ignore by marking utterances that discuss subjects which are out of scope for your application as being irrelevant. Such utterances become counterexamples for the model.

The best way to build an assistant that understands your domain and the specific needs of your customers is to take the time to build good training data.

## Counterexample limits
{: #irrelevance-detection-limits}

The maximum number of counterexamples that you can create for any plan type is 25,000.
