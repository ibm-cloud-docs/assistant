---

copyright:
  years: 2015, 2021
lastupdated: "2021-06-16"

keywords: global support, universal language, universal model, another language

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
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:gif: data-image-type='gif'}
{:video: .video}

{{site.data.content.newlink}}

# Adding support for global audiences
{: #assistant-language}

Your customers come from all around the globe. You need an assistant that can talk to them in their own language and in a familiar style. Choose the approach that best fits your business needs.

- **Quickest solution**: The simplest way to add language support is to author the conversational skill in a single language. You can translate each message that is sent to your assistant from the customer's local language to the skill language. Later you can translate each response from the skill language back to the customer's local language.

  This approach simplifies the process of authoring and maintaining the conversational skill. You can build one skill and use it for all languages. However, the intention and meaning of the customer message can be lost in the translation.

  For more information about webhooks you can use for translation, see [Webhook overview](https://cloud.ibm.com/docs/assistant?topic=assistant-webhook-overview).

- **Most precise solution**: If you have the time and resources, the best user experience can be achieved when you build multiple conversational skills, one for each language that you want to support. {{site.data.keyword.assistant_classic_short}} has built-in support for all languages. Use one of 13 language-specific models or the universal model, which adapts to any other language you want to support.

  When you build a skill that is dedicated to a language, a language-specific classifier model is used by the skill. The precision of the model means that your assistant can better understand and recognize the goals of even the most colloquial message from a customer.

  Use the new universal language model to create an assistant that is fluent even in languages that {{site.data.keyword.assistant_classic_short}} doesn't support with built-in models.

  To deploy, attach each language skill to its own assistant that you can deploy in a way that optimizes its use by your target audience.

  For example, use the web chat integration with your French-speaking assistant to deploy to a French-language page on your website. Deploy your German-speaking assistant to the German page of your website. Maybe you have a support phone number for French customers. You can configure your French-speaking assistant to answer those calls, and configure another phone number that German customers can use.

## Understanding the universal language model 
{: #assistant-language-universal}

A skill that uses the universal language model applies a set of shared linguistic characteristics and rules from multiple languages as a starting point. It then learns from the training data that you add to it.

The universal language classifier can adapt to a single language per skill. It cannot be used to support multiple languages within a single skill. However, you can use the universal language model in one skill to support one language, such as Russian, and in another skill to support another language, such as Hindi. The key is to add enough training examples or intent user examples in your target language to teach the model about the unique syntactic and grammatical rules of the language.

Use the universal language model when you want to create a conversation in a language for which no dedicated language model is available, and which is unique enough that an existing model is insufficient.

For more information about feature support in the universal language model, see [Supported languages](/docs/assistant?topic=assistant-language-support).

## Creating a skill that uses the universal language model
{: #assistant-language-universal-task}

To create a skill that uses the universal language model, complete the following steps:

1.  From the *Skills* page, click **Create skill**.

1.  Choose to create either a dialog or actions skill, and then click **Next**.

    For more information, see [Choosing a conversational skill](/docs/assistant?topic=assistant-skills-choose). 

1.  Name your skill, and optionally add a description. From the *Language* field, choose **Another language**.

    Remember, if the language you want to support is listed individually, choose it instead of using the universal model. The built-in language models provide optimal language support.
    {: tip}

1.  Create the skill.

1.  If your skill will support a language with right-to-left directional text, such as Hebrew, configure the bidirectional capabilities.

    Click the options icon from the skill tile ![Skill options icon](images/kebab.png), and then select **Language preferences**. Click the *Enable bidirectional capabilities* switch. Specify any settings that you want to configure. 
    
    For more information, see [Configuring bidirectional languages](/docs/assistant?topic=assistant-language-support#language-support-configure-bidirectional).

Next, start building your conversation.

As you follow the normal steps to design a conversational flow, you teach the universal language model about the language you want your skill to support. It is by adding training data that is written in the target language that the universal model is constructed. For an actions skill, add actions. For a dialog skill, add intents, intent user examples, and entities. The universal language model adapts to understand and support your language of choice.

## Integration considerations
{: #assistant-language-integrations}

When your skill is ready, you can add it to an assistant and deploy it. Keep these tips in mind:

- **Phone integration**: If you want to deploy an assistant that uses the universal language model with the phone integration, you must connect to custom Speech service language models that can understand the language you're using. For more information about supported language models, see the [Speech to Text](https://cloud.ibm.com/docs/speech-to-text?topic=speech-to-text-models#modelsList){: external} and [Text to Speech](https://cloud.ibm.com/docs/text-to-speech?topic=text-to-speech-voices#languageVoices){: external} documentation.
- **Search skill**: If you build an assistant that specializes in a single language, be sure to connect it to data collections that are written in that language. For more information about the languages that are supported by {{site.data.keyword.discoveryshort}}, see [Language support](/docs/discovery-data?topic=discovery-data-language-support){: external}.
- **Web chat**: Web chat has some hardcoded strings that you can customize to reflect your target language. For more information, see [Global audience support](https://cloud.ibm.com/docs/assistant?topic=assistant-web-chat-basics#web-chat-basics-global).


