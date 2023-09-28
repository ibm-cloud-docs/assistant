---

copyright:
  years: 2015, 2023
lastupdated: "2022-12-16"

subcollection: assistant

---

{{site.data.keyword.attribute-definition-list}}

Documentation for the **classic {{site.data.keyword.assistant_classic_short}}** experience has moved. For the most up-to-date version, see [Algorithm version and training](/docs/watson-assistant?topic=watson-assistant-algorithm-version){: external}.
{: attention}

# Algorithm version and training
{: #algorithm-version}

The **Algorithm version** setting allows you to choose which {{site.data.keyword.assistant_classic_short}} algorithm is used for training. Your assistant is trained when you update the content or settings, or through [automatic retraining](#algorithm-version-auto-retrain).
{: shortdesc}

There are three choices:
- **Beta**: Use this to preview and test what is coming. The capability in the beta version at any given time is likely to become a supported version later on. It's not recommended to use the beta version in a production deployment.
- **Latest**: The current supported version that's recommended for your live production assistant. 
- **Previous**: The last supported before the latest version. Support for this version ends when the next version is released.

To choose an algorithm version for actions:

1. On the **Actions** page, click **Global settings** ![Gear icon](../../icons/settings.svg).

1. Click the **Algorithm Version** tab.

1. Choose a version, then click **Save**.

To choose an algorithm version for dialog:

1. On the **Dialog** page, open the **Options** section.

1. Click **Algorithm Version**.

1. Choose a version.

The latest and previous versions have date labels such as **Latest (01-Jun-2022)** or **Previous (01-Jan-2022)**. See the [release notes](/docs/watson-assistant?topic=watson-assistant-watson-assistant-release-notes){: external} for details about each algorithm version release.

Algorithm version choices are currently available for Arabic, Chinese (Simplified), Chinese (Traditional), Czech, Dutch, English, French, German, Japanese, Korean, Italian, Portuguese, and Spanish. The universal language model uses a default algorithm.

## Automatic retraining
{: #algorithm-version-auto-retrain}

[IBM Cloud]{: tag-ibm-cloud}

{{site.data.keyword.assistant_classic_short}} was released as a service in July 2016. Since then, users have been creating and updating skills to meet their virtual assistant needs. Behind the scenes, {{site.data.keyword.assistant_classic_short}} creates machine learning (ML) models to perform a variety of tasks on the user's behalf. 

The primary ML models deal with action recognition, intent classification, and entity detection. For example, the model might detect what a user intends when saying `I want to open a checking account`, and what type of account the user is talking about.

These ML models rely on a sophisticated infrastructure. There are many intricate components that are responsible for analyzing what the user has said, breaking down the user's input, and processing it so the ML model can more easily predict what the user is asking.

Since {{site.data.keyword.assistant_classic_short}} was first released, the product team has been making continuous updates to the algorithms that generate these sophisticated ML models. Older models have continued to function while running in the context of newer algorithms. Historically, the behavior of these existing ML model did not change unless the skill was updated, at which point the skill was retrained and a new model generated to replace the older one. This meant that many older models never benefited from improvements in our ML algorithms.

{{site.data.keyword.assistant_classic_short}} uses continuous retraining. The {{site.data.keyword.assistant_classic_short}} service continually monitors all ML models, and automatically retrains those models that have not been retrained in the previous 6 months. {{site.data.keyword.assistant_classic_short}} retrains using the selected algorithm version. If the version you had selected is no longer supported, {{site.data.keassistant_classic_shortnshort}} retrains using the version labeled as **Previous**. This means that your assistant will automatically have the supported technologies applied. Assistants that have been modified during the previous 6 months will not be affected.

In most cases, this retraining will be seamless from an end-user point of view. The same inputs will result in the same actions, intents, and entities being detected. In some cases, the retraining might cause changes in accuracy.
{: note}

## Instructions for {{site.data.keyword.assistant_classic_short}} for {{site.data.keyword.icp4dfull_notm}}
{: #algorithm-version-cp4d}

[IBM Cloud Pak for Data]{: tag-cp4d}

When upgrading your instance of {{site.data.keyword.assistant_classic_short}} for {{site.data.keyword.icp4dfull_notm}}, as long as your existing models have been trained using an algorithm version that is still supported, your models will not be retrained during or after the upgrade.

New algorithm versions will be included in major releases (for example, 4.0.0 or 5.0.0) or minor releases (for example, 4.5.0 or 4.6.0). A monthly release might include a new algorithm version if there is more than 6 months between major or minor releases. Each algorithm version supports 2 major/minor releases or a maximum time of 12 months, whichever is first. For more information, see the [{{site.data.keyword.icp4dfull_notm}} Software Support Lifecycle Addendum](https://www.ibm.com/support/pages/ibm-cloud-pak-data-software-support-lifecycle-addendum){: external}

Each new release includes full support for the version listed as **Latest** in the most recent prior release. This version is then labeled as **Previous** after you upgrade. In addition, each new release will support running models that were trained on the version prior to that so that upgrading won't impact your runtime. For example, if you upgrade from {{site.data.keyword.icp4dfull_notm}} 4.6 to 4.7, and were using **Latest (01-Jun-2022)** that version becomes listed as **Previous (01 June 2022)** and remains your selected version.

### Automatic retraining after upgrading
{: #algorithm-version-cp4d-auto-retrain}

After your {{site.data.keyword.assistant_classic_short}} for {{site.data.keyword.icp4dfull_notm}} upgrade is complete, {{site.data.keassistant_classic_shortnshort}} performs automatic retraining for any assistant models that were trained using a version that is no longer supported. In this case, {{site.dassistant_classic_shortrsationshort}} automatically retrains your assistant to the **Latest** version.  This automatic retraining is required to assure your ability to run your trained models in your next upgrade.

### Best practices
{: #algorithm-version-cp4d-best-practices}

It's recommended to use the **Latest** version in your production deployment of {{site.data.keyword.assistant_classic_short}} for {{site.data.keyword.icp4dfull_notm}}. This is the default for newly-created assistants. During an upgrade, your settings don't automatically switch existing assistants to use the latest version. If prior to your upgrade you had selected **Latest**, your settings continue to use that version, now labeled as **Previous**. After you upgrade, it's recommended you choose **Latest** and run basic regression tests. 

IBM performs robust testing on a variety of data sets to minimize impacts on existing assistants. But given the nature of machine learning models and the nuance and subtlety of natural languages processing, you may find some discrepancies from version to version. If you find a major issue through your tests, you have the ability to switch your settings and use **Previous** to return to the prior behavior.  In this event, we recommend you contact IBM and provide details of your test so that that IBM can support you in the steps to resolve the problem.

It's also recommended that you try the **Beta** version in one of your test systems after you upgrade. This gives you early visibility to changes that are likely to be delivered in a future version, and will reduce the probability of negative impacts to your production systems. IBM values both positive and negative feedback from customers who use Beta. You will have the opportunity to shape how the algorithms function before the version is promoted to **Latest** in a future version. If you choose **Beta**, your assistant always trains on the most current beta version. 
