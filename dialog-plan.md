---

copyright:
  years: 2015, 2020
lastupdated: "2020-04-24"

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
{:gif: data-image-type='gif'}

# Plan the dialog
{: #dialog-plan}

Learn how to approach building a dialog.
{: shortdesc}

- Plan out the design of the dialog that you want to build before you add a single dialog node. Sketch it out on paper, if necessary.
- Whenever possible, base your design decisions on data from real-world evidence and behaviors. 

  Do not add nodes to handle a situation that someone *thinks* might occur.
- Avoid copying business processes as-is. They are rarely conversational.
- If people already use a process, examine how they approach it. People typically optimize the process from a conversational perspective.
- Decide on the tone, personality, and positioning of your assistant. Consistently reflect these choices in the dialog you create.
- Never misrepresent the assistant as being a human. If users believe the assistant is a person, then find out it's not, they are likely to distrust it.

  In fact, some states have laws that require chat bots to identify themselves as chat bots.
- Not everything has to be a conversation. Sometimes a web form works better. Don't force it.
