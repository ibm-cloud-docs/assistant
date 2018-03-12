---

copyright:
  years: 2015, 2018
lastupdated: "2018-01-25"

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

# Creating a dialog
{: #dialog-beta}

Use this information as you try out beta features of dialog.
{: shortdesc}

**BETA** Most of the features described in this documentation are beta features that have been made available for your evaluation. Beta features might be unstable, might change frequently, and might be discontinued with short notice. Beta features also might not provide the same level of performance or compatibility that generally available features provide and are not intended for use in a production environment.

## Testing your dialog
{: #test}

As you make changes to your dialog, you can test it at any time to see how it responds to input.

1.  From the Dialog tab, click the ![Ask Watson](images/ask_watson.png) icon.
1.  In the chat pane, type some text and then press Enter.

    Make sure the system has finished training on your most recent changes before you start to test the dialog. If the system is still training, a message is displayed in the *Try it out* pane:
    {: tip}

    ![Screen capture of training message](images/training.png)
1.  Check the response to see if the dialog correctly interpreted your input and chose the appropriate response.

    The chat window indicates what intents and entities were recognized in the input:

    ![Screen capture of test dialog output](images/test_dialog_output.png)

1.  **BETA**: If you want to know which node in the dialog tree triggered a response, click the **Location** ![Location](images/location.png) icon next to it.

    The source node is given focus and the route that the service traversed through the tree to get to it is highlighted.
1.  To check or set the value of a context variable, click the **Manage context** link.

    Any context variables that you defined in the dialog are displayed.

    In addition, a `$timezone` context variable is listed. The *Try it out* pane user interface gets user locale information from the web browser and uses it to set the `$timezone` context variable. This context variable makes it easier to deal with time references in test dialog exchanges. Consider doing something similar in your user application. If not specified, Greenwich Mean Time (GMT) is used.

    You can add a variable and set its value to see how the dialog responds in the next test dialog turn. This capability is helpful if, for example, the dialog is set up to show different responses based on a context variable value that is provided by the user.

    1.  To add a context variable, specify the variable name, and press **Enter**.
    1.  To define a default value for the context variable, find the context variable you added in the list, and then specify a value for it.

    See [Context variables](dialog-runtime.html#context) for more information.

1.  Continue to interact with the dialog to see how the conversation flows through it.
    - To find and resubmit a test utterance, you can press the Up key to cycle through your recent inputs.
    - To remove prior test utterances from the chat pane and start over, click the **Clear** link. Not only are the test utterances and responses removed, but this action also clears the values of any context variables that were set as a result of your interactions with the dialog. Context variable values that you explicitly set or change are not cleared.

### What to do next

If you determine that the wrong intents or entities are being recognized, you might need to modify your intent or entity definitions.

If the correct intents and entities are being recognized, but the wrong nodes are being triggered in your dialog, make sure your conditions are written properly.

Click [here](docs/services/conversation/dialog-build.html#test) to go to the generally available version of this information, which describes how the feature works currently.

## Defining a context variable BETA
{: #context-var-define}

Define a context variable by defining a name and value pair for the variable in one of the following editors:

- **Context editor**: When opened, adds a **Variable** and a corresponding **Value** field to the node edit view that you can fill with the context variable name and value information.

- **JSON editor**: When opened, it provides a view into the underlying JSON content that is passed with the /message API request that is sent to the Conversation service. You can define context variables by adding name and value pairs to the `"context":{}` section of the JSON body.

The name and value pair must meet these requirements:

- The `name` can contain any upper- and lowercase alphabetic characters, numeric characters (0-9), and underscores.

  **Note**: You can include other characters, such as periods and hyphens, in the name. However, if you do, then you must use one of the following approaches every time you subsequently reference the variable:

  - **context['variable-name']**

      The full SpEL expression syntax.
  - **$(variable-name)**

      Shorthand syntax with the variable name enclosed in parentheses.
    See [Accessing and evaluating objects](expression-language.html#shorthand-syntax-for-context-variables) for more details.

- The `value` can be any supported JSON type, such as a simple string variable, a number, or a JSON array. When you define the context variable using the JSON editor, you can specify a JSON object as the value also.

The following table shows how to define name and value pairs in context variable editor fields:

| Variable       | Value              |
|:---------------|--------------------|
| dessert        | cake               |
| toppings_array | ["onion","olives"] |
| age            | 18                 |

The following JSON sample defines values for the $dessert string, $toppings_array array, and $age number context variables:

```json
{
  "context": {
    "dessert": "cake",
    "toppings_array": ["onion", "olives"],
    "age": 18
  }
}
```
{: codeblock}

To define a context variable, complete the following steps:

1.  Define the context variable in the section of the node that represents the time at which you want the variable to be set during dialog node evaluation.

    **Note**: Any existing context variable values that are defined for this node are displayed in a set of corresponding **Variable** and **Value** fields. If you do not want them to be displayed in the edit view of the node, you must close the context editor. You can close the editor from the same menu that is used to open it; the following steps describe how to access the menu.

    - To add a context variable that is set or changed after the node response is processed, add the context variable into the response section.

      Click the **Options**  ![Advanced response](images/kabob.png) icon that is associated with the response, and then choose an editor by selecting one of the following options:

      - **Open JSON editor**
      - **Open context editor**

      ![Shows how to access the JSON editor associated with a standard node response.](images/contextvar-json-response.png)

      If the **Multiple responses** setting is **On** for the node, then you must click the **Edit response** ![Edit response](images/edit-slot.png) icon first.

      ![Shows how to access the JSON editor associated with a standard node that has multiple conditional responses enabled for it.](images/contextvar-json-multi-response.png)

    - To add a context variable that is set or updated after a slot condition is met, click the **Edit slot** ![Edit response](images/edit-slot.png) icon. From the **Options** ![Advanced response](images/kabob.png) menu in the *Configure slot* view header, click **Open JSON editor**. (For more information about slots, see [Gathering information with slots](dialog-slots.html).)

      **Note**: There is currently no way to use the context editor to define context variables that are set during this phase of dialog node evaluation.

      ![Shows how to access the JSON editor associated with a slot condition.](images/contextvar-json-slot-condition.png)

    - To add a context variable that is processed after a response condition for a slot is met, click the **Edit slot** ![Edit response](images/edit-slot.png) icon. Click the **Options** ![Advanced response](images/kabob.png) icon, and then select **Enable conditional responses**. Click the **Edit response** ![Edit response](images/edit-slot.png) icon next to the response with which you want to associate the context variable. Click the **Options** ![Advanced response](images/kabob.png) icon in the response section, and then choose an editor by selecting one of the following options:

      - **Open JSON editor**
      - **Open context editor**

      ![Shows how to access the JSON editor associated with the conditional response for a slot.](images/contextvar-json-slot-multi-response.png)
1.  To define the context variable in the context editor, add the variable name and value pair to the **Variable** and **Value** fields.
1.  To define the context variable in the JSON editor, complete these additional steps:

    - Add a `"context":{}` block if one is not present.

      ```json
      {
        "context":{},
        "output":{}
      }
      ```
      {: codeblock}

    - In the context block, add a name and value pair for each context variable that you want to define.

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

## Common context variable tasks
{: #context-common-tasks}

- To store the entire string that was provided by the user as input, use `input.text`:

    | Variable | Value            |
    |----------|------------------|
    | repeat   | `<?input.text?>` |

    ```json
    {
      "context": {
        "repeat": "<?input.text?>"
      }
    }
    ```
    {: codeblock}

- To store the value of an entity in a context variable, use this syntax:

    | Variable | Value            |
    |----------|------------------|
    | place    | @place           |

    ```json
    {
      "context": {
        "place": "@place"
      }
    }
    ```
    {: codeblock}

- To store the value of a string that you extract from the user's input, you can include a SpEL expression that uses the extract method to apply a regular expression to the user input. The following expression extracts a number from the user input, and saves it to the `$number` context variable.

    | Variable | Value                               |
    |----------|-------------------------------------|
    | number   | `<?input.text.extract('[\d]+',0)?>` |

    ```json
    {
      "context": {
         "number": "<?input.text.extract('[\\d]+',0)?>"
      }
    }
    ```
    {: codeblock}

    When you define a regular expression in the JSON editor, you must escape any back slashes that you use in the expression with another back slash (`\\`). You do not need to escape back slashes in regular expressions that you define using the context variable editor.
    {: tip}

- To store the value of a pattern entity, append .literal to the entity name. Using this syntax ensures that the exact span of text from user input that matched the specified pattern is stored in the variable.

    | Variable | Value            |
    |----------|------------------|
    | email    | @email.literal   |

    ```json
    {
      "context": {
        "email": "<? @email.literal ?>"
      }
    }
    ```
    {: codeblock}

Click [here](docs/services/conversation/dialog-runtime.html#context-var-define) to go to the generally available version of this information, which describes how the feature works currently.

## Organizing the dialog with folders BETA
{: #dialog-folders-add}

You can group dialog nodes together by adding them to a folder. There are lots of reasons to group nodes, including:

- To keep nodes that address a similar subject together to make them easier to find. For example, you might group nodes that address questions about user accounts in a *User account* folder and nodes that handle payment-related queries in a *Payment* folder.
- To group together a set of nodes that you want the dialog to process only if a certain condition is met. Use a condition, such as `$isPlatinumMember`, for example, to group together nodes that offer extra services that should only be processed if the current user is entitled to receive the extra services.
- To apply the same configuration settings for digressing into a node to multiple root nodes at once. See [Digressions](#digressions) for more information.

The only facet of the folder that is processed at run time is the folder condition, if one is specified. The dialog service evaluates the folder condition to determine whether to process the nodes within it. Otherwise, the folder itself is ignored. It has no impact on the the node tree hierarchy. Any root nodes that you add to the folder continue to function as root nodes, for example. They do not become child nodes of the folder.

The evaluation order of nodes in the tree from the first to last is not interrupted by folders. As the service travels down the tree, when it encounters a folder, if the folder condition is true, it immediately processes the first root node in the folder, and continues down the tree in order from there.

To add a folder to a dialog tree, complete the following steps:

1.  From the tree view of the **Dialog** tab, click **Add folder**.

    The folder is added to the end of the dialog tree, just before the **Anything else** node.

    If you want to add the folder elsehwere in the tree, click the **More** ![More icon](images/kabob.png) icon on the node above the spot where you want to add it, and then select **Add folder**.

    The folder is opened in edit view.

1.  **Optional**: Name the folder.

1.  **Optional**: Define a condition for the folder.

    If you do not specify a condition, `true` is used, meaning the nodes in the folder are always processed.

1.  To add existing dialog nodes to the folder, you must move them to the folder one at a time. 

    On the node that you want to move, click the **More** ![More icon](images/kabob.png) icon, select **Move**, and then click the folder. Select **To folder** as the move to target.

    As you move nodes, they are added to the top of the tree within the folder. If you want to retain the order of a set of consecutive root dialog nodes, move them starting with the last node first.
    {: tip}

1.  To add a new dialog node to the folder, click the **More** ![More icon](images/kabob.png) icon on the folder, and then select **Add node to folder**.

    The dialog node is added as a root node and is positioned at the bottom of the dialog tree within the folder.

## Deleting a folder
{: #dialog-folders-delete}

You can delete either a folder alone or the folder and all of the dialog nodes in it.

To delete a folder, complete the following steps:

1.  From the tree view of the **Dialog** tab, find the folder that you want to delete.

1.  Click the **More** ![More icon](images/kabob.png) icon on the folder, and then select **Delete**.

1.  Do one of the following things:

    - To delete the folder, but keep the dialog nodes that are in the folder, deselect the **Delete the nodes inside the folder** checkbox, and then click **Yes, delete it**.
    - To delete the folder and all of the dialog nodes in it, click **Yes, delete it**.

If you deleted the folder only, then the nodes that were in the folder are displayed in the main dialog tree in the spot where the folder used be.

## Digressions BETA
{: #digressions}

A digression occurs when a user is in the middle of a dialog flow that is designed to address one goal, and abruptly switches topics to initiate a dialog flow that is designed to address a different goal. The dialog has always supported the user's ability to change subjects. If none of the nodes in the dialog branch that is being processed match the goal of the user's latest input, the conversation goes back out to the tree to check the root node conditions for an appropriate match.

The digression settings that are available per node give you the ability to tailor this behavior even more. Namely, you can allow the conversation to return to the dialog flow that was interrupted when the digression occurred. For example, the user might be ordering a new phone, but switches topics to ask about tablets. Your dialog can answer the question about tablets, and then bring the user back to where they left off in the process of ordering a phone. Allowing digressions to occur and return gives your users more control over the flow of the conversation at run time. They can change topics, follow a dialog flow about the unrelated topic to its end, and then return to where they were before. The result is a dialog flow that more closely simulates a human-to-human conversation.

The following image uses a mockup of the dialog tree user interface to illustrate the concept of a digression. It shows how a user interacts with dialog nodes that are configured to allow digressions that return to the dialog flow that was in progress. The user starts to provide the information required to make a dinner reservation. In the middle of filling slots in the #reservation node, the user asks a question about vegetarian menu options. The dialog answers the user's new question by finding a node that addresses it amongst the root nodes (a node that conditions on the #cuisine intent). It then returns to the conversation that was in progress by showing the prompt for the next empty slot from the original dialog node.

![Shows someone who is providing details about a dinner reservation ask about vegetarian options, get an answer, and then return to providing reservation details.](images/digression.gif)

You can also limit where digressions are allowed.

- Block users from digressing away from a dialog branch. For example, when a branch has a complex flow that you do not want interrupted.
- Stop users from returning to a node if the node's response has already been displayed, and its child nodes are incidental to the node's goal.
- Prevent a root node from being considered as a possible digression target. See [Disabling digressions into a root node](#disable-digressions) for more details.

### Before you begin

As you test your overall dialog, decide when and where it makes sense to allow digressions and returns from digressions to occur. The following digression controls are applied to the nodes automatically. Only take action if you want to change this default behavior.

- Every root node in your dialog is configured to allow digressions to target them by default. A child node cannot be the target of a digression.
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

    The configuration options differ depending on the whether the node you are editing is a root node, a child node, a node with children, or a node with slots.

    **Digressions away from this node**

    If the circumstances listed earlier do not apply, then you can make the following choices:

    - **All node types**: Choose whether to allow users to digress away from the current node before they reach the end of the current dialog branch.

    - **All nodes that have children**: Choose whether you want the conversation to come back to the current node after a digression if the current node's response has already been displayed. Set the *Allow return from digressions triggered after this node's response* toggle to **No** to prevent the dialog from returning to the current node and continuing to process its branch.

      For example, if the user asks, `Do you sell cupcakes?` and the response, `We offer cupcakes in a variety of flavors and sizes` has been displayed, and then the user changes subjects, you might not want the dialog to return to where it left off. If the child node only captures any possible follow-up questions from the user, then it can safely be ignored.

      However, if the node relies on its child nodes to answer the bulk of the question, then you might want to force the conversation to return and continue processing the nodes in the current branch. For example, the initial response might be, `We offer cupcakes in all shapes and sizes. Which menu do you want to see: gluten-free, dairy-free, or regular?` If the user changes subjects at this point, you might want the dialog to return so the user can pick a menu type and get the information they wanted.

    - **Nodes with slots**: Choose whether you want to allow users to digress away from the node before all of the slots are filled. Set the *Allow digressions away while slot filling* toggle to **Yes** to enable digressions away.

      If enabled, when the conversation returns from the digression, the prompt for the next unfilled slot is displayed to encourage the user to continue providing information. If disabled, then any inputs that the user submits which do not contain a value that can fill a slot are ignored. However, you can address unsolicited questions that you anticipate your users might ask while they interact with the node by defining node-level handlers. See [Adding slots](dialog-slots.html#add-slots) for more information.

      The following image shows you how the #reservation node with slots that is shown in the earlier illustration is configured.

      ![Shows the digressions away settings from a node with slots.](images/digress-away-slots-full.png)

    - **Nodes with slots**: Choose whether the user is only allowed to digress away if they will return to the current node by selecting the **Only digress from slots to nodes that allow returns** checkbox.

      When set, as the dialog looks for a node to answer the user's unrelated question, it does not consider any root nodes that are not configured to allow the dialog to return. Select this checkbox if you want to prevent users from being able to permanently leave the node before they have finished filling the required slots.

    **Digressions into this node**

    You can make the following choices about how digressions into a node behave:

    - Prevent users from being able to digress into the node. See [Disabling digressions into a root node](#diable-digressions) for more details.

    - When digressions into the node are enabled, choose whether the dialog must go back to the dialog flow that it digressed away from after the processing of the current node's branch is finished. To allow the dialog to return afterwards, select **Go back after digression**.

    The following image shows you how the #cuisine node that is shown in the earlier illustration is configured.

    ![Shows the digressions away settings from a node with slots.](images/digress-into-cuisine-full.png)

1.  Click **Apply**.

1.  Use the "Try it out" pane to test the digression behavior.

    Again, you cannot define the start and end of a digression. The user controls where and when digressions happen. You can only apply settings that determine how a single node participates in one. Because digressions are so amorphous, it is hard to predict how your configuration decisions will impact the overall conversation. To truly see the impact of the choices you made, you must test the dialog.

The #reservation and #cuisine nodes represent two dialog branches that participate in a single user-directed digression. The digression settings that are configured for each individual node are what make this type of digression possible.

![Shows two dialogs, one that sets the digressions away from the reservation slots node and one that sets the digression into the cuisine node.](images/digression-settings.png)

### Disabling digressions into a root node
{: #disable-digressions}

When a flow digresses into a root node, it follows the course of the dialog that is configured for that node. So, it might process a series of child nodes before it reaches the end of the node branch, and then, if configured to do so, goes back to the dialog flow that was interrupted. Through dialog testing, you might find that a root node is triggered too often, or at unexpected times, or that its dialog is too complex and leads the user too far off course to be a good candidate for a temporary digression. If you determine that you would rather not allow users to digress into it, you can configure it to not allow digressions in.

To disable digressions into a root node altogether, complete the following steps:

1.  Click to open the root node that you want to edit.
1.  Click **Customize**, and then click the **Digressions** tab.
1.  Set the *Allow digressions into this node* toggle to **Off**.
1.  Click **Apply**.

If you decide that you want to prevent digressions into several root nodes, but do not want to edit each one individually, you can add the nodes to a folder. From the *Customize* page of the folder, you can set the *Allow digressions into this node* toggle to **Off** to apply the configuration to all of the nodes at once.

### Design considerations
{: #digression-design-considerations}

- **Avoid fallback node proliferation**: Many dialog designers include a node with a `true` or `anything_else` condition at the end of every dialog branch as a way to prevent users from getting stuck in the branch. This design returns a generic message if the user input does not match anything that you anticipated and included a specific dialog node to address. However, users cannot digress away from dialog flows that use this approach. Evaluate any branches that use it to see whether it would be better to allow digressions away from the branch. If the user's input does not match anything you anticipated, it might find a match against an entirely different dialog flow in your tree. Rather than responding with a generic message, you can put the rest of the dialog to work to try to address the user's input. And the root-level `Anything else` node can always respond to input that none of the other root nodes can address.

- **Reconsider jumps to a closing node**: Many dialogs are designed to ask a standard closing question, such as, `Did I answer your question today?` Users cannot digress away from nodes that are configured to jump to another node. So, if you configure all of your final branch nodes to jump to a common closing node, digressions cannot occur. Consider tracking user satisfaction through metrics or some other means.

- **Test possible digression chains**: If a user digresses away from the current node to another node that allows digressions away, the user could potentially digress away from that other node, and repeat this pattern one or more times again. If all the nodes in the digression chain are configured to go back after the digression, then the user will eventually be brought back to the current dialog node. However, test scenarios that digress multiple times to determine whether individual nodes function as expected.

- **Remember that the current node gets priority**: Remember that nodes outside the current flow are only considered as digression targets if the current flow cannot address the user input. It is even more important in a node with slots that allows digressions away, in particular, to make it clear to users what information is needed from them, and to add confirmation statements that are displayed after the user provides a value.

  Any slot can be filled during the slot-filling process. So, a slot might capture user input unexpectedly. For example, you might have a node with slots that collects the information necessary to make a dinner reservation. One of the slots collects date information. While providing the reservation details, the user might ask, `What's the weather meant to be tomorrow?` You might have a root node that conditions on #forecast which could answer the user. However, because the user's input includes the word `tomorrow` and the reservation node with slots is being processed, the service assumes the user is providing or updating the reservation date instead. *The current node always gets priority.* If you define a clear confirmation statement, such as, `Ok, setting the reservation date to tomorrow,` the user is more likely to realize there was a miscommunication and correct it.

  Conversely, while filling slots, if the user provides a value that is not expected by any of the slots, there is a chance it will match against a completely unrelated root node that the user never intended to digress to.

  Be sure to do lots of testing as you configure the digression behavior.

- **When to use digressions instead of slot node-level handlers**: For general questions that users might ask at any time, use a root node that allows digressions into it, and that goes back to the flow that was in progress. For nodes with slots, try to anticipate the types of related questions users might want to ask while filling in the slots, and address them by adding handlers to the node.

For example, if the node with slots collects the information required to fill out an insurance claim, then you might want to add handlers that address common questions about insurance. However, for questions about how to get help, or your stores locations, or the history of your company, use a root level node.
