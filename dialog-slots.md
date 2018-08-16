---

copyright:
  years: 2015, 2018
lastupdated: "2018-08-15"

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

# Gathering information with slots
{: #dialog-slots}

Add slots to a dialog node to gather multiple pieces of information from a user within that node. Slots collect information at the users' pace. Details the user provides upfront are saved, and the service asks only for the details they do not.

<iframe class="embed-responsive-item" id="youtubeplayer" title="Adding slots to a node" type="text/html" width="640" height="390" src="https://www.youtube.com/embed/kMLyKfmO9wI?rel=0" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>

## Why add slots?
{: #why-add-slots}

Use slots to get the information you need before you can respond accurately to the user. For example, if users ask about operating hours, but the hours differ by store location, you could ask a follow-up question about which store location they plan to visit before you answer. You can then add response conditions that take the provided location information into account.

![Asks for location information before answering the question, When do you open?.](images/op-hours.png)

Slots can help you to collect multiple pieces of information that you need to complete a complex task for a user, such as making a dinner reservation.

![Shows four slots that prompt for the information needed to make a dinner reservation.](images/reservation.png)

The user might provide values for mutliple slots at once. For example, the input might include the information, `There will be 6 of us dining at 7 PM.` This one input contains two of the missing required values: the number of guests and time of the reservation. The service recognizes and stores both of them, each one in its corresponding slot. It then displays the prompt that is associated with the next empty slot.

![Shows that two slots are filled, and the service prompts for the remaining one.](images/pass-in-info.png)

Slots make it possible for the service to answer follow-up questions without having to re-establish the user's goal. For example, a user might ask for a weather forecast, then ask a follow-up question about weather in another location or on a different day. If you save the required forecast variables, such as location and day, in slots, then if a user asks a follow-up question with new variable values, you can overwrite the slot values with the new values provided, and give a response that reflects the new information. (For more information about how to call an external service from a dialog, see [Making programmatic calls from a dialog node](dialog-actions.html)).

![Shows someone asking for a weather forecast, and then following up with a question about weather for a different location and time.](images/follow-up.png)

Using slots produces a more natural dialog flow between the user and the service, and is easier for you to manage than trying to collect the information by using many separate nodes.

## Adding slots
{: #add-slots}

1.  Identify the units of information that you want to collect. For example, to order a pizza for someone, you might want to collect the following information:

    - Delivery time
    - Size

1.  If you have not started to create a dialog, follow the instructions in [Creating a dialog](dialog-build.html) to create one.

1.  From the dialog node edit view, click **Customize**, and then click the toggle next to **Slots** to turn it **On**.

    **Note**: For more information about the **Prompt for everything** checkbox, see [Asking for everything at once](dialog-slots.html#slots-prompt-for-everything).

1.  **Add a slot for each unit of required information**. For each slot, specify these details:

    - **Check for**: Identify the type of information you want to extract from the user's response to the slot prompt. In most cases, you check for entity values. In fact, the condition builder that is displayed suggests entities that you can check for. However, you can also check for an intent; just type the intent name into the field. You can use AND and OR operators here to define more complex conditions.

      **Important**: The *Check for* value is first used as a condition, but then becomes the value of the context variable that you name in the *Save as* field. If you want to change how the value is saved (reformat it, for example), then add the expression that reformats the value directly to the **Check for** field.

      For example, if the entity has regular expression patterns defined for it, then after adding the entity name, append `.literal` to it. After you choose `@email` from the list of defined entities, for example, edit the **Check for** field to contain `@email.literal`. By adding the `.literal` property, you indicate that you want to capture the exact text that was entered by the user and was identified as an email address based on its pattern. Make this syntax change directly in the **Check for** field.

      **Warning** If you want to apply a complex expression to the value before you save the value, then you can open the JSON editor to define the complex SpEL expression. However, the complex expression that you define in the JSON editor will not be reflected in the **Check for** field when you exit the JSON editor. And if you click the **Check for** field to give the field focus at any time after you define the complex expression for the field, then the expression is removed.

      Avoid checking for context variable values. Because the value you check for is also the value that is saved, when you use a context variable in the condition, it can lead to unexpected behavior when it gets used in the context. Do not try to use an optional slot to display a response only if a given context variable is set. If the variable is set, then the slot Found response that you define for the optional slot will be displayed along with the response that is returned by every other slot, over and over again.
      {: tip}

    - **Save as**: Provide a name for the context variable in which to store the value of interest from the user's response to the slot prompt. Do not specify a context variable that is used earlier in the dialog, and therefor might have a value. It is only when the context variable for the slot is null that the prompt for the slot is displayed.

    - **Prompt**: Write a statement that elicits the piece of the information you need from the user. After displaying this prompt, the conversation pauses and the service waits for the user to respond.

    - If you want different follow-up statements to be shown based on whether the user provides the information you need in response to the initial slot prompt, you can edit the slot (by clicking the **Edit slot** ![Edit slot](images/edit-slot.png) icon) and define the follow-up statements:

      - **Found**: Displayed after the user provides the expected information.

      - **Not found**: Displayed if the information provided by the user is not understood, or is not provided in the expected format. If the slot is filled successfully, or the user input is understood and handled by a slot handler, then this statement is never displayed.

      For information about how to define conditions and associated actions for Found and Not found responses, see [Adding conditions to Found and Not found responses](dialog-slots.html#slot-handler-next-steps).

    This table shows example slot values for a node that helps users place a pizza order by collecting two pieces of information, the pizza size and delivery time.

    <table>
    <caption>Example slots for pizza order</caption>
    <tr>
      <th>Check for</th>
      <th>Save as</th>
      <th>Prompt</th>
      <th>Follow-up if found</th>
      <th>Follow-up if not found</th>
    </tr>
    <tr>
      <td>@size</td>
      <td>$size</td>
      <td>What size pizza would you like?</td>
      <td>$size it is.</td>
      <td>What size did you want? We have small, medium, and large.</td>
    </tr>
    <tr>
      <td>@sys-time</td>
      <td>$time</td>
      <td>When do you need the pizza by?</td>
      <td>For delivery by $time.</td>
      <td>What time did you want it delivered? We need at least a half hour to prepare it.</td>
    </tr>
    </table>

1.  **Make a slot optional or disable it under certain conditions**. You can optionally configure a slot in these ways:

    - **Optional**: To make a slot optional, add a slot without a prompt. The service does not ask the user for the information, but it does look for the information in the user input, and saves the value if the user provides it. For example, you might add a slot that captures dietary restriction informations in case the user specifies any. However, you don't want to ask all users for dietary information since it is irrelevant in most cases.

       <table>
       <caption>Optional slot</caption>
       <tr>
          <th>Information</th>
          <th>Check for</th>
          <th>Save as</th>
       </tr>
       <tr>
          <td>Wheat restriction</td>
          <td>@dietary</td>
          <td>$dietary</td>
      </tr>
      </table>

      If you make a slot optional, only reference its context variable in the node-level response text if you can word it such that it makes sense even if no value is provided for the slot. For example, you might word a summary statement like this, `I am ordering a $size $dietary pizza for delivery at $time.` The resulting text makes sense whether the dietary restriction information, such as `gluten-free` or `dairy-free`, is provided or not. The result is either, `I am ordering a large gluten-free pizza for delivery at 3:00PM.` or `I am ordering a large pizza for delivery at 3:00PM.`
      {: tip}

    - **Conditional**: If you want a slot to be enabled only under certain conditions, then you can add a condition to it. For example, if slot 1 asks for a meeting start time, slot 2 captures the meeting duration, and slot 3 captures the end time, then you might want to enable slot 3 (and ask for the meeting end time) only if a value for slot 2 is not provided. To make a slot conditional, edit the slot, and then from the **More** ![More icon](images/kabob.png) menu, select **Enable condition**. Define the condition that must be met for the slot to be enabled.

      You can condition on the value of a context variable from an earlier slot because the order in which the slots are listed is the order in which they are evaluated. However, only condition on a slot context variable that you can be confident will contain a value when this slot is evaluated. The earlier slot must be a required slot, for example.
    {: tip}
1.  **Keep users on track**. You can optionally define slot handlers that provide responses to questions users might ask during the interaction that are tangential to the purpose of the node.

    For example, the user might ask about the tomato sauce recipe or where you get your ingredients. To handle such off-topic questions, click the **Manage handlers** link and add a condition and response for each anticipated question.

    ![Shows a user ask about the sauce recipe. The response is, I'll take it to my grave'.](images/sauce.png)

    After responding to the off-topic question, the prompt associated with the current empty slot is displayed.

    This condition is triggered if the user provides input that matches the slot handler conditions at any time during the dialog node flow up until the node-level response is displayed. See [Handling requests to exit a process](dialog-slots.html#slots-node-level-handler) for more ways to use the slot handler.
1.  **Add a node-level response**. The node-level response is not executed until after all of the required slots are filled. You can add a response that summarizes the information you collected. For example, `A $size pizza is scheduled for delivery at $time. Enjoy!`

    If you want to define different responses based on certain conditions, click **Customize**, and then click the **Multiple responses** toggle to turn it **On**. For information about conditional responses, see [Conditional responses](dialog-overview.html#multiple).
1.  **Add logic that resets the slot context variables**. As you collect answers from the user per slot, they are saved in context variables. You can use the context variables to pass the information to another node or to an application or external service for use. However, after passing the information, you must set the context variables to null to reset the node so it can start collecting information again. You cannot null the context variables within the current node because the service will not exit the node until the required slots are filled. Instead, consider using one of the following methods:

    - Add processing to the external application that nulls the variables.
    - Add a child node that nulls the variables.
    - Insert a parent node that nulls the variables, and then jumps to the node with slots.

Give it a try! Follow the step-by-step [tutorial](tutorial-slots.html).

## Slots usage tips
{: #slots-tips}

The following slot properties can help you check and set values in slot context variables.

| Property name          | Description |
|------------------------|-------------|
| `all_slots_filled`     | Evaluates to true only if all of the context variables for all of the slots in the node have been set. See [Preventing a Found response from displaying when it is not needed](dialog-slots.html#slots-stifle-found-responses) for a usage example. |
| `event.current_value`  | Current value of the context variable for this slot. See [Replacing a slot context variable value](dialog-slots.html#slots-found-handler-event-properties) for a usage example for this property and the event.previous_value property. |
| `event.previous_value` | Previous value of the context variable for this slot. |
| `has_skipped_slots`    | True if any of the slots or slot handlers that are configured with a next step option that skips slots was processed. See [Adding conditions to Found and Not found responses](dialog-slots.html#slot-handler-next-steps) for more information about next step options for slots and [Handling requests to exit a process](dialog-slots.html#slots-node-level-handler) for information about next step options for slot handlers. |
| `slot_in_focus`        | Forces the slot condition to be applied to the current slot only. See [Getting confirmation](dialog-slots.html#slots-get-confirmation) for more details. |
{: caption="Slot properties" caption-side="top"}

Consider using these approaches for handling common tasks.

- [Asking for everything at once](dialog-slots.html#slots-prompt-for-everything)
- [Capturing multiple values](dialog-slots.html#slots-multiple-entity-values)
- [Reformatting values](dialog-slots.html#slots-reformat-values)
- [Dealing with zeros](dialog-slots.html#slots-zero)
- [Getting confirmation](dialog-slots.html#slots-get-confirmation)
- [Replacing a slot context variable value](dialog-slots.html#slots-found-handler-event-properties)
- [Avoiding number confusion](dialog-slots.html#slots-avoid-number-confusion)
- [Adding conditions to Found and Not found responses](dialog-slots.html#slot-handler-next-steps)
- [Moving on after multiple failed attempts](dialog-slots.html#slots-stop-trying-after-3)
- [Preventing a Found response from displaying when it is not needed](dialog-slots.html#slots-stifle-found-responses)
- [Handling requests to exit a process](dialog-slots.html#slots-node-level-handler)

### Asking for everything at once
{: #slots-prompt-for-everything}

Include an initial prompt for the whole node that clearly tells users which units of information you want them to provide. Displaying this prompt first gives users the opportunity to provide all the details at once and not have to wait to be prompted for each piece of information one at a time.

For example, when the node is triggered because a customer wants to order a pizza, you can respond with the preliminary prompt, `I can take your pizza order. Tell me what size pizza you want and the time that you want it delivered.`

If the user provides even one piece of this information in their initial request, then the prompt is not displayed. For example, the initial input might be, `I want to order a large pizza.` When the service analyzes the input, it recognizes `large` as the pizza size and fills the **Size** slot with the value provided. Because one of the slots is filled, it skips displaying the initial prompt to avoid asking for the pizza size information again. Instead, it displays the prompts for any remaining slots with missing information.

From the Customize pane where you enabled the Slots feature, select the **Prompt for everything** checkbox to enable the intial prompt. This setting adds the **If no slots are pre-filled, ask this first** field to the node, where you can specify the text that prompts the user for everything.

### Capturing multiple values
{: #slots-multiple-entity-values}

You can ask for a list of items and save them in one slot.

For example, you might want to ask users whether they want toppings on their pizza. To do so define an entity (@toppings), and the accepted values for it (pepperoni, cheese, mushroom, and so on). Add a slot that asks the user about toppings. Use the values property of the entity type to capture multiple values, if provided.

<table>
<caption>Multiple value slot</caption>
<tr>
  <th>Check for</th>
  <th>Save as</th>
  <th>Prompt</th>
  <th>Follow-up if found</th>
  <th>Follow-up if not found</th>
</tr>
<tr>
  <td>@toppings.values</td>
  <td>$toppings</td>
  <td>Any toppings on that?</td>
  <td>Great addition.</td>
  <td>What toppings would you like? We offer ...</td>
</tr>
</table>

To reference the user-specified toppings later, use the `<? $entity-name.join(',') ?>` syntax to list each item in the toppings array and separate the values with a comma. For example, `I am ordering you a $size pizza with <? $toppings.join(',') ?> for delivery by $time.`

### Reformatting values
{: #slots-reformat-values}

Because you are asking for information from the user and need to reference their input in responses, consider reformatting the values so you can display them in a friendlier format.

For example, time values are saved in the `hh:mm:ss` format. You can use the JSON editor for the slot to reformat the time value as you save it so it uses the `hour:minutes AM/PM` format instead:

```json
{
  "context":{
    "time": "<? @sys-time.reformatDateTime('h:mm a') ?>"
  }
}
```
{: codeblock}

See [Methods to process values](dialog-methods.html) for other reformatting ideas.

### Dealing with zeros
{: #slots-zero}

Using `@sys-number` in a slot condition is helpful for capturing any numbers that users specify in their input. However, it does not behave as expected when users specify the number zero (0). Instead of treating zero as a valid number, the condition is evaluated to false, and the service prompts the user for a number again. To prevent this behavior, check for `@sys-number` or `@sys-number:0` in the slot condition.

To ensure that a slot condition that checks for number mentions deals with zeros properly, complete the following steps:

1.  Add `@sys-number || @sys-number:0` to the slot condition field, and then provide the context variable name and text prompt.
1.  Click the **Edit response** ![Edit response](images/edit-slot.png) icon.
1.  Click the **More** ![More icon](images/kabob.png) menu, and then select **Open JSON editor**.
1.  Update the context variable which now has the syntax,  `"number":"@sys-number || @sys-number:0"`, to specify `@sys-number` only.

    ```json
    {
      "context":{
        "number":"@sys-number"
      }
    }
    ```
    {: codeblock}

If you do not want to accept a zero as the number value, then you can add a conditional response for the slot to check for zero, and tell the user that they must provide a number greater than zero. But, it is important for the slot condition to be able to recognize a zero when it is provided as input.

### Getting confirmation
{: #slots-get-confirmation}

Add a slot after the others that asks the user to confirm that the information you have collected is accurate and complete. The slot can look for responses that match the #yes or #no intent.

<table>
<caption>Confirmation slot</caption>
<tr>
  <th>Check for</th>
  <th>Save as</th>
  <th>Prompt</th>
  <th>Follow-up if found</th>
  <th>Follow-up if not found</th>
</tr>
<tr>
  <td>#yes || #no</td>
  <td>$confirmation</td>
  <td>I'm going to order you a `$size` pizza for delivery at `$time`. Should I go ahead?</td>
  <td>Your pizza is on its way!</td>
  <td>see *Complex response*</td>
</tr>
</table>

**Complex response** Because users might include affirmative or negative statements at other times during the dialog (*Oh yes, we want the pizza delivered at 5pm*) or (*no guests tonight, let's make it a small*), use the `slot_in_focus` property to make it clear in the slot condition that you are looking for a Yes or No response to the prompt for this slot only.

```json
(#yes || #no) && slot_in_focus
```
{: codeblock}

The `slot_in_focus` property always evaluates to a Boolean (true or false) value. Only include it in a condition for which you want a boolean result. Do not use it in slot conditions that checks for an entity type and then save the entity value, for example.
{: tip}

In the **Not found** prompt, clarify that you are expecting the user to provide a Yes or No answer.

```json
{
  "output":{
    "text": {
      "values": [
        "Respond with Yes to indicate that you want the order to
         be placed as-is, or No to indicate that you do not."
      ]
    }
  }
}
```
{: codeblock}

In the **Found** prompt, add a condition that checks for a No response (#no). When found, ask for the information all over again and reset the context variables that you saved earlier.

```json
{
  "conditions": "#no",
  "output":{
    "text": {
      "values": [
        "Let's try this again. Tell me what size pizza
         you want and the time..."
      ]
    }
  },
  "context":{
    "size": null,
    "time": null,
    "confirmation": null
  }
}
```
{: codeblock}

### Replacing a slot context variable value
{: #slots-found-handler-event-properties}

If, at any time before the user exits a node with slots, the user provides a new value for a slot, then the new value is saved in the slot context variable, replacing the previously-specified value. Your dialog can acknowledge explicitly that this replacement has occurred by using special properties that are defined for the Found condition:

- `event.previous_value`: Previous value of the context variable for this slot.
- `event.current_value`: Current value of the context variable for this slot.

For example, your dialog asks for a destination city for a flight reservation. The user provides `Paris.` You set the $destination slot context variable to *Paris*. Then, the user says, `Oh wait. I want to fly to Madrid instead.` If you set up the Found condition as follows, then your dialog can handle this type of change gracefully.

When user responds, if @destination is found:

```json
Condition: (event.previous_value != null) &&
           (event.previous_value != event.current_value)
    Response: Ok, updating destination from
    <? event.previous_value ?> to <? event.current_value ?>.
Response: Ok, destination is $destination.
```
{: codeblock}

This slot configuration enables your dialog to react to the user's change in destination by saying, `Ok, updating the destination from Paris to Madrid.`

### Avoiding number confusion
{: #slots-avoid-number-confusion}

Some values that are provided by users can be identified as more than one entity type.

You might have two slots that store the same type of value, such as an arrival date and departure date, for example. Build logic into your slot conditions to distinguish such similar values from one another.

In addition, the service can recognize multiple entity types in a single user input. For example, when a user provides a currency, it is recognized as both a @sys-currency and @sys-number entity type. Do some testing in the *Try it out* pane to understand how the system will interpret different user inputs, and build logic into your conditions to prevent possible misinterpretations.

In logic that is unique to the slots feature, when two entities are recognized in a single user input, the one with the larger span is used. For example, if the user enters *May 2*, even though the {{site.data.keyword.conversationshort}} service recognizes both @sys-date (05022017) and @sys-number (2) entities in the text, only the entity with the longer span (@sys-date) is registered and applied to a slot.
{: tip}

### Adding conditions to Found and Not found responses
{: #slot-handler-next-steps}

For each slot, you can use conditional responses with associated actions to help you extract the information you need from the user. To do so, follow these steps:

1.  Click the **Edit slot** ![Edit slot](images/edit-slot.png) icon for the slot to which you want to add conditional Found and Not found responses.
1.  From the **More** ![More icon](images/kabob.png) menu, select **Enable conditional responses**.
1.  Enter the condition and the response to display if the condition is met.

    **Found example**: The slot is expecting the time for a dinner reservation. You might use @sys-time in the *Check for* field to capture it. To prevent an invalid time from being saved, you can add a conditional response that checks whether the time provided is before the restaurant's last seating time, for example. `@sys-time.after('21:00:00')` The corresponding response might be something like, *Our last seating is at 9PM.*

    **Not found example**: The slot is expecting a @location entity that accepts a specific set of cities where the restaurant chain has restaurants. The Not found condition might check for @sys-location in case the user specifies a valid city, but one in which the chain has no sites. The corresponding response might be, *We have no restaurants in that location.*

1.  If you want to customize what happens next if the condition is met, then click the **Edit response** ![Edit response](images/edit-slot.png) icon.

    For Found responses (that are displayed when the user provides a value that matches the value type specified in the Check for field), you can choose one of these actions to perform next:

      - **Move on (Default)**: Instructs the service to move on to the next empty slot after displaying the response. In the associated response, assure the user that their input was understood. For example, *Ok. You want to schedule it for $date.*
      - **Clear slot and prompt again**: If you are using an entity in the *Check for* field that could pick up the wrong value, add conditions that catch any likely misinterpretations, and use this action to clear the current slot value and prompt for the correct value.
      - **Skip to response**: If, when the condition you define is met, you no longer need to fill any of the remaining slots in this node, choose this action to skip the remaining slots and go directly to the node-level response next. For example, you could add a condition that checks whether the user's age is under 16. If so, you might skip the remaining slots which ask questions about the user's driving record.

    For Not found responses (that are displayed when the user does not provide a valid value), you can choose one of these actions to perform:

      - **Wait for user input (Default)**: Pauses the conversation and the service waits for the user to respond. In the simplest case, the text you specify here can more explicitly state the type of information you need the user to provide. If you use this action with a conditional response, be sure to word the conditional response such that you clearly state what was wrong with the user's answer and what you expect them to provide instead.
      - **Prompt again**: After displaying the Not found response, the service repeats the slot prompt again and waits for the user to respond. If you use this action with a conditional response, the response can merely explain what was wrong about the answer the user provided. It does not need to reiterate the type of information you want the user to provide because the slot prompt typically explains that.

        If you choose this option, consider adding at least one variation of the Not found response so that the user does not see the exact same text more than once. Take the opportunity to use different wording to explain to the user what information you need them to provide and in what format.
        {: tip}

      - **Skip this slot**: Instructs the service to stop trying to fill the current slot, and instead, move on to the prompt for the next empty slot. This option is useful in a slot where you want to both make the slot optional and to display a prompt that asks the user for information. For example, you might have a @seating entity that captures restaurant seating preferences, such as *outside*, *near the fireplace*, *private*, and so on. You can add a slot that prompts the user with, *Do you have any seating preferences?* and checks for `@seating.values`. If a valid response is provided, it saves the preference information to `$seating_preferences`. However, by choosing this action as the Not found response next step, you instruct the service to stop trying to fill this slot if the user does not provide a valid value for it.
      - **Skip to response**: If, when the condition you define is met, you no longer need to fill any of the remaining slots in this node, choose this action to skip the remaining slots and go directly to the node-level response next. For example, if after capturing the one-way flight information, the slot prompt is, *Are you buying round trip tickets?* the Not found condition can check for #No. If #No is found, use this option to skip the remaining slots that capture information about the return flight, and go straight to the node-level response instead.

    Click **Back** to return to the edit view of the slot.
1.  To add another conditional response, click **Add a response**, and then enter the condition and the response to display if the condition is met.

    Be sure to add at least one response that will be displayed no matter what. You can leave the condition field blank for this catch all response. The service automatically populates the empty condition field with the `true` special condition.

1.  Click **Save** to save your changes, close the edit view of the slot, and return to the edit view of the node.

### Moving on after multiple failed attempts
{: #slots-stop-trying-after-3}

You can provide users with a way to exit a slot if they cannot answer it correctly after several attempts by using Not found conditional responses. In the catchall response, open the JSON editor to add a counter context variable that will keep track of the number of times the Not found response is returned. In an earlier node, be sure to set the initial counter context variable value to 0.

In this example, the service asks for the pizza size. It lets the user answer the question incorrectly 3 times before applying a size (medium) to the variable for the user. (You can include a confirmation slot where users can always correct the size when they are asked to confirm the order information.)

Check for: @size
Save as: $size
Not found catchall condition:

```json
{
  "output": {
    "text": {
      "values": [
        "What size did you want? We have small, medium, and large."
      ],
      "selection_policy": "sequential"
    }
  },
  "context": {
    "counter": "<? context['counter'] + 1 ?>"
  }
}
```
{: codeblock}

To respond differently after 3 attempts, add another Not found condition like this:
```json
{
  "conditions": "$counter > 1",
  "output": {
    "text": {
      "values": [
        "We will bring you a size medium pizza."
      ]
    }
  },
  "context": {
    "size": "medium"
  }
  ```
  {: codeblock}

This Not found condition is more precise than the Not found catchall condition, which defaults to `true`. Therefore, you must move this response so it comes before the original conditional response or it will never be triggered. Select the conditional response and use the up arrow to move it up.

### Preventing a Found response from displaying when it is not needed
{: #slots-stifle-found-responses}

If you specify Found responses for multiple slots, then if a user provides values for multiple slots at once, the Found response for at least one of the slots will be displayed. You probably want either the Found response for all of them or none of them to be returned.

To prevent Found responses from being displayed, you can do one of the following things to each Found response:

- Add a condition to the response that prevents it from being displayed if particular slots are filled. For example, you can add a condition, like `!($size && $time)`, that prevents the response from being displayed if the $size and $time context variables are both provided.
- Add the `!all_slots_filled` condition to the response. This setting prevents the response from being displayed if all of the slots are filled. Do not use this approach if you are including a confirmation slot. The confirmation slot is also a slot, and you typically want to prevent the Found responses from being displayed before the confirmation slot itself is filled.

### Handling requests to exit a process
{: #slots-node-level-handler}

Add at least one slot handler that can recognize it when a user wants to exit the node.

For example, in a node that collects information to schedule a pet grooming appointment, you can add a handler that conditions on the #cancel intent, which recognizes utterances such as, <q>Forget it. I changed my mind.</q>

1.  In the JSON editor for the handler, fill all of the slot context variables with dummy values to prevent the node from continuing to ask for any that are missing. And in the handler response, add a message such as, `Ok, we'll stop there. No appointment will be scheduled.`
1.  Choose what action you want the service to take next from the following options:

    - **Prompt again (Default)**: Displays the prompt for the slot that the user was working with just before asking the off-topic question.
    - **Skip current slot**: Displays the prompt associated with the slot that comes after the slot that the user was working with just before asking the off-topic question. And the service makes not further attempts to fill the skipped slot.
    - **Skip to response**: Skips the prompts for all remaining empty slots including the slot the user was working with just before asking the off-topic question.

1.  In the node-level response, add a condition that checks for a dummy value in one of the slot context variables. If found, show a final message such as, `If you decide to make an appointment later, I'm here to help.` If not found, it displays the standard summary message for the node, such as `I am making a grooming appointment for your $animal at $time on $date.`

Here's a sample of JSON that defines a handler for the pizza example. Note that, as described earlier, the context variables are all being set to dummy values. In fact, the `$size` context variable is being set to `dummy`. This $size value triggers the node-level response to show the appropriate message and exit the slots node.

```json
{
"conditions": "#cancel",
 "output": {
   "text": {
     "values": [
       "Ok, we'll stop there. No pizza delivery will be scheduled."
     ],
    "selection_policy": "sequential"
    }
  },
"context": {
   "time": "12:00:00",
   "size": "dummy",
   "confirmation":"true"
}
}
```
{: codeblock}

**Important**: Take into account the logic used in conditions that are evaluated before this condition so you can build distinct conditions. When a user input is received, the conditions are evaluated in the following order:

- Current slot level Found conditions.
- Slot handlers in the order they are listed.
- If digressions away are allowed, root level node conditions are checked for a match.
- Current slot level Not found conditions.

Be careful about adding conditions that always evaluate to true (such as the special conditions, `true` or `anything_else`) as slot handlers. Per slot, if the slot handler evaluates to true, then the Not found condition is skipped entirely. So, using a slot handler that always evaluates to true effectively prevents the Not found condition for every slot from being evaluated.
{: tip}

For example, you groom all animals except cats. For the Animal slot, you might be tempted to use the following slot condition to prevent `cat` from being saved in the Animal slot:

```json
Check for @animal && !@animal:cat, then save it as $animal.
```
{: codeblock}

And to let users know that you do not accept cats, you might specify the following value in the Not found condition of the Animal slot:

```json
If @animal:cat then, "I'm sorry. We do not groom cats."
```
{: codeblock}

While logical, if you also define an #exit slot handler, then - given the order of condition evaluation - this Not found condition will likely never get triggered. Instead, you can use this slot condition:

```json
Check for @animal, then save it as $animal.
```
{: codeblock}

And to deal with a possible `cat` response, add this value to the Found condition:

```json
If @animal:cat then, "I'm sorry. We do not groom cats."
```
{: codeblock}

In the JSON editor for the Found condition, reset the value of the $animal context variable because it is currently set to cat and should not be.

```json
{
  "output":{
    "text": {
      "values": [
        "I'm sorry. We do not groom cats."
      ]
    }
  },
  "context":{
    "animal": null
  }
}
```
{: codeblock}

## Slots examples

To access JSON files that implement different common slot usage scenarios, go to the community [conversation repo ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://github.com/watson-developer-cloud/community/tree/master/watson-assistant){: new_window} in GitHub.

To explore an example, download one of the example JSON files, and then import it as a new conversational skill. From the Dialog tab, you can review the dialog nodes to see how slots were implemented to address different use cases.
