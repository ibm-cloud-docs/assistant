---

copyright:
  years: 2015, 2020
lastupdated: "2019-11-26"

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
{:table: .aria-labeledby="caption"}

# A new approach ![Technology preview experience only](images/preview.png)
{: #dialog-evolution}

The product user interface is being reimagined. The new experience is available as a technology preview for a select set of customers.

The goal of the reimagined user experience is to make the quality of the conversation that your assistant has with your customers the focus of your work. To acheive it, we are making it easier for business subject matter experts to participate directly in designing the assistant. Let the people who best understand your business and how to talk to your customers decide how your assistant speaks and behaves.

We have simplified the steps you take to train your assistant. You can quickly identify discrete customer goals, and in the context of each goal, define the interactions that must occur to satisfy them.

Providing good training data is as critical a step as ever for building powerful virtual assistants. However, you can leave the mechanics of how the training data is created and used to us. Expend your energy on delivering experiences that will delight your customers. The product is designed to generate the data that is needed for the service's algorithmic models automatically, as a byproduct of your work.

## What is changing?
{: #dialog-evolution-what}

The following changes are visible in the technology preview experience only.
{: preview}

The **Intents** and **Dialog** pages are being replaced with a page named **Actions**. 

- **Intents**: This page was where you defined the intents that you wanted the assistant to understand. Intents represent a customer goal. This is where you added sample user utterances as intent user examples to teach your assistant how to recognize when a customer articulated a request that mapped to the intent.  

- **Dialog**: This page is where you authored the script for your assistant to follow as it talked with your customers. The dialog was represented as a hierarchical tree of dialog nodes. Each root node in the tree represented a distinct major topic of conversation. Follow-up questions were represented as child nodes of the root node or other child nodes. The root and child nodes formed a branch of the conversation.  At run time, the dialog processed the nodes in the tree starting from the top of the tree, and flowed down the tree or across a branch depending on which conditions were met by each new user input.

- **Actions**: This page incorporates the purposes that were served by both the *Intents* and *Dialog* pages into a single page. The new approach represents the conversation as a set of distinct actions. You teach the action to recognize customer requests for the action first. You then add steps to each action to define the interactions your assistant will have with your customers to help them reach their goal. A step might be to ask a follow-up question or ask the customer to choose from a list of options, and so on. See [Creating actions](/docs/services/assistant?topic=assistant-actions).

## Key functional differences
{: #dialog-evolution-differences}

If you are an existing user who is familiar with the standard product, the following list explains the key differences in the new experience.

- An action serves two purposes: 1. it is equivalent to an intent, and 2. it serves as a root dialog node. Steps that you add to the action are equivalent to the root node's child nodes.
- An action conditions on a single intent only. The steps attached to an action condition on context variables or entities only; they cannot condition on intents.
- The run time conversation does not flow through the root nodes from the top to the bottom of a dialog tree. Rather, the action defined for the request that is detected in user input is triggered independently. The steps within an action continue to be processed from top to bottom.
- Context variables are scoped to the action that uses them. They are nulled when the action is finished. An action's context variable cannot be set and retrieved by any other action that is triggered during a conversation.