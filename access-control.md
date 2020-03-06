---

copyright:
  years: 2015, 2020
lastupdated: "2020-03-06"

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

# Managing access to resources
{: #access-control}

You can give other people access to your {{site.data.keyword.conversationshort}} resources, and control the level of access they get.
{: shortdesc}

Maybe you want one development team to have access to a test assistant and another development team to have access to a production assistant. And you want data scientists to be able to view analtyics for user conversation logs from both assistants. And maybe you want a writer to be able to author the dialog that is used by your assistant to converse with your customers. To manage who can do what with your skills and assistants, you can assign different access roles to different people.

## Access control improvements
{: #access-control-new}

Historically, the service access role assignments that were defined for a {{site.data.keyword.conversationshort}} service instance in the IBM Cloud dashboard were ignored. Users with either Reader- or Writer-level service access to an instance effectively had Manager-level access. This behavior is changing. When the access control improvements are released, service-level access role assignments will be honored by the product.

### Location support
{: #access-control-geos}

Service level access roles are supported in the Tokyo and Seoul data centers only. Access assignments specified at the resource-level are not yet supported.

## How to keep your access
{: #access-control-prep}

When service level access control support is added to instances in your data center, if you have the wrong service level access role to an instance, you will be unable to do things you could do before. If you have Manager service access to a {{site.data.keyword.conversationshort}} service, then no action is required. You will have the same privileges and can do the same actions within the product after service-level access control is enabled.

If you have a Reader or Writer service access role, then what you can do will change when service-level access control support is added.

- **I'm a Reader**

  With a Reader role, you cannot take any actions in the {{site.data.keyword.conversationshort}} application user interface. You can only view the assistants and skills pages. You cannot edit, create, or delete anything, and cannot view analytics.

- **I'm a Writer**

  With a Writer role, you can view, edit, create, and delete resources, such as assistants or skills. However, you cannot view analytics or user conversation log information. Only people with a Manager service access role can view the analytics page.

Act now to get the service instance owner to change your access level.

1.  Contact the person who invited you to work with them on the resources in the service instance. 

    There is no way from the IBM Cloud dashboard to determine who owns a service instance.
    {: note}

1.  Ask the owner of the service instance to change your service access role assignment.

    You can copy, edit, and then paste the following text into an email or message that you send to the instance owner.

    ```
    You are the owner of the {service instance name} {{site.data.keyword.conversationshort}} service instance. 
    I am currently a {role-you-have} of the instance. 
    Please change my service access role to {role-you-want}.
    For instructions, see https://cloud.ibm.com/docs/assistant?topic=assistant-access-control-pre#access-control-admin-prep.
    ```
    {: codeblock}

## Before you grant access to others
{: #access-control-prereqs}

For each person to whom you grant access to your instance, decide whether you want to give the person a role with global access, which applies to all of the assistants and skills in a single service instance. Or give the person a resource level role so he or she can read or edit only individual skills and assistants within a service instance.

Resource-level roles can be defined for service instances that are managed by Identity and Access Management (IAM) only, not those managed by Cloud Foundry.
{: note}

## Granting users access to your resources
{: #access-control-task}

Only one person can edit an intent, entity, or a dialog node at a time. If multiple people work on the same item at the same time, then the changes made by the person who saves their changes last are the only changes applied. Changes that are made at the same time by someone else and that are saved first are not retained. Coordinate the updates that you plan to make with your team members to prevent anyone from losing their work.
{: important}

1.  If you plan to give a user access to a single skill or assistant in your service instance, get the ID for the skill or assistant. You'll need to provide the ID in a later step.

    - To get the assistant ID, go to the Assistants page. Click the overflow menu for the assistant, and then click **Settings > API Details**. Copy the assistant ID and paste it somewhere that you can access it from later.

    - To get the skill ID, go to the Skills page. Click the overflow menu for the skill, and then click **View API Details**. Copy the skill ID and paste it somewhere that you can access it from later.

1.  Click the User ![User](images/user-icon2.png) icon in the page header, 

1.  Make a note of the current instance name, and then select **Manage Users** from the drop-down.

    You are directed to the {{site.data.keyword.cloud}} Identity and Access Management (IAM) page.
1.  Click **Invite users**, and then enter the email addresses of the people to whom you want to grant access. 

    If you plan to grant specific resource-level access to different team members, then add one person at a time.
    {: tip}

1.  Expand *Assign users additional resources*, and then click **IAM services**.
1.  In the *No access* field, choose **Watson Assistant**.

    The *Account* resource group is used unless you change it in the next field.
    {: note}

1.  Optionally select a specific <!--region and -->service instance to share with this user. 

    Otherwise, the user access you define here will be applied to all of your services instances<!-- in all data center locations where you have instances-->.

    Remember, you made a note of the name of the current service instance in an earlier step.

1.  Optionally, indicate that you want to give this user access to a specific skill or assistant only by selecting an option from the **Resource Type** field. And then paste the associated ID (that you copied in an earlier step) into the **Assistant or Skill ID** field.
 
1.  Assign the person to the following role types for the service instance:

    - Platform
    - Service

    For help, see [Recommended role assignments](#access-control-recommendations). For a description of all your options, see [Global roles](#access-control-global-roles).

1.  **Optional**: Select a single resource, such as a skill or assistant, and then assign the person to the appropriate service role for the resource.

    For a description of all your options, see [Resource access](#access-control-resource-roles).

    Repeat this step to grant the same person access to other resources in this instance.

1.  Click **Add**.

1.  Click **Invite** to finish the process.

With more people contributing to the development of a dialog skill, unintended changes can occur, including skill deletions. Consider creating backup copies of your dialog skill on a regular basis, so you can roll back to an earlier version if necessary. To create a backup, simply [download the skill as a JSON file](/docs/services/assistant?topic=assistant-skill-dialog-add#skill-dialog-add-download).
{: tip}

## Recommended role assignments
{: #access-control-recommendations}

To get you started quickly, consider applying these roles at first.

- To give someone the same level of access to a {{site.data.keyword.conversationshort}} service instance as you have, assign the user to the following roles:

  - Instance platform role: **Administrator**
  - Instance service role: **Manager**

- To limit the person's privileges to certain resources, assign the user to the following roles:

  - Instance platform role: **Viewer**
  - Instance service role: **Reader** 
  - For each individual assistant or skill, assign the **Manager** role.

## Understanding roles
{: #access-control-iam-roles}

Access to {{site.data.keyword.conversationshort}}, its service instances and all of the resources that are used by the service is managed by {{site.data.keyword.cloud}}. To share a service instance with other users and control access to the instance, you must use the {{site.data.keyword.cloud_notm}} Identify and Access Management user interface. 

The breaks down access definitions into the following role types:

- Service instance (or global) platform roles
- Service instance (or global) service roles
- Resource service roles

### Global access
{: #access-control-global-roles}

To define someone's global access, meaning access roles that apply to a service instance and all of the resources that are created as part of the instance, decide which roles to assign to the user from among the following role types:

**Platform roles**

Choose a platform role to assign to the user.

| Role | Privileges of the users in this role |
|------|---------------------------------------|
| **Administrator** | Access and change all resources for the instance, and invite other people to access the instance. |
| **Viewer** | View the service instance, but cannot invite others to access the instance. |
{: caption="Table 1. Global platform role details" caption-side="top"}

The **Editor** and **Operator** platform roles are equivalent to the Administrator role.

At a minimum, you must give someone *Viewer* access to a service instance or they will not be able to access anything.
{: important}

**Service roles**

Optionally, choose one or more service roles for the user that will apply to all of the skills and assistants in the instance.

| Role | Privileges of the users in this role |
|------|---------------------------------------|
| **Reader** | Open and read all assistants and skills in the service instance, but not edit them. |
| **Writer** | Read, edit, delete, or create assistants and skill in the service instance. |
| **Manager** | Read, edit, delete, or create assistants and skill in the service instance; view all dialog skill conversation logs; access the v1 API for all skills in the instance. |
{: caption="Table 2. Global service role details" caption-side="top"}
<!--| Logs | View conversation logs for all of the dialog skills in the service instance. |
| Creator | Create new assistants and skills. |-->

### Resource access
{: #access-control-resource-roles}

To fine tune a person's level of access to individual skills and assistants within the instance, assign one or more resource-level service roles to the user.

You do not need to assign resource-level roles to people who already have access to all the resources based on their global service role. But, if you want to tailor a bit more who can read, edit, or add to specific assistants or skills within the instance, then apply resource-level roles.

**Service roles**

| Role | Privileges of the users in this role |
|------|---------------------------------------|
| Reader | Open and read the assistant or skill, but not edit it. |
| Writer | Read, edit, delete, or create an assistant or skill. |
| Manager | Read, edit, delete, or create an assistant or skill, and view conversation logs. |
{: caption="Table 3. Resource service role details" caption-side="top"}
<!--| Administrator | Invite others to collaborate on the specific assistant or skill, but not read or edit it. |
| Logs | View conversation logs for the specific assistant or skill, but not edit anything. |-->

Anyone who creates an assistant or skill is automatically granted the Manager role to that resource.

#### Service access examples
{: #access-control-ui-impact}

- Reader

  The following screen capture shows you what someone with a Reader service access role to a resource sees when viewing the Dialog page.

  ![Shows the dialog page with disabled buttons.](images/access-control-read-only.png)

  Most buttons, such as **Add node**, **Add folder**, and **Save new version** are disabled. There is no Analytics option in the navigation. The "Try it out" pane is available.
  {: note}

- Writer

  This screen capture shows you what someone with a Writer service access role to a resource sees when viewing the same page.

  ![Shows the dialog page with buttons enabled, but no Analytics option in nav.](images/access-control-writer.png)

  All of the buttons are available. There is no Analytics option in the navigation.
  {: note}

## Common role assignments
{: #access-control-common-roles}

| Goal | Global access platform role | Global access service roles | Resource service roles |
|------------------------|-----------------------------|-----------------------------|------------------------|
| Make someone a service instance co-owner | Administrator | Manager | N/A |
| Prevent someone from seeing a skill or assistant | Viewer | N/A | Reader |
| Allow somone to edit and view logs of all the skills and assistants in the service instance | Viewer | Manager | N/A |
| Allow somone to edit a skill or assistant in the service instance, but not view logs | Viewer | Manager | Writer |
{: caption="Table 4. Common role assignments" caption-side="top"}

N/A stands for no assignment, meaning no role of the type is assigned.

### Role assignments for lifecycle stages
{: #access-control-lifecycle-roles}

Use resource-level role assignments to limit who can edit live assistants that are interacting with your customers in a production environment. And protect customer data by limiting who can view analytics and user conversation logs.

## Resource-level role impact on user interface actions
{: #access-control-ui-impact}

The {{site.data.keyword.conversationshort}} user interface complies with the access roles that are defined for a service instance. When someone logs in, the user interface adjusts to show only what the current user can access, and it disables functions that the user does not have permissions to do. For example, a person assigned to the Reader role for a dialog skill cannot create or edit entities, intents, or dialog nodes in the skill because the **Create {resource}** and **Edit** functions are disabled. 

The following table shows the actions that can be performed by different resource-level service roles.

| Action | Reader | Writer | Manager |
|--------|--------|--------|---------|
| Open and view an assistant, skill, or integration | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| View API details for an assistant or skill | | | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Create an assistant or skill | | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Rename, edit, delete an assistant or skill | | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Create, edit, or delete an integration | | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Add, swap, or remove skills | | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Swap skill versions for an assistant | | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Revert to a previous skill version | | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Save or delete skill versions | | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Export a skill or skill version | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Duplicate a skill | | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Change bidirectional preferences | | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Open and view intents and intent examples | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Import, add (including from content catalog), edit, or delete intents or intent examples | | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Export intents | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Manage intent conflicts | | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Open and view entities | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Import, add, edit, or delete entities | | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Export entities | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Enable system entities and fuzzy matching | | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Add contextual entities | | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Open and view dialog nodes | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Add, move, edit, or delete dialog nodes or folders | | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Change dialog node settings | | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Test and set context in dialog skill "Try it out" pane | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Test in search skill "Try it out" pane | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| Improve data from dialog skill "Try it out" pane | | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| View analytics | | | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| View intent and intent example recommendations | | | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| v1 and v2 API POST requests to `/message` endpoint | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| v1 API requests (all but POST to `/message` endpoint) | | | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| v2 API all GET requests, v2 POST requests to `/sessions` endpoint| ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
| v2 API POST and DELETE requests (to all but `/message` and `/sessions` endpoints) | | ![Checkmark icon](../../icons/checkmark-icon.svg) | ![Checkmark icon](../../icons/checkmark-icon.svg) |
{: row-headers}
{: class="comparison-table"}
{: caption="Table 5. Action privileges per service role" caption-side="top"}
{: summary="This table has row and column headers. The row headers identify the user interface actions. The column headers indentify the different resource-level roles. To understand which roles can do an action, navigate to the row that describes the action, and find the columns for the role you are interested in."}
