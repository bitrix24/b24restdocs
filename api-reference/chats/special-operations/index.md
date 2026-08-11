# Special Operations in Chats: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Special operations help manage chats: pin a chat, set the "unread" label, mark all chats as read at once, disable notifications, and remove a dialog from the recent list.

The methods of this subsection change not the conversation itself, but the way the chat looks in the current user's recent chats list. Working with the messages themselves is collected in the [Messages](../messages/index.md) subsection, and the whole messenger — in the [Chats in Bitrix24](../index.md) section.

All five methods are current: they have no deprecated variants or `im.v2` counterparts. Each method returns `result` of the `boolean` type and a `time` object, and returns no other data.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Chats in Bitrix24: Interface and Capabilities](https://helpdesk.bitrix24.com/open/21924784/)

## Connection of Special Operations with Other Objects

**Chat.** The [im.recent.pin](./im-recent-pin.md), [im.recent.unread](./im-recent-unread.md), and [im.recent.hide](./im-recent-hide.md) methods specify the chat with the `DIALOG_ID` parameter. The [im.chat.mute](./im-chat-mute.md) method accepts `DIALOG_ID` or a numeric `CHAT_ID`. The [im.dialog.read.all](./im-dialog-read-all.md) method has no parameters — it affects all the user's chats.

`DIALOG_ID` accepts a number for a private dialog, `chatXXX` for a group chat, and `sgXXX` for a group or project chat. `CHAT_ID` is the same value without the `chat` prefix. You can obtain the chat identifier using the chat creation method [im.chat.add](../im-chat-add.md) or by retrieving the chat identifier with [im.chat.get](../im-chat-get.md).

**Recent chats list.** The result of the methods is visible in the recent chats list. You can retrieve this list with the [im.recent.get](../im-recent-get.md) and [im.recent.list](../im-recent-list.md) methods.

## How to Choose a Method

#|
|| **If You Need To** | **Method** ||
|| Pin or unpin a chat at the top of the list | [im.recent.pin](./im-recent-pin.md) ||
|| Set or remove the "unread" label on a chat — in the interface it is "View Later" | [im.recent.unread](./im-recent-unread.md) ||
|| Mark all the user's chats as read at once | [im.dialog.read.all](./im-dialog-read-all.md) ||
|| Disable or enable notifications from a chat | [im.chat.mute](./im-chat-mute.md) ||
|| Remove a chat from the recent list | [im.recent.hide](./im-recent-hide.md) ||
|#

The direction of the action is set by the `PIN`, `ACTION`, and `MUTE` parameters: the same method both sets the flag and removes it.

## Mark a Chat as Unread {#unread}

If you want to return to a read message later, use the "unread" label — in the interface it is called "View Later". The label is managed by the [im.recent.unread](./im-recent-unread.md) method.

The [im.dialog.read.all](./im-dialog-read-all.md) method works the other way round: it marks all the user's chats as read at once.

The label is applied to the chat in the recent list. To manage the read status of messages inside a single dialog, use the [im.dialog.read](../messages/im-dialog-read.md) and [im.dialog.unread](../messages/im-dialog-unread.md) methods from the [Messages](../messages/index.md) subsection.

## Disable Chat Notifications {#mute}

In group chats where immediate responses to messages are not required, notifications can be disabled. The [im.chat.mute](./im-chat-mute.md) method both disables notifications from the chat and enables them again.

The method disables notifications only from a specific chat. The messenger notifications themselves — sending, reading, and deleting — are managed by the methods of the [Notifications](../notifications/index.md) subsection.

## Overview of Methods {#all-methods}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#| 
|| **Method** | **Description** ||
|| [im.recent.pin](./im-recent-pin.md) | Pins the chat at the top of the chat list ||
|| [im.recent.unread](./im-recent-unread.md) | Sets or removes the "unread" label on the chat ||
|| [im.dialog.read.all](./im-dialog-read-all.md) | Sets the "read" status for all user chats ||
|| [im.chat.mute](./im-chat-mute.md) | Disables notifications from the chat ||
|| [im.recent.hide](./im-recent-hide.md) | Removes the chat from the recent list ||
|#