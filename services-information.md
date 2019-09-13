---

copyright:
  years: 2015, 2019
lastupdated: "2019-09-12"

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

# IBM Cloud services information
{: #services-information}

The assistant is a fully hosted bot that is managed by {{site.data.keyword.cloud}}, which means you do not need to worry about setting up or maintaining infrastructure to support it.
{: shortdesc}

## Service plan information
{: #services-information-plans}

Explore the {{site.data.keyword.conversationshort}} [service plan options ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/watson-assistant/pricing/){: new_window}.

Before you create a service instance, decide how you want to organize the resources in your {{site.data.keyword.cloud_notm}} account. If you do not define your own resource group, the **default** resource group is used, and you *cannot* change it later. See [Best practices for organizing resources in a resource group ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/docs/resources/bestpractice_rgs#bp_resourcegroups){: new_window} for more details. All users must have a platform access role of Operator. (Service access roles are not leveraged by {{site.data.keyword.conversationshort}}.)

To find out the service plan to which your current instance belongs, complete these steps:

1.  Make a note of the name of the instance you are currently using. (You can find and change the instance from the main Skills or Assistants pages.)
1.  Go to the [IBM Cloud Resource list ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/resources) page.
1.  Expand the **Services** section, find the instance name that you noted earlier, and click it to see the associated plan information.

### Plan limits by artifact type
{: #services-information-limits}

Information about the artifact limits per plan is available from the topics that describe how to create the artifacts, so you can refer to the limits when you need to know them. Here are links to the topics:

- [Assistants](/docs/services/assistant?topic=assistant-assistant-add#assistant-add-limits)
- [Dialog nodes](/docs/services/assistant?topic=assistant-dialog-build#dialog-build-node-limits)
- [Entities](/docs/services/assistant?topic=assistant-entities#entities-limits)
- [Inactivity timeout](/docs/services/assistant?topic=assistant-assistant-settings#assistant-settings-session-limits)
- [Intents](/docs/services/assistant?topic=assistant-intents#intents-limits)
- [Integrations](/docs/services/assistant?topic=assistant-deploy-integration-add#deploy-integration-add-limits)
- [Logs](/docs/services/assistant?topic=assistant-logs#logs-limits)
- [Skills](/docs/services/assistant?topic=assistant-skill-add#skill-add-limits)
- [Versions](/docs/services/assistant?topic=assistant-versions#versions-limits)

### API call limits
{: #services-information-api-limits}

The number of API calls allowed per instance depends on your service plan. See your plan description for details.

If you have a Lite plan and reach your API call limit, but the logs show that you have made fewer calls than expected, remember that the Lite plan stores log information for only 7 days.

If you want to upgrade from one plan to another, see [Upgrading](/docs/services/assistant?topic=assistant-upgrade).

### Plus and Premium plan features ![Plus or Premium plan only](images/plus.png)
{: #services-information-premium}

The following features are available only to users of Plus or Premium plans only.

- [Disambiguation](/docs/services/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation)
- [Intent conflict resolution](/docs/services/assistant?topic=assistant-intents#intents-resolve-conflicts)
- [Intent recommendations and intent user example recommendations](/docs/services/assistant?topic=assistant-intent-recommendations)
- [Intercom integration](/docs/services/assistant?topic=assistant-deploy-intercom)
- [Search skill](/docs/services/assistant?topic=assistant-skill-search-add)

### User-based plans
{: #services-information-user-based-plans}

Unlike API-based plans, which measure usage by the number of API calls made during a specified timeframe, the new Plus plan and updated Premium plan use user-based billing. They measure usage by the number of unique users who interacted with the assistant during a specified timeframe.

{{site.data.keyword.conversationshort}} checks for the following information from API requests in this order for billing purposes:

  1.  **user_id**: A property defined in the API that is sent in the context object of a /message API call. Using this property is the best way to ensure that you accurately attribute /message API calls to unique users. For more information about the user ID property, see the API reference documentation:
  
    - `context.global.system.user_id`: [v2 API](https://cloud.ibm.com/apidocs/assistant-v2#send-user-input-to-assistant)
    - `context.metadata.user_id`: [v1 API](https://cloud.ibm.com/apidocs/assistant#get-response-to-user-input)

  1.  **session_id**: A property defined in the v2 API that identifies a single conversation between a user and the assistant. A session ID is provided in /message API calls that are generated by the built-in integrations. The session ends when a user closes the chat window or after the chat is inactive for 60 minutes.

  1.  **conversation_id**: A property defined in the v1 API that is stored in the context object of a /message API call. This property can be used to identify multiple /message API calls that are associated with a single conversational exchange with one user. However, the same ID is only used if you explicitly retain the ID and pass it back with each request that is made as part of the same conversation. Otherwise, a new ID is generated for each new /message API call.

To get the most benefit from the new user-based service plans, design any custom applications that you use to deploy your assistant to capture a unique user ID or session ID and pass the information to {{site.data.keyword.conversationshort}}.

For example, if the same person chats with your assistant on three separate occasions over the same billing period, how you represent that user in the API call impacts how the interactions are billed. If you identify the user interaction with a `user_id`, it will count as 1 use. If you identify the user interaction with a `session_id`, then it will count as 3 uses (because there is a separate session created for each interaction).

## Authenticating API calls
{: #services-information-authenticate-api-calls}

The authentication mechanism used by your service instance impacts how you must provide credentials when making an API call.

1.  Get the service credentials.

    - Find and click the service instance in the [{{site.data.keyword.Bluemix_notm}}  Resource List ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com){: new_window}.

    - Click to open your service instance, click **Service credentials**, and then click **View credentials**.

      **Cloud Foundry credentials**

      ![Shows the service credentials page for Cloud Foundry-hosted instances.](images/cf-cred-ui.png)

      **IAM credentials**

      ![Shows the service credentials page for IAM-hosted instances.](images/iam-creds.png)

1.  Use these credentials in your API call.

    **Cloud Foundry API call**

    Provide your username and password credentials.

    ```curl
    curl -X GET \
    --user {username}:{password} \
    'https://gateway.watson.net/assistant/api/v1/workspaces?version=2018-09-20'
    ```
    {: codeblock}

     **IAM API call**

    - The base url must include the location. Use the syntax `gateway-<location>.watsonplatform.net` to specify the location in which you created the service instance. The location codes are listed in the *Data center locations* table.
    - Provide the appropriate type of token in the header. You can pass either a bearer token or an API key.

      - Tokens support authenticated requests without embedding service credentials in every call. The following example shows a bearer token being used.

        ```curl
        curl -X GET \
        'https://gateway-syd.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        --header 'Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs'
        ```
        {: codeblock}

      - API keys use basic authentication. The following example shows an apikey being used.

        ```curl
        curl -X GET -u "apikey:3Df... ...Y7Pc9" \
        'https://gateway-us-east.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        ```
        {: codeblock}

        When you use any of the Watson SDKs, you can pass the API key and let the SDK manage the lifecycle of the tokens.
        {: note}

        IAM resources cannot be managed with the Cloud Foundry Command Line Interface (CLI). For example, Cloud Foundry CLI commands (beginning with `cf`) that create or manage service instances do not work with instances hosted in locations using IAM. Instead, you must use the {{site.data.keyword.cloud_notm}} CLI and its associated commands. See [Working with resources and resource groups ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_commands_resource) for more details.

        See [Authenticating with IAM tokens ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/services/watson?topic=watson-iam){: new_window} for more information.

    For examples, see  [Authentication ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://{DomainName}/apidocs/assistant-v2#authentication){: new_window} for your language in the API reference.

### Data centers
{: #services-information-regions}

{{site.data.keyword.cloud_notm}} has a network of global data centers that provide performance benefits to its cloud services. See [{{site.data.keyword.cloud_notm}} global data centers ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/data-centers/){: new_window} for more details.

{{site.data.keyword.cloud_notm}} changed from managing user access with Cloud Foundry to using token-based Identity and Access Management (IAM) authentication. IAM was rolled out in different locations at different times. You can migrate a service instance to move it from its current Cloud Foundry org and space to a resource group. See [Migrating](/docs/services/watson?topic=watson-migrate) for more details.

You can create {{site.data.keyword.conversationshort}} service instances that are hosted in the following data center locations:

| Location    | Location code | Authentication type | IAM adoption date | Notes |
|-------------|---------------|---------------------|-------------------|-------|
| Dallas      | us-south      | IAM                 | 30 October 2018 | N/A |
| Frankfurt   | eu-de         | IAM                 | 30 October 2018 | N/A |
| Sydney      | au-syd        | IAM                 | 7 May 2018 | Instances created before May 7 were syndicated to Dallas |
| Tokyo       | jp-tok        | IAM                 | 8 November 2018 | N/A |
| London      | eu-gb, lon    | IAM                 | 13 December 2018 | Instances that were created in the United Kingdom region before December 13 were syndicated to the US South region |
| Washington DC  | us-east    | IAM                 | 14 June 2018 | N/A |
{: caption="Data center locations" caption-side="top"}

For information about the data centers in which other {{site.data.keyword.cloud_notm}} services are hosted, see [Services by region ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/docs/resources/services_region#services_region){: new_window}.

## Terms and security
{: #services-information-terms}

To learn more about service terms and data security, read the following information:

- [Service terms ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www-03.ibm.com/software/sla/sladb.nsf/sla/home?OpenDocument){: new_window}
- [Data security and privacy ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/software/sla/sladb.nsf/sla/csdsp?OpenDocument){: new_window}
- [Information security](/docs/services/assistant?topic=assistant-information-security)

See [Platform overview ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/overview?topic=overview-whatis-platform){: new_window} for more information about {{site.data.keyword.cloud_notm}}.

## Still have questions? 
{: #services-information-sales}

Contact [IBM Sales ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www-01.ibm.com/marketing/iwm/dre/signup?source=urx-20970){: new_window}.
