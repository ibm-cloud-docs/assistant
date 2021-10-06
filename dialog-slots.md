---

copyright:
  years: 2015, 2021
lastupdated: "2021-09-27"

keywords: slot, slots

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
{:table: .aria-labeledby="caption"}
{:video: .video}

# Gathering information with slots
{: #dialog-slots}

Add slots to a dialog node to gather multiple pieces of information from a user within that node. Slots collect information at the user's pace. Details that a user provides up front are saved, and your assistant asks only for the missing details it needs to fulfill the request.

![Adding slots to a node](https://www.youtube.com/embed/kMLyKfmO9wI){: video output="iframe" id="youtubeplayer" frameborder="0" width="560" height="315" webkitallowfullscreen mozallowfullscreen allowfullscreen}

To read a transcript of the video, [open the video on YouTube.com](https://www.youtube.com/watch?v=kMLyKfmO9wI&feature=emb_imp_woyt), click the *More actions* icon, and then choose *Open transcript*.

## Why add slots?
{: #dialog-slots-why}

Use slots to get the information you need before you can respond accurately to the user. For example, if users ask about operating hours, but the hours differ by store location, you could ask a follow-up question about which store location they plan to visit before you answer. You can then add response conditions that take the provided location information into account.

![Asks for location information before answering the question, When do you open?.](images/op-hours.png)

Slots can help you to collect multiple pieces of information that you need to complete a complex task for a user, such as making a dinner reservation.

![Shows four slots that prompt for the information needed to make a dinner reservation.](images/reservation.png)

The user might provide values for mutliple slots at once. For example, the input might include the information, `There will be 6 of us dining at 7 PM.` This one input contains two of the missing required values: the number of guests and time of the reservation. Your assistant recognizes and stores both of them, each one in its corresponding slot. It then displays the prompt that is associated with the next empty slot.

![Shows that two slots are filled, and the service prompts for the remaining one.](images/pass-in-info.png)

Slots make it possible for your assistant to answer follow-up questions without having to reestablish the user's goal. For example, a user might ask for a weather forecast, then ask a follow-up question about weather in another location or on a different day. If you save the required forecast variables, such as location and day, in slots, then if a user asks a follow-up question with new variable values, you can overwrite the slot values with the new values provided, and give a response that reflects the new information. (For more information about how to call an external service from a dialog, see [Making programmatic calls from a dialog node](/docs/assistant?topic=assistant-dialog-webhooks)).

![Shows someone asking for a weather forecast, and then following up with a question about weather for a different location and time.](images/follow-up.png)

Using slots produces a more natural dialog flow between the user and your assistant, and is easier for you to manage than trying to collect the information by using many separate nodes.

## Adding slots
{: #dialog-slots-add}

1.  Identify the units of information that you want to collect. For example, to order a pizza for someone, you might want to collect the following information:

    - Delivery time
    - Size

1.  If you have not started to create a dialog, follow the instructions in [Creating a dialog](/docs/assistant?topic=assistant-dialog-overview) to create one.

1.  From the dialog node edit view, click **Customize**, and then set the **Slots** switch to **On**.

    For more information about the **Prompt for everything** checkbox, see [Asking for everything at once](#dialog-slots-prompt-for-everything).

1.  Add a slot for each unit of required information. {: #dialog-slots-response-types}

    For each slot, specify these details:

    - **Check for**: Identify the type of information you want to extract from the user's response to the slot prompt. In most cases, you check for entity values. In fact, the condition builder that is displayed suggests entities that you can check for. However, you can also check for an intent; just type the intent name into the field. You can use AND and OR operators here to define more complex conditions.

      The *Check for* value is first used as a condition, but then becomes the value of the context variable that you name in the *Save it as* field. It specifies both **what to check for** and **what to save**. If you want to change how the value is saved, then add the expression that reformats the value to the *Save it as* field.
      {: important}

      For example, if the entity is a pattern entity, such as `@email`, then after adding the entity name, append `.literal` to it. Adding `.literal` indicates that you want to capture the exact text that was entered by the user and was identified as an email address based on its pattern.

      In some cases, you might want to use an expression to capture the value, but not apply the expression to what is saved. In such cases, you can use one value in the *Check for* field to capture the value, and then open the JSON editor to change the value of the context variable, so it saves something else.

      Any edit you make to a slot's context variable value in the JSON editor is not reflected in the **Check for** field after you exit the JSON editor. You must reopen the JSON editor to see what will be saved as the context variable value.
      {: note}

      Avoid checking for context variable values in the *Check for* field. Because the value you check for is also the value that is saved, using a context variable in the condition can lead to unexpected behavior.

    - **Save it as**: Provide a name for the context variable in which to store the value of interest from the user's response to the slot prompt.

       Do not reuse a context variable that is used elsewhere in the dialog. If the context variable has a value already, then the slot's prompt is not displayed. It is only when the context variable for the slot is null that the prompt for the slot is displayed.

    - **Prompt**: Write a statement that elicits the piece of the information you need from the user. After displaying this prompt, the conversation pauses and your assistant waits for the user to respond.

      If you want the prompt to be something other than a text response, you can change the response type by clicking the **Customize slot** ![Customize slot](images/edit-slot.png) icon. Click **Text** to choose a different response type. 
      
      Response type options:

    - [**Connect to human agent**](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-add-connect-to-human-agent)
    - [**Image**](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-add-image)
    - [**Option**](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-add-option)
    - [**Pause**](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-add-pause)
    - [**Search skill**](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-add-search-skill) ![Plus or higher plans only](images/plus.png) 
    
      This response type is visible only to users of paid plans.
      {: note}

    - [**Text**](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-simple-text)

    This table shows example slot values for a node that helps users place a pizza order by collecting two pieces of information, the pizza size and delivery time.

    | Check for | Save it as | Prompt | Follow-up if found | Follow-up if not found |
    | --- | --- | --- | --- | --- |
    | @size | $size | What size pizza would you like? | $size it is. | What size did you want? We have small, medium, and large. |
    | @sys-time | $time | When do you need the pizza by? | For delivery by $time. | What time did you want it delivered? We need at least a half hour to prepare it. |
    {: caption="Example slots for pizza order" caption-side="top"}

1.  **Add slot value validation**: If you want different follow-up statements to be shown based on whether the user provides the information you need in response to the initial slot prompt, you can edit the slot (by clicking the **Customize slot** ![Customize slot](images/edit-slot.png) icon) and define the follow-up statements:

    **Found**: Displayed after the user provides the expected information.
  
    **Not found**: Displayed only if the information provided by the user is not understood, which means all of the following are true:
      - None of the active slots in the node are filled successfully
      - No slot handlers are understood
      - If digressions away are enabled for the node, no top-level dialog nodes are triggered as a digression from slot filling

    For information about how to define conditions and associated actions for *Found* and *Not found* responses, see [Adding conditions to Found and Not found responses](#dialog-slots-handler-next-steps).

1.  **Make a slot optional or disable it under certain conditions**. You can optionally configure a slot in these ways:

    - **Optional**: To make a slot optional, add a slot without a prompt. Your assistant does not ask the user for the information, but it does look for the information in the user input, and saves the value if the user provides it. For example, you might add a slot that captures dietary restriction informations in case the user specifies any. However, you don't want to ask all users for dietary information since it is irrelevant in most cases.

    | Information | Check for | Save it as |
    | --- | --- | --- |
    | Wheat restriction | @dietary | $dietary |
    {: caption="Optional slot" caption-side="top"}
      
      If you make a slot optional, only reference its context variable in the node-level response text if you can word it such that it makes sense even if no value is provided for the slot. For example, you might word a summary statement like this, `I am ordering a $size $dietary pizza for delivery at $time.` The resulting text makes sense whether the dietary restriction information, such as `gluten-free` or `dairy-free`, is provided or not. The result is either, `I am ordering a large gluten-free pizza for delivery at 3:00PM.` or `I am ordering a large pizza for delivery at 3:00PM.`
      {: tip}

    - **Conditional**: If you want a slot to be enabled only under certain conditions, then you can add a condition to it. For example, if slot 1 asks for a meeting start time, slot 2 captures the meeting duration, and slot 3 captures the end time, then you might want to enable slot 3 (and ask for the meeting end time) only if a value for slot 2 is not provided. To make a slot conditional, edit the slot, and then from the **More** ![More icon](images/kebab.png) menu, select **Enable condition**. Define the condition that must be met for the slot to be enabled.

      You can condition on the value of a context variable from an earlier slot because the order in which the slots are listed is the order in which they are evaluated. However, only condition on a slot context variable that you can be confident will contain a value when this slot is evaluated. The earlier slot must be a required slot, for example.
    {: tip}

1.  **Keep users on track**. You can optionally define slot handlers that provide responses to questions users might ask during the interaction that are tangential to the purpose of the node.

    For example, the user might ask about the tomato sauce recipe or where you get your ingredients. To handle such off-topic questions, click the **Manage handlers** link and add a condition and response for each anticipated question.

    ![Shows a user ask about the sauce recipe. The response is, I'll take it to my grave'.](images/sauce.png)

    After responding to the off-topic question, the prompt associated with the current empty slot is displayed.

    This condition is triggered if the user provides input that matches the slot handler conditions at any time during the dialog node flow up until the node-level response is displayed. See [Handling requests to exit a process](#dialog-slots-node-level-handler) for more ways to use the slot handler.
1.  **Add a node-level response**. The node-level response is not executed until after all of the required slots are filled. You can add a response that summarizes the information you collected. For example, `A $size pizza is scheduled for delivery at $time. Enjoy!`

    You can alternatively show an image or list of options as a response instead of a text response. See [Response type options](#dialog-slots-response-types).

    If you want to define different responses based on certain conditions, click **Customize**, and then set the **Multiple responses** switch to **On**. For information about conditional responses, see [Conditional responses](/docs/assistant?topic=assistant-dialog-overview#dialog-overview-multiple).
1.  **Add logic that resets the slot context variables**. As you collect answers from the user per slot, they are saved in context variables. You can use the context variables to pass the information to another node or to an application or external service for use. However, after passing the information, you must set the context variables to null to reset the node so it can start collecting information again. You cannot null the context variables within the current node because your assistant will not exit the node until the required slots are filled. Instead, consider using one of the following methods:

    - Add processing to the external application that nulls the variables.
    - Add a child node that nulls the variables.
    - Insert a parent node that nulls the variables, and then jumps to the node with slots.

Give it a try! Follow the step-by-step [tutorial](/docs/assistant?topic=assistant-tutorial-slots).

## Slots usage tips
{: #dialog-slots-tips}

The following slot properties can help you check and set values in slot context variables.

| Property name          | Description |
|------------------------|-------------|
| `all_slots_filled`     | Evaluates to true only if all of the context variables for all of the slots in the node have been set. See [Preventing a Found response from displaying when it is not needed](#dialog-slots-stifle-found-responses) for a usage example. |
| `event.current_value`  | Current value of the context variable for this slot. See [Replacing a slot context variable value](#dialog-slots-found-handler-event-properties) for a usage example for this property and the event.previous_value property. |
| `event.previous_value` | Previous value of the context variable for this slot. |
| `has_skipped_slots`    | True if any of the slots or slot handlers that are configured with a next step option that skips slots was processed. See [Adding conditions to Found and Not found responses](#slot-handler-next-steps) for more information about next step options for slots and [Handling requests to exit a process](#dialog-slots-node-level-handler) for information about next step options for slot handlers. |
| `slot_in_focus`        | Forces the slot condition to be applied to the current slot only. See [Getting confirmation](#dialog-slots-get-confirmation) for more details. You can use this property to collect and store the exact words that are submitted by a customer. See [Collecting summary information from the customer](#dialog-slots-get-confirmation). |
{: caption="Slot properties" caption-side="top"}

Consider using these approaches for handling common tasks.

- [Asking for everything at once](#dialog-slots-prompt-for-everything)
- [Capturing multiple values](#dialog-slots-multiple-entity-values)
- [Reformatting values](#dialog-slots-reformat-values)
- [Dealing with zeros](#dialog-slots-zero)
- [Getting confirmation](#dialog-slots-get-confirmation)
- [Collecting summary information from the customer](#dialog-slots-get-summary)
- [Replacing a slot context variable value](#dialog-slots-found-handler-event-properties)
- [Avoiding number confusion](#dialog-slots-avoid-slot-confusion)
- [Adding conditions to Found and Not found responses](#dialog-slots-handler-next-steps)
- [Moving on after multiple failed attempts](#dialog-slots-stop-trying-after-3)
- [Preventing a Found response from displaying when it is not needed](#dialog-slots-stifle-found-responses)
- [Handling requests to exit a process](#dialog-slots-node-level-handler)

### Asking for everything at once
{: #dialog-slots-prompt-for-everything}

Include an initial prompt for the whole node that clearly tells users which units of information you want them to provide. Displaying this prompt first gives users the opportunity to provide all the details at once and not have to wait to be prompted for each piece of information one at a time.

For example, when the node is triggered because a customer wants to order a pizza, you can respond with the preliminary prompt, `I can take your pizza order. Tell me what size pizza you want and the time that you want it delivered.`

If the user provides even one piece of this information in their initial request, then the prompt is not displayed. For example, the initial input might be, `I want to order a large pizza.` When your assistant analyzes the input, it recognizes `large` as the pizza size and fills the **Size** slot with the value provided. Because one of the slots is filled, it skips displaying the initial prompt to avoid asking for the pizza size information again. Instead, it displays the prompts for any remaining slots with missing information.

From the Customize pane where you enabled the Slots feature, select the **Prompt for everything** checkbox to enable the intial prompt. This setting adds the **If no slots are pre-filled, ask this first** field to the node, where you can specify the text that prompts the user for everything.

### Capturing multiple values
{: #dialog-slots-multiple-entity-values}

You can ask for a list of items and save them in one slot.

For example, you might want to ask users whether they want toppings on their pizza. To do so define an entity (@toppings), and the accepted values for it (pepperoni, cheese, mushroom, and so on). Add a slot that asks the user about toppings. Use the values property of the entity type to capture multiple values, if provided.

| Check for | Save it as | Prompt | Follow-up if found | Follow-up if not found | 
| --- | --- | --- | --- | --- |
| @toppings.values | $toppings | Any toppings on that? | Great addition. | What toppings would you like? We offer ... |
{: caption="Multiple value slot" caption-side="top"}

To reference the user-specified toppings later, use the `<? $entity-name.join(',') ?>` syntax to list each item in the toppings array and separate the values with a comma. For example, `I am ordering you a $size pizza with <? $toppings.join(',') ?> for delivery by $time.`

### Reformatting values
{: #dialog-slots-reformat-values}

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

See [Expression language methods](/docs/assistant?topic=assistant-dialog-methods) for other reformatting ideas.

### Dealing with zeros
{: #dialog-slots-zero}

Using `@sys-number` in a slot condition is helpful for capturing any numbers that users specify in their input. However, it does not behave as expected when users specify the number zero (0). Instead of treating zero as a valid number, the condition is evaluated to false, and your assistant prompts the user for a number again. To prevent this behavior, check for an `@sys-number` mention that is greater than or equal to zero in the slot condition.

To ensure that a slot condition that checks for number mentions deals with zeros properly, complete the following step:

1.  Add `@sys-number >= 0` to the slot condition field, and then provide the context variable name and text prompt.
    
    What you check for in the input is also what is saved in the slot context variable. However, in this case, you want only the number (such as `5`) to be saved. You do not want to save `5 > = 0`. To change what is saved, you must edit the value of the context variable.

1.  Open the slot to edit it by clicking the **Customize slot** ![Customize slot](images/edit-slot.png) icon. From the **Options** ![More icon](images/kebab.png) menu, open the JSON editor.

1.  Change the context variable value.

    The value will look like this:

    ```json
    {
      "context": {
        "number": "@sys-number >= 0"
      }
    }
    ```
    {: codeblock}

    Change it to look like this:

    ```json
    {
      "context": {
        "number": "@sys-number"
      }
    }
    ```
    {: codeblock}

1.  Save your changes. 

The change you made to the context variable value is not reflected in the Check for field. You must reopen the JSON editor see what will be saved.
{: note}

If you do not want to accept a zero as the number value, then you can add a conditional response for the slot to check for zero, and tell the user that they must provide a number greater than zero. But, it is important for the slot condition to be able to recognize a zero when it is provided as input.

### Getting confirmation
{: #dialog-slots-get-confirmation}

Add a slot after the others that asks the user to confirm that the information you have collected is accurate and complete. The slot can look for responses that match the #yes or #no intent.

| Check for | Save it as | Prompt | Follow-up if found | Follow-up if not found | 
| --- | --- | --- | --- | --- |
| #yes || #no | $confirmation | I'm going to order you a `$size` pizza for delivery at `$time`. Should I go ahead? | Your pizza is on its way! | see *Complex response* |
{: caption="Confirmation slot" caption-side="top"}

**Complex response** Because users might include affirmative or negative statements at other times during the dialog (*Oh yes, we want the pizza delivered at 5pm*) or (*no guests tonight, let's make it a small*), use the `slot_in_focus` property to make it clear in the slot condition that you are looking for a Yes or No response to the prompt for this slot only.

```json
(#yes || #no) && slot_in_focus
```
{: codeblock}

The `slot_in_focus` property always evaluates to a boolean (true or false) value. Only include it in a condition for which you want a boolean result. Do not use it in slot conditions that check for an entity type and then save the entity value, for example.
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

### Collecting summary information from the customer
{: #dialog-slots-get-summary}

You might want to prompt a user to supply free form text in a dialog node with slots that you can save and refer to later. To do so, follow these steps:

1.  In the *Check for* field, add the following special property: `slot_in_focus`.
1.  Optionally, change the context variable name for the slot in the *Save it as* field. For example, you might want to change it to something like `summary`.
1.  In the *If not present, ask* field, ask the user to provide open-ended information. For example, `Can you summarize the problem?`
1.  To store the input in the customer's exact words, edit what is saved by using the JSON editor.
1.  Open the slot to edit it by clicking the **Customize slot** ![Customize slot](images/edit-slot.png) icon. From the **Options** ![More icon](images/kebab.png) menu, open the JSON editor.

1.  Change the context variable value.

    The value will look like this:

    ```json
    {
      "context": {
        "summary": "slot_in_focus"
      }
    }
    ```
    {: codeblock}

    Change the value that is saved in the `$summary` context variable. You want to capture and store whatever text the user submits. To do so, use a SpEL expression that captures the input text.

    ```json
    {
      "context": {
        "summary": "<? input.text ?>"
      }
    }
    ```
    {: codeblock}

1.  Save your changes. 

The change you made to the context variable value is not reflected in the *Check for* field. You must reopen the JSON editor to see what will be saved.
{: note}

### Replacing a slot context variable value
{: #dialog-slots-found-handler-event-properties}

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

### Avoiding slot filling confusion
{: #dialog-slots-avoid-slot-confusion}

When a user input is evaluated, the slot with the first slot condition to match it is filled only. Test for the following possible causes of misinterpretation, and address them:

- **Problem**: The same entity is used in more than one slot. 

    For example, `@sys-date` is used to capture the departure date in one slot and arrival date in another.

    **Solution**: Use slot found conditions that get clarification from the user about which date you are saving in a slot before you save it.

- **Problem**: A term fully or partially matches the entities in more than one slot condition.

    For example, if one slot captures a product ID (`@id`) with a syntax like `GR1234` and another slot captures a number (`@number`), such as `1234`, then user input that contains an ID, such as `BR3344` might get claimed by the `@number` slot as a number reference and fill the `$number` context variable with `3344`.

    **Solution**: Place the slot with the entity condition that captures the longer pattern (@id) higher in the list of slots than the condition that captures the shorter pattern (@number).

- **Problem**: A term is recognized as more than one system entity type.

    For example, if the user enters *May 2*, then your assistant recognizes both the `@sys-date` (2017-05-02) and `@sys-number` (2) entities.

    **Solution**: In logic that is unique to the slots feature, when two system entities are recognized in a single user input, the one with the larger span is used. Therefore, even though your assistant recognizes both system entities in the text, only the system entity with the longer span (`@sys-date` with `2017-05-02`) is registered and applied to the slot.

    This workaround is not necessary if you are using the revised system entities. With the updated entities, a date reference is considered to be a `@sys-date` mention only, and is not also treated as a `@sys-number` mention. For more details, see [System entities](/docs/assistant?topic=assistant-system-entities).
    {: note}

### Adding conditions to Found and Not found responses
{: #dialog-slots-handler-next-steps}

For each slot, you can use conditional responses with associated actions to help you extract the information you need from the user. To do so, follow these steps:

1.  Click the **Customize slot** ![Customize slot](images/edit-slot.png) icon for the slot to which you want to add conditional Found and Not found responses.
1.  From the **More** ![More icon](images/kebab.png) menu, select **Enable conditional responses**.
1.  Enter the condition and the response to display if the condition is met.

    **Found example**: A slot is expecting the time for a dinner reservation. You might use @sys-time in the *Check for* field to capture it. To prevent an invalid time from being saved, you can add a conditional response that checks whether the time provided is before the restaurant's last seating time, for example, `@sys-time.after('21:00:00')`. The corresponding response might be something like, *Our last seating is at 9PM.*
    
    **Not found example**: The slot is expecting a @sys-number entity for the number of pizzas in a takeout order. The Not found condition of `true` might be used to display a message prompting the user, just in case a number isn't provided in the conversation (for example, `We need to know how many pizzas you want.`).

1.  If you want to customize what happens next if the condition is met, then click the **Edit response** ![Edit response](images/edit-slot.png) icon.

    For Found responses (that are displayed when the user provides a value that matches the value type specified in the Check for field), you can choose one of these actions to perform next:

      - **Move on (Default)**: Instructs your assistant to move on to the next empty slot after displaying the response. In the associated response, assure the user that their input was understood. For example, *Ok. You want to schedule it for $date.*
      - **Clear slot and prompt again**: If you are using an entity in the *Check for* field that could pick up the wrong value, add conditions that catch any likely misinterpretations, and use this action to clear the current slot value and prompt for the correct value.
      - **Skip to response**: If, when the condition you define is met, you no longer need to fill any of the remaining slots in this node, choose this action to skip the remaining slots and go directly to the node-level response next. For example, you could add a condition that checks whether the user's age is under 16. If so, you might skip the remaining slots which ask questions about the user's driving record.

    For Not found responses (that are displayed when the user does not provide a valid value), you can choose one of these actions to perform:

      - **Wait for user input (Default)**: Pauses the conversation and your assistant waits for the user to respond. In the simplest case, the text you specify here can more explicitly state the type of information you need the user to provide. If you use this action with a conditional response, be sure to word the conditional response such that you clearly state what was wrong with the user's answer and what you expect them to provide instead.
      - **Prompt again**: After displaying the Not found response, your assistant repeats the slot prompt again and waits for the user to respond. If you use this action with a conditional response, the response can merely explain what was wrong about the answer the user provided. It does not need to reiterate the type of information you want the user to provide because the slot prompt typically explains that.

        If you choose this option, consider adding at least one variation of the Not found response so that the user does not see the exact same text more than once. Take the opportunity to use different wording to explain to the user what information you need them to provide and in what format.
        {: tip}

      - **Skip this slot**: Instructs your assistant to stop trying to fill the current slot, and instead, move on to the prompt for the next empty slot. This option is useful in a slot where you want to both make the slot optional and to display a prompt that asks the user for information. For example, you might have a @seating entity that captures restaurant seating preferences, such as *outside*, *near the fireplace*, *private*, and so on. You can add a slot that prompts the user with, *Do you have any seating preferences?* and checks for `@seating.values`. If a valid response is provided, it saves the preference information to `$seating_preferences`. However, by choosing this action as the Not found response next step, you instruct your assistant to stop trying to fill this slot if the user does not provide a valid value for it.
      - **Skip to response**: If, when the condition you define is met, you no longer need to fill any of the remaining slots in this node, choose this action to skip the remaining slots and go directly to the node-level response next. For example, if after capturing the one-way flight information, the slot prompt is, *Are you buying round trip tickets?* the Not found condition can check for #No. If #No is found, use this option to skip the remaining slots that capture information about the return flight, and go straight to the node-level response instead.

    Click **Back** to return to the edit view of the slot.
1.  To add another conditional response, click **Add a response**, and then enter the condition and the response to display if the condition is met.

    Be sure to add at least one response that will be displayed no matter what. You can leave the condition field blank for this catch all response. Your assistant automatically populates the empty condition field with the `true` special condition.

1.  Click **Save** to save your changes, close the edit view of the slot, and return to the edit view of the node.

### Moving on after multiple failed attempts
{: #dialog-slots-stop-trying-after-3}

You can provide users with a way to exit a slot if they cannot answer it correctly after several attempts by using Not found conditional responses. In the catchall response, open the JSON editor to add a counter context variable that will keep track of the number of times the Not found response is returned. In an earlier node, be sure to set the initial counter context variable value to 0.

In this example, your assistant asks for the pizza size. It lets the user answer the question incorrectly 3 times before applying a size (medium) to the variable for the user. (You can include a confirmation slot where users can always correct the size when they are asked to confirm the order information.)

Check for: @size
Save it as: $size
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
{: #dialog-slots-stifle-found-responses}

If you specify Found responses for multiple slots, then if a user provides values for multiple slots at once, the Found response for at least one of the slots will be displayed. You probably want either the Found response for all of them or none of them to be returned.

To prevent Found responses from being displayed, you can do one of the following things to each Found response:

- Add a condition to the response that prevents it from being displayed if particular slots are filled. For example, you can add a condition, like `!($size && $time)`, that prevents the response from being displayed if the $size and $time context variables are both provided.
- Add the `!all_slots_filled` condition to the response. This setting prevents the response from being displayed if all of the slots are filled. Do not use this approach if you are including a confirmation slot. The confirmation slot is also a slot, and you typically want to prevent the Found responses from being displayed before the confirmation slot itself is filled.

### Handling requests to exit a process
{: #dialog-slots-node-level-handler}

Add at least one slot handler that can recognize it when a user wants to exit the node.

For example, in a node that collects information to schedule a pet grooming appointment, you can add a handler that conditions on the #cancel intent, which recognizes utterances such as, <q>Forget it. I changed my mind.</q>

1.  In the JSON editor for the handler, fill all of the slot context variables with dummy values to prevent the node from continuing to ask for any that are missing. And in the handler response, add a message such as, `Ok, we'll stop there. No appointment will be scheduled.`
1.  Choose what action you want your assistant to take next from the following options:

    - **Prompt again (Default)**: Displays the prompt for the slot that the user was working with just before asking the off-topic question.
    - **Skip current slot**: Displays the prompt associated with the slot that comes after the slot that the user was working with just before asking the off-topic question. And your assistant makes no further attempts to fill the skipped slot.
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

- Slot handlers in the order they are listed.
- First slot condition.
- Current slot level Found conditions.
- If digressions away are allowed, root level node conditions are checked for a match (except the final `anything else` node in the dialog tree root or a root folder).
- Current slot level Not found conditions.
- Final `anything else` node condition.

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
{: #dialog-slots-examples}

To access JSON files that implement different common slot usage scenarios, go to the community [conversation repo](https://github.com/watson-developer-cloud/community/tree/master/watson-assistant){: external} in GitHub.

To explore an example, download one of the example JSON files, and then import it as a new dialog skill. From the Dialog tab, you can review the dialog nodes to see how slots were implemented to address different use cases.
