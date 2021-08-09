---

copyright:
  years: 2021
lastupdated: "2021-08-10"

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

# Automatic retrain of old skills and workspaces
{: #skill-auto-retrain}

IBM Watson Assistant was released as a service in July 2016. Since then, users have been creating and updating skills to meet their virtual assistant needs. Behind the scenes, Watson Assistant creates machine learning (ML) models to perform a variety of tasks on the user's behalf. 

The primary ML models deal with intent classification and entity detection, for example, what did the user intend when they said `I want to open a checking account`, and what type of account was the user talking about. 

These ML models rely on a sophisticated infrastructure in order to operate. There are many intricate components that are responsible for analyzing what the user has said, breaking down the user's input and processing it such that the ML model can more easily predict what it is the user is asking.

Since Watson Assistant was first released, the product team has been making continuous updates to the algorithms that generate these sophisticated ML models. As the code has evolved we have been mindful to ensure that older models continue to function while running in the context of the newer code. Over time this adds a large maintenance burden, and also results in older models not being able to automatically take advantage of the latest ML advancements. Historically, once a ML model was created in Watson Assistant, its behavior would not change without the customer making a change to their skill, at which point the skill is trained and a new model is generated, replacing the older one. One of the consequences of this approach is that older models never benefit from improvements in our ML algorithms, without the customer explicitly retraining the skill.

As of August 23, 2021, Watson Assistant will enable continuous retraining. The Watson Assistant service will continually monitor all ML models, and will automatically retrain those models that have not been retrained within 6 months of the time the retrain job runs. 

Each day, Watson Assistant will scan all ML models, and those that have not been trained within the prior 6 months will automatically be retrained, which means that your skill will automatically have the latest technologies applied. In the majority of cases this retraining will be seamless from an end-user point of view. The same inputs will result in the same intents and entities being detected. In a small subset of cases, the retraining may cause a change in accuracy. The continuous retraining job will only impact skills that have not been modified during the prior six months. Recently modified skills will not be retrained.

