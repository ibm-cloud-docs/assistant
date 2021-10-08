---

copyright:
  years: 2015, 2021
lastupdated: "2021-03-03"

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

{{site.data.content.newlink}}

# Advanced analysis and log-related tasks
{: #logs-resources}

Learn about APIs and other tools you can use to access and analyze log data.
{: shortdesc}

## Using Jupyter notebooks for analysis
{: #logs-resources-jupyter-notebooks}

IBM created Jupyter notebooks that you can use to analyze the behavior or your assistant. A Jupyter notebook is a web-based environment for interactive computing. You can run small pieces of code that process your data, and you can immediately view the results of your computation.

You can use the notebooks with English-language skills only.
{: note}

### Analysis notebooks
{: #logs-resources-jupyter-logs}

There is a set of analysis notebooks that you can use with standard Python tools and a set that is designed for optimal use with {{site.data.keyword.DSX_full}}. 

{{site.data.keyword.DSX_short}} is a product that provides an environment in which you can pick and choose the tools you need to analyze and visualize data, to cleanse and shape data, to ingest streaming data, or to create, train, and deploy machine learning models. See the [product documentation](https://dataplatform.cloud.ibm.com/docs/content/wsj/getting-started/welcome-main.html){: external} for more details.

The [Watson Assistant Continuous Improvement Best Practices Guide](https://github.com/watson-developer-cloud/assistant-improve-recommendations-notebook/raw/master/notebook/IBM%20Watson%20Assistant%20Continuous%20Improvement%20Best%20Practices.pdf){: external} describes how to get the most out of these notebooks.

### Using the notebooks with {{site.data.keyword.DSX}}
{: #logs-resources-notebooks-studio}

The following notebooks are available:

- [Dialog skill analysis for Watson Assistant](https://dataplatform.cloud.ibm.com/exchange/public/entry/view/4d77701840fcb2f21587e39fdb887049){: external}
- [Measure Watson Assistant Performance](https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f635e568){: external}.
- [Analyze Watson Assistant Effectiveness](https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f636921c){: external}
- [Dialog Flow Analysis for Watson Assistant](https://dataplatform.cloud.ibm.com/exchange/public/entry/view/013c690997e27f3a8d9133265327a9e5){: external}

If you choose to use the notebooks that are designed for use with {{site.data.keyword.DSX}}, the steps are roughly these:

1.  Create a {{site.data.keyword.DSX}} account, [create a project](https://dataplatform.cloud.ibm.com/docs/content/wsj/getting-started/projects.html){: external}, and add a Cloud Object Storage account to it.
1.  From the {{site.data.keyword.DSX}} community, choose a notebook.

    Early in the development process, use the **Dialog skill analysis for Watson Assistant** notebook to help you get started. It offers the following types of insights:

    - Examines the terms that are correlated with each intent in your training data to find anomalies that might identify problems that you can investigate further.
    - Uses a blind test set that you provide to calculate performance on statistical metrics like Accuracy, Precision, Recall & F1. 
    - Offers advanced features that you can use to find the causes of common issues such as why some sentences are often misidentified.

    To learn more about how this notebook can help you improve your dialog, read this [Medium.com blog post](https://medium.com/ibm-watson/announcing-dialog-skill-analysis-for-watson-assistant-83cdfb968178){: external}.

1.  After you deploy a version of the assistant, and have some conversation log data collected, run the **Measure Watson Assistant Performance** notebook.

1.  Follow the step-by-step instructions provided with the notebooks to analyze a subset of the dialog exchanges from the logs.

    Run the following notebook first:

    - **Measure**: Gathers metrics that focus on coverage (how often the assistant is confident enough to respond to users) and effectiveness (when the assistant does respond, whether the responses are satisfying user needs).

    The insights are visualized in ways that make it easier to understand areas for improvement in your assistant.
1.  Export a sample set of the logs from ineffective conversations, and then analyze and annotate them.

    For example, indicate whether a response is correct. If correct, mark whether it is helpful. If a response is incorrect, then identify the root cause, the wrong intent or entity was detected, for example, or the wrong dialog node was triggered. After identifying the root cause, indicate what the correct choice would have been.
1.  Feed the annotated spreadsheet to the **Analyze Watson Assistant Effectiveness** notebook.

    - **Effectiveness**: Performs a deeper analysis of your logs to help you understand the steps you can take to improve your assistant.
1.  Use the **Dialog Flow Analysis for Watson Assistant** notebook to review your dialog. The notebook can help you pinpoint the dialog nodes where customers most frequently abandon the conversation. 

    For more information about how this notebook can help you analyze and assess abandonment, read this [Medium.com blog post](https://medium.com/ibm-watson/do-you-know-where-and-why-users-drop-off-the-conversation-6246e99baddc){: external}.

This process helps you to understand the steps you can take to improve your assistant.

### Using the notebooks with standard Python tools
{: #logs-resources-notebooks-python}

If you choose to use standard Python tools to run the notebooks, you can get the notebooks from GitHub.

- [Dialog Skill Analysis for Watson Assistant](https://github.com/watson-developer-cloud/assistant-dialog-skill-analysis){: external}
- [Watson Assistant Recommendation notebooks (Measure and Analyze Effectiveness)](https://github.com/watson-developer-cloud/assistant-improve-recommendations-notebook){: external}
- [Watson Assistant Dialog Flow Analysis notebook](https://github.com/watson-developer-cloud/assistant-dialog-flow-analysis){: external}

Again, the [Watson Assistant Continuous Improvement Best Practices Guide](https://github.com/watson-developer-cloud/assistant-improve-recommendations-notebook/raw/master/notebook/IBM%20Watson%20Assistant%20Continuous%20Improvement%20Best%20Practices.pdf){: external} outlines which notebook to use at each stage of your improvement process.

## Using the logs API
{: #logs-resources-api}

You can use the `/logs` API to list events from the transcripts of conversations that occurred between your users and your assistant. For conversations created by using the v2 `/message` API, use the instance-level endpoint to [list log events in all workspaces](https://cloud.ibm.com/apidocs/assistant/assistant-v1#listalllogs){: external}, and then filter by Assistant ID. For more information about filtering logs, see [Filter query reference](/docs/assistant?topic=assistant-filter-reference).

The API logs messages that are exchanged in conversations that are defined by a dialog skill only.
{: important}

The number of days that logs are stored differs by service plan type. See [Log limits](/docs/assistant?topic=assistant-logs#logs-limits) for details.

For a Python script you can run to export logs and convert them to CSV format, download the `export_logs_py.py` file from the [Watson Assistant GitHub)](https://github.com/watson-developer-cloud/community/blob/master/watson-assistant/export_logs_py.py){: external} repository.

## Understanding logs-related terminology
{: #logs-resources-terminology}

First, review the definitions of terms that are associated with {{site.data.keyword.conversationshort}} logs:

- ***Assistant***: An application - sometimes referred to as a 'chat bot' - that implements your {{site.data.keyword.conversationshort}} content.
- ***Assistant ID***: The unique identifier of an assistant.
- ***Conversation***: A set of messages consisting of the messages that an individual user sends to your assistant, and the messages your assistant sends back.
- ***Conversation ID***: Unique identifier that is added to individual message calls to link related message exchanges together. App developers using the V1 version of the {{site.data.keyword.conversationshort}} API add this value to the message calls in a conversation by including the ID in the metadata of the context object.
- ***Customer ID***: A unique ID that can be used to label customer data such that it can be subsequently deleted if the customer requests the removal of their data.
- ***Deployment ID***: A unique label that app developers using the V1 version of the {{site.data.keyword.conversationshort}} API pass with each user message to help identify the deployment environment that produced the message.
- ***Instance***: Your deployment of {{site.data.keyword.conversationshort}}, accessible with unique credentials. A {{site.data.keyword.conversationshort}} instance might contain multiple assistants.
- ***Message***: A message is a single utterance a user sends to the assistant.
- ***Skill ID***: The unique identifier of a skill.
- ***User***: A user is anyone who interacts with your assistant; often these are your customers.
- ***User ID***: A unique label that is used to track the level of service usage of a specific user.
- ***Workspace ID***: The unique identifier of a workspace. Although any workspaces that you created before November 9 are shown as skills in the product user interface, a skill and a workspace are not the same thing. A skill is effectively a wrapper for a V1 workspace.

**Important**: The **User ID** property is *not* equivalent to the **Customer ID** property, though both can be passed with the message. The **User ID** field is used to track levels of usage for billing purposes, whereas the **Customer ID** field is used to support the labeling and subsequent deletion of messages that are associated with end users. Customer ID is used consistently across all Watson services and is specified in the `X-Watson-Metadata` header. User ID is used exclusively by the {{site.data.keyword.conversationshort}} service and is passed in the context object of each /message API call.

## Associating message data with a user for deletion
{: #logs-resources-customer_id}

There might come a time when you want to completely remove a set of your user's data from a {{site.data.keyword.conversationshort}} instance. When the delete feature is used, then the Overview metrics will no longer reflect those deleted messages; for example, they will have fewer Total Conversations.

### Before you begin
{: #logs-resources-delete-customer-id-prereqs}

To delete messages for one or more individuals, you first need to associate a message with a unique **Customer ID** for each individual. To specify the **Customer ID** for any message sent using the `/message` API, include the `X-Watson-Metadata: customer_id` property in your header. You can pass multiple **Customer ID** entries with semicolon separated `field=value` pairs, using `customer_id`, as in the following example:

```sh
curl -X POST -u "apikey:3Df... ...Y7Pc9" \
 --header \
   "Content-Type: application/json" \
   "X-Watson-Metadata: customer_id={first-customer-ID};customer_id={second-customer-ID}" \
 --data "{\"input\":{\"text\":\"hello\"}}" \
 "{url}/v2/assistants/{assistant_id}/sessions/{session_id}/message?version=2019-02-28"
```
{: pre}

where {url} is the appropriate URL for your instance. For more details, see [Service endpoint](https://cloud.ibm.com/apidocs/assistant/assistant-v2#service-endpoint){: external}
}.

The `customer_id` string cannot include the semicolon (`;`) or equal sign (`=`) characters. You are responsible for ensuring that each `Customer ID` parameter is unique across your customers.
{: note}

To delete messages using `customer_id` values, see the [Information security](/docs/assistant?topic=assistant-information-security#information-security-gdpr-wa) topic.

