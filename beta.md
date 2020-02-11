---

copyright:
  years: 2015, 2020
lastupdated: "2020-01-31"

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
{:preview: .preview}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Early access program
{: #beta}

When you participate in the early access program, IBM gives you early access to features for your evaluation. These features are classified as beta, which means they might be unstable, might change frequently, and might be discontinued with short notice. Beta features also might not provide the same level of performance or compatibility that generally available features provide and are not intended for use in a production environment. Beta features are supported only on the [IBM Developer Answers](https://developer.ibm.com/answers/topics/watson-assistant/){: external}.

## Available beta features
{: #beta-features}

The following features are available for use by participants in the early access program only. To find out how to request access, see [Participate in the early access program](/docs/assistant?topic=assistant-feedback#feedback-beta).
{: preview}

- Delight your customers with 360-degree support by integrating your web chat with a third-party service desk solution. When a customer asks to speak to a person, you can connect them to an agent through a service desk solution, such as LiveEngage or Zendesk. For more information, see [Adding support for transfers](/docs/assistant?topic=assistant-beta-deploy-web-chat#deploy-web-chat-haa).

- You can now use the chat log from one of your production assistants as the source for intent and intent user example recommendations. See [Intent recommendations](/docs/assistant?topic=assistant-beta-intent-recommendations).

  With the introduction of this feature, the way in which CSV log files are stored also changed. Previously, a log CSV file that you uploaded to one skill was shared by all of the skills in that service instance. Now, a CSV file that you upload to one skill is available for use only by that one skill. For existing instances with CSV files, the shared CSV files have been made available to each of the skills in the instance. You can delete a CSV file from a skill that doesn't use it by managing the recommendation sources for the skill.
  {: important}

## Tell us what you think

To share your feedback, join the [Watson Developer Community on Slack](http://wdc-slack-inviter.mybluemix.net/)(: external). Add your first impressions and suggestions to the **#assistant-early-access** channel.