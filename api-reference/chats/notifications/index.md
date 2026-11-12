# Notifications in Chats: Overview of Methods

{% note tip "" %}

Choose a tool for developing with an AI agent:

- use [Alaio Vibecode](../../../ai-tools/vibecode.md) to build an app for Bitrix24 from a task description without knowing any programming language. The agent writes the code and deploys the app to a server, with no manual hosting setup
- use the [MCP server](../../../ai-tools/mcp.md) to develop a REST API integration in your own project. The agent refers to the official REST documentation

{% endnote %}

A chat notification is a message containing information from the system or a user. It arrives in the messenger notification center, not in the chat conversation. The group of methods `im.notify.*` manages notifications. The subsection is part of the [Chats in Bitrix24](../index.md) section.

The `im.notify.*` methods are current and have no deprecated variants.

{% note warning "" %}

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
3. Retain the notification identifier from the `result` field of the response — it is needed for the read and delete methods. If the recipient is not found or is inactive, `result` contains `false`
4. Manage the state of the notifications with the [im.notify.read](./im-notify-read.md), [im.notify.read.list](./im-notify-read-list.md), [im.notify.read.all](./im-notify-read-all.md), and [im.notify.delete](./im-notify-delete.md) methods

## Linking Notifications to Other Objects

**User.** A notification goes to the user specified in `USER_ID`. You can obtain the user identifier using the [user.get](../../user/user-get.md) method.

**Application.** The `TAG` and `SUB_TAG` tags are retained only when an application calls the sending method. When the method is called via a webhook, the tags are ignored without an error. The `TAG` tag is unique within the application and the recipient: a new notification with the same `TAG` deletes the previous one for that recipient. `SUB_TAG` is an auxiliary tag without a uniqueness check.

**Bitrix24 modules.** Notifications with a quick reply or with buttons are created by modules — for example, by the calendar when inviting a user to an event. The `im.notify.*` sending methods do not create such notifications, so [im.notify.answer](./im-notify-answer.md) and [im.notify.confirm](./im-notify-confirm.md) work only with notifications from modules.

**Attachments.** Structured content can be attached to a notification in the `ATTACH` parameter — the format is described in the [Attachments](../messages/attachments.md) article.

**Search.** The [im.notify.history.search](./im-notify-history-search.md) method helps you find a notification in the history, and the search across chats, employees, and departments is collected in the [Search](../search/index.md) subsection.

## How to Choose a Method

#|
|| **Task** | **Method** ||
|| Send a notification | [im.notify](./im-notify.md) — one method for both types, the type is set by the `TYPE` parameter. The separate methods [im.notify.personal.add](./im-notify-personal-add.md) and [im.notify.system.add](./im-notify-system-add.md) solve the same tasks ||
|| Retrieve notifications and types | [im.notify.get](./im-notify-get.md), [im.notify.schema.get](./im-notify-schema-get.md) ||
|| Manage the read status | [im.notify.read](./im-notify-read.md) — a notification and all newer ones, or only one with `ONLY_CURRENT = Y`; [im.notify.read.list](./im-notify-read-list.md) — a list; [im.notify.read.all](./im-notify-read-all.md) — all ||
|| Reply or press a button | [im.notify.answer](./im-notify-answer.md), [im.notify.confirm](./im-notify-confirm.md). After a button is pressed, the notification is deleted ||
|| Delete or find in the history | [im.notify.delete](./im-notify-delete.md) — by `ID`, deletes the current user's notifications, by `TAG`, deletes them for all recipients; [im.notify.history.search](./im-notify-history-search.md) — search ||
|#

## Method Responses and Pagination

- the sending methods return the ID of the created notification in the root `result` field of type `integer`, for example `"result": 43999`. The same ID is returned in the `id` field of the notification in the [im.notify.get](./im-notify-get.md) response and is accepted in the `ID` parameter of the [im.notify.delete](./im-notify-delete.md) method
- [im.notify.get](./im-notify-get.md) returns notifications in pages of `LIMIT` items, up to 50. Request the first page without a cursor. For the next page, pass `LAST_ID` — the `id` of the last element of `result.notifications` — together with `LAST_TYPE` — `1` if that element has `notify_type = 1`, otherwise `3`. The selection is complete when fewer notifications than `LIMIT` are returned. For details, see [Pagination](./im-notify-get.md#pagination)
- [im.notify.history.search](./im-notify-history-search.md) is paged by `LAST_ID` only, with the same `LIMIT` restriction. The `total_results` field is returned only in the response for the first page
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
|| [im.notify.confirm](./im-notify-confirm.md) | Presses a button in a notification ||
|| [im.notify.delete](./im-notify-delete.md) | Deletes notifications ||
|| [im.notify.history.search](./im-notify-history-search.md) | Searches through notification history ||
|#