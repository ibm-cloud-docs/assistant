---

copyright:
  years: 2015, 2020
lastupdated: "2020-07-29"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:external: target="_blank" .external}
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

# Web chat release notes
{: #release-notes-chat}

The web chat change log lists changes ordered by version number. For more information about the web chat, see [Integrating with your website](/docs/assistant?topic=assistant-deploy-web-chat).

## 2.2.0
{: #2.2.0}

*Release date: 29 July 2020*

- **Introduced the `instance.writeableElements()` method**: The `instance.writeableElements()` method gives you zones in the web chat user interface where you can embed your own content. For example, you can add content to the end of the header where it is displayed even as the chat content changes. Or you can add custom content that is displayed before the welcome node. For more information, see [Instance methods](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#writeableelements){: external}.

- **Gave the *Send* button a new look** 

  Changed the icon from ![Arrow send icon](images/web-chat-send-old.png) to ![Paper airplane send icon](images/web-chat-send.png).

- **Securing your web chat is easier**: You no longer need to specify the `acr` claim when you define the JWT. The Authentication Context Class reference is managed by the web chat automatically.

- Improved the quality of non-English translations.

- Made a few minor bug fixes.

## 2.1.2
{: #2.1.2}

*Release date: 2 July 2020*

- **Fixed a loading issue**: Addressed an issue that prevented the web chat from loading properly for some deployments with short JWT expiration claims.

## 2.1.1
{: #2.1.1}

*Release date: 1 July 2020*

- **Service desk agent initials are displayed**: When the web chat transfers a user to a service desk agent, the agent's avatar is displayed in the chat window to identify messages sent from the service desk agent. If the agent does not have an avatar, the first initial of the agent's given name is displayed instead.

## 2.0
{: #2.0}

*Release date: 16 June 2020*

- **Versioning was added to web chat**: For more information, see [Versioning](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=key-concepts#versioning){: external}.

- **Added bidirectional support**: You can now use the `direction` parameter to choose whether to show text and elements in the web chat in right-to-left or left-to-right order. For more information, see  [Configuration](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-configuration){: external}.

- **Introduced the `instance.destroySession()` method**: The `instance.destroySession()` method removes all cookie and browser storage that is related to the current web chat session. You can use this method as part of your website logout sequence. For more information, see [Instance methods](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#destroySession){: external}.

## 1.5.3
{: #1.5.3}

*Release date: 14 April 2020*

- **The *Font family* field was removed from the configuration page**: The text that is displayed in the chat window uses the fonts: `IBMPlexSans, Arial, Helvetica, sans-serif`. If you want to use a different set of fonts, you can customize the CSS for your web chat implementation. For more information, see [Theming](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#theming).
- When your implementation does not specify a unique user ID, the web chat adds a first party cookie with a generated anonymous ID to use to identify the unique user. The generated cookie now expires after 45 days. For more information, see [GDPR and cookie policies](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=key-concepts#datapolicy){: external}.

## 1.5.2 
{: #1.5.2}

*Release date: *2 April 2020*

- **Introduced the `learningOptOut` parameter** You can add the `learningOptOut` parameter and set it to `true` to add the `X-Watson-Learning-Opt-Out` header to each `/message` request that originates from the Web Chat. For more information about the header, see [Data collection](https://cloud.ibm.com/apidocs/assistant/assistant-v2#data-collection){: external}. For more information about the parameter, see [Configuration](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-configuration){: external}.

## 1.4.0
{: #1.4.0}

*Release date: 20 March 2020*

- **Customize the CSS theme**: For more information, see [Theming](https://integrations.us-south.assistant.watson.cloud.ibm.com/web/developer-documentation/api-instance-methods#theming){: new_window}.
  
- **Shadow DOM is no longer used**: When you use custom response types or HTML in your dialog, you can apply CSS styles that are defined in your web page to the assistant's response. To override any default styling in the web chat, you must specify the `!important` modifier in your CSS. For more information, see [Rendering response types](https://integrations.us-south.assistant.watson.cloud.ibm.com/web/developer-documentation/api-render#html){: new_window}.
