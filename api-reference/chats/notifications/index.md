# Notifications in Chats: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A chat notification is a message containing information from the system or a user. The group of methods `im.notify.*` manages notifications.

> Quick Navigation: [All Methods and Events](#all-methods)  
> User Documentation: [Notifications in Bitrix24](https://helpdesk.bitrix24.com/open/19423886/)

## Linking Notifications to Other Objects

**User.** A notification is sent to a user by their `USER_ID`. You can obtain the user ID using the [user.get](../../user/user-get.md) method.

## Send Notification

You can send a message with information to the messenger, which will appear in the notifications list.

- **Universal Method.** The [im.notify](./im-notify.md) method sends a personal or system notification via the `TYPE` parameter.

- **Personal Notification.** An employee will receive a notification on behalf of the user who initiated the method. The [im.notify.personal.add](./im-notify-personal-add.md) method sends a personal notification.

- **System Notification.** The [im.notify.system.add](./im-notify-system-add.md) method sends a notification from the system.

Each notification can be associated with a unique tag `TAG`. If a new notification is sent with the same tag, the system will remove the previous notification.

All sending methods return the notification ID, which can be used in other methods.

## Get Notifications

The [im.notify.get](./im-notify-get.md) method returns the current notifications with pagination.

The [im.notify.schema.get](./im-notify-schema-get.md) method returns the available notification types by modules.

## Read Notifications

The [im.notify.read.list](./im-notify-read-list.md) method manages the Read label for the list of notifications, except for the `CONFIRM` type with confirmation buttons. Using the `ACTION` parameter, you can mark notifications as read `Y` or unread `N`.

The [im.notify.read](./im-notify-read.md) method reads a single notification or all notifications after a specified one. The `ONLY_CURRENT` parameter defines the scope:

- `Y` — the Read label will be set only for the specified notification,

- `N` or absence of the parameter — for all notifications with `ID` equal to or greater than the specified one.

The [im.notify.read.all](./im-notify-read-all.md) method marks all notifications of the current user as read.

## Delete Notification

The [im.notify.delete](./im-notify-delete.md) method deletes a notification by its `ID` or by tags `TAG`, `SUB_TAG`.

## Reply to Notification

You can reply to a notification that supports quick responses using the [im.notify.answer](./im-notify-answer.md) method.

You can interact with buttons in the notification using the [im.notify.confirm](./im-notify-confirm.md) method. The `NOTIFY_VALUE` parameter should contain the button value. For example, for a meeting invitation notification, the Accept button has a value of `Y`, and the Decline button has a value of `N`.

## Search Notification History

The [im.notify.history.search](./im-notify-history-search.md) method performs a search through the notification history with filters for text, type, date, and group tag.

## Overview of Methods {#all-methods}

> Scope: [`im`](../../scopes/permissions.md)  
> Who can execute the method: any user

#|
|| **Method** | **Description** ||
|| [im.notify](./im-notify.md) | Sends a notification ||
|| [im.notify.personal.add](./im-notify-personal-add.md) | Sends a personal notification ||
|| [im.notify.system.add](./im-notify-system-add.md) | Sends a system notification ||
|| [im.notify.get](./im-notify-get.md) | Returns user notifications ||
|| [im.notify.schema.get](./im-notify-schema-get.md) | Returns the notification types schema ||
|| [im.notify.read.list](./im-notify-read-list.md) | Reads the list of notifications ||
|| [im.notify.read](./im-notify-read.md) | Reads a notification or returns it as unread ||
|| [im.notify.read.all](./im-notify-read-all.md) | Reads all notifications ||
|| [im.notify.answer](./im-notify-answer.md) | Replies to a notification that supports quick response ||
|| [im.notify.confirm](./im-notify-confirm.md) | Interacts with notification buttons ||
|| [im.notify.delete](./im-notify-delete.md) | Deletes notifications ||
|| [im.notify.history.search](./im-notify-history-search.md) | Searches through notification history ||
|#