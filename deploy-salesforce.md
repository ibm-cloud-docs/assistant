---

copyright:
  years: 2020
lastupdated: "2020-06-02"

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

# Integrating with Salesforce ![Beta](images/beta.png)
{: #deploy-salesforce}

Integrate your Web Chat with a Salesforce service desk solution so your customers always get the help they need.
{: shortdesc}

![Plus or Premium plan only](images/plus.png) This integration type is available to Plus or Premium plan users only.

Integrate with a Salesforce service desk by deploying your assistant with the Web Chat integration. The Web Chat serves as the client interface for your assistant. If, in the course of a conversation with your assistant, a customer asks to speak to a person, you can transfer the conversation directly to a Salesforce agent.

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

1.  Create a Web Chat integration. For more information, see [Integrating with your website](/docs/assistant?topic=assistant-deploy-web-chat).

1.  From the Web Chat integration page in {{site.data.keyword.conversationshort}}, switch the **Allow transfers to live agents** toggle to **On**, and then choose **Salesforce** as the service desk type. Click **Set up**.

1.  {: #deploy-salesforce-get-deployment-info}For {{site.data.keyword.conversationshort}} to connect to a Salesforce service desk, it needs information about your organization's Salesforce chat deployment and button implementations. Specifically, it needs the API endpoint, organization ID, deployment ID, and button ID. The service can derive the values that it needs from code snippets that you copy and paste to this configuration page.

    In a separate browser tab or window, open your Salesforce account settings page. Log in with a user ID that has administrative privileges. You must switch back and forth between your Salesforce and {{site.data.keyword.conversationshort}} Web Chat integration setup pages. It's easier to do so if you have both pages open at once.
    {: tip}

    - Get the deployment code for your Salesforce Agent Configuration chat deployment. 
    
      Go to the Salesforce **Setup>Chat>Deployments** page. Find your organization's deployment. Scroll to the end of the chat deployment configuration page and copy the *Deployment Code* snippet.
    - Paste the deployment code snippet into the **Deployment code** field in the {{site.data.keyword.conversationshort}} Salesforce configuration page.
    - Get the Chat Button code. 
    
      Go to the Salesforce **Setup>Chat>Chat Buttons & Invitations** page. Find your organization's button implementation. Scroll to the end of the page, and then copy the *Chat Button Code* snippet.
    - Paste the chat button code snippet into the **Chat button code** field in the {{site.data.keyword.conversationshort}} Salesforce configuration page, and then click **Next**.

1.  {: #deploy-salesforce-add-visualforce-page}Add a chat app that enables the Salesforce agent to see a history of the chat. To do so, create a Visualforce page, and then add a chat app to the page.

1.  {: #deploy-salesforce-add-custom-fields}Add custom fields to the Salesforce chat transcript layout.

    This is a one-time task. If the fields already exist for your organization, you can skip this step.
    {: note}

    These custom fields are referenced from the Visualforce page code that you will use in the next step.

    See [Create Custom Fields](https://help.salesforce.com/articleView?id=adding_fields.htm&type=5){: external}.

    From the Salesforce **Setup>Object Manager>Chat Transcript>Fields & Relationships** page, create the following custom fields:

    - **Session**: Stores a session ID that tracks ongoing conversations.

      - **Data Type**: Text
      - **Field Label**: `x-watson-assistant-session`
      - **Field Length**: 36

    - **Token**: Stores a {{site.data.keyword.conversationshort}} authentication token that secures the communication between Salesforce and your assistant.

      - **Data Type**: Text Area (Long)
      - **Field Label**: `x-watson-assistant-token`
      - **Field Length**: Specify the maximum length allowed to ensure it can hold a token that might contain over 100,000 characters.

1.  Create a Visualforce page.

    Visualforce pages are the mechanism that Salesforce provides for you to customize a live agent's console by adding your own pages to it. A Visualforce page is similar to a standard web page, but it provides ways for you to access, display, and update your organization’s data. Pages can be referenced and invoked by using a unique URL, just as HTML pages on a traditional web server can be. See [Create Visualforce Pages](https://help.salesforce.com/articleView?id=pages_creating.htm&type=5){: external}

    - From the Web Chat integration page in {{site.data.keyword.conversationshort}}, copy the code snippet from the Visualforce page markup field.
    - Switch to your Salesforce web page. Search for **Visualforce Pages**. Create a page. Add a label and name to the page, and then paste the code snippet that you copied in the previous step into the page markup field.

1.  {: #deploy-salesforce-add-page-to-layout}Add the Visualforce page that you created to the Salesforce chat app.

    To ensure the Salesforce agents can see history of the chat between the customer and your assistant, you must add the page that you created earlier into the console that they use to keep track of their work. See [Create and Configure Lightning Experience Record Pages](https://help.salesforce.com/articleView?id=lightning_app_builder_customize_lex_pages.htm&type=5){: external}.
    
    - From the Salesforce App Launcher, open the chat app that you created for your agents to talk to customers.
    - Open the *Chat Transcripts* object, and then select a transcript page.
    - Click the *Setup* icon, and then select *Edit Page*.
    - Drag the Visualforce component and drop it into the Chat Transcript Record page layout where you want the chat window to be displayed.
    - In the component editor, select the Visualforce page that you created earlier, make any adjustments to the widget height that you want, and then click *Save*.
    - Click *Activation*, and then click the APP, RECORD TYPE, AND PROFILE tab.
    - Select the apps to which you want to apply the page layout, and then click *Next*.
    - Select the appropriate record type, such as Master, and then click *Next*.
    - Select user profiles to give the appropriate set of users access to the page. Limit the group to include only those who you want to be able to view chat history information in the page. 
    - Click *Next*, and then click *Save*.

1.  From the Salesforce configuration page in {{site.data.keyword.conversationshort}}, click **Save** to finish setting up the connection.

When you test the service desk integration, make sure there is at least one agent with `Available` status.

Watch the following 5-minute video to watch someone set up a connection to a Salesforce service desk.

<iframe class="embed-responsive-item" id="youtubeplayer0" title="Setting up a Salesforce service desk connection" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/mUx-qvZH-qo" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Adding transfer support to your dialog
{: #deploy-salesforce-dialog-prereq}

Update your dialog to make sure it understands when users request to speak to a person, and can transfer the conversation properly. See [Adding transfer support to your dialog](/docs/assistant?topic=assistant-deploy-web-chat#deploy-web-chat-dialog-prereq).