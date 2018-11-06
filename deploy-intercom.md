---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-01"

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

# Integrating with Intercom ![Premium plan only](images/premium0.png)
{: #deploy-intercom}

Intercom is a customer service messaging platform that evaluates user support inquiries and routes them to the best human agent or team of agents to address the user's question or request.
{: shortdesc}

Intercom has partnered with IBM to add a new agent to the team, a virtual Watson Assistant. You can integrate your assistant with an Intercom application to enable the app to seamlessly pass user conversations between your assistant and human support agents.

If you integrate the assistant with Intercom, the Intercom application becomes the client application for your skill. All interactions with users are initiated through and managed by Intercom.

There is currently no way to pass an ongoing conversation from one integration channel to another.

## Before you begin
{: #intercom-prereq}

Complete the following steps before you add the Intercom integration. (These steps duplicate tasks described in the checklist that is available from the integration configuration pages.)

1.  Add an intent to your skill that can recognize a user's request to speak to a human.

    You can create your own intent or add the prebuilt intent named `#General_Connect_to_Agent` that is provided with the **General** content catalog developed by IBM.

1.  Add a root node to your dialog that conditions on the intent that you created in the previous step. Choose **Connect to human agent** as the response type.

1.  Decide which dialog branches you want the assistant to be able to handle on its own. For each branch, fill in the node purpose field of its root node by adding a summary of the purpose of the branch. For example, *Find a store*. Root nodes in folders are supported also.

    ![Screenshot of the field in the node edit view where you add the node purpose summary.](images/disambig-node-purpose.png)

    Adding a value to the node purpose field opts the node in for consideration as a topic that can be assigned to a team or agent by the Intercom app. (You will assign each opted-in dialog node to an agent, be it human or virtual, later when you configure the Intercom teammate interactions.)

    **Important**: Do **not** add a node purpose summary to the root node that you created in the previous step. Identifying the node that the user interacted with just before asking to speak to a human is important. It can help the service identify nodes that need to be improved. To find these nodes, when an escalation occurs, the service looks for the node purpose summary for the most recently processed node. Including a summary for the root node that does no actual work, but only transfers users to human agents can hinder the service's ability to identify the last goal-oriented node that the user interacted with before requesting the escalation.

1.  If a child node in the branch conditions on a follow-up request or question that you do not want the assistant to handle, add a **Connect to human agent** response type to the node. At run time, if the conversation reaches this child node, the dialog is passed to a human agent at this point. (Later, when you set up the Intercom integration, you will choose a human agent as a backup for each topic that you assign to your assistant.)

1.  Create a corporate email address for your assistant.

    All agents must have valid email addresses before they can be added to a team in Intercom.

## Adding an Intercom integration
{: #add-intercom}

1.  From the Assistants tab, click to open the assistant tile that you want to deploy.

1.  From the Integrations section, click **Add Integration**.

1.  Click the **Select Integration** button for *Intercom*.

1.  Follow the instructions that are provided on the screen to complete the integration process.

    **Note**: You are instructed to create an Intercom app. At a minimum, you need a subscription to the *Inbox* product from Intercom.

The instructions guide you through the steps required to invite team members and edit your profile. Click the Intercom profile icon in the navigation bar to access profile and workspace settings.

![Screenshot of the Intercom Setting page.](images/intercom-settings.png)

## Configuring Intercom interactions
{: #config-interactions}

After you add your assistant as a teammate to an Intercom workspace, your assistant watches user interactions as they are logged in the Intercom workspace, primarily those that surface in the Unassigned inbox. The assistant does not take any action, unless it has permission to do so. Even when it has permission to act, the assistant does not do so unless it is confident in its response.

You decide which actions the assistant can take for each type of goal users want help with. As part of the initial integration process, you configure how you want each opted-in dialog branch to be managed. You can start conservatively, allowing the assistant only to assign interactions to other teammates, and over time give it more responsibility, including permission to address users directly. As you gain confidence in the capabilities of the assistant, and as your dialog changes, you will likely return again and again to the Intercom integration configuration page to make incremental changes.

To edit the Intercom configuration, complete the following steps:

1.  From the assistant, click to open the tile for the Intercom integration you created earlier.

1.  In the *Intercom Configuration* section, find the *Teammate Interactions* table.

1.  **Optional**: If you want to apply a single interaction configuration to all of the dialog branches at once, define an interaction for **All other nodes**.

    If you make no changes, the assistant is configured to take no action and agent requests are unassigned.

1.  Click **Add Integration** to define a unique interaction pattern for a specific dialog branch.

1.  From the *Select a node* drop-down list, choose the node purpose summary for the branch you want to configure.

    Remember, branches are opted in for Intercom integration when you add a summary to the *node purpose* field on the root node of the branch. If you do not see a branch that you want the Intercom app to manage from the list of options, then you might have forgotten to add text to its *node purpose* field. Go to the dialog, open the branch's root node in the edit view, and add a summary of the branch's purpose to its *node purpose* field now. For example, *Place an order*. And then return here to pick it from the drop-down list and configure it.

1.  Pick the type of action that you want the assistant to perform when this dialog node is triggered. The action type options are these:

    - **No action**: The assistant is not involved in the response.
    - **Route to agent**: The assistant evaluates the user input to determine its goal, and then routes it to the appropriate teammate.
    - **Suggest to agent**: The assistant provides the teammate with suggestions for how to respond by sharing notes with the human agent through the internal Intercom app.

      - If the user input triggers a dialog branch, meaning a root dialog node with child nodes that represents a comprehensive interaction, then the assistant indicates that it is capable of addressing the request, and offers to do so. The human agent can decide whether or not to let the assistant take over.
      - If the user input triggers a root node with no children, then the assistant simply shares the programmed response from the node with the human agent, but does not respond directly to the user.

    - **Take over**: The assistant responds to the user directly, without conferring with any other teammates.

1.  Choose the team or human agent responsible for responding to user inquiries that trigger this dialog branch.

    - An assignment is required for any nodes that are configured with the *Route to agent* action type.
    - The team or human that you assign to nodes with a *Take over* action type only gets involved if the user requests an escalation or the course of the converation leads to a dialog node that has a *Connect to human agent* response type.

    Click **Save** periodically, as you add interactions to the table.
    {: tip}

1.  To define unique interaction settings for other dialog branches, click **Add interaction** again, and repeat the previous steps.

    **Note**: If you added a *node purpose summary* to a root node in your dialog for the purpose of making the node eligible for disambiguation only, and do not want the node to be managed by the Intercom app, then you should explicitly add an interaction configuration for it, and specify the **No action** action type. Otherwise, the configuration that you define for all other nodes will be applied to it, which might specify an action type that you do not want applied to the node.

1.  When you are done configuring interactions, click **Save**, and then close the Intercom integration page.

If you subsequently want to change the integration name or description, or need to change Intercom workspace connection details, open the Intercom integration page, scroll down, and then click **Set up** to return to the integration settings page.

![Screenshot of the Set up button.](images/setup.png)

Interactions can also be assigned directly to your assistant either by Intercom's assignment rules, which can automatically assign inbound conversations to a teammate or team inbox based on some criteria, or by a manual reassignment made by a human agent at run time. Your dialog must be able to gracefully handle it if another Intercom agent mistakenly assigns a user interaction to the assistant that the assistant is not designed to handle.

## Testing the integration
{: #intercom-test}

To effectively test your Intercom integration from end-to-end, you must have access to an Intercom end-user application. You already created or edited an Intercom workspace. The workspace must have an associated user interface client. If it does not, see [Apps in Intercom ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.intercom.com/help/apps-in-intercom){: new_window} for help with setting one up.

Submit test user queries through a client application that is associated with your Intercom workspace to see how the messages are handled by Intercom. Verify that messages that are meant to be taken over by the assistant are generating the appropriate responses, and that the assistant is not responding to messages that it is not configured to take over.

## Dialog considerations
{: #intercom-dialog}

Some rich responses that you add to a dialog are displayed differently within the "Try it out" pane from how they are displayed to Intercom users. The table below describes how the response types are treated by Intercom.

| Response type | How displayed to Intercom users  |
|---------------|---------------------------|
| **Option**    | The options are displayed as a numbered list. In the **title** or **description** field, provide instructions that explain to the user how to choose an option from the list. |
| **Image**     | The image **title**, **description**, and the image itself are rendered. |
| **Pause**     | Whether or not you enable it, a typing indicator is not displayed during the pause. |

See [Rich responses](dialog-overview.html#multimedia) for more information about response types.
