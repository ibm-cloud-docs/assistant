---

copyright:
  years: 2015, 2018
lastupdated: "2018-09-19"

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

## Before you begin
{: #prereqs}

The information in this topic applies to {{site.data.keyword.conversationshort}} beta release only. To request access to the beta release, from the bottom of the Workspaces page of your {{site.data.keyword.conversationshort}} instance, click **Request Beta**, and then confirm that you accept the terms and conditions for participating in the beta program.

If you do not have a service instance, go to the [Getting started tutorial (for the generally available {{site.data.keyword.conversationshort}} service) ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/conversation/getting-started.html) for information about how to get a free subscription to one.

**Note**: If you are approved for beta participation, it is the service instance where you request beta access that will be upgraded. And every workspace in that service instance will be converted to a skill. You can read more about skills in [Step 2](#add-skill) below. Do not request beta participation from an instance with workspaces that you are actively using. To try out the beta release safely, without impacting workspaces that you want to continue using, you can create a new service instance, and then request access from it.

## Step 1: Open the tool
{: #launch-tool}

When you are accepted into the beta program, you are sent an email invitation that includes a link to the beta environment. Follow the link, and launch the {{site.data.keyword.conversationshort}} tool.

## Step 2: Create a dialog skill
{: #add-skill}

Your first step in the {{site.data.keyword.conversationshort}} tool is to create a skill.

A [*dialog skill*](create-skill.html) is a container for the artifacts that define the flow of a conversation that your assistant can have with your customers.

1.  From the home page of the {{site.data.keyword.conversationshort}} tool, click **Create a Skill**.

    **Note**: If you created or were given developer role access to any workspaces that were built with the generally available version of the {{site.data.keyword.conversationshort}} service, you will see them listed on the Skills page as dialog skills.

    ![Shows the Add skill button from the Home page](images/gs-new-skill.png)

1.  Click **Create new**.

    ![Shows the Create new button from the Skills page](images/gs-click-create-new.png)

1.  Give your skill the name `Conversational skill tutorial`.
1.  **Optional**. If the dialog you plan to build will use a language other than English, then choose the appropriate language from the list.
1.  Click **Create**.

    ![Finish creating the skill](images/gs-add-skill-done.png)

You'll land on the Intents page of the tool.

## Step 3: Add intents from a content catalog
{: #add-catalog}

Add training data that was built by IBM to your workspace by adding intents from a content catalog. In particular, you will give your assistant access to the **General** content catalog so your dialog can greet users, and end conversations with them.

1.  In the {{site.data.keyword.conversationshort}} tool, click the **Content Catalog** tab.
1.  Find **General** in the list, and then click **Add to skill**.

    ![Shows the Content Catalog and highlights the Add to skill button for the General catalog.](images/gs-add-general-catalog.png)
1.  Open the **Intents** tab to review the intents and associated example utterances that were added to your training data. You can recognize them because each intent name begins with the prefix `#General_`. You will add the `#General_Greetings` and `#General_Ending` intents to your dialog in the next step.

    ![Shows the intents that are displayed in the Intents tab after the General catalog is added.](images/gs-general-added.png)

You have successfully started to build your training data by adding prebuilt content from IBM.

## Step 4: Build a dialog
{: #build-dialog}

A [dialog](dialog-overview.html) defines the flow of your conversation in the form of a logic tree. It matches intents (what users say) to responses (what the bot says back). Each node of the tree has a condition that triggers it, based on user input.

We'll create a simple dialog that handles greeting and ending intents, each with a single node.

### Adding a start node

1.  In the {{site.data.keyword.conversationshort}} tool, click the **Dialog** tab.
1.  Click **Create**. You'll see two nodes:
    - **Welcome**: Contains a greeting that is displayed to your users when they first engage with the assistant.
    - **Anything else**: Contains phrases that are used to reply to users when their input is not recognized.

    ![A new dialog with two builtin nodes](images/gs-new-dialog0.png)
1.  Click the **Welcome** node to open it in the edit view.
1.  Replace the default response with the text, `Welcome to the Watson Assistant tutorial!`.

    ![Editing the welcome node response](images/gs-edit-welcome0.png)
1.  Click ![Close](images/close.png) to close the edit view.

You created a dialog node that is triggered by the `welcome` condition. (`welcome` is a special condition that functions like an intent, but does not begin with a `#`.) It is triggered when a new conversation starts. Your node specifies that when a new conversation starts, the system should respond with the welcome message that you add to the response section of this first node.

### Testing the start node

You can test your dialog at any time to verify the dialog. Let's test it now.

- Click the ![Try it](images/ask_watson.png) icon to open the "Try it out" pane. You should see your welcome message.

### Adding nodes to handle intents

Now let's add nodes to handle our intents between the `Welcome` node and the `Anything else` node.

1.  Click the More icon ![More options](images/kabob.png) on the **Welcome** node, and then select **Add node below**.
1.  Type `#General_Greetings` in the **Enter a condition** field of this node. Then select the **`#General_Greetings`** option.
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

{{site.data.keyword.watson}} can recognize your intents even when your input doesn't exactly match the examples you included. The dialog uses intents to identify the purpose of the user's input regardless of the precise wording used, and then responds in the way you specify.

### Result of building a dialog

That's it. You created a simple conversation with two intents and a dialog to recognize them.

## Step 5: Create an assistant
{: #create-assistant}

An [*assistant*](assistants.html) is a cognitive bot to which you add a skill that enables it to interact with your customers in useful ways.

1.  Click the **Assistants** tab.
1.  Click **Create new**.

    ![Create new button on the Assistant tab](images/gs-create-assistant.png)
1.  Name the assistant `Watson Assistant tutorial`.
1.  In the Description field, enter `This is a sample assistant that I am creating to help me learn.`
1.  Click **Create**.

    ![Finish creating the new assistant](images/gs-create-assistant-done0.png)

## Step 6: Adding your skill to your assistant
{: #add-skill-to-assistant}

Add the dialog skill that you build to the assistant you created.

1.  From the new assistant page, click **Add skill**.

    **Note**: If you created or were given developer role access to any workspaces that were built with the generally available version of the {{site.data.keyword.conversationshort}} service, you will see them listed on the Skills page as conversational skills.

    ![Shows the Add skill button from the Assistant page](images/gs-add-skill.png)
1.  Choose to add the skill that you created earlier to the assistant.

## Step 7: Integrate the assistant
{: #integrate-assistant}

Now that you have an assistant that can participate in a simple conversational exchange, publish it to a public web page where you can test it out.

1.  Click the **Assistants** tab, find the `Watson Assistant tutorial` assistant you created, and open it.
1.  From the *Integrations* area, click **Shareable Link**.

    The Shareable Link integration is provisioned for you automatically.
1.  Click the URL that is displayed on the page.

    The page opens in a new tab.
1.  Say `hello` to your assistant, and watch it respond. You can share the URL with others who might want to try out your assistant.

    **Note**: Unlike when you send test utterances to the service from the "Try it out" pane, standard usage charges apply to API calls that result from utterances that are submited to the chat widget.

## Next steps
{: #next-steps}

This tutorial is built around a simple example. For a real application, you'll need to define some more interesting intents, some entities, and a more complex dialog that uses them both. When you have a polished version of the assistant, you can integrate it with channels that your customers use, such as Slack. As traffic increases between the assistant and your customers, you can use the tools provided in the **Improve** tab to analyze real conversations, and identify areas for improvement.

- Complete follow-on tutorials that build more advanced dialogs. Add standard nodes with the [Building a complex dialog](tutorial.html) tutorial or learn about slots with the [Adding a node with slots](tutorial-slots.html) tutorial.
- Check out more [sample apps](sample-applications.html) to get ideas.
