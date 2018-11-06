---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-29"

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

# Metrics overview

The Overview page provides a summary of the interactions between users and your assistant. You can view the amount of traffic for a given time period, as well as the intents and entities that were recognized most often in user conversations.
{: shortdesc}

Use the metrics to answer questions like:

* Which days had the largest or smallest numbers of conversations in the last month?
* What was the average number of conversations per week during the last month?
* Which intents appeared most often last week?
* Which entity values were recognized the most times during February?

To see metrics information, select **Overview** in the navigation bar.

  ![Overview page](images/oview.png)

## Controls
You can use the following controls to filter the information:

- *Intents* and *Entities* filters - Use either of these drop-down filters to show data for a specific intent or entity in your skill.

  **Important** - The intent and entities filters are populated by the intents and entities in the ***skill***, and not what is in the data source. If you have [selected a data source](logs.html#deploy_id) other than the skill, you might not see an intent or entity from your data source logs as an option in the filters, unless those intents and entities are also in the skill.

- *Refresh data* - Allows you to refresh the Overview page statistics immediately. The Overview page shows when the data that it displays was last updated. You can select **Refresh data** if you think that newer data might be available.

  The statistics represent external traffic (from users or API calls) that has interacted with your assistant; they do not include interactions from the *Try it out* pane in the tool.

- *Time period control* - Use this control to choose the period for which data is displayed. This control affects all data shown on the page: not just the number of conversations displayed in the graph, but also the statistics displayed along with the graph, and the lists of top intents and entities. 

  **Note**: The statistics can cover a longer time period than the period for which logs of conversations are retained.

  ![Time period control](images/oview-time.png)

  You can choose whether to view data for a single day, a week, a month, or a quarter. In each case, the data points on the graph adjust to an appropriate measurement period. For example, when viewing a graph for a day, the data is presented in hourly values, but when viewing a graph for a week, the data is shown by day. A week always runs from Sunday through Saturday. You cannot create custom time periods, such as a week that runs from Thursday to the following Wednesday, or a month that begins on any date other than the first.

  **NOTE**: If you choose a single day view, for example, the time shown for each conversation is localized to reflect the time zone of your browser. This may differ from the timestamp shown if you review the same conversation log via an API call; API log calls are always shown in UTC.

    ![Time period control](images/oview-time2.png)

## Graphs and statistics
Several statistical scorecards provide log data for your application:

* *Total conversations* - The total number of conversations between active users and your application, during the selected time period, as shown in the corresponding graph.

  **Note**: A single conversation is a set of messages consisting of the messages that an active user sends to your application, and the messages your application responds with.

  **Important**: A 'conversation' is considered to be *any* set of messages sent or received by an application/bot, so if the service starts by saying "Hi, how can I help you?", and then the user closes their browser without responding, that message is included in the total conversation count.

* *Avg. msg. per conversation* - The total messages received during the selected time period divided by the total conversations during the selected time period, as shown in the corresponding graph.
* *Max. conversations* - The maximum number of conversations for a single data point within the selected time period.
* *Weak understanding* - The number of individual messages with weak understanding. These messages are not classified by an intent, and do not contain any known entities. These can be useful in identifying potential dialog problems.

Detailed graphs provide additional information:

* *Total conversations* - The total number of conversations between active users and your application, during the selected time period.

  While viewing the ***Conversations*** graph, you can click on an individual data point to see the numeric value, as shown here:

  ![Single data point](images/oview-point.png)

* *Avg. msg. per conversation* - The total messages received during the selected time period divided by the total conversations during the selected time period.
* *Total messages* - The total number of messages received from active users over the selected time period.
* *Active users* - The number of unique users who have engaged with your application within the selected time period.
* *Avg. conversations per user* - The total conversations during the selected time period divided by the total unique users during the selected time period.

  **Note**: Statistics for *Active users* and *Avg. conversations per user* require a unique `user_id` parameter. See [Enabling user metrics](#user_id) for more information.

## Top Intents and Top Entities

You can also view the intents and entities that were recognized most often during the specified time period.

* *Top intents* - Intents are shown in a simple list. In addition to seeing the number of times an intent was recognized, you can select an intent to open the [User conversations](logs.html) page with the date range filtered to match the data you are viewing, and the intent filtered to match the selected intent.

* *Top entities* are also shown in a list. For each entity you can select from the **Values** column to see a list of the most common values that were identified for this entity during the time period. You can also select an entity to open the [User conversations](logs.html) page with the date range filtered to match the data you are viewing, and the entity filtered to match the selected entity.

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
curl -X POST -u "apikey:3Df... ...Y7Pc9"
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id={first-customer-ID};customer_id={second-customer-ID}'
 --data '{"input":{"text":"hello"}}' 'https:// gateway-us-south.watsonplatform.net/assistant/api/v1/workspaces/{workspaceID}/message?version=2018-09-20'
```
{: codeblock}

**Note**: The `customer_id` string cannot include the semicolon (`;`) or equal sign (`=`) characters. You are responsible for ensuring that each `Customer ID` parameter is unique across your customers.

To delete messages using `customer_id` values, see the [Information security](information-security.html#gdpr-wa) topic.
