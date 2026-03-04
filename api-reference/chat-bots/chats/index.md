# Chat Bot Chats: Overview of Methods

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- No content (only the list of methods is available)
- From Sergei's file: individual, group, chat owner

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: depends on the method

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [imbot.chat.add](./imbot-chat-add.md) | Creates a new chat ||
    || [imbot.chat.get](./imbot-chat-get.md) | Returns information about the chat ||
    || [imbot.chat.leave](./imbot-chat-leave.md) | Executes the chat bot's exit from the specified chat ||
    || [imbot.chat.setManager](./imbot-chat-set-manager.md) | Assigns or revokes admin rights from a chat participant ||
    || [imbot.chat.sendTyping](./imbot-chat-send-typing.md) | Sends a typing indicator in the chat ||
    || [imbot.chat.updateAvatar](./imbot-chat-update-avatar.md) | Updates the chat avatar ||
    || [imbot.chat.updateColor](./imbot-chat-update-color.md) | Updates the chat color ||
    || [imbot.chat.updateTitle](./imbot-chat-update-title.md) | Updates the chat title ||
    || [imbot.chat.user.add](./imbot-chat-user-add.md) | Adds a user to the chat ||
    || [imbot.chat.user.list](./imbot-chat-user-list.md) | Returns a list of users in the chat ||
    || [imbot.chat.user.delete](./imbot-chat-user-delete.md) | Removes a user from the chat ||
    || [imbot.dialog.get](./imbot-dialog-get.md) | Returns information about the dialog ||
    |#

- Events

    #| 
    || **Event** | **Triggered** ||
    || [ONIMBOTJOINCHAT](./events/on-imbot-join-chat.md) | When the chat bot receives information about being added to a chat (or personal conversation) ||
    |#

{% endlist %}