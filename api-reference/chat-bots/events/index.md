# About Events

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- almost no content
- links to pages that have not yet been created are not specified

{% endnote %}

{% endif %}

When generating an event (adding a new message, opening a dialog with the chat bot, inviting the chat bot to a chat, removing the chat bot from the client side), a request is formed for the event handler specified by you during bot registration. You can read more about the event mechanism [here](../../events/index.md).

- ONAPPINSTALL - event for application installation.
- ONAPPUPDATE - event for application update.
- ONIMBOTMESSAGEADD - event that occurs when a message is sent.
- ONIMBOTMESSAGEUPDATE - event that occurs when a message is edited.
- ONIMBOTMESSAGEDELETE - event that occurs when a message is deleted.
- ONIMCOMMANDADD - event for receiving a command by the chat bot.
- ONIMBOTJOINCHAT - event for the chat bot receiving information about being added to a chat (or personal conversation).
- ONIMBOTDELETE - event for the deletion of the chat bot.