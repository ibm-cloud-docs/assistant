---

copyright:
  years: 2015, 2019
lastupdated: "2019-07-29"

subcollection: assistant

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
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

# Changing the inactivity timeout setting ![Plus or Premium plan only](images/plus.png)
{: #assistant-settings}

When a user interacts with your assistant through one of the built-in integrations, the chat session ends after a specific period of time passes in which the user does not interact with the assistant. You can specify the amount of time to wait before a chat session is reset by changing the inactivity timeout setting.
{: shortdesc}

When a chat session is reset, the dialog loses any contextual information that it saved during the previous exchange with the user. For example, if the dialog asks for the user's name and then calls the user by that name throughout the rest of the conversation, then after the chat session ends and a new one begins, the dialog will start by asking for the user's name again.

## Session limits
{: #assistant-settings-session-limits}

The length allowed for an inactivity timeout differs by service instance plan type. The following table lists the limits.

| Service plan | Chat session default inactivity period | Chat session maximum inactivity period |
|--------------|--------------------------------:|----------------------------:|
| Premium      |                          1 hour |          168 hours (7 days) |
| Plus         |                          1 hour |                    24 hours |
| Standard     |                       5 minutes |                   5 minutes |
| Plus Trial   |                       5 minutes |                   5 minutes |
| Lite*        |                       5 minutes |                   5 minutes |
{: caption="Service plan details" caption-side="top"}

## Changing the setting
{: #assistant-settings-timeout-task}

1.  Click the menu for your assistant, and then choose **Settings**.

1.  Change the time specified in the **Timeout limit** field.

    Use `1:00` instead of `00:60` to specify one hour.
    {: tip}

1.  Close the settings page. Your change is saved automatically.
