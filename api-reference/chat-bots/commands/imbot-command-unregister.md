# Remove the command imbot.command.unregister

{% note warning "We are still updating this page" %}

Some data may be missing here — we will fill it in shortly.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

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

> Scope: [`imbot`](../../scopes/permissions.md)
>
> Who can execute the method: any user

The method `imbot.command.unregister` removes the command handling.

#|
|| **Parameter** | **Example** | **Description** | **Revision** ||
|| **COMMAND_ID**
[`unknown`](../../data-types.md) | `13` | Identifier of the command to be removed | ||
|| **CLIENT_ID**
[`unknown`](../../data-types.md) | `''` | String identifier of the chatbot, used only in Webhook mode | ||
|#

## Examples

{% include [Explanation about restCommand](../_includes/rest-command.md) %}

```php
$result = restCommand(
    'imbot.command.unregister',
    Array(
        'COMMAND_ID' => 13,
        'CLIENT_ID' => '',
    ),
    $_REQUEST[
        "auth"
    ]
);
```

{% include [Footnote about examples](../../../_includes/examples.md) %}

{% note warning %}

To handle the command, the application must process the command addition event [ONIMCOMMANDADD](./events/index.md).

{% endnote %}

## Success response

`true`

## Error response

error

### Possible error codes

#|
|| **Code** | **Description** ||
|| **COMMAND_ID_ERROR** | Command not found. ||
|| **APP_ID_ERROR** | The chatbot does not belong to this application. Only chatbots installed within the application can be used. ||
|| **WRONG_REQUEST** | Something went wrong. ||
|#