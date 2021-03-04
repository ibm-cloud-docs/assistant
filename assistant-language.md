---

copyright:
  years: 2015, 2021
lastupdated: "2021-03-02"

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

# Adding support for global audiences
{: #assistant-language}

Your customers come from all around the globe. You need an assistant that can talk to them in their own language and in a familiar style. Choose the approach that best fits your business needs.

- **Quickest solution**: The simplest way to add language support is to author the conversational skill in a single language. You can translate each message that is sent to your assistant from the customer's local language to the skill language. Later you can translate each response from the skill language back to the customer's local language.

  This approach simplifies the process of authoring and maintaining the conversational skill. You can build one skill and use it for all languages. However, the intention and meaning of the customer message can be lost in the translation.

  For more information about webhooks you can use for translation, see [Webhook overview](https://cloud.ibm.com/docs/assistant?topic=assistant-webhook-overview).

- **Most precise solution**: If you have the time and resources, the best user experience can be achieved when you build multiple conversational skills, one for each language that you want to support. {{site.data.keyword.conversationshort}} has built-in support for all languages. Use one of 13 language-specific models or the universal model, which adapts to any other language you want to support.

  When you build a skill that is dedicated to a language, a language-specific classifier model is used by the skill. The precision of the model means that your assistant can better understand and recognize the goals of even the most colloquial message from a customer.

  Use the new universal language model to create an assistant that is fluent even in languages that {{site.data.keyword.conversationshort}} doesn't support with built-in models.

  To deploy, attach each language skill to its own assistant that you can deploy in a way that optimizes its use by your target audience.

  For example, use the web chat integration with your French-speaking assistant to deploy to a French-language page on your website. Deploy your German-speaking assistant to the German page of your website. Maybe you have a support phone number for French customers. You can configure your French-speaking assistant to answer those calls, and configure another phone number that German customers can use.

## Understanding the universal language model ![Beta](images/beta.png) 
{: #assistant-language-universal}

A skill that uses the universal language model applies a set of shared linguistic characteristics and rules from multiple languages as a starting point. It then learns from the training data that you add to it.

The universal language model is available as a beta feature.
{: note}

The universal language classifier can adapt to a single language per skill. It cannot be used to support multiple languages within a single skill. However, you can use the universal language model in one skill to support one language, Russian, for example, and in another skill to support another language, such as Hindi. The key is to add enough training examples or intent user examples in your target language to teach the model about the unique syntactic and grammatical rules of the language.

Use the universal language model when you want to create a conversation in a language for which no dedicated language model is available. And the language is unique enough that an existing model is insufficient.

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

<!--## How locale information is used
{: #assistant-language-locale}

Locale information is used by the system entities to recognize localized mentions of things like dates, times, and currency.

The conversational skill assigns a locale setting based on the language that you choose to use for the skill. Therefore, if a dedicated language model exists for your preferred language, you don't need to specify a locale yourself. 

For example, if you set the skill language to French, the assistant can recognize European date and time formats, and mentions of relevant currencies, such as euros, francs, Swiss francs, US dollars, and others.

The locale for the universal model is specified as `xx`. A few characteristics of the initial model include:

- Default date format: `DDMMYYYY`
- Supported currencies: US dollars, Canadian dollars, Australian dollars, Swiss francs, British pounds, euros, Indian rupees, Japanese yen, Mexican pesos, Russian rubles.

There are situations in which you might want to specify the locale yourself. These include:

- When the skill language is English

  If you choose to create an English skill, the American English (`en-us`) locale is used automatically. As a result, dates and times are formatted for American customers. If your audience is mostly British English speakers or other European English speakers, setting the locale to `en-gb` can be beneficial. The `en-gb` locale changes the date format to `DDMMYYYY`, and times to the 24 hour format, for example. For a Canadian audience, you can change the locale to `en-ca`.
- If you choose to use the universal language model in your skill

  If you are creating a conversation in one of the following languages, you might want to specify a locale setting also:

  - Danish (da-dk)
  - Hebrew (iw-il)
  - Hindi (hi-in): support for number recognition only
  - Swedish (sv-se)
  - Slovak (sk-sk)
  - Turkish (tr-tr): limited support for date and time recognition

  When you specify one of these locales, you can leverage support that is provided (in varying degrees) by the system entities to recognize date, time, number, currency, and percent mentions that are specified in the language.
  
  For example, if you want to interact with Swedish-speaking customers, you can choose the universal language model as the skill language. You can also specify the `sv-se` locale to leverage the system entity support for Swedish currencies and date formats.

  If you are creating a conversation in another language but one for which dates and times are referenced in a similar manner to one of the built-in language models, specify a locale to take advantage of the built-in support. For example, you might use the universal language model to create a conversational skill in Catalan. But, you can set the locale to `es-es` to leverage system entity support for Spanish language date and number mentions.

To specify a locale setting, complete the following steps:

1.  In a dialog skill, add a context variable at the start of the dialog that sets the locale. 

    For example, you can add a node before the welcome node that uses the `conversation_start` special condition. In this node, if you want to apply a Swedish locale (`sv-se`) to the conversation, add a context variable like this:

    <table>
      <caption>Locale context variable</caption>
      <tr>
      <th>Name</th>
      <th>Value</th>
      </tr>
      <tr>
      <td>system</td>
      <td>`{"locale":"sv-se"}`</td>
      </tr>
      </table>

    For more information about the `conversation_start` dialog node, see [Starting and ending the dialog](/docs/assistant?topic=assistant-dialog-start).

    For more information about context variables, see [Personalizing the dialog with context](/docs/assistant?topic=assistant-dialog-runtime-context).

1.  To specify the locale for v2 api so it is applied to the built-in integrations, TBD. See issue 44534.-->
