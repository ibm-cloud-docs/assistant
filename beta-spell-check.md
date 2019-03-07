---

copyright:
  years: 2015, 2019
lastupdated: "2019-03-07"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
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

# Beta: Correcting user input
{: #dialog-runtime-spell-check}

This feature is available for use by participants in the beta program only. To find out how to request access, see [Participate in the beta program](/docs/services/assistant?topic=assistant-feedback#feedback-beta).

![Beta](images/beta.png) IBM releases services, features, and language support for your evaluation that are classified as beta. These features might be unstable, might change frequently, and might be discontinued with short notice. Beta features also might not provide the same level of performance or compatibility that generally available features provide and are not intended for use in a production environment.

Enable the *spell check* feature to fix misspellings that users make in the utterances that they submit as user input. When spell check is enabled, the misspelled words are automatically corrected. And it is the corrected words that are used to evaluate the input. When given more precise input, the service can more often recognize entity mentions and understand the user's intent.

Currently, this setting can be enabled for English-language dialog skills only.
{: note}

With spell check enabled, user input is corrected in the following way:

- Orignal input: `letme applt for a memberdhip`
- Corrected input: `let me apply for a membership`

When the service evaluates whether to correct the spelling of a word, it does not rely on a simple dictionary lookup process. Instead, it uses a combination of Natural Language Processing and probabalistic models to assess whether a term is, in fact, misspelled and should be corrected.

## Enabling spell check
{: #beta-spell-check-enable}

To enable the spell check feature, complete the following steps:

1.  From the Skills page, open your skill.
1.  Click the **Options** tab.
1.  From the *Spell Check* page, turn on **Spell check auto-correction**.

### Testing spelling correction
{: #beta-spell-check-test}

1.  From the "Try it out" pane, submit an utterance that includes some misspelled words.

    If words in your input are misspelled, they are corrected automatically, and an ![auto-correct](images/auto-correct.png) icon is displayed. The corrected utterance is underlined.
1.  Hover over the underlined utterance to see the original wording.

If there are misspelled terms that you expected the service to correct, but it did not, then review the rules that the service uses to decide whether to correct a word to see if the word falls into the category of words that the service intentionally does not change.

To avoid overcorrection, the service does not correct the spelling of the following types of input:

- Capitalized words
- Emojis
- Location entities, such as states and street addresses
- Numbers and units of measurement or time
- Proper nouns, such as common first names or company names
- Text within quotation marks
- Words containing special characters, such as hyphens (-), asterisks (*), and ampersands (&)
- Words that *belong* in this skill, meaning words that have implied significance because they occur in entity values, entity synonyms, or intent user examples.

If the word that is not corrected is not obviously one of these types of input, then it might be worth checking whether the entity has fuzzy matching enabled for it.

#### How is spelling correction related to fuzzy matching?
{: #beta-spell-check-vs-fuzzy-matching}

Fuzzy matching helps the service recognize dictionary-based entity mentions in user input. It uses a dictionary lookup approach to match a word from the user input to an existing entity value or synonym in the skill's training data. For example, if the user enters `books`, and your training data contains the entity synonym `book`, fuzzy matching recognizes that these two terms mean the same thing.

When you enable both spell check and fuzzy matching, the fuzzy matching function runs before spell check is triggered. If it finds a term that it can match to an existing dictionary entity value or synonym, it adds the term to the list of words that *belong* to the skill, and therefore are not to be corrected. Likewise, if a user enters a sentence like `I want to buy a boook`, fuzzy matching recognizes that the term `boook` means the same thing as your entity synonym `book`, and adds it to the protected words list. As a result, the service does *not* correct the spelling of `boook`.

#### How it works
{: #beta-spell-check-how-it-works}

Normally, user input is saved as-is in the `text` field of the `input` object of the message. If, and only if the user input is corrected in some way, a new field is created in the `input` object, called `original_text`. This field stores the user's original input that includes any misspelled words in it. And the corrected text is added to the `input.text` field.
