---

copyright:
  years: 2020, 2021
lastupdated: "2021-01-26"

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
{:table: .aria-labeledby="caption"}

# Choosing a conversational skill
{: #skills-choose}

Choose the right type of conversational skill for your use case. 
{: shortdesc}

When you build the flow of conversation between your assistant and your customers, there are two skill types to choose from:

- [Actions skill](#skills-choose-actions)
- [Dialog skill](#skills-choose-dialog)

When you want your assistant to get its answer from your existing help content, use a *search skill*. The search skill can be added to an assistant along with a conversational flow skill. For more information, see [Creating a search skill](/docs/assistant?topic=assistant-skill-search-add).

## Actions skill ![Beta](images/beta.png)
{: #skills-choose-actions}

The actions skill is the best choice when you want to approach the assistant with a focus on content. The actions skill offers the following benefits:

- The words that your assistant says can be authored by the people in your organization who have expertise in customer care, not by your developers. The process of creating a conversational flow is easier. With a simplified process, anyone, even those without machine learning or programming knowledge, can build a dialog.
- Provides better visibility into the customer's interaction and satisfaction with the assistant. With actions, you can more easily track user progress through a task and identify where users hit snags in the process, because each task is discrete and has a clear beginning and ending.
- The conversation designer doesn't have to manage data that is collected from the user during the conversation. By default, information that your assistant collects is stored and available for the duration of the current action only. No extra steps are required to delete saved data or reset the conversation. But you can choose to store certain types of information, such as the customer's name, for the duration of a conversation if you want.
- Multiple people can work in the same skill at the same time, because each person can work with a separate, self-contained action. The order of actions within a conversation doesn't matter. Only the order of steps within an action matters. And the action author can use drag and drop to reorganize the steps in the action for optimal flow.

## Dialog skill
{: #skills-choose-dialog}

The dialog skill is the best choice when you want greater control over the logic of the conversational flow. The dialog skill editor exposes more of the underlying artifacts (such as intents and entities) that are used to build the AI models. The Dialog page represents the dialog flow with an if-then-else style structure that might be familiar to developers, but not to content designers or customer-care experts.

## How actions skills are different from dialog skills
{: #skills-choose-comparison}

If you are already familiar with dialog skill, learn more about how the actions skill compares.

Over time, the actions skill will have greater feature parity with the dialog skill. The following table describes feature support in actions and dialog skills right now.

| Feature | Actions skill | Dialog skill |
|---------|---------------|--------------|
| Keep track of context | ![checkmark icon](../../icons/checkmark-icon.svg) | ![checkmark icon](../../icons/checkmark-icon.svg) |
| Automatic reset of context | ![checkmark icon](../../icons/checkmark-icon.svg) | |
| @sys-number detection | ![checkmark icon](../../icons/checkmark-icon.svg) | ![checkmark icon](../../icons/checkmark-icon.svg) |
| Detection of other system entities | | ![checkmark icon](../../icons/checkmark-icon.svg) |
| Contextual entities | | ![checkmark icon](../../icons/checkmark-icon.svg) |
| Collect info, as with slots | ![checkmark icon](../../icons/checkmark-icon.svg) | ![checkmark icon](../../icons/checkmark-icon.svg) |
| Options response type | ![checkmark icon](../../icons/checkmark-icon.svg) | ![checkmark icon](../../icons/checkmark-icon.svg) |
| Collect numbers | ![checkmark icon](../../icons/checkmark-icon.svg) | ![checkmark icon](../../icons/checkmark-icon.svg) |
| Connect to human agent response type | | ![checkmark icon](../../icons/checkmark-icon.svg) |
| Free text response type | | ![checkmark icon](../../icons/checkmark-icon.svg) |
| Image response type | | ![checkmark icon](../../icons/checkmark-icon.svg) |
| Search skill response type | ![checkmark icon](../../icons/checkmark-icon.svg) | ![checkmark icon](../../icons/checkmark-icon.svg) |
| Rich text editor for text responses | ![checkmark icon](../../icons/checkmark-icon.svg) | |
| Response validation | ![checkmark icon](../../icons/checkmark-icon.svg) | ![checkmark icon](../../icons/checkmark-icon.svg) |
| User input validation | ![checkmark icon](../../icons/checkmark-icon.svg) | ![checkmark icon](../../icons/checkmark-icon.svg) |
| Support multiple users by notifying them when simultaneous edits are made to the skill | ![checkmark icon](../../icons/checkmark-icon.svg) | |
| Use SpEL expressions | ![checkmark icon](../../icons/checkmark-icon.svg) | ![checkmark icon](../../icons/checkmark-icon.svg) |
| Step logic validation | ![checkmark icon](../../icons/checkmark-icon.svg) | |
| Disambiguation | ![checkmark icon](../../icons/checkmark-icon.svg) | ![checkmark icon](../../icons/checkmark-icon.svg) |
| Digression support | | ![checkmark icon](../../icons/checkmark-icon.svg) |
| Spelling correction | ![checkmark icon](../../icons/checkmark-icon.svg) | ![checkmark icon](../../icons/checkmark-icon.svg) |
| Webhook support | | ![checkmark icon](../../icons/checkmark-icon.svg) |
{: row-headers}
{: class="comparison-table"}
{: caption="Conversational flow skill feature support" caption-side="top"}
{: summary="This table has row and column headers. The row headers identify features. The column headers identify the different skill types. To understand which features are supported by a skill, go to the row that describes the feature, and find the columns for the skill you are interested in."}

For some functions, there is parity but you follow different steps to implement the behavior you want.

- **Jump-to**: In an actions skill, you cannot jump from one action to another. However, you can jump from one step to another. In a dialog skill, you use a jump-to to skip to a specific dialog node in the same branch of the conversation. With an actions skill, you can jump to a different step within an action also. However, to do so, you use conditions on the intervening steps to prevent them from being processed, rather than using an explicit jump-to. The benefit of this approach is that it's easier to anticipate the path of a conversation and follow it later if there are not multiple jump-tos sprinkled throughout the flow. 
- **Slots**: In a dialog skill, you add slots to a dialog node to call out a set of values that you want to collect from the user, and that you will take and store in any order. In the actions skill, every step in the action acts like a slot. If the user provides information that address step 10 when answering the question to step 1, both step 1 and step 10 are filled. In fact, if you want step 10 to ask the question explicitly, you must select the **Always as for this** option on step 10.

*Want to get started with actions, but need features that are available only from dialog skills right now?* Use both. Call an action from your dialog skill to perform a discrete task. For more information, see [Calling an actions skill from dialog](/docs/assistant?topic=assistant-dialog-call-action).
{: important}
