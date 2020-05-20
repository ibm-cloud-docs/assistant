---

copyright:
  years: 2015, 2020
lastupdated: "2020-05-20"

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
{:faq: data-hd-content-type='faq'}

# FAQ
{: #faqs}

Find answers to frequently-asked questions and quick fixes for common problems. 
{: shortdesc}

## What's a...
{: #faqs-answers}
{: faq}

| Term | Definition |
|------|------------|
| Assistant | Container for your skills. You add skills to an assistant, and then deploy the assistant when you are ready to start helping your customers. [Learn more](/docs/assistant?topic=assistant-assistants). |
| Content catalog | A set of prebuilt intents that are categorized by subject, such as customer care. You can add these intents to your skill and start using them immediately. Or you can edit them to complement other intents that you create. [Learn more](/docs/assistant?topic=assistant-catalog). |
| Context variable | A variable that you can use to collect information during a conversation, and reference it later in the same conversation. For example, you might want to ask for the customer's name and then address the person by name later on. [Learn more](/docs/assistant?topic=assistant-dialog-runtime#dialog-runtime-context-variables).|
| Dialog | The component where you build the conversation that your assistant has with your customers. For each defined intent, you can author the response your assistant should return. [Learn more](/docs/assistant?topic=assistant-dialog-overview). |
| Digression | A feature that gives the user the power to direct the conversation. It prevents customers from getting stuck in a dialog thread; they can switch topics whenever they choose. [Learn more](/docs/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions). |
| Disambiguation | A feature that enables the assistant to ask customers to clarify their meaning when the assistant isn't sure what a user wants to do next. [Learn more](/docs/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation). |
| Entity | Information in the user input that is related to the user's purpose. An intent represents the action a user wants to do. An entity represents the object of that action. [Learn more](/docs/assistant?topic=assistant-entities). |
| Intent | The goal that is expressed in the user input, such as answering a question or processing a bill payment. [Learn more](/docs/assistant?topic=assistant-intents) |
| Skill | Does the work of the assistant. A dialog skill has the training data and dialog that your assistant uses to chat with customers. A search skill is configured to search the appropriate external data sources for answers to customer inquiries. [Learn more](/docs/assistant?topic=assistant-skills). |
| Skill version | Versions are snapshots of a skill that you can create at key points during the development lifecycle. You can deploy one version to production, while you continue to make and test improvements that you make to another version of the skill. [Learn more](/docs/assistant?topic=assistant-versions). |
| Slots | A special set of fields that you can add to a dialog node that enable the assistant to collect necessary pieces of information from the customer. For example, the assistant can require a customer to provide valid date and location details before it gets weather forecast information on the customer's behalf. [Learn more](/docs/assistant?topic=assistant-dialog-slots). |
| System entity | Prebuilt entities that recognize references to common things like dates and numbers. You can add these to your skill and start using them immediately. [Learn more](/docs/assistant?topic=assistant-system-entities). |
| Webhook | A mechanism for calling out to an external program as part of the dialog. For example, if a customer asks the assistant to translate a string from English to French, the dialog can call an external language translation service to translate the phrase and return the translation to the customer in the course of the conversation. [Learn more](/docs/assistant?topic=assistant-dialog-webhooks). |

## I can't log in
{: #faqs-cannot-login}
{: faq}

If you are having trouble logging in to a service instance or see messages about tokens, such as `unable to fetch access token`, it might mean that you need to clear your browser cache. Open a private browser window, and then try again. 

- If accessing the page by using a private browsing window fixes the issue, then consider always using a private window or clear the cache of your browser. You can typically find an option for clearing the cache or deleting cookies in the browser's privacy and security settings.
- If accessing the page by using a private browsing window doesn't fix the issue, then try deleting the API key for the instance and creating a new one.

## I'm being asked to log in repeatedly
{: #faqs-login-repeatedly}
{: faq}

If you keep getting messages, such as `you are getting redirected to login`, it might mean that the Lite plan you were using has expired. Lite plans expire if they are not used within a 30-day span.

## I'm getting a `401` response
{: #faqs-getting-401}
{: faq}

The 401 response code is returned for many reasons, including:

- You exceeded the API call limit for your plan for this month. For example, for Lite plans, the monthly limit for API calls 10,000 messages per month. If you reach your limit for the month, but the logs show that you have made fewer calls than the limit, remember that the Lite plan stores log information for 7 days only. The 401 response will go away as soon as the next billing cycle begins, which is the first day of the calendar month.
- You are trying to use an instance-level API key that you created in one data center location to connect to a service instance that is hosted from another location. You must create an API key in the same data center location where your service instance is hosted.
- Your service instance might be using Cloud Foundry for resource management. If you authenticate with a username and password, then it is likely that you are still using Cloud Foundry. If so, you must migrate the instance so it uses {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) instead. For more details, see [Migrating Watson services from Cloud Foundry](/docs/assistant?topic=watson-migrate).

## I'm getting a `403` response
{: #faqs-getting-403}
{: faq}

You are using a valid API Key, but it is not the correct key for the service instance that you are trying to access programmatically.

## Getting `Unable to fetch access token for account` message
{: #faqs-cannot-fetch-token}
{: faq}

The full message is, `Assistants could not be loaded at this time. Unable to fetch access token for account`.

This message is displayed for a few reasons:

- Your {{site.data.keyword.cloud}} refresh tokens might have expired. Log out and then log back in to {{site.data.keyword.cloud_notm}} to generate fresh tokens. 
- Make sure that the IBM ID that you logged in with has access to this service instance. To confirm, you can go to the [{{site.data.keyword.cloud_notm}} resources list](https://cloud.ibm.com/resources), and then log in with the same IBM ID. You should see this service instance listed as one of your available services.

## Getting `Authentication Required` or `Sign in` message
{: #faqs-authentication-required}
{: faq}

You are being asked for credentials to access a {{site.data.keyword.conversationshort}} service instance that you have been able to access without trouble in the past. You might see, `Authentication Required: https://assistant-{location}-watsonplatform.net is requesting your username and password.` or just a `Sign in` dialog box with fields for a username and password.

This message is displayed for service instances that have not been migrated from Cloud Foundry. If you have not migrated your instance, you must do so now. See [Migrating Watson services from Cloud Foundry](/docs/services/assistant?topic=watson-migrate).

It can also be displayed for service instances that were migrated from Cloud Foundry, but for which access roles were not subsequently updated. After the migration, the service instance owner must update the user permissions to ensure that anyone who needs access to the instance is assigned to the appropriate Platform and Service access roles.

To regain access to the service instance, ask the service instance owner to review your access permissions. Ask to be given at least a service access role of Writer. 

After your access roles are fixed, be sure to use the correct web address, the URL of the migrated service instance, to open it.

## I don’t see the Analytics page
{: #faqs-view-analytics}
{: faq}

To view the Analytics page, you must have a service role of Manager and a platform role of at least Viewer. For more information about access roles and how to request an access role change, see [Managing access to resources](/docs/assistant?topic=assistant-access-control).

## I am unable to view the API details, API key, or service credentials
{: #faqs-view-api-details}
{: faq}

If you cannot view the API details or service credentials, it is likely that you do not have Manager access to the service instance in which the resource was created. Only people with Manager service access to the instance can use the service credentials. For more information, see [Getting API information](/docs/assistant?topic=assistant-assistant-settings#assistant-settings-api-details).

## I can't edit intents, entities, or dialog nodes
{: #faqs-edit-skill}
{: faq}

To edit skills, you must have Writer service access to the service instance and a platform role of at least Viewer. For more information about access roles and how to request an access role change, see [Managing access to resources](/docs/assistant?topic=assistant-access-control).

## Can I export the user conversations from the Analtyics page?
{: #faqs-export-conversation}
{: faq}

You cannot directly export conversations from the User conversation page.  You can, however, use the `/logs` API to list events from the transcripts of conversations that occurred between your users and your assistant. For more information, see the [API reference](https://cloud.ibm.com/apidocs/assistant/assistant-v1#list-log-events-in-a-workspace){: external} and the [Filter query reference](/docs/assistant?topic=assistant-filter-reference#filter-query-syntax).

## Can I export and import dialog nodes?
{: #faqs-nodes}
{: faq}

No, you cannot export and import dialog nodes from the product user interface. You can use the API to export an entire workspace if you include the export=true parameter. For more information, see [API reference](https://cloud.ibm.com/apidocs/assistant/assistant-v1#get-information-about-a-workspace).

Alternatively, you can use the API to process dialog nodes data. Follow these steps:

1.  Use the API to list the nodes from the dialog that you want to export.

    For more information, see [Dialog nodes](https://cloud.ibm.com/apidocs/assistant-v1#list-dialog-nodes) in the API reference.
1.  Pipe the list of nodes to a JSON file.
1.  Parse the JSON file for each dialog node.
1.  Use the API to add a dialog node for each of the nodes that are listed in the JSON file to a new dialog.

    The API looks for `dialog_node`, so the JSON file needs to have `dialog_node` before each node that you want to create.
    {: note}

## Is it possible to recover a deleted skill?  
{: #faqs-delete-workspace}
{: faq}

Regularly [backing up data](/docs/assistant?topic=assistant-backup) can prevent problems that might arise from inadvertent deletions. If you do not have a backup, there is a short window of time during which a deleted skill might be recoverable. Immediately following the deletion, [open a case](/docs/get-support?topic=get-support-open-case#opentechcase) with Support to determine if the data can be recovered. Include the following information in your case: 

- skill ID
- instance ID or name
- region where the service instance is hosted from which the skill was deleted

## Can I change a Plus Trial or a Standard plan to a Lite plan?
{: #faqs-downgrade-plan}
{: faq}

No, you cannot upgrade from a Plus Trial to a Standard plan.  And you cannot change from a Plus Trial or Standard plan to a Lite plan. For more information, see [Upgrading](/docs/assistant?topic=assistant-upgrade).

## Can I change from a Standard plan to a Premium plan?
{: #faqs-upgrade-plan}
{: faq}

Yes, you can upgrade to a Premium plan. However, you cannot do an in-place upgrade of your service instance. After a Premium instance is provisioned, you export dialog skills from the old plan and import them to the new plan instance. For more information, see [Upgrading](/docs/assistant?topic=assistant-upgrade) to learn more.

## How long are log files kept for a workspace?
{: #faqs-assistant-logs}
{: faq}

The length of time for which messages are retained depends on your service plan. For more information, see [Log limits](/docs/assistant?topic=assistant-logs#logs-limits).

## Can I have more than one entry in the URL field for a webhook?
{: #faqs-webhook-url}
{: faq}

No, you can define only one webhook URL for a dialog skill. For more information, see [Defining the webhook](/docs/assistant?topic=assistant-dialog-webhooks#dialog-webhooks-create).

## I received the message “Query cancelled” when importing a skill
{: #faqs-query-cancel}
{: faq}

This message is displayed when the skill import stops because artifacts in the skill, such as dialog nodes or synonyms, exceed the plan limits. For information about how to address this problem, see [Troubleshooting skill import issues](/docs/assistant-data?topic=assistant-data-skill-dialog-add#skill-dialog-add-import-errors).  

If a time out occurs due to the size of the skill but no plan limits are exceeded, you can reduce the number of elements that are imported at a time by completing the following steps:

1.	Make a copy of the JSON file that you are trying to import.
1.	Open the copy of the JSON file in an editor, and delete the entities array.
1.	Import the edited JSON file as a new skill.
1.	If this step is successful, edit the original copy of the JSON file.
1.	Remove only the dialog_nodes, intents and counterexamples arrays.
1.	Update the skill by using the API. Be sure to include the workspace ID and the `append=true` flag, as in this example:

```curl
curl -X POST -H "content-type: application/json" -H "accept: application/json" -u "apikey:{apikey}" -d@./skill.json "url/api/v1/workspaces/{workspace_id}?version=2019-02-28&append=true"
```
{: codeblock}

## The training process takes a long time and appears stuck
{: #faqs-stuck-training}
{: faq}

If the training process gets stuck, you can start a new training process to stop the current process and start over. To do so, add a new intent or entity, and then delete it. This action starts a new training process.
