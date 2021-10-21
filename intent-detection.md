---

copyright:
  years: 2015, 2021
lastupdated: "2021-09-17"

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

# Improved intent recognition ![Beta](images/beta.png)
{: #intent-detection}

The new intent detection model improves your assistant's ability to understand what customers want.
{: shortdesc}

The new intent detection model is faster and more accurate. It combines traditional machine learning, transfer learning, and deep learning techniques in a cohesive model that is highly responsive at run time. The new model is highly effective in skills with a small training data set.

New service instances use the new intent recognition model automatically. It is disabled for existing dialog skills. You can turn it on for your existing dialog skills to start taking advantage of the improved performance that it offers. However, be aware that switching this feature on in an existing dialog skill will result in noticeable changes to the confidence scores associated with your existing intents.

You can learn more about the model by reading the [Watson Assistant improves intent detection accuracy, leads against AI vendors cited in published study](https://www.ibm.com/blogs/watson/2020/12/watson-assistant-improves-intent-detection-accuracy-leads-against-ai-vendors-cited-in-published-study/){: external} blog post, which has a link to a downloadable benchmarking paper.

The new model is available for:
-  English, French, Italian, and Spanish dialog skills
-  Actions skills

## Enabling the new intent detection model
{: #intent-detection-task}

The new model is the only model that is available for use in actions skills and skills that use the universal language model.
{: note}

To enable the new intent detection model in a dialog skill (English, French, Italian, or Spanish), complete the following steps:

1.  From the Skills page, open your dialog skill.
1.  From the skill menu, click **Options**.
1.  On the *Intent detection* page, choose **Enhanced version**.

If you use the dialog skill options to choose enhanced intent detection, it is automatically paired with enhanced irrelevance detection. If you use the dialog skill options to choose existing intent detection, it is automatically paired with existing irrelevance detection. For more information, see [Defining what's irrelevant](/docs/assistant?topic=assistant-irrelevance-detection).
    
If necessary, you can use the [Update workspace API](/apidocs/assistant/assistant-v1?curl=#updateworkspace) to set your English-language assistant to one of the four combinations of intent and irrelevance detection:

- Enhanced intent recognition and enhanced irrelevance detection
- Enhanced intent recognition and existing irrelevance detection
- Existing intent recognition and enhanced irrelevance detection
- Existing intent recognition and existing irrelevance detection

For French, Italian, and Spanish, you can use the API to set your assistant to these combinations:
  - Enhanced intent recognition and enhanced irrelevance detection
  - Existing intent recognition and existing irrelevance detection

Test the new intent detection capabilities in the "Try it out" pane. 

If you don't like the results that you get with the new model, you can switch back to using the classic version of the model in your dialog skill. Alternatively, you can leave it on and make dialog changes to accommodate any differences in your assistant's behavior that you want to address.
