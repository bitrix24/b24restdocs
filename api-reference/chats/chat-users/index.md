# Chat Participants: Overview of Methods

Group chats facilitate communication among multiple users simultaneously. Manage chat participants using the im.chat.user.* methods and additional methods like [im.dialog.users.list](./im-dialog-users-list.md) and [im.chat.leave](./im-chat-leave.md).

> Quick navigation: [all methods and events](#all-methods)

## Linking Chat Participants to Other Objects

**User.** To add participants to a chat, provide a list of their identifiers in the `USERS` parameter. To remove a participant, specify their identifier in the `USER_ID` parameter. You can obtain a user's identifier using the [user.get](https://apidocs.bitrix24.ru/api-reference/user/user-get.md) method.

**Chat.** Users are linked to the chat by the chat identifier `CHAT_ID`. You can obtain the chat identifier through the [create chat](../im-chat-add.md) method or the [get chat identifier](../im-chat-get.md) method.

## Add Participants

The [im.chat.user.add](./im-chat-user-add.md) method adds users to the chat. By default, new users do not see the chat history. You can configure a new participant's access to the history using the `HIDE_HISTORY` parameter.

## Get List of Participants

The [im.chat.user.list](./im-chat-user-list.md) method retrieves a list of chat participant identifiers.

To get detailed information about each user, use the [im.dialog.users.list](./im-dialog-users-list.md) method. It supports pagination for handling large lists. You can exclude system users from the list or include only specific types.

## Exclude Participants

The [im.chat.user.delete](./im-chat-user-delete.md) method removes participants from the chat. To leave the chat as the current user, use the [im.chat.leave](./im-chat-leave.md) method.

## Overview of Methods {#all-methods}

#|
|| **Method** | **Description** ||

|| [im.chat.user.add](./im-chat-user-add.md) | Invite participants to the chat ||
|| [im.chat.user.list](./im-chat-user-list.md) | Get identifiers of chat participants ||
|| [im.dialog.users.list](./im-dialog-users-list.md) | Get list of participants ||
|| [im.chat.user.delete](./im-chat-user-delete.md) | Exclude participants from the chat ||
|| [im.chat.leave](./im-chat-leave.md) | Leave the chat as the current user ||
|#