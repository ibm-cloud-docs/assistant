---

copyright:
  years: 2020, 2021
lastupdated: "2021-06-16"

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

# Actions skill overview <!--![Beta](images/beta.png)-->
{: #actions-overview}

Learn how the actions skill makes it easier to construct an engaging conversation.
{: shortdesc}

## Actions
{: #actions-overview-actions}

Actions represent the discrete tasks or questions that your assistant is designed to help customers with.

Each action has a beginning and an end. An action begins when it recognizes a goal based on the words the customer uses to articulate the goal. An action ends after the steps that are required to satisfy the customer's goal are completed.

The body of the action is composed of one or more *steps* that elicit the information your assistant needs to fulfill the customer's goal.

An action is processed when a customer submits a message that the action is designed to understand and address. The order in which an action is placed in the actions list does not impact the priority that is given to the action.

There are a few actions that are provided by IBM automatically; these actions are called *system* actions.

## Steps
{: #actions-overview-steps}

A step represents a single interaction or exchange of information with a customer, a turn in the conversation.

Each step defines the following things:

- one or more conditions that determine whether the step is processed at run time
- what the assistant says to the customer when the step is processed
- rules for how the customer can reply to what the assistant says
- what to do next

The steps in an action are numbered and are processed from first to last. As you build and test an action, you can drag and drop steps within the steps list to reorder them as you construct the best conversational flow.

## Step conditions
{: #actions-overview-step-conditions}

Step conditions are how your assistant knows whether to include a step in the current exchange with a customer. 

For example, you might have an *Open an account* action. In one step the assistant can ask, *Do you want me to help you open an account?* and allow the customer to answer with *Yes* or *No*. In the next step, you can add a step condition that checks whether the customer answered *No* in the previous step. If so, the assistant says, *OK. Let me know if there's something else I can help you with.* and ends that branch of the conversation in the action. Then, you can add one or more additional steps to walk the customer through the process of opening an account.

## Customer responses
{: #actions-overview-step-responses}

When you define the customer response for a step, you identify the type of data that the assistant expects to find in a reply. If the assistant asks for a number, you define a customer response of type *Numbers*. If the customer does not provide a number in the reply, the conversation stops. The assistant cannot complete a task on the customer's behalf if it doesn't have the data it needs to do so. 

At run time, validation occurs for each step to check the data type of the customer's reply. If the data type doesn't match what's expected, a message is displayed to help customers understand what is required of them. You can customize the validation message and how many times it occurs for a single step.

## Variables
{: #actions-overview-step-variables}

You can save information that the customer shares with the assistant as a *variable*. The assistant can then reference the variable to refer back to what the customer said. Variables help to personalize the conversation between your assistant and the customer. 

The variable is named after the step in which the data that is stored in the variable is collected from the customer. For example, let's say the variable in which the account type is stored is collected from the customer in Step 5, which asks `What type of account?`. When the variable is subsequently referenced in a step text response, the variable is represented as `5. What type of account?` When the variable is shown in the steps list for the action, its name is shortened to `$Step 5`.

Variables exist for the duration of a single action.

## Session variables
{: #actions-overview-step-variables-global}

A session variable is a variable that you can set and use across all actions. A session variable exists for the duration of a single session. A session is what we call an instance of a conversation between the assistant and a customer.

When you define a session variable, you give it a short name, such as `membership status`. You can use this name later to reference the variable when you set its value from within a step.

Client applications or dialog skills that call an action can set the value of a session variable that is used by the action when the action is triggered.

## Expressions
{: #actions-overview-step-expressions}

Expressions are a powerful tool for processing or reformatting data that you collect from the customer. 

For example, you can use an expression to do simple multiplication to calculate a tip recommendation. If the customer shared a bill total amount in a previous step, you can use the following expression to calculate an 18% tip and recommend it as part of the assistant's response:

*For a bill total of $`${step_292}`, I recommend a tip of $`${step_292} * 0.18`.*

At run time, the response looks like this:

*For a bill total of $250, I recommend a tip of $45.*

## Ready to create an actions skill? 
{: #actions-overview-start}

For more information, see [Adding an actions skill](/docs/assistant?topic=assistant-skill-actions-add).