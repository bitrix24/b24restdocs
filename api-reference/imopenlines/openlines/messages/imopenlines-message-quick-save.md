# Save Message as Quick Reply imopenlines.message.quick.save

{% note warning "We are still updating this page" %}

Some data may be missing — we will complete it soon.

{% endnote %}

{% if build == 'dev' %}

{% note alert "TO-DO _not deployed to prod_" %}

- parameter types are not specified
- examples are missing
- success response is absent
- error response is absent

{% endnote %}

{% endif %}

> Scope: [`imopenlines`](../../../scopes/permissions.md)
>
> Who can execute the method: any user

This method saves a message as a quick reply.

## Method Parameters

{% include [Note on parameters](../../../../_includes/required.md) %}

#|
|| **Name**
`Type` | **Example** | **Description** ||
|| **CHAT_ID***
[`unknown`](../../../data-types.md) | `2014` | Identifier of the chat ||
|| **MESSAGE_ID***
[`unknown`](../../../data-types.md) | `18715` | Identifier of the message ||
|#

## Examples

{% include [Note on examples](../../../../_includes/examples.md) %}

{% list tabs %}

- cURL (Webhook)

    // example for cURL (Webhook)

- cURL (OAuth)

    // example for cURL (OAuth)

- JS

    ```js
    BX.callMethod(
        'imopenlines.message.quick.save',
        {
            CHAT_ID: 2014,
            MESSAGE_ID: 18715
        },
        function(result)
        {
            if(result.error())
            {
                console.error(result.error().ex);
            }
            else
            {
                console.log(result.data());
            }
        }
    );
    ```

- PHP

    // example for PHP

{% endlist %}

## Success Response

```json
true
```

## Error Response

### Possible Error Codes

#|
|| **Code** | **Description** ||
|| **ACCESS_DENIED** | The current user does not have access to the specified method ||
|| **CANT_SAVE_QUICK_ANSWER** | Error saving quick reply ||
|#