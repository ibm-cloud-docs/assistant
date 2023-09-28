---

copyright:
  years: 2015, 2023
lastupdated: "2023-03-13"

keywords: autocorrection, spelling correction, spell check

subcollection: assistant

---

{{site.data.keyword.attribute-definition-list}}

Documentation for the **classic {{site.data.keyword.assistant_classic_short}}** experience has moved. For the most up-to-date version, see [Autocorrecting user input](/docs/watson-assistant?topic=watson-assistant-autocorrection){: external}.
{: attention}

# Autocorrecting user input
{: #autocorrection}

*Autocorrection* fixes misspellings that users make in their requests. The corrected words are used to match to an action or an intent.

Autocorrection corrects user input in the following way:

- Original input: `letme applt for a memberdhip`
- Corrected input: `let me apply for a membership`

When your assistant evaluates whether to correct the spelling of a word, it does not rely on a simple dictionary lookup process. Instead, it uses a combination of natural language processing and probabalistic models to assess whether a term is, in fact, misspelled and should be corrected.

Autocorrection is enabled automatically for all English-language assistants. It is also available in French-language assistants, but is disabled by default. Autocorrection isn't available for any other languages.


## Disabling autocorrection
{: #autocorrection-disable}

If necessary, you can disable autocorrection for your assistant. 

If you find that a domain-specific term is being corrected that shouldn't be, you can prevent the correction from happening by adding the term or phrase to your training data. For more details, see [Autocorrection rules](#autocorrection-rules).
{: note}

If you are using actions in your assistant, follow these steps to disable autocorrection:

1. On the **Actions** page, click **Global settings** !![Gear icon](../../icons/settings.svg).

1. Click the **Autocorrection** tab.

1. Set the switch to **Off**, then click **Save**.

If you are using dialog in your assistant, follow these steps to disable autocorrection:

1.  In the **Options** section, click **Autocorrection**.

1. Set the switch to **Off**.

## Testing autocorrection in dialog
{: #autocorrection-test}

If you are using dialog, you can test autocorrection using **Try it out**.

1.  In **Try it out**, enter a request that includes some misspelled words.

    If words in your input are misspelled, they are corrected automatically, and an ![auto-correct](images/auto-correct.png) icon is displayed. The corrected utterance is underlined.

1.  Hover over the underlined utterance to see the original wording.

If there are misspelled terms that you expected your assistant to correct, but it did not, then review the rules that your assistant uses to decide whether to correct a word to see if the word falls into the category of words that your assistant intentionally does not change.

## Autocorrection rules
{: #autocorrection-rules}

To avoid overcorrection, your assistant does not correct the spelling of the following types of input:

- Capitalized words
- Emojis
- Locations, such as states and street addresses
- Numbers and units of measurement or time
- Proper nouns, such as common first names or company names
- Text within quotation marks
- Words containing special characters, such as hyphens (-), asterisks (*), ampersands (&), or at signs (@), including those used in email addresses or URLs.
- Words that *belong*, meaning words that have implied significance because they occur in your action steps or dialog entity values, entity synonyms, or intent user examples.

### How is spelling autocorrection related to fuzzy matching?
{: #autocorrection-vs-fuzzy-matching}

In dialog, *fuzzy matching* helps your assistant recognize dictionary-based entity mentions in user input. It uses a dictionary lookup approach to match a word from the user input to an existing entity value or synonym in the skill's training data. For example, if the user enters `boook`, and your training data contains a `@reading_material` entity with a `book` value, then fuzzy matching recognizes that the two terms (`boook` and `book`) mean the same thing.

In dialog, when you enable both autocorrection and fuzzy matching, the fuzzy matching function runs before autocorrection is triggered. If it finds a term that it can match to an existing dictionary entity value or synonym, it adds the term to the list of words that *belong* to the skill, and does not correct it.

For example, if a user enters a sentence like `I wnt to buy a boook`, fuzzy matching recognizes that the term `boook` means the same thing as your entity value `book`, and adds it to the protected words list. Your assistant corrects the input to be, `I want to buy a boook`. Notice that it corrects `wnt` but does *not* correct the spelling of `boook`. If you see this type of result when you are testing your dialog, you might think your assistant is misbehaving. However, your assistant is not. Thanks to fuzzy matching, it correctly identifies `boook` as a `@reading_material` entity mention. And thanks to autocorrection revising the term to `want`, your assistant is able to map the input to your `#buy_something` intent. Each feature does its part to help your assistant understand the meaning of the user input.

