---

copyright:
  years: 2015, 2020
lastupdated: "2020-10-02"

keywords: context, context variable, digression, disambiguation, autocorrection, spelling correction, spell check, confidence 

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

# Controlling the dialog flow
{: #dialog-runtime}

Understand how your dialog is processed when a person interacts with your assistant at run time. Learn to use the conversation flow to your advantage and alter the flow if necessary.
{: shortdesc}

## Anatomy of a dialog call
{: #dialog-runtime-message-anatomy}

Each user utterance is passed to the dialog as a /message API call. This includes utterances that users make in reply to prompts from the dialog that ask them for more information. Some subscription plans include a set number of API calls, so it helps to understand what constitutes a call. A single /message API call is equivalent to a single dialog turn, which consists of an input from the user and a corresponding response from the dialog.

The body of the /message API call request and response includes the following objects:

- `context`: Contains variables that are meant to be persisted. To pass information from one call to the next, the application developer must pass the previous API call's response context in with each subsequent API call. For example, the dialog can collect the user's name and then refer to the user by name in subsequent nodes. The following example shows how the context object is represented in the dialog JSON editor:

  ```json
  {
    "context" : {
      "user_name" : "<? @name.literal ?>"
    }
  ```
  {: codeblock}

  See [Retaining information across dialog turns](#dialog-runtime-context) for more information.

- `input`: The string of text that was submitted by the user. The text string can contain up to 2,048 characters. The following example shows how the input object is represented in the dialog JSON editor:

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

In the resulting API /message response, the text response is formatted as follows:

```json
{
   "text": "This is my response text.",
   "response_type": "text"
}
```

The following `output` object JSON format is supported for backwards compatibility. Any workspaces that specify a text response by using this format will continue to function properly. With the introduction of rich response types, the `output.text` structure was augmented with the `output.generic` structure to facilitate supporting other types of responses in addition to text. Use the new format when you create new nodes to give yourself more flexibility, because you can subsequently change the response type, if needed.
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

You can learn more about the /message API call from the [API reference](https://{DomainName}/apidocs/assistant/assistant-v2){: external}.

### Retaining information across dialog turns
{: #dialog-runtime-context}

The dialog in a dialog skill is stateless, meaning that it does not retain information from one interaction with the user to the next. When you add a dialog skill to an assistant and deploy it, the assistant saves the context from one message call and then re-submits it on the next request throughout the current session. The current session lasts for as long a user interacts with the assistant, and then up to 60 minutes of inactivity for Plus or Premium plans (5 minutes for Lite or Standard plans). If you do not add the dialog skill to an assistant, it is your responsibility as the custom application developer to maintain any continuing information that the application needs. The application must look for, and store the context object in the message API response, and pass it in the context object with the next /message API request that is made as part of the conversation flow.

One way to retain the information yourself is to store the entire context object in memory in the client application, in a web browser, for example. As an application becomes more complex, or if it needs to pass and store personally identifiable information, then you can store and retrieve the information from a database. Of course, the simplest approach is one that prevents you from having to store context at all. To implement this approach, add the dialog skill to an assistant and let the assistant keep track of the context for you.

The application can pass information to the dialog, and the dialog can update this information and pass it back to the application, or to a subsequent node. The dialog does so by using *context variables*.

## Context variables
{: #dialog-runtime-context-variables}

A context variable is a variable that you define in a node. You can specify a default value for it. Other nodes, application logic, or user input can subsequently set or change the value of the context variable.

You can condition against context variable values by referencing a context variable from a dialog node condition to determine whether to execute a node. You can also reference a context variable from dialog node response conditions to show different reponses depending on a value provided by an external service or by the user.

Learn more:

- [Passing context from the application](#dialog-runtime-context-from-app)
- [Passing context from node to node](#dialog-runtime-context-node-to-node)
- [Defining a context variable](#dialog-runtime-context-var-define)
- [Common context variable tasks](#dialog-runtime-context-common-tasks)
- [Deleting a context variable](#dialog-runtime-context-delete)
- [Updating a context variable](#dialog-runtime-context-update)
- [How context variables are processed](#dialog-runtime-context-processing)
- [Order of operation](#dialog-runtime-context-order-of-ops)
- [Adding context variables to a node with slots](#dialog-runtime-context-var-slots)

### Passing context from the application
{: #dialog-runtime-context-from-app}

Pass information from the application to the dialog by setting a context variable and passing the context variable to the dialog.

For example, your application can set a $time_of_day context variable, and pass it to the dialog which can use the information to tailor the greeting it displays to the user.

![Shows a Welcome node that uses response conditions to check for the value of the $time_of_day context variable that is passed to the dialog from the application.](images/set-context.png)

In this example, the dialog knows that the application sets the variable to one of these values: *morning*, *afternoon*, or *evening*. It can check for each value, and depending on which value is present, return the appropriate greeting. If the variable is not passed or has a value that does not match one of the expected values, then a more generic greeting is displayed to the user.

### Passing context from node to node
{: #dialog-runtime-context-node-to-node}

The dialog can also add context variables to pass information from one node to another or to update the values of context variables. As the dialog asks for and gets information from the user, it can keep track of the information and reference it later in the conversation.

For example, in one node you might ask users for their name, and in a later node address them by name.

![Shows an introductions node that asks the user for their name, and stores it as a context variable. The next node refers to the user by name by using the $username context variable.](images/set-context-username.png)

In this example, the system entity @sys-person is used to extract the user's name from the input if the user provides one. In the JSON editor, the username context variable is defined and set to the @sys-person value. In a subsequent node, the $username context variable is included in the response to address the user by name.

### Defining a context variable
{: #dialog-runtime-context-var-define}

Define a context variable by adding the variable name to the **Variable** field and adding a default value for it to the **Value** field in the node's edit view.

1.  Click to open the dialog node to which you want to add a context variable.

1.  Go to the *Assistant responds* section and click the menu icon ![overflow menu icon](images/more-icon.png).

      If you're using multiple conditioned responses, you must click the *Customize response* icon ![gear icon](images/customize-response-icon.png) to see the menu that is associated with the response.
      {: tip}

1.  Click **Open context editor**.

      ![Shows the Open context editor menu option selected from the list](images/open-context-editor.png)

1.  Add the variable name and value pair to the **Variable** and **Value** fields.

    - The `name` can contain any upper- and lowercase alphabetic characters, numeric characters (0-9), and underscores.

      You can include other characters, such as periods and hyphens, in the name. However, if you do, then you must specify the shorthand syntax `$(variable-name)` every time you subsequently reference the variable. See [Expressions for accessing objects](/docs/assistant?topic=assistant-expression-language#expression-language-shorthand-syntax) for more details.
      {:tip}

    - The `value` can be any supported JSON type, such as a simple string variable, a number, a JSON array, or a JSON object.

The following table shows some examples of how to define name and value pairs for different types of values:

| Variable       | Value                         | Value Type |
|:---------------|-------------------------------|------------|
| dessert        | "cake"                        | String     |
| age            | 18                            | Number     |
| toppings_array | ["onions","olives"]            | JSON Array |
| full_name      | {"first":"John","last":"Doe"} | JSON Object |

To subsequently refer to these context variables, use the syntax `$name` where *name* is the name of the context variable that you defined.

For example, you might specify the following expression as the dialog response:

`The customer, $age-year-old <? $full_name.first ?>, wants a pizza with <? $toppings_array.join(' and ') ?>, and then $dessert.`

The resulting output is displayed as follows:

`The customer, 18-year-old John, wants a pizza with onions and olives, and then cake.`

You can use the JSON editor to define context variables also. You might prefer to use the JSON editor if you want to add a complex expression as the variable value. See [Context variables in the JSON editor](#dialog-runtime-context-var-json) for more details.

### Common context variable tasks
{: #dialog-runtime-context-common-tasks}

To store the entire string that was provided by the user as input, use `input.text`:

| Variable | Value            |
|----------|------------------|
| repeat   | `<?input.text?>` |

For example, the user input is, `I want to order a device.` If the node response is, `You said: $repeat`, then the response would be displayed as, `You said: I want to order a device.`

To store the value of an entity in a context variable, use this syntax:

| Variable | Value            |
|----------|------------------|
| place    | `@place`         |

For example, the user input is, `I want to go to Paris.` If your `@place` entity recognizes `Paris`, then your assistant saves `Paris` in the `$place` context variable.

To store the value of a string that you extract from the user's input, you can include a SpEL expression that uses the `extract` method to apply a regular expression to the user input. The following expression extracts a number from the user input, and saves it to the `$number` context variable.

| Variable | Value                               |
|----------|-------------------------------------|
| number   | `<?input.text.extract('[\d]+',0)?>` |

To store the value of a pattern entity, append .literal to the entity name. Using this syntax ensures that the exact span of text from user input that matched the specified pattern is stored in the variable.

| Variable | Value                  |
|----------|------------------------|
| email    | `<? @email.literal ?>` |

For example, the user input is `Contact me at joe@example.com.` Your entity named `@email` recognizes the `name@domain.com` email format. By configuring the context variable to store `@email.literal`, you indicate that you want to store the part of the input that matched the pattern. If you omit the `.literal` property from the value expression, then the entity value name that you specified for the pattern is returned instead of the segment of user input that matched the pattern.

Many of these value examples use methods to capture different parts of the user input. For more information about the methods available for you to use, see [Expression language methods](/docs/assistant?topic=assistant-dialog-methods).

### Deleting a context variable
{: #dialog-runtime-context-delete}

To delete a context variable, set the variable to null.

| Variable   | Value            |
|------------|------------------|
| order_form | `null`           |

Alternatively you can delete the context variable in your application logic. For information about how to remove the variable entirely, see [Deleting a context variable in JSON](#dialog-runtime-context-delete-json).

### Updating a context variable value
{: #dialog-runtime-context-update}

To update a context variable's value, define a context variable with the same name as the previous context variable, but this time, specify a different value for it.

When more than one node sets the value of the same context variable, the value for the context variable can change over the course of a conversation with a user. Which value is applied at any given time depends on which node is being triggered by the user in the course of the conversation. The value specified for the context variable in the last node that is processed overwrites any values that were set for the variable by nodes that were processed previously.

For information about how to update the value of a context variable when the value is a JSON object or JSON array data type, see [Updating a context variable value in JSON](#dialog-runtime-context-update-json)

### How context variables are processed
{: #dialog-runtime-context-processing}

Where you define the context variable matters. The context variable is not created and set to the value that you specify for it until your assistant processes the part of the dialog node where you defined the context variable. In most cases, you define the context variable as part of the node response. When you do so, the context variable is created and given the specified value when your assistant returns the node response.

For a node with conditional responses, the context variable is created and set when the condition for a specific response is met and that response is processed. For example, if you define a context variable for conditional response #1 and your assistant processes conditional response #2 only, then the context variable that you defined for conditional response #1 is not created and set.

For information about where to add context variables that you want your assistant to create and set as a user interacts with a node with slots, see [Adding context variables to a node with slots](#dialog-runtime-context-var-slots).

### Order of operation
{: #dialog-runtime-context-order-of-ops}

When you define multiple variables to be processed together, the order in which you define them does not determine the order in which they are evaluated by your assistant. Your assistant evaluates the variables in random order. Do not set a value in the first context variable in the list and expect to be able to use it in the second variable in the list, because there is no guarantee that the first context variable will be executed before the second one. For example, do not use two context variables to implement logic that checks whether the user input contains the word `Yes` in it.

| Variable        | Value            |
|-----------------|------------------|
| user_input      | <? input.text ?> |
| contains_yes    | <? $user_input.contains('Yes') ?> |

Instead, use a slightly more complex expression to avoid having to rely on the value of the first variable in your list (user_input) being evaluated before the second variable (contains_yes) is evaluated.

| Variable      | Value            |
|---------------|------------------|
| contains_yes  | <? input.text.contains('Yes') ?> |

### Adding context variables to a node with slots
{: #dialog-runtime-context-var-slots}

For more information about slots, see [Gathering information with slots](/docs/assistant?topic=assistant-dialog-slots).

1.  Open the node with slots in the edit view.

    - To add a context variable that is processed after a response condition for a slot is met, perform the following steps:

      1.  Click the *Edit slot* icon ![Edit response](images/edit-slot.png).
      1.  Click the *Options* icon ![Advanced response](images/kabob.png), and then select **Enable conditional responses**.
      1.  Click the *Edit response* icon ![Edit response](images/edit-slot.png) next to the response with which you want to associate the context variable.
      1.  Click the *Options* icon ![Advanced response](images/kabob.png) in the response section, and then click **Open context editor**.
      1.  Add the variable name and value pair to the **Variable** and **Value** fields.

      ![Shows how to access the JSON editor associated with the conditional response for a slot.](images/contextvar-json-slot-multi-response.png)

    - To add a context variable that is set or updated after a slot condition is met, complete the following steps:

      1.  Click the *Edit slot* icon ![Edit response](images/edit-slot.png).
      1.  From the *Options* ![Advanced response](images/kabob.png) menu in the *Configure slot* view header, click **Open JSON editor**.
      1.  Add the variable name and value pair in JSON format.

          ```json
          {
            "time_of_day": "morning"
          }
          ```
          {: codeblock}

      There is currently no way to use the context editor to define context variables that are set during this phase of dialog node evaluation. You must use the JSON editor instead. For more information about using the JSON editor, see [Context variables in the JSON editor](#dialog-runtime-context-var-json).
      {: note}

      ![Shows how to access the JSON editor associated with a slot condition.](images/contextvar-json-slot-condition.png)

## Context variables in the JSON editor
{: #dialog-runtime-context-var-json}

You can also define a context variable in the JSON editor. You might want to use the JSON editor if you are defining a complex context variable and want to be able to see the full SpEL expression as you add or change it.

The name and value pair must meet these requirements:

- The `name` can contain any upper- and lowercase alphabetic characters, numeric characters (0-9), and underscores.

  You can include other characters, such as periods and hyphens, in the name. However, if you do, then you must specify the shorthand syntax `$(variable-name)` every time you subsequently reference the variable. See [Expressions for accessing objects](/docs/assistant?topic=assistant-expression-language#expression-language-shorthand-syntax) for more details.
  {:tip}

- The `value` can be any supported JSON type, such as a simple string variable, a number, a JSON array, or a JSON object.

The following JSON sample defines values for the $dessert string, $toppings_array array, $age number, and $full_name object context variables:

```json
{
  "context": {
    "dessert": "cake",
    "toppings_array": [
      "onions",
      "olives"
    ],
    "age": 18,
    "full_name": {
      "first": "Jane",
      "last": "Doe"
    }
  },
  "output":{}
}
```
{: codeblock}

To define a context variable in JSON format, complete the following steps:

1.  Click to open the dialog node to which you want to add the context variable.

    Any existing context variable values that are defined for this node are displayed in a set of corresponding **Variable** and **Value** fields. If you do not want them to be displayed in the edit view of the node, you must close the context editor. You can close the editor from the same menu that is used to open the JSON editor; the following steps describe how to access the menu.
    {: note}

1.  Click the *Options* icon ![Advanced response](images/kabob.png) that is associated with the response, and then click **Open JSON editor**.

    ![Shows how to access the JSON editor associated with a standard node response.](images/contextvar-json-response.png)

    If the **Multiple responses** setting is **On** for the node, then you must first click the **Edit response** ![Edit response](images/edit-slot.png) icon for the response with which you want to associate the context variable.

    ![Shows how to access the JSON editor associated with a standard node that has multiple conditional responses enabled for it.](images/contextvar-json-multi-response.png)

1.  Add a `"context":{}` block if one is not present.

    ```json
    {
      "context":{},
      "output":{}
    }
    ```
    {: codeblock}

1.  In the context block, add a `"name"` and `"value"` pair for each context variable that you want to define.

    ```json
    {
      "context":{
        "name": "value"
    },
      "output": {}
    }
    ```
    {: codeblock}

    In this example, a variable named `new_variable` is added to a context block that already contains a variable.

    ```json
    {
      "context":{
        "existing_variable": "value",
        "new_variable":"value"
      }
    }
    ```
    {: codeblock}

    To subsequently reference the context variable, use the syntax `$name` where *name* is the name of the context variable that you defined. For example, `$new_variable`.

Learn more:

- [Deleting a context variable in JSON](#dialog-runtime-context-delete-json)
- [Updating a context variable value in JSON](#dialog-runtime-context-update-json)
- [Setting one context variable equal to another](#dialog-runtime-var-equals-var)

### Deleting a context variable in JSON
{: #dialog-runtime-context-delete-json}

To delete a context variable, set the variable to null.

```json
{
  "context": {
    "order_form": null
  }
}
```
{: codeblock}

If you want to remove all trace of the context variable, you can use the JSONObject.remove(string) method to delete it from the context object. However, you must use a variable to perform the removal. Define the new variable in the message output so it will not be saved beyond the current call.

```json
{
  "output": {
    "text" : {},
    "deleted_variable" : "<? context.remove('order_form') ?>"
  }
}
```
{: codeblock}

Alternatively you can delete the context variable in your application logic.

### Updating a context variable value in JSON
{: #dialog-runtime-context-update-json}

In general, if a node sets the value of a context variable that is already set, then the previous value is overwritten by the new value.

#### Updating a complex JSON object

Previous values are overwritten for all JSON types except a JSON object. If the context variable is a complex type such as JSON object, a JSON merge procedure is used to update the variable. The merge operation adds any newly defined properties and overwrites any existing properties of the object.

In this example, a name context variable is defined as a complex object.

```json
{
  "context": {
    "complex_object": {
      "user_firstname" : "Paul",
      "user_lastname" : "Pan",
      "has_card" : false
    }
  }
}
```
{: codeblock}

A dialog node updates the context variable JSON object with the following values:

```json
{
  "complex_object": {
    "user_firstname": "Peter",
    "has_card": true
  }
}
```
{: codeblock}

The result is this context:

```json
{
  "complex_object": {
    "user_firstname": "Peter",
    "user_lastname": "Pan",
    "has_card": true
  }
}
```
{: codeblock}

See [Expression language methods](/docs/assistant?topic=assistant-dialog-methods#dialog-methods-objects) for more information about methods you can perform on objects.

#### Updating arrays

If your dialog context data contains an array of values, you can update the array by appending values, removing a value, or replacing all the values.

Choose one of these actions to update the array. In each case, we see the array before the action, the action, and the array after the action has been applied.

- **Append**: To add values to the end of an array, use the `append` method.

    For this Dialog runtime context:

    ```json
    {
      "context": {
        "toppings_array": ["onion", "olives"]
      }
    }
    ```
    {: codeblock}

    Make this update:

    ```json
    {
      "context": {
        "toppings_array": "<? $toppings_array.append('ketchup', 'tomatoes') ?>"
      }
    }
    ```
    {: codeblock}

    Result:

    ```json
    {
      "context": {
        "toppings_array": ["onion", "olives", "ketchup", "tomatoes"]
      }
    }
    ```
    {: codeblock}

- **Remove**: To remove an element, use the `remove` method and specify its value or position in the array.

    - **Remove by value** removes an element from an array by its value.

        For this Dialog runtime context:

        ```json
        {
          "context": {
            "toppings_array": ["onion", "olives"]
          }
        }
        ```
        {: codeblock}

        Make this update:

        ```json
        {
          "context": {
            "toppings_array": "<? $toppings_array.removeValue('onion') ?>"
          }
        }
        ```
        {: codeblock}

        Result:

        ```json
        {
          "context": {
            "toppings_array": ["olives"]
          }
        }
        ```
        {: codeblock}

    - **Remove by position**: Removing an element from an array by its index position:

        For this Dialog runtime context:

        ```json
        {
          "context": {
            "toppings_array": ["onion", "olives"]
          }
        }
        ```
        {: codeblock}

        Make this update:

        ```json
        {
          "context": {
            "toppings_array": "<? $toppings_array.remove(0) ?>"
          }
        }
        ```
        {: codeblock}

        Result:

        ```json
        {
          "context": {
            "toppings_array": ["olives"]
          }
        }
        ```
        {: codeblock}

- **Overwrite**: To overwrite the values in an array, simply set the array to the new values:

    For this Dialog runtime context:

        ```json
        {
          "context": {
            "toppings_array": ["onion", "olives"]
          }
        }
        ```
        {: codeblock}

    Make this update:

        ```json
        {
          "context": {
            "toppings_array": ["ketchup", "tomatoes"]
          }
        }
        ```
        {: codeblock}

    Result:

        ```json
        {
          "context": {
            "toppings_array": ["ketchup", "tomatoes"]
          }
        }
        ```
        {: codeblock}

See [Expression language methods](/docs/assistant?topic=assistant-dialog-methods#dialog-methods-arrays) for more information about methods you can perform on arrays.

### Setting one context variable equal to another
{: #dialog-runtime-var-equals-var}

When you set one context variable equal to another context variable, you define a pointer from one to the other. If the value of one of the variables subsequently changes, then the value of the other variable is changed also.

For example, if you specify a context variable as follows, then when the value of either `$var1` or `$var2` subsequently changes, the value of the other changes too.

| Variable  | Value  |
|-----------|--------|
| var2      | var1   |

Do not set one variable equal to another to capture a point in time value. When dealing with arrays, for example, if you want to capture an array value stored in a context variable at a certain point in the dialog to save it for later use, you can create a new variable based on the current value of the variable instead.

For example, to create a copy of the values of an array at a certain point of the dialog flow, add a new array that is populated with the values for the existing array. To do so, you can use the following syntax:

```json
{
"context": {
   "var2": "<? output.var2?:new JsonArray().append($var1) ?>"
 }
 }
 ```
{: codeblock}

## Digressions
{: #dialog-runtime-digressions}

A digression occurs when a user is in the middle of a dialog flow that is designed to address one goal, and abruptly switches topics to initiate a dialog flow that is designed to address a different goal. The dialog has always supported the user's ability to change subjects. If none of the nodes in the dialog branch that is being processed match the goal of the user's latest input, the conversation goes back out to the tree to check the root node conditions for an appropriate match. The digression settings that are available per node give you the ability to tailor this behavior even more.

With digression settings, you can allow the conversation to return to the dialog flow that was interrupted when the digression occurred. For example, the user might be ordering a new phone, but switches topics to ask about tablets. Your dialog can answer the question about tablets, and then bring the user back to where they left off in the process of ordering a phone. Allowing digressions to occur and return gives your users more control over the flow of the conversation at run time. They can change topics, follow a dialog flow about the unrelated topic to its end, and then return to where they were before. The result is a dialog flow that more closely simulates a human-to-human conversation.

![Shows someone who is providing details about a dinner reservation ask about vegetarian options, get an answer, and then return to providing reservation details.](images/digression.gif){: gif}

The animated image uses a mockup of the dialog tree user interface to illustrate the concept of a digression. It shows how a user interacts with dialog nodes that are configured to allow digressions that return to the dialog flow that was in progress. The user starts to provide the information required to make a dinner reservation. In the middle of filling slots in the #reservation node, the user asks a question about vegetarian menu options. The dialog answers the user's new question by finding a node that addresses it amongst the root nodes (a node that conditions on the #cuisine intent). It then returns to the conversation that was in progress by showing the prompt for the next empty slot from the original dialog node.

Watch this video to learn more.

![Digressions overview](https://www.youtube.com/embed/I3K7mQ46K3o){: video output="iframe" id="youtubeplayer" frameborder="0" width="560" height="315" webkitallowfullscreen mozallowfullscreen allowfullscreen}

- [Before you begin](#dialog-runtime-digression-prereqs)
- [Customizing digressions](#dialog-runtime-enable-digressions)
- [Digression usage tips](#dialog-runtime-digress-tips)
- [Disabling digressions into a root node](#dialog-runtime-disable-digressions)
- [Digression tutorial](#dialog-runtime-digression-tutorial)
- [Design considerations](#dialog-runtime-digression-design-considerations)

### Before you begin
{: #dialog-runtime-digression-prereqs}

As you test your overall dialog, decide when and where it makes sense to allow digressions and returns from digressions to occur. The following digression controls are applied to the nodes automatically. Only take action if you want to change this default behavior.

- Every root node in your dialog is configured to allow digressions to target them by default. Child nodes cannot be the target of a digression.
- Nodes with slots are configured to prevent digressions away. All other nodes are configured to allow digressions away. However, the conversation cannot digress away from a node under the following circumstances:

  - If any of the child nodes of the current node contain the `anything_else` or `true` condition

    These conditions are special in that they always evaluate to true. Because of their known behavior, they are often used in dialogs to force a parent node to evaluate a specific child node in succession. To prevent breaking existing dialog flow logic, digression are not allowed in this case. Before you can enable digressions away from such a node, you must change the child node's condition to something else.

  - If the node is configured to jump to another node or skip user input after it is processed

    The final step section of a node specifies what should happen after the node is processed. When the dialog is configured to jump directly to another node, it is often to ensure that a specific sequence is followed. And when the node is configured to skip user input, it is equivalent to forcing the dialog to process the first child node after the current node in succession. To prevent breaking existing dialog flow logic, digressions are not allowed in either of these cases. Before you can enable digressions away from this node, you must change what is specified in the final step section.

### Customizing digressions
{: #dialog-runtime-enable-digressions}

You do not define the start and end of a digression. The user is entirely in control of the digression flow at run time. You only specify how each node should or should not participate in a user-led digression. For each node, you configure whether:

- a digression can start from and leave the node
- a digression that starts elsewhere can target and enter the node
- a digression that starts elsewhere and enters the node must return to the interrupted dialog flow after the current dialog flow is completed

To change the digression behavior for an individual node, complete the following steps:

1.  Click the node to open its edit view.

1.  Click **Customize**, and then click the **Digressions** tab.

    The configuration options differ depending on whether the node you are editing is a root node, a child node, a node with children, or a node with slots.

    **Digressions away from this node**

    If the circumstances listed earlier do not apply, then you can make the following choices:

    - **All node types**: Choose whether to allow users to digress away from the current node before they reach the end of the current dialog branch.

    - **All nodes that have children**: Choose whether you want the conversation to come back to the current node after a digression if the current node's response has already been displayed and its child nodes are incidental to the node's goal. Set the **Allow return from digressions triggered after this node's response** switch to **No** to prevent the dialog from returning to the current node and continuing to process its branch.

      For example, if the user asks, `Do you sell cupcakes?` and the response, `We offer cupcakes in a variety of flavors and sizes` is displayed before the user changes subjects, you might not want the dialog to return to where it left off. Especially, if the child nodes only address possible follow-up questions from the user and can safely be ignored.

      However, if the node relies on its child nodes to address the question, then you might want to force the conversation to return and continue processing the nodes in the current branch. For example, the initial response might be, `We offer cupcakes in all shapes and sizes. Which menu do you want to see: gluten-free, dairy-free, or regular?` If the user changes subjects at this point, you might want the dialog to return so the user can pick a menu type and get the information they wanted.

    - **Nodes with slots**: Choose whether you want to allow users to digress away from the node before all of the slots are filled. Set the **Allow digressions away while slot filling** switch to **Yes** to enable digressions away.

      If enabled, when the conversation returns from the digression, the prompt for the next unfilled slot is displayed to encourage the user to continue providing information. If disabled, then any inputs that the user submits which do not contain a value that can fill a slot are ignored. However, you can address unsolicited questions that you anticipate your users might ask while they interact with the node by defining slot handlers. See [Adding slots](/docs/assistant?topic=assistant-dialog-slots#dialog-slots-add) for more information.

      The following image shows you how digressions away from the #reservation node with slots (shown in the earlier illustration) are configured.

      ![Shows the digressions away settings from a node with slots.](images/digress-away-slots-full.png)

    - **Nodes with slots**: Choose whether the user is only allowed to digress away if they will return to the current node by selecting the **Only digress from slots to nodes that allow returns** checkbox.

      When selected, as the dialog looks for a node to answer the user's unrelated question, it ignores any root nodes that are not configured to return after the digression. Select this checkbox if you want to prevent users from being able to permanently leave the node before they have finished filling the required slots.

    **Digressions into this node**

    You can make the following choices about how digressions into a node behave:

    - Prevent users from being able to digress into the node. See [Disabling digressions into a root node](#dialog-runtime-disable-digressions) for more details.

    - When digressions into the node are enabled, choose whether the dialog must go back to the dialog flow that it digressed away from. When selected, after the current node's branch is done being processed, the dialog flow goes back to the interrupted node. To make the dialog return afterwards, select **Return after digression**.

    The following image shows you how digressions into the #cuisine node (shown in the earlier illustration) are configured.

    ![Shows the digressions away settings from a node with slots.](images/digress-into-cuisine-full.png)

1.  Click **Apply**.

1.  Use the "Try it out" pane to test the digression behavior.

    Again, you cannot define the start and end of a digression. The user controls where and when digressions happen. You can only apply settings that determine how a single node participates in one. Because digressions are so unpredictable, it is hard to know how your configuration decisions will impact the overall conversation. To truly see the impact of the choices you made, you must test the dialog.

The #reservation and #cuisine nodes represent two dialog branches that can participate in a single user-directed digression. The digression settings that are configured for each individual node are what make this type of digression possible at run time.

![Shows two dialogs, one that sets the digressions away from the reservation slots node and one that sets the digression into the cuisine node.](images/digression-settings.png)

### Digression usage tips
{: #dialog-runtime-digress-tips}

This section describes solutions to situations that you might encounter when using digressions.

- **Custom return message**: For any nodes where you enable returns from digressions away, consider adding wording that lets users know they are returning to where they left off in a previous dialog flow. In your text response, use a special syntax that lets you add two versions of the response.

  If you do not take action, the same text response is displayed a second time to let users know they have returned to the node they digressed away from. You can make it clearer to users that they have returned to the original conversation thread by specifying a unique message to be displayed when they return.

  For example, if the original text response for the node is, `What's the order number?`, then you might want to display a message like, `Now let's get back to where we left off. What is the order number?` when users return to the node.

  To do so, use the following syntax to specify the node text response:

  `<? (returning_from_digression)? "post-digression message" : "first-time message" ?>`

  For example:

  ```bash
  <? (returning_from_digression)? "Now, let's get back to where we left off.
  What is the order number?" : "What's the order number?" ?>
  ```
  {: codeblock}

  You cannot include SpEL expressions or shorthand syntax in the text responses that you add. In fact, you cannot use shorthand syntax at all. Instead, you must build the message by concatenating the text strings and full SpEL expression syntax together to form the full response.
  {: note}
  
  For example, use the following syntax to include a context variable in a text response that you would normally specify as, `What can I do for you, $username?`:

  ```bash
  <? (returning_from_digression)? "Where were we, " +
  context["username"] + "? Oh right, I was asking what can I do
  for you today." : "What can I do for you today, " +
  context["username"] + "?" ?>
  ```

  For full SpEL expression syntax details, see [Expression for accessing objects](/docs/assistant?topic=assistant-expression-language#expression-language-shorthand-syntax).

- **Preventing returns**: In some cases, you might want to prevent a return to the interrupted conversation flow based on a choice the user makes in the current dialog flow. You can use special syntax to prevent a return from a specific node.

  For example, you might have a node that conditions on `#General_Connect_To_Agent` or a similar intent. When triggered, if you want to get the user's confirmation before you transfer them to an external service, you might add a response such as, `Do you want me to transfer you to an agent now?` You could then add two child nodes that condition on `#yes` and `#no` respectively.
  
  The best way to manage digressions for this type of branch is to set the root node to allow digression returns. However, on the `#yes` node, include the SpEL expression `<? clearDialogStack() ?>` in the response. For example:
  
    ```bash
  OK. I will transfer you now. <? clearDialogStack() ?>
  ```
  {: codeblock}

  This SpEL expression prevents the digression return from happening from this node. When a confirmation is requested, if the user says yes, the proper response is displayed, and the dialog flow that was interrupted is not resumed. If the user says no, then the user is returned to the flow that was interrupted.

### Disabling digressions into a root node
{: #dialog-runtime-disable-digressions}

When a flow digresses into a root node, it follows the course of the dialog that is configured for that node. So, it might process a series of child nodes before it reaches the end of the node branch, and then, if configured to do so, goes back to the dialog flow that was interrupted. Through dialog testing, you might find that a root node is triggered too often, or at unexpected times, or that its dialog is too complex and leads the user too far off course to be a good candidate for a temporary digression. If you determine that you would rather not allow users to digress into it, you can configure the root node to not allow digressions in.

To disable digressions into a root node altogether, complete the following steps:

1.  Click to open the root node that you want to edit.
1.  Click **Customize**, and then click the **Digressions** tab.
1.  Set the **Allow digressions into this node** switch to **Off**.
1.  Click **Apply**.

If you decide that you want to prevent digressions into several root nodes, but do not want to edit each one individually, you can add the nodes to a folder. From the *Customize* page of the folder, you can set the **Allow digressions into this node** switch to **Off** to apply the configuration to all of the nodes at once. See [Organizing the dialog with folders](/docs/assistant?topic=assistant-dialog-tasks#dialog-tasks-folders) for more information.

### Digression tutorial
{: #dialog-runtime-digression-tutorial}

Follow the [tutorial](/docs/assistant?topic=assistant-tutorial-digressions) to import a workspace that has a set of nodes already defined. You can walk through some exercises that illustrate how digressions work.

### Design considerations
{: #dialog-runtime-digression-design-considerations}

- **Avoid fallback node proliferation**: Many dialog designers include a node with a `true` or `anything_else` condition at the end of every dialog branch as a way to prevent users from getting stuck in the branch. This design returns a generic message if the user input does not match anything that you anticipated and included a specific dialog node to address. However, users cannot digress away from dialog flows that use this approach.

  Evaluate any branches that use this approach to determine whether it would be better to allow digressions away from the branch. If the user's input does not match anything you anticipated, it might find a match against an entirely different dialog flow in your tree. Rather than responding with a generic message, you can effectively put the rest of the dialog to work to try to address the user's input. And the root-level `Anything else` node can always respond to input that none of the other root nodes can address.

- **Reconsider jumps to a closing node**: Many dialogs are designed to ask a standard closing question, such as, `Did I answer your question today?` Users cannot digress away from nodes that are configured to jump to another node. So, if you configure all of your final branch nodes to jump to a common closing node, digressions cannot occur. Consider tracking user satisfaction through metrics or some other means.

- **Test possible digression chains**: If a user digresses away from the current node to another node that allows digressions away, the user could potentially digress away from that other node, and repeat this pattern one or more times again. If the starting node in the digression chain is configured to return after the digression, then the user will eventually be brought back to the current dialog node. In fact, any subsequent nodes in the chain that are configured not to return are excluded from being considered as digression targets. Test scenarios that digress multiple times to determine whether individual nodes function as expected.

- **Remember that the current node gets priority**: Remember that nodes outside the current flow are only considered as digression targets if the current flow cannot address the user input. It is even more important in a node with slots that allows digressions away, in particular, to make it clear to users what information is needed from them, and to add confirmation statements that are displayed after the user provides a value.

  Any slot can be filled during the slot-filling process. So, a slot might capture user input unexpectedly. For example, you might have a node with slots that collects the information necessary to make a dinner reservation. One of the slots collects date information. While providing the reservation details, the user might ask, `What's the weather meant to be tomorrow?` You might have a root node that conditions on #forecast which could answer the user. However, because the user's input includes the word `tomorrow` and the reservation node with slots is being processed, your assistant assumes the user is providing or updating the reservation date instead. *The current node always gets priority.* If you define a clear confirmation statement, such as, `Ok, setting the reservation date to tomorrow,` the user is more likely to realize there was a miscommunication and correct it.

  Conversely, while filling slots, if the user provides a value that is not expected by any of the slots, there is a chance it will match against a completely unrelated root node that the user never intended to digress to.

  Be sure to do lots of testing as you configure the digression behavior.

- **When to use digressions instead of slot handlers**: For general questions that users might ask at any time, use a root node that allows digressions into it, processes the input, and then goes back to the flow that was in progress. For nodes with slots, try to anticipate the types of related questions users might want to ask while filling in the slots, and address them by adding handlers to the node.

  For example, if the node with slots collects the information required to fill out an insurance claim, then you might want to add handlers that address common questions about insurance. However, for questions about how to get help, or your stores locations, or the history of your company, use a root level node.

## Correcting user input
{: #dialog-runtime-spell-check}

*Autocorrection* fixes misspellings that users make in the utterances that they submit as user input. When autocorrection is enabled, the misspelled words are automatically corrected. And it is the corrected words that are used to evaluate the input. When given more precise input, your assistant can more often recognize entity mentions and understand the user's intent.

Autocorrection is enabled automatically for all new English-language dialog skills. It is available as a beta feature in French-language dialog skills. You must turn it on to use it with French-language skills. For more information about language support, see [Supported languages](/docs/assistant?topic=assistant-language-support).
{: note}

Autocorrection corrects user input in the following way:

- Orignal input: `letme applt for a memberdhip`
- Corrected input: `let me apply for a membership`

When your assistant evaluates whether to correct the spelling of a word, it does not rely on a simple dictionary lookup process. Instead, it uses a combination of Natural Language Processing and probabalistic models to assess whether a term is, in fact, misspelled and should be corrected.

### Turning autocorrection on or off
{: #dialog-runtime-spell-check-disable}

Autocorrection helps your assistant understand user input. It is enabled automatically for some languages and available, but disabled in others.

If you decide to disable it, you must turn it off entirely. You cannot disable autocorrection for a single word or phrase. 

If you find that a domain-specific term is being corrected that shouldn't be, you can prevent the correction from happening by adding the term or phrase to your training data. For more details, see [Autocorrection rules](#dialog-runtime-spell-check-rules).

To turn autocorrection on or off, complete the following steps:

1.  Click the **Skills** icon ![Skills menu icon](images/nav-skills-icon.png), and then open your skill.
1.  From the Skills menu, click **Options**, and then click **Autocorrection**.
1.  Click the switch to enable or disable the feature.

### Testing autocorrection
{: #dialog-runtime-spell-check-test}

1.  From the "Try it out" pane, submit an utterance that includes some misspelled words.

    If words in your input are misspelled, they are corrected automatically, and an ![auto-correct](images/auto-correct.png) icon is displayed. The corrected utterance is underlined.
1.  Hover over the underlined utterance to see the original wording.

If there are misspelled terms that you expected your assistant to correct, but it did not, then review the rules that your assistant uses to decide whether to correct a word to see if the word falls into the category of words that your assistant intentionally does not change.

### Autocorrection rules
{: #dialog-runtime-spell-check-rules}

To avoid overcorrection, your assistant does not correct the spelling of the following types of input:

- Capitalized words
- Emojis
- Location entities, such as states and street addresses
- Numbers and units of measurement or time
- Proper nouns, such as common first names or company names
- Text within quotation marks
- Words containing special characters, such as hyphens (-), asterisks (*), ampersands (&), or at signs (@), including those used in email addresses or URLs.
- Words that *belong* in this skill, meaning words that have implied significance because they occur in entity values, entity synonyms, or intent user examples.

  Mentions of a contextual entity can be corrected inadvertently. That's because terms that function as contextual entity mentions are fluid; they cannot be predetermined and avoided by the spell checker function in the way a list of dictionary-based terms can be.
  {: note}

If the word that is not corrected is not obviously one of these types of input, then it might be worth checking whether the entity has fuzzy matching enabled for it.

#### How is spelling autocorrection related to fuzzy matching?
{: #dialog-runtime-spell-check-vs-fuzzy-matching}

Fuzzy matching helps your assistant recognize dictionary-based entity mentions in user input. It uses a dictionary lookup approach to match a word from the user input to an existing entity value or synonym in the skill's training data. For example, if the user enters `boook`, and your training data contains a `@reading_material` entity with a `book` value, then fuzzy matching recognizes that the two terms (`boook` and `book`) mean the same thing.

When you enable both autocorrection and fuzzy matching, the fuzzy matching function runs before autocorrection is triggered. If it finds a term that it can match to an existing dictionary entity value or synonym, it adds the term to the list of words that *belong* to the skill, and does not correct it.

For example, if a user enters a sentence like `I wnt to buy a boook`, fuzzy matching recognizes that the term `boook` means the same thing as your entity value `book`, and adds it to the protected words list. Your assistant corrects the input to be, `I want to buy a boook`. Notice that it corrects `wnt` but does *not* correct the spelling of `boook`. If you see this type of result when you are testing your dialog, you might think your assistant is misbehaving. However, your assistant is not. Thanks to fuzzy matching, it correctly identifies `boook` as a `@reading_material` entity mention. And thanks to autocorrection revising the term to `want`, your assistant is able to map the input to your `#buy_something` intent. Each feature does its part to help your assistant understand the meaning of the user input.

#### How autocorrection works
{: #dialog-runtime-spell-check-how-it-works}

Normally, user input is saved as-is in the `text` field of the `input` object of the message. If, and only if the user input is corrected in some way, a new field is created in the `input` object, called `original_text`. This field stores the user's original input that includes any misspelled words in it. And the corrected text is added to the `input.text` field.

If you want to ask users to confirm the assistant's understanding of their meaning, you can do so in a way that takes into account that their input might have been corrected. Set the condition for the dialog node or conditional response that is asking for confirmation to `original_text`. This means that if the user's input was automatically corrected, show the corresponding response. And the response can contain the expression: `You said: <? input.original_text ?>. Did you mean: <? input.text ?>?`

Remember, the `input.text` field stores either the never-corrected original text from the user or the user’s text after it is corrected. The `input.original_text` field is only created if the input is corrected. And the user’s incorrect input is stored in it.”

## Disambiguation
{: #dialog-runtime-disambiguation}

Disambiguation instructs your assistant to ask the customer for help when more than one dialog node can respond to a customer's input. Instead of guessing which node to process, your assistant shares a list of the top node options with the user, and asks the user to pick the right one.

![Shows a sample conversation between a user and the assistant, where the assistant asks for clarification from the user.](images/disambig-demo.png)

Disambiguation is triggered when the following conditions are met:

- The confidence scores of the runner-up intents that are detected in the user input are close in value to the confidence score of the top intent.
- The confidence score of the top intent is above 0.2.

Even when these conditions are met, disambiguation does not occur unless two or more independent nodes in your dialog meet the following criteria:

- The node condition includes one of the intents that triggered disambiguation.
- A description of the node's purpose is provided for the node in the node name field. (Alternatively, a description can be included in the external node name field.)

A node with a boolean node condition that evaluates to true is likely to be included in the disambiguation list. For example, if the node checks for an entity type and the entity is mentioned in the user input, it is eligible to be included in the list. Such nodes do not trigger disambiguation, but if disambiguation is triggered, they are likely to be included in the resulting disambiguation list.

Learn more

- [Disambiguation example](#dialog-runtime-disambig-example)
- [Editing the disambiguation configuration](#dialog-runtime-disambig-edit)
- [Choosing nodes to disable](#dialog-runtime-disambig-choose-nodes)
- [Disabling disambiguation](#dialog-runtime-disambig-disable)
- [Handling none of the above](#dialog-runtime-handle-none)
- [Testing disambiguation](#dialog-runtime-disambig-test)

### Disambiguation example
{: #dialog-runtime-disambig-example}

For example, you have a dialog that has two nodes with intent conditions that address cancellation requests. The conditions are:

- eCommerce_Cancel_Product_Order
- Customer_Care_Cancel_Account

If the user input is `i must cancel it today`, then the following intents might be detected in the input:

```json
[
  {"intent":"Customer_Care_Cancel_Account","confidence":0.5602024316787719},
  {"intent":"eCommerce_Cancel_Product_Order","confidence":0.46903514862060547},
  {"intent":"Customer_Care_Appointments","confidence":0.29033891558647157},
  {"intent":"General_Greetings","confidence":0.2894785046577454},
```
{: code block}

In fact, if you test from the "Try it out" pane, you can hover over the eye icon to see the top three intents that were recognized in the test input.

![Shows the top 3 intents recognized in the user input from the Try it out pane.](images/tryit-disambig-intent-details.png)

Your assistant is `0.5618281841278076` (56%) confident that the user goal matches the `#Customer_Care_Cancel_Account` intent. If another intent has a confidence score that is close to the score of this top intent, then disambiguation is triggered. In this example, the `#eCommerce_Cancel_Product_Order` intent has a close confidence score of 46%.

As a result, when the user input is `i must cancel it today`, both dialog nodes are likely to be considered viable candidates to respond. To determine which dialog node to process, the assistant asks the user to pick one. And to help the user choose between them, the assistant provides a short summary of what each node does. The summary text is extracted directly from the node's *name* field. If present and if a description is added to it, then the text is taken from the *external node name* field instead.

The description that is displayed in the disambiguation list comes from the name (or external node name) of the last node that is processed in the branch where the intent match occurs. For more information, see [Disable jumped-to utility nodes](#dialog-runtime-disambig-choose-nodes).
{: note}

![Service prompts the user to choose from a list of dialog options, including Cancel an account, Cancel a product order, and None of the above.](images/disambig-tryitout.png)

Notice that your assistant recognizes the term `today` in the user input as a date, a mention of the `@sys-date` entity. If your dialog tree contains a node that conditions on the `@sys-date` entity, then it is also likely to be included in the list of disambiguation choices. This image shows it included in the list as the *Capture date information* option.

![Service prompts the user to choose from a list of dialog options, including Capture date information.](images/disambig-tryitout-date.png)

The following video explains the benefits of using disambiguation. A few things have changed since the video was created:

- You enable disambiguation from the *Options* page instead of a **Settings** link from the *Dialog* page.
- You can also set a maximum number of options to display in the disambiguation list.

  ![Disambiguation overview](https://www.youtube.com/embed/VVyklAXlmbA){: video output="iframe" id="youtubeplayer0" frameborder="0" width="560" height="315" webkitallowfullscreen mozallowfullscreen allowfullscreen}

### Editing the disambiguation configuration
{: #dialog-runtime-disambig-edit}

Disambiguation is enabled automatically for all new dialog skills. You can change the settings that are applied automatically to disambiguation from the *Options* page. 

To edit the disambiguation settings, complete the following steps:

1.  From the Skills menu, click **Options**.
1.  Click *Disambiguation*.
1.  In the **Disambiguation message** field, add text to show before the list of dialog node options. For example, *What do you want to do?*
1.  In the **Anything else** field, add text to display as an additional option that users can pick if none of the other dialog node options reflect what the user wants to do. For example, *None of the above*.

    Keep the message short, so it displays inline with the other options. The message must be less than 512 characters. For information about what your assistant does if a user chooses this option, see [Handling none of the above](#dialog-runtime-handle-none).

1.  If you want to limit the number of disambiguation options that can be displayed to a user, then in the **Maximum number of suggestions** field, specify a number between 2 and 5. 

    Your changes are automatically saved.

Next, you must decide which dialog nodes you want to make eligible for disambiguation. From the Skills menu, click **Dialog**.

### Choosing nodes to disable
{: #dialog-runtime-disambig-choose-nodes}

All nodes are eligible to be included in the disambiguation list. 

  - Nodes at any level of the tree hierarchy are included.
  - Nodes that condition on intents, entities, special conditions, context variables, or any combination of these values are included.

Consider preventing some nodes from being included in the list by disabling disambiguation on them.

- **Disable root nodes with `welcome` and `anything_else` conditions**

  Unless you added extra functionality to these nodes, they typically are not useful options to include in a disambiguation list.

- **Disable jumped-to utility nodes**

  The text that is displayed in the disambiguation list is populated from the node name (or external node name) of the *last node that is processed* in the branch where the node condition is matched.
  {: important}

  You do not want the name of a utility node, such as one that thanks the user, or says goodbye, or asks for feedback on answer quality to be shown in the disambiguation list instead of a phrase that describes the purpose of the matched root node.
  
  For example, maybe a root node with the matching intent condition of `#store-location` jumps to a node that asks users if they are satisfied with the response. If the `#check_satisfaction` node has a node name and has disambiguation enabled, then the name for that jumped-to node is displayed in the disambiguation list. As a result, `Check satisfaction` is displayed in the disambiguation list to represent the `#store-location` branch instead of the `Get store location` name from the root node. Prevent a jumped-to node from misrepresenting a disambiguation list option by disabling disambiguation on jumped-to nodes.

- **Disable root nodes that condition on an entity or context variable only**

  Again, unless you have a specific function in mind, disable disambiguation on these root nodes. While only a node with a matched intent can trigger disambiguation, once it's triggered, any node with a condition that matches is included in the disambiguation list. When such nodes opt in to disambiguation, the order of nodes in the tree hierarchy can become significant in unexpected ways.

  - The order of nodes impacts whether disambiguation is triggered at all
  
    Look at the [scenario](#dialog-runtime-disambig-example) that is used earlier to introduce disambiguation, for example. If the node that conditions on `@sys-date` was placed higher in the dialog tree than the nodes that condition on the `#Customer_Care_Cancel_Account` and `#eCommerce_Cancel_Product_Order` intents, disambiguation would never be triggered when a user enters, `i must cancel it today`. That's because your assistant would consider the date mention (`today`) to be more important than the intent references due to the placement of the corresponding nodes in the tree.

  - The order of nodes impacts which nodes are included in the disambiguation options list
  
    Sometimes a node is not listed as a disambiguation option as expected. This can happen if a condition value is also referenced by a node that is *not* eligible for inclusion in the disambiguation list for some reason. For example, an entity mention might trigger a node that is situated earlier in the dialog tree but is not enabled for disambiguation. If the same entity is the only condition for a node that *is* enabled for disambiguation, but is situated lower in the tree, then it might not be added as a disambiguation option because your assistant never reaches it. If it matched against the earlier node and was omitted, your assistant might not process the later node.

### Disabling disambiguation
{: #dialog-runtime-disambig-disable}

You can disable disambiguation for the entire dialog or for an individual dialog node.

- To disable disambiguation entirely: 

  - From the Skills menu, click **Options**. 
  - On the *Disambiguation* page, set the switch to **Off**.

- To disable disambiguation for a single dialog node: 

  - From the Skills menu, click **Dialog**. Click the node to open it in the edit view.

    ![Shows the Settings link after the description text for the node name field.](images/disambig-node-setting.png)

  - Click **Settings**.

    ![Shows the expanded disambiguation settings section where the toggle is.](images/disambig-node-level-toggle.png)

  - Set the **Show node name** switch to **Off**.

  - ![Plus or Premium plan only](images/plus.png) If you added a node summary description to the **external node name** field instead of the *name* field, remove it.
  
    The *external node name* field serves two purposes. It provides information about the node to customers when it is included in a disambiguation list. It also describes the node in a chat summary that is shared with service desk agents when a conversation is transferred to a person. The *external node name* field is only visible in skills that are part of a Plus or Premium plan instance. If the *external node name* field contains text, its text is used, whether or not there is text in the *name* field.

    ![Shows the external node name field.](images/disambig-external-node-name.png)    

For each node, test scenarios in which you expect the node to be included in the disambiguation options list. Testing gives you a chance to make adjustments to the node order or other factors that might impact how well disambiguation works at run time. See [Testing disambiguation](#dialog-runtime-disambig-test).

{{site.data.keyword.conversationshort}} can recognize intent conflicts, which occur when two or more intents have user examples that overlap. [Resolve any such conflicts](/docs/assistant?topic=assistant-intents#intents-resolve-conflicts) first to ensure that the intents themselves are as unique as possible, which helps your assistant attain better intent confidence scores.
{: tip}

### Handling none of the above
{: #dialog-runtime-handle-none}

When a user clicks the *None of the above* option, your assistant strips the intents that were recognized in the user input from the message and submits it again. This action typically triggers the anything else node in your dialog tree.

To customize the response that is returned in this situation, you can add a root node with a condition that checks for a user input with no recognized intents (the intents are stripped, remember) and contains a `suggestion_id` property. A `suggestion_id` property is added by your assistant when disambiguation is triggered.
{: tip}

Add a root node with the following condition:

```json
intents.size()==0 && input.suggestion_id
```
{: codeblock}

This condition is met only by input that has triggered a set of disambiguation options of which the user has indicated none match her goal.

Add a response that lets users know that you understand that none of the options that were suggested met their needs, and take appropriate action.

Again, the placement of nodes in the tree matters. If a node that conditions on an entity type that is mentioned in the user input is higher in the tree than this node, its response will be displayed instead.

### Testing disambiguation
{: #dialog-runtime-disambig-test}

When testing, the order in which the options are listed might change from test run to test run. In fact, the options themselves that are included in the disambiguation list might change from one test run to the next.
{: important}

This behavior is intended. As part of development that is in progress to help the assistant learn automatically from user choices, the choices included and their order in the disambiguation list is being randomized on purpose. Changing the order helps to avoid bias that can be introduced by a percentage of people who always pick the first option without carefully reviewing all of their choices beforehand.

To test disambiguation, complete the following steps:

1.  From the "Try it out" pane, enter a test utterance that you think is a good candidate for disambiguation, meaning two or more of your dialog nodes are configured to address utterances like it.

1.  If the response does not contain a list of dialog node options for you to choose from as expected, first check that you added summary information to the node name (or external node name) field for each of the nodes.

1.  If disambiguation is still not triggered, it might be that the confidence scores for the nodes are not as close in value as you thought.

    - To see the confidence scores of the top three intents that were detected in the input, hover over the eye icon in the "Try it out" pane.

    - To see the confidence scores of all the intents that are detected in the user input, temporarily add `<? intents ?>` to the end of the node response for a node that you know will be triggered.

      This SpEL expression shows the intents that were detected in the user input as an array. The array includes the intent name and the level of confidence that your assistant has that the intent reflects the user's intended goal.

    - To see which entities, if any, were detected in the user input, you can temporarily replace the current response with a single text response that contains the SpEL expression, `<? entities ?>`.

      This SpEL expression shows the entities that were detected in the user input as an array. The array includes the entity name, location of the entity mention within the user input string, the entity mention string, and the level of confidence that your assistant has that the term is a mention of the entity type specified.

    - To see details for all of the artifacts at once, including other properties, such as the value of a given context variable at the time of the call, you can inspect the entire API response. See [Viewing API call details](/docs/assistant?topic=assistant-dialog-tips#dialog-tips-inspect-api).

1.  Temporarily remove the description you added to the *name* field (or *external node name* field) for at least one of the nodes that you anticipate will be listed as a disambiguation option.

1.  Enter the test utterance into the "Try it out" pane again.

    If you added the `<? intents ?>` expression to the response, then the text returned includes a list of the intents that your assistant recognized in the test utterance, and includes the confidence score for each one.

    ![Service returns an array of intents, including Customer_Care_Cancel_Account and eCommerce_Cancel_Product_Order.](images/disambig-show-intents.png)

After you finish testing, remove any SpEL expressions that you appended to node responses, or add back any original responses that you replaced with expressions, and repopulate any *name* or *external node name* fields from which you removed text.
