# Remove chat-bot imbot.unregister

{% note warning "We are still updating this page" %}

Some data may be missing here — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not exported to prod_" %}

- edits needed for writing standards
- parameter types are not specified
- parameter requirements are not indicated
- not all parameters have examples in the table
- examples are missing
- success response is absent
- error response is absent
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

{% include [Footnote about examples](../../_includes/examples.md) %}

## Success response

`true`

## Error response

error

### Possible error codes

#|
|| **Code** | **Description** ||
|| **BOT_ID_ERROR** | Chat-bot not found. ||
|| **APP_ID_ERROR** | Chat-bot does not belong to this application; only chat-bots installed within the application can be used. ||
|#

## Related links

[Rest API - Installation and update events](./events/index.md)