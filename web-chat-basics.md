---

copyright:
  years: 2019, 2021
lastupdated: "2021-01-19"

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

# Web chat overview
{: #web-chat-basics}

Learn more about the web chat that you can add to your company website.
{: shortdesc}

Web chat is a framework-agnostic code snippet you can immediately embed in your website. 

When you build a custom user interface for your assistant, you spend a lot of time and effort writing code to solve typical UI problems. For example, you must keep up with browser support changes, manage scrolling behavior, validate input, and design the layout and styling. The time you spend designing and maintaing a UI can be better spent building a quality assistant instead. When you use the web chat integration, you can rely on us to manage the user interface, so you can focus on designing conversational exchanges that address the unique business needs of your customers. Cutting-edge functionality from IBM Design and Research is incorporated into the web chat to deliver an exceptional user experience.

For more information about how to deploy the web chat, see [Integrating the web chat with your website](/docs/assistant?topic=assistant-deploy-web-chat).

## Browser support
{: #web-chat-basics-browsers}

The web chat supports a variety of devices and platforms. As a general rule, if the last two versions of a browser account for more than 1% of all desktop or mobile traffic, the web chat supports that browser.

The following list specifies the minimum required browser software for the web chat (including the two most recent versions, except as noted):

- Google Chrome
- Apple Safari
- Mobile Safari
- Chrome for Android
- Microsoft Internet Explorer 11 (most recent version only)
- Microsoft Edge (Chromium and non-Chromium)
- Mozilla Firefox
- Firefox ESR (most recent ESR only)
- Opera
- Samsung Mobile Browser
- UC Browser for Android
- Mobile Firefox

For optimal results when rendering the web chat on mobile devices, the `<head>` element of your web page must include the following metadata element:

```html
<meta name="viewport" content="width=device-width, initial-scale=1" />
```
{: codeblock}

## Global audience support
{: #web-chat-basics-global}

The underlying skills understand customer messages that are written in any of the languages that are supported by the service. For more information, see [Supported languages](/docs/assistant?topic=assistant-language-support). The responses from your assistant are defined by you in the underlying skill and can be written in any language you want.

Even if your skill includes responses in a language other than English, some of the phrases that are displayed in the web chat widget are added by the web chat itself and do not come from the underlying skill. These hardcoded phrases are specified in English unless you choose to apply a different language.

There are language files that contain translations of each English-language phrase that is used by the web chat. You can instruct the web chat to use one of these other languages files by using the `instance.updateLanguagePack()` method.

Likewise, the web chat applies an American English locale to content that is added by the web chat unless you specify something else. The locale setting affects how values such as dates and times are formatted. 

To configure the web chat for customers outside the US, follow these steps:

1.  To apply the appropriate syntax to dates and times *and* to use a translation of the English-language phrases, set the locale. Use the `instance.updateLocale()` method.

    For example, if you apply the Spanish locale (`es`), the web chat uses Spanish-language phrases that are listed in the `es.json` file, and uses the `DD-MM-YYYY` format for dates instead of `MM-DD-YYYY`. 
    
    The locale you specify for the web chat does not impact the syntax of dates and times that are returned by the underlying skill.
    {: note}

1.  To change only the language of the hardcoded English phrases, use the `instance.updateLanguagePack()` method.

    For more information, see [Instance methods](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#languages){: external}.

1.  To change the text direction of the page from right to left, use the `direction` method. For more information, see [Configuration](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-configuration#configurationobject){: external}.

### Customizing phrases
{: #web-chat-basics-phrasing}

You can edit the hardcoded phrases that are displayed in the web chat to customize them for branding or style. Whether your web chat responds in English or another language, you can customize the wording that is used. For example, you might want to replace the phrase that says, `The live agent is typing` with the phrase, `A customer support agent is typing`.

The language files contain [ICU Message Format](http://userguide.icu-project.org/formatparse/messages){: external} JSON representations of all of the languages that are supported by the web chat. You can edit all or a subset of the phrases in a JSON file, and then configure the web chat to use your version by specifying the `instance.updateLanguagePack()` method.

For more information, see [Languages](https://web-chat.global.assistant.watson.cloud.ibm.com/docs.html?to=api-instance-methods#languages){: external}.

## Versioning
{: #web-chat-basics-versions}

Web chat follows semantic versioning practices. Starting with web chat version 2.0.0, you can set the version of web chat that you want to use as a configuration option.

If you don't specify a version, the latest version is used automatically (`clientVersion="latest"`). When you apply the latest version, you benefit from the continuous improvements, feature additions, and bug fixes that are made to the web chat regularly.

However, if you apply extensive customizations to your deployment, such as overriding the theme with your own custom theme, for example, you might want to lock your deployment to a specific version. Locking on a version enables you to test new versions before you apply them to your live web chat.

To use a specific version (`clientVersion="major.minor.patch"`), specify it as follows:

The following examples show what to specify when the current version is `2.3.1`.

- If you want to stay on a major version, but get the latest minor and patch releases, specify `clientVersion="2"`.
- If you want to stay on a minor version, but get the latest patch releases, specify `clientVersion="2.3"`. 
- If you want to lock on to a specific minor version and patch release, specify `clientVersion="2.3.1"`.

To test the updates in a version release of web chat before you apply the version to your live web chat, follow these steps:

1.  Lock the web chat that is running in your production environment to a specific version. 
1.  Embed your web chat deployment into a new page in a test environment, and then override the version lock setting. For example, specify `clientVersion="latest"`.
1.  Test the web chat, and make adjustments to the configuration if necessary.
1.  Update your production deployment to use the latest version and apply any other configuration changes that you determined to be necessary after testing.

For more information about features that were introduced in previous web chat versions, see the [Web chat release notes](/docs/assistant?topic=assistant-release-notes-chat).

## Billing
{: #web-chat-basics-billing}

Watson Assistant charges based on the number of unique monthly active users (MAU).

The web chat uses the following methods to track users:

- You can pass in a de—identified user ID either as part of a secure JWT or as a string passed to `instance.updateUserID` method. If you are using advanced security features, the user ID is derived from the sub claim in the JWT.
- If you do not pass an identifier for the user when the session begins, the web chat creates one for you. It creates a first-party cookie with a generated anonymous ID. The cookie remains active for 45 days. If the same user returns to your site later in the month and chats with your assistant again, the web chat integration recognizes the user. And you are charged only once when the same anonymous user interacts with your assistant multiple times in a single month.

For more information about billing, see [User-based plans explained](/docs/assistant?topic=assistant-services-information#services-information-user-based-plans).

For more information about MAU limits per plan, see [Web chat integration limits](/docs/assistant?topic=assistant-deploy-web-chat#deploy-web-chat-limits).

For more information about deleting a user's data, see [Labeling and deleting data](/docs/assistant?topic=assistant-information-security#information-security-gdpr-wa).

## Human agent service integration
{: #web-chat-basics-haa}

You can configure the web chat to transfer a customer to a human customer support agent if the customer asks for help from a person. The following service desk integrations are supported:

Built-in service desk integrations:

- [Intercom](/docs/assistant?topic=assistant-deploy-intercom)  ![Plus or Premium plan only](images/plus.png)
- [Web chat with Salesforce support](/docs/assistant?topic=assistant-deploy-salesforce)
- [Web chat with Zendesk support](/docs/assistant?topic=assistant-deploy-zendesk)

Web chat service desk reference implementations:

- [Genesys Cloud](https://github.com/watson-developer-cloud/assistant-web-chat-service-desk-starter/tree/main/src/middleware/genesys){: external}
- [NICE inContact](https://github.com/watson-developer-cloud/assistant-web-chat-service-desk-starter/tree/main/src/middleware/incontact){: external}
- [Twilio Flex](https://github.com/watson-developer-cloud/assistant-web-chat-service-desk-starter/tree/main/src/middleware/flex){: external}
- [Bring your own (starter kit)](https://github.com/watson-developer-cloud/assistant-web-chat-service-desk-starter/tree/main/src/middleware/genesys){: external}

## Accessibility
{: #web-chat-basics-accessibility}

IBM strives to provide products with usable access for everyone, regardless of age or ability. 

The web chat integration is enabled for [Web Content Accessibility 2.1 Level AA](https://www.w3.org/WAI/standards-guidelines/wcag/new-in-21/){: external} compliance. It is tested with both screen readers and automated tools on a continual basis.

## More information
{: #web-chat-basics-more}

To learn more about advanced tasks you can perform with the web chat, read the following topics:

- [Securing the web chat](/docs/assistant?topic=assistant-web-chat-security)
- [Applying advanced customizations](/docs/assistant?topic=assistant-web-chat-config)
