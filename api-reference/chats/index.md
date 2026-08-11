# Chats in Bitrix24: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

A chat in Bitrix24 helps you:

- communicate one-on-one
- discuss tasks in a group
- work with notifications, files, and messages within a single messenger interface

Chats are managed by the `im.*` methods. Individual scenarios — participants, messages, notifications, search, files, and special operations — are placed in subsections.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Chats in Bitrix24: Interface and Capabilities](https://helpdesk.bitrix24.com/open/21924784/)

## How to Choose a Subsection

#|
|| **If You Need To** | **Open the Subsection** ||
|| Change the title, color, avatar, or owner of a chat | [Chat Update](./chat-update/index.md) ||
|| Add, retrieve, or remove participants | [Chat Participants](./chat-users/index.md) ||
|| Send, modify, and read messages | [Messages](./messages/index.md) ||
|| Format a message, build an attachment, a keyboard, or a context menu | [Formatting](./messages/formatting.md), [Attachments](./messages/attachments.md), [Keyboards](./messages/keyboards.md), [Context Menu](./messages/menu.md) ||
|| Send notifications and manage their read status | [Notifications](./notifications/index.md) ||
|| Upload and download chat files | [Files](./files/index.md) ||
|| Search chats, employees, and departments | [Search](./search/index.md) ||
|| Retrieve user data and manage the status | [Users](./users/index.md) ||
|| Retrieve the composition of company departments | [Departments](./departments/index.md) ||
|| Pin, hide, and mute chats | [Special Operations](./special-operations/index.md) ||
|| Understand the mechanisms of the previous generation of chat applications | [Deprecated](./outdated/index.md) ||
|#

## Chat Identifiers

A group chat and a private dialog differ by the `DIALOG_ID` identifier:

#|
|| **Format** | **What It Means** | **Example** ||
|| `XXX` | A private dialog, where `XXX` is the user identifier | `47` ||
|| `chatXXX` | A group chat, where `XXX` is the chat identifier | `chat2935` ||
|| `sgXXX` | A workgroup or project chat, where `XXX` is the group identifier | `sg17` ||
|#

Some methods accept not `DIALOG_ID` but a numeric `CHAT_ID` — the same value without the `chat` prefix.

Chats linked to CRM, tasks, the calendar, and Open Channels are found by the `ENTITY_TYPE` and `ENTITY_ID` pair with the [im.chat.get](./im-chat-get.md) method.

## Authorization and Limits

- the `im.*` and `im.v2.*` methods work in the `im` scope. The exception is the `imbot.app.*` methods from the [Deprecated](./outdated/index.md) subsection — they require the `imbot` scope
- the notification sending methods [im.notify](./notifications/im-notify.md), [im.notify.personal.add](./notifications/im-notify-personal-add.md), and [im.notify.system.add](./notifications/im-notify-system-add.md) cannot be called with session authorization — call them via a webhook or with an application token
- when calling via a webhook, the `TAG` and `SUB_TAG` tags are passed together with `CLIENT_ID`
- the size of the serialized `ATTACH` attachment is limited to 60,000 characters
- in the search methods, the search phrase must be at least two characters, and `LIMIT` is 50 at most
- the chat avatar is passed as a Base64 string, and the maximum image size is 5000×5000 pixels
- the content of a file uploaded to a chat is passed as a Base64 string, with a maximum size of 100 MB

## How to Get Started

1. Create a chat with the [im.chat.add](./im-chat-add.md) method or obtain an existing identifier through [im.chat.get](./im-chat-get.md)
2. Retrieve the basic dialog data with the [im.dialog.get](./im-dialog-get.md) method and, if necessary, the list of recent chats through [im.recent.list](./im-recent-list.md)
3. Add participants to the chat with the [im.chat.user.add](./chat-users/im-chat-user-add.md) method
4. Configure the chat if needed: change the title, color, avatar, or owner with the methods of the [Chat Update](./chat-update/index.md) subsection
5. Send a message through [im.message.add](./messages/im-message-add.md) or a notification through [im.notify](./notifications/im-notify.md)

## Interaction with Other Objects

**User.** Most methods operate on behalf of the current user or use the identifiers `USER_ID`, `USERS`. You can obtain a user identifier with the [user.get](../user/user-get.md) method. You can work with users using the methods of the [Users](./users/index.md) subsection.

**Company departments.** The methods for searching and working with departments use the department identifier `ID`. You can obtain a department identifier with the [get department list](../departments/department-get.md) method or the [search departments by name](./search/im-search-department-list.md) method.

**Files.** A chat file is stored on Drive and linked to a message. How to upload and download a file is described in the [Files](./files/index.md) subsection.

**CRM, tasks, and the calendar.** A chat can be linked to an external object. The link is set by the `ENTITY_TYPE` and `ENTITY_ID` pair when creating the chat with the [im.chat.add](./im-chat-add.md) method, and you can find the linked chat by this pair with the [im.chat.get](./im-chat-get.md) method.

**Chatbots.** The same operations on behalf of a bot are performed by the methods of the [Chatbots](../chat-bots/index.md) section.

## Current API Version

For new integrations, use the `im.*` methods from this section and the `im.v2` methods wherever the scenario has already been migrated to the new generation of the API.

What is replaced by what:

#|
|| **Deprecated Path** | **Current Replacement** ||
|| [im.disk.folder.get](./files/im-disk-folder-get.md) + upload through Drive methods + [im.disk.file.commit](./files/im-disk-file-commit.md) | [im.v2.File.upload](../chat-bots/chat-bots-v2/im.v2/files/file-upload.md) — a single call instead of a chain ||
|| [im.disk.file.save](./files/im-disk-file-save.md), [im.disk.file.delete](./files/im-disk-file-delete.md) | There is no replacement yet, use these methods ||
|| [im.search.last.add](./search/im-search-last-add.md), [im.search.last.get](./search/im-search-last-get.md), [im.search.last.delete](./search/im-search-last-delete.md) | There is no replacement: the methods work, but the result is not displayed in the M1 chat interface ||
|| [im.user.status.idle.start](./users/im-user-status-idle-start.md), [im.user.status.idle.end](./users/im-user-status-idle-end.md) | There is no replacement: the methods work, but the result is not displayed in the M1 chat interface ||
|| The previous generation of chat applications — the [Deprecated](./outdated/index.md) subsection | [Chatbots](../chat-bots/index.md) and [messenger widgets](../widgets/im/index.md) ||
|#

The user events of the messenger are collected in the [im.v2: Events](../chat-bots/chat-bots-v2/im.v2/events/index.md) section.

The remaining `im.*` methods from this section are current and have no deprecated variants.

## Widgets

You can embed an application into the chat interface. An embedding adds an action next to the input field, an item in the chat sidebar, an action in the context menu of a message, or your own section in the messenger navigation menu.

- [Item in the panel above the input field](../widgets/im/textarea.md) `IM_TEXTAREA`
- [Item in the chat sidebar](../widgets/im/sidebar.md) `IM_SIDEBAR`
- [Item in the context menu of a message](../widgets/im/context-menu.md) `IM_CONTEXT_MENU`
- [Item in the messenger navigation menu](../widgets/im/navigation.md) `IM_NAVIGATION`

To register an embedding point, use the method [placement.bind](../widgets/placement-bind.md) and pass the required code in the `PLACEMENT` parameter. All placements of the section, with the setup order and the call context, are collected in the overview [{#T}](../widgets/im/index.md).

## Overview of Methods {#all-methods}

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
|| [im.chat.updateTitle](./chat-update/im-chat-update-title.md) | Changes the chat title ||
|| [im.chat.updateAvatar](./chat-update/im-chat-update-avatar.md) | Changes the chat avatar ||
|| [im.chat.updateColor](./chat-update/im-chat-update-color.md) | Changes the chat color ||
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
|| [im.message.share](./messages/im-message-share.md) | Creates a chat, task, post, or calendar event based on a message ||
|| [im.message.command](./messages/im-message-command.md) | Executes a chatbot command ||
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
|| [im.notify.answer](./notifications/im-notify-answer.md) | Replies to a notification with a quick response ||
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
|#

#### Methods of the Previous Version of the Chat

#|
|| **Method** | **Description** ||
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
|#

#### Methods of the Previous Version of the Chat

#|
|| **Method** | **Description** ||
|| [im.user.status.idle.start](./users/im-user-status-idle-start.md) | Sets the automatic status "Away" ||
|| [im.user.status.idle.end](./users/im-user-status-idle-end.md) | Disables the automatic status "Away" ||
|#

### Special Operations

#|
|| **Method** | **Description** ||
|| [im.recent.pin](./special-operations/im-recent-pin.md) | Pins the chat at the top of the list ||
|| [im.recent.unread](./special-operations/im-recent-unread.md) | Sets or removes the "unread" label on the chat ||
|| [im.dialog.read.all](./special-operations/im-dialog-read-all.md) | Marks all chats of the user as "read" ||
|| [im.chat.mute](./special-operations/im-chat-mute.md) | Disables notifications from the chat ||
|| [im.recent.hide](./special-operations/im-recent-hide.md) | Removes the chat from the recent list ||
|#

### Files

#|
|| **Method** | **Description** ||
|| [im.v2.File.upload](../chat-bots/chat-bots-v2/im.v2/files/file-upload.md) | Uploads a file to the chat ||
|| [im.v2.File.download](../chat-bots/chat-bots-v2/im.v2/files/file-download.md) | Returns a link to download the file ||
|| [im.disk.file.save](./files/im-disk-file-save.md) | Saves a file to your Drive ||
|| [im.disk.file.delete](./files/im-disk-file-delete.md) | Deletes a file from the chat folder ||
|#

#### Methods of the Previous Generation of the API

#|
|| **Method** | **Description** ||
|| [im.disk.file.commit](./files/im-disk-file-commit.md) | Adds a file to the chat. Replaced by the [im.v2.File.upload](../chat-bots/chat-bots-v2/im.v2/files/file-upload.md) method ||
|| [im.disk.folder.get](./files/im-disk-folder-get.md) | Retrieves the folder for storing chat files. The folder is no longer needed to upload a file ||
|#

### Previous Generation of Chat Applications

The methods work in the `imbot` scope and are kept only to support existing integrations. For new development, use [chatbots](../chat-bots/index.md) and [messenger widgets](../widgets/im/index.md).

#|
|| **Method** | **Description** ||
|| [imbot.app.register](./outdated/create-app/imbot-app-register.md) | Registers a chat application ||
|| [imbot.app.update](./outdated/create-app/imbot-app-update.md) | Updates the data of a chat application ||
|| [imbot.app.unregister](./outdated/create-app/imbot-app-unregister.md) | Deletes a chat application ||
|#

### Working with Messenger Events

#|
|| **Method** | **Description** ||
|| [im.v2.Event.subscribe](../chat-bots/chat-bots-v2/im.v2/events/event-subscribe.md) | Subscribes the current user to event logging ||
|| [im.v2.Event.get](../chat-bots/chat-bots-v2/im.v2/events/event-get.md) | Returns accumulated events ||
|| [im.v2.Event.unsubscribe](../chat-bots/chat-bots-v2/im.v2/events/event-unsubscribe.md) | Stops event logging ||
|#
