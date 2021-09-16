---

copyright:
  years: 2019, 2021
lastupdated: "2021-09-16"

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

# Applying advanced customizations
{: #web-chat-config}

Tailor the web chat to match your website and brand, and to behave in ways that meet your business needs.
{: shortdesc}

## API overview
{: #web-chat-config-api}

The following APIs are available:

- **Configuration object**: When the embedded web chat widget starts, a configuration object named `watsonAssistantChatOptions` is used to define the widget. By editing the configuration object, you can customize the appearance and behavior of the web chat before it is rendered.
- **Runtime methods**: Use the instance methods to perform tasks before a conversation between the assistant and your customer begins or after it ends.
- **Events**: Your website can listen for these events, and then take custom actions.

A developer can use these APIs to customize the web chat in the following ways:

- [Change how the web chat opens and closes](#web-chat-config-open-close)
- [Change the conversation](#web-chat-config-convo)
- [Change the look of the web chat](#web-chat-config-look)

## Change how the web chat opens and closes
{: #web-chat-config-open-close}

You can change the color of the launcher icon from the *Style* tab of the web chat configuration page. If you want to make more advanced customizations, you can make the following types of changes:

- Change the launcher icon that is used to open the web chat widget. For a tutorial the shows you how, see [Using a custom launcher](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=tutorials-launcher){: external}.
- Change how the web chat widget opens. For example, you might want to launch the web chat from some other button or process that exists on your website, or maybe open it in a different location, or at a different size. For a tutorial that shows you how, see [Render to a custom element](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=tutorials-example-element){: external}.
- Hide the launcher icon entirely and automatically start the web widget in open state, at its full length. For more information, see the [`openChatByDefault` method](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-configuration#configurationobject){: external}.
- Hide the close button so users cannnot close the web chat widget. For more information, see the [`hideCloseButton` method](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-configuration#configurationobject){: external}.

## Change the conversation
{: #web-chat-config-convo}

The core of the conversation is defined in your skill. If you use more than one integration type, focus on defining an engaging conversation in the underlying skill. The same skill is used for all integrations. To customize the conversation for the web chat only, you can take the following actions:

- Update the text of a message before it is sent or after it is received, such as to hide personally-identifiable information. 
- Pass contextual information, such as the customer's name, from the web chat to the underlying skill. For examples of how to complete common tasks, see [Passing values](#web-chat-config-context).
- Change the language that is used by the web chat. For more information, see [Global audience support](/docs/assistant?topic=assistant-web-chat-basics#web-chat-basics-global).
- Render your own custom response types inside the web chat widget, including responses that incorporate code from your website at run time. For a tutorial that shows you how, see [Creating a custom response](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=tutorials-user-defined-response){: external}.
- Use React portals to render your custom response type as part of your application. For a tutorial that shows you how, see [Custom responses with React](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=tutorials-react-portals){: external}.

## Change the look of the web chat
{: #web-chat-config-look}

You can make simple changes to the color of things like the text font and web chat header from the *Style* tab of the web chat configuration page. To make more extensive style changes, you can set the CSS style color theme to a different theme or specify your own theme.

- You can choose to use a different base Carbon Design theme. The supported base themes are color themes that are defined by [IBM Carbon Design](https://www.carbondesignsystem.com/guidelines/color/usage/){: external}. For more information, see [Theming](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#theming){: external}.
- Alternatively, you can set individual variables within the theme to customize specific UI elements. For example, the text that is displayed in the chat window uses the fonts `IBMPlexSans, Arial, Helvetica, sans-serif`. If you want to use a different font, you can specify it by using the `instance.updateCSSVariables()` method.
- Apply style settings to user-defined response types. If you enable the web chat to return custom response types, be sure to apply existing or custom CSS classes to them. For more information, see [Custom content](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-render){: external}.
- Change the style of the home screen that can be displayed when the web chat is opened. For more information about the home screen, see [Adding a home screen](/docs/assistant?topic=assistant-deploy-web-chat#deploy-web-chat-home-screen). For more information about how to customize it, see [HTML content](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-render#html){: external}
  
The web chat is embedded directly on your page, not inside an iframe. Therefore, the cascading style sheet (CSS) rules for your website can sometimes override the web chat CSS rules. The web chat applies aggressive CSS resets, but the resets can be affected if your website uses the `!important` property in elements where style is defined.
{: note}

### Passing values
{: #web-chat-config-context}

Here are some common tasks you might want to perform:

- [Setting and passing context variable values](#web-chat-config-set-context})
- [Adding user identity information (if you don't enable security)](#web-chat-config-userid)

For a tutorial that describes how to set context values from the web chat, see [Setting context](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=tutorials-setting-context){: external}.

#### Setting and passing context variable values
{: #web-chat-config-set-context}

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
    t.src='https://web-chat.global.assistant.dev.watson.appdomain.cloud/versions/' +
      (window.watsonAssistantChatOptions.clientVersion || 'latest') +
      '/WatsonAssistantChatEntry.js';
    document.head.appendChild(t);});

</script>
```
{: codeblock}

You can reference the `$ismember` context variable from your dialog. For example, the following screen capture shows a dialog node that conditions on #General_Greetings. It has multiple conditioned responses. The first response checks whether the current user is a member of your rewards program by checking for the presence of the `$ismember` context variable. If the variable is present, the response addresses the user as a member. The next response has a more generic greeting.

![Shows multiple conditioned responses in a dialog node, one of which references the ismember context variable](images/web-chat-use-context-var.png)

If you enable security, you can encrypt the data that you pass to your dialog. For more information, see [Passing sensitive data](/docs/assistant?topic=assistant-web-chat-security#web-chat-security-encrypt).

If you're using a Lite plan, remember that a session ends if there's no interaction with the user after 5 minutes. When the session ends, any contextual information that you pass or collect is reset. 
{: important} 

#### Adding user identity information
{: #web-chat-config-userid}

If you do not enable security, and you want to perform tasks where you need to know the user who submitted the input, then you must pass the user ID to the web chat integration.

If you do enable security, you set the user ID in the JSON Web Token instead. For more information, see [Authenticating users](/docs/assistant?topic=assistant-web-chat-security#web-chat-security-authenticate).

Choose a non-human-identifiable ID. For example, do not use a person's email address as the `user_id`.

User information is used in the following ways:

- User-based service plans use the `user_id` associated with user input for billing purposes. See [User-based plans](/docs/assistant?topic=assistant-services-information#services-information-user-based-plans). 
- The ability to delete any data created by someone who requests to be forgotten requires that a `customer_id` be associated with the user input. When a `user_id` is defined, the product can reuse it to pass a `customer_id` parameter. See [Labeling and deleting data](/docs/assistant?topic=assistant-information-security#information-security-gdpr-wa).

  Because the `user_id` value that you submit is included in the `customer_id` value that is added to the `X-Watson-Metadata` header in each message request, the `user_id` syntax must meet the requirements for header fields as defined in [RFC 7230](https://tools.ietf.org/html/rfc7230#section-3.2).
{: note} 

To support these user-based capabilities, add the [`updateUserID()` method](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#updateuserid){: external} in the code snippet before you paste it into your web page.

In the following example, the user ID `L12345` is added to the script.

```html
<script>
  window.watsonAssistantChatOptions = {
      integrationID: 'YOUR_INTEGRATION_ID',
      region: 'YOUR_REGION', 
      serviceInstanceID: 'YOUR_SERVICE_INSTANCE',
      onLoad: function(instance) { 
        instance.updateUserID('L12345');
        instance.render(); 
        }
    };
  setTimeout(function(){
    const t=document.createElement('script');
    t.src='https://web-chat.global.assistant.dev.watson.appdomain.cloud/versions/' +
      (window.watsonAssistantChatOptions.clientVersion || 'latest') +
      '/WatsonAssistantChatEntry.js';
    document.head.appendChild(t);
  });
</script>
```
{: codeblock} 
