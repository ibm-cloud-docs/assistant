---

copyright:
  years: 2015, 2018
lastupdated: "2018-03-23"

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
{:download: .download}
{:gif: data-image-type='gif'}

# Getting started tutorial
{: #getting-started}

In this short tutorial, we introduce the {{site.data.keyword.conversationshort}} tool and go through the process of creating your first assistant.
{: shortdesc}

## Step 1: Request to participate in the Beta
{: #launch-tool}

From the bottom of the Workspaces page of your {{site.data.keyword.conversationshort}} instance, click **Request Beta**, and then confirm that you accept terms and conditions for participating in the Beta. 

If you do not have a service instance, go to the [Getting started tutorial (for the generally available {{site.data.keyword.conversationshort}} service) ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/conversation/getting-started.html) for information about how to get a free subscription to one.

**Note**: If you are approved for Beta participation, it is the service instance where you request Beta access that will be upgraded. And every workspace in that service instance will be upgraded. Do not request Beta participation from an instance with workspaces that you are actively using. To try out the Beta safely, you can create a new service instance, and then request access from it.

## Step 2: Create an assistant
{: #create-assistant}

Your first step in the {{site.data.keyword.conversationshort}} tool is to create an assistant.

An [*assistant*](assistants.html) is a cognitive bot to which you add skills that enable it to interact with your customers in useful ways.

1.  From the home page of the {{site.data.keyword.conversationshort}} tool, click the **Assistants** tab.
1.  Click **Create new**.

    ![Create new button on the Assistant tab](images/gs-create-assistant.png)
1.  Name the assistant `Watson Assistant tutorial`.
1.  In the Description field, enter `This is a sample assistant that I am creating to help me learn.`
1.  **Optional**. If you want to create an assistant that communicates in a language other than English, then choose the appropriate language from the list.
1.  Click **Create**.

    ![Finish creating the new assistant](images/gs-create-assistant-done.png)

## Step 3: Add a conversational skill to your assistant
{: #add-skill}

A [*conversational skill*](create-convo-skill.html) is a container for the artifacts that define the flow of a conversation that your assistant can have with your customers.

1.  From the new assistant page, click **Add skill**.

    **Note**: If you created or were given developer role access to any workspaces that were built with the generally available version of the {{site.data.keyword.conversationshort}} service, you will see them listed on the Skills page as conversational skills.

    ![Shows the Add skill button from the Assistant page](images/gs-add-skill.png)

1.  Click **Create new**.
1.  Give your skill the name `Conversational skill tutorial`.
1.  **Optional**. If the dialog you plan to build will use a language other than English, then choose the appropriate language from the list.
1.  Click **Create**.

    ![Finish creating the skill](images/gs-add-skill-done.png)

## Step 4: Create intents
{: #create-intents}

An [intent](intents.html) represents the purpose of a user's input. You can think of intents as the actions your users might want to perform with your application.

For this example, we're going to keep things simple and define only two intents: one for saying hello, and one for saying goodbye.

1.  From the Skills page, find the tile for the `Conversational skill tutorial` that you just created, and click **Edit**.

    You'll land on the Intents page of the tool.

    ![Landing on the intents page of the tool](images/gs-add-skill-done2.png)
1.  Click **Add intent**.
1.  Name the intent `hello`, and then click **Create intent**.
1.  Type `hello` into the **Add user example** field, and then press **Enter**.

   *Examples* tell the {{site.data.keyword.conversationshort}} service what kinds of user input you want to match to the intent. The more examples you provide, the more accurate the service can be at recognizing user intents.
1.  Add four more examples:
    - `good morning`
    - `greetings`
    - `hi`
    - `howdy`

1.  Click the **Close** ![Close arrow](images/close_arrow.png) icon to finish creating the #hello intent.

    ![Adding a hello intent with examples](images/gs-hello-intent-done.png)
1.  Create another intent named #goodbye with these five examples:
    - `bye`
    - `farewell`
    - `goodbye`
    - `I'm done`
    - `see you later`

    ![Adding a goodbye intent with examples](images/gs-goodbye-intent-done.png)

You've created two intents, #hello and #goodbye, and provided example user input to train {{site.data.keyword.watson}} to recognize these intents in your users' input.

![Showing the created intents](images/gs-intents-done.png)

## Step 5: Add intents from a content catalog
{: #add-catalog}

Add training data that was built by IBM to your workspace by adding intents from a content catalog. In particular, you will give your assistant access to the `eCommerce` content catalog so your dialog can address user requests to complete common online transactions.

1.  In the {{site.data.keyword.conversationshort}} tool, click the **Content Catalog** tab.
1.  Find **eCommerce** in the list, and then click **Add to skill**.
1.  Open the **Intents** tab to review the intents and associated example utterances that were added to your training data. You can recognize them because each intent name begins with the prefix `#eCommerce_`. You will add the `#eCommerce_Cancel_Product_Order` intent to your dialog in a later step.

You have successfully supplemented your training data with prebuilt content provided by IBM.

## Step 6: Build a dialog
{: #build-dialog}

A [dialog](dialog-overview.html) defines the flow of your conversation in the form of a logic tree. Each node of the tree has a condition that triggers it, based on user input.

We'll create a simple dialog that handles our #hello and #goodbye intents, each with a single node.

### Adding a start node

1.  In the {{site.data.keyword.conversationshort}} tool, click the **Dialog** tab.
1.  Click **Create**. You'll see two nodes:
    - **Welcome**: Contains a greeting that is displayed to your users when they first engage with the assistant.
    - **Anything else**: Contains phrases that are used to reply to users when their input is not recognized.

    ![A new dialog with two builtin nodes](images/gs-new-dialog.png)
1.  Click the **Welcome** node to open it in the edit view.
1.  Replace the default response with the text, `Welcome to the Watson Assistant tutorial!`.

    ![Editing the welcome node response](images/gs-edit-welcome.png)
1.  Click ![Close](images/close.png) to close the edit view.

You created a dialog node that is triggered by the `welcome` condition, which is a special condition that indicates that the user has started a new conversation. Your node specifies that when a new conversation starts, the system should respond with the welcome message.

### Testing the start node

You can test your dialog at any time to verify the dialog. Let's test it now.

- Click the ![Try it](images/ask_watson.png) icon to open the "Try it out" pane. You should see your welcome message.

### Adding nodes to handle intents

Now let's add nodes to handle our intents between the `Welcome` node and the `Anything else` node.

1.  Click the More icon ![More options](images/kabob.png) on the **Welcome** node, and then select **Add node below**.
1.  Type `#hello` in the **Enter a condition** field of this node. Then select the **#hello** option.
1.  Add the response, `Good day to you.`

    ![Adding a hello node to the dialog between welcome and anything else](images/gs-add-hello-node.png)
1.  Click ![Close](images/close.png) to close the edit view.
1.  Click the More icon ![More options](images/kabob.png) on this node, and then select **Add node below** to create a peer node. In the peer node, specify `#eCommerce_Cancel_Product_Order` as the condition.
1.  Add the following text as the response.

    `I can help you cancel your order.`

    ![Adding a cancel order node after the hello node](images/gs-add-ecommerce-node.png)
1.  Click the More icon ![More options](images/kabob.png) on this node, and then select **Add node below** to create another peer node. In the peer node, specify `#goodbye` as the condition, and `OK. See you later!` as the response.

    ![Adding a goodbye node after the cancel order node and before anything else](images/gs-add-goodbye-node.png)

### Testing intent recognition

You  built a simple dialog to recognize and respond to both hello and goodbye inputs. Let's see how well it works.

1.  Click the ![Try it](images/ask_watson.png) icon to open the "Try it out" pane. There's that reassuring welcome message.
1.  At the bottom of the pane, type `Hello` and press Enter. The output indicates that the #hello intent was recognized, and the appropriate response (`Good day to you.`) appears.
1.  Try the following input:
    - `bye`
    - `howdy`
    - `see ya`
    - `good morning`
    - `sayonara`

1.  Enter `I want to cancel an order I placed.` and press Enter. The output indicates that the `#eCommerce_Cancel_Product_Order` intent was recognized, and the response that you added for it is displayed.

  **Note**: In a dialog that is used by an assistant in production, you would likely add more child nodes that collect the order number and any other necessary information from the user, and then make a programmatic call to your order tracking backend service to cancel the order on the user's behalf.

![Testing the dialog in the Try it out pane](images/gs-test-dialog.gif){: gif}

{{site.data.keyword.watson}} can recognize your intents even when your input doesn't exactly match the examples you included. The dialog uses intents to identify the purpose of the user's input regardless of the precise wording used, and then responds in the way you specify.

### Result of building a dialog

That's it. You created a simple conversation with two intents and a dialog to recognize them.

## Step 7: Integrate the assistant
{: #integrate-assistant}

Now that you have an assistant that can participate in a simple conversational exchange, publish it to the Facebook Messenger messaging channel to test it out.

1.  Go to the `Watson Assistant tutorial` page.
1.  Click **Add integration**.
1.  Choose **Facebook Messenger** as the channel integration.
1.  Follow the instructions to complete the integration.
1.  From the Facebook Messenger user interface, add the assistant as a contact.
1.  Say `hello` to your assistant, and watch it respond.

## Next steps
{: #next-steps}

This tutorial is built around a simple example. For a real application, you'll need to define some more interesting intents, some entities, and a more complex dialog that uses them both. As traffic increases, you can use the tools provided in the **Improve** tab to analyze real conversations between your assistant and users, and identify areas for improvement.

- Complete follow-on tutorials that build more advanced dialogs. Add standard nodes with the [Building a complex dialog](tutorial.html) tutorial or learn about slots with the [Adding a node with slots](tutorial-slots.html) tutorial.
- Check out more [sample apps](sample-applications.html) to get ideas.
