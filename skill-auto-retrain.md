---

copyright:
  years: 2021
lastupdated: "2022-12-16"

subcollection: assistant

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

{{site.data.content.newlink}}

# Automatic retrain of old skills and workspaces
{: #skill-auto-retrain}

{{site.data.keyword.conversationshort}} was released as a service in July 2016. Since then, users have been creating and updating skills to meet their virtual assistant needs. Behind the scenes, {{site.data.keyword.conversationshort}} creates machine learning (ML) models to perform a variety of tasks on the user's behalf. 

The primary ML models deal with action recognition, intent classification, and entity detection. For example, the model might detect what a user intends when saying `I want to open a checking account`, and what type of account the user is talking about.

These ML models rely on a sophisticated infrastructure. There are many intricate components that are responsible for analyzing what the user has said, breaking down the user's input, and processing it so the ML model can more easily predict what the user is asking.

Since {{site.data.keyword.conversationshort}} was first released, the product team has been making continuous updates to the algorithms that generate these sophisticated ML models. Older models have continued to function while running in the context of newer algorithms. Historically, the behavior of these existing ML model did not change unless the skill was updated, at which point the skill was retrained and a new model generated to replace the older one. This meant that many older models never benefited from improvements in our ML algorithms.

{{site.data.keyword.conversationshort}} uses continuous retraining. The {{site.data.keyword.conversationshort}} service continually monitors all ML models, and automatically retrains those models that have not been retrained in the previous 6 months. {{site.data.keyword.conversationshort}} retrains using the selected algorithm version. If the version you had selected is no longer supported, {{site.data.keyword.conversationshort}} retrains using the version labeled as **Previous**. This means that your assistant will automatically have the supported technologies applied. Assistants that have been modified during the previous 6 months will not be affected. For more information, see [Algorithm version](/docs/assistant?topic=assistant-algorithm-version).

In most cases, this retraining will be seamless from an end-user point of view. The same inputs will result in the same actions, intents, and entities being detected. In some cases, the retraining might cause changes in accuracy.
{: note}

