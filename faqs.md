---

copyright:
  years: 2015, 2021
lastupdated: "2021-08-05"

subcollection: assistant

content-type: faq

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
| Action | An action that you add to an actions skill represents a discrete task or question that your assistant is designed to help customers with. [Learn more](/docs/assistant?topic=assistant-actions-overview#actions-overview-actions). |
| Assistant | Container for your skills. You add skills to an assistant, and then deploy the assistant when you are ready to start helping your customers. [Learn more](/docs/assistant?topic=assistant-assistants). |
| Condition | Logic that is defined in the *If assistant recognizes* section of a dialog node that determines whether the node is processed. The dialog node conditions is equivalent to an If statement in If-Then-Else programming logic.
| Content catalog | A set of prebuilt intents that are categorized by subject, such as customer care. You can add these intents to your skill and start using them immediately. Or you can edit them to complement other intents that you create. [Learn more](/docs/assistant?topic=assistant-catalog). |
| Context variable | A variable that you can use to collect information during a conversation, and reference it later in the same conversation. For example, you might want to ask for the customer's name and then address the person by name later on. A context variable is used by the dialog skill. [Learn more](/docs/assistant?topic=assistant-dialog-runtime-context#dialog-runtime-context-variables).|
| Dialog | The component where you build the conversation that your assistant has with your customers. For each defined intent, you can author the response your assistant should return. [Learn more](/docs/assistant?topic=assistant-dialog-overview). |
| Digression | A feature that gives the user the power to direct the conversation. It prevents customers from getting stuck in a dialog thread; they can switch topics whenever they choose. [Learn more](/docs/assistant?topic=assistant-dialog-runtime#dialog-runtime-digressions). |
| Disambiguation | A feature that enables the assistant to ask customers to clarify their meaning when the assistant isn't sure what a user wants to do next. [Learn more](/docs/assistant?topic=assistant-dialog-runtime#dialog-runtime-disambiguation). |
| Entity | Information in the user input that is related to the user's purpose. An intent represents the action a user wants to do. An entity represents the object of that action. [Learn more](/docs/assistant?topic=assistant-entities). |
| Integrations | Ways you can deploy your assistant to existing platforms or social media channels. [Learn more](/docs/assistant?topic=assistant-deploy-integration-add). |
| Intent | The goal that is expressed in the user input, such as answering a question or processing a bill payment. [Learn more](/docs/assistant?topic=assistant-intents). |
| Message | A single turn within a conversation that includes a single call to the `/message` API endpoint and its corresponding response. |
| Monthly active user (MAU) | A single unique user who interacts with an assistant one or many times in a given month. |
| Preview | Embeds your assistant in a chat window that is displayed on an IBM-branded web page. From the preview, you can test how a conversation flows through any and all skills that are attached to your assistant, from end to end. [Learn more](/docs/assistant?topic=assistant-deploy-web-link). |
| Response | Logic that is defined in the *Assistant responds* section of a dialog node that determines how the assistant responds to the user. When the node's condition evaluates to true, the response is processed. The response can consist of an answer, a follow-up question, a webhook that sends a programmatic request to an external service, or slots which represent pieces of information that you need the user to provide before the assistant can help. The dialog node response is equivalent to a Then statement in If-Then-Else programming logic. |
| Skill | Does the work of the assistant. A dialog skill has the training data and dialog that your assistant uses to chat with customers. An actions skill is a new way to build a conversation. Actions offer step-by-step flows for a conversations and are made so that anybody can build them. A search skill is configured to search the appropriate external data sources for answers to customer questions. [Learn more](/docs/assistant?topic=assistant-skill-add). |
| Skill version | Versions are snapshots of a skill that you can create at key points during the development lifecycle. You can deploy one version to production, while you continue to make and test improvements that you make to another version of the skill. [Learn more](/docs/assistant?topic=assistant-versions). |
| Slots | A special set of fields that you can add to a dialog node that enable the assistant to collect necessary pieces of information from the customer. For example, the assistant can require a customer to provide valid date and location details before it gets weather forecast information on the customer's behalf. [Learn more](/docs/assistant?topic=assistant-dialog-slots). |
| Step | A step that you add to an action represents a single interaction or exchange of information with a customer, a turn in the conversation. [Learn more](/docs/assistant?topic=assistant-actions-overview#actions-overview-steps). |
| System entity | Prebuilt entities that recognize references to common things like dates and numbers. You can add these to your skill and start using them immediately. [Learn more](/docs/assistant?topic=assistant-system-entities). |
| Try it out | A chat window that you use to test as you build. For example, from the dialog skill's "Try it out" pane, you can mimic the behavior of a customer and enter a query to see how the assistant responds. You can test only the current skill; you cannot test your assistant and all attached skills from end to end. [Learn more](/docs/assistant?topic=assistant-dialog-tasks). |  
| Variable | A variable is data that a customer shares with the assistant, which is collected and saved so it can be referenced later. In an actions skill, you can collect *action* and *session* variables. [Learn more](/docs/assistant?topic=assistant-actions-overview#actions-overview-step-variables). |
| Web chat | An integration that you can use to embed your assistant in your company website. [Learn more](/docs/assistant?topic=assistant-deploy-web-chat). |
| Webhook | A mechanism for calling out to an external program during a conversation. For example, your assistant can call an external service to translate a string from English to French and back again in the course of the conversation. [Learn more](/docs/assistant?topic=assistant-webhook-overview). |

## I can't log in
{: #faqs-cannot-login}
{: faq}

If you are having trouble logging in to a service instance or see messages about tokens, such as `unable to fetch access token` or `400 bad request - header or cookie too large`, it might mean that you need to clear your browser cache. Open a private browser window, and then try again.

- If accessing the page by using a private browsing window fixes the issue, then consider always using a private window or clear the cache of your browser. You can typically find an option for clearing the cache or deleting cookies in the browser's privacy and security settings.
- If accessing the page by using a private browsing window doesn't fix the issue, then try deleting the API key for the instance and creating a new one.

## I'm being asked to log in repeatedly
{: #faqs-login-repeatedly}
{: faq}

If you keep getting messages, such as `you are getting redirected to login`, it might be due to one of the following things:

- The Lite plan you were using has expired. Lite plans expire if they are not used within a 30-day span. To begin again, log in to IBM Cloud and create a new service instance of Watson Assistant.
- An instance is locked when you exceed the plan limits for the month. To log in successfully, wait until the start of the next month when the plan limit totals are reset.

## I'm getting a `401` response
{: #faqs-getting-401}
{: faq}

The 401 response code is returned for many reasons, including:

- You exceeded the API call limit for your plan for this month. For example, for Lite plans, the monthly limit for API calls 10,000 messages per month. If you reach your limit for the month, but the logs show that you have made fewer calls than the limit, remember that the Lite plan stores log information for 7 days only. The 401 response will go away as soon as the next billing cycle begins, which is the first day of the calendar month.
- You are trying to use an instance-level API key that you created in one data center location to connect to a service instance that is hosted from another location. You must create an API key in the same data center location where your service instance is hosted.

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

You are being asked for credentials to access a {{site.data.keyword.conversationshort}} service instance that you have been able to access without trouble in the past. You might see, `Authentication Required: {service-url} is requesting your username and password.` or just a `Sign in` dialog box with fields for a username and password.

This message can be displayed for service instances that were migrated from Cloud Foundry, but for which access roles were not subsequently updated. After the migration, the service instance owner must update the user permissions to ensure that anyone who needs access to the instance is assigned to the appropriate Platform and Service access roles.

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

## Where can I find an example for creating my first assistant?
{: #faqs-get-started}
{: faq}

Follow the steps in the [Getting started with {{site.data.keyword.conversationshort}}](/docs/assistant?topic=assistant-getting-started) tutorial for a product introduction and to get help creating your first assistant.

## Can I export the user conversations from the Analytics page?
{: #faqs-export-conversation}
{: faq}

You cannot directly export conversations from the User conversation page.  You can, however, use the `/logs` API to list events from the transcripts of conversations that occurred between your users and your assistant. For more information, see the [API reference](https://cloud.ibm.com/apidocs/assistant/assistant-v1#listlogs){: external} and the [Filter query reference](/docs/assistant?topic=assistant-filter-reference).

## Can I export and import dialog nodes?
{: #faqs-nodes}
{: faq}

No, you cannot export and import dialog nodes from the product user interface.

If you want to copy dialog nodes from one skill into another skill, follow these steps:

1.  Download as JSON files both the dialog skill that you want to copy the dialog nodes from and the dialog skill that you want to copy the nodes to.
1.  In a text editor, open the JSON file for the dialog skill that you want to copy the dialog nodes from.
1.  Find the `dialog_nodes` array, and copy it.
1.  In a text editor, open the JSON file for the dialog skill that you want to copy the dialog nodes to, and then paste the `dialog_nodes` array into it.
1.  Import the JSON file that you edited in the previous step to create a new dialog skill with the dialog nodes you wanted.

## Is it possible to recover a deleted skill?
{: #faqs-delete-workspace}
{: faq}

Regularly [back up data](/docs/assistant?topic=assistant-backup) to prevent problems that might arise from inadvertent deletions. If you do not have a backup, there is a short window of time during which a deleted skill might be recoverable. Immediately following the deletion, [open a case](/docs/get-support?topic=get-support-open-case) with Support to determine if the data can be recovered. Include the following information in your case:

- skill ID
- instance ID or name
- region where the service instance is hosted from which the skill was deleted

## Can I change my plan to a Lite plan?
{: #faqs-downgrade-plan}
{: faq}

No, you cannot change from a Trial, Plus, or Standard plan to a Lite plan. And you cannot upgrade from a Trial to a Standard plan. For more information, see [Upgrading](/docs/assistant?topic=assistant-upgrade).

## How long are log files kept for a workspace?
{: #faqs-assistant-logs}
{: faq}

The length of time for which messages are retained depends on your service plan. For more information, see [Log limits](/docs/assistant?topic=assistant-logs#logs-limits).

## How do I create a webhook?
{: #faqs-webhook-how}
{: faq}

To define a webhook and add its details, open the skill where you want to add the webhook. Open the **Options** page, and then click **Webhooks** to add details about your webhook. To invoke the webhook, call it from one or more of your dialog nodes. For more information, see [Making a programmatic call from dialog](/docs/assistant?topic=assistant-dialog-webhooks).

## Can I have more than one entry in the URL field for a webhook?
{: #faqs-webhook-url}
{: faq}

No, you can define only one webhook URL for a dialog skill. For more information, see [Defining the webhook](/docs/assistant?topic=assistant-dialog-webhooks#dialog-webhooks-create).

## Can I extend the webhook time limit?
{: #faqs-webhook-timeout}
{: faq}

No. The service that you call from the webhook must return a response in 8 seconds or less, or the call is canceled. You cannot increase this time limit. For more information about strategies for handling complex actions with a webhook, watch the video in [Making a programmatic call from dialog](/docs/assistant?topic=assistant-dialog-webhooks).

## I received the message “Query cancelled” when importing a skill
{: #faqs-query-cancel}
{: faq}

This message is displayed when the skill import stops because artifacts in the skill, such as dialog nodes or synonyms, exceed the plan limits. For information about how to address this problem, see [Troubleshooting skill import issues](/docs/assistant-data?topic=assistant-data-skill-dialog-add#skill-dialog-add-import-errors).

If a timeout occurs due to the size of the skill but no plan limits are exceeded, you can reduce the number of elements that are imported at a time by completing the following steps:

1.	Make a copy of the JSON file that you are trying to import.
1.	Open the copy of the JSON file in an editor, and delete the `entities` array.
1.	Import the edited JSON file as a new skill.
1.	If this step is successful, edit the original copy of the JSON file.
1.	Remove the `dialog_nodes`, `intents`, and `counterexamples` arrays.
1.	Update the skill by using the API. Be sure to include the workspace ID and the `append=true` flag, as in this example:

```curl
curl -X POST -H "content-type: application/json" -H "accept: application/json" -u "apikey:{apikey}" -d@./skill.json "url/api/v1/workspaces/{workspace_id}?version=2019-02-28&append=true"
```
{: codeblock}

## The training process takes a long time and appears stuck
{: #faqs-stuck-training}
{: faq}

If the training process gets stuck, first check whether there is an outage for the service by going to the [Cloud status page](https://cloud.ibm.com/status){: external}. You can start a new training process to stop the current process and start over. To do so, add a new intent or entity, and then delete it. This action starts a new training process.

## Is there a range of IP addresses that are being used by a webhook?
{: #faqs-webhook-ip}
{: faq}

Unfortunately, the IP address ranges from which Watson Assistant may call a webhook URL are subject to change, which in turn prevent using them in any static firewall configuration. Please use the https transport and specify an authorization header to control access to the webhook.

## How do I see my monthly active users in Watson Assistant?
{: #faqs-see-mau}
{: faq}

To see your monthly active users (MAU) do the following:
1.  Sign in to https://cloud.ibm.com
1.  Click on the **Manage** menu, then choose **Billing and usage**.
1.  Click on **Usage**.
1.  For Watson Assistant, select **View Plans**.
1.  Under Time Frame, select the month you need.
1.  Select your Plus plans or Plus Trial plans to see monthly active users and the API calls.

## Error: New Off Topic not supported
{: #faqs-offtopic-error}
{: faq}

You see the error `New Off Topic not supported` after editing the JSON file for a dialog skill and changing the skill language from English to another language.

To resolve this issue, modify the JSON file by setting `off_topic` to `false`. Off-topic, or irrelvant detection, is only supported for English-language dialog skills. For more information about this feature, see [Defining what's irrelevant](/docs/assistant?topic=assistant-irrelevance-detection).
