---

copyright:
  years: 2015, 2019
lastupdated: "2019-08-06"

keywords: assistant, omnichannel, virtual agent, virtual assistant, chatbot, conversation, watson assistant, watson conversation

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:deprecated: .deprecated}
{:important: .important}
{:note: .note}
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
{:gif: data-image-type='gif'}

# Getting started with {{site.data.keyword.conversationshort}}
{: #getting-started}

In this short tutorial, we introduce {{site.data.keyword.conversationfull}} and walk you through the process of creating your first assistant.
{: shortdesc}

## Before you begin
{: #getting-started-prerequisites}
{: hide-dashboard}

You need a service instance to start.
{: hide-dashboard}

1.  {: hide-dashboard} Go to the [{{site.data.keyword.conversationshort}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/catalog/services/watson-assistant) page in the {{site.data.keyword.cloud}} catalog.

    The service instance will be created in the **default** resource group if you do not choose a different one, and it *cannot* be changed later. This group is sufficient for the purposes of trying out the product.

    If you're creating an instance for more robust use, then learn more about [resource groups ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/docs/resources/bestpractice_rgs#bp_resourcegroups){: new_window}.
1.  {: hide-dashboard} Sign up for a free {{site.data.keyword.cloud_notm}} account or log in.
1.  {: hide-dashboard} Click **Create**.

## Step 1: Open Watson Assistant
{: #getting-started-launch-tool}

After you create a {{site.data.keyword.conversationshort}} service instance, you land on the **Manage** page of the {{site.data.keyword.conversationshort}} dashboard.
{: hide-dashboard}

1.  Click **Launch {{site.data.keyword.conversationshort}}**. If you're prompted to log in, provide your {{site.data.keyword.cloud_notm}} credentials.

<!-- Remove this text after dedicated instances have the developer console: begin -->

{{site.data.keyword.Bluemix_dedicated_notm}}: Select your service instance from the Dashboard to launch the product.

<!-- Remove this text after dedicated instances have the Developer Console: end -->

If you are a brand new user, an assistant named *My first assistant* is created for you automatically. Skip the next step. 

![Shows that an assistant was automatically added to Assistants page](images/gs-ass-created-for-me.png)

If available in your location, a tour begins that you can step through to learn about the product. Follow the tour; it overlaps with these tutorial steps, so you can resume this tutorial after the tour ends.
  {: tip}

An [*assistant*](/docs/services/assistant?topic=assistant-assistants) is a cognitive bot to which you add a skill that enables it to interact with your customers in useful ways.

If an assistant is not created automatically, your first step is to create an assistant.

## Step 2: Create an assistant
{: #getting-started-create-assistant}

1.  Click **Create assistant**.

    ![Create assistant button on the Assistants page.](images/gs-create-assistant.png)
1.  Name the assistant `My first assistant`.
1.  Click **Create assistant**.

    ![Finish creating the new assistant](images/gs-create-assistant-done.png)

## Step 3: Create a dialog skill
{: #getting-started-add-skill}

A *dialog skill* is a container for the artifacts that define the flow of a conversation that your assistant can have with your customers.

1.  If the assistant was created for you, click the *My first assistant* tile to open the assistant.

1.  Click **Add dialog skill**.

    ![Shows the Add skill button from the home page](images/gs-new-skill.png)

1.  Give your skill the name `Conversational skill tutorial`.
1.  **Optional**. If the dialog you plan to build will use a language other than English, then choose the appropriate language from the list.

    ![Finish creating the skill](images/gs-add-skill-done.png)

1.  Click **Create dialog skill**.

    ![Finish creating the skill](images/gs-skill-added.png)

1.  Click to open the skill you just created.

You land on the Intents page.

## Step 4: Add intents from a content catalog
{: #getting-started-add-catalog}

Add training data that was built by IBM to your skill by adding intents from a content catalog. In particular, you will give your assistant access to the **General** content catalog so your dialog can greet users, and end conversations with them.

1.  Click the **Content Catalog** tab.
1.  Find **General** in the list, and then click **Add to skill**.

    ![Shows the Content Catalog and highlights the Add to skill button for the General catalog.](images/gs-add-general-catalog.png)
1.  Open the **Intents** tab to review the intents and associated example utterances that were added to your training data. You can recognize them because each intent name begins with the prefix `#General_`. You will add the `#General_Greetings` and `#General_Ending` intents to your dialog in the next step.

    ![Shows the intents that are displayed in the Intents tab after the General catalog is added.](images/gs-general-added.png)

You successfully started to build your training data by adding prebuilt content from {{site.data.keyword.IBM_notm}}.

## Step 5: Build a dialog
{: #getting-started-build-dialog}

A [dialog](/docs/services/assistant?topic=assistant-dialog-overview) defines the flow of your conversation in the form of a logic tree. It matches intents (what users say) to responses (what the bot says back). Each node of the tree has a condition that triggers it, based on user input.

We'll create a simple dialog that handles greeting and ending intents, each with a single node.

### Adding a start node

1.  Click the **Dialog** tab.
1.  Click **Create dialog**. You see two nodes:
    - **Welcome**: Contains a greeting that is displayed to your users when they first engage with the assistant.
    - **Anything else**: Contains phrases that are used to reply to users when their input is not recognized.

    ![A new dialog with two built-in nodes](images/gs-new-dialog.png)
1.  Click the **Welcome** node to open it in the edit view.
1.  Replace the default response with the text, `Welcome to the Watson Assistant tutorial!`.

    ![Editing the welcome node response](images/gs-edit-welcome.png)
1.  Click ![Close](images/close.png) to close the edit view.

You created a dialog node that is triggered by the `welcome` condition. (`welcome` is a special condition that functions like an intent, but does not begin with a `#`.) It is triggered when a new conversation starts. Your node specifies that when a new conversation starts, the system should respond with the welcome message that you add to the response section of this first node.

### Testing the start node

You can test your dialog at any time to verify the dialog. Let's test it now.

- Click the ![Try it](images/ask_watson.png) icon to open the "Try it out" pane. You should see your welcome message.

### Adding nodes to handle intents

Now let's add nodes between the `Welcome` node and the `Anything else` node that handle our intents.

1.  Click the More icon ![More options](images/kabob.png) on the **Welcome** node, and then select **Add node below**.
1.  In the **If assistant recognizes** field of this node, start to type `#General_Greetings`. Then, select the **`#General_Greetings`** option.
1.  Add the response text, `Good day to you!`
1.  Click ![Close](images/close.png) to close the edit view.

   ![A general greeting node was added to the dialog.](images/gs-add-greeting-node.png)

1.  Click the More icon ![More options](images/kabob.png) on this node, and then select **Add node below** to create a peer node. In the peer node, specify `#General_Ending` in the **If assistant recognizes** field, and `OK. See you later.` as the response text.

   ![Adding an ending node to the dialog.](images/gs-add-ending-node.png)

1.  Click ![Close](images/close.png) to close the edit view.

### Testing intent recognition

You built a simple dialog to recognize and respond to both greeting and ending inputs. Let's see how well it works.

1.  Click the ![Try it](images/ask_watson.png) icon to open the "Try it out" pane. There's that reassuring welcome message.
1.  At the bottom of the pane, type `Hello` and press Enter. The output indicates that the `#General_Greetings` intent was recognized, and the appropriate response (`Good day to you.`) is displayed.
1.  Try the following input:
    - `bye`
    - `howdy`
    - `see ya`
    - `good morning`
    - `sayonara`

![Testing the dialog in the Try it out pane](images/gs-try-it.gif){: gif}

{{site.data.keyword.watson}} can recognize your intents even when your input doesn't exactly match the examples that you included. The dialog uses intents to identify the purpose of the user's input regardless of the precise wording used, and then responds in the way you specify.

### Result of building a dialog

That's it. You created a simple conversation with two intents and a dialog to recognize them.

## Step 6: Integrate the assistant
{: #getting-started-integrate-assistant}

Now that you have an assistant that can participate in a simple conversational exchange, test it.

1.  Click the **Assistants** tab, find the *My first assistant* assistant, and open it.
1.  Do one of the following things to test your assistant with a preview link integration. 

    The preview link integration builds your assistant into a chat widget that is hosted by an IBM-branded web page. You can open the web page and chat with your assistant to test it out.

    - If the assistant was created for you, you must add a preview link integration. From the *Integrations* area, click **Add integration**, and then click **Preview Link**. Click **Create**.

    - If you created the assistant yourself, then click the preview link integration tile to open it. 
    
      When you create an assistant yourself, a preview link integration is created for you automatically.

1.  Click the URL that is displayed on the page.

    The test web page opens in a new tab.
1.  Type `hello` into the text field, and watch your assistant respond. 

    ![The widget in the preview link integration showing a single dialog exchange.](images/gs-test-from-preview-link.png)

    You can share the URL with others who might want to try out your assistant.

1.  After testing, close the web page. Click the **X** to close the preview link integration page.

## Next steps
{: #getting-started-next-steps}

This tutorial is built around a simple example. For a real application, you need to define some more interesting intents, some entities, and a more complex dialog that uses them both. When you have a polished version of the assistant, you can integrate it with channels that your customers use, such as Slack. As traffic increases between the assistant and your customers, you can use the tools that are provided in the **Analytics** tab to analyze real conversations, and identify areas for improvement.

- Complete follow-on tutorials that build more advanced dialogs:
    - Add standard nodes with the [Building a complex dialog](/docs/services/assistant?topic=assistant-tutorial) tutorial.
    - Learn about slots with the [Adding a node with slots](/docs/services/assistant?topic=assistant-tutorial-slots) tutorial.
- Check out more [sample apps](/docs/services/assistant?topic=assistant-sample-apps) to get ideas.
