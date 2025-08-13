# Users: Overview of Methods

The user profile is a key object in Bitrix24. Without a user profile, an employee cannot use the corporate account and CRM. The user ID can be used to:
- configure access permissions,
- assign tasks among employees,
- obtain information about employee actions.

> Quick navigation: [all methods](#all-methods)
> 
> User documentation: [How to invite employees to Bitrix24](https://helpdesk.bitrix24.com/open/21408738/)

## How to Work with Users

Use the method:

- [user.add](./user-add.md) to invite new users. The username must not exceed 25 characters.
- [user.update](./user-update.md) to edit existing user profiles.

You cannot terminate a user using the `user.*` methods. Termination is only available in the Bitrix24 interface.

{% note tip "User Documentation" %}

- [How to dismiss an employee in Bitrix24](https://helpdesk.bitrix24.com/open/25785571/)

{% endnote %}

## Connection with Other Objects

**CRM.** The user `ID` is used in system fields:

- editable: responsible person, observers,
- read-only: who created, who modified, who moved to the stage.

Use the [crm.item.*](../crm/universal/index.md) methods to read or modify fields.

**Calendar.** To work with an employee's calendar, specify the user `ID` in the `ownerID` field in the [calendar.section.*](../calendar/index.md) methods. To create or modify a meeting with multiple participants, specify the user `IDs` in the `host` and `attendees` fields in the [calendar.event.*](../calendar/calendar-event/index.md) methods.

**News Feed.** Use user `IDs` in the methods:

- [log.blogpost.*](../log/index.md) to address a post to specific employees,
- [log.blogpost.getusers.important](../log/log-blogpost-getusers-important.md) to check who among employees has read an important post.

**Online Store.** The user `ID` is used in system fields:

- editable: responsible for the order, shipment, payment,
- read-only: who changed the status, who marked, who canceled, who processed a return.

Use the [sale.order.*](../sale/order/index.md), [sale.payment.*](../sale/payment/index.md), [sale.shipment.*](../sale/shipment/index.md) methods to read or modify fields.

**Chats.** Use user `IDs` in the methods [im.chat.*](../chats/index.md), [im.chat.user.*](../chats/chat-users/index.md), [im.user.*](../chats/users/index.md) to assign a chat owner, edit the list of participants, or obtain information about chat participants.

**Open Channels.** Use user `IDs`:

- to configure the queue of responsible employees in the [imopenlines.config.*](../imopenlines/openlines/index.md) methods,
- to assign dialogues to employees in the [imopenlines.bot.session.*](../imopenlines/openlines/chat-bots/index.md), [imopenlines.crm.chat.user.*](../imopenlines/openlines/chats/index.md), [imopenlines.operator.*](../imopenlines/openlines/operators/index.md) methods.

**Telephony.** Use the user `ID`:

- to direct a phone call to them in the [telephony.externalcall.*](../telephony/index.md) methods,
- to obtain call statistics for the employee using the [voximplant.statistic.get](../telephony/voximplant-statistic-get.md) method,
- to configure SIP devices in the [voximplant.user.*](../telephony/voximplant/users/index.md) methods.

**Time Tracking.** Use user `IDs`:

- to manage the user's workday in the [timeman.*](../timeman/base/index.md) methods,
- to configure the tool for tracking working hours in the [timeman.timecontrol.*](../timeman/timecontrol/index.md) methods,
- to obtain information about the work schedule using the [timeman.schedule.get](../timeman/schedule/index.md) method.

**Tasks.** The user `ID` is used in system fields:

- editable: creator, assignee, participants, observers,
- read-only: who changed the task, who changed the status, who completed the task.

Use the [tasks.task.*](../tasks/index.md) methods to read or modify fields. To find out or edit the time spent on a task for a user — [task.elapseditem.*](../tasks/elapsed-item/index.md).

**Workgroups and Projects.** Use user `IDs` in the [sonet_group.user.*](../sonet-group/members/index.md) methods to change the composition of group members and their roles. Specify the user `ID` in the `SCRUM_MASTER_ID` field to create a [Scrum](../sonet-group/scrum/index.md) group.

**Company Structure.** Use user `IDs` to manage the binding of employees to company departments in the [department.*](../departments/index.md) methods.

### User Binding Type

In Bitrix24, you can create custom fields with the type "user binding." Fields accept a user `ID` if they are simple and an array of `IDs` if they are multiple.

- Custom fields are available in the [CRM](../crm/index.md) and [online store](../sale/index.md) sections.
- Custom properties are available in the [universal lists](../lists/index.md), [data storage](../entity/index.md), and in the [product catalog](../catalog/index.md).
- Parameters and variables are available in [workflows](../bizproc/index.md).

### User Access Permissions

Use the user `ID` to configure access permissions in the methods:

- disk file [disk.folder.uploadfile](../disk/folder/disk-folder-upload-file.md) and [disk.storage.uploadfile](../disk/storage/disk-storage-upload-file.md),
- document generator [documentgenerator.role.fillaccesses](../document-generator/role/document-generator-role-fill-accesses.md),
- sites [landing.site.setRights](../landing/rights/role-model/landing-role-set-rights.md).

## Working with Extranet Users

Extranet users are users who have limited access to Bitrix24 functionality. CRM is not available for extranet users, but chats, groups, and calendar are accessible.

{% note tip "User Documentation" %}

- [Extranet users in Bitrix24](https://helpdesk.bitrix24.com/open/18070866/)

{% endnote %}

To invite an extranet user, use the [user.add](./user-add.md) method with the parameters:

- `EXTRANET: Y` — mark as an external user,
- `SONET_GROUP_ID: [...]` — `IDs` of groups the user will belong to.

When adding an extranet user to a group using the [sonet_group.user.add](../sonet-group/members/sonet-group-user-add.md) method, the group automatically changes its type to external.

## **Widgets**

You can embed an application in the user profile. With embedding, you can use the application without leaving the user card.

- [Context menu item in the profile](../widgets/user-profile/profile-menu.md) `USER_PROFILE_MENU`

- [Context menu item in the top profile button](../widgets/user-profile/profile-toolbar.md) `USER_PROFILE_TOOLBAR`

{% note tip " " %}

- [Widget embedding mechanism](../widgets/index.md)

{% endnote %}

## User Data Access Levels

To ensure the security of employee data, different versions of the User scope are available for applications and webhooks.

- `user_brief` provides access to user information without contact details. This is sufficient for scenarios where displaying the user's full name in a third-party application interface is required.

- `user_basic` opens basic information and contact details of users. This is required for scenarios related to making calls or sending e-mails.

- `user` provides full access to user information, the ability to invite new users, and modify existing data.

{% note tip " " %}

- [User Scope Versions](user-scope.md)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`user`](../scopes/permissions.md)
>
> Who can perform the method: depending on the method

#|
|| **Method** | **Description** ||
|| [user.add](user-add.md) | Invites a user ||
|| [user.update](user-update.md) | Updates user data ||
|| [user.current](user-current.md) | Retrieves information about the current user ||
|| [user.get](user-get.md) | Retrieves a filtered list of users ||
|| [user.search](user-search.md) | Retrieves a list of users with accelerated search by personal data ||
|| [user.fields](user-fields.md) | Retrieves a list of user profile fields ||
|#