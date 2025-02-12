# Open Lines in Bitrix24

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- from Sergey's file: how they work, what the connection is between them and the connectors, what scenarios are implemented in REST

{% endnote %}

{% endif %}

## Overview of Methods

#|
|| **Method** | **Description** ||
|| [imopenlines.config.add](./imopenlines-config-add.md) | Adds a new open line ||
|| [imopenlines.config.delete](./imopenlines-config-delete.md) | Deletes an open line ||
|| [imopenlines.config.get](./imopenlines-config-get.md) | Retrieves an open line by Id ||
|| [imopenlines.config.list.get](./imopenlines-config-list-get.md) | Retrieves a list of open lines ||
|| [imopenlines.config.path.get](./imopenlines-config-path-get.md) | Gets a link to the public page of open lines in the account ||
|| [imopenlines.config.update](./imopenlines-config-update.md) | Modifies an open line ||
|| [imopenlines.network.join](./imopenlines-network-join.md) | Connects an external open line to the account ||
|| [imopenlines.revision.get](./imopenlines-revision-get.md) | Retrieves information about API revisions ||
|#

### Chatbots in Open Lines

#|
|| **Method** | **Description** ||
|| [imopenlines.bot.session.finish](./chat-bots/imopenlines-bot-session-finish.md) | Ends the dialogue ||
|| [imopenlines.bot.session.message.send](./chat-bots/imopenlines-bot-session-message-send.md) | Sends a welcome message ||
|| [imopenlines.bot.session.operator](./chat-bots/imopenlines-bot-session-operator.md) | Switches the dialogue to an available operator ||
|| [imopenlines.bot.session.transfer](./chat-bots/imopenlines-bot-session-transfer.md) | Transfers the dialogue to an operator by Id ||
|#

### Chats in Open Lines

#|
|| **Method** | **Description** ||
|| [imopenlines.crm.chat.getLastId](./chats/imopenlines-crm-chat-get-last-id.md) | Retrieves the Id of the last chat ||
|| [imopenlines.crm.chat.get](./chats/imopenlines-crm-chat-get.md) | Retrieves the chat for a CRM object ||
|| [imopenlines.crm.chat.user.add](./chats/imopenlines-crm-chat-user-add.md) | Adds a user to an existing chat ||
|| [imopenlines.crm.chat.user.delete](./chats/imopenlines-crm-chat-user-delete.md) | Removes a user from the chat ||
|#

### Messages in Open Lines

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [imopenlines.crm.message.add](./messages/imopenlines-crm-message-add.md) | Sends a message to the open line ||
    || [imopenlines.message.quick.save](./messages/imopenlines-message-quick-save.md) | Saves a message as a quick reply ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [OnOpenLineMessageAdd](./events/on-open-line-message-add.md) | When a message is added to the chat ||
    || [OnOpenLineMessageUpdate](./events/on-open-line-message-update.md) | When a message is updated in the chat ||
    || [OnOpenLineMessageDelete](./events/on-open-line-message-delete.md) | When a message is deleted from the chat ||
    |#

{% endlist %}

### Operators of Open Lines

#|
|| **Method** | **Description** ||
|| [imopenlines.operator.another.finish](./operators/imopenlines-operator-another-finish.md) | Ends the dialogue of another operator ||
|| [imopenlines.operator.answer](./operators/imopenlines-operator-answer.md) | Takes the dialogue for themselves ||
|| [imopenlines.operator.finish](./operators/imopenlines-operator-finish.md) | Ends their own dialogue ||
|| [imopenlines.operator.skip](./operators/imopenlines-operator-skip.md) | Skips the dialogue ||
|| [imopenlines.operator.spam](./operators/imopenlines-operator-spam.md) | Marks the dialogue as "spam" ||
|| [imopenlines.operator.transfer](./operators/imopenlines-operator-transfer.md) | Transfers the dialogue to another operator or to another line ||
|#

### Dialogues in Open Lines

{% list tabs %}

- Methods

    #|
    || **Method** | **Description** ||
    || [imopenlines.crm.lead.create](./sessions/imopenlines-crm-lead-create.md) | Creates a lead based on the dialogue ||
    || [imopenlines.dialog.get](./sessions/imopenlines-dialog-get.md) | Retrieves information about the operator's dialogue (chat) in the open line ||
    || [imopenlines.message.session.start](./sessions/imopenlines-message-session-start.md) | Starts a new dialogue based on a message ||
    || [imopenlines.session.head.vote](./sessions/imopenlines-session-head-vote.md) | Rates the employee's performance in the dialogue ||
    || [imopenlines.session.history.get](./sessions/imopenlines-session-history-get.md) | Retrieves chat and dialogue messages ||
    || [imopenlines.session.intercept](./sessions/imopenlines-session-intercept.md) | Takes the dialogue from the current operator ||
    || [imopenlines.session.join](./sessions/imopenlines-session-join.md) | Joins the dialogue ||
    || [imopenlines.session.mode.pinAll](./sessions/imopenlines-session-mode-pin-all.md) | Pins all available dialogues to the operator ||
    || [imopenlines.session.mode.pin](./sessions/imopenlines-session-mode-pin.md) | Pins or unpins the dialogue ||
    || [imopenlines.session.mode.silent](./sessions/imopenlines-session-mode-silent.md) | Switches the dialogue to "hidden" mode ||
    || [imopenlines.session.mode.unpinAll](./sessions/imopenlines-session-mode-unpin-all.md) | Unpins all dialogues from the operator ||
    || [imopenlines.session.open](./sessions/imopenlines-session-open.md) | Retrieves the chat by symbolic code ||
    || [imopenlines.session.start](./sessions/imopenlines-session-start.md) | Starts a new dialogue ||
    |#

- Events

    #|
    || **Event** | **Triggered** ||
    || [OnSessionStart](./events/on-session-start.md) | When a chat is created ||
    || [OnSessionFinish](./events/on-session-finish.md) | When a chat is closed ||
    |#

{% endlist %}