# Chats in Bitrix24: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

The chat feature in Bitrix24 helps to:
- communicate one-on-one
- discuss tasks in groups
- manage notifications, files, and messages within a single messenger interface

To work with chats, use the methods `im.*`. This section includes methods for managing chats and subsections for specific scenarios: participants, messages, notifications, search, files, and special operations.

For new developments, keep in mind that some scenarios have already been migrated to `im.v2`. This is particularly important for working with files and messenger events.

> Quick navigation: [all methods and events](#all-methods)
>
> User documentation: [Chats in Bitrix24: interface and capabilities](https://helpdesk.bitrix24.com/open/21924784/)

## Main Components of Chat

A chat in Bitrix24 can be viewed as an object composed of several blocks:

- dialog type and identifiers
- chat settings and design
- participants and roles
- messages and history
- files
- notifications and special actions

### Dialog Type and Identifiers

Group chats and personal dialogs differ by the identifier `DIALOG_ID`:

- `XXX` — personal chat with a user, where `XXX` is the user identifier
- `chatXXX` — group chat
- `sgXXX` — chat for a group or project

Group chats are created using the method [im.chat.add](./im-chat-add.md). In the `TYPE` parameter, you can choose:

- `CHAT` — closed chat
- `OPEN` — open chat

If you need to find an existing chat associated with an external object, use [im.chat.get](./im-chat-get.md). For chats linked to CRM, tasks, calendars, open channels, and other objects, use the parameters `ENTITY_TYPE` and `ENTITY_ID`.

### Chat Settings and Design

When creating a chat, you can specify the name, description, color, avatar, and initial message through [im.chat.add](./im-chat-add.md).

If you do not provide a `TITLE` when creating the chat, the system will generate a name automatically. In working scenarios, assign a clear name and, if necessary, link the chat to an external object.

After creating the chat, you can modify its design and owner using methods from the [Chat Update](./chat-update/index.md) section.

### Participants and Roles

The chat owner is automatically assigned upon chat creation. The user who invoked [im.chat.add](./im-chat-add.md) is added to the chat as the owner.

The owner identifier, the role of the current user, the list of managers, and action restrictions in the chat are returned by the methods [im.dialog.get](./im-dialog-get.md), [im.recent.get](./im-recent-get.md), and [im.recent.list](./im-recent-list.md).

To work with participants, use the methods from the [Chat Participants](./chat-users/index.md) section:

- [im.chat.user.add](./chat-users/im-chat-user-add.md) — add participants
- [im.chat.user.delete](./chat-users/im-chat-user-delete.md) — remove participants
- [im.chat.user.list](./chat-users/im-chat-user-list.md) — get participant identifiers
- [im.dialog.users.list](./chat-users/im-dialog-users-list.md) — get detailed participant list

You can transfer ownership to another participant using the method [im.chat.setOwner](./chat-update/im-chat-set-owner.md).

### Messages and History

The primary workflow for messaging begins with methods from the [Messages](./messages/index.md) section.

To send messages, use [im.message.add](./messages/im-message-add.md), to read history — [im.dialog.messages.get](./messages/im-dialog-messages-get.md), and to search through history — [im.dialog.messages.search](./messages/im-dialog-messages-search.md).

The read status and service actions for messaging are managed by the methods [im.dialog.read](./messages/im-dialog-read.md), [im.dialog.unread](./messages/im-dialog-unread.md), and [im.dialog.writing](./messages/im-dialog-writing.md).

### Files

You can send files in the chat. To work with files in new integrations, use the methods from the `im.v2` section of [Files](../chat-bots/chat-bots-v2/im.v2/files/index.md).

Use the methods from the group [im.disk.*](./files/index.md) only for existing integrations with the old scenario.

### Notifications and Special Actions

A notification in the messenger is a message containing information from the system or a user. Notifications are handled by the group of methods [im.notify.*](./notifications/index.md).

Methods for service actions with chats and recent dialog lists can be found in the [Special Operations](./special-operations/index.md) section. You can pin a chat, mark it as read, and hide it from the recent list.

## How to Get Started

1. Create a chat using the method [im.chat.add](./im-chat-add.md) or obtain an existing identifier through [im.chat.get](./im-chat-get.md).
2. Retrieve basic dialog data using the method [im.dialog.get](./im-dialog-get.md) and, if necessary, the list of recent chats through [im.recent.list](./im-recent-list.md).
3. Add participants to the chat using the method [im.chat.user.add](./chat-users/im-chat-user-add.md).
4. Configure the chat as needed: change the name, color, avatar, or owner using methods from the [Chat Update](./chat-update/index.md) section.
5. Send a message using [im.message.add](./messages/im-message-add.md) or a notification using [im.notify](./notifications/im-notify.md).

## Interaction with Other Objects

**User.** Most methods operate on behalf of the current user or use identifiers `USER_ID`, `USERS`. You can obtain a user identifier using the method [user.get](../user/user-get.md). You can work with users using methods from the [Users](./users/index.md) section.

**Company Departments.** Methods for searching and working with departments use the department identifier `ID`. You can obtain a department identifier using the [get department list](../departments/department-get.md) method or the [search departments by name](./search/im-search-department-list.md) method.

**Files.** Old file upload scenarios use methods `im.disk.*`, while new scenarios have been moved to `im.v2`. Details are collected in the [Files](./files/index.md) section.

## Current API Version

For new integrations, use the current sections `im.*` and new methods `im.v2`, if the scenario has already been migrated to the new generation of the API.

- For files, use [im.v2.File.upload](../chat-bots/chat-bots-v2/im.v2/files/file-upload.md) and [im.v2.File.download](../chat-bots/chat-bots-v2/im.v2/files/file-download.md).
- For user events in the messenger, use the section [im.v2: events](../chat-bots/chat-bots-v2/im.v2/events/index.md).
- Methods from the sections [Files](./files/index.md), [Search](./search/index.md), and [Users](./users/index.md) contain notes about scenarios from the previous version of the chat, which operate with limitations in the M1 interface.

## Widgets

You can embed an application into the chat interface. By embedding, you can add an action next to the input field, a separate item in the chat sidebar, or an action in the context menu of a message.

- [Item in the panel above the input field](../widgets/im/textarea.md) `IM_TEXTAREA`
- [Item in the chat sidebar](../widgets/im/sidebar.md) `IM_SIDEBAR`
- [Item in the context menu of a message](../widgets/im/context-menu.md) `IM_CONTEXT_MENU`

To register an embedding point, use the method [placement.bind](../widgets/placement-bind.md) and pass the required code in the `PLACEMENT` parameter.

## Overview of Methods and Events {#all-methods}

> Scope: [`im`](../scopes/permissions.md)
>
> Who can execute the method: depending on the method

### Main Chat Methods

#|
|| **Method** | **Description** ||
|| [im.chat.add](./im-chat-add.md) | Creates a chat ||
|| [im.chat.get](./im-chat-get.md) | Retrieves the chat identifier ||
|| [im.dialog.get](./im-dialog-get.md) | Retrieves chat data ||
|| [im.recent.get](./im-recent-get.md) | Retrieves a shortened list of recent chats ||
|| [im.recent.list](./im-recent-list.md) | Retrieves a list of chats ||
|| [im.counters.get](./im-counters-get.md) | Retrieves message and notification counters ||
|| [im.revision.get](./im-revision-get.md) | Retrieves API revisions for the IM module ||
|#

### Chat Update

#|
|| **Method** | **Description** ||
|| [im.chat.setOwner](./chat-update/im-chat-set-owner.md) | Changes the chat owner ||
|| [im.chat.updateAvatar](./chat-update/im-chat-update-avatar.md) | Changes the chat avatar ||
|| [im.chat.updateColor](./chat-update/im-chat-update-color.md) | Changes the chat color ||
|| [im.chat.updateTitle](./chat-update/im-chat-update-title.md) | Changes the chat title ||
|#

### Chat Participants

#|
|| **Method** | **Description** ||
|| [im.chat.user.add](./chat-users/im-chat-user-add.md) | Adds participants to the chat ||
|| [im.chat.user.list](./chat-users/im-chat-user-list.md) | Retrieves participant identifiers of the chat ||
|| [im.dialog.users.list](./chat-users/im-dialog-users-list.md) | Retrieves the list of participants ||
|| [im.chat.user.delete](./chat-users/im-chat-user-delete.md) | Removes participants from the chat ||
|| [im.chat.leave](./chat-users/im-chat-leave.md) | Allows the current user to leave the chat ||
|#

### Messages

#|
|| **Method** | **Description** ||
|| [im.message.add](./messages/im-message-add.md) | Adds a message ||
|| [im.message.update](./messages/im-message-update.md) | Modifies a sent message ||
|| [im.message.delete](./messages/im-message-delete.md) | Deletes a message ||
|| [im.message.like](./messages/im-message-like.md) | Changes the "like" status of a message ||
|| [im.message.share](./messages/im-message-share.md) | Creates an object based on a message ||
|| [im.message.command](./messages/im-message-command.md) | Executes a chat-bot command ||
|| [im.dialog.messages.get](./messages/im-dialog-messages-get.md) | Retrieves the list of recent messages ||
|| [im.dialog.messages.search](./messages/im-dialog-messages-search.md) | Searches for messages in the chat ||
|| [im.dialog.read](./messages/im-dialog-read.md) | Marks messages as "read" ||
|| [im.dialog.unread](./messages/im-dialog-unread.md) | Marks messages as "unread" ||
|| [im.dialog.writing](./messages/im-dialog-writing.md) | Sends the "User is typing" status ||
|#

### Notifications

#|
|| **Method** | **Description** ||
|| [im.notify](./notifications/im-notify.md) | Sends a notification ||
|| [im.notify.personal.add](./notifications/im-notify-personal-add.md) | Sends a personal notification ||
|| [im.notify.system.add](./notifications/im-notify-system-add.md) | Sends a system notification ||
|| [im.notify.get](./notifications/im-notify-get.md) | Returns user notifications ||
|| [im.notify.schema.get](./notifications/im-notify-schema-get.md) | Returns the schema of notification types ||
|| [im.notify.read.list](./notifications/im-notify-read-list.md) | Marks a list of notifications as read ||
|| [im.notify.read](./notifications/im-notify-read.md) | Marks a notification as read or returns it to unread ||
|| [im.notify.read.all](./notifications/im-notify-read-all.md) | Marks all notifications as read ||
|| [im.notify.answer](./notifications/im-notify-answer.md) | Responds to a notification with a quick reply ||
|| [im.notify.confirm](./notifications/im-notify-confirm.md) | Interacts with notification buttons ||
|| [im.notify.delete](./notifications/im-notify-delete.md) | Deletes notifications ||
|| [im.notify.history.search](./notifications/im-notify-history-search.md) | Searches through notification history ||
|#

### Search

#|
|| **Method** | **Description** ||
|| [im.search.chat.list](./search/im-search-chat-list.md) | Searches chats by name ||
|| [im.search.department.list](./search/im-search-department-list.md) | Searches departments ||
|| [im.search.user.list](./search/im-search-user-list.md) | Searches users ||
|| [im.search.last.add](./search/im-search-last-add.md) | Adds search to history ||
|| [im.search.last.get](./search/im-search-last-get.md) | Retrieves search history ||
|| [im.search.last.delete](./search/im-search-last-delete.md) | Deletes search from history ||
|#

### Departments

#|
|| **Method** | **Description** ||
|| [im.department.get](./departments/im-department-get.md) | Retrieves information about a department ||
|| [im.department.managers.get](./departments/im-department-managers-get.md) | Retrieves a list of department managers ||
|| [im.department.employees.get](./departments/im-department-employees-get.md) | Retrieves a list of department employees ||
|| [im.department.colleagues.list](./departments/im-department-colleagues-list.md) | Retrieves a list of colleagues of the current user ||
|#

### Users

#|
|| **Method** | **Description** ||
|| [im.user.get](./users/im-user-get.md) | Retrieves user data ||
|| [im.user.list.get](./users/im-user-list-get.md) | Retrieves data about a list of users ||
|| [im.user.status.set](./users/im-user-status-set.md) | Sets the user's status in the chat ||
|| [im.user.status.get](./users/im-user-status-get.md) | Retrieves the user's set status ||
|| [im.user.status.idle.start](./users/im-user-status-idle-start.md) | Sets the automatic status "Away" ||
|| [im.user.status.idle.end](./users/im-user-status-idle-end.md) | Disables the automatic status "Away" ||
|#

### Special Operations

#|
|| **Method** | **Description** ||
|| [im.recent.pin](./special-operations/im-recent-pin.md) | Pins the chat at the top of the list ||
|| [im.recent.unread](./special-operations/im-recent-unread.md) | Sets or removes the "read" status for the chat ||
|| [im.dialog.read.all](./special-operations/im-dialog-read-all.md) | Marks all chats of the user as "read" ||
|| [im.chat.mute](./special-operations/im-chat-mute.md) | Disables notifications from the chat ||
|| [im.recent.hide](./special-operations/im-recent-hide.md) | Removes the chat from the recent list ||
|#

### Files

#|
|| **Method** | **Description** ||
|| [im.v2.File.upload](../chat-bots/chat-bots-v2/im.v2/files/file-upload.md) | Uploads a file to the chat ||
|| [im.v2.File.download](../chat-bots/chat-bots-v2/im.v2/files/file-download.md) | Returns a link to download the file ||
|| [im.disk.file.commit](./files/im-disk-file-commit.md) | Adds a file to the chat ||
|| [im.disk.file.save](./files/im-disk-file-save.md) | Saves a file to your drive ||
|| [im.disk.file.delete](./files/im-disk-file-delete.md) | Deletes a file from the chat folder ||
|| [im.disk.folder.get](./files/im-disk-folder-get.md) | Retrieves the folder for storing chat files ||
|#

### New API Events

#|
|| **Method** | **Description** ||
|| [im.v2.Event.subscribe](../chat-bots/chat-bots-v2/im.v2/events/event-subscribe.md) | Subscribes the current user to event logging ||
|| [im.v2.Event.get](../chat-bots/chat-bots-v2/im.v2/events/event-get.md) | Returns accumulated events ||
|| [im.v2.Event.unsubscribe](../chat-bots/chat-bots-v2/im.v2/events/event-unsubscribe.md) | Stops event logging ||
|#