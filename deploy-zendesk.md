---

copyright:
  years: 2020
lastupdated: "2020-05-04"

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

# Integrating with Zendesk ![Beta](images/beta.png)
{: #deploy-zendesk}

Integrate your Web Chat with a Zendesk service desk solution so your customers always get the help they need.
{: shortdesc}

Integrate with a Zendesk service desk by deploying your assistant with the Web Chat integration. The Web Chat serves as the client interface for your assistant. If, in the course of a conversation with your assistant, a customer asks to speak to a person, you can transfer the conversation directly to a Zendesk agent.

Zendesk Chat lets you help customers in real time, which increases customer satisfaction. And satisfied customers are happier customers. To learn more about this service desk solution, see the [Zendesk website](https://www.zendesk.com/chat/){: external}.

Zendesk Chat is an add-on to Zendesk Support. Zendesk Support puts all your customer support interactions in one place, so communication is seamless, personal, and efficient, which means more productive agents and satisfied customers.

## Before you begin
{: #deploy-zendesk-prereqs}

1.  You must have a Zendesk account. If not, create one. 

    A Zendesk Chat Enterprise plan is required.
    {: important}

## Setting up the Zendesk service desk connection
{: #deploy-zendesk-task}

To set up a Zendesk service desk integration, complete the following steps:

1.  Create a Web Chat integration. For more information, see [Integrating with your website](/docs/assistant?topic=assistant-deploy-web-chat).

1.  From the Web Chat integration page in {{site.data.keyword.conversationshort}}, switch the **Allow transfers to live agents** toggle to **On**, and then choose **Zendesk** as the service desk type, and then click **Set up**.
1.  {: #deploy-zendesk-get-account-key}Add the account key for your Zendesk account. To get the account key for your Zendesk account, follow these steps:

    - Log in to your Zendesk subdomain.
    
    - Open the Zendesk Chat Dashboard.

       From the Zendesk Support dashboard, you can click the *Zendesk Products* icon in the header, and then click the *Chat* icon.

       ![Screen capture of the chat icon in the header.](images/zd-open-chat.png)
    
    - Click your profile, and then click *Check Connection*.

       ![Screen capture of the Zendesk user interface to show where the profile is located.](images/zd-status-dropdown.png)

    - Copy the account key value.

       ![Screen capture of the connection dialog.](images/zd-account-key.png)

    - Return to the setup page in {{site.data.keyword.conversationshort}}, and then paste the key into the field. Click **Access account**.

1.  {: #deploy-zendesk-add-private-app}Install the {{site.data.keyword.conversationshort}} private application in your Zendesk Chat subdomain.

    When you create a Zendesk Chat account, you specify a subdomain. Afterward, your Zendesk console is available from a URL with the syntax: `<subdomain>.zendesk.com`. For example, `ibm.zendesk.com`.
    
    IBM provides an application that you can install in your Zendesk Chat domain. When a customer asks to speak to a person, your assistant will share a chat summary for the transferred conversation with the Zendesk agent by using this private app.

    - Download the Watson Assistant Zendesk application from the Zendesk Chat setup page in {{site.data.keyword.conversationshort}}.

    - Copy the credentials that are generated for you in the **Watson Assistant Zendesk app credentials** field. You will need them in a later step.

    - Log in to Zendesk with a user ID that has administrative privileges.

    - Install the Watson Assistant Zendesk app to your Zendesk Chat subdomain as a new private app. 
    
      - From the Chat dashboard navigation pane, expand *Settings*, and then click *Account*
      - Open the *Apps* tab.
      - Click *Upload private app*, and then browse for the application file that you downloaded earlier.    
      - When credentials are requested, paste the Watson Assistant Zendesk app credentials that you copied earlier.

      ![Screen capture of the Zendesk Account page where you can upload a private app.](images/zd-upload-app.png)

      For more information, see [Uploading and installing a private app in Zendesk Chat](https://develop.zendesk.com/hc/en-us/articles/360001069347-Uploading-and-installing-a-private-app){: external}.

1.  Optionally, add an agent avatar image. Edit your profile to upload an avatar image. The image file that you upload cannot be larger than 50 x 50 pixels and 100 KB.

1.  Click **Save** to finish setting up the connection to the Zendesk Chat service desk.

When you test the service desk integration, make sure there is at least one agent with `Online` status. Agent status is set to `Invisible` unless it is explicitly changed.

Watch the following 4-minute video to see someone set up a connection to a Zendesk service desk.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Setting up a Zendesk service desk connection" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/hegheiqUqiM" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Adding transfer support to your dialog
{: #deploy-zendesk-dialog-prereq}

Update your dialog to make sure it understands when users request to speak to a person, and can transfer the conversation properly. See [Adding transfer support to your dialog](/docs/assistant?topic=assistant-deploy-web-chat#deploy-web-chat-dialog-prereq).
