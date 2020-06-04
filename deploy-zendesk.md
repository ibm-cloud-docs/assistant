---

copyright:
  years: 2020
lastupdated: "2020-06-04"

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

# Integrating with Zendesk
{: #deploy-zendesk}

Integrate your Web Chat with a Zendesk service desk solution so your customers always get the help they need.
{: shortdesc}

Integrate with a Zendesk service desk by deploying your assistant with the Web Chat integration. The Web Chat serves as the client interface for your assistant. If, in the course of a conversation with your assistant, a customer asks to speak to a person, you can transfer the conversation directly to a Zendesk agent.

![Plus or Premium plan only](images/plus.png) This integration type is available to Plus or Premium plan users only.

Zendesk Chat lets you help customers in real time, which increases customer satisfaction. And satisfied customers are happier customers. To learn more about this service desk solution, see the [Zendesk website](https://www.zendesk.com/chat/){: external}.

Zendesk Chat is an add-on to Zendesk Support. Zendesk Support puts all your customer support interactions in one place, so communication is seamless, personal, and efficient, which means more productive agents and satisfied customers.

## Before you begin
{: #deploy-zendesk-prereqs}

1.  You must have a Zendesk account. If not, create one. 

    A Zendesk Chat Enterprise plan is required.
    {: important}

1.  Decide whether you want to enable security.

    If you choose to enable security in Zendesk, you must collect the name and email address of each user. This information must be passed to the Web Chat so it can be provided to Zendesk when the conversation is transferred.

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

      On Safari, the application files are extracted from the ZIP file into a folder. To keep the file archived as a .zip file, so you can upload it later, edit the Safari preferences. Clear the *Open safe files after downloading* checkbox.
      {: note}

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

Update your dialog to make sure it understands when users request to speak to a person, and can transfer the conversation properly. For more information, see [Adding transfer support to your dialog](/docs/assistant?topic=assistant-deploy-web-chat#deploy-web-chat-dialog-prereq).

## Securing the transfer to Zendesk
{: #deploy-zendesk-secure}

Before you can secure the Zendesk connection, complete the following required tasks:

1.  {: #deploy-zendesk-secure-prereqs}Secure the Web Chat. For more information see [Enable security](/docs/assistant?topic=assistant-deploy-web-chat#deploy-web-chat-security-task).
1.  Encrypt sensitive information that you pass to the Web Chat. 

    When you enable security in Zendesk, you must provide the name and email address of the current user with each request. Configure the Web Chat to pass this information in the payload.

    Specify the information by using the following syntax. Use the exact names (`name` and `email`) for the two name and value pairs.

    ```yaml
    {
    user_payload : {  
             name: '#{customerName}',
             email: '#{customerEmail}'
      }
    }     
    ```
    {: codeblock}

    For more information, see [Passing sensitive data](/docs/assistant?topic=assistant-deploy-web-chat#deploy-web-chat-security-encrypt).

    Zendesk also expects `iat` and `external_id` name and value pairs. However, there's no need for you to provide this information. IBM automatically provides a JWT that contains these values.

    For example:

    ```json
    const userPayload = {
     "name" : "Cade Jones",
     "email" : "cade@example.com",
    }
    ```
    {: codeblock}

    ```javascript
    // Sample NodeJS code on your server.
    const jwt = require('jsonwebtoken');
    const RSA = require('node-rsa');

    const rsaKey = new RSA(process.env.PUBLIC_IBM_RSA_KEY);

    /**
    * Returns a signed JWT. Optionally, adds an encrypted user_payload in stringified JSON.
    */
    function mockLogin(userID, userPayload) {
        const payload = {
          sub: userID, // Required
          iss: 'www.ibm.com', // Required
          acr: 'loa1' // Required
          // A short-lived exp claim is automatically added by the jsonwebtoken library.
        };
        if (userPayload) {
            // If there is a user payload, it is encrypted in base64 format using the IBM public key.
            payload.user_payload = rsaKey.encrypt(userPayload, 'base64');
        }
        const token = jwt.sign(payload, process.env.YOUR_PRIVATE_RSA_KEY, { algorithm: 'RS256', expiresIn: '10000ms' });
        return token;
        }
    ```
    {: codeblock}

1.  From the Zendesk application, enable visitor authentication.

    - From the Chat dashboard navigation pane, expand *Settings*, and then click *Widget*.
    - Open the *Widget security* tab.
    - In the *Visitor Authentication* section, click the *Generate* button.

    For more information, see [Enabling authenticated visitors in the Chat widget](https://support.zendesk.com/hc/en-us/articles/360022185314-Enabling-authenticated-visitors-in-the-Chat-widget){: external}. You do not need to follow the steps to create a JWT. The Assistant service generates a JSON Web Token for you.
1.  Copy the shared secret from Zendesk.

To secure the Zendesk connection, complete the following steps:

1.  {: #deploy-zendesk-secure-task}In the *Authenticate users* section, set the toggle to **On**.

1.  Paste the secret that you copied from the Zendesk setup page into the **Zendesk shared secret** field.

1.  {: #deploy-zendesk-secure-anonymous}Decide whether to allow unidentified users to access Zendesk.

    The Web Chat allows anonymous users to initiate chats. However, as soon as you enable visitor authentication, Zendesk requires that the name and email of each user be provided. If you try to connect without passing the required information, the connection will be refused. 

    If you want to allow anonymous users to connect to Zendesk, you can provide fictitious name and email data. Write a function to populate the two fields with fictitious name and email values.

    For example, your function must check whether you know the name and email of the current user, and if not, add canned values for them:

    ```json
    const userPayload = {
     "name" : "Jane Doe1",
     "email" : "jdoe1@example.com",
    }
    ```
    {: codeblock}

    After writing a function that ensures that name and email values are always provided, set the *Authenticate anonymous user chat transfers* toggle to **On**.

If you haven't yet, update your dialog to make sure it understands when users request to speak to a person, and can transfer the conversation properly. For more information, see [Adding transfer support to your dialog](/docs/assistant?topic=assistant-deploy-web-chat#deploy-web-chat-dialog-prereq).