---

copyright:
  years: 2015, 2020
lastupdated: "2020-04-21"

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

# Opening assistants and skills
{: #assistant-settings}

Learn how to find and open assistants and skills that you created or can access.
{: shortdesc}

Assistants and skills are created within a {{site.data.keyword.conversationshort}} service instance. To continue working with a skill or assistant, open the service instance that contains the skill or assistant. If you can't remember the service instance name, you can switch between instances from within the {{site.data.keyword.conversationshort}} user interface.

1.  Go to the [{{site.data.keyword.Bluemix_notm}} Resource list)](https://cloud.ibm.com){: external}.

1.  Log in.

    A list of the service instances that you own or were given access to is displayed.

1.  Click a service instance to open it.

1.  Click **Launch {{site.data.keyword.conversationshort}}** from the service instance details page to open the product in a new browser tab or window.

1.  Click the appropriate icon from the navigation pane to see a list of your assistants or skills.

  - ![Assistants menu icon](images/nav-ass-icon.png) Assistants
  - ![Skills menu icon](images/nav-skills-icon.png) Skills

If you do not see the skill or assistant you are looking for, you can look for it in a different service instance.

## Switching the service instance
{: #assistant-settings-switch-instance}

To switch to a different {{site.data.keyword.conversationshort}} service instance, complete the following steps:
    
1. From the header of any page in the current instance, click the User icon ![user icon](images/user-icon.png).
1.  Choose **Switch instance** from the menu.
1.  A list of the available service instances is displayed. Click a different service instance to open it.

## Getting API information
{: #assistant-settings-api-details}

Get information, such as the assistant ID and skill ID and credentials that you need before you can use the API. 

- For an assistant, click the overflow menu, and then choose **Settings**. Click **API Details**.
- For a skill, click the overflow menu, and then choose **View API Details**.

  You can find the ID of the workspace that is associated with a dialog skill on the *Skill API Details* page also. It is included in the Legacy v1 workspace URL, after the `/workspaces/` segment.
  {: note}

If you cannot view the API details or service credentials, it is likely that you do not have Manager access to the service instance in which the resource was created. Only people with Manager service access to the instance can use the service credentials. 

If you have a Writer or Reader service access role to the instance and want to use the API, you can create a personal API key. Go to the [IBM Cloud API keys](https://cloud.ibm.com/iam/apikeys){: external} page to create an IBM Cloud API key to use to authenticate your API requests. For more information, see [Managing user API keys](/docs/iam?topic=iam-userapikey){: external}. With the personal key, you can make API requests that you have the appropriate privileges to complete. For more information about API privileges per access role, see [Resource-level role impact on available actions](/docs/assistant?topic=assistant-access-control#access-control-ui-impact).

If the assistant ID is not displayed, you can copy it from the URL of the assistant web page. The assistant ID is listed just after `/assistants/{assistantID}`. Similarly, you can get the skill ID from the URL of the skill page. The skill ID is listed just after `/skills/{skillID}`.
{: tip}

## Changing the inactivity timeout setting ![Plus or Premium plan only](images/plus.png)
{: #assistant-settings-change-timeout}

When a user interacts with your assistant through one of the built-in integrations, the chat session ends after a specific period of time passes in which the user does not interact with the assistant. You can specify the amount of time to wait before a chat session is reset by changing the inactivity timeout setting.

When a chat session is reset, the dialog loses any contextual information that it saved during the previous exchange with the user. For example, if the dialog asks for the user's name and then calls the user by that name throughout the rest of the conversation, then after the chat session ends and a new one begins, the dialog will start by asking for the user's name again.

To change the timeout, complete the following steps:

1.  Click the menu for your assistant ![open and close list of options](images/kabob-beta.png), and then choose **Settings**.

1.  Change the time specified in the **Timeout limit** field.

1.  Close the settings page. Your change is saved automatically.

## Session limits
{: #assistant-settings-session-limits}

The length allowed for an inactivity timeout differs by service instance plan type. The following table lists the limits.

| Service plan | Chat session default inactivity period | Chat session maximum inactivity period |
|--------------|--------------------------------:|----------------------------:|
| Premium      |                          1 hour |          168 hours (7 days) |
| Plus         |                          1 hour |                    24 hours |
| Standard (legacy) |                  5 minutes |                   5 minutes |
| Plus Trial   |                       5 minutes |                   5 minutes |
| Lite         |                       5 minutes |                   5 minutes |
{: caption="Service plan details" caption-side="top"}
