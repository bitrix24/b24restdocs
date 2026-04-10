# Special Operations in Chats: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../sdk/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Special operations help manage chats: marking messages as read, pinning chats, disabling notifications, or removing dialogues from the recent chats list.

> Quick Navigation: [All Methods and Events](#all-methods) 
> 
> User Documentation: [Chats in Bitrix24: Interface and Capabilities](https://helpdesk.bitrix24.com/open/21924784/)

## Connection of Special Operations with Other Objects

**Chat.** Special operations with chats are performed using the chat identifier `DIALOG_ID` or `CHAT_ID`. You can obtain the chat identifier using the chat creation method [im.chat.add](../im-chat-add.md) or by retrieving the chat identifier with [im.chat.get](../im-chat-get.md).

## Pin a Chat

To avoid losing an important chat, pin it at the top of the list. You can pin or unpin a chat using the method [im.recent.pin](./im-recent-pin.md).

## Mark a Chat as Read

If you want to return to a read message later, use the "View Later" label. The method `im.recent.unread` manages this label:

- The parameter `ACTION` with the value `Y` sets the "View Later" label.

- The parameter `ACTION` with the value `N` marks all messages as read and removes the label.

The method [im.dialog.read.all](./im-dialog-read-all.md) sets the "Read" label for all dialogues.

## Disable Chat Notifications

In group chats where immediate responses to messages are not required, you can disable notifications. The method [im.chat.mute](./im-chat-mute.md) allows you to turn notifications off or on.

## Remove a Chat from Recent List

The method [im.recent.hide](./im-recent-hide.md) removes a dialogue from the recent chats list.

## Overview of Methods {#all-methods}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: depending on the method

#| 
|| **Method** | **Description** ||
|| [im.recent.pin](./im-recent-pin.md) | Pins the chat at the top of the chat list ||
|| [im.recent.unread](./im-recent-unread.md) | Sets or removes the "read" status for the chat ||
|| [im.dialog.read.all](./im-dialog-read-all.md) | Sets the "read" status for all user chats ||
|| [im.chat.mute](./im-chat-mute.md) | Disables notifications from the chat ||
|| [im.recent.hide](./im-recent-hide.md) | Removes the chat from the recent list ||
|#