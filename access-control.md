---

copyright:
  years: 2015, 2019
lastupdated: "2020-01-23"

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

# Preventing loss of access
{: #access-control}

Access control is the ability to give other people access to your {{site.data.keyword.conversationshort}} resources, and control the level of access they get. Changes to how {{site.data.keyword.conversationshort}} access is managed are underway. Prepare your service instances for these access control changes today to prevent users from losing their current level of access when the feature updates are released.
{: shortdesc}

Currently, the platform access role assignment that is applied to a {{site.data.keyword.conversationshort}} service instance from the IBM Cloud dashboard is used by IBM Cloud. However, the service access role assignments for users are ignored. Users with either Reader- or Writer-level access to a service instance effectively have Manager-level access. This behavior will change when the upcoming access control enhancements are released. When released, service-level access control will be enabled automatically in all service instances; you will not be able to disable it for individual instances.

When service access control is added to {{site.data.keyword.conversationshort}}, the service access role assignment will matter.
{: important}  

The service access roles will apply the following restrictions.

| Service Access Role | Privileges of the users in this role |
|------|---------------------------------------|
| Reader | Open and read an assistant or skill, but not edit it. |
| Writer | Read, edit, create, and delete an assistant or skill, but cannot view analytics. |
| Manager | Read, edit, create, and delete an assistant or skill, and view analytics. |
{: caption="Table 3. Service access role details" caption-side="top"}

## How to keep your access
{: #access-control-prep}

If you have a Manager service access role to a {{site.data.keyword.conversationshort}} service, then no action is required. You will have the same privileges and can do the same actions within the product after service-level access control is enabled.

If you have a Reader or Writer service access role, then what you can do will change when service-level access control is released.

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
    For instructions, see https://cloud.ibm.com/docs/services/assistant?topic=assistant-access-control-pre#access-control-admin-prep.
    ```
    {: codeblock}
