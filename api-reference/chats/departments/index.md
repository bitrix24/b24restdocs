# Departments in Chats: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The methods `im.department.*` retrieve information about users in departments from the company's structure. They help you collect the composition of a department and find managers and colleagues in order to add these people to a chat or send them a notification. The subsection is part of the [Chats in Bitrix24](../index.md) section.

The `im.department.*` methods are current and have no `im.v2` counterparts or deprecated variants.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Company Structure: New Interface and Features](https://helpdesk.bitrix24.com/open/23583540/)

## Relationship of Methods with Other Objects

**Department.** The methods obtain information about users by the department identifier `ID`. You can get the department identifier using the [get department list method](../../departments/department-get.md) from the [Company Structure](../../departments/index.md) section or the [search departments by name method](../search/im-search-department-list.md).

**User.** The methods return employee identifiers. These identifiers are suitable for the chat methods: they can be passed in `USERS` of the [im.chat.user.add](../chat-users/im-chat-user-add.md) method or in `USER_ID` of the [im.notify](../notifications/im-notify.md) method. Detailed messenger user data is returned by the methods of the [Users in Chats](../users/index.md) subsection.

## How to Choose a Method

#|
|| **If You Need To** | **Method** ||
|| The department name and the identifier of its manager | [im.department.get](./im-department-get.md) ||
|| The managers of the specified departments | [im.department.managers.get](./im-department-managers-get.md) ||
|| The employees of the specified departments | [im.department.employees.get](./im-department-employees-get.md) ||
|| The colleagues of the current user, and for a manager — subordinates and all managers | [im.department.colleagues.list](./im-department-colleagues-list.md) ||
|#

## Specifics of the Methods

- by default, the methods return only user identifiers, and [im.department.get](./im-department-get.md) — the fields of the department itself. To add detailed user data to the response, pass `USER_DATA` with the value `Y`
- pagination is available only for the [im.department.colleagues.list](./im-department-colleagues-list.md) method: it is set by the `OFFSET` and `LIMIT` parameters, and the response contains `total` and `next`. The other methods return the whole result at once
- the methods work only for company employees: they are not available to extranet users and bots

## Overview of Methods {#all-methods}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any intranet user, except for bots

#|
|| **Method** | **Description** ||
|| [im.department.get](./im-department-get.md) | Retrieves information about the department ||
|| [im.department.managers.get](./im-department-managers-get.md) | Retrieves a list of department managers ||
|| [im.department.employees.get](./im-department-employees-get.md) | Retrieves a list of department employees ||
|| [im.department.colleagues.list](./im-department-colleagues-list.md) | Retrieves a list of colleagues of the current user ||
|#