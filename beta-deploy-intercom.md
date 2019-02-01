---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-01"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
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

# Integrating with Intercom ![Plus or Premium plan only](images/premium.png)
{: #beta-deploy-intercom}

Intercom is a customer service messaging platform that evaluates user support inquiries and routes them to the best human agent or team of agents to address the user's question or request.
{: shortdesc}

This feature is available for use by participants in the beta program only. To find out how to request access, see [Particpate in the beta program](feedback.html#feedback-beta).

![Beta](images/beta.png) IBM releases services, features, and language support for your evaluation that are classified as beta. These features might be unstable, might change frequently, and might be discontinued with short notice. Beta features also might not provide the same level of performance or compatibility that generally available features provide and are not intended for use in a production environment.

Intercom has partnered with IBM to add a new agent to the team, a virtual Watson Assistant. You can integrate your assistant with an Intercom application to enable the app to seamlessly pass user conversations between your assistant and human support agents.

This integration is available only to Plus or Premium plan users.
{: tip}

If you integrate the assistant with Intercom, the Intercom application becomes the client application for your skill. All interactions with users are initiated through and managed by Intercom.

There is currently no way to pass an ongoing conversation from one integration channel to another.

## One-time agent creation
{: #beta-deploy-intercom-account-prereq}

You or someone in your organziation must complete these one-time prerequisite steps before you add the Intercom integration to your assistant.

1.  Create a functional email account for your assistant.

    Each assistant must have a valid email address before it can be added to a team in Intercom.
1.  From your Intercom workspace, add the assistant to your team as a new agent.

    Go to the teammate settings page in your Intercom workspace, invite the assistant as a new agent by adding the email to the invite field.

    If you don't have an Intercom workspace set up yet, create one at [www.intercom.com](https://www.intercom.com). At a minimum, you need a subscription to the *Inbox* product from Intercom to be able to create a workspace.

1.  From the assistant email account you created earlier, find the email invitation sent by Intercom. Click the link in the email to join the team. Sign up using the assistant's functional email address. Join the team.

1.  **Optional**: Update the profile for your assistant.

    You can edit the name and profile picture for your assistant. This profile represents the assistant in private agent communications within your workspace, and in public interactions with customers through your Intercom apps. Create a profile that reflects your brand.

    Click the Intercom profile icon in the navigation bar to access profile and workspace settings.

    ![Screenshot of the Intercom Setting page.](images/intercom-settings.png)

## Preparing the dialog
{: #beta-deploy-intercom-dialog-prereq}

Complete these steps in your dialog skill so the assistant can handle user requests, and can pass the conversation to a human agent when someone asks for one.

1.  Add an intent to your skill that can recognize a user's request to speak to a human.

    You can create your own intent or add the prebuilt intent named `#General_Connect_to_Agent` that is provided with the **General** content catalog developed by IBM.

1.  Add a root node to your dialog that conditions on the intent that you created in the previous step. Choose **Connect to human agent** as the response type.

1.  Prepare each dialog branch to be triggered by the assistant from the Intercom app.

    Every root dialog node in your dialog is available to the assistant from Intercom, including root nodes in folders. You will specify the action that should be taken for each dialog branch later when you configure the Intercom teammate interactions.

    Fill in the following fields of the root node of each branch:

    - **External node name**: Add a summary of the purpose of the dialog branch. For example, *Find a store*.

      This information is shown to other agents on the assistant's team when the assistant offers to answer a user query. If there is more than one dialog node that can address the query, the assistant shares a list of response options with human agent teammates to get their advice about which response to use.
    - **Node name**: Give the node a name. This name is how you will identify the node when you configure interactions for it later. If you don't add a name, you will have to choose the node based on its node ID instead.

    ![Screenshot of the field in the node edit view where you add the node purpose summary.](images/disambig-node-purpose.png)

    **Important**: Do **not** add an external node name to the root node that you created in the previous step. Identifying the node that the user interacted with just before asking to speak to a human is important. It can help the service identify nodes that need to be improved. To find these nodes, when an escalation occurs, the service looks for the node purpose summary for the most recently processed node. Including a summary for the root node that does no actual work, but only transfers users to human agents can hinder the service's ability to identify the last goal-oriented node that the user interacted with before requesting the escalation.

1.  If a child node in the branch conditions on a follow-up request or question that you do not want the assistant to handle, add a **Connect to human agent** response type to the node. For example, you might want to add this to nodes that cover sensitive issues only a human should handle or that track when an assistant repeatedly fails to understand a user.

    At run time, if the conversation reaches this child node, the dialog is passed to a human agent at this point. (Later, when you set up the Intercom integration, you will choose a human agent as a backup for each topic that you assign to your assistant.)

Your dialog is now ready to support your assistant in Intercom.

## Adding an Intercom integration
{: #add-intercom}

1.  From the Assistants tab, click to open the assistant tile that you want to deploy.

1.  From the Integrations section, click **Add Integration**.

1.  Click **Intercom**.

1.  Follow the instructions that are provided on the screen to complete the integration process.

    When you are instructed to connect your assistant to Intercom, be sure to log in to Intercom using the assistant's functional email address and password, not your own.

## Configuring message routing
{: #beta-deploy-intercom-config-backup}

After you add your assistant as a teammate to an Intercom workspace, your assistant watches user interactions as they are logged in the Intercom workspace, specifically those that are routed to the *Unassigned* inbox. If the assistant sees a query that it is confident it can answer, it shows one of its human teammates a list of possible responses and gets the teammate's approval before it responds.

Assign a human teammate as a backup for the assistant in case the assistant needs to transfer an in-progress conversation to a human. You can choose different teams or teammates for each dialog branch.

To set up routing assignments, complete the following steps:

1.  From the assistant, click to open the tile for the Intercom integration you created earlier.

1.  Click **It's ready** to confirm that you have prepared your dialog.

1.  In the *Settings* section, click **Manage rules**.

    If you make no changes, the assistant is configured to take no action and agent requests are unassigned.

1.  Click **New rule** to define how you want a specific dialog branch to be routed to a human teammate, if necessary.

1.  From the *Choose node* drop-down list, choose the node for the branch you want to configure.

    Remember, branches are identified by their node name. If you did not specify a node name, then the node's ID is displayed instead.

1.  Choose the team or human agent teammate responsible for responding to user inquiries that trigger this dialog branch.

1.  To define routing rules for other dialog branches, click **New rule** again, and repeat the previous steps.

1.  After adding rules, click **Return to overview** to exit the page.

You can start incorporating the assistant into your Intercom team conservatively, allowing the assistant only to suggest responses for messages that it routes to other teammates. Over time, you can give it more responsibility, including permission to address users directly. When you think the assistant it ready to take on a bigger role, complete the next procedure.

## Give the assistant permission to answer user queries
{: #beta-deploy-intercom-config-action}

The assistant does not take any action, unless it has permission to do so. Even when it has permission to act, the assistant does not do so unless it is confident in its response.

You decide which actions the assistant can take for each type of goal users want help with. As part of the initial integration set up process, you configure how you want each dialog branch to be managed by defining rules.

To edit the Intercom configuration, complete the following steps:

1.  From the assistant, click to open the tile for the Intercom integration you created earlier.

1.  From the *Enable your assistant to monitor an inbox* section, click to switch it **On**.

1.  In *Settings*, click **Manage rules**.

1.  **Optional**: If you want to apply a single interaction configuration to all of the dialog branches at once, define an interaction for **All other nodes**.

    If you make no changes, the assistant is configured to take no action and agent requests are unassigned.

1.  Click **New rule** to define a unique interaction pattern for a specific dialog branch.

1.  From the *Choose node* drop-down list, choose the node for the branch you want to configure.

    Remember, branches are identified by their node name. If you did not specify a node name, then the node's ID is displayed instead.

1.  Pick the type of action that you want the assistant to perform when this dialog node is triggered. The action type options are these:

    - **Do nothing**: The assistant is not involved in the response.
    - **Send to team or teammate**: The assistant evaluates the user input to determine its goal, and then routes it to the appropriate teammate.
    - **Suggest to team or teammate**: The assistant provides the teammate with suggestions for how to respond by sharing notes with the human agent through the internal Intercom app.

      - If the user input triggers a dialog branch, meaning a root dialog node with child nodes that represents a comprehensive interaction, then the assistant indicates that it is capable of addressing the request, and offers to do so. The human agent can decide whether or not to let the assistant take over.
      - If the user input triggers a root node with no children, then the assistant simply shares the programmed response from the node with the human agent, but does not respond directly to the user.

    - **Answer**: The assistant responds to the user directly, without conferring with any other teammates.

    Remember, some actions require teammate involvement. Be sure to go back and add a rule to assign the dialog node to the right person. An assignment is required for any nodes that you assign the *Send to team or teammate* action type, for example.

1.  To define unique interaction settings for other dialog branches, click **New rule** again, and repeat the previous steps.

1.  After adding rules, click **Return to overview** to exit the page.

As you gain confidence in the capabilities of the assistant, and as your dialog changes, you will likely return again and again to the Intercom integration configuration page to make incremental changes.

Interactions can also be assigned directly to your assistant either by Intercom's assignment rules, which can automatically assign inbound conversations to a teammate or team inbox based on some criteria, or by a manual reassignment made by a human agent at run time. Your dialog must be able to gracefully handle it if another Intercom agent mistakenly assigns a user interaction to the assistant that the assistant is not designed to handle.

## Testing the integration
{: #beta-deploy-intercom-test}

To effectively test your Intercom integration from end-to-end, you must have access to an Intercom end-user application. You already created or edited an Intercom workspace. The workspace must have an associated user interface client. If it does not, see [Apps in Intercom ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.intercom.com/help/apps-in-intercom){: new_window} for help with setting one up.

Submit test user queries through a client application that is associated with your Intercom workspace to see how the messages are handled by Intercom. Verify that messages that are meant to be answered by the assistant are generating the appropriate responses, and that the assistant is not responding to messages that it is not configured to answer.

## Dialog considerations
{: #beta-deploy-intercom-dialog}

Some rich responses that you add to a dialog are displayed differently within the "Try it out" pane from how they are displayed to Intercom users. The table below describes how the response types are treated by Intercom.

| Response type | How displayed to Intercom users  |
|---------------|---------------------------|
| **Option**    | The options are displayed as a numbered list. In the **title** or **description** field, provide instructions that explain to the user how to choose an option from the list. |
| **Image**     | The image **title**, **description**, and the image itself are rendered. |
| **Pause**     | Whether or not you enable it, a typing indicator is not displayed during the pause. |

See [Rich responses](/docs/services/assistant/dialog-overview.html#multimedia) for more information about response types.
