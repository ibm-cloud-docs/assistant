---

copyright:
  years: 2015, 2018
lastupdated: "2018-10-19"
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

# Release notes

## Service API Versioning
{: shortdesc}

API requests require a version parameter that takes a date in the format `version=YYYY-MM-DD`. Whenever we change the API in a backwards-incompatible way, we release a new minor version of the API.

Send the version parameter with every API request. The service uses the API version for the date you specify, or the most recent version before that date. Don't default to the current date. Instead, specify a date that matches a version that is compatible with your app, and don't change it until your app is ready for a later version.

- The current version for V1 is `2018-09-20`.
- The only supported version for V2 is `2018-09-20`.
- The "Try it out" pane in the {{site.data.keyword.conversationshort}} tooling is using version `2018-07-10`.

## Beta features

IBM releases services, features, and language support for your evaluation that are classified as beta. These features might be unstable, might change frequently, and might be discontinued with short notice. Beta features also might not provide the same level of performance or compatibility that generally available features provide and are not intended for use in a production environment. Beta features are supported only on the [IBM Developer Answers ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://developer.ibm.com/answers/topics/watson-assistant/){: new_window}.

## API authentication method will change on October 30

(See [Authenticating with IAM tokens ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/watson/getting-started-iam.html) for more information.)

The method used to authenticate with IAM service instances is different from the method used to authenticate with Cloud Foundry instances. Existing applications that use Cloud Foundry will continue to work. However, if you migrate a service instance or create a new service instance in a region that uses IAM, you must update the code that handles authentication.

## Changes
{: #change-log}

The following new features and changes to the service are available.

### 22 October 2018
{: #22October2018}

- **Introducing search skills**: Add a search skill to your assistant that can connect to an {{site.data.keyword.discoveryfull}} data collection and retrieve information that is relevant to a user's query. See [Building a search skill](create-search-skill.html) for more details.

### 11 October 2018
{: #11October2018}

- **Intercom integration**: You can now add your assistant as a teammate in Intercom and assign certain types of support requests to the assistant. See [Integrating with Intercom](deploy-intercom.html) for more details.

### 3 October 2018
{: #3October2018}

- When reviewing logs for one skill, you can switch the data source to view metrics about message data from a different assistant and gain insights from it. You can make improvements to the current skill based on what you learn from the data. See [Improving across assistants](logs.html#deploy_id).

### 21 September 2018
{: #21September2018}

- **Intent recommendations**: You can upload a file that contains raw user inputs for the service to analyze and find candidate intent user examples. See [Adding examples from log files](intents.html#intent-recommendations).
- **New major API version**: A V2 version of the API is available. This version introduces the following changes:

  - Provides access to methods you can use to interact with an assistant at run time.
  - The `errors[].path` attribute of the error object that is returned by the API is now expressed as a [JSON Pointer](https://tools.ietf.org/html/rfc6901) instead of in dot notation form.

  See [API Overview](api-overview.html) for more details.

- **New workflow**: The user interface was updated to encourage a new workflow that starts with creating a skill first, and then adding the skill to an assistant.
- **Web actions support**: You can now call {{site.data.keyword.openwhisk_short}} web actions from a dialog node. See [Making programmatic calls from a dialog node](dialog-actions.html) for more details.
- **New terminology**: The documentation was updated to use 'dialog skill' instead of 'conversational skill' when referring to skills.
- **Dialog node limit changes**: The dialog node limit decreased for the following service plans:

  <table>
  <caption>Dialog node limit changes</caption>
    <tr>
      <th>Plan</th>
      <th>Old limit</th>
      <th>New limit</th>
    </tr>
    <tr>
      <td>Standard</td>
      <td>100,000</td>
      <td>500</td>
    </tr>
    <tr>
       <td>Lite</td>
       <td>25,000</td>
       <td>100</td>
    </tr>
  </table>

    Users of dialogs that were created before the limit change have 6 months to upgrade, or edit the dialog to meet the new limit requirements. After the grace period ends, adding new nodes will be prohibited until the dialog complies with the limits. Existing nodes will not be removed.

    See [Troubleshooting skill import issues](create-skill.html#import-errors) for information about how to edit workspaces or skills that you want to continue using.

-  **Artifact changes**: The tool was re-architected to use the new V2 APIs. As a result, assistants that existing users created before today are impacted. Specifically, any skills that you added to assistants have been removed from them. Also, any integrations you created (Facebook or Slack, for example) have been removed. You must add skills to your assistants again, and recreate integrations for your assistants.

### 6 August 2018
{: #6August2018}

- **Digression return message**: You can now specify text to display when the user returns to a node after a digression. The user will have seen the prompt for the node already. You can change the message slightly to let users know they are returning to where they left off. For example, specify a response like, `Where were we? Oh, yes...` See [Digressions](dialog-runtime.html#digressions) for more details.

- **Jump-to fix**: Fixed a bug in the Dialogs tool which prevented you from being able to configure a jump-to that targets the response of a node with the `anything_else` special condition.

### 20 June 2018
{: #20June2018}

- **Intent conflict resolution**: The {{site.data.keyword.conversationshort}} can now help you to resolve conflicts when two or more intent examples in separate intents are so similar that {{site.data.keyword.conversationshort}} is confused as to which intent to use. See [Resolving intent conflicts](intents.html#conflict-intents) for details.

- **Disambiguation**: Enable disambiguation to allow your assistant to ask the user for help when it needs to decide between two or more viable dialog nodes to process for a response. See [Disambiguation](dialog-runtime.html#disambiguation) for more details.

### 12 June 2018
{: #12June2018}

- **Pattern limit expanded**: When using the **Patterns** field to [define specific patterns for an entity value](entities.html#patterns), the pattern (regular expression) is now limited to 512 characters.

### 4 June 2018
{: #4June2018}

- **Rich responses**: You can now add rich responses, that include elements such as images or buttons in addition to text, to your dialog. See [Rich responses](dialog-overview.html#multimedia) for more information.

- **Contextual entities**: Entity annotations teach the service about the context in which entities are typically mentioned in speech. Add annotations by labeling entity mentions that occur in your intent user examples. This feature is currently available for English only. See [Defining contextual entities](entities.html#defining-contextual-entities) for more information.

### 7 May 2018
{: #7May2018}

- You can now chat with a live IBM representative. Click the ![Help](images/help_icon.png) icon from the tooling header, and then click **Chat with support**.

### 12 April 2018
{: #12April2018}

- A group of select customers were given access to a private beta build of {{site.data.keyword.conversationfull}} for evaluation. The Beta introduces customers to a new way of building assistants, one that manages the orchestration of information from one dialog call to the next, and that simplifies the deployment process so the assistant can start helping customers faster. Read [this blog post ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/blogs/watson/2018/03/the-future-of-watson-conversation-watson-assistant/) for more details.
