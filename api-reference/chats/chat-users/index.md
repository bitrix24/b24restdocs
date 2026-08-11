# Chat Participants: Overview of Methods

{% note tip "" %}

If you are developing integrations for Bitrix24 using AI tools (Codex, Claude Code, Cursor), connect to the [MCP server](../../../ai-tools/mcp.md) so that the assistant can utilize the official REST documentation.

{% endnote %}

Group chats facilitate communication among multiple users simultaneously. The composition of participants is managed by the `im.chat.user.*` method group, joined by [im.dialog.users.list](./im-dialog-users-list.md) and [im.chat.leave](./im-chat-leave.md). The subsection is part of the [Chats in Bitrix24](../index.md) section.

The methods of this subsection are current and have no deprecated counterparts. If the same operations have to be performed on behalf of a chatbot, use the `imbot.v2.Chat.User.*` methods from the [Chats imbot.v2](../../chat-bots/chat-bots-v2/imbot.v2/chats/index.md) subsection.

> Quick navigation: [all methods](#all-methods)
>
> User documentation: [Chats in Bitrix24: Interface and Capabilities](https://helpdesk.bitrix24.com/open/21924784/)

## How to Get Started

1. Retrieve the chat identifier `CHAT_ID` with the [im.chat.add](../im-chat-add.md) or [im.chat.get](../im-chat-get.md) method
2. Retrieve the user identifiers with the [user.get](../../user/user-get.md) method
3. Add the participants with the [im.chat.user.add](./im-chat-user-add.md) method and decide whether to show them the conversation history
4. Check the chat composition with the [im.chat.user.list](./im-chat-user-list.md) or [im.dialog.users.list](./im-dialog-users-list.md) method
5. If necessary, remove a participant with the [im.chat.user.delete](./im-chat-user-delete.md) method or leave the chat with the [im.chat.leave](./im-chat-leave.md) method

## Linking Chat Participants to Other Objects

**User.** To add participants to a chat, pass a list of their identifiers in the `USERS` parameter. To exclude a participant, specify their identifier in the `USER_ID` parameter. You can obtain a user's identifier using the [user.get](../../user/user-get.md) method.

**Chat.** Users are linked to the chat by the chat identifier `CHAT_ID`. You can obtain the chat identifier through the [create chat](../im-chat-add.md) method or the [get chat identifier](../im-chat-get.md) method. The `im.chat.user.*` methods and [im.chat.leave](./im-chat-leave.md) accept a numeric `CHAT_ID`, whereas [im.dialog.users.list](./im-dialog-users-list.md) works with `DIALOG_ID`: `chatXXX` — a group chat, `sgXXX` — a group or project chat, `XXX` — the user identifier for a private chat.

**Chat owner.** The owner appears automatically when the chat is created. The role can be passed to another participant with the [im.chat.setOwner](../chat-update/im-chat-set-owner.md) method from the [Chat Update](../chat-update/index.md) subsection.

**User data.** The methods of this subsection return identifiers and participant cards in the messenger format. The full employee profile is returned by the methods of the [Users](../../user/index.md) section, and the messenger data — by the methods of the [Users in Chats](../users/index.md) subsection. You can find an employee by name with the [im.search.user.list](../search/im-search-user-list.md) method.

## How to Choose a Method

#|
|| **If You Need To** | **Method** ||
|| Add users to a chat | [im.chat.user.add](./im-chat-user-add.md) ||
|| Retrieve only the participant identifiers | [im.chat.user.list](./im-chat-user-list.md) ||
|| Retrieve participant cards with names and statuses, with pagination | [im.dialog.users.list](./im-dialog-users-list.md) ||
|| Remove a participant from a chat | [im.chat.user.delete](./im-chat-user-delete.md) ||
|| Leave a chat on behalf of the current user | [im.chat.leave](./im-chat-leave.md) ||
|#

## Overview of Methods {#all-methods}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

All the methods of this subsection are available to a chat participant. Participants can be added and removed by a participant with the corresponding permission in the chat, and [im.dialog.users.list](./im-dialog-users-list.md) — by any user with access to the chat. If the permission is missing, the method returns the `ACCESS_DENIED_EXTEND` or `ACCESS_ERROR` error.

#|
|| **Method** | **Description** ||
|| [im.chat.user.add](./im-chat-user-add.md) | Adds participants to the chat ||
|| [im.chat.user.list](./im-chat-user-list.md) | Retrieves the identifiers of chat participants ||
|| [im.dialog.users.list](./im-dialog-users-list.md) | Retrieves the list of participants with user data ||
|| [im.chat.user.delete](./im-chat-user-delete.md) | Removes participants from the chat ||
|| [im.chat.leave](./im-chat-leave.md) | Allows the current user to leave the chat ||
|#