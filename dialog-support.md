---

copyright:
  years: 2019, 2021
lastupdated: "2020-10-09"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
{:tip: .tip}
{:preview: .preview}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:video: .video}

# Connecting customers with support
{: #dialog-support}

No matter where you deploy your assistant, give your customers a way to get additional support when they need it.
{: shortdesc}

Build your dialog to recognize when customers need help that cannot be provided by the assistant. Add logic that can connect your customers to whatever type of professional support you offer. Your solutions might include:

- A toll-free phone number to a call center that is manned by human agents
- An online support ticket form that customers fill out and submit
- An integration with Intercom
- A service desk solution that is configured to work with your web chat integration or a custom client application

Design your dialog to recognize customer requests for help and address them. Add an intent that understands the customer request, and then add a dialog branch that handles the request.

For example, you might add an intent and use it in a dialog node like the intents that are shown in the table below.

| Intent name | Intent user example 1 | Intent user example 2 | Response from dialog node that conditions on intent |
|--------|-----------------------|-----------------------|-----------------------------------------------------|
| `#call_support` | *How do I reach support?* | *What's your toll-free number?* | *`Call 1-800-555-0123 to reach a call center agent at any time.`* |
| `#support_ticket` | *How do I get help?* | *Who can help me with an issue I'm having?* |  *`Go to [Support Center](https://example.com/support) and open a support ticket.`* |
{: caption="Alternative support request intent examples" caption-side="top"}

If you deploy your assistant with an integration that has built-in service desk support, you can use the special *Connect to human agent* response type in your dialog response to initiate a transfer.

## Adding chat transfer support
{: #dialog-support-transfers}

Design your dialog so that it can transfer customers to human agents. Consider adding support for initiating a transfer in the following scenarios:

- Any time a user asks to speak to a person. 

  Create an intent that can recognize when a customer asks to speak to someone. After defining the intent, you can add a root-level dialog node that conditions on the intent. As the dialog node response, add a *Connect to human agent* response type. At run time, if the user asks to speak to someone, this node is triggered and a transfer is initiated on the user's behalf.

- When the conversation broaches a topic that is sensitive in nature, you can start a transfer. 

  For example, an insurance company might want questions about bereavement benefits always to be handled by a person. Or, if a customer wants to close an account, you might want to transfer the conversation to a respresentative who is authorized to offer incentives to keep the customer's business.

- When the assistant repeatedly fails to understand a customer's request. Instead of asking the customer to retry or rephrase the question over and over, your assistant can offer to transfer the customer to a human agent.

To design a dialog that can transfer the conversation, complete the following steps:

1.  Add an intent to your skill that can recognize a user's request to speak to a human.

    You can create your own intent or add the prebuilt intent named `#General_Connect_to_Agent` that is provided with the **General** content catalog.

1.  Add a root node to your dialog that conditions on the intent you created in the previous step. Choose **Connect to human agent** as the response type.

    For more information, see [Adding a *Connect to human agent* response type](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-add-connect-to-human-agent).

1.  Add meaningful names to the dialog nodes in your dialog.

    When a conversation is transferred to an agent, the name of the most-recently processed dialog node is sent with it. To ensure that a useful summary is provided to service desk agents when a conversation is transferred to them, add short dialog node names that reflect the node's purpose. For example, *Find a store*.
    
    Some older skills specify the purpose of the node in the **external node name** field instead.

    ![Screen capture of the field in the node edit view where you add the node purpose summary.](images/disambig-node-purpose.png)

    Every dialog branch can be processed by the assistant while it chats with a customer, including branches with root nodes in folders.
    {: note}

1.  If a child node in any dialog branch conditions on a request or question that you do not want the assistant to handle, add a **Connect to human agent** response type to the child node.

    At run time, if the conversation reaches this child node, the dialog is passed to a human agent at that point.

Your dialog is now ready to support transfers from your assistant to a service desk agent.

For more information about different service desk solutions, see the following resources:

- [Adding service desk support to the web chat](/docs/assistant?topic=assistant-deploy-web-chat#deploy-web-chat-haa)
- [Integrating with Intercom](/docs/assistant?topic=assistant-deploy-intercom)

## Measuring containment
{: #dialog-support-containment}

From the Analytics page, you can measure conversation containment. Containment is the rate at which your assistant is able to satisfy a customer's request without human intervention per conversation.

To measure containment accurately, the metric must be able to identify when a human intervention occurs. The metric primarily uses the *Connect to human agent* response type as an indicator. If a user conversation log includes a call to a *Connect to human agent* response type, then the conversation is considered to be *not contained*.

However, not all human interventions are transacted by using a *Connect to human agent* response type. If you use an alternate method to deliver additional support, you must take additional steps to register the fact that the customer's need was not fulfilled by the assistant. For example, you might direct customers to your call center phone number or to an online support ticket form URL. If so, you need to flag these types of redirects so the containment metric is able to pick them up.

To enable the containment metric, complete the following steps:

1.  For any dialog nodes that transfer the chat to a service desk agent, add a *Connect to human agent* response type.

    For more information, see [Adding a *Connect to human agent* response type](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-add-connect-to-human-agent).

1.  For any dialog nodes that redirect customers to alternate forms of customer support, add a context variable named `context_connect_to_agent` and set it to `true`.

    The process that calculates the containment metric looks for instances of this context variable, and registers any occurrences that it finds as interventions.

    - From the dialog node, go to the *Assistant responds* section and click the menu icon ![overflow menu icon](images/more-icon.png).

      If you're using multiple conditioned responses, you must click the *Customize response* icon ![gear icon](images/customize-response-icon.png) to see the menu that is associated with the response.
      {: tip}

    - Click **Open context editor**.

      ![Shows the Open context editor menu option selected from the list](images/open-context-editor.png)

    - Add the following variable name and value to define the context variable:

      <table>
      <caption>Special containment context variable</caption>
      <tr>
        <th>Variable</th>
        <th>Value</th>
      </tr>
      <tr>
        <td>`context_connect_to_agent`</td>
        <td>`true`</td>
      </tr>
      </table>

    - Click **X** to close the dialog node. Your change is saved automatically.

      If you're using multiple conditioned responses, you must click **Save** first.
      {: tip}
 
 To track containment for your assistant, go to the *Analytics* page. For more information, see [Metrics overview](/docs/assistant?topic=assistant-logs-overview#logs-overview-graphs).