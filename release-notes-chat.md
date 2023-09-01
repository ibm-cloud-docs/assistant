---

copyright:
  years: 2015, 2023
lastupdated: "2023-09-01"

subcollection: assistant

---

{{site.data.keyword.attribute-definition-list}}

# Web chat release notes
{: #release-notes-chat}

The release notes for the **classic {{site.data.keyword.assistant_classic_short}}** experience have moved. All release notes for Web chat for {{site.data.keyword.assistant_classic_short}} can be found here: [Web chat release notes](/docs/watson-assistant?topic=watson-assistant-release-notes-chat){: external}.
{: attention}

## 7.4.0
{: #7.4.0}

*Release date: 12 June 2023*

- Added CSS variables for customizing the launcher. For more information, see [`instance.updateCSSVariables`](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#updatecssvariables){: external}.

## 7.3.0
{: #7.3.0}

*Release date: 30 May 2023*

- Released a beta of a contact center integration for Genesys Web Messenger, which currently only includes user information strings in English. For more information, see [`Genesys Web Messenger`](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=service-desks-genesys){: external}. 
- Added beta support for file sharing with custom service desk integrations, which currently only includes user information strings in English. For more information, see [`Custom service desks`](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=service-desks-custom-sd){: external}. 

## 7.2.2
{: #7.2.2}

*Release date: 1 May 2023*

- Bug fixes.

## 7.2.1
{: #7.2.1}

*Release date: 24 April 2023*

- Bug fixes.

## 7.2.0
{: #7.2.0}

*Release date: 10 April 2023*

- Added support for inline iframe responses. For more information, see [Response types reference](/docs/watson-assistant?topic=watson-assistant-response-types-reference#iframe).

- Redesigned the agent conversation experience.

- Bug fixes.

## 7.1.1
{: #7.1.1}

*Release date: 13 February 2023*

- **New journey events**: The new [`tour:start`](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-events#tourstart){: external}, [`tour:end`](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-events#tourend){: external}, and [`tour:step`](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-events#tourstep){: external} events provide details about the user's progress through a journey (also known as a *tour*). These events can be used to navigate to a specific page when the user starts a journey or reaches a certain step, or to show a survey after a journey ends.

- **New journey instance methods**: The new [`tours`](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#tours){: external} object supports instance methods that provide better control over journeys. You can use these methods to start or end a journey, or to automatically navigate through a journey in response to user actions.

- **Added journey strings to the language pack**: New strings for journeys have been added to the language pack. You can modify the strings in the language pack by using the [`updateLanguagePack()`](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#updatelanguagepack){: external} instance method. For more information about language packs, see [Languages](/docs/watson-assistant?topic=watson-assistant-topicid){: external}.

For more information about the journeys beta feature, see [Guiding customers with journeys](/docs/watson-assistant?topic=watson-assistant-journeys).

## 7.1.0
{: #7.1.0}

*Release date: 17 January 2023*

- **Updated Zendesk agent app**: The agent app for Zendesk has been updated for compatibility with Zendesk workspaces.

- In the service desk starter kits, the instance of the web chat integration has been added to the [`serviceDeskFactory` parameters](https://github.com/watson-developer-cloud/assistant-web-chat-service-desk-starter/blob/main/src/common/types/serviceDesk.ts){: external} to make it accessible to custom service desk implementations.

- **New instance methods**: The new [`elements.getMessageInput()`](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#elements-get-message-input){: external} and [`elements.getHomeScreenInput()`](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#elements-get-home-screen-input){: external} instance methods enable access to the input fields used by the customer to send messages. You can use these methods to change the input or to take action as the user is typing (for example, to implement for a type-ahead feature).

- **New event**: The new [`agent:pre:sessionHistory`](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-events#agentpresessionhistory){: external} event enables you to filter potential PII from messages sent from a customer or service desk agent before the messages are sent to the assistant for storage in the session history.

- **New property in web chat state object**: In the object returned from the [`getState()`](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#getState){: external} instance method, the new `isDebugEnabled` property indicates whether the web chat debug flag is set to `true`.

## 7.0.0
{: #7.0.0}

*Release date: 5 December 2022*

- **Streamlined live agent handoff**: The live agent handoff experience has been streamlined and simplified. Instead of opening the live agent chat in a separate window, the web chat now shows the live agent entering the conversation in the same window.

    Because of this change, the [`updateCustomMenuOptions`](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#updatecustommenuoptions){: external} instance method has changed to reflect the fact that there is now only a single view, with a single list of custom menu options. If you want to customize menu options only during a live agent chat, you can subscribe to the [`agent:pre:startChat`](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-events#agentprestartchat){: external} and [`agent:endChat`](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-events#agentendchat){: external} events to trigger your customizations.

- **`agent:endChat` changes**: The [`agent:endChat`](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-events#agentendchat){: external} event now also fires if the customer cancels a live agent request before the agent has joined. If you want to show a post-chat form only after a live agent chat, you can use the new `requestCancelled` flag on the event to determine whether the request was canceled.

- **New configuration options**: The following new options are available in the configuration object:
    - `serviceDesk.availabilityTimeoutSeconds`: Specifies how long the web chat waits for an available agent before automatically canceling the live agent request.
    - `serviceDesk.disableAgentSessionHistory`: Disables storage of live agent chats in the session history. If this option is set to `true`, live agent chat history is not stored; this means that if the web chat is reloaded, the live agent chat history is lost.

    For more information, see [Service desk options](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-configuration#servicedeskoptions){: external}.

- **`elements` instance property**: A new [`elements`](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-configuration#servicedeskoptions){: external} instance property provides methods you can use to apply CSS styles to individual elements used by the web chat. (Currently, only the main window is supported.)

- **`skip_card` option for journeys**: The journeys beta feature has been updated to support a new `skip_card` property. You can use this property to start a journey immediately without waiting for the customer to click the introductory card, or even to start a journey from your website without opening the web chat at all. For more information, see [Guiding customers with journeys](/docs/watson-assistant?topic=watson-assistant-journeys).

- **Path changes**: Some internal paths used for communication with the assistant have changed. If you have firewall or proxy rules that are configured to allow specific paths, you might need to update your configuration to allow the following paths:

    - `/<SUBSCRIPTION_ID>/chat/<INTEGRATION_ID>/config`
    - `/<SUBSCRIPTION_ID>/chat/<INTEGRATION_ID>/message`

## 6.9.0
{: #6.9.0}

*Release data: 14 November 2022*

- You can now create _journeys_ to guide your customers through tasks they can already complete on your website. A journey is an interactive, multipart response that can combine text, video, and images, presented in sequence in a small window superimposed over your website.

    Journeys are available as a beta feature. For more information, see [Guiding customers with journeys](/docs/watson-assistant?topic=watson-assistant-journeys).

## 6.8.1
{: #6.8.1}

*Release date: 7 November 2022*

- Bug fixes.

## 6.8.0
{: #6.8.0}

*Release date: 31 October 2022*

- Added support for sending pre-chat information to the Salesforce integration.

## 6.7.0
{: #6.7.0}

*Release date: 10 October 2022*

- **New `updateIsTypingCounter()` method**: The new `updateIsTypingCounter()` instance method updates the counter that determines whether the typing indicator is displayed. For more information, see [instance.updateIsTypingCounter()](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#updateistypingcounter){: external}.

- **New `updateBotUnreadIndicatorVisibility()` method**: The new `updateBotUnreadIndicatorVisibility()` instance method specifies whether the unread indicator on the launcher icon is shown or hidden. For more information, see [instance.updateBotUnreadIndicatorVisibility()](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#updatebotunreadindicatorvisibility){: external}.

- Connect to Agent and custom cards now have rounded corners.

- Bug fixes.

## 6.6.2
{: #6.6.2}

*Release date: 15 August 2022*

- Bug fixes

## 6.6.1
{: #6.6.1}

*Release date: 8 August 2022*

- The `servers` property now supports a new `webChatScriptPrefix` option. Use this property to configure a proxy between your users' browsers and the {{site.data.keyword.cloud_notm}} servers that host the web chat JavaScript code. For more information, see [Setting up a proxy](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-configuration#serversconfig){: external}.

## 6.6.0
{: #6.6.0}

*Release date: 25 July 2022*

- A new `servers` property is now available in the web chat configuration options. You can use this property to set up a proxy between your users' browsers and {{site.data.keyword.assistant_classic_short}}. For more information, see [Setting up a proxy](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-configuration#serversconfig){: external}.

## 6.5.2
{: #6.5.2}

*Release date: 11 July 2022*

- Bug fix for `date` response type.

## 6.5.1
{: #6.5.1}

*Release date: 15 June 2022*

- Bug fix for the Zendesk integration.

## 6.5.0
{: #6.5.0}

*Release date: 6 June 2022*

- **New agent events**: New events are now fired by the web chat when messages are sent or received during a conversation with a human agent using a service desk integration. For more information, see [Agent events summary](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-events#agent-summary){: external}.

- Bug fixes.

## 6.4.1
{: #6.4.1}

*Release date: 16 May 2022*

- **Minimum size**: The minimum allowed size of the rendered web chat window has been reduced to satisfy the accessibility requirements defined by the [Web Content Accessibility Guidelines (WCAG) 2.1](https://www.w3.org/TR/WCAG21/){: external} standard.

- **Pop-up windows and tabs from iframes**: The web chat now allows pop-up windows and new tabs to be opened from content rendered inside `iframe` responses.

- **Faster responses**: Responses received from the assistant are now displayed more quickly and without a **`...`** typing indicator.

## 6.4.0
{: #6.4.0}

*Release date: 18 April 2022*

- **Date picker**: If you configure a step to collect a **Date** customer response, the step now uses the new `date` response type to request that the web chat display a graphical date picker the customer can use to select a date, as an alternative to typing the date in the input field. Existing steps do not automatically inherit this behavior; if you want to use the date picker, you must delete the existing Date response and then re-add it.

- **Skip "connect to agent" card**: A new `serviceDesk.skipConnectAgentCard` configuration option is available. If this option is enabled, the web chat immediately connects to an agent when it receives a _Connect to agent_ response, without first displaying a card and waiting for the user to click.

- **Close button**: A new `showCloseAndRestartButton` configuration option specifies whether the web chat interface shows an **`X`** (Close) button in addition to the existing **`-`** (Minimize) button. A customer can click this button to close the web chat, end the conversation, and end any conversation with a human agent. The chat transcript is also cleared, but any transcript of a conversation with a human agent is preserved.

## 6.3.0
{: #6.3.0}

*Release date: 24 March 2022*

- **Search cards**: Search cards have a new design.

- **New `restartConversation()` method**: The new `restartConversation()` instance method restarts the conversation with the assistant by clearing the web chat transcript and starting a new session. It also fires two new events (`pre:restartConversation` and `restartConversation`).

    For more information, see [instance.restartConversation()](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#restartconversation){: external}, [`pre:restartConversation`](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-events#prerestartconversation){: external}, and [`restartConversation`](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-events#restartconversation){: external}.
  
- **New `agentEndConversation()` method**: The new `agentEndConversation()` instance method immediately ends the conversation with a human agent without requesting confirmation from the user. For more information, see [`instance.agentEndConversation()`](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#agentendconversation){: external}.

- Bug fixes.

## 6.2.0
{: #6.2.0}

*Release date: 7 March 2022*

- **Navigation**: The web chat has been updated with new navigation features. For example, new “Back” and “Minimize” buttons make it easier to navigate between the home screen, the chat view, and panels. A new customizable drop-down menu appears near the avatar in both the assistant and agent chat views. This new functionality allows you to easily add new options to the menu of the web chat (for more information, see [updateCustomMenuOptions](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#updatecustommenuoptions){: external}.

    The experience of connecting to a human agent has also been improved to make it clearer how a user requests an agent, returns to chatting with the assistant, and ends the conversation.

    The animations for the web chat panels have also been improved to make the whole experience more seamless and cohesive.

- **Launcher**: The new web chat launcher bounces on two different occasions to attract attention and encourage customer engagement. For more information, see [Launcher appearance and behavior](/docs/watson-assistant?topic=watson-assistant-web-chat-architecture#web-chat-architecture-launcher).

- **Launcher**: Support has been added to control what text the launcher greets the user with, and when the greeting message is shown. For more information, see [Launcher](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#launcher){: external}.

- **Locale**: The web chat no longer sets the locale in the system context to `en-us` when no locale is configured. The locale is set in the system context only if it is configured for web chat.

- Bug fixes.

## 6.1.0
{: #6.1.0}

*Release date: 7 February 2022*

- Updated to support internal changes to the preview link feature.

## 6.0.1
{: #6.0.1}

*Release date: 24 January 2022*

- Bug fix for the disclaimer. For more information about the disclaimer, see [Configuration options object](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-configuration#configurationobject){: external}.

## 6.0.0
{: #6.0.0}

*Release date: 19 January 2022*

- **API version**: The web chat now uses the `2021-11-27` version of the {{site.data.keyword.assistant_classic_short}} API. Previously it used the `2020-09-24` API version. For information about API changes that have been introduced since the `2020-09-24` version, see the release notes for [27 November 2021](/docs/watson-assistant?topic=watson-assistant-watson-assistant-release-notes#watson-assistant-nov272021) and [16 July 2021](https://cloud.ibm.com/docs/watson-assistant?topic=watson-assistant-watson-assistant-release-notes#watson-assistant-jul162021).

- **Launcher**: The new web chat launcher welcomes and engages customers so they know where to find help if they need it. For more information, see [Launcher appearance and behavior](/docs/watson-assistant?topic=watson-assistant-web-chat-overview#web-chat-architecture-launcher).

- **Home screen**: The web chat home screen has been updated to have a more modern look. For more information about the home screen, see [Configuring the home screen](/docs/watson-assistant?topic=watson-assistant-web-chat-config#web-chat-configure-home-screen).

- **Agent events**: New events are now fired by the web chat when interacting with a human agent using a service desk integration. If you are using a custom service desk integration based on the [starter kit](https://github.com/watson-developer-cloud/assistant-web-chat-service-desk-starter){: external}, you can use these events to create a pre-chat form before the agent escalation occurs, to create a post-chat form after the agent conversation ends, or to specify what happens if an agent isn’t available (like create a ticket submission form). For more information, see [Agent events summary](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-events#summary){: external}.

- **Markdown support**: The web chat now fully supports common Markdown formatting in messages received from an assistant. You might need to review existing assistant output that contains strings that might be recognized as Markdown. (For example, a line of text that begins with a greater-than (`>`) character is interpreted as a block quote.)

- **Time zone**: The time zone set in the context by the web chat no longer overrides any time zone set by the assistant.

- **Locale**: Any locale configured for the web chat is now sent to the assistant as part of the context.

- **Window open events**: The `window:pre:open` and `window:open` events now fire any time the chat window is opened, regardless of the reason. In previous releases, these events only fired if the window was opened by the customer clicking on the built-in launcher. Other methods of opening the chat window, such as session history or custom launchers, did not fire these events.

    The event data passed to the listener has a new `reason` property that indicates the reason the window was opened. If you want to preserve the previous behavior, you can modify your handler to check this property:

    ```javascript
    instance.on({ type: "window:open", handler: event => {
      if (event.data.reason === 'default_launcher') {
        // Previous code.
      }
    }});
    ```

    For more information, see [Window open reasons](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-events#windowopenreasons){: external}.

- **hideCloseButton property renamed**: The `hideCloseButton` property for custom panels has been renamed `hideBackButton`. The behavior of the property has not changed. For more information, see [customPanel.open()](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#custompanelopen){: external}.

## 5.1.2
{: #5.1.2}

*Release date: 11 December 2021*

- Bug fix for Salesforce integration.

## 5.1.1
{: #5.1.1}

*Release date: 5 November 2021*

- **"User is typing" support**: The web chat now supports displaying the "user is typing" message for service desks. This feature is supported for the Salesforce and Zendesk integrations, as well as any [starter kit](https://github.com/watson-developer-cloud/assistant-web-chat-service-desk-starter){: external} integration that implements it.
- Bug fixes.

## 5.1.0
{: #5.1.0}

*Release date: 28 October 2021*

- **Custom Panels**: The web chat now supports customizable panels you can use to display any custom HTML content (for example, a feedback form or a multistep process). Your code can use instance methods to dynamically populate a custom panel, as well as open and close it. For more information, see [Custom Panels](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#custompanels){: external}.

## 5.0.2
{: #5.0.2}

*Release date: 4 October 2021*

- A [new tutorial](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=tutorials-react-portals){: external} is now available that shows how to use Carbon components to customize user-defined responses and writeable elements.
- Bug fixes.

## 5.0.1
{: #5.0.1}

*Release date: 20 September 2021*

- Bug fixes.

## 5.0.0
{: #5.0.0}

*Release date: 16 September 2021*

- **New response types**: The web chat now supports the new `video`, `audio`, and `iframe` response types. For more information about these response types, see [Rich responses](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-multimedia){: external}.

- **Link to start web chat**: You can now create a set of HTML links that go directly to your web chat and start conversations on specific topics. For example, you might want to send an email inviting customers to update their account information; you can include a link that opens the web chat on your site and sends the initial message `I want to update my account`. For more information, see [Creating links to web chat](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-configuration#pageLinks){: external}.

- **CSS improvements**: Support for CSS styles has been improved to change the way the web chat resets styles in areas where you can include your own custom content, such as user-defined responses and writeable elements. The new approach better protects custom content from accidental style overrides. For more information about custom content and CSS classes, see [Theming &amp; custom content](https://web-chat.global.assistant.watson.cloud.ibm.com/testfest.html?to=api-render){: external}.

    If you have any custom content (such as user-defined responses or writeable elements), verify that any styling is still rendering as you expect. Consider using the new [`ibm-web-chat--default-styles` class](https://web-chat.global.assistant.watson.cloud.ibm.com/testfest.html?to=api-render#helper_classes){: external} to maintain consistency with the web chat default styles.
    {: note}

- **Support for Carbon components**: As part of the new styling support, you can now use [Carbon components](https://www.carbondesignsystem.com/components/overview/){: external} in user-defined responses and web chat writeable elements. These components will inherit any theming customizations you have made to the web chat.

- **New embedded script**: The embedded script you use to add the web chat to your website has been updated to avoid unexpected code changes when you lock on to a web chat version. (For more information about web chat versioning, see [Versioning](/docs/assistant?topic=assistant-web-chat-basics#web-chat-basics-versions){: external}.) The previous version of the script will continue to work but is now deprecated. If you want to upgrade your existing web chat deployments to use the new script, copy the updated code snippet from the **Embed** tab of the web chat integration settings. (Remember to reapply any customizations you have made.)
- **Removal of deprecated methods and events**:
    - The `error` event has been replaced by the `onError` method in the [configuration object](https://web-chat.global.assistant.watson.cloud.ibm.com/testfest.html?to=api-configuration#configurationobject){: external}.
    - The `getID` method has been removed.
- Microsoft Internet Explorer 11 is no longer a supported browser.

## 4.5.1
{: #4.5.1}

*Release date: 30 August 2021*

- Bug fixes for the interactive launcher beta feature. (For more information about this feature, see the `launcherBeta` configuration option at [Configuration options object](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-configuration#configurationobject){: external}.)

## 4.5.0
{: #4.5.0}

*Release date: 29 July 2021*

- A new `scrollToMessage` method is available for scrolling the web chat view to a specified message in the chat history. For more information, see [instance.scrollToMessage()](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#scrollToMessage){: external}.
- A new `pre:open` event is available. This event is fired when the web chat window is opened, but before the welcome message or chat history are loaded. For more information, see [window:pre:open](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-events#windowpreopen){: external}.
- A new chat history widget is available for embedding in service desk agent UIs. This new widget is based on a read-only view of the standard web chat widget. For information about using the new chat history widget in integrations built using the starter kit, see [Embedded agent application](https://github.com/watson-developer-cloud/assistant-web-chat-service-desk-starter/blob/main/docs/AGENT_APP.md){: external}.

## 4.4.1
{: #4.4.1}

*Release date: 6 July 2021*

- Bug fixes.

## 4.4.0
{: #4.4.0}

*Release date: 25 June 2021*

- Bug fixes.

## 4.3.0
{: #4.3.0}

*Release date: 7 June 2021*

- **Search suggestions**: If a search skill is configured for your assistant, the suggestions include a new **View related content** section. This section contains search results that are relevant to the user input.
- **Focus trap**: A new `enableFocusTrap` option enables maintaining focus inside the web chat widget while it is open. For more information, see [Configuration options object](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-configuration#configurationobject){: external}.

## 4.2.1
{: #4.2.1}

*Release date: 6 May 2021*

- **Service URLs updated**: The URLs used by the web chat to communicate with the Assistant service have been updated to remove the dependency on the deprecated `watsonplatform.net` domain. This change applies retroactively to version 3.3.0 and all subsequent web chat releases. Make sure the system that hosts the web chat widget has access to the new URL; for more information, see [Deploy your assistant in production](/docs/assistant?topic=assistant-deploy-web-chat#deploy-web-chat-snippet){: external}.

## 4.2.0
{: #4.2.0}

*Release date: 27 April 2021*

- **Conversation starters in suggestions**: The conversation starters you configure for the home screen are now shown as suggestions. If suggestions are enabled, the conversation starters appear in a new section titled **People are also interested in**. This provides an easy way for customers to change the subject or start the conversation over. For more information about the suggestions feature, see [Showing more suggestions](/docs/assistant?topic=assistant-deploy-web-chat#deploy-web-chat-alternate){: external}. For more information about the home screen, see [Configuring the home screen](/docs/assistant?topic=assistant-deploy-web-chat#deploy-web-chat-home-screen){: external}.

- **`onError` callback**: The new `onError` callback option in the web chat configuration enables you to specify a callback function that is called if errors occur in the web chat. This makes it possible for you to handle any errors or outages that occur with the web chat. For more information, see [Listening for errors](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-configuration#onerror-detail){: external}.

- **Session ID available in widget state**: The state information returned by the `getState()` instance method now includes the session ID for the current conversation. For more information, see [instance.getState()](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#getState){: external}.

- **IBM watermark**: The web chat can now display a **Built with IBM Watson** watermark to users. This watermark is always enabled for any new web chat integrations on Lite plans. For more information, see [Create a web chat instance to add to your website](/docs/assistant?topic=assistant-deploy-web-chat#deploy-web-chat-task){: external}.

- **Fixes to rendering of list items**: The rendering of HTML list items in the web chat widget has been updated.

## 4.1.0
{: #4.1.0}

*Release date: 8 April 2021*

- **Home screen now generally available**: Ease your customers into the conversation by adding a home screen to your web chat window. The home screen greets your customers and shows conversation starter messages that customers can click to easily start chatting with the assistant. For more information about the home screen feature, see [Configuring the home screen](/docs/assistant?topic=assistant-deploy-web-chat#deploy-web-chat-home-screen){: external}.

- **Home screen enabled by default**: The home screen feature is now enabled by default for all new web chat deployments.

- **Home screen context support**: You can now access context variables from the home screen. Note that initial context must be set using a `conversation_start` node. For more information, see [Starting the conversation](/docs/assistant?topic=assistant-dialog-start#dialog-start-welcome){: external}.

## 4.0.0
{: #4.0.0}

*Release date: 16 March 2021*

- **Session history now generally available**: Session history allows your web chats to maintain conversation history and context when users refresh a page or change to a different page on the same website. It is enabled by default. For more information about this feature, see [Session history](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=tutorials-session-history){: external}.

  Session history persists within only one browser tab, not across multiple tabs. The dialog provides an option for links to open in a new tab or the same tab. See [this example](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=tutorials-session-history#Tutorial1) for more information on how to format links to open in the same tab.

  Session history saves changes that are made to messages with the [pre:receive event](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-events#prereceive) so that messages still look the same on rerender. This data is only saved for the length of the session. If you prefer to discard the data, set `event.updateHistory = false;` so the message is rerendered without the changes that were made in the pre:receive event.

  [instance.updateHistoryUserDefined()](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#updateHistoryUserDefined) provides a way to save state for any message response. With the state saved, a response can be rerendered with the same state. This saved state is available in the `history.user_defined` section of the message response on reload. The data is saved during the user session. When the session expires, the data is discarded.

  Two new history events, [history:begin](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-events#historybegin) and [history:end](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-events#historyend) announce the beginning and end of the history of a reloaded session. These events can be used to view the messages that are being reloaded. The history:begin event allows you to edit the messages before they are displayed.

  See this example for more information on saving the state of [customResponse](https://web-chat.global.assistant.watson.cloud.ibm.com/testfest.html?to=api-events#customresponse) types in session history.

- **Channel switching**: You can now create a dialog response type to functionally generate a connect-to-agent response within channels other than web chat. If a user is in a channel such as Slack or Facebook, they can trigger a channel transfer response type. The user receives a link that forwards them to your organization's website where a connection to an agent response can be started within web chat. For more information, see [Adding a Channel transfer response type](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-add-channel-transfer){: external}.

## 3.5.0
{: #3.5.0}

*Release date: 17 February 2021*

- **Session history (beta)**: Web chat session history (beta) is now available. This feature makes it possible to maintain conversation history and context when customers refresh the page or navigate to a different page on the same website. For more information about this beta feature, see [Session history (beta)](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=tutorials-session-history).

## 3.4.1
{: #3.4.1}

*Release date: 2 February 2021*

- Made an accessibility enhancement to the chat history. Now, you can use keys to navigate the messages by clicking the chat history, pressing Enter, and then using the arrow keys to move from one message to the next.
- The `instance.updateAssistantInputFieldVisibility()` method was added. You can use it to hide or show the text input field. For example, you might use the `pre:receive` event to check whether an options response type is being returned and if so, hide the text field so the user is forced to pick one of the available options only.
- The `instance.getState()` method was added. You can use it to check for specific conditions, such as `isWebChatOpen`, before you perform an action that might rely on the condition being true.

For more information about the new methods, see [Instance methods](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods){: external}.

## 3.3.2
{: #3.3.2}

*Release date: 17 December 2020*

- Addressed accessibility issues.

## 3.3.1
{: #3.3.1}

*Release date: 3 December 2020*

- The translated strings in the [language files](https://github.com/watson-developer-cloud/assistant-web-chat/tree/main/languages) were revised and improved.
- An error message is shown now if a Java Web Token (JWT) that is provided with an incoming message is invalid. If the first JWT fails when the web chat opens, an error message is displayed in place of the web chat window that says, `There was an error communicating with Watson Assistant`. If the initial JWT is valid, but the token for a subsequent message is invalid, a more discreet error message is displayed in response to the insecure input.
- Bug fixes.

## 3.3.0
{: #3.3.0}

*Release date: 23 November 2020*

- Added support for passing contextual information to a service desk agent from web chat.
- You can now customize a `user_defined` response type. For more information, see the [Custom response type tutorial](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=tutorials-user-defined-response).
- Bug fixes.

## 3.2.1
{: #3.2.1}

*Release date: 2 November 2020*

- **Bug fix**: Fixing a bug that prevented the web chat integration preview from working after security was enabled.

## 3.2.0
{: #3.2.0}

*Release date: 26 October 2020*

- **Security improvement**: If you enable security, you no longer need to include the `identityToken` property when the web chat is loaded on a web page. If a token is not initially provided, the existing identityTokenExpired event will be fired when the web chat is first opened to obtain one from your handler.

- **Starter kit update**: The starter kit now allow you to customize the timeout that occurs when the web chat integration checks whether any service desk agents are online.

## 3.1.1
{: #3.1.1}

*Release date: 22 October 2020*

- **Accessibility improvement**: Changed how the announcement text is generated to prevent announcements from being duplicated. Announcement text is hidden text that is provided for use by screen readers to indicate when dynamic web page changes occur.

## 3.1.0
{: #3.1.0}

*Release date: 8 October 2020*

- **Suggestions now allow for trial and error**: If customers select a suggestion and find that the response is not helpful, they can open the suggestions list again and try a different suggestion.

## 3.0.0
{: #3.0.0}

*Release date: 22 September 2020*

- **Choose when a link to support is included in suggestions**: The Suggestions beta feature has moved to its own tab. Now you can enable suggestions even if your web chat is not set up to connect to a service desk solution. That's because now you can control if and when the option to connect to customer support is available from the suggestions list. For more information, see [Showing more suggestions](/docs/assistant?topic=assistant-deploy-web-chat#deploy-web-chat-alternate){: external}.

- **Search result format change**: To support the ability to show more than 3 search results in a response, the search skill response type format changed. If you are using `pre:receive` or `receive` handlers to process search results, you might need to update your code. The `results` property was replaced by the `primary_results` and `additional_results` properties. For more information about the new search skill response type format, see the [API reference](https://cloud.ibm.com/apidocs/assistant/assistant-v2#message){: external}.

- **Language pack key change**: Due to improvements that were made to allow you to specify separate chat transfer messages for situations where agents are available and unavailable, the [language source file](https://github.com/watson-developer-cloud/assistant-web-chat/tree/master/languages) was updated. The `agent_chatDescription` was renamed to `default_agent_availableMessage` and another key (`default_agent_unavailableMessage`) was added. If you defined a custom string for the `agent_chatDescription` key, you must modify your code to reflect this change. For more information about the new availability messages and how they are used, see [Adding a *Connect to human agent* response type](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-add-connect-to-human-agent){: external}.

## 2.4.0
{: #2.4.0}

*Release date: 2 September 2020*

- **Add a home screen**: Ease your customers into the conversation by adding a home screen to your web chat window. The home screen greets your customers and shows conversation starter messages that customers can click to submit to the assistant. For more information about this beta feature, see [Adding a home screen](/docs/assistant?topic=assistant-deploy-web-chat#deploy-web-chat-home-screen){: external}.

## 2.3.0
{: #2.3.0}

*Release date: 10 August 2020*

- **Introducing *Suggestions***: Enable this beta feature to show a helper icon in the chat that customers can click to see alternate topics or to connect to an agent. For more information, see [Showing more suggestions](/docs/assistant?topic=assistant-deploy-web-chat#deploy-web-chat-alternate){: external}.

- **Search results are now expandable**: When search results are displayed in the web chat, users can click **Show more** to see more of the search result text.

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

*Release date: 2 April 2020*

- **Introduced the `learningOptOut` parameter**: You can add the `learningOptOut` parameter and set it to `true` to add the `X-Watson-Learning-Opt-Out` header to each `/message` request that originates from the Web Chat. For more information about the header, see [Data collection](https://cloud.ibm.com/apidocs/assistant/assistant-v2#data-collection){: external}. For more information about the parameter, see [Configuration](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-configuration){: external}.

## 1.4.0
{: #1.4.0}

*Release date: 20 March 2020*

- **Customize the CSS theme**: For more information, see [Theming](https://integrations.us-south.assistant.watson.cloud.ibm.com/web/developer-documentation/api-instance-methods#theming){: new_window}.

- **Shadow DOM is no longer used**: When you use custom response types or HTML in your dialog, you can apply CSS styles that are defined in your web page to the assistant's response. To override any default styling in the web chat, you must specify the `!important` modifier in your CSS. For more information, see [Rendering response types](https://integrations.us-south.assistant.watson.cloud.ibm.com/web/developer-documentation/api-render#html){: new_window}.
