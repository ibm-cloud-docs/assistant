---

copyright:
  years: 2015, 2023
lastupdated: "2021-02-16"

keywords: dialog overview

subcollection: assistant

---

{{site.data.keyword.attribute-definition-list}}

Documentation for the **classic {{site.data.keyword.assistant_classic_short}}** experience has moved. For the most up-to-date version, see [How your dialog is processed](/docs/watson-assistant?topic=watson-assistant-dialog-build){: external}.
{: attention}

# How your dialog is processed
{: #dialog-build}

The dialog uses the intents that are identified in the user's input, plus context from the application, to interact with the user and ultimately provide a useful response.
{: shortdesc}

The dialog matches intents (what users say they want to do) to responses (what the bot says back). The response might be the answer to a question such as `What are your store hours?` or the execution of a command, such as placing an order. The intent and entity might be enough information to identify the correct response, or the dialog might ask the user for more input that is needed to respond correctly. For example, if a user asks, `Where can I get some food?` you might want to clarify whether they want a restaurant or a grocery store, to dine in or take out, and so on. You can ask for more details in a text response and create one or more child nodes to process the new input.

The dialog is represented graphically in {{site.data.keyword.assistant_classic_short}} as a tree. Create a branch to process each intent that you want your conversation to handle. A branch is composed of multiple nodes.

## Dialog nodes
{: #dialog-build-nodes}

Each dialog node contains, at a minimum, a condition and a response.

![Shows user input going to a box that contains the statement If: CONDITION, Then: RESPONSE](images/node1-empty.png)

- Condition: Specifies the information that must be present in the user input for this node in the dialog to be triggered. The information is typically a specific intent. It might also be an entity type, an entity value, or a context variable value. See [Conditions](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-conditions) for more information.
- Response: The utterance that your assistant uses to respond to the user. The response can also be configured to show an image or a list of options, or to trigger programmatic actions. See [Responses](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-responses) for more information.

You can think of the node as having an if/then construction: if this condition is true, then return this response.

For example, the following node is triggered if the natural language processing function of your assistant determines that the user input contains the `#cupcake-menu` intent. As a result of the node being triggered, your assistant responds with an appropriate answer.

![Shows the user asking about cupcake flavors. the If condition is #cupcake-menu and the Then response is a list of cupcake flavors.](images/node1-simple.png)

A single node with one condition and response can handle simple user requests. But, more often than not, users have more sophisticated questions or want help with more complex tasks. You can add child nodes that ask the user to provide any additional information that your assistant needs.

![Shows that the first node in the dialog asks which type of cupcake the user wants, gluten-free or regular, and has two child nodes that provide a different response depending on the user's answer.](images/node1-children.png)

## Dialog flow
{: #dialog-build-flow}

The dialog that you create is processed by your assistant from the first node in the tree to the last.

![Arrow points down next to 3 nodes to show that dialog flows from the first node to the last](images/node-flow-down.png)

As it travels down the tree, if your assistant finds a condition that is met, it triggers that node. It then moves along the triggered node to check the user input against any child node conditions. As it checks the child nodes it moves again from the first child node to the last.

Your assistant continues to work its way through the dialog tree from first to last node, along each triggered node, then from first to last child node, and along each triggered child node until it reaches the last node in the branch it is following.

![Shows arrow 1 pointing from the first root node to the last, arrow 2 pointing from along the length of a triggered node, and arrow 3 pointing from the first to the last child nodes of the triggered node.](images/node-flow.png)

When you start to build the dialog, you must determine the branches to include, and where to place them. The order of the branches is important because nodes are evaluated from first to last. The first root node whose condition matches the input is used; any nodes that come later in the tree are not triggered.

When your assistant reaches the end of a branch, or cannot find a condition that evaluates to true from the current set of child nodes it is evaluating, it jumps back out to the base of the tree. And once again, your assistant processes the root nodes from first to the last. If none of the conditions evaluates to true, then the response from the last node in the tree, which typically has a special `anything_else` condition that always evaluates to true, is returned.

You can disrupt the standard first-to-last flow in the following ways:

- By customizing what happens after a node is processed. For example, you can configure a node to jump directly to another node after it is processed, even if the other node is positioned earlier in the tree. See [Defining what to do next](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-jump-to) for more information.
- By configuring conditional responses to jump to other nodes. See [Conditional responses](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-multiple) for more information.
- By configuring digression settings for dialog nodes. Digressions can also impact how users move through the nodes at run time. If you enable digressions away from most nodes and configure returns, users can jump from one node to another and back again more easily. See [Digressions](/docs/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions) for more information.

## Sample dialog
{: #dialog-depiction}

This diagram shows a mockup of a dialog tree that is built with the graphical user interface dialog editor.
{: shortdesc}

![A sample dialog tree with example content](images/dialog-depiction-full.png)

The dialog tree in this diagram contains two root dialog nodes. A typical dialog tree would likely have many more nodes, but this depiction provides a glimpse of what a subset of nodes might look like.

- The first root node conditions on an intent value. It has two child nodes that each condition on an entity value.  The second child node defines two responses. The first response is returned to the user if the value of the context variable matches the value specified in the condition. Otherwise, the second response is returned.

  This standard type of node is useful to capture questions about a certain topic and then in the root response ask a follow-up question that is addressed by the child nodes. For example, it might recognize a user question about discounts and ask a follow-up question about whether the user is a member of any associations with which the company has special discount arrangements. And the child nodes provide different responses based on the user's answer to the question about association membership.

- The second root node is a node with slots. It also conditions on an intent value. It defines a set of slots, one for each piece of information that you want to collect from the user. Each slot asks a question to elicit the answer from the user. It looks for a specific entity value in the user's reply to the prompt, which it then saves in a slot context variable.

  This type of node is useful for collecting details you might need to perform a transaction on the user's behalf. For example, if the user's intent is to book a flight, the slots can collect the origin and destination location information, travel dates, and so on.

## Ready to get started?
{: #dialog-build-start}

For more information, see [Creating a dialog](/docs/assistant?topic=assistant-dialog-overview).
