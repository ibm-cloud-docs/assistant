---

copyright:
  years: 2019, 2020
lastupdated: "2020-07-21"

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

# Integrating web chat with your website
{: #deploy-web-chat}

Add your assistant to your company website as a web chat widget that can help your customers with common questions and tasks, and can transfer customers to human agents.
{: shortdesc}

When you create a web chat integration, code is generated that calls a script that is written in JavaScript. The script instantiates a unique instance of your assistant. You can then copy and paste the HTML `script` element into any page or pages on your website where you want users to be able to ask your assistant for help.

![Plus or Premium plan only](images/plus.png) This integration type is available to Plus or Premium plan users only.

To learn more about web chat, watch the following 3-minute video.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Web chat overview" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/52bpMKVigGU" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Create a web chat instance to add to your website
{: #deploy-web-chat-task}

To add the assistant to a web page on your company website, complete the following steps:

1.  From the Assistants page, click to open the assistant tile that you want to deploy to your site.

1.  From the *Integrations* section, click the **Web chat** tile.

1.  **Optional**: Change the web chat integration name from *Web chat* to something more descriptive.

1.  Click **Create** to generate the script.

    A code snippet is created and added to the page that contains an HTML `script` element. The `script` tag calls JavaScript code that is hosted on an IBM site. The code creates an instance of a widget that communicates with the assistant. The generated code includes a region and unique integration ID. Do not change these parameter values.

1.  **Optional**: Customize the chat. You can make the following changes:

    - **Public assistant name**. Name by which the assistant is known to users. This name is displayed in the header of the chat window. The name can be up to 18 characters in length. 

    - **Primary color**. Sets the color of the web chat header.

      Click the white dot to open a color switcher where you can choose a color. The color is saved as an HTML color code, such as `#FF33FC` for pink and `#329A1D` for green. Alternatively, you can add an HTML color code directly to the field to set the color.

    - **Secondary color**: Sets the color of the user input message bubble.

    - **Accent color**. Sets the color of interactive elements, including:

      - Chat launcher button that is embedded in your web page
      - Send button associated with the input text field
      - Input text field border when in focus
      - Marker that shows the start of the assistant’s response
      - Border of a button after it is clicked
      - Typing indicator that is shown to repesent a pause response
      - Active state for dropdown color of border

    - **Assistant image**: This image is displayed in the web chat header along with the assistant name to represent your assistant or organization. Specify the URL for a publicly accessible hosted image, such as a company or brand logo or an assistant avatar.
    
      The image file must be between 64 x 64 and 100 x 100 pixels in size. 

    Style changes you make are immediately applied to the preview that is shown on the page, so you can see how your choices impact the style of the chat UI.

1.  Copy the `script` HTML element.

    Do not click *Save changes* until you are done with making web chat edits; it closes the page.
    {: tip}

1.  Open the HTML source for a web page on your website where you want the chat window to be displayed. Paste the code snippet into the page.

    Paste the code as close to the closing `</body>` tag as possible to ensure that your page renders faster.
    {: tip}

    The following HTML snippet is the source for a test page that you can copy and save as a file with a .html extension for testing purposes. You would replace the script element block here with the script elements you copied from the web chat integration setup page.

    ```html
    <html>
    <head></head>
    <body>
        <title>My Test Page</title>
        <p>The body of my page.</p>
        <!-- copied script elements -->
        </body>
    </html>
    ```
    {: codeblock}

1.  Refresh the web page.

    For information about the web browsers that are supported by the web chat, see [Browser Support](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=key-concepts#browsersupport){: external}.

    ![Chat icon](images/web-chat-icon.png)
    
    The web chat launcher icon is displayed at the end of the page. The icon is blue unless you customize the accent color.

    The placement of the web chat icon is always the same regardless of where you paste the script element into the web page source. The chat window is represented by a `div` HTML element.
    {: important}

1.  Click the icon to open the chat window and talk to your assistant.

    ![Web chat window](images/web-chat-window.png)

1.  Paste the code snippet into each web page where you want the assistant to be available to your customers.

    You can paste the same script tag into as many pages on your website as you want. Add it anywhere where you want users to be able to reach your assistant for help. However, be sure to add it only one time per page.
    {: tip}

1.  Submit test utterances from the chat widget that is displayed on your web page to see how the assistant responds.

    No responses are returned until after you create a dialog skill and add it to the assistant.
    {: note}

    If you don't extend the session timeout setting for the assistant, the dialog flow for the current session is restarted after 60 minutes of inactivity. This means that if a user stops interacting with the assistant, after 60 minutes, any context variable values that were set during the previous conversation are set to null or back to their initial values.

1.  Click **Save changes** to save the web chat name and any customization information that you added and close the integration page. Alternatively, you can click the **X** to close the page. 

    The web chat instance is created as soon as you click the *Create* button, and does not need to be saved.

You can apply more advanced customizations to the style of the web chat by using the {{site.data.keyword.conversationshort}} web chat toolkit on [GitHub](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-configuration){: external}. For example, the text that is displayed in the chat window uses the fonts: `IBMPlexSans, Arial, Helvetica, sans-serif`. If you want to use a different font, you can specify it by using the `instance.updateCSSVariables()` method.

## Dialog considerations
{: #deploy-web-chat-dialog}

The rich responses that you add to a dialog are displayed in the web chat as expected, with the following exceptions:

- **Connect to human agent**: This response type is ignored.
- **Option**: If your option list contains up to four choices, they are displayed as buttons. If your list contains five or more options, then they are displayed in a drop-down list.
- **Pause**: This response type pauses the assistant's activity in the chat. However, activity does not resume after the pause until another response is triggered. Whenever you include a `pause` response type, add another, different response type, such as `text`, after it.

For more information about rich response types, see [Rich responses](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia).

## Extending the web chat
{: #deploy-web-chat-extend}

A developer can extend the capabilities of the web chat by using the {{site.data.keyword.conversationshort}} web chat toolkit on [GitHub](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html){: external}.

If you choose to use the provided methods, you implement them by editing the code snippet that was generated earlier. You then embed the updated code snippet into your web page.

### Setting and passing context variable values
{: #deploy-web-chat-set-context}

A context variable is a variable that you can use to pass information to your assistant before a conversation starts. It can also collect information during a conversation, and reference it later in the same conversation. For example, you might want to ask for the customer's name and then address the person by name later on.

The following script preserves the context of the conversation. In addition, it adds an `$ismember` context variable and sets it to `true`.

The name that is specified for the skill (`main skill`) is a hardcoded name that is used to refer to any skill that you create from the product user interface. You do not need to edit your skill name.

```html
<script>
  function preSendhandler(event) {
    event.data.context.skills['main skill'].user_defined.ismember = true;    
  }
  window.watsonAssistantChatOptions = {
    integrationID: "YOUR_INTEGRATION_ID",
    region: "YOUR_REGION",
    serviceInstanceID: "YOUR_SERVICE_INSTANCE_ID",

    onLoad: function(instance) {
      // Subscribe to the "pre:send" event.
      instance.on({ type: "pre:send", handler: preSendhandler });
      instance.render();
    }
  };

  setTimeout(function(){
    const t=document.createElement('script');
    t.src='https://web-chat.global.assistant.watson.appdomain.cloud/loadWatsonAssistantChat.js';
    document.head.appendChild(t);});

</script>
```
{: codeblock}

You can reference the `$ismember` context variable from your dialog. For example, the following screen capture shows a dialog node that conditions on #General_Greetings. It has multiple conditioned responses. The first response checks whether the current user is a member of your Rewards Program by checking for the presence of the `$ismember` context variable. If the variable is present, the response addresses the user as a member. The next response has a more generic greeting.

![Shows multiple conditioned responses in a dialog node, one of which references the ismember context variable](images/web-chat-use-context-var.png)

If you enable security, you can encrypt the data that you pass to your dialog. For more information, see [Passing sensitive data](#deploy-web-chat-security-encrypt).

### Adding user identity information
{: #deploy-web-chat-userid}

If you do not enable security, and you want to perform tasks where you need to know the user who submitted the input, then you must pass the user ID to the web chat integration.

If you do enable security, you set the user ID in the JSON Web Token instead. For more information, see [Authenticating users](#deploy-web-chat-security-authenticate).

Choose a non-human-identifiable ID. For example, do not use a person's email address as the `user_id`.

User information is used in the following ways:

- User-based service plans use the `user_id` associated with user input for billing purposes. See [User-based plans](/docs/assistant?topic=assistant-services-information#services-information-user-based-plans). 
- The ability to delete any data created by someone who requests to be forgotten requires that a `customer_id` be associated with the user input. When a `user_id` is defined, the product can reuse it to pass a `customer_id` parameter. See [Labeling and deleting data](/docs/assistant?topic=assistant-information-security#information-security-gdpr-wa).

Because the `user_id` value that you submit is included in the `customer_id` value that is added to the `X-Watson-Metadata` header in each message request, the `user_id` syntax must meet the requirements for header fields as defined in [RFC 7230](https://tools.ietf.org/html/rfc7230#section-3.2).
{: note} 

To support these user-based capabilities, add the `updateUserID()` method in the code snippet before you paste it into your web page.

In the following example, the user ID `L12345` is added to the script.

```html
<script>
  window.watsonAssistantChatOptions = {
      integrationID: 'YOUR_INTEGRATION_ID',
      region: 'YOUR_REGION', 
      serviceInstanceID: 'YOUR_SERVICE_INSTANCE',
      onLoad: function(instance) { 
        instance.updateUserID(L12345);
        instance.render(); 
        }
    };
  setTimeout(function(){
    const t=document.createElement('script');
    t.src="https://web-chat.global.assistant.watson.appdomain.cloud/loadWatsonAssistantChat.js";
    document.head.appendChild(t);
  });
</script>
```
{: codeblock} 

## Securing the web chat
{: #deploy-web-chat-security}

Configure the web chat to authenticate users and send private data from your embedded web chat.

All messages that are sent from the web chat are encrypted. When you enable security, your assistant takes an additional step to verify that messages originate from the web chat that is embedded in your website only.

The web chat uses an RSA signature with SHA-256 to encrypt communication. RS256 cryptography is a sophisticated type of RSA encryption. An RSA key pair includes a private and a public key. The RSA private key is used to generate digital signatures, and the RSA public key is used to verify digital signatures. The complexity of the RSA algorithm that is used to scramble the message makes it nearly impossible to unscramble the message without the key.

You can implement the following security measures:

- Ensure that messages sent from the web chat to your assistant come from your customers only
- Send private data from the web chat to your assistant

### Enable security
{: #deploy-web-chat-security-task}

The process you use to add the web chat to your website is simple. Its simplicity also means it can be misused. That's why it's important to verify that the messages sent to your assistant are coming from authorized users only.

After you enable security, users cannot submit messages through the web chat unless you take steps to prove their origin. Do not enable it until you have support for authentication in place.
{: important}

Before you enable security, complete the following steps:

1.  {: #deploy-web-chat-security-origin}Create a RS256 private/public key pair.

    You can use a tool such as the OpenSSL command line or PuTTYgen.

    - For example, to create the key pair: `openssl genrsa -out key.pem 2048`
    
1.  Use your private key to sign a JSON Web Token (JWT). You will pass the token with the messages that are sent from your website as proof of their origin.

    The JWT payload must specify values for the following claims:

    - `iss`: Represents the issuer of the JWT. This value is a case-sensitive string.
    - `sub`: Represents the principal that is the subject of the JWT. This value must either be scoped to be locally unique in the context of the issuer or be globally unique. The value you specify for `sub` is used as the `user_id`. The syntax of the value must meet the requirements for header fields as defined in [RFC 7230](https://tools.ietf.org/html/rfc7230#section-3.2){: external}.
    - `exp`: Represents the expiration time on or after which the JWT cannot be accepted for processing. Many libraries set this value for you automatically. Set a short-lived `exp` claim with whatever library you use.

    For more information about JSON Web Tokens, see the [RFC7519](https://tools.ietf.org/html/rfc7519){: external} and [OpenID Connect 1.0](https://openid.net/specs/openid-connect-core-1_0.html){: external} specifications.
    
    Most programming languages offer JWT libraries that you can use to generate a token. The following NodeJS code sample illustrates how to generate a JWT token.

    ```javascript
    // Sample NodeJS code on your server.
    const jwt = require('jsonwebtoken');
    
    /**
     * Returns a signed JWT generated by RS256 algorithm.
     */
    function mockLogin() {
        const payload = {
            /*
             * Even if this is an unauthenticated user, add a userID in the sub claim that can be used 
             * for billing purposes.
             * This ID will help us keep track "unique users". For unauthenticated users, drop a
             * cookie in the browser so you can make sure the user is counted uniquely across visits.
             */
            sub: 'some-user-id', // Required
            iss: 'yourdomain.com' // Required
        };
        // The "expiresIn" option adds an "exp" claim to the payload.
        return jwt.sign(payload, process.env.YOUR_PRIVATE_RSA_KEY, { algorithm: 'RS256', expiresIn: '10000ms' });
    }
    ```
    {: codeblock}

To enable security, complete the following steps:

1.  From the web chat integration page in {{site.data.keyword.conversationshort}}, switch the **Secure your web chat** toggle to **On**.

1.  Add your public key to the **Your public key** field.
    
    The public key that you add is used to verify that data which claims to come from your web chat instance *is* coming from your web chat instance. 
    
1.  To prove that a message is coming from your website, each message that is submitted from your web chat implementation must include the JSON Web Token (JWT) that you created earlier.

    Add the token to the web chat code snippet that you embed in your website page. Specify the token in the `identityToken` property.

    For example:

    ```html
    <script>
      window.watsonAssistantChatOptions = {
          integrationID: 'YOUR_INTEGRATION_ID',
          region: 'YOUR_REGION', 
          serviceInstanceID: 'YOUR_SERVICE_INSTANCE',
          identityToken: 'YOUR_JWT',
          onLoad: function(instance) {
            instance.render(); 
            }
        };
      setTimeout(function(){
        const t=document.createElement('script');
        t.src="https://web-chat.global.assistant.watson.appdomain.cloud/loadWatsonAssistantChat.js";
        document.head.appendChild(t);
      });
    </script>
    ```
    {: codeblock}
    
    The JSON Web Token is automatically included on each subsequent request that is sent from the web chat until it expires.

1.  You can add an event that is triggered when your token expires. The event has a callback you can use to update the token and process any messages that were added to a queue to wait to be processed while the token was expired.

    For example:

    ```html
    <script>
    window.watsonAssistantChatOptions = {
      integrationID: 'YOUR_INTEGRATION_ID',
      region: 'YOUR_REGION',
      serviceInstanceID: 'YOUR_SERVICE_INSTANCE',
      identityToken: 'YOUR_JWT',
      onLoad: function(instance) {
        instance.on({ type: 'identityTokenExpired', handler: function(event) {
          // Perform whatever actions you need to take on your system to get a new token.
          return new Promise(function(resolve, reject) {
            // And then pass the new JWT into the callback and the service will resume processing messages.
            event.identityToken = 'YOUR NEW JWT';
            resolve();
          });
        }});
        instance.render();
      }
    };
    setTimeout(function(){
        const t=document.createElement('script');
        t.src="https://web-chat.global.assistant.watson.appdomain.cloud/loadWatsonAssistantChat.js";
        document.head.appendChild(t);
      });
    </script>
    ```
    {: codeblock}

### Passing sensitive data
{: #deploy-web-chat-security-encrypt}

You can optionally copy the public key that is provided by IBM, and use it to add an additional level of encryption to support passing sensitive data from the web chat.

Use this method to send sensitive information in messages that come from your website, such as a information about a customer's loyalty level, a user ID, or security tokens to use in webhooks that you call from your dialog. Information that is passed to your assistant in this way is stored in a private variable in your assistant. Private variables cannot be seen by customers and are never sent back to the web chat.

For example, you might start a business process for a VIP customer that is different from the process you start for less important customers. You likely do not want non-VIPs to know that they are categorized as such. But you must pass this informataion to your dialog because it changes the route of the conversation. You can pass the customer MVP status as an encrypted variable. This private context variable will be available for use by the dialog, but not by anything else.

1.  From the web chat configuration page, copy the public key from the **IBM provided public key** field.
1.  From your website, write a function that signs a JSON Web Token.

    For example, the following NodeJS code snippet shows a function that accepts a userID and payload content and sends it to the web chat. If a payload is provided, its content is encrypted and signed with the IBM public key.

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
          iss: 'www.ibm.com' // Required
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

1.   The encrypted user payload is decrypted and then saved to the `context.integrations.chat.private.user_payload` object. For information about how to access the payload data from the dialog, see [Web chat: Accessing sensitive data](/docs/assistant?topic=assistant-dialog-integrations#dialog-integrations-chat-private). You might want to access the payload, for example, to get the customer importance information or single sign-on credentials that you can subsequently use to authenticate a webhook.

### Authenticating users
{: #deploy-web-chat-security-authenticate}

To authenticate and specify a unique ID for each customer, add the user ID information to the token.

1.  From the web chat configuration page, copy the public key from the **IBM provided public key** field. You will specify this value as the `PUBLIC_IBM_RSA_KEY` later.
1.  From your website, write a function that signs a JSON Web Token.

    The function must accept a UserID parameter and set the userID as the `sub` claim value.

    For example, the following NodeJS code snippet set the user's ID to `L12345`.

    ```javascript
    // Sample NodeJS code on your server.
    const jwt = require('jsonwebtoken');
    const RSA = require('node-rsa');

    const rsaKey = new RSA(process.env.PUBLIC_IBM_RSA_KEY);

    /**
    * Returns a signed JWT. Optionally, adds an encrypted user payload in stringified JSON.
    */
    function mockLogin() {
      const payload = {
        sub: 'L12345', // Required
        iss: 'www.example.com' // Required
      };
      const token = jwt.sign(payload, process.env.YOUR_PRIVATE_RSA_KEY, { algorithm: 'RS256', expiresIn: '10000ms' });
    return token;
    }
    ```
    {: codeblock}

    After you set the value of the `sub` claim to be the user's userID, you cannot change the claim to another user. The userID that you specify with this method is used for billing purposes and can be used to delete customer data upon request.

If you disable security, then you can use the `instance.updateUserID()` method to specify user IDs. For more information, see [Adding user identity information](#deploy-web-chat-userid).

## Adding support for transfers
{: #deploy-web-chat-haa}

Delight your customers with 360-degree support by integrating your web chat with a third-party service desk solution. 

The following service desk offerings are supported:

1.   {: #deploy-web-chat-zendesk}[Zendesk](/docs/assistant?topic=assistant-deploy-zendesk)
1.   {: #deploy-web-chat-salesforce}[Salesforce](/docs/assistant?topic=assistant-deploy-salesforce)

After you set up the service desk integration, you must update your dialog to ensure it understands user requests to speak to someone, and can transfer the conversation properly.

## Adding transfer support to your dialog
{: #deploy-web-chat-dialog-prereq}

If no dialog skill is associated with your assistant, create one or add one to your assistant now. See [Building a dialog](/docs/assistant?topic=assistant-dialog-overview) for more details.

Complete these steps in your dialog skill so the assistant can pass the conversation to a service desk agent when a customer asks to speak to a person.

1.  Add an intent to your skill that can recognize a user's request to speak to a human.

    You can create your own intent or add the prebuilt intent named `#General_Connect_to_Agent` that is provided with the **General** content catalog.

1.  Add a root node to your dialog that conditions on the intent that you created in the previous step. Choose **Connect to human agent** as the response type.

1.  To ensure that a useful summary is provided to service desk agents when a conversation is transferred to them, fill in the **external node name** field of the root node of each dialog branch.

      ![Screen capture of the field in the node edit view where you add the node purpose summary.](images/disambig-node-purpose.png)

      Every dialog branch can be processed by the assistant while it chats with a customer, including branches with root nodes in folders. Add a summary of the purpose of the dialog branch to the root node of each branch. For example, *Find a store*.

1.  If a child node in any dialog branch conditions on a follow-up request or question that you do not want the assistant to handle, add a **Connect to human agent** response type to the node.

    For example, you might want to add this response type to nodes that cover sensitive issues that only a human should handle or that track when an assistant repeatedly fails to understand a user.

    At run time, if the conversation reaches this child node, the dialog is passed to a human agent at that point.

Your dialog is now ready to support transfers from your assistant to service desk agents.
