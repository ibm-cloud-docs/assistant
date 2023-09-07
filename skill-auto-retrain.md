---

copyright:
  years: 2021, 2023
lastupdated: "2022-12-16"

subcollection: assistant

---

{{site.data.keyword.attribute-definition-list}}

Documentation for the **classic {{site.data.keyword.assistant_classic_short}}** experience has moved. For the most up-to-date version, see [Automatic retraining](/docs/watson-assistant?topic=watson-assistant-algorithm-version#algorithm-version-auto-retrain){: external} To see all documentation for the new {{site.data.keyword.conversationshort}}, please go [here](https://cloud.ibm.com/docs/watson-assistant){: external}.
{: attention}

# Automatic retrain of old skills and workspaces
{: #skill-auto-retrain}

{{site.data.keyword.assistant_classic_short}} was released as a service in July 2016. Since then, users have been creating and updating skills to meet their virtual assistant needs. Behind the scenes, {{site.data.keyword.assistant_classic_short}} creates machine learning (ML) models to perform a variety of tasks on the user's behalf. 

The primary ML models deal with action recognition, intent classification, and entity detection. For example, the model might detect what a user intends when saying `I want to open a checking account`, and what type of account the user is talking about.

These ML models rely on a sophisticated infrastructure. There are many intricate components that are responsible for analyzing what the user has said, breaking down the user's input, and processing it so the ML model can more easily predict what the user is asking.

Since {{site.data.keyword.assistant_classic_short}} was first released, the product team has been making continuous updates to the algorithms that generate these sophisticated ML models. Older models have continued to function while running in the context of newer algorithms. Historically, the behavior of these existing ML model did not change unless the skill was updated, at which point the skill was retrained and a new model generated to replace the older one. This meant that many older models never benefited from improvements in our ML algorithms.

{{site.data.keyword.assistant_classic_short}} uses continuous retraining. The {{site.data.keassistant_classic_shortnshort}} service continually monitors all ML models, and automatically retrains those models that have not been retrained in the previous 6 months. {{site.dassistant_classic_shortrsationshort}} retrains using the selected algorithm version. If the version you had selected is no longer supported, {{assistant_classic_short.assistant_classic_short}} retrains using the version labeled as **Previous**. This means that your assistant will automatically have the supported technologies applied. Assistants that have been modified during the previous 6 months will not be affected. For more information, see [Algorithm version](/docs/assistant?topic=assistant-algorithm-version).

In most cases, this retraining will be seamless from an end-user point of view. The same inputs will result in the same actions, intents, and entities being detected. In some cases, the retraining might cause changes in accuracy.
{: note}

