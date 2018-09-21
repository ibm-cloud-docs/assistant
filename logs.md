---

copyright:
  years: 2015, 2018
lastupdated: "2018-09-20"

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

The Improve component of {{site.data.keyword.conversationshort}} provides a history of conversations between users and your deployed assistant. You can use this history to improve your assistant's ability to understand user inputs.
{: shortdesc}

Sections of the **Improve** panel:

* [Overview](logs_oview.html): A summary of conversations that users have had with your assistant.
* [User conversations](logs_convo.html): A list of individual user messages. You can directly edit your intents and entities based on insights you glean while reviewing individual user messages. **Note**: A single user conversation may consist of multiple messages.
* [Recommendations](logs_recommend.html) : Ways to improve your system. Available only to Premium users.

To understand how to use message data to make improvements across assistants, it is helpful to review the following definitions associated with the {{site.data.keyword.conversationshort}} service:

* ***Instance***: Your deployment of {{site.data.keyword.conversationshort}}, accessible with unique credentials. A {{site.data.keyword.conversationshort}} instance might contain multiple assistants.
* ***Assistant***: An application - sometimes referred to as a 'chat bot' - that implements your {{site.data.keyword.conversationshort}} content.
* ***User***: A user is anyone who interacts with your assistant; often these are your customers.
* ***User ID***: A unique label that is used to track data for a specific user.
* ***Message***: A message is a single utterance a user sends to the assistant.
* ***Conversation***: A set of messages consisting of the messages that an individual user sends to your assistant, and the messages your assistant sends back.
* ***Conversation ID***: Unique identifier that is added to individual message calls to link related message exchanges together. App developers using the V1 version of the service API add this value to the message calls in a conversation by including the ID in the metadata of the context object.
* ***Workspace ID***: The unique identifier of a dialog skill.
* ***Deployment ID***: A unique label that app developers using the V1 version of the service API pass with each user message to help identify the deployment environment that produced the message.
* ***Customer ID***: A unique label that can be used to delete data for a specific customer.

**Important**: The **User ID** property is *not* equivalent to the **Customer ID** property, though both may be passed in the `/message` API header. The **User ID** field is used to track levels of usage, whereas the **Customer ID** field is used to support subsequent deletion of messages associated with end users. To understand the difference between the two, as an example, if a customer service representative (CSR) interacts with a {{site.data.keyword.conversationshort}} assistant in reference to a client, then **User ID** and **Customer ID** may both be set, but with different values. In this case, the direct interaction with {{site.data.keyword.conversationshort}} is through the CSR, so you would probably want to set `user_id=CSR`. However, since the CSR is relaying information about their client, you would probably set `customer_id=client`.

## Enabling user metrics
{: #user_id}

User metrics allow you to see, for example, the number of unique users who have engaged with your assistant, or the average number of conversations per user over a given time interval on the [Overview page](logs_oview.html). User metrics are enabled by using a unique `User ID` parameter.

To specify the `User ID` for a message sent using the `/message` API, include the `user_id` property inside the metadata object in your [context ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#message){: new_window}, as in this example::

```
"context" : {
  "metadata" : {
       "user_id": "{UserID}"
  }
}
```
{: codeblock}

## Associating message data with a user for deletion
{: #customer_id}

There might come a time when you want to completely remove a set of your user's data from a {{site.data.keyword.conversationshort}} instance. When the delete feature is used, then the Overview metrics will no longer reflect those deleted messages; for example, they will have fewer Total Conversations.

### Before you begin
To delete messages for one or more individuals, you first need to associate a message with a unique **Customer ID** for each individual. To specify the **Customer ID** for any message sent using the `/message` API, include the `X-Watson-Metadata: customer_id` property in your header. You can pass multiple **Customer ID** entries with semicolon separated `field=value` pairs, using `customer_id`, as in the following example:

```
curl -X POST
 --user {username}:{password}
 --header
   'Content-Type: application/json'
   'Accept: application/json'
   'X-Watson-Metadata: customer_id={first-customer-ID};customer_id={second-customer-ID}'
 --data '{"input":{"text":"hello"}}' 'https://gateway.watson.net/conversation/api/v1/workspaces/{workspaceID}/message?version=2017-05-26'
```
{: codeblock}

**Note**: The `customer_id` string cannot include the semicolon (`;`) or equal sign (`=`) characters. You are responsible for ensuring that each `Customer ID` parameter is unique across your customers.

To delete messages using `customer_id` values, see the [Information security](information-security.html#gdpr-wa) topic.
