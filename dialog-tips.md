---

copyright:
  years: 2015, 2019
lastupdated: "2019-09-05"

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

# Dialog building tips
{: #dialog-tips}

Learn how to approach building a dialog and get some tips on completing more complex steps.
{: shortdesc}

Review these tips from experienced dialog designers.

## Planning the overall dialog
{: #dialog-tips-plan}

- Plan out the design of the dialog that you want to build before you add a single dialog node. Sketch it out on paper, if necessary.
- Whenever possible, base your design decisions on data from real-world evidence and behaviors. Do not add nodes to handle a situation that someone *thinks* might occur.
- Avoid copying business processes as-is. They are rarely conversational.
- If people already use a process, examine how they approach it. People typically optimize the process from a conversational perspective.
- Decide on the tone, personality, and positioning of your assistant. Consistently reflect these choices in the dialog you create.
- Never misrepresent the assistant as being a human. If users believe the assistant is a person, then find out it's not, they are likely to distrust it.
- Not everything has to be a conversation. Sometimes a web form works better.

## Adding nodes
{: #dialog-tips-nodes}

- Add a node name that describes the purpose of the node.

  You know what the node does right now, but months from now you might not. Your future self and any team members will thank you for adding a descriptive node name. And the node name is displayed in the log, which can help you debug a conversation later.
- To gather the information that is required to perform a task, try using a node with slots instead of a bunch of separate nodes to elicit information from users. See [Gathering information with slots](/docs/services/assistant?topic=assistant-dialog-slots).
- For a complex process flow, tell users about any information they will need to provide at the start of the process.
- Understand how your assistant travels through the dialog tree and the impact that folders, branches, jump-tos, and digressions have on the route. See [Dialog flow](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-flow).
- Do not add jump-tos everywhere. They increase the complexity of the dialog flow, and make it harder to debug the dialog later.
- To jump to a node in the same branch as the current node, use *Skip user input* instead of a *Jump-to*.

  This choice prevents you from having to edit the current node's settings when you remove or reorder the child nodes being jumped to. See [Defining what to do next](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-jump-to).
- Before you enable digressions away from a node, test the most common user scenarios. And be sure that likely digressed-to nodes are configured to return. See [Digressions](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions).

## Adding responses
{: #dialog-tips-responses}

- Keep answers short and useful.
- Reflect the user's intent in the response.

  Doing so assures users that the bot is understanding them, or if it is not, gives users a chance to correct a misunderstanding right away.
- Only include links to external sites in responses if the answer depends on data that changes frequently.
- Avoid overusing buttons. Encouraging users to pick predefined options from a set of buttons is less like a real conversation, and decreases your ability to learn what users really want to do. When you let real users ask for things in their own words, you can use the input to train the system and derive better intents.
- Avoid using a bunch of nodes when one node will do. For example, add multiple conditional responses to a single node to return different responses depending on details provided by the user. See [Conditional responses](/docs/services/assistant?topic=assistant-dialog-overview#dialog-overview-multiple).
- Word your responses carefully. You can change how someone reacts to your system based simply on how you phrase a response. Changing one line of text can prevent you from having to write multiple lines of code to implement a complex programmatic solution.
- Back up your skill frequently. See [Downloading a skill](/docs/services/assistant?topic=assistant-skill-add#skill-add-download).

## Tips for capturing information from user input
{: #dialog-tips-user-input}

It can be difficult to know the syntax to use in your dialog node to accurately capture the information you want to find in the user input. Here are some approaches you can use to address common goals.

- **Returning the user's input**: You can capture the exact text uttered by the user and return it in your response. Use the following SpEL expression in a response to repeat the text that the user specified back in the response:

  `You said: <? input.text ?>.`

  If autocorrection is on, and you want to return the user's original input before it was corrected, you can use `<? input.original_text ?>`. But, be sure to use a response condition that checks whether the `original_text` field exists first.
  {: note}

- **Determining the number of words in user input**: You can perform any of the supported String methods on the input.text object. For example, you can find out how many words there are in a user utterance by using the following SpEL expression:

  `input.text.split(' ').size()`

  See [Expression language methods for String](/docs/services/assistant?topic=assistant-dialog-methods#dialog-methods-strings) to learn about more methods you can use.

- **Dealing with multiple intents**: A user enters input that expresses a wish to complete two separate tasks. `I want to open a savings account and apply for a credit card.` How does the dialog recognize and address both of them? See the [Compound questions](https://sodoherty.ai/2017/02/06/compound-questions/){: external} entry from Simon O'Doherty's blog for strategies you can try. (Simon is a developer on the {{site.data.keyword.conversationshort}} team.)

- **Dealing with ambiguous intents**: A user enters input that expresses a wish that is ambiguous enough that your assistant finds two or more nodes with intents that could potentially address it. How does the dialog know which dialog branch to follow? If you enable disambiguation, it can show users their options and ask the user to pick the right one. See [Disambiguation](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation) for more details.

- **Handling multiple entities in input**: If you want to evaluate only the value of the first detected instance of an entity type, you can use the syntax  `@entity == 'specific-value'` instead of the `@entity:(specific-value)` format.

  For example, when you use `@appliance == 'air conditioner'`, you are evaluating only the value of the first detected `@appliance` entity. But, using `@appliance:(air conditioner)` gets expanded to `entity['appliance'].contains('air conditioner')`, which matches whenever there is at least one `@appliance` entity of value 'air conditioner' detected in the user input.

- **Hide data from the log**: You can prevent information from being stored in Watson logs by storing it in a context variable and nesting the context variable within the `$private` section of the message context. For example: `$private.my_info`. Storing data in the private object hides it from the logs only. The information is still stored in the underlying JSON object. Do not allow this information to be exposed to the client application.

## Condition usage tips
{: #dialog-tips-condition-usage}

- **Checking for values with special characters**: If you want to check whether an entity or context variable contains a value, and the value includes a special character, such as an apostrophe ('), then you must surround the value that you want to check with parentheses. For example, to check if an entity or context variable contains the name `O'Reilly`, you must surround the name with parentheses.

  `@person:(O'Reilly)` and `$person:(O'Reilly)`

  Your assistant converts these shorthand references into these full SpEL expressions:

  `entities['person']?.contains('O''Reilly')` and `context['person'] == 'O''Reilly'`

  SpEL uses a second apostrophe to escape the single apostrophe in the name.
  {: note}

- **Checking for multiple values**: If you want to check for more than one value, you can create a condition that uses OR operators (`||`) to list multiple values in the condition. For example, to define a condition that is true if the context variable `$state` contains the abbreviations for Massachusetts, Maine, or New Hampshire, you can use this expression:

  `$state:MA || $state:ME || $state:NH`

- **Checking for number values**: When comparing numbers, first make sure the entity or variable you are checking has a value. If the entity or variable does not have a number value, it is treated as having a null value (0) in a numeric comparison.

  For example, you want to check whether a dollar value that a user specified in user input is less than 100. If you use the condition `@price < 100`, and the `@price` entity is null, then the condition is evaluated as `true` because 0 is less than 100, even though the price was never set. To prevent this type of inaccurate result, use a condition such as `@price AND @price < 100`. If `@price` has no value, then this condition correctly returns false.

- **Checking for intents with a specific intent name pattern**: You can use a condition that looks for intents that match a pattern. For example, to find any detected intents with intent names that start with 'User_', you can use a syntax like this in the condition:

  `intents[0].intent.startsWith("User_")`

  However, when you do so, all of the detected intents are considered, even those with a confidence lower than 0.2. Also check that intents which are considered irrelevant by Watson based on their confidence score are not returned. To do so, change the condition as follows:

  `!irrelevant && intents[0].intent.startsWith("User_")`

- **How fuzzy matching impacts entity recognition**: If you use an entity as the condition and fuzzy matching is enabled, then `@entity_name` evaluates to true only if the confidence of the match is greater than 30%. That is, only if `@entity_name.confidence > .3`.

## Storing and recognizing entity pattern groups in input
{: #dialog-tips-get-pattern-groups}

To store the value of a pattern entity in a context variable, append .literal to the entity name. Using this syntax ensures that the exact span of text from user input that matched the specified pattern is stored in the variable.

| Variable   | Value               |
|------------|---------------------|
| email      | <? @email.literal ?> |

To store the text from a single group in a pattern entity with groups defined, specify the array number of the group that you want to store. For example, assume that the entity pattern is defined as follows for the @phone_number entity. (Remember, the parentheses denote pattern groups):

`\b((958)|(555))-(\d{3})-(\d{4})\b`

To store only the area code from the phone number that is specified in user input, you can use the following syntax:

| Variable       | Value                         |
|----------------|-------------------------------|
| area_code      | <? @phone_number.groups[1] ?> |

The groups are delimited by the regular expression that is used to define the group pattern. For example, if the user input that matches the pattern defined in the entity `@phone_number` is: `958-234-3456`, then the following groups are created:

| Group number | Regex engine value  | Dialog value   | Explanation |
|--------------|---------------------|----------------|-------------|
| groups[0]    | `958-234-3456`      | `958-234-3456` | The first group is always the full matching string. |
| groups[1]    | `((958)`l`(555))`   | `958`          | String that matches the regex for the first defined group, which is `((958)`l`(555))`. |
| groups[2]    | `(958)`             | `958`          | Match against the group that is included as the first operand in the OR expression `((958)`l`(555))` |
| groups[3]    | `(555)`             | `null`         | No match against the group that is included as the second operand in the OR expression `((958)`l`(555))` |
| groups[4]    | `(\d{3})`           | `234`          | String that matches the regular expression that is defined for the group. |
| groups[5]    | `(\d{4})`           | `3456`         | String that matches the regular expression that is defined for the group. |
{: caption="Group details" caption-side="top"}

To help you decipher which group number to use to capture the section of input you are interested in, you can extract information about all the groups at once. Use the following syntax to create a context variable that returns an array of all the grouped pattern entity matches:

| Variable                 | Value                      |
|--------------------------|----------------------------|
| array_of_matched_groups  | <? @phone_number.groups ?> |

Use the "Try it out" pane to enter some test phone number values. For the input `958-123-2345`, this expression sets `$array_of_matched_groups` to `["958-123-2345","958","958",null,"123","2345"]`.

You can then count each value in the array starting with 0 to get the group number for it.

| Array element value | Array element number |
|---------------------|----------------------|
| "958-123-2345"      | 0 |
| "958"               | 1 |
| "958"               | 2 |
| null                | 3 |
| "123"               | 4 |
| "2345"              | 5 |
{: caption="Array elements" caption-side="top"}

From the result, you can determine that, to capture the last four digits of the phone number, you need group #5, for example.

To return the JSONArray structure that is created to represent the grouped pattern entity, use the following syntax:

| Variable             | Value                           |
|----------------------|---------------------------------|
| json_matched_groups  | <? @phone_number.groups_json ?> |

This expression sets `$json_matched_groups` to the following JSON array:

```json
[
  {"group": "group_0","location": [0, 12]},
  {"group": "group_1","location": [0, 3]},
  {"group": "group_2","location": [0, 3]},
  {"group": "group_3"},
  {"group": "group_4","location": [4, 7]},
  {"group": "group_5","location": [8, 12]}
]
```
{: codeblock}

`location` is a property of an entity that uses a zero-based character offset to indicate where the detected entity value begins and ends in the input text.
{: note}

If you expect two phone numbers to be supplied in the input, then you can check for two phone numbers. If present, use the following syntax to capture the area code of the second number, for example.

| Variable         | Value                                       |
|------------------|---------------------------------------------|
| second_areacode  | <? entities['phone_number'][1].groups[1] ?> |

If the input is `I want to change my phone number from 958-234-3456 to 555-456-5678`, then `$second_areacode` equals `555`.

## Viewing API call details
{: #dialog-tips-inspect-api}

As you test your dialog with the "Try it out" pane, you might want to know what the underlying API calls look like that are being returned from the service. You can use the developer tools provided by your web browser to inspect them.

From Chrome, for example, open the Developer tools. Click the Network tool. The Name section lists multiple API calls. Click the message call associated with your test utterance, and then click the Response column to see the API response body. It lists the intents and entities that were recognized in the user input with their confidence scores, and the values of context variables at the time of the call. To view the response body in structured format, click the Preview column.

![Shows how to view the API call details by using Chrome web browser developer tools.](images/api-browser-dev.png)
