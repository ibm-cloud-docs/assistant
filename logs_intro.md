---

copyright:
  years: 2015, 2019
lastupdated: "2019-01-24"

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

# Improve your skill
{: #logs_intro}

The Improve component of {{site.data.keyword.conversationshort}} provides a history of conversations between users and a deployed assistant. You can use this history to improve how your assistants understand and respond to user requests.
{: shortdesc}

Use the tool to do the following things:

- View metrics that provide insights into the quality of the conversations. See [Metrics overview](logs_oview.html) for more information.
- Review the individual user messages from a conversation transcript. You can directly edit your intents and entities based on what you learn as you review individual user messages. A single user conversation typically consists of multiple messages. See [Learn from conversations](logs.html) for more information.

<!-- ### Querying data
Use the `/logs` API `filter` parameter to search an assistant log for specific user data. For example, to search for data specific to a `User ID` that matches `my_best_customer`, the query might be:

```
curl -X GET
 --user {username}:{password}
 --data 'https://gateway.watson.net/conversation/api/v1/workspaces/{workspaceID}/logs?version=2018-02-16&filter=(language::en,request.header.metadata.user_id::my_best_customer)'
```
{: codeblock}

See the [Filter query reference](filter-reference.html) for additional details. -->