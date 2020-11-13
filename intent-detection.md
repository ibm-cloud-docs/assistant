---

copyright:
  years: 2015, 2020
lastupdated: "2020-11-13"

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

# Improved intent recognition ![Beta](images/beta.png)
{: #intent-detection}

The new intent detection model improves your assistant's ability to understand what customers want.
{: shortdesc}

The new intent detection model is faster and more accurate. It combines traditional machine learning, transfer learning, and deep learning techniques in a cohesive model that is highly responsive at run time. The new model is highly effective in skills with a small training data set.

New English-language service instances use the new intent recognition model automatically. It is disabled for existing dialog skills. You can turn it on for your existing dialog skills to start taking advantage of the improved performance that it offers. However, be aware that switching this feature on in an existing dialog skill will result in noticeable changes to the confidence scores associated with your existing intents.

The new model is available as a beta feature for English dialog and actions skills only.
{: note}

## Enabling the new intent detection model
{: #intent-detection-task}

The new model is used by action skills automatically and cannot be disabled.

To enable the new intent detection model in a dialog skill, complete the following steps:

1.  From the Skills page, open your skill.
1.  From the skill menu, click **Options**.
1.  On the *Intent detection* page, choose **Enhanced version**.

Test the new intent detection capabilities in the "Try it out" pane. 

If you don't like the results that you get with the new model, you can switch back to using the classic version of the model. Alternatively, you can leave it on and make dialog changes to accommodate any differences in your assistant's behavior that you want to address.
