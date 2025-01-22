# Working with Messages

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- no content (only the list of methods is available)
- links to pages that have not been created yet are not specified

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

{% note warning %}

These methods for working with messages are typically used when a user writes something, so it is essential to handle the event [ONIMBOTMESSAGEADD](./events/on-imbot-message-add.md).

{% endnote %}

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [imbot.message.add](./imbot-message-add.md) | Adds a new message from the chat bot ||
    || [imbot.message.update](./imbot-message-update.md) | Updates an existing message from the chat bot ||
    || [imbot.message.delete](./imbot-message-delete.md) | Deletes a message from the chat bot ||
    || [imbot.message.like](./imbot-message-like.md) | Likes a message from the chat bot ||
    || [imbot.chat.sendTyping](./imbot-chat-send-typing.md) | Sends a typing indicator in the chat ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [ONIMBOTMESSAGEADD](./events/on-imbot-message-add.md) | When a message is sent ||
    || [ONIMBOTMESSAGEUPDATE](./events/on-imbot-message-update.md) | When a message from the chat bot is updated ||
    || [ONIMBOTMESSAGEDELETE](./events/on-imbot-message-delete.md) | When a message from the chat bot is deleted ||
    |#

{% endlist %}