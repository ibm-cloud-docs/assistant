---

copyright:
  years: 2015, 2018
lastupdated: "2018-04-06"

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
{:table: .aria-labeledby="caption"}

# Dialog overview
{: #dialog-overview}

The dialog uses the intents and entities that are identified in the user's input, plus context from the application, to interact with the user and ultimately provide a useful response.
{: shortdesc}

The response might be the answer to a question such as `Where can I get some gas?` or the execution of a command, such as turning on the radio. The intent and entity might be enough information to identify the correct response, or the dialog might ask the user for more input that is needed to respond correctly. For example, if a user asks, `Where can I get some food?` you might want to clarify whether they want a restaurant or a grocery store, to dine in or take out, and so on. You can ask for more details in a text response and create one or more child nodes to process the new input.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/DVqPvOI8clw?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

The dialog is represented graphically in the {{site.data.keyword.conversationshort}} tool as a tree. Create a branch to process each intent that you want your conversation to handle. A branch is composed of multiple nodes.

## Dialog nodes

Each dialog node contains, at a minimum, a condition and a response.

![Shows user input going to a box that contains the statement If: CONDITION, Then: RESPONSE](images/node1-empty.png)

- Condition: Specifies the information that must be present in the user input for this node in the dialog to be triggered. The information might be a specific intent, an entity value, or a context variable value. See [Conditions](dialog-runtime.html#conditions) for more information.
- Response: The utterance that the service uses to respond to the user. The response can also be configured to trigger programmatic actions. See [Responses](#responses) for more information.

You can think of the node as having an if/then construction: if this condition is true, then return this response.

For example, the following node is triggered if the natural language processing function of the service determines that the user input contains the `#cupcake-menu` intent. As a result of the node being triggered, the service responds with an appropriate answer.

![Shows the user asking about cupcake flavors. the If condition is #cupcake-menu and the Then response is a list of cupcake flavors.](images/node1-simple.png)

A single node with one condition and response can handle simple user requests. But, more often than not, users have more sophisticated questions or want help with more complex tasks. You can add child nodes that ask the user to provide any additional information that the service needs.

![Shows that the first node in the dialog asks which type of cupcake the user wants, gluten-free or regular, and has two child nodes that provide a different response depending on the user's answer.](images/node1-children.png)

## Dialog flow

The dialog that you create is processed by the service from the first node in the tree to the last.

![Arrow points down next to 3 nodes to show that dialog flows from the first node to the last](images/node-flow-down.png)

As it travels down the tree, if the service finds a condition that is met, it triggers that node. It then moves along the triggered node to check the user input against any child node conditions. As it checks the child nodes it moves again from the first child node to the last.

The services continues to work its way through the dialog tree from first to last node, along each triggered node, then from first to last child node, and along each triggered child node until it reaches the last node in the branch it is following.

![Shows arrow 1 pointing from the first root node to the last, arrow 2 pointing from along the length of a triggered node, and arrow 3 pointing from the first to the last child nodes of the triggered node.](images/node-flow.png)

When you start to build the dialog, you must determine the branches to include, and where to place them. The order of the branches is important because nodes are evaluated from first to last. The first root node whose condition matches the input is used; any nodes that come later in the tree are not triggered.

When the service reaches the end of a branch, or cannot find a condition that evaluates to true from the current set of child nodes it is evaluating, it jumps back out to the base of the tree. And once again, the service processes the root nodes from first to the last. If none of the conditions evaluates to true, then the response from the last node in the tree, which typically has a special `anything_else` condition that always evaluates to true, is returned.

You can disrupt the standard first-to-last flow by customizing what happens after a node is processed. For example, you can configure a node to jump directly to another node after it is processed, even if the other node is positioned earlier in the tree. See [Defining what to do next](dialog-overview.html#jump-to) for more details.

How you configure digression settings for each node can also impact how users move through the nodes at run time. If you enable digressions away from most nodes, users can jump from one node to another and back again more easily. See [Digressions](dialog-runtime.html#digressions) for more information.

## Conditions
{: #conditions}

A node condition determines whether that node is used in the conversation. Response conditions determine which response to display to a user.

- [Condition artifacts](dialog-overview.html#condition-artifacts)
- [Condition syntax details](dialog-overview.html#condition-syntax)
- [Condition usage tips](dialog-overview.html#condition-tips)
- [Special conditions](dialog-overview.html#special-conditions)

### Condition artifacts
{: #condition-artifacts}

You can use one or more of the following artifacts in any combination to define a condition:

- **Context variable**: The node is used if the context variable expression that you specify is true. Use the syntax, `$variable_name:value` or `$variable_name == 'value'`. For example, `$city:Boston` checks whether the `$city` context variable contains the value, `Boston`. If so, the node or response is processed.

  Do not define a node or response condition based on the value of a context variable in the same dialog node in which you set the context variable value.
  {: tip}

  For more information about context variables, see [Context variables](dialog-runtime.html#context).

- **Entity**: The node is used when any value or synonym for the entity is recognized in the user input. Use the syntax, `@entity_name`. For example, `@city` checks whether any of the city names that are defined for the @city entity were detected in the user input. If so, the node or response is processed.

  Be sure to create a peer node to handle the case where none of the entity's values or synonyms are recognized.
  {: tip}

  For more information about entities, see [Defining entities](entities.html).

- **Entity value**: The node is used if the entity value is detected in the user input. Use the syntax, `@entity_name:value` and specify a defined value for the entity, not a synonym. For example: `@city:Boston` checks whether the specific city name, `Boston`, was detected in the user input.

  If the entity is a pattern entity with capture groups, then you can check for a certain group value match. For example, you can use the syntax: `@us_phone.groups[1] == '617'`
  See [Storing pattern entity values in context variables](dialog-runtime.html#context-pattern-entities) for more information.

  If you check for the presence of the entity, without specifying a particular value for it, in a peer node, be sure to position this node (which checks for a particular entity value) before the peer node that checks only for the presence of the entity. Otherwise, this node will never be evaluated.
  {: tip}

- **Intent**: The simplest condition is a single intent. The node is used if the user's input maps to that intent. Use the syntax, `#intent_name`. For example, `#weather` checks if the intent detected in the user input is `weather`. If so, the node is processed.

  For more information about intents, see [Defining intents](intents.html).

- **Special condition**: Conditions that are provided with the service that you can use to perform common dialog functions. See the **Special conditions** table in the next section for details.

### Special conditions
{: #special-conditions}

| Condition syntax     | Description |
|----------------------|-------------|
| `anything_else`      | You can use this condition at the end of a dialog, to be processed when the user input does not match any other dialog nodes. The **Anything else** node is triggered by this condition. Do not use in dialog branches where you want digressions away to occur. |
| `conversation_start` | Like **welcome**, this condition is evaluated as true during the first dialog turn. Unlike **welcome**, it is true whether or not the initial request from the application contains user input. A node with the **conversation_start** condition can be used to initialize context variables or perform other tasks at the beginning of the dialog. |
| `false`              | This condition is always evaluated to false. You might use this at the start of a branch that is under development, to prevent it from being used, or as the condition for a node that provides a common function and is used only as the target of a **Jump to** action. |
| `irrelevant`         | This condition will evaluate to true if the user’s input is determined to be irrelevant by the {{site.data.keyword.conversationshort}} service. |
| `true`               | This condition is always evaluated to true. You can use it at the end of a list of nodes or responses to catch any responses that did not match any of the previous conditions. |
| `welcome`            | This condition is evaluated as true during the first dialog turn (when the conversation starts), only if the initial request from the application does not contain any user input. It is evaluated as false in all subsequent dialog turns. The **Welcome** node is triggered by this condition. Typically, a node with this condition is used to greet the user, for example, to display a message such as `Welcome to our Pizza ordering app.` This node is never processed during interactions that occur through channels such as Facebook or Slack.|
{: caption="Special conditions" caption-side="top"}

### Condition syntax details
{: #condition-syntax}

Use one of these syntax options to create valid expressions in conditions:

- Shorthand notations to refer to intents, entities, and context variables. See [Accessing and evaluating objects](expression-language.html).

- Spring Expression (SpEL) language, which is an expression language that supports querying and manipulating an object graph at runtime. See [Spring Expression Language (SpEL) language ![External link icon](../../icons/launch-glyph.svg "External link icon")](http://docs.spring.io/spring/docs/current/spring-framework-reference/html/expressions.html){: new_window} for more information.

You can use regular expressions to check for values to condition against.  To find a matching string, for example, you can use the `String.find` method. See  [Methods](dialog-methods.html) for more details.

### Condition usage tips
{: #condition-tips}

- **Checking for values with special characters**: If you want to check whether an entity or context variable contains a value, and the value includes a special character, such as an apostrophe ('), then you must surround the value that you want to check with parentheses. For example, to check if an entity or context variable contains the name `O'Reilly`, you must surround the name with parentheses.

  `@person:(O'Reilly)` and `$person:(O'Reilly)`

  The service converts these shorthand references into these full SpEL expressions:

  `entities['person']?.contains('O''Reilly')` and `context['person'] == 'O''Reilly'`

  **Note**: SpEL uses a second apostrophe to escape the single apostrophe in the name.

- **Checking for multiple values**: If you want to check for more than one value, you can create a condition that uses OR operators (`||`) to list multiple values in the condition. For example, to define a condition that is true if the context variable `$state` contains the abbreviations for Massachusetts, Maine, or New Hampshire, you can use this expression:

  `$state:MA || $state:ME || $state:NH`

- **Checking for number values**: When using numeric variables, make sure the variables have values. If a variable does not have a value, it is treated as having a null value (0) in a numeric comparison.

  For example, if you check the value of a variable with the condition `@price < 100`, and the @price entity is null, then the condition is evaluated as `true` because 0 is less than 100, even though the price was never set. To prevent the checking of null variables, use a condition such as `@price AND @price < 100`. If `@price` has no value, then this condition correctly returns false.

- **Checking for intents with a specific intent name pattern**: You can use a condition that looks for intents that match a pattern. For example, to find any detected intents with intent names that start with 'User_', you can use a syntax like this in the condition:

  `intents[0].intent.startsWith("User_")`

  However, when you do so, all of the detected intents are considered, even those with a confidence lower than 0.2. Also check that intents which are considered irrelevant by Watson based on their confidence score are not returned. To do so, change the condition as follows:

  `!irrelevant && intents[0].intent.startsWith("User_")`

- **How fuzzy matching impacts entity recognition**: If you use an entity as the condition and fuzzy matching is enabled, then `@entity_name` evaluates to true only if the confidence of the match is greater than 30%. That is, only if `@entity_name.confidence > .3`.

- **Handling multiple entities in input**: If you want to evaluate only the value of the first detected instance of an entity type, you can use the syntax  `@entity == 'specific-value'` instead of the `@entity:(specific-value)` format.

  For example, when you use `@appliance == 'air conditioner'`, you are evaluating only the value of the first detected `@appliance` entity. But, using `@appliance:(air conditioner)` gets expanded to `entity['appliance'].contains('air conditioner')`, which matches whenever there is at least one `@appliance` entity of value 'air conditioner' detected in the user input.

## Responses
{: #responses}

The dialog response defines how to reply to the user.

You can reply with one of these response types:

- [Simple text response](#simple-text)
- [Conditional responses](#multiple)
- [Complex response](#complex)
- [Multimedia response](#multimedia)

### Simple text response
{: #simple-text}

If you want to provide a text response, simply enter the text that you want the service to display to the user.

![Shows a node that shows a user ask, Where are you located, and the dialog response is, We have no brick and mortar stores! But, with an internet connection, you can shop us from anywhere.](images/response-simple.png)

To include a context variable value in the response, use the syntax `$variable-name` to specify it. See [Context variables](dialog-runtime.html#context) for more information.

```
Hello $user
```
{: screen}

If you include an email address in the response, you must escape the at symbol (`@`) with a backslash (`\`). For example, `Send us your feedback at feedback\@example.com.` Likewise, if you include a number sign (`#`) in the response, you must escape it. For example, `We are the \#1 seller of lobster rolls in Maine.` Entity names begin with `@` and intent names begin with `#`. Escaping these symbols prevents the service from misreading the response text.
{: tip}

#### Adding variety
{: #variety}

If your users return to your conversation service frequently, they might be bored to hear the same greetings and responses every time.  You can add *variations* to your responses so that your conversation can respond to the same condition in different ways.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/nAlIW3YPrAs?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

In this example, the answer that the service provides in response to questions about store locations differs from one interaction to the next:

![Shows a node that shows a user ask, Where are you located, and the dialog has three different responses defined.](images/variety.png)

You can choose to rotate through the response variations sequentially or in random order. By default, responses are rotated sequentially, as if they were chosen from an ordered list.

### Conditional responses
{: #multiple}

A single dialog node can provide different responses, each one triggered by a different condition.  Use this approach to address multiple scenarios in a single node.

<iframe class="embed-responsive-item" id="youtubeplayer" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/Q5_-f7_Iyvg?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

The node still has a main condition, which is the condition for using the node and processing the conditions and responses that it contains.

In this example, the service uses information that it collected earlier about the user's location to tailor its response, and provide information about the store nearest the user. See [Context variables](dialog-runtime.html#context) for more information about how to store information collected from the user.

![Shows a node that shows a user ask, Where are you located, and the dialog has three different responses depending on conditions that use info from the $state context variable to specify locations in those states.](images/multiple-responses.png)

This single node now provides the equivalent function of four separate nodes.

To add conditional responses to a node, click **Customize** and then click the **Multiple responses** toggle to turn it **On**.

The conditions within a node are evaluated in order, just as nodes are.  Be sure that your conditions and responses are listed in the correct order.  If you need to change the order, select a condition and move it up or down in the list using the arrows that are displayed. If you want to update the context, you must do so in the JSON editor for each individual response.  There is not a common JSON editor for all responses. If you associated a **Jump to** action with the node, the jump does not occur until after any responses are processed.
{: tip}

### A complex response
{: #complex}

To specify a more complex response, you can use the JSON editor to specify the response in the `"output":{}` property. The output.text field is always stored as a JSON array. If you try to save it in any other format, your text is replaced with an empty JSON array.

To specify more than one statement that you want to display on separate lines, define the output as a JSON array.

```json
{
  "output": {
    "text": ["Hello there.", "How are you?"]
  }
}
```
{: codeblock}

The first sentence is displayed on one line, and the second sentence is displayed as a new line after it.

To implement more complex behavior, you can define the output text as a complex JSON object. For example, you can use a complex object in the JSON output to mimic the behavior of adding response variations to the node. You can include the following properties in the complex object:

- **values**: A JSON array of strings that holds multiple versions of output text that this dialog node can return. The order in which values in the array are returned depends on the attribute `selection_policy`.

- **selection_policy**: The following values are valid:

    - **random**: The system randomly selects output text from the `values` array and does not repeat them consecutively. For example, consider output.text that contains three values. For the first three times, a random value is selected but not repeated another time. After all the output values are given, the system randomly selects another value and repeats the process.

        ```json
        {
            "output":{
                "text":{
                    "values":["Hello.","Hi.","Howdy!"],
                    "selection_policy":"random"
                }
            }
        }
        ```
        {: codeblock}

    The system returns one greeting from these three options that it picks at random. The next time the response is triggered, another greeting from the list is displayed. The greeting is again chosen at random, except the previously used greeting is intentionally not repeated.

    - **sequential**: The system returns the first output text the first time the dialog node is triggered, the second output text the second time the node is triggered, and so on.

        ```json
        {
            "output":{
                "text":{
                    "values":["Hello.", "Hi.", "Howdy!"],
                    "selection_policy":"sequential"
                }
            }
        }
        ```
        {: codeblock}

- **append**: Specifies whether to append a value to an array or overwrite the values in the array with the new value or values. When set to false, the output collected in previously executed dialog nodes is overwritten by the text value specified in this particular node.

    ```json
    {
        "output":{
            "text":{
                "values": ["Hello."],
                "append":false
            }
        }
    }
    ```
    {: codeblock}

    In this case, all other output text is overwritten by this output text.

The default behavior assumes `selection_policy = random` and `append = true`. When the values array contains more than one item, the output text is randomly selected from its elements.

### Multimedia response
{: #multimedia}

If you plan to integrate an assistant to which this conversational skill is added with Slack or Facebook Messenger, you can specify dialog node responses that include multimedia or interactive elements such as clickable buttons.

See [Multimedia responses](dialog-multimedia.html) for more information.

## Defining what to do next
{: #jump-to}

After making the specified response, you can instruct the service to do one of the following things:

- **Wait for user input**: The service waits for the user to provide new input that the response elicits. For example, the response might ask the user a yes or no question. The dialog will not progress until the user provides more input.
- **Skip user input**:  Use this option when you want to bypass waiting for user input and go directly to the first child node of the current node instead.

  **Note**: The current node must have at least one child node for this option to be available.

- **Jump to another dialog node**: Use this option when you want the conversation to go directly to an entirely different dialog node. You can use a *Jump to* action to route the flow to a common dialog node from multiple locations in the tree, for example.

  **Note**: The target node that you want to jump to must exist before you can configure the jump to action to use it.

### Configuring the Jump to action
{: #jump-to-config}

If you choose to jump to another node, specify when the target node is processed by choosing one of the following options:

- **Condition**: If the statement targets the condition section of the selected dialog node, the service checks first whether the condition of the targeted node evaluates to true.
    - If the condition evaluates to true, the system processes the target node immediately.
    - If the condition does not evaluate to true, the system moves to the next sibling node of the target node to evaluate its condition, and repeats this process until it finds a dialog node with a condition that evaluates to true.
    - If the system processes all the siblings and none of the conditions evaluate to true, the basic fallback strategy is used, and the dialog evaluates the nodes at the base level of the dialog tree.

    Targeting the condition is useful for chaining the conditions of dialog nodes. For example, you might want to first check whether the input contains an intent, such as `#turn_on`, and if it does, you might want to check whether the input contains entities, such as `@lights`, `@radio`, or `@wipers`. Chaining conditions helps to structure larger dialog trees.

- **Response**: If the statement targets the response section of the selected dialog node, it is run immediately. That is, the system does not evaluate the condition of the selected dialog node; it processes the response of the selected dialog node immediately.

  Targeting the response is useful for chaining several dialog nodes together. The response is processed as if the condition of this dialog node is true. If the selected dialog node has another **Jump to** action, that action is run immediately, too.

- **Wait for user input**: Waits for new input from the user, and then begins to process it from the node that you jump to. This option is useful if the source node asks a question, for example, and you want to jump to a separate node to process the user's answer to the question.

## More information

For information about the expression language used by dialog, plus methods, system entities, and other useful details, see the **Reference** section in the navigation pane.
