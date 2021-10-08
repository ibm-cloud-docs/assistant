---

copyright:
  years: 2020, 2021
lastupdated: "2021-07-29"

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

{{site.data.content.newlink}}

# Integrating with Salesforce
{: #deploy-salesforce}

Integrate your web chat with a Salesforce service desk solution so your customers always get the help they need.
{: shortdesc}

Integrate with a Salesforce service desk by deploying your assistant with the web chat integration. The web chat serves as the client interface for your assistant. If, in the course of a conversation with your assistant, a customer asks to speak to a person, you can transfer the conversation directly to a Salesforce agent.

Salesforce is a customer relationship management solution that brings companies and customers together. It is one integrated CRM platform that gives all your departments, including marketing, sales, commerce, and service, a single, shared view of every customer.

## Before you begin
{: #deploy-salesforce-prereqs}

To connect to a Salesforce service desk, your organization must have a Salesforce Service Cloud plan that supports Live Agent Chat. Chat support is available in Salesforce Service Cloud Unlimited and Enterprise plans. It is also available with Performance or Developer plans that were created after 14 June 2012.

Your organization must have a [Salesforce chat app](https://help.salesforce.com/articleView?id=dev_tabsets.htm&type=5){: external} with the following characteristics:

- Console navigation
- Navigation items: Cases, Chat sessions, Chat transcripts
- User profiles: Apply the appropriate profiles to ensure that agents can access the app and view chat history information. You can limit access to this page later. See [Profiles](https://help.salesforce.com/articleView?id=admin_userprofiles.htm&type=5){: external}.
- A [chat deployment](https://help.salesforce.com/articleView?id=live_agent_create_deployments.htm&type=5){: external}.
- A [chat button deployment](https://help.salesforce.com/articleView?id=live_agent_create_buttons.htm&type=5){: external}.
- Routing must be configured for the chat button. See [Chat routing options](https://help.salesforce.com/articleView?id=live_agent_chat_routing_options.htm&type=5){: external}. 
- If you choose omni-channel routing, be sure to include omni-channel as a utility in the chat app. See [Omni-Channel](https://help.salesforce.com/articleView?id=omnichannel_intro.htm&type=5){: external}.

You must have a level of access to your Salesforce service desk deployment that allows you to do the following things:

- Edit the chat app
- Get chat deployment and button code details
- Add custom fields to layout objects
- Create Visualforce pages

If you don't, ask someone with the appropriate level of access to perform this procedure for you.

## Setting up the Salesforce service desk connection
{: #deploy-salesforce-task}

To set up a Salesforce service desk integration, complete the following steps:

1.  Create a web chat integration. For more information, see [Integrating the web chat with your website](/docs/assistant?topic=assistant-deploy-web-chat).

1.  From the web chat integration page in {{site.data.keyword.conversationshort}}, set the **Allow transfers to live agents** switch to **On**, and then choose **Salesforce** as the service desk type. Click **Next**.

1.  For {{site.data.keyword.conversationshort}} to connect to a Salesforce service desk, it needs information about your organization's Salesforce chat deployment and button implementations. Specifically, it needs the API endpoint, organization ID, deployment ID, and button ID. The service can derive the values that it needs from code snippets that you copy and paste to this configuration page. {: #deploy-salesforce-get-deployment-info}

    In a separate browser tab or window, open your Salesforce account settings page. Log in with a user ID that has administrative privileges. You must switch back and forth between your Salesforce and {{site.data.keyword.conversationshort}} web chat integration setup pages. It's easier to do so if you have both pages open at once.
    {: tip}

    - Get the deployment code for your Salesforce Agent Configuration chat deployment. 
    
      Go to the Salesforce **Feature Settings>Service>Chat>Deployments** page. Find your organization's deployment. Scroll to the end of the chat deployment configuration page and copy the *Deployment Code* snippet.
    - Paste the deployment code snippet into the **Deployment code** field in the {{site.data.keyword.conversationshort}} Salesforce configuration page.
    - Get the Chat Button code. 
    
      Go to the Salesforce **Feature Settings>Service>Chat>Chat Buttons & Invitations** page. Find your organization's button implementation. Scroll to the end of the page, and then copy the *Chat Button Code* snippet.
    - Paste the chat button code snippet into the **Chat button code** field in the {{site.data.keyword.conversationshort}} Salesforce configuration page, and then click **Next**.

1.  Add a chat app that enables the Salesforce agent to see a history of the chat. To do so, create a Visualforce page, and then add a chat app to the page. {: #deploy-salesforce-add-visualforce-page}

1.  Add custom fields to the Salesforce chat transcript layout. {: #deploy-salesforce-add-custom-fields}

    This is a one-time task. If the fields already exist for your organization, you can skip this step.
    {: note}

    These custom fields are referenced from the Visualforce page code that you will use in the next step.

    See [Create Custom Fields](https://help.salesforce.com/articleView?id=adding_fields.htm&type=5){: external}.

    From the Salesforce **Data>Objects and Fields>Object Manager>Chat Transcript>Fields & Relationships** page, create the following custom fields:

    - **Token**: Stores a {{site.data.keyword.conversationshort}} authentication token that secures the communication between Salesforce and your assistant.

      - **Data Type**: Text Area (Long)
      - **Field Label**: `x-watson-assistant-key`
      - **Field Length**: Specify the maximum length allowed to ensure it can hold a token that might contain over 100,000 characters.

1.  Create a Visualforce page.

    Visualforce pages are the mechanism that Salesforce provides for you to customize a live agent's console by adding your own pages to it. A Visualforce page is similar to a standard web page, but it provides ways for you to access, display, and update your organization’s data. Pages can be referenced and invoked by using a unique URL, just as HTML pages on a traditional web server can be. See [Create Visualforce Pages](https://help.salesforce.com/articleView?id=pages_creating.htm&type=5){: external}

    - From the web chat integration page in {{site.data.keyword.conversationshort}}, copy the code snippet from the Visualforce page markup field.
    - Switch to your Salesforce web page. Search for **Visualforce Pages**. Create a page. Add a label and name to the page. Select the *Available for Lightning Experience, Lightning Communities, and the mobile app* checkbox. Paste the code snippet that you copied in the previous step into the page markup field.

1.  Add the Visualforce page that you created to the Salesforce chat app. {: #deploy-salesforce-add-page-to-layout}

    To ensure the Salesforce agents can see history of the chat between the customer and your assistant, you must add the page that you created earlier into the console that they use to keep track of their work. See [Create and Configure Lightning Experience Record Pages](https://help.salesforce.com/articleView?id=lightning_app_builder_customize_lex_pages.htm&type=5){: external}.
    
    - From the Salesforce App Launcher, open the chat app that you created for your agents to talk to customers.
    - Open the *Chat Transcripts* object, and then select a transcript page.
    - Click the *Setup* icon, and then select *Edit Page*.
    - Drag the Visualforce component and drop it into the Chat Transcript Record page layout where you want the chat window to be displayed.
    - In the component editor, select the Visualforce page that you created earlier, make any adjustments to the component height that you want, and then click *Save*.

      If you do make changes, make sure the height of the Visualforce page is 20 px smaller than the height of the component that you add it to. By default, the height of the component is 300 px and the height of the Visualforce page is 280 px. (The height of the Visualforce page is specified in the `height` attribute of the `iframe` HTML element in the code snippet that you copy and paste.)
      {: important}
 
    - Click *Activation*, and then click the APP, RECORD TYPE, AND PROFILE tab.
    - Select the apps to which you want to apply the page layout, and then click *Next*.
    - Select the appropriate record type, such as Main, and then click *Next*.
    - Select user profiles to give the appropriate set of users access to the page. Limit the group to include only those who you want to be able to view chat history information in the page. 
    - Click *Next*, and then click *Save*.

1.  From the Salesforce configuration page in {{site.data.keyword.conversationshort}}, click **Save and exit** to finish setting up the connection.

When you test the service desk integration, make sure there is at least one agent with `Available` status.

Watch the following 5-minute video to watch someone set up a connection to a Salesforce service desk.

![Setting up a Salesforce service desk connection](https://www.youtube.com/embed/mUx-qvZH-qo){: video output="iframe" id="youtubeplayer" frameborder="0" width="560" height="315" webkitallowfullscreen mozallowfullscreen allowfullscreen}

To read a transcript of the video, [open the video on YouTube.com](https://www.youtube.com/watch?v=mUx-qvZH-qo&feature=emb_imp_woyt), click the *More actions* icon, and then choose *Open transcript*.

## Adding transfer support to your dialog
{: #deploy-salesforce-dialog-prereq}

Update your dialog to make sure it understands when users request to speak to a person, and can transfer the conversation properly. For more information, see [Adding chat transfer support](/docs/assistant?topic=assistant-dialog-support#dialog-support-transfers).

## Adding routing logic for transfers
{: #deploy-salesforce-routing}

When you enable transfers to the Salesforce service desk, the default routing preference that you specify is used. However, there might be times when you want to route a customer to a different Salesforce agent queue. For example, your dialog might have a root dialog node that conditions on a `#close_account` intent. For that branch of the conversation only, you want to transfer customers to agents in the sales queue who are authorized to offer incentives as a way to retain customers. You can direct transfers to specific agent queues by adding routing logic to your dialog.

You can specify alternate routing preferences based on:

- browser information
- the current topic of conversation

### Routing based on browser information
{: #deploy-salesforce-routing-browser-info}

When a customer interacts with the web chat, information about the current web browser session is collected. For example, the URL of the current page is collected. You can use this information to add custom routing rules to your dialog. For example, if the customer is on the Products page when a transfer to a human is requested, you might want to route the chat transfer to a queue with agents who are experts in your product portfolio. If the customer is on the Returns page, you might want to route the chat transfer to a queue with agents who know how to help customers return merchandise. 

For more information, see [Web chat: Accessing browser information](/docs/assistant?topic=assistant-dialog-integrations#dialog-integrations-chat-browser-info).

### Routing by topic
{: #deploy-salesforce-routing-topic}

When you enable transfers to the Salesforce service desk, you copy and paste code snippets from Salesforce into the service desk transfer setup page. These code snippets define how transferred conversations are handled within Salesforce. Routing rules are included in the initial transfer configuration. The routing rules identify the queue of agents to which messages from the assistant are transferred by default.

The code that you add to the setup page when you configure the service desk integration shares the following required information with {{site.data.keyword.conversationshort}}:

- `organization_id`: Unique ID of the organization. A company can have more than one organization set up in Salesforce.
- `chat_api_endpoint`: Salesforce API endpoint that is used by the integration to communicate with Salesforce.
- `deployment_id`: Unique ID of the deployment. An organization can have multiple deployments.
- `button_id`: Unique ID of a button, which defines the specific routing rules for incoming messages. Each deployment can have multiple buttons associated with it.

To override the default routing rules, you must specify a new value for the `button_id`. Before you perform this procedure, find out the ID of the Salesforce button implementation with the alternative routing rules that you want to use.

To add custom routing logic, complete the following steps:

1.  From the *Dialog* page, find the root dialog node for the branch of the conversation that you want to route to a specific group of Salesforce agents which is distinct from the default group.

1.  Find the dialog node in the branch where you want the transfer to take place, and then add the *Connect to human agent* response type as the dialog node response type.

    For more information, see [Adding a *Connect to human agent* response type](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-add-connect-to-human-agent).

1.  After you add the response type and customize the transfer messages, select *Salesforce* from the **Service desk routing** field.

1.  In the **Button ID** field, add the `button_id` value for the alternate routing destination that you want the assistant to use for conversations about only this topic. For example, `5733i0000008yGz`.

    Be sure to specify the exact right syntax for the `button_id` value. The value is not validated by the service as you add it to your dialog.
    {: note}

If the special routing that you apply fails to deliver the message for any reason, the routing preference that is specified in the Salesforce service desk setup page is used. If both routing preferences fail, the transferred message is treated like a new message in Salesforce. The standard Salesforce routing rules are followed.
