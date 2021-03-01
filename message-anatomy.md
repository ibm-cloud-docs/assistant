---

copyright:
  years: 2015, 2021
lastupdated: "2021-03-01"

keywords: message call, message objects

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
{:video: .video}

# Anatomy of a message
{: #message-anatomy}

A single `/message` API call is equivalent to a single turn in a conversation, which consists of a message that is submitted by a customer and a corresponding response from your assistant.
{: shortdesc}

Each reply that a customer makes in response to a prompt from the assistant is passed as an independent `/message` API call.

The body of the `/message` API call request and response includes the following objects:

- `context`: Contains variables that are meant to be persisted. For the dialog to subsequently reference information that is submitted by the user, you must store the information in the context object. For example, the dialog can collect the user's name and then refer to the user by name in subsequent nodes. The following example shows how the context object is represented in the dialog JSON editor:

  ```json
  {
    "context" : {
      "user_name" : "<? @name.literal ?>"
    }
  ```
  {: codeblock}

  See [Retaining information across dialog turns](#dialog-runtime-context-dialog) for more information.

- `input`: The string of text that was submitted by the user. The text string can contain up to 2,048 characters. The following example shows how the `input` object is represented in the dialog JSON editor:

  ```json
  {
    "input" : {
      "text" : "Where's your nearest store?"
    }
  ```
  {: codeblock}

- `output`: The dialog response to return to the user. The following example shows how the output object is represented in the dialog JSON editor:

  ```json
  {
  "output": {
    "generic": [
      {
        "values": [
          {
            "text": "This is my response text."
          }
        ],
        "response_type": "text",
        "selection_policy": "sequential"
      }
    ]
  }
  }
  ```
  {: codeblock}

In the resulting API `/message` response, the text response is formatted as follows:

```json
{
   "text": "This is my response text.",
   "response_type": "text"
}
```

The following `output` object JSON format is supported for backwards compatibility. With the introduction of rich response types, the `output.text` structure was augmented with the `output.generic` structure to facilitate supporting other types of responses in addition to text. Use the new format when you create new nodes to give yourself more flexibility, because you can subsequently change the response type, if needed.
{: note}

  ```json
  {
  "output": {
    "text": {
      "values": [
        "This is my response text."
      ]
    }
  }
  ```
  {: codeblock}

There are response types other than a text response that you can define. See [Responses](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-responses) for more details.

For more information about the `/message` API call, see the [API reference](https://{DomainName}/apidocs/assistant/assistant-v2){: external}.

## Viewing API call details
{: #message-anatomy-inspect-api}

As you test your conversation, you might want to know what the underlying API calls look like that are being returned from the service. You can use the developer tools provided by your web browser to inspect them.

From Chrome, for example, open the developer tools. Open the Network tool. The Name section lists multiple API calls. Click the message call associated with your test utterance, and then click the Response column to see the API response body. It lists the intents and entities that were recognized in the user input with their confidence scores. It also lists the values of context variables at the time of the call. To view the response body in structured format, click the Preview column.

![Shows how to view the API call details by using Chrome web browser developer tools.](images/api-browser-dev.png)
