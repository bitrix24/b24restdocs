# About Chats

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no content (only the list of methods is available)
- from Sergei's file: individual, group, chat owner

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [imbot.chat.add](./imbot-chat-add.md) | Creates a new chat ||
    || [imbot.chat.get](./imbot-chat-get.md) | Returns information about the chat ||
    || [imbot.chat.leave](./imbot-chat-leave.md) | Makes the chat bot leave the specified chat ||
    || [imbot.chat.setOwner](./imbot-chat-set-owner.md) | Sets a new owner for the chat ||
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
    || [ONIMBOTDELETE](./events/on-imbot-delete.md) | When the chat bot is deleted ||
    || [ONIMBOTJOINCHAT](./events/on-imbot-join-chat.md) | When the chat bot receives information about being added to a chat (or personal conversation) ||
    |#

{% endlist %}