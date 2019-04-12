---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-12"

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

# Getting started tutorial
{: #getting-started}

In this short tutorial, we introduce the {{site.data.keyword.conversationshort}} tool and go through the process of creating your first assistant.
{: shortdesc}

## Before you begin
{: #getting-started-prerequisites}
{: hide-dashboard}

You need a service instance to start.
{: hide-dashboard}

1.  {: hide-dashboard} Go to the [{{site.data.keyword.conversationshort}} ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/catalog/services/watson-assistant) page in the {{site.data.keyword.cloud_notm}} catalog.

    The service instance will be created in the **default** resource group if you do not choose a different one, and it *cannot* be changed later. This group is sufficient for the purposes of trying out the product.

    If you're creating an instance for more robust use, then learn more about [resource groups ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/docs/resources/bestpractice_rgs#bp_resourcegroups){: new_window}.
1.  {: hide-dashboard} Sign up for a free {{site.data.keyword.cloud_notm}} account or log in.
1.  {: hide-dashboard} Click **Create**.

## Step 1: Open the tool
{: #getting-started-launch-tool}

After you create a {{site.data.keyword.conversationshort}} service instance, you land on the **Manage** page of the {{site.data.keyword.conversationshort}} dashboard.
{: hide-dashboard}

1.  Click **Launch tool**. If you're prompted to log in to the tool, provide your {{site.data.keyword.cloud_notm}} credentials.

<!-- Remove this text after dedicated instances have the developer console: begin -->

{{site.data.keyword.Bluemix_dedicated_notm}}: Select your service instance from the Dashboard to launch the tool.

<!-- Remove this text after dedicated instances have the Developer Console: end -->

## Step 2: Create a dialog skill
{: #getting-started-add-skill}

Your first step in the {{site.data.keyword.conversationshort}} tool is to create a dialog skill.

A *dialog skill* is a container for the artifacts that define the flow of a conversation that your assistant can have with your customers.

1.  From the home page of the {{site.data.keyword.conversationshort}} tool, click **Create a Skill**.

    ![Shows the Add skill button from the home page](images/gs-new-skill.png)

1.  Click **Create skill**.

    ![Shows the Create new button from the Skills page](images/gs-click-create-new.png)

1.  Give your skill the name `Conversational skill tutorial`.
1.  **Optional**. If the dialog you plan to build will use a language other than English, then choose the appropriate language from the list.
1.  Click **Create skill**.

    ![Finish creating the skill](images/gs-add-skill-done.png)

You land on the Intents page of the tool.

## Step 3: Add intents from a content catalog
{: #getting-started-add-catalog}

Add training data that was built by IBM to your skill by adding intents from a content catalog. In particular, you will give your assistant access to the **General** content catalog so your dialog can greet users, and end conversations with them.

1.  In the {{site.data.keyword.conversationshort}} tool, click the **Content Catalog** tab.
1.  Find **General** in the list, and then click **Add to skill**.

    ![Shows the Content Catalog and highlights the Add to skill button for the General catalog.](images/gs-add-general-catalog.png)
1.  Open the **Intents** tab to review the intents and associated example utterances that were added to your training data. You can recognize them because each intent name begins with the prefix `#General_`. You will add the `#General_Greetings` and `#General_Ending` intents to your dialog in the next step.

    ![Shows the intents that are displayed in the Intents tab after the General catalog is added.](images/gs-general-added.png)

You successfully started to build your training data by adding prebuilt content from {{site.data.keyword.IBM_notm}}.

## Step 4: Build a dialog
{: #getting-started-build-dialog}

A [dialog](/docs/services/assistant?topic=assistant-dialog-overview) defines the flow of your conversation in the form of a logic tree. It matches intents (what users say) to responses (what the bot says back). Each node of the tree has a condition that triggers it, based on user input.

We'll create a simple dialog that handles greeting and ending intents, each with a single node.

### Adding a start node

1.  In the {{site.data.keyword.conversationshort}} tool, click the **Dialog** tab.
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
1.  Type `#General_Greetings` in the **Enter a condition** field of this node. Then, select the **`#General_Greetings`** option.
1.  Add the response, `Good day to you!`
1.  Click ![Close](images/close.png) to close the edit view.

   ![A general greeting node was added to the dialog.](images/gs-add-greeting-node.png)

1.  Click the More icon ![More options](images/kabob.png) on this node, and then select **Add node below** to create a peer node. In the peer node, specify `#General_Ending` as the condition, and `OK. See you later.` as the response.

   ![Adding an ending node to the dialog.](images/gs-add-ending-node.png)

1.  Click ![Close](images/close.png) to close the edit view.

   ![Shows that a general ending node was also added to the dialog.](images/gs-ending-added.png)

### Testing intent recognition

You built a simple dialog to recognize and respond to both greeting and ending inputs. Let's see how well it works.

1.  Click the ![Try it](images/ask_watson.png) icon to open the "Try it out" pane. There's that reassuring welcome message.
1.  At the bottom of the pane, type `Hello` and press Enter. The output indicates that the #hello intent was recognized, and the appropriate response (`Good day to you.`) appears.
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

## Step 5: Create an assistant
{: #getting-started-create-assistant}

An [*assistant*](/docs/services/assistant?topic=assistant-assistants) is a cognitive bot to which you add a skill that enables it to interact with your customers in useful ways.

1.  Click the **Assistants** tab.
1.  Click **Create assistant**.

    ![Create new button on the Assistant tab](images/gs-create-assistant.png)
1.  Name the assistant `Watson Assistant tutorial`.
1.  In the Description field, enter `This is a sample assistant that I am creating to help me learn.`
1.  Click **Create assistant**.

    ![Finish creating the new assistant](images/gs-create-assistant-done0.png)

## Step 6: Add your skill to your assistant
{: #getting-started-add-skill-to-assistant}

Add the dialog skill that you built to the assistant you created.

1.  From the new assistant page, click **Add dialog skill**.

    If you created or were given developer role access to any workspaces that were built with the generally available version of the {{site.data.keyword.conversationshort}} service, you will see them listed on the Skills page as conversational skills.
    {: tip}

    ![Shows the Add skill button from the Assistant page](images/gs-add-skill.png)
1.  Choose to add the skill that you created earlier to the assistant.

## Step 7: Integrate the assistant
{: #getting-started-integrate-assistant}

Now that you have an assistant that can participate in a simple conversational exchange, test it. The product provides a built-in integration that is called a Preview Link. When you create an assistant, this type of integration is created for you automatically. The Preview Link integration builds your assistant into a chat widget that is hosted by an IBM-branded web page. You can open the web page and chat with your assistant to test it out.

1.  Click the **Assistants** tab, find the `Watson Assistant tutorial` assistant that you created, and open it.
1.  From the *Integrations* area, click to open the **Preview Link** integration.

1.  Click the URL that is displayed on the page.

    The page opens in a new tab.
1.  Say `hello` to your assistant, and watch it respond. You can share the URL with others who might want to try out your assistant.

## Next steps
{: #getting-started-next-steps}

This tutorial is built around a simple example. For a real application, you need to define some more interesting intents, some entities, and a more complex dialog that uses them both. When you have a polished version of the assistant, you can integrate it with channels that your customers use, such as Slack. As traffic increases between the assistant and your customers, you can use the tools that are provided in the **Analytics** tab to analyze real conversations, and identify areas for improvement.

- Complete follow-on tutorials that build more advanced dialogs:
    - Add standard nodes with the [Building a complex dialog](/docs/services/assistant?topic=assistant-tutorial) tutorial.
    - Learn about slots with the [Adding a node with slots](/docs/services/assistant?topic=assistant-tutorial-slots) tutorial.
- Check out more [sample apps](/docs/services/assistant?topic=assistant-sample-apps) to get ideas.
