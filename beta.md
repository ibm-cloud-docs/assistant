---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-29"

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

# Early access program
{: #beta}

When you participate in the early access program, IBM gives you early access to features for your evaluation. These features are classified as beta, which means they might be unstable, might change frequently, and might be discontinued with short notice. Beta features also might not provide the same level of performance or compatibility that generally available features provide and are not intended for use in a production environment. Beta features are supported only on the [IBM Developer Answers](https://developer.ibm.com/answers/topics/watson-assistant/){: external}.

## Available beta features
{: #beta-features}

The following features are available for use by participants in the early access program only. To find out how to request access, see [Participate in the early access program](/docs/services/assistant?topic=assistant-feedback#feedback-beta).
{: preview}

- The main menu options of **Assistants** and **Skills** have moved from being displayed at the top of the page to being shown as icons on the side of the page. <!--1.74-->

  ![Assistants menu icon](images/nav-ass-icon.png) Assistants
  ![Skills menu icon](images/nav-skills-icon.png) Skills

- The user interface controls that you use to create entity annotations on the Intents page have changed. You can now click the Annotate entities toggle to enable annotation, and then more easily pick one or more words to annotate. <!--1.73-->

- The user interface of the Entities and Intents <!--1.72 --> pages were updated to use a new JavaScript library that increases the page responsiveness. As a result, the look of some graphical user interface elements, such as buttons, changed slightly, but the function did not.

- Add your assistant as a Zendesk Chat agent. See [Integrating with Zendesk Chat](/docs/services/assistant?topic=assistant-deploy-zendesk).

- Try out the new irrelevant topic detection feature. When enabled, a supplemental model is used to help identify utterances that are irrelevant and should not be answered by the dialog skill. This new model is especially beneficial for skills that have not been trained on what subjects to ignore. For more information, see [New irrelevant topic detection](/docs/services/assistant?topic=assistant-irrelevance-detection).

- Add rich response types to dialog node slot prompts. For example, you can display a list of options for a user to choose from as the prompt for the slot. See [Adding rich responses to slots](dialog-slots-multimedia). <!--1.71 -->

- You can test out pubishing your assistant as a \[24\]7.ai chatbot. See [Testing a \[24\]7.ai chatbot integration](/docs/services/assistant?topic=assistant-deploy-247ai). <!--1.72 -->

- Deploy your assistant in minutes. Create a web chat integration to generate an HTML script element that you can copy and paste into a page on your website to embed the assistant as a chat widget. See [Integrating with your own website](/docs/services/assistant?topic=assistant-deploy-web-chat). <!--1.72 -->

## Tell us what you think

To share your feedback, join the [Watson Developer Community on Slack](http://wdc-slack-inviter.mybluemix.net/)(: external). Add your first impressions and suggestions to the **#assistant-early-access** channel.