# Users: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

The user profile is a key object in Bitrix24. Without a profile, an employee cannot work in Bitrix24 and CRM. The user ID can be used to:

- configure access permissions,
- assign tasks among employees,
- retrieve information about employee actions.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [How to invite employees to Bitrix24](https://helpdesk.bitrix24.com/open/21408738/)

## How to Work with Users

1. Find out which profile fields are available to the application using the [user.fields](./user-fields.md) method. The set of fields depends on the [scope version](user-scope.md).
2. Find a user and their `ID`: by filter using the [user.get](./user-get.md) method, or by name, position, or department using the [user.search](./user-search.md) method. To retrieve the profile of the user on whose behalf the request is executed, use [user.current](./user-current.md).
3. Invite a new employee using the [user.add](./user-add.md) method. For an employee, pass the `UF_DEPARTMENT` department; for an external user, pass `EXTRANET` and `SONET_GROUP_ID`.
4. Update the profile using the [user.update](./user-update.md) method.
5. To terminate an employee, pass the `ACTIVE` field with the `N` value to [user.update](./user-update.md). To reinstate them, pass `Y`.

The `user.get` and `user.search` methods return up to 50 users per call. To retrieve the next page, pass `start` = `(N-1) * 50`.

{% note tip "User Documentation" %}

- [How to terminate an employee in Bitrix24](https://helpdesk.bitrix24.com/open/25785571/)

{% endnote %}

## Custom Fields of the Profile

You can add your own fields to an employee profile, for example, a list of skills or a hire date. The fields are managed by the [user.userfield.*](./userfields/index.md) methods — they work in the `user.userfield` scope and can be executed only by an administrator.

- The code of a new field is supplemented with the `UF_USR_` prefix, for example, `UF_USR_SKILLS`.
- The field value is written using the [user.update](./user-update.md) method — pass the field code together with the user `ID`.
- The values are returned by the [user.get](./user-get.md) and [user.search](./user-search.md) methods. To retrieve all custom fields, pass the `UF_*` mask in the `select` parameter.

## Relationships with Other Objects

The user `ID` from [user.get](./user-get.md) or [user.search](./user-search.md) is passed to fields and parameters of methods in other sections.

**CRM.** The user `ID` is stored in the responsible person and observer fields, which can be modified. The "created by", "modified by", and "moved to stage by" fields are read-only. Read and modify the fields using the [crm.item.*](../crm/universal/index.md) methods.

**Tasks.** The `ID` is specified in the creator, assignee, participant, and observer fields of the [tasks.task.*](../tasks/index.md) methods. The time spent by an employee is tracked by the [task.elapseditem.*](../tasks/elapsed-item/index.md) methods.

**Calendar.** An employee's calendar is selected by the `ID` in the `ownerID` field of the [calendar.section.*](../calendar/index.md) methods. The meeting host and attendees are passed in the `host` and `attendees` fields of the [calendar.event.*](../calendar/calendar-event/index.md) methods.

**Chats.** User `IDs` set the chat owner and participants in the [im.chat.*](../chats/index.md), [im.chat.user.*](../chats/chat-users/index.md), and [im.user.*](../chats/users/index.md) methods.

**Company Structure.** An employee's departments are stored in the `UF_DEPARTMENT` profile field. Departments and the assignment of employees to them are managed by the [department.*](../departments/index.md) methods.

**Workgroups and Projects.** Group membership and member roles are changed by the [sonet_group.user.*](../sonet-group/members/index.md) methods. To turn a group into a [Scrum](../sonet-group/scrum/index.md), pass the user `ID` in the `SCRUM_MASTER_ID` field.

**Other Sections.** The user `ID` is also used in:
- [news feed](../log/index.md) — post recipients and read tracking with the [log.blogpost.getusers.important](../log/log-blogpost-getusers-important.md) method
- [open channels](../imopenlines/openlines/index.md) — the operator queue and dialogue assignment in the [imopenlines.bot.session.*](../imopenlines/openlines/chat-bots/index.md), [imopenlines.crm.chat.user.*](../imopenlines/openlines/chats/index.md), and [imopenlines.operator.*](../imopenlines/openlines/operators/index.md) methods
- [telephony](../telephony/index.md) — the call recipient, statistics with the [voximplant.statistic.get](../telephony/voximplant/voximplant-statistic-get.md) method, and the SIP device in the [voximplant.user.*](../telephony/voximplant/users/index.md) methods
- [mail](../mail/index.md) — the methods work with the current user's mailboxes, recipients are found by [mail.recipient.listemployees](../mail/recipient/mail-recipient-listemployees.md), and scenarios are collected in the [Mail: Common Use Cases](../../tutorials/mail/index.md) section
- [online store](../sale/order/index.md) — the person responsible for the order, [payment](../sale/payment/index.md), and [shipment](../sale/shipment/index.md)
- [time tracking](../timeman/base/index.md) — the workday, [time control](../timeman/timecontrol/index.md), and [work schedule](../timeman/schedule/index.md)

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

Extranet users are external participants with limited access to Bitrix24. CRM is not available to them, but chats, groups, and the calendar are accessible.

{% note tip "User Documentation" %}

- [Extranet Users in Bitrix24](https://helpdesk.bitrix24.com/open/18070866/)

{% endnote %}

To invite an extranet user, use the method [user.add](./user-add.md) with the parameters:

- `EXTRANET: Y` — marking as an external user,
- `SONET_GROUP_ID: [...]` — `IDs` of the groups the user will belong to.

When adding an extranet user to a group using the method [sonet_group.user.add](../sonet-group/members/sonet-group-user-add.md), the group automatically changes its type to external.

## Widgets

An application can be embedded in the user profile — then you can work with it without leaving the employee detail form.

- [Context menu item in the profile](../widgets/user-profile/profile-menu.md) `USER_PROFILE_MENU`
- [Context menu item for the top profile button](../widgets/user-profile/profile-toolbar.md) `USER_PROFILE_TOOLBAR`

{% note tip " " %}

- [Widget Embedding Mechanism](../widgets/index.md)

{% endnote %}

## User Data Access Levels

To ensure the security of employee data, three versions of the `user` scope are available for applications and webhooks. The version determines the set of profile fields that the methods return.

- `user_brief` provides access to names, positions, photos, and other data, excluding e-mail addresses, phone numbers, and postal addresses.
- `user_basic` additionally opens the contact details of employees.
- `user` provides access to all profile fields, the ability to invite new employees, and modify existing data.

{% note tip " " %}

- [User Scope Versions](user-scope.md)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`user`](../scopes/permissions.md), [`user_basic`](../scopes/permissions.md), [`user_brief`](../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#| 
|| **Method** | **Description** ||
|| [user.add](user-add.md) | Invites a user ||
|| [user.update](user-update.md) | Updates user data ||
|| [user.get](user-get.md) | Retrieves a filtered list of users ||
|| [user.search](user-search.md) | Retrieves a list of users with accelerated search by personal data ||
|| [user.current](user-current.md) | Retrieves information about the current user ||
|| [user.fields](user-fields.md) | Retrieves a list of user profile fields ||
|#

### Related Methods and Events

#|
|| **Method or Event** | **Description** ||
|| [user.admin](../common/users/user-admin.md) | Checks whether the current user is an administrator ||
|| [user.access](../common/users/user-access.md) | Checks whether the current user has at least one of the passed access codes ||
|| [user.option.get](../common/settings/user-option-get.md), [user.option.set](../common/settings/user-option-set.md) | Retrieve and retain application configurations for the current user ||
|| [onUserAdd](../common/events/on-user-add.md) | The event on adding a user. Fires after the invited user completes registration ||
|#

The methods for working with custom profile fields are collected in the [Custom Fields](./userfields/index.md) subsection.
