---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-15"

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

# Improve your skill
{: #logs_intro}

The Improve component of {{site.data.keyword.conversationshort}} provides a history of conversations between users and your deployed assistant. You can use this history to improve your assistant's ability to understand user inputs.
{: shortdesc}

Sections of the **Improve** panel:

* [Overview](logs_oview.html): A summary of conversations that users have had with your assistant.
* [User conversations](logs.html): A list of individual user messages. You can directly edit your intents and entities based on insights you glean while reviewing individual user messages. **Note**: A single user conversation may consist of multiple messages.

To understand how to use message data to make improvements across assistants, it is helpful to review the following definitions associated with the {{site.data.keyword.conversationshort}} service:

- ***Assistant***: An application - sometimes referred to as a 'chat bot' - that implements your {{site.data.keyword.conversationshort}} content.
- ***Conversation***: A set of messages consisting of the messages that an individual user sends to your assistant, and the messages your assistant sends back.
- ***Conversation ID***: Unique identifier that is added to individual message calls to link related message exchanges together. App developers using the V1 version of the service API add this value to the message calls in a conversation by including the ID in the metadata of the context object.
- ***Customer ID***: A unique label that can be used to identify and therefor support the deletion of data associated with a specific customer should the customer request it.
- ***Deployment ID***: A unique label that app developers using the V1 version of the service API pass with each user message to help identify the deployment environment that produced the message.

- ***Instance***: Your deployment of {{site.data.keyword.conversationshort}}, accessible with unique credentials. A {{site.data.keyword.conversationshort}} instance might contain multiple assistants.
- ***Message***: A message is a single utterance a user sends to the assistant.
- ***Skill ID***: The unique identifier of a skill.
- ***User***: A user is anyone who interacts with your assistant; often these are your customers.
- ***User ID***: A unique label that is used to track the level of service usage of a specific user.
- ***Workspace ID***: The unique identifier of a workspace. Although any workspaces that you created before November 9 are shown as skills skills in the tool, a skill and a workspace are not the same thing. A skill is effectively a wrapper for a V1 workspace.

**Important**: The **User ID** property is *not* equivalent to the **Customer ID** property, though both can be passed to the service. The **User ID** field is used to track levels of usage for billing purposes, whereas the **Customer ID** field is used to support the labeling and subsequent deletion of messages that are associated with end users. Customer ID is used consistently across all Watson services and is specified in the `X-Watson-Metadata` header. User ID is used exclusively by the {{site.data.keyword.conversationshort}} service and is passed in the context object of each /message API call.

<!-- ### Querying data
Use the `/logs` API `filter` parameter to search an assistant log for specific user data. For example, to search for data specific to a `User ID` that matches `my_best_customer`, the query might be:

```
curl -X GET
 --user {username}:{password}
 --data 'https://gateway.watson.net/conversation/api/v1/workspaces/{workspaceID}/logs?version=2018-02-16&filter=(language::en,request.header.metadata.user_id::my_best_customer)'
```
{: codeblock}

See the [Filter query reference](filter-reference.html) for additional details. -->