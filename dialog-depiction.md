---

copyright:
  years: 2015, 2019
lastupdated: "2019-02-21"

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

# Dialog depiction
{: #dialog-depiction}

![A sample dialog tree with example content](images/dialog-depiction-full.png)

This diagram shows a mockup of a dialog tree that is built with the graphical user interface dialog editor tool. It contains two root dialog nodes. A typical dialog tree would likely have many more nodes, but this depiction provides a glimpse of what a subset of nodes might look like.

- The first root node conditions on an intent value. It has two child nodes that each condition on an entity value.  The second child node defines two responses. The first response is returned to the user if the value of the context variable matches the value specified in the condition. Otherwise, the second response is returned.

  This standard type of node is useful to capture questions about a certain topic and then in the root response ask a follow-up question that is addressed by the child nodes. For example, it might recognize a user question about discounts and ask a follow-up question about whether the user is a member of any associations with which the company has special discount arrangements. And the child nodes provide different responses based on the user's answer to the question about association membership.

- The second root node is a node with slots. It also conditions on an intent value. It defines a set of slots, one for each piece of information that you want to collect from the user. Each slot asks a question to elicit the answer from the user. It looks for a specific entity value in the user's reply to the prompt, which it then saves in a slot context variable.

  This type of node is useful for collecting details you might need to perform a transaction on the user's behalf. For example, if the user's intent is to book a flight, the slots can collect the origin and destination location information, travel dates, and so on.
