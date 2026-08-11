# Users in Chats: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

To work with users in chats, use the group of methods `im.user.*`. They return the data the way the messenger shows it: the display name, avatar, color, status in the chat, the time of the last activity, and the flags of a bot, an external user, or an Open Channel user. The subsection is part of the [Chats in Bitrix24](../index.md) section.

The `im.user.*` methods are current and have no `im.v2` counterparts. The exception is the automatic *Away* status methods — they are placed in a separate group, [Methods for the Previous Version of the Chat](#legacy-methods).

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Chats in Bitrix24: Interface and Capabilities](https://helpdesk.bitrix24.com/open/21924784/)

## How to Choose a Method

#|
|| **If You Need To** | **Method** ||
|| The data of the current user or of one employee | [im.user.get](./im-user-get.md) ||
|| The data of several users at once | [im.user.list.get](./im-user-list-get.md) ||
|| Set your own status in the messenger | [im.user.status.set](./im-user-status-set.md) ||
|| Read your own status | [im.user.status.get](./im-user-status-get.md) ||
|| Find an employee by name, position, or department | [im.search.user.list](../search/im-search-user-list.md) from the [Search](../search/index.md) subsection ||
|| Retrieve the participants of a specific chat with their data | [im.dialog.users.list](../chat-users/im-dialog-users-list.md) from the [Chat Participants](../chat-users/index.md) subsection ||
|| The full employee profile: contacts, departments, custom fields | The methods of the [Users](../../user/index.md) section ||
|#

## Relationship of Methods with Other Objects

**User.** The methods retrieve information about users by the identifier `ID`. You can find the chat participant identifiers using the [im.chat.user.list](../chat-users/im-chat-user-list.md) method. The same identifier is returned by the [user.get](../../user/user-get.md) and [user.search](../../user/user-search.md) methods: in Bitrix24, a user has a single identifier for all methods.

**Departments.** You can retrieve the identifiers of employees and managers by the company structure with the methods of the [Departments](../departments/index.md) subsection.

**Extranet.** An extranet user has access only to the members of their extranet groups. In this case, the [im.user.get](./im-user-get.md) method returns the `ACCESS_DENIED` error, while [im.user.list.get](./im-user-list-get.md) silently skips the unavailable identifiers.

## User Status in Chats

The user status controls notifications and is displayed in the messenger. The [im.user.status.set](./im-user-status-set.md) and [im.user.status.get](./im-user-status-get.md) methods work only for the current user: you cannot set or read the status of another employee.

The [im.user.status.set](./im-user-status-set.md) method accepts four values: `online`, `dnd`, `away`, and `break`. In the interface of the current chat version, only `online` is displayed — the other statuses are set but not shown.

{% note tip "User Documentation" %}

- [Notifications in Bitrix24](https://helpdesk.bitrix24.com/open/19423886/)

{% endnote %}

## Overview of Methods {#all-methods}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [im.user.get](./im-user-get.md) | Retrieves user data ||
|| [im.user.list.get](./im-user-list-get.md) | Retrieves data about the list of users ||
|| [im.user.status.set](./im-user-status-set.md) | Sets the user's status in the chat ||
|| [im.user.status.get](./im-user-status-get.md) | Retrieves the user's set status ||
|#

### Methods for the Previous Version of the Chat {#legacy-methods}

The automatic *Away* status methods were designed for the previous version of the chat. In the current version of chat M1, they work, but the results are not displayed in the interface. They have no replacement in the current version and are not suitable for new development.

{% note tip "User Documentation" %}

- [Bitrix24 Chat: New Messenger](https://helpdesk.bitrix24.com/open/25661218/)

{% endnote %}

#|
|| **Method** | **Description** ||
|| [im.user.status.idle.start](./im-user-status-idle-start.md) | Sets the automatic status *Away* ||
|| [im.user.status.idle.end](./im-user-status-idle-end.md) | Disables the automatic status *Away* ||
|#