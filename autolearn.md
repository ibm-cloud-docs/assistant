---

copyright:
  years: 2020
lastupdated: "2020-09-16"

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

# Empower your skill to learn automatically ![Beta](images/beta.png)
{: #autolearn}

Use autolearning to enable your skill to learn from interactions between your customers and your assistants.
{: shortdesc}

![Plus or Premium plan only](images/plus.png) This beta feature is available to Plus or Premium plan users with English-language dialog skills only.
{: note}

When customers interact with your assistant, they often make choices. If your underlying dialog skill pays attention, it can learn from these user decisions over time.

For example, when a customer asks a question that the assistant isn't sure it understands, the assistant often shows a list of topics to the customer and asks the customer to choose the right one. This process is called *disambiguation*. If, when a similar list of options is shown, customers most often click the same one (option #2, for example), then your skill can learn from that experience. It can learn that option #2 is the best answer to that type of question. And next time, it can list option #2 as the first choice, so customers can get to it more quickly. And, if the pattern persists over time, it can change its behavior even more. Instead of making the customer choose from a list of options at all, it can return option #2 as the answer immediately.

The advantage of enabling your skill to apply what it learns from observing customer choices is clear. As your skill learns over time, your customers get the best answer more often and in fewer clicks.

To learn more about how this feaature can benefit you, read the [Building the complete virtual assistant with Watson Assistant](https://www.ibm.com/blogs/watson/2020/05/building-the-complete-virtual-assistant-with-watson-assistant/){: external} blog post.

Before your skill can learn from customer behavior, it must observe customer behavior. You can give it real user conversation data to learn from by connecting your skill to a live assistant. When you connect to an assistant, the logs from conversations that occur between the assistant and your customers serve as the data source for observation. 

In fact, if you already connected your skill to a live assistant to use the assistant logs as the data source for intent or intent user example recommendations, then you can skip this step. The same assistant that you connect to for recommendations is used to observe customer behavior.

Autolearning uses observations of actions that are made by only your customers to improve only your skills.

## How autolearning works
{: #autolearn-how-it-works}

Autolearn gains insights from the following user action:

- The choice made from a list of disambiguation options that is displayed for an utterance
<!-- The choice made from a list of more options that is included with the response in web chat integrations-->

These customer interactions occur in the context of one of the product's built-in integrations, such as the web chat, or in a custom application. To say that your skill *observes* the choices that users make means that it analyzes logs of the exchanges after the interactions take place. It does not mean that the skill watches the clicks that users make within the client-facing app in real time.

When you connect a live assistant as the data source for recommendations, you enable observation. When you turn on autolearning, you put the observed insights to use to improve your skill, which results in a better customer experience.

## Enabling autolearning
{: #autolearn-task}

You can enable autolearning when the following conditions are met:

- Disambiguation is enabled for the skill.

  Disambiguation is enabled automatically for new skills.
  {: note}
- You have at least one deployed assistant that is actively interacting with customers and has accumulated log data that the skill can learn from.

<!--Autolearning is optimized for use with the built-in web chat integration. This integration, in particular, has a *more options* feature which increases the opportunities for users to make choices, and therefore for the skill to learn from them.
{: tip}-->

To enable autolearning, complete the following steps:

1.  From the Skills menu, click **Options**, and then click **Autolearning**.
1.  Click **Select assistant**, choose an assistant, and then click **Save**.

    Choose an assistant that is live, meaning it is deployed in a production environment and is actively engaging with customers.

    An assistant is selected already if you are using the logs from a live assistant to get intent and intent user example recommendations. The assistant that you connect your skill to for getting recommendations is automatically used. While you can add one dialog skill to more than one assistant, you can only connect one live assistant to a dialog skill to use its logs as a data source. Both features must use log data from the same assistant. For more information about the recommendations features, see [Get help defining intents](/docs/assistant?topic=assistant-intent-recommendations).
      {: note}

    To change the assistant that is used as the source for observation, click **Change assistant**, choose an assistant, and then click **Save**.

    Do not change the assistant without first considering the impact of this change. When you swap the assistant to observe, it changes the live assistant that is used as the data source for the recommendations feature also.
    {: important}

1.  Set the **Enable Autolearning** switch to **On**.

<!--## Advanced configuration
{: #autolearn-v1}

If you have an advanced use case where more than one assistant submits production message traffic for a skill, you still can enable autolearning. From the Autolearning page where you enable the feature, expand the **Advanced** section, and then select **Observe all messages**. When you do so, you indicate that you want your skill to observe and learn from every `POST` request that is sent to the `/message` API endpoint for this skill.

When you configure autolearning to use all messages, you must be sure to flag any requests that are not customer-generated that are sent to the service. Do not mix test utterances with legitimate, customer-generated utterances. You might run manual or automated tests of your skill, for example. You must prevent this canned data from skewing the insights that can otherwise be gained from analyzing choices that are made by real customers. 

Build your test framework in such a way that each test message is identified as a test message and does not feed the autolearning algorithm. To do so, include the `auto_learn.learn:false` property in each test request. For more information, see the [API reference](https://cloud.ibm.com/apidocs/assistant/assistant-v2#message){: external}.-->

##Tracking customer effort
{: #autolearn-track}

To track the impact that autolearning has over time, use the **Customer Effort Analysis Notebook**. The notebook analyzes a metric called *Customer Effort*, which captures the effort that your customers must expend to get an answer to a question or find the solution to a problem. 

Autolearning works in tandem with disambiguation. (For more information about disambiguation, see [Disambiguation](/docs/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).) The Customer Effort metric measures how your customers respond to the options that are presented by disambiguation. For example, if your customer chooses the third option in a list of choices, the effort expended is considered to be higher than the effort expended to choose the first option. Likewise, if a customer chooses **None of the above** to signify that none of the options address a need, then the effort metric is even higher. Use this notebook to plot the Customer Effort metric values graphically, so you have a visual indication of how the metric changes over time. You can also see related information such as disambiguation volume and which dialog nodes are most frequently included as disambiguation list options.

You can use the notebook from [GitHub](https://github.com/watson-developer-cloud/assistant-improve-recommendations-notebook/blob/master/notebook/Customer%20Effort%20Notebook.ipynb){: external}<!-- or use the notebook with Watson Studio-->.

For more information about this notebook and others that you can use to improve your assistant, see [Jupyter notebooks](https://cloud.ibm.com/docs/assistant?topic=assistant-logs-resources#logs-resources-jupyter-notebooks).

## Learning from your data
{: #autolearn-data}

Observations are made of only your customers` choices to improve only your skills. The observed insights are not reused by IBM or shared in any way.

This observed user choices data is separate from the log data for which metrics are displayed in the Analytics page. User conversation log data in all but Premium plan service instances is used by IBM for general service improvements. You can opt out of such use by specifying an opt-out header in your `/message` API requests. For more information, see [Opting out of logging](/docs/assistant?topic=assistant-information-security#information-security-log-opt-out).

You can limit how the service uses data in the following ways:

- To prevent IBM from using your log data for general {{site.data.keyword.conversationshort}} service improvements, you must specify an opt-out header.
- To prevent your own skill from applying what it learns by observing user choices to your skill, you must turn off the autolearning feature.
- To prevent your skill from observing user choices, disconnect the live assistant from the skill. 

  Don't disconnect a live assistant if its logs are being used as the source of intent and intent user example recommendations.
  {: tip}
