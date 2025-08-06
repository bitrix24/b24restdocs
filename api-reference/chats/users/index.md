# Users in Chats: Overview of Methods

To work with users in chats, use the group of methods `im.user.*`.

> Quick navigation: [all methods](#all-methods)

## Relationship of Methods with Other Objects

**User.** The methods retrieve information about users by the identifier `ID`. You can find the chat user identifiers using the method [im.chat.user.list](../chat-users/im-chat-user-list.md).

## User Information

The method [im.user.get](./im-user-get.md) retrieves information about the current user or a specific user if the `ID` parameter is filled in.

To get information about multiple users, you can use the method [im.user.list.get](./im-user-list-get.md). In the `ID` parameter, pass an array with user identifiers, for example, `ID: [3,5,7]`.

## User Status in Chats

The method [im.user.status.set](./im-user-status-set.md) sets the user's status in the chat:

-  `online` — the user receives all notifications,

-  `dnd` — Do Not Disturb mode, the user does not receive notifications about new messages.

You can find out the status using the method [im.user.status.get](./im-user-status-get.md).

The methods work only for the current user.

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
|| [im.user.status.get](./im-user-status-get.md) | Retrieves information about the user's set status ||
|#

### Methods for the Previous Version of the Chat

The methods were designed for the previous version of the chat. In the current version of chat M1, they work, but the results are not displayed in the interface.

{% note tip "User Documentation" %}

- [Bitrix24 Chat: New Messenger](https://helpdesk.bitrix24.com/open/25661218/)

{% endnote %}

#|
|| **Method** | **Description** ||
|| [im.user.status.idle.start](./im-user-status-idle-start.md) | Sets the automatic status *Away* ||
|| [im.user.status.idle.end](./im-user-status-idle-end.md) | Disables the automatic status *Away* ||
|#