---

copyright:
  years: 2015, 2021
lastupdated: "2020-12-15"

subcollection: assistant

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

# Supported languages
{: #language-support}

The {{site.data.keyword.conversationshort}} service supports the languages listed. Individual features are supported to a greater or lesser extent for each language.
{: shortdesc}

In the following tables, the level of language and feature support is indicated by these codes:

- GA: The feature is generally available and supported for this language. Note that features might continue to be updated even after they are generally available.
- Beta: The feature is supported only as a Beta release, and is still undergoing testing before it is made generally available in this language.
- NA: Indicates that a feature is not available in this language.

Your assistant might be able to identify intents in unsupported languages depending on the complexity of your data, but if the language is not listed here it is not officially supported and results cannot be guaranteed.
{: note}

## Feature support details
{: #language-support-tables}

The following tables illustrate the level of language support available for product features.

### Skill support details
{: #language-support-skills}

| Language | [**Actions skill**](/docs/assistant?topic=assistant-skill-actions-add) |[**Dialog skill**](/docs/assistant?topic=assistant-skill-dialog-add) | [**Search skill**](/docs/assistant?topic=assistant-skill-search-add) |
|:---:|:---:|:---:|
| **English (en)**                   | Beta | GA | GA |
| **Arabic (ar)**                    | Beta | GA | GA |
| **Chinese (Simplified) (zh-cn)**   | Beta | GA | GA |
| **Chinese (Traditional) (zh-tw)**  | Beta | GA | GA |
| **Czech (cs)**                     | Beta | GA | GA |
| **Dutch (nl)**                     | Beta | GA | GA |
| **French (fr)**                    | Beta | GA | GA |
| **German (de)**                    | Beta | GA | GA |
| **Italian (it)**                   | Beta | GA | GA |
| **Japanese (ja)**                  | Beta | GA | GA |
| **Korean (ko)**                    | Beta | GA | GA |
| **Portuguese (Brazilian) (pt-br)** | Beta | GA | GA |
| **Spanish (es)**                   | Beta | GA | GA |
{: caption="Table 1. Skill support details" caption-side="top"}

### Intent feature support details
{: #language-support-intents}

| Language | **[Content Catalog](/docs/assistant?topic=assistant-catalog)** | **[Irrelevance detection](/docs/assistant?topic=assistant-irrelevance-detection)** | **[Enhanced intent detection](/docs/assistant?topic=assistant-intent-detection)** |
|:---:|:---:|:---:|:---:|
| **English (en)**                   | GA | GA | Beta |
| **Arabic (ar)**                    | GA (except Covid-19) | NA | NA |
| **Chinese (Simplified) (zh-cn)**   | NA | NA | NA |
| **Chinese (Traditional) (zh-tw)**  | NA | NA | NA |
| **Czech (cs)**                     | NA | NA | NA |
| **Dutch (nl)**                     | NA | NA | NA |
| **French (fr)**                    | GA | NA | NA |
| **German (de)**                    | GA (except Covid-19) | NA | NA |
| **Italian (it)**                   | GA (except Covid-19) | NA | NA |
| **Japanese (ja)**                  | GA (except Covid-19) | NA | NA |
| **Korean (ko)**                    | NA | NA | NA |
| **Portuguese (Brazilian) (pt-br)** | GA | NA | NA |
| **Spanish (es)**                   | GA | NA | NA |
{: caption="Table 2. Intent feature support details" caption-side="top"}

### Recommendations feature support details continued
{: #language-support-recommendations}

| Language | **[User example recommendations](/docs/assistant?topic=assistant-intent-recommendations#intent-recommendations-get-example-recommendations-task)** | **[Intent recommendations](/docs/assistant?topic=assistant-intent-recommendations#intent-recommendations-get-intent-recommendations-task)** |
|:---:|:---:|:---:|
| **English (en)**                   | GA | GA |
| **Arabic (ar)**                    | NA | NA |
| **Chinese (Simplified) (zh-cn)**   | NA | NA |
| **Chinese (Traditional) (zh-tw)**  | NA | NA |
| **Czech (cs)**                     | NA | NA |
| **Dutch (nl)**                     | NA | NA |
| **French (fr)**                    | NA | NA |
| **German (de)**                    | NA | NA |
| **Italian (it)**                   | NA | NA |
| **Japanese (ja)**                  | GA | NA |
| **Korean (ko)**                    | NA | NA |
| **Portuguese (Brazilian) (pt-br)** | NA | NA |
| **Spanish (es)**                   | NA | NA |
{: caption="Table 3. Recommendations feature support details continued" caption-side="top"}

### User input processing support details
{: #language-support-input}

| Language | **[Entity fuzzy matching (*Misspelling only*)](/docs/assistant?topic=assistant-entities#entities-fuzzy-matching)** | **[Entity fuzzy matching (*Stemming*, *Partial match*)](/docs/assistant?topic=assistant-entities#entities-fuzzy-matching)** | **[Autocorrection](/docs/assistant?topic=assistant-dialog-runtime-spell-check)** |
|:---:|:---:|:---:|:---:|
| **English (en)**                   | GA | GA | GA |
| **Arabic (ar)**                    | GA | NA | NA |
| **Chinese (Simplified) (zh-cn)**   | NA | NA | NA |
| **Chinese (Traditional) (zh-tw)**  | NA | NA | NA |
| **Czech (cs)**                     | GA | GA | NA |
| **Dutch (nl)**                     | GA | NA | NA |
| **French (fr)**                    | GA | GA | Beta |
| **German (de)**                    | GA | GA | NA |
| **Italian (it)**                   | GA | NA | NA |
| **Japanese (ja)**                  | GA | NA | NA |
| **Korean (ko)**                    | GA | NA | NA |
| **Portuguese (Brazilian) (pt-br)** | GA | NA | NA |
| **Spanish (es)**                   | GA | NA | NA |
{: caption="Table 4. User input processing support details" caption-side="top"}

### Entity feature support details
{: #language-support-entities}

| Language | **[Contextual entities](/docs/assistant?topic=assistant-entities#entities-create-annotation-based)** | **[Synonym recommendations](/docs/assistant?topic=assistant-entities#entities-synonyms)** |
|:---:|:---:|:---:|
| **English (en)**                   | GA | GA |
| **Arabic (ar)**                    | NA | NA |
| **Chinese (Simplified) (zh-cn)**   | NA | NA |
| **Chinese (Traditional) (zh-tw)**  | NA | NA |
| **Czech (cs)**                     | NA | NA |
| **Dutch (nl)**                     | NA | NA |
| **French (fr)**                    | Beta | GA |
| **German (de)**                    | NA | NA |
| **Italian (it)**                   | NA | NA |
| **Japanese (ja)**                  | NA | GA |
| **Korean (ko)**                    | NA | NA |
| **Portuguese (Brazilian) (pt-br)** | NA | NA |
| **Spanish (es)**                   | NA | GA |
{: caption="Table 5. Entity feature support details" caption-side="top"}

### System entity feature support details
{: #language-support-system-entities}

| Language | **[System entities](/docs/assistant?topic=assistant-system-entities)** | **[Legacy system entities](/docs/assistant?topic=assistant-legacy-system-entities)** |
|:---|:---:|:---:|
| **English (en)**                   | GA | Deprecated |
| **Arabic (ar)**                    | GA | Deprecated |
| **Chinese (Simplified) (zh-cn)**   | GA | Deprecated |
| **Chinese (Traditional) (zh-tw)**  | GA | Deprecated |
| **Czech (cs)**                     | GA | Deprecated |
| **Dutch (nl)**                     | GA | Deprecated |
| **French (fr)**                    | GA | Deprecated |
| **German (de)**                    | GA | Deprecated |
| **Italian (it)**                   | GA | Deprecated |
| **Japanese (ja)**                  | GA | Deprecated |
| **Korean (ko)**                    | GA | Deprecated |
| **Portuguese (Brazilian) (pt-br)** | GA | Deprecated |
| **Spanish (es)**                   | GA | Deprecated |
{: caption="Table 6. System entity feature support details" caption-side="top"}

The {{site.data.keyword.conversationshort}} service supports multiple languages as noted, but the tool interface itself (descriptions, labels, etc.) is in English. All supported languages can be input and trained through the English interface.
{: note}

GB18030 compliance: GB18030 is a Chinese standard that specifies an extended code page for use in the Chinese market. This code page standard is important for the software industry because the China National Information Technology Standardization Technical Committee has mandated that any software application that is released for the Chinese market after September 1, 2001, be enabled for GB18030. The {{site.data.keyword.conversationshort}} service supports this encoding, and is certified GB18030-compliant

## Changing a skill language
{: #language-support-change-language}

Once a skill has been created, its language cannot be modified. If it is necessary to change the supported language of a skill, the user should download the skill. Then, edit the resulting JSON file in a text editor, searching for a JSON property called `language`.

The `language` property should be set to the original language of the skill; for example, English would be `en`. Modify the value of this property, changing it to the desired language (`fr` for French, `de` for German, etc.). Save the changes to the JSON file, and import the modified file into your {{site.data.keyword.conversationshort}} service instance.

## Configuring bidirectional languages
{: #language-support-configure-bidirectional}

For bidirectional languages, for example Arabic, you can change your skill preferences. 

1.  From your skill tile, click the *Actions* drop-down menu, and then select **Language Preferences**.

    This option is only available for skills that are configured to use a bidirectional language, such as Arabic.
    {: note}

1.  Select from the following options for your skill:

- **GUI Direction**: Specifies the layout direction of elements, such as buttons or menus, in the graphical user interface. Choose `LTR` (left-to-right) or `RTL` (right-to-left). If not specified, the tool follows the web browser GUI direction setting.
- **Text Direction**: Specifies the direction of typed text. Choose `LTR` (left-to-right) or `RTL` (right-to-left), or select `Auto` which will automatically choose the text direction based on your system settings. The `None` option will display left-to-right text.
- **Numeric Shaping**: Specifies which form of numerals to use when presenting regular digits. Choose from `Nominal`, `Arabic-Indic`, or `Arabic-European`. The `None` option will display Western numerals.
- **Calendar Type**: Specifies how you choose filtering dates in the skill UI. Choose `Islamic-Civil`, `Islamic-Tabular`, `Islamic-Umm al-Qura`, or `Gregorian`.

  This setting is not reflected in the "Try it out" panel.
  {: note}

![Bidi options](images/bidi-options.png)

1.  Click the **X** to close the page. Your changes are saved automatically.

## Working with accented characters
{: #language-support-accents}

In a conversational setting, users might or might not use accents while interacting with the {{site.data.keyword.conversationshort}} service. As such, both accented and non-accented versions of words might be treated the same for intent detection and entity recognition.

However for some languages, like Spanish, some accents can alter the meaning of the entity. Thus, for entity detection, although the original entity might implicitly have an accent, your assistant can also match the non-accented version of the same entity, but with a slightly lower confidence score.

For example, for the word "barrió", which has an accent and corresponds to the past tense of the verb "barrer" (to sweep), your assistant can also match the word "barrio" (neighborhood), but with a slightly lower confidence.

The system will provide the highest confidence scores in entities with exact matches. For example, `barrio` will not be detected if `barrió` is in the training set; and `barrió` will not be detected if `barrio` is in the training set.

You are expected to train the system with the proper characters and accents. For example, if you are expecting `barrió` as a response, then you should put `barrió` into the training set.

Although not an accent mark, the same applies to words using, for example, the Spanish letter `ñ` vs. the letter `n`, such as "uña" vs. "una". In this case the letter `ñ` is not simply an `n` with an accent; it is a unique, Spanish-specific letter.

You can enable fuzzy matching if you think your customers will not use the appropriate accents, or misspell words (including, for example, putting a `n` instead of a `ñ`), or you can explicitly include them in the training examples.
