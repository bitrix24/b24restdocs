# About Chatbot Commands

{% note warning "" %}

**DEPRECATED**

The development of methods `imbot.command.*` has been halted.  
Please use the section [Commands (`imbot.v2.Command.*`)](../../chat-bots-v2/imbot.v2/commands/index.md).

{% endnote %}

> Scope: [`imbot`](../../../scopes/permissions.md)  
> Who can execute the method: any user

## Methods

#|
|| **Method** | **Description** ||
|| [imbot.command.register](./imbot-command-register.md) | Registers a new command for the chatbot ||
|| [imbot.command.unregister](./imbot-command-unregister.md) | Removes a registered command from the chatbot ||
|| [imbot.command.update](./imbot-command-update.md) | Updates information about a registered command of the chatbot ||
|| [imbot.command.answer](./imbot-command-answer.md) | Publishes a response to the chatbot command ||
|#

## Events

#|
|| **Event** | **Triggered** ||
|| [ONIMCOMMANDADD](./events/on-im-command-add.md) | When a new command is added by the chatbot ||
|#