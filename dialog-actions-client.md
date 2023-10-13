---

copyright:
  years: 2015, 2021
lastupdated: "2021-01-14"

subcollection: assistant

---

{{site.data.keyword.attribute-definition-list}}

Documentation for the **classic {{site.data.keyword.assistant_classic_short}}** experience has moved. For the most up-to-date version, see [Requesting client actions](/docs/watson-assistant?topic=watson-assistant-dialog-actions-client){: external}.
{: attention}

# Requesting client actions ![BETA](images/beta.png)
{: #dialog-actions-client}

Add a client action to your dialog node to request that a client-side process run and return a result to the dialog.
{: shortdesc}

If your assistant is integrated with a custom client using the API, you can use client actions to invoke functions that the client application completes. A client action defines a request for a client-side process, in a standardized format that your external client application can understand. Your external client application must use the provided information to carry out the requested action, which can be a local programmatic function, a call to an external service, or any other action the client can perform. The client then returns the result from the action to the dialog, which can process it further, display it to the user, or use it as a condition to determine subsequent flow.

Unlike other action types, a client action does not make a direct call itself. Instead, it simply makes a request for the client application to do something, in the form of an action object that is included in a response from the dialog (along with any output text). This object includes a name identifying the requested action, along with any required parameters and the name of a context variable where the result should be stored. It is the responsibility of the client application to carry out the requested action, store the result in the context, and send it back to the dialog with the next message.

You might call a client application to do the following types of things:

- Validate information that you collected from the user.
- Do calculations or string manipulations on user input that are too complex for supported SpEL expression methods to handle.
- Get data from another application or service.

The following diagram illustrates how client actions work, using the example of an action to get a weather forecast.

![Shows someone asking for a weather forecast and the dialog sending a request to a client app, which sends it to the external service and returns the result](images/forecast.png)

The flow of requests and responses follows this pattern:

1. The client application sends a message containing user input that asks for a weather forecast (using the `message` or `message_stateless` method).

1. The user input triggers a dialog node conditioned on a #weather intent. In its response to the client, this node specifies the `get_weather` client action, which is a name that the client application recognizes. (This is in addition to any text response, such as `Checking the weather forecast...`.)

1. When it receives this response, the client application recognizes that the `get_weather` action is being requested. It calls an external web service (`/weather`) to get the actual forecast information, passing any specified parameters (such as the user's location). The client application then stores the returned information in context variable specified in the action request (`$weather_forecast`).

1. The client application sends another message to the service, including the updated context that contains the weather forecast information. From the dialog's perspective, this message is effectively the next round of user input, although the actual input text is blank.

1. A child node of the #weather node is triggered by the presence of the `$weather_forecast` context variable. This node responds with text output that includes the weather forecast, which the client application displays to the user. (If the `$weather_forecast` variable is not set, another child node can handle this case and report an error.)

It is also possible to call an external Web service directly from a dialog node, without involving the client application, by defining a webhook. For more information about how to call an external service using a webhook, see [Making a programmatic call from a dialog node](/docs/assistant?topic=assistant-dialog-webhooks).
{: tip}

## Procedure
{: #dialog-actions-client-call}

To request a client action from a dialog node, complete the following steps:

1.  In the dialog node from which you want to request the client action, open the JSON editor for the node response.

  ![Shows how to access the JSON editor associated with a standard noderesponse.](images/contextvar-json-response.png)

1.  Use the following syntax to define the client action you want to request.

    ```json
    {
      "context": {
        "variable_name" : "variable_value"
      },
      "actions": [
        {
          "name":"<actionName>",
          "type":"client",
          "parameters": {
            "<parameter_name>":"<parameter_value>",
            "<parameter_name>":"<parameter_value>"
          },
          "result_variable": "<result_variable_name>"
        }
      ],
      "output": {
        "text": "response text"
      }
    }
    ```
    {: codeblock}

    The `actions` array specifies the actions you want the client application to carry out. It can define up to five separate actions. Specify the following name and value pairs in the JSON array:

    - `<actionName>`: Required. The name of the action you want the client application to perform. This name can be in any format (for example, `calculateRate` or `get_balance`), but it must be a name that your client application will recognize and know how to handle. The name cannot be longer than 256 characters.

    - `<type>`: Indicates the type of call to make. For a client action, this property is optional (`client` is the default value).

    - `<action_parameters>`: Any parameters that are required for the client action, which are specified as a JSON object. If the action does not require any parameters, omit this property.

    - `<result_variable_name>`: The name of the context variable you want to use to store the JSON object that is returned by the client action. The client application is expected to use the specified variable to send the return value in the context of the next `/message` input, and subsequent dialog nodes can then access it. You can specify the `result_variable_name` by using the following syntax:

      - `my_result`
      - `$my_result`

      The name cannot be longer than 64 characters. The variable name cannot contain the following characters: parentheses `()`, brackets (`[]`), a single quotation mark (`'`), a quotation mark (`"`), or a backslash (`\`).

      You can optionally specify a `context.` location keyword prefix for this variable (for example, `context.my_result`). However, this is not necessary, because all context variables are stored in this location by default.

      You can include periods in the variable name to create a nested JSON object. For example, you can define these variables to capture results from two separate requests to a weather service for forecasts for today and tomorrow:

      - `context.weather.today`
      - `context.weather.tomorrow`

      The results (in this example, `temp` and `rain` properties) are stored in the context in this structure:

      ```json
      {
        "weather": {
          "today": {
            "temp": "20",
            "rain": "30"
          },
          "tomorrow": {
            "temp": "23",
            "rain": "80"
          }
        }
      }
      ```
      {: codeblock}

      If multiple actions in a single JSON action array add the result of their programmatic call to the same context variable, then the order in which the context is updated matters. Per action type, the order in which the actions are defined in the array determines the order in which the context variable's value is set. The context variable value returned by the last action in the array overwrites the values calculated by any other actions.

      The `result_variable` property is required. If the client action does not return any result, specify `null` as the value.

## Client action example
{: #dialog-actions-client-example}

The following example shows what a request for a call to an external weather service might look like. It is added to the JSON editor that is associated with the node response. By the time the node-level response is triggered, slots have collected and stored the date and location information from the user. This example assumes that the client action is named `weather_forecast`, that it takes a `location` parameter, and that the results are to be stored in the `weather_forecast` context variable.

 ``` json
{
  "actions": [
    {
      "name": "get_weather",
      "type": "client",
      "parameters": {
        "location": "$location"
      },
      "result_variable": "weather_forecast"
    }
  ]
}
```
{: codeblock}

The client application must check for the presence of any client actions in the responses to messages it sends to the assistant. When it recognizes a request for the `get_weather` action, it executes the action (calling the external weather service), and it stores the result in the specified context variable (`weather_forecast`). It then sends a message to the service, including the updated context.

To handle this message in your dialog, create a child node following the node that requested the action. You can condition this child node on the special condition `true` to ensure that it is always triggered by the message that the client sends after completing the requested action. In this child node, add the response to show the user, reading the stored action result from the `$my_forecast` context variable:

``` json
{
  "output": {
    "text": {
      "values": [
        "It will be $weather_forecast in $location.literal today."
      ]
    }
  }
```
{: codeblock}

 For more information about how to implement the client side of this exchange, see [Implementing app actions](/docs/assistant?topic=assistant-api-client#api-client-implement-actions).
 {: tip}
