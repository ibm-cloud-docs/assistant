---

copyright:
  years: 2015, 2021
lastupdated: "2020-10-28"

keywords: autocorrection, spelling correction, spell check

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

{{site.data.content.newlink}}

# Correcting user input
{: #dialog-runtime-spell-check}

*Autocorrection* fixes misspellings that users make in the utterances that they submit as user input. When autocorrection is enabled, the misspelled words are automatically corrected. And it is the corrected words that are used to evaluate the input. When given more precise input, your assistant can more often recognize entity mentions and understand the user's intent.

Autocorrection is enabled automatically for all new English-language dialog skills. It is available as a beta feature in French-language dialog skills. You must turn it on to use it with French-language skills. For more information about language support, see [Supported languages](/docs/assistant?topic=assistant-language-support).
{: note}

Autocorrection corrects user input in the following way:

- Orignal input: `letme applt for a memberdhip`
- Corrected input: `let me apply for a membership`

When your assistant evaluates whether to correct the spelling of a word, it does not rely on a simple dictionary lookup process. Instead, it uses a combination of Natural Language Processing and probabalistic models to assess whether a term is, in fact, misspelled and should be corrected.

## Turning autocorrection on or off
{: #dialog-runtime-spell-check-disable}

Autocorrection helps your assistant understand user input. It is enabled automatically for some languages and available, but disabled in others.

You can disable autocorrection in your dialog skill, but not in your actions skill.
{: note}

If you decide to disable it, you must turn it off entirely. You cannot disable autocorrection for a single word or phrase. 

If you find that a domain-specific term is being corrected that shouldn't be, you can prevent the correction from happening by adding the term or phrase to your training data. For more details, see [Autocorrection rules](#dialog-runtime-spell-check-rules).

To turn autocorrection on or off, complete the following steps:

1.  Click the **Skills** icon ![Skills menu icon](images/nav-skills-icon.png), and then open your skill.
1.  From the Skills menu, click **Options**, and then click **Autocorrection**.
1.  Click the switch to enable or disable the feature.

## Testing autocorrection
{: #dialog-runtime-spell-check-test}

1.  From the "Try it out" pane, submit an utterance that includes some misspelled words.

    If words in your input are misspelled, they are corrected automatically, and an ![auto-correct](images/auto-correct.png) icon is displayed. The corrected utterance is underlined.
1.  Hover over the underlined utterance to see the original wording.

If there are misspelled terms that you expected your assistant to correct, but it did not, then review the rules that your assistant uses to decide whether to correct a word to see if the word falls into the category of words that your assistant intentionally does not change.

## Autocorrection rules
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

### How is spelling autocorrection related to fuzzy matching?
{: #dialog-runtime-spell-check-vs-fuzzy-matching}

Fuzzy matching helps your assistant recognize dictionary-based entity mentions in user input. It uses a dictionary lookup approach to match a word from the user input to an existing entity value or synonym in the skill's training data. For example, if the user enters `boook`, and your training data contains a `@reading_material` entity with a `book` value, then fuzzy matching recognizes that the two terms (`boook` and `book`) mean the same thing.

When you enable both autocorrection and fuzzy matching, the fuzzy matching function runs before autocorrection is triggered. If it finds a term that it can match to an existing dictionary entity value or synonym, it adds the term to the list of words that *belong* to the skill, and does not correct it.

For example, if a user enters a sentence like `I wnt to buy a boook`, fuzzy matching recognizes that the term `boook` means the same thing as your entity value `book`, and adds it to the protected words list. Your assistant corrects the input to be, `I want to buy a boook`. Notice that it corrects `wnt` but does *not* correct the spelling of `boook`. If you see this type of result when you are testing your dialog, you might think your assistant is misbehaving. However, your assistant is not. Thanks to fuzzy matching, it correctly identifies `boook` as a `@reading_material` entity mention. And thanks to autocorrection revising the term to `want`, your assistant is able to map the input to your `#buy_something` intent. Each feature does its part to help your assistant understand the meaning of the user input.

### How autocorrection works
{: #dialog-runtime-spell-check-how-it-works}

Normally, user input is saved as-is in the `text` field of the `input` object of the message. If, and only if the user input is corrected in some way, a new field is created in the `input` object, called `original_text`. This field stores the user's original input that includes any misspelled words in it. And the corrected text is added to the `input.text` field.

If you want to ask users to confirm the assistant's understanding of their meaning, you can do so in a way that takes into account that their input might have been corrected. Set the condition for the dialog node or conditional response that is asking for confirmation to `original_text`. This means that if the user's input was automatically corrected, show the corresponding response. And the response can contain the expression: `You said: <? input.original_text ?>. Did you mean: <? input.text ?>?`

Remember, the `input.text` field stores either the never-corrected original text from the user or the user’s text after it is corrected. The `input.original_text` field is only created if the input is corrected. And the user’s incorrect input is stored in it.”
