---

copyright:
  years: 2021, 2023
lastupdated: "2023-09-01"

subcollection: assistant

---

{{site.data.keyword.attribute-definition-list}}

This documentation for the **classic {{site.data.keyword.assistant_classic_short}}** experience has moved. For the most up-to-date version, see [Securing your assistant](/docs/watson-assistant?topic=watson-assistant-admin-securing){: external}. To see all documentation for the new {{site.data.keyword.conversationshort}}, please go [here](https://cloud.ibm.com/docs/watson-assistant){: external}.
{: attention}

# Securing your assistant
{: #admin-securing}

IBM is committed to providing our clients and partners with innovative data privacy, security, and governance solutions.
{: shortdesc}

**Notice:**
Clients are responsible for ensuring their own compliance with various laws and regulations, including the European Union General Data Protection Regulation (GDPR). Clients are solely responsible for obtaining advice of competent legal counsel as to the identification and interpretation of any relevant laws and regulations that may affect the clients’ business and any actions the clients may need to take to comply with such laws and regulations.

The products, services, and other capabilities described herein are not suitable for all client situations and may have restricted availability. IBM does not provide legal, accounting or auditing advice or represent or warrant that its services or products will ensure that clients are in compliance with any law or regulation.

If you need to request GDPR support for {{site.data.keyword.cloud}} {{site.data.keyword.watson}} resources that are created

- In the European Union, see [Requesting support for IBM Cloud Watson resources created in the European Union](/docs/watson/getting-started-gdpr-sar?topic=watson-gdpr-sar#request-EU){: external}.
- Outside the European Union, see [Requesting support for resources outside the European Union](/docs/watson/getting-started-gdpr-sar?topic=watson-gdpr-sar#request-non-EU){: external}.

## European Union General Data Protection Regulation (GDPR)
{: #securing-gdpr}

IBM is committed to providing our clients and partners with innovative data privacy, security and governance solutions to assist them on their journey to GDPR compliance.

Learn more about IBM's own GDPR readiness journey and our GDPR capabilities and offerings to support your compliance journey [here](https://www.ibm.com/data-responsibility/gdpr/){: external}.

## Health Insurance Portability and Accountability Act (HIPAA)
{: #securing-hipaa}

US Health Insurance Portability and Accountability Act (HIPAA) support is available for Enterprise plans that are hosted in the Washington, DC or Dallas locations. For more information, see [Enabling HIPAA support for your account](/docs/account?topic=account-enabling-hipaa){: external}.

Do not add personal health information (PHI) to the training data (entities and intents, including user examples) that you create. In particular, be sure to remove any PHI from files that contain real user utterances that you upload to mine for intent or intent user example recommendations.

## Opting out of log data use
{: #securing-log-opt-out}

IBM uses log data, Enterprise plan data excluded, to continually learn from and improve the {{site.data.keyword.assistant_classic_short}} product. The logged data is not shared or made public.

To prevent IBM from using your log data for general service improvements, complete one of the following tasks:

- If you are using a custom application, for each API `/message` request, set the `X-Watson-Learning-Opt-Out` header parameter to `true`.

    For more information, see [Data collection](/apidocs/assistant/assistant-v2#data-collection){: external}.
- If you are using the web chat integration, add the `learningOptOut` parameter to the script that you embed in your web page, and set it to `true`.

    For more information, see [Configuration](https://integrations.us-south.assistant.watson.cloud.ibm.com/web/developer-documentation/api-configuration){: external}.

## Labeling and deleting data in Watson Assistant
{: #securing-gdpr-wa}

Do not add personal data to the training data (actions and steps, including user examples) that you create. In particular, be sure to remove any personally-identifiable information from files that contain real user utterances that you upload to mine for user example recommendations.

Experimental and beta features are not intended for use with a production environment and therefore are not guaranteed to function as expected when labeling and deleting data. Experimental and beta features should not be used when implementing a solution that requires the labeling and deletion of data.

If you need to remove a customer's message data from a {{site.data.keyword.assistant_classic_short}} instance, you can do so based on the customer ID of the client, as long as you associate the message with a customer ID when the message is sent to {{site.data.keyword.assistant_classic_short}}.

Removing message data must be an occasional event only for individual customer IDs. To disable analytics logs, you can upgrade to an Enterprise with Data Isolation plan.
{: note}

- The assistant preview and automatic Facebook integration do not support the labeling and therefore deletion of data based on customer ID. They should not be used in a solution that must support the ability to delete data based on a customer ID.
- For Intercom, the `customer_id` is the `user_id` prepended with `intercom_`. The Intercom `user_id` property is the `id` of the `author` message object in the Conversation Model that is defined by Intercom.

    - To get the ID, open the channel from a web browser. Open the web developer tools to view the console. Look for `author`.

    The full customer ID looks like this: `customer_id=intercom_5c499e5535ddf5c7fa2d72b3`.
- For Slack, the `customer_id` is the `user_id` prepended with `slack_`. The Slack `user_id` property is a concatenation of the team ID, such as `T09LVDR7Y`, and the member ID of the user, such has `W4F8K9JNF`. For example: `T09LVDR7YW4F8K9JNF`.

    - To get the team ID, open the channel from a web browser. Open the web developer tools to view the console. Look for `[BOOT] Initial team ID`.
    - You can copy the member ID from the user's Slack profile.
    - To get the IDs programmatically, use the Slack API. For more information, see [Overview](https://api.slack.com/apis){: external}.
    The full customer ID looks like this: `customer_id=slack_T09LVDR7YW4F8K9JNF`.

- For the web chat integration, the service takes the `user_id` that is passed in and adds it as the `customer_id` parameter value to the `X-Watson-Metadata` header with each request.

### Before you begin
{: #securing-delete-user-data-prereqs}

To be able to delete message data associated with a specific user, you must first associate all messages with a unique **customer ID** for each user. To specify the **customer ID** for any messages sent using the `/message` API, include the `X-Watson-Metadata: customer_id` property in your header. For example:

```sh
curl -X POST -u "apikey:3Df... ...Y7Pc9"
 --header
   'Content-Type: application/json'
   'X-Watson-Metadata: customer_id=abc'
 --data
   '{"input":{"text":"hello"}}'
  '{url}/v2/assistants/{assistant_id}/sessions/{session_id}/message?version=2019-02-28'
```
{: pre}

where {url} is the appropriate URL for your instance. For more details, see [Service endpoint](/apidocs/assistant/assistant-v2#service-endpoint){: external}.

The `customer_id` string cannot include the semicolon (`;`) or equal sign (`=`) characters. You are responsible for ensuring that each `customer ID` property is unique across your customers.
{: note}

Only the first **customer ID** value that is passed in the `X-Watson-Metadata` header is used as the `customer_id` string for the message log. This **customer ID** value can be deleted with `DELETE /user_data` v1 API calls.

If you add search to an assistant, user input that is submitted to the assistant is passed to the {{site.data.keyword.discoveryshort}} service as a search query. If the {{site.data.keyword.assistant_classic_short}} integration provides a customer ID, then the resulting `/message` API request includes the customer ID in the header, and the ID is passed through to the {{site.data.keyword.discoveryshort}} `/query` API request. To delete any query data that is associated with a specific customer, you must send a separate delete request directly to the {{site.data.keyword.discoveryshort}} service instance that is linked your the assistant. See the {{site.data.keyword.discoveryshort}} [information security](/docs/discovery/information-security?topic=discovery-information-security) topic for details.

### Querying user data
{: #securing-query-customer-id}

Use the v1 `/logs` method `filter` parameter to search an application log for specific user data. For example, to search for data specific to a `customer_id` that matches `my_best_customer`, the query might be:

``` sh
curl -X GET -u "apikey:3Df... ...Y7Pc9" \
"{url}/v2/assistants/{assistant_id}/logs?version=2020-04-01&filter=customer_id::my_best_customer"
```
{: pre}

where {url} is the appropriate URL for your instance. For more details, see [Service endpoint](/apidocs/assistant/assistant-v2#service-endpoint){: external}.

See the [Filter query reference](/docs/watson-assistant?topic=watson-assistant-filter-reference){: external} for additional details.

### Deleting data
{: #securing-delete-data}

To delete any message log data associated with a specific user that your assistant might have stored, use the `DELETE /user_data` v1 API method. Specify the customer ID of the user by passing a `customer_id` parameter with the request.

Only data that was added by using the `POST /message` API endpoint with an associated customer ID can be deleted using this delete method. Data that was added by other methods cannot be deleted based on customer ID. For example, entities and intents that were added from customer conversations, cannot be deleted in this way. Personal Data is not supported for those methods.

**IMPORTANT**: Specifying a `customer_id` will delete *all* messages with that `customer_id` that were received before the delete request, across your entire {{site.data.keyword.assistant_classic_short}} instance, not just within one skill.

As an example, to delete any message data associated with a user that has the customer ID `abc` from your {{site.data.keyword.assistant_classic_short}} instance, send the following cURL command:

```sh
curl -X DELETE -u "apikey:3Df... ...Y7Pc9" \
"{url}/v2/user_data?customer_id=abc&version=2020-04-01"
```
{: pre}

where {url} is the appropriate URL for your instance. For more details, see [Service endpoint](/apidocs/assistant/assistant-v2#service-endpoint){: external}.

An empty JSON object `{}` is returned.

For more information, see the [API reference](/apidocs/assistant/assistant-v2#deleteuserdata).

**Note:** Delete requests are processed in batches and may take up to 24 hours to complete.

## Web chat usage data
{: #web-chat-data}

The Watson Assistant web chat sends limited usage data to the [Amplitude service](https://amplitude.com/){: external}. When the web chat widget is being interacted with by a user, we track the features that are being used, and events such as how many times the widget is opened and how many users start conversations. This information does not include Assistant training data or the content of any chat interactions. The information being sent to Amplitude is not Content as defined in the Cloud Service Agreement (CSA); it is Account Usage Information as described in Section 9.d of the CSA and is handled accordingly as described in the [IBM Privacy Statement](https://www.ibm.com/privacy){: external}. The purpose of this information gathering is limited to establishing statistics about use and effectiveness of the web chat and making general improvements.

## Private network endpoints
{: #security-private-endpoints}

You can set up a private network for {{site.data.keyword.assistant_classic_short}} instances that are part of a Plus or Enterprise service plan. Using a private network prevents data from being transferred over the public internet, and ensures greater data isolation.

![Plus or higher plans only](images/plus.png) This feature is available only to users of paid plans.

Private network endpoints support routing services over the {{site.data.keyword.cloud_notm}} private network instead of the public network. A private network endpoint provides a unique IP address that is accessible to you without a VPN connection.

For implementation details, see [Public and private network endpoints](/docs/watson?topic=watson-public-private-endpoints){: external}.

### Important private network endpoint notes
{: #security-private-endpoint-notes}

- The integrations that are provided with the product require endpoints that are available on the public internet. Therefore, any built-in integrations you add to your assistant will have public endpoints. If you only want to connect to a client application or messaging channel over the private network, then you must build your own custom client application or channel integration.
- Before you can use a search integration or search skill, you must create a {{site.data.keyword.discoveryshort}} instance with a private network endpoint. The list of {{site.data.keyword.discoveryshort}} instances that are displayed for you to connect to includes only instances with private network endpoints.

## Related topics
{: #security-related}

- [Security architecture](https://www.ibm.com/cloud/architecture/architecture/practices/watson-security-architecture/){: external}: Describes the security components that are needed for secure cloud development, deployment, and operations.
- [IBM Cloud compliance programs](https://www.ibm.com/cloud/compliance){: external}: Describes how to manage regulatory compliance and internal governance requirements with IBM Cloud services.
