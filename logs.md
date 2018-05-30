---

copyright:
  years: 2015, 2018
lastupdated: "2018-03-19"

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

# About the Improve component

The Improve component of {{site.data.keyword.conversationshort}} provides a history of conversations between users and your application. You can use this history to improve your application's understanding of users' inputs.
{: shortdesc}

Sections of the **Improve** panel:

* [Overview](logs_oview.html): A summary of conversations that users had with your application.
* [User conversations](logs_convo.html): A list of individual user utterances. You can update intents and entities while viewing an individual user utterance. **Note**: A single user conversation may consist of multiple utterances.
* [Recommendations](logs_recommend.html): Ways to improve your system. Available only to Premium users.

## Improving across skills
To understand how to use utterance data to make improvements across skills, it is helpful to review the following definitions associated with the {{site.data.keyword.conversationshort}} service:

* ***Instance***: Your deployment of {{site.data.keyword.conversationshort}}, accessible with unique credentials. A {{site.data.keyword.conversationshort}} instance may be made up of multiple applications.
* ***Application/Bot***: An application - often referred to as a 'bot' - is one model of your {{site.data.keyword.conversationshort}} content.
* ***User***: A user is anyone who interacts with your application; often these are your customers.
* ***Utterance***: An utterance is a single message a user sends to the application.
* ***Conversation***: A set of utterances consisting of the messages that an individual user sends to your application, and the messages your application responds with.
* ***Conversation ID***: A unique identifier used to link related exchanges. This identifier is sent using the `/message` API, by including the Conversation ID inside the metadata object in your [context ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#message)
* ***Workspace ID***: The unique identifier of an application
* ***Deployment ID***: Unique labels that are passed with user utterances to help identify the deployment environment that the utterances come from.
<!-- * ***Customer ID***: A unique label that can identify a specific user of your application. -->

Creating a conversational skill is a very iterative process. While you develop your skill, you use the *Try it out* pane to verify that your skill recognizes the correct intents and entities in test inputs, and to make corrections as needed.

In the **Improve** panel, you can view information about actual interactions with your users, and make similar corrections to improve the accuracy with which intents and entities are recognized by your skill. It is difficult to know exactly *how* your users will ask questions, or what random utterances they may make, so it is important to frequently visit the **Improve** panel, in order to improve your skills.

For a {{site.data.keyword.conversationshort}} instance that includes multiple skills, there may be times when it is useful to use utterance data from one skill to improve another skill within that same instance. **Note**: If you are a {{site.data.keyword.conversationshort}} Premium user, your premium instances can optionally be configured to allow access to log data from skills across your different premium instances.

As an example, say you have a {{site.data.keyword.conversationshort}} instance named *HelpDesk*. You may have both a Production skill and a Development skill in your HelpDesk instance. When working in the Development skill, you can filter utterances based on the `Deployment ID` for the Production skill, so that you are using Production skill utterances to improve your Development skill.

![Data source link](images/data_source_1.png)

Any edits you then make within the Development skill will only affect the Development skill, even if you’re using Production skill utterances. See [Selecting a data source](logs_convo.html#selecting-a-data-source) for additional information.

To specify the Deployment ID for an utterance sent using the `/message` API, include the deployment property inside the metadata object in your [context ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#message){: new_window}, as in this example:
{: #deploy_id}

```json
"context" : {
  "metadata" : {
       "deployment": "HelpDesk-Production"
  }
}
```

{: codeblock}

## Enabling user metrics
{: #user_id}

User metrics allow you to see, for example, the number of unique users who have engaged with your assistant, or the average number of conversations per user over a given time interval on the [Overview page](logs_oview.html). User metrics are enabled by using a unique `User ID` parameter.

To specify the `User ID` for an utterance sent using the `/message` API, include the `user_id` property inside the metadata object in your [context ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#message){: new_window}, as in this example::

```
"context" : {
  "metadata" : {
       "user_id": "{UserID}"
  }
}
```
{: codeblock}
