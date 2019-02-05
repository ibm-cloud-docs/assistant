---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-04"

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

    If you don't have an Intercom workspace set up yet, create one at [www.intercom.com ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.intercom.com). At a minimum, you need a subscription to the *Inbox* product from Intercom to be able to create a workspace.

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

    Every root dialog node in your dialog is available to the assistant from Intercom, including root nodes in folders. You will specify the action that should be taken for each dialog branch later when you configure the Intercom integration.

    Fill in the following fields of the root node of each dialog branch:

    - **External node name**: Add a summary of the purpose of the dialog branch. For example, *Find a store*.

      This information is shown to other agents on the assistant's team when the assistant offers to answer a user query. If there is more than one dialog node that can address the query, the assistant shares a list of response options with human agent teammates to get their advice about which response to use.
    - **Node name**: Give the node a name. This name is how you will identify the node when you configure interactions for it later. If you don't add a name, you will have to choose the node based on its node ID instead.

    ![Screenshot of the field in the node edit view where you add the node purpose summary.](images/disambig-node-purpose.png)

    Do **not** add an external node name to the root node that you created in the previous step. Identifying the node that the user interacted with just before asking to speak to a human is important. It can help the service identify nodes that need to be improved. To find these nodes, when an escalation occurs, the service looks for the node purpose summary for the most recently processed node. Including a summary for the root node that does no actual work, but only transfers users to human agents can hinder the service's ability to identify the last goal-oriented node that the user interacted with before requesting the escalation.
    {: important}

1.  If a child node in the branch conditions on a follow-up request or question that you do not want the assistant to handle, add a **Connect to human agent** response type to the node. For example, you might want to add this response type to nodes that cover sensitive issues only a human should handle or that track when an assistant repeatedly fails to understand a user.

    At run time, if the conversation reaches this child node, the dialog is passed to a human agent at that point. (Later, when you set up the Intercom integration, you will choose a human agent as a backup for each branch.)

Your dialog is now ready to support your assistant in Intercom.

## Adding an Intercom integration
{: #beta-deploy-intercom-add-intercom}

1.  From the Assistants tab, click to open the assistant tile that you want to deploy.

1.  From the Integrations section, click **Add Integration**.

1.  Click **Intercom**.

1.  Follow the instructions that are provided on the screen to complete the integration process.

## Connecting the assistant to Intercom
{: #beta-deploy-intercom-connect}

As soon as you give Intercom permission to use the assistant, the assistant becomes a viable member of the Intercom team.

Human agents can assign messages to the assistant by using Intercom's assignment rules, which can automatically assign inbound conversations to a teammate or team inbox based on some criteria, or by a manual reassignment made by a human agent at run time. See the [Intercom documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.intercom.com/help/support-and-retain-customers/work-as-a-team/assign-conversations-to-teammates-and-teams) for more details.

The fact that the assistant is now available to receive assignments from Intercom teammates is the reason it's so important that you set up your dialog *before* you connect the assistant to Intercom.
{: important}

After you click **Connect** and are redirected to the Intercom site, be sure to log in to Intercom using the assistant's functional email address and password, not your own.

## Configuring message routing
{: #beta-deploy-intercom-config-backup}

After you connect your assistant to an Intercom workspace, other Intercom teammates can assign messages to the assistant. Assign a human teammate as a backup for the assistant in case the assistant needs to transfer an in-progress conversation back to a human. You can choose a different team or teammate to be the backup contact for each dialog branch.

To set up routing assignments for escalations from the assistant to a human, complete the following steps:

1.  From the Intercom integration page, click **My Dialog Skill is ready** to confirm that you have prepared your dialog.

    Do not click this button unless you have completed the [Preparing the dialog](#beta-deploy-intercom-dialog-prereq) procedure.
    {: important}

1.  In the *Settings* section, click **Manage rules**.

    If you make no changes, the assistant is configured to take no action and the backup contact for all of the nodes remains unassigned.

1.  Click **New rule**.

1.  From the *Choose node* drop-down list, choose the node for the dialog branch you want to configure.

    Remember, branches are identified by their node name. If you did not specify a node name, then the node's ID is displayed instead.

1.  Choose the team or human agent teammate to be the backup contact for this dialog branch. The user query will be escalated to this person if the assistant cannot answer the query or hits a sensitive child node that is configured to be answered only by a human.

1.  To define routing rules for other dialog branches, click **New rule** again, and repeat the previous steps.

1.  After adding rules, click **Return to overview** to exit the page.

## Give the assistant permission to answer user queries
{: #beta-deploy-intercom-config-action}

When you want the assistant to start monitoring and answering messages that are sent to one or more Intercom inboxes, switch on monitoring.

Your assistant watches user inquiries as they are logged in the Intercom workspace, specifically those that are routed to the *Unassigned* inbox. When it is confident that it knows how to answer a user query, the assistant responds to the user directly. (The assistant is confident when the top intent identified by the service has a confidence score of 0.75 or higher.)

If you do not want the assistant to answer certain types of user queries, then you can add rules to specify other actions for the assistant to take per dialog branch. For example, you might want to start incorporating the assistant into the Intercom team more conservatively, allowing the assistant only to suggest responses as it routes user messages to other teammates for them to answer. Over time, after the assistant proves its usefulness, you can give it more responsibility.

To configure how you want the assistant to handle specific dialog branches, define rules.

1.  From the Intercom integration page, in the *Enable your assistant to monitor an inbox* section, switch monitoring **On**.

1.  In *Settings*, click **Manage rules**.

    If you do not define rules, the assistant is configured to answer user inquiries that it is confident it can address.

1.  Click **New rule** to define a unique interaction pattern for a specific dialog branch.

1.  From the *Choose node* drop-down list, choose the node for the branch you want to configure.

    Remember, branches are identified by their node name. If you did not specify a node name, then the node's ID is displayed instead.

1.  Pick the type of action that you want the assistant to perform when this dialog node is triggered. The action type options are these:

    - **Do nothing**: The assistant is not involved in the response.
    - **Send to team or teammate**: The assistant evaluates the user input to determine its goal, and then routes it to the appropriate teammate.
    - **Suggest to team or teammate**: The assistant provides the teammate with suggestions for how to respond by sharing notes with the human agent through the internal Intercom app.

      - If the user input triggers a dialog branch, meaning a root dialog node with child nodes that represents a comprehensive interaction, then the assistant indicates that it is capable of addressing the request, and offers to do so. The assistant shows the human teammate a list of possible responses and gets the teammate's approval before it responds. The human agent can decide whether or not to let the assistant take over.
      - If the user input triggers a root node with no children, then the assistant simply shares the programmed response from the node with the human agent, but does not respond directly to the user.

    - **Answer**: The assistant responds to the user directly, without conferring with any other teammates.

    Some actions require teammate involvement. Be sure to go back and add a rule to assign the dialog node to the right person. An assignment is required for any nodes that you assign the *Send to team or teammate* action type, for example.

1.  To define unique interaction settings for other dialog branches, click **New rule** again, and repeat the previous steps.

1.  After adding rules, click **Return to overview** to exit the page.

As your dialog changes, you will likely return to the Intercom integration page to make incremental configuration changes.

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
