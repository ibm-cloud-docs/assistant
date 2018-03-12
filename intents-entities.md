---

copyright:
  years: 2015, 2017
lastupdated: "2017-12-05"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Planning your intents and entities
{: #planning-your-entities}

To plan the *intents* for your application, you need to consider what your customers might want to do, and what you want your application to be able to handle. Choosing the correct intent for a user's input is the first step in providing a useful response. The intents you identify for your application will determine the dialog flows you need to create; they also might determine which back-end systems your application needs to integrate with in order to complete customer requests (such as customer databases or payment-processing systems).

An *entity* represents a term or object in the user's input that provides clarification or specific context for a particular intent. If intents represent verbs (something a user wants to do), entities represent nouns (such as the object of, or the context for, an action). Entities make it possible for a single intent to represent multiple specific actions. An entity defines a class of objects, with specific values representing possible objects in that class.

Essentially, if you want to capture a request, or perform an action, use an intent. If you want to capture information that may affect how you respond to a request or action, use an entity.

As an example, suppose you want to create a cognitive car dashboard application that enables users to turn accessories on or off:

1.  Gather as many actual customer questions, commands, or other inputs as possible. Using input from real users gives a better picture of the expected input than having experts create lists of possible utterances. Remember that customers might phrase the same kind of request in many different ways. For example, the following example utterances all represent requests to turn something off:

    - `Headlights off`
    - `Turn the radio off`
    - `Stop the air conditioner`

1.  Once you have a list of examples, sort them into categories based on the capabilities you want your application to support; these categories represent the intents you will define, while the examples will help your application to identify those intents in new input.

    Do not make your intents too similar. Similar intents can be difficult for the {{site.data.keyword.conversationshort}} service to distinguish. If you find that you have several intents that are close in meaning, consider whether you could combine them into a single intent, and then use entities to provide multiple possible responses for that intent.
    {: tip}

    Since the previous examples all represent the same intent (to turn something off), you might call this category `#turn_off`.
    Remember that a customer's input does not have to be an exact match for any of your examples; intents are recognized using natural-language processing.
    {: tip}

1.  You would then use an entity to represent what the user wants to turn off. To do this, you might create an entity called `@accessory`, and give it the following possible values:

    - `headlights`
    - `radio`
    - `air conditioner`

    For each value, you can also specify synonyms. For example, you might specify `AC` and `cooling` as synonyms for `air conditioner`; all three strings are treated as the same value. Listing multiple ways that users might refer to the same value helps improve your application's accuracy.
1.  When the user's input is received, the conversation recognizes both intents and entities. Your dialog flow can then use these to provide the best answer. You should only create an entity for something that matters in terms of changing the way the application responds to an intent.
    In addition to the entities you define, the service provides a set of system entities that are already defined and available for you to use. For more information, see [Enabling system entities](entities.html#enable_system_entities).
    {: tip}

1.  Continue to refine your intents, entities, and examples as needed. Do not think of your set of intents and entities as finished products. It is likely that when you [build your dialogs](dialog-build.html), you will identify additional intents and entities that you need to add. You can also continue to gather input from new customers, and use it to add new examples; this iterative process improves your application's ability to recognize intents and entities accurately.
