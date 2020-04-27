---

copyright:
  years: 2015, 2020
lastupdated: "2020-04-23"

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

The first table shows the level of support for all features, except those related to intents and entities, which are shown in the second and third tables.

## Feature support details
{: #language-support-tables}

Table 1. Feature support details

| Language | **Defining [intents](/docs/assistant?topic=assistant-intents)**, **[entities](/docs/assistant?topic=assistant-entities)**, and **[dialog](/docs/assistant?topic=assistant-dialog-build)** | **Keyword search function in dialog skill** |
|:---:|:---:|:---:|
| **English (en)**                   | GA | GA |
| **Arabic (ar)**                    | GA | NA |
| **Chinese (Simplified) (zh-cn)**   | GA | Beta |
| **Chinese (Traditional) (zh-tw)**  | Beta | Beta |
| **Czech (cs)**                     | GA | Beta |
| **Dutch (nl)**                     | GA | Beta |
| **French (fr)**                    | GA | Beta |
| **German (de)**                    | GA | Beta |
| **Italian (it)**                   | GA | Beta |
| **Japanese (ja)**                  | GA | Beta |
| **Korean (ko)**                    | GA | Beta |
| **Portuguese (Brazilian) (pt-br)** | GA | Beta |
| **Spanish (es)**                   | GA | Beta |
{: caption="Feature support details" caption-side="top"}

Table 2a. Intent feature support details

| Language | **[Absolute scoring](/docs/assistant?topic=assistant-intents#intents-absolute-scoring)** and **[Mark as irrelevant](/docs/assistant?topic=assistant-logs#logs-mark-irrelevant)** | **[Content Catalog](/docs/assistant?topic=assistant-catalog)** | **[Autocorrection](/docs/assistant?topic=assistant-dialog-runtime#dialog-runtime-spell-check)** | 
|:---:|:---:|:---:|:---:|
| **English (en)**                   | GA | GA | GA |
| **Arabic (ar)**                    | Beta | GA (except Covid-19) | NA |
| **Chinese (Simplified) (zh-cn)**   | GA | NA | NA |
| **Chinese (Traditional) (zh-tw)**  | Beta | NA | NA |
| **Czech (cs)**                     | GA | NA | NA |
| **Dutch (nl)**                     | GA | NA | NA |
| **French (fr)**                    | GA | GA | Beta |
| **German (de)**                    | GA | GA (except Covid-19) | NA |
| **Italian (it)**                   | GA | GA (except Covid-19) | NA |
| **Japanese (ja)**                  | GA | GA (except Covid-19) | NA |
| **Korean (ko)**                    | GA | NA | NA |
| **Portuguese (Brazilian) (pt-br)** | GA | GA | NA |
| **Spanish (es)**                   | GA | GA | NA |
{: caption="Intent feature support details" caption-side="top"}

Table 2b. Intent feature support details continued

| Language | **[User example recommendations](/docs/assistant?topic=assistant-intent-recommendations#intent-recommendations-get-example-recommendations-task)** | **[Intent recommendations](/docs/assistant?topic=assistant-intent-recommendations#intent-recommendations-get-intent-recommendations-task)** | **[Irrelevance detection](/docs/assistant?topic=assistant-irrelevance-detection)** |
|:---:|:---:|:---:|
| **English (en)**                   | GA | GA | GA |
| **Arabic (ar)**                    | NA | NA | NA |
| **Chinese (Simplified) (zh-cn)**   | NA | NA | NA |
| **Chinese (Traditional) (zh-tw)**  | NA | NA | NA |
| **Czech (cs)**                     | NA | NA | NA |
| **Dutch (nl)**                     | NA | NA | NA |
| **French (fr)**                    | NA | NA | NA |
| **German (de)**                    | NA | NA | NA |
| **Italian (it)**                   | NA | NA | NA |
| **Japanese (ja)**                  | GA | NA | NA |
| **Korean (ko)**                    | NA | NA | NA |
| **Portuguese (Brazilian) (pt-br)** | NA | NA | NA |
| **Spanish (es)**                   | NA | NA | NA |
{: caption="Intent feature support details continued" caption-side="top"}

Table 3. Entity feature support details

| Language | **[Entity fuzzy matching](/docs/assistant?topic=assistant-entities#entities-fuzzy-matching)** | **[Contextual entities](/docs/assistant?topic=assistant-entities#entities-create-annotation-based)** | **[Synonym recommendations](/docs/assistant?topic=assistant-entities#entities-synonyms)** |
|:---:|:---:|:---:|:---:|
| **English (en)**                   | GA | GA | GA |
| **Arabic (ar)**                    | GA (Misspelling only) | NA | NA |
| **Chinese (Simplified) (zh-cn)**   | NA | NA | NA |
| **Chinese (Traditional) (zh-tw)**  | NA | NA | NA |
| **Czech (cs)**                     | GA | NA | NA |
| **Dutch (nl)**                     | GA (Misspelling only) | NA | NA |
| **French (fr)**                    | GA | Beta | GA |
| **German (de)**                    | GA | NA | NA |
| **Italian (it)**                   | GA (Misspelling only) | NA | NA |
| **Japanese (ja)**                  | GA (Misspelling only) | NA | GA |
| **Korean (ko)**                    | GA (Misspelling only) | NA | NA |
| **Portuguese (Brazilian) (pt-br)** | GA (Misspelling only) | NA | NA |
| **Spanish (es)**                   | GA (Misspelling only) | NA | GA |
{: caption="Entity feature support details" caption-side="top"}

Table 4. System entity feature support details

| Language | **System entities ([number](/docs/assistant?topic=assistant-system-entities#system-entities-sys-number), [currency](/docs/assistant?topic=assistant-system-entities#system-entities-sys-currency), [percentage](/docs/assistant?topic=assistant-system-entities#system-entities-sys-percentage), [date, time](/docs/assistant?topic=assistant-system-entities#system-entities-sys-date-time))** | **[New system entities](/docs/assistant?topic=assistant-new-system-entities)** |
|:---|:---:|:---:|
| **English (en)**                   | GA, Beta ([location](/docs/assistant?topic=assistant-system-entities#system-entities-sys-location), [person](/docs/assistant?topic=assistant-system-entities#system-entities-sys-person)) | GA |
| **Arabic (ar)**                    | Beta | Beta |
| **Chinese (Simplified) (zh-cn)**   | GA | Beta |
| **Chinese (Traditional) (zh-tw)**  | Beta | Beta |
| **Czech (cs)**                     | GA | GA |
| **Dutch (nl)**                     | GA | GA |
| **French (fr)**                    | GA | GA |
| **German (de)**                    | GA | GA |
| **Italian (it)**                   | GA | GA |
| **Japanese (ja)**                  | GA | Beta |
| **Korean (ko)**                    | GA | Beta |
| **Portuguese (Brazilian) (pt-br)** | GA | GA |
| **Spanish (es)**                   | GA | GA |
{: caption="System entity feature support details" caption-side="top"}

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
