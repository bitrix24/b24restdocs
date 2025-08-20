# Chat Update: Overview of Methods

After creating a chat, you can assign a different owner or change its appearance. Who can change the appearance depends on the chat settings:
-  all participants,
-  the owner and chat administrators,
-  only the owner.

> Quick navigation: [all methods](#all-methods)

## Relationship of Methods with Other Objects

**Chat.** To change the chat parameters, pass the chat identifier `CHAT_ID` to the method. You can obtain the chat identifier using the method:
-  [creating a chat](../im-chat-add.md),
-  [getting the chat identifier](../im-chat-get.md),
-  [getting the list of chats](../im-recent-list.md),
-  [getting a shortened list of recent chats](../im-recent-get.md).

**User.** To change the owner of the chat, specify their identifier in the `USER_ID` parameter. You can obtain the user identifier using the [getting the list of users](../../user/user-get.md) method or the [getting the list of chat participants](../chat-users/im-chat-user-list.md) method.

## Change Chat Owner

You can change the chat owner using the [im.chat.setOwner](./im-chat-set-owner.md) method. The new owner will receive full access permissions to the chat.

Only the current owner of the chat can use this method.

## Update Icon

You can update the chat icon using the [im.chat.updateAvatar](../chat-update/im-chat-update-avatar.md) method. Pass the image in the `AVATAR` parameter as a `base64` string, for example, `'AVATAR': 'iVBOKLSR…Rw0K=='`.

After the update, a system message will be sent to the chat: *John Smith changed the chat icon.*

{% note tip "Documentation" %}

- [How to upload files](../../files/how-to-upload-files.md)

{% endnote %}

## Change Color

By default, the system randomly selects the chat color. The color is displayed if there is no icon.

You can change the color using the [im.chat.updateColor](../chat-update/im-chat-update-color.md) method. In the `COLOR` parameter, pass the name of the color from the list of supported colors. After the change, a system message will be sent to the chat: *John Smith changed the chat color to "Mint".*

## Change Title

You can change the chat title using the [im.chat.updateTitle](../chat-update/im-chat-update-title.md) method. After the change, a system message will be sent to the chat: *John Smith changed the chat title to "New Chat Title".*

## Overview of Methods {#all-methods}

> Scope: [`im`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

#|
|| **Method** | **Description** ||
|| [im.chat.setOwner](./im-chat-set-owner.md) | Changes the chat owner ||
|| [im.chat.updateAvatar](./im-chat-update-avatar.md) | Changes the chat icon ||
|| [im.chat.updateColor](./im-chat-update-color.md) | Changes the chat color ||
|| [im.chat.updateTitle](./im-chat-update-title.md) | Changes the chat title ||
|#