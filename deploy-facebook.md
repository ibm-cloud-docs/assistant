---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-12"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Integrating with Facebook Messenger
{: #deploy-facebook}

After you configure a conversational skill and add it to an assistant, you can integrate the assistant with Facebook Messenger.

1.  From the Assistants tab, click to open the assistant tile that you want to deploy.

1.  From the Integrations section, click **Add Integration**.

1.  Click the **Select Integration** button for *Facebook Messenger*.

1.  Follow the instructions that are provided on the screen to complete the integration process.

## Chatting with the assistant

To start a chat with the assistant, complete the following steps:

1.  Open Facebook Messenger.
1.  Type the name of the page you created earlier.
1.  After the page comes up, click it, and then start chatting with the assistant.

The dialog flow for the current session is restarted after 60 minutes of inactivity. Meaning if a user stops interacting with the assistant, after 60 minutes any context variable values that were set during the previous conversation are set to null or back to their default values.