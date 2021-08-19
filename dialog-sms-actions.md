---

copyright:
  years: 2020, 2021
lastupdated: "2021-02-09"

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

# Handling SMS with Twilio interactions
{: #dialog-sms-actions}

Learn about common actions you can use to manage the flow of conversations that your assistant has with customers by using SMS text messaging.
{: shortdesc}

Before you add customizations to your dialog that support SMS messaging interactions, you must set up the SMS with Twilio integration. For more information, see [Integrating with SMS with Twilio](/docs/assistant?topic=assistant-deploy-sms).

You can perform the following types of actions:

- [Sending multimedia content over text](#dialog-sms-actions-mms)
- [Customizing lists in a text message](#dialog-sms-actions-lists)
- [Sending a text message during a phone conversation](/docs/assistant?topic=assistant-dialog-voice-actions#dialog-voice-actions-sms)

For command reference documentation, see [SMS with Twilio integration commands reference](/docs/assistant?topic=assistant-commands-sms).

## Adding SMS-based actions to your dialog
{: #dialog-sms-actions-add}

When calling messaging-specific actions from a dialog, follow these guidelines:

- Define the action within the `context` object, not the `output` object of the dialog node JSON snippet.
- Define only one action or one sequence per conversation turn.
- Do not jump from a dialog node with an action configured for it to another dialog node with an action configured for it.

To enable text messaging-specific actions, you must add a JSON code block to the dialog node where you want the action to trigger. 

To add a JSON code block to a dialog node, complete the following steps:

1.  From your dialog skill, go to the *Dialog* page.
1.  Open the dialog node where you want to call the action.
1.  From the *Assistant responds* section, click the menu ![Overflow menu](images/more-icon.png), and then choose **Open JSON editor**.

    ![Shows the Assistant responds section of a dialog node with the user selecting the Open JSON editor from the options icon.](images/open-json-editor.png)
1.  Add the action command JSON code block to the `context` object. (If no `context` object exists, add one. The `context` object is a peer to the `output` object.)

    For example:

    ```json
    {
      "output": {
        "generic": [
        ]
      },
      "context": {
        "smsAction": {
          "command": "<command-name>",
          "parameters": {
            "<first-parameter>": "<parameter-value>"
          }
        }
      }
    }
    ```
    {: codeblock}

## Sending multimedia content over text
{: #dialog-sms-actions-mms}

To allow multimedia content, such as an image, to be sent in a text message, use the `smsActSendMedia` command.

```json
{
  "output": {
      "generic": [
      ]
  },
  "context": {
    "smsAction": {
          "command": "smsActSendMedia",
          "parameters":{
            "mediaURL": [
              "https://upload.wikimedia.org/wikipedia/commons/3/39/We_love_kids_but.jpg"
            ]
         }  
    }
  }
}
```
{: codeblock}

You can specify the following parameter value for the `vsmsActSendMedia` command:

- `mediaURL`: A JSON array of one or more publicly-accessible media URLs for images or videos.

## Customizing lists
{: #dialog-sms-actions-lists}

You can customize how these lists are displayed and handled.

- [Options list](#dialog-sms-actions-options)
- [Disambiguation](#dialog-sms-actions-disambiguation)

### Options list
{: #dialog-sms-actions-options}

The dialog supports an `option` response type, which shows the customer multiple options to choose from. You can customize how the options that are defined for an option response type are shown to customers and the ways in which a customer can select an option by adding the `vgwActSetOptionsConfig` action command.

The following example shows how to customize the option response type.

```json
{
  "output": {
    "generic": [
      {
      "title": "Which of these items do you want to insure?",
      "options": [
      {
        "label": "Boat",
        "value": {
          "input": {
            "text": "I want to buy boat insurance."
          }
        },
          "label": "Car",
          "value": {
            "input": {
              "text": "I want to buy car insurance."
            }
          },
          "label": "House",
          "value": {
            "input": {
              "text": "I want to buy house insurance."
            }
          }
        }
      ],
      "description": "Insurance types.",
      "response_type": "option"
      }
    ]
  },
  "context": {
    "smsAction": {
      "command": "smsActSetOptionsConfig",
      "parameters": {
        "prefixText": "%s."
      }
    }
  }
}
```
{: codeblock}

First the value specified in the `title` attribute is displayed to the user. Then, the text specified in each `label` attribute. For example, `Which of these items do you want to insure? 1.Boat 2.Car 3.House`

To configure what the assistant shows before each option, edit the `prefixText` parameter. Use `%s` to represent the number corresponding to the option; it is replaced with the actual number at run time.

```json
"smsAction": {
  "command": "smsActSetOptionsConfig",
  "parameters": {
    "prefixText": "Enter %s for "
  }
}
```
{: codeblock}

For example, `Which of these items do you want to insure? Enter 1 for Boat Enter 2 for Car Enter 3 for House`

### Disambiguation list
{: #dialog-sms-actions-disambiguation}

When the dialog is confident that more than one dialog node might be the right one to process in response to a customer query, disambiguation is triggered. Disambiguation asks the customer to clarify which path they want to follow to get an answer. For more information, see [Disambiguation](/docs/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation).

You can customize how the disambiguation list choices are displayed and how a customer can select a disambiguation choice by adding the `smsActSetDisambiguationConfig` action command. 

You might want to define the customization in the welcome node or another node that is triggered early in the conversation so it is applied any time disambiguation is triggered.

```json
{
  "output": {
      "generic": [
      ]
  },
  "context": {
    "smsAction": {
      "command": "smsActSetDisambiguationConfig",
      "parameters": {
        "prefixText": "%s."
      }
    }
  }
}
```
{: codeblock}

When displayed, the assistant shows the introductory text that is configured for disambiguation, such as `Did you mean?`. Then it lists the choices from the disambiguation list as numbered choices.

The `prefixText` parameter prepends a number to the text specified in a `label`. The list choices are numbered sequentially and are displayed to the user in the order in which they appear in the list. The user can type a number to pick one of the choices.

For example, if label is configured as follows:

```
"label": "I'd like to order a drink."
```
{: codeblock}

The assistant sends this message to the user:

```
1. I'd like to order a drink.
```
{: codeblock}

You can configure the text that will be prepended to each label. In the `prefixText` attribute, `%s` represents the number corresponding to the suggestion; it is replaced with the actual number at run time.

```json
"context": {
    "smsAction": {
      "command": "smsActSetDisambiguationConfig",
      "parameters": {
        "prefixText": "Enter %s for:"
      }
    }
  }
```
{: codeblock}

When `prefixText` is set to `Enter %s for:`, the following output is sent to the customer:

```
Enter 1 for: I'd like to order a drink.
```
{: codeblock}
