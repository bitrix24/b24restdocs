# Users: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The user profile is a key object in Bitrix24. Without a user profile, an employee cannot utilize the corporate account and CRM. The user ID can be used to:
- configure access permissions,
- assign tasks among employees,
- retrieve information about employee actions.

> Quick navigation: [all methods](#all-methods)
> 
> User documentation: [How to invite employees to Bitrix24](https://helpdesk.bitrix24.com/open/21408738/)

## How to Work with Users

Use the method:

- [user.add](./user-add.md) to invite new users. The username must not exceed 25 characters.
- [user.update](./user-update.md) to edit existing user profiles, terminate or reinstate an employee.

{% note tip "User Documentation" %}

- [How to terminate an employee in Bitrix24](https://helpdesk.bitrix24.com/open/25785571/)

{% endnote %}

## Connection with Other Objects

**CRM.** The user `ID` is used in system fields:

- editable: responsible person, observers,
- read-only: who created, who modified, who moved to the stage.

Use the methods [crm.item.*](../crm/universal/index.md) to read or modify fields.

**Calendar.** To work with an employee's calendar, specify the user `ID` in the `ownerID` field in the methods [calendar.section.*](../calendar/index.md). To create or modify a meeting with multiple participants, specify the user `IDs` in the `host` and `attendees` fields in the methods [calendar.event.*](../calendar/calendar-event/index.md).

**News Feed.** Use user `IDs` in the methods:

- [log.blogpost.*](../log/index.md) to address a post to specific employees,
- [log.blogpost.getusers.important](../log/log-blogpost-getusers-important.md) to check who among the employees has read an important post.

**Online Store.** The user `ID` is used in system fields:

- editable: responsible for the order, shipment, payment,
- read-only: who changed the status, who marked, who canceled, who processed a return.

Use the methods [sale.order.*](../sale/order/index.md), [sale.payment.*](../sale/payment/index.md), [sale.shipment.*](../sale/shipment/index.md) to read or modify fields.

**Chats.** Use user `IDs` in the methods [im.chat.*](../chats/index.md), [im.chat.user.*](../chats/chat-users/index.md), [im.user.*](../chats/users/index.md) to assign a chat owner, edit the list of participants, or retrieve information about chat participants.

**Open Channels.** Use user `IDs`:

- to configure the queue of responsible employees in the methods [imopenlines.config.*](../imopenlines/openlines/index.md),
- to assign dialogues to employees in the methods [imopenlines.bot.session.*](../imopenlines/openlines/chat-bots/index.md), [imopenlines.crm.chat.user.*](../imopenlines/openlines/chats/index.md), [imopenlines.operator.*](../imopenlines/openlines/operators/index.md).

**Telephony.** Use the user `ID`:

- to route a phone call to them in the methods [telephony.externalcall.*](../telephony/index.md),
- to obtain call statistics for the employee using the method [voximplant.statistic.get](../telephony/voximplant/voximplant-statistic-get.md),
- to configure a SIP device in the methods [voximplant.user.*](../telephony/voximplant/users/index.md).

**Time Tracking.** Use user `IDs`:

- to manage the user's workday in the methods [timeman.*](../timeman/base/index.md),
- to configure the tool for time control in the methods [timeman.timecontrol.*](../timeman/timecontrol/index.md),
- to retrieve information about the work schedule using the method [timeman.schedule.get](../timeman/schedule/index.md).

**Tasks.** The user `ID` is used in system fields:

- editable: Creator, executor, Participants, observers,
- read-only: who modified the task, who changed the status, who completed the task.

Use the methods [tasks.task.*](../tasks/index.md) to read or modify fields. To find out or edit the time spent on a task for a user — [task.elapseditem.*](../tasks/elapsed-item/index.md).

**Workgroups and Projects.** Use user `IDs` in the methods [sonet_group.user.*](../sonet-group/members/index.md) to change the composition of group members and their roles. Specify the user `ID` in the `SCRUM_MASTER_ID` field to create a [Scrum](../sonet-group/scrum/index.md) group.

**Company Structure.** Use user `IDs` to manage the assignment of employees to company departments in the methods [department.*](../departments/index.md).

### User Binding Type

In Bitrix24, you can create custom fields with the type "user binding." Fields accept a user `ID` if they are simple, and an array of `IDs` if they are multiple.

- Custom fields are available in the sections [CRM](../crm/index.md) and [online store](../sale/index.md).
- Custom properties are available in the sections [universal lists](../lists/index.md), [data storage](../entity/index.md), and in the [product catalog](../catalog/index.md).
- Parameters and variables are available in [workflows](../bizproc/index.md).

### User Access Rights

Use the user `ID` to configure access rights in the methods:

- Drive file methods [disk.folder.uploadfile](../disk/folder/disk-folder-upload-file.md) and [disk.storage.uploadfile](../disk/storage/disk-storage-upload-file.md),
- Document generator [documentgenerator.role.fillaccesses](../document-generator/role/document-generator-role-fill-accesses.md),
- Sites [landing.site.setRights](../landing/rights/role-model/landing-role-set-rights.md).

## Working with Extranet Users

Extranet users are those who have limited access to Bitrix24 functionality. CRM is not available for extranet users, but chats, groups, and the calendar are accessible.

{% note tip "User Documentation" %}

- [Extranet Users in Bitrix24](https://helpdesk.bitrix24.com/open/18070866/)

{% endnote %}

To invite an extranet user, use the method [user.add](./user-add.md) with the parameters:

- `EXTRANET: Y` — marking as an external user,
- `SONET_GROUP_ID: [...]` — `IDs` of the groups the user will belong to.

When adding an extranet user to a group using the method [sonet_group.user.add](../sonet-group/members/sonet-group-user-add.md), the group automatically changes its type to external.

## **Widgets**

An application can be embedded in the user profile. This allows the application to be used without leaving the user detail form.

- [Context menu item in the profile](../widgets/user-profile/profile-menu.md) `USER_PROFILE_MENU`

- [Context menu item for the top profile button](../widgets/user-profile/profile-toolbar.md) `USER_PROFILE_TOOLBAR`

{% note tip " " %}

- [Widget embedding mechanism](../widgets/index.md)

{% endnote %}

## User Data Access Levels

To ensure the security of employee data, different versions of the User scope are available for applications and webhooks.

- `user_brief` provides access to user information without contact details. This is sufficient for scenarios where displaying the user's full name in a third-party application interface is required.

- `user_basic` opens basic information and contact details of users. This is required for scenarios related to making calls or sending e-mail messages.

- `user` provides full access to user information, the ability to invite new users, and modify existing data.

{% note tip " " %}

- [User Scope Versions](user-scope.md)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`user`](../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#| 
|| **Method** | **Description** ||
|| [user.add](user-add.md) | Invites a user ||
|| [user.update](user-update.md) | Updates user data ||
|| [user.current](user-current.md) | Retrieves information about the current user ||
|| [user.get](user-get.md) | Retrieves a filtered list of users ||
|| [user.search](user-search.md) | Retrieves a list of users with accelerated search by personal data ||
|| [user.fields](user-fields.md) | Retrieves a list of user profile fields ||
|#