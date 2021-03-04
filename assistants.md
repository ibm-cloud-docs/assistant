---

copyright:
  years: 2018, 2021
lastupdated: "2021-03-04"

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

# Planning your assistant
{: #assistants}

Build an assistant that meets the needs of your customers with speed and ease.
{: shortdesc}

To start, consider these factors and make key decisions up front that will keep you on track as you build.

## Pick your assistant's areas of expertise
{: #assistants-plan-intents}

Decide what you want your assistant to specialize in. What questions or tasks will it help customers with? To make an informed decision, review any support call logs that you have access to.

Start small. Pick one or a handful of customer issues that will deliver the highest value to start. 

It might be valuable for your assistant to answer a simple question that is asked all the time. Or maybe there's a task, such as scheduling appointments, that can be offloaded to the assistant to tackle 30% of all incoming customer requests.

Choose a narrow set of user goals first. After your assistant is live, you can use built-in tools to gain insights from the incoming traffic that will tell you what areas to focus on next.

## Give the right type of answer to meet the need
{: #assistants-plan-intents}

A conversational exchange is what your assistant does best. But, your assistant can do other things too. The best response to a question might be a single answer with a link somewhere else. Think about the right way to answer customer questions; don't try to fit everything into one type of conversational exchange.

Here are some examples:

- The customer wants information about your store location. Your assistant answers with text (the store address) and an image (an area map).
- The customer wants to activate a credit card. Your assistant can use a conversational flow and a webhook call to verify the user's identity, and then submit the request to activate the card on the user's behalf.
- The customer wants to complete a simple task that involves a complicated application. Your assistant can link them to a 2-minute video that illustrates how to complete the task.
- The customer has a question about an insurance plan after the death of a loved one. Your assistant can connect the customer directly to a person who can show empathy and patience as the matter is addressed.
- The customer has a problem that requires a long and involved procedure to fix. Instead of trying to walk the customer through the procedure step-by-step in conversation, your assistant can link to a help center that documents the full procedure in detail.
- The customer calls support and your assistant answers. The assistant needs a lot of detail from the customer before it can help. Instead of trying to prompt the customer for each piece of information and transcribe it properly, your assistant can switch to SMS text messaging. After years of interacting with bad interactive voice response systems, many customers are more likely to yell `Agent` over and over than to engage in a long exchange. But, if you give them a chance to explain something in writing, they do so willingly.

## How will customers find your assistant?
{: #assistants-plan-deployment}

Deploy your assistant where customers can find it with ease. For example, you can embed the assistant in your company website or add it to a messaging platform such as Facebook, Slack, or WhatsApp.

While the conversational skill is defined in text, customers can talk with your assistant over the phone when you deploy it by using the phone integration.

For more information about ways to deploy the assistant, see [Adding integrations](/docs/assistant?topic=assistant-deploy-integration-add).

Understanding where you will deploy the assistant before you begin can help you author the right types of answers for a given channel or platform.

## What languages does your assistant speak?
{: #assistants-plan-global}

Decide whether and how you want to handle more than one spoken language. For more information about ways to approach language support, see [Adding support for global audiences](/docs/assistant?topic=assistant-language).

## Does your assistant see the glass as half full?
{: #assistants-plan-personality}

Is your assistant an optimist or a pessimist, a shy intellectual type, or an upbeat sidekick? Choose a personality for your assistant, and then write conversations that reflect that personality. Don't overdo it. Don't sacrifice usability for the sake of keeping your assistant in character. But, strive to present a consistent tone and attitude.

## Choose a conversational skill type
{: #assistants-plan-conversation}

*Which conversational skill type should I use?*

Use both. Leverage advanced capabilities that are available from a dialog skill and build individual actions to perform finite tasks that you want to support. You can call the actions from your dialog skill.

For more information, see [Choosing a conversational skill](/docs/assistant?topic=assistant-skills-choose).

## Do you have existing content to leverage?
{: #assistants-plan-search}

The answer to many a common question is already documented somewhere in your organization's technical information collateral. If only you could find it! 

Give your assistant access to this information by adding a search skill to your assistant. The search skill uses {{site.data.keyword.conversationshort}} to return smart answers to natural language questions.

## Expand its responsibilities
{: #assistants-plan-expand}

If you start small, and choose the goals with the highest impact first, you'll have room and time to grow the expertise of your assistant. The built-in metrics of active user conversations help you understand what your customers are asking about and how well your assistant is able to meet their needs. 

Nothing beats real customer data. It will tell you what areas to tackle next.

See [Creating assistants](/docs/assistant?topic=assistant-assistant-add) to get started.