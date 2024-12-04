# Get the list of chatbots imbot.bot.list

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- examples are missing
- response on success is missing
- response on error is missing

{% endnote %}

{% endif %}

> Scope: [`imbot`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.bot.list` retrieves a list of available chatbots.

No parameters.

## Examples

{% include [Explanation about restCommand](./_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'imbot.bot.list',
        array(),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}

### Response on success

An array of arrays containing data about the chatbots:

- **ID** - bot identifier.
- **NAME** - chatbot name.
- **CODE** - internal code.
- **OPENLINE** - whether it supports Open Channels or not.