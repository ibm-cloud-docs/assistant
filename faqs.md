---

copyright:
  years: 2015, 2020
lastupdated: "2020-01-27"

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
{:faq: data-hd-content-type='faq'}

# FAQ
{: #faqs}

Find answers to frequently-asked questions and quick fixes for common problems. 
{: shortdesc}

## What's a...
{: #faqs-answers}
{: faq}

| Term | Definition |
|------|------------|
| Assistant | Container for your skills. You add skills to an assistant, and then deploy the assistant when you are ready to start helping your customers. [Learn more](/docs/services/assistant?topic=assistant-assistants). |
| Content catalog | A set of prebuilt intents that are categorized by subject, such as customer care. You can add these intents to your skill and start using them immediately. Or you can edit them to complement other intents that you create. [Learn more](/docs/services/assistant?topic=assistant-catalog). |
| Context variable | A variable that you can use to collect information at the start of a conversation, and reference it later in the same conversation. For example, you might want to ask for the customer's name and then address the person by name later on. [Learn more](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-variables).|
| Dialog | The component where you build the conversation that your assistant has with your customers. For each defined intent, you can author the response your assistant should return. [Learn more](/docs/services/assistant?topic=assistant-dialog-overview). |
| Digression | A feature that gives the user the power to direct the conversation. It prevents customers from getting stuck in a dialog thread; they can switch topics whenever they choose. [Learn more](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions). |
| Disambiguation | A feature that enables the assistant to ask customers to clarify their meaning when the assistant isn't sure what a user wants to do next. [Learn more](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation). |
| Entity | Information in the user input that is related to the user's purpose. Intents represent the action a user wants to do. Entities represent the object of that action. [Learn more](/docs/services/assistant?topic=assistant-entities). |
| Intent | The goal that is expressed in the user input, such as answering a question or processing a bill payment. [Learn more](/docs/services/assistant?topic=assistant-intents) |
| Skill | Does the work of the assistant. A dialog skill has the training data and dialog that your assistant uses to chat with customers. A search skill is configured to search the appropriate external data sources for answers to customer inquiries. [Learn more](/docs/services/assistant?topic=assistant-skills). |
| Skill version | Versions are snapshots of a skill that you can create at key points during the development lifecycle. You can deploy one version to production, while you continue to make and test improvements that you make to another version of the skill. [Learn more](/docs/services/assistant?topic=assistant-versions). |
| Slots | A special set of fields that you can add to a dialog node that enable the assistant to collect necessary pieces of information from the customer. For example, the assistant can require a customer to provide valid date and location details before it gets weather forecast information on the customer's behalf. [Learn more](/docs/services/assistant?topic=assistant-dialog-slots). |
| System entity | Prebuilt entities that recognize references to common things like dates and numbers. You can add these to your skill and start using them immediately. [Learn more](/docs/services/assistant?topic=assistant-system-entities). |
| Webhook | A mechanism for calling out to an external program as part of the dialog. For example, if a customer asks the assistant to translate a string from English to French, the dialog can call an external language translation service to translate the phrase and return the translation to the customer in the course of the conversation. [Learn more](/docs/services/assistant?topic=assistant-dialog-webhooks). |

## I can't log in
{: #faqs-cannot-login}
{: faq}

If you are having trouble logging in to a service instance or see messages about tokens, such as `unable to fetch access token`, it might mean that you need to clear your browser cache. Open a private browser window, and then try again. 

- If accessing the page by using a private browsing window fixes the issue, then consider always using a private window or clear the cache of your browser. You can typically find an option for clearing the cache or deleting cookies in the browser's privacy and security settings.
- If accessing the page by using a private browsing window doesn't fix the issue, then try deleting the API key for the instance and creating a new one.

## I'm being asked to log in repeatedly
{: #faqs-login-repeatedly}
{: faq}

If you keep getting messages, such as `you are getting redirected to login`, it might mean that the Lite plan you were using has expired. Lite plans expire if they are not used within a 30-day span.

## I'm getting a `401` response
{: #faqs-getting-401}
{: faq}

The 401 response code is returned for many reasons, including:

- You exceeded the API call limit for your plan for this month. For example, for Lite plans, the monthly limit for API calls 10,000 messages per month. The 401 response will go away as soon as the next billing cycle begins, which is the first day of the calendar month.
- You are trying to use an instance-level API key that you created in one data center location to connect to a service instance that is hosted from another location. You must create an API key in the same data center location where your service instance is hosted.
- Your service instance might be using Cloud Foundry for resource management. If you authenticate with a username and password, then it is likely that you are still using Cloud Foundry. If so, you must migrate the instance so it uses IBM Cloud Identity and Access Management (IAM) instead. For more details, see [Migrating Watson services from Cloud Foundry](/docs/services/assistant?topic=watson-migrate).

## I'm getting a `403` response
{: #faqs-getting-403}
{: faq}

You are using a valid API Key, but it is not the right key for the service instance that you are trying to access programmatically.
