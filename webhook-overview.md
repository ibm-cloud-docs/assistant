---

copyright:
  years: 2019, 2021
lastupdated: "2021-01-15"

subcollection: assistant

---

{{site.data.keyword.attribute-definition-list}}

Documentation for the **classic {{site.data.keyword.assistant_classic_short}}** experience has moved. For the most up-to-date version, see [Extending your assistant with webhooks](/docs/watson-assistant?topic=watson-assistant-webhook-overview){: external}.
{: attention}

# Webhook overview
{: #webhook-overview}

Make a call to an external service or application during a conversation.
{: shortdesc}

{{site.data.keyword.assistant_classic_short}} supports the following types of webhooks:

- Skill-level webhooks

   The following type of webhook can be set up for use from a dialog skill:

   - [Dialog](/docs/assistant?topic=assistant-dialog-webhooks)

- Assistant-level webhooks
 
   The following types of webhooks can be set up for an assistant:

   - [Logs](/docs/assistant?topic=assistant-webhook-log)
   - [Premessage](/docs/assistant?topic=assistant-webhook-pre)
   - [Postmessage](/docs/assistant?topic=assistant-webhook-post)

## Which type of webhook should I use?
{: #webhook-overview-difs}

The skill-level webhook is different from the assistant-level webhooks in the following ways:

- Frequency with which the webhooks is called

   The dialog webhook is called on the rare occasion that the dialog node from which it is triggered is processed. 
  
   The message processing webhooks are called with every single exchange in a conversation between the customer and the assistant. 
  
   The log webhook is called with each message and its corresponding response.

- Where the condition is defined

   For a dialog webhook, the condition to meet before an action is taken is defined in the dialog skill. If the node condition is not met, then the dialog webhook is never called. 
  
   For the message processing webhooks, the condition to check for before taking an action must be defined in the external application code. For example, even if your webhook performs a simple language translation, you'd want to use a condition to check the language of the incoming message before sending the text to the translation service.
  
   You don't need to define a condition for the log webhook unless you want to filter the messages somehow. In most cases, the goal is to write out every message that is submitted, so the messages can be stored for as long as you want, and analyzed by an external application or service.
  
- Where you configure them

   You configure the dialog webhook from the the **Options>Webhook** page of the dialog skill. You then initialize it from one or more dialog nodes by customizing the node. 
  
   You configure the assistant-level webhooks from the **Settings>Webhooks** page for the assistant.
  