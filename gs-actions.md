---

copyright:
  years: 2015, 2022
lastupdated: "2022-04-01"

subcollection: assistant
content-type: tutorial
account-plan: lite
completion-time: 10m

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
{:hide-dashboard: .hide-dashboard}
{:download: .download}
{:video: .video}
{:step: data-tutorial-type='step'}

{{site.data.content.newlink}}
 
# Getting started with an actions skill
{: #gs-actions}
{: toc-content-type="tutorial"}
{: toc-completion-time="10m"}

In this short tutorial, we help you use an actions skill to build your first conversation.
{: shortdesc}

An *actions skill* contains actions that represent the tasks you want your assistant to help your customers with.

## Add an action
{: #gs-actions-add-action-hello}
{: step}

Each *action* contains a series of *steps* that represent individual exchanges with a customer. In the first action step, you define the user goal that the action will satisfy.

1.  Click **Create a new action**.

1.  We are going to keep it simple and just exchange greetings with the customer. 

    To recognize when a customer is just saying hello, add some examples of ways in which a person might do so.

    Add the following examples:

    - `Hi`
    - `Hello`
    - `Good morning.`
    - `Good afternoon!`
    - `Good evening`
    - `Howdy!`
    - `Yo, what's up?`

1.  Notice that the action has been given a name automatically. Our first example was used to name the skill `Hi`. Let's change it.

    In the field at the start of the page, change `Hi` to `Hello`.

1.  Click **Save** to save your action.

## Add a step
{: #gs-actions-add-step-hello}
{: step}

In the step you add, you'll define the greeting that your assistant will return to the customer.

1.  In the navigation pane, click **New step**.

1.  In the **Assistant says** field, enter `Good day to you!`

    Because we want to end the exchange of greetings there, let's end the action without adding any more steps.
1.  In the **And then** field, choose *End the action*.

1.  Click **Save**.

Congratulations! You created a simple turn in a conversation. Now, let's see if it works.

## Test the action
{: #gs-actions-test-say-hello}
{: step}

You test the action by entering a salutation into the "Preview" pane, which simulates what the customer's experience would be if they were interacting with the assistant.

1.  In the text field of the "Preview" pane, type `hello there`, and then submit it.

    You are submitting a greeting that doesn't exactly match any of the training examples you added earlier. However, the examples you provided should have taught your assistant to understand what a typical greeting looks like.

    The assistant responds with `Good day to you!` The preview also identifies the action that was recognized, by returning *Hello*.

You did it! 

Next, we'll add an action that understands when a customer says goodbye. 

## Add a goodbye action
{: #gs-actions-add-action-goodbye}
{: step}

Let's make sure your assistant is able to say goodbye to your customers when appropriate. We will add a new action that can understand customer messages that aim to end the conversation.

1.  Click **Close** to exit the *Say hello* action that you created successfully.

    You are returned to a page where you can see your new *Hello* action listed in an Actions table.

1.  Minimize the "Preview" panel, and then click **New action**.

1.  Add examples of phrases that customers might use to indicate that they want to end the conversation.

    Add the following examples:

    - `Goodbye`
    - `I'm all set now.`
    - `Thanks.`
    - `Have a good day`
    - `Thank you for your help`
    - `I'm done.`
    - `Bye`

This time, the action is named `Goodbye`, which is a good name, so we won't bother changing it.

## Add the assistant's farewell
{: #gs-actions-add-step-goodbye}
{: step}

1.  In the navigation pane, click **New step**.

1.  In the **Assistant says** field, enter `Farewell!`

    Again, let's end the action without adding any more steps.
1.  In the **And then** field, choose *End the action*.

1.  Click **Save**.

## Test the farewell action
{: #gs-actions-test-say-goodbye}
{: step}

Let's make sure the action is able to recognize when a customer wants to end the conversation.

1.  Click **Preview** to open the "Preview" panel.

1.  Click the refresh icon ![Refresh icon](images/preview-refresh-icon.png) to restart the conversation.

1.  In the text field, type `I'm finished`, and then submit it.

    The assistant responds with `Farewell!` and indicates that it recognized the *Goodbye* action.

1.  Click **Close**.

You have successfully created two actions that understand two different customer intentions and can respond to them properly.

## Preview the assistant
{: #gs-actions-integrate-assistant}
{: step}

Now that you have an actions skill that can maintain a simple conversational exchange, let's see how it works in the assistant.

1.  Click the **Assistants** icon ![Assistants menu icon](images/nav-ass-icon.png) to open a list of your assistants.
1.  Find the *My first assistant* assistant, and open it.
1.  Click **Preview** to open the Preview page, which shows your assistant embedded in a simple IBM-hosted web page.

    You can also use the URL displayed under **Share this link** to preview the assistant from another browser window or another computer. Previewing the assistant using this URL might incur charges, depending on your service plan. For more information, see [Testing your assistant](/docs/assistant?topic=assistant-deploy-web-link).
    {: note}

1.  In the "Assistant preview" pane, type `hello` into the text field. Press Enter, and watch your assistant respond.

    ![The widget in the assistant preview, showing a single dialog exchange.](images/gs-test-from-preview-link.png)

    You can share the URL with others who might want to try out your assistant.

1.  After testing, click the **X** to close the Preview page.

## Next steps
{: #gs-actions-next-steps}

This tutorial is built around a simple example. For a real application, you need to define some more complex exchanges. When you have a polished version of the assistant, you can integrate it with web sites or channels, such as Slack, that your customers already use. As traffic increases between the assistant and your customers, you can use the tools that are provided in the **Analytics** page to analyze real conversations, and identify areas for improvement.

- Check out more [sample apps](/docs/assistant?topic=assistant-sample-apps) to get ideas.
