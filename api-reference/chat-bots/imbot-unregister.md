# Remove chat-bot imbot.unregister

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- required parameters are not indicated
- not all parameters have examples in the table
- examples are missing
- response on success is missing
- response on error is missing
- links to pages that have not yet been created are not specified

{% endnote %}

{% endif %}

> Scope: [`imbot`](../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.unregister` removes the chat-bot from the system.

{% note warning %}

All one-on-one chats of this chat-bot with users will be lost.

{% endnote %}

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **BOT_ID**
[`unknown`](../data-types.md) | `39` | numeric identifier of the bot | ||
|| **CLIENT_ID**
[`unknown`](../data-types.md) | `''` | string identifier of the chat-bot, used only in Webhook mode | ||
|#

## Examples

{% include [Explanation about restCommand](./_includes/rest-command.md) %}

{% list tabs %}

- PHP

    ```php
    $result = restCommand(
        'imbot.unregister',
        Array(
            'BOT_ID' => 39,
            'CLIENT_ID' => '',
        ),
        $_REQUEST[
            "auth"
        ]
    );
    ```

{% endlist %}

{% include [Footnote about examples](../../_includes/examples.md) %}

## Response on success

`true`

## Response on error

error

### Possible error codes

#|
|| **Code** | **Description** ||
|| **BOT_ID_ERROR** | Chat-bot not found. ||
|| **APP_ID_ERROR** | Chat-bot does not belong to this application; you can only work with chat-bots installed within the application. ||
|#

## Related links

[Rest API - Installation and update events](./events/index.md)