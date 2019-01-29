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

# Advanced tasks
{: #logs-resources}

Learn about APIs and other tools you can use to access and analyze log data.
{: shortdesc}

## API
{: #api}

You can use the `/logs` API to list events from the transcripts of conversations that occured between your users and your assistant. For detailed API reference documentation, see [List log events ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/apidocs/assistant#list-log-events-in-a-workspace).

The number of days that logs are stored differs by service plan type. See [Log limits](logs.html#log-limits) for details.

For a Python script you can run to export logs and convert them to CSV format, download the `export_logs.py` file from the [Watson Assistant GitHub ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud/community/blob/master/watson-assistant/export_logs.py) repository.

## Logs-related terminology

First, review the definitions of terms that are associated with {{site.data.keyword.conversationshort}} logs:

- ***Assistant***: An application - sometimes referred to as a 'chat bot' - that implements your {{site.data.keyword.conversationshort}} content.
- ***Conversation***: A set of messages consisting of the messages that an individual user sends to your assistant, and the messages your assistant sends back.
- ***Conversation ID***: Unique identifier that is added to individual message calls to link related message exchanges together. App developers using the V1 version of the service API add this value to the message calls in a conversation by including the ID in the metadata of the context object.
- ***Customer ID***: A unique ID that can be used to label customer data such that it can be subsequently deleted if the customer requests the removal of their data.
- ***Deployment ID***: A unique label that app developers using the V1 version of the service API pass with each user message to help identify the deployment environment that produced the message.
- ***Instance***: Your deployment of {{site.data.keyword.conversationshort}}, accessible with unique credentials. A {{site.data.keyword.conversationshort}} instance might contain multiple assistants.
- ***Message***: A message is a single utterance a user sends to the assistant.
- ***Skill ID***: The unique identifier of a skill.
- ***User***: A user is anyone who interacts with your assistant; often these are your customers.
- ***User ID***: A unique label that is used to track the level of service usage of a specific user.
- ***Workspace ID***: The unique identifier of a workspace. Although any workspaces that you created before November 9 are shown as skills in the tool, a skill and a workspace are not the same thing. A skill is effectively a wrapper for a V1 workspace.

**Important**: The **User ID** property is *not* equivalent to the **Customer ID** property, though both can be passed to the service. The **User ID** field is used to track levels of usage for billing purposes, whereas the **Customer ID** field is used to support the labeling and subsequent deletion of messages that are associated with end users. Customer ID is used consistently across all Watson services and is specified in the `X-Watson-Metadata` header. User ID is used exclusively by the {{site.data.keyword.conversationshort}} service and is passed in the context object of each /message API call.

## Enabling user metrics
{: #user_id}

User metrics allow you to see, for example, the number of unique users who have engaged with your assistant, or the average number of conversations per user over a given time interval on the [Overview page](logs_oview.html). User metrics are enabled by using a unique `User ID` parameter.

To specify the `User ID` for a message sent using the `/message` API, include the `user_id` property inside the metadata object in your [context ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/apidocs/assistant?curl=#get-response-to-user-input){: new_window}, as in this example::

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
{: #delete-customer-id-prereqs}

To delete messages for one or more individuals, you first need to associate a message with a unique **Customer ID** for each individual. To specify the **Customer ID** for any message sent using the `/message` API, include the `X-Watson-Metadata: customer_id` property in your header. You can pass multiple **Customer ID** entries with semicolon separated `field=value` pairs, using `customer_id`, as in the following example:

```
curl -X POST -u "apikey:3Df... ...Y7Pc9"
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id={first-customer-ID};customer_id={second-customer-ID}'
 --data '{"input":{"text":"hello"}}' 'https:// gateway-us-south.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

The `customer_id` string cannot include the semicolon (`;`) or equal sign (`=`) characters. You are responsible for ensuring that each `Customer ID` parameter is unique across your customers.
{: note}

To delete messages using `customer_id` values, see the [Information security](information-security.html#gdpr-wa) topic.

## Jupyter notebooks
{: jupyter-notebooks}

IBM created Jupyter notebooks that you can use to analyze your log data in more detail. A Jupyter notebook is a web-based environment for interactive computing. You can run small pieces of code that process your data, and you can immediately view the results of your computation.

There is a set of notebooks that you can use with standard Python tools and a set that is designed for optimal use with {{site.data.keyword.DSX_full}}. {{site.data.keyword.DSX_short}} is a product that provides an environment in which you can pick and choose the tools you need to analyze and visualize data, to cleanse and shape data, to ingest streaming data, or to create, train, and deploy machine learning models. See the [product documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/welcome-main.html){:new_window} for more details.

The following notebooks are available:

- **Measure**: Gathers metrics that focus on coverage (how often the assistant is confident enough to respond to users) and effectiveness (when the assistant does respond, whether the the responses are satisying user needs).

- **Effectiveness**: Performs a deeper analysis of your logs to help you understand the steps you can take to improve your assistant.

The [Watson Assistant Continuous Improvement Best Practices Guide ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/common/ssi/cgi-bin/ssialias?htmlfid=54022554USEN&) describes how to get the most out of the notebooks.

### Using the notebooks with {{site.data.keyword.DSX}}
{: #notebook-steps}

If you choose to use the notebooks that are designed for use with {{site.data.keyword.DSX}}, at a high level, the steps are:

1.  Create a {{site.data.keyword.DSX}} account, [create a project ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://dataplatform.cloud.ibm.com/docs/content/getting-started/projects.html?context=analytics){:new_window}, and add a Cloud Object Storage account to it.
1.  Give Object Storage access to your service instance logs from conversations that users have had with your assistant (by providing the workspace id and credentials).
1.  From the {{site.data.keyword.DSX}} community, get the [Measure Watson Assistant Performance notebook ![External link icon](../../icons/launch-glyph.svg "External link icon")]( https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f635e568).
1   Follow the step-by-step instructions provided with the notebook to analyze a subset of the dialog exchanges from the logs.

    The insights are visualized in ways that make it easier to understand the assistant's coverage and effectiveness.
1.  Export a sample set of the logs underlying visualizations from ineffective conversations, and then analyze and annotate them.

    For example, indicate whether a response is correct. If correct, mark whether it is helpful. If a response is incorrect, then identify the root cause, the wrong intent or entity was detected, for example, or the wrong dialog node was triggered. After identifying the root cause, indicate what the correct choice would have been.
1.  Feed the annotated spreadsheet to the [Analyze Watson Assistant Effectiveness notebook](https://dataplatform.cloud.ibm.com/exchange/public/entry/view/133dfc4cd1480bbe4eaa78d3f636921c).

This process helps you to understand the steps you can take to improve your assistant. There is no way to automatically apply what you learn back to your service instance. Keep track of any changes you make to improve the system, so you can subsequently apply them to the training data of your dialog skill directly.

### Using the notebooks with standard Python tools

If you choose to use standard Python tools to run the notebooks, you can get the notebooks from the [IBM GitHub repository](https://github.com/watson-developer-cloud/assistant-improve-recommendations-notebook/tree/master/notebook). Be sure to run them in the following order:

1.  Measure Notebook.jpynb
1.  Effectiveness Notebook.jpynb
