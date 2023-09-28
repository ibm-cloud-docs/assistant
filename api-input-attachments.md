---

copyright:
  years: 2015, 2021
lastupdated: "2021-11-19"

subcollection: assistant


---

{{site.data.keyword.attribute-definition-list}}

Documentation for the **classic {{site.data.keyword.assistant_classic_short}}** experience has moved. For the most up-to-date version, see [Processing input attachments](/docs/watson-assistant?topic=watson-assistant-api-input-attachments){: external}. To see all documentation for the new {{site.data.keyword.conversationshort}}, please go [here](https://cloud.ibm.com/docs/watson-assistant){: external}.
{: attention}

# Processing input attachments
{: #api-input-attachments}

If you are building a custom channel application using the REST API, you can add support for sending media files as input attachments.
{: shortdesc}

The request body of the `message` method supports an `attachments` array, which can specify up to 5 media objects. Media objects sent in the `attachments` array can be intercepted and processed by a configured premessage webhook. ({{site.data.keyword.assistant_classic_short}} itself does not process input attachments.)

For detailed information about how to access attachments using the API, see the [API reference](/apidocs/assistant/assistant-v2#message){: external}.

## Examples
{: #api-input-attachments-examples}

The following example shows the request body sent to a premessage webhook with a message body that includes the text `Hello` and a JPEG image file as an attachment. The application receiving the webhook request can parse the `attachments` array and process the attachment, optionally modifying the message, before it is processed by the assistant.

```json
{
  "event":{
    "name":"message_received"
  },
  "options":{
  },
  "payload":{
    "input":{
      "message_type":"text",
      "text":"Hello",
      "source":{
        "type":"user",
        "id":"00000000000000000000000000000000"
      },
      "options":{
        "suggestion_only":false,
        "return_context":true
      },
      "attachments": [
        {
          "media_type": "image/jpeg",
          "url": "https://yourphoto.com/yourphoto.jpeg"
        }
      ]
    },
    "context":{
      "global":{
        "system":{
          "user_id":"00000000000000000000000000000000"
        }
      },
      "integrations":{
        "text_messaging":{
          "assistant_phone_number":"+12223334444",
          "private":{
            "user_phone_number":"+14443332222",
            "request_id":"00000000-0000-0000-0000-000000000000",
            "ip_address":"172.10.10.10"
          }
        }
      }
    }
  }
}
```
{: codeblock}

This example shows a stateful `/message` request sent by a custom channel application and including a single media attachment.

```javascript
service
  .message({
    assistant_id: '{assistant_id}',
    session_id: '{session_id}',
    input: {
      message_type: 'text',
      text: 'Hello',
      attachments: [
        {
          'media_type': 'image/jpeg',
          'url': 'https://yourphoto.com/yourphoto.jpeg'
        }
      ]
    }
  })
  .then(res => {
    console.log(JSON.stringify(res, null, 2));
  })
  .catch(err => {
    console.log(err);
  });
```
{: codeblock}
{: javascript}

```python
response=service.message(
    assistant_id='{assistant_id}',
    session_id='{session_id}',
    input={
        'message_type': 'text',
        'text': 'Hello',
        'attachments': [
            {
                'media_type': 'image/jpeg',
                'url': 'https://yourphoto.com/yourphoto.jpeg'
            }
        ]
    }
).get_result()

print(json.dumps(response, indent=2))
```
{: codeblock}
{: python}


