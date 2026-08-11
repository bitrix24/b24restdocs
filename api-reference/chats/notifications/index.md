# Notifications in Chats: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A chat notification is a message containing information from the system or a user. It arrives in the messenger notification center, not in the chat conversation. The group of methods `im.notify.*` manages notifications. The subsection is part of the [Chats in Bitrix24](../index.md) section.

The `im.notify.*` methods are current and have no deprecated variants.

{% note warning %}

The sending methods [im.notify](./im-notify.md), [im.notify.personal.add](./im-notify-personal-add.md), and [im.notify.system.add](./im-notify-system-add.md) cannot be called with session authorization — they return the `WRONG_AUTH_TYPE` error. Call them via a webhook or with an application token.

{% endnote %}

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Notifications in Bitrix24](https://helpdesk.bitrix24.com/open/19423886/)

## Personal and System Notification

Notifications come in two types, and the type determines on whose behalf the message arrives:

- a personal notification arrives on behalf of the user who called the method. The `USER` type in the `TYPE` parameter, which is also used by default
- a system notification arrives on behalf of the system, without an author. The `SYSTEM` type

## Notification or Message

#|
|| **Task** | **What to Use** ||
|| Inform a user about an event in the integration without creating a conversation | The `im.notify.*` methods of this subsection ||
|| Write to a dialogue or a group chat | [im.message.add](../messages/im-message-add.md) from the [Messages](../messages/index.md) subsection ||
|| Find out the number of unread messages and notifications | [im.counters.get](../im-counters-get.md) ||
|#

## How to Get Started

1. Retrieve the recipient identifier with the [user.get](../../user/user-get.md) method
2. Send the notification with the [im.notify](./im-notify.md) method and choose the type in the `TYPE` parameter
3. Retain the notification identifier from the response — it is needed for the read, reply, and delete methods
4. Manage the state of the notifications with the [im.notify.read](./im-notify-read.md), [im.notify.read.list](./im-notify-read-list.md), and [im.notify.delete](./im-notify-delete.md) methods

## Linking Notifications to Other Objects

**User.** A notification goes to the user specified in `USER_ID`. You can obtain the user identifier using the [user.get](../../user/user-get.md) method.

**Application.** Notification tags are bound to the application. The `TAG` tag is unique within it: if a new notification is sent with the same `TAG`, the system removes the previous one. `SUB_TAG` is an auxiliary tag without a uniqueness check. When calling via a webhook, both tags are passed together with `CLIENT_ID`.

**Attachments.** Structured content can be attached to a notification in the `ATTACH` parameter — the format is described in the [Attachments](../messages/attachments.md) article.

**Search.** The [im.notify.history.search](./im-notify-history-search.md) method helps you find a notification in the history, and the search across chats, employees, and departments is collected in the [Search](../search/index.md) subsection.

## How to Choose a Method

#|
|| **Task** | **Method** ||
|| Send a notification | [im.notify](./im-notify.md) — one method for both types, the type is set by the `TYPE` parameter. The separate methods [im.notify.personal.add](./im-notify-personal-add.md) and [im.notify.system.add](./im-notify-system-add.md) solve the same tasks ||
|| Retrieve notifications and types | [im.notify.get](./im-notify-get.md), [im.notify.schema.get](./im-notify-schema-get.md) ||
|| Manage the read status | [im.notify.read](./im-notify-read.md) — a single notification, [im.notify.read.list](./im-notify-read-list.md) — a list, [im.notify.read.all](./im-notify-read-all.md) — all ||
|| Reply or press a button | [im.notify.answer](./im-notify-answer.md), [im.notify.confirm](./im-notify-confirm.md) ||
|| Delete or find in the history | [im.notify.delete](./im-notify-delete.md), [im.notify.history.search](./im-notify-history-search.md) ||
|#

## Limits and Pagination

- the [im.notify.get](./im-notify-get.md) and [im.notify.history.search](./im-notify-history-search.md) methods return the selection page by page using a cursor, with a maximum of 50 notifications per page
- the size of the serialized `ATTACH` attachment is limited to 60,000 characters

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
|| [im.notify.read.list](./im-notify-read-list.md) | Marks a list of notifications as read ||
|| [im.notify.read](./im-notify-read.md) | Marks a notification as read or returns it to unread ||
|| [im.notify.read.all](./im-notify-read-all.md) | Marks all notifications as read ||
|| [im.notify.answer](./im-notify-answer.md) | Replies to a notification with a quick response ||
|| [im.notify.confirm](./im-notify-confirm.md) | Interacts with notification buttons ||
|| [im.notify.delete](./im-notify-delete.md) | Deletes notifications ||
|| [im.notify.history.search](./im-notify-history-search.md) | Searches through notification history ||
|#