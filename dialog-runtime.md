---

copyright:
  years: 2015, 2018
lastupdated: "2018-07-31"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}
{:gif: data-image-type='gif'}

# How the dialog is processed
{: #dialog-runtime}

Understand how your dialog is processed when a person interacts with your instance of the deployed {{site.data.keyword.conversationshort}} service at run time.
{: shortdesc}

## Anatomy of a dialog call
{: message-anatomy}

Each user utterance is passed to the dialog as a /message API call. This includes utterances that users make in reply to prompts from the dialog that ask them for more information. Some subscription plans include a set number of API calls, so it helps to understand what constitutes a call. A single /message API call is equivalent to a single dialog turn, which consists of an input from the user and a corresponding response from the dialog.

The body of the /message API call request and response includes the following objects:

- `context`: Contains variables that are meant to be persisted. To pass information from one call to the next, the application developer must pass the previous API call's response context in with each subsequent API call. For example, the dialog can collect the user's name and then refer to the user by name in subsequent nodes.

  ```json
  {
    "context" : {
      "user_name" : "<? @sys-person.literal ?>"
    }
  ```
  {: codeblock}

  See [Retaining information across dialog turns](dialog-runtime.html#context) for more information.

- `input`: The string of text that was submitted by the user. The text string can contain up to 2,048 characters.

  ```json
  {
    "input" : {
      "text" : "Where's your nearest store?"
    }
  ```
  {: codeblock}

- `output`: The dialog response to return to the user.

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

**Note**: The following `output` object format is supported for backwards compatibility. Any workspaces that specify a text response by using this format will continue to function properly. With the introduction of rich response types, the `output.text` structure was augmented with the `output.generic` structure to facilitate supporting other types of responses in addition to text. Use the new format when you create new nodes to give yourself more flexibility, because you can subsequently change the response type, if needed.

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

There are response types other than a text response that you can define. See [Responses](dialog-overview.html#responses) for more details.

You can learn more about the /message API call from the [API reference ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/conversation/api/v1/){: new_window}.

### Retaining information across dialog turns
{: #context}

The dialog in a conversational skill is stateless, meaning that it does not retain information from one interaction with the user to the next. When you add a conversational skill to an assistant and deploy it, the assistant saves the context from one message call and then re-submits it on the next request throughout the current session. (The current session lasts for as long a user interacts with the assistant and then up to 60 minutes of inactivity.) If you do not add the conversational skill to an assistant, it is your responsibility as the custom application developer to maintain any continuing information that the application needs. The application must look for, and store the context object in the message API response, and pass it in the context object with the next /message API request that is made as part of the conversation flow.

One way to retain the information yourself is to store the entire context object in memory in the client application, in a web browser, for example. As an application becomes more complex, or if it needs to pass and store personally identifiable information, then you can store and retrieve the information from a database. Of course, the simplest approach is one that prevents you from having to store context at all. To implement this approach, add the conversational skill to an assistant and let the assistant keep track of the context for you.

The application can pass information to the dialog, and the dialog can update this information and pass it back to the application, or to a subsequent node. The dialog does so by using *context variables*.

## Context variables
{: #context-variables}

A context variable is a variable that you define in a node. You can specify a default value for it. Other nodes, application logic, or user input can subsequently set or change the value of the context variable.

You can condition against context variable values by referencing a context variable from a dialog node condition to determine whether to execute a node. You can also reference a context variable from dialog node response conditions to show different reponses depending on a value provided by an external service or by the user.

Learn more:

- [Passing context from the application](#context-from-app)
- [Passing context from node to node](#context-node-to-node)
- [Defining a context variable](#context-var-define)
- [Common context variable tasks](#context-common-tasks)
- [Deleting a context variable](#context-delete)
- [Updating a context variable](#context-update)
- [How context variables are processed](#context-processing)
- [Order of operation](#context-order-of-ops)
- [Adding context variables to a node with slots](#context-var-slots)

### Passing context from the application
{: #context-from-app}

Pass information from the application to the dialog by setting a context variable and passing the context variable to the dialog.

For example, your application can set a $time_of_day context variable, and pass it to the dialog which can use the information to tailor the greeting it displays to the user.

![Shows a Welcome node that uses response conditions to check for the value of the $time_of_day context variable that is passed to the dialog from the application.](images/set-context.png)

In this example, the dialog knows that the application sets the variable to one of these values: *morning*, *afternoon*, or *evening*. It can check for each value, and depending on which value is present, return the appropriate greeting. If the variable is not passed or has a value that does not match one of the expected values, then a more generic greeting is displayed to the user.

### Passing context from node to node
{: #context-node-to-node}

The dialog can also add context variables to pass information from one node to another or to update the values of context variables. As the dialog asks for and gets information from the user, it can keep track of the information and reference it later in the conversation.

For example, in one node you might ask users for their name, and in a later node address them by name.

![Shows an introductions node that asks the user for their name, and stores it as a context variable. The next node refers to the user by name by using the $username context variable.](images/set-context-username.png)

In this example, the system entity @sys-person is used to extract the user's name from the input if the user provides one. In the JSON editor, the username context variable is defined and set to the @sys-person value. In a subsequent node, the $username context variable is included in the response to address the user by name.

### Defining a context variable
{: #context-var-define}

Define a context variable by adding the variable name to the **Variable** field and adding a default value for it to the **Value** field in the node's edit view.

1.  Click to open the dialog node to which you want to add a context variable.

1.  Click the **Options**  ![Advanced response](images/kabob.png) icon that is associated with the node response, and then click **Open context editor**.

      ![Shows how to access the JSON editor associated with a standard node response.](images/contextvar-json-response.png)

      If the **Multiple responses** setting is **On** for the node, then you must first click the **Edit response** ![Edit response](images/edit-slot.png) icon for the response with which you want to associate the context variable.

      ![Shows how to access the JSON editor associated with a standard node that has multiple conditional responses enabled for it.](images/contextvar-json-multi-response.png)

1.  Add the variable name and value pair to the **Variable** and **Value** fields.

    - The `name` can contain any upper- and lowercase alphabetic characters, numeric characters (0-9), and underscores.

      You can include other characters, such as periods and hyphens, in the name. However, if you do, then you must specify the shorthand syntax `$(variable-name)` every time you subsequently reference the variable. See [Expressions for accessing objects](expression-language.html#shorthand-context) for more details.
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

You can use the JSON editor to define context variables also. You might prefer to use the JSON editor if you want to add a complex expression as the variable value. See [Context variables in the JSON editor](dialog-runtime.html#context-var-json) for more details.

### Common context variable tasks
{: #context-common-tasks}

To store the entire string that was provided by the user as input, use `input.text`:

| Variable | Value            |
|----------|------------------|
| repeat   | `<?input.text?>` |

For example, the user input is, `I want to order a device.` If the node response is, `You said: $repeat`, then the response would be displayed as, `You said: I want to order a device.`

To store the value of an entity in a context variable, use this syntax:

| Variable | Value            |
|----------|------------------|
| place    | `@place`         |

For example, the user input is, `I want to go to Paris.` If your @place entity recognizes `Paris`, then the service saves `Paris` in the `$place` context variable.

To store the value of a string that you extract from the user's input, you can include a SpEL expression that uses the `extract` method to apply a regular expression to the user input. The following expression extracts a number from the user input, and saves it to the `$number` context variable.

| Variable | Value                               |
|----------|-------------------------------------|
| number   | `<?input.text.extract('[\d]+',0)?>` |

To store the value of a pattern entity, append .literal to the entity name. Using this syntax ensures that the exact span of text from user input that matched the specified pattern is stored in the variable.

| Variable | Value                  |
|----------|------------------------|
| email    | `<? @email.literal ?>` |

For example, the user input is `Contact me at joe@example.com.` Your entity named `@email` recognizes the `name@domain.com` email format. By configuring the context variable to store `@email.literal`, you indicate that you want to store the part of the input that matched the pattern. If you omit the `.literal` property from the value expression, then the entity value name that you specified for the pattern is returned instead of the segment of user input that matched the pattern.

Many of these value examples use methods to capture different parts of the user input. For more information about the methods available for you to use, see [Expression language methods](dialog-methods.html).

### Deleting a context variable
{: #context-delete}

To delete a context variable, set the variable to null.

| Variable   | Value            |
|------------|------------------|
| order_form | `null`           |

Alternatively you can delete the context variable in your application logic. For information about how to remove the variable entirely, see [Deleting a context variable in JSON](dialog-runtime.html#context-delete-json).

### Updating a context variable value
{: #context-update}

To update a context variable's value, define a context variable with the same name as the previous context variable, but this time, specify a different value for it.

When more than one node sets the value of the same context variable, the value for the context variable can change over the course of a conversation with a user. Which value is applied at any given time depends on which node is being triggered by the user in the course of the conversation. The value specified for the context variable in the last node that is processed overwrites any values that were set for the variable by nodes that were processed previously.

For information about how to update the value of a context variable when the value is a JSON object or JSON array data type, see [Updating a context variable value in JSON](dialog-runtime.html#context-update-json)

### How context variables are processed
{: #context-processing}

Where you define the context variable matters. The context variable is not created and set to the value that you specify for it until the service processes the part of the dialog node where you defined the context variable. In most cases, you define the context variable as part of the node response. When you do so, the context variable is created and given the specified value when the service returns the node response.

For a node with conditional responses, the context variable is created and set when the condition for a specific response is met and that response is processed. For example, if you define a context variable for conditional response #1 and the service processes conditional response #2 only, then the context variable that you defined for conditional response #1 is not created and set.

For information about where to add context variables that you want the service to create and set as a user interacts with a node with slots, see [Adding context variables to a node with slots](dialog-runtime.html#context-var-slots).

### Order of operation
{: #context-order-of-ops}

When you define multiple variables to be processed together, the order in which you define them does not determine the order in which they are evaluated by the service. The service evaluates the variables in random order. Do not set a value in the first context variable in the list and expect to be able to use it in the second variable in the list, because there is no guarantee that the first context variable will be executed before the second one. For example, do not use two context variables to implement logic that checks whether the user input contains the word `Yes` in it.

| Variable        | Value            |
|-----------------|------------------|
| user_input      | <? input.text ?> |
| contains_yes    | <? $user_input.contains('Yes') ?> |

Instead, use a slightly more complex expression to avoid having to rely on the value of the first variable in your list (user_input) being evaluated before the second variable (contains_yes) is evaluated.

| Variable      | Value            |
|---------------|------------------|
| contains_yes  | <? input.text.contains('Yes') ?> |

### Adding context variables to a node with slots
{: #context-var-slots}

For more information about slots, see [Gathering information with slots](dialog-slots.html).

1.  Open the node with slots in the edit view.

    - To add a context variable that is processed after a response condition for a slot is met, perform the following steps:

      1.  Click the **Edit slot** ![Edit response](images/edit-slot.png) icon.
      1.  Click the **Options** ![Advanced response](images/kabob.png) icon, and then select **Enable conditional responses**.
      1.  Click the **Edit response** ![Edit response](images/edit-slot.png) icon next to the response with which you want to associate the context variable.
      1.  Click the **Options** ![Advanced response](images/kabob.png) icon in the response section, and then click **Open context editor**.
      1.  Add the variable name and value pair to the **Variable** and **Value** fields.

      ![Shows how to access the JSON editor associated with the conditional response for a slot.](images/contextvar-json-slot-multi-response.png)

    - To add a context variable that is set or updated after a slot condition is met, complete the following steps:

      1.  Click the **Edit slot** ![Edit response](images/edit-slot.png) icon.
      1.  From the **Options** ![Advanced response](images/kabob.png) menu in the *Configure slot* view header, click **Open JSON editor**.
      1.  Add the variable name and value pair in JSON format.

          ```json
          {
            "time_of_day": "morning"
          }
          ```
          {: codeblock}

      **Note**: There is currently no way to use the context editor to define context variables that are set during this phase of dialog node evaluation. You must use the JSON editor instead. For more information about using the JSON editor, see [Context variables in the JSON editor](dialog-runtime.html#context-var-json).

      ![Shows how to access the JSON editor associated with a slot condition.](images/contextvar-json-slot-condition.png)

## Context variables in the JSON editor
{: #context-var-json}

You can also define a context variable in the JSON editor. You might want to use the JSON editor if you are defining a complex context variable and want to be able to see the full SpEL expression as you add or change it.

The name and value pair must meet these requirements:

- The `name` can contain any upper- and lowercase alphabetic characters, numeric characters (0-9), and underscores.

  You can include other characters, such as periods and hyphens, in the name. However, if you do, then you must specify the shorthand syntax `$(variable-name)` every time you subsequently reference the variable. See [Expressions for accessing objects](expression-language.html#shorthand-context) for more details.
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

    **Note**: Any existing context variable values that are defined for this node are displayed in a set of corresponding **Variable** and **Value** fields. If you do not want them to be displayed in the edit view of the node, you must close the context editor. You can close the editor from the same menu that is used to open the JSON editor; the following steps describe how to access the menu.

1.  Click the **Options**  ![Advanced response](images/kabob.png) icon that is associated with the response, and then click **Open JSON editor**.

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

- [Deleting a context variable in JSON](#context-delete-json)
- [Updating a context variable value in JSON](#context-update-json)
- [Setting one context variable equal to another](#var-equals-var)

### Deleting a context variable in JSON
{: #context-delete-json}

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
{: #context-update-json}

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

See [Expression language methods](dialog-methods.html#objects) for more information about methods you can perform on objects.

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

See [Expression language methods](dialog-methods.html#arrays) for more information about methods you can perform on arrays.

### Setting one context variable equal to another
{: #var-equals-var}

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
{: #digressions}

A digression occurs when a user is in the middle of a dialog flow that is designed to address one goal, and abruptly switches topics to initiate a dialog flow that is designed to address a different goal. The dialog has always supported the user's ability to change subjects. If none of the nodes in the dialog branch that is being processed match the goal of the user's latest input, the conversation goes back out to the tree to check the root node conditions for an appropriate match. The digression settings that are available per node give you the ability to tailor this behavior even more.

With digression settings, you can allow the conversation to return to the dialog flow that was interrupted when the digression occurred. For example, the user might be ordering a new phone, but switches topics to ask about tablets. Your dialog can answer the question about tablets, and then bring the user back to where they left off in the process of ordering a phone. Allowing digressions to occur and return gives your users more control over the flow of the conversation at run time. They can change topics, follow a dialog flow about the unrelated topic to its end, and then return to where they were before. The result is a dialog flow that more closely simulates a human-to-human conversation.

![Shows someone who is providing details about a dinner reservation ask about vegetarian options, get an answer, and then return to providing reservation details.](images/digression.gif){: gif}

The animated image uses a mockup of the dialog tree user interface to illustrate the concept of a digression. It shows how a user interacts with dialog nodes that are configured to allow digressions that return to the dialog flow that was in progress. The user starts to provide the information required to make a dinner reservation. In the middle of filling slots in the #reservation node, the user asks a question about vegetarian menu options. The dialog answers the user's new question by finding a node that addresses it amongst the root nodes (a node that conditions on the #cuisine intent). It then returns to the conversation that was in progress by showing the prompt for the next empty slot from the original dialog node.

Watch this video to learn more.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Digressions overview" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/I3K7mQ46K3o?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

- [Before you begin](dialog-runtime.html#prereqs)
- [Customizing digressions](dialog-runtime.html#enable-digressions)
- [Disabling digressions into a root node](dialog-runtime.html#disable-digressions)
- [Digression tutorial](dialog-runtime.html#digression-tutorial)
- [Design considerations](dialog-runtime.html#digression-design-considerations)

### Before you begin
{: #prereqs}

As you test your overall dialog, decide when and where it makes sense to allow digressions and returns from digressions to occur. The following digression controls are applied to the nodes automatically. Only take action if you want to change this default behavior.

- Every root node in your dialog is configured to allow digressions to target them by default. Child nodes cannot be the target of a digression.
- Nodes with slots are configured to prevent digressions away. All other nodes are configured to allow digressions away. However, the conversation cannot digress away from a node under the following circumstances:

  - If any of the child nodes of the current node contain the `anything_else` or `true` condition

    These conditions are special in that they always evaluate to true. Because of their known behavior, they are often used in dialogs to force a parent node to evaluate a specific child node in succession. To prevent breaking existing dialog flow logic, digression are not allowed in this case. Before you can enable digressions away from such a node, you must change the child node's condition to something else.

  - If the node is configured to jump to another node or skip user input after it is processed

    The final step section of a node specifies what should happen after the node is processed. When the dialog is configured to jump directly to another node, it is often to ensure that a specific sequence is followed. And when the node is configured to skip user input, it is equivalent to forcing the dialog to process the first child node after the current node in succession. To prevent breaking existing dialog flow logic, digressions are not allowed in either of these cases. Before you can enable digressions away from this node, you must change what is specified in the final step section.

### Customizing digressions
{: #enable-digressions}

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

    - **All nodes that have children**: Choose whether you want the conversation to come back to the current node after a digression if the current node's response has already been displayed and its child nodes are incidental to the node's goal. Set the *Allow return from digressions triggered after this node's response* toggle to **No** to prevent the dialog from returning to the current node and continuing to process its branch.

      For example, if the user asks, `Do you sell cupcakes?` and the response, `We offer cupcakes in a variety of flavors and sizes` is displayed before the user changes subjects, you might not want the dialog to return to where it left off. Especially, if the child nodes only address possible follow-up questions from the user and can safely be ignored.

      However, if the node relies on its child nodes to address the question, then you might want to force the conversation to return and continue processing the nodes in the current branch. For example, the initial response might be, `We offer cupcakes in all shapes and sizes. Which menu do you want to see: gluten-free, dairy-free, or regular?` If the user changes subjects at this point, you might want the dialog to return so the user can pick a menu type and get the information they wanted.

    - **Nodes with slots**: Choose whether you want to allow users to digress away from the node before all of the slots are filled. Set the *Allow digressions away while slot filling* toggle to **Yes** to enable digressions away.

      If enabled, when the conversation returns from the digression, the prompt for the next unfilled slot is displayed to encourage the user to continue providing information. If disabled, then any inputs that the user submits which do not contain a value that can fill a slot are ignored. However, you can address unsolicited questions that you anticipate your users might ask while they interact with the node by defining slot handlers. See [Adding slots](dialog-slots.html#add-slots) for more information.

      The following image shows you how digressions away from the #reservation node with slots (shown in the earlier illustration) are configured.

      ![Shows the digressions away settings from a node with slots.](images/digress-away-slots-full.png)

    - **Nodes with slots**: Choose whether the user is only allowed to digress away if they will return to the current node by selecting the **Only digress from slots to nodes that allow returns** checkbox.

      When selected, as the dialog looks for a node to answer the user's unrelated question, it ignores any root nodes that are not configured to return after the digression. Select this checkbox if you want to prevent users from being able to permanently leave the node before they have finished filling the required slots.

    **Digressions into this node**

    You can make the following choices about how digressions into a node behave:

    - Prevent users from being able to digress into the node. See [Disabling digressions into a root node](#diable-digressions) for more details.

    - When digressions into the node are enabled, choose whether the dialog must go back to the dialog flow that it digressed away from. When selected, after the current node's branch is done being processed, the dialog flow goes back to the interrupted node. To make the dialog return afterwards, select **Return after digression**.

    The following image shows you how digressions into the #cuisine node (shown in the earlier illustration) are configured.

    ![Shows the digressions away settings from a node with slots.](images/digress-into-cuisine-full.png)

1.  Click **Apply**.

1.  Use the "Try it out" pane to test the digression behavior.

    Again, you cannot define the start and end of a digression. The user controls where and when digressions happen. You can only apply settings that determine how a single node participates in one. Because digressions are so unpredictable, it is hard to know how your configuration decisions will impact the overall conversation. To truly see the impact of the choices you made, you must test the dialog.

The #reservation and #cuisine nodes represent two dialog branches that can participate in a single user-directed digression. The digression settings that are configured for each individual node are what make this type of digression possible at run time.

![Shows two dialogs, one that sets the digressions away from the reservation slots node and one that sets the digression into the cuisine node.](images/digression-settings.png)

### Disabling digressions into a root node
{: #disable-digressions}

When a flow digresses into a root node, it follows the course of the dialog that is configured for that node. So, it might process a series of child nodes before it reaches the end of the node branch, and then, if configured to do so, goes back to the dialog flow that was interrupted. Through dialog testing, you might find that a root node is triggered too often, or at unexpected times, or that its dialog is too complex and leads the user too far off course to be a good candidate for a temporary digression. If you determine that you would rather not allow users to digress into it, you can configure the root node to not allow digressions in.

To disable digressions into a root node altogether, complete the following steps:

1.  Click to open the root node that you want to edit.
1.  Click **Customize**, and then click the **Digressions** tab.
1.  Set the *Allow digressions into this node* toggle to **Off**.
1.  Click **Apply**.

If you decide that you want to prevent digressions into several root nodes, but do not want to edit each one individually, you can add the nodes to a folder. From the *Customize* page of the folder, you can set the *Allow digressions into this node* toggle to **Off** to apply the configuration to all of the nodes at once. See [Organizing the dialog with folders](dialog-build.html#folders) for more information.

### Digression tutorial
{: #digression-tutorial}

Follow the [tutorial](tutorial-digressions.html) to import a workspace that has a set of nodes already defined. You can walk through some exercises that illustrate how digressions work.

### Design considerations
{: #digression-design-considerations}

- **Avoid fallback node proliferation**: Many dialog designers include a node with a `true` or `anything_else` condition at the end of every dialog branch as a way to prevent users from getting stuck in the branch. This design returns a generic message if the user input does not match anything that you anticipated and included a specific dialog node to address. However, users cannot digress away from dialog flows that use this approach.

  Evaluate any branches that use this approach to determine whether it would be better to allow digressions away from the branch. If the user's input does not match anything you anticipated, it might find a match against an entirely different dialog flow in your tree. Rather than responding with a generic message, you can effectively put the rest of the dialog to work to try to address the user's input. And the root-level `Anything else` node can always respond to input that none of the other root nodes can address.

- **Reconsider jumps to a closing node**: Many dialogs are designed to ask a standard closing question, such as, `Did I answer your question today?` Users cannot digress away from nodes that are configured to jump to another node. So, if you configure all of your final branch nodes to jump to a common closing node, digressions cannot occur. Consider tracking user satisfaction through metrics or some other means.

- **Test possible digression chains**: If a user digresses away from the current node to another node that allows digressions away, the user could potentially digress away from that other node, and repeat this pattern one or more times again. If the starting node in the digression chain is configured to return after the digression, then the user will eventually be brought back to the current dialog node. In fact, any subsequent nodes in the chain that are configured not to return are excluded from being considered as digression targets. Test scenarios that digress multiple times to determine whether individual nodes function as expected.

- **Remember that the current node gets priority**: Remember that nodes outside the current flow are only considered as digression targets if the current flow cannot address the user input. It is even more important in a node with slots that allows digressions away, in particular, to make it clear to users what information is needed from them, and to add confirmation statements that are displayed after the user provides a value.

  Any slot can be filled during the slot-filling process. So, a slot might capture user input unexpectedly. For example, you might have a node with slots that collects the information necessary to make a dinner reservation. One of the slots collects date information. While providing the reservation details, the user might ask, `What's the weather meant to be tomorrow?` You might have a root node that conditions on #forecast which could answer the user. However, because the user's input includes the word `tomorrow` and the reservation node with slots is being processed, the service assumes the user is providing or updating the reservation date instead. *The current node always gets priority.* If you define a clear confirmation statement, such as, `Ok, setting the reservation date to tomorrow,` the user is more likely to realize there was a miscommunication and correct it.

  Conversely, while filling slots, if the user provides a value that is not expected by any of the slots, there is a chance it will match against a completely unrelated root node that the user never intended to digress to.

  Be sure to do lots of testing as you configure the digression behavior.

- **When to use digressions instead of slot handlers**: For general questions that users might ask at any time, use a root node that allows digressions into it, processes the input, and then goes back to the flow that was in progress. For nodes with slots, try to anticipate the types of related questions users might want to ask while filling in the slots, and address them by adding handlers to the node.

  For example, if the node with slots collects the information required to fill out an insurance claim, then you might want to add handlers that address common questions about insurance. However, for questions about how to get help, or your stores locations, or the history of your company, use a root level node.

## Disambiguation ![Premium plan only](images/premium0.png)
{: #disambiguation}

When you enable disambiguation, you instruct the service to ask users for help when it finds that more than one dialog node can respond to their input. Instead of guessing which node to process, your assistant shares a list of the top node options with the user, and asks the user to pick the right one.

![Shows a sample conversation between a user and the assistant, where the assistant asks for clarification from the user.](images/disambig-demo.png)

If enabled, disambiguation is not triggered unless the following conditions are met:

- The confidence score of one or more of the runner-up intents detected in the user input is greater than 55% of the confidence score of the top intent.
- The confidence score of the top intent is above 0.2.

Even when these conditions are met, disambiguation does not occur unless two or more independent nodes in your dialog meet the following criteria:

- The node condition includes one of the intents that triggered disambiguation. Or the node condition otherwise evaluates to true. For example, if the node checks for an entity type and the entity is mentioned in the user input, it is eligible.
- There is text in the node's *external node name* field.

### Disambiguation example
{: #disambig-example}

For example, you have a dialog that has two nodes with intent conditions that address cancellation requests. The conditions are:

- eCommerce_Cancel_Product_Order
- Customer_Care_Cancel_Account

If the user input is `i must cancel it today`, then the following intents might be detected in the input:

`[`
`{"intent":"Customer_Care_Cancel_Account","confidence":0.6618281841278076},`
`{"intent":"eCommerce_Cancel_Product_Order","confidence":0.4330700159072876},`
`{"intent":"Customer_Care_Appointments","confidence":0.2902342438697815},`
`{"intent":"Customer_Care_Store_Hours","confidence":0.2550420880317688},`
`...]`

The service is `0.6618281841278076` (66%) confident that the user goal matches the `#Customer_Care_Cancel_Account` intent. If any other intent has a confidence score that is greater than 55% of 66%, then it fits the criteria for being a disambiguation candidate.

`0.66 x 0.55 = 0.36`

Intents with a score that is greater than 0.36 are eligible.

In our example, the `#eCommerce_Cancel_Product_Order` intent is over the threshold, with a confidence score of `0.4330700159072876`.

When the user input is `i must cancel it today`, both dialog nodes will be considered viable candidates to respond. To determine which dialog node to process, the assistant asks the user to pick one. And to help the user choose between them, the assistant provides a short summary of what each node does. The summary text it displays is extracted directly from the *external node name* information that was specified for each node.

![Service prompts the user to choose from a list of dialog options, including Cancel an account, Cancel a product order, and None of the above.](images/disambig-tryitout.png)

Notice that the service recognizes the term `today` in the user input as a date, a mention of the `@sys-date` entity. If your dialog tree contains a node that condition on the `@sys-date` entity, then it is also included in the list of disambiguation choices.

![Service prompts the user to choose from a list of dialog options, including Capture date information.](images/disambig-tryitout-date.png)

It is not only nodes that condition on intents that can be included in the disambiguation options list.

### Enabling disambiguation
{: #disambiguation-enable}

To enable disambiguation, complete the following steps:

1.  From the Dialogs page, click **Settings**.
1.  Click **Disambiguation**.
1.  In the *Enable disambiguation* section, switch the toggle to **On**.
1.  In the prompt message field, add text to show before the list of dialog node options. For example, *What do you want to do?*
1.  **Optional**: In the none of the above message field, add text to display as an additional option that users can pick if none of the other dialog nodes reflect what the user wants to do. For example, *None of the above*.

    Keep the message short, so it displays inline with the other options. The message must be less than 512 characters. For information about what the service does if a user chooses this option, see [Handling none of the above](#handle-none).

1.  Click **Close**
1.  Decide which dialog nodes you want the assistant to ask for help with.

    - You can pick nodes at any level of the tree hierarchy.
    - You can pick nodes that condition on intents, entities, special conditions, context variables, or any combination of these values.

    See [Choosing nodes](#choose-nodes) for tips.

    For each node that you want to opt in to disambiguation, complete the following steps:

    1.  Click to open the node in edit view.
    1.  In the *external node name* field, describe the user task that this dialog node is designed to handle. For example, *Cancel an account*.

        ![Shows where to add the external node name information in the node edit view.](images/disambig-node-purpose.png)

### Choosing nodes
{: #choose-nodes}

Choose nodes that serve as the root of a distinct branch of the dialog to be disambiguation choices. The node can be a child of another node. But, if it conditions on some distinct value or values that distinguish it from everything else, it is probably a good candidate for disambiguation.

Keep in mind:

- How the disambiguation process will treat nodes that condition on intents is easier to predict than nodes that condition on other values.

  If the service is confident that the node's intent condition matches the user's intent, then the node is included as a disambiguation option.
- How nodes with boolean conditions, conditions that evaluate to either true or false, are treated can be less predictable.

  A node that conditions on an entity type, for example, is included as a disambiguation option if the entity is mentioned in the input that triggers disambiguation. **Unless** the entity is also referenced by a node that is not eligible for inclusion in the disambiguation list for some reason. For example, the entity mention might trigger a node that is situated earlier in the dialog tree but is not enabled for disambiguation. If the same entity is the condition for a node that *is* enabled for disambiguation, but is situated lower in the tree, then it cannot be added as a disambiguation option because the service never reaches it. It matched against the earlier node and was omitted, and the service does not process the later node.
- The order of nodes in the tree hierarchy matters to disambiguation.

  Look at the [scenario](#disambig-example) that is used earlier to introduce disambiguation, for example. If the node that conditions on `@sys-date` was placed higher in the dialog tree than the nodes that condition on the `#Customer_Care_Cancel_Account` and `#eCommerce_Cancel_Product_Order` intents, then disambiguation would never be triggered when a user enters, `i must cancel it today`. That's because the service would consider the date mention (`today`) to be more important than the intent references due to the placement of the corresponding nodes in the tree.

For each node that you elect to be a disambiguation option, test scenarios in which you expect the node to be displayed as a disambiguation option.

### Handling none of the above
{: #handle-none}

When a user clicks the *None of the above* option, the service strips the intents that were recognized in the user input from the message and submits it again. This action typically triggers the anything else node in your dialog tree.

To customize the response that is returned in this situation, you can add a root node with a condition that checks for a user input with no recognized intents (the intents are stripped, remember) and contains a `suggestion_id` property. A `suggestion_id` property is added by the service when disambiguation is triggered.
{: tip}

Add a root node with the following condition:

```json
intents.size()==0 && input.suggestion_id
```
{: codeblock}

This condition is met only by input that has triggered a set of disambiguation options of which the user has indicated none match her goal.

Add a response that lets users know that you understand that none of the options that were suggested met their needs, and take appropriate action.

### Testing disambiguation
{: #disambiguation-test}

To test disambiguation, complete the following steps:

1.  From the "Try it out" pane, enter a test utterance that you think is a good candidate for disambiguation, meaning two or more of your dialog nodes are configured to address utterances like it.

1.  If the response does not contain a list of dialog node options for you to choose from as expected, first check that you added summary information to the external node name field for each of the nodes.

1.  If disambiguation is still not triggered, it might be that the confidence scores for the nodes are not as close in value as you thought. 

    You can get information about the intents, entities, and other properties that are returned for certain user inputs.

    - To see the confidence scores of the intents that were detected in user input, temporarily add `<? intents ?>` to the end of the node response for a node that you know will be triggered.

      This SpEL expression shows the intents that were detected in the user input as an array. The array includes the intent name and the level of confidence that the service has that the intent reflects the user's intended goal.

    - To see which entities, if any, were detected in the user input, you can temporarily replace the current response with a single text response that contains the SpEL expression, `<? entities ?>`.

      This SpEL expression shows the entities that were detected in the user input as an array. The array includes the entity name, location of the entity mention within the user input string, the entity mention string, and the level of confidence that the service has that the term is a mention of the entity type specified.

    - To see details for all of the artifacts at once, including other properties, such as the value of a given context variable at the time of the call, you can inspect the entire API response. See [Viewing API call details](dialog-tips.html#inspect-api).

1.  Temporarily remove the description you added to the *external node name* field for at least one of the nodes that you anticipate will be listed as a disambiguation option.

1.  Enter the test utterance into the "Try it out" pane again.

    If you added the `<? intents ?>` expression to the response, then the text returned includes a list of the intents that the service recognized in the test utterance, and includes the confidence score for each one.

    ![Service returns an array of intents, including Customer_Care_Cancel_Account and eCommerce_Cancel_Product_Order.](images/disambig-show-intents.png)

After you finish testing, remove any SpEL expressions that you appended to node responses, or add back any original responses that you replaced with expressions, and repopulate any *external node name* fields from which you removed text.
