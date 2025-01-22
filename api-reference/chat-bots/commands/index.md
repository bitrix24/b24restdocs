# About Chatbot Commands

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- edits needed for writing standards
- links to pages not yet created are not specified
- from Sergei's file: how they look, how to apply

{% endnote %}

{% endif %}

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

Commands can be **global** or **local**.

> Quick navigation: [all methods and events](#all-methods) 

{% note warning %}

Please note! To process the command, the application must handle the event of adding a command [ONIMCOMMANDADD](./events/on-im-command-add.md).

{% endnote %}

## Global

They work in any dialogue or chat. An example of such a command (`COMMON = Y`):

```
/giphy image
```

Calling it in any chat will generate a response from the chatbot, even in a chat where the chatbot is not a member.

![Command Selection](./_images/command1.png)

## Local

They work only in direct messages with the chatbot and in group chats where it is a participant. An example of such a command (`COMMON = N`):

```
/help
```

Calling it in the [EchoBot](https://github.com/bitrix24com/bots) (**bot.php**) will display a list of available commands.

![Command Selection](./_images/keyboard1.png)


## Overview of Methods {#all-methods}

{% list tabs %}

- Methods

    #| 
    || **Method** | **Description** ||
    || [imbot.command.register](./imbot-command-register.md) | Registers a new command for the chatbot ||
    || [imbot.command.unregister](./imbot-command-unregister.md) | Removes a registered command from the chatbot ||
    || [imbot.command.update](./imbot-command-update.md) | Updates information about a registered command of the chatbot ||
    || [imbot.command.answer](./imbot-command-answer.md) | Publishes a response to the chatbot command ||
    |#

- Events

    #| 
    || **Event** | **Triggered** ||
    || [ONIMCOMMANDADD](./events/on-im-command-add.md) | When a new command is added by the chatbot ||
    |#

{% endlist %}